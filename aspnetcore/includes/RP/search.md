# <a name="adding-search-to-a-razor-pages-app"></a><span data-ttu-id="0a5a2-101">新增搜尋至 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="0a5a2-101">Adding search to a Razor Pages app</span></span>

<span data-ttu-id="0a5a2-102">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0a5a2-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0a5a2-103">在本文件中，我們會將搜尋功能新增至 Index 頁面，以便由「內容類型」或「名稱」來搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="0a5a2-104">使用下列程式碼來更新 Index 頁面的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="0a5a2-105">`OnGetAsync` 方法的第一行會建立 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-105">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="0a5a2-106">這時候，系統只會「定義」查詢，而尚**未**對資料庫執行查詢。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="0a5a2-107">如果 `searchString` 參數包含字串，則會修改電影查詢來篩選搜尋字串：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="0a5a2-108">`s => s.Title.Contains()` 程式碼是一種 [Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-108">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="0a5a2-109">在以方法為基礎的 [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) 查詢中，會將 Lambda 作為標準查詢運算子方法的引數，例如 [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-109">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="0a5a2-110">定義 LINQ 查詢或藉由呼叫像是 `Where`、`Contains` 或 `OrderBy` 等方法進行修改時，並不會加以執行。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="0a5a2-111">而會延後執行查詢。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="0a5a2-112">這表示系統會延遲評估運算式，直到該運算式的實現值受到逐一查看，或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="0a5a2-113">如需詳細資訊，請參閱[查詢執行](/dotnet/framework/data/adonet/ef/language-reference/query-execution)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-113">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="0a5a2-114">**注意：**[Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-114">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="0a5a2-115">查詢是否區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="0a5a2-116">在 SQL Server 上，`Contains` 對應至 [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-116">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="0a5a2-117">而在 SQLlite 中，由於使用預設定序，因此會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="0a5a2-118">巡覽至 Movies 頁面，並將 `?searchString=Ghost` 這類查詢字串附加至 URL (例如，`http://localhost:5000/Movies?searchString=Ghost`)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="0a5a2-119">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-119">The filtered movies are displayed.</span></span>

![索引檢視](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="0a5a2-121">如果您已將下列路由範本新增至 Index 頁面，即可透過 URL 區段的形式來傳遞搜尋字串 (例如 `http://localhost:5000/Movies/ghost`)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="0a5a2-122">上述的路由條件約束可讓您以路由資料的形式 (URL 區段) 搜尋標題，而不是以查詢字串值的形式。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="0a5a2-123">在 `?` 中，`"{searchString?}"` 表示此為選擇性的路由參數。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![索引檢視，其中已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩部電影](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="0a5a2-125">但是，您不能期望使用者會在搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="0a5a2-126">在此步驟中，會新增用來篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="0a5a2-127">如果您已新增路由條件約束 `"{searchString?}"`，請將它移除。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="0a5a2-128">開啟 *Pages/Movies/Index.cshtml* 檔案，並新增下列程式碼中反白顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="0a5a2-129">HTML`<form>` 標記會使用[表單標記協助程式](xref:mvc/views/working-with-forms#the-form-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="0a5a2-130">提交表單時，系統會將篩選條件字串傳送至 *Pages/Movies/Index* 頁面。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="0a5a2-131">儲存變更並測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-131">Save the changes and test the filter.</span></span>

![索引檢視，其中已將 ghost 一詞輸入 [標題] 篩選條件文字方塊](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="0a5a2-133">依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="0a5a2-133">Search by genre</span></span>

<span data-ttu-id="0a5a2-134">將下列反白顯示的屬性新增至 *Pages/Movies/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-134">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="0a5a2-135">`SelectList Genres` 包含內容類型清單。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="0a5a2-136">這可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="0a5a2-137">`MovieGenre` 屬性包含使用者選取的特定內容類型 (例如「西部片」)。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="0a5a2-138">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="0a5a2-139">下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="0a5a2-140">內容類型的 `SelectList` 是由投影不同的內容類型來建立。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="0a5a2-141">新增依內容類型的搜尋</span><span class="sxs-lookup"><span data-stu-id="0a5a2-141">Adding search by genre</span></span>

<span data-ttu-id="0a5a2-142">更新 *Index.cshtml*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0a5a2-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="0a5a2-143">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a5a2-143">Test the app by searching by genre, by movie title, and by both.</span></span>
