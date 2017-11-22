## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="65cf9-101">實作其他的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="65cf9-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="65cf9-102">在以下章節中，`Create`、`Update` 和 `Delete` 方法會被新增到控制器。</span><span class="sxs-lookup"><span data-stu-id="65cf9-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="65cf9-103">建立</span><span class="sxs-lookup"><span data-stu-id="65cf9-103">Create</span></span>

<span data-ttu-id="65cf9-104">新增下列 `Create` 方法。</span><span class="sxs-lookup"><span data-stu-id="65cf9-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="65cf9-105">上述程式碼是 HTTP POST 方法，以 [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性表示。</span><span class="sxs-lookup"><span data-stu-id="65cf9-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="65cf9-106">[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="65cf9-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="65cf9-107">`CreatedAtRoute` 方法：</span><span class="sxs-lookup"><span data-stu-id="65cf9-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="65cf9-108">傳回 201 回應。</span><span class="sxs-lookup"><span data-stu-id="65cf9-108">Returns a 201 response.</span></span> <span data-ttu-id="65cf9-109">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="65cf9-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="65cf9-110">將位置標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="65cf9-110">Adds a Location header to the response.</span></span> <span data-ttu-id="65cf9-111">位置標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="65cf9-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="65cf9-112">請參閱 [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。</span><span class="sxs-lookup"><span data-stu-id="65cf9-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="65cf9-113">使用 "GetTodo" 具名路由來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="65cf9-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="65cf9-114">"GetTodo" 具名路由定義在 `GetById` 中：</span><span class="sxs-lookup"><span data-stu-id="65cf9-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="65cf9-115">使用 Postman 來傳送建立要求</span><span class="sxs-lookup"><span data-stu-id="65cf9-115">Use Postman to send a Create request</span></span>

![Postman 主控台](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="65cf9-117">將 HTTP 方法設定為 `POST`</span><span class="sxs-lookup"><span data-stu-id="65cf9-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="65cf9-118">選取 [主體] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="65cf9-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="65cf9-119">選取 [原始] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="65cf9-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="65cf9-120">將類型設定為 JSON</span><span class="sxs-lookup"><span data-stu-id="65cf9-120">Set the type to JSON</span></span>
* <span data-ttu-id="65cf9-121">在索引鍵/值編輯器中，輸入待辦事項，例如</span><span class="sxs-lookup"><span data-stu-id="65cf9-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="65cf9-122">選取 [傳送]</span><span class="sxs-lookup"><span data-stu-id="65cf9-122">Select **Send**</span></span>
* <span data-ttu-id="65cf9-123">選取下方窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭：</span><span class="sxs-lookup"><span data-stu-id="65cf9-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman 主控台的 [標頭] 索引標籤](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="65cf9-125">位置標頭 URI 可用來存取新項目。</span><span class="sxs-lookup"><span data-stu-id="65cf9-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="65cf9-126">更新</span><span class="sxs-lookup"><span data-stu-id="65cf9-126">Update</span></span>

<span data-ttu-id="65cf9-127">新增以下 `Update` 方法：</span><span class="sxs-lookup"><span data-stu-id="65cf9-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="65cf9-128">`Update` 類似於 `Create`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="65cf9-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="65cf9-129">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="65cf9-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="65cf9-130">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。</span><span class="sxs-lookup"><span data-stu-id="65cf9-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="65cf9-131">若要支援部分更新，請使用 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="65cf9-131">To support partial updates, use HTTP PATCH.</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="65cf9-133">刪除</span><span class="sxs-lookup"><span data-stu-id="65cf9-133">Delete</span></span>

<span data-ttu-id="65cf9-134">新增以下 `Delete` 方法：</span><span class="sxs-lookup"><span data-stu-id="65cf9-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="65cf9-135">`Delete` 回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="65cf9-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="65cf9-136">測試 `Delete`：</span><span class="sxs-lookup"><span data-stu-id="65cf9-136">Test `Delete`:</span></span> 

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmd.png)
