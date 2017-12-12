---
title: "在 ASP.NET Core 中測試控制器邏輯"
author: ardalis
description: "了解如何在 ASP.NET Core Moq 和 xUnit 中測試控制器邏輯。"
keywords: "ASP.NET Core 控制器、 測試、 測試、 單元測試、 單元測試、 整合測試、 整合測試、 xUnit"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: aa60912e06946bd0df4936d33c88d3bf7b69984c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a><span data-ttu-id="6f2a1-104">在 ASP.NET Core 中測試控制器邏輯</span><span class="sxs-lookup"><span data-stu-id="6f2a1-104">Testing controller logic in ASP.NET Core</span></span>

<span data-ttu-id="6f2a1-105">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6f2a1-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6f2a1-106">ASP.NET MVC 應用程式中的控制站應該小，並著重於使用者介面考量。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-106">Controllers in ASP.NET MVC apps should be small and focused on user-interface concerns.</span></span> <span data-ttu-id="6f2a1-107">處理非 UI 考量的大型控制站會更難以測試及維護。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-107">Large controllers that deal with non-UI concerns are more difficult to test and maintain.</span></span>

[<span data-ttu-id="6f2a1-108">檢視或從 GitHub 下載範例</span><span class="sxs-lookup"><span data-stu-id="6f2a1-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a><span data-ttu-id="6f2a1-109">測試控制器</span><span class="sxs-lookup"><span data-stu-id="6f2a1-109">Testing controllers</span></span>

<span data-ttu-id="6f2a1-110">控制站都是中央任何 ASP.NET Core MVC 應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-110">Controllers are a central part of any ASP.NET Core MVC application.</span></span> <span data-ttu-id="6f2a1-111">因此，您應該有它們的行為如預期般應用程式的信心。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-111">As such, you should have confidence they behave as intended for your app.</span></span> <span data-ttu-id="6f2a1-112">自動化的測試可以提供您進行此部署，並在到達生產環境之前，可以偵測錯誤。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-112">Automated tests can provide you with this confidence and can detect errors before they reach production.</span></span> <span data-ttu-id="6f2a1-113">請務必避免將放在您的控制站不必要的責任，並確保只能在控制器責任您測試的焦點。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-113">It's important to avoid placing unnecessary responsibilities within your controllers and ensure your tests focus only on controller responsibilities.</span></span>

<span data-ttu-id="6f2a1-114">控制器邏輯應該很小，並不會著重於商務邏輯或基礎結構考量 （例如，資料存取）。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-114">Controller logic should be minimal and not be focused on business logic or infrastructure concerns (for example, data access).</span></span> <span data-ttu-id="6f2a1-115">測試控制器邏輯，不是架構。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-115">Test controller logic, not the framework.</span></span> <span data-ttu-id="6f2a1-116">測試如何控制器*行為*根據有效或無效的輸入。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-116">Test how the controller *behaves* based on valid or invalid inputs.</span></span> <span data-ttu-id="6f2a1-117">測試控制器回應根據它所執行的商務作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-117">Test controller responses based on the result of the business operation it performs.</span></span>

<span data-ttu-id="6f2a1-118">典型的控制器責任：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-118">Typical controller responsibilities:</span></span>

* <span data-ttu-id="6f2a1-119">確認`ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-119">Verify `ModelState.IsValid`.</span></span>
* <span data-ttu-id="6f2a1-120">如果傳回錯誤回應`ModelState`無效。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-120">Return an error response if `ModelState` is invalid.</span></span>
* <span data-ttu-id="6f2a1-121">從持續性儲存中擷取的商務實體。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-121">Retrieve a business entity from persistence.</span></span>
* <span data-ttu-id="6f2a1-122">商務實體上執行的動作。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-122">Perform an action on the business entity.</span></span>
* <span data-ttu-id="6f2a1-123">將商務實體儲存至持續性。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-123">Save the business entity to persistence.</span></span>
* <span data-ttu-id="6f2a1-124">傳回適當`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-124">Return an appropriate `IActionResult`.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="6f2a1-125">單元測試</span><span class="sxs-lookup"><span data-stu-id="6f2a1-125">Unit testing</span></span>

<span data-ttu-id="6f2a1-126">[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)包括從其基礎結構和相依性隔離測試應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-126">[Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) involves testing a part of an app in isolation from its infrastructure and dependencies.</span></span> <span data-ttu-id="6f2a1-127">單元測試控制器邏輯，只有單一動作的內容進行測試時，不是行為架構本身或其相依性。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-127">When unit testing controller logic, only the contents of a single action is tested, not the behavior of its dependencies or of the framework itself.</span></span> <span data-ttu-id="6f2a1-128">為您的單元測試控制器動作，請確定您只著重於其行為。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-128">As you unit test your controller actions, make sure you focus only on its behavior.</span></span> <span data-ttu-id="6f2a1-129">控制器單元測試可避免等[篩選](filters.md)，[路由](../../fundamentals/routing.md)，或[模型繫結](../models/model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-129">A controller unit test avoids things like [filters](filters.md), [routing](../../fundamentals/routing.md), or [model binding](../models/model-binding.md).</span></span> <span data-ttu-id="6f2a1-130">把焦點放在測試只是一件事，通常的單元測試是容易撰寫且快速地執行。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-130">By focusing on testing just one thing, unit tests are generally simple to write and quick to run.</span></span> <span data-ttu-id="6f2a1-131">編寫完善的組的單元測試可以經常執行，不多的負荷。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-131">A well-written set of unit tests can be run frequently without much overhead.</span></span> <span data-ttu-id="6f2a1-132">不過，單元測試不會偵測問題中元件之間的互動，也就是目的[整合測試](xref:mvc/controllers/testing#integration-testing)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-132">However, unit tests do not detect issues in the interaction between components, which is the purpose of [integration testing](xref:mvc/controllers/testing#integration-testing).</span></span>

<span data-ttu-id="6f2a1-133">如果您正在撰寫自訂篩選條件、 路由和其他內容，您應該將單元測試，但不是上特定控制器執行測試的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-133">If you're writing custom filters, routes, etc, you should unit test them, but not as part of your tests on a particular controller action.</span></span> <span data-ttu-id="6f2a1-134">它們應該地接受獨立測試。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-134">They should be tested in isolation.</span></span>

> [!TIP]
> <span data-ttu-id="6f2a1-135">[建立及執行單元測試使用 Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-135">[Create and run unit tests with Visual Studio](https://docs.microsoft.com/visualstudio/test/unit-test-your-code).</span></span>

<span data-ttu-id="6f2a1-136">若要示範的單元測試，請檢閱下列控制站。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-136">To demonstrate unit testing, review the following controller.</span></span> <span data-ttu-id="6f2a1-137">它會顯示一份腦力激盪工作，並可讓新腦力激盪使用 POST 來建立的工作階段：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-137">It displays a list of brainstorming sessions and allows new brainstorming sessions to be created with a POST:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

<span data-ttu-id="6f2a1-138">之後，控制器[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)，預期為提供的執行個體的相依性插入`IBrainstormSessionRepository`。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-138">The controller is following the [explicit dependencies principle](http://deviq.com/explicit-dependencies-principle/), expecting dependency injection to provide it with an instance of `IBrainstormSessionRepository`.</span></span> <span data-ttu-id="6f2a1-139">這樣很容易就能測試使用模擬物件架構，例如[Moq](https://www.nuget.org/packages/Moq/)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-139">This makes it fairly easy to test using a mock object framework, like [Moq](https://www.nuget.org/packages/Moq/).</span></span> <span data-ttu-id="6f2a1-140">`HTTP GET Index`方法有沒有迴圈或分支，而且只會呼叫一個方法。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-140">The `HTTP GET Index` method has no looping or branching and only calls one method.</span></span> <span data-ttu-id="6f2a1-141">若要測試這`Index`方法中，我們需要確認`ViewResult`會傳回與`ViewModel`從儲存機制的`List`方法。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-141">To test this `Index` method, we need to verify that a `ViewResult` is returned, with a `ViewModel` from the repository's `List` method.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

<span data-ttu-id="6f2a1-142">`HomeController` `HTTP POST Index`應該驗證方法 （如上所示）：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-142">The `HomeController` `HTTP POST Index` method (shown above) should verify:</span></span>

* <span data-ttu-id="6f2a1-143">動作方法會傳回不正確的要求`ViewResult`以適當的資料時`ModelState.IsValid`是`false`</span><span class="sxs-lookup"><span data-stu-id="6f2a1-143">The action method returns a Bad Request `ViewResult` with the appropriate data when `ModelState.IsValid` is `false`</span></span>

* <span data-ttu-id="6f2a1-144">`Add`儲存機制上的方法呼叫和`RedirectToActionResult`傳回正確的引數使用時`ModelState.IsValid`為 true。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-144">The `Add` method on the repository is called and a `RedirectToActionResult` is returned with the correct arguments when `ModelState.IsValid` is true.</span></span>

<span data-ttu-id="6f2a1-145">無效的模型狀態可以透過加入使用錯誤測試`AddModelError`底下第一項測試中所示。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-145">Invalid model state can be tested by adding errors using `AddModelError` as shown in the first test below.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

<span data-ttu-id="6f2a1-146">第一項測試會確認當`ModelState`無效，相同`ViewResult`傳回與`GET`要求。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-146">The first test confirms when `ModelState` is not valid, the same `ViewResult` is returned as for a `GET` request.</span></span> <span data-ttu-id="6f2a1-147">請注意，測試不會嘗試將傳入無效的模型。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-147">Note that the test doesn't attempt to pass in an invalid model.</span></span> <span data-ttu-id="6f2a1-148">不會運作，還是因為模型繫結不執行 (雖然[整合測試](xref:mvc/controllers/testing#integration-testing)會使用練習模型繫結)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-148">That wouldn't work anyway since model binding isn't running (though an [integration test](xref:mvc/controllers/testing#integration-testing) would use exercise model binding).</span></span> <span data-ttu-id="6f2a1-149">在此情況下，模型繫結不是正在測試。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-149">In this case, model binding is not being tested.</span></span> <span data-ttu-id="6f2a1-150">這些單元測試只有測試動作方法中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-150">These unit tests are only testing what the code in the action method does.</span></span>

<span data-ttu-id="6f2a1-151">第二個測試會驗證當`ModelState`是有效的新`BrainstormSession`加入 （透過儲存機制），而且方法會傳回`RedirectToActionResult`與預期的屬性。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-151">The second test verifies that when `ModelState` is valid, a new `BrainstormSession` is added (via the repository), and the method returns a `RedirectToActionResult` with the expected properties.</span></span> <span data-ttu-id="6f2a1-152">模擬不呼叫的呼叫是通常會被忽略，但呼叫`Verifiable`結尾的安裝程式呼叫可讓它在測試中驗證。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-152">Mocked calls that aren't called are normally ignored, but calling `Verifiable` at the end of the setup call allows it to be verified in the test.</span></span> <span data-ttu-id="6f2a1-153">這是呼叫`mockRepo.Verify`，如果預期的方法，不會呼叫，這將會失敗的測試。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-153">This is done with the call to `mockRepo.Verify`, which will fail the test if the expected method was not called.</span></span>

> [!NOTE]
> <span data-ttu-id="6f2a1-154">此範例中使用 Moq 程式庫輕鬆地混合驗證，或"strict"，模擬與非可驗證的模擬 （也稱為 「 鬆散 「 模擬 」 或 「 虛設常式）。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-154">The Moq library used in this sample makes it easy to mix verifiable, or "strict", mocks with non-verifiable mocks (also called "loose" mocks or stubs).</span></span> <span data-ttu-id="6f2a1-155">深入了解[自訂 Moq 的模擬行為](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-155">Learn more about [customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span></span>

<span data-ttu-id="6f2a1-156">應用程式中的另一個控制站會顯示與特定腦力激盪工作階段相關的資訊。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-156">Another controller in the app displays information related to a particular brainstorming session.</span></span> <span data-ttu-id="6f2a1-157">它包含某些邏輯，以處理無效的識別碼值：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-157">It includes some logic to deal with invalid id values:</span></span>

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

<span data-ttu-id="6f2a1-158">控制器動作有三種情況，若要測試，其中每個`return`陳述式：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-158">The controller action has three cases to test, one for each `return` statement:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

<span data-ttu-id="6f2a1-159">應用程式公開 web API （腦力激和方法的新概念加入至工作階段相關聯的想法清單） 的功能：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-159">The app exposes functionality as a web API (a list of ideas associated with a brainstorming session and a method for adding new ideas to a session):</span></span>

<a name="ideas-controller"></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

<span data-ttu-id="6f2a1-160">`ForSession`方法會傳回一份`IdeaDTO`型別。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-160">The `ForSession` method returns a list of `IdeaDTO` types.</span></span> <span data-ttu-id="6f2a1-161">避免直接透過 API 呼叫傳回商業網域項目，因為經常包括更多超過 API 用戶端要求，而且它們不必要地結合您的應用程式的內部網域模型與應用程式開發介面公開 （expose） 外部的資料。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-161">Avoid returning your business domain entities directly via API calls, since frequently they include more data than the API client requires, and they unnecessarily couple your app's internal domain model with the API you expose externally.</span></span> <span data-ttu-id="6f2a1-162">網域實體與您透過網路將傳回的型別之間的對應動作可手動 (使用 LINQ`Select`如下所示) 或使用程式庫如[AutoMapper](https://github.com/AutoMapper/AutoMapper)</span><span class="sxs-lookup"><span data-stu-id="6f2a1-162">Mapping between domain entities and the types you will return over the wire can be done manually (using a LINQ `Select` as shown here) or using a library like [AutoMapper](https://github.com/AutoMapper/AutoMapper)</span></span>

<span data-ttu-id="6f2a1-163">適用於單元測試`Create`和`ForSession`API 方法：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-163">The unit tests for the `Create` and `ForSession` API methods:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

<span data-ttu-id="6f2a1-164">如前所述，若要測試之方法的行為時`ModelState`無效，將模型錯誤加入至控制器，做為測試的一部分。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-164">As stated previously, to test the behavior of the method when `ModelState` is invalid, add a model error to the controller as part of the test.</span></span> <span data-ttu-id="6f2a1-165">請勿嘗試在您的單元測試中測試模型驗證或模型繫結-只是測試動作方法的行為與特定距離`ModelState`值。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-165">Don't try to test model validation or model binding in your unit tests - just test your action method's behavior when confronted with a particular `ModelState` value.</span></span>

<span data-ttu-id="6f2a1-166">第二個測試的儲存機制，讓模擬的儲存機制設定成傳回 null 則傳回 null，而定。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-166">The second test depends on the repository returning null, so the mock repository is configured to return null.</span></span> <span data-ttu-id="6f2a1-167">不需要建立測試資料庫 (在記憶體中或其他方式)，然後建構查詢，傳回這個結果-可以透過在單一陳述式所示。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-167">There's no need to create a test database (in memory or otherwise) and construct a query that will return this result - it can be done in a single statement as shown.</span></span>

<span data-ttu-id="6f2a1-168">最後一次測試會驗證儲存機制的`Update`方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-168">The last test verifies that the repository's `Update` method is called.</span></span> <span data-ttu-id="6f2a1-169">我們在先前呼叫模擬`Verifiable`並接著模擬儲存機制的`Verify`呼叫方法，確認已執行的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-169">As we did previously, the mock is called with `Verifiable` and then the mocked repository's `Verify` method is called to confirm the verifiable method was executed.</span></span> <span data-ttu-id="6f2a1-170">它不是單元測試必須負責確保`Update`方法儲存的資料，則可以完成整合測試。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-170">It's not a unit test responsibility to ensure that the `Update` method saved the data; that can be done with an integration test.</span></span>

## <a name="integration-testing"></a><span data-ttu-id="6f2a1-171">整合測試</span><span class="sxs-lookup"><span data-stu-id="6f2a1-171">Integration testing</span></span>

<span data-ttu-id="6f2a1-172">[整合測試](../../testing/integration-testing.md)為了確保您的應用程式工作中的個別模組的正確一起。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-172">[Integration testing](../../testing/integration-testing.md) is done to ensure separate modules within your app work correctly together.</span></span> <span data-ttu-id="6f2a1-173">一般而言，任何您可以測試與單元測試，您也可以測試具有整合測試，但是反向並不正確。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-173">Generally, anything you can test with a unit test, you can also test with an integration test, but the reverse isn't true.</span></span> <span data-ttu-id="6f2a1-174">但是，整合測試通常比單元測試變得很慢。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-174">However, integration tests tend to be much slower than unit tests.</span></span> <span data-ttu-id="6f2a1-175">因此，最好測試任何單元測試，可以和整合測試用於包含多個共同作業者案例。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-175">Thus, it's best to test whatever you can with unit tests, and use integration tests for scenarios that involve multiple collaborators.</span></span>

<span data-ttu-id="6f2a1-176">雖然它們仍然可能會很有用，但很少會整合測試中使用模擬物件。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-176">Although they may still be useful, mock objects are rarely used in integration tests.</span></span> <span data-ttu-id="6f2a1-177">在單元測試，模擬物件會是有效的方式來控制共同作業者之外所測試的單元測試目的的行為方式。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-177">In unit testing, mock objects are an effective way to control how collaborators outside of the unit being tested should behave for the purposes of the test.</span></span> <span data-ttu-id="6f2a1-178">在整合測試，實際的共同作業者可用來確認整個子系統一起正常運作。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-178">In an integration test, real collaborators are used to confirm the whole subsystem works together correctly.</span></span>

### <a name="application-state"></a><span data-ttu-id="6f2a1-179">應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="6f2a1-179">Application state</span></span>

<span data-ttu-id="6f2a1-180">執行整合測試時的一個重要考量是如何設定您的應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-180">One important consideration when performing integration testing is how to set your app's state.</span></span> <span data-ttu-id="6f2a1-181">測試需要執行彼此獨立，因此每個測試的開頭應為已知的狀態中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-181">Tests need to run independent of one another, and so each test should start with the app in a known state.</span></span> <span data-ttu-id="6f2a1-182">如果您的應用程式不會使用資料庫，或是有任何持續性，這可能不是問題。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-182">If your app doesn't use a database or have any persistence, this may not be an issue.</span></span> <span data-ttu-id="6f2a1-183">不過，大部分的真實世界應用程式保存到某種資料存放區，其狀態，讓任何一項測試所做的修改無法影響另一個測試，除非資料存放區會重設。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-183">However, most real-world apps persist their state to some kind of data store, so any modifications made by one test could impact another test unless the data store is reset.</span></span> <span data-ttu-id="6f2a1-184">使用內建`TestServer`，它是非常簡單，我們的整合測試內的主機 ASP.NET Core 應用程式，但是，不一定會授與它將會使用的資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-184">Using the built-in `TestServer`, it's very straightforward to host ASP.NET Core apps within our integration tests, but that doesn't necessarily grant access to the data it will use.</span></span> <span data-ttu-id="6f2a1-185">如果您使用實際的資料庫，其中一個方法是應用程式連接到測試資料庫，其可以存取您的測試，並確認會重設為已知的狀態。 每項測試執行之前。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-185">If you're using an actual database, one approach is to have the app connect to a test database, which your tests can access and ensure is reset to a known state before each test executes.</span></span>

<span data-ttu-id="6f2a1-186">在此範例應用程式中，我在使用 Entity Framework Core 的 InMemoryDatabase 支援，因此我無法只連線到它，從我的測試專案。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-186">In this sample application, I'm using Entity Framework Core's InMemoryDatabase support, so I can't just connect to it from my test project.</span></span> <span data-ttu-id="6f2a1-187">相反地，我公開`InitializeDatabase`方法從應用程式的`Startup`類別，如果是在應用程式啟動時，我呼叫`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-187">Instead, I expose an `InitializeDatabase` method from the app's `Startup` class, which I call when the app starts up if it's in the `Development` environment.</span></span> <span data-ttu-id="6f2a1-188">我的整合測試會自動從中獲益，只要它們將環境設定為`Development`。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-188">My integration tests automatically benefit from this as long as they set the environment to `Development`.</span></span> <span data-ttu-id="6f2a1-189">我不必擔心重設資料庫，因為應用程式重新啟動每次重設 InMemoryDatabase。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-189">I don't have to worry about resetting the database, since the InMemoryDatabase is reset each time the app restarts.</span></span>

<span data-ttu-id="6f2a1-190">`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-190">The `Startup` class:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

<span data-ttu-id="6f2a1-191">您會看到`GetTestSession`經常用於下列整合測試的方法。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-191">You'll see the `GetTestSession` method used frequently in the integration tests below.</span></span>

### <a name="accessing-views"></a><span data-ttu-id="6f2a1-192">存取檢視</span><span class="sxs-lookup"><span data-stu-id="6f2a1-192">Accessing views</span></span>

<span data-ttu-id="6f2a1-193">每個整合測試類別設定`TestServer`執行 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-193">Each integration test class configures the `TestServer` that will run the ASP.NET Core app.</span></span> <span data-ttu-id="6f2a1-194">根據預設，`TestServer`裝載 web 應用程式執行所在-在此情況下，測試專案資料夾的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-194">By default, `TestServer` hosts the web app in the folder where it's running - in this case, the test project folder.</span></span> <span data-ttu-id="6f2a1-195">因此，當您嘗試測試控制器動作傳回`ViewResult`，您可能會看到這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-195">Thus, when you attempt to test controller actions that return `ViewResult`, you may see this error:</span></span>

```
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

<span data-ttu-id="6f2a1-196">若要更正此問題，您需要設定伺服器的內容的根目錄，因此它可以尋找受測專案的檢視。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-196">To correct this issue, you need to configure the server's content root, so it can locate the views for the project being tested.</span></span> <span data-ttu-id="6f2a1-197">這是由呼叫`UseContentRoot`中`TestFixture`類別，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-197">This is done by a call to `UseContentRoot` in the `TestFixture` class, shown below:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

<span data-ttu-id="6f2a1-198">`TestFixture`類別會負責設定及建立`TestServer`、 設定`HttpClient`與通訊`TestServer`。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-198">The `TestFixture` class is responsible for configuring and creating the `TestServer`, setting up an `HttpClient` to communicate with the `TestServer`.</span></span> <span data-ttu-id="6f2a1-199">整合的每一個都會測試使用`Client`連接到測試伺服器，並提出要求的屬性。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-199">Each of the integration tests uses the `Client` property to connect to the test server and make a request.</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

<span data-ttu-id="6f2a1-200">在上述第一項測試`responseString`保存實際的方式轉譯 HTML 檢視中，可以檢查以確認它包含預期的結果。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-200">In the first test above, the `responseString` holds the actual rendered HTML from the View, which can be inspected to confirm it contains expected results.</span></span>

<span data-ttu-id="6f2a1-201">第二個測試建構表單 POST 具有唯一的工作階段名稱並將它張貼至應用程式，然後驗證會傳回預期的重新導向。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-201">The second test constructs a form POST with a unique session name and POSTs it to the app, then verifies that the expected redirect is returned.</span></span>

### <a name="api-methods"></a><span data-ttu-id="6f2a1-202">API 方法</span><span class="sxs-lookup"><span data-stu-id="6f2a1-202">API methods</span></span>

<span data-ttu-id="6f2a1-203">如果您的應用程式公開 web Api，它的建議讓自動化測試，確認如預期般執行。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-203">If your app exposes web APIs, it's a good idea to have automated tests confirm they execute as expected.</span></span> <span data-ttu-id="6f2a1-204">內建`TestServer`輕鬆測試 web 應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-204">The built-in `TestServer` makes it easy to test web APIs.</span></span> <span data-ttu-id="6f2a1-205">如果您應用程式開發介面的方法使用模型繫結，請一律檢查`ModelState.IsValid`，和整合測試是正確的位置，以確認您的模型驗證是否正常運作。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-205">If your API methods are using model binding, you should always check `ModelState.IsValid`, and integration tests are the right place to confirm that your model validation is working properly.</span></span>

<span data-ttu-id="6f2a1-206">下列測試的目標集`Create`方法中的[IdeasController](xref:mvc/controllers/testing#ideas-controller)類別如上所示：</span><span class="sxs-lookup"><span data-stu-id="6f2a1-206">The following set of tests target the `Create` method in the [IdeasController](xref:mvc/controllers/testing#ideas-controller) class shown above:</span></span>

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

<span data-ttu-id="6f2a1-207">與不同的是整合測試的 HTML 檢視會傳回的動作，web 應用程式開發介面方法會傳回結果，通常可以還原序列化為強型別物件，如上面最後一次測試所示。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-207">Unlike integration tests of actions that returns HTML views, web API methods that return results can usually be deserialized as strongly typed objects, as the last test above shows.</span></span> <span data-ttu-id="6f2a1-208">在此情況下，測試還原序列化結果`BrainstormSession`執行個體，並確認這個概念已正確加入至其集合的概念。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-208">In this case, the test deserializes the result to a `BrainstormSession` instance, and confirms that the idea was correctly added to its collection of ideas.</span></span>

<span data-ttu-id="6f2a1-209">在這篇文章中找到其他範例的整合測試[範例專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)。</span><span class="sxs-lookup"><span data-stu-id="6f2a1-209">You'll find additional examples of integration tests in this article's [sample project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample).</span></span>
