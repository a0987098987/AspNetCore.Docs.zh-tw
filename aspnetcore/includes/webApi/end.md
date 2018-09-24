## <a name="implement-the-other-crud-operations"></a>實作其他的 CRUD 作業

在以下章節中，`Create`、`Update` 和 `Delete` 方法會被新增到控制器。

### <a name="create"></a>建立

新增以下 `Create` 方法：

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。 [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。 MVC 會從 HTTP 要求的主體取得待辦事項的值。

::: moniker-end

`CreatedAtRoute` 方法：

* 傳回 201 回應。 對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。
* 將位置標頭新增至回應。 位置標頭指定新建立之待辦事項的 URI。 請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。
* 使用 "GetTodo" 具名路由來建立 URL。 "GetTodo" 具名路由定義在 `GetById` 中：

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>使用 Postman 來傳送建立要求

* 啟動應用程式。
* 開啟 Postman。

![Postman 主控台](../../tutorials/first-web-api/_static/pmc.png)

* 更新 localhost URL 中的通訊埠號碼。
* 將 HTTP 方法設定為 *POST*。
* 按一下 [本文] 索引標籤。
* 選取 [原始] 選項按鈕。
* 將類型設定為 *JSON (application/json)*。
* 輸入要求本文與待辦項目，類似於下列 JSON：

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* 按一下 [傳送] 按鈕。

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> 如果按一下 [傳送] 之後沒有顯示任何回應，請停用 [SSL 憑證驗證] 選項。 這位於 [檔案] > [設定] 下。 停用設定之後，再按一下 [傳送] 按鈕。

::: moniker-end

按一下 [回應] 窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭值：

![Postman 主控台的 [標頭] 索引標籤](../../tutorials/first-web-api/_static/pmc2.png)

位置標頭 URI 可用來存取新項目。

### <a name="update"></a>更新

新增以下 `Update` 方法：

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

`Update` 類似於 `Create`，但是會使用 HTTP PUT。 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。 根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。 若要支援部分更新，請使用 HTTP PATCH。

使用 Postman 將待辦項目的名稱更新為 "walk cat"：

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>刪除

新增以下 `Delete` 方法：

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。

使用 Postman 刪除待辦項目：

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmd.png)
