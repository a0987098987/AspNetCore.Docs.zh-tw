---
title: ASP.NET Core 中的部分檢視
author: ardalis
description: 了解部分檢視如何在另一個檢視內呈現，以及何時應該在 ASP.NET Core 應用程式中使用它們。
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 7cb20fc30609adad83cb40e91316da115817f035
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378679"
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core 中的部分檢視

作者：[Steve Smith](https://ardalis.com/)、[Maher JENDOUBI](https://twitter.com/maherjend)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core 支援部分檢視。 部分檢視是用來跨不同的檢視共用可重複使用的網頁部分。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>什麼是部分檢視

部分檢視是一種可在其他檢視中進行轉譯的檢視。 系統會將執行部分檢視所產生的 HTML 輸出，轉譯成呼叫檢視 (或父檢視)。 部分檢視和其他檢視一樣都使用 *.cshtml* 副檔名。

例如，ASP.NET Core 2.1 **Web 應用程式**專案範本包含 *_CookieConsentPartial.cshtml* 部分檢視。 部分檢視會從 *_Layout.cshtml* 中載入：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>使用部分檢視的時機

如果您想將大型檢視拆解成較小的元件，部分檢視是一種有效的方式。 它們可以降低檢視內容重複的情況，並重複使用檢視的項目。 您應該在 [_Layout.cshtml](xref:mvc/views/layout) 中指定一般版面配置項目。 非版面配置的可重複使用內容則可以封裝到部分檢視中。

在多個邏輯項目組成的複雜頁面中，很適合以該頁面的部分檢視處理各項目。 該頁面的各項目可和頁面的其餘項目分開檢視。 頁面本身的檢視變得更加簡易，原因是只包含整體頁面結構以及轉譯部分檢視的呼叫。

ASP.NET Core MVC 控制器有 [PartialView](/dotnet/api/microsoft.aspnetcore.mvc.controller.partialview#Microsoft_AspNetCore_Mvc_Controller_PartialView) 方法，可從動作方法呼叫此方法。 Razor Pages 沒有對應的 `PartialView` 方法。

## <a name="declare-partial-views"></a>宣告部分檢視

部分檢視的建立方式與一般檢視相同：您可以在 *Views* 資料夾內建立 *.cshtml* 檔案。 部分檢視和一般檢視之間沒有任何語意差別，只是轉譯方式不同。 控制器的 [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult) 會直接傳回檢視，您可以使用這個檢視作為部分檢視。 一般檢視與部分檢視的轉譯方式主要差異在於，部分檢視不會執行 *_ViewStart.cshtml*。 一般檢視則會執行 *_ViewStart.cshtml*。 深入了解[版面配置](xref:mvc/views/layout)中的 *_ViewStart.cshtml*。

依照慣例，部分檢視檔案名稱的開頭通常是 `_`。 您不一定要依照這項慣例命名，但最好先在視覺呈現上將部分檢視與一般檢視區別開來。

## <a name="reference-a-partial-view"></a>參考部分檢視

在檢視頁面中，您可以使用數種方法轉譯部分檢視。 最佳做法是使用非同步轉譯。

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Partial 標籤協助程式

Partial 標籤協助程式需要使用 ASP.NET Core 2.1 或更新版本。 它會非同步轉譯，並使用類似 HTML 的語法：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>。

::: moniker-end

### <a name="asynchronous-html-helper"></a>非同步 HTML 協助程式

在使用 HTML 協助程式時，最佳做法是使用 [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_)。 它會傳回包裝在 `Task` 中的 [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) 類型。 方法的參考方式是在呼叫前面加上 `@`：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

或者，您可以使用 [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync) 轉譯部分檢視。 這個方法不會傳回結果， ，而是將轉譯輸出直接串流給回應。 因為該方法不會傳回結果，所以您必須在 Razor 程式碼區塊內呼叫它：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

由於它會直接串流結果，因此在某些情況下，`RenderPartialAsync` 的效能可能更好。 不過，建議您使用 `PartialAsync`。

### <a name="synchronous-html-helper"></a>同步 HTML 協助程式

[Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) 和 [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) 分別是 `PartialAsync` 和 `RenderPartialAsync` 的同步對等方法。 不建議使用同步對等方法，原因是會出現這些方法發生死結的情況。 未來版本不會包含同步方法。

> [!IMPORTANT]
> 如果您的檢視需要執行程式碼，請使用[檢視元件](xref:mvc/views/view-components)，而非部分檢視。

::: moniker range=">= aspnetcore-2.1"

在 ASP.NET Core 2.1 或更新版本中，呼叫 `Partial` 或 `RenderPartial` 會產生分析器警告。 例如，使用 `Partial` 會產生下列警告訊息：

> 使用 IHtmlHelper.Partial 可能會導致應用程式死結。 請考慮使用 `<partial>` 標籤協助程式或 `IHtmlHelper.PartialAsync`。

將對 `@Html.Partial` 的呼叫取代為對 `@await Html.PartialAsync` 的呼叫或 Partial 標籤協助程式。 如需 Partial 標籤協助程式移轉的詳細資訊，請參閱[從 HTML 協助程式移轉](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper)。

::: moniker-end

## <a name="partial-view-discovery"></a>部分檢視探索

在參考部分檢視時，您可以透過數種方式參考它的位置。 例如: 

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

上述範例會使用 Partial 標籤協助程式，這需要使用 ASP.NET Core 2.1 或更新版本。 下列範例會使用非同步 HTML 協助程式來完成相同的工作。

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

您可以在不同的檢視資料夾中，以相同名稱命名不同的部分檢視。 當您依據名稱 (不含副檔名) 參考檢視時，每個資料夾中的檢視會使用同一個資料夾中的部分檢視。 您也可以指定使用預設的部分檢視，並將其放在 *Shared* 資料夾中。 如果檢視沒有專屬版本的部分檢視，就會使用共用的部分檢視。 您可以使用預設的部分檢視 (在 *Shared*)，而與父檢視位於相同資料夾中的同名部分檢視會將其覆寫。

部分檢視可以互相「鏈結」，並呼叫另一個部分檢視 (只要您不建立迴圈)。 在每個檢視或部分檢視內，相對路徑一律相對於該檢視，而非相對於根檢視或父檢視。

> [!NOTE]
> 父檢視不會顯示在部分檢視中定義的 [Razor](xref:mvc/views/razor) `section`。 `section` 只會顯示在具有其定義的部分檢視。

## <a name="access-data-from-partial-views"></a>從部分檢視存取資料

將部分檢視具現化時，它會取得一份父檢視的 `ViewData` 字典。 父檢視不會保存部分檢視內的資料更新。 傳回部分檢視時，部分檢視內的 `ViewData` 變更會遺失。

您可以將 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 的執行個體傳遞給部分檢視：

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

您可以將模型傳入部分檢視。 該模型可以是頁面的檢視模型或自訂物件。 您可以將模型傳遞給 `PartialAsync` 或 `RenderPartialAsync`：

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

您可以將 `ViewDataDictionary` 的執行個體和檢視模型傳遞給部分檢視：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

下列標記表示 *Views/Articles/Read.cshtml* 檢視，內含兩個部分檢視。 第二個部分檢視將模型和 `ViewData` 傳入部分檢視。 使用醒目提示的 `ViewDataDictionary` 建構函式多載傳遞新的 `ViewData` 字典，同時保留現有的 `ViewData` 字典。

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Views/Shared/_AuthorPartial*：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*_ArticleSection* 部分檢視：

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

在執行階段期間，會將部分檢視轉譯為父檢視，而這個父檢視本身則在共用的 *_Layout.cshtml* 中轉譯。

![部分檢視輸出](partial/_static/output.png)

## <a name="additional-resources"></a>其他資源

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
