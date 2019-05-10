# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core 回應快取範例

此範例說明如何使用 ASP.NET Core[回應快取中介軟體](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)。

應用程式的回應使用其索引 頁面中，包括`Cache-Control`標頭，若要設定快取行為。 應用程式也會設定`Vary`標頭來設定快取服務的回應才`Accept-Encoding`後續的要求標頭符合原始要求。

當執行範例時，[索引] 頁面會提供從快取時儲存，並快取最多 10 秒。
