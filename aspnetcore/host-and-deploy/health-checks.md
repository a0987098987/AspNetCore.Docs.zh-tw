---
title: ASP.NET Core 中的健康狀態檢查
author: guardrex
description: 了解如何為 ASP.NET Core 基礎結構 (例如應用程式和資料庫) 設定健康狀態檢查。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: 9157c94c6e8f433869c8163ebf7772a7271b11ba
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265324"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="b96df-103">ASP.NET Core 中的健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="b96df-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="b96df-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="b96df-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="b96df-105">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，來報告應用程式基礎結構元件的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="b96df-106">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="b96df-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="b96df-107">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="b96df-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="b96df-108">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="b96df-109">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="b96df-110">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b96df-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="b96df-111">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="b96df-112">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="b96df-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="b96df-113">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b96df-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b96df-114">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="b96df-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="b96df-115">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="b96df-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="b96df-116">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="b96df-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b96df-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="b96df-117">Prerequisites</span></span>

<span data-ttu-id="b96df-118">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="b96df-119">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="b96df-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="b96df-120">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="b96df-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="b96df-121">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="b96df-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="b96df-122">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="b96df-123">[資料庫探查](#database-probe)案例使用 [BeatPulse](https://github.com/Xabaril/BeatPulse) 來檢查資料庫連線的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-123">The [database probe](#database-probe) scenario checks the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="b96df-124">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="b96df-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="b96df-125">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="b96df-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="b96df-126">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="b96df-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="b96df-127">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="b96df-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="b96df-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="b96df-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="b96df-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="b96df-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="b96df-130">Microsoft 不會維護或支援 [BeatPulse](https://github.com/Xabaril/BeatPulse)。</span><span class="sxs-lookup"><span data-stu-id="b96df-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="b96df-131">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="b96df-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="b96df-132">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="b96df-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="b96df-133">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="b96df-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="b96df-134">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="b96df-134">Basic health probe</span></span>

<span data-ttu-id="b96df-135">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="b96df-136">基本組態會登錄健康狀態檢查服務，並呼叫健康狀態檢查中介軟體在健康狀態回應的 URL 端點做出回應。</span><span class="sxs-lookup"><span data-stu-id="b96df-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="b96df-137">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="b96df-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="b96df-138">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="b96df-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="b96df-139">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="b96df-139">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="b96df-140">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="b96df-140">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b96df-141">在 `Startup.Configure` 的要求處理管線中，使用 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 新增健康狀態檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b96df-141">Add Health Check Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="b96df-142">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="b96df-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="b96df-143">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="b96df-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="b96df-144">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="b96df-144">Docker example</span></span>

<span data-ttu-id="b96df-145">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="b96df-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="b96df-146">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="b96df-146">Create health checks</span></span>

<span data-ttu-id="b96df-147">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="b96df-147">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="b96df-148"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回指出狀態為 `Healthy`、`Degraded` 或 `Unhealthy` 的 `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>`。</span><span class="sxs-lookup"><span data-stu-id="b96df-148">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="b96df-149">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="b96df-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="b96df-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="b96df-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="b96df-151">範例健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="b96df-151">Example health check</span></span>

<span data-ttu-id="b96df-152">下列 `ExampleHealthCheck` 類別示範健康狀態檢查的配置：</span><span class="sxs-lookup"><span data-stu-id="b96df-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context,
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
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

### <a name="register-health-check-services"></a><span data-ttu-id="b96df-153">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="b96df-153">Register health check services</span></span>

<span data-ttu-id="b96df-154">使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 將 `ExampleHealthCheck` 類型新增至健康狀態檢查服務：</span><span class="sxs-lookup"><span data-stu-id="b96df-154">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="b96df-155">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="b96df-155">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="b96df-156">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="b96df-156">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="b96df-157">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="b96df-158">您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="b96df-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="b96df-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="b96df-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="b96df-160">在下列範例中，健康狀態檢查名稱指定為 `Example`，且檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="b96df-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () =>
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="b96df-161">使用健康狀態檢查中介軟體</span><span class="sxs-lookup"><span data-stu-id="b96df-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="b96df-162">在 `Startup.Configure` 中，使用端點 URL 或相對路徑呼叫處理管線中的 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>：</span><span class="sxs-lookup"><span data-stu-id="b96df-162">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="b96df-163">如果健康狀態檢查應該接聽特定連接埠，請使用 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 的多載來設定連接埠 ([依連接埠篩選](#filter-by-port)一節中將進一步說明)：</span><span class="sxs-lookup"><span data-stu-id="b96df-163">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="b96df-164">健康狀態檢查中介軟體是應用程式要求處理管線中的「終端機中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="b96df-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="b96df-165">系統會執行所遇到第一個與要求 URL 完全相符的健康狀態檢查端點，並跳過中介軟體管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="b96df-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="b96df-166">若出現跳過，則不會在相符的健康狀態檢查之後執行中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b96df-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="b96df-167">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="b96df-167">Health check options</span></span>

<span data-ttu-id="b96df-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="b96df-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="b96df-169">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="b96df-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="b96df-170">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="b96df-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="b96df-171">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="b96df-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="b96df-172">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="b96df-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="b96df-173">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="b96df-173">Filter health checks</span></span>

<span data-ttu-id="b96df-174">根據預設，健康狀態檢查中介軟體會執行所有登錄的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="b96df-175">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="b96df-175">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="b96df-176">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="b96df-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () =>
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") ||
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="b96df-177">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="b96df-177">Customize the HTTP status code</span></span>

<span data-ttu-id="b96df-178">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="b96df-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="b96df-179">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="b96df-179">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="b96df-180">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="b96df-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="b96df-181">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="b96df-181">Suppress cache headers</span></span>

<span data-ttu-id="b96df-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> 會控制健康狀態檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="b96df-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="b96df-183">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="b96df-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="b96df-184">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="b96df-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="b96df-185">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="b96df-185">Customize output</span></span>

<span data-ttu-id="b96df-186"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。</span><span class="sxs-lookup"><span data-stu-id="b96df-186">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="b96df-187">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="b96df-187">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext,
    HealthReport result)
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

