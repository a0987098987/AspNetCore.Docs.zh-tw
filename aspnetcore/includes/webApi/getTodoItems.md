<span data-ttu-id="a711e-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="a711e-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="a711e-102">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="a711e-102">The preceding code:</span></span>

* <span data-ttu-id="a711e-103">定義空白控制器類別。</span><span class="sxs-lookup"><span data-stu-id="a711e-103">Defines an empty controller class.</span></span> <span data-ttu-id="a711e-104">在接下來的幾節中，我們將新增實作 API 的方法。</span><span class="sxs-lookup"><span data-stu-id="a711e-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="a711e-105">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`TodoContext `) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="a711e-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="a711e-106">控制器中的每一個 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="a711e-106">The database context is used in each of the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="a711e-107">建構函式會將項目新增至記憶體內部資料庫 (如果項目不存在的話)。</span><span class="sxs-lookup"><span data-stu-id="a711e-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="a711e-108">取得待辦事項</span><span class="sxs-lookup"><span data-stu-id="a711e-108">Getting to-do items</span></span>

<span data-ttu-id="a711e-109">若要取得待辦事項，請在 `TodoController` 類別中加入下列方法。</span><span class="sxs-lookup"><span data-stu-id="a711e-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="a711e-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="a711e-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="a711e-111">這些方法會實作兩個 GET 方法：</span><span class="sxs-lookup"><span data-stu-id="a711e-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="a711e-112">以下是 `GetAll` 方法的範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="a711e-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="a711e-113">稍後在本教學課程中，我們將示範如何使用 [Postman](https://www.getpostman.com/) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) 檢視 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="a711e-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="a711e-114">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="a711e-114">Routing and URL paths</span></span>

<span data-ttu-id="a711e-115">`[HttpGet]` 屬性會指定 HTTP GET 方法。</span><span class="sxs-lookup"><span data-stu-id="a711e-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="a711e-116">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="a711e-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="a711e-117">在控制器的 Route 屬性中採用範本字串：</span><span class="sxs-lookup"><span data-stu-id="a711e-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="a711e-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="a711e-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="a711e-119">將 "[Controller]" 取代為控制器的名稱，也就是控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="a711e-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="a711e-120">在此範例中，控制器類別名稱是 **Todo**Controller，而根名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="a711e-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="a711e-121">ASP.NET Core [傳送](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a711e-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="a711e-122">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("/products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="a711e-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="a711e-123">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="a711e-123">This sample doesn't use a template.</span></span> <span data-ttu-id="a711e-124">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性傳送](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="a711e-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="a711e-125">在 `GetById` 方法中：</span><span class="sxs-lookup"><span data-stu-id="a711e-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="a711e-126">`"{id}"` 是 `todo` 項目識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="a711e-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="a711e-127">在叫用 `GetById` 時，它會將 URL 中的 "{id}" 值指派給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="a711e-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="a711e-128">`Name = "GetTodo"` 會建立具名路由，並可讓您在 HTTP 回應中連結到此路由。</span><span class="sxs-lookup"><span data-stu-id="a711e-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="a711e-129">稍後，我將利用範例進行說明。</span><span class="sxs-lookup"><span data-stu-id="a711e-129">I'll explain it with an example later.</span></span> <span data-ttu-id="a711e-130">如需詳細資訊，請參閱[傳送至控制器動作](xref:mvc/controllers/routing)。</span><span class="sxs-lookup"><span data-stu-id="a711e-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="a711e-131">傳回值</span><span class="sxs-lookup"><span data-stu-id="a711e-131">Return values</span></span>

<span data-ttu-id="a711e-132">`GetAll` 方法會傳回 `IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="a711e-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="a711e-133">MVC 會自動將物件序列化為 [JSON](http://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="a711e-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="a711e-134">這個方法的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a711e-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="a711e-135">(未處理的例外狀況會轉譯成 5xx 錯誤)。</span><span class="sxs-lookup"><span data-stu-id="a711e-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="a711e-136">反之，`GetById` 方法會傳回更普通的 `IActionResult` 類型，其代表各種不同的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="a711e-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="a711e-137">`GetById` 有兩種不同的傳回型別：</span><span class="sxs-lookup"><span data-stu-id="a711e-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="a711e-138">如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="a711e-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="a711e-139">藉由傳回 `NotFound` 即可達到此目的。</span><span class="sxs-lookup"><span data-stu-id="a711e-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="a711e-140">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="a711e-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="a711e-141">藉由傳回 `ObjectResult` 即可達到此目的</span><span class="sxs-lookup"><span data-stu-id="a711e-141">This is done by returning an `ObjectResult`</span></span>
