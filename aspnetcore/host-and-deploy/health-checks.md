---
<span data-ttu-id="1fc86-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1fc86-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1fc86-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1fc86-102">'Blazor'</span></span>
- <span data-ttu-id="1fc86-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1fc86-103">'Identity'</span></span>
- <span data-ttu-id="1fc86-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1fc86-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="1fc86-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1fc86-105">'Razor'</span></span>
- <span data-ttu-id="1fc86-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1fc86-106">'SignalR' uid:</span></span> 

---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="1fc86-107">ASP.NET Core 中的健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-107">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="1fc86-108">依[Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="1fc86-108">By [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1fc86-109">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="1fc86-109">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="1fc86-110">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-110">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="1fc86-111">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="1fc86-111">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="1fc86-112">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-112">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="1fc86-113">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-113">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="1fc86-114">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-114">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="1fc86-115">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-115">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="1fc86-116">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-116">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="1fc86-117">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1fc86-117">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1fc86-118">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="1fc86-118">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="1fc86-119">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="1fc86-119">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="1fc86-120">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="1fc86-120">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fc86-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="1fc86-121">Prerequisites</span></span>

<span data-ttu-id="1fc86-122">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-122">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="1fc86-123">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="1fc86-123">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="1fc86-124">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-124">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="1fc86-125">ASP.NET Core 應用程式會以隱含方式參考[HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks)套件。</span><span class="sxs-lookup"><span data-stu-id="1fc86-125">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="1fc86-126">若要使用 Entity Framework Core 執行健康情況檢查，請將套件參考新增至[HealthChecks. microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore)套件。</span><span class="sxs-lookup"><span data-stu-id="1fc86-126">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="1fc86-127">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-127">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="1fc86-128">[資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。</span><span class="sxs-lookup"><span data-stu-id="1fc86-128">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="1fc86-129">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-129">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="1fc86-130">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="1fc86-130">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="1fc86-131">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="1fc86-131">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="1fc86-132">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="1fc86-132">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="1fc86-133">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="1fc86-133">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="1fc86-134">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1fc86-134">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="1fc86-135">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-135">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="1fc86-136">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-136">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="1fc86-137">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-137">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="1fc86-138">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="1fc86-138">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="1fc86-139">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-139">Basic health probe</span></span>

<span data-ttu-id="1fc86-140">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」\*\*)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-140">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="1fc86-141">基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-141">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="1fc86-142">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="1fc86-142">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="1fc86-143">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="1fc86-143">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="1fc86-144">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-144">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="1fc86-145">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-145">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-146">在中呼叫，以建立健康狀態檢查端點 `MapHealthChecks` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-146">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="1fc86-147">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="1fc86-147">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="1fc86-148">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-148">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="1fc86-149">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="1fc86-149">Docker example</span></span>

<span data-ttu-id="1fc86-150">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-150">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="1fc86-151">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-151">Create health checks</span></span>

