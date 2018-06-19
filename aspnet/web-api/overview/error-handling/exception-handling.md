---
uid: web-api/overview/error-handling/exception-handling
title: ASP.NET Web API 的例外狀況處理 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506957"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="e8f59-102">ASP.NET Web API 的例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="e8f59-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e8f59-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e8f59-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e8f59-104">本文說明錯誤和例外狀況處理中 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="e8f59-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="e8f59-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="e8f59-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="e8f59-106">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="e8f59-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="e8f59-107">註冊例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="e8f59-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="e8f59-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="e8f59-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="e8f59-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="e8f59-109">HttpResponseException</span></span>

<span data-ttu-id="e8f59-110">如果 Web API 控制器會擲回無法攔截的例外狀況會怎樣？</span><span class="sxs-lookup"><span data-stu-id="e8f59-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="e8f59-111">根據預設，大部分的例外狀況會轉譯為 HTTP 回應狀態碼 500 內部伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="e8f59-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="e8f59-112">**HttpResponseException**型別是特殊案例。</span><span class="sxs-lookup"><span data-stu-id="e8f59-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="e8f59-113">這個例外狀況傳回您指定的例外狀況建構函式中的任何 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e8f59-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="e8f59-114">例如，下列方法會傳回 404 找不到，如果*識別碼*參數無效。</span><span class="sxs-lookup"><span data-stu-id="e8f59-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="e8f59-115">如需更多控制回應的詳細資訊，也可以建構整個回應訊息和將它與包含**HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="e8f59-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="e8f59-116">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="e8f59-116">Exception Filters</span></span>

<span data-ttu-id="e8f59-117">您可以自訂 Web 應用程式開發介面撰寫所處理的例外狀況*例外狀況篩選條件*。</span><span class="sxs-lookup"><span data-stu-id="e8f59-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="e8f59-118">例外狀況篩選條件執行控制器方法擲回任何未處理的例外狀況時*不* **HttpResponseException**例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e8f59-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="e8f59-119">**HttpResponseException**型別是特殊案例中，因為它設計為傳回 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="e8f59-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="e8f59-120">例外狀況篩選條件實作**System.Web.Http.Filters.IExceptionFilter**介面。</span><span class="sxs-lookup"><span data-stu-id="e8f59-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="e8f59-121">撰寫例外狀況篩選條件的最簡單方式是衍生自**System.Web.Http.Filters.ExceptionFilterAttribute**類別並覆寫**OnException**方法。</span><span class="sxs-lookup"><span data-stu-id="e8f59-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="e8f59-122">ASP.NET Web API 中的例外狀況篩選條件是 ASP.NET MVC 中類似的項目。</span><span class="sxs-lookup"><span data-stu-id="e8f59-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="e8f59-123">不過，它們是不同的命名空間和函式中個別宣告。</span><span class="sxs-lookup"><span data-stu-id="e8f59-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="e8f59-124">特別是， **HandleErrorAttribute**在 MVC 中使用的類別不會處理由 Web API 控制器會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e8f59-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="e8f59-125">以下是將篩選**NotImplementedException** HTTP 狀態例外狀況程式碼 501，未實作：</span><span class="sxs-lookup"><span data-stu-id="e8f59-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="e8f59-126">**回應**屬性**HttpActionExecutedContext**物件包含 HTTP 回應訊息將傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="e8f59-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="e8f59-127">註冊例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="e8f59-127">Registering Exception Filters</span></span>

<span data-ttu-id="e8f59-128">有幾種方式可以註冊 Web 應用程式開發介面的例外狀況篩選條件：</span><span class="sxs-lookup"><span data-stu-id="e8f59-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="e8f59-129">動作</span><span class="sxs-lookup"><span data-stu-id="e8f59-129">By action</span></span>
- <span data-ttu-id="e8f59-130">控制站</span><span class="sxs-lookup"><span data-stu-id="e8f59-130">By controller</span></span>
- <span data-ttu-id="e8f59-131">全域</span><span class="sxs-lookup"><span data-stu-id="e8f59-131">Globally</span></span>

