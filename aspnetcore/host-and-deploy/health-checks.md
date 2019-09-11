---
title: ASP.NET Core 中的健康狀態檢查
author: guardrex
description: 了解如何為 ASP.NET Core 基礎結構 (例如應用程式和資料庫) 設定健康狀態檢查。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 09/10/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: cc30b3fc67cec42eada20aed494642cf6d88b289
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878451"
---
# <a name="health-checks-in-aspnet-core"></a>ASP.NET Core 中的健康狀態檢查

作者：[Luke Latham](https://github.com/guardrex) 和 [Glenn Condron](https://github.com/glennc)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。

應用程式會將健康狀態檢查公開為 HTTP 端點。 您可以針對各種即時監控案例來設定健康狀態檢查端點：

* 容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。 例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。 負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。
* 您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。
* 健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式包含本主題中所述的案例範例。 若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。 如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。

## <a name="prerequisites"></a>必要條件

健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。 將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。 監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。

將套件參考新增至[AspNetCore HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks)套件。 若要使用 Entity Framework Core 執行健康情況檢查，請將套件參考新增至[HealthChecks. microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore)套件。

範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。 [資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。 [DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。 為了探索資料庫案例，範例應用程式會：

* 建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。
* 在其專案檔中具有下列套件參考：
  * [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。 範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。 如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。

## <a name="basic-health-probe"></a>基本健康狀態探查

對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。

基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。 預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。 如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。 預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 在中呼叫`MapHealthChecks` ，以`Startup.Configure`建立健康狀態檢查端點。

在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHealthChecks("/health");
        });
    }
}
```

若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a>Docker 範例

[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>建立健康狀態檢查

健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。 結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。

下列`ExampleHealthCheck`類別示範健全狀況檢查的版面配置。 健康情況檢查邏輯會放在`CheckHealthAsync`方法中。 下列範例會將虛擬變數`healthCheckResultHealthy`設定為。 `true` 如果的值`healthCheckResultHealthy`設定為`false`，則會傳回[HealthCheckResult](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("A healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("An unhealthy result."));
    }
}
```

## <a name="register-health-check-services"></a>登錄健康狀態檢查服務

類型會加入至<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>中`Startup.ConfigureServices`的健康狀態檢查服務： `ExampleHealthCheck`

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。 此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。

您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。 在下列範例中，健康狀態檢查名稱指定為 `Example`，且檢查一律會傳回狀況良好狀態：

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a>使用健全狀況檢查路由

在`Startup.Configure`中， `MapHealthChecks`使用端點 URL 或相對路徑在端點產生器上呼叫：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a>需要主機

呼叫`RequireHost`以指定一或多個允許的主機用於健康情況檢查端點。 主機應該是 Unicode 而不是 punycode，而且可能包含埠。 如果未提供集合，則會接受任何主機。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。

### <a name="require-authorization"></a>需要授權

呼叫`RequireAuthorization`以在健康情況檢查要求端點上執行授權中介軟體。 `RequireAuthorization`多載會接受一或多個授權原則。 如果未提供原則，則會使用預設的授權原則。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a>啟用跨原始來源要求 (CORS)

雖然從瀏覽器手動執行健康情況檢查並不是常見的使用案例，但您可以呼叫`RequireCors`健全狀況檢查端點來啟用 CORS 中介軟體。 多載會接受 CORS 原則產生器委派`CorsPolicyBuilder`（）或原則名稱。 `RequireCors` 如果未提供原則，則會使用預設的 CORS 原則。 如需詳細資訊，請參閱 <xref:security/cors>。

## <a name="health-check-options"></a>健康狀態檢查選項

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：

