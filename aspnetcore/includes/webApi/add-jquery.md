## <a name="call-the-web-api-with-jquery"></a>使用 jQuery 呼叫 Web API

在本節中，將會新增 HTML 網頁，以使用 jQuery 來呼叫 Web API。 jQuery 會起始要求，並使用來自 API 回應的詳細資料更新頁面。

請設定專案來提供靜態檔案，並啟用預設檔案對應。 這是透過在 *Startup.Configure* 中叫用 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 和 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 擴充方法來達成。 如需詳細資訊，請參閱[靜態檔案](xref:fundamentals/static-files)。

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

將名為 *index.html* 的 HTML 檔案新增至專案的 *wwwroot* 目錄。 將其內容取代為下列標記：

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

將名為 *site.js* 的 JavaScript 檔案新增至專案的 *wwwroot* 目錄。 將其內容取代為下列程式碼：

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定。 在專案的 *Properties* 目錄中開啟 *launchSettings.json*。 移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。

您可以使用幾種方式來取得 jQuery。 在上述的程式碼片段中，從 CDN 載入程式庫。 這個範例是使用 jQuery 呼叫 API 的完整 CRUD 範例。 這個範例還有可讓體驗更加豐富的額外功能。 以下是關於呼叫 API 的說明。

### <a name="get-a-list-of-to-do-items"></a>取得待辦事項的清單

若要取得待辦事項的清單，請將 HTTP GET 要求傳送至 */api/todo*。

JQuery [ajax](https://api.jquery.com/jquery.ajax/) 函式會將 AJAX 要求傳送至 API，API 則會傳回代表物件或陣列的 JSON。 此函式可以處理所有形式的 HTTP 互動，並將 HTTP 要求傳送至指定的 `url`。 `GET` 會當作 `type` 使用。 如果要求成功，則會叫用 `success` 回呼函式。 在回呼中，DOM 已使用待辦事項資訊進行更新。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>新增待辦事項

若要新增待辦事項，請將 HTTP POST 要求傳送到 */api/todo/*。 要求本文應該包含待辦事項物件。 [ajax](https://api.jquery.com/jquery.ajax/) 函式會使用 `POST` 來呼叫 API。 對於 `POST` 和 `PUT` 要求，要求本文代表傳送至 API 的資料。 此 API 需要有 JSON 要求本文。 `accepts` 和 `contentType` 選項都設定為 `application/json`，以分別分類接收和傳送的媒體類型。 資料會使用 [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON 物件。 當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>更新待辦事項

更新待辦事項非常類似於新增待辦事項，因為這兩者都依賴要求內文。 在此情況下，兩者之間的唯一差異在於 `url` 變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>刪除待辦事項

刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
