---
title: Razor Pages ASP.NET Core 中的單元測試
author: rick-anderson
description: 瞭解如何建立 Razor Pages 應用程式的單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: test/razor-pages-tests
ms.openlocfilehash: 0e217b6b7f15519a3da44f5d074cf80fa96a3b3a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664406"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>Razor Pages ASP.NET Core 中的單元測試

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 支援 Razor Pages 應用程式的單元測試。 資料存取層（DAL）和頁面模型的測試有助於確保：

* Razor Pages 應用程式的元件會在應用程式建立期間獨立和一個單位一起工作。
* 類別和方法的責任範圍有限。
* 應用程式的行為方式有其他檔。
* 自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。

本主題假設您對 Razor Pages 應用程式和單元測試有基本瞭解。 如果您不熟悉 Razor Pages 應用程式或測試概念，請參閱下列主題：

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例專案是由兩個應用程式所組成：

| App         | 專案資料夾                     | 描述 |
| ----------- | ---------------------------------- | ----------- |
| 訊息應用程式 | *src/RazorPagesTestSample*         | 可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。 |
| 測試應用程式    | *測試/RazorPagesTestSample。測試* | 用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。 |

測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/RazorPagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>訊息應用程式組織

訊息應用程式是具有下列特性的 Razor Pages 訊息系統：

* 應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。
* 訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （索引鍵）和 `Text` （message）。 `Text` 屬性是必要的，而且限制為200個字元。
* 訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。
* 應用程式在其資料庫內容類別中包含 DAL，`AppDbContext` （*Data/AppDbCoNtext .cs*）。 DAL 方法會標示 `virtual`，這可讓您模擬要在測試中使用的方法。
* 如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。 這些*植*入的訊息也會用於測試中。

&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 跨不同測試架構的測試概念和測試執行類似，但不完全相同。

雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。 如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和 <xref:mvc/controllers/testing> （範例會執行存放庫模式）。

## <a name="test-app-organization"></a>測試應用程式組織

測試應用程式是 [測試]/[ *RazorPagesTestSample* ] 資料夾內的主控台應用程式。

| 測試應用程式資料夾 | 描述 |
| --------------- | ----------- |
| *Run-unittests*     | <ul><li>*DataAccessLayerTest.cs*包含 DAL 的單元測試。</li><li>*IndexPageTests.cs*包含索引頁面模型的單元測試。</li></ul> |
| *公用程式*     | 包含用來為每個 DAL 單元測試建立新資料庫內容選項的 `TestDbContextOptions` 方法，以便將資料庫重設為每個測試的基準條件。 |

