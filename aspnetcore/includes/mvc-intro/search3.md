<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="4d26b-101">現在當您提交搜尋時，URL 中會包含搜尋查詢字串。</span><span class="sxs-lookup"><span data-stu-id="4d26b-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="4d26b-102">即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="4d26b-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![顯示 URL 中 searchString=ghost 且傳回的電影 Ghostbusters 和 Ghostbusters 2 包含 ghost 一詞的瀏覽器視窗](../../tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="4d26b-104">下列標記顯示 `form` 標記的變更：</span><span class="sxs-lookup"><span data-stu-id="4d26b-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="4d26b-105">新增依內容類型搜尋</span><span class="sxs-lookup"><span data-stu-id="4d26b-105">Adding Search by genre</span></span>

<span data-ttu-id="4d26b-106">將下列 `MovieGenreViewModel` 類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="4d26b-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="4d26b-107">電影內容類型檢視模型將包含：</span><span class="sxs-lookup"><span data-stu-id="4d26b-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="4d26b-108">電影清單。</span><span class="sxs-lookup"><span data-stu-id="4d26b-108">A list of movies.</span></span>
   * <span data-ttu-id="4d26b-109">包含內容類型清單的 `SelectList`。</span><span class="sxs-lookup"><span data-stu-id="4d26b-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="4d26b-110">這可讓使用者從清單中選取內容類型。</span><span class="sxs-lookup"><span data-stu-id="4d26b-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="4d26b-111">包含所選取內容類型的 `movieGenre`。</span><span class="sxs-lookup"><span data-stu-id="4d26b-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="4d26b-112">以下列程式碼取代 `MoviesController.cs` 中的 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="4d26b-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="4d26b-113">下列程式碼是從資料庫中擷取所有內容類型的 `LINQ` 查詢。</span><span class="sxs-lookup"><span data-stu-id="4d26b-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="4d26b-114">藉由投射不同的內容類型來建立內容類型的 `SelectList` (我們不希望選取清單中有重複的內容類型)。</span><span class="sxs-lookup"><span data-stu-id="4d26b-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="4d26b-115">將依內容類型搜尋新增至 Index 檢視</span><span class="sxs-lookup"><span data-stu-id="4d26b-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="4d26b-116">如下所示更新 `Index.cshtml`：</span><span class="sxs-lookup"><span data-stu-id="4d26b-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="4d26b-117">檢查下列 HTML 協助程式中使用的 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="4d26b-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="4d26b-118">在上述程式碼中，`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="4d26b-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="4d26b-119">由於 Lambda 運算式是檢查而不是評估，因此您在 `model`、`model.movies` 或 `model.movies[0]` 是 `null` 或空白時，不會收到存取違規。</span><span class="sxs-lookup"><span data-stu-id="4d26b-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="4d26b-120">在評估 Lambda 運算式時 (例如，`@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="4d26b-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="4d26b-121">依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="4d26b-121">Test the app by searching by genre, by movie title, and by both.</span></span>
