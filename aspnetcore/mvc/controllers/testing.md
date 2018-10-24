---
title: 測試 ASP.NET Core 中的控制器邏輯
author: ardalis
description: 了解如何使用 Moq 和 xUnit 測試 ASP.NET Core 中的控制器邏輯。
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2018
uid: mvc/controllers/testing
ms.openlocfilehash: f036181f43d12ece89243fa3b0b0070ea84f8bc7
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010984"
---
# <a name="test-controller-logic-in-aspnet-core"></a>測試 ASP.NET Core 中的控制器邏輯

作者：[Steve Smith](https://ardalis.com/)

[控制器](xref:mvc/controllers/actions)在任何 ASP.NET Core MVC 應用程式中都扮演重要角色。 因此，您應該確信控制器的行為符合預期。 在將應用程式部署至生產環境前，自動化測試可以偵測錯誤。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="unit-tests-of-controller-logic"></a>控制器邏輯的單元測試

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)包括將應用程式的一部分與其基礎結構和相依性隔離測試。 對控制器邏輯進行單元測試時，只會測試單一動作的內容，而不會測試其相依性或架構本身的行為。

設定單元測試的控制器動作，以專注於控制器的行為。 控制器單元測試會避開[篩選](xref:mvc/controllers/filters)、[路由](xref:fundamentals/routing)和[模型繫結](xref:mvc/models/model-binding)等情況。 涵蓋共同回應要求之元件之間互動的測試，都由*整合測試*處理。 如需整合測試的詳細資訊，請參閱 <xref:test/integration-tests>。

如果您想要撰寫自訂篩選和路由，請單獨對其進行單元測試，而不是當作特定控制器動作測試的一部分。

若要進行控制器單元測試，請檢閱下列範例應用程式中的控制器。 主控制器會顯示一份腦力激盪工作階段清單，並允許使用 POST 要求來建立新的腦力激盪工作階段：

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?name=snippet_HomeController&highlight=1,5,10,31-32)]

上述控制器會：

* 遵循[明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。
* 預期[相依性插入 (DI)](xref:fundamentals/dependency-injection) 以提供 `IBrainstormSessionRepository` 的執行個體。
* 可以使用模擬物件架構 (例如 [Moq](https://www.nuget.org/packages/Moq/)) 透過模擬 `IBrainstormSessionRepository` 服務進行測試。 「模擬物件」是製作出來的物件，具有一組用於測試的預定屬性和方法行為。 如需詳細資訊，請參閱[整合測試簡介](xref:test/integration-tests#introduction-to-integration-tests)。

`HTTP GET Index` 方法有沒有迴圈或分支，而且只會呼叫一個方法。 此動作的單元測試：

* 使用 `IBrainstormSessionRepository` 方法來模擬 `GetTestSessions` 服務。 `GetTestSessions` 會建立兩個具有日期和工作階段名稱的模擬腦力激盪工作階段。
* 執行 `Index` 方法。
* 對方法傳回的結果進行判斷提示：
  * 會傳回 <xref:Microsoft.AspNetCore.Mvc.ViewResult>。
  * [ViewDataDictionary.Model](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary.Model*) 是 `StormSessionViewModel`。
  * 在 `ViewDataDictionary.Model` 中有兩個腦力激盪工作階段。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_Index_ReturnsAViewResult_WithAListOfBrainstormSessions&highlight=14-17)]

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_GetTestSessions)]

主控制器的 `HTTP POST Index` 方法測試會驗證：

* 當 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid*) 為 `false` 時，動作方法會傳回具有適當資料的 *400 Bad Request* <xref:Microsoft.AspNetCore.Mvc.ViewResult>。
* 當 `ModelState.IsValid` 為 `true`：
  * 會呼叫 `Add` 存放庫上的方法。
  * 會傳回具有正確引數的 <xref:Microsoft.AspNetCore.Mvc.RedirectToActionResult>。

您可以使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 新增錯誤，來測試無效的模型狀態，如下列第一項測試中所示：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?name=snippet_ModelState_ValidOrInvalid&highlight=9,16-17,38-41)]

