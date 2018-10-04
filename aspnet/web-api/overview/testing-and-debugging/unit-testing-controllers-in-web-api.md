---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: 單元測試 ASP.NET Web API 2 中的控制站 |Microsoft Docs
author: MikeWasson
description: 本主題描述一些單元測試控制器，Web API 2 中的特定技術。 閱讀本主題之前，您可能想要閱讀本教學課程單位...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794820"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="fa6f3-104">單元測試 ASP.NET Web API 2 中的控制站</span><span class="sxs-lookup"><span data-stu-id="fa6f3-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fa6f3-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fa6f3-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="fa6f3-106">本主題描述一些單元測試控制器，Web API 2 中的特定技術。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="fa6f3-107">閱讀本主題之前，您可能想要閱讀本教學課程[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，其中顯示如何將單元測試專案新增至您的解決方案。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fa6f3-108">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="fa6f3-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="fa6f3-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="fa6f3-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="fa6f3-110">Web API 2</span><span class="sxs-lookup"><span data-stu-id="fa6f3-110">Web API 2</span></span>
> - <span data-ttu-id="fa6f3-111">[Moq](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="fa6f3-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="fa6f3-112">我使用 Moq，但相同的概念適用於任何模擬的架構。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="fa6f3-113">Moq 4.5.30 （和更新版本） 支援 Visual Studio 2017，Roslyn 及.NET 4.5 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="fa6f3-114">在單元測試中的常見模式是&quot;排列-act assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="fa6f3-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="fa6f3-115">排列： 設定要執行測試的任何必要條件。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="fa6f3-116">Act： 執行測試。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-116">Act: Perform the test.</span></span>
- <span data-ttu-id="fa6f3-117">判斷提示： 確認測試成功。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="fa6f3-118">在排列步驟中，您會經常使用模擬或虛設常式物件。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="fa6f3-119">可以降低相依性，讓測試著重於測試一件事。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="fa6f3-120">以下是一些您應該在 Web API 控制器中使用單元測試的事項：</span><span class="sxs-lookup"><span data-stu-id="fa6f3-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="fa6f3-121">此動作會傳回正確的回應類型。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="fa6f3-122">無效的參數傳回正確的錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="fa6f3-123">動作會呼叫正確的方法，在存放庫或服務層級。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="fa6f3-124">如果回應包含的領域模型，請確認模型類型。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="fa6f3-125">以下是一些一般項目，若要測試，但細節取決於您的控制器實作。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="fa6f3-126">特別是，它沒有太大差別控制器動作傳回是否**HttpResponseMessage**或是**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="fa6f3-127">如需這些結果類型的詳細資訊，請參閱[Web Api 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="fa6f3-128">傳回 HttpResponseMessage 的測試動作</span><span class="sxs-lookup"><span data-stu-id="fa6f3-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="fa6f3-129">以下是其動作傳回的控制站的範例**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="fa6f3-130">請注意，控制器會使用相依性插入來插入`IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="fa6f3-131">這可讓控制器進行測試，因為您可以插入模擬儲存機制。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="fa6f3-132">下列的單元測試會驗證`Get`方法寫入`Product`與回應本文。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="fa6f3-133">假設`repository`是 mock `IProductRepository`。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="fa6f3-134">請務必設定**要求**並**組態**控制站上。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="fa6f3-135">否則，測試將會失敗，並**ArgumentNullException**或是**InvalidOperationException**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="fa6f3-136">測試連結產生</span><span class="sxs-lookup"><span data-stu-id="fa6f3-136">Testing Link Generation</span></span>

<span data-ttu-id="fa6f3-137">`Post`方法呼叫**UrlHelper.Link**建立在回應中的連結。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="fa6f3-138">這需要一些更多的設定，在單元測試：</span><span class="sxs-lookup"><span data-stu-id="fa6f3-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="fa6f3-139">**UrlHelper**類別需要要求 URL 和路由資料，因此測試這些設定值。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="fa6f3-140">另一個選項是模擬或虛設常式**UrlHelper**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="fa6f3-141">您可以使用此方法時，取代預設值[ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx)模擬或虛設常式版本會傳回固定的值。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="fa6f3-142">讓我們重新撰寫測試方法[Moq](https://github.com/Moq) framework。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="fa6f3-143">安裝`Moq`在測試專案的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="fa6f3-144">在此版本中，您不需要設定任何路由資料中，因為 mock **UrlHelper**傳回常數字串。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>


## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="fa6f3-145">傳回 IHttpActionResult 的測試動作</span><span class="sxs-lookup"><span data-stu-id="fa6f3-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="fa6f3-146">在 Web API 2 中，控制器動作可以傳回**IHttpActionResult**，這相當於**ActionResult** ASP.NET MVC 中。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="fa6f3-147">**IHttpActionResult**介面會定義建立 HTTP 回應的命令模式。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="fa6f3-148">而不是直接建立回應，控制器會傳回**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="fa6f3-149">更新版本中，管線會叫用**IHttpActionResult**建立回應。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="fa6f3-150">這種方法可讓您更輕鬆地撰寫單元測試，因為您可以略過安裝程式所需的大量**HttpResponseMessage**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="fa6f3-151">以下是其動作傳回的範例控制器**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="fa6f3-152">此範例示範一些常見的模式，使用**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="fa6f3-153">我們來看看如何進行單元測試它們。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="fa6f3-154">動作會傳回 200 （確定） 回應內文</span><span class="sxs-lookup"><span data-stu-id="fa6f3-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="fa6f3-155">`Get`方法呼叫`Ok(product)`如果找到的產品。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="fa6f3-156">在單元測試時，請確定傳回的型別是**OkNegotiatedContentResult**和傳回的產品都有正確的識別碼。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="fa6f3-157">請注意，單元測試不會執行動作結果。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="fa6f3-158">您可以假設動作結果正確建立 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="fa6f3-159">(這就是為什麼 Web API 架構有其自己的單元測試 ！)</span><span class="sxs-lookup"><span data-stu-id="fa6f3-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="fa6f3-160">動作會傳回 404 （找不到）</span><span class="sxs-lookup"><span data-stu-id="fa6f3-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="fa6f3-161">`Get`方法呼叫`NotFound()`如果找不到產品。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="fa6f3-162">在此情況下，單元測試只檢查傳回的型別是否**NotFoundResult**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="fa6f3-163">動作會傳回 200 （確定） 沒有回應本文</span><span class="sxs-lookup"><span data-stu-id="fa6f3-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="fa6f3-164">`Delete`方法呼叫`Ok()`傳回空的 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="fa6f3-165">如上述範例中，單元測試會檢查傳回的型別，在此情況下**OkResult**。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="fa6f3-166">動作會傳回 201 （已建立） 的位置標頭</span><span class="sxs-lookup"><span data-stu-id="fa6f3-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="fa6f3-167">`Post`方法呼叫`CreatedAtRoute`Location 標頭中傳回 HTTP 201 的回應的 uri。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="fa6f3-168">在單元測試時，確認此動作會設定正確的路由值。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="fa6f3-169">動作會傳回另一個 2xx 回應內文</span><span class="sxs-lookup"><span data-stu-id="fa6f3-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="fa6f3-170">`Put`方法呼叫`Content`傳回回應主體與 HTTP 202 （已接受） 回應。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="fa6f3-171">此案例類似於傳回 200 （確定），但單元測試也應該檢查的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="fa6f3-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="fa6f3-172">Additional Resources</span></span>

- [<span data-ttu-id="fa6f3-173">模擬 Entity Framework 時的單元測試 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fa6f3-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="fa6f3-174">[撰寫測試的 ASP.NET Web API 服務](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)（Youssef Moussaoui 部落格文章）。</span><span class="sxs-lookup"><span data-stu-id="fa6f3-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="fa6f3-175">偵錯 ASP.NET Web API 路由偵錯工具</span><span class="sxs-lookup"><span data-stu-id="fa6f3-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
