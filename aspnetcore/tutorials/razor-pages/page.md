---
title: ASP.NET Core 中的 Scaffold Razor 頁面
author: rick-anderson
description: 說明 Scaffolding 所產生的 Razor 頁面。
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 00a8458b9bee4d30c5774a980ff5c23fb8872737
ms.sourcegitcommit: 38cac2552029fc19428722bb204ff9e16eb94225
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2019
ms.locfileid: "69573151"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="71d34-103">ASP.NET Core 中的 Scaffold Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="71d34-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="71d34-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71d34-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71d34-105">本教學課程會檢查在[上一個教學課程](xref:tutorials/razor-pages/model)中 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="71d34-106">Create、Delete、Details 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="71d34-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="71d34-107">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="71d34-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="71d34-108">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="71d34-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="71d34-109">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="71d34-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="71d34-110">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `RazorPagesMovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="71d34-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="71d34-111">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="71d34-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="71d34-112">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="71d34-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="71d34-113">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="71d34-114">在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="71d34-114">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="71d34-115">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="71d34-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="71d34-116">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回 `Task` 時，並未使用任何傳回方法。</span><span class="sxs-lookup"><span data-stu-id="71d34-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="71d34-117">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="71d34-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="71d34-118">例如，*Pages/Movies/Create.cshtml.cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="71d34-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="71d34-119">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="71d34-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="71d34-120">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="71d34-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="71d34-121">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="71d34-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="71d34-122">`@page` Razor 指示詞可讓檔案成為 MVC 動作，這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="71d34-122">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="71d34-123">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="71d34-123">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="71d34-124">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="71d34-124">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="71d34-125">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="71d34-125">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="71d34-126">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="71d34-126">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="71d34-127">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="71d34-127">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="71d34-128">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="71d34-128">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="71d34-129">這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。</span><span class="sxs-lookup"><span data-stu-id="71d34-129">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="71d34-130">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="71d34-130">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="71d34-131">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="71d34-131">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="71d34-132">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="71d34-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="71d34-133">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="71d34-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="71d34-134">此模型用於頁面上的 `@Html.DisplayNameFor` 和 `@Html.DisplayFor` [HTML 協助程式](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)。</span><span class="sxs-lookup"><span data-stu-id="71d34-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="71d34-135">版面配置頁</span><span class="sxs-lookup"><span data-stu-id="71d34-135">The layout page</span></span>

<span data-ttu-id="71d34-136">選取功能表連結 (**RazorPagesMovie**、**Home** 及 **Privacy**)。</span><span class="sxs-lookup"><span data-stu-id="71d34-136">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="71d34-137">每個頁面會顯示相同的功能表配置。</span><span class="sxs-lookup"><span data-stu-id="71d34-137">Each page shows the same menu layout.</span></span> <span data-ttu-id="71d34-138">功能表配置會在 *Pages/Shared/_Layout.cshtml* 檔案中實作。</span><span class="sxs-lookup"><span data-stu-id="71d34-138">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="71d34-139">開啟 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71d34-139">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="71d34-140">[版面配置](xref:mvc/views/layout)範本可讓 HTML 容器版面配置：</span><span class="sxs-lookup"><span data-stu-id="71d34-140">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="71d34-141">指定在一個位置。</span><span class="sxs-lookup"><span data-stu-id="71d34-141">Specified in one place.</span></span>
* <span data-ttu-id="71d34-142">套用於網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-142">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="71d34-143">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="71d34-143">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="71d34-144">`RenderBody` 是一個預留位置，可供顯示所有頁面特定檢視 (「包裝」  在版面配置頁面中)。</span><span class="sxs-lookup"><span data-stu-id="71d34-144">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="71d34-145">例如，選取 [隱私權]  連結，就會在 `RenderBody` 方法內轉譯 *Pages/Privacy.cshtml* 檢視。</span><span class="sxs-lookup"><span data-stu-id="71d34-145">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="71d34-146">ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="71d34-146">ViewData and layout</span></span>

