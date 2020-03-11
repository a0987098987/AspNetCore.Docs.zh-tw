# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core 回應快取範例

這個範例說明 ASP.NET Core 回應快取[中介軟體](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)的用法。

應用程式會以其 [索引] 頁面回應，包括用來設定快取行為的 `Cache-Control` 標頭。 應用程式也會設定 `Vary` 標頭，只有在後續要求的 `Accept-Encoding` 標頭符合原始要求的時，才會將快取設定為提供回應。

執行此範例時，會在儲存並快取最多10秒時，從快取提供索引頁面。

若要測試快取行為：

* 請勿使用瀏覽器來測試快取行為。 瀏覽器通常會在重載時加入快取控制標頭，以防止中介軟體提供快取頁面。 例如，瀏覽器可能會加入值為 `max-age=0`的 `Cache-Control` 標頭。
* 使用允許明確設定要求標頭的開發人員工具，例如<a href="https://www.telerik.com/fiddler">Fiddler</a>或<a href="https://www.getpostman.com/">Postman</a>。
