---
title: ASP.NET Core 中的部分檢視
author: ardalis
description: 了解如何使用部分檢視來分割大型的標記檔案，並減少 ASP.NET Core 應用程式中跨 Web 網頁一般標記的重複。
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/partial
ms.openlocfilehash: 47bd91f4d2bf166a4d0c9a0829e24cbe26a81a10
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85399708"
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core 中的部分檢視

作者：[Steve Smith](https://ardalis.com/)、[Maher JENDOUBI](https://twitter.com/maherjend)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Scott Sauber](https://twitter.com/scottsauber)

部分視圖是在 [Razor](xref:mvc/views/razor) 另一個標記檔案轉譯輸出*中*呈現 HTML 輸出的標記檔案（*. cshtml*）。

::: moniker range=">= aspnetcore-2.1"

開發 MVC 應用程式（其中標記檔案稱為*views*）或頁面應用程式（其中標記檔案稱為頁面）時，會使用「*部分視圖*」一詞 Razor 。 *pages* 本主題的一般是以標記檔案的形式來參考 MVC views 和 Razor pages 頁面。 *markup files*

::: moniker-end

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="when-to-use-partial-views"></a>使用部分檢視的時機

針對下列項目，部分檢視是有效的方法：

* 將大型標記檔案分解為較小的元件。

  在由多個邏輯部分組成的大型複雜標記檔案中，使用隔離至部分檢視中的每個部分具有優勢。 標記檔案中的程式碼為可管理，因為標記僅包含整體頁面結構和對部分檢視的參考。
* 減少標記檔案中一般標記內容的重複。

  當在標記檔案中使用相同的標記項目時，部分檢視會將重複的標記內容移除至某個部分檢視檔案中。 在部分檢視中變更標記時，會更新使用部分檢視之標記檔案的轉譯輸出。

部分檢視不應該用於維護一般版面配置項目。 您應該在 [_Layout.cshtml](xref:mvc/views/layout) 檔案中指定一般版面配置項目。

請勿使用需要複雜轉譯邏輯或程式碼執行來轉譯標記的部分檢視。 請使用[檢視元件](xref:mvc/views/view-components)，而非部分檢視。

## <a name="declare-partial-views"></a>宣告部分檢視

::: moniker range=">= aspnetcore-2.0"

部分視圖是在*Views*資料夾（MVC）或*pages*資料夾（pages）中維護的 *. cshtml*標記檔案。 Razor

在 ASP.NET Core MVC 中，控制器的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 能夠傳回檢視或部分檢視。 在 Razor 頁面中， <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 可以傳回以物件表示的部分視圖 <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> 。 [參考部分檢視](#reference-a-partial-view)一節介紹參考和轉譯部分檢視。

不同於 MVC 檢視或網頁轉譯，部分檢視不會執行 *_ViewStart.cshtml*。 如需 *_ViewStart.cshtml* 的詳細資訊，請參閱 <xref:mvc/views/layout>。

部分檢視檔案名稱通常以底線 (`_`) 開頭。 您不一定要依照這項慣例命名，但最好先在視覺呈現上將部分檢視與檢視和頁面區別開來。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

部分檢視是在「檢視」** 資料夾中維護的 *.cshtml* 標記檔案。

控制器的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 能夠傳回檢視或部分檢視。 [參考部分檢視](#reference-a-partial-view)一節介紹參考和轉譯部分檢視。

不同於 MVC 檢視轉譯，部分檢視不會執行 *_ViewStart.cshtml*。 如需 *_ViewStart.cshtml* 的詳細資訊，請參閱 <xref:mvc/views/layout>。

部分檢視檔案名稱通常以底線 (`_`) 開頭。 您不一定要依照這項慣例命名，但最好先在視覺呈現上將部分檢視與檢視區別開來。

::: moniker-end

## <a name="reference-a-partial-view"></a>參考部分檢視

::: moniker range=">= aspnetcore-2.0"

### <a name="use-a-partial-view-in-a-razor-pages-pagemodel"></a>在頁面 PageModel 中使用部分視圖 Razor

在 ASP.NET Core 2.0 或2.1 中，下列處理常式方法會將* \_ AuthorPartialRP*部分視圖呈現給回應：

```csharp
public IActionResult OnGetPartial() =>
    new PartialViewResult
    {
        ViewName = "_AuthorPartialRP",
        ViewData = ViewData,
    };
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

在 ASP.NET Core 2.2 或更新版本中，處理常式方法也可以呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> 方法來產生 `PartialViewResult` 物件：

[!code-csharp[](partial/sample/PartialViewsSample/Pages/DiscoveryRP.cshtml.cs?name=snippet_OnGetPartial)]

::: moniker-end

### <a name="use-a-partial-view-in-a-markup-file"></a>在標記檔案中使用部分檢視

::: moniker range=">= aspnetcore-2.1"

在標記檔案中，您可以使用數種方法參考部分檢視。 我們建議應用程式使用下列其中一個非同步轉譯方法：

* [部分標記協助程式](#partial-tag-helper)
* [非同步 HTML 協助程式](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

在標記檔案中，您可以使用兩種方法參考部分檢視：

* [非同步 HTML 協助程式](#asynchronous-html-helper)
* [同步 HTML 協助程式](#synchronous-html-helper)

我們建議應用程式使用[非同步 HTML 協助程式](#asynchronous-html-helper)。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Partial 標籤協助程式

[部分標記](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)協助程式需要 ASP.NET Core 2.1 或更新版本。

部分標籤協助程式會以非同步方式轉譯內容，並使用類似於 HTML 的語法：

```cshtml
<partial name="_PartialName" />
```

當存在檔案副檔名時，部分標籤協助程式會參考部分檢視，該檢視必須與呼叫部分檢視的標記檔案位於相同資料夾中：

```cshtml
<partial name="_PartialName.cshtml" />
```

下列範例會從應用程式根目錄參考部分檢視。 開頭為波狀符號斜線 (`~/`) 或斜線 (`/`) 的路徑表示應用程式根目錄：

**Razor頁面**

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

**MVC**

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

下列範例會以相對路徑參考部分檢視：

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

如需詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper> 。

::: moniker-end

### <a name="asynchronous-html-helper"></a>非同步 HTML 協助程式

使用 HTML 協助程式時，最佳做法是使用 <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>。 `PartialAsync` 會傳回包裝在 <xref:System.Threading.Tasks.Task%601> 的 <xref:Microsoft.AspNetCore.Html.IHtmlContent> 類型。 方法的參考方式，是在等候的呼叫前面加上 `@` 字元：

```cshtml
@await Html.PartialAsync("_PartialName")
```

當存在檔案副檔名時，HTML 協助程式會參考部分檢視，該檢視必須與呼叫部分檢視的標記檔案位於相同資料夾中：

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

下列範例會從應用程式根目錄參考部分檢視。 開頭為波狀符號斜線 (`~/`) 或斜線 (`/`) 的路徑表示應用程式根目錄：

::: moniker range=">= aspnetcore-2.1"

**Razor頁面**

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

**MVC**

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

下列範例會以相對路徑參考部分檢視：

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

或者，您可以使用 <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*> 轉譯部分檢視。 此方法不會傳回 <xref:Microsoft.AspNetCore.Html.IHtmlContent>。 ，而是將轉譯輸出直接串流給回應。 因為方法不會傳回結果，所以必須在程式碼區塊內呼叫它 Razor ：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

由於 `RenderPartialAsync` 會串流轉譯的內容，因此在某些情況下可提供更好的效能。 在效能十分重要的情況下，請使用這兩種方法對頁面進行效能評定，並使用可產生更快速回應的方法。

### <a name="synchronous-html-helper"></a>同步 HTML 協助程式

<xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> 和 <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> 分別是 `PartialAsync` 和 `RenderPartialAsync` 的同步對等方法。 不建議使用同步對等，原因是會出現這些方法發生死結的情況。 同步方法將於未來版本中移除。

> [!IMPORTANT]
> 如果您需要執行程式碼，請使用[檢視元件](xref:mvc/views/view-components)，而非部分檢視。

::: moniker range=">= aspnetcore-2.1"

呼叫 `Partial` 或 `RenderPartial` 會產生 Visual Studio Analyzer 警告。 例如，`Partial` 的存在會產生下列警告訊息：

> 使用 IHtmlHelper.Partial 可能會導致應用程式死結。 請考慮使用&lt;部分&gt;標籤協助程式或 IHtmlHelper.PartialAsync。

將對的呼叫取代為 `@Html.Partial` `@await Html.PartialAsync` 或[部分標記](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)協助程式。 如需 Partial 標籤協助程式移轉的詳細資訊，請參閱[從 HTML 協助程式移轉](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper)。

::: moniker-end

## <a name="partial-view-discovery"></a>部分檢視探索

如果以不含檔案副檔名的名稱參考部分檢視，則會依所述順序搜尋下列位置：

::: moniker range=">= aspnetcore-2.1"

**Razor頁面**

1. 目前正在執行頁面的資料夾
1. 頁面資料夾上方的目錄圖表
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

**MVC**

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

部分檢視探索適用於下列慣例：

* 當部分檢視位於不同的資料夾中時，可允許使用具有相同檔案名稱的不同部分檢視。
* 當以不含檔案副檔名的名稱參考部分檢視，且部分檢視出現在呼叫者的資料夾和*共用*資料夾中時，呼叫者資料夾中的部分檢視會提供部分檢視。 如果呼叫者資料夾中不存在部分檢視，則會從「共用」** 資料夾中提供部分檢視。 「共用」** 資料夾中的部分檢視稱為「共用部分檢視」** 或「預設部分檢視」**。
* 部分視圖可以*連結* &mdash; ，部分視圖可以在呼叫不是由迴圈參考時呼叫另一個部分視圖。 相對路徑一律相對於目前的檔案，而不是相對於檔案的根目錄或父檔案。

> [!NOTE]
> [Razor](xref:mvc/views/razor) `section` 父標記檔案不會顯示在部分視圖中定義的。 `section` 只會顯示在具有其定義的部分檢視。

## <a name="access-data-from-partial-views"></a>從部分檢視存取資料

將部分檢視具現化時，會收到父檢視 `ViewData` 字典的*複本*。 父檢視不會保存部分檢視內的資料更新。 傳回部分檢視時，部分檢視內的 `ViewData` 變更會遺失。

下列範例示範如何將 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 的執行個體傳遞給部分檢視：

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

您可以將模型傳入部分檢視。 模型可以是自訂物件。 您可以使用 `PartialAsync` (對呼叫者呈現內容區塊) 或 `RenderPartialAsync` (將內容串流至輸出) 來傳遞模型：

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

**Razor頁面**

範例應用程式中的下列標記來自 *Pages/ArticlesRP/ReadRP.cshtml* 頁面。 此頁面包含兩個部分檢視。 第二個部分檢視將模型和 `ViewData` 傳入部分檢視。 `ViewDataDictionary` 建構函式多載用於傳遞新的 `ViewData` 字典，同時保留現有的 `ViewData` 字典。

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-20)]

*Pages/Shared/_AuthorPartialRP.cshtml* 是由 *ReadRP.cshtml* 標記檔案參考的第一個部分檢視：

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

*Pages/ArticlesRP/_ArticleSectionRP.cshtml* 是由 *ReadRP.cshtml* 標記檔案參考的第二個部分檢視：

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

**MVC**

::: moniker-end

範例應用程式中的下列標記顯示 *Views/Articles/Read.cshtml* 檢視。 此檢視包含兩個部分檢視。 第二個部分檢視將模型和 `ViewData` 傳入部分檢視。 `ViewDataDictionary` 建構函式多載用於傳遞新的 `ViewData` 字典，同時保留現有的 `ViewData` 字典。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-20)]

*Views/Shared/_AuthorPartial。 cshtml*是*Read. cshtml*標記檔案所參考的第一個部分視圖：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*Views/Articles/_ArticleSection.cshtml* 是由 *Read.cshtml* 標記檔案參考的第二個部分檢視：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

在執行階段期間，會將部分檢視轉譯為父標記檔案的轉譯輸出，而這個父檢視本身則在共用的 *_Layout.cshtml* 中轉譯。 第一個部分檢視會轉譯文章作者的名稱和發行日期：

> Abraham Lincoln
>
> 此部分檢視來自&lt;共用的部分檢視檔案路徑&gt;。
> 1863 年 11 月 19 日上午 12:00:00

第二個部分檢視會轉譯文章的章節：

> 第一節索引：0
>
> 八十七年前...
>
> 第二節索引：1
>
> 如今，我們正在進行一場偉大的內戰，考驗著...
>
> 第三節索引：2
>
> 然而，從更廣泛的意義上說，我們無法在此奉獻...

## <a name="additional-resources"></a>其他資源

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