<span data-ttu-id="71d34-147">請考慮來自 *Pages/Movies/Index.cshtml* 檔案的下列標記：</span><span class="sxs-lookup"><span data-stu-id="71d34-147">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="71d34-148">上述強調顯示的標記是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="71d34-148">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="71d34-149">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="71d34-149">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="71d34-150">`PageModel` 基底類別包含 `ViewData` 字典屬性，可用來新增資料並傳遞到檢視。</span><span class="sxs-lookup"><span data-stu-id="71d34-150">The `PageModel` base class contains a `ViewData` dictionary property that can be used to add data that and pass it to a View.</span></span> <span data-ttu-id="71d34-151">物件會使用機碼/值模式新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="71d34-151">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="71d34-152">在上述範例中，`"Title"` 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="71d34-152">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="71d34-153">*Pages/Shared/_Layout.cshtml* 檔案中使用 `"Title"` 屬性。</span><span class="sxs-lookup"><span data-stu-id="71d34-153">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="71d34-154">下列標記會顯示 *_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="71d34-154">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="71d34-155">行 `@*Markup removed for brevity.*@` 是 Razor 註解。</span><span class="sxs-lookup"><span data-stu-id="71d34-155">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="71d34-156">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="71d34-156">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="71d34-157">更新配置</span><span class="sxs-lookup"><span data-stu-id="71d34-157">Update the layout</span></span>

<span data-ttu-id="71d34-158">變更 *Pages/Shared/_Layout.cshtml* 檔案中的 `<title>` 項目，以顯示 **Movie** 而不是 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="71d34-158">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="71d34-159">在 *Pages/Shared/_Layout.cshtml* 檔案中尋找下列錨點元素。</span><span class="sxs-lookup"><span data-stu-id="71d34-159">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="71d34-160">以下列標記來取代上述元素：</span><span class="sxs-lookup"><span data-stu-id="71d34-160">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="71d34-161">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="71d34-161">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="71d34-162">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="71d34-162">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="71d34-163">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="71d34-163">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="71d34-164">`asp-area` 屬性值為空白，因此不會在連結中使用該區域。</span><span class="sxs-lookup"><span data-stu-id="71d34-164">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="71d34-165">如需詳細資訊，請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="71d34-165">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="71d34-166">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d34-166">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="71d34-167">如有任何問題，請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="71d34-167">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="71d34-168">測試其他連結 (**Home**、**RpMovie**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="71d34-168">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="71d34-169">每一頁都會設定標題，您可以在瀏覽器索引標籤中看到該標題。當您將某個頁面加為書籤時，會使用標題來表示書籤。</span><span class="sxs-lookup"><span data-stu-id="71d34-169">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="71d34-170">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="71d34-170">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="71d34-171">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="71d34-171">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="71d34-172">請參閱此 [GitHub 問題 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)，以取得加入十進位逗號的指示。</span><span class="sxs-lookup"><span data-stu-id="71d34-172">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="71d34-173">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="71d34-173">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="71d34-174">上述標記會針對 *Pages* 資料夾下的所有 Razor 檔案，將版面配置檔案設定為 *Pages/Shared/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="71d34-174">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="71d34-175">如需詳細資訊，請參閱 [Layout](xref:razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="71d34-175">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="71d34-176">Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="71d34-176">The Create page model</span></span>

<span data-ttu-id="71d34-177">檢查 *Pages/Movies/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="71d34-177">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="71d34-178">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="71d34-178">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="71d34-179">建立頁面沒有任何要初始化的狀態，所以傳回 `Page`。</span><span class="sxs-lookup"><span data-stu-id="71d34-179">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="71d34-180">稍後在此教學課程中，會顯示 `OnGet` 初始化狀態的範例。</span><span class="sxs-lookup"><span data-stu-id="71d34-180">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="71d34-181">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-181">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="71d34-182">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="71d34-182">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="71d34-183">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="71d34-183">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="71d34-184">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="71d34-184">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="71d34-185">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="71d34-185">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="71d34-186">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="71d34-186">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="71d34-187">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="71d34-187">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="71d34-188">稍後的教學課程中將討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="71d34-188">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="71d34-189">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-189">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="71d34-190">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="71d34-190">The Create Razor Page</span></span>

