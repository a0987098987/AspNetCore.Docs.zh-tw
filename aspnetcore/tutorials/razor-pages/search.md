---
title: 將搜尋新增至 ASP.NET Core Razor 頁面
author: rick-anderson
description: 示範如何將搜尋新增至 ASP.NET Core Razor 頁面
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/search
ms.openlocfilehash: 8228207b0f37a6923b29891ac3115dd0be115501
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667703"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="26b2c-103">將搜尋新增至 ASP.NET Core Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="26b2c-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="26b2c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="26b2c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="26b2c-105">在下列各節中，會新增依「內容類型」\*\* 或「名稱」\*\* 搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="26b2c-105">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="26b2c-106">將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="26b2c-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="26b2c-107">`SearchString`：包含使用者在搜尋文字方塊中輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="26b2c-107">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="26b2c-108">`SearchString`具有[`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="26b2c-108">`SearchString` has the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="26b2c-109">`[BindProperty]` 使用與屬性相同的名稱來繫結表單值和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="26b2c-109">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="26b2c-110">需要 `(SupportsGet = true)` 才能在 GET 要求上進行繫結。</span><span class="sxs-lookup"><span data-stu-id="26b2c-110">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="26b2c-111">`Genres`：包含內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="26b2c-111">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="26b2c-112">`Genres` 可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="26b2c-112">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="26b2c-113">`SelectList` 需要 `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="26b2c-113">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="26b2c-114">`MovieGenre`：包含使用者所選取的特定內容類型 (例如「西部片」)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-114">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="26b2c-115">稍後在本教學課程中將會使用 `Genres` 和 `MovieGenre`。</span><span class="sxs-lookup"><span data-stu-id="26b2c-115">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="26b2c-116">使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="26b2c-116">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="26b2c-117">`OnGetAsync` 方法的第一行會建立 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="26b2c-117">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="26b2c-118">這時候，系統只會「定義」查詢**，而尚**未\*\*對資料庫執行查詢。</span><span class="sxs-lookup"><span data-stu-id="26b2c-118">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="26b2c-119">如果 `SearchString` 屬性不是 Null 或空白，則會修改電影查詢來篩選搜尋字串：</span><span class="sxs-lookup"><span data-stu-id="26b2c-119">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="26b2c-120">`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-120">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="26b2c-121">在以方法為基礎的 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-121">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="26b2c-122">定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會加以執行。</span><span class="sxs-lookup"><span data-stu-id="26b2c-122">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="26b2c-123">而會延後執行查詢。</span><span class="sxs-lookup"><span data-stu-id="26b2c-123">Rather, query execution is deferred.</span></span> <span data-ttu-id="26b2c-124">這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="26b2c-124">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="26b2c-125">如需詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-125">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="26b2c-126">[Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="26b2c-126">The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="26b2c-127">查詢是否區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="26b2c-127">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="26b2c-128">在 SQL Server 上，`Contains` 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="26b2c-128">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="26b2c-129">而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="26b2c-129">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="26b2c-130">巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`https://localhost:5001/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-130">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="26b2c-131">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="26b2c-131">The filtered movies are displayed.</span></span>

![索引檢視](search/_static/ghost.png)

<span data-ttu-id="26b2c-133">如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-133">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="26b2c-134">上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。</span><span class="sxs-lookup"><span data-stu-id="26b2c-134">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="26b2c-135">在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。</span><span class="sxs-lookup"><span data-stu-id="26b2c-135">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](search/_static/g2.png)

