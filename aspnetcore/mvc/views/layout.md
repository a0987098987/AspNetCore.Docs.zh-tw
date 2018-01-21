---
title: "配置"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: f225e2a93edfc552961f9f16294bc0ace6eb0002
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="layout"></a>配置

由[Steve Smith](https://ardalis.com/)

檢視通常會共用視覺與程式設計項目。 在本文中，您將學習如何使用一般的版面配置、 共用指示詞，以及之前呈現檢視的通用程式碼中執行您的 ASP.NET 應用程式。

## <a name="what-is-a-layout"></a>什麼是版面配置

大部分的 web 應用程式有通用的版面配置可提供使用者一致的體驗它們瀏覽 頁面。 配置通常包括一般使用者介面項目，例如應用程式標頭、 瀏覽或功能表項目和頁尾。

![頁面版面配置範例](layout/_static/page-layout.png)

常見的 HTML 結構，例如指令碼和樣式表也經常使用的應用程式中的許多網頁。 所有這些共用項目中可能會定義*配置*然後可以在應用程式中使用任何檢視所參考的檔案。 配置減少重複的程式碼，在檢視中，協助他們遵循[不重複自行 （乾） 原則](http://deviq.com/don-t-repeat-yourself/)。

依照慣例，名為 ASP.NET 應用程式的預設版面配置`_Layout.cshtml`。 Visual Studio ASP.NET Core MVC 專案範本包含在此配置檔案`Views/Shared`資料夾：

![在 [方案總管] 的 [檢視] 資料夾](layout/_static/web-project-views.png)

此配置命名為應用程式中定義檢視的最上層範本。 應用程式不需要配置，且應用程式可以定義一個以上的版面配置，以指定不同的版面配置的不同檢視。

範例`_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>指定的配置

Razor 檢視有`Layout`屬性。 個別檢視指定版面配置設定此屬性：

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

指定的配置可以使用完整路徑 (範例： `/Views/Shared/_Layout.cshtml`) 或部分名稱 (範例： `_Layout`)。 提供的部分名稱時，Razor 檢視引擎將搜尋配置檔使用其標準的探索程序。 控制器相關聯的資料夾中搜尋第一次，後面接著`Shared`資料夾。 此探索程序等同於用來探索[部分檢視](partial.md)。

根據預設，必須呼叫每個配置`RenderBody`。 無論在何處呼叫`RenderBody`是用來放置，檢視的內容會呈現。

<a name="layout-sections-label"></a>

### <a name="sections"></a>章節

配置可以選擇性地參考一個或多個*區段*，藉由呼叫`RenderSection`。 各節會提供一種方式組織特定頁面項目應放置的位置。 每次呼叫`RenderSection`可以指定該區段是必要或選擇性。 如果找不到所需的區段，將會擲回例外狀況。 個別檢視指定的內容區段使用呈現`@section`Razor 語法。 如果檢視定義的區段，必須加以呈現 （或會發生錯誤）。

範例`@section`定義在檢視中：

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

在上述程式碼，驗證指令碼會加入至`scripts`包含表單的檢視上一節。 相同的應用程式中的其他檢視可能不需要任何額外的指令碼，並因此將不需要重新定義指令碼區段。

在檢視中所定義的是僅適用於其立即配置頁面。 無法從 partials、 檢視元件或檢視系統其他部分參考它們。

### <a name="ignoring-sections"></a>正在略過區段

根據預設，本文和內容頁面中的所有區段必須全部轉譯的版面配置頁。 Razor 檢視引擎將會強制此追蹤是否呈現在主體和每個區段。

若要指示要略過的內容或章節的檢視引擎，請呼叫`IgnoreBody`和`IgnoreSection`方法。

在主體和 Razor 頁面中的每個區段必須轉譯或忽略。

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>匯入共用的指示詞

檢視可以執行許多動作，例如匯入命名空間，或使用 Razor 指示詞[相依性插入](dependency-injection.md)。 由許多檢視共用的指示詞可指定在一般`_ViewImports.cshtml`檔案。 `_ViewImports`檔案支援下列指示詞：

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

檔案不支援其他 Razor 的功能，例如函式和區段定義。

範例`_ViewImports.cshtml`檔案：

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml`檔案 ASP.NET Core MVC 應用程式通常會放在`Views`資料夾。 A`_ViewImports.cshtml`檔案可以放在其中只會套用至案例檢視該資料夾以及其子資料夾內的任何資料夾內。 `_ViewImports`在根層級中，開始處理的檔案，然後針對每個備妥的資料夾檢視本身的位置，設定指定在根層級可能會覆寫資料夾層級。

比方說，如果根層級`_ViewImports.cshtml`檔案會指定`@model`和`@addTagHelper`，和另一個`_ViewImports.cshtml`控制器相關聯的檢視資料夾中的檔案指定不同`@model`並加入另一個`@addTagHelper`，檢視將能存取這兩個標記協助程式，並使用後者`@model`。

若為多個`_ViewImports.cshtml`檔案會執行檢視，結合中包含的指示詞的行為`ViewImports.cshtml`檔案會顯示如下：

* `@addTagHelper``@removeTagHelper`： 所有執行，順序

* `@tagHelperPrefix`： 檢視最接近的其中一個會覆寫任何其他項目

* `@model`： 檢視最接近的其中一個會覆寫任何其他項目

* `@inherits`： 檢視最接近的其中一個會覆寫任何其他項目

* `@using`： 所有都包含在內。會忽略重複的項目

* `@inject`： 每個屬性，以檢視最接近的其中一個會覆寫具有相同屬性名稱的任何其他

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>執行每個檢視之前的程式碼

如果您的程式碼，您必須執行的每個檢視之前，這應該放在`_ViewStart.cshtml`檔案。 依照慣例，`_ViewStart.cshtml`檔案位於`Views`資料夾。 中所列的陳述式`_ViewStart.cshtml`（不版面配置和非部分檢視） 的每個完整檢視之前執行。 像[ViewImports.cshtml](xref:mvc/views/layout#viewimports)，`_ViewStart.cshtml`是階層式。 如果`_ViewStart.cshtml`檔案在控制器相關聯的檢視資料夾中所定義，它會執行之後的根目錄中定義的`Views`資料夾 （如果有的話）。

範例`_ViewStart.cshtml`檔案：

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

上面的檔案可讓您指定的所有檢視將都使用`_Layout.cshtml`版面配置。

> [!NOTE]
> 既不`_ViewStart.cshtml`也`_ViewImports.cshtml`通常會放在`/Views/Shared`資料夾。 這些檔案的應用程式層級版本應該放在`/Views`資料夾。