<span data-ttu-id="1fc86-152">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="1fc86-152">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="1fc86-153"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-153">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="1fc86-154">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-154">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="1fc86-155"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="1fc86-155"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="1fc86-156">下列 `ExampleHealthCheck` 類別示範健全狀況檢查的版面配置。</span><span class="sxs-lookup"><span data-stu-id="1fc86-156">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="1fc86-157">健康情況檢查邏輯會放在 `CheckHealthAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="1fc86-157">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="1fc86-158">下列範例會將虛擬變數設定 `healthCheckResultHealthy` 為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-158">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="1fc86-159">如果的值 `healthCheckResultHealthy` 設定為，則 `false` 會傳回[HealthCheckResult](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-159">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

## <a name="register-health-check-services"></a><span data-ttu-id="1fc86-160">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="1fc86-160">Register health check services</span></span>

<span data-ttu-id="1fc86-161">`ExampleHealthCheck`類型會加入至中的健康狀態檢查 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 服務 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-161">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="1fc86-162">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-162">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="1fc86-163">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-163">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="1fc86-164">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-164">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="1fc86-165">您可以使用「標籤」\*\* 來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-165">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="1fc86-166"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-166"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="1fc86-167">在下列範例中，健康狀態檢查名稱指定為 `Example`，且檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-167">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

<span data-ttu-id="1fc86-168">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> 以將引數傳遞至健康情況檢查的執行。</span><span class="sxs-lookup"><span data-stu-id="1fc86-168">Call <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> to pass arguments to a health check implementation.</span></span> <span data-ttu-id="1fc86-169">在下列範例中， `TestHealthCheckWithArgs` 會接受整數和字串，以便在呼叫時使用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-169">In the following example, `TestHealthCheckWithArgs` accepts an integer and a string for use when <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> is called:</span></span>

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

<span data-ttu-id="1fc86-170">`TestHealthCheckWithArgs`會藉由呼叫並 `AddTypeActivatedCheck` 傳遞至執行的整數和字串來進行註冊：</span><span class="sxs-lookup"><span data-stu-id="1fc86-170">`TestHealthCheckWithArgs` is registered by calling `AddTypeActivatedCheck` with the integer and string passed to the implementation:</span></span>

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="1fc86-171">使用健全狀況檢查路由</span><span class="sxs-lookup"><span data-stu-id="1fc86-171">Use Health Checks Routing</span></span>

<span data-ttu-id="1fc86-172">在中 `Startup.Configure` ， `MapHealthChecks` 使用端點 URL 或相對路徑在端點產生器上呼叫：</span><span class="sxs-lookup"><span data-stu-id="1fc86-172">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="1fc86-173">需要主機</span><span class="sxs-lookup"><span data-stu-id="1fc86-173">Require host</span></span>

<span data-ttu-id="1fc86-174">呼叫 `RequireHost` 以指定一或多個允許的主機用於健康情況檢查端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-174">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="1fc86-175">主機應該是 Unicode 而不是 punycode，而且可能包含埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-175">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="1fc86-176">如果未提供集合，則會接受任何主機。</span><span class="sxs-lookup"><span data-stu-id="1fc86-176">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="1fc86-177">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="1fc86-177">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="1fc86-178">需要授權</span><span class="sxs-lookup"><span data-stu-id="1fc86-178">Require authorization</span></span>

<span data-ttu-id="1fc86-179">呼叫 `RequireAuthorization` 以在健康情況檢查要求端點上執行授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1fc86-179">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="1fc86-180">多載會 `RequireAuthorization` 接受一或多個授權原則。</span><span class="sxs-lookup"><span data-stu-id="1fc86-180">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="1fc86-181">如果未提供原則，則會使用預設的授權原則。</span><span class="sxs-lookup"><span data-stu-id="1fc86-181">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="1fc86-182">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="1fc86-182">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="1fc86-183">雖然從瀏覽器手動執行健康情況檢查並不是常見的使用案例，但您可以呼叫 `RequireCors` 健全狀況檢查端點來啟用 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1fc86-183">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="1fc86-184">多載會 `RequireCors` 接受 CORS 原則產生器委派（ `CorsPolicyBuilder` ）或原則名稱。</span><span class="sxs-lookup"><span data-stu-id="1fc86-184">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="1fc86-185">如果未提供原則，則會使用預設的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="1fc86-185">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="1fc86-186">如需詳細資訊，請參閱<xref:security/cors>。</span><span class="sxs-lookup"><span data-stu-id="1fc86-186">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="1fc86-187">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="1fc86-187">Health check options</span></span>

<span data-ttu-id="1fc86-188"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="1fc86-188"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="1fc86-189">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-189">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="1fc86-190">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="1fc86-190">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="1fc86-191">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="1fc86-191">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="1fc86-192">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="1fc86-192">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="1fc86-193">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-193">Filter health checks</span></span>

<span data-ttu-id="1fc86-194">根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-194">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="1fc86-195">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-195">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="1fc86-196">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="1fc86-196">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="1fc86-197">在 `Startup.ConfigureServices` 中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-197">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="1fc86-198">在中 `Startup.Configure` ，會 `Predicate` 篩選出 ' Bar ' 健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-198">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="1fc86-199">只有 Foo 和 Baz.png execute。：</span><span class="sxs-lookup"><span data-stu-id="1fc86-199">Only Foo and Baz execute.:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="1fc86-200">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="1fc86-200">Customize the HTTP status code</span></span>

<span data-ttu-id="1fc86-201">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-201">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="1fc86-202">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="1fc86-202">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="1fc86-203">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-203">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="1fc86-204">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-204">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="1fc86-205">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="1fc86-205">Suppress cache headers</span></span>

<span data-ttu-id="1fc86-206"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="1fc86-206"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="1fc86-207">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="1fc86-207">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="1fc86-208">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="1fc86-208">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="1fc86-209">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-209">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="1fc86-210">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="1fc86-210">Customize output</span></span>

<span data-ttu-id="1fc86-211">在中 `Startup.Configure` ，將[HealthCheckOptions. ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter)選項設定為用於寫入回應的委派：</span><span class="sxs-lookup"><span data-stu-id="1fc86-211">In `Startup.Configure`, set the [HealthCheckOptions.ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter) option to a delegate for writing the response:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="1fc86-212">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-212">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="1fc86-213">下列自訂委派會輸出自訂 JSON 回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-213">The following custom delegates output a custom JSON response.</span></span>

<span data-ttu-id="1fc86-214">範例應用程式的第一個範例示範如何使用 <xref:System.Text.Json?displayProperty=fullName> ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-214">The first example from the sample app demonstrates how to use <xref:System.Text.Json?displayProperty=fullName>:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_SystemTextJson)]

<span data-ttu-id="1fc86-215">第二個範例示範如何使用[Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-215">The second example demonstrates how to use [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_NewtonSoftJson)]

<span data-ttu-id="1fc86-216">在範例應用程式中，批註 `SYSTEM_TEXT_JSON` *CustomWriterStartup.cs*中的[預處理器](xref:index#preprocessor-directives-in-sample-code)指示詞，以啟用的 `Newtonsoft.Json` 版本 `WriteResponse` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-216">In the sample app, comment out the `SYSTEM_TEXT_JSON` [preprocessor directive](xref:index#preprocessor-directives-in-sample-code) in *CustomWriterStartup.cs* to enable the `Newtonsoft.Json` version of `WriteResponse`.</span></span>

<span data-ttu-id="1fc86-217">健康情況檢查 API 不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。</span><span class="sxs-lookup"><span data-stu-id="1fc86-217">The health checks API doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="1fc86-218">視需要自訂上述範例中的回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-218">Customize the response in the preceding examples as needed.</span></span> <span data-ttu-id="1fc86-219">如需有關使用 JSON 序列化的詳細資訊 `System.Text.Json` ，請參閱[如何在 .net 中序列化和](/dotnet/standard/serialization/system-text-json-how-to)還原序列化 json。</span><span class="sxs-lookup"><span data-stu-id="1fc86-219">For more information on JSON serialization with `System.Text.Json`, see [How to serialize and deserialize JSON in .NET](/dotnet/standard/serialization/system-text-json-how-to).</span></span>

## <a name="database-probe"></a><span data-ttu-id="1fc86-220">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-220">Database probe</span></span>

<span data-ttu-id="1fc86-221">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-221">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="1fc86-222">範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-222">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="1fc86-223">`AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="1fc86-223">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="1fc86-224">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="1fc86-224">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="1fc86-225">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="1fc86-225">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="1fc86-226">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="1fc86-226">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="1fc86-227">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-227">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="1fc86-228">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-228">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="1fc86-229">包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1fc86-229">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="1fc86-230">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="1fc86-230">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="1fc86-231">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="1fc86-231">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="1fc86-232">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-232">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-233">範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="1fc86-233">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1fc86-234">健康狀態檢查端點是藉由呼叫中的來建立 `MapHealthChecks` `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-234">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="1fc86-235">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-235">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="1fc86-236">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-236">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="1fc86-237">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-237">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="1fc86-238">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="1fc86-238">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="1fc86-239">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="1fc86-239">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="1fc86-240">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-240">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="1fc86-241">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1fc86-241">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="1fc86-242">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-242">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="1fc86-243">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="1fc86-243">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="1fc86-244">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="1fc86-244">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="1fc86-245">依照預設：</span><span class="sxs-lookup"><span data-stu-id="1fc86-245">By default:</span></span>

* <span data-ttu-id="1fc86-246">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="1fc86-246">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="1fc86-247">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="1fc86-247">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="1fc86-248">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="1fc86-248">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="1fc86-249">在範例應用程式中， `AppDbContext` 會提供給， `AddDbContextCheck` 並在 `Startup.ConfigureServices` （*DbCoNtextHealthStartup.cs*）中註冊為服務：</span><span class="sxs-lookup"><span data-stu-id="1fc86-249">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1fc86-250">健康狀態檢查端點是藉由呼叫中的來建立 `MapHealthChecks` `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-250">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="1fc86-251">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-251">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="1fc86-252">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="1fc86-252">If the database exists, delete it.</span></span>