<span data-ttu-id="26b2c-137">ASP.NET Core 執行階段使用[模型繫結](xref:mvc/models/model-binding)來設定查詢字串 (`?searchString=Ghost`) 中的 `SearchString` 屬性值或路由傳送資料 (`https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-137">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="26b2c-138">模型繫結是不區分大小寫的。</span><span class="sxs-lookup"><span data-stu-id="26b2c-138">Model binding is not case sensitive.</span></span>

<span data-ttu-id="26b2c-139">但是，您不能期望使用者會在搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="26b2c-139">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="26b2c-140">在此步驟中，會新增用來篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="26b2c-140">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="26b2c-141">如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。</span><span class="sxs-lookup"><span data-stu-id="26b2c-141">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="26b2c-142">開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="26b2c-142">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="26b2c-143">HTML `<form>` 標籤會使用下列[標籤協助程式](xref:mvc/views/tag-helpers/intro)：</span><span class="sxs-lookup"><span data-stu-id="26b2c-143">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="26b2c-144">[表單標記說明程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-144">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="26b2c-145">提交表單時，系統會透過查詢字串將篩選字串傳送至 *Pages/Movies/Index* 頁面。</span><span class="sxs-lookup"><span data-stu-id="26b2c-145">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="26b2c-146">輸入標記協助程式</span><span class="sxs-lookup"><span data-stu-id="26b2c-146">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="26b2c-147">儲存變更並測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="26b2c-147">Save the changes and test the filter.</span></span>

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="26b2c-149">依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="26b2c-149">Search by genre</span></span>

<span data-ttu-id="26b2c-150">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="26b2c-150">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="26b2c-151">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="26b2c-151">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="26b2c-152">內容類型的 `SelectList` 是由投影不同的內容類型來建立。</span><span class="sxs-lookup"><span data-stu-id="26b2c-152">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="26b2c-153">將依內容類型搜尋新增至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="26b2c-153">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="26b2c-154">更新 *Index.cshtml*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="26b2c-154">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="26b2c-155">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="26b2c-155">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26b2c-156">其他資源</span><span class="sxs-lookup"><span data-stu-id="26b2c-156">Additional resources</span></span>

* [<span data-ttu-id="26b2c-157">本教學的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="26b2c-157">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="26b2c-158">[上一篇:更新](xref:tutorials/razor-pages/da1)
> [頁面下一頁:新增新欄位](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="26b2c-158">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="26b2c-159">在下列各節中，會新增依「內容類型」\*\* 或「名稱」\*\* 搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="26b2c-159">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="26b2c-160">將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="26b2c-160">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="26b2c-161">`SearchString`：包含使用者在搜尋文字方塊中輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="26b2c-161">`SearchString`: contains the text users enter in the search text box.</span></span> <span data-ttu-id="26b2c-162">`SearchString`具有[`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="26b2c-162">`SearchString` has the [`[BindProperty]`](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) attribute.</span></span> <span data-ttu-id="26b2c-163">`[BindProperty]` 使用與屬性相同的名稱來繫結表單值和查詢字串。</span><span class="sxs-lookup"><span data-stu-id="26b2c-163">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="26b2c-164">需要 `(SupportsGet = true)` 才能在 GET 要求上進行繫結。</span><span class="sxs-lookup"><span data-stu-id="26b2c-164">`(SupportsGet = true)` is required for binding on GET requests.</span></span>
* <span data-ttu-id="26b2c-165">`Genres`：包含內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="26b2c-165">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="26b2c-166">`Genres` 可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="26b2c-166">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="26b2c-167">`SelectList` 需要 `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="26b2c-167">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="26b2c-168">`MovieGenre`：包含使用者所選取的特定內容類型 (例如「西部片」)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-168">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>
* <span data-ttu-id="26b2c-169">稍後在本教學課程中將會使用 `Genres` 和 `MovieGenre`。</span><span class="sxs-lookup"><span data-stu-id="26b2c-169">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="26b2c-170">使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="26b2c-170">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="26b2c-171">`OnGetAsync` 方法的第一行會建立 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="26b2c-171">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="26b2c-172">這時候，系統只會「定義」查詢**，而尚**未\*\*對資料庫執行查詢。</span><span class="sxs-lookup"><span data-stu-id="26b2c-172">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="26b2c-173">如果 `SearchString` 屬性不是 Null 或空白，則會修改電影查詢來篩選搜尋字串：</span><span class="sxs-lookup"><span data-stu-id="26b2c-173">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="26b2c-174">`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-174">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="26b2c-175">在以方法為基礎的 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-175">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="26b2c-176">定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會加以執行。</span><span class="sxs-lookup"><span data-stu-id="26b2c-176">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="26b2c-177">而會延後執行查詢。</span><span class="sxs-lookup"><span data-stu-id="26b2c-177">Rather, query execution is deferred.</span></span> <span data-ttu-id="26b2c-178">這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="26b2c-178">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="26b2c-179">如需詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-179">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="26b2c-180">**注意：**[Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="26b2c-180">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="26b2c-181">查詢是否區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="26b2c-181">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="26b2c-182">在 SQL Server 上，`Contains` 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="26b2c-182">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="26b2c-183">而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="26b2c-183">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="26b2c-184">巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`https://localhost:5001/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-184">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `https://localhost:5001/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="26b2c-185">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="26b2c-185">The filtered movies are displayed.</span></span>

![索引檢視](search/_static/ghost.png)

<span data-ttu-id="26b2c-187">如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-187">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `https://localhost:5001/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="26b2c-188">上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。</span><span class="sxs-lookup"><span data-stu-id="26b2c-188">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="26b2c-189">在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。</span><span class="sxs-lookup"><span data-stu-id="26b2c-189">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](search/_static/g2.png)

<span data-ttu-id="26b2c-191">ASP.NET Core 執行階段使用[模型繫結](xref:mvc/models/model-binding)來設定查詢字串 (`?searchString=Ghost`) 中的 `SearchString` 屬性值或路由傳送資料 (`https://localhost:5001/Movies/Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-191">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="26b2c-192">模型繫結是不區分大小寫的。</span><span class="sxs-lookup"><span data-stu-id="26b2c-192">Model binding is not case sensitive.</span></span>

<span data-ttu-id="26b2c-193">但是，您不能期望使用者會在搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="26b2c-193">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="26b2c-194">在此步驟中，會新增用來篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="26b2c-194">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="26b2c-195">如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。</span><span class="sxs-lookup"><span data-stu-id="26b2c-195">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="26b2c-196">開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="26b2c-196">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="26b2c-197">HTML `<form>` 標籤會使用下列[標籤協助程式](xref:mvc/views/tag-helpers/intro)：</span><span class="sxs-lookup"><span data-stu-id="26b2c-197">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="26b2c-198">[表單標記說明程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="26b2c-198">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="26b2c-199">提交表單時，系統會透過查詢字串將篩選字串傳送至 *Pages/Movies/Index* 頁面。</span><span class="sxs-lookup"><span data-stu-id="26b2c-199">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="26b2c-200">輸入標記協助程式</span><span class="sxs-lookup"><span data-stu-id="26b2c-200">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="26b2c-201">儲存變更並測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="26b2c-201">Save the changes and test the filter.</span></span>

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="26b2c-203">依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="26b2c-203">Search by genre</span></span>

<span data-ttu-id="26b2c-204">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="26b2c-204">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="26b2c-205">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="26b2c-205">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="26b2c-206">內容類型的 `SelectList` 是由投影不同的內容類型來建立。</span><span class="sxs-lookup"><span data-stu-id="26b2c-206">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="26b2c-207">將依內容類型搜尋新增至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="26b2c-207">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="26b2c-208">更新 *Index.cshtml*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="26b2c-208">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="26b2c-209">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="26b2c-209">Test the app by searching by genre, by movie title, and by both.</span></span>
<span data-ttu-id="26b2c-210">前面的代碼使用[選擇標記説明器](xref:mvc/views/working-with-forms#the-select-tag-helper)和選項標記説明程式。</span><span class="sxs-lookup"><span data-stu-id="26b2c-210">The preceding code uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper) and Option Tag Helper.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="26b2c-211">其他資源</span><span class="sxs-lookup"><span data-stu-id="26b2c-211">Additional resources</span></span>

* [<span data-ttu-id="26b2c-212">本教學的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="26b2c-212">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="26b2c-213">[上一篇:更新](xref:tutorials/razor-pages/da1)
> [頁面下一頁:新增新欄位](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="26b2c-213">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end
