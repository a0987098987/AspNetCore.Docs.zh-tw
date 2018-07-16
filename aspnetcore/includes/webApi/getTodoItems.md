::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="2da91-101">上述程式碼中定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="2da91-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="2da91-102">在接下來的幾節中，我們將新增方法藉以實作 API。</span><span class="sxs-lookup"><span data-stu-id="2da91-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="2da91-103">上述程式碼中定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="2da91-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="2da91-104">在接下來的幾節中，我們將新增方法藉以實作 API。</span><span class="sxs-lookup"><span data-stu-id="2da91-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="2da91-105">類別將以 `[ApiController]` 屬性進行標註來啟用一些便捷的功能。</span><span class="sxs-lookup"><span data-stu-id="2da91-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="2da91-106">如需屬性所啟用功能的資訊，請參閱[以 ApiControllerAttribute 標註類別](xref:web-api/index#annotate-class-with-apicontrollerattribute)。</span><span class="sxs-lookup"><span data-stu-id="2da91-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="2da91-107">控制器的建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`TodoContext`) 插入至控制器中。</span><span class="sxs-lookup"><span data-stu-id="2da91-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="2da91-108">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="2da91-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="2da91-109">建構函式會將項目新增至記憶體內部資料庫 (如果項目不存在的話)。</span><span class="sxs-lookup"><span data-stu-id="2da91-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="2da91-110">取得待辦事項</span><span class="sxs-lookup"><span data-stu-id="2da91-110">Get to-do items</span></span>

<span data-ttu-id="2da91-111">若要取得待辦事項，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="2da91-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="2da91-112">這些方法會實作兩個 GET 方法：</span><span class="sxs-lookup"><span data-stu-id="2da91-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="2da91-113">以下是 `GetAll` 方法的範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="2da91-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="2da91-114">稍後在本教學課程中，我們將示範如何使用 [Postman](https://www.getpostman.com/) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) 來檢視 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="2da91-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="2da91-115">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="2da91-115">Routing and URL paths</span></span>

<span data-ttu-id="2da91-116">`[HttpGet]` 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="2da91-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="2da91-117">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="2da91-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="2da91-118">在控制器的 `Route` 屬性中採用範本字串：</span><span class="sxs-lookup"><span data-stu-id="2da91-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="2da91-119">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="2da91-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="2da91-120">在此範例中，控制器類別名稱是 **Todo**Controller，而根名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="2da91-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="2da91-121">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2da91-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="2da91-122">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("/products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="2da91-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="2da91-123">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="2da91-123">This sample doesn't use a template.</span></span> <span data-ttu-id="2da91-124">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="2da91-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="2da91-125">在下列 `GetById` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="2da91-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="2da91-126">在叫用 `GetById` 時，它會將 URL 中的 `"{id}"` 值指派給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="2da91-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="2da91-127">`Name = "GetTodo"` 會建立具名路由。</span><span class="sxs-lookup"><span data-stu-id="2da91-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="2da91-128">具名路由：</span><span class="sxs-lookup"><span data-stu-id="2da91-128">Named routes:</span></span>

* <span data-ttu-id="2da91-129">讓應用程式以該路由名稱建立 HTTP 連結。</span><span class="sxs-lookup"><span data-stu-id="2da91-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="2da91-130">本教學課程稍後會說明。</span><span class="sxs-lookup"><span data-stu-id="2da91-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="2da91-131">傳回值</span><span class="sxs-lookup"><span data-stu-id="2da91-131">Return values</span></span>

<span data-ttu-id="2da91-132">`GetAll` 方法會傳回 `TodoItem` 物件的集合。</span><span class="sxs-lookup"><span data-stu-id="2da91-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="2da91-133">MVC 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="2da91-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="2da91-134">這個方法的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2da91-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="2da91-135">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2da91-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="2da91-136">反之，`GetById` 方法會傳回更普通的 [IActionResult 類型](xref:web-api/action-return-types#iactionresult-type)，其代表各種不同的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="2da91-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="2da91-137">`GetById` 有兩種不同的傳回型別：</span><span class="sxs-lookup"><span data-stu-id="2da91-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="2da91-138">如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2da91-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="2da91-139">傳回 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 會傳回 HTTP 404 回應。</span><span class="sxs-lookup"><span data-stu-id="2da91-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="2da91-140">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="2da91-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="2da91-141">傳回 [OK](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="2da91-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="2da91-142">反之，`GetById` 方法會傳回 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)，其代表各種不同的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="2da91-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="2da91-143">`GetById` 有兩種不同的傳回型別：</span><span class="sxs-lookup"><span data-stu-id="2da91-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="2da91-144">如果沒有項目符合所要求的識別碼，方法會傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2da91-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="2da91-145">傳回 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 會傳回 HTTP 404 回應。</span><span class="sxs-lookup"><span data-stu-id="2da91-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="2da91-146">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="2da91-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="2da91-147">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="2da91-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
