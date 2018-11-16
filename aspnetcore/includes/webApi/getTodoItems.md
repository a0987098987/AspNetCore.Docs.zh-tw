::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="b8e4c-101">上述程式碼中定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="b8e4c-102">在接下來的幾節中，我們將新增方法藉以實作 API。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-102">In the next sections, methods are added to implement the API.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="b8e4c-103">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-103">The preceding code:</span></span>

* <span data-ttu-id="b8e4c-104">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="b8e4c-105">當 `TodoItems` 是空的時建立新待辦事項。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="b8e4c-106">您將無法刪除所有待辦事項，因為建構函式會在 `TodoItems` 是空的時建立一個新的待辦事項。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="b8e4c-107">在接下來的幾節中，我們將新增方法藉以實作 API。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="b8e4c-108">類別將以 `[ApiController]` 屬性進行標註來啟用一些便捷的功能。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="b8e4c-109">如需屬性所啟用功能的資訊，請參閱[以 ApiControllerAttribute 標註](xref:web-api/index#annotation-with-apicontrollerattribute)。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-109">For information on features enabled by the attribute, see [Annotation with ApiControllerAttribute](xref:web-api/index#annotation-with-apicontrollerattribute).</span></span>

::: moniker-end

<span data-ttu-id="b8e4c-110">控制器的建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`TodoContext`) 插入至控制器中。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="b8e4c-111">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="b8e4c-112">建構函式會將項目新增至記憶體內部資料庫 (如果項目不存在的話)。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="b8e4c-113">取得待辦事項</span><span class="sxs-lookup"><span data-stu-id="b8e4c-113">Get to-do items</span></span>

<span data-ttu-id="b8e4c-114">若要取得待辦事項，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

<span data-ttu-id="b8e4c-115">這些方法會實作兩個 GET 方法：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="b8e4c-116">以下是 `GetAll` 方法的範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="b8e4c-117">稍後在本教學課程中，我們將示範如何使用 [Postman](https://www.getpostman.com/) 或 [curl](https://curl.haxx.se/docs/manpage.html) 來檢視 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="b8e4c-118">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="b8e4c-118">Routing and URL paths</span></span>

<span data-ttu-id="b8e4c-119">`[HttpGet]` 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="b8e4c-120">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="b8e4c-121">在控制器的 `Route` 屬性中採用範本字串：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* <span data-ttu-id="b8e4c-122">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-122">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="b8e4c-123">在此範例中，控制器類別名稱是 **Todo**Controller，而根名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="b8e4c-124">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="b8e4c-125">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("/products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="b8e4c-126">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-126">This sample doesn't use a template.</span></span> <span data-ttu-id="b8e4c-127">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="b8e4c-128">在下列 `GetById` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="b8e4c-129">在叫用 `GetById` 時，它會將 URL 中的 `"{id}"` 值指派給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

<span data-ttu-id="b8e4c-130">`Name = "GetTodo"` 會建立具名路由。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="b8e4c-131">具名路由：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-131">Named routes:</span></span>

* <span data-ttu-id="b8e4c-132">讓應用程式以該路由名稱建立 HTTP 連結。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="b8e4c-133">本教學課程稍後會說明。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="b8e4c-134">傳回值</span><span class="sxs-lookup"><span data-stu-id="b8e4c-134">Return values</span></span>

<span data-ttu-id="b8e4c-135">`GetAll` 方法會傳回 `TodoItem` 物件的集合。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="b8e4c-136">MVC 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="b8e4c-137">這個方法的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="b8e4c-138">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="b8e4c-139">反之，`GetById` 方法會傳回更普通的 [IActionResult 類型](xref:web-api/action-return-types#iactionresult-type)，其代表各種不同的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="b8e4c-140">`GetById` 有兩種不同的傳回型別：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="b8e4c-141">如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="b8e4c-142">傳回 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 會傳回 HTTP 404 回應。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="b8e4c-143">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="b8e4c-144">傳回 [OK](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b8e4c-145">反之，`GetById` 方法會傳回 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)，其代表各種不同的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="b8e4c-146">`GetById` 有兩種不同的傳回型別：</span><span class="sxs-lookup"><span data-stu-id="b8e4c-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="b8e4c-147">如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="b8e4c-148">傳回 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 會傳回 HTTP 404 回應。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="b8e4c-149">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="b8e4c-150">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="b8e4c-150">Returning `item` results in an HTTP 200 response.</span></span>

::: moniker-end
