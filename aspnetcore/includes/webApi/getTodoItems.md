[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="5aca8-101">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="5aca8-101">The preceding code:</span></span>

* <span data-ttu-id="5aca8-102">定義空白控制器類別。</span><span class="sxs-lookup"><span data-stu-id="5aca8-102">Defines an empty controller class.</span></span> <span data-ttu-id="5aca8-103">在接下來的幾節中，我們將新增方法藉以實作 API。</span><span class="sxs-lookup"><span data-stu-id="5aca8-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="5aca8-104">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`TodoContext `) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="5aca8-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="5aca8-105">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="5aca8-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="5aca8-106">建構函式會將項目新增至記憶體內部資料庫 (如果項目不存在的話)。</span><span class="sxs-lookup"><span data-stu-id="5aca8-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="5aca8-107">取得待辦事項</span><span class="sxs-lookup"><span data-stu-id="5aca8-107">Getting to-do items</span></span>

<span data-ttu-id="5aca8-108">若要取得待辦事項，請在 `TodoController` 類別中加入下列方法。</span><span class="sxs-lookup"><span data-stu-id="5aca8-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="5aca8-109">這些方法會實作兩個 GET 方法：</span><span class="sxs-lookup"><span data-stu-id="5aca8-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="5aca8-110">以下是 `GetAll` 方法的範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="5aca8-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="5aca8-111">稍後在本教學課程中，我們將示範如何使用 [Postman](https://www.getpostman.com/) \(英文\) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) \(英文\) 來檢視 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="5aca8-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="5aca8-112">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="5aca8-112">Routing and URL paths</span></span>

<span data-ttu-id="5aca8-113">`[HttpGet]` 屬性會指定 HTTP GET 方法。</span><span class="sxs-lookup"><span data-stu-id="5aca8-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="5aca8-114">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="5aca8-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="5aca8-115">在控制器的 `Route` 屬性中採用範本字串：</span><span class="sxs-lookup"><span data-stu-id="5aca8-115">Take the template string in the controller’s `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="5aca8-116">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="5aca8-116">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="5aca8-117">在此範例中，控制器類別名稱是 **Todo**Controller，而根名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="5aca8-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="5aca8-118">ASP.NET Core [路由傳送](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="5aca8-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="5aca8-119">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("/products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="5aca8-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="5aca8-120">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="5aca8-120">This sample doesn't use a template.</span></span> <span data-ttu-id="5aca8-121">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性傳送](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="5aca8-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="5aca8-122">在 `GetById` 方法中：</span><span class="sxs-lookup"><span data-stu-id="5aca8-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="5aca8-123">`"{id}"` 是 `todo` 項目識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="5aca8-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="5aca8-124">在叫用 `GetById` 時，它會將 URL 中的 "{id}" 值指派給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="5aca8-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="5aca8-125">`Name = "GetTodo"` 會建立具名路由。</span><span class="sxs-lookup"><span data-stu-id="5aca8-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="5aca8-126">具名路由：</span><span class="sxs-lookup"><span data-stu-id="5aca8-126">Named routes:</span></span>

* <span data-ttu-id="5aca8-127">讓應用程式以該路由名稱建立 HTTP 連結。</span><span class="sxs-lookup"><span data-stu-id="5aca8-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="5aca8-128">本教學課程稍後會說明。</span><span class="sxs-lookup"><span data-stu-id="5aca8-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="5aca8-129">傳回值</span><span class="sxs-lookup"><span data-stu-id="5aca8-129">Return values</span></span>

<span data-ttu-id="5aca8-130">`GetAll` 方法會傳回 `IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="5aca8-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="5aca8-131">MVC 會自動將物件序列化為 [JSON](http://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="5aca8-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="5aca8-132">這個方法的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5aca8-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="5aca8-133">(未處理的例外狀況會轉譯成 5xx 錯誤)。</span><span class="sxs-lookup"><span data-stu-id="5aca8-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="5aca8-134">反之，`GetById` 方法會傳回更普通的 `IActionResult` 類型，其代表各種不同的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="5aca8-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="5aca8-135">`GetById` 有兩種不同的傳回型別：</span><span class="sxs-lookup"><span data-stu-id="5aca8-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="5aca8-136">如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="5aca8-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="5aca8-137">傳回 `NotFound` 會傳回 HTTP 404 回應。</span><span class="sxs-lookup"><span data-stu-id="5aca8-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="5aca8-138">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="5aca8-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="5aca8-139">傳回 `ObjectResult` 會傳回 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="5aca8-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
