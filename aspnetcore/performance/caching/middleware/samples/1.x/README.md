# <a name="aspnet-core-response-caching-sample-aspnet-core-1x"></a>ASP.NET Core 回應快取範例 (ASP.NET Core 1.x)

此範例說明使用 ASP.NET Core[回應快取中介軟體](xref:performance/caching/middleware)與 ASP.NET Core 1.x。 如需 ASP.NET Core 2.x 範例中，請參閱[ASP.NET Core 回應快取範例 (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/2.x)。

在應用程式傳送`Hello World!`訊息和目前的時間，連同`Cache-Control`標頭來設定快取行為。 應用程式也會傳送`Vary`標頭設定快取提供回應才`Accept-Encoding`後續要求的標頭符合原始要求。

執行範例時，提供回應從快取時可能和儲存多達 10 秒。
