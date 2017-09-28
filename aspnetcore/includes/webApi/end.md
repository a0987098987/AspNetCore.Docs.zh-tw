## <a name="implement-the-other-crud-operations"></a>實作其他的 CRUD 作業

我們會將 `Create`、`Update` 和 `Delete` 方法新增至控制器。 這些是佈景主題的變化，因此我只會顯示程式碼，並強調顯示主要的差異。 請在新增或變更程式碼之後建置專案。

### <a name="create"></a>建立

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

這是 HTTP POST 方法，以 [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性表示。 [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。

`CreatedAtRoute` 方法會傳回 201 回應，這是 HTTP POST 方法的標準回應，可在伺服器上建立新的資源。 `CreatedAtRoute` 也會將位置標頭新增至回應。 位置標頭指定新建立之待辦事項的 URI。 請參閱 [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。

### <a name="use-postman-to-send-a-create-request"></a>使用 Postman 來傳送建立要求

![Postman 主控台](../../tutorials/first-web-api/_static/pmc.png)

* 將 HTTP 方法設定為 `POST`
* 選取 [主體] 選項按鈕
* 選取 [原始] 選項按鈕
* 將類型設定為 JSON
* 在索引鍵/值編輯器中，輸入待辦事項，例如 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* 選取 [傳送]

* 選取下方窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭：

![Postman 主控台的 [標頭] 索引標籤](../../tutorials/first-web-api/_static/pmget.png)

您可以使用 [位置] 標頭 URI 來存取您剛才建立的資源。 請重新叫用建立 `"GetTodo"` 具名路由的 `GetById` 方法：

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>更新

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` 類似於 `Create`，但是會使用 HTTP PUT。 回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。 根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。 若要支援部分更新，請使用 HTTP PATCH。

![顯示「204 (沒有內容) 回應」的 Postman 主控台](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>刪除

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。

![顯示 204 (沒有內容) 回應的 Postman 主控台](../../tutorials/first-web-api/_static/pmd.png)
