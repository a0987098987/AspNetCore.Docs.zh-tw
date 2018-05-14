---
title: 測試 ASP.NET Core 中的控制器邏輯
author: ardalis
description: 了解如何使用 Moq 和 xUnit 測試 ASP.NET Core 中的控制器邏輯。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/testing
ms.openlocfilehash: 51b7a02c697807c9e3504b70f89370126ee0e781
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="test-controller-logic-in-aspnet-core"></a>測試 ASP.NET Core 中的控制器邏輯

作者：[Steve Smith](https://ardalis.com/)

ASP.NET MVC 應用程式中的控制器應該很小，並著重於使用者介面考量。 處理非 UI 考量的大型控制器會比較難測試及維護。

[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>測試控制器

控制器是任何 ASP.NET Core MVC 應用程式的核心部分。 因此，您應該確信其行為符合應用程式的預期。 自動化的測試可建立您這方面的信心，並可在錯誤到達生產環境之前就偵測到錯誤。 請務必避免對您的控制器加諸不必要的職責，並確保您的測試只會著重於控制器職責。

控制器邏輯應該是最小，且不著重於商務邏輯或基礎結構考量 (例如資料存取)。 測試控制器邏輯，而不是架構。 根據輸入有效或無效，測試控制器的「行為」方式。 根據控制器執行的商務作業結果，測試控制器回應。

一般控制器職責如下：

* 驗證 `ModelState.IsValid`。
* 如果 `ModelState` 無效，則傳回錯誤回應。
* 從持續性儲存區擷取商務實體。
* 在商務實體上執行動作。
* 將商務實體儲存至持續性儲存區。
* 傳回適當的 `IActionResult`。

## <a name="unit-testing"></a>單元測試

[單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)包括將應用程式的一部分與其基礎結構和相依性隔離測試。 對控制器邏輯進行單元測試時，只會測試單一動作的內容，而不會測試其相依性或架構本身的行為。 當您對控制器動作進行單元測試時，請務必只著重於其行為。 控制器單元測試會避開[篩選](filters.md)、[路由](../../fundamentals/routing.md)或[模型繫結](../models/model-binding.md)等作業。 由於測試的焦點只有一個，因此單元測試通常很容易撰寫，而且執行速度很快。 一組編寫完善的單元測試可以經常執行，而不會造成太多額外負荷。 不過，單元測試不會偵測元件之間的互動問題，這需要使用[整合測試](xref:mvc/controllers/testing#integration-testing)。

如果您想要撰寫自訂篩選、路由等，您應該對其進行單元測試，而不是當作特定控制器動作測試的一部分。 這些項目應該隔離測試。

> [!TIP]
> [使用 Visual Studio 建立和執行單元測試](/visualstudio/test/unit-test-your-code)。

若要示範單元測試，請檢閱下列控制器。 它顯示一份腦力激盪工作階段清單，並允許使用 POST 建立新的腦力激盪工作階段：

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

此控制器遵循 [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) (明確的相依性原則)，預期會插入相依性以提供 `IBrainstormSessionRepository` 的執行個體。 因此可讓您使用模擬物件架構 (例如 [Moq](https://www.nuget.org/packages/Moq/)) 輕鬆進行測試。 `HTTP GET Index` 方法有沒有迴圈或分支，而且只會呼叫一個方法。 若要測試此 `Index` 方法，我們需要驗證透過儲存機制的 `List` 方法可傳回 `ViewResult` 和 `ViewModel`。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` 方法 (如上所示) 應該會驗證：

* 當 `ModelState.IsValid` 為 `false` 時，動作方法會傳回不正確的要求 `ViewResult` 及適當的資料

* 當 `ModelState.IsValid` 為 true 時，會呼叫儲存機制的 `Add` 方法，並傳回具有正確引數的 `RedirectToActionResult`。

您可以使用 `AddModelError` 新增錯誤，來測試無效的模型狀態，如下列第一項測試中所示。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

第一項測試會驗證當 `ModelState` 無效時，會傳回與 `GET` 要求相同的 `ViewResult`。 請注意，此測試不會嘗試傳入無效的模型。 即使傳入也沒有作用，因為模型繫結並未執行 (但[整合測試](xref:mvc/controllers/testing#integration-testing)會使用練習模型繫結)。 在此情況下，不會測試模型繫結。 這些單元測試只會測試動作方法中的程式碼功能。

第二項測試會驗證當 `ModelState` 有效時，會新增新的 `BrainstormSession` (透過儲存機制)，而且方法會傳回具有預期屬性的 `RedirectToActionResult`。 通常會忽略未呼叫的模擬呼叫，但在安裝程式呼叫結束時呼叫 `Verifiable` 即可在測試中對其進行驗證。 這會透過呼叫 `mockRepo.Verify` 來完成，如果未呼叫預期的方法，則測試會失敗。

> [!NOTE]
> 此範例中所使用的 Moq 程式庫可讓您輕鬆混合可驗證 (或「嚴格」) 的模擬與無法驗證的模擬 (也稱為「鬆散」的模擬或虛設常式)。 如需詳細資訊，請參閱 [Customizing Mock Behavior with Moq](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior) (使用 Moq 自訂模擬行為)。

應用程式中的另一個控制器會顯示與特定腦力激盪工作階段相關的資訊。 其中包含一些可處理無效識別碼值的邏輯：

[!code-csharp[](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

此控制器動作有三個待測試案例，每個 `return` 陳述式各代表一個案例：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

此應用程式會以 Web API 形式公開功能 (一份與腦力激盪工作階段建立關聯的想法清單，以及一個將新的想法新增工作清單的方法)：

<a name="ideas-controller"></a>

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` 方法會傳回 `IdeaDTO` 類型清單。 請避免直接透過 API 呼叫來傳回公司領域實體，因為這些實體通常包含比 API 用戶端所需更多的資料，而且會不必要地結合您應用程式的內部領域模型與您對外公開的 API。 您可以手動 (使用此處所示的 LINQ `Select`) 或使用 [AutoMapper](https://github.com/AutoMapper/AutoMapper) 等程式庫，來對應領域實體與您將透過網路傳回的類型

`Create` 和 `ForSession` API 方法的單元測試為：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

如前所述，若要測試當 `ModelState` 無效時的方法行為，請將模型錯誤新增控制器作為測試的一部分。 請勿嘗試在您的單元測試中測試模型驗證或模型繫結，只要測試當遇到特定 `ModelState` 值時的動作方法行為即可。

第二項測試需要儲存機制傳回 Null，因此會設定模擬儲存機制傳回 Null。 不需要建立測試資料庫 (在記憶體中或其他位置)，也不需要建構傳回此結果的查詢，這會透過上述的單一陳述式來完成。

最後一項測試會驗證是否呼叫儲存機制的 `Update` 方法。 如前所述，先使用 `Verifiable` 呼叫模擬，再呼叫模擬儲存機制的 `Verify` 方法，確認是否已執行可驗證的方法。 單元測試無須負責確保 `Update` 方法已儲存資料，這會透過整合測試來完成。

## <a name="integration-testing"></a>整合測試

[整合測試](../../testing/integration-testing.md)的目的是為了確保應用程式中的個別模組可正常搭配運作。 一般而言，您可以使用單元測試進行測試的任何情節，也可以使用整合測試進行測試，但反之則不然。 不過，整合測試通常比單元測試慢。 因此，最好使用單元測試來測試任何情節，再針對涉及多個共同作業者的情節使用整合測試。

雖然模擬物件仍然有用，但很少會用於整合測試中。 在單元測試中，模擬物件可有效地控制單元外待測試的共同作業者針對測試目的應該有的行為方式。 在整合測試中，會使用實際的共同作業者來確認整個子系統可正常搭配運作。

### <a name="application-state"></a>應用程式狀態

執行整合測試時的一個重要考量是如何設定應用程式狀態。 測試需要彼此獨立執行，因此每項測試應該從已知狀態的應用程式開始。 如果您的應用程式未使用資料庫或沒有任何持續性儲存區，這可能不是問題。 不過，大部分真實世界應用程式會將其狀態保存到某種資料存放區，因此除非重設資料存放區，否則某項測試所做的任何修改都可能影響另一項測試。 透過內建 `TestServer`，輕鬆就可在整合測試中裝載 ASP.NET Core 應用程式，但這不一定會授權存取它將使用的資料。 如果您使用實際資料庫，一個方法是讓應用程式連線到您的測試可存取的測試資料庫，並確保在每項測試執行之前都已重設為已知狀態。

在此範例應用程式中，我將使用 Entity Framework Core 的 InMemoryDatabase 支援，因此無法直接從測試專案連線到資料庫。 相反地，我會公開應用程式之 `Startup` 類別中的 `InitializeDatabase` 方法，當應用程式啟動時，若在 `Development` 環境中，就會呼叫此方法。 只要我的整合測試將環境設定為`Development`，就會自動從中獲益。 我不必擔心重設資料庫，因為每次應用程式重新啟動，都會重設 InMemoryDatabase。

`Startup` 類別：

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

您會在下列整合測試中看到經常使用 `GetTestSession` 方法。

### <a name="accessing-views"></a>存取檢視

每個整合測試類別會設定將執行 ASP.NET Core 應用程式的 `TestServer`。 根據預設，`TestServer` 會在執行所在的資料夾中裝載 Web 應用程式，在本例中會是測試專案資料夾。 因此，當您嘗試測試傳回 `ViewResult` 的控制器動作時，您可能會看到此錯誤：

```
The view 'Index' wasn't found. The following locations were searched:
(list of locations)
```

若要更正此問題，您需要設定伺服器的內容根目錄，讓它可以找到受測專案的檢視。 這會透過呼叫 `TestFixture` 類別中的 `UseContentRoot` 來完成，如下所示：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture` 類別負責設定及建立 `TestServer`，並設定 `HttpClient` 與 `TestServer` 通訊。 每項整合測試都會使用 `Client` 屬性來連線到測試伺服器並提出要求。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

在上述第一項測試中，`responseString` 會保存檢視中實際轉譯的 HTML，您可以檢查此 HTML 確認它包含預期的結果。

第二項測試使用唯一的工作階段名稱建構表單 POST 並將它張貼至應用程式，然後驗證是否傳回預期的重新導向。

### <a name="api-methods"></a>API 方法

如果您的應用程式公開 Web API，建議使用自動化的測試來確認這些 API 如預期般執行。 內建 `TestServer` 可讓您輕鬆測試 Web API。 如果您的 API 方法使用模型繫結，請務必檢查 `ModelState.IsValid`，而整合測試是確認模型驗證是否正常運作的理想位置。

下列一組測試是以上述 [IdeasController](xref:mvc/controllers/testing#ideas-controller) 類別中的 `Create` 方法為目標：

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

不同於動作的整合測試會傳回 HTML 檢視，Web API 方法會傳回結果，您通常可以將這些結果還原序列化為強型別物件，如上面最後一項測試所示。 在本例中，測試會將結果還原序列化為 `BrainstormSession` 執行個體，並確認此想法已正確新增其想法集合。

您可以在本文的[範例專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)中，找到整合測試的其他範例。
