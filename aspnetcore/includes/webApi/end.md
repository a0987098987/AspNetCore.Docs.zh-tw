## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="416e6-101">實作其他的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="416e6-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="416e6-102">在以下章節中，`Create`、`Update` 和 `Delete` 方法會被新增到控制器。</span><span class="sxs-lookup"><span data-stu-id="416e6-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="416e6-103">建立</span><span class="sxs-lookup"><span data-stu-id="416e6-103">Create</span></span>

<span data-ttu-id="416e6-104">新增以下 `Create` 方法：</span><span class="sxs-lookup"><span data-stu-id="416e6-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="416e6-105">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="416e6-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="416e6-106">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="416e6-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="416e6-107">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="416e6-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="416e6-108">MVC 會從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="416e6-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="416e6-109">`CreatedAtRoute` 方法：</span><span class="sxs-lookup"><span data-stu-id="416e6-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="416e6-110">傳回 201 回應。</span><span class="sxs-lookup"><span data-stu-id="416e6-110">Returns a 201 response.</span></span> <span data-ttu-id="416e6-111">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="416e6-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="416e6-112">將位置標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="416e6-112">Adds a Location header to the response.</span></span> <span data-ttu-id="416e6-113">位置標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="416e6-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="416e6-114">請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。</span><span class="sxs-lookup"><span data-stu-id="416e6-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="416e6-115">使用 "GetTodo" 具名路由來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="416e6-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="416e6-116">"GetTodo" 具名路由定義在 `GetById` 中：</span><span class="sxs-lookup"><span data-stu-id="416e6-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="416e6-117">使用 Postman 來傳送建立要求</span><span class="sxs-lookup"><span data-stu-id="416e6-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="416e6-118">啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="416e6-118">Start the app.</span></span>
* <span data-ttu-id="416e6-119">開啟 Postman。</span><span class="sxs-lookup"><span data-stu-id="416e6-119">Open Postman.</span></span>

![Postman 主控台](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="416e6-121">更新 localhost URL 中的通訊埠號碼。</span><span class="sxs-lookup"><span data-stu-id="416e6-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="416e6-122">將 HTTP 方法設定為 *POST*。</span><span class="sxs-lookup"><span data-stu-id="416e6-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="416e6-123">按一下 [本文] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="416e6-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="416e6-124">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="416e6-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="416e6-125">將類型設定為 *JSON (application/json)*。</span><span class="sxs-lookup"><span data-stu-id="416e6-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="416e6-126">輸入要求本文與待辦項目，類似於下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="416e6-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="416e6-127">按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="416e6-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="416e6-128">如果按一下 [傳送] 之後沒有顯示任何回應，請停用 [SSL 憑證驗證] 選項。</span><span class="sxs-lookup"><span data-stu-id="416e6-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="416e6-129">這位於 [檔案] > [設定] 下。</span><span class="sxs-lookup"><span data-stu-id="416e6-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="416e6-130">停用設定之後，再按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="416e6-130">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="416e6-131">按一下 [回應] 窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="416e6-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman 主控台的 [標頭] 索引標籤](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="416e6-133">位置標頭 URI 可用來存取新項目。</span><span class="sxs-lookup"><span data-stu-id="416e6-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="416e6-134">更新</span><span class="sxs-lookup"><span data-stu-id="416e6-134">Update</span></span>

<span data-ttu-id="416e6-135">新增以下 `Update` 方法：</span><span class="sxs-lookup"><span data-stu-id="416e6-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="416e6-136">`Update` 類似於 `Create`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="416e6-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="416e6-137">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="416e6-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="416e6-138">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。</span><span class="sxs-lookup"><span data-stu-id="416e6-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="416e6-139">若要支援部分更新，請使用 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="416e6-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="416e6-140">使用 Postman 將待辦項目的名稱更新為 "walk cat"：</span><span class="sxs-lookup"><span data-stu-id="416e6-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="416e6-142">刪除</span><span class="sxs-lookup"><span data-stu-id="416e6-142">Delete</span></span>

<span data-ttu-id="416e6-143">新增以下 `Delete` 方法：</span><span class="sxs-lookup"><span data-stu-id="416e6-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="416e6-144">`Delete` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="416e6-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="416e6-145">使用 Postman 刪除待辦項目：</span><span class="sxs-lookup"><span data-stu-id="416e6-145">Use Postman to delete the to-do item:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmd.png)
