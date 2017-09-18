<span data-ttu-id="975c3-101">上述強調顯示的程式碼會顯示要新增至[相依性插入](xref:fundamentals/dependency-injection)容器的電影資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="975c3-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="975c3-102">不會顯示 `services.AddDbContext<MvcMovieContext>(options =>` 的後續行 (請參閱您的程式碼)。</span><span class="sxs-lookup"><span data-stu-id="975c3-102">The line following `services.AddDbContext<MvcMovieContext>(options =>` is not shown (see your code).</span></span> <span data-ttu-id="975c3-103">它指定要使用的資料庫和連線字串。</span><span class="sxs-lookup"><span data-stu-id="975c3-103">It specifies the database to use and the connection string.</span></span> <span data-ttu-id="975c3-104">`=>` 是 [Lambda 運算子](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator)。</span><span class="sxs-lookup"><span data-stu-id="975c3-104">`=>` is a [lambda operator](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="975c3-105">開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：</span><span class="sxs-lookup"><span data-stu-id="975c3-105">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

<span data-ttu-id="975c3-106">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="975c3-106">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)]</span></span> 

<span data-ttu-id="975c3-107">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext `) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="975c3-107">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="975c3-108">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="975c3-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name=strongly-typed-models-keyword-label></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="975c3-109">強型別模型和 @model 關鍵字</span><span class="sxs-lookup"><span data-stu-id="975c3-109">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="975c3-110">稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。</span><span class="sxs-lookup"><span data-stu-id="975c3-110">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="975c3-111">`ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="975c3-111">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="975c3-112">MVC 也提供將強型別模型物件傳遞至檢視的能力。</span><span class="sxs-lookup"><span data-stu-id="975c3-112">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="975c3-113">強型別方法可讓程式碼的編譯時期檢查變得更佳。</span><span class="sxs-lookup"><span data-stu-id="975c3-113">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="975c3-114">Scaffolding 機制在建立方法和檢視時，已使用此方法 (也就是傳遞強型別模型) 來搭配 `MoviesController` 類別和檢視。</span><span class="sxs-lookup"><span data-stu-id="975c3-114">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="975c3-115">檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：</span><span class="sxs-lookup"><span data-stu-id="975c3-115">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

<span data-ttu-id="975c3-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span><span class="sxs-lookup"><span data-stu-id="975c3-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]</span></span>

<span data-ttu-id="975c3-117">`id` 參數通常會傳遞為路由資料。</span><span class="sxs-lookup"><span data-stu-id="975c3-117">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="975c3-118">例如，`http://localhost:5000/movies/details/1` 設定：</span><span class="sxs-lookup"><span data-stu-id="975c3-118">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="975c3-119">控制器為 `movies` 控制器 (第一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="975c3-119">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="975c3-120">動作為 `details` (第二個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="975c3-120">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="975c3-121">識別碼為 1 (最後一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="975c3-121">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="975c3-122">您也可以在 `id` 中使用查詢字串傳遞，如下所示：</span><span class="sxs-lookup"><span data-stu-id="975c3-122">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="975c3-123">如果未提供識別碼值，`id` 參數會定義為[可為 Null 的型別](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。</span><span class="sxs-lookup"><span data-stu-id="975c3-123">The `id` parameter is defined as a [nullable type](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value is not provided.</span></span>

<span data-ttu-id="975c3-124">[Lambda 運算式](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `SingleOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。</span><span class="sxs-lookup"><span data-stu-id="975c3-124">A [lambda expression](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `SingleOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

<span data-ttu-id="975c3-125">如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：</span><span class="sxs-lookup"><span data-stu-id="975c3-125">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="975c3-126">檢查 *Views/Movies/Details.cshtml* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="975c3-126">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

<span data-ttu-id="975c3-127">[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="975c3-127">[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]</span></span>

<span data-ttu-id="975c3-128">藉由在檢視檔案的最上方包含 `@model` 陳述式，您可以指定檢視預期要有的物件類型。</span><span class="sxs-lookup"><span data-stu-id="975c3-128">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="975c3-129">當您建立電影控制器時，Visual Studio 會在 *Details.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="975c3-129">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="975c3-130">這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。</span><span class="sxs-lookup"><span data-stu-id="975c3-130">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="975c3-131">例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="975c3-131">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="975c3-132">`Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。</span><span class="sxs-lookup"><span data-stu-id="975c3-132">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="975c3-133">檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="975c3-133">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="975c3-134">請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。</span><span class="sxs-lookup"><span data-stu-id="975c3-134">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="975c3-135">此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="975c3-135">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

<span data-ttu-id="975c3-136">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]</span><span class="sxs-lookup"><span data-stu-id="975c3-136">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]</span></span>

<span data-ttu-id="975c3-137">當您建立電影控制器時，Scaffolding 會在 *Index.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="975c3-137">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

<span data-ttu-id="975c3-138">[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]</span><span class="sxs-lookup"><span data-stu-id="975c3-138">[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]</span></span>

<span data-ttu-id="975c3-139">`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。</span><span class="sxs-lookup"><span data-stu-id="975c3-139">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="975c3-140">例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：</span><span class="sxs-lookup"><span data-stu-id="975c3-140">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

<span data-ttu-id="975c3-141">[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]</span><span class="sxs-lookup"><span data-stu-id="975c3-141">[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]</span></span>

<span data-ttu-id="975c3-142">因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。</span><span class="sxs-lookup"><span data-stu-id="975c3-142">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="975c3-143">撇開其他優點，這表示您會進行程式碼編譯時期檢查：</span><span class="sxs-lookup"><span data-stu-id="975c3-143">Among other benefits, this means that you get compile-time checking of the code:</span></span>