當 [ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary) 無效時，會傳回與 GET 要求相同的 `ViewResult`。 此測試不會嘗試傳入無效的模型。 傳遞無效模型不是有效的方法，因為模型繫結並未執行 (儘管[整合測試](xref:test/integration-tests)確實使用模型繫結)。 在此情況下，不會測試模型繫結。 這些單元測試只會測試動作方法中的程式碼。

第二項測試會驗證 `ModelState` 何時有效：

* 新增 `BrainstormSession` (透過[存放庫](xref:fundamentals/repository-pattern))。
* 此方法會以預期屬性傳回 `RedirectToActionResult`。

通常會忽略未呼叫的模擬呼叫，但在安裝程式呼叫結束時呼叫 `Verifiable` 即可在測試中對其進行模擬驗證。 可透過呼叫 `mockRepo.Verify` 來執行，如果未呼叫預期的方法，則測試會失敗。

> [!NOTE]
> 此範例中所使用的 Moq 程式庫，可讓您混合可驗證 (或「嚴格」) 的模擬，以及無法驗證的模擬 (也稱為「鬆散」的模擬或虛設常式)。 如需詳細資訊，請參閱 [Customizing Mock Behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior) (使用 Moq 自訂模擬行為)。

範例應用程式中的另一個控制器，會顯示與特定腦力激盪工作階段相關的資訊。 控制器包含處理無效 `id` 值的邏輯 (下列範例中有兩個 `return` 案例來說明這些案例)。 最終 `return` 陳述式會將新的 `StormSessionViewModel` 傳回至檢視：

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?name=snippet_SessionController&highlight=12-16,18-22,31)]

單元測試包含工作階段控制器 `Index` 動作中每個 `return` 案例的一項測試：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?name=snippet_SessionControllerTests&highlight=2,11-14,18,31-32,36,50-55)]

當移動至構想控制器，應用程式會將功能公開為 `api/ideas` 路徑上的 Web API：

* `ForSession` 方法會傳回與腦力激盪工作階段建立關聯的構想清單 (`IdeaDTO`)。
* `Create` 方法會將新構想新增至工作階段。

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionAndCreate&highlight=1-2,21-22)]

請避免直接透過 API 呼叫來傳回商務網域實體。 領域實體：

* 通常會包含超過用戶端所需的資料。
* 會不必要地將應用程式內部網域模型與公用公開的 API 結合在一起。

可以執行網域實體與傳回給用戶端之類型間的對應：

* 透過 LINQ `Select` 手動進行，如範例應用程式使用的方式一樣。 如需詳細資訊，請參閱 [LINQ (Language Integrated Query)](/dotnet/standard/using-linq)。
* 透過程式庫自動進行，例如 [AutoMapper](https://github.com/AutoMapper/AutoMapper)。

接下來，範例應用程式會示範構想控制器 `Create` 和 `ForSession` API 方法的單元測試。

範例應用程式包含兩項 `ForSession` 測試。 第一項測試會判斷 `ForSession` 是否會為無效的工作階段傳回 <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult> (找不到 HTTP)：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests4&highlight=5,7-8,15-16)]

第二項 `ForSession` 測試會判斷 `ForSession` 是否會傳有效工作階段的工作階段構想清單 (`<List<IdeaDTO>>`)。 檢查也會查驗第一個構想，以確認其 `Name` 屬性是否正確：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests5&highlight=5,7-8,15-18)]

為了測試 `ModelState` 無效時的 `Create` 方法行為，範例應用程式會將模型錯誤新增至控制器作為測試的一部分。 請勿嘗試在單元測試中測試模型驗證或模型繫結&mdash;只要測試當遇到無效 `ModelState` 時的動作方法行為即可：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests1&highlight=7,13)]

第二項 `Create` 測試需要儲存機制傳回 `null`，因此會設定模擬儲存機制傳回 `null`。 不需要建立測試資料庫 (在記憶體中或其他位置)，也不需要建構傳回此結果的查詢。 測試可在單一陳述式中完成，如範例程式碼所示：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests2&highlight=7-8,15)]

