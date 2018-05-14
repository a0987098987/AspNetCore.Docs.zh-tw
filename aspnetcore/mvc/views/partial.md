---
title: ASP.NET Core 中的部分檢視
author: ardalis
description: 了解部分檢視如何在另一個檢視內呈現，以及何時應該在 ASP.NET Core 應用程式中使用它們。
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: 3deaaeb666e5443d0784f2ac6977e58e1b25d711
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2018
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core 中的部分檢視

作者：[Steve Smith](https://ardalis.com/)、[Maher JENDOUBI](https://twitter.com/maherjend)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Scott Sauber](https://twitter.com/scottsauber)

當您想要在不同檢視之間共用網頁的可重複使用組件時，ASP.NET Core MVC 支援的部分檢視就相當實用。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>什麼是部分檢視？

部分檢視是一種可在其他檢視中進行轉譯的檢視。 系統會將執行部分檢視所產生的 HTML 輸出，轉譯成呼叫檢視 (或父檢視)。 部分檢視和其他檢視一樣都使用 *.cshtml* 副檔名。

## <a name="when-should-i-use-partial-views"></a>何時應該使用部分檢視？

如果您想將大型檢視拆解成較小的元件，部分檢視是一種有效的方式。 它們可以降低檢視內容重複的情況，並重複使用檢視的項目。 您應該在 [_Layout.cshtml](layout.md) 中指定一般版面配置項目。 非版面配置的可重複使用內容則可以封裝到部分檢視中。

如果您的複雜頁面是由多個邏輯項目所組成，就很適合將每個項目作為個別的部分檢視。 每個頁面片段都可以獨立於其餘頁面進行檢視，因此頁面本身的檢視會變得簡單許多，因為它只包含整體頁面結構以及轉譯部分檢視的呼叫。

提示：使用檢視時，請遵循 [Don't Repeat Yourself](http://deviq.com/don-t-repeat-yourself/) (不重複) 原則。

## <a name="declaring-partial-views"></a>宣告部分檢視

部分檢視的建立方式與其他檢視一樣：您可以在 *Views* 資料夾內建立一個 *.cshtml* 檔案。 部分檢視和一般檢視之間沒有任何語意差別，只是以不同的方式轉譯。 控制器的 `ViewResult` 會直接傳回檢視，您可以將這個檢視作為部分檢視。 一般檢視與部分檢視的轉譯方式主要差異在於，部分檢視不會執行 *_ViewStart.cshtml* (但一般檢視則會執行，如需詳細資訊，請參閱[版面配置](layout.md)中的 *_ViewStart.cshtml*)。

## <a name="referencing-a-partial-view"></a>參考部分檢視

在檢視頁面中，您可以使用數種方法來轉譯部分檢視。 最佳做法是使用 `Html.PartialAsync`，它會傳回 `IHtmlString`，您可在呼叫前加上 `@` 以參考這個項目：

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

您可以使用 `RenderPartialAsync` 來轉譯部分檢視。 這個方法不會傳回結果，而會將轉譯的輸出直接串流給回應。 既然它不會傳回結果，您就必須在 Razor 程式碼區塊內對其進行呼叫：

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=11-13)]

由於它會直接串流結果，因此在某些情況下，`RenderPartialAsync` 可能效能會更好。 不過，建議您使用 `PartialAsync`。

雖然有 `Html.PartialAsync` (`Html.Partial`) 和 `Html.RenderPartialAsync` (`Html.RenderPartial`) 的同步對等項目，但不建議使用同步對等項目，因為存在其發生死結的情況。 同步方法在未來版本中無法使用。

> [!NOTE]
> 如果您的檢視需要執行程式碼，則建議的模式是使用[檢視元件](view-components.md)，而不是使用部分檢視。

### <a name="partial-view-discovery"></a>部分檢視探索

參考部分檢視時，您可以透過數種方式來參考它的位置：

```cshtml
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@await Html.PartialAsync("ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@await Html.PartialAsync("~/Views/Folder/ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@await Html.PartialAsync("../Account/LoginPartial.cshtml")
```

您可以在不同的檢視資料夾中，以相同名稱命名不同的部分檢視。 依據名稱 (不含副檔名) 參考檢視時，每個資料夾中的檢視會使用位於相同資料夾中的部分檢視。 您也可以指定使用預設的部分檢視，並將其放在 *Shared* 資料夾中。 如果檢視沒有專屬版本的部分檢視，就會使用共用的部分檢視。 您可以使用預設的部分檢視 (在 *Shared*)，而與父檢視位於相同資料夾中的同名部分檢視會將其覆寫。

您可以「鏈結」部分檢視。 也就是說，部分檢視可以呼叫另一個部分檢視 (只要您不建立迴圈)。 在每一個檢視或部分檢視內，相對路徑一律是相對於該檢視，而非相對於根目錄或父檢視。

> [!NOTE]
> 如果您在部分檢視中宣告 [Razor](razor.md) `section`，即會將其限於部分檢視，而不會向其父檢視顯示。

## <a name="accessing-data-from-partial-views"></a>從部分檢視存取資料

將部分檢視具現化時，它會取得一份父檢視的 `ViewData` 字典。 父檢視不會保存部分檢視內的資料更新。 傳回部分檢視時，部分檢視內有所變更的 `ViewData` 會遺失。

您可以將 `ViewDataDictionary` 的執行個體傳遞給部分檢視：

```cshtml
@await Html.PartialAsync("PartialName", customViewData)
```

您也可以將模型傳入部分檢視。 這可以是頁面的檢視模型或自訂物件。 您可以將模型傳遞給 `PartialAsync` 或 `RenderPartialAsync`：

```cshtml
@await Html.PartialAsync("PartialName", viewModel)
```

您可以將 `ViewDataDictionary` 的執行個體和檢視模型傳遞給部分檢視：

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

下列標記表示 *Views/Articles/Read.cshtml* 檢視，其中包含兩個部分檢視。 第二個部分檢視將模型和 `ViewData` 傳入部分檢視。 如果您使用 `ViewDataDictionary` 的建構函式多載 (醒目標示如下)，即可傳遞新的 `ViewData` 字典，同時保留現有的 `ViewData`：

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Views/Shared/AuthorPartial*：

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection* 部分檢視：

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

執行階段期間會將部分檢視轉譯為父檢視，而這個父檢視本身則在共用的 *_Layout.cshtml* 當中轉譯

![部分檢視輸出](partial/_static/output.png)
