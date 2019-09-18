---
title: ASP.NET Core 中的健康狀態檢查
author: guardrex
description: 了解如何為 ASP.NET Core 基礎結構 (例如應用程式和資料庫) 設定健康狀態檢查。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 09/10/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 8fdb1332882fd25bd61f5403a3b1f10e8a0bc7f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081523"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="39591-103">ASP.NET Core 中的健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="39591-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="39591-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="39591-105">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="39591-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="39591-106">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="39591-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="39591-107">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="39591-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="39591-108">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="39591-109">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="39591-110">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="39591-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="39591-111">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="39591-112">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="39591-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="39591-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="39591-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="39591-114">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="39591-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="39591-115">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="39591-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="39591-116">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="39591-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39591-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="39591-117">Prerequisites</span></span>

<span data-ttu-id="39591-118">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="39591-119">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="39591-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="39591-120">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="39591-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="39591-121">將套件參考新增至[AspNetCore HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks)套件。</span><span class="sxs-lookup"><span data-stu-id="39591-121">Add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span> <span data-ttu-id="39591-122">若要使用 Entity Framework Core 執行健康情況檢查，請將套件參考新增至[HealthChecks. microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore)套件。</span><span class="sxs-lookup"><span data-stu-id="39591-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="39591-123">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="39591-124">[資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。</span><span class="sxs-lookup"><span data-stu-id="39591-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="39591-125">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="39591-126">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="39591-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="39591-127">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="39591-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="39591-128">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="39591-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="39591-129">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="39591-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="39591-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="39591-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="39591-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="39591-132">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="39591-133">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="39591-134">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="39591-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="39591-135">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="39591-135">Basic health probe</span></span>

<span data-ttu-id="39591-136">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="39591-137">基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。</span><span class="sxs-lookup"><span data-stu-id="39591-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="39591-138">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="39591-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="39591-139">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="39591-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="39591-140">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="39591-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="39591-141">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-142">在中呼叫`MapHealthChecks` ，以`Startup.Configure`建立健康狀態檢查端點。</span><span class="sxs-lookup"><span data-stu-id="39591-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="39591-143">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="39591-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="39591-144">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="39591-145">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="39591-145">Docker example</span></span>

<span data-ttu-id="39591-146">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="39591-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="39591-147">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-147">Create health checks</span></span>