測試架構為[xUnit](https://xunit.github.io/)。 物件模擬架構為[Moq](https://github.com/moq/moq4)。

## <a name="unit-tests-of-the-data-access-layer-dal"></a>資料存取層（DAL）的單元測試

訊息應用程式的 DAL 具有四個包含在 `AppDbContext` 類別中的方法（*src/RazorPagesTestSample/Data/AppDbCoNtext .cs*）。 每個方法在測試應用程式中都有一個或兩個單元測試。

| DAL 方法               | 函式                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | 從以 `Text` 屬性排序的資料庫取得 `List<Message>`。 |
| `AddMessageAsync`        | 將 `Message` 加入至資料庫。                                          |
| `DeleteAllMessagesAsync` | 刪除資料庫中的所有 `Message` 專案。                           |
| `DeleteMessageAsync`     | 藉由 `Id`，從資料庫中刪除單一 `Message`。                      |

為每個測試建立新的 `AppDbContext` 時，DAL 的單元測試需要 <xref:Microsoft.EntityFrameworkCore.DbContextOptions>。 建立每個測試 `DbContextOptions` 的一種方法是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>：

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。 嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。 若要強制 `AppDbContext` 針對每項測試使用新的資料庫內容，請提供以新服務提供者為基礎的 `DbContextOptions` 實例。 測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` （*測試/RazorPagesTestSample 測試/公用程式/公用程式 .cs*）來執行此動作：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

使用 DAL 單元測試中的 `DbContextOptions`，可讓每個測試以自動方式與全新的資料庫實例一起執行：

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

`DataAccessLayerTest` 類別（*run-unittests/DataAccessLayerTest*）中的每個測試方法都遵循類似的相片順序-判斷法模式：

1. 排列：已針對測試設定資料庫，並（或）定義預期的結果。
1. Act：執行測試。
1. 判斷提示：決定測試結果是否成功的判斷提示。

例如，`DeleteMessageAsync` 方法會負責移除其 `Id` 所識別的單一訊息（*src/RazorPagesTestSample/Data/AppDbCoNtext*）：

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

這個方法有兩個測試。 一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。 另一個方法則是測試如果訊息 `Id` 不存在時，資料庫不會變更。 `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` 方法如下所示：

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。 植入訊息會取得並保留在 `seedMessages`中。 植入訊息會儲存到資料庫中。 具有 `1` `Id` 的訊息已設定為要刪除。 執行 `DeleteMessageAsync` 方法時，預期的訊息應該會有所有訊息，但是除了具有 `Id` 的 `1`以外。 `expectedMessages` 變數代表此預期的結果。

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法的作用： `DeleteMessageAsync` 方法會在 `recId` 的 `1`中執行：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最後，方法會從內容取得 `Messages`，並將其與 `expectedMessages` 判斷提示兩者相等：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

為了比較這兩個 `List<Message>` 是相同的：

* 訊息會依 `Id`排序。
* 訊息配對會在 `Text` 屬性上進行比較。

類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 檢查嘗試刪除不存在之訊息的結果。 在此情況下，在執行 `DeleteMessageAsync` 方法之後，資料庫中預期的訊息應該等於實際的訊息。 資料庫的內容應該不會有任何變更：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>頁面模型方法的單元測試

另一組單元測試負責測試頁面模型方法。 在訊息應用程式中，索引頁面模型位於*src/RazorPagesTestSample/Pages/Index. cshtml .cs*的 `IndexModel` 類別中。

| 頁面模型方法 | 函式 |
| ----------------- | -------- |
| `OnGetAsync` | 使用 `GetMessagesAsync` 方法，從 DAL 取得適用于 UI 的訊息。 |
| `OnPostAddMessageAsync` | 如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 將訊息新增至資料庫。 |
| `OnPostDeleteAllMessagesAsync` | 呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。 |
| `OnPostDeleteMessageAsync` | 執行 `DeleteMessageAsync`，以使用指定的 `Id` 刪除訊息。 |
| `OnPostAnalyzeMessagesAsync` | 如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。 |

頁面模型方法會使用 `IndexPageTests` 類別中的七個測試進行測試（[*測試]/[RazorPagesTestSample]/[run-unittests]/[IndexPageTests*]）。 測試會使用熟悉的「排列-判斷提示-Act」模式。 這些測試著重于：

* 判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。
* 確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult>。
* 檢查是否已正確設定屬性值。

這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。 例如，`AppDbContext` 的 `GetMessagesAsync` 方法是模擬來產生輸出。 當頁面模型方法執行這個方法時，mock 會傳回結果。 資料不是來自資料庫。 這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何模擬頁面模型的 `GetMessagesAsync` 方法：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。

單元測試 Act 步驟（test */RazorPagesTestSample/run-unittests/IndexPageTests*）：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` 頁面模型的 `OnGetAsync` 方法（*src/RazorPagesTestSample/Pages/Index. cshtml .cs*）：

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

DAL 中的 `GetMessagesAsync` 方法不會傳回這個方法呼叫的結果。 模擬版本的方法會傳回結果。

在 `Assert` 步驟中，會從頁面模型的 `Messages` 屬性指派實際的訊息（`actualMessages`）。 指派訊息時也會執行類型檢查。 預期和實際的訊息會依其 `Text` 屬性進行比較。 測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

此群組中的其他測試會建立頁面模型物件，其中包括 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>、<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>、建立 `PageContext`的 <xref:Microsoft.AspNetCore.Mvc.ActionContext>、`ViewDataDictionary`和 `PageContext`。 這些適用于進行測試。 例如，訊息應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 建立 `ModelState` 錯誤，以確認執行 `OnPostAddMessageAsync` 時傳回的是有效的 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>其他資源

* [使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq 快速入門](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 支援 Razor Pages 應用程式的單元測試。 資料存取層（DAL）和頁面模型的測試有助於確保：

* Razor Pages 應用程式的元件會在應用程式建立期間獨立和一個單位一起工作。
* 類別和方法的責任範圍有限。
* 應用程式的行為方式有其他檔。
* 自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。

本主題假設您對 Razor Pages 應用程式和單元測試有基本瞭解。 如果您不熟悉 Razor Pages 應用程式或測試概念，請參閱下列主題：

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例專案是由兩個應用程式所組成：

| App         | 專案資料夾                     | 描述 |
| ----------- | ---------------------------------- | ----------- |
| 訊息應用程式 | *src/RazorPagesTestSample*         | 可讓使用者新增訊息、刪除一則郵件、刪除所有訊息，以及分析訊息（尋找每個訊息的平均字數）。 |
| 測試應用程式    | *測試/RazorPagesTestSample。測試* | 用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。 |

測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/RazorPagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>訊息應用程式組織

訊息應用程式是具有下列特性的 Razor Pages 訊息系統：

* 應用程式的 [索引] 頁面（[*pages/食指*] 和 [ *pages/Index*]）提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析（尋找每個訊息的平均字數）。
* 訊息是由 `Message` 類別（*Data/message .cs*）描述，其中包含兩個屬性： `Id` （索引鍵）和 `Text` （message）。 `Text` 屬性是必要的，而且限制為200個字元。
* 訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224;來儲存。
* 應用程式在其資料庫內容類別中包含 DAL，`AppDbContext` （*Data/AppDbCoNtext .cs*）。 DAL 方法會標示 `virtual`，這可讓您模擬要在測試中使用的方法。
* 如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。 這些*植*入的訊息也會用於測試中。

&#8224;EF 主題「[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)」說明如何使用記憶體內部資料庫，以搭配 MSTest 進行測試。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 跨不同測試架構的測試概念和測試執行類似，但不完全相同。

雖然範例應用程式不會使用存放庫模式，而且不是有效的[工作單位（UoW）模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)範例，Razor Pages 支援這些開發模式。 如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和 <xref:mvc/controllers/testing> （範例會執行存放庫模式）。

## <a name="test-app-organization"></a>測試應用程式組織

測試應用程式是 [測試]/[ *RazorPagesTestSample* ] 資料夾內的主控台應用程式。

| 測試應用程式資料夾 | 描述 |
| --------------- | ----------- |
| *Run-unittests*     | <ul><li>*DataAccessLayerTest.cs*包含 DAL 的單元測試。</li><li>*IndexPageTests.cs*包含索引頁面模型的單元測試。</li></ul> |
| *公用程式*     | 包含用來為每個 DAL 單元測試建立新資料庫內容選項的 `TestDbContextOptions` 方法，以便將資料庫重設為每個測試的基準條件。 |

測試架構為[xUnit](https://xunit.github.io/)。 物件模擬架構為[Moq](https://github.com/moq/moq4)。

## <a name="unit-tests-of-the-data-access-layer-dal"></a>資料存取層（DAL）的單元測試

訊息應用程式的 DAL 具有四個包含在 `AppDbContext` 類別中的方法（*src/RazorPagesTestSample/Data/AppDbCoNtext .cs*）。 每個方法在測試應用程式中都有一個或兩個單元測試。

| DAL 方法               | 函式                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | 從以 `Text` 屬性排序的資料庫取得 `List<Message>`。 |
| `AddMessageAsync`        | 將 `Message` 加入至資料庫。                                          |
| `DeleteAllMessagesAsync` | 刪除資料庫中的所有 `Message` 專案。                           |
| `DeleteMessageAsync`     | 藉由 `Id`，從資料庫中刪除單一 `Message`。                      |

為每個測試建立新的 `AppDbContext` 時，DAL 的單元測試需要 <xref:Microsoft.EntityFrameworkCore.DbContextOptions>。 建立每個測試 `DbContextOptions` 的一種方法是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>：

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。 嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。 若要強制 `AppDbContext` 針對每項測試使用新的資料庫內容，請提供以新服務提供者為基礎的 `DbContextOptions` 實例。 測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` （*測試/RazorPagesTestSample 測試/公用程式/公用程式 .cs*）來執行此動作：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

使用 DAL 單元測試中的 `DbContextOptions`，可讓每個測試以自動方式與全新的資料庫實例一起執行：

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

`DataAccessLayerTest` 類別（*run-unittests/DataAccessLayerTest*）中的每個測試方法都遵循類似的相片順序-判斷法模式：

1. 排列：已針對測試設定資料庫，並（或）定義預期的結果。
1. Act：執行測試。
1. 判斷提示：決定測試結果是否成功的判斷提示。

例如，`DeleteMessageAsync` 方法會負責移除其 `Id` 所識別的單一訊息（*src/RazorPagesTestSample/Data/AppDbCoNtext*）：

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

這個方法有兩個測試。 一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。 另一個方法則是測試如果訊息 `Id` 不存在時，資料庫不會變更。 `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` 方法如下所示：

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。 植入訊息會取得並保留在 `seedMessages`中。 植入訊息會儲存到資料庫中。 具有 `1` `Id` 的訊息已設定為要刪除。 執行 `DeleteMessageAsync` 方法時，預期的訊息應該會有所有訊息，但是除了具有 `Id` 的 `1`以外。 `expectedMessages` 變數代表此預期的結果。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法的作用： `DeleteMessageAsync` 方法會在 `recId` 的 `1`中執行：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最後，方法會從內容取得 `Messages`，並將其與 `expectedMessages` 判斷提示兩者相等：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

為了比較這兩個 `List<Message>` 是相同的：

* 訊息會依 `Id`排序。
* 訊息配對會在 `Text` 屬性上進行比較。

類似的測試方法，`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 檢查嘗試刪除不存在之訊息的結果。 在此情況下，在執行 `DeleteMessageAsync` 方法之後，資料庫中預期的訊息應該等於實際的訊息。 資料庫的內容應該不會有任何變更：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>頁面模型方法的單元測試

另一組單元測試負責測試頁面模型方法。 在訊息應用程式中，索引頁面模型位於*src/RazorPagesTestSample/Pages/Index. cshtml .cs*的 `IndexModel` 類別中。

| 頁面模型方法 | 函式 |
| ----------------- | -------- |
| `OnGetAsync` | 使用 `GetMessagesAsync` 方法，從 DAL 取得適用于 UI 的訊息。 |
| `OnPostAddMessageAsync` | 如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 將訊息新增至資料庫。 |
| `OnPostDeleteAllMessagesAsync` | 呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。 |
| `OnPostDeleteMessageAsync` | 執行 `DeleteMessageAsync`，以使用指定的 `Id` 刪除訊息。 |
| `OnPostAnalyzeMessagesAsync` | 如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。 |

頁面模型方法會使用 `IndexPageTests` 類別中的七個測試進行測試（[*測試]/[RazorPagesTestSample]/[run-unittests]/[IndexPageTests*]）。 測試會使用熟悉的「排列-判斷提示-Act」模式。 這些測試著重于：

* 判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。
* 確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult>。
* 檢查是否已正確設定屬性值。

這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。 例如，`AppDbContext` 的 `GetMessagesAsync` 方法是模擬來產生輸出。 當頁面模型方法執行這個方法時，mock 會傳回結果。 資料不是來自資料庫。 這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何模擬頁面模型的 `GetMessagesAsync` 方法：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。

單元測試 Act 步驟（test */RazorPagesTestSample/run-unittests/IndexPageTests*）：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` 頁面模型的 `OnGetAsync` 方法（*src/RazorPagesTestSample/Pages/Index. cshtml .cs*）：

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

DAL 中的 `GetMessagesAsync` 方法不會傳回這個方法呼叫的結果。 模擬版本的方法會傳回結果。

在 `Assert` 步驟中，會從頁面模型的 `Messages` 屬性指派實際的訊息（`actualMessages`）。 指派訊息時也會執行類型檢查。 預期和實際的訊息會依其 `Text` 屬性進行比較。 測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

此群組中的其他測試會建立頁面模型物件，其中包括 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext>、<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary>、建立 `PageContext`的 <xref:Microsoft.AspNetCore.Mvc.ActionContext>、`ViewDataDictionary`和 `PageContext`。 這些適用于進行測試。 例如，訊息應用程式會使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 建立 `ModelState` 錯誤，以確認執行 `OnPostAddMessageAsync` 時傳回的是有效的 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>其他資源

* [使用 dotnet C#測試和 xUnit 在 .net Core 中進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [對程式碼進行單元測試](/visualstudio/test/unit-test-your-code)（Visual Studio）
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq 快速入門](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end
