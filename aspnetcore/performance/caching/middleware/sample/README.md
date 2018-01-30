# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core 回應快取範例

此範例說明使用 ASP.NET Core[回應快取中介軟體](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)。

應用程式的回應其索引頁，包括`Cache-Control`標頭來設定快取行為。 應用程式也會設定`Vary`標頭設定快取提供回應才`Accept-Encoding`後續要求的標頭符合原始要求。

執行範例時，就會從快取儲存與快取多達 10 秒時提供的索引頁面。
