---
title: "ASP.NET Core 中的 Scaffold Razor 頁面"
author: rick-anderson
description: "說明 Scaffolding 所產生的 Razor 頁面。"
keywords: "ASP.NET Core, Razor 頁面, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: 7ae83b9bdadf5ebf8846b0c09c585da406708d12
ms.sourcegitcommit: 94b7e0f95b92c98b182a93d2b3dc0287e5f97976
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2017
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>ASP.NET Core 中的 Scaffold Razor 頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會查看在上一個教學課程 ([新增模型](xref:tutorials/razor-pages/model#scaffold-the-movie-model)) 中透過 Scaffolding 所建立的 Razor 頁面。 

[檢視或下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)範例。

## <a name="the-create-delete-details-and-edit-pages"></a>Create、Delete、Details 和 Edit 頁面。

檢查 *Pages/Movies/Index.cshtml.cs* 程式碼後置檔案：[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Razor 頁面衍生自 `PageModel`。 依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。 建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `MovieContext` 新增至頁面中。 所有 Scaffold 頁面都遵循這個模式。

當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。 在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。 在此情況下，`OnGetAsync` 會取得要顯示的電影清單。

請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。 當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。

`@page` Razor 指示詞可讓檔案成為 MVC 動作 &mdash; 這表示它可以處理要求。 `@page` 必須是頁面上的第一個 Razor 指示詞。 `@page` 是轉換成 Razor 特定標記的範例。 如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。

檢查下列 HTML 協助程式中使用的 Lambda 運算式：

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。 Lambda 運算式是進行檢查而不是評估。 這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。 在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。

<a name="md"></a>
### <a name="the-model-directive"></a>@model 指示詞

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。 在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。 此模型用於頁面上的 `@Html.DisplayNameFor` 和 `@Html.DisplayName` [HTML 協助程式](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)。

<!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd">
</a>
### ViewData 和 Layout

請考慮下列程式碼：

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

上述強調顯示的程式碼是 Razor 轉換成 C# 的範例。 `{` 和 `}` 字元中含括 C# 程式碼的區塊。

`PageModel` 基底類別有 `ViewData` 字典屬性，可用來新增至您想要傳遞到檢視的資料。 您可以使用索引鍵/值模式將物件新增至 `ViewData` 字典。 在上述範例中，"Title" 屬性會新增至 `ViewData` 字典。 "Title" 屬性是用於 *Pages/_Layout.cshtml* 檔案。 下列標記會顯示 *Pages/_Layout.cshtml* 檔案的前幾行。

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

行 `@*Markup removed for brevity.*@` 是 Razor 註解。 不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。

執行應用程式並測試專案中的連結 (**Home**、**About**、**Contact**、**Create**、**Edit** 和 **Delete**)。 每一頁都會設定標題，您可以在瀏覽器索引標籤中看到該標題。當您將某個頁面加為書籤時，會使用標題來表示書籤。 *Pages/Index.cshtml* 和 *Pages/Movies/Index.cshtml* 目前有相同的標題，但您可以修改它們使其擁有不同的值。

`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

上述的標記會針對 *Pages* 資料夾下的所有 Razor 檔案．將配置檔設定為 *Pages/_Layout.cshtml*。 如需詳細資訊，請參閱 [Layout](xref:mvc/razor-pages/index#layout)。

### <a name="update-the-layout"></a>更新配置

變更 *Pages/_Layout.cshtml* 檔案中的 `<title>` 項目，以使用較短的字串。

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

在 *Pages/_Layout.cshtml* 檔案中尋找下列錨點項目。

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
以下列標記來取代上述項目。

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。 在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。 `asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。

儲存變更，並按一下 **RpMovie** 連結來測試應用程式。 請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) 檔案。

### <a name="the-create-code-behind-page"></a>Create 程式碼後置頁面

請檢查 *Pages/Movies/Create.cshtml.cs* 程式碼後置檔案：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

`OnGet` 方法會初始化頁面所需的任何狀態。 Create 頁面沒有任何要初始化的狀態。 `Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。

`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。 當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。

當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。 大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。 模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。 我們將在稍後的教學課程中詳細討論用戶端驗證和模型驗證。

如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。

### <a name="the-create-razor-page"></a>[Create Razor] (建立 Razor) 頁面

檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

Visual Studio 會以用於標記協助程式的特殊字型顯示 `<form method="post">` 標記。 `<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。 表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。

![Create.cshtml 頁面的 VS17 檢視](page/_static/th.png)

Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 ` <span asp-validation-for`) 會顯示驗證錯誤。 驗證將於本文稍後詳細討論到。

[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `Title` 屬性 (property) 的標籤標題和 `for` 屬性 (attribute)。

[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) 會使用[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。

下一個教學課程說明 SQL Server LocalDB 和植入資料庫。

>[!div class="step-by-step"]
[上一步：新增模型](xref:tutorials/razor-pages/model)
[下一步：SQL Server LocalDB](xref:tutorials/razor-pages/sql)