## <a name="database-probe"></a><span data-ttu-id="b96df-188">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="b96df-188">Database probe</span></span>

<span data-ttu-id="b96df-189">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="b96df-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="b96df-190">範例應用程式使用適用於 ASP.NET Core 應用程式的健康狀態檢查程式庫 [BeatPulse](https://github.com/Xabaril/BeatPulse)，在 SQL Server 資料庫上執行健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-190">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="b96df-191">BeatPulse 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="b96df-191">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="b96df-192">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="b96df-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="b96df-193">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="b96df-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="b96df-194">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="b96df-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="b96df-195">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="b96df-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="b96df-196">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="b96df-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="b96df-197">若要使用 BeatPulse 程式庫，請包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="b96df-197">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="b96df-198">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="b96df-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="b96df-199">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="b96df-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="b96df-200">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="b96df-200">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b96df-201">範例應用程式使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 BeatPulse 的 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="b96df-201">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="b96df-202">在 `Startup.Configure` 中，呼叫應用程式處理管線中的健康狀態檢查中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b96df-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="b96df-203">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="b96df-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="b96df-204">Microsoft 不會維護或支援 [BeatPulse](https://github.com/Xabaril/BeatPulse)。</span><span class="sxs-lookup"><span data-stu-id="b96df-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="b96df-205">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="b96df-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="b96df-206">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="b96df-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="b96df-207">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="b96df-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="b96df-208">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="b96df-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="b96df-209">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="b96df-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="b96df-210">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="b96df-211">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="b96df-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="b96df-212">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="b96df-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="b96df-213">根據預設：</span><span class="sxs-lookup"><span data-stu-id="b96df-213">By default:</span></span>

* <span data-ttu-id="b96df-214">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="b96df-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="b96df-215">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="b96df-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="b96df-216">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="b96df-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="b96df-217">在範例應用程式中，`AppDbContext` 會提供給 `AddDbContextCheck` 並在 `Startup.ConfigureServices` 中登錄為服務。</span><span class="sxs-lookup"><span data-stu-id="b96df-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="b96df-218">*DbContextHealthStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="b96df-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="b96df-219">在範例應用程式中，`UseHealthChecks` 會在 `Startup.Configure` 中新增健康狀態檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b96df-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="b96df-220">*DbContextHealthStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="b96df-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="b96df-221">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b96df-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="b96df-222">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="b96df-222">If the database exists, delete it.</span></span>