* [篩選健康狀態檢查](#filter-health-checks)
* [自訂 HTTP 狀態碼](#customize-the-http-status-code)
* [隱藏快取標頭](#suppress-cache-headers)
* [自訂輸出](#customize-output)

### <a name="filter-health-checks"></a>篩選健康狀態檢查

根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。 若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。 在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：

在 `Startup.ConfigureServices`中：

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

在`Startup.Configure`中`Predicate` ，會篩選出 ' Bar ' 健康情況檢查。 只有 Foo 和 Baz.png execute。：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
});
```

### <a name="customize-the-http-status-code"></a>自訂 HTTP 狀態碼

您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。 下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。 請變更狀態碼值以符合您的需求。

在 `Startup.Configure`中：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
});
```

### <a name="suppress-cache-headers"></a>隱藏快取標頭

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。 如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。 如果值為 `true`，則中介軟體不會修改回應的快取標頭。

在 `Startup.Configure`中：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a>自訂輸出

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。

在 `Startup.Configure`中：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。 下列自訂委派`WriteResponse`會輸出自訂 JSON 回應：

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

健全狀況檢查系統不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。 您可以視需要自`JObject`定義上述範例中的，以符合您的需求。

## <a name="database-probe"></a>資料庫探查

健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。

範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。 `AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。

> [!WARNING]
> 使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。 查詢方法具有多載資料庫而降低其效能的風險。 在大部分情況下，不需要執行測試查詢。 只要成功建立資料庫連線就已足夠。 如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。

包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。

在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。 應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

健康狀態檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario db
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

## <a name="entity-framework-core-dbcontext-probe"></a>Entity Framework Core DbContext 探查

`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。 應用程式支援 `DbContext` 檢查：

* 使用 [Entity Framework (EF) Core](/ef/core/)。
* 包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。

`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。 `DbContext` 會以 `TContext` 形式提供給方法。 多載可用來設定失敗狀態、標籤和自訂測試查詢。

根據預設：

* `DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。 您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。
* 健康狀態檢查的名稱是 `TContext` 類型的名稱。

在範例應用程式中`AppDbContext` ，會提供`AddDbContextCheck`給，並在（*DbCoNtextHealthStartup.cs*）中`Startup.ConfigureServices`註冊為服務：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

健康狀態檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。 如果資料庫存在，請予以刪除。

在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario dbcontext
```

在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。 資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：

```
Unhealthy
```

觸發範例應用程式以建立資料庫。 對 `/createdatabase` 提出要求。 應用程式會回應：

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

對 `/health` 端點提出要求。 資料庫和內容存在，因此應用程式會回應：

```
Healthy
```

觸發範例應用程式以刪除資料庫。 對 `/deletedatabase` 提出要求。 應用程式會回應：

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

對 `/health` 端點提出要求。 應用程式會提供狀況不良回應：

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>個別的整備度與活躍度探查

在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：

* 應用程式正常運作但尚未準備好接收要求。 此狀態是指應用程式的「整備度」。
* 應用程式正常運作並回應要求。 此狀態是指應用程式的「活躍度」。

整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。 活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。 當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。

範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。 `StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。 工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。 由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

健康情況檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立。 在範例應用程式中，健康狀態檢查端點會建立于：

* `/health/ready`以進行準備檢查。 整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。
* `/health/live`做為活動檢查。 活動檢查`StartupHostedServiceHealthCheck`會藉`false`由在[HealthCheckOptions](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate)中傳回來篩選掉（如需詳細資訊，請參閱[篩選健全狀況檢查](#filter-health-checks)）

在下列範例程式碼中：

* 準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。
* 會`Predicate`排除所有檢查並傳回 200-Ok。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario liveness
```

在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。 健康狀態檢查在前 15 秒會回報 *Unhealthy*。 15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。

此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。 如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。

### <a name="kubernetes-example"></a>Kubernetes 範例

使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。 在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。 使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。 如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。

下列範例示範 Kubernetes 整備度探查組態：

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a>透過自訂回應寫入器的計量型探查

範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。

如果應用程式使用超過指定的記憶體閾值 (在範例應用程式中為 1 GB)，`MemoryHealthCheck` 會報告降級的狀態。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 `MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。 所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。 建議將健康狀態檢查服務登錄為單一服務。

在範例應用程式（*CustomWriterStartup.cs*）中：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

健康情況檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立。 當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario writer
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

## <a name="filter-by-port"></a>依連接埠篩選

使用指定埠的 URL 模式來呼叫`RequireHost` ，以限制對指定埠的健康狀態檢查要求。 `MapHealthChecks` 這通常會用於容器環境，以公開監視服務的連接埠。

範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。 連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。 您也必須將伺服器設定為在管理連接埠上接聽要求。

若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。

範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 在中呼叫`MapHealthChecks` ，以`Startup.Configure`建立健康狀態檢查端點。

在範例應用程式中，對`RequireHost`上`Startup.Configure`端點的呼叫會從設定中指定管理埠：

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

端點會在的範例應用程式`Startup.Configure`中建立。 在下列範例程式碼中：

* 準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。
* 會`Predicate`排除所有檢查並傳回 200-Ok。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health/ready", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("ready"),
    });

    endpoints.MapHealthChecks("/health/live", new HealthCheckOptions()
    {
        Predicate = (_) => false
    });
}
```

> [!NOTE]
> 您可以在程式碼中明確設定管理埠，以避免在範例應用程式中建立*launchsettings.json。* 在建立的<xref:Microsoft.Extensions.Hosting.HostBuilder> Program.cs 中， <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*>新增對的呼叫，並提供應用程式的管理埠端點。 在`Configure` *ManagementPortStartup.cs*的中，使用`RequireHost`指定管理埠：
>
> *Program.cs*：
>
> ```csharp
> return new HostBuilder()
>     .ConfigureWebHostDefaults(webBuilder =>
>     {
>         webBuilder.UseKestrel()
>             .ConfigureKestrel(serverOptions =>
>             {
>                 serverOptions.ListenAnyIP(5001);
>             })
>             .UseStartup(startupType);
>     })
>     .Build();
> ```
>
> *ManagementPortStartup.cs*：
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>發佈健康狀態檢查程式庫

若要發佈健康狀態檢查作為程式庫：

1. 寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。 此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。

   在健康情況`CheckHealthAsync`檢查的邏輯：

   * `data1`和`data2`會在方法中用來執行探查的健康情況檢查邏輯。
   * `AccessViolationException`已處理。

   當發生時<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> ，會與一起<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>傳回，讓使用者能夠設定健全狀況檢查失敗狀態。 <xref:System.AccessViolationException>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. 使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。 在下列範例中，假設健康狀態檢查方法簽章如下：

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。 此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。 在下列範例中，呼叫者會選擇性地指定：

   * 健康狀態檢查名稱 (`name`)。 如果為 `null`，則會使用 `example_health_check`。
   * 健康狀態檢查的字串資料點 (`data1`)。
   * 健康狀態檢查的整數資料點 (`data2`)。 如果為 `null`，則會使用 `1`。
   * 失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 預設為 `null`。 如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。
   * 標籤 (`IEnumerable<string>`)。

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a>健康狀態檢查發行者

當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。 這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 初始延遲會在應用程式啟動後且執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體前套用。 在啟動後就會套用延遲，但不會套用至後續的反覆項目。 預設值是五秒鐘。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。 預設值為 30 秒。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` (預設)，健康狀態檢查發行者服務就會執行所有已註冊的健康狀態檢查。 若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。 每個期間都會評估該述詞。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體之健康狀態檢查的逾時。 若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。 預設值為 30 秒。