<span data-ttu-id="1fc86-253">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-253">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="1fc86-254">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-254">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="1fc86-255">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-255">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="1fc86-256">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-256">Trigger the sample app to create the database.</span></span> <span data-ttu-id="1fc86-257">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-257">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="1fc86-258">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-258">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1fc86-259">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-259">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1fc86-260">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-260">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="1fc86-261">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-261">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="1fc86-262">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-262">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="1fc86-263">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-263">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1fc86-264">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-264">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1fc86-265">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-265">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="1fc86-266">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-266">Separate readiness and liveness probes</span></span>

<span data-ttu-id="1fc86-267">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-267">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="1fc86-268">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-268">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="1fc86-269">此狀態是指應用程式的「整備度」\*\*。</span><span class="sxs-lookup"><span data-stu-id="1fc86-269">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="1fc86-270">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-270">The app is functioning and responding to requests.</span></span> <span data-ttu-id="1fc86-271">此狀態是指應用程式的「活躍度」\*\*。</span><span class="sxs-lookup"><span data-stu-id="1fc86-271">This state is the app's *liveness*.</span></span>

<span data-ttu-id="1fc86-272">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-272">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="1fc86-273">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-273">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="1fc86-274">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="1fc86-274">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="1fc86-275">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="1fc86-275">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="1fc86-276">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-276">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="1fc86-277">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-277">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="1fc86-278">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="1fc86-278">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="1fc86-279">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="1fc86-279">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="1fc86-280">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="1fc86-280">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1fc86-281">健康情況檢查端點是藉由呼叫中的來建立 `MapHealthChecks` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-281">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="1fc86-282">在範例應用程式中，健康狀態檢查端點會建立于：</span><span class="sxs-lookup"><span data-stu-id="1fc86-282">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="1fc86-283">`/health/ready`以進行準備檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-283">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="1fc86-284">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-284">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="1fc86-285">`/health/live`做為活動檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-285">`/health/live` for the liveness check.</span></span> <span data-ttu-id="1fc86-286">活動檢查會藉 `StartupHostedServiceHealthCheck` 由 `false` 在[HealthCheckOptions](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate)中傳回來篩選掉（如需詳細資訊，請參閱[篩選健全狀況檢查](#filter-health-checks)）</span><span class="sxs-lookup"><span data-stu-id="1fc86-286">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="1fc86-287">在下列範例程式碼中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-287">In the following example code:</span></span>

