---
title: 測試 ASP.NET Core 中的控制器邏輯
author: ardalis
description: 了解如何使用 Moq 和 xUnit 測試 ASP.NET Core 中的控制器邏輯。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751770"
---
# <a name="test-controller-logic-in-aspnet-core"></a><span data-ttu-id="61282-103">測試 ASP.NET Core 中的控制器邏輯</span><span class="sxs-lookup"><span data-stu-id="61282-103">Test controller logic in ASP.NET Core</span></span>

<span data-ttu-id="61282-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="61282-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="61282-105">控制器是任何 ASP.NET Core MVC 應用程式的核心部分。</span><span class="sxs-lookup"><span data-stu-id="61282-105">Controllers are a central part of any ASP.NET Core MVC application.</span></span> <span data-ttu-id="61282-106">因此，您應該確信其行為符合應用程式的預期。</span><span class="sxs-lookup"><span data-stu-id="61282-106">As such, you should have confidence they behave as intended for your app.</span></span> <span data-ttu-id="61282-107">自動化的測試可建立您這方面的信心，並可在錯誤到達生產環境之前就偵測到錯誤。</span><span class="sxs-lookup"><span data-stu-id="61282-107">Automated tests can provide you with this confidence and can detect errors before they reach production.</span></span> <span data-ttu-id="61282-108">請務必避免對您的控制器加諸不必要的職責，並確保您的測試只會著重於控制器職責。</span><span class="sxs-lookup"><span data-stu-id="61282-108">It's important to avoid placing unnecessary responsibilities within your controllers and ensure your tests focus only on controller responsibilities.</span></span>

<span data-ttu-id="61282-109">控制器邏輯應該是最小，且不著重於商務邏輯或基礎結構考量 (例如資料存取)。</span><span class="sxs-lookup"><span data-stu-id="61282-109">Controller logic should be minimal and not be focused on business logic or infrastructure concerns (for example, data access).</span></span> <span data-ttu-id="61282-110">測試控制器邏輯，而不是架構。</span><span class="sxs-lookup"><span data-stu-id="61282-110">Test controller logic, not the framework.</span></span> <span data-ttu-id="61282-111">根據輸入有效或無效，測試控制器的「行為」方式。</span><span class="sxs-lookup"><span data-stu-id="61282-111">Test how the controller *behaves* based on valid or invalid inputs.</span></span> <span data-ttu-id="61282-112">根據控制器執行的商務作業結果，測試控制器回應。</span><span class="sxs-lookup"><span data-stu-id="61282-112">Test controller responses based on the result of the business operation it performs.</span></span>

<span data-ttu-id="61282-113">一般控制器職責如下：</span><span class="sxs-lookup"><span data-stu-id="61282-113">Typical controller responsibilities:</span></span>

