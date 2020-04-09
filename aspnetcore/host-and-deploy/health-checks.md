---
title: ASP.NET Core 中的健康狀態檢查
author: rick-anderson
description: 了解如何為 ASP.NET Core 基礎結構 (例如應用程式和資料庫) 設定健康狀態檢查。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 314e55c818cddf1dad2e3ec74d4d1e041ce7366f
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664882"
---
# <a name="health-checks-in-aspnet-core"></a>ASP.NET Core 中的健康狀態檢查

由[葛籣·康得龍](https://github.com/glennc)

::: moniker range=">= aspnetcore-3.0"

ASP.NET核心提供運行狀況檢查中間件和庫,用於報告應用基礎結構元件的運行狀況。

應用程式會將健康狀態檢查公開為 HTTP 端點。 您可以針對各種即時監控案例來設定健康狀態檢查端點：

* 容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。 例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。 負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。
* 您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。
* 健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples)([如何下載](xref:index#how-to-download-a-sample))

範例應用程式包含本主題中所述的案例範例。 若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。 如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。

## <a name="prerequisites"></a>Prerequisites

健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。 將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。 監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。

[Microsoft.AspNetCore.診斷.運行狀況檢查](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks)包被隱式引用ASP.NET核心應用。 要使用實體框架核心執行運行狀況檢查,請使用包引用到[Microsoft.擴展.診斷.運行狀況檢查.實體框架核心](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore)包。

範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。 [資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。 [DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。 為了探索資料庫案例，範例應用程式會：

* 建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。
* 在其專案檔中具有下列套件參考：
  * [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。 範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。 如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。

## <a name="basic-health-probe"></a>基本健康狀態探查

對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」**)，便足以探索應用程式的狀態。

基本設定註冊運行狀況檢查服務並調用運行狀況檢查中間件以回應具有運行狀況回應的 URL 終結點。 預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。 如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。 預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 以呼叫`MapHealthChecks`建立執行狀況檢查終結`Startup.Configure`點 。

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

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a>Docker 範例

[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a>建立健康狀態檢查

健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。 結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。

以下`ExampleHealthCheck`類演示運行狀況檢查的佈局。 執行狀況檢查邏輯放置在 方法`CheckHealthAsync`中 。 下面的範例設定一個虛擬變數,`healthCheckResultHealthy`到`true`。 如果的值`healthCheckResultHealthy`設置`false`為 ,則返回[運行狀況檢查結果。不正常](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態將返回。

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

該`ExampleHealthCheck`類型將添加到 運行狀況檢查<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>`Startup.ConfigureServices`服務中 ,

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。 此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。

您可以使用「標籤」** 來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。

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

調用<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*>將參數傳遞給運行狀況檢查實現。 在下面的範例中,`TestHealthCheckWithArgs`接受整數和一個字串,用於在<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*>呼叫時:

```csharp
private class TestHealthCheckWithArgs : IHealthCheck
{
    public TestHealthCheckWithArgs(int i, string s)
    {
        I = i;
        S = s;
    }

    public int I { get; set; }

    public string S { get; set; }

    public Task<HealthCheckResult> CheckHealthAsync(HealthCheckContext context, 
        CancellationToken cancellationToken = default)
    {
        ...
    }
}
```

`TestHealthCheckWithArgs`以呼叫`AddTypeActivatedCheck`傳遞給實現的整數和字串進行註冊:

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a>使用執行狀況檢查路由

在`Startup.Configure`中`MapHealthChecks`,使用終結點網址 或相對路徑呼叫終結點產生器:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a>需要主機

呼叫`RequireHost`以指定運行狀況檢查終結點的一個或多個允許的主機。 主機應為 Unicode 而不是小代碼,並且可能包括埠。 如果未提供集合,則接受任何主機。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。

### <a name="require-authorization"></a>需要授權

調用`RequireAuthorization`在運行狀況檢查請求終結點上運行授權中間件。 `RequireAuthorization`重載接受一個或多個授權策略。 如果未提供策略,則使用默認授權策略。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a>啟用跨原始來源要求 (CORS)

儘管從瀏覽器手動執行運行狀況檢查不是常見使用方案,但可以通過調`RequireCors`用 運行狀況檢查終結點啟用 CORS 中間件。 `RequireCors`重載接受 CORS 策略產生`CorsPolicyBuilder`器委託 ( ) 或策略名稱。 如果未提供策略,則使用預設的 CORS 策略。 如需詳細資訊，請參閱 <xref:security/cors>。

## <a name="health-check-options"></a>健康狀態檢查選項

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：

* [篩選健康狀態檢查](#filter-health-checks)
* [自訂 HTTP 狀態碼](#customize-the-http-status-code)
* [隱藏快取標頭](#suppress-cache-headers)
* [自訂輸出](#customize-output)

### <a name="filter-health-checks"></a>篩選健康狀態檢查

默認情況下,運行狀況檢查中間件運行所有已註冊的運行狀況檢查。 若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。 在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：

在 `Startup.ConfigureServices` 中：

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

在`Startup.Configure``Predicate`中 ,篩選出「Bar」運行狀況檢查。 只有 Foo 和 Baz 執行:

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

在 `Startup.Configure` 中：

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

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制執行狀況檢查中間件是否向探測器回應添加 HTTP 標頭以防止回應緩存。 如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。 如果值為 `true`，則中介軟體不會修改回應的快取標頭。

在 `Startup.Configure` 中：

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

在`Startup.Configure`中,將[執行狀況檢查選項.回應Writer](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter)選項設定為用於編寫回應的委託:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。 以下自定義委託輸出自定義 JSON 回應。

範例使用一個範例的如何使用<xref:System.Text.Json?displayProperty=fullName>:

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_SystemTextJson)]

第二個示例演示如何使用[牛頓軟。](https://www.nuget.org/packages/Newtonsoft.Json/)

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_NewtonSoftJson)]

在`SYSTEM_TEXT_JSON`範例應用程式中,註釋掉*CustomWriterStartup.cs*的[預處理器指令](xref:index#preprocessor-directives-in-sample-code),以啟用`Newtonsoft.Json`的版本`WriteResponse`。

運行狀況檢查 API 不為複雜的 JSON 傳回格式提供內建支援,因為該格式特定於您選擇的監控系統。 根據需要自定義上述示例中的回應。 有關`System.Text.Json`使用的 JSON 序列化的詳細資訊,請參閱[如何在 .NET 中序列化和反序列化 JSON。](/dotnet/standard/serialization/system-text-json-how-to)

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

以呼叫`MapHealthChecks`建立執行狀況檢查終結`Startup.Configure`點 :

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

## <a name="entity-framework-core-dbcontext-probe"></a>Entity Framework Core DbContext 探查

`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。 應用程式支援 `DbContext` 檢查：

* 使用 [Entity Framework (EF) Core](/ef/core/)。
* 包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。

`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。 `DbContext` 會以 `TContext` 形式提供給方法。 多載可用來設定失敗狀態、標籤和自訂測試查詢。

依照預設：

* `DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。 您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。
* 健康狀態檢查的名稱是 `TContext` 類型的名稱。

在示例應用中`AppDbContext`,提供`AddDbContextCheck``Startup.ConfigureServices`給 *(DbContextHealthStartup.cs*) 中的服務並註冊為服務。

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

以呼叫`MapHealthChecks`建立執行狀況檢查終結`Startup.Configure`點 :

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。 如果資料庫存在，請予以刪除。

在命令殼層中執行來自專案資料夾的下列命令：

```dotnetcli
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

* 應用程式正常運作但尚未準備好接收要求。 此狀態是指應用程式的「整備度」**。
* 應用程式正常運作並回應要求。 此狀態是指應用程式的「活躍度」**。

整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。 活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。 當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。

範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。 `StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。 工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。 由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

以呼叫`MapHealthChecks`建立執行狀況檢查終結`Startup.Configure`點 。 在範例應用中,執行狀況檢查終結點創建於:

* `/health/ready`用於準備檢查。 整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。
* `/health/live`用於活度檢查。 活動性`StartupHostedServiceHealthCheck``false`檢查 通過在[執行狀況檢查選項](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate)傳回來篩選出(有關詳細資訊,請參閱[篩選器執行狀況檢查](#filter-health-checks))

在以下範例代碼中:

* 就緒檢查使用帶有"就緒"標記的所有已註冊檢查。
* `Predicate`排除所有檢查並返回 200-Ok。

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

```dotnetcli
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

在範例應用程式*CustomWriterStartup.cs:*

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

以呼叫`MapHealthChecks`建立執行狀況檢查終結`Startup.Configure`點 。 向`WriteResponse`microsoft <提供委託.AspNetCore.診斷.運行狀況檢查.運行狀況檢查選項.回應Writer>屬性,用於在運行狀況檢查執行時輸出自定義 JSON 回應:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

委託`WriteResponse`將格式化`CompositeHealthCheckResult`為 JSON 物件,並為運行狀況檢查回應生成 JSON 輸出。 有關詳細資訊,請參閱[自定義輸出](#customize-output)部分。

若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。
>
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

## <a name="filter-by-port"></a>依連接埠篩選

使用`RequireHost``MapHealthChecks`URL 模式呼叫,該模式指定埠以將運行狀況檢查請求限制為指定的埠。 這通常會用於容器環境，以公開監視服務的連接埠。

範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。 連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。 您也必須將伺服器設定為在管理連接埠上接聽要求。

若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。

範例應用程式中的以下*屬性/啟動設定.json*檔不包括在範例應用程式的項目檔中,必須手動建立:

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

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 以呼叫`MapHealthChecks`建立執行狀況檢查終結`Startup.Configure`點 。

在範例應用中,對`RequireHost`終結點的呼叫`Startup.Configure`指定來自配置的管理埠:

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

終結點在`Startup.Configure`中 的範例應用中創建。 在以下範例代碼中:

* 就緒檢查使用帶有"就緒"標記的所有已註冊檢查。
* `Predicate`排除所有檢查並返回 200-Ok。

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
> 通過在代碼中顯式設置管理埠,可以避免在示例應用中創建*啟動設置.json*檔。 在*創建*Program.cs<xref:Microsoft.Extensions.Hosting.HostBuilder>中,向應用<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*>添加調用並提供應用的管理埠終結點。 在`Configure` *ManagementPortStartup.cs*中,`RequireHost`使用指定管理埠:
>
> *Program.cs*:
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

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>發佈健康狀態檢查程式庫

若要發佈健康狀態檢查作為程式庫：

1. 寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。 此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。

   執行狀況檢查邏輯中`CheckHealthAsync`:

   * `data1`並`data2`用於運行探測器的運行狀況檢查邏輯的方法。
   * `AccessViolationException`已處理。

   發生時<xref:System.AccessViolationException>,<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus>將傳<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>回 , 以允許使用者設定執行狀況檢查失敗狀態。

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
   * 失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 預設值為 `null`。 如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。
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

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>&ndash;應用在執行<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>實例之前啟動后應用的初始延遲。 在啟動後就會套用延遲，但不會套用至後續的反覆項目。 預設值是五秒鐘。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash;執行期間<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>。 預設值為 30 秒。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>&ndash;如果<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>`null`為 (預設值),運行狀況檢查發佈者服務將運行所有已註冊的運行狀況檢查。 若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。 每個期間都會評估該述詞。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>&ndash;執行所有<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>實例的運行狀況檢查的超時。 若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。 預設值為 30 秒。

在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。 紀錄每次檢查的執行狀況檢查狀態,紀錄等級為:

* 如果執行狀況<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>檢查狀態<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>為, 則資訊 ( )
* 如果狀態<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>為<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>或, 則發生錯誤 (

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。 為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。
>
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

## <a name="restrict-health-checks-with-mapwhen"></a>利用 MapWhen 限制健康情況檢查

使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。

在以下範例中,`MapWhen`如果`api/HealthCheck`收到 終結點的 GET 請求,則分支請求管道以啟動執行狀況檢查中間件:

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

ASP.NET核心提供運行狀況檢查中間件和庫,用於報告應用基礎結構元件的運行狀況。

應用程式會將健康狀態檢查公開為 HTTP 端點。 您可以針對各種即時監控案例來設定健康狀態檢查端點：

* 容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。 例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。 負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。
* 您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。
* 健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples)([如何下載](xref:index#how-to-download-a-sample))

範例應用程式包含本主題中所述的案例範例。 若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。 如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。

## <a name="prerequisites"></a>Prerequisites

健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。 將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。 監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。

參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) 套件的套件參考。

範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。 [資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。 [DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。 為了探索資料庫案例，範例應用程式會：

* 建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。
* 在其專案檔中具有下列套件參考：
  * [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。 範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。 如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。

## <a name="basic-health-probe"></a>基本健康狀態探查

對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」**)，便足以探索應用程式的狀態。

基本設定註冊運行狀況檢查服務並調用運行狀況檢查中間件以回應具有運行狀況回應的 URL 終結點。 預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。 如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。 預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 在 請求處理<xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>管道 中添加運行狀況檢查中間`Startup.Configure`件的 終結點。

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

```dotnetcli
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

以下`ExampleHealthCheck`類演示運行狀況檢查的佈局。 執行狀況檢查邏輯放置在 方法`CheckHealthAsync`中 。 下面的範例設定一個虛擬變數,`healthCheckResultHealthy`到`true`。 如果的值`healthCheckResultHealthy`設置`false`為 ,則返回[運行狀況檢查結果。不正常](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態將返回。

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

這個`ExampleHealthCheck`型態新增到中的執行狀況檢查服務`Startup.ConfigureServices`中<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。 此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。

您可以使用「標籤」** 來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。 在下面的`Startup.ConfigureServices`範例中,執行狀況檢查名稱指定`Example`為 ,檢查始終返回正常狀態:

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

默認情況下,運行狀況檢查中間件運行所有已註冊的運行狀況檢查。 若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。 在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：

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

在 `Startup.Configure` 中：

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

<xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制執行狀況檢查中間件是否向探測器回應添加 HTTP 標頭以防止回應緩存。 如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。 如果值為 `true`，則中介軟體不會修改回應的快取標頭。

在 `Startup.Configure` 中：

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

在 `Startup.Configure` 中：

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。 以下自訂的`WriteResponse`委託, 輸出自訂 JSON 回應:

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

運行狀況檢查系統不提供對複雜 JSON 返回格式的內置支援,因為該格式特定於您選擇的監控系統。 根據需要,請根據需要自定義`JObject`上述示例中的。

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

在使用的處理導管中呼叫執行狀況檢查`Startup.Configure`中間 元件:

```csharp
app.UseHealthChecks("/health");
```

若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

## <a name="entity-framework-core-dbcontext-probe"></a>Entity Framework Core DbContext 探查

`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。 應用程式支援 `DbContext` 檢查：

* 使用 [Entity Framework (EF) Core](/ef/core/)。
* 包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。

`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。 `DbContext` 會以 `TContext` 形式提供給方法。 多載可用來設定失敗狀態、標籤和自訂測試查詢。

依照預設：

* `DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。 您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。
* 健康狀態檢查的名稱是 `TContext` 類型的名稱。

在示例應用中`AppDbContext`,提供`AddDbContextCheck``Startup.ConfigureServices`給 *(DbContextHealthStartup.cs*) 中的服務並註冊為服務。

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

在範例應用中,`UseHealthChecks`在`Startup.Configure`中 添加運行狀況檢查中間件。

```csharp
app.UseHealthChecks("/health");
```

若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。 如果資料庫存在，請予以刪除。

在命令殼層中執行來自專案資料夾的下列命令：

```dotnetcli
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

* 應用程式正常運作但尚未準備好接收要求。 此狀態是指應用程式的「整備度」**。
* 應用程式正常運作並回應要求。 此狀態是指應用程式的「活躍度」**。

整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。 活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。 當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。

範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。 `StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。 工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。 由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

在中的應用程式處理管道中調用運行狀況檢查中間`Startup.Configure`件。 在範例應用程式中，健康狀態檢查端點是在 `/health/ready` (針對整備度檢查) 和 `/health/live` (針對活躍度檢查) 建立。 整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。 活躍度檢查會藉由在 [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) 中傳回 `false` 來篩選掉 `StartupHostedServiceHealthCheck` (如需詳細資訊，請參閱[篩選健康狀態檢查](#filter-health-checks))：

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

```dotnetcli
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

`MemoryHealthCheck`如果應用使用超過給定記憶體閾值(示例應用中為1 GB),則報告不正常狀態。 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。 `MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。 所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。 建議將健康狀態檢查服務登錄為單一服務。

在範例應用程式中 *(CustomWriterStartup.cs*):

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

在中的應用程式處理管道中調用運行狀況檢查中間`Startup.Configure`件。 當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：

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

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。
>
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

## <a name="filter-by-port"></a>依連接埠篩選

呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 並提供連接埠會限制對指定的連接埠提出健康狀態檢查要求。 這通常會用於容器環境，以公開監視服務的連接埠。

範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。 連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。 您也必須將伺服器設定為在管理連接埠上接聽要求。

若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。

範例應用程式中的以下*屬性/啟動設定.json*檔不包括在範例應用程式的項目檔中,必須手動建立:

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
> *Program.cs*:
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

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a>發佈健康狀態檢查程式庫

若要發佈健康狀態檢查作為程式庫：

1. 寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。 此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。

   執行狀況檢查邏輯中`CheckHealthAsync`:

   * `data1`並`data2`用於運行探測器的運行狀況檢查邏輯的方法。
   * `AccessViolationException`已處理。

   發生時<xref:System.AccessViolationException>,<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus>將傳<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>回 , 以允許使用者設定執行狀況檢查失敗狀態。

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
   * 失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。 預設值為 `null`。 如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。
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

* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>&ndash;應用在執行<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>實例之前啟動后應用的初始延遲。 在啟動後就會套用延遲，但不會套用至後續的反覆項目。 預設值是五秒鐘。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>&ndash;執行期間<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>。 預設值為 30 秒。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>&ndash;如果<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>`null`為 (預設值),運行狀況檢查發佈者服務將運行所有已註冊的運行狀況檢查。 若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。 每個期間都會評估該述詞。
* <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>&ndash;執行所有<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>實例的運行狀況檢查的超時。 若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。 預設值為 30 秒。

> [!WARNING]
> 在 ASP.NET Core 2.2 版中，設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>並不會獲得 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作遵守；它會設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> 的值。 此問題已在ASP.NET核心3.0中得到解決。

在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。 紀錄每次檢查的運行狀況檢查狀態,作為以下任一檢查:

* 如果執行狀況<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>檢查狀態<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>為, 則資訊 ( )
* 如果狀態<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>為<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>或, 則發生錯誤 (

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。 為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> 以下因應措施可允許在已將一或多個其他託管服務新增至應用程式的情況下，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體新增至服務容器。 在 ASP.NET 酷 3.0 中不需要此解決方法。
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
> [AspNetCore.診斷.健康檢查](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不受微軟的維護或支援。

## <a name="restrict-health-checks-with-mapwhen"></a>利用 MapWhen 限制健康情況檢查

使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。

在以下範例中,`MapWhen`如果`api/HealthCheck`收到 終結點的 GET 請求,則分支請求管道以啟動執行狀況檢查中間件:

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#use-run-and-map>。

::: moniker-end