* <span data-ttu-id="1fc86-288">準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-288">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="1fc86-289">會 `Predicate` 排除所有檢查並傳回 200-Ok。</span><span class="sxs-lookup"><span data-stu-id="1fc86-289">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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

<span data-ttu-id="1fc86-290">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-290">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="1fc86-291">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="1fc86-291">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="1fc86-292">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="1fc86-292">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="1fc86-293">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="1fc86-293">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="1fc86-294">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-294">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="1fc86-295">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="1fc86-295">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="1fc86-296">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="1fc86-296">Kubernetes example</span></span>

<span data-ttu-id="1fc86-297">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-297">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="1fc86-298">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-298">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="1fc86-299">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="1fc86-299">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="1fc86-300">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-300">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="1fc86-301">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-301">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```yml
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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="1fc86-302">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-302">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="1fc86-303">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-303">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="1fc86-304">如果應用程式使用超過指定的記憶體閾值 (在範例應用程式中為 1 GB)，`MemoryHealthCheck` 會報告降級的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-304">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="1fc86-305"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-305">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="1fc86-306">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-306">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-307">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-307">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="1fc86-308">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-308">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="1fc86-309">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-309">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="1fc86-310">在範例應用程式的*CustomWriterStartup.cs*中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-310">In *CustomWriterStartup.cs* of the sample app:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="1fc86-311">健康情況檢查端點是藉由呼叫中的來建立 `MapHealthChecks` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-311">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="1fc86-312">在 `WriteResponse` 執行健康狀態檢查時，會將委派提供給 <的 HealthCheckOptions> 屬性，以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-312">A `WriteResponse` delegate is provided to the <Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="1fc86-313">委派會將 `WriteResponse` 格式化為 `CompositeHealthCheckResult` json 物件，並產生健康情況檢查回應的 JSON 輸出。</span><span class="sxs-lookup"><span data-stu-id="1fc86-313">The `WriteResponse` delegate formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response.</span></span> <span data-ttu-id="1fc86-314">如需詳細資訊，請參閱[自訂輸出](#customize-output)一節。</span><span class="sxs-lookup"><span data-stu-id="1fc86-314">For more information, see the [Customize output](#customize-output) section.</span></span>

<span data-ttu-id="1fc86-315">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-315">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="1fc86-316">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-316">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="1fc86-317">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-317">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="1fc86-318">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="1fc86-318">Filter by port</span></span>

<span data-ttu-id="1fc86-319">`RequireHost` `MapHealthChecks` 使用指定埠的 URL 模式來呼叫，以限制對指定埠的健康狀態檢查要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-319">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="1fc86-320">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-320">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="1fc86-321">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-321">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1fc86-322">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="1fc86-322">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="1fc86-323">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-323">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="1fc86-324">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1fc86-324">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="1fc86-325">範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：</span><span class="sxs-lookup"><span data-stu-id="1fc86-325">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="1fc86-326">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-326">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-327">在中呼叫，以建立健康狀態檢查端點 `MapHealthChecks` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-327">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="1fc86-328">在範例應用程式中，對 `RequireHost` 上端點的呼叫會 `Startup.Configure` 從設定中指定管理埠：</span><span class="sxs-lookup"><span data-stu-id="1fc86-328">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="1fc86-329">端點會在的範例應用程式中建立 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-329">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="1fc86-330">在下列範例程式碼中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-330">In the following example code:</span></span>

* <span data-ttu-id="1fc86-331">準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-331">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="1fc86-332">會 `Predicate` 排除所有檢查並傳回 200-Ok。</span><span class="sxs-lookup"><span data-stu-id="1fc86-332">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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
> <span data-ttu-id="1fc86-333">您可以在程式碼中明確設定管理埠，以避免在範例應用程式中建立*launchsettings.json。*</span><span class="sxs-lookup"><span data-stu-id="1fc86-333">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="1fc86-334">在建立的*Program.cs*中 <xref:Microsoft.Extensions.Hosting.HostBuilder> ，新增對的呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> ，並提供應用程式的管理埠端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-334">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="1fc86-335">在 `Configure` *ManagementPortStartup.cs*的中，使用指定管理埠 `RequireHost` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-335">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="1fc86-336">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="1fc86-336">*Program.cs*:</span></span>
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
> <span data-ttu-id="1fc86-337">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="1fc86-337">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="1fc86-338">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-338">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="1fc86-339">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="1fc86-339">Distribute a health check library</span></span>

<span data-ttu-id="1fc86-340">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="1fc86-340">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="1fc86-341">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-341">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="1fc86-342">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="1fc86-342">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="1fc86-343">在健康情況檢查的邏輯 `CheckHealthAsync` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-343">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="1fc86-344">`data1`和 `data2` 會在方法中用來執行探查的健康情況檢查邏輯。</span><span class="sxs-lookup"><span data-stu-id="1fc86-344">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="1fc86-345">`AccessViolationException`已處理。</span><span class="sxs-lookup"><span data-stu-id="1fc86-345">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="1fc86-346">當 <xref:System.AccessViolationException> 發生時， <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> 會與一起傳回， <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 讓使用者能夠設定健全狀況檢查失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-346">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="1fc86-347">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="1fc86-347">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="1fc86-348">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="1fc86-348">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="1fc86-349">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="1fc86-349">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="1fc86-350">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="1fc86-350">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="1fc86-351">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="1fc86-351">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="1fc86-352">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-352">health check name (`name`).</span></span> <span data-ttu-id="1fc86-353">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-353">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="1fc86-354">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-354">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="1fc86-355">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-355">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="1fc86-356">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-356">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="1fc86-357">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-357">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="1fc86-358">預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-358">The default is `null`.</span></span> <span data-ttu-id="1fc86-359">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-359">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="1fc86-360">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-360">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="1fc86-361">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="1fc86-361">Health Check Publisher</span></span>

<span data-ttu-id="1fc86-362">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="1fc86-362">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="1fc86-363">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-363">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="1fc86-364"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="1fc86-364">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="1fc86-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="1fc86-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="1fc86-366"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>：在執行實例之前，應用程式啟動後所套用的初始延遲 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-366"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>: The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1fc86-367">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="1fc86-367">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="1fc86-368">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="1fc86-368">The default value is five seconds.</span></span>
* <span data-ttu-id="1fc86-369"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>：執行的期間 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-369"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>: The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="1fc86-370">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="1fc86-370">The default value is 30 seconds.</span></span>
* <span data-ttu-id="1fc86-371"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>：如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` （預設值），則健全狀況檢查發行者服務會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-371"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>: If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="1fc86-372">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-372">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="1fc86-373">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="1fc86-373">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="1fc86-374"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>：執行所有實例之健全狀況檢查的時間 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-374"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>: The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1fc86-375">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="1fc86-375">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="1fc86-376">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="1fc86-376">The default value is 30 seconds.</span></span>

