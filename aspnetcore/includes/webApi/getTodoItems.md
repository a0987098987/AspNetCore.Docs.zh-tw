[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

上述程式碼：

* 定義空白控制器類別。 在接下來的幾節中，我們將新增實作 API 的方法。
* 建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`TodoContext `) 插入到控制器中。 控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。
* 建構函式會將項目新增至記憶體內部資料庫 (如果項目不存在的話)。

## <a name="getting-to-do-items"></a>取得待辦事項

若要取得待辦事項，請在 `TodoController` 類別中加入下列方法。

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

這些方法會實作兩個 GET 方法：

* `GET /api/todo`
* `GET /api/todo/{id}`

以下是 `GetAll` 方法的範例 HTTP 回應：

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

稍後在本教學課程中，我們將示範如何使用 [Postman](https://www.getpostman.com/) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) 檢視 HTTP 回應。

### <a name="routing-and-url-paths"></a>傳送和 URL 路徑

`[HttpGet]` 屬性會指定 HTTP GET 方法。 每個方法的 URL 路徑的建構方式如下：

* 在控制器的 Route 屬性中採用範本字串：

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* 將 "[Controller]" 取代為控制器的名稱，也就是控制器類別名稱減去 "Controller" 字尾。 在此範例中，控制器類別名稱是 **Todo**Controller，而根名稱是 "todo"。 ASP.NET Core [傳送](xref:mvc/controllers/routing)不區分大小寫。
* 如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("/products")]`)，請將其附加到路徑。 此範例不使用範本。 如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性傳送](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。

在 `GetById` 方法中：

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

`"{id}"` 是 `todo` 項目識別碼的預留位置變數。 在叫用 `GetById` 時，它會將 URL 中的 "{id}" 值指派給方法的 `id` 參數。

`Name = "GetTodo"` 會建立具名路由，並可讓您在 HTTP 回應中連結到此路由。 稍後，我將利用範例進行說明。 如需詳細資訊，請參閱[傳送至控制器動作](xref:mvc/controllers/routing)。

### <a name="return-values"></a>傳回值

`GetAll` 方法會傳回 `IEnumerable`。 MVC 會自動將物件序列化為 [JSON](http://www.json.org/)，並將 JSON 寫入至回應訊息的本文。 這個方法的回應碼為 200，假設沒有任何未處理的例外狀況。 (未處理的例外狀況會轉譯成 5xx 錯誤)。

反之，`GetById` 方法會傳回更普通的 `IActionResult` 類型，其代表各種不同的傳回型別。 `GetById` 有兩種不同的傳回型別：

* 如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。  藉由傳回 `NotFound` 即可達到此目的。

* 否則，方法會傳回 200 與 JSON 回應本文。 藉由傳回 `ObjectResult` 即可達到此目的
