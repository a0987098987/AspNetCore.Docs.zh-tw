<!--
[!code-html[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

現在當您提交搜尋時，URL 中會包含搜尋查詢字串。 即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。

![顯示 URL 中 searchString=ghost 且傳回的電影 Ghostbusters 和 Ghostbusters 2 包含 ghost 一詞的瀏覽器視窗](../../tutorials/first-mvc-app/search/_static/search_get.png)

下列標記顯示 `form` 標記的變更：

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>新增依內容類型搜尋

將下列 `MovieGenreViewModel` 類別新增至 *Models* 資料夾：

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

電影內容類型檢視模型將包含：

   * 電影清單。
   * 包含內容類型清單的 `SelectList`。 這可讓使用者從清單中選取內容類型。
   * 包含所選取內容類型的 `movieGenre`。

以下列程式碼取代 `MoviesController.cs` 中的 `Index` 方法：

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

下列程式碼是從資料庫中擷取所有內容類型的 `LINQ` 查詢。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

藉由投射不同的內容類型來建立內容類型的 `SelectList` (我們不希望選取清單中有重複的內容類型)。

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a>將依內容類型搜尋新增至 Index 檢視

如下所示更新 `Index.cshtml`：

[!code-HTML[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

檢查下列 HTML 協助程式中使用的 Lambda 運算式：

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
在上述程式碼中，`DisplayNameFor` HTML 協助程式會檢查 Lambda 運算式中參考的 `Title` 屬性來判斷顯示名稱。 由於 Lambda 運算式是檢查而不是評估，因此您在 `model`、`model.movies` 或 `model.movies[0]` 是 `null` 或空白時，不會收到存取違規。 在評估 Lambda 運算式時 (例如，`@Html.DisplayFor(modelItem => item.Title)`)，會評估模型的屬性值。

依據內容類型、電影標題和這兩者進行搜尋，藉以測試應用程式。
