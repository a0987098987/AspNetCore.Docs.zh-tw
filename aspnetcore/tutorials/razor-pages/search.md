---
title: "將搜尋新增至 ASP.NET Core Razor 頁面"
author: rick-anderson
description: "示範如何將搜尋新增至 ASP.NET Core Razor 頁面"
keywords: "ASP.NET Core, 搜尋, Razor 頁面"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: 8a272b63edb1d173c4ae0324fe4bbdbfede424c6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="adding-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="8172c-104">將搜尋新增至 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="8172c-104">Adding search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="8172c-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8172c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8172c-106">在本文件中，我們會將搜尋功能新增至 Index 頁面，以便由「內容類型」或「名稱」來搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="8172c-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="8172c-107">使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="8172c-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

<span data-ttu-id="8172c-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]</span><span class="sxs-lookup"><span data-stu-id="8172c-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]</span></span>

<span data-ttu-id="8172c-109">`OnGetAsync` 方法的第一行會建立 [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="8172c-109">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
 var movies = from m in _context.Movie
              select m;
```

<span data-ttu-id="8172c-110">這時候，系統只會「定義」查詢，而尚**未**對資料庫執行查詢。</span><span class="sxs-lookup"><span data-stu-id="8172c-110">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="8172c-111">如果 `searchString` 參數包含字串，則會修改電影查詢來篩選搜尋字串：</span><span class="sxs-lookup"><span data-stu-id="8172c-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

<span data-ttu-id="8172c-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]</span><span class="sxs-lookup"><span data-stu-id="8172c-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]</span></span>

<span data-ttu-id="8172c-113">`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="8172c-113">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="8172c-114">在以方法為基礎的 [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="8172c-114">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="8172c-115">在您定義 LINQ 查詢，或藉由呼叫 `Where`、`Contains` 或 `OrderBy` 這類方法加以修改時，不會執行 LINQ 查詢，</span><span class="sxs-lookup"><span data-stu-id="8172c-115">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="8172c-116">而會延後執行查詢。</span><span class="sxs-lookup"><span data-stu-id="8172c-116">Rather, query execution is deferred.</span></span> <span data-ttu-id="8172c-117">這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="8172c-117">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="8172c-118">如需詳細資訊，請參閱[查詢執行](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution)。</span><span class="sxs-lookup"><span data-stu-id="8172c-118">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="8172c-119">**注意：**[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="8172c-119">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="8172c-120">查詢是否區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="8172c-120">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="8172c-121">在 SQL Server 上，`Contains` 對應至 [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8172c-121">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="8172c-122">而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="8172c-122">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="8172c-123">巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`http://localhost:5000/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="8172c-123">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="8172c-124">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="8172c-124">The filtered movies are displayed.</span></span>

![索引檢視](search/_static/ghost.png)

<span data-ttu-id="8172c-126">如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `http://localhost:5000/Movies/ghost`)。</span><span class="sxs-lookup"><span data-stu-id="8172c-126">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="8172c-127">上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。</span><span class="sxs-lookup"><span data-stu-id="8172c-127">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="8172c-128">在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。</span><span class="sxs-lookup"><span data-stu-id="8172c-128">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](search/_static/g2.png)

<span data-ttu-id="8172c-130">但是，您不能期望使用者會在搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="8172c-130">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="8172c-131">在此步驟中，會新增用來篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="8172c-131">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="8172c-132">如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。</span><span class="sxs-lookup"><span data-stu-id="8172c-132">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="8172c-133">開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="8172c-133">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

<span data-ttu-id="8172c-134">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]</span><span class="sxs-lookup"><span data-stu-id="8172c-134">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]</span></span>

<span data-ttu-id="8172c-135">HTML`<form>` 標記會使用[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="8172c-135">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="8172c-136">提交表單時，系統會將篩選條件字串傳送至 *Pages/Movies/Index* 頁面。</span><span class="sxs-lookup"><span data-stu-id="8172c-136">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="8172c-137">儲存變更並測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="8172c-137">Save the changes and test the filter.</span></span>

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="8172c-139">依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="8172c-139">Search by genre</span></span>

<span data-ttu-id="8172c-140">將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="8172c-140">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

<span data-ttu-id="8172c-141">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]</span><span class="sxs-lookup"><span data-stu-id="8172c-141">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]</span></span>

<span data-ttu-id="8172c-142">`SelectList Genres` 包含內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="8172c-142">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="8172c-143">這可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="8172c-143">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="8172c-144">`MovieGenre` 屬性包含使用者選取的特定內容類型 (例如「西部片」)。</span><span class="sxs-lookup"><span data-stu-id="8172c-144">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="8172c-145">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="8172c-145">Update the `OnGetAsync` method with the following code:</span></span>

<span data-ttu-id="8172c-146">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="8172c-146">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="8172c-147">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="8172c-147">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="8172c-148">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="8172c-148">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="8172c-149">內容類型的 `SelectList` 是由投影不同的內容類型來建立。</span><span class="sxs-lookup"><span data-stu-id="8172c-149">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="8172c-150">新增依內容類型的搜尋</span><span class="sxs-lookup"><span data-stu-id="8172c-150">Adding search by genre</span></span>

<span data-ttu-id="8172c-151">更新 *Index.cshtml*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8172c-151">Update *Index.cshtml* as follows:</span></span>

<span data-ttu-id="8172c-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]</span><span class="sxs-lookup"><span data-stu-id="8172c-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]</span></span>

<span data-ttu-id="8172c-153">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="8172c-153">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8172c-154">[上一步：更新頁面](xref:tutorials/razor-pages/da1)
[下一步：新增欄位](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="8172c-154">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
