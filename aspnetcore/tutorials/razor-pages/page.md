---
title: ASP.NET Core 中的 Scaffold Razor 頁面
author: rick-anderson
description: 說明 Scaffolding 所產生的 Razor 頁面。
ms.author: riande
ms.date: 08/17/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: cec4295a2c08c89db0975808583f41c7d09bfc88
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662446"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="f474e-103">ASP.NET Core 中的 Scaffold Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="f474e-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f474e-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="f474e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f474e-105">本教學課程會檢查在[上一個教學課程](xref:tutorials/razor-pages/model)中 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="f474e-106">Create、Delete、Details 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="f474e-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="f474e-107">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="f474e-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="f474e-108">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="f474e-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="f474e-109">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="f474e-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="f474e-110">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `RazorPagesMovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="f474e-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="f474e-111">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="f474e-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="f474e-112">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="f474e-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="f474e-113">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="f474e-114">呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="f474e-114">`OnGetAsync` or `OnGet` is called to initialize the state of the page.</span></span> <span data-ttu-id="f474e-115">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="f474e-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="f474e-116">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回`Task`時，不會使用 return 語句。</span><span class="sxs-lookup"><span data-stu-id="f474e-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return statement is used.</span></span> <span data-ttu-id="f474e-117">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="f474e-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="f474e-118">例如， *Pages/電影/Create. cshtml .cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="f474e-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="f474e-119">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="f474e-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="f474e-120">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="f474e-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="f474e-121">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="f474e-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

### <a name="the-page-directive"></a><span data-ttu-id="f474e-122">@page 指示詞</span><span class="sxs-lookup"><span data-stu-id="f474e-122">The @page directive</span></span>

<span data-ttu-id="f474e-123">`@page` Razor 指示詞會讓檔案成為 MVC 動作，這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="f474e-123">The `@page` Razor directive makes the file an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="f474e-124">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="f474e-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="f474e-125">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="f474e-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="f474e-126">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="f474e-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="f474e-127">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="f474e-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="f474e-128">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="f474e-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="f474e-129">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="f474e-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="f474e-130">這表示當 `model`、`model.Movie`或 `model.Movie[0]` `null` 或空白時，不會發生存取違規。</span><span class="sxs-lookup"><span data-stu-id="f474e-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` is `null` or empty.</span></span> <span data-ttu-id="f474e-131">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="f474e-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="f474e-132">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="f474e-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="f474e-133">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="f474e-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="f474e-134">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="f474e-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="f474e-135">此模型會在 `@Html.DisplayNameFor` 中使用，並在頁面上 `@Html.DisplayFor` [HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)協助程式。</span><span class="sxs-lookup"><span data-stu-id="f474e-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="f474e-136">版面配置頁</span><span class="sxs-lookup"><span data-stu-id="f474e-136">The layout page</span></span>

<span data-ttu-id="f474e-137">選取功能表連結 (**RazorPagesMovie**、**Home** 及 **Privacy**)。</span><span class="sxs-lookup"><span data-stu-id="f474e-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="f474e-138">每個頁面會顯示相同的功能表配置。</span><span class="sxs-lookup"><span data-stu-id="f474e-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="f474e-139">功能表配置會在 *Pages/Shared/_Layout.cshtml* 檔案中實作。</span><span class="sxs-lookup"><span data-stu-id="f474e-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="f474e-140">開啟 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f474e-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="f474e-141">[版面配置](xref:mvc/views/layout)範本可讓 HTML 容器版面配置：</span><span class="sxs-lookup"><span data-stu-id="f474e-141">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="f474e-142">指定在一個位置。</span><span class="sxs-lookup"><span data-stu-id="f474e-142">Specified in one place.</span></span>
* <span data-ttu-id="f474e-143">套用於網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-143">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="f474e-144">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="f474e-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="f474e-145">`RenderBody` 是一個預留位置，可供顯示所有頁面特定檢視 (「包裝」在版面配置頁面中)。</span><span class="sxs-lookup"><span data-stu-id="f474e-145">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="f474e-146">例如，選取 [隱私權] 連結，就會在 *方法內轉譯*Pages/Privacy.cshtml`RenderBody` 檢視。</span><span class="sxs-lookup"><span data-stu-id="f474e-146">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="f474e-147">ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="f474e-147">ViewData and layout</span></span>