在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。 健康狀態檢查狀態會記錄在 `Entries` 中，並針對每項檢查進行記錄：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。 為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

## <a name="restrict-health-checks-with-mapwhen"></a>利用 MapWhen 限制健康情況檢查

使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。

在下列範例中， `MapWhen`如果收到`api/HealthCheck`端點的 GET 要求，則會將要求管線分支以啟動健康狀態檢查中介軟體：

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
});
```

如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#use-run-and-map>。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。

應用程式會將健康狀態檢查公開為 HTTP 端點。 您可以針對各種即時監控案例來設定健康狀態檢查端點：

* 容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。 例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。 負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。
* 您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。
* 健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式包含本主題中所述的案例範例。 若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。 如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。

## <a name="prerequisites"></a>必要條件

健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。 將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。 監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。

參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) 套件的套件參考。

範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。 [資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。 [DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。 為了探索資料庫案例，範例應用程式會：

* 建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。
* 在其專案檔中具有下列套件參考：
  * [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。 範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。 如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。

## <a name="basic-health-probe"></a>基本健康狀態探查

對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。

基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。 預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。 如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。 預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 在的要求處理<xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> `Startup.Configure`管線中，新增健康情況檢查中介軟體的端點。

在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：

```csharp
public class BasicStartup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks();
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseHealthChecks("/health");
    }
}
```

若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a>Docker 範例

[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>建立健康狀態檢查

健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。 結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。

### <a name="example-health-check"></a>範例健康狀態檢查

下列`ExampleHealthCheck`類別示範健全狀況檢查的版面配置。 健康情況檢查邏輯會放在`CheckHealthAsync`方法中。 下列範例會將虛擬變數`healthCheckResultHealthy`設定為。 `true` 如果的值`healthCheckResultHealthy`設定為`false`，則會傳回[HealthCheckResult](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a>登錄健康狀態檢查服務

此`ExampleHealthCheck`類型會加入至<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>中`Startup.ConfigureServices`的健康狀態檢查服務：

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。 此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。

您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。 在下列`Startup.ConfigureServices`範例中，健康情況檢查名稱指定為`Example` ，而檢查一律會傳回狀況良好狀態：

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a>使用健康狀態檢查中介軟體

在 `Startup.Configure` 中，使用端點 URL 或相對路徑呼叫處理管線中的 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>：

```csharp
app.UseHealthChecks("/health");
```

如果健康狀態檢查應該接聽特定連接埠，請使用 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 的多載來設定連接埠 ([依連接埠篩選](#filter-by-port)一節中將進一步說明)：

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a>健康狀態檢查選項

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：

* [篩選健康狀態檢查](#filter-health-checks)
* [自訂 HTTP 狀態碼](#customize-the-http-status-code)
* [隱藏快取標頭](#suppress-cache-headers)
* [自訂輸出](#customize-output)

### <a name="filter-health-checks"></a>篩選健康狀態檢查

根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。 若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。 在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () =>
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () =>
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), 
                tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a>自訂 HTTP 狀態碼

您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。 下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。 請變更狀態碼值以符合您的需求。

在 `Startup.Configure`中：

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResultStatusCodes =
    {
        [HealthStatus.Healthy] = StatusCodes.Status200OK,
        [HealthStatus.Degraded] = StatusCodes.Status200OK,
        [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
    }
});
```

