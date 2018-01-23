以下列內容取代 *Controllers/HelloWorldController.cs* 的內容：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_1)]

控制器中的每個 `public` 方法可呼叫作為 HTTP 端點。 在上述範例中，這兩種方法都會傳回一個字串。  請注意每個方法之前的註解。

HTTP 端點是 Web 應用程式中的可設定目標 URL，例如 `http://localhost:1234/HelloWorld`，其結合使用的通訊協定：`HTTP`、網頁伺服器的網路位置 (包括 TCP 連接埠)：`localhost:1234`，以及目標 URI `HelloWorld`。

第一個註解指出這是 [HTTP GET](https://www.w3schools.com/tags/ref_httpmethods.asp) 方法，叫用方式是將 "/HelloWorld/" 附加至基底 URL。 第二個註解指定 [HTTP GET](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) 方法，叫用方式是將 "/HelloWorld/Welcome/" 附加至 URL。 稍後在本教學課程中，您將使用 Scaffolding 引擎產生 `HTTP POST` 方法。

在非偵錯模式中執行應用程式，並將 "HelloWorld" 附加至網址列中的路徑。 `Index` 方法會傳回一個字串。

![顯示應用程式回應 "This is my default action" 的瀏覽器視窗](../../tutorials/first-mvc-app/adding-controller/_static/hell1.png)

MVC 會根據傳入 URL 叫用控制器類別 (和其中的動作方法)。 MVC 使用的預設 [URL 路由邏輯](../../mvc/controllers/routing.md)使用像這樣的格式來判斷要叫用的程式碼：

`/[Controller]/[ActionName]/[Parameters]`

您可以在 *Startup.cs* 的 `Configure` 方法中設定路由的格式。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

當您執行應用程式而不提供任何 URL 區段時，則會預設為上方強調顯示之範本行中指定的 "Home" 控制器和 "Index" 方法。

第一個 URL 區段決定要執行的控制器類別。 因此，`localhost:xxxx/HelloWorld` 會對應至 `HelloWorldController` 類別。 URL 區段的第二部分則決定類別上的動作方法。 因此，`localhost:xxxx/HelloWorld/Index` 會導致 `HelloWorldController` 類別的 `Index` 方法執行。 請注意，您只需要瀏覽至 `localhost:xxxx/HelloWorld`，根據預設就會呼叫 `Index` 方法。 這是因為 `Index` 是未明確指定方法名稱時，將在控制器上呼叫的預設方法。 URL 區段的第三個部分 (`id`) 是路由資料。 您稍後會在本教學課程中看到路由資料。

瀏覽至 `http://localhost:xxxx/HelloWorld/Welcome`。 `Welcome` 方法隨即執行，並傳回字串 "This is the Welcome action method..."。 在此 URL 中，控制器是 `HelloWorld`，而 `Welcome` 是動作方法。 您尚未使用 URL 的 `[Parameters]` 部分。

![顯示應用程式回應 "This is the Welcome action method" 的瀏覽器視窗](../../tutorials/first-mvc-app/adding-controller/_static/welcome.png)

修改程式碼 ，將 URL 中的某些參數資訊傳遞到控制器。 例如，`/HelloWorld/Welcome?name=Rick&numtimes=4`。 變更 `Welcome` 方法以包含兩個參數，如下列程式碼所示。 

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_2)]

上述程式碼：

* 使用 C# 選擇性參數功能來指出若未針對 `numTimes` 參數傳遞任何值時，該參數預設為 1。
* 使用 `HtmlEncoder.Default.Encode` 來保護應用程式免於遭受惡意輸入 (也就是 JavaScript) 攻擊。 
* 使用[字串插值](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/keywords/interpolated-strings)。

執行您的應用程式，然後瀏覽至：

   `http://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

(將 xxxx 取代為您的連接埠編號)。您可以在 URL 中針對 `name` 和 `numtimes` 嘗試不同的值。 MVC [模型繫結](../../mvc/models/model-binding.md)系統會自動將網址列上查詢字串中的具名參數對應至方法中的參數。 如需詳細資訊，請參閱[模型繫結](../../mvc/models/model-binding.md)。

![顯示應用程式回應 "Hello Rick, NumTimes is: 4" 的瀏覽器視窗](../../tutorials/first-mvc-app/adding-controller/_static/rick4.png)

在上方的影像中，不會使用 URL 區段 (`Parameters`) ，`name` 和 `numTimes` 則作為[查詢字串](https://wikipedia.org/wiki/Query_string)傳遞。 上述 URL 中的 `?` (問號) 是分隔符號，隨後接著查詢字串。 `&` 字元可分隔查詢字串。

以下列程式碼取代 `Welcome` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_3)]

執行應用程式，並輸入下列 URL：`http://localhost:xxx/HelloWorld/Welcome/3?name=Rick`

![顯示應用程式回應 "Hello Rick, ID: 3" 的瀏覽器視窗](../../tutorials/first-mvc-app/adding-controller/_static/rick_routedata.png)

此時，第三個 URL 區段符合路由參數 `id`。 `Welcome` 方法包含符合 `MapRoute` 方法中的 URL 範本的 `id` 參數。 結尾的 `?` (在 `id?` 中) 表示 `id` 是選擇性參數。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=snippet_1&highlight=5)]

在這些範例中，控制器已執行 MVC 的 "VC" 部分，也就是檢視和控制器工作。 控制器會直接傳回 HTML。 一般來說，您不希望控制器直接傳回 HTML，因為撰寫程式碼和維護會變得很麻煩。 相反地，您通常使用個別的 Razor 檢視範本檔案來協助產生 HTML 回應。 您可在接下來的教學課程中這麼做。