<span data-ttu-id="f474e-148">請考慮來自 *Pages/Movies/Index.cshtml* 檔案的下列標記：</span><span class="sxs-lookup"><span data-stu-id="f474e-148">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="f474e-149">上述強調顯示的標記是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="f474e-149">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="f474e-150">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="f474e-150">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="f474e-151">`PageModel` 基類包含可用來將資料傳遞至 View 的 `ViewData` dictionary 屬性。</span><span class="sxs-lookup"><span data-stu-id="f474e-151">The `PageModel` base class contains a `ViewData` dictionary property that can be used to pass data to a View.</span></span> <span data-ttu-id="f474e-152">物件會使用機碼/值模式新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="f474e-152">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="f474e-153">在上述範例中，`"Title"` 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="f474e-153">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="f474e-154">`"Title"`Pages/Shared/_Layout.cshtml*檔案中使用* 屬性。</span><span class="sxs-lookup"><span data-stu-id="f474e-154">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="f474e-155">下列標記會顯示 *_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="f474e-155">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="f474e-156">行 `@*Markup removed for brevity.*@` 是 Razor 註解。</span><span class="sxs-lookup"><span data-stu-id="f474e-156">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="f474e-157">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="f474e-157">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="f474e-158">更新配置</span><span class="sxs-lookup"><span data-stu-id="f474e-158">Update the layout</span></span>