* <span data-ttu-id="61282-114">驗證 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="61282-114">Verify `ModelState.IsValid`.</span></span>
* <span data-ttu-id="61282-115">如果 `ModelState` 無效，則傳回錯誤回應。</span><span class="sxs-lookup"><span data-stu-id="61282-115">Return an error response if `ModelState` is invalid.</span></span>
* <span data-ttu-id="61282-116">從持續性儲存區擷取商務實體。</span><span class="sxs-lookup"><span data-stu-id="61282-116">Retrieve a business entity from persistence.</span></span>
* <span data-ttu-id="61282-117">在商務實體上執行動作。</span><span class="sxs-lookup"><span data-stu-id="61282-117">Perform an action on the business entity.</span></span>
* <span data-ttu-id="61282-118">將商務實體儲存至持續性儲存區。</span><span class="sxs-lookup"><span data-stu-id="61282-118">Save the business entity to persistence.</span></span>
* <span data-ttu-id="61282-119">傳回適當的 `IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="61282-119">Return an appropriate `IActionResult`.</span></span>

<span data-ttu-id="61282-120">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="61282-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="unit-tests-of-controller-logic"></a><span data-ttu-id="61282-121">控制器邏輯的單元測試</span><span class="sxs-lookup"><span data-stu-id="61282-121">Unit tests of controller logic</span></span>

<span data-ttu-id="61282-122">[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)包括將應用程式的一部分與其基礎結構和相依性隔離測試。</span><span class="sxs-lookup"><span data-stu-id="61282-122">[Unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) involve testing a part of an app in isolation from its infrastructure and dependencies.</span></span> <span data-ttu-id="61282-123">對控制器邏輯進行單元測試時，只會測試單一動作的內容，而不會測試其相依性或架構本身的行為。</span><span class="sxs-lookup"><span data-stu-id="61282-123">When unit testing controller logic, only the contents of a single action is tested, not the behavior of its dependencies or of the framework itself.</span></span> <span data-ttu-id="61282-124">當您對控制器動作進行單元測試時，請務必只著重於其行為。</span><span class="sxs-lookup"><span data-stu-id="61282-124">As you unit test your controller actions, make sure you focus only on its behavior.</span></span> <span data-ttu-id="61282-125">控制器單元測試會避開[篩選](xref:mvc/controllers/filters)、[路由](xref:fundamentals/routing)或[模型繫結](xref:mvc/models/model-binding)等作業。</span><span class="sxs-lookup"><span data-stu-id="61282-125">A controller unit test avoids things like [filters](xref:mvc/controllers/filters), [routing](xref:fundamentals/routing), or [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="61282-126">由於測試的焦點只有一個，因此單元測試通常很容易撰寫，而且執行速度很快。</span><span class="sxs-lookup"><span data-stu-id="61282-126">By focusing on testing just one thing, unit tests are generally simple to write and quick to run.</span></span> <span data-ttu-id="61282-127">一組編寫完善的單元測試可以經常執行，而不會造成太多額外負荷。</span><span class="sxs-lookup"><span data-stu-id="61282-127">A well-written set of unit tests can be run frequently without much overhead.</span></span> <span data-ttu-id="61282-128">不過，單元測試不會偵測元件之間的互動問題，這需要使用[整合測試](xref:test/integration-tests)。</span><span class="sxs-lookup"><span data-stu-id="61282-128">However, unit tests don't detect issues in the interaction between components, which is the purpose of [integration tests](xref:test/integration-tests).</span></span>

<span data-ttu-id="61282-129">如果您想要撰寫自訂篩選和路由，您應該單獨對其進行單元測試，而不是當作特定控制器動作測試的一部分。</span><span class="sxs-lookup"><span data-stu-id="61282-129">If you're writing custom filters and routes, you should unit test them in isolation, not as part of your tests on a particular controller action.</span></span>

> [!TIP]
> <span data-ttu-id="61282-130">[使用 Visual Studio 建立和執行單元測試](/visualstudio/test/unit-test-your-code)。</span><span class="sxs-lookup"><span data-stu-id="61282-130">[Create and run unit tests with Visual Studio](/visualstudio/test/unit-test-your-code).</span></span>

<span data-ttu-id="61282-131">若要示範單元測試，請檢閱下列控制器。</span><span class="sxs-lookup"><span data-stu-id="61282-131">To demonstrate unit testing, review the following controller.</span></span> <span data-ttu-id="61282-132">它顯示一份腦力激盪工作階段清單，並允許使用 POST 建立新的腦力激盪工作階段：</span><span class="sxs-lookup"><span data-stu-id="61282-132">It displays a list of brainstorming sessions and allows new brainstorming sessions to be created with a POST:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

<span data-ttu-id="61282-133">此控制器遵循 [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) (明確的相依性原則)，預期會插入相依性以提供 `IBrainstormSessionRepository` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="61282-133">The controller is following the [explicit dependencies principle](http://deviq.com/explicit-dependencies-principle/), expecting dependency injection to provide it with an instance of `IBrainstormSessionRepository`.</span></span> <span data-ttu-id="61282-134">因此可讓您使用模擬物件架構 (例如 [Moq](https://www.nuget.org/packages/Moq/)) 輕鬆進行測試。</span><span class="sxs-lookup"><span data-stu-id="61282-134">This makes it fairly easy to test using a mock object framework, like [Moq](https://www.nuget.org/packages/Moq/).</span></span> <span data-ttu-id="61282-135">`HTTP GET Index` 方法有沒有迴圈或分支，而且只會呼叫一個方法。</span><span class="sxs-lookup"><span data-stu-id="61282-135">The `HTTP GET Index` method has no looping or branching and only calls one method.</span></span> <span data-ttu-id="61282-136">若要測試此 `Index` 方法，我們需要驗證透過儲存機制的 `List` 方法可傳回 `ViewResult` 和 `ViewModel`。</span><span class="sxs-lookup"><span data-stu-id="61282-136">To test this `Index` method, we need to verify that a `ViewResult` is returned, with a `ViewModel` from the repository's `List` method.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

<span data-ttu-id="61282-137">`HomeController` `HTTP POST Index` 方法 (如上所示) 應該會驗證：</span><span class="sxs-lookup"><span data-stu-id="61282-137">The `HomeController` `HTTP POST Index` method (shown above) should verify:</span></span>

* <span data-ttu-id="61282-138">當 `ModelState.IsValid` 為 `false` 時，動作方法會傳回不正確的要求 `ViewResult` 及適當的資料。</span><span class="sxs-lookup"><span data-stu-id="61282-138">The action method returns a Bad Request `ViewResult` with the appropriate data when `ModelState.IsValid` is `false`.</span></span>

* <span data-ttu-id="61282-139">當 `ModelState.IsValid` 為 true 時，會呼叫儲存機制的 `Add` 方法，並傳回具有正確引數的 `RedirectToActionResult`。</span><span class="sxs-lookup"><span data-stu-id="61282-139">The `Add` method on the repository is called and a `RedirectToActionResult` is returned with the correct arguments when `ModelState.IsValid` is true.</span></span>

<span data-ttu-id="61282-140">您可以使用 `AddModelError` 新增錯誤，來測試無效的模型狀態，如下列第一項測試中所示。</span><span class="sxs-lookup"><span data-stu-id="61282-140">Invalid model state can be tested by adding errors using `AddModelError` as shown in the first test below.</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

<span data-ttu-id="61282-141">第一項測試會驗證當 `ModelState` 無效時，會傳回與 `GET` 要求相同的 `ViewResult`。</span><span class="sxs-lookup"><span data-stu-id="61282-141">The first test confirms when `ModelState` isn't valid, the same `ViewResult` is returned as for a `GET` request.</span></span> <span data-ttu-id="61282-142">請注意，此測試不會嘗試傳入無效的模型。</span><span class="sxs-lookup"><span data-stu-id="61282-142">Note that the test doesn't attempt to pass in an invalid model.</span></span> <span data-ttu-id="61282-143">即使傳入也沒有作用，因為模型繫結並未執行 (但[整合測試](xref:test/integration-tests)會使用練習模型繫結)。</span><span class="sxs-lookup"><span data-stu-id="61282-143">That wouldn't work anyway since model binding isn't running (though an [integration test](xref:test/integration-tests) would use exercise model binding).</span></span> <span data-ttu-id="61282-144">在此情況下，不會測試模型繫結。</span><span class="sxs-lookup"><span data-stu-id="61282-144">In this case, model binding isn't being tested.</span></span> <span data-ttu-id="61282-145">這些單元測試只會測試動作方法中的程式碼功能。</span><span class="sxs-lookup"><span data-stu-id="61282-145">These unit tests are only testing what the code in the action method does.</span></span>

<span data-ttu-id="61282-146">第二項測試會驗證當 `ModelState` 有效時，會新增新的 `BrainstormSession` (透過儲存機制)，而且方法會傳回具有預期屬性的 `RedirectToActionResult`。</span><span class="sxs-lookup"><span data-stu-id="61282-146">The second test verifies that when `ModelState` is valid, a new `BrainstormSession` is added (via the repository), and the method returns a `RedirectToActionResult` with the expected properties.</span></span> <span data-ttu-id="61282-147">通常會忽略未呼叫的模擬呼叫，但在安裝程式呼叫結束時呼叫 `Verifiable` 即可在測試中對其進行驗證。</span><span class="sxs-lookup"><span data-stu-id="61282-147">Mocked calls that aren't called are normally ignored, but calling `Verifiable` at the end of the setup call allows it to be verified in the test.</span></span> <span data-ttu-id="61282-148">這會透過呼叫 `mockRepo.Verify` 來完成，如果未呼叫預期的方法，則測試會失敗。</span><span class="sxs-lookup"><span data-stu-id="61282-148">This is done with the call to `mockRepo.Verify`, which will fail the test if the expected method wasn't called.</span></span>

> [!NOTE]
> <span data-ttu-id="61282-149">此範例中所使用的 Moq 程式庫可讓您輕鬆混合可驗證 (或「嚴格」) 的模擬與無法驗證的模擬 (也稱為「鬆散」的模擬或虛設常式)。</span><span class="sxs-lookup"><span data-stu-id="61282-149">The Moq library used in this sample makes it easy to mix verifiable, or "strict", mocks with non-verifiable mocks (also called "loose" mocks or stubs).</span></span> <span data-ttu-id="61282-150">如需詳細資訊，請參閱 [Customizing Mock Behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior) (使用 Moq 自訂模擬行為)。</span><span class="sxs-lookup"><span data-stu-id="61282-150">Learn more about [customizing Mock behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior).</span></span>

<span data-ttu-id="61282-151">應用程式中的另一個控制器會顯示與特定腦力激盪工作階段相關的資訊。</span><span class="sxs-lookup"><span data-stu-id="61282-151">Another controller in the app displays information related to a particular brainstorming session.</span></span> <span data-ttu-id="61282-152">其中包含一些可處理無效識別碼值的邏輯：</span><span class="sxs-lookup"><span data-stu-id="61282-152">It includes some logic to deal with invalid id values:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

<span data-ttu-id="61282-153">此控制器動作有三個待測試案例，每個 `return` 陳述式各代表一個案例：</span><span class="sxs-lookup"><span data-stu-id="61282-153">The controller action has three cases to test, one for each `return` statement:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

<span data-ttu-id="61282-154">此應用程式會以 Web API 形式公開功能 (一份與腦力激盪工作階段建立關聯的想法清單，以及一個將新的想法新增工作清單的方法)：</span><span class="sxs-lookup"><span data-stu-id="61282-154">The app exposes functionality as a web API (a list of ideas associated with a brainstorming session and a method for adding new ideas to a session):</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

<span data-ttu-id="61282-155">`ForSession` 方法會傳回 `IdeaDTO` 類型清單。</span><span class="sxs-lookup"><span data-stu-id="61282-155">The `ForSession` method returns a list of `IdeaDTO` types.</span></span> <span data-ttu-id="61282-156">請避免直接透過 API 呼叫來傳回公司領域實體，因為這些實體通常包含比 API 用戶端所需更多的資料，而且會不必要地結合您應用程式的內部領域模型與您對外公開的 API。</span><span class="sxs-lookup"><span data-stu-id="61282-156">Avoid returning your business domain entities directly via API calls, since frequently they include more data than the API client requires, and they unnecessarily couple your app's internal domain model with the API you expose externally.</span></span> <span data-ttu-id="61282-157">您可以手動 (使用此處所示的 LINQ `Select`) 或使用 [AutoMapper](https://github.com/AutoMapper/AutoMapper) 等程式庫，來對應領域實體與您將透過網路傳回的類型。</span><span class="sxs-lookup"><span data-stu-id="61282-157">Mapping between domain entities and the types you will return over the wire can be done manually (using a LINQ `Select` as shown here) or using a library like [AutoMapper](https://github.com/AutoMapper/AutoMapper).</span></span>

<span data-ttu-id="61282-158">`Create` 和 `ForSession` API 方法的單元測試為：</span><span class="sxs-lookup"><span data-stu-id="61282-158">The unit tests for the `Create` and `ForSession` API methods:</span></span>

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

<span data-ttu-id="61282-159">如前所述，若要測試當 `ModelState` 無效時的方法行為，請將模型錯誤新增控制器作為測試的一部分。</span><span class="sxs-lookup"><span data-stu-id="61282-159">As stated previously, to test the behavior of the method when `ModelState` is invalid, add a model error to the controller as part of the test.</span></span> <span data-ttu-id="61282-160">請勿嘗試在您的單元測試中測試模型驗證或模型繫結，只要測試當遇到特定 `ModelState` 值時的動作方法行為即可。</span><span class="sxs-lookup"><span data-stu-id="61282-160">Don't try to test model validation or model binding in your unit tests - just test your action method's behavior when confronted with a particular `ModelState` value.</span></span>

<span data-ttu-id="61282-161">第二項測試需要儲存機制傳回 Null，因此會設定模擬儲存機制傳回 Null。</span><span class="sxs-lookup"><span data-stu-id="61282-161">The second test depends on the repository returning null, so the mock repository is configured to return null.</span></span> <span data-ttu-id="61282-162">不需要建立測試資料庫 (在記憶體中或其他位置)，也不需要建構傳回此結果的查詢，這會透過上述的單一陳述式來完成。</span><span class="sxs-lookup"><span data-stu-id="61282-162">There's no need to create a test database (in memory or otherwise) and construct a query that will return this result - it can be done in a single statement as shown.</span></span>

<span data-ttu-id="61282-163">最後一項測試會驗證是否呼叫儲存機制的 `Update` 方法。</span><span class="sxs-lookup"><span data-stu-id="61282-163">The last test verifies that the repository's `Update` method is called.</span></span> <span data-ttu-id="61282-164">如前所述，先使用 `Verifiable` 呼叫模擬，再呼叫模擬儲存機制的 `Verify` 方法，確認是否已執行可驗證的方法。</span><span class="sxs-lookup"><span data-stu-id="61282-164">As we did previously, the mock is called with `Verifiable` and then the mocked repository's `Verify` method is called to confirm the verifiable method was executed.</span></span> <span data-ttu-id="61282-165">單元測試無須負責確保 `Update` 方法已儲存資料，這會透過整合測試來完成。</span><span class="sxs-lookup"><span data-stu-id="61282-165">It's not a unit test responsibility to ensure that the `Update` method saved the data; that can be done with an integration test.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61282-166">其他資源</span><span class="sxs-lookup"><span data-stu-id="61282-166">Additional resources</span></span>

* <xref:test/integration-tests>
