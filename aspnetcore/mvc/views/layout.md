---
title: ASP.NET Core 中的配置
author: ardalis
description: 了解如何先使用通用配置、共用指示詞，以及執行通用程式碼，再轉譯 ASP.NET 應用程式中的檢視。
ms.author: riande
ms.date: 07/30/2019
uid: mvc/views/layout
ms.openlocfilehash: 3ba2f459ca2b04a3001e261acab26880b6582500
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289002"
---
# <a name="layout-in-aspnet-core"></a>ASP.NET Core 中的配置

作者：[Steve Smith](https://ardalis.com/) 和 [Dave Brock](https://twitter.com/daveabrock)

頁面和檢視經常會共用視覺效果和程式設計項目。 本文示範如何：

* 使用常見配置。
* 共用指示詞。
* 執行常見的程式碼，再轉譯頁面或檢視。

本文件將探討 ASP.NET Core MVC 兩種不同方法的配置：Razor Pages 和包含檢視的控制器。 針對本主題，差異很小：

* Razor Pages 位於 *Pages* 資料夾。
* 包含檢視的控制器，使用 *Views* 資料夾進行檢視。

## <a name="what-is-a-layout"></a>何謂配置

大部分的 Web 應用程式都會有通用配置，可提供使用者一致的巡覽頁面體驗。 配置通常會包括一般使用者介面項目，例如應用程式標頭、導覽或功能表項目，以及頁尾。

![頁面配置範例](layout/_static/page-layout.png)

應用程式內的許多頁面也經常使用指令碼和樣式表這類一般 HTML 結構。 所有這些共用項目可能都定義在 *layout* 檔案中，而之後應用程式內使用的任何檢視都可以參考該檔案。 配置可減少檢視中的重複程式碼。

依照慣例，ASP.NET Core 應用程式的預設配置命名為 *_Layout.cshtml*。 使用範本建立的新 ASP.NET Core 專案配置檔案為：

* Razor Pages：*Pages/Shared/_Layout.cshtml*

  ![方案總管中的 Pages 資料夾](layout/_static/rp-web-project-views.png)

* 包含檢視的控制器：*Views/Shared/_Layout.cshtml*

  ![方案總管中的 Views 資料夾](layout/_static/mvc-web-project-views.png)

此配置會定義應用程式中檢視的最上層範本。 應用程式不需要配置。 應用程式可以定義數個配置，即不同的檢視指定不同的配置。

下列程式碼顯示使用範本建立之專案 (包含控制器和檢視) 的配置檔案：

[!code-cshtml[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a>指定配置

Razor 檢視具有 `Layout` 屬性。 個別檢視透過設定此屬性來指定配置：

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

指定的配置可以使用完整路徑 (例如 */Pages/Shared/_Layout.cshtml* 或 */Views/Shared/_Layout.cshtml*) 或部分名稱 (例如：`_Layout`)。 提供部分名稱時，Razor 檢視引擎會使用其標準探索程序來搜尋配置檔案。 首先搜尋處理常式方法 (或控制器) 所在的資料夾，接著搜尋 *Shared* 資料夾。 此探索程序相當於用來探索[部分檢視](xref:mvc/views/partial#partial-view-discovery)的程序。

根據預設，每個配置都必須呼叫 `RenderBody`。 不論在何處呼叫 `RenderBody`，都會轉譯檢視內容。

<a name="layout-sections-label"></a>
<!-- https://stackoverflow.com/questions/23327578 -->
### <a name="sections"></a>章節

配置可以選擇性地呼叫 *，以參考一或多個「區段」* `RenderSection`。 區段提供一種方式，來組織特定頁面項目應放置的位置。 每次呼叫 `RenderSection`，都可以指定該區段是必要區段或是選擇性區段：

```html
<script type="text/javascript" src="~/scripts/global.js"></script>

@RenderSection("Scripts", required: false)
```

如果找不到必要區段，將會擲回例外狀況。 個別檢視指定要使用 `@section` Razor 語法在區段內轉譯的內容。 如果頁面或檢視定義區段，則必須進行轉譯 (否則會發生錯誤)。

Razor Pages 檢視中的範例 `@section` 定義：

```html
@section Scripts {
     <script type="text/javascript" src="~/scripts/main.js"></script>
}
```

在前述程式碼中，*scripts/main.js* 會新增至頁面或檢視上的 `scripts` 區段。 相同應用程式中的其他頁面或檢視可能不需要此指令碼，也不會定義指令碼區段。

下列標記會使用 [部分標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) 來轉譯 *_ValidationScriptsPartial.cshtml*：

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

前述標記由 [Scaffolding 識別](xref:security/authentication/scaffold-identity) 產生。

頁面或檢視中所定義的區段僅適用於其立即配置頁面。 無法從部分、檢視元件或檢視系統的其他部分參考它們。

### <a name="ignoring-sections"></a>忽略區段

根據預設，內容頁面中的本文和所有區段都必須透過配置頁面進行轉譯。 Razor 檢視引擎會強制執行此作業，方法是追蹤是否已轉譯本文和每個區段。

若要指示檢視引擎略過本文或區段，請呼叫 `IgnoreBody` 和 `IgnoreSection` 方法。

Razor 頁面中的本文和每個區段都必須進行轉譯或忽略。

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>匯入共用指示詞

檢視和頁面可以使用 Razor 指示詞匯入命名空間，並使用[相依性插入](dependency-injection.md)。 許多檢視所共用的指示詞可能指定於通用 *_ViewImports.cshtml* 檔案中。 `_ViewImports` 檔案支援下列指示詞：

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

檔案不支援其他 Razor 功能，例如函式和區段定義。

範例 `_ViewImports.cshtml` 檔案：

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

ASP.NET Core MVC 應用程式的 *_ViewImports.cshtml* 檔案通常會放在 *Pages* (或 *Views*) 資料夾中。 *_ViewImports.cshtml* 檔案可以放在任何資料夾內；在此案例中，該檔案只會套用至該資料夾和其子資料夾內的頁面或檢視。 `_ViewImports` 檔案會從根層級開始處理，然後處理導向頁面或檢視本身位置的每個資料夾。 根層級指定的 `_ViewImports` 設定可以在資料夾層級覆寫。

例如，假設：

* 根層級 *_ViewImports.cshtml* 檔案包含 `@model MyModel1` 和 `@addTagHelper *, MyTagHelper1`。
* 子資料夾 *_ViewImports.cshtml* 檔案包含 `@model MyModel2` 和 `@addTagHelper *, MyTagHelper2`。

子資料夾中的頁面和檢視可以存取標籤協助程式和 `MyModel2` 模型。

如果在檔案階層有多個 *_ViewImports.cshtml* 檔案，指示詞的合併行為是：

* `@addTagHelper`、`@removeTagHelper`：全部依序執行
* `@tagHelperPrefix`：最接近檢視的項目會覆寫任何其他項目
* `@model`：最接近檢視的項目會覆寫任何其他項目
* `@inherits`：最接近檢視的項目會覆寫任何其他項目
* `@using`：全部包括，但忽略重複項目
* `@inject`：針對每個屬性，最接近檢視的項目會覆寫任何其他具有相同屬性名稱的項目

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>在每個檢視之前執行程式碼

需要在每個檢視或頁面之前執行的程式碼應放在 *_ViewStart.cshtml* 檔案中。 依照慣例， *_ViewStart.cshtml* 檔案會位於*頁面* (或*檢視*) 資料夾中。 *_ViewStart.cshtml* 中所列的陳述式會在每個完整檢視之前執行 (不是配置，也不是部分檢視)。 與 [ViewImports.cshtml](xref:mvc/views/layout#viewimports) 類似， *_ViewStart.cshtml* 是階層式的。 如果 *_ViewStart.cshtml* 檔案是定義於檢視或頁面資料夾中，則會在 *Pages* (或 *Views*) 資料夾 (如果有的話) 根目錄中所定義的檔案後面執行。

範例 *_ViewStart.cshtml* 檔案：

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

上面的檔案指定所有檢視都會使用 *_Layout.cshtml* 配置。

*_ViewStart.cshtml* 和 *_ViewImports.cshtml* 通常**不會**放在 */Pages/Shared* (或 */Views/Shared*) 資料夾中。 這些檔案的應用程式層級版本應該直接放在 */Pages* (或 */Views*) 資料夾中。
