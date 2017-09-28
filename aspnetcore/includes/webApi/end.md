## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="df28c-101">實作其他的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="df28c-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="df28c-102">我們會將 `Create`、`Update` 和 `Delete` 方法新增至控制器。</span><span class="sxs-lookup"><span data-stu-id="df28c-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="df28c-103">這些是佈景主題的變化，因此我只會顯示程式碼，並強調顯示主要的差異。</span><span class="sxs-lookup"><span data-stu-id="df28c-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="df28c-104">請在新增或變更程式碼之後建置專案。</span><span class="sxs-lookup"><span data-stu-id="df28c-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="df28c-105">建立</span><span class="sxs-lookup"><span data-stu-id="df28c-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="df28c-106">這是 HTTP POST 方法，以 [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性表示。</span><span class="sxs-lookup"><span data-stu-id="df28c-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="df28c-107">[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="df28c-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="df28c-108">`CreatedAtRoute` 方法會傳回 201 回應，這是 HTTP POST 方法的標準回應，可在伺服器上建立新的資源。</span><span class="sxs-lookup"><span data-stu-id="df28c-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="df28c-109">`CreatedAtRoute` 也會將位置標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="df28c-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="df28c-110">位置標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="df28c-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="df28c-111">請參閱 [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。</span><span class="sxs-lookup"><span data-stu-id="df28c-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="df28c-112">使用 Postman 來傳送建立要求</span><span class="sxs-lookup"><span data-stu-id="df28c-112">Use Postman to send a Create request</span></span>

![Postman 主控台](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="df28c-114">將 HTTP 方法設定為 `POST`</span><span class="sxs-lookup"><span data-stu-id="df28c-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="df28c-115">選取 [主體] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="df28c-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="df28c-116">選取 [原始] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="df28c-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="df28c-117">將類型設定為 JSON</span><span class="sxs-lookup"><span data-stu-id="df28c-117">Set the type to JSON</span></span>
* <span data-ttu-id="df28c-118">在索引鍵/值編輯器中，輸入待辦事項，例如</span><span class="sxs-lookup"><span data-stu-id="df28c-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="df28c-119">選取 [傳送]</span><span class="sxs-lookup"><span data-stu-id="df28c-119">Select **Send**</span></span>

* <span data-ttu-id="df28c-120">選取下方窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭：</span><span class="sxs-lookup"><span data-stu-id="df28c-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman 主控台的 [標頭] 索引標籤](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="df28c-122">您可以使用 [位置] 標頭 URI 來存取您剛才建立的資源。</span><span class="sxs-lookup"><span data-stu-id="df28c-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="df28c-123">請重新叫用建立 `"GetTodo"` 具名路由的 `GetById` 方法：</span><span class="sxs-lookup"><span data-stu-id="df28c-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="df28c-124">更新</span><span class="sxs-lookup"><span data-stu-id="df28c-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="df28c-125">`Update` 類似於 `Create`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="df28c-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="df28c-126">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="df28c-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="df28c-127">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。</span><span class="sxs-lookup"><span data-stu-id="df28c-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="df28c-128">若要支援部分更新，請使用 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="df28c-128">To support partial updates, use HTTP PATCH.</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="df28c-130">刪除</span><span class="sxs-lookup"><span data-stu-id="df28c-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="df28c-131">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="df28c-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![顯示 204 (沒有內容) 回應的 Postman 主控台](../../tutorials/first-web-api/_static/pmd.png)
