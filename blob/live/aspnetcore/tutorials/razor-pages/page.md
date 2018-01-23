---
title: "ASP.NET Core 中的 Scaffold Razor 頁面"
author: rick-anderson
description: "說明 Scaffolding 所產生的 Razor 頁面。"
ms.author: riande
manager: wpickett
ms.date: 09/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad2a2b48beb31dddcfd78a8aab79ac58ccda28f3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="7a88f-103">ASP.NET Core 中的 Scaffold Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="7a88f-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="7a88f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a88f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7a88f-105">本教學課程會查看在上一個教學課程 ([新增模型](xref:tutorials/razor-pages/model#scaffold-the-movie-model)) 中透過 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a88f-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial topic [Adding a model](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span></span> 

<span data-ttu-id="7a88f-106">[檢視或下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)範例。</span><span class="sxs-lookup"><span data-stu-id="7a88f-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="7a88f-107">Create、Delete、Details 和 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a88f-107">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="7a88f-108">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="7a88f-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model: [!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]</span></span>

<span data-ttu-id="7a88f-109">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="7a88f-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="7a88f-110">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="7a88f-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="7a88f-111">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `MovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="7a88f-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="7a88f-112">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="7a88f-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="7a88f-113">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="7a88f-114">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a88f-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="7a88f-115">在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="7a88f-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="7a88f-116">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="7a88f-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span> 

<span data-ttu-id="7a88f-117">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回 `Task` 時，並未使用任何傳回方法。</span><span class="sxs-lookup"><span data-stu-id="7a88f-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="7a88f-118">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="7a88f-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="7a88f-119">例如，*Pages/Movies/Create.cshtml.cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="7a88f-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

<!-- TODO - replace with snippet
[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
 -->

```csharp
public async Task<IActionResult> OnPostAsync()
{
    if (!ModelState.IsValid)
    {
        return Page();
    }

    _context.Movie.Add(Movie);
    await _context.SaveChangesAsync();

    return RedirectToPage("./Index");
}
```
<span data-ttu-id="7a88f-120">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="7a88f-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="7a88f-121">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="7a88f-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="7a88f-122">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="7a88f-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="7a88f-123">`@page` Razor 指示詞可讓檔案成為 MVC 動作 &mdash; 這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="7a88f-123">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="7a88f-124">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="7a88f-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="7a88f-125">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="7a88f-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="7a88f-126">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="7a88f-127">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="7a88f-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="7a88f-128">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="7a88f-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="7a88f-129">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="7a88f-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="7a88f-130">這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。</span><span class="sxs-lookup"><span data-stu-id="7a88f-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="7a88f-131">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="7a88f-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="7a88f-132">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="7a88f-132">The @model directive</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="7a88f-133">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="7a88f-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="7a88f-134">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="7a88f-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="7a88f-135">此模型用於頁面上的 `@Html.DisplayNameFor` 和 `@Html.DisplayName` [HTML 協助程式](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](https://docs.microsoft.com/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="7a88f-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd">
</a>
### ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="7a88f-136"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="7a88f-137">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7a88f-137">Consider the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-)]

<span data-ttu-id="7a88f-138">上述強調顯示的程式碼是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="7a88f-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="7a88f-139">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="7a88f-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="7a88f-140">`PageModel` 基底類別有 `ViewData` 字典屬性，可用來新增至您想要傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="7a88f-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="7a88f-141">您可以使用索引鍵/值模式將物件新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="7a88f-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="7a88f-142">在上述範例中，"Title" 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="7a88f-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> <span data-ttu-id="7a88f-143">"Title" 屬性是用於 *Pages/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7a88f-143">The "Title" property is used in the *Pages/_Layout.cshtml* file.</span></span> <span data-ttu-id="7a88f-144">下列標記會顯示 *Pages/_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="7a88f-144">The following markup shows the first few lines of the *Pages/_Layout.cshtml* file.</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-)]

<span data-ttu-id="7a88f-145">行 `@*Markup removed for brevity.*@` 是 Razor 註解。</span><span class="sxs-lookup"><span data-stu-id="7a88f-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="7a88f-146">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="7a88f-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="7a88f-147">執行應用程式並測試專案中的連結 (**Home**、**About**、**Contact**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="7a88f-148">每一頁都會設定標題，您可以在瀏覽器索引標籤中看到該標題。當您將某個頁面加為書籤時，會使用標題來表示書籤。</span><span class="sxs-lookup"><span data-stu-id="7a88f-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="7a88f-149">*Pages/Index.cshtml* 和 *Pages/Movies/Index.cshtml* 目前有相同的標題，但您可以修改它們使其擁有不同的值。</span><span class="sxs-lookup"><span data-stu-id="7a88f-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

<span data-ttu-id="7a88f-150">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="7a88f-150">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="7a88f-151">上述的標記會針對 *Pages* 資料夾下的所有 Razor 檔案．將配置檔設定為 *Pages/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="7a88f-151">The preceding markup sets the layout file to *Pages/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="7a88f-152">如需詳細資訊，請參閱 [Layout](xref:mvc/razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-152">See [Layout](xref:mvc/razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="7a88f-153">更新配置</span><span class="sxs-lookup"><span data-stu-id="7a88f-153">Update the layout</span></span>

<span data-ttu-id="7a88f-154">變更 *Pages/_Layout.cshtml* 檔案中的 `<title>` 項目，以使用較短的字串。</span><span class="sxs-lookup"><span data-stu-id="7a88f-154">Change the `<title>` element in the *Pages/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="7a88f-155">在 *Pages/_Layout.cshtml* 檔案中尋找下列錨點項目。</span><span class="sxs-lookup"><span data-stu-id="7a88f-155">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="7a88f-156">以下列標記來取代上述項目。</span><span class="sxs-lookup"><span data-stu-id="7a88f-156">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="7a88f-157">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-157">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="7a88f-158">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-158">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="7a88f-159">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="7a88f-159">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="7a88f-160">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="7a88f-160">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="7a88f-161">請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="7a88f-161">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-code-behind-page"></a><span data-ttu-id="7a88f-162">Create 程式碼後置頁面</span><span class="sxs-lookup"><span data-stu-id="7a88f-162">The Create code-behind page</span></span>

<span data-ttu-id="7a88f-163">請檢查 *Pages/Movies/Create.cshtml.cs* 程式碼後置檔案：</span><span class="sxs-lookup"><span data-stu-id="7a88f-163">Examine the *Pages/Movies/Create.cshtml.cs* code-behind file:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="7a88f-164">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="7a88f-164">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="7a88f-165">Create 頁面沒有任何要初始化的狀態。</span><span class="sxs-lookup"><span data-stu-id="7a88f-165">The Create page doesn't have any state to initialize.</span></span> <span data-ttu-id="7a88f-166">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a88f-166">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="7a88f-167">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-167">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="7a88f-168">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="7a88f-168">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="7a88f-169">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="7a88f-169">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="7a88f-170">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="7a88f-170">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="7a88f-171">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="7a88f-171">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="7a88f-172">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="7a88f-172">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="7a88f-173">我們將在稍後的教學課程中詳細討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="7a88f-173">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="7a88f-174">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="7a88f-174">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="7a88f-175">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="7a88f-175">The Create Razor Page</span></span>

<span data-ttu-id="7a88f-176">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="7a88f-176">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<span data-ttu-id="7a88f-177">Visual Studio 會以用於標記協助程式的特殊字型顯示 `<form method="post">` 標記。</span><span class="sxs-lookup"><span data-stu-id="7a88f-177">Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers.</span></span> <span data-ttu-id="7a88f-178">`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-178">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="7a88f-179">表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-179">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

![Create.cshtml 頁面的 VS17 檢視](page/_static/th.png)

<span data-ttu-id="7a88f-181">Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7a88f-181">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="7a88f-182">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 ` <span asp-validation-for`) 會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="7a88f-182">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="7a88f-183">驗證將於本文稍後詳細討論到。</span><span class="sxs-lookup"><span data-stu-id="7a88f-183">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="7a88f-184">[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `Title` 屬性 (property) 的標籤標題和 `for` 屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="7a88f-184">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="7a88f-185">[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) 會使用[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="7a88f-185">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="7a88f-186">下一個教學課程說明 SQL Server LocalDB 和植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="7a88f-186">The next tutorial explains SQL Server LocalDB and seeding the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7a88f-187">[上一步：新增模型](xref:tutorials/razor-pages/model)
[下一步：SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="7a88f-187">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: SQL Server LocalDB](xref:tutorials/razor-pages/sql)</span></span>
