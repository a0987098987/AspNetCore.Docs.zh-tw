# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="588f4-101">ASP.NET Core 中的 Scaffold Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="588f4-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="588f4-102">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="588f4-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="588f4-103">本教學課程會檢查在先前教學課程中 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="588f4-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="588f4-104">[檢視或下載](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)範例。</span><span class="sxs-lookup"><span data-stu-id="588f4-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="588f4-105">Create、Delete、Details 和 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="588f4-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="588f4-106">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="588f4-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="588f4-107">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="588f4-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="588f4-108">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="588f4-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="588f4-109">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `MovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="588f4-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="588f4-110">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="588f4-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="588f4-111">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="588f4-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="588f4-112">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="588f4-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="588f4-113">在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="588f4-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="588f4-114">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="588f4-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="588f4-115">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回 `Task` 時，並未使用任何傳回方法。</span><span class="sxs-lookup"><span data-stu-id="588f4-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="588f4-116">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="588f4-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="588f4-117">例如，*Pages/Movies/Create.cshtml.cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="588f4-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<span data-ttu-id="588f4-118">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="588f4-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="588f4-119">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="588f4-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="588f4-120">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="588f4-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="588f4-121">`@page` Razor 指示詞可讓檔案成為 MVC 動作 &mdash; 這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="588f4-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="588f4-122">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="588f4-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="588f4-123">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="588f4-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="588f4-124">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="588f4-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="588f4-125">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="588f4-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="588f4-126">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="588f4-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="588f4-127">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="588f4-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="588f4-128">這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。</span><span class="sxs-lookup"><span data-stu-id="588f4-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="588f4-129">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="588f4-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="588f4-130">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="588f4-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="588f4-131">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="588f4-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="588f4-132">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="588f4-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="588f4-133">此模型用於頁面上的 `@Html.DisplayNameFor` 和 `@Html.DisplayName` [HTML 協助程式](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)。</span><span class="sxs-lookup"><span data-stu-id="588f4-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayName` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="588f4-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="588f4-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="588f4-135">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="588f4-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="588f4-136">上述強調顯示的程式碼是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="588f4-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="588f4-137">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="588f4-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="588f4-138">`PageModel` 基底類別有 `ViewData` 字典屬性，可用來新增至您想要傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="588f4-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="588f4-139">您可以使用索引鍵/值模式將物件新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="588f4-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="588f4-140">在上述範例中，"Title" 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="588f4-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="588f4-141">"Title" 屬性是用於 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="588f4-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="588f4-142">下列標記顯示 *Pages/Shared/_Layout.cshtml* 檔案中的前幾行。</span><span class="sxs-lookup"><span data-stu-id="588f4-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="588f4-143">"Title" 屬性是用於 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="588f4-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="588f4-144">下列標記會顯示 *_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="588f4-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="588f4-145">行 `@*Markup removed for brevity.*@` 是 Razor 註解。</span><span class="sxs-lookup"><span data-stu-id="588f4-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="588f4-146">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="588f4-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="588f4-147">執行應用程式並測試專案中的連結 (**Home**、**About**、**Contact**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="588f4-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="588f4-148">每一頁都會設定標題，您可以在瀏覽器索引標籤中看到該標題。當您將某個頁面加為書籤時，會使用標題來表示書籤。</span><span class="sxs-lookup"><span data-stu-id="588f4-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="588f4-149">*Pages/Index.cshtml* 和 *Pages/Movies/Index.cshtml* 目前有相同的標題，但您可以修改它們使其擁有不同的值。</span><span class="sxs-lookup"><span data-stu-id="588f4-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="588f4-150">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="588f4-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="588f4-151">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="588f4-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="588f4-152">這個 [GitHub 問題 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) 有加入小數逗號的指示。</span><span class="sxs-lookup"><span data-stu-id="588f4-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="588f4-153">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="588f4-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="588f4-154">上述標記會針對 *Pages* 資料夾下的所有 Razor 檔案，將版面配置檔案設定為 *Pages/Shared/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="588f4-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="588f4-155">如需詳細資訊，請參閱[版面配置](xref:razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="588f4-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="588f4-156">更新版面配置</span><span class="sxs-lookup"><span data-stu-id="588f4-156">Update the layout</span></span>

<span data-ttu-id="588f4-157">變更 *Pages/Shared/_Layout.cshtml* 檔案中的 `<title>` 元素，以使用較短的字串。</span><span class="sxs-lookup"><span data-stu-id="588f4-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="588f4-158">在 *Pages/_Layout.cshtml* 檔案中尋找下列錨點項目。</span><span class="sxs-lookup"><span data-stu-id="588f4-158">Find the following anchor element in the *Pages/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="588f4-159">以下列標記來取代上述項目。</span><span class="sxs-lookup"><span data-stu-id="588f4-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="588f4-160">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="588f4-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="588f4-161">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="588f4-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="588f4-162">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="588f4-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="588f4-163">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="588f4-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="588f4-164">請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="588f4-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="588f4-165">Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="588f4-165">The Create page model</span></span>

<span data-ttu-id="588f4-166">檢查 *Pages/Movies/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="588f4-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]
::: moniker-end


<span data-ttu-id="588f4-167">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="588f4-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="588f4-168">建立頁面沒有任何要初始化的狀態，所以傳回 `Page`。</span><span class="sxs-lookup"><span data-stu-id="588f4-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="588f4-169">稍後在本教學課程中您會看到 `OnGet` 方法初始化狀態。</span><span class="sxs-lookup"><span data-stu-id="588f4-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="588f4-170">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="588f4-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="588f4-171">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="588f4-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="588f4-172">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="588f4-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="588f4-173">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="588f4-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="588f4-174">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="588f4-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="588f4-175">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="588f4-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="588f4-176">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="588f4-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="588f4-177">我們將在稍後的教學課程中詳細討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="588f4-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="588f4-178">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="588f4-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="588f4-179">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="588f4-179">The Create Razor Page</span></span>

<span data-ttu-id="588f4-180">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="588f4-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
