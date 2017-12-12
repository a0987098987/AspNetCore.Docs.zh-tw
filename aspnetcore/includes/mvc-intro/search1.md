# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>將搜尋新增至 ASP.NET Core MVC 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中，您將搜尋功能新增至 `Index` 動作方法，可讓您依據「內容類型」或「名稱」搜尋電影。

以下列程式碼取代 `Index` 方法：
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

`Index` 動作方法的第一行會建立 [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) 查詢，以選取電影：

```csharp
var movies = from m in _context.Movie
             select m;
```

查詢｢只｣會在此時定義，它尚**未**對資料庫執行。

如果 `searchString` 參數包含字串，則會修改電影查詢來篩選搜尋字串的值：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

上述 `s => s.Title.Contains()` 程式碼是 [Lambda 運算式](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。 在以方法為基礎的 [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) 查詢中，Lambda 會用來作為標準查詢運算子方法的引數，例如 [Where](https://docs.microsoft.com//dotnet/api/system.linq.enumerable.where) 方法或 `Contains` (用於上述程式碼)。 在定義 LINQ 查詢或呼叫方法 (例如`Where`、`Contains`或 `OrderBy`) 來修改它們時，不會執行這些查詢。 而是會延後查詢執行。  這是指延遲評估運算式，直到實際反覆運算其實現值或呼叫 `ToListAsync` 方法為止。 如需延後查詢執行的詳細資訊，請參閱[查詢執行](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution)。

注意：[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法是在資料庫上執行，而不是在上方顯示的 C# 程式碼中執行。 查詢的區分大小寫取決於資料庫和定序。 在 SQL Server 上，[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 對應至 [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql)，因此不區分大小寫。 在 SQLlite 中，由於使用預設定序，它會區分大小寫。

巡覽至 `/Movies/Index`。 將查詢字串 (例如 `?searchString=Ghost`) 附加至 URL。 隨即顯示篩選過的電影。

![索引檢視](../../tutorials/first-mvc-app/search/_static/ghost.png)

如果您將 `Index` 方法的簽章變更為包含名為 `id` 的參數，則 `id` 參數會比對 *Startup.cs* 中所設定之預設路由的選擇性 `{id}` 預留位置。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
