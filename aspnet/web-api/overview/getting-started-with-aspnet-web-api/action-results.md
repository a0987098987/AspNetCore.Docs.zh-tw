---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: 動作會導致 Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 7726829ac9eba339ff3ac1c94c86132cb1090783
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395525"
---
<a name="action-results-in-web-api-2"></a><span data-ttu-id="25ad3-102">Web API 2 中的動作結果</span><span class="sxs-lookup"><span data-stu-id="25ad3-102">Action Results in Web API 2</span></span>
====================
<span data-ttu-id="25ad3-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25ad3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="25ad3-104">本主題說明 ASP.NET Web API 將傳回的值轉換從控制器動作至 HTTP 回應訊息的方式。</span><span class="sxs-lookup"><span data-stu-id="25ad3-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="25ad3-105">Web API 控制器動作可以傳回下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="25ad3-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="25ad3-106">void</span><span class="sxs-lookup"><span data-stu-id="25ad3-106">void</span></span>
2. <span data-ttu-id="25ad3-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="25ad3-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="25ad3-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="25ad3-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="25ad3-109">其他類型</span><span class="sxs-lookup"><span data-stu-id="25ad3-109">Some other type</span></span>

<span data-ttu-id="25ad3-110">根據這些傳回時，Web API 會使用不同的機制來建立 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="25ad3-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="25ad3-111">傳回型別</span><span class="sxs-lookup"><span data-stu-id="25ad3-111">Return type</span></span> | <span data-ttu-id="25ad3-112">Web API 建立回應的方式</span><span class="sxs-lookup"><span data-stu-id="25ad3-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="25ad3-113">void</span><span class="sxs-lookup"><span data-stu-id="25ad3-113">void</span></span> | <span data-ttu-id="25ad3-114">傳回空 204 （沒有內容）</span><span class="sxs-lookup"><span data-stu-id="25ad3-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="25ad3-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="25ad3-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="25ad3-116">直接將轉換的 HTTP 回應訊息。</span><span class="sxs-lookup"><span data-stu-id="25ad3-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="25ad3-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="25ad3-117">**IHttpActionResult**</span></span> | <span data-ttu-id="25ad3-118">呼叫**ExecuteAsync**來建立**HttpResponseMessage**，然後將轉換為 HTTP 回應訊息。</span><span class="sxs-lookup"><span data-stu-id="25ad3-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="25ad3-119">其他類型</span><span class="sxs-lookup"><span data-stu-id="25ad3-119">Other type</span></span> | <span data-ttu-id="25ad3-120">將序列化的傳回值寫入至回應主體中;傳回 200 （確定）。</span><span class="sxs-lookup"><span data-stu-id="25ad3-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="25ad3-121">本主題的其餘部分描述更詳細的每個選項。</span><span class="sxs-lookup"><span data-stu-id="25ad3-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="25ad3-122">void</span><span class="sxs-lookup"><span data-stu-id="25ad3-122">void</span></span>

<span data-ttu-id="25ad3-123">如果傳回的型別`void`，Web API 只會傳回空的 HTTP 回應狀態碼 204 （沒有內容）。</span><span class="sxs-lookup"><span data-stu-id="25ad3-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="25ad3-124">範例控制器：</span><span class="sxs-lookup"><span data-stu-id="25ad3-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="25ad3-125">HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="25ad3-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="25ad3-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="25ad3-126">HttpResponseMessage</span></span>

<span data-ttu-id="25ad3-127">如果此動作會傳回[HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)，Web API 轉換為傳回值直接 HTTP 回應訊息使用的屬性**HttpResponseMessage**來填入的物件回應。</span><span class="sxs-lookup"><span data-stu-id="25ad3-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="25ad3-128">此選項可讓您控制回應訊息很多。</span><span class="sxs-lookup"><span data-stu-id="25ad3-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="25ad3-129">例如，下列控制器動作設定 Cache-control 標頭。</span><span class="sxs-lookup"><span data-stu-id="25ad3-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="25ad3-130">回應：</span><span class="sxs-lookup"><span data-stu-id="25ad3-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="25ad3-131">如果您傳遞至網域模型**CreateResponse**方法中，Web API 會使用[媒體格式器](../formats-and-model-binding/media-formatters.md)寫入回應主體中序列化的模型。</span><span class="sxs-lookup"><span data-stu-id="25ad3-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="25ad3-132">若要選擇的格式器，web API 會使用 Accept 標頭在要求中。</span><span class="sxs-lookup"><span data-stu-id="25ad3-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="25ad3-133">如需詳細資訊，請參閱 <<c0> [ 內容交涉](../formats-and-model-binding/content-negotiation.md)。</span><span class="sxs-lookup"><span data-stu-id="25ad3-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="25ad3-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="25ad3-134">IHttpActionResult</span></span>