<span data-ttu-id="1fc86-377">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-377">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="1fc86-378">會針對記錄層級的每個檢查記錄健全狀況檢查狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-378">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="1fc86-379"><xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>如果健康情況檢查狀態為，則為資訊（） <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-379">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="1fc86-380"><xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>如果狀態為或，則為錯誤（） <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-380">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="1fc86-381">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-381">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="1fc86-382">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="1fc86-382">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="1fc86-383">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-383">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="1fc86-384">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-384">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="1fc86-385">利用 MapWhen 限制健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-385">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="1fc86-386">使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="1fc86-386">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="1fc86-387">在下列範例中， `MapWhen` 如果收到端點的 GET 要求，則會將要求管線分支以啟動健康狀態檢查中介軟體 `api/HealthCheck` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-387">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

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

<span data-ttu-id="1fc86-388">如需詳細資訊，請參閱<xref:fundamentals/middleware/index#use-run-and-map>。</span><span class="sxs-lookup"><span data-stu-id="1fc86-388">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1fc86-389">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="1fc86-389">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="1fc86-390">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-390">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="1fc86-391">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="1fc86-391">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="1fc86-392">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-392">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="1fc86-393">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-393">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="1fc86-394">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-394">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="1fc86-395">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-395">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="1fc86-396">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-396">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="1fc86-397">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1fc86-397">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1fc86-398">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="1fc86-398">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="1fc86-399">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="1fc86-399">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="1fc86-400">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="1fc86-400">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fc86-401">必要條件</span><span class="sxs-lookup"><span data-stu-id="1fc86-401">Prerequisites</span></span>

<span data-ttu-id="1fc86-402">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-402">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="1fc86-403">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="1fc86-403">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="1fc86-404">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-404">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="1fc86-405">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1fc86-405">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="1fc86-406">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-406">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="1fc86-407">[資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。</span><span class="sxs-lookup"><span data-stu-id="1fc86-407">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="1fc86-408">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-408">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="1fc86-409">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="1fc86-409">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="1fc86-410">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="1fc86-410">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="1fc86-411">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="1fc86-411">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="1fc86-412">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="1fc86-412">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="1fc86-413">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="1fc86-413">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="1fc86-414">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-414">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="1fc86-415">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-415">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="1fc86-416">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-416">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="1fc86-417">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="1fc86-417">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="1fc86-418">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-418">Basic health probe</span></span>

<span data-ttu-id="1fc86-419">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」\*\*)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-419">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="1fc86-420">基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-420">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="1fc86-421">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="1fc86-421">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="1fc86-422">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="1fc86-422">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="1fc86-423">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-423">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="1fc86-424">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-424">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-425"><xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>在的要求處理管線中，新增健康情況檢查中介軟體的端點 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-425">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="1fc86-426">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="1fc86-426">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="1fc86-427">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-427">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="1fc86-428">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="1fc86-428">Docker example</span></span>