<span data-ttu-id="39591-148">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="39591-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="39591-149"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。</span><span class="sxs-lookup"><span data-stu-id="39591-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="39591-150">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="39591-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="39591-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="39591-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="39591-152">下列`ExampleHealthCheck`類別示範健全狀況檢查的版面配置。</span><span class="sxs-lookup"><span data-stu-id="39591-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="39591-153">健康情況檢查邏輯會放在`CheckHealthAsync`方法中。</span><span class="sxs-lookup"><span data-stu-id="39591-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="39591-154">下列範例會將虛擬變數`healthCheckResultHealthy`設定為。 `true`</span><span class="sxs-lookup"><span data-stu-id="39591-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="39591-155">如果的值`healthCheckResultHealthy`設定為`false`，則會傳回[HealthCheckResult](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

## <a name="register-health-check-services"></a><span data-ttu-id="39591-156">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="39591-156">Register health check services</span></span>

<span data-ttu-id="39591-157">類型會加入至<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>中`Startup.ConfigureServices`的健康狀態檢查服務： `ExampleHealthCheck`</span><span class="sxs-lookup"><span data-stu-id="39591-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="39591-158">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="39591-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="39591-159">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="39591-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="39591-160">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="39591-161">您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="39591-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="39591-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="39591-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="39591-163">在下列範例中，健康狀態檢查名稱指定為 `Example`，且檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="39591-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="39591-164">使用健全狀況檢查路由</span><span class="sxs-lookup"><span data-stu-id="39591-164">Use Health Checks Routing</span></span>

<span data-ttu-id="39591-165">在`Startup.Configure`中， `MapHealthChecks`使用端點 URL 或相對路徑在端點產生器上呼叫：</span><span class="sxs-lookup"><span data-stu-id="39591-165">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="39591-166">需要主機</span><span class="sxs-lookup"><span data-stu-id="39591-166">Require host</span></span>

<span data-ttu-id="39591-167">呼叫`RequireHost`以指定一或多個允許的主機用於健康情況檢查端點。</span><span class="sxs-lookup"><span data-stu-id="39591-167">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="39591-168">主機應該是 Unicode 而不是 punycode，而且可能包含埠。</span><span class="sxs-lookup"><span data-stu-id="39591-168">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="39591-169">如果未提供集合，則會接受任何主機。</span><span class="sxs-lookup"><span data-stu-id="39591-169">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="39591-170">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="39591-170">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="39591-171">需要授權</span><span class="sxs-lookup"><span data-stu-id="39591-171">Require authorization</span></span>

<span data-ttu-id="39591-172">呼叫`RequireAuthorization`以在健康情況檢查要求端點上執行授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="39591-172">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="39591-173">`RequireAuthorization`多載會接受一或多個授權原則。</span><span class="sxs-lookup"><span data-stu-id="39591-173">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="39591-174">如果未提供原則，則會使用預設的授權原則。</span><span class="sxs-lookup"><span data-stu-id="39591-174">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="39591-175">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="39591-175">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="39591-176">雖然從瀏覽器手動執行健康情況檢查並不是常見的使用案例，但您可以呼叫`RequireCors`健全狀況檢查端點來啟用 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="39591-176">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="39591-177">多載會接受 CORS 原則產生器委派`CorsPolicyBuilder`（）或原則名稱。 `RequireCors`</span><span class="sxs-lookup"><span data-stu-id="39591-177">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="39591-178">如果未提供原則，則會使用預設的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="39591-178">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="39591-179">如需詳細資訊，請參閱 <xref:security/cors>。</span><span class="sxs-lookup"><span data-stu-id="39591-179">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="39591-180">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="39591-180">Health check options</span></span>

<span data-ttu-id="39591-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="39591-181"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="39591-182">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-182">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="39591-183">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="39591-183">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="39591-184">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="39591-184">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="39591-185">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="39591-185">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="39591-186">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-186">Filter health checks</span></span>

<span data-ttu-id="39591-187">根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-187">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="39591-188">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="39591-188">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="39591-189">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="39591-189">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="39591-190">在 `Startup.ConfigureServices`中：</span><span class="sxs-lookup"><span data-stu-id="39591-190">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="39591-191">在`Startup.Configure`中`Predicate` ，會篩選出 ' Bar ' 健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-191">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="39591-192">只有 Foo 和 Baz.png execute。：</span><span class="sxs-lookup"><span data-stu-id="39591-192">Only Foo and Baz execute.:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="39591-193">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="39591-193">Customize the HTTP status code</span></span>

<span data-ttu-id="39591-194">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="39591-194">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="39591-195">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="39591-195">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="39591-196">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="39591-196">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="39591-197">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="39591-197">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="39591-198">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="39591-198">Suppress cache headers</span></span>

<span data-ttu-id="39591-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="39591-199"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="39591-200">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="39591-200">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="39591-201">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="39591-201">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="39591-202">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="39591-202">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="39591-203">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="39591-203">Customize output</span></span>

<span data-ttu-id="39591-204"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。</span><span class="sxs-lookup"><span data-stu-id="39591-204">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span>

<span data-ttu-id="39591-205">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="39591-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="39591-206">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="39591-206">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="39591-207">下列自訂委派`WriteResponse`會輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="39591-207">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="39591-208">健全狀況檢查系統不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。</span><span class="sxs-lookup"><span data-stu-id="39591-208">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="39591-209">您可以視需要自`JObject`定義上述範例中的，以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="39591-209">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="39591-210">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="39591-210">Database probe</span></span>

<span data-ttu-id="39591-211">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="39591-211">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="39591-212">範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-212">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="39591-213">`AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="39591-213">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="39591-214">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="39591-214">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="39591-215">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="39591-215">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="39591-216">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="39591-216">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="39591-217">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="39591-217">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="39591-218">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="39591-218">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="39591-219">包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="39591-219">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="39591-220">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="39591-220">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="39591-221">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="39591-221">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="39591-222">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-222">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-223">範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="39591-223">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="39591-224">健康狀態檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立：</span><span class="sxs-lookup"><span data-stu-id="39591-224">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="39591-225">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-225">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="39591-226">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-226">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="39591-227">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="39591-227">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="39591-228">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="39591-228">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="39591-229">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="39591-229">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="39591-230">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="39591-230">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="39591-231">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="39591-231">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="39591-232">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-232">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="39591-233">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="39591-233">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="39591-234">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="39591-234">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="39591-235">根據預設：</span><span class="sxs-lookup"><span data-stu-id="39591-235">By default:</span></span>

* <span data-ttu-id="39591-236">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="39591-236">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="39591-237">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="39591-237">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="39591-238">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="39591-238">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="39591-239">在範例應用程式中`AppDbContext` ，會提供`AddDbContextCheck`給，並在（*DbCoNtextHealthStartup.cs*）中`Startup.ConfigureServices`註冊為服務：</span><span class="sxs-lookup"><span data-stu-id="39591-239">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="39591-240">健康狀態檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立：</span><span class="sxs-lookup"><span data-stu-id="39591-240">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="39591-241">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-241">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="39591-242">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="39591-242">If the database exists, delete it.</span></span>

<span data-ttu-id="39591-243">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-243">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="39591-244">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-244">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="39591-245">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="39591-245">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="39591-246">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-246">Trigger the sample app to create the database.</span></span> <span data-ttu-id="39591-247">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-247">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="39591-248">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="39591-248">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="39591-249">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-249">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="39591-250">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="39591-250">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="39591-251">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-251">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="39591-252">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-252">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="39591-253">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="39591-253">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="39591-254">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-254">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="39591-255">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="39591-255">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="39591-256">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="39591-256">Separate readiness and liveness probes</span></span>

<span data-ttu-id="39591-257">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="39591-257">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="39591-258">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="39591-258">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="39591-259">此狀態是指應用程式的「整備度」。</span><span class="sxs-lookup"><span data-stu-id="39591-259">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="39591-260">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="39591-260">The app is functioning and responding to requests.</span></span> <span data-ttu-id="39591-261">此狀態是指應用程式的「活躍度」。</span><span class="sxs-lookup"><span data-stu-id="39591-261">This state is the app's *liveness*.</span></span>

<span data-ttu-id="39591-262">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="39591-262">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="39591-263">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="39591-263">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="39591-264">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="39591-264">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="39591-265">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="39591-265">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="39591-266">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="39591-266">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="39591-267">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="39591-267">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="39591-268">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="39591-268">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="39591-269">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="39591-269">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="39591-270">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="39591-270">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="39591-271">健康情況檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立。</span><span class="sxs-lookup"><span data-stu-id="39591-271">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="39591-272">在範例應用程式中，健康狀態檢查端點會建立于：</span><span class="sxs-lookup"><span data-stu-id="39591-272">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="39591-273">`/health/ready`以進行準備檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-273">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="39591-274">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-274">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="39591-275">`/health/live`做為活動檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-275">`/health/live` for the liveness check.</span></span> <span data-ttu-id="39591-276">活動檢查`StartupHostedServiceHealthCheck`會藉`false`由在[HealthCheckOptions](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate)中傳回來篩選掉（如需詳細資訊，請參閱[篩選健全狀況檢查](#filter-health-checks)）</span><span class="sxs-lookup"><span data-stu-id="39591-276">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="39591-277">在下列範例程式碼中：</span><span class="sxs-lookup"><span data-stu-id="39591-277">In the following example code:</span></span>

* <span data-ttu-id="39591-278">準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="39591-278">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="39591-279">會`Predicate`排除所有檢查並傳回 200-Ok。</span><span class="sxs-lookup"><span data-stu-id="39591-279">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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

<span data-ttu-id="39591-280">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-280">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="39591-281">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="39591-281">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="39591-282">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="39591-282">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="39591-283">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="39591-283">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="39591-284">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="39591-284">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="39591-285">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="39591-285">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="39591-286">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="39591-286">Kubernetes example</span></span>

<span data-ttu-id="39591-287">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="39591-287">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="39591-288">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="39591-288">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="39591-289">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="39591-289">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="39591-290">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="39591-290">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="39591-291">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="39591-291">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="39591-292">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="39591-292">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="39591-293">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-293">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="39591-294">如果應用程式使用超過指定的記憶體閾值 (在範例應用程式中為 1 GB)，`MemoryHealthCheck` 會報告降級的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-294">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="39591-295"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="39591-295">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="39591-296">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-296">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-297">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-297">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="39591-298">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="39591-298">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="39591-299">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="39591-299">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="39591-300">在範例應用程式（*CustomWriterStartup.cs*）中：</span><span class="sxs-lookup"><span data-stu-id="39591-300">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="39591-301">健康情況檢查端點是藉由呼叫`MapHealthChecks`中`Startup.Configure`的來建立。</span><span class="sxs-lookup"><span data-stu-id="39591-301">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="39591-302">當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="39591-302">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="39591-303">`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：</span><span class="sxs-lookup"><span data-stu-id="39591-303">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="39591-304">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-304">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="39591-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-305">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="39591-306">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-306">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="39591-307">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="39591-307">Filter by port</span></span>

<span data-ttu-id="39591-308">使用指定埠的 URL 模式來呼叫`RequireHost` ，以限制對指定埠的健康狀態檢查要求。 `MapHealthChecks`</span><span class="sxs-lookup"><span data-stu-id="39591-308">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="39591-309">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-309">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="39591-310">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-310">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="39591-311">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="39591-311">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="39591-312">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="39591-312">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="39591-313">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="39591-313">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="39591-314">範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：</span><span class="sxs-lookup"><span data-stu-id="39591-314">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="39591-315">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-315">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-316">在中呼叫`MapHealthChecks` ，以`Startup.Configure`建立健康狀態檢查端點。</span><span class="sxs-lookup"><span data-stu-id="39591-316">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="39591-317">在範例應用程式中，對`RequireHost`上`Startup.Configure`端點的呼叫會從設定中指定管理埠：</span><span class="sxs-lookup"><span data-stu-id="39591-317">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="39591-318">端點會在的範例應用程式`Startup.Configure`中建立。</span><span class="sxs-lookup"><span data-stu-id="39591-318">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="39591-319">在下列範例程式碼中：</span><span class="sxs-lookup"><span data-stu-id="39591-319">In the following example code:</span></span>

* <span data-ttu-id="39591-320">準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="39591-320">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="39591-321">會`Predicate`排除所有檢查並傳回 200-Ok。</span><span class="sxs-lookup"><span data-stu-id="39591-321">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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
> <span data-ttu-id="39591-322">您可以在程式碼中明確設定管理埠，以避免在範例應用程式中建立*launchsettings.json。*</span><span class="sxs-lookup"><span data-stu-id="39591-322">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="39591-323">在建立的<xref:Microsoft.Extensions.Hosting.HostBuilder> Program.cs 中， <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*>新增對的呼叫，並提供應用程式的管理埠端點。</span><span class="sxs-lookup"><span data-stu-id="39591-323">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="39591-324">在`Configure` *ManagementPortStartup.cs*的中，使用`RequireHost`指定管理埠：</span><span class="sxs-lookup"><span data-stu-id="39591-324">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="39591-325">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="39591-325">*Program.cs*:</span></span>
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
> <span data-ttu-id="39591-326">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="39591-326">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="39591-327">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-327">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="39591-328">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="39591-328">Distribute a health check library</span></span>

<span data-ttu-id="39591-329">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="39591-329">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="39591-330">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="39591-330">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="39591-331">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="39591-331">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="39591-332">在健康情況`CheckHealthAsync`檢查的邏輯：</span><span class="sxs-lookup"><span data-stu-id="39591-332">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="39591-333">`data1`和`data2`會在方法中用來執行探查的健康情況檢查邏輯。</span><span class="sxs-lookup"><span data-stu-id="39591-333">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="39591-334">`AccessViolationException`已處理。</span><span class="sxs-lookup"><span data-stu-id="39591-334">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="39591-335">當發生時<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> ，會與一起<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>傳回，讓使用者能夠設定健全狀況檢查失敗狀態。 <xref:System.AccessViolationException></span><span class="sxs-lookup"><span data-stu-id="39591-335">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="39591-336">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="39591-336">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="39591-337">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="39591-337">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="39591-338">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="39591-338">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="39591-339">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="39591-339">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="39591-340">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="39591-340">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="39591-341">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="39591-341">health check name (`name`).</span></span> <span data-ttu-id="39591-342">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="39591-342">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="39591-343">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="39591-343">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="39591-344">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="39591-344">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="39591-345">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="39591-345">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="39591-346">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="39591-346">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="39591-347">預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="39591-347">The default is `null`.</span></span> <span data-ttu-id="39591-348">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="39591-348">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="39591-349">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="39591-349">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="39591-350">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="39591-350">Health Check Publisher</span></span>

<span data-ttu-id="39591-351">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="39591-351">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="39591-352">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-352">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="39591-353"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="39591-353">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="39591-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="39591-354"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="39591-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 初始延遲會在應用程式啟動後且執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體前套用。</span><span class="sxs-lookup"><span data-stu-id="39591-355"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="39591-356">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="39591-356">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="39591-357">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="39591-357">The default value is five seconds.</span></span>
* <span data-ttu-id="39591-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。</span><span class="sxs-lookup"><span data-stu-id="39591-358"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="39591-359">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="39591-359">The default value is 30 seconds.</span></span>
* <span data-ttu-id="39591-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` (預設)，健康狀態檢查發行者服務就會執行所有已註冊的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="39591-361">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="39591-361">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="39591-362">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="39591-362">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="39591-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體之健康狀態檢查的逾時。</span><span class="sxs-lookup"><span data-stu-id="39591-363"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="39591-364">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="39591-364">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="39591-365">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="39591-365">The default value is 30 seconds.</span></span>

<span data-ttu-id="39591-366">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="39591-366">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="39591-367">健康狀態檢查狀態會記錄在 `Entries` 中，並針對每項檢查進行記錄：</span><span class="sxs-lookup"><span data-stu-id="39591-367">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="39591-368">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-368">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="39591-369">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="39591-369">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="39591-370">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="39591-370">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="39591-371">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-371">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="39591-372">利用 MapWhen 限制健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="39591-372">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="39591-373">使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="39591-373">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="39591-374">在下列範例中， `MapWhen`如果收到`api/HealthCheck`端點的 GET 要求，則會將要求管線分支以啟動健康狀態檢查中介軟體：</span><span class="sxs-lookup"><span data-stu-id="39591-374">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

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

<span data-ttu-id="39591-375">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#use-run-and-map>。</span><span class="sxs-lookup"><span data-stu-id="39591-375">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="39591-376">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="39591-376">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="39591-377">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="39591-377">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="39591-378">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="39591-378">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="39591-379">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-379">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="39591-380">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-380">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="39591-381">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="39591-381">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="39591-382">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-382">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="39591-383">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="39591-383">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="39591-384">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="39591-384">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="39591-385">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="39591-385">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="39591-386">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="39591-386">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="39591-387">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="39591-387">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39591-388">必要條件</span><span class="sxs-lookup"><span data-stu-id="39591-388">Prerequisites</span></span>

<span data-ttu-id="39591-389">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-389">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="39591-390">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="39591-390">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="39591-391">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="39591-391">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="39591-392">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="39591-392">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="39591-393">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-393">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="39591-394">[資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。</span><span class="sxs-lookup"><span data-stu-id="39591-394">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="39591-395">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-395">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="39591-396">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="39591-396">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="39591-397">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="39591-397">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="39591-398">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="39591-398">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="39591-399">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="39591-399">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="39591-400">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="39591-400">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="39591-401">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-401">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="39591-402">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-402">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="39591-403">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-403">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="39591-404">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="39591-404">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="39591-405">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="39591-405">Basic health probe</span></span>

<span data-ttu-id="39591-406">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-406">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="39591-407">基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。</span><span class="sxs-lookup"><span data-stu-id="39591-407">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="39591-408">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="39591-408">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="39591-409">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="39591-409">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="39591-410">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="39591-410">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="39591-411">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-411">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-412">在的要求處理<xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> `Startup.Configure`管線中，新增健康情況檢查中介軟體的端點。</span><span class="sxs-lookup"><span data-stu-id="39591-412">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="39591-413">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="39591-413">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="39591-414">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-414">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="39591-415">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="39591-415">Docker example</span></span>

<span data-ttu-id="39591-416">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="39591-416">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="39591-417">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-417">Create health checks</span></span>

<span data-ttu-id="39591-418">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="39591-418">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="39591-419"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。</span><span class="sxs-lookup"><span data-stu-id="39591-419">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="39591-420">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="39591-420">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="39591-421"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="39591-421"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="39591-422">範例健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-422">Example health check</span></span>

<span data-ttu-id="39591-423">下列`ExampleHealthCheck`類別示範健全狀況檢查的版面配置。</span><span class="sxs-lookup"><span data-stu-id="39591-423">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="39591-424">健康情況檢查邏輯會放在`CheckHealthAsync`方法中。</span><span class="sxs-lookup"><span data-stu-id="39591-424">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="39591-425">下列範例會將虛擬變數`healthCheckResultHealthy`設定為。 `true`</span><span class="sxs-lookup"><span data-stu-id="39591-425">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="39591-426">如果的值`healthCheckResultHealthy`設定為`false`，則會傳回[HealthCheckResult](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-426">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="39591-427">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="39591-427">Register health check services</span></span>

<span data-ttu-id="39591-428">此`ExampleHealthCheck`類型會加入至<xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>中`Startup.ConfigureServices`的健康狀態檢查服務：</span><span class="sxs-lookup"><span data-stu-id="39591-428">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="39591-429">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="39591-429">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="39591-430">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="39591-430">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="39591-431">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-431">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="39591-432">您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="39591-432">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="39591-433"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="39591-433"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="39591-434">在下列`Startup.ConfigureServices`範例中，健康情況檢查名稱指定為`Example` ，而檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="39591-434">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="39591-435">使用健康狀態檢查中介軟體</span><span class="sxs-lookup"><span data-stu-id="39591-435">Use Health Checks Middleware</span></span>

<span data-ttu-id="39591-436">在 `Startup.Configure` 中，使用端點 URL 或相對路徑呼叫處理管線中的 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>：</span><span class="sxs-lookup"><span data-stu-id="39591-436">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="39591-437">如果健康狀態檢查應該接聽特定連接埠，請使用 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 的多載來設定連接埠 ([依連接埠篩選](#filter-by-port)一節中將進一步說明)：</span><span class="sxs-lookup"><span data-stu-id="39591-437">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="39591-438">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="39591-438">Health check options</span></span>

<span data-ttu-id="39591-439"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="39591-439"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="39591-440">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-440">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="39591-441">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="39591-441">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="39591-442">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="39591-442">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="39591-443">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="39591-443">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="39591-444">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="39591-444">Filter health checks</span></span>

<span data-ttu-id="39591-445">根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-445">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="39591-446">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="39591-446">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="39591-447">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="39591-447">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="39591-448">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="39591-448">Customize the HTTP status code</span></span>

<span data-ttu-id="39591-449">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="39591-449">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="39591-450">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="39591-450">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="39591-451">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="39591-451">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="39591-452">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="39591-452">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="39591-453">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="39591-453">Suppress cache headers</span></span>

<span data-ttu-id="39591-454"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses>控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="39591-454"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="39591-455">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="39591-455">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="39591-456">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="39591-456">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="39591-457">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="39591-457">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="39591-458">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="39591-458">Customize output</span></span>

<span data-ttu-id="39591-459"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。</span><span class="sxs-lookup"><span data-stu-id="39591-459">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="39591-460">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="39591-460">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="39591-461">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="39591-461">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="39591-462">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="39591-462">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="39591-463">下列自訂委派`WriteResponse`會輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="39591-463">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="39591-464">健全狀況檢查系統不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。</span><span class="sxs-lookup"><span data-stu-id="39591-464">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="39591-465">您可以視需要自`JObject`定義上述範例中的，以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="39591-465">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="39591-466">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="39591-466">Database probe</span></span>

<span data-ttu-id="39591-467">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="39591-467">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="39591-468">範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-468">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="39591-469">`AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="39591-469">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="39591-470">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="39591-470">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="39591-471">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="39591-471">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="39591-472">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="39591-472">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="39591-473">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="39591-473">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="39591-474">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="39591-474">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="39591-475">包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="39591-475">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="39591-476">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="39591-476">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="39591-477">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="39591-477">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="39591-478">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-478">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-479">範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="39591-479">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="39591-480">在的應用程式處理管線中`Startup.Configure`呼叫健全狀況檢查中介軟體：</span><span class="sxs-lookup"><span data-stu-id="39591-480">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="39591-481">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-481">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="39591-482">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-482">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="39591-483">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="39591-483">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="39591-484">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="39591-484">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="39591-485">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="39591-485">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="39591-486">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="39591-486">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="39591-487">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="39591-487">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="39591-488">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-488">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="39591-489">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="39591-489">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="39591-490">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="39591-490">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="39591-491">根據預設：</span><span class="sxs-lookup"><span data-stu-id="39591-491">By default:</span></span>

* <span data-ttu-id="39591-492">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="39591-492">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="39591-493">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="39591-493">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="39591-494">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="39591-494">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="39591-495">在範例應用程式中`AppDbContext` ，會提供`AddDbContextCheck`給，並在（*DbCoNtextHealthStartup.cs*）中`Startup.ConfigureServices`註冊為服務：</span><span class="sxs-lookup"><span data-stu-id="39591-495">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="39591-496">在範例應用程式中`UseHealthChecks` ，會在中`Startup.Configure`新增健全狀況檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="39591-496">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="39591-497">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-497">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="39591-498">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="39591-498">If the database exists, delete it.</span></span>

<span data-ttu-id="39591-499">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-499">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="39591-500">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-500">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="39591-501">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="39591-501">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="39591-502">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-502">Trigger the sample app to create the database.</span></span> <span data-ttu-id="39591-503">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-503">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="39591-504">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="39591-504">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="39591-505">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-505">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="39591-506">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="39591-506">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="39591-507">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="39591-507">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="39591-508">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-508">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="39591-509">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="39591-509">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="39591-510">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="39591-510">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="39591-511">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="39591-511">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="39591-512">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="39591-512">Separate readiness and liveness probes</span></span>

<span data-ttu-id="39591-513">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="39591-513">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="39591-514">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="39591-514">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="39591-515">此狀態是指應用程式的「整備度」。</span><span class="sxs-lookup"><span data-stu-id="39591-515">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="39591-516">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="39591-516">The app is functioning and responding to requests.</span></span> <span data-ttu-id="39591-517">此狀態是指應用程式的「活躍度」。</span><span class="sxs-lookup"><span data-stu-id="39591-517">This state is the app's *liveness*.</span></span>

<span data-ttu-id="39591-518">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="39591-518">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="39591-519">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="39591-519">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="39591-520">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="39591-520">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="39591-521">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="39591-521">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="39591-522">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="39591-522">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="39591-523">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="39591-523">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="39591-524">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="39591-524">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="39591-525">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="39591-525">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="39591-526">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="39591-526">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="39591-527">在中的應用程式處理管線中`Startup.Configure`呼叫健全狀況檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="39591-527">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="39591-528">在範例應用程式中，健康狀態檢查端點是在 `/health/ready` (針對整備度檢查) 和 `/health/live` (針對活躍度檢查) 建立。</span><span class="sxs-lookup"><span data-stu-id="39591-528">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="39591-529">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-529">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="39591-530">活躍度檢查會藉由在 [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) 中傳回 `false` 來篩選掉 `StartupHostedServiceHealthCheck` (如需詳細資訊，請參閱[篩選健康狀態檢查](#filter-health-checks))：</span><span class="sxs-lookup"><span data-stu-id="39591-530">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

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

<span data-ttu-id="39591-531">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-531">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="39591-532">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="39591-532">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="39591-533">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="39591-533">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="39591-534">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="39591-534">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="39591-535">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="39591-535">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="39591-536">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="39591-536">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="39591-537">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="39591-537">Kubernetes example</span></span>

<span data-ttu-id="39591-538">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="39591-538">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="39591-539">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="39591-539">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="39591-540">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="39591-540">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="39591-541">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="39591-541">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="39591-542">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="39591-542">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="39591-543">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="39591-543">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="39591-544">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-544">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="39591-545">`MemoryHealthCheck`如果應用程式使用超過指定的記憶體閾值（範例應用程式中為 1 GB），會回報狀況不良的狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-545">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="39591-546"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="39591-546">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="39591-547">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-547">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-548">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-548">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="39591-549">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="39591-549">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="39591-550">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="39591-550">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="39591-551">在範例應用程式（*CustomWriterStartup.cs*）中：</span><span class="sxs-lookup"><span data-stu-id="39591-551">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="39591-552">在中的應用程式處理管線中`Startup.Configure`呼叫健全狀況檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="39591-552">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="39591-553">當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="39591-553">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

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

<span data-ttu-id="39591-554">`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：</span><span class="sxs-lookup"><span data-stu-id="39591-554">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="39591-555">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-555">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="39591-556">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-556">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="39591-557">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-557">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="39591-558">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="39591-558">Filter by port</span></span>

<span data-ttu-id="39591-559">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 並提供連接埠會限制對指定的連接埠提出健康狀態檢查要求。</span><span class="sxs-lookup"><span data-stu-id="39591-559">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="39591-560">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-560">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="39591-561">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-561">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="39591-562">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="39591-562">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="39591-563">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="39591-563">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="39591-564">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="39591-564">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="39591-565">範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：</span><span class="sxs-lookup"><span data-stu-id="39591-565">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="39591-566">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="39591-566">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="39591-567">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 會指定管理連接埠 (*ManagementPortStartup.cs*)：</span><span class="sxs-lookup"><span data-stu-id="39591-567">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="39591-568">您可以在程式碼中明確設定 URL 和管理連接埠，避免在範例應用程式中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="39591-568">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="39591-569">在 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 建立所在的 *Program.cs* 中，新增 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> 呼叫並提供應用程式的正常回應端點和管理連接埠端點。</span><span class="sxs-lookup"><span data-stu-id="39591-569">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="39591-570">在 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 呼叫所在的 *ManagementPortStartup.cs* 中，明確指定管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="39591-570">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="39591-571">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="39591-571">*Program.cs*:</span></span>
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
> <span data-ttu-id="39591-572">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="39591-572">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="39591-573">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="39591-573">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="39591-574">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="39591-574">Distribute a health check library</span></span>

<span data-ttu-id="39591-575">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="39591-575">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="39591-576">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="39591-576">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="39591-577">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="39591-577">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="39591-578">在健康情況`CheckHealthAsync`檢查的邏輯：</span><span class="sxs-lookup"><span data-stu-id="39591-578">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="39591-579">`data1`和`data2`會在方法中用來執行探查的健康情況檢查邏輯。</span><span class="sxs-lookup"><span data-stu-id="39591-579">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="39591-580">`AccessViolationException`已處理。</span><span class="sxs-lookup"><span data-stu-id="39591-580">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="39591-581">當發生時<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> ，會與一起<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>傳回，讓使用者能夠設定健全狀況檢查失敗狀態。 <xref:System.AccessViolationException></span><span class="sxs-lookup"><span data-stu-id="39591-581">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="39591-582">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="39591-582">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="39591-583">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="39591-583">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="39591-584">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="39591-584">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="39591-585">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="39591-585">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="39591-586">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="39591-586">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="39591-587">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="39591-587">health check name (`name`).</span></span> <span data-ttu-id="39591-588">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="39591-588">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="39591-589">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="39591-589">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="39591-590">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="39591-590">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="39591-591">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="39591-591">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="39591-592">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="39591-592">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="39591-593">預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="39591-593">The default is `null`.</span></span> <span data-ttu-id="39591-594">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="39591-594">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="39591-595">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="39591-595">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="39591-596">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="39591-596">Health Check Publisher</span></span>

<span data-ttu-id="39591-597">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="39591-597">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="39591-598">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="39591-598">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="39591-599"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="39591-599">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="39591-600"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="39591-600"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="39591-601"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 初始延遲會在應用程式啟動後且執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體前套用。</span><span class="sxs-lookup"><span data-stu-id="39591-601"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="39591-602">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="39591-602">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="39591-603">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="39591-603">The default value is five seconds.</span></span>
* <span data-ttu-id="39591-604"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。</span><span class="sxs-lookup"><span data-stu-id="39591-604"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="39591-605">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="39591-605">The default value is 30 seconds.</span></span>
* <span data-ttu-id="39591-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` (預設)，健康狀態檢查發行者服務就會執行所有已註冊的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-606"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="39591-607">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="39591-607">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="39591-608">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="39591-608">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="39591-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體之健康狀態檢查的逾時。</span><span class="sxs-lookup"><span data-stu-id="39591-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="39591-610">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="39591-610">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="39591-611">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="39591-611">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="39591-612">在 ASP.NET Core 2.2 版中，設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>並不會獲得 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作遵守；它會設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> 的值。</span><span class="sxs-lookup"><span data-stu-id="39591-612">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="39591-613">ASP.NET Core 3.0 已解決此問題。</span><span class="sxs-lookup"><span data-stu-id="39591-613">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="39591-614">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="39591-614">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="39591-615">健康狀態檢查狀態會記錄在 `Entries` 中，並針對每項檢查進行記錄：</span><span class="sxs-lookup"><span data-stu-id="39591-615">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="39591-616">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="39591-616">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="39591-617">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="39591-617">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="39591-618">以下因應措施可允許在已將一或多個其他託管服務新增至應用程式的情況下，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="39591-618">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="39591-619">ASP.NET Core 3.0 不需要此因應措施。</span><span class="sxs-lookup"><span data-stu-id="39591-619">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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
> <span data-ttu-id="39591-620">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="39591-620">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="39591-621">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 是 [BeatPulse](https://github.com/xabaril/beatpulse) 的連接埠，不受 Microsoft 維護或支援。</span><span class="sxs-lookup"><span data-stu-id="39591-621">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) is a port of [BeatPulse](https://github.com/xabaril/beatpulse) and isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="39591-622">利用 MapWhen 限制健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="39591-622">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="39591-623">使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="39591-623">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="39591-624">在下列範例中， `MapWhen`如果收到`api/HealthCheck`端點的 GET 要求，則會將要求管線分支以啟動健康狀態檢查中介軟體：</span><span class="sxs-lookup"><span data-stu-id="39591-624">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="39591-625">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#use-run-and-map>。</span><span class="sxs-lookup"><span data-stu-id="39591-625">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