第三項 `Create` 測試 (`Create_ReturnsNewlyCreatedIdeaForSession`) 會確認是否呼叫存放庫的 `UpdateAsync` 方法。 會使用 `Verifiable` 呼叫模擬，再呼叫模擬存放庫的 `Verify` 方法，確認是否已執行可驗證的方法。 單元測試無須負責確保 `UpdateAsync` 方法已儲存資料&mdash;而是透過整合測試來完成。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ApiIdeasControllerTests3&highlight=20-22,28-33)]

::: moniker range=">= aspnetcore-2.1"

## <a name="test-actionresultlttgt"></a>測試 ActionResult&lt;T&gt;

在 ASP.NET Core 2.1 或更新版本中，[ActionResult&lt;T&gt;](xref:web-api/action-return-types#actionresultt-type) (<xref:Microsoft.AspNetCore.Mvc.ActionResult`1>) 可讓您傳回衍生自 `ActionResult` 的類型或傳回特定類型。

範例應用程式包含為指定工作階段 `id` 傳回 `List<IdeaDTO>` 的方法。 如果工作階段 `id` 不存在，則控制器會傳回 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>：

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_ForSessionActionResult&highlight=10,21)]

`ForSessionActionResult` 控制器的兩項測試會包含於 `ApiIdeasControllerTests`。

第一項測試會確認控制器傳回 `ActionResult`，而不是為不存在之工作階段 `id` 傳回不存在的構想清單：

* `ActionResult` 類型為 `ActionResult<List<IdeaDTO>>`。
* <xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*> 是 <xref:Microsoft.AspNetCore.Mvc.NotFoundObjectResult>。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=7,10,13-14)]

針對有效的工作階段 `id`，第二項測試會確認此方法傳回：

* 具有 `ActionResult` 類型的 `List<IdeaDTO>`。
* [ActionResult&lt;T&gt;.Value](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) 是 `List<IdeaDTO>` 類型。
* 清單中的第一個項目，是與儲存在模擬工作階段中構想相符的有效構想 (透過呼叫 `GetTestSession` 來取得)。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_ForSessionActionResult_ReturnsIdeasForSession&highlight=7-8,15-18)]

範例應用程式也包含方法，為指定工作階段建立新的 `Idea`。 控制器會傳回：

* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> (針對無效的模型)。
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> (如果工作階段不存在)。
* <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> (以新構想更新工作階段時)。

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?name=snippet_CreateActionResult&highlight=9,16,29)]

`CreateActionResult` 的三項測試都包含在 `ApiIdeasControllerTests` 中。

第一項測試會確認為無效的模型傳回 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsBadRequest_GivenInvalidModel&highlight=7,13-14)]

如果工作階段不存在，第二項測試將檢查是否傳回 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNotFoundObjectResultForNonexistentSession&highlight=5,15,22-23)]

針對有效的工作階段 `id`，最終測試會確認：

* 此方法會以 `BrainstormSession` 類型傳回 `ActionResult`。
* [ActionResult&lt;T&gt;.Result](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Result*) 是 <xref:Microsoft.AspNetCore.Mvc.CreatedAtActionResult>. `CreatedAtActionResult` 類似於具有 `Location` 標頭尸的「201 已建立」回應。
* [ActionResult&lt;T&gt;.Value](xref:Microsoft.AspNetCore.Mvc.ActionResult`1.Value*) 是 `BrainstormSession` 類型。
* 已叫用更新工作階段 (`UpdateAsync(testSession)`) 的模擬呼叫。 `Verifiable` 方法呼叫會透過在判斷提示中執行 `mockRepo.Verify()` 來檢查。
* 會為工作階段傳回兩個 `Idea` 物件。
* 最後一個項目 (模擬呼叫 `UpdateAsync` 新增的 `Idea`) 會與在測試中新增至工作階段的 `newIdea` 相符。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?name=snippet_CreateActionResult_ReturnsNewlyCreatedIdeaForSession&highlight=20-22,28-34)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:test/index>
* <xref:test/integration-tests>
* [使用 Visual Studio 建立和執行單元測試](/visualstudio/test/unit-test-your-code)。
* <xref:fundamentals/repository-pattern>
* [明確相依性準則](https://deviq.com/explicit-dependencies-principle/)
