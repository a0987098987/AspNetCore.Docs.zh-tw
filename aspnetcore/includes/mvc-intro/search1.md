# <a name="adding-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="7fffb-101">將搜尋新增至 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="7fffb-101">Adding Search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="7fffb-102">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7fffb-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7fffb-103">在本節中，您將搜尋功能新增至 `Index` 動作方法，可讓您依據「內容類型」或「名稱」搜尋電影。</span><span class="sxs-lookup"><span data-stu-id="7fffb-103">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="7fffb-104">以下列程式碼取代 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="7fffb-104">Update the `Index` method with the following code:</span></span>
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

<span data-ttu-id="7fffb-105">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]</span><span class="sxs-lookup"><span data-stu-id="7fffb-105">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]</span></span>

<span data-ttu-id="7fffb-106">`Index` 動作方法的第一行會建立 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 查詢，以選取電影：</span><span class="sxs-lookup"><span data-stu-id="7fffb-106">The first line of the `Index` action method creates a [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="7fffb-107">查詢｢只｣會在此時定義，它尚**未**對資料庫執行。</span><span class="sxs-lookup"><span data-stu-id="7fffb-107">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="7fffb-108">如果 `searchString` 參數包含字串，則會修改電影查詢來篩選搜尋字串的值：</span><span class="sxs-lookup"><span data-stu-id="7fffb-108">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

<span data-ttu-id="7fffb-109">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]</span><span class="sxs-lookup"><span data-stu-id="7fffb-109">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]</span></span>

<span data-ttu-id="7fffb-110">上述 `s => s.Title.Contains()` 程式碼是 [Lambda 運算式](http://msdn.microsoft.com/library/bb397687.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7fffb-110">The `s => s.Title.Contains()` code above is a [Lambda Expression](http://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="7fffb-111">在以方法為基礎的 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 查詢中，Lambda 會用來作為標準查詢運算子方法的引數，例如 [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) 方法或 `Contains` (用於上述程式碼)。</span><span class="sxs-lookup"><span data-stu-id="7fffb-111">Lambdas are used in method-based [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="7fffb-112">在定義 LINQ 查詢或呼叫方法 (例如`Where`、`Contains`或 `OrderBy`) 來修改它們時，不會執行這些查詢。</span><span class="sxs-lookup"><span data-stu-id="7fffb-112">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="7fffb-113">而是會延後查詢執行。</span><span class="sxs-lookup"><span data-stu-id="7fffb-113">Rather, query execution is deferred.</span></span>  <span data-ttu-id="7fffb-114">這是指延遲評估運算式，直到實際反覆運算其實現值或呼叫 `ToListAsync` 方法為止。</span><span class="sxs-lookup"><span data-stu-id="7fffb-114">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="7fffb-115">如需延後查詢執行的詳細資訊，請參閱[查詢執行](http://msdn.microsoft.com/library/bb738633.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7fffb-115">For more information about deferred query execution, see [Query Execution](http://msdn.microsoft.com/library/bb738633.aspx).</span></span>

<span data-ttu-id="7fffb-116">注意：[Contains](http://msdn.microsoft.com/library/bb155125.aspx) 方法是在資料庫上執行，而不是在上方顯示的 C# 程式碼中執行。</span><span class="sxs-lookup"><span data-stu-id="7fffb-116">Note: The [Contains](http://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="7fffb-117">查詢的區分大小寫取決於資料庫和定序。</span><span class="sxs-lookup"><span data-stu-id="7fffb-117">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="7fffb-118">在 SQL Server 上，[Contains](http://msdn.microsoft.com/library/bb155125.aspx) 對應至 [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx)，因此不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="7fffb-118">On SQL Server, [Contains](http://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span> <span data-ttu-id="7fffb-119">在 SQLlite 中，由於使用預設定序，它會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="7fffb-119">In SQLlite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="7fffb-120">巡覽至 `/Movies/Index`。</span><span class="sxs-lookup"><span data-stu-id="7fffb-120">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="7fffb-121">將查詢字串 (例如 `?searchString=Ghost`) 附加至 URL。</span><span class="sxs-lookup"><span data-stu-id="7fffb-121">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="7fffb-122">隨即顯示篩選過的電影。</span><span class="sxs-lookup"><span data-stu-id="7fffb-122">The filtered movies are displayed.</span></span>

![索引檢視](../../tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="7fffb-124">如果您將 `Index` 方法的簽章變更為包含名為 `id` 的參數，則 `id` 參數會比對 *Startup.cs* 中所設定之預設路由的選擇性 `{id}` 預留位置。</span><span class="sxs-lookup"><span data-stu-id="7fffb-124">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

<span data-ttu-id="7fffb-125">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="7fffb-125">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]</span></span>