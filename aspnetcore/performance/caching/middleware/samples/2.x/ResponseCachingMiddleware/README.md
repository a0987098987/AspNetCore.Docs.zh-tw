# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core 回應快取範例

此範例說明如何使用 ASP.NET Core[回應快取中介軟體](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)。

應用程式的回應使用其索引 頁面中，包括`Cache-Control`標頭，若要設定快取行為。 應用程式也會設定`Vary`標頭來設定快取服務的回應才`Accept-Encoding`後續的要求標頭符合原始要求。

當執行範例時，[索引] 頁面會提供從快取時儲存，並快取最多 10 秒。

若要測試的快取行為：

* 不使用瀏覽器測試快取行為。 瀏覽器通常會重新載入，以防止在中介軟體提供快取的頁面上加入快取控制標頭。 例如，`Cache-Control`標頭值是`max-age=0`) 可能會加入瀏覽器。
* 使用允許明確地設定要求標頭的開發人員工具這類<a href="https://www.telerik.com/fiddler">Fiddler</a>或是<a href="https://www.getpostman.com/">Postman</a>。
