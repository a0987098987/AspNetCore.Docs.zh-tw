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
ms.openlocfilehash: e8a464e75dea3a0ec08c13a11888884e6bb6a4c7
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a>在 ASP.NET Core 中測試控制器邏輯

由[Steve Smith](https://ardalis.com/)

ASP.NET MVC 應用程式中的控制站應該小，並著重於使用者介面考量。 處理非 UI 考量的大型控制站會更難以測試及維護。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>測試控制器

控制站都是中央任何 ASP.NET Core MVC 應用程式的一部分。 因此，您應該有它們的行為如預期般應用程式的信心。 自動化的測試可以提供您進行此部署，並在到達生產環境之前，可以偵測錯誤。 請務必避免將放在您的控制站不必要的責任，並確保只能在控制器責任您測試的焦點。

控制器邏輯應該很小，並不會著重於商務邏輯或基礎結構考量 （例如，資料存取）。 測試控制器邏輯，不是架構。 測試如何控制器*行為*根據有效或無效的輸入。 測試控制器回應根據它所執行的商務作業的結果。

典型的控制器責任：

* 確認`ModelState.IsValid`。
* 如果傳回錯誤回應`ModelState`無效。
* 從持續性儲存中擷取的商務實體。
* 商務實體上執行的動作。
* 將商務實體儲存至持續性。
* 傳回適當`IActionResult`。

## <a name="unit-testing"></a>單元測試

[單元測試](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)包括從其基礎結構和相依性隔離測試應用程式的一部分。 單元測試控制器邏輯，只有單一動作的內容進行測試時，不是行為架構本身或其相依性。 為您的單元測試控制器動作，請確定您只著重於其行為。 控制器單元測試可避免等[篩選](filters.md)，[路由](../../fundamentals/routing.md)，或[模型繫結](../models/model-binding.md)。 把焦點放在測試只是一件事，通常的單元測試是容易撰寫且快速地執行。 編寫完善的組的單元測試可以經常執行，不多的負荷。 不過，單元測試不會偵測問題中元件之間的互動，也就是目的[整合測試](xref:mvc/controllers/testing#integration-testing)。

如果您正在撰寫自訂篩選條件、 路由和其他內容，您應該將單元測試，但不是上特定控制器執行測試的一部分。 它們應該地接受獨立測試。

> [!TIP]
> [建立及執行單元測試使用 Visual Studio](https://www.visualstudio.com/docs/code/create-and-run-unit-tests-vs)。

若要示範的單元測試，請檢閱下列控制站。 它會顯示一份腦力激盪工作，並可讓新腦力激盪使用 POST 來建立的工作階段：

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

之後，控制器[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)，預期為提供的執行個體的相依性插入`IBrainstormSessionRepository`。 這樣很容易就能測試使用模擬物件架構，例如[Moq](https://www.nuget.org/packages/Moq/)。 `HTTP GET Index`方法有沒有迴圈或分支，而且只會呼叫一個方法。 若要測試這`Index`方法中，我們需要確認`ViewResult`會傳回與`ViewModel`從儲存機制的`List`方法。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index`應該驗證方法 （如上所示）：

* 動作方法會傳回不正確的要求`ViewResult`以適當的資料時`ModelState.IsValid`是`false`

* `Add`儲存機制上的方法呼叫和`RedirectToActionResult`傳回正確的引數使用時`ModelState.IsValid`為 true。

無效的模型狀態可以透過加入使用錯誤測試`AddModelError`底下第一項測試中所示。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

第一項測試會確認當`ModelState`無效，相同`ViewResult`傳回與`GET`要求。 請注意，測試不會嘗試將傳入無效的模型。 不會運作，還是因為模型繫結不執行 (雖然[整合測試](xref:mvc/controllers/testing#integration-testing)會使用練習模型繫結)。 在此情況下，模型繫結不是正在測試。 這些單元測試只有測試動作方法中的程式碼。

第二個測試會驗證當`ModelState`是有效的新`BrainstormSession`加入 （透過儲存機制），而且方法會傳回`RedirectToActionResult`與預期的屬性。 模擬不呼叫的呼叫是通常會被忽略，但呼叫`Verifiable`結尾的安裝程式呼叫可讓它在測試中驗證。 這是呼叫`mockRepo.Verify`，如果預期的方法，不會呼叫，這將會失敗的測試。

> [!NOTE]
> 此範例中使用 Moq 程式庫輕鬆地混合驗證，或"strict"，模擬與非可驗證的模擬 （也稱為 「 鬆散 「 模擬 」 或 「 虛設常式）。 深入了解[自訂 Moq 的模擬行為](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)。

應用程式中的另一個控制站會顯示與特定腦力激盪工作階段相關的資訊。 它包含某些邏輯，以處理無效的識別碼值：

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

控制器動作有三種情況，若要測試，其中每個`return`陳述式：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

應用程式公開 web API （腦力激和方法的新概念加入至工作階段相關聯的想法清單） 的功能：

<a name=ideas-controller></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession`方法會傳回一份`IdeaDTO`型別。 避免直接透過 API 呼叫傳回商業網域項目，因為經常包括更多超過 API 用戶端要求，而且它們不必要地結合您的應用程式的內部網域模型與應用程式開發介面公開 （expose） 外部的資料。 網域實體與您透過網路將傳回的型別之間的對應動作可手動 (使用 LINQ`Select`如下所示) 或使用程式庫如[AutoMapper](https://github.com/AutoMapper/AutoMapper)

適用於單元測試`Create`和`ForSession`API 方法：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

如前所述，若要測試之方法的行為時`ModelState`無效，將模型錯誤加入至控制器，做為測試的一部分。 請勿嘗試在您的單元測試中測試模型驗證或模型繫結-只是測試動作方法的行為與特定距離`ModelState`值。

第二個測試的儲存機制，讓模擬的儲存機制設定成傳回 null 則傳回 null，而定。 不需要建立測試資料庫 (在記憶體中或其他方式)，然後建構查詢，傳回這個結果-可以透過在單一陳述式所示。

最後一次測試會驗證儲存機制的`Update`方法呼叫。 我們在先前呼叫模擬`Verifiable`並接著模擬儲存機制的`Verify`呼叫方法，確認已執行的驗證方法。 它不是單元測試必須負責確保`Update`方法儲存的資料，則可以完成整合測試。

## <a name="integration-testing"></a>整合測試

[整合測試](../../testing/integration-testing.md)為了確保您的應用程式工作中的個別模組的正確一起。 一般而言，任何您可以測試與單元測試，您也可以測試具有整合測試，但是反向並不正確。 但是，整合測試通常比單元測試變得很慢。 因此，最好測試任何單元測試，可以和整合測試用於包含多個共同作業者案例。

雖然它們仍然可能會很有用，但很少會整合測試中使用模擬物件。 在單元測試，模擬物件會是有效的方式來控制共同作業者之外所測試的單元測試目的的行為方式。 在整合測試，實際的共同作業者可用來確認整個子系統一起正常運作。

### <a name="application-state"></a>應用程式狀態

執行整合測試時的一個重要考量是如何設定您的應用程式狀態。 測試需要執行彼此獨立，因此每個測試的開頭應為已知的狀態中的應用程式。 如果您的應用程式不會使用資料庫，或是有任何持續性，這可能不是問題。 不過，大部分的真實世界應用程式保存到某種資料存放區，其狀態，讓任何一項測試所做的修改無法影響另一個測試，除非資料存放區會重設。 使用內建`TestServer`，它是非常簡單，我們的整合測試內的主機 ASP.NET Core 應用程式，但是，不一定會授與它將會使用的資料的存取權。 如果您使用實際的資料庫，其中一個方法是應用程式連接到測試資料庫，其可以存取您的測試，並確認會重設為已知的狀態。 每項測試執行之前。

在此範例應用程式中，我在使用 Entity Framework Core 的 InMemoryDatabase 支援，因此我無法只連線到它，從我的測試專案。 相反地，我公開`InitializeDatabase`方法從應用程式的`Startup`類別，如果是在應用程式啟動時，我呼叫`Development`環境。 我的整合測試會自動從中獲益，只要它們將環境設定為`Development`。 我不必擔心重設資料庫，因為應用程式重新啟動每次重設 InMemoryDatabase。

`Startup`類別：

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

您會看到`GetTestSession`經常用於下列整合測試的方法。

### <a name="accessing-views"></a>存取檢視

每個整合測試類別設定`TestServer`執行 ASP.NET Core 應用程式。 根據預設，`TestServer`裝載 web 應用程式執行所在-在此情況下，測試專案資料夾的資料夾中。 因此，當您嘗試測試控制器動作傳回`ViewResult`，您可能會看到這個錯誤：

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "none"} -->

```none
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

若要更正此問題，您需要設定伺服器的內容的根目錄，因此它可以尋找受測專案的檢視。 這是由呼叫`UseContentRoot`中`TestFixture`類別，如下所示：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture`類別會負責設定及建立`TestServer`、 設定`HttpClient`與通訊`TestServer`。 整合的每一個都會測試使用`Client`連接到測試伺服器，並提出要求的屬性。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

在上述第一項測試`responseString`保存實際的方式轉譯 HTML 檢視中，可以檢查以確認它包含預期的結果。

第二個測試建構表單 POST 具有唯一的工作階段名稱並將它張貼至應用程式，然後驗證會傳回預期的重新導向。

### <a name="api-methods"></a>API 方法

如果您的應用程式公開 web Api，它的建議讓自動化測試，確認如預期般執行。 內建`TestServer`輕鬆測試 web 應用程式開發介面。 如果您應用程式開發介面的方法使用模型繫結，請一律檢查`ModelState.IsValid`，和整合測試是正確的位置，以確認您的模型驗證是否正常運作。

下列測試的目標集`Create`方法中的[IdeasController](xref:mvc/controllers/testing#ideas-controller)類別如上所示：

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

與不同的是整合測試的 HTML 檢視會傳回的動作，web 應用程式開發介面方法會傳回結果，通常可以還原序列化為強型別物件，如上面最後一次測試所示。 在此情況下，測試還原序列化結果`BrainstormSession`執行個體，並確認這個概念已正確加入至其集合的概念。

在這篇文章中找到其他範例的整合測試[範例專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)。
