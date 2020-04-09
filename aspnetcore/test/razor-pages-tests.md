---
title: 剃刀頁單元測試在ASP.NET核心
author: rick-anderson
description: 瞭解如何為 Razor 頁面應用創建單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 0e217b6b7f15519a3da44f5d074cf80fa96a3b3a
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664406"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>剃刀頁單元測試在ASP.NET核心

::: moniker range=">= aspnetcore-3.0"

ASP.NET核心支援剃刀頁面應用的單位測試。 資料存取層 (DAL) 和頁面模型的測試有助於確保:

* Razor Pages 應用的某些部分在應用構建期間作為一個單元獨立地協同工作。
* 類和方法的責任範圍有限。
* 有關應用應如何操作的其他文檔。
* 回歸是代碼更新引起的錯誤,在自動生成和部署期間找到。

本主題假定您對 Razor Pages 應用和單元測試有基本的瞭解。 如果您不熟悉 Razor 頁面應用或測試概念,請參閱以下主題:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)([如何下載](xref:index#how-to-download-a-sample))

範例項目由兩個應用組成:

| App         | 專案資料夾                     | 描述 |
| ----------- | ---------------------------------- | ----------- |
| 訊息應用 | *src/剃鬚刀測試樣品*         | 允許使用者添加郵件、刪除一條消息、刪除所有郵件和分析郵件(尋找每條消息的平均字數)。 |
| 測試應用程式    | *測試/剃鬚刀測試樣本.測試* | 用於單元測試消息應用的 DAL 和索引頁模型。 |

測試可以使用 IDE 的內建測試功能執行,例如[Mac 的視覺化工作室](/visualstudio/test/unit-test-your-code)或[視覺工作室](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。 如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesTestSample.Test.Test.Test*資料夾中的命令提示符執行以下命令:

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>訊息應用組織

訊息應用是具有以下特徵的 Razor Pages 訊息系統:

* 應用程式的索引頁(*頁面/Index.cshtml*和*頁面/Index.cshtml.cs)* 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(尋找每條消息的平均字數)。
* 消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。 該`Text`屬性是必需的,限制為 200 個字元。
* 消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。
* 該應用程式在其資料庫上下文類中包含`AppDbContext`DAL(*資料/AppDbContext.cs)。* DAL 方法被標`virtual`記 ,這允許類比用於測試的方法。
* 如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。 這些*種子消息*也用於測試。

&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。 本主題使用[xUnit](https://xunit.github.io/)測試框架。 不同測試框架的測試概念和測試實現相似但不相同。

儘管示例應用不使用儲存庫模式,並且不是[工作單元 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,但 Razor Pages 支援這些開發模式。 有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和<xref:mvc/controllers/testing>(示例實現存儲庫模式)。

## <a name="test-app-organization"></a>測試應用組織

測試應用是*測試/RazorPagesTestSample.Test.測試*資料夾中的控制台應用。

| 測試套用資料夾 | 描述 |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs*包含 DAL 的單元測試。</li><li>*IndexPageTests.cs*包含索引頁模型的單位測試。</li></ul> |
| *公用事業*     | 包含用於為每個`TestDbContextOptions`DAL 單元測試創建新資料庫上下文選項的方法,以便資料庫重置為每個測試的基線條件。 |

測試框架是[xUnit](https://xunit.github.io/)。 對象類比框架是[Moq。](https://github.com/moq/moq4)

## <a name="unit-tests-of-the-data-access-layer-dal"></a>資料存取層 (DAL) 的單位測試

消息應用具有包含`AppDbContext`類 *(src/RazorPagesTestSample/資料/AppDbContext.cs)* 中的四種方法的 DAL。 每個方法在測試應用中都有一個或兩個單元測試。

| DAL 方法               | 函式                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | 從順序`Text``List<Message>`的資料庫抓取 。 |
| `AddMessageAsync`        | 新增資料庫`Message`加入資料庫 。                                          |
| `DeleteAllMessagesAsync` | 從資料庫中刪除`Message`所有條目。                           |
| `DeleteMessageAsync`     | 通過從資料庫`Message`中刪除單個。 `Id`                      |

在為每個測試創建新測試時<xref:Microsoft.EntityFrameworkCore.DbContextOptions>,需要 DAL 的`AppDbContext`單位測試。 為每個測試建立`DbContextOptions`的一種方法是使用<xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

此方法的問題在於,每個測試都以上次測試離開的任何狀態接收資料庫。 當嘗試編寫不相互干擾的原子單元測試時,這可能會有問題。 要強制`AppDbContext`對每個測試使用新的資料庫上下文,請使用基於新服務提供`DbContextOptions`者 的實例。 測試應用程式展示如何使用其`Utilities`類別`TestDbContextOptions`方法 (*測試/ RazorPagesTestSample. 測試/ 實用程式.cs)* 執行此操作:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

`DbContextOptions`使用 DAL 單元測試允許每個測試使用新的資料庫實體以原子方式執行:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

`DataAccessLayerTest`類別的每個測試方法 (*單元測試 / 資料存取層測試.cs*) 都遵循類似的排列-行為斷言模式:

1. 排列:為測試配置了資料庫和/或定義了預期結果。
1. 操作:執行測試。
1. 斷言:斷言是確定測試結果是否成功。

例如,`DeleteMessageAsync`該方法負責刪除由`Id`其 *(src/RazorPagesTestSample/Data/AppDbContext.cs)* 識別的單一訊息:

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

此方法有兩個測試。 一個測試檢查當消息存在於資料庫中時,該方法是否刪除消息。 另一種方法測試,如果刪除消息`Id`不存在,資料庫不會更改。 該方法`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`如下所示:

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先,該方法執行"排列"步驟,在其中準備 Act 步驟。 種子化消息在`seedMessages`中 獲取並持有。 種子設定消息將保存到資料庫中。 具有的`Id``1`已設置為刪除的消息。 執行`DeleteMessageAsync`方法時,預期消息應具有除`Id``1`具有的的消息之外的所有消息。 變數`expectedMessages`表示此預期結果。

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法作用:該方法`DeleteMessageAsync`在`recId`中`1`執行傳遞:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最後,該方法從上下文中獲取`Messages`,並將其`expectedMessages`與 斷言兩者相等:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

為了比較兩`List<Message>`者是相同的:

* 消息按 排序`Id`。
* 在`Text`屬性上比較消息對。

類似的測試方法檢查`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`嘗試刪除不存在的消息的結果。 在這種情況下,資料庫中的預期消息應等於執行`DeleteMessageAsync`方法后的實際消息。 不要變更資料庫的內容:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>頁面模型方法的單位測試

另一組單元測試負責頁面模型方法的測試。 在消息應用中,索引頁模型在`IndexModel`*src/RazorPagesTestSample/Pages/Index.cs.cs*的類中找到。

| 頁面模型方法 | 函式 |
| ----------------- | -------- |
| `OnGetAsync` | 使用`GetMessagesAsync`方法從 UI 的 DAL 獲取消息。 |
| `OnPostAddMessageAsync` | 如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效`AddMessageAsync`,則調用以向資料庫添加消息。 |
| `OnPostDeleteAllMessagesAsync` | 呼叫`DeleteAllMessagesAsync`以刪除資料庫中的所有消息。 |
| `OnPostDeleteMessageAsync` | 執行`DeleteMessageAsync`以刪除指定`Id`郵件。 |
| `OnPostAnalyzeMessagesAsync` | 如果資料庫中有一個或多個消息,則計算每條消息的平均單詞數。 |

頁面模型方法使用`IndexPageTests`類中的七個測試(*測試/RazorPagesTestSample.測試/單元測試/IndexPageTest.cs)* 進行測試。 測試使用熟悉的排列-斷言-行為模式。 這些測試側重於:

* 確定當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時,這些方法是否遵循正確的行為。
* 確認方法會產生正確的<xref:Microsoft.AspNetCore.Mvc.IActionResult>。
* 檢查屬性值分配是否正確。

這組測試通常嘲笑 DAL 的方法,以便為執行頁面模型方法的 Act 步驟生成預期數據。 例如,`GetMessagesAsync``AppDbContext`將類比方法以生成輸出。 當頁面模型方法執行此方法時,類比將返回結果。 數據不來自資料庫。 這為在頁面模型測試中使用 DAL 創造了可預測的、可靠的測試條件。

測試`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`顯示如何對`GetMessagesAsync`頁面 模型模擬該方法:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

在`OnGetAsync`Act 步驟中執行該方法時,它將調用`GetMessagesAsync`頁面模型 的方法。

單元測試行動步驟(*測試/ RazorPages 測試樣本.測試/ 單元測試/索引頁面測試.cs):*

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`頁面模型`OnGetAsync`的方法 (*src/ RazorPages 測試範例/ 頁面/ 索引.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

DAL`GetMessagesAsync`中的方法不會返回此方法調用的結果。 方法的類比版本返回結果。

在步驟`Assert`中,實際消息`actualMessages`( )`Messages`是從頁面模型的屬性中分配的。 分配消息時,也會執行類型檢查。 預期消息和實際消息按其`Text`屬性進行比較。 測試斷言這兩`List<Message>`個實例包含相同的消息。

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

此群組中的其他測試建立頁面模型<xref:Microsoft.AspNetCore.Http.DefaultHttpContext>物件,其中包括<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary><xref:Microsoft.AspNetCore.Mvc.ActionContext>`PageContext``ViewDataDictionary``PageContext`、 、 這些在進行測試時非常有用。 例如,訊息應用建立一`ModelState`個錯誤,<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>用於檢查在執行<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>`OnPostAddMessageAsync`時 是否傳回有效:

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>其他資源

* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [單元測試代碼](/visualstudio/test/unit-test-your-code)(可視化工作室)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [開始使用xUnit.net:使用 .NET Core 與 .NET SDK 指令列](https://xunit.github.io/docs/getting-started-dotnet-core)
* [莫克](https://github.com/moq/moq4)
* [莫克快速入門](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET核心支援剃刀頁面應用的單位測試。 資料存取層 (DAL) 和頁面模型的測試有助於確保:

* Razor Pages 應用的某些部分在應用構建期間作為一個單元獨立地協同工作。
* 類和方法的責任範圍有限。
* 有關應用應如何操作的其他文檔。
* 回歸是代碼更新引起的錯誤,在自動生成和部署期間找到。

本主題假定您對 Razor Pages 應用和單元測試有基本的瞭解。 如果您不熟悉 Razor 頁面應用或測試概念,請參閱以下主題:

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)([如何下載](xref:index#how-to-download-a-sample))

範例項目由兩個應用組成:

| App         | 專案資料夾                     | 描述 |
| ----------- | ---------------------------------- | ----------- |
| 訊息應用 | *src/剃鬚刀測試樣品*         | 允許使用者添加郵件、刪除一條消息、刪除所有郵件和分析郵件(尋找每條消息的平均字數)。 |
| 測試應用程式    | *測試/剃鬚刀測試樣本.測試* | 用於單元測試消息應用的 DAL 和索引頁模型。 |

測試可以使用 IDE 的內建測試功能執行,例如[Mac 的視覺化工作室](/visualstudio/test/unit-test-your-code)或[視覺工作室](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。 如果使用[Visual Studio 代碼](https://code.visualstudio.com/)或命令列,請在*測試/RazorPagesTestSample.Test.Test.Test*資料夾中的命令提示符執行以下命令:

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>訊息應用組織

訊息應用是具有以下特徵的 Razor Pages 訊息系統:

* 應用程式的索引頁(*頁面/Index.cshtml*和*頁面/Index.cshtml.cs)* 提供了一個 UI 和頁面模型方法來控制消息的添加、刪除和分析(尋找每條消息的平均字數)。
* 消息由具有兩個屬性`Message``Id``Text`的類 (*Data/Message.cs*) 描述。 該`Text`屬性是必需的,限制為 200 個字元。
* 消息使用[實體框架的記憶體中資料庫](/ef/core/providers/in-memory/)&#8224;存储。
* 該應用程式在其資料庫上下文類中包含`AppDbContext`DAL(*資料/AppDbContext.cs)。* DAL 方法被標`virtual`記 ,這允許類比用於測試的方法。
* 如果資料庫在應用啟動時為空,則消息存儲將用三條消息進行初始化。 這些*種子消息*也用於測試。

&#8224;EF 主題[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)「解釋了如何使用記憶體中資料庫進行 MSTest 測試。 本主題使用[xUnit](https://xunit.github.io/)測試框架。 不同測試框架的測試概念和測試實現相似但不相同。

儘管示例應用不使用儲存庫模式,並且不是[工作單元 (UoW) 模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效示例,但 Razor Pages 支援這些開發模式。 有關詳細資訊,請參閱[設計基礎結構持久性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和<xref:mvc/controllers/testing>(示例實現存儲庫模式)。

## <a name="test-app-organization"></a>測試應用組織

測試應用是*測試/RazorPagesTestSample.Test.測試*資料夾中的控制台應用。

| 測試套用資料夾 | 描述 |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs*包含 DAL 的單元測試。</li><li>*IndexPageTests.cs*包含索引頁模型的單位測試。</li></ul> |
| *公用事業*     | 包含用於為每個`TestDbContextOptions`DAL 單元測試創建新資料庫上下文選項的方法,以便資料庫重置為每個測試的基線條件。 |

測試框架是[xUnit](https://xunit.github.io/)。 對象類比框架是[Moq。](https://github.com/moq/moq4)

## <a name="unit-tests-of-the-data-access-layer-dal"></a>資料存取層 (DAL) 的單位測試

消息應用具有包含`AppDbContext`類 *(src/RazorPagesTestSample/資料/AppDbContext.cs)* 中的四種方法的 DAL。 每個方法在測試應用中都有一個或兩個單元測試。

| DAL 方法               | 函式                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | 從順序`Text``List<Message>`的資料庫抓取 。 |
| `AddMessageAsync`        | 新增資料庫`Message`加入資料庫 。                                          |
| `DeleteAllMessagesAsync` | 從資料庫中刪除`Message`所有條目。                           |
| `DeleteMessageAsync`     | 通過從資料庫`Message`中刪除單個。 `Id`                      |

在為每個測試創建新測試時<xref:Microsoft.EntityFrameworkCore.DbContextOptions>,需要 DAL 的`AppDbContext`單位測試。 為每個測試建立`DbContextOptions`的一種方法是使用<xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

此方法的問題在於,每個測試都以上次測試離開的任何狀態接收資料庫。 當嘗試編寫不相互干擾的原子單元測試時,這可能會有問題。 要強制`AppDbContext`對每個測試使用新的資料庫上下文,請使用基於新服務提供`DbContextOptions`者 的實例。 測試應用程式展示如何使用其`Utilities`類別`TestDbContextOptions`方法 (*測試/ RazorPagesTestSample. 測試/ 實用程式.cs)* 執行此操作:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

`DbContextOptions`使用 DAL 單元測試允許每個測試使用新的資料庫實體以原子方式執行:

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

`DataAccessLayerTest`類別的每個測試方法 (*單元測試 / 資料存取層測試.cs*) 都遵循類似的排列-行為斷言模式:

1. 排列:為測試配置了資料庫和/或定義了預期結果。
1. 操作:執行測試。
1. 斷言:斷言是確定測試結果是否成功。

例如,`DeleteMessageAsync`該方法負責刪除由`Id`其 *(src/RazorPagesTestSample/Data/AppDbContext.cs)* 識別的單一訊息:

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

此方法有兩個測試。 一個測試檢查當消息存在於資料庫中時,該方法是否刪除消息。 另一種方法測試,如果刪除消息`Id`不存在,資料庫不會更改。 該方法`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`如下所示:

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先,該方法執行"排列"步驟,在其中準備 Act 步驟。 種子化消息在`seedMessages`中 獲取並持有。 種子設定消息將保存到資料庫中。 具有的`Id``1`已設置為刪除的消息。 執行`DeleteMessageAsync`方法時,預期消息應具有除`Id``1`具有的的消息之外的所有消息。 變數`expectedMessages`表示此預期結果。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法作用:該方法`DeleteMessageAsync`在`recId`中`1`執行傳遞:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最後,該方法從上下文中獲取`Messages`,並將其`expectedMessages`與 斷言兩者相等:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

為了比較兩`List<Message>`者是相同的:

* 消息按 排序`Id`。
* 在`Text`屬性上比較消息對。

類似的測試方法檢查`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`嘗試刪除不存在的消息的結果。 在這種情況下,資料庫中的預期消息應等於執行`DeleteMessageAsync`方法后的實際消息。 不要變更資料庫的內容:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>頁面模型方法的單位測試

另一組單元測試負責頁面模型方法的測試。 在消息應用中,索引頁模型在`IndexModel`*src/RazorPagesTestSample/Pages/Index.cs.cs*的類中找到。

| 頁面模型方法 | 函式 |
| ----------------- | -------- |
| `OnGetAsync` | 使用`GetMessagesAsync`方法從 UI 的 DAL 獲取消息。 |
| `OnPostAddMessageAsync` | 如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效`AddMessageAsync`,則調用以向資料庫添加消息。 |
| `OnPostDeleteAllMessagesAsync` | 呼叫`DeleteAllMessagesAsync`以刪除資料庫中的所有消息。 |
| `OnPostDeleteMessageAsync` | 執行`DeleteMessageAsync`以刪除指定`Id`郵件。 |
| `OnPostAnalyzeMessagesAsync` | 如果資料庫中有一個或多個消息,則計算每條消息的平均單詞數。 |

頁面模型方法使用`IndexPageTests`類中的七個測試(*測試/RazorPagesTestSample.測試/單元測試/IndexPageTest.cs)* 進行測試。 測試使用熟悉的排列-斷言-行為模式。 這些測試側重於:

* 確定當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時,這些方法是否遵循正確的行為。
* 確認方法會產生正確的<xref:Microsoft.AspNetCore.Mvc.IActionResult>。
* 檢查屬性值分配是否正確。

這組測試通常嘲笑 DAL 的方法,以便為執行頁面模型方法的 Act 步驟生成預期數據。 例如,`GetMessagesAsync``AppDbContext`將類比方法以生成輸出。 當頁面模型方法執行此方法時,類比將返回結果。 數據不來自資料庫。 這為在頁面模型測試中使用 DAL 創造了可預測的、可靠的測試條件。

測試`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`顯示如何對`GetMessagesAsync`頁面 模型模擬該方法:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

在`OnGetAsync`Act 步驟中執行該方法時,它將調用`GetMessagesAsync`頁面模型 的方法。

單元測試行動步驟(*測試/ RazorPages 測試樣本.測試/ 單元測試/索引頁面測試.cs):*

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`頁面模型`OnGetAsync`的方法 (*src/ RazorPages 測試範例/ 頁面/ 索引.cshtml.cs*):

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

DAL`GetMessagesAsync`中的方法不會返回此方法調用的結果。 方法的類比版本返回結果。

在步驟`Assert`中,實際消息`actualMessages`( )`Messages`是從頁面模型的屬性中分配的。 分配消息時,也會執行類型檢查。 預期消息和實際消息按其`Text`屬性進行比較。 測試斷言這兩`List<Message>`個實例包含相同的消息。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

此群組中的其他測試建立頁面模型<xref:Microsoft.AspNetCore.Http.DefaultHttpContext>物件,其中包括<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary><xref:Microsoft.AspNetCore.Mvc.ActionContext>`PageContext``ViewDataDictionary``PageContext`、 、 這些在進行測試時非常有用。 例如,訊息應用建立一`ModelState`個錯誤,<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*>用於檢查在執行<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>`OnPostAddMessageAsync`時 是否傳回有效:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>其他資源

* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [單元測試代碼](/visualstudio/test/unit-test-your-code)(可視化工作室)
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [開始使用xUnit.net:使用 .NET Core 與 .NET SDK 指令列](https://xunit.github.io/docs/getting-started-dotnet-core)
* [莫克](https://github.com/moq/moq4)
* [莫克快速入門](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
