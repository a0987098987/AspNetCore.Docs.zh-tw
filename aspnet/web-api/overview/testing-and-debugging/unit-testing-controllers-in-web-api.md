---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: "單元測試中 ASP.NET Web API 2 控制器 |Microsoft 文件"
author: MikeWasson
description: "本主題說明一些特定的技術，單元測試中 Web API 2 控制器。 閱讀本主題之前，可能會想要閱讀本教學課程單位..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 167cd24d27977c3652f6a8903054654f5edf7756
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="3f78d-104">單元測試中 ASP.NET Web API 2 控制器</span><span class="sxs-lookup"><span data-stu-id="3f78d-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3f78d-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3f78d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3f78d-106">本主題說明一些特定的技術，單元測試中 Web API 2 控制器。</span><span class="sxs-lookup"><span data-stu-id="3f78d-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="3f78d-107">閱讀本主題之前，您可能想要閱讀本教學課程[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，其中示範如何將單元測試專案加入至您的方案。</span><span class="sxs-lookup"><span data-stu-id="3f78d-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3f78d-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="3f78d-108">Software versions used in the tutorial</span></span>
> 
> - [<span data-ttu-id="3f78d-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3f78d-109">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="3f78d-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="3f78d-110">Web API 2</span></span>
> - <span data-ttu-id="3f78d-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="3f78d-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="3f78d-112">我使用 Moq，但相同的概念適用於任何模擬的架構。</span><span class="sxs-lookup"><span data-stu-id="3f78d-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="3f78d-113">Moq 4.5.30 （和更新版本） 支援 Visual Studio 2017、 Roslyn 和.NET 4.5 及更新版本。</span><span class="sxs-lookup"><span data-stu-id="3f78d-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="3f78d-114">在單元測試中常見的模式是&quot;排列-act assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="3f78d-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="3f78d-115">排列： 設定要執行測試的任何必要條件。</span><span class="sxs-lookup"><span data-stu-id="3f78d-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="3f78d-116">Act： 執行測試。</span><span class="sxs-lookup"><span data-stu-id="3f78d-116">Act: Perform the test.</span></span>
- <span data-ttu-id="3f78d-117">判斷提示： 確認測試成功。</span><span class="sxs-lookup"><span data-stu-id="3f78d-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="3f78d-118">在排列步驟中，您通常會使用模擬或虛設常式物件。</span><span class="sxs-lookup"><span data-stu-id="3f78d-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="3f78d-119">讓測試著重於測試一件事，可減少相依性的數目。</span><span class="sxs-lookup"><span data-stu-id="3f78d-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="3f78d-120">以下是您在 Web API 控制器應該單元測試的一些事項：</span><span class="sxs-lookup"><span data-stu-id="3f78d-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="3f78d-121">此動作會傳回正確的回應類型。</span><span class="sxs-lookup"><span data-stu-id="3f78d-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="3f78d-122">無效的參數傳回正確的錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="3f78d-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="3f78d-123">動作的儲存機制或服務的圖層上呼叫正確的方法。</span><span class="sxs-lookup"><span data-stu-id="3f78d-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="3f78d-124">如果回應包含網域模型，請確認模型類型。</span><span class="sxs-lookup"><span data-stu-id="3f78d-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="3f78d-125">以下是一些一般項目，若要測試，但是細節取決於您控制站的實作。</span><span class="sxs-lookup"><span data-stu-id="3f78d-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="3f78d-126">特別是，它會很大的差異是否控制器動作傳回**HttpResponseMessage**或**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="3f78d-127">如需這些結果類型的詳細資訊，請參閱[Web Api 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。</span><span class="sxs-lookup"><span data-stu-id="3f78d-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="3f78d-128">傳回 HttpResponseMessage 的測試動作</span><span class="sxs-lookup"><span data-stu-id="3f78d-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="3f78d-129">以下是其動作傳回的控制站範例**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="3f78d-130">請注意，將插入的控制器會使用的相依性插入`IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="3f78d-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="3f78d-131">這使控制站更可測試，因為您可以將插入模擬儲存機制。</span><span class="sxs-lookup"><span data-stu-id="3f78d-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="3f78d-132">以下的單元測試會驗證`Get`方法寫入`Product`回應主體。</span><span class="sxs-lookup"><span data-stu-id="3f78d-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="3f78d-133">假設`repository`是模擬`IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="3f78d-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="3f78d-134">請務必設定**要求**和**組態**控制站上。</span><span class="sxs-lookup"><span data-stu-id="3f78d-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="3f78d-135">否則，測試將會失敗，並**ArgumentNullException**或**InvalidOperationException**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="3f78d-136">測試連結產生</span><span class="sxs-lookup"><span data-stu-id="3f78d-136">Testing Link Generation</span></span>

<span data-ttu-id="3f78d-137">`Post`方法呼叫**UrlHelper.Link**回應中建立連結。</span><span class="sxs-lookup"><span data-stu-id="3f78d-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="3f78d-138">您需要在單元測試中一些更多的設定：</span><span class="sxs-lookup"><span data-stu-id="3f78d-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="3f78d-139">**UrlHelper**類別需要要求 URL 和路由資料，因此測試這些設定值。</span><span class="sxs-lookup"><span data-stu-id="3f78d-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="3f78d-140">另一個選項是模擬或虛設常式**UrlHelper**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="3f78d-141">您可以使用這個方法，取代預設值[ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx)模擬或虛設常式版本會傳回固定的值。</span><span class="sxs-lookup"><span data-stu-id="3f78d-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="3f78d-142">讓我們重新撰寫使用測試[Moq](https://github.com/Moq)架構。</span><span class="sxs-lookup"><span data-stu-id="3f78d-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="3f78d-143">安裝`Moq`測試專案中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="3f78d-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="3f78d-144">在此版本中，您不需要設定任何路由的資料，因為模擬**UrlHelper**傳回常數字串。</span><span class="sxs-lookup"><span data-stu-id="3f78d-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="3f78d-145">傳回 IHttpActionResult 的測試動作</span><span class="sxs-lookup"><span data-stu-id="3f78d-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="3f78d-146">在 Web API 2 控制器動作，可以傳回**IHttpActionResult**，相當於**ActionResult** ASP.NET MVC 中。</span><span class="sxs-lookup"><span data-stu-id="3f78d-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="3f78d-147">**IHttpActionResult**介面會定義建立的 HTTP 回應的命令模式。</span><span class="sxs-lookup"><span data-stu-id="3f78d-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="3f78d-148">控制站會傳回而不是直接建立回應， **IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="3f78d-149">更新版本中，管線會叫用**IHttpActionResult**建立回應。</span><span class="sxs-lookup"><span data-stu-id="3f78d-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="3f78d-150">這種方法可讓您更輕鬆地撰寫單元測試，因為您可以略過所需的安裝程式的許多**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="3f78d-151">以下是此範例控制器會執行其動作傳回**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="3f78d-152">這個範例會示範一些常見的模式使用**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="3f78d-153">我們來看看如何單位測試它們。</span><span class="sxs-lookup"><span data-stu-id="3f78d-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="3f78d-154">動作將會傳回 200 （確定） 回應內文</span><span class="sxs-lookup"><span data-stu-id="3f78d-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="3f78d-155">`Get`方法呼叫`Ok(product)`如果找到的產品。</span><span class="sxs-lookup"><span data-stu-id="3f78d-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="3f78d-156">在單元測試時，請確定傳回的型別是**OkNegotiatedContentResult**且傳回的產品具有權限的識別碼。</span><span class="sxs-lookup"><span data-stu-id="3f78d-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="3f78d-157">請注意，單元測試不會執行動作結果。</span><span class="sxs-lookup"><span data-stu-id="3f78d-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="3f78d-158">您可以假設在動作結果會建立正確的 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="3f78d-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="3f78d-159">(這就是為什麼 Web API 架構有它自己的單元測試 ！)</span><span class="sxs-lookup"><span data-stu-id="3f78d-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="3f78d-160">動作將會傳回 404 （找不到）</span><span class="sxs-lookup"><span data-stu-id="3f78d-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="3f78d-161">`Get`方法呼叫`NotFound()`如果找不到產品。</span><span class="sxs-lookup"><span data-stu-id="3f78d-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="3f78d-162">此案例中，單元測試只檢查傳回的型別是否**NotFoundResult**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="3f78d-163">動作會傳回 「 200 （確定） 沒有回應主體</span><span class="sxs-lookup"><span data-stu-id="3f78d-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="3f78d-164">`Delete`方法呼叫`Ok()`傳回空的 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="3f78d-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="3f78d-165">就像先前範例中，單元測試會檢查傳回的型別，在此情況下**OkResult**。</span><span class="sxs-lookup"><span data-stu-id="3f78d-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="3f78d-166">動作將會傳回 201 （已建立） 的位置標頭</span><span class="sxs-lookup"><span data-stu-id="3f78d-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="3f78d-167">`Post`方法呼叫`CreatedAtRoute`以位置標頭中傳回 HTTP 201 回應的 uri。</span><span class="sxs-lookup"><span data-stu-id="3f78d-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="3f78d-168">在單元測試，確認動作設定正確的路由值。</span><span class="sxs-lookup"><span data-stu-id="3f78d-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="3f78d-169">動作傳回的回應主體的另一個 2xx</span><span class="sxs-lookup"><span data-stu-id="3f78d-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="3f78d-170">`Put`方法呼叫`Content`傳回回應主體與 HTTP 202 （已接受） 回應。</span><span class="sxs-lookup"><span data-stu-id="3f78d-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="3f78d-171">此情況下傳回 200 （確定），類似，但在單元測試也應該檢查的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="3f78d-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="3f78d-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="3f78d-172">Additional Resources</span></span>

- [<span data-ttu-id="3f78d-173">模擬 Entity Framework 時單元測試 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="3f78d-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="3f78d-174">[撰寫測試的 ASP.NET Web API 服務](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)（Youssef Moussaoui 部落格文章）。</span><span class="sxs-lookup"><span data-stu-id="3f78d-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="3f78d-175">偵錯 ASP.NET Web API 路由偵錯工具</span><span class="sxs-lookup"><span data-stu-id="3f78d-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