<span data-ttu-id="1fc86-429">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-429">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="1fc86-430">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-430">Create health checks</span></span>

<span data-ttu-id="1fc86-431">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="1fc86-431">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="1fc86-432"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-432">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="1fc86-433">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-433">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="1fc86-434"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="1fc86-434"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="1fc86-435">範例健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-435">Example health check</span></span>

<span data-ttu-id="1fc86-436">下列 `ExampleHealthCheck` 類別示範健全狀況檢查的版面配置。</span><span class="sxs-lookup"><span data-stu-id="1fc86-436">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="1fc86-437">健康情況檢查邏輯會放在 `CheckHealthAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="1fc86-437">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="1fc86-438">下列範例會將虛擬變數設定 `healthCheckResultHealthy` 為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-438">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="1fc86-439">如果的值 `healthCheckResultHealthy` 設定為，則 `false` 會傳回[HealthCheckResult](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-439">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="1fc86-440">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="1fc86-440">Register health check services</span></span>

<span data-ttu-id="1fc86-441">此 `ExampleHealthCheck` 類型會加入至中的健康狀態檢查服務 `Startup.ConfigureServices` <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-441">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="1fc86-442">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-442">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="1fc86-443">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-443">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="1fc86-444">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-444">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="1fc86-445">您可以使用「標籤」\*\* 來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-445">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="1fc86-446"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-446"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="1fc86-447">在下列 `Startup.ConfigureServices` 範例中，健康情況檢查名稱指定為 `Example` ，而檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-447">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="1fc86-448">使用健康狀態檢查中介軟體</span><span class="sxs-lookup"><span data-stu-id="1fc86-448">Use Health Checks Middleware</span></span>

<span data-ttu-id="1fc86-449">在 `Startup.Configure` 中，使用端點 URL 或相對路徑呼叫處理管線中的 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>：</span><span class="sxs-lookup"><span data-stu-id="1fc86-449">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="1fc86-450">如果健康狀態檢查應該接聽特定連接埠，請使用 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 的多載來設定連接埠 ([依連接埠篩選](#filter-by-port)一節中將進一步說明)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-450">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="1fc86-451">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="1fc86-451">Health check options</span></span>

<span data-ttu-id="1fc86-452"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="1fc86-452"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="1fc86-453">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-453">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="1fc86-454">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="1fc86-454">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="1fc86-455">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="1fc86-455">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="1fc86-456">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="1fc86-456">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="1fc86-457">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-457">Filter health checks</span></span>

<span data-ttu-id="1fc86-458">根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-458">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="1fc86-459">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-459">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="1fc86-460">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="1fc86-460">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="1fc86-461">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="1fc86-461">Customize the HTTP status code</span></span>

<span data-ttu-id="1fc86-462">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-462">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="1fc86-463">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="1fc86-463">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="1fc86-464">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-464">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="1fc86-465">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-465">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="1fc86-466">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="1fc86-466">Suppress cache headers</span></span>

<span data-ttu-id="1fc86-467"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="1fc86-467"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="1fc86-468">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="1fc86-468">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="1fc86-469">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="1fc86-469">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="1fc86-470">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-470">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="1fc86-471">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="1fc86-471">Customize output</span></span>

<span data-ttu-id="1fc86-472"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。</span><span class="sxs-lookup"><span data-stu-id="1fc86-472">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="1fc86-473">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-473">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="1fc86-474">在 `Startup.Configure` 中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-474">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="1fc86-475">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-475">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="1fc86-476">下列自訂委派會 `WriteResponse` 輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-476">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="1fc86-477">健全狀況檢查系統不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。</span><span class="sxs-lookup"><span data-stu-id="1fc86-477">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="1fc86-478">您可以視需要自訂 `JObject` 上述範例中的，以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-478">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="1fc86-479">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-479">Database probe</span></span>

<span data-ttu-id="1fc86-480">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="1fc86-480">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="1fc86-481">範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-481">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="1fc86-482">`AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="1fc86-482">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="1fc86-483">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="1fc86-483">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="1fc86-484">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="1fc86-484">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="1fc86-485">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="1fc86-485">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="1fc86-486">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-486">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="1fc86-487">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-487">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="1fc86-488">包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1fc86-488">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="1fc86-489">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="1fc86-489">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="1fc86-490">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="1fc86-490">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="1fc86-491">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-491">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-492">範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="1fc86-492">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1fc86-493">在的應用程式處理管線中呼叫健全狀況檢查中介軟體 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-493">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="1fc86-494">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-494">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="1fc86-495">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-495">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="1fc86-496">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-496">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="1fc86-497">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="1fc86-497">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="1fc86-498">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="1fc86-498">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="1fc86-499">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-499">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="1fc86-500">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="1fc86-500">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="1fc86-501">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-501">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="1fc86-502">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="1fc86-502">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="1fc86-503">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="1fc86-503">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="1fc86-504">依照預設：</span><span class="sxs-lookup"><span data-stu-id="1fc86-504">By default:</span></span>

