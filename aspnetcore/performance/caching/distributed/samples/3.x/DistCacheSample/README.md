# <a name="aspnet-core-distributed-cache-sample"></a>ASP.NET Core 分散式快取範例

這個範例說明分散式快取的使用。 這個範例會示範在[ASP.NET Core 中使用分散式](https://docs.microsoft.com/aspnet/core/performance/caching/distributed)快取主題中所述的案例。

在生產環境中，範例應用程式會設定為使用分散式 SQL Server 快取。 若要將應用程式重新設定為使用分散式 Redis 快取，請將*Startup.cs*檔案頂端的預處理器指示詞變更為`#define Redis // SQLServer`使用 Redis （）。 如需詳細資訊，請參閱[範例程式碼中的預處理器](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code)指示詞。