<span data-ttu-id="b96df-223">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="b96df-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="b96df-224">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="b96df-225">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="b96df-226">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="b96df-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="b96df-227">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="b96df-228">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="b96df-229">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="b96df-230">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="b96df-231">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="b96df-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="b96df-232">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="b96df-233">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="b96df-234">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="b96df-235">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="b96df-236">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="b96df-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="b96df-237">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="b96df-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="b96df-238">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="b96df-239">此狀態是指應用程式的「整備度」。</span><span class="sxs-lookup"><span data-stu-id="b96df-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="b96df-240">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="b96df-241">此狀態是指應用程式的「活躍度」。</span><span class="sxs-lookup"><span data-stu-id="b96df-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="b96df-242">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="b96df-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="b96df-243">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="b96df-244">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="b96df-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="b96df-245">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="b96df-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="b96df-246">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b96df-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="b96df-247">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="b96df-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="b96df-248">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="b96df-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="b96df-249">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="b96df-249">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="b96df-250">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="b96df-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="b96df-251">在 `Startup.Configure` 中，呼叫應用程式處理管線中的健康狀態檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b96df-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="b96df-252">在範例應用程式中，健康狀態檢查端點是在 `/health/ready` (針對整備度檢查) 和 `/health/live` (針對活躍度檢查) 建立。</span><span class="sxs-lookup"><span data-stu-id="b96df-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="b96df-253">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="b96df-254">活躍度檢查會藉由在 [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) 中傳回 `false` 來篩選掉 `StartupHostedServiceHealthCheck` (如需詳細資訊，請參閱[篩選健康狀態檢查](#filter-health-checks))：</span><span class="sxs-lookup"><span data-stu-id="b96df-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="b96df-255">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="b96df-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="b96df-256">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="b96df-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="b96df-257">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="b96df-257">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="b96df-258">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="b96df-258">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="b96df-259">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="b96df-259">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="b96df-260">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="b96df-260">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="b96df-261">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="b96df-261">Kubernetes example</span></span>

<span data-ttu-id="b96df-262">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="b96df-262">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="b96df-263">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="b96df-263">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="b96df-264">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b96df-264">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="b96df-265">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="b96df-265">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="b96df-266">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="b96df-266">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="b96df-267">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="b96df-267">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="b96df-268">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-268">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="b96df-269">如果應用程式使用超過指定的記憶體閾值 (在範例應用程式中為 1 GB)，`MemoryHealthCheck` 會報告降級的狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-269">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="b96df-270"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b96df-270">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="b96df-271">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="b96df-271">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b96df-272">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-272">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="b96df-273">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="b96df-273">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="b96df-274">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="b96df-274">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="b96df-275">*CustomWriterStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="b96df-275">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="b96df-276">在 `Startup.Configure` 中，呼叫應用程式處理管線中的健康狀態檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b96df-276">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="b96df-277">當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-277">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="b96df-278">`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：</span><span class="sxs-lookup"><span data-stu-id="b96df-278">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="b96df-279">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="b96df-279">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="b96df-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) 隨附計量型健康狀態檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="b96df-281">Microsoft 不會維護或支援 [BeatPulse](https://github.com/Xabaril/BeatPulse)。</span><span class="sxs-lookup"><span data-stu-id="b96df-281">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="b96df-282">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="b96df-282">Filter by port</span></span>

<span data-ttu-id="b96df-283">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 並提供連接埠會限制對指定的連接埠提出健康狀態檢查要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-283">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="b96df-284">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="b96df-284">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="b96df-285">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="b96df-285">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b96df-286">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="b96df-286">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="b96df-287">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="b96df-287">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="b96df-288">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b96df-288">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="b96df-289">下列 *launchSettings.json* 檔案並未包含在範例應用程式的專案檔中，因此必須手動建立。</span><span class="sxs-lookup"><span data-stu-id="b96df-289">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="b96df-290">*Properties/launchSettings.json*：</span><span class="sxs-lookup"><span data-stu-id="b96df-290">*Properties/launchSettings.json*:</span></span>

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

<span data-ttu-id="b96df-291">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="b96df-291">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b96df-292">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 會指定管理連接埠 (*ManagementPortStartup.cs*)：</span><span class="sxs-lookup"><span data-stu-id="b96df-292">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="b96df-293">您可以在程式碼中明確設定 URL 和管理連接埠，避免在範例應用程式中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="b96df-293">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="b96df-294">在 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 建立所在的 *Program.cs* 中，新增 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> 呼叫並提供應用程式的正常回應端點和管理連接埠端點。</span><span class="sxs-lookup"><span data-stu-id="b96df-294">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="b96df-295">在 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 呼叫所在的 *ManagementPortStartup.cs* 中，明確指定管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="b96df-295">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="b96df-296">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="b96df-296">*Program.cs*:</span></span>
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
> <span data-ttu-id="b96df-297">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="b96df-297">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="b96df-298">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="b96df-298">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="b96df-299">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="b96df-299">Distribute a health check library</span></span>

<span data-ttu-id="b96df-300">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="b96df-300">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="b96df-301">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="b96df-301">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="b96df-302">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="b96df-302">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

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
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

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

1. <span data-ttu-id="b96df-303">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b96df-303">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="b96df-304">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="b96df-304">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="b96df-305">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="b96df-305">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="b96df-306">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="b96df-306">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="b96df-307">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="b96df-307">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="b96df-308">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="b96df-308">health check name (`name`).</span></span> <span data-ttu-id="b96df-309">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="b96df-309">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="b96df-310">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="b96df-310">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="b96df-311">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="b96df-311">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="b96df-312">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="b96df-312">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="b96df-313">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="b96df-313">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="b96df-314">預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="b96df-314">The default is `null`.</span></span> <span data-ttu-id="b96df-315">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="b96df-315">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="b96df-316">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="b96df-316">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder,
           string name = default,
           string data1,
           int data2 = 1,
           HealthStatus? failureStatus = default,
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="b96df-317">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="b96df-317">Health Check Publisher</span></span>

<span data-ttu-id="b96df-318">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="b96df-318">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="b96df-319">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="b96df-319">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="b96df-320"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="b96df-320">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="b96df-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="b96df-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="b96df-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 初始延遲會在應用程式啟動後且執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體前套用。</span><span class="sxs-lookup"><span data-stu-id="b96df-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="b96df-323">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="b96df-323">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="b96df-324">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="b96df-324">The default value is five seconds.</span></span>
* <span data-ttu-id="b96df-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。</span><span class="sxs-lookup"><span data-stu-id="b96df-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="b96df-326">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="b96df-326">The default value is 30 seconds.</span></span>
* <span data-ttu-id="b96df-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 為 `null` (預設)，健康狀態檢查發行者服務就會執行所有已註冊的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="b96df-328">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="b96df-328">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="b96df-329">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="b96df-329">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="b96df-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體之健康狀態檢查的逾時。</span><span class="sxs-lookup"><span data-stu-id="b96df-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="b96df-331">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="b96df-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="b96df-332">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="b96df-332">The default value is 30 seconds.</span></span>

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> <span data-ttu-id="b96df-333">在 ASP.NET Core 2.2 版中，設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>並不會獲得 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作遵守；它會設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> 的值。</span><span class="sxs-lookup"><span data-stu-id="b96df-333">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="b96df-334">在 ASP.NET Core 3.0 中會修正此問題。</span><span class="sxs-lookup"><span data-stu-id="b96df-334">This issue will be fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="b96df-335">如需詳細資訊，請參閱 [HealthCheckPublisherOptions.Period 設定 .Delay 的值](https://github.com/aspnet/Extensions/issues/1041) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b96df-335">For more information, see [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041).</span></span>

::: moniker-end

<span data-ttu-id="b96df-336">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="b96df-336">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="b96df-337">健康狀態檢查狀態會記錄在 `Entries` 中，並針對每項檢查進行記錄：</span><span class="sxs-lookup"><span data-stu-id="b96df-337">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="b96df-338">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="b96df-338">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="b96df-339">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="b96df-339">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="b96df-340">以下因應措施可允許在已將一或多個其他託管服務新增至應用程式的情況下，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="b96df-340">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="b96df-341">使用 ASP.NET Core 3.0 版時，不需要採取此因應措施。</span><span class="sxs-lookup"><span data-stu-id="b96df-341">This workaround won't be required with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="b96df-342">如需詳細資訊，請參閱： https://github.com/aspnet/Extensions/issues/639 \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b96df-342">For more information, see: https://github.com/aspnet/Extensions/issues/639.</span></span>
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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="b96df-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="b96df-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="b96df-344">Microsoft 不會維護或支援 [BeatPulse](https://github.com/Xabaril/BeatPulse)。</span><span class="sxs-lookup"><span data-stu-id="b96df-344">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