<span data-ttu-id="e8f59-132">若要將篩選套用至特定的動作，新增的屬性篩選器動作：</span><span class="sxs-lookup"><span data-stu-id="e8f59-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="e8f59-133">將篩選套用到所有的控制站上的動作，加入篩選條件做為屬性加入控制器類別：</span><span class="sxs-lookup"><span data-stu-id="e8f59-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="e8f59-134">將篩選套用到全域所有的 Web API 控制器，加入篩選，以執行個體**GlobalConfiguration.Configuration.Filters**集合。</span><span class="sxs-lookup"><span data-stu-id="e8f59-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="e8f59-135">在此集合中的例外狀況篩選會套用至任何 Web API 控制器動作。</span><span class="sxs-lookup"><span data-stu-id="e8f59-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="e8f59-136">如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 專案範本來建立專案時，將 Web API 組態程式碼內`WebApiConfig`類別，位於應用程式\_開始資料夾：</span><span class="sxs-lookup"><span data-stu-id="e8f59-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="e8f59-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="e8f59-137">HttpError</span></span>

<span data-ttu-id="e8f59-138">**HttpError**物件提供一致的方式回應主體中傳回的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e8f59-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="e8f59-139">下列範例會示範如何傳回 HTTP 狀態碼 404 （找不到） 與**HttpError**回應主體中。</span><span class="sxs-lookup"><span data-stu-id="e8f59-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="e8f59-140">**CreateErrorResponse**擴充方法定義中**System.Net.Http.HttpRequestMessageExtensions**類別。</span><span class="sxs-lookup"><span data-stu-id="e8f59-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="e8f59-141">就內部而言， **CreateErrorResponse**建立**HttpError**執行個體，然後建立**HttpResponseMessage**包含**HttpError**.</span><span class="sxs-lookup"><span data-stu-id="e8f59-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="e8f59-142">在此範例中，如果該方法成功，它會傳回 HTTP 回應中的產品。</span><span class="sxs-lookup"><span data-stu-id="e8f59-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="e8f59-143">但如果找不到要求的產品，HTTP 回應將包含**HttpError**要求主體中。</span><span class="sxs-lookup"><span data-stu-id="e8f59-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="e8f59-144">回應可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="e8f59-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="e8f59-145">請注意， **HttpError**已序列化為 JSON，在此範例中。</span><span class="sxs-lookup"><span data-stu-id="e8f59-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="e8f59-146">使用的其中一個優點**HttpError**是它會進行相同[內容交涉](../formats-and-model-binding/content-negotiation.md)和序列化處理任何其他的強型別模型。</span><span class="sxs-lookup"><span data-stu-id="e8f59-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="e8f59-147">HttpError 與模型驗證</span><span class="sxs-lookup"><span data-stu-id="e8f59-147">HttpError and Model Validation</span></span>

<span data-ttu-id="e8f59-148">模型驗證，您可以傳遞至模型狀態**CreateErrorResponse**，若要在回應中包含的驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="e8f59-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="e8f59-149">此範例中，可能會傳回下列回應：</span><span class="sxs-lookup"><span data-stu-id="e8f59-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="e8f59-150">如需模型驗證的詳細資訊，請參閱[ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="e8f59-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="e8f59-151">使用 HttpResponseException HttpError</span><span class="sxs-lookup"><span data-stu-id="e8f59-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="e8f59-152">前一個範例傳回**HttpResponseMessage**訊息從控制器動作，但您也可以使用**HttpResponseException**傳回**HttpError**。</span><span class="sxs-lookup"><span data-stu-id="e8f59-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="e8f59-153">這可讓您在一般的成功情況中，傳回強型別模型，而仍可傳回**HttpError**如果發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="e8f59-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
