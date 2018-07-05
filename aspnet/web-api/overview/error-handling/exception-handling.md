---
uid: web-api/overview/error-handling/exception-handling
title: ASP.NET Web API 中處理的例外狀況 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: ac01a4f35cde99a1f8ec699e6d31bf597f1d334e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400816"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="a310e-102">ASP.NET Web API 中處理的例外狀況</span><span class="sxs-lookup"><span data-stu-id="a310e-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a310e-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a310e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a310e-104">本文說明錯誤和 ASP.NET Web API 中處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a310e-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="a310e-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="a310e-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="a310e-106">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="a310e-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="a310e-107">註冊的例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="a310e-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="a310e-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="a310e-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="a310e-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="a310e-109">HttpResponseException</span></span>

<span data-ttu-id="a310e-110">如果 Web API 控制器會擲回未攔截到例外狀況，會發生什麼事？</span><span class="sxs-lookup"><span data-stu-id="a310e-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="a310e-111">根據預設，大部分的例外狀況會轉譯成 HTTP 回應狀態碼 500 內部伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="a310e-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="a310e-112">**HttpResponseException**型別是特殊案例。</span><span class="sxs-lookup"><span data-stu-id="a310e-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="a310e-113">這個例外狀況傳回您指定的例外狀況的建構函式中的任何 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="a310e-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="a310e-114">例如，下列方法會傳回 404，找不到，如果*識別碼*參數無效。</span><span class="sxs-lookup"><span data-stu-id="a310e-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="a310e-115">進一步控制回應的詳細資訊，您可以也建構整個回應訊息並將它包含與**HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="a310e-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="a310e-116">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="a310e-116">Exception Filters</span></span>

<span data-ttu-id="a310e-117">您可以自訂 Web API 撰寫所處理的例外狀況*例外狀況篩選條件*。</span><span class="sxs-lookup"><span data-stu-id="a310e-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="a310e-118">控制器方法擲回任何未處理例外狀況時，會執行例外狀況篩選條件*未* **HttpResponseException**例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a310e-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="a310e-119">**HttpResponseException**型別是特殊案例，因為它專為傳回 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="a310e-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="a310e-120">例外狀況篩選條件實作**System.Web.Http.Filters.IExceptionFilter**介面。</span><span class="sxs-lookup"><span data-stu-id="a310e-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="a310e-121">撰寫例外狀況篩選條件的最簡單方式是衍生自**System.Web.Http.Filters.ExceptionFilterAttribute**類別並覆寫**OnException**方法。</span><span class="sxs-lookup"><span data-stu-id="a310e-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="a310e-122">ASP.NET Web API 中的例外狀況篩選條件會類似於 ASP.NET MVC 中的項目。</span><span class="sxs-lookup"><span data-stu-id="a310e-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="a310e-123">不過，它們是不同的命名空間和函式中個別宣告。</span><span class="sxs-lookup"><span data-stu-id="a310e-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="a310e-124">特別是， **HandleErrorAttribute**在 MVC 中使用的類別不會處理 Web API 控制器所擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a310e-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="a310e-125">以下是將篩選條件**NotImplementedException**例外狀況變成 HTTP 狀態碼 501，未實作：</span><span class="sxs-lookup"><span data-stu-id="a310e-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="a310e-126">**回應**屬性**HttpActionExecutedContext**物件包含將傳送至用戶端的 HTTP 回應訊息。</span><span class="sxs-lookup"><span data-stu-id="a310e-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="a310e-127">註冊的例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="a310e-127">Registering Exception Filters</span></span>

<span data-ttu-id="a310e-128">有數種方式來註冊 Web API 例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="a310e-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="a310e-129">由動作</span><span class="sxs-lookup"><span data-stu-id="a310e-129">By action</span></span>
- <span data-ttu-id="a310e-130">控制站</span><span class="sxs-lookup"><span data-stu-id="a310e-130">By controller</span></span>
- <span data-ttu-id="a310e-131">全域</span><span class="sxs-lookup"><span data-stu-id="a310e-131">Globally</span></span>

<span data-ttu-id="a310e-132">若要將篩選套用至特定的動作，新增的屬性篩選條件的動作：</span><span class="sxs-lookup"><span data-stu-id="a310e-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="a310e-133">若要將篩選套用至所有控制站上的動作，新增的屬性篩選條件至控制器類別：</span><span class="sxs-lookup"><span data-stu-id="a310e-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="a310e-134">若要將篩選全域套用至所有的 Web API 控制器，將新增的篩選，以執行個體**GlobalConfiguration.Configuration.Filters**集合。</span><span class="sxs-lookup"><span data-stu-id="a310e-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="a310e-135">在此集合中的例外狀況篩選會套用至任何 Web API 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="a310e-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="a310e-136">如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 專案範本來建立專案時，將您的 Web API 組態程式碼內`WebApiConfig`類別，其位於應用程式\_開始資料夾：</span><span class="sxs-lookup"><span data-stu-id="a310e-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="a310e-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="a310e-137">HttpError</span></span>

<span data-ttu-id="a310e-138">**HttpError**物件提供一致的方式，回應主體中傳回錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="a310e-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="a310e-139">下列範例會示範如何傳回 HTTP 狀態碼 404 （找不到） 與**HttpError**回應主體中。</span><span class="sxs-lookup"><span data-stu-id="a310e-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="a310e-140">**CreateErrorResponse**擴充方法定義於**System.Net.Http.HttpRequestMessageExtensions**類別。</span><span class="sxs-lookup"><span data-stu-id="a310e-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="a310e-141">就內部而言， **CreateErrorResponse**會建立**HttpError**執行個體，然後建立**HttpResponseMessage**包含**HttpError**.</span><span class="sxs-lookup"><span data-stu-id="a310e-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="a310e-142">在此範例中，如果方法成功，它會傳回 HTTP 回應中的產品。</span><span class="sxs-lookup"><span data-stu-id="a310e-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="a310e-143">但如果找不到要求的產品，HTTP 回應會包含**HttpError**要求主體中。</span><span class="sxs-lookup"><span data-stu-id="a310e-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="a310e-144">回應看起來可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="a310e-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="a310e-145">請注意， **HttpError**已序列化為 JSON，在此範例中。</span><span class="sxs-lookup"><span data-stu-id="a310e-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="a310e-146">使用的優點之一**HttpError**是它會通過相同[內容交涉](../formats-and-model-binding/content-negotiation.md)和序列化處理任何其他的強型別模型。</span><span class="sxs-lookup"><span data-stu-id="a310e-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="a310e-147">HttpError 和驗證模型</span><span class="sxs-lookup"><span data-stu-id="a310e-147">HttpError and Model Validation</span></span>

<span data-ttu-id="a310e-148">驗證模型，您可以傳遞至模型狀態**CreateErrorResponse**，以便在回應中包含的驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="a310e-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="a310e-149">此範例中，可能會傳回下列回應：</span><span class="sxs-lookup"><span data-stu-id="a310e-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="a310e-150">如需有關模型驗證的詳細資訊，請參閱 < [ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="a310e-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="a310e-151">使用 HttpResponseException HttpError</span><span class="sxs-lookup"><span data-stu-id="a310e-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="a310e-152">前一個範例會傳回**HttpResponseMessage**訊息從控制器動作，但您也可以使用**HttpResponseException**返回**HttpError**。</span><span class="sxs-lookup"><span data-stu-id="a310e-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="a310e-153">這可讓您在一般的成功的案例中，傳回強型別模型，而仍可傳回**HttpError**如果發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="a310e-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
