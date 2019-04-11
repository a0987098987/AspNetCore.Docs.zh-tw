# <a name="aspnet-core-distributed-cache-sample"></a>ASP.NET Core 分散式快取範例

此範例說明如何使用分散式快取。 這個範例會示範中所述的案例[使用 ASP.NET Core 中的分散式快取](https://docs.microsoft.com/aspnet/core/performance/caching/distributed)主題。

在生產環境中，範例應用程式設定為使用分散式的 SQL Server 快取。 若要重新設定應用程式使用分散式的 Redis 快取，請變更 前置處理器指示詞，在頂端*Startup.cs*檔案，以使用 Redis (`#define Redis // SQLServer`)。 如需詳細資訊，請參閱 <<c0> [ 範例程式碼中的前置處理器指示詞](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code)。