* <span data-ttu-id="1fc86-505">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="1fc86-505">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="1fc86-506">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="1fc86-506">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="1fc86-507">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="1fc86-507">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="1fc86-508">在範例應用程式中， `AppDbContext` 會提供給， `AddDbContextCheck` 並在 `Startup.ConfigureServices` （*DbCoNtextHealthStartup.cs*）中註冊為服務：</span><span class="sxs-lookup"><span data-stu-id="1fc86-508">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1fc86-509">在範例應用程式中，會 `UseHealthChecks` 在中新增健全狀況檢查中介軟體 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-509">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="1fc86-510">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-510">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="1fc86-511">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="1fc86-511">If the database exists, delete it.</span></span>

<span data-ttu-id="1fc86-512">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-512">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="1fc86-513">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-513">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="1fc86-514">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-514">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="1fc86-515">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-515">Trigger the sample app to create the database.</span></span> <span data-ttu-id="1fc86-516">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-516">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="1fc86-517">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-517">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1fc86-518">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-518">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1fc86-519">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-519">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="1fc86-520">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="1fc86-520">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="1fc86-521">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-521">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="1fc86-522">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-522">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="1fc86-523">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-523">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="1fc86-524">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-524">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="1fc86-525">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-525">Separate readiness and liveness probes</span></span>

<span data-ttu-id="1fc86-526">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-526">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="1fc86-527">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-527">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="1fc86-528">此狀態是指應用程式的「整備度」\*\*。</span><span class="sxs-lookup"><span data-stu-id="1fc86-528">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="1fc86-529">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-529">The app is functioning and responding to requests.</span></span> <span data-ttu-id="1fc86-530">此狀態是指應用程式的「活躍度」\*\*。</span><span class="sxs-lookup"><span data-stu-id="1fc86-530">This state is the app's *liveness*.</span></span>

