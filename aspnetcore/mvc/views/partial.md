---
title: "部分檢視"
author: ardalis
description: "使用 ASP.NET Core MVC 中的部分檢視"
keywords: "ASP.NET Core，部分檢視部分"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 4be1b12c-b74e-44ff-826b-99ce86e8d464
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: 60f5255ca31accbffffec18053b29810977a5ff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="partial-views"></a>部分檢視

由[Steve Smith](https://ardalis.com/)， [Maher JENDOUBI](https://twitter.com/maherjend)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC 支援部分檢視，當您有想要不同的檢視之間共用的 web 網頁的可重複使用的組件時相當實用。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>部分檢視有哪些？

部分檢視是另一個檢視中呈現的檢視。 所執行的部分檢視產生的 HTML 輸出轉譯成呼叫 （或父） 檢視。 部分檢視所使用的檢視，例如*.cshtml*檔案副檔名。

## <a name="when-should-i-use-partial-views"></a>何時應該使用部分檢視？

部分檢視是較大檢視重大的有效方式。 它們可以減少重複的檢視內容，並允許可重複使用的檢視項目。 一般的版面配置項目應該指定在[_Layout.cshtml](layout.md)。 非版面配置可重複使用的內容可以封裝到部分檢視。

如果您有幾個邏輯部分所組成的複雜頁面時，可能很有用，才能使用它自己的部分檢視為每個片段。 每個頁面片段就可以在獨立於其餘的頁面上，檢視，因為它只包含整體頁面結構和呼叫來呈現部分檢視網頁本身的檢視會變得簡單許多。

提示： 請依照下列[不重複自行原則](http://deviq.com/don-t-repeat-yourself/)在檢視中。

## <a name="declaring-partial-views"></a>宣告的部分檢視

部分檢視會建立任何其他檢視一樣： 您建立*.cshtml*檔案內*檢視*資料夾。 沒有任何部分檢視和一般檢視之間的語意差異-只會以不同的方式呈現。 您可以檢視所傳回的控制站的直接`ViewResult`，和相同檢視可用來當做部分檢視。 在檢視和部分檢視轉譯的方式主要差異在於，部分檢視不會執行*_viewstart.vbhtml* (雖然檢視動作-深入了解*_viewstart.vbhtml*中[版面配置](layout.md)).

## <a name="referencing-a-partial-view"></a>參考的部分檢視

從在檢視頁面上，有數種您可以在將部分檢視轉譯的方式。 最簡單的是，若要使用`Html.Partial`，它會傳回`IHtmlString`前置詞與呼叫的方式，來參考和`@`:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

`PartialAsync`方法有可用的部分 （雖然一般不提倡檢視中的程式碼），檢視包含的非同步程式碼：

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

您可以呈現部分檢視與`RenderPartial`。 這個方法未傳回結果。傳送至回應直接轉譯的輸出資料流。 因為它未傳回結果，必須呼叫 Razor 程式碼區塊內 (您也可以呼叫`RenderPartialAsync`如有必要):

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

因為它會直接串流結果`RenderPartial`和`RenderPartialAsync`在某些情況下可能會更好。 不過，在大部分情況下，則建議您使用`Partial`和`PartialAsync`。

> [!NOTE]
> 如果您的檢視，就需要執行程式碼，但建議的模式是使用[檢視元件](view-components.md)而不是部分檢視。

### <a name="partial-view-discovery"></a>部分檢視探索

當參考的部分檢視，您可以參考其位置數種方式：

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

您可以在不同檢視資料夾具有相同名稱的不同部分檢視。 當參考檢視的名稱 （不含副檔名），每個資料夾中的檢視會使用部分檢視它們的相同資料夾中。 您也可以指定預設部分檢視，若要使用，放在*共用*資料夾。 共用的部分檢視將供沒有自己的版本部分檢視的任何檢視。 您可以有預設的部分檢視 (在*共用*)，這會覆寫具有相同名稱做為父檢視的相同資料夾中的部分檢視。

部分檢視可以是*鏈結*。 也就是說，部分檢視 （只要您未建立迴圈），可以呼叫另一個部分檢視。 在每一個檢視或部分檢視，相對路徑一律是相對於該檢視，而非從根目錄或父檢視。

> [!NOTE]
> 如果您宣告[Razor](razor.md) `section`在部分檢視中，它將看不到其父系，則會限制為部分檢視。

## <a name="accessing-data-from-partial-views"></a>從部分檢視存取資料

部分檢視具現化時，它會取得一份父檢視`ViewData`字典。 父檢視不會保存對部分檢視內的資料更新。 `ViewData`變更的部分檢視時，遺失部分檢視會傳回。

您可以將傳遞的執行個體`ViewDataDictionary`部分檢視：

```csharp
@Html.Partial("PartialName", customViewData)
   ```

您也可以將模型傳遞至部分檢視。 這可以是頁面檢視模型，或某些部分或自訂的物件。 您可以將模型傳遞至`Partial`，`PartialAsync`， `RenderPartial`，或`RenderPartialAsync`:

```csharp
@Html.Partial("PartialName", viewModel)
   ```

您可以將傳遞的執行個體`ViewDataDictionary`和部分檢視的檢視模型：

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

下面顯示的標記*Views/Articles/Read.cshtml*其中包含兩個部分檢視的檢視。 在模型中的第二個部分檢視通過和`ViewData`部分檢視。 您可以在新的傳遞`ViewData`同時保留現有的字典`ViewData`如果您使用的建構函式多載`ViewDataDictionary`反白顯示如下：

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*檢視/共用/AuthorPartial*:

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection*部分：

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

在執行階段，會轉譯 partials 呈現父檢視中，而其本身內共用*_Layout.cshtml*

![部分檢視輸出](partial/_static/output.png)