<span data-ttu-id="71d34-191">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="71d34-191">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d34-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d34-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="71d34-193">Visual Studio 會以用於標籤協助程式的特殊粗體字型顯示下列標籤：</span><span class="sxs-lookup"><span data-stu-id="71d34-193">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Create.cshtml 頁面的 VS17 檢視](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d34-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71d34-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="71d34-196">上述標記中會顯示下列標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="71d34-196">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d34-197">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="71d34-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71d34-198">Visual Studio 會以用於標籤協助程式的特殊粗體字型顯示下列標籤：</span><span class="sxs-lookup"><span data-stu-id="71d34-198">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="71d34-199">`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="71d34-199">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="71d34-200">表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="71d34-200">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="71d34-201">Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="71d34-201">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="71d34-202">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 `<span asp-validation-for`) 會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="71d34-202">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="71d34-203">驗證將於本文稍後詳細討論到。</span><span class="sxs-lookup"><span data-stu-id="71d34-203">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="71d34-204">[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `Title` 屬性 (property) 的標籤標題和 `for` 屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="71d34-204">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="71d34-205">[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) 會使用[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="71d34-205">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="71d34-206">如需標籤協助程式 (例如 `<form method="post">`) 的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="71d34-206">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71d34-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="71d34-207">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="71d34-208">[上一步：新增模型](xref:tutorials/razor-pages/model)
> [下一步：資料庫](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="71d34-208">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="71d34-209">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="71d34-209">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="71d34-210">本教學課程會檢查在[上一個教學課程](xref:tutorials/razor-pages/model)中 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-210">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="71d34-211">[檢視或下載](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22)範例。</span><span class="sxs-lookup"><span data-stu-id="71d34-211">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="71d34-212">Create、Delete、Details 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="71d34-212">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="71d34-213">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="71d34-213">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="71d34-214">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="71d34-214">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="71d34-215">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="71d34-215">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="71d34-216">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `RazorPagesMovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="71d34-216">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="71d34-217">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="71d34-217">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="71d34-218">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="71d34-218">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="71d34-219">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-219">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="71d34-220">在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="71d34-220">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="71d34-221">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="71d34-221">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="71d34-222">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回 `Task` 時，並未使用任何傳回方法。</span><span class="sxs-lookup"><span data-stu-id="71d34-222">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="71d34-223">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="71d34-223">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="71d34-224">例如，*Pages/Movies/Create.cshtml.cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="71d34-224">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="71d34-225">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="71d34-225">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="71d34-226">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="71d34-226">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="71d34-227">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="71d34-227">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="71d34-228">`@page` Razor 指示詞可讓檔案成為 MVC 動作，這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="71d34-228">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="71d34-229">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="71d34-229">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="71d34-230">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="71d34-230">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="71d34-231">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="71d34-231">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="71d34-232">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="71d34-232">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="71d34-233">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="71d34-233">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="71d34-234">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="71d34-234">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="71d34-235">這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。</span><span class="sxs-lookup"><span data-stu-id="71d34-235">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="71d34-236">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="71d34-236">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="71d34-237">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="71d34-237">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="71d34-238">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="71d34-238">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="71d34-239">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="71d34-239">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="71d34-240">此模型用於頁面上的 `@Html.DisplayNameFor` 和 `@Html.DisplayFor` [HTML 協助程式](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)。</span><span class="sxs-lookup"><span data-stu-id="71d34-240">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="71d34-241">版面配置頁</span><span class="sxs-lookup"><span data-stu-id="71d34-241">The layout page</span></span>

<span data-ttu-id="71d34-242">選取功能表連結 (**RazorPagesMovie**、**Home** 及 **Privacy**)。</span><span class="sxs-lookup"><span data-stu-id="71d34-242">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="71d34-243">每個頁面會顯示相同的功能表配置。</span><span class="sxs-lookup"><span data-stu-id="71d34-243">Each page shows the same menu layout.</span></span> <span data-ttu-id="71d34-244">功能表配置會在 *Pages/Shared/_Layout.cshtml* 檔案中實作。</span><span class="sxs-lookup"><span data-stu-id="71d34-244">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="71d34-245">開啟 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71d34-245">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="71d34-246">[版面配置](xref:mvc/views/layout)範本可讓您在某個位置指定網站的 HTML 容器配置，然後將它套用到網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-246">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="71d34-247">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="71d34-247">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="71d34-248">`RenderBody` 是一個「包裝」  在版面配置頁中的預留位置，可供顯示您建立的所有頁面特定檢視。</span><span class="sxs-lookup"><span data-stu-id="71d34-248">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="71d34-249">例如，如果您選取 [Privacy]  連結，就會在 `RenderBody` 方法內呈現 **Pages/Privacy.cshtml** 檢視。</span><span class="sxs-lookup"><span data-stu-id="71d34-249">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="71d34-250">ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="71d34-250">ViewData and layout</span></span>

<span data-ttu-id="71d34-251">請考慮來自 *Pages/Movies/Index.cshtml* 檔案的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="71d34-251">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="71d34-252">上述強調顯示的程式碼是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="71d34-252">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="71d34-253">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="71d34-253">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="71d34-254">`PageModel` 基底類別有 `ViewData` 字典屬性，可用來新增至您想要傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="71d34-254">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="71d34-255">您可以使用索引鍵/值模式將物件新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="71d34-255">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="71d34-256">在上述範例中，"Title" 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="71d34-256">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="71d34-257">"Title" 屬性是用於 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="71d34-257">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="71d34-258">下列標記會顯示 *_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="71d34-258">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="71d34-259">行 `@*Markup removed for brevity.*@` 是 Razor 註解，不會出現在您的配置檔案中。</span><span class="sxs-lookup"><span data-stu-id="71d34-259">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="71d34-260">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="71d34-260">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="71d34-261">更新配置</span><span class="sxs-lookup"><span data-stu-id="71d34-261">Update the layout</span></span>

<span data-ttu-id="71d34-262">變更 *Pages/Shared/_Layout.cshtml* 檔案中的 `<title>` 項目，以顯示 **Movie** 而不是 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="71d34-262">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="71d34-263">在 *Pages/Shared/_Layout.cshtml* 檔案中尋找下列錨點元素。</span><span class="sxs-lookup"><span data-stu-id="71d34-263">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="71d34-264">以下列標記來取代上述項目。</span><span class="sxs-lookup"><span data-stu-id="71d34-264">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="71d34-265">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="71d34-265">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="71d34-266">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="71d34-266">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="71d34-267">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="71d34-267">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="71d34-268">`asp-area` 屬性值為空白，因此不會在連結中使用該區域。</span><span class="sxs-lookup"><span data-stu-id="71d34-268">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="71d34-269">如需詳細資訊，請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="71d34-269">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="71d34-270">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="71d34-270">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="71d34-271">如有任何問題，請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="71d34-271">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="71d34-272">測試其他連結 (**Home**、**RpMovie**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="71d34-272">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="71d34-273">每一頁都會設定標題，您可以在瀏覽器索引標籤中看到該標題。當您將某個頁面加為書籤時，會使用標題來表示書籤。</span><span class="sxs-lookup"><span data-stu-id="71d34-273">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="71d34-274">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="71d34-274">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="71d34-275">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="71d34-275">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="71d34-276">這個 [GitHub 問題 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) 有加入小數逗號的指示。</span><span class="sxs-lookup"><span data-stu-id="71d34-276">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="71d34-277">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="71d34-277">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="71d34-278">上述標記會針對 *Pages* 資料夾下的所有 Razor 檔案，將版面配置檔案設定為 *Pages/Shared/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="71d34-278">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="71d34-279">如需詳細資訊，請參閱 [Layout](xref:razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="71d34-279">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="71d34-280">Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="71d34-280">The Create page model</span></span>

<span data-ttu-id="71d34-281">檢查 *Pages/Movies/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="71d34-281">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="71d34-282">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="71d34-282">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="71d34-283">建立頁面沒有任何要初始化的狀態，所以傳回 `Page`。</span><span class="sxs-lookup"><span data-stu-id="71d34-283">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="71d34-284">稍後在本教學課程中您會看到 `OnGet` 方法初始化狀態。</span><span class="sxs-lookup"><span data-stu-id="71d34-284">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="71d34-285">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-285">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="71d34-286">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="71d34-286">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="71d34-287">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="71d34-287">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="71d34-288">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="71d34-288">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="71d34-289">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="71d34-289">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="71d34-290">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="71d34-290">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="71d34-291">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="71d34-291">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="71d34-292">稍後的教學課程中將討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="71d34-292">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="71d34-293">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="71d34-293">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="71d34-294">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="71d34-294">The Create Razor Page</span></span>

<span data-ttu-id="71d34-295">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="71d34-295">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71d34-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71d34-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="71d34-297">Visual Studio 會以特別的粗體字型顯示 `<form method="post">` 標籤，用於標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="71d34-297">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Create.cshtml 頁面的 VS17 檢視](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71d34-299">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71d34-299">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="71d34-300">如需標籤協助程式 (例如 `<form method="post">`) 的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="71d34-300">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71d34-301">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="71d34-301">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="71d34-302">Visual Studio for Mac 會以特別的粗體字型顯示 `<form method="post">` 標籤，用於標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="71d34-302">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="71d34-303">`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="71d34-303">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="71d34-304">表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="71d34-304">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="71d34-305">Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="71d34-305">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="71d34-306">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 `<span asp-validation-for`) 會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="71d34-306">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="71d34-307">驗證將於本文稍後詳細討論到。</span><span class="sxs-lookup"><span data-stu-id="71d34-307">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="71d34-308">[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `Title` 屬性 (property) 的標籤標題和 `for` 屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="71d34-308">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="71d34-309">[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) 會使用[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="71d34-309">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71d34-310">其他資源</span><span class="sxs-lookup"><span data-stu-id="71d34-310">Additional resources</span></span>

* [<span data-ttu-id="71d34-311">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="71d34-311">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="71d34-312">[上一步：新增模型](xref:tutorials/razor-pages/model)
> [下一步：資料庫](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="71d34-312">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
