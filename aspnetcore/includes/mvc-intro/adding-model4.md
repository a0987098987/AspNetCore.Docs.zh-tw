上述強調顯示的程式碼會顯示要新增至[相依性插入](xref:fundamentals/dependency-injection)容器的電影資料庫內容。 不會顯示 `services.AddDbContext<MvcMovieContext>(options =>` 的後續行 (請參閱您的程式碼)。 它指定要使用的資料庫和連線字串。 `=>` 是 [Lambda 運算子](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator)。

開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext `) 插入到控制器中。 控制器中的每一個 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。

<a name=strongly-typed-models-keyword-label></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>強型別模型和 @model 關鍵字

稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。 `ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。

MVC 也提供將強型別模型物件傳遞至檢視的能力。 強型別方法可讓程式碼的編譯時期檢查變得更佳。 Scaffolding 機制在建立方法和檢視時，已使用此方法 (也就是傳遞強型別模型) 來搭配 `MoviesController` 類別和檢視。

檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

`id` 參數通常會傳遞為路由資料。 例如，`http://localhost:5000/movies/details/1` 設定：

* 控制器為 `movies` 控制器 (第一個 URL 區段)。
* 動作為 `details` (第二個 URL 區段)。
* 識別碼為 1 (最後一個 URL 區段)。

您也可以在 `id` 中使用查詢字串傳遞，如下所示：

`http://localhost:1234/movies/details?id=1`

如果未提供識別碼值，`id` 參數會定義為[可為 Null 的型別](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。

[Lambda 運算式](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `SingleOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：

```csharp
return View(movie);
   ```

檢查 *Views/Movies/Details.cshtml* 檔案的內容：

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

藉由在檢視檔案的最上方包含 `@model` 陳述式，您可以指定檢視預期要有的物件類型。 當您建立電影控制器時，Visual Studio 會在 *Details.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：

```HTML
@model MvcMovie.Models.Movie
   ```

這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。 例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。 `Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。

檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。 請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。 此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

當您建立電影控制器時，Scaffolding 會在 *Index.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。 例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。 撇開其他優點，這表示您會進行程式碼編譯時期檢查：
