---
title: ASP.NET Core 中的 Scaffold Razor 頁面
author: rick-anderson
description: 說明 Scaffolding 所產生的 Razor 頁面。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: acfc446732803c67714943fe3e5b7a31055ebcd7
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862001"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="022ad-103">ASP.NET Core 中的 Scaffold Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="022ad-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="022ad-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="022ad-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="022ad-105">本教學課程會檢查在先前教學課程中 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="022ad-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="022ad-106">[檢視或下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22)範例。</span><span class="sxs-lookup"><span data-stu-id="022ad-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="022ad-107">Create、Delete、Details 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="022ad-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="022ad-108">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="022ad-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="022ad-109">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="022ad-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="022ad-110">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="022ad-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="022ad-111">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `RazorPagesMovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="022ad-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="022ad-112">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="022ad-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="022ad-113">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="022ad-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="022ad-114">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="022ad-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="022ad-115">在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="022ad-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="022ad-116">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="022ad-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="022ad-117">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回 `Task` 時，並未使用任何傳回方法。</span><span class="sxs-lookup"><span data-stu-id="022ad-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="022ad-118">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="022ad-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="022ad-119">例如，*Pages/Movies/Create.cshtml.cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="022ad-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="022ad-120">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="022ad-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="022ad-121">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="022ad-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="022ad-122">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="022ad-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="022ad-123">`@page` Razor 指示詞可讓檔案成為 MVC 動作，這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="022ad-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="022ad-124">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="022ad-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="022ad-125">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="022ad-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="022ad-126">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="022ad-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="022ad-127">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="022ad-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="022ad-128">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="022ad-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="022ad-129">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="022ad-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="022ad-130">這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。</span><span class="sxs-lookup"><span data-stu-id="022ad-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="022ad-131">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="022ad-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="022ad-132">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="022ad-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="022ad-133">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="022ad-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="022ad-134">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="022ad-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="022ad-135">此模型用於頁面上的 `@Html.DisplayNameFor` 和 `@Html.DisplayFor` [HTML 協助程式](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)。</span><span class="sxs-lookup"><span data-stu-id="022ad-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<a name="vd"></a>
### <a name="viewdata-and-layout"></a><span data-ttu-id="022ad-136">ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="022ad-136">ViewData and layout</span></span>

<span data-ttu-id="022ad-137">請考慮來自 *Pages/Movies/Index.cshtml* 檔案的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="022ad-137">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="022ad-138">上述強調顯示的程式碼是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="022ad-138">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="022ad-139">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="022ad-139">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="022ad-140">`PageModel` 基底類別有 `ViewData` 字典屬性，可用來新增至您想要傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="022ad-140">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="022ad-141">您可以使用索引鍵/值模式將物件新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="022ad-141">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="022ad-142">在上述範例中，"Title" 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="022ad-142">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

<span data-ttu-id="022ad-143">"Title" 屬性是用於 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="022ad-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="022ad-144">下列標記會顯示 *_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="022ad-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="022ad-145">行 `@*Markup removed for brevity.*@` 是 Razor 註解，不會出現在您的配置檔案中。</span><span class="sxs-lookup"><span data-stu-id="022ad-145">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="022ad-146">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="022ad-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="022ad-147">更新配置</span><span class="sxs-lookup"><span data-stu-id="022ad-147">Update the layout</span></span>

<span data-ttu-id="022ad-148">變更 *Pages/Shared/_Layout.cshtml* 檔案中的 `<title>` 元素，以顯示 **Movie** 而不是 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="022ad-148">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


<span data-ttu-id="022ad-149">在 *Pages/Shared/_Layout.cshtml* 檔案中尋找下列錨點元素。</span><span class="sxs-lookup"><span data-stu-id="022ad-149">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="022ad-150">以下列標記來取代上述項目。</span><span class="sxs-lookup"><span data-stu-id="022ad-150">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="022ad-151">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="022ad-151">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="022ad-152">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="022ad-152">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="022ad-153">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="022ad-153">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="022ad-154">`asp-area` 屬性值為空白，因此不會在連結中使用該區域。</span><span class="sxs-lookup"><span data-stu-id="022ad-154">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="022ad-155">如需詳細資訊，請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="022ad-155">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="022ad-156">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="022ad-156">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="022ad-157">如有任何問題，請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="022ad-157">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="022ad-158">測試其他連結 (**Home**、**RpMovie**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="022ad-158">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="022ad-159">每一頁都會設定標題，您可以在瀏覽器索引標籤中看到該標題。當您將某個頁面加為書籤時，會使用標題來表示書籤。</span><span class="sxs-lookup"><span data-stu-id="022ad-159">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="022ad-160">*Pages/Index.cshtml* 和 *Pages/Movies/Index.cshtml* 目前有相同的標題，但您可以修改它們使其擁有不同的值。</span><span class="sxs-lookup"><span data-stu-id="022ad-160">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="022ad-161">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="022ad-161">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="022ad-162">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="022ad-162">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="022ad-163">這個 [GitHub 問題 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) 有加入小數逗號的指示。</span><span class="sxs-lookup"><span data-stu-id="022ad-163">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="022ad-164">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="022ad-164">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="022ad-165">上述標記會針對 *Pages* 資料夾下的所有 Razor 檔案，將版面配置檔案設定為 *Pages/Shared/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="022ad-165">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="022ad-166">如需詳細資訊，請參閱 [Layout](xref:razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="022ad-166">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="022ad-167">Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="022ad-167">The Create page model</span></span>

<span data-ttu-id="022ad-168">檢查 *Pages/Movies/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="022ad-168">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="022ad-169">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="022ad-169">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="022ad-170">建立頁面沒有任何要初始化的狀態，所以傳回 `Page`。</span><span class="sxs-lookup"><span data-stu-id="022ad-170">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="022ad-171">稍後在本教學課程中您會看到 `OnGet` 方法初始化狀態。</span><span class="sxs-lookup"><span data-stu-id="022ad-171">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="022ad-172">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="022ad-172">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="022ad-173">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="022ad-173">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="022ad-174">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="022ad-174">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="022ad-175">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="022ad-175">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="022ad-176">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="022ad-176">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="022ad-177">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="022ad-177">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="022ad-178">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="022ad-178">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="022ad-179">稍後的教學課程中將討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="022ad-179">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="022ad-180">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="022ad-180">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="022ad-181">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="022ad-181">The Create Razor Page</span></span>

<span data-ttu-id="022ad-182">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="022ad-182">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="022ad-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="022ad-183">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="022ad-184">Visual Studio 會以特別的粗體字型顯示 `<form method="post">` 標籤，用於標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="022ad-184">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

<span data-ttu-id="022ad-185">![Create.cshtml 頁面的 VS17 檢視](page/_static/th.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="022ad-185">![VS17 view of Create.cshtml page](page/_static/th.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="022ad-186">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="022ad-186">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="022ad-187">如需標籤協助程式 (例如 `<form method="post">`) 的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="022ad-187">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="022ad-188">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="022ad-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="022ad-189">Visual Studio for Mac 會以特別的粗體字型顯示 `<form method="post">` 標籤，用於標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="022ad-189">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="022ad-190">`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="022ad-190">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="022ad-191">表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="022ad-191">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="022ad-192">Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="022ad-192">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="022ad-193">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 ` <span asp-validation-for`) 會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="022ad-193">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="022ad-194">驗證將於本文稍後詳細討論到。</span><span class="sxs-lookup"><span data-stu-id="022ad-194">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="022ad-195">[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `Title` 屬性 (property) 的標籤標題和 `for` 屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="022ad-195">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="022ad-196">[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) 會使用[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="022ad-196">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="022ad-197">[上一步：新增模型](xref:tutorials/razor-pages/model)
> [下一步：資料庫](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="022ad-197">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Data Base](xref:tutorials/razor-pages/sql)</span></span>
