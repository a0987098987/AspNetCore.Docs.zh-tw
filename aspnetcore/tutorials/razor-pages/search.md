---
title: 將搜尋新增至 ASP.NET Core Razor 頁面
author: rick-anderson
description: 示範如何將搜尋新增至 ASP.NET Core Razor 頁面
ms.author: riande
ms.date: 7/23/2019
uid: tutorials/razor-pages/search
ms.openlocfilehash: d1aa3f914ebcab4d095b6fca1dac3cf44855d516
ms.sourcegitcommit: 16502797ea749e2690feaa5e652a65b89c007c89
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/25/2019
ms.locfileid: "68483292"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="d712d-103">將搜尋新增至 ASP.NET Core Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d712d-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="d712d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d712d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="d712d-105">在下列各節中，會新增依「內容類型」  或「名稱」  搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="d712d-105">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="d712d-106">將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="d712d-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="d712d-107">`SearchString`：包含使用者在搜尋文字方塊中輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="d712d-107">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="d712d-108">`SearchString` 是以 [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) 屬性裝飾。</span><span class="sxs-lookup"><span data-stu-id="d712d-108">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="d712d-109">`[BindProperty]` 使用與屬性相同的名稱來繫結表單值和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d712d-109">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="d712d-110">需要 `(SupportsGet = true)` 才能在 GET 要求上進行繫結。</span><span class="sxs-lookup"><span data-stu-id="d712d-110">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="d712d-111">`Genres`：包含內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="d712d-111">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="d712d-112">`Genres` 可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="d712d-112">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="d712d-113">`SelectList` 需要 `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="d712d-113">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="d712d-114">`MovieGenre`：包含使用者所選取的特定內容類型 (例如「西部片」)。</span><span class="sxs-lookup"><span data-stu-id="d712d-114">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="d712d-115">稍後在本教學課程中將會使用 `Genres` 和 `MovieGenre`。</span><span class="sxs-lookup"><span data-stu-id="d712d-115">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="d712d-116">使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d712d-116">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="d712d-117">`OnGetAsync` 方法的第一行會建立 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="d712d-117">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="d712d-118">這時候，系統只會「定義」查詢  ，而尚**未**對資料庫執行查詢。</span><span class="sxs-lookup"><span data-stu-id="d712d-118">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="d712d-119">如果 `SearchString` 屬性不是 Null 或空白，則會修改電影查詢來篩選搜尋字串：</span><span class="sxs-lookup"><span data-stu-id="d712d-119">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="d712d-120">`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="d712d-120">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d712d-121">在以方法為基礎的 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="d712d-121">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="d712d-122">定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會加以執行。</span><span class="sxs-lookup"><span data-stu-id="d712d-122">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="d712d-123">而會延後執行查詢。</span><span class="sxs-lookup"><span data-stu-id="d712d-123">Rather, query execution is deferred.</span></span> <span data-ttu-id="d712d-124">這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="d712d-124">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="d712d-125">如需詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。</span><span class="sxs-lookup"><span data-stu-id="d712d-125">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="d712d-126">**注意：** [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="d712d-126">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="d712d-127">查詢是否區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="d712d-127">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="d712d-128">在 SQL Server 上，`Contains` 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d712d-128">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="d712d-129">而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d712d-129">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="d712d-130">巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`https://localhost:5001/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="d712d-130">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="d712d-131">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="d712d-131">The filtered movies are displayed.</span></span>

![索引檢視](search/_static/ghost.png)

<span data-ttu-id="d712d-133">如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="d712d-133">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="d712d-134">上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。</span><span class="sxs-lookup"><span data-stu-id="d712d-134">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="d712d-135">在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。</span><span class="sxs-lookup"><span data-stu-id="d712d-135">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](search/_static/g2.png)

