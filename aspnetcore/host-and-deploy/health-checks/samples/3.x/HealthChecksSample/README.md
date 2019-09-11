# <a name="aspnet-core-health-check-sample"></a>ASP.NET Core 健康狀態檢查範例

此範例說明如何使用健康狀態檢查中介軟體和服務。 此範例示範 [ASP.NET Core 中的健康狀態檢查](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks)主題中所述的案例。

若要在該主題所述案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](https://docs.microsoft.com/dotnet/core/tools/dotnet-run) 命令。 傳遞您要探索的案例參數。 若未提供參數給 `dotnet run`，應用程式會預設為 `basic` 組態。

| 狀況                                               | 範例應用程式命令               | 描述 |
| ------------------------------------------------------ | -------------------------------- | ----------- |
| 基本健康狀態探查 (預設)                           | `dotnet run --scenario basic`    | 確認應用程式可處理 HTTP 要求。 |
| 資料庫探查                                         | `dotnet run --scenario db`       | 檢查 SQL Server 資料庫連線。 如需指示，請參閱該主題的[資料庫探查](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#database-probe)一節。 |
| 整備度/活躍度探查                              | `dotnet run --scenario liveness` | 執行即時應用程式狀態 (「活躍度」) 與應用程式準備成為即時 (「整備度」) 的檢查。 |
| 計量型探查 (記憶體)/<br>自訂回應寫入器 | `dotnet run --scenario writer`   | 檢查記憶體使用量，並在檢查健康狀態端點時撰寫自訂 JSON。 |
| 依連接埠篩選                                         | `dotnet run --scenario port`     | 將健康狀態檢查篩選至指定的連接埠。 如需指示，請參閱該主題的[依連接埠篩選](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#filter-by-port)一節。 |

資料庫探查和連接埠篩選案例需要其他組態。 如需詳細資訊，請參閱[健康狀態檢查](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks)主題。