<span data-ttu-id="f474e-159">變更 `<title>`Pages/Shared/_Layout.cshtml*檔案中的* 項目，以顯示 **Movie** 而不是 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="f474e-159">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="f474e-160">在 *Pages/Shared/_Layout.cshtml* 檔案中尋找下列錨點元素。</span><span class="sxs-lookup"><span data-stu-id="f474e-160">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="f474e-161">以下列標記來取代上述元素：</span><span class="sxs-lookup"><span data-stu-id="f474e-161">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="f474e-162">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="f474e-162">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="f474e-163">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="f474e-163">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="f474e-164">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="f474e-164">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="f474e-165">`asp-area` 屬性值為空白，因此不會在連結中使用該區域。</span><span class="sxs-lookup"><span data-stu-id="f474e-165">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="f474e-166">如需詳細資訊，請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="f474e-166">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="f474e-167">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f474e-167">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="f474e-168">如有任何問題，請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="f474e-168">See the [_Layout.cshtml](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="f474e-169">測試其他連結 (**Home**、**RpMovie**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="f474e-169">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="f474e-170">每個頁面都會設定標題，您可以在 [瀏覽器] 索引標籤中看到它。當您將頁面加入書簽時，會將標題用於書簽。</span><span class="sxs-lookup"><span data-stu-id="f474e-170">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="f474e-171">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="f474e-171">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="f474e-172">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="f474e-172">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="f474e-173">請參閱此 [GitHub 問題 4076](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)，以取得加入十進位逗號的指示。</span><span class="sxs-lookup"><span data-stu-id="f474e-173">See this [GitHub issue 4076](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="f474e-174">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="f474e-174">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="f474e-175">上述標記會針對 *Pages* 資料夾下的所有 Razor 檔案，將版面配置檔案設定為 *Pages/Shared/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f474e-175">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="f474e-176">如需詳細資訊，請參閱 [Layout](xref:razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="f474e-176">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="f474e-177">Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="f474e-177">The Create page model</span></span>

<span data-ttu-id="f474e-178">檢查 *Pages/Movies/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="f474e-178">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="f474e-179">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="f474e-179">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="f474e-180">建立頁面沒有任何要初始化的狀態，所以傳回 `Page`。</span><span class="sxs-lookup"><span data-stu-id="f474e-180">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="f474e-181">稍後在此教學課程中，會顯示 `OnGet` 初始化狀態的範例。</span><span class="sxs-lookup"><span data-stu-id="f474e-181">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="f474e-182">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-182">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="f474e-183">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="f474e-183">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="f474e-184">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="f474e-184">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="f474e-185">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="f474e-185">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="f474e-186">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="f474e-186">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="f474e-187">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="f474e-187">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="f474e-188">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="f474e-188">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="f474e-189">稍後的教學課程中將討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="f474e-189">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="f474e-190">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-190">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="f474e-191">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="f474e-191">The Create Razor Page</span></span>

<span data-ttu-id="f474e-192">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="f474e-192">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studio"></a>[<span data-ttu-id="f474e-193">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f474e-193">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f474e-194">Visual Studio 會以用於標籤協助程式的特殊粗體字型顯示下列標籤：</span><span class="sxs-lookup"><span data-stu-id="f474e-194">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Create.cshtml 頁面的 VS17 檢視](page/_static/th3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="f474e-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f474e-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f474e-197">上述標記中會顯示下列標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="f474e-197">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f474e-198">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f474e-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f474e-199">Visual Studio 會以用於標籤協助程式的特殊粗體字型顯示下列標籤：</span><span class="sxs-lookup"><span data-stu-id="f474e-199">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="f474e-200">`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="f474e-200">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="f474e-201">表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="f474e-201">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="f474e-202">Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f474e-202">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="f474e-203">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 `<span asp-validation-for`) 會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="f474e-203">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="f474e-204">驗證將於本文稍後詳細討論到。</span><span class="sxs-lookup"><span data-stu-id="f474e-204">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="f474e-205">[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `for` 屬性 (property) 的標籤標題和 `Title` 屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="f474e-205">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="f474e-206">[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) 會使用[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="f474e-206">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="f474e-207">如需標籤協助程式 (例如 `<form method="post">`) 的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="f474e-207">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f474e-208">其他資源</span><span class="sxs-lookup"><span data-stu-id="f474e-208">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f474e-209">[上一步：新增模型](xref:tutorials/razor-pages/model)
> [下一個：資料庫](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="f474e-209">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f474e-210">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="f474e-210">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f474e-211">本教學課程會檢查在[上一個教學課程](xref:tutorials/razor-pages/model)中 Scaffolding 所建立的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-211">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="f474e-212">[檢視或下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22)範例。</span><span class="sxs-lookup"><span data-stu-id="f474e-212">[View or download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="f474e-213">Create、Delete、Details 和 Edit 頁面</span><span class="sxs-lookup"><span data-stu-id="f474e-213">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="f474e-214">檢查 *Pages/Movies/Index.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="f474e-214">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="f474e-215">Razor 頁面衍生自 `PageModel`。</span><span class="sxs-lookup"><span data-stu-id="f474e-215">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="f474e-216">依照慣例，`PageModel` 衍生的類別稱為 `<PageName>Model`。</span><span class="sxs-lookup"><span data-stu-id="f474e-216">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="f474e-217">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將 `RazorPagesMovieContext` 新增至頁面中。</span><span class="sxs-lookup"><span data-stu-id="f474e-217">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="f474e-218">所有 Scaffold 頁面都遵循這個模式。</span><span class="sxs-lookup"><span data-stu-id="f474e-218">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="f474e-219">如需使用 Entity Framework 進行非同步程式設計的詳細資訊，請參閱[非同步程式碼](xref:data/ef-rp/intro#asynchronous-code)。</span><span class="sxs-lookup"><span data-stu-id="f474e-219">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programming with Entity Framework.</span></span>

<span data-ttu-id="f474e-220">當針對頁面提出要求時，`OnGetAsync` 方法會將電影清單傳回 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-220">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="f474e-221">在 Razor 頁面上會呼叫 `OnGetAsync` 或 `OnGet` 來初始化頁面的狀態。</span><span class="sxs-lookup"><span data-stu-id="f474e-221">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="f474e-222">在此情況下，`OnGetAsync` 會取得電影清單並加以顯示。</span><span class="sxs-lookup"><span data-stu-id="f474e-222">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="f474e-223">當 `OnGet` 傳回 `void` 或 `OnGetAsync` 傳回 `Task` 時，並未使用任何傳回方法。</span><span class="sxs-lookup"><span data-stu-id="f474e-223">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="f474e-224">當傳回型別是 `IActionResult` 或 `Task<IActionResult>` 時，必須提供傳回陳述式。</span><span class="sxs-lookup"><span data-stu-id="f474e-224">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="f474e-225">例如， *Pages/電影/Create. cshtml .cs* `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="f474e-225">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="f474e-226">請檢查 *Pages/Movies/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="f474e-226">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="f474e-227">Razor 可以從 HTML 轉換成 C# 或 Razor 特定標記。</span><span class="sxs-lookup"><span data-stu-id="f474e-227">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="f474e-228">當 `@` 符號後面接著 [Razor 保留關鍵字](xref:mvc/views/razor#razor-reserved-keywords) 時，它會轉換成 Razor 特定標記，否則就會轉換成 C#。</span><span class="sxs-lookup"><span data-stu-id="f474e-228">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="f474e-229">`@page` Razor 指示詞可讓檔案成為 MVC 動作，這表示它可以處理要求。</span><span class="sxs-lookup"><span data-stu-id="f474e-229">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="f474e-230">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="f474e-230">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="f474e-231">`@page` 是轉換成 Razor 特定標記的範例。</span><span class="sxs-lookup"><span data-stu-id="f474e-231">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="f474e-232">如需詳細資訊，請參閱 [Razor 語法](xref:mvc/views/razor#razor-syntax)。</span><span class="sxs-lookup"><span data-stu-id="f474e-232">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="f474e-233">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="f474e-233">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title)
```

<span data-ttu-id="f474e-234">`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="f474e-234">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="f474e-235">Lambda 運算式是進行檢查而不是評估。</span><span class="sxs-lookup"><span data-stu-id="f474e-235">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="f474e-236">這表示當 `model`、`model.Movie` 或 `model.Movie[0]` 是 `null` 或空白時，不會有任何存取違規。</span><span class="sxs-lookup"><span data-stu-id="f474e-236">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="f474e-237">在評估 Lambda 運算式時 (例如，使用 `@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="f474e-237">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="f474e-238">@model 指示詞</span><span class="sxs-lookup"><span data-stu-id="f474e-238">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="f474e-239">`@model` 指示詞會指定傳遞至 Razor 頁面的模型類型。</span><span class="sxs-lookup"><span data-stu-id="f474e-239">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="f474e-240">在上述範例中，`@model` 行可讓 `PageModel` 衍生的類別供 Razor 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="f474e-240">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="f474e-241">此模型會在 `@Html.DisplayNameFor` 中使用，並在頁面上 `@Html.DisplayFor` [HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)協助程式。</span><span class="sxs-lookup"><span data-stu-id="f474e-241">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="f474e-242">版面配置頁</span><span class="sxs-lookup"><span data-stu-id="f474e-242">The layout page</span></span>

<span data-ttu-id="f474e-243">選取功能表連結 (**RazorPagesMovie**、**Home** 及 **Privacy**)。</span><span class="sxs-lookup"><span data-stu-id="f474e-243">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="f474e-244">每個頁面會顯示相同的功能表配置。</span><span class="sxs-lookup"><span data-stu-id="f474e-244">Each page shows the same menu layout.</span></span> <span data-ttu-id="f474e-245">功能表配置會在 *Pages/Shared/_Layout.cshtml* 檔案中實作。</span><span class="sxs-lookup"><span data-stu-id="f474e-245">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="f474e-246">開啟 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f474e-246">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="f474e-247">[版面配置](xref:mvc/views/layout)範本可讓您在某個位置指定網站的 HTML 容器配置，然後將它套用到網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-247">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="f474e-248">找到 `@RenderBody()` 這行。</span><span class="sxs-lookup"><span data-stu-id="f474e-248">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="f474e-249">`RenderBody` 是一個「包裝」在版面配置頁中的預留位置，可供顯示您建立的所有頁面特定檢視。</span><span class="sxs-lookup"><span data-stu-id="f474e-249">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="f474e-250">例如，如果您選取 [Privacy] 連結，就會在 **方法內呈現**Pages/Privacy.cshtml`RenderBody` 檢視。</span><span class="sxs-lookup"><span data-stu-id="f474e-250">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="f474e-251">ViewData 和 Layout</span><span class="sxs-lookup"><span data-stu-id="f474e-251">ViewData and layout</span></span>

<span data-ttu-id="f474e-252">請考慮來自 *Pages/Movies/Index.cshtml* 檔案的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f474e-252">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="f474e-253">上述強調顯示的程式碼是 Razor 轉換成 C# 的範例。</span><span class="sxs-lookup"><span data-stu-id="f474e-253">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="f474e-254">`{` 和 `}` 字元中含括 C# 程式碼的區塊。</span><span class="sxs-lookup"><span data-stu-id="f474e-254">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="f474e-255">`PageModel` 基底類別有 `ViewData` 字典屬性，可用來新增至您想要傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="f474e-255">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="f474e-256">您可以使用索引鍵/值模式將物件新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="f474e-256">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="f474e-257">在上述範例中，"Title" 屬性會新增至 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="f474e-257">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="f474e-258">"Title" 屬性是用於 *Pages/Shared/_Layout.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f474e-258">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="f474e-259">下列標記會顯示 *_Layout.cshtml* 檔案的前幾行。</span><span class="sxs-lookup"><span data-stu-id="f474e-259">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="f474e-260">行 `@*Markup removed for brevity.*@` 是 Razor 註解，不會出現在您的配置檔案中。</span><span class="sxs-lookup"><span data-stu-id="f474e-260">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="f474e-261">不同於 HTML 註解 (`<!-- -->`)，Razor 註解不會傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="f474e-261">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="f474e-262">更新配置</span><span class="sxs-lookup"><span data-stu-id="f474e-262">Update the layout</span></span>

<span data-ttu-id="f474e-263">變更 `<title>`Pages/Shared/_Layout.cshtml*檔案中的* 項目，以顯示 **Movie** 而不是 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="f474e-263">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="f474e-264">在 *Pages/Shared/_Layout.cshtml* 檔案中尋找下列錨點元素。</span><span class="sxs-lookup"><span data-stu-id="f474e-264">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="f474e-265">以下列標記來取代上述項目。</span><span class="sxs-lookup"><span data-stu-id="f474e-265">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="f474e-266">上述的錨點項目是[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="f474e-266">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="f474e-267">在此情況下，它是[錨點標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="f474e-267">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="f474e-268">`asp-page="/Movies/Index"` 標記協助程式的屬性和值會建立 `/Movies/Index` Razor 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="f474e-268">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="f474e-269">`asp-area` 屬性值為空白，因此不會在連結中使用該區域。</span><span class="sxs-lookup"><span data-stu-id="f474e-269">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="f474e-270">如需詳細資訊，請參閱[區域](xref:mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="f474e-270">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="f474e-271">儲存變更，並按一下 **RpMovie** 連結來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f474e-271">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="f474e-272">如有任何問題，請參閱 GitHub 中的 [_Layout.cshtml](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) 檔案。</span><span class="sxs-lookup"><span data-stu-id="f474e-272">See the [_Layout.cshtml](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="f474e-273">測試其他連結 (**Home**、**RpMovie**、**Create**、**Edit** 和 **Delete**)。</span><span class="sxs-lookup"><span data-stu-id="f474e-273">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="f474e-274">每個頁面都會設定標題，您可以在 [瀏覽器] 索引標籤中看到它。當您將頁面加入書簽時，會將標題用於書簽。</span><span class="sxs-lookup"><span data-stu-id="f474e-274">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="f474e-275">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="f474e-275">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="f474e-276">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期欄位支援 [jQuery 驗證](https://jqueryvalidation.org/)，您必須採取將應用程式全球化的步驟。</span><span class="sxs-lookup"><span data-stu-id="f474e-276">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="f474e-277">這個 [GitHub 問題 4076](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) 有加入小數逗號的指示。</span><span class="sxs-lookup"><span data-stu-id="f474e-277">This [GitHub issue 4076](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="f474e-278">`Layout` 屬性是在 *Pages/_ViewStart.cshtml* 檔案中設定：</span><span class="sxs-lookup"><span data-stu-id="f474e-278">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="f474e-279">上述標記會針對 *Pages* 資料夾下的所有 Razor 檔案，將版面配置檔案設定為 *Pages/Shared/_Layout.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f474e-279">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="f474e-280">如需詳細資訊，請參閱 [Layout](xref:razor-pages/index#layout)。</span><span class="sxs-lookup"><span data-stu-id="f474e-280">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="f474e-281">Create 頁面模型</span><span class="sxs-lookup"><span data-stu-id="f474e-281">The Create page model</span></span>

<span data-ttu-id="f474e-282">檢查 *Pages/Movies/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="f474e-282">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="f474e-283">`OnGet` 方法會初始化頁面所需的任何狀態。</span><span class="sxs-lookup"><span data-stu-id="f474e-283">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="f474e-284">建立頁面沒有任何要初始化的狀態，所以傳回 `Page`。</span><span class="sxs-lookup"><span data-stu-id="f474e-284">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="f474e-285">稍後在本教學課程中您會看到 `OnGet` 方法初始化狀態。</span><span class="sxs-lookup"><span data-stu-id="f474e-285">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="f474e-286">`Page` 方法會建立 `PageResult` 物件，用以呈現 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-286">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="f474e-287">`Movie` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 來加入[模型繫結](xref:mvc/models/model-binding)。</span><span class="sxs-lookup"><span data-stu-id="f474e-287">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="f474e-288">當 Create 表單發佈表單值時，ASP.NET Core 執行階段會將發佈的值繫結至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="f474e-288">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="f474e-289">當頁面發佈表單資料時，即會執行 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="f474e-289">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="f474e-290">如果沒有任何模型錯誤，將會重新顯示表單，以及任何發佈的表單資料。</span><span class="sxs-lookup"><span data-stu-id="f474e-290">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="f474e-291">大部分的模型錯誤可以在發佈表單之前，於用戶端上攔截到。</span><span class="sxs-lookup"><span data-stu-id="f474e-291">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="f474e-292">模型錯誤的範例為針對日期欄位發佈無法轉換為日期的值。</span><span class="sxs-lookup"><span data-stu-id="f474e-292">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="f474e-293">稍後的教學課程中將討論用戶端驗證和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="f474e-293">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="f474e-294">如果沒有任何模型錯誤，就會儲存資料，而瀏覽器則會重新導向至 Index 頁面。</span><span class="sxs-lookup"><span data-stu-id="f474e-294">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="f474e-295">[Create Razor] (建立 Razor) 頁面</span><span class="sxs-lookup"><span data-stu-id="f474e-295">The Create Razor Page</span></span>

<span data-ttu-id="f474e-296">檢查 *Pages/Movies/Create.cshtml* Razor 頁面檔案：</span><span class="sxs-lookup"><span data-stu-id="f474e-296">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studio"></a>[<span data-ttu-id="f474e-297">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f474e-297">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f474e-298">Visual Studio 會以特別的粗體字型顯示 `<form method="post">` 標籤，用於標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="f474e-298">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Create.cshtml 頁面的 VS17 檢視](page/_static/th.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="f474e-300">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f474e-300">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f474e-301">如需標籤協助程式 (例如 `<form method="post">`) 的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="f474e-301">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f474e-302">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f474e-302">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f474e-303">Visual Studio for Mac 會以特別的粗體字型顯示 `<form method="post">` 標籤，用於標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="f474e-303">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="f474e-304">`<form method="post">` 項目是[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="f474e-304">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="f474e-305">表單標記協助程式會自動包含 [antiforgery 語彙基元](xref:security/anti-request-forgery)。</span><span class="sxs-lookup"><span data-stu-id="f474e-305">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="f474e-306">Scaffolding 引擎會在模型中建立每個欄位的 Razor 標記 (除了識別碼)，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f474e-306">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="f474e-307">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` 和 `<span asp-validation-for`) 會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="f474e-307">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="f474e-308">驗證將於本文稍後詳細討論到。</span><span class="sxs-lookup"><span data-stu-id="f474e-308">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="f474e-309">[標籤標記協助程式](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) 會產生 `for` 屬性 (property) 的標籤標題和 `Title` 屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="f474e-309">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="f474e-310">[輸入標記協助程式](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) 會使用[DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="f474e-310">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f474e-311">其他資源</span><span class="sxs-lookup"><span data-stu-id="f474e-311">Additional resources</span></span>

* [<span data-ttu-id="f474e-312">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="f474e-312">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="f474e-313">[上一步：新增模型](xref:tutorials/razor-pages/model)
> [下一個：資料庫](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="f474e-313">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end