<span data-ttu-id="1fc86-531">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-531">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="1fc86-532">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-532">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="1fc86-533">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="1fc86-533">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="1fc86-534">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="1fc86-534">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="1fc86-535">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-535">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="1fc86-536">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-536">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="1fc86-537">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="1fc86-537">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="1fc86-538">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="1fc86-538">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="1fc86-539">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="1fc86-539">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="1fc86-540">在中的應用程式處理管線中呼叫健全狀況檢查中介軟體 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-540">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="1fc86-541">在範例應用程式中，健康狀態檢查端點是在 `/health/ready` (針對整備度檢查) 和 `/health/live` (針對活躍度檢查) 建立。</span><span class="sxs-lookup"><span data-stu-id="1fc86-541">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="1fc86-542">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-542">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="1fc86-543">活躍度檢查會藉由在 [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) 中傳回 `false` 來篩選掉 `StartupHostedServiceHealthCheck` (如需詳細資訊，請參閱[篩選健康狀態檢查](#filter-health-checks))：</span><span class="sxs-lookup"><span data-stu-id="1fc86-543">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

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

<span data-ttu-id="1fc86-544">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-544">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="1fc86-545">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="1fc86-545">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="1fc86-546">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="1fc86-546">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="1fc86-547">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="1fc86-547">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="1fc86-548">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-548">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="1fc86-549">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="1fc86-549">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="1fc86-550">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="1fc86-550">Kubernetes example</span></span>

<span data-ttu-id="1fc86-551">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-551">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="1fc86-552">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-552">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="1fc86-553">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="1fc86-553">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="1fc86-554">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-554">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="1fc86-555">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="1fc86-555">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="1fc86-556">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="1fc86-556">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="1fc86-557">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-557">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="1fc86-558">`MemoryHealthCheck`如果應用程式使用超過指定的記憶體閾值（範例應用程式中為 1 GB），會回報狀況不良的狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-558">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="1fc86-559"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-559">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="1fc86-560">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-560">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-561">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-561">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="1fc86-562">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="1fc86-562">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="1fc86-563">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-563">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="1fc86-564">在範例應用程式（*CustomWriterStartup.cs*）中：</span><span class="sxs-lookup"><span data-stu-id="1fc86-564">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="1fc86-565">在中的應用程式處理管線中呼叫健全狀況檢查中介軟體 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-565">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="1fc86-566">當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-566">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

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

<span data-ttu-id="1fc86-567">`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：</span><span class="sxs-lookup"><span data-stu-id="1fc86-567">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="1fc86-568">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-568">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="1fc86-569">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-569">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="1fc86-570">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-570">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="1fc86-571">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="1fc86-571">Filter by port</span></span>

<span data-ttu-id="1fc86-572">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 並提供連接埠會限制對指定的連接埠提出健康狀態檢查要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-572">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="1fc86-573">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-573">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="1fc86-574">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-574">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1fc86-575">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="1fc86-575">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="1fc86-576">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="1fc86-576">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="1fc86-577">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1fc86-577">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="1fc86-578">範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：</span><span class="sxs-lookup"><span data-stu-id="1fc86-578">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="1fc86-579">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="1fc86-579">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1fc86-580">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 會指定管理連接埠 (*ManagementPortStartup.cs*)：</span><span class="sxs-lookup"><span data-stu-id="1fc86-580">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="1fc86-581">您可以在程式碼中明確設定 URL 和管理連接埠，避免在範例應用程式中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1fc86-581">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="1fc86-582">在 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 建立所在的 *Program.cs* 中，新增 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> 呼叫並提供應用程式的正常回應端點和管理連接埠端點。</span><span class="sxs-lookup"><span data-stu-id="1fc86-582">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="1fc86-583">在 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 呼叫所在的 *ManagementPortStartup.cs* 中，明確指定管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="1fc86-583">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="1fc86-584">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="1fc86-584">*Program.cs*:</span></span>
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
> <span data-ttu-id="1fc86-585">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="1fc86-585">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="1fc86-586">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="1fc86-586">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="1fc86-587">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="1fc86-587">Distribute a health check library</span></span>

<span data-ttu-id="1fc86-588">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="1fc86-588">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="1fc86-589">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-589">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="1fc86-590">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="1fc86-590">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="1fc86-591">在健康情況檢查的邏輯 `CheckHealthAsync` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-591">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="1fc86-592">`data1`和 `data2` 會在方法中用來執行探查的健康情況檢查邏輯。</span><span class="sxs-lookup"><span data-stu-id="1fc86-592">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="1fc86-593">`AccessViolationException`已處理。</span><span class="sxs-lookup"><span data-stu-id="1fc86-593">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="1fc86-594">當 <xref:System.AccessViolationException> 發生時， <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> 會與一起傳回， <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 讓使用者能夠設定健全狀況檢查失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-594">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="1fc86-595">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="1fc86-595">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="1fc86-596">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="1fc86-596">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="1fc86-597">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="1fc86-597">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="1fc86-598">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="1fc86-598">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="1fc86-599">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="1fc86-599">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="1fc86-600">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-600">health check name (`name`).</span></span> <span data-ttu-id="1fc86-601">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-601">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="1fc86-602">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-602">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="1fc86-603">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-603">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="1fc86-604">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-604">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="1fc86-605">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-605">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="1fc86-606">預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="1fc86-606">The default is `null`.</span></span> <span data-ttu-id="1fc86-607">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-607">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="1fc86-608">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-608">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="1fc86-609">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="1fc86-609">Health Check Publisher</span></span>

<span data-ttu-id="1fc86-610">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="1fc86-610">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="1fc86-611">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="1fc86-611">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="1fc86-612"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="1fc86-612">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="1fc86-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="1fc86-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="1fc86-614"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>：在執行實例之前，應用程式啟動後所套用的初始延遲 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-614"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>: The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1fc86-615">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="1fc86-615">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="1fc86-616">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="1fc86-616">The default value is five seconds.</span></span>
* <span data-ttu-id="1fc86-617"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>：執行的期間 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-617"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>: The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="1fc86-618">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="1fc86-618">The default value is 30 seconds.</span></span>
* <span data-ttu-id="1fc86-619"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>：如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` （預設值），則健全狀況檢查發行者服務會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-619"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate>: If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="1fc86-620">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="1fc86-620">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="1fc86-621">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="1fc86-621">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="1fc86-622"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>：執行所有實例之健全狀況檢查的時間 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-622"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout>: The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="1fc86-623">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="1fc86-623">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="1fc86-624">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="1fc86-624">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="1fc86-625">在 ASP.NET Core 2.2 版中，設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>並不會獲得 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作遵守；它會設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> 的值。</span><span class="sxs-lookup"><span data-stu-id="1fc86-625">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="1fc86-626">ASP.NET Core 3.0 已解決此問題。</span><span class="sxs-lookup"><span data-stu-id="1fc86-626">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="1fc86-627">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="1fc86-627">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="1fc86-628">健全狀況檢查狀態會針對每個檢查記錄為下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="1fc86-628">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="1fc86-629"><xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>如果健康情況檢查狀態為，則為資訊（） <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-629">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="1fc86-630"><xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>如果狀態為或，則為錯誤（） <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy> 。</span><span class="sxs-lookup"><span data-stu-id="1fc86-630">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="1fc86-631">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="1fc86-631">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="1fc86-632">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="1fc86-632">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="1fc86-633">以下因應措施可允許在已將一或多個其他託管服務新增至應用程式的情況下，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="1fc86-633">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="1fc86-634">ASP.NET Core 3.0 不需要此因應措施。</span><span class="sxs-lookup"><span data-stu-id="1fc86-634">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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
> <span data-ttu-id="1fc86-635">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="1fc86-635">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="1fc86-636">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="1fc86-636">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="1fc86-637">利用 MapWhen 限制健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="1fc86-637">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="1fc86-638">使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="1fc86-638">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="1fc86-639">在下列範例中， `MapWhen` 如果收到端點的 GET 要求，則會將要求管線分支以啟動健康狀態檢查中介軟體 `api/HealthCheck` ：</span><span class="sxs-lookup"><span data-stu-id="1fc86-639">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="1fc86-640">如需詳細資訊，請參閱<xref:fundamentals/middleware/index#use-run-and-map>。</span><span class="sxs-lookup"><span data-stu-id="1fc86-640">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