### <a name="suppress-cache-headers"></a>隱藏快取標頭

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。 如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。 如果值為 `true`，則中介軟體不會修改回應的快取標頭。

在 `Startup.Configure`中：

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a>自訂輸出

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。 預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。

在 `Startup.Configure`中：

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。 下列自訂委派`WriteResponse`會輸出自訂 JSON 回應：

```csharp
private static Task WriteResponse(HttpContext httpContext, HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

健全狀況檢查系統不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。 您可以視需要自`JObject`定義上述範例中的，以符合您的需求。

## <a name="database-probe"></a>資料庫探查

健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。

範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。 `AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。

> [!WARNING]
> 使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。 查詢方法具有多載資料庫而降低其效能的風險。 在大部分情況下，不需要執行測試查詢。 只要成功建立資料庫連線就已足夠。 如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。

包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。

在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。 應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

在的應用程式處理管線中`Startup.Configure`呼叫健全狀況檢查中介軟體：

```csharp
app.UseHealthChecks("/health");
```

若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario db
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

## <a name="entity-framework-core-dbcontext-probe"></a>Entity Framework Core DbContext 探查

`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。 應用程式支援 `DbContext` 檢查：

* 使用 [Entity Framework (EF) Core](/ef/core/)。
* 包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。

`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。 `DbContext` 會以 `TContext` 形式提供給方法。 多載可用來設定失敗狀態、標籤和自訂測試查詢。

根據預設：

* `DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。 您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。
* 健康狀態檢查的名稱是 `TContext` 類型的名稱。

在範例應用程式中`AppDbContext` ，會提供`AddDbContextCheck`給，並在（*DbCoNtextHealthStartup.cs*）中`Startup.ConfigureServices`註冊為服務：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

在範例應用程式中`UseHealthChecks` ，會在中`Startup.Configure`新增健全狀況檢查中介軟體。

```csharp
app.UseHealthChecks("/health");
```

若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。 如果資料庫存在，請予以刪除。

在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario dbcontext
```

在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。 資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：

```
Unhealthy
```

觸發範例應用程式以建立資料庫。 對 `/createdatabase` 提出要求。 應用程式會回應：

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

對 `/health` 端點提出要求。 資料庫和內容存在，因此應用程式會回應：

```
Healthy
```

觸發範例應用程式以刪除資料庫。 對 `/deletedatabase` 提出要求。 應用程式會回應：

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

對 `/health` 端點提出要求。 應用程式會提供狀況不良回應：

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a>個別的整備度與活躍度探查

在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：

* 應用程式正常運作但尚未準備好接收要求。 此狀態是指應用程式的「整備度」。
* 應用程式正常運作並回應要求。 此狀態是指應用程式的「活躍度」。

整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。 活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。 當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。

範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。 `StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。 工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。 由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

在中的應用程式處理管線中`Startup.Configure`呼叫健全狀況檢查中介軟體。 在範例應用程式中，健康狀態檢查端點是在 `/health/ready` (針對整備度檢查) 和 `/health/live` (針對活躍度檢查) 建立。 整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。 活躍度檢查會藉由在 [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) 中傳回 `false` 來篩選掉 `StartupHostedServiceHealthCheck` (如需詳細資訊，請參閱[篩選健康狀態檢查](#filter-health-checks))：

```csharp
app.UseHealthChecks("/health/ready", new HealthCheckOptions()
{
    Predicate = (check) => check.Tags.Contains("ready"), 
});

app.UseHealthChecks("/health/live", new HealthCheckOptions()
{
    Predicate = (_) => false
});
```

若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario liveness
```

在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。 健康狀態檢查在前 15 秒會回報 *Unhealthy*。 15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。

此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。 如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。

### <a name="kubernetes-example"></a>Kubernetes 範例

使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。 在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。 使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。 如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。