<span data-ttu-id="25ad3-135">**IHttpActionResult** Web API 2 中引進了介面。</span><span class="sxs-lookup"><span data-stu-id="25ad3-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="25ad3-136">基本上，它會定義**HttpResponseMessage** factory。</span><span class="sxs-lookup"><span data-stu-id="25ad3-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="25ad3-137">以下是使用的一些優點**IHttpActionResult**介面：</span><span class="sxs-lookup"><span data-stu-id="25ad3-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="25ad3-138">可簡化[單元測試](../testing-and-debugging/unit-testing-controllers-in-web-api.md)控制器。</span><span class="sxs-lookup"><span data-stu-id="25ad3-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="25ad3-139">將移到另一個類別建立 HTTP 回應的一般邏輯。</span><span class="sxs-lookup"><span data-stu-id="25ad3-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="25ad3-140">藉由隱藏建構回應的低層級的詳細資料可更清楚，控制器動作的意圖。</span><span class="sxs-lookup"><span data-stu-id="25ad3-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="25ad3-141">**IHttpActionResult**包含單一方法**ExecuteAsync**，以非同步方式建立**HttpResponseMessage**執行個體。</span><span class="sxs-lookup"><span data-stu-id="25ad3-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="25ad3-142">如果控制器動作傳回**IHttpActionResult**，Web API 會呼叫**ExecuteAsync**方法，以建立**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="25ad3-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="25ad3-143">然後它會將轉換**HttpResponseMessage**至 HTTP 回應訊息。</span><span class="sxs-lookup"><span data-stu-id="25ad3-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="25ad3-144">以下是簡單的實作的**IHttpActionResult**所建立的純文字回應：</span><span class="sxs-lookup"><span data-stu-id="25ad3-144">Here is a simple implementaton of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="25ad3-145">範例控制器動作：</span><span class="sxs-lookup"><span data-stu-id="25ad3-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="25ad3-146">回應：</span><span class="sxs-lookup"><span data-stu-id="25ad3-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="25ad3-147">通常您會使用**IHttpActionResult**中所定義的實作**[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** 命名空間。</span><span class="sxs-lookup"><span data-stu-id="25ad3-147">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="25ad3-148">**ApiController**類別定義會傳回這些內建動作結果的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="25ad3-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="25ad3-149">在下列範例中，如果要求不符合現有的產品識別碼，控制器會呼叫[ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx)建立 404 （找不到） 回應。</span><span class="sxs-lookup"><span data-stu-id="25ad3-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="25ad3-150">否則，控制器會呼叫[ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx)，這會建立 200 （確定） 回應，包含產品。</span><span class="sxs-lookup"><span data-stu-id="25ad3-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="25ad3-151">其他傳回型別</span><span class="sxs-lookup"><span data-stu-id="25ad3-151">Other Return Types</span></span>

<span data-ttu-id="25ad3-152">對於所有其他傳回類型，Web API 會使用[媒體格式器](../formats-and-model-binding/media-formatters.md)序列化傳回的值。</span><span class="sxs-lookup"><span data-stu-id="25ad3-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="25ad3-153">Web API 會寫入回應主體中序列化的值。</span><span class="sxs-lookup"><span data-stu-id="25ad3-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="25ad3-154">回應狀態碼為 200 （確定）。</span><span class="sxs-lookup"><span data-stu-id="25ad3-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="25ad3-155">這種方法的缺點是您不能直接傳回錯誤碼 404 等。</span><span class="sxs-lookup"><span data-stu-id="25ad3-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="25ad3-156">不過，您可以擲回**HttpResponseException**錯誤代碼。</span><span class="sxs-lookup"><span data-stu-id="25ad3-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="25ad3-157">如需詳細資訊，請參閱 < [ASP.NET Web API 中的例外狀況處理](../error-handling/exception-handling.md)。</span><span class="sxs-lookup"><span data-stu-id="25ad3-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="25ad3-158">若要選擇的格式器，web API 會使用 Accept 標頭在要求中。</span><span class="sxs-lookup"><span data-stu-id="25ad3-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="25ad3-159">如需詳細資訊，請參閱 <<c0> [ 內容交涉](../formats-and-model-binding/content-negotiation.md)。</span><span class="sxs-lookup"><span data-stu-id="25ad3-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="25ad3-160">範例要求</span><span class="sxs-lookup"><span data-stu-id="25ad3-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="25ad3-161">範例回應：</span><span class="sxs-lookup"><span data-stu-id="25ad3-161">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