<span data-ttu-id="d712d-137">ASP.NET Core 執行階段使用[模型繫結](xref:mvc/models/model-binding)來設定查詢字串 (`?searchString=Ghost`) 中的 `SearchString` 屬性值或路由傳送資料 (`https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="d712d-137">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="d712d-138">模型繫結是不區分大小寫的。</span><span class="sxs-lookup"><span data-stu-id="d712d-138">Model binding is not case sensitive.</span></span>

<span data-ttu-id="d712d-139">但是，您不能期望使用者會在搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="d712d-139">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="d712d-140">在此步驟中，會新增用來篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="d712d-140">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="d712d-141">如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。</span><span class="sxs-lookup"><span data-stu-id="d712d-141">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="d712d-142">開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="d712d-142">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="d712d-143">HTML `<form>` 標籤會使用下列[標籤協助程式](xref:mvc/views/tag-helpers/intro)：</span><span class="sxs-lookup"><span data-stu-id="d712d-143">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="d712d-144">[表單標籤協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="d712d-144">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="d712d-145">提交表單時，系統會透過查詢字串將篩選字串傳送至 *Pages/Movies/Index* 頁面。</span><span class="sxs-lookup"><span data-stu-id="d712d-145">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="d712d-146">輸入標記協助程式</span><span class="sxs-lookup"><span data-stu-id="d712d-146">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="d712d-147">儲存變更並測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d712d-147">Save the changes and test the filter.</span></span>

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="d712d-149">依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="d712d-149">Search by genre</span></span>

<span data-ttu-id="d712d-150">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d712d-150">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="d712d-151">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="d712d-151">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="d712d-152">內容類型的 `SelectList` 是由投影不同的內容類型來建立。</span><span class="sxs-lookup"><span data-stu-id="d712d-152">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="d712d-153">將依內容類型搜尋新增至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d712d-153">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="d712d-154">更新 *Index.cshtml*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d712d-154">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="d712d-155">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="d712d-155">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d712d-156">其他資源</span><span class="sxs-lookup"><span data-stu-id="d712d-156">Additional resources</span></span>

* [<span data-ttu-id="d712d-157">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="d712d-157">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="d712d-158">[上一步：更新頁面](xref:tutorials/razor-pages/da1)
> [下一步：新增欄位](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="d712d-158">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="d712d-159">在下列各節中，會新增依「內容類型」  或「名稱」  搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="d712d-159">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="d712d-160">將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="d712d-160">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="d712d-161">`SearchString`：包含使用者在搜尋文字方塊中輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="d712d-161">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="d712d-162">`SearchString` 是以 [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) 屬性裝飾。</span><span class="sxs-lookup"><span data-stu-id="d712d-162">`SearchString` is decorated with the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="d712d-163">`[BindProperty]` 使用與屬性相同的名稱來繫結表單值和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="d712d-163">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="d712d-164">需要 `(SupportsGet = true)` 才能在 GET 要求上進行繫結。</span><span class="sxs-lookup"><span data-stu-id="d712d-164">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="d712d-165">`Genres`：包含內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="d712d-165">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="d712d-166">`Genres` 可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="d712d-166">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="d712d-167">`SelectList` 需要 `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="d712d-167">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="d712d-168">`MovieGenre`：包含使用者所選取的特定內容類型 (例如「西部片」)。</span><span class="sxs-lookup"><span data-stu-id="d712d-168">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="d712d-169">稍後在本教學課程中將會使用 `Genres` 和 `MovieGenre`。</span><span class="sxs-lookup"><span data-stu-id="d712d-169">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="d712d-170">使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d712d-170">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="d712d-171">`OnGetAsync` 方法的第一行會建立 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="d712d-171">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="d712d-172">這時候，系統只會「定義」查詢  ，而尚**未**對資料庫執行查詢。</span><span class="sxs-lookup"><span data-stu-id="d712d-172">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="d712d-173">如果 `SearchString` 屬性不是 Null 或空白，則會修改電影查詢來篩選搜尋字串：</span><span class="sxs-lookup"><span data-stu-id="d712d-173">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="d712d-174">`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="d712d-174">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d712d-175">在以方法為基礎的 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="d712d-175">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="d712d-176">定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會加以執行。</span><span class="sxs-lookup"><span data-stu-id="d712d-176">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="d712d-177">而會延後執行查詢。</span><span class="sxs-lookup"><span data-stu-id="d712d-177">Rather, query execution is deferred.</span></span> <span data-ttu-id="d712d-178">這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="d712d-178">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="d712d-179">如需詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。</span><span class="sxs-lookup"><span data-stu-id="d712d-179">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="d712d-180">**注意：** [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="d712d-180">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="d712d-181">查詢是否區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="d712d-181">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="d712d-182">在 SQL Server 上，`Contains` 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d712d-182">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="d712d-183">而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d712d-183">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="d712d-184">巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`https://localhost:5001/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="d712d-184">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="d712d-185">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="d712d-185">The filtered movies are displayed.</span></span>

![索引檢視](search/_static/ghost.png)

<span data-ttu-id="d712d-187">如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="d712d-187">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="d712d-188">上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。</span><span class="sxs-lookup"><span data-stu-id="d712d-188">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="d712d-189">在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。</span><span class="sxs-lookup"><span data-stu-id="d712d-189">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](search/_static/g2.png)

<span data-ttu-id="d712d-191">ASP.NET Core 執行階段使用[模型繫結](xref:mvc/models/model-binding)來設定查詢字串 (`?searchString=Ghost`) 中的 `SearchString` 屬性值或路由傳送資料 (`https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="d712d-191">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="d712d-192">模型繫結是不區分大小寫的。</span><span class="sxs-lookup"><span data-stu-id="d712d-192">Model binding is not case sensitive.</span></span>

<span data-ttu-id="d712d-193">但是，您不能期望使用者會在搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="d712d-193">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="d712d-194">在此步驟中，會新增用來篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="d712d-194">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="d712d-195">如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。</span><span class="sxs-lookup"><span data-stu-id="d712d-195">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="d712d-196">開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="d712d-196">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="d712d-197">HTML `<form>` 標籤會使用下列[標籤協助程式](xref:mvc/views/tag-helpers/intro)：</span><span class="sxs-lookup"><span data-stu-id="d712d-197">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="d712d-198">[表單標籤協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="d712d-198">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="d712d-199">提交表單時，系統會透過查詢字串將篩選字串傳送至 *Pages/Movies/Index* 頁面。</span><span class="sxs-lookup"><span data-stu-id="d712d-199">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="d712d-200">輸入標記協助程式</span><span class="sxs-lookup"><span data-stu-id="d712d-200">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="d712d-201">儲存變更並測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d712d-201">Save the changes and test the filter.</span></span>

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="d712d-203">依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="d712d-203">Search by genre</span></span>

<span data-ttu-id="d712d-204">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d712d-204">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="d712d-205">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="d712d-205">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="d712d-206">內容類型的 `SelectList` 是由投影不同的內容類型來建立。</span><span class="sxs-lookup"><span data-stu-id="d712d-206">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="d712d-207">將依內容類型搜尋新增至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d712d-207">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="d712d-208">更新 *Index.cshtml*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d712d-208">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="d712d-209">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="d712d-209">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d712d-210">其他資源</span><span class="sxs-lookup"><span data-stu-id="d712d-210">Additional resources</span></span>

* [<span data-ttu-id="d712d-211">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="d712d-211">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="d712d-212">[上一步：更新頁面](xref:tutorials/razor-pages/da1)
> [下一步：新增欄位](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="d712d-212">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end