下列範例示範 Kubernetes 整備度探查組態：

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a>透過自訂回應寫入器的計量型探查

範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。

`MemoryHealthCheck`如果應用程式使用超過指定的記憶體閾值（範例應用程式中為 1 GB），會回報狀況不良的狀態。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 `MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。 所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。 建議將健康狀態檢查服務登錄為單一服務。

在範例應用程式（*CustomWriterStartup.cs*）中：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

在中的應用程式處理管線中`Startup.Configure`呼叫健全狀況檢查中介軟體。 當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // This custom writer formats the detailed status as JSON.
        ResponseWriter = WriteResponse
    });
}
```

`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario writer
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

## <a name="filter-by-port"></a>依連接埠篩選

呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 並提供連接埠會限制對指定的連接埠提出健康狀態檢查要求。 這通常會用於容器環境，以公開監視服務的連接埠。

範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。 連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。 您也必須將伺服器設定為在管理連接埠上接聽要求。

若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。

範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 會指定管理連接埠 (*ManagementPortStartup.cs*)：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> 您可以在程式碼中明確設定 URL 和管理連接埠，避免在範例應用程式中建立 *launchSettings.json* 檔案。 在 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 建立所在的 *Program.cs* 中，新增 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> 呼叫並提供應用程式的正常回應端點和管理連接埠端點。 在 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 呼叫所在的 *ManagementPortStartup.cs* 中，明確指定管理連接埠。
>
> *Program.cs*：
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> *ManagementPortStartup.cs*：
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>發佈健康狀態檢查程式庫

若要發佈健康狀態檢查作為程式庫：

1. 寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。 此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。

   在健康情況`CheckHealthAsync`檢查的邏輯：

   * `data1`和`data2`會在方法中用來執行探查的健康情況檢查邏輯。
   * `AccessViolationException`已處理。

   當發生時<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> ，會與一起<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>傳回，讓使用者能夠設定健全狀況檢查失敗狀態。 <xref:System.AccessViolationException>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public class ExampleHealthCheck : IHealthCheck
   {
       private readonly string _data1;
       private readonly int? _data2;

       public ExampleHealthCheck(string data1, int? data2)
       {
           _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
           _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
       }

       public async Task<HealthCheckResult> CheckHealthAsync(
           HealthCheckContext context, CancellationToken cancellationToken)
       {
           try
           {
               return HealthCheckResult.Healthy();
           }
           catch (AccessViolationException ex)
           {
               return new HealthCheckResult(
                   context.Registration.FailureStatus,
                   description: "An access violation occurred during the check.",
                   exception: ex,
                   data: null);
           }
       }
   }
   ```

1. 使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。 在下列範例中，假設健康狀態檢查方法簽章如下：

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。 此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。 在下列範例中，呼叫者會選擇性地指定：

   * 健康狀態檢查名稱 (`name`)。 如果為 `null`，則會使用 `example_health_check`。
   * 健康狀態檢查的字串資料點 (`data1`)。
   * 健康狀態檢查的整數資料點 (`data2`)。 如果為 `null`，則會使用 `1`。
   * 失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 預設為 `null`。 如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。
   * 標籤 (`IEnumerable<string>`)。

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string DefaultName = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? DefaultName,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a>健康狀態檢查發行者

當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。 這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 初始延遲會在應用程式啟動後且執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體前套用。 在啟動後就會套用延遲，但不會套用至後續的反覆項目。 預設值是五秒鐘。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。 預設值為 30 秒。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` (預設)，健康狀態檢查發行者服務就會執行所有已註冊的健康狀態檢查。 若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。 每個期間都會評估該述詞。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體之健康狀態檢查的逾時。 若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。 預設值為 30 秒。

> [!WARNING]
> 在 ASP.NET Core 2.2 版中，設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>並不會獲得 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作遵守；它會設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> 的值。 ASP.NET Core 3.0 已解決此問題。

在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。 健康狀態檢查狀態會記錄在 `Entries` 中，並針對每項檢查進行記錄：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。 為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> 以下因應措施可允許在已將一或多個其他託管服務新增至應用程式的情況下，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體新增至服務容器。 ASP.NET Core 3.0 不需要此因應措施。
>
> ```csharp
> private const string HealthCheckServiceAssembly =
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService),
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。
>
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。

## <a name="restrict-health-checks-with-mapwhen"></a>利用 MapWhen 限制健康情況檢查

使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。

在下列範例中， `MapWhen`如果收到`api/HealthCheck`端點的 GET 要求，則會將要求管線分支以啟動健康狀態檢查中介軟體：

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#use-run-and-map>。

::: moniker-end
