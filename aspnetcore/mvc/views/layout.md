---
title: ASP.NET Core 中的配置
author: ardalis
description: 了解如何先使用通用配置、共用指示詞，以及執行通用程式碼，再轉譯 ASP.NET 應用程式中的檢視。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/layout
ms.openlocfilehash: 8e89c8e6cf18c47abb6bf432cdc6bb6b97e8aeb0
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="layout-in-aspnet-core"></a>ASP.NET Core 中的配置

作者：[Steve Smith](https://ardalis.com/)

檢視經常會共用視覺效果和程式設計項目。 在本文中，您將了解如何先使用通用配置、共用指示詞，以及執行通用程式碼，再轉譯 ASP.NET 應用程式中的檢視。

## <a name="what-is-a-layout"></a>何謂配置

大部分的 Web 應用程式都會有通用配置，可提供使用者一致的巡覽頁面體驗。 配置通常會包括一般使用者介面項目，例如應用程式標頭、導覽或功能表項目，以及頁尾。

![頁面配置範例](layout/_static/page-layout.png)

應用程式內的許多頁面也經常使用指令碼和樣式表這類一般 HTML 結構。 所有這些共用項目可能都定義在 *layout* 檔案中，而之後應用程式內使用的任何檢視都可以參考該檔案。 配置可減少檢視中的重複程式碼，協助它們遵循[不重複 (DRY) 原則](http://deviq.com/don-t-repeat-yourself/)。

依照慣例，ASP.NET 應用程式的預設配置命名為 `_Layout.cshtml`。 Visual Studio ASP.NET Core MVC 專案範本會將此配置檔案包含在 `Views/Shared` 資料夾中：

![方案總管中的 views 資料夾](layout/_static/web-project-views.png)

此配置定義應用程式中檢視的最上層範本。 應用程式不需要配置，而且應用程式可以定義數個配置，即不同的檢視指定不同的配置。

範例 `_Layout.cshtml`：

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>指定配置

Razor 檢視具有 `Layout` 屬性。 個別檢視透過設定此屬性來指定配置：

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

指定的配置可以使用完整路徑 (範例：`/Views/Shared/_Layout.cshtml`) 或部分名稱 (範例：`_Layout`)。 提供部分名稱時，Razor 檢視引擎將使用其標準探索程序來搜尋配置檔案。 會先搜尋控制器相關聯資料夾，接著再搜尋 `Shared` 資料夾。 此探索程序相當於用來探索[部分檢視](partial.md)的程序。

根據預設，每個配置都必須呼叫 `RenderBody`。 不論在何處呼叫 `RenderBody`，都會轉譯檢視內容。

<a name="layout-sections-label"></a>

### <a name="sections"></a>章節

配置可以選擇性地呼叫 `RenderSection`，以參考一或多個「區段」。 區段提供一種方式，來組織特定頁面項目應放置的位置。 每次呼叫 `RenderSection` 都可以指定該區段是必要區段還是選擇性區段。 如果找不到必要區段，則會擲回例外狀況。 個別檢視指定要使用 `@section` Razor 語法在區段內轉譯的內容。 如果檢視定義區段，則必須進行轉譯 (或發生錯誤)。

檢視中的範例 `@section` 定義：

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

在上述程式碼中，會將驗證指令碼新增至包含表單之檢視上的 `scripts` 區段。 相同應用程式中的其他檢視可能不需要任何額外的指令碼，因此不需要定義 scripts 區段。

檢視中所定義的區段僅適用於其立即配置頁面。 無法從部分、檢視元件或檢視系統的其他部分參考它們。

### <a name="ignoring-sections"></a>忽略區段

根據預設，內容頁面中的本文和所有區段都必須透過配置頁面進行轉譯。 Razor 檢視引擎會強制執行此作業，方法是追蹤是否已轉譯本文和每個區段。

若要指示檢視引擎略過本文或區段，請呼叫 `IgnoreBody` 和 `IgnoreSection` 方法。

Razor 頁面中的本文和每個區段都必須進行轉譯或忽略。

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>匯入共用指示詞

檢視可以使用 Razor 指示詞來執行許多動作，例如匯入命名空間或執行[相依性插入](dependency-injection.md)。 許多檢視所共用的指示詞可能指定於通用 `_ViewImports.cshtml` 檔案中。 `_ViewImports` 檔案支援下列指示詞：

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

檔案不支援其他 Razor 功能，例如函式和區段定義。

範例 `_ViewImports.cshtml` 檔案：

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

ASP.NET Core MVC 應用程式的 `_ViewImports.cshtml` 檔案通常會放在 `Views` 資料夾中。 `_ViewImports.cshtml` 檔案可以放在任何資料夾內；在此情況下，它只會套用至該資料夾和其子資料夾內的檢視。 `_ViewImports` 檔案的處理是從根層級開始，然後針對到檢視本身位置的所有資料夾；因此，在資料夾層級，可能會覆寫根層級上指定的設定。

例如，如果根層級 `_ViewImports.cshtml` 檔案指定 `@model` 和 `@addTagHelper`，但檢視之控制器相關聯資料夾中的另一個 `_ViewImports.cshtml` 檔案指定不同的 `@model` 並新增另一個 `@addTagHelper`，則檢視可以存取這兩個標籤協助程式，並使用後面這個 `@model`。

如果為檢視執行多個 `_ViewImports.cshtml` 檔案，則 `ViewImports.cshtml` 檔案中所含指示詞的結合行為將如下：

* `@addTagHelper`、`@removeTagHelper`：全部依序執行

* `@tagHelperPrefix`：最接近檢視的項目會覆寫任何其他項目

* `@model`：最接近檢視的項目會覆寫任何其他項目

* `@inherits`：最接近檢視的項目會覆寫任何其他項目

* `@using`：全部包括，但忽略重複項目

* `@inject`：針對每個屬性，最接近檢視的項目會覆寫任何其他具有相同屬性名稱的項目

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>在每個檢視之前執行程式碼

如果您的程式碼需要在每個檢視之前執行，則這應該放在 `_ViewStart.cshtml` 檔案中。 依照慣例，`_ViewStart.cshtml` 檔案位於 `Views` 資料夾中。 `_ViewStart.cshtml` 中所列的陳述式會在每個完整檢視之前執行 (不是配置，也不是部分檢視)。 與 [ViewImports.cshtml](xref:mvc/views/layout#viewimports) 類似，`_ViewStart.cshtml` 是階層式的。 如果 `_ViewStart.cshtml` 檔案是定義於控制器相關聯檢視資料夾，則會在 `Views` 資料夾 (如果有的話) 根目錄中所定義的檔案後面進行執行。

範例 `_ViewStart.cshtml` 檔案：

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

上面的檔案指定所有檢視都會使用 `_Layout.cshtml` 配置。

> [!NOTE]
> `_ViewStart.cshtml` 和 `_ViewImports.cshtml` 通常都不是放在 `/Views/Shared` 資料夾中。 這些檔案的應用程式層級版本應該直接放在 `/Views` 資料夾中。
