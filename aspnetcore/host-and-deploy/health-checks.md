---
title: ASP.NET Core 中的健康狀態檢查
author: guardrex
description: 了解如何為 ASP.NET Core 基礎結構 (例如應用程式和資料庫) 設定健康狀態檢查。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: dfd26b775b6c6a1af0108d34981d7ec3737980dd
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75356120"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="10a6c-103">ASP.NET Core 中的健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="10a6c-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Glenn Condron](https://github.com/glennc)</span><span class="sxs-lookup"><span data-stu-id="10a6c-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="10a6c-105">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="10a6c-105">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="10a6c-106">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="10a6c-107">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="10a6c-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="10a6c-108">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="10a6c-109">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="10a6c-110">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="10a6c-111">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="10a6c-112">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="10a6c-113">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10a6c-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10a6c-114">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="10a6c-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="10a6c-115">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="10a6c-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="10a6c-116">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="10a6c-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10a6c-117">必要條件：</span><span class="sxs-lookup"><span data-stu-id="10a6c-117">Prerequisites</span></span>

<span data-ttu-id="10a6c-118">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="10a6c-119">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="10a6c-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="10a6c-120">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="10a6c-121">ASP.NET Core 應用程式會以隱含方式參考[HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks)套件。</span><span class="sxs-lookup"><span data-stu-id="10a6c-121">The [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package is referenced implicitly for ASP.NET Core apps.</span></span> <span data-ttu-id="10a6c-122">若要使用 Entity Framework Core 執行健康情況檢查，請將套件參考新增至[HealthChecks. microsoft.entityframeworkcore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore)套件。</span><span class="sxs-lookup"><span data-stu-id="10a6c-122">To perform health checks using Entity Framework Core, add a package reference to the [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore) package.</span></span>

<span data-ttu-id="10a6c-123">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-123">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="10a6c-124">[資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。</span><span class="sxs-lookup"><span data-stu-id="10a6c-124">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="10a6c-125">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-125">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="10a6c-126">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="10a6c-126">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="10a6c-127">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="10a6c-127">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="10a6c-128">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="10a6c-128">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="10a6c-129">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="10a6c-129">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="10a6c-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="10a6c-130">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="10a6c-131">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-131">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="10a6c-132">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-132">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="10a6c-133">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-133">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="10a6c-134">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="10a6c-134">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="10a6c-135">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-135">Basic health probe</span></span>

<span data-ttu-id="10a6c-136">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-136">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="10a6c-137">基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-137">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="10a6c-138">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="10a6c-138">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="10a6c-139">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="10a6c-139">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="10a6c-140">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-140">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="10a6c-141">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-141">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-142">藉由呼叫 `Startup.Configure`中的 `MapHealthChecks` 來建立健康狀態檢查端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-142">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="10a6c-143">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="10a6c-143">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="10a6c-144">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-144">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="10a6c-145">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="10a6c-145">Docker example</span></span>

<span data-ttu-id="10a6c-146">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-146">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="10a6c-147">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-147">Create health checks</span></span>

<span data-ttu-id="10a6c-148">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="10a6c-148">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="10a6c-149"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-149">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="10a6c-150">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-150">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="10a6c-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="10a6c-151"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

<span data-ttu-id="10a6c-152">下列 `ExampleHealthCheck` 類別示範健全狀況檢查的配置。</span><span class="sxs-lookup"><span data-stu-id="10a6c-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="10a6c-153">健康情況檢查邏輯會放在 `CheckHealthAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="10a6c-153">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="10a6c-154">下列範例會將虛擬變數 `healthCheckResultHealthy`設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-154">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="10a6c-155">如果 `healthCheckResultHealthy` 的值設定為 `false`，則會傳回[HealthCheckResult 狀況不良](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-155">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

## <a name="register-health-check-services"></a><span data-ttu-id="10a6c-156">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="10a6c-156">Register health check services</span></span>

<span data-ttu-id="10a6c-157">`ExampleHealthCheck` 類型會新增至 `Startup.ConfigureServices`中具有 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 的健全狀況檢查服務：</span><span class="sxs-lookup"><span data-stu-id="10a6c-157">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="10a6c-158">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-158">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="10a6c-159">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-159">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="10a6c-160">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-160">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="10a6c-161">您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-161">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="10a6c-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-162"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="10a6c-163">在下列範例中，健康狀態檢查名稱指定為 `Example`，且檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-163">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

<span data-ttu-id="10a6c-164">呼叫 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> 以將 arugments 傳遞至健康情況檢查的執行。</span><span class="sxs-lookup"><span data-stu-id="10a6c-164">Call <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddTypeActivatedCheck*> to pass arugments to a health check implementation.</span></span> <span data-ttu-id="10a6c-165">在下列範例中，`TestHealthCheckWithArgs` 會接受整數和字串，以便在呼叫 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 時使用：</span><span class="sxs-lookup"><span data-stu-id="10a6c-165">In the following example, `TestHealthCheckWithArgs` accepts an integer and a string for use when <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> is called:</span></span>

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

<span data-ttu-id="10a6c-166">`TestHealthCheckWithArgs` 是藉由使用傳遞至實作為的整數和字串來呼叫 `AddTypeActivatedCheck` 進行註冊：</span><span class="sxs-lookup"><span data-stu-id="10a6c-166">`TestHealthCheckWithArgs` is registered by calling `AddTypeActivatedCheck` with the integer and string passed to the implementation:</span></span>

```csharp
services.AddHealthChecks()
    .AddTypeActivatedCheck<TestHealthCheckWithArgs>(
        "test", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" }, 
        args: new object[] { 5, "string" });
```

## <a name="use-health-checks-routing"></a><span data-ttu-id="10a6c-167">使用健全狀況檢查路由</span><span class="sxs-lookup"><span data-stu-id="10a6c-167">Use Health Checks Routing</span></span>

<span data-ttu-id="10a6c-168">在 `Startup.Configure`中，使用端點 URL 或相對路徑，在端點產生器上呼叫 `MapHealthChecks`：</span><span class="sxs-lookup"><span data-stu-id="10a6c-168">In `Startup.Configure`, call `MapHealthChecks` on the endpoint builder with the endpoint URL or relative path:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
});
```

### <a name="require-host"></a><span data-ttu-id="10a6c-169">需要主機</span><span class="sxs-lookup"><span data-stu-id="10a6c-169">Require host</span></span>

<span data-ttu-id="10a6c-170">呼叫 `RequireHost`，為健康狀態檢查端點指定一或多個允許的主機。</span><span class="sxs-lookup"><span data-stu-id="10a6c-170">Call `RequireHost` to specify one or more permitted hosts for the health check endpoint.</span></span> <span data-ttu-id="10a6c-171">主機應該是 Unicode 而不是 punycode，而且可能包含埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-171">Hosts should be Unicode rather than punycode and may include a port.</span></span> <span data-ttu-id="10a6c-172">如果未提供集合，則會接受任何主機。</span><span class="sxs-lookup"><span data-stu-id="10a6c-172">If a collection isn't supplied, any host is accepted.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireHost("www.contoso.com:5001");
});
```

<span data-ttu-id="10a6c-173">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="10a6c-173">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

### <a name="require-authorization"></a><span data-ttu-id="10a6c-174">需要授權</span><span class="sxs-lookup"><span data-stu-id="10a6c-174">Require authorization</span></span>

<span data-ttu-id="10a6c-175">呼叫 `RequireAuthorization` 以在健康情況檢查要求端點上執行授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-175">Call `RequireAuthorization` to run Authorization Middleware on the health check request endpoint.</span></span> <span data-ttu-id="10a6c-176">`RequireAuthorization` 多載接受一或多個授權原則。</span><span class="sxs-lookup"><span data-stu-id="10a6c-176">A `RequireAuthorization` overload accepts one or more authorization policies.</span></span> <span data-ttu-id="10a6c-177">如果未提供原則，則會使用預設的授權原則。</span><span class="sxs-lookup"><span data-stu-id="10a6c-177">If a policy isn't provided, the default authorization policy is used.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health").RequireAuthorization();
});
```

### <a name="enable-cross-origin-requests-cors"></a><span data-ttu-id="10a6c-178">啟用跨原始來源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="10a6c-178">Enable Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="10a6c-179">雖然從瀏覽器手動執行健康情況檢查不是常見的使用案例，但可以藉由呼叫健全狀況檢查端點上的 `RequireCors` 來啟用 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-179">Although performing health checks manually from a browser isn't a common use scenario, CORS Middleware can be enabled by calling `RequireCors` on health checks endpoints.</span></span> <span data-ttu-id="10a6c-180">`RequireCors` 多載會接受 CORS 原則產生器委派（`CorsPolicyBuilder`）或原則名稱。</span><span class="sxs-lookup"><span data-stu-id="10a6c-180">A `RequireCors` overload accepts a CORS policy builder delegate (`CorsPolicyBuilder`) or a policy name.</span></span> <span data-ttu-id="10a6c-181">如果未提供原則，則會使用預設的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="10a6c-181">If a policy isn't provided, the default CORS policy is used.</span></span> <span data-ttu-id="10a6c-182">如需詳細資訊，請參閱<xref:security/cors>。</span><span class="sxs-lookup"><span data-stu-id="10a6c-182">For more information, see <xref:security/cors>.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="10a6c-183">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="10a6c-183">Health check options</span></span>

<span data-ttu-id="10a6c-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="10a6c-184"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="10a6c-185">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-185">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="10a6c-186">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="10a6c-186">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="10a6c-187">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="10a6c-187">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="10a6c-188">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="10a6c-188">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="10a6c-189">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-189">Filter health checks</span></span>

<span data-ttu-id="10a6c-190">根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-190">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="10a6c-191">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-191">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="10a6c-192">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="10a6c-192">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

<span data-ttu-id="10a6c-193">在 `Startup.ConfigureServices`中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-193">In `Startup.ConfigureServices`:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Foo", () =>
        HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
    .AddCheck("Bar", () =>
        HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
    .AddCheck("Baz", () =>
        HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
```

<span data-ttu-id="10a6c-194">在 `Startup.Configure`中，`Predicate` 會篩選出 ' Bar ' 的健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-194">In `Startup.Configure`, the `Predicate` filters out the 'Bar' health check.</span></span> <span data-ttu-id="10a6c-195">只有 Foo 和 Baz.png execute。：</span><span class="sxs-lookup"><span data-stu-id="10a6c-195">Only Foo and Baz execute.:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="10a6c-196">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="10a6c-196">Customize the HTTP status code</span></span>

<span data-ttu-id="10a6c-197">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-197">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="10a6c-198">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="10a6c-198">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="10a6c-199">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-199">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="10a6c-200">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-200">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="10a6c-201">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="10a6c-201">Suppress cache headers</span></span>

<span data-ttu-id="10a6c-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> 控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="10a6c-202"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="10a6c-203">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="10a6c-203">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="10a6c-204">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="10a6c-204">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="10a6c-205">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-205">In `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        AllowCachingResponses = false
    });
});
```

### <a name="customize-output"></a><span data-ttu-id="10a6c-206">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="10a6c-206">Customize output</span></span>

<span data-ttu-id="10a6c-207">在 `Startup.Configure`中，將[HealthCheckOptions. ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter)選項設定為用於寫入回應的委派：</span><span class="sxs-lookup"><span data-stu-id="10a6c-207">In `Startup.Configure`, set the [HealthCheckOptions.ResponseWriter](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter) option to a delegate for writing the response:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
});
```

<span data-ttu-id="10a6c-208">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-208">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="10a6c-209">下列自訂委派會輸出自訂 JSON 回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-209">The following custom delegates output a custom JSON response.</span></span>

<span data-ttu-id="10a6c-210">範例應用程式的第一個範例示範如何使用 <xref:System.Text.Json?displayProperty=fullName>：</span><span class="sxs-lookup"><span data-stu-id="10a6c-210">The first example from the sample app demonstrates how to use <xref:System.Text.Json?displayProperty=fullName>:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_SystemTextJson)]

<span data-ttu-id="10a6c-211">第二個範例示範如何使用[Newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-211">The second example demonstrates how to use [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse_NewtonSoftJson)]

<span data-ttu-id="10a6c-212">在範例應用程式中，將*CustomWriterStartup.cs*中的 `SYSTEM_TEXT_JSON`[預處理器](xref:index#preprocessor-directives-in-sample-code)指示詞批註，以啟用 `Newtonsoft.Json` 版本的 `WriteResponse`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-212">In the sample app, comment out the `SYSTEM_TEXT_JSON` [preprocessor directive](xref:index#preprocessor-directives-in-sample-code) in *CustomWriterStartup.cs* to enable the `Newtonsoft.Json` version of `WriteResponse`.</span></span>

<span data-ttu-id="10a6c-213">健康情況檢查 API 不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。</span><span class="sxs-lookup"><span data-stu-id="10a6c-213">The health checks API doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="10a6c-214">視需要自訂上述範例中的回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-214">Customize the response in the preceding examples as needed.</span></span> <span data-ttu-id="10a6c-215">如需有關使用 `System.Text.Json`進行 JSON 序列化的詳細資訊，請參閱[如何在 .net 中序列化和](/dotnet/standard/serialization/system-text-json-how-to)還原序列化 json。</span><span class="sxs-lookup"><span data-stu-id="10a6c-215">For more information on JSON serialization with `System.Text.Json`, see [How to serialize and deserialize JSON in .NET](/dotnet/standard/serialization/system-text-json-how-to).</span></span>

## <a name="database-probe"></a><span data-ttu-id="10a6c-216">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-216">Database probe</span></span>

<span data-ttu-id="10a6c-217">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-217">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="10a6c-218">範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-218">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="10a6c-219">`AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="10a6c-219">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="10a6c-220">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="10a6c-220">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="10a6c-221">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="10a6c-221">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="10a6c-222">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="10a6c-222">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="10a6c-223">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-223">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="10a6c-224">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-224">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="10a6c-225">包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="10a6c-225">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="10a6c-226">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="10a6c-226">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="10a6c-227">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="10a6c-227">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/3.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="10a6c-228">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-228">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-229">範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="10a6c-229">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="10a6c-230">健康狀態檢查端點是藉由呼叫 `Startup.Configure`中的 `MapHealthChecks` 所建立：</span><span class="sxs-lookup"><span data-stu-id="10a6c-230">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="10a6c-231">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-231">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="10a6c-232">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-232">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="10a6c-233">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-233">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="10a6c-234">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="10a6c-234">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="10a6c-235">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="10a6c-235">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="10a6c-236">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-236">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="10a6c-237">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="10a6c-237">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="10a6c-238">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-238">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="10a6c-239">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="10a6c-239">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="10a6c-240">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="10a6c-240">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="10a6c-241">根據預設：</span><span class="sxs-lookup"><span data-stu-id="10a6c-241">By default:</span></span>

* <span data-ttu-id="10a6c-242">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="10a6c-242">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="10a6c-243">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="10a6c-243">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="10a6c-244">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="10a6c-244">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="10a6c-245">在範例應用程式中，`AppDbContext` 提供給 `AddDbContextCheck`，並在 `Startup.ConfigureServices` （*DbCoNtextHealthStartup.cs*）中註冊為服務：</span><span class="sxs-lookup"><span data-stu-id="10a6c-245">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="10a6c-246">健康狀態檢查端點是藉由呼叫 `Startup.Configure`中的 `MapHealthChecks` 所建立：</span><span class="sxs-lookup"><span data-stu-id="10a6c-246">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health");
}
```

<span data-ttu-id="10a6c-247">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-247">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="10a6c-248">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="10a6c-248">If the database exists, delete it.</span></span>

<span data-ttu-id="10a6c-249">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-249">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="10a6c-250">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-250">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="10a6c-251">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-251">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="10a6c-252">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-252">Trigger the sample app to create the database.</span></span> <span data-ttu-id="10a6c-253">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-253">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="10a6c-254">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-254">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="10a6c-255">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-255">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="10a6c-256">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-256">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="10a6c-257">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-257">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="10a6c-258">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-258">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="10a6c-259">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-259">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="10a6c-260">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-260">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="10a6c-261">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-261">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="10a6c-262">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-262">Separate readiness and liveness probes</span></span>

<span data-ttu-id="10a6c-263">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-263">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="10a6c-264">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-264">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="10a6c-265">此狀態是指應用程式的「整備度」。</span><span class="sxs-lookup"><span data-stu-id="10a6c-265">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="10a6c-266">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-266">The app is functioning and responding to requests.</span></span> <span data-ttu-id="10a6c-267">此狀態是指應用程式的「活躍度」。</span><span class="sxs-lookup"><span data-stu-id="10a6c-267">This state is the app's *liveness*.</span></span>

<span data-ttu-id="10a6c-268">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-268">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="10a6c-269">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-269">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="10a6c-270">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="10a6c-270">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="10a6c-271">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="10a6c-271">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="10a6c-272">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-272">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="10a6c-273">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-273">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="10a6c-274">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="10a6c-274">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="10a6c-275">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="10a6c-275">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="10a6c-276">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="10a6c-276">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="10a6c-277">健康狀態檢查端點是藉由呼叫 `Startup.Configure`中的 `MapHealthChecks` 所建立。</span><span class="sxs-lookup"><span data-stu-id="10a6c-277">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="10a6c-278">在範例應用程式中，健康狀態檢查端點會建立于：</span><span class="sxs-lookup"><span data-stu-id="10a6c-278">In the sample app, the health check endpoints are created at:</span></span>

* <span data-ttu-id="10a6c-279">準備就緒檢查的 `/health/ready`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-279">`/health/ready` for the readiness check.</span></span> <span data-ttu-id="10a6c-280">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-280">The readiness check filters health checks to the health check with the `ready` tag.</span></span>
* <span data-ttu-id="10a6c-281">活動檢查的 `/health/live`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-281">`/health/live` for the liveness check.</span></span> <span data-ttu-id="10a6c-282">活動檢查會藉由傳回[HealthCheckOptions](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate)中的 `false` 來篩選出 `StartupHostedServiceHealthCheck` （如需詳細資訊，請參閱[篩選健全狀況檢查](#filter-health-checks)）</span><span class="sxs-lookup"><span data-stu-id="10a6c-282">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks))</span></span>

<span data-ttu-id="10a6c-283">在下列範例程式碼中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-283">In the following example code:</span></span>

* <span data-ttu-id="10a6c-284">準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-284">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="10a6c-285">`Predicate` 排除所有檢查，並傳回 200-Ok。</span><span class="sxs-lookup"><span data-stu-id="10a6c-285">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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

<span data-ttu-id="10a6c-286">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-286">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="10a6c-287">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="10a6c-287">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="10a6c-288">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="10a6c-288">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="10a6c-289">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="10a6c-289">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="10a6c-290">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-290">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="10a6c-291">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="10a6c-291">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="10a6c-292">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="10a6c-292">Kubernetes example</span></span>

<span data-ttu-id="10a6c-293">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-293">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="10a6c-294">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-294">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="10a6c-295">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="10a6c-295">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="10a6c-296">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-296">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="10a6c-297">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-297">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="10a6c-298">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-298">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="10a6c-299">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-299">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="10a6c-300">如果應用程式使用超過指定的記憶體閾值 (在範例應用程式中為 1 GB)，`MemoryHealthCheck` 會報告降級的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-300">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="10a6c-301"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-301">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="10a6c-302">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-302">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-303">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-303">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="10a6c-304">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-304">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="10a6c-305">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-305">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="10a6c-306">在範例應用程式的*CustomWriterStartup.cs*中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-306">In *CustomWriterStartup.cs* of the sample app:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="10a6c-307">健康狀態檢查端點是藉由呼叫 `Startup.Configure`中的 `MapHealthChecks` 所建立。</span><span class="sxs-lookup"><span data-stu-id="10a6c-307">A health check endpoint is created by calling `MapHealthChecks` in `Startup.Configure`.</span></span> <span data-ttu-id="10a6c-308">當健康情況檢查執行時，會提供 `WriteResponse` 委派給 < HealthChecks > HealthCheckOptions，以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-308">A `WriteResponse` delegate is provided to the <Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> property to output a custom JSON response when the health check executes:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapHealthChecks("/health", new HealthCheckOptions()
    {
        ResponseWriter = WriteResponse
    });
}
```

<span data-ttu-id="10a6c-309">`WriteResponse` 委派會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生健康情況檢查回應的 JSON 輸出。</span><span class="sxs-lookup"><span data-stu-id="10a6c-309">The `WriteResponse` delegate formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response.</span></span> <span data-ttu-id="10a6c-310">如需詳細資訊，請參閱[自訂輸出](#customize-output)一節。</span><span class="sxs-lookup"><span data-stu-id="10a6c-310">For more information, see the [Customize output](#customize-output) section.</span></span>

<span data-ttu-id="10a6c-311">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-311">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="10a6c-312">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-312">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="10a6c-313">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-313">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="10a6c-314">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="10a6c-314">Filter by port</span></span>

<span data-ttu-id="10a6c-315">在具有 URL 模式的 `MapHealthChecks` 上呼叫 `RequireHost`，以指定埠以限制對指定埠的健康狀態檢查要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-315">Call `RequireHost` on `MapHealthChecks` with a URL pattern that specifies a port to restrict health check requests to the port specified.</span></span> <span data-ttu-id="10a6c-316">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-316">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="10a6c-317">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-317">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="10a6c-318">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="10a6c-318">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="10a6c-319">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-319">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="10a6c-320">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="10a6c-320">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="10a6c-321">範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：</span><span class="sxs-lookup"><span data-stu-id="10a6c-321">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="10a6c-322">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-322">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-323">藉由呼叫 `Startup.Configure`中的 `MapHealthChecks` 來建立健康狀態檢查端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-323">Create a health check endpoint by calling `MapHealthChecks` in `Startup.Configure`.</span></span>

<span data-ttu-id="10a6c-324">在範例應用程式中，`Startup.Configure` 中端點的 `RequireHost` 呼叫會從設定中指定管理埠：</span><span class="sxs-lookup"><span data-stu-id="10a6c-324">In the sample app, a call to `RequireHost` on the endpoint in `Startup.Configure` specifies the management port from configuration:</span></span>

```csharp
endpoints.MapHealthChecks("/health")
    .RequireHost($"*:{Configuration["ManagementPort"]}");
```

<span data-ttu-id="10a6c-325">端點會在 `Startup.Configure`的範例應用程式中建立。</span><span class="sxs-lookup"><span data-stu-id="10a6c-325">Endpoints are created in the sample app in `Startup.Configure`.</span></span> <span data-ttu-id="10a6c-326">在下列範例程式碼中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-326">In the following example code:</span></span>

* <span data-ttu-id="10a6c-327">準備就緒檢查會將所有已註冊的檢查與「就緒」標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-327">The readiness check uses all registered checks with the 'ready' tag.</span></span>
* <span data-ttu-id="10a6c-328">`Predicate` 排除所有檢查，並傳回 200-Ok。</span><span class="sxs-lookup"><span data-stu-id="10a6c-328">The `Predicate` excludes all checks and return a 200-Ok.</span></span>

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
> <span data-ttu-id="10a6c-329">您可以在程式碼中明確設定管理埠，以避免在範例應用程式中建立*launchsettings.json。*</span><span class="sxs-lookup"><span data-stu-id="10a6c-329">You can avoid creating the *launchSettings.json* file in the sample app by setting the management port explicitly in code.</span></span> <span data-ttu-id="10a6c-330">在建立 <xref:Microsoft.Extensions.Hosting.HostBuilder> 的*Program.cs*中，新增對 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> 的呼叫，並提供應用程式的管理埠端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-330">In *Program.cs* where the <xref:Microsoft.Extensions.Hosting.HostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenAnyIP*> and provide the app's management port endpoint.</span></span> <span data-ttu-id="10a6c-331">在*ManagementPortStartup.cs*的 `Configure` 中，指定具有 `RequireHost`的管理埠：</span><span class="sxs-lookup"><span data-stu-id="10a6c-331">In `Configure` of *ManagementPortStartup.cs*, specify the management port with `RequireHost`:</span></span>
>
> <span data-ttu-id="10a6c-332">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="10a6c-332">*Program.cs*:</span></span>
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
> <span data-ttu-id="10a6c-333">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="10a6c-333">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseEndpoints(endpoints =>
> {
>     endpoints.MapHealthChecks("/health").RequireHost("*:5001");
> });
> ```

<span data-ttu-id="10a6c-334">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-334">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="10a6c-335">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="10a6c-335">Distribute a health check library</span></span>

<span data-ttu-id="10a6c-336">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="10a6c-336">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="10a6c-337">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-337">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="10a6c-338">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="10a6c-338">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="10a6c-339">在健康情況檢查中，`CheckHealthAsync`的邏輯：</span><span class="sxs-lookup"><span data-stu-id="10a6c-339">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="10a6c-340">方法中會使用 `data1` 和 `data2` 來執行探查的健康情況檢查邏輯。</span><span class="sxs-lookup"><span data-stu-id="10a6c-340">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="10a6c-341">會處理 `AccessViolationException`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-341">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="10a6c-342">當發生 <xref:System.AccessViolationException> 時，會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus>，並提供 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，讓使用者能夠設定健康情況檢查失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-342">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="10a6c-343">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="10a6c-343">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="10a6c-344">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="10a6c-344">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="10a6c-345">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="10a6c-345">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="10a6c-346">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-346">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="10a6c-347">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="10a6c-347">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="10a6c-348">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-348">health check name (`name`).</span></span> <span data-ttu-id="10a6c-349">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-349">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="10a6c-350">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-350">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="10a6c-351">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-351">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="10a6c-352">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-352">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="10a6c-353">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-353">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="10a6c-354">預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-354">The default is `null`.</span></span> <span data-ttu-id="10a6c-355">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-355">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="10a6c-356">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-356">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="10a6c-357">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="10a6c-357">Health Check Publisher</span></span>

<span data-ttu-id="10a6c-358">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="10a6c-358">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="10a6c-359">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-359">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="10a6c-360"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="10a6c-360">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="10a6c-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="10a6c-361"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="10a6c-362"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 應用程式啟動後，在執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實例之前所套用的初始延遲。</span><span class="sxs-lookup"><span data-stu-id="10a6c-362"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="10a6c-363">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="10a6c-363">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="10a6c-364">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="10a6c-364">The default value is five seconds.</span></span>
* <span data-ttu-id="10a6c-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。</span><span class="sxs-lookup"><span data-stu-id="10a6c-365"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="10a6c-366">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="10a6c-366">The default value is 30 seconds.</span></span>
* <span data-ttu-id="10a6c-367"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 是 `null` （預設值），健全狀況檢查發行者服務就會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-367"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="10a6c-368">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-368">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="10a6c-369">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="10a6c-369">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="10a6c-370"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實例健全狀況檢查的超時時間。</span><span class="sxs-lookup"><span data-stu-id="10a6c-370"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="10a6c-371">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="10a6c-371">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="10a6c-372">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="10a6c-372">The default value is 30 seconds.</span></span>

<span data-ttu-id="10a6c-373">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-373">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="10a6c-374">會針對記錄層級的每個檢查記錄健全狀況檢查狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-374">The health check status is logged for each check at a log level of:</span></span>

* <span data-ttu-id="10a6c-375">如果健康情況檢查狀態為 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>，則為資訊（<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>）。</span><span class="sxs-lookup"><span data-stu-id="10a6c-375">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="10a6c-376">如果狀態為 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> 或 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>，則為錯誤（<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>）。</span><span class="sxs-lookup"><span data-stu-id="10a6c-376">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="10a6c-377">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-377">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="10a6c-378">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="10a6c-378">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/3.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

> [!NOTE]
> <span data-ttu-id="10a6c-379">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-379">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="10a6c-380">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-380">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="10a6c-381">利用 MapWhen 限制健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-381">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="10a6c-382">使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="10a6c-382">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="10a6c-383">在下列範例中，如果收到 `api/HealthCheck` 端點的 GET 要求，`MapWhen` 會將要求管線分支以啟動健康狀態檢查中介軟體：</span><span class="sxs-lookup"><span data-stu-id="10a6c-383">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

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

<span data-ttu-id="10a6c-384">如需詳細資訊，請參閱<xref:fundamentals/middleware/index#use-run-and-map>。</span><span class="sxs-lookup"><span data-stu-id="10a6c-384">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="10a6c-385">ASP.NET Core 提供健康狀態檢查中介軟體和程式庫，以報告應用程式基礎結構元件的健全狀況。</span><span class="sxs-lookup"><span data-stu-id="10a6c-385">ASP.NET Core offers Health Checks Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="10a6c-386">應用程式會將健康狀態檢查公開為 HTTP 端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-386">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="10a6c-387">您可以針對各種即時監控案例來設定健康狀態檢查端點：</span><span class="sxs-lookup"><span data-stu-id="10a6c-387">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="10a6c-388">容器協調器和負載平衡器可以使用健康狀態探查，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-388">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="10a6c-389">例如，容器協調器可能會暫停輪流部署或重新啟動容器，來回應失敗的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-389">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="10a6c-390">負載平衡器可能會將流量從失敗的執行個體路由傳送至狀況良好的執行個體，來回應狀況不良的應用程式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-390">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="10a6c-391">您可以監控所使用記憶體、磁碟及其他實體伺服器資源的健康狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-391">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="10a6c-392">健康狀態檢查可以測試應用程式的相依性 (例如資料庫和外部服務端點)，確認其是否可用且正常運作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-392">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="10a6c-393">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10a6c-393">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="10a6c-394">範例應用程式包含本主題中所述的案例範例。</span><span class="sxs-lookup"><span data-stu-id="10a6c-394">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="10a6c-395">若要在指定的案例中執行範例應用程式，請在命令殼層中使用來自專案資料夾的 [dotnet run](/dotnet/core/tools/dotnet-run) 命令。</span><span class="sxs-lookup"><span data-stu-id="10a6c-395">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="10a6c-396">如需如何使用範例應用程式的詳細資訊，請參閱範例應用程式的 *README.md* 檔案和本主題中的案例描述。</span><span class="sxs-lookup"><span data-stu-id="10a6c-396">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10a6c-397">必要條件：</span><span class="sxs-lookup"><span data-stu-id="10a6c-397">Prerequisites</span></span>

<span data-ttu-id="10a6c-398">健康狀態檢查通常會搭配使用外部監視服務或容器協調器，來檢查應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-398">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="10a6c-399">將健康狀態檢查新增至應用程式之前，請決定要使用的監控系統。</span><span class="sxs-lookup"><span data-stu-id="10a6c-399">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="10a6c-400">監控系統會指定要建立哪些健康狀態檢查類型，以及如何設定其端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-400">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="10a6c-401">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="10a6c-401">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="10a6c-402">範例應用程式提供啟動程式碼，來示範數個案例的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-402">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="10a6c-403">[資料庫探查](#database-probe)案例會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 來檢查資料庫連線的健康情況。</span><span class="sxs-lookup"><span data-stu-id="10a6c-403">The [database probe](#database-probe) scenario checks the health of a database connection using [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks).</span></span> <span data-ttu-id="10a6c-404">[DbContext 探查](#entity-framework-core-dbcontext-probe)案例使用 EF Core `DbContext` 來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-404">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="10a6c-405">為了探索資料庫案例，範例應用程式會：</span><span class="sxs-lookup"><span data-stu-id="10a6c-405">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="10a6c-406">建立資料庫，並在 *appsettings.json* 檔案中提供其連接字串。</span><span class="sxs-lookup"><span data-stu-id="10a6c-406">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="10a6c-407">在其專案檔中具有下列套件參考：</span><span class="sxs-lookup"><span data-stu-id="10a6c-407">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="10a6c-408">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="10a6c-408">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="10a6c-409">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="10a6c-409">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="10a6c-410">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-410">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="10a6c-411">另一個健康狀態檢查案例示範如何將健康狀態檢查篩選至管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-411">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="10a6c-412">範例應用程式會要求您建立 *Properties/launchSettings.json* 檔案，其中包含管理 URL 和管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-412">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="10a6c-413">如需詳細資訊，請參閱[依連接埠篩選](#filter-by-port)一節。</span><span class="sxs-lookup"><span data-stu-id="10a6c-413">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="10a6c-414">基本健康狀態探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-414">Basic health probe</span></span>

<span data-ttu-id="10a6c-415">對於許多應用程式，報告應用程式是否可處理要求的基本健康狀態探查組態 (「活躍度」)，便足以探索應用程式的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-415">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="10a6c-416">基本設定會註冊健康情況檢查服務，並呼叫健全狀況檢查中介軟體，以回應具有健康情況回應的 URL 端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-416">The basic configuration registers health check services and calls the Health Checks Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="10a6c-417">預設並未登錄特定健康狀態檢查來測試任何特定相依性或子系統。</span><span class="sxs-lookup"><span data-stu-id="10a6c-417">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="10a6c-418">如果應用程式能夠在健康狀態端點 URL 做出回應，則視為狀況良好。</span><span class="sxs-lookup"><span data-stu-id="10a6c-418">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="10a6c-419">預設回應寫入器會將狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) 以純文字回應形式回寫到用戶端，指出狀態為 [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)[HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) 或 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-419">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="10a6c-420">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-420">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-421">使用 `Startup.Configure`的要求處理管線中的 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>，為健康狀態檢查中介軟體新增端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-421">Add an endpoint for Health Checks Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="10a6c-422">在範例應用程式中，健康狀態檢查端點是在 `/health` (*BasicStartup.cs*) 建立：</span><span class="sxs-lookup"><span data-stu-id="10a6c-422">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

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

<span data-ttu-id="10a6c-423">若要使用範例應用程式執行基本組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-423">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="10a6c-424">Docker 範例</span><span class="sxs-lookup"><span data-stu-id="10a6c-424">Docker example</span></span>

<span data-ttu-id="10a6c-425">[Docker](xref:host-and-deploy/docker/index) 提供內建 `HEALTHCHECK` 指示詞，可用來檢查使用基本健康狀態檢查組態的應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-425">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="10a6c-426">建立健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-426">Create health checks</span></span>

<span data-ttu-id="10a6c-427">健康狀態檢查是藉由實作 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="10a6c-427">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="10a6c-428"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> 方法會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，指出健康狀態為 `Healthy`、`Degraded` 或 `Unhealthy`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-428">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="10a6c-429">結果會寫成具有可設定狀態碼的純文字回應 ([健康狀態檢查選項](#health-check-options)一節中將說明如何進行組態)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-429">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="10a6c-430"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 也可以傳回選擇性索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="10a6c-430"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="10a6c-431">範例健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-431">Example health check</span></span>

<span data-ttu-id="10a6c-432">下列 `ExampleHealthCheck` 類別示範健全狀況檢查的配置。</span><span class="sxs-lookup"><span data-stu-id="10a6c-432">The following `ExampleHealthCheck` class demonstrates the layout of a health check.</span></span> <span data-ttu-id="10a6c-433">健康情況檢查邏輯會放在 `CheckHealthAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="10a6c-433">The health checks logic is placed in the `CheckHealthAsync` method.</span></span> <span data-ttu-id="10a6c-434">下列範例會將虛擬變數 `healthCheckResultHealthy`設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-434">The following example sets a dummy variable, `healthCheckResultHealthy`, to `true`.</span></span> <span data-ttu-id="10a6c-435">如果 `healthCheckResultHealthy` 的值設定為 `false`，則會傳回[HealthCheckResult 狀況不良](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*)狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-435">If the value of `healthCheckResultHealthy` is set to `false`, the [HealthCheckResult.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult.Unhealthy*) status is returned.</span></span>

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

### <a name="register-health-check-services"></a><span data-ttu-id="10a6c-436">登錄健康狀態檢查服務</span><span class="sxs-lookup"><span data-stu-id="10a6c-436">Register health check services</span></span>

<span data-ttu-id="10a6c-437">`ExampleHealthCheck` 類型會使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>新增至 `Startup.ConfigureServices` 中的健康狀態檢查服務：</span><span class="sxs-lookup"><span data-stu-id="10a6c-437">The `ExampleHealthCheck` type is added to health check services in `Startup.ConfigureServices` with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>("example_health_check");
```

<span data-ttu-id="10a6c-438">下列範例中所顯示的 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 多載會設定在健康狀態檢查報告失敗時所要報告的失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-438">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="10a6c-439">如果將失敗狀態設定為 `null` (預設)，則會回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-439">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="10a6c-440">此多載對程式庫作者很有用。若健康狀態檢查實作採用此設定，則當健康狀態檢查失敗時，應用程式就會強制程式庫指出失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-440">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="10a6c-441">您可以使用「標籤」來篩選健康狀態檢查 ([篩選健康狀態檢查](#filter-health-checks)一節中將進一步說明)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-441">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check",
        failureStatus: HealthStatus.Degraded,
        tags: new[] { "example" });
```

<span data-ttu-id="10a6c-442"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 也可以執行匿名函式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-442"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="10a6c-443">在下列 `Startup.ConfigureServices` 範例中，健康情況檢查名稱指定為 `Example`，而檢查一律會傳回狀況良好狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-443">In the following `Startup.ConfigureServices` example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck("Example", () =>
        HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" });
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="10a6c-444">使用健康狀態檢查中介軟體</span><span class="sxs-lookup"><span data-stu-id="10a6c-444">Use Health Checks Middleware</span></span>

<span data-ttu-id="10a6c-445">在 `Startup.Configure` 中，使用端點 URL 或相對路徑呼叫處理管線中的 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>：</span><span class="sxs-lookup"><span data-stu-id="10a6c-445">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="10a6c-446">如果健康狀態檢查應該接聽特定連接埠，請使用 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 的多載來設定連接埠 ([依連接埠篩選](#filter-by-port)一節中將進一步說明)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-446">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

## <a name="health-check-options"></a><span data-ttu-id="10a6c-447">健康狀態檢查選項</span><span class="sxs-lookup"><span data-stu-id="10a6c-447">Health check options</span></span>

<span data-ttu-id="10a6c-448"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> 讓您有機會自訂健康狀態檢查行為：</span><span class="sxs-lookup"><span data-stu-id="10a6c-448"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="10a6c-449">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-449">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="10a6c-450">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="10a6c-450">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="10a6c-451">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="10a6c-451">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="10a6c-452">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="10a6c-452">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="10a6c-453">篩選健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-453">Filter health checks</span></span>

<span data-ttu-id="10a6c-454">根據預設，健康情況檢查中介軟體會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-454">By default, Health Checks Middleware runs all registered health checks.</span></span> <span data-ttu-id="10a6c-455">若要執行健康狀態檢查子集，請對 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> 選項提供傳回布林值的函式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-455">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="10a6c-456">在下列範例中，會依函式條件陳述式中的標籤 (`bar_tag`) 篩選出 `Bar` 健康狀態檢查。只有在健康狀態檢查的 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> 屬性符合 `foo_tag` 或 `baz_tag` 時，才傳回 `true`：</span><span class="sxs-lookup"><span data-stu-id="10a6c-456">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

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

### <a name="customize-the-http-status-code"></a><span data-ttu-id="10a6c-457">自訂 HTTP 狀態碼</span><span class="sxs-lookup"><span data-stu-id="10a6c-457">Customize the HTTP status code</span></span>

<span data-ttu-id="10a6c-458">您可以使用 <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> 來自訂健康狀態與 HTTP 狀態碼的對應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-458">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="10a6c-459">下列 <xref:Microsoft.AspNetCore.Http.StatusCodes> 指派是中介軟體所使用的預設值。</span><span class="sxs-lookup"><span data-stu-id="10a6c-459">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="10a6c-460">請變更狀態碼值以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-460">Change the status code values to meet your requirements.</span></span>

<span data-ttu-id="10a6c-461">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-461">In `Startup.Configure`:</span></span>

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

### <a name="suppress-cache-headers"></a><span data-ttu-id="10a6c-462">隱藏快取標頭</span><span class="sxs-lookup"><span data-stu-id="10a6c-462">Suppress cache headers</span></span>

<span data-ttu-id="10a6c-463"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> 控制健全狀況檢查中介軟體是否將 HTTP 標頭新增至探查回應，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="10a6c-463"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Checks Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="10a6c-464">如果值為 `false` (預設)，則中介軟體會設定或覆寫 `Cache-Control`、`Expires` 和 `Pragma` 標頭，以防止回應快取。</span><span class="sxs-lookup"><span data-stu-id="10a6c-464">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="10a6c-465">如果值為 `true`，則中介軟體不會修改回應的快取標頭。</span><span class="sxs-lookup"><span data-stu-id="10a6c-465">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

<span data-ttu-id="10a6c-466">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-466">In `Startup.Configure`:</span></span>

```csharp
//using Microsoft.AspNetCore.Diagnostics.HealthChecks;
//using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    AllowCachingResponses = false
});
```

### <a name="customize-output"></a><span data-ttu-id="10a6c-467">自訂輸出</span><span class="sxs-lookup"><span data-stu-id="10a6c-467">Customize output</span></span>

<span data-ttu-id="10a6c-468"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> 選項會取得或設定用來寫入回應的委派。</span><span class="sxs-lookup"><span data-stu-id="10a6c-468">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="10a6c-469">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-469">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

<span data-ttu-id="10a6c-470">在 `Startup.Configure`中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-470">In `Startup.Configure`:</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics.HealthChecks;
// using Microsoft.Extensions.Diagnostics.HealthChecks;

app.UseHealthChecks("/health", new HealthCheckOptions()
{
    ResponseWriter = WriteResponse
});
```

<span data-ttu-id="10a6c-471">預設委派會使用 [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status)的字串值撰寫最基本的純文字回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-471">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span> <span data-ttu-id="10a6c-472">下列自訂委派（`WriteResponse`）會輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-472">The following custom delegate, `WriteResponse`, outputs a custom JSON response:</span></span>

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

<span data-ttu-id="10a6c-473">健全狀況檢查系統不會針對複雜的 JSON 傳回格式提供內建支援，因為此格式是您選擇的監視系統所特有。</span><span class="sxs-lookup"><span data-stu-id="10a6c-473">The health checks system doesn't provide built-in support for complex JSON return formats because the format is specific to your choice of monitoring system.</span></span> <span data-ttu-id="10a6c-474">您可以視需要自訂上述範例中的 `JObject`，以符合您的需求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-474">Feel free to customize the `JObject` in the preceding example as necessary to meet your needs.</span></span>

## <a name="database-probe"></a><span data-ttu-id="10a6c-475">資料庫探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-475">Database probe</span></span>

<span data-ttu-id="10a6c-476">健康狀態檢查可指定資料庫查詢以布林測試方式來執行，藉此指出資料庫是否正常回應。</span><span class="sxs-lookup"><span data-stu-id="10a6c-476">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="10a6c-477">範例應用程式會使用 [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) (此為適用於 ASP.NET Core 應用程式的健康情況檢查程式庫)，在 SQL Server 資料庫上執行健康情況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-477">The sample app uses [AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="10a6c-478">`AspNetCore.Diagnostics.HealthChecks` 會對資料庫執行 `SELECT 1` 查詢，以確認資料庫連線狀況良好。</span><span class="sxs-lookup"><span data-stu-id="10a6c-478">`AspNetCore.Diagnostics.HealthChecks` executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="10a6c-479">使用查詢檢查資料庫連線時，請選擇快速傳回的查詢。</span><span class="sxs-lookup"><span data-stu-id="10a6c-479">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="10a6c-480">查詢方法具有多載資料庫而降低其效能的風險。</span><span class="sxs-lookup"><span data-stu-id="10a6c-480">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="10a6c-481">在大部分情況下，不需要執行測試查詢。</span><span class="sxs-lookup"><span data-stu-id="10a6c-481">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="10a6c-482">只要成功建立資料庫連線就已足夠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-482">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="10a6c-483">如果您發現有必要執行查詢，請選擇簡單的 SELECT 查詢，例如 `SELECT 1`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-483">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="10a6c-484">包含 [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="10a6c-484">Include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="10a6c-485">在範例應用程式的 *appsettings.json* 檔案中提供有效的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="10a6c-485">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="10a6c-486">應用程式使用名為 `HealthCheckSample` 的 SQL Server 資料庫：</span><span class="sxs-lookup"><span data-stu-id="10a6c-486">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="10a6c-487">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-487">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-488">範例應用程式會使用資料庫的連接字串 (*DbHealthStartup.cs*) 來呼叫 `AddSqlServer` 方法：</span><span class="sxs-lookup"><span data-stu-id="10a6c-488">The sample app calls the `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="10a6c-489">在 `Startup.Configure`中呼叫應用程式處理管線中的「健康情況檢查」中介軟體：</span><span class="sxs-lookup"><span data-stu-id="10a6c-489">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="10a6c-490">若要使用範例應用程式執行資料庫探查案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-490">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="10a6c-491">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-491">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="10a6c-492">Entity Framework Core DbContext 探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-492">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="10a6c-493">`DbContext` 檢查會確認應用程式是否可與針對 EF Core `DbContext` 設定的資料庫通訊。</span><span class="sxs-lookup"><span data-stu-id="10a6c-493">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="10a6c-494">應用程式支援 `DbContext` 檢查：</span><span class="sxs-lookup"><span data-stu-id="10a6c-494">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="10a6c-495">使用 [Entity Framework (EF) Core](/ef/core/)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-495">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="10a6c-496">包含 [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="10a6c-496">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="10a6c-497">`AddDbContextCheck<TContext>` 會登錄 `DbContext` 的健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-497">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="10a6c-498">`DbContext` 會以 `TContext` 形式提供給方法。</span><span class="sxs-lookup"><span data-stu-id="10a6c-498">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="10a6c-499">多載可用來設定失敗狀態、標籤和自訂測試查詢。</span><span class="sxs-lookup"><span data-stu-id="10a6c-499">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="10a6c-500">根據預設：</span><span class="sxs-lookup"><span data-stu-id="10a6c-500">By default:</span></span>

* <span data-ttu-id="10a6c-501">`DbContextHealthCheck` 會呼叫 EF Core 的 `CanConnectAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="10a6c-501">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="10a6c-502">您可以自訂使用 `AddDbContextCheck` 方法多載檢查健康狀態時所要執行的作業。</span><span class="sxs-lookup"><span data-stu-id="10a6c-502">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="10a6c-503">健康狀態檢查的名稱是 `TContext` 類型的名稱。</span><span class="sxs-lookup"><span data-stu-id="10a6c-503">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="10a6c-504">在範例應用程式中，`AppDbContext` 提供給 `AddDbContextCheck`，並在 `Startup.ConfigureServices` （*DbCoNtextHealthStartup.cs*）中註冊為服務：</span><span class="sxs-lookup"><span data-stu-id="10a6c-504">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices` (*DbContextHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="10a6c-505">在範例應用程式中，`UseHealthChecks` 會在 `Startup.Configure`中新增健全狀況檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-505">In the sample app, `UseHealthChecks` adds the Health Checks Middleware in `Startup.Configure`.</span></span>

```csharp
app.UseHealthChecks("/health");
```

<span data-ttu-id="10a6c-506">若要使用範例應用程式來執行 `DbContext` 探查案例，請確認 SQL Server 執行個體中沒有連接字串所指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-506">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="10a6c-507">如果資料庫存在，請予以刪除。</span><span class="sxs-lookup"><span data-stu-id="10a6c-507">If the database exists, delete it.</span></span>

<span data-ttu-id="10a6c-508">在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-508">Execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario dbcontext
```

<span data-ttu-id="10a6c-509">在應用程式執行之後，對瀏覽器中的 `/health` 端點提出要求來檢查健康狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-509">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="10a6c-510">資料庫和 `AppDbContext` 不存在，因此應用程式會提供下列回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-510">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="10a6c-511">觸發範例應用程式以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-511">Trigger the sample app to create the database.</span></span> <span data-ttu-id="10a6c-512">對 `/createdatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-512">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="10a6c-513">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-513">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="10a6c-514">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-514">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="10a6c-515">資料庫和內容存在，因此應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-515">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="10a6c-516">觸發範例應用程式以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="10a6c-516">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="10a6c-517">對 `/deletedatabase` 提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-517">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="10a6c-518">應用程式會回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-518">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="10a6c-519">對 `/health` 端點提出要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-519">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="10a6c-520">應用程式會提供狀況不良回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-520">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="10a6c-521">個別的整備度與活躍度探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-521">Separate readiness and liveness probes</span></span>

<span data-ttu-id="10a6c-522">在某些主控案例中，使用一組健康狀態檢查來區分兩個應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-522">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="10a6c-523">應用程式正常運作但尚未準備好接收要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-523">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="10a6c-524">此狀態是指應用程式的「整備度」。</span><span class="sxs-lookup"><span data-stu-id="10a6c-524">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="10a6c-525">應用程式正常運作並回應要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-525">The app is functioning and responding to requests.</span></span> <span data-ttu-id="10a6c-526">此狀態是指應用程式的「活躍度」。</span><span class="sxs-lookup"><span data-stu-id="10a6c-526">This state is the app's *liveness*.</span></span>

<span data-ttu-id="10a6c-527">整備度檢查通常會執行一組更廣泛且耗時的檢查，以判斷應用程式的所有子系統和資源是否都可供使用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-527">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="10a6c-528">活躍度檢查只會執行快速檢查，以判斷應用程式是否可處理要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-528">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="10a6c-529">當應用程式通過其整備度檢查之後，就不需要再對應用程式執行這組耗費資源的整備度檢查&mdash;後續檢查只需要檢查活躍度。</span><span class="sxs-lookup"><span data-stu-id="10a6c-529">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="10a6c-530">範例應用程式包含健康狀態檢查，會在[託管服務](xref:fundamentals/host/hosted-services)中長時間執行的啟動工作完成時回報。</span><span class="sxs-lookup"><span data-stu-id="10a6c-530">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="10a6c-531">`StartupHostedServiceHealthCheck` 會公開屬性 `StartupTaskCompleted`。託管服務可在其長時間執行的工作完成時，將此屬性設定為 `true` (*StartupHostedServiceHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-531">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=7-11)]

<span data-ttu-id="10a6c-532">[託管服務](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*) 會啟動長時間執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-532">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="10a6c-533">工作完成時，`StartupHostedServiceHealthCheck.StartupTaskCompleted` 會設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="10a6c-533">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="10a6c-534">健康狀態檢查是 `Startup.ConfigureServices` 中使用 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 隨託管服務一起登錄。</span><span class="sxs-lookup"><span data-stu-id="10a6c-534">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="10a6c-535">由於託管服務必須在健康狀態檢查上設定此屬性，因此也會在服務容器 (*LivenessProbeStartup.cs*) 中登錄健康狀態檢查：</span><span class="sxs-lookup"><span data-stu-id="10a6c-535">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="10a6c-536">在 `Startup.Configure`的應用程式處理管線中呼叫健全狀況檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-536">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="10a6c-537">在範例應用程式中，健康狀態檢查端點是在 `/health/ready` (針對整備度檢查) 和 `/health/live` (針對活躍度檢查) 建立。</span><span class="sxs-lookup"><span data-stu-id="10a6c-537">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="10a6c-538">整備度檢查使用 `ready` 標籤來篩選健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-538">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="10a6c-539">活躍度檢查會藉由在 [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) 中傳回 `false` 來篩選掉 `StartupHostedServiceHealthCheck` (如需詳細資訊，請參閱[篩選健康狀態檢查](#filter-health-checks))：</span><span class="sxs-lookup"><span data-stu-id="10a6c-539">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

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

<span data-ttu-id="10a6c-540">若要使用範例應用程式執行整備度/活躍度組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-540">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario liveness
```

<span data-ttu-id="10a6c-541">在瀏覽器中，瀏覽 `/health/ready` 數次，直到經過 15 秒。</span><span class="sxs-lookup"><span data-stu-id="10a6c-541">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="10a6c-542">健康狀態檢查在前 15 秒會回報 *Unhealthy*。</span><span class="sxs-lookup"><span data-stu-id="10a6c-542">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="10a6c-543">15 秒之後，端點會回報 *Healthy*，這反映託管服務長時間執行的工作已完成。</span><span class="sxs-lookup"><span data-stu-id="10a6c-543">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="10a6c-544">此範例也會建立一個以 2 秒延遲執行第一次整備檢查的「健康狀態檢查發行者」(<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-544">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="10a6c-545">如需詳細資訊，請參閱[健康狀態檢查發行者](#health-check-publisher)一節。</span><span class="sxs-lookup"><span data-stu-id="10a6c-545">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="10a6c-546">Kubernetes 範例</span><span class="sxs-lookup"><span data-stu-id="10a6c-546">Kubernetes example</span></span>

<span data-ttu-id="10a6c-547">使用個別的整備度與活躍度檢查在 [Kubernetes](https://kubernetes.io/) 之類的環境中很有用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-547">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="10a6c-548">在 Kubernetes 中，應用程式可能需要執行耗時的啟動工作才能接受要求 (例如基礎資料庫可用性測試)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-548">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="10a6c-549">使用個別的檢查可讓協調器區分應用程式是否為正常運作但尚未準備好，或應用程式是否無法啟動。</span><span class="sxs-lookup"><span data-stu-id="10a6c-549">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="10a6c-550">如需 Kubernetes 中整備度與活躍度探查的詳細資訊，請參閱 Kubernetes 文件中的 [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) (設定活躍度與整備度探查)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-550">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="10a6c-551">下列範例示範 Kubernetes 整備度探查組態：</span><span class="sxs-lookup"><span data-stu-id="10a6c-551">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

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

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="10a6c-552">透過自訂回應寫入器的計量型探查</span><span class="sxs-lookup"><span data-stu-id="10a6c-552">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="10a6c-553">範例應用程式示範透過自訂回應寫入器的記憶體健康狀態檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-553">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="10a6c-554">如果應用程式使用超過指定的記憶體閾值（在範例應用程式中為 1 GB），`MemoryHealthCheck` 會回報狀況不良的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-554">`MemoryHealthCheck` reports an unhealthy status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="10a6c-555"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> 包含應用程式的記憶體回收行程 (GC) 資訊 (*MemoryHealthCheck.cs*)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-555">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="10a6c-556">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-556">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-557">`MemoryHealthCheck` 會登錄為服務，而不是將健康狀態檢查傳遞至 <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> 以啟用檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-557">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="10a6c-558">所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 登錄的服務都可供健康狀態檢查服務和中介軟體使用。</span><span class="sxs-lookup"><span data-stu-id="10a6c-558">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="10a6c-559">建議將健康狀態檢查服務登錄為單一服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-559">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="10a6c-560">在範例應用程式（*CustomWriterStartup.cs*）中：</span><span class="sxs-lookup"><span data-stu-id="10a6c-560">In the sample app (*CustomWriterStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="10a6c-561">在 `Startup.Configure`的應用程式處理管線中呼叫健全狀況檢查中介軟體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-561">Call Health Checks Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="10a6c-562">當健康狀態檢查執行時，會將 `WriteResponse` 委派提供給 `ResponseWriter` 屬性以輸出自訂 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-562">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

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

<span data-ttu-id="10a6c-563">`WriteResponse` 方法會將 `CompositeHealthCheckResult` 格式化為 JSON 物件，並產生 JSON 輸出作為健康狀態檢查回應：</span><span class="sxs-lookup"><span data-stu-id="10a6c-563">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="10a6c-564">若要使用範例應用程式透過自訂回應寫入器輸出執行計量型探查，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-564">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="10a6c-565">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附計量型健康情況檢查案例，包括磁碟儲存體和最大值活躍度檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-565">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="10a6c-566">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-566">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="10a6c-567">依連接埠篩選</span><span class="sxs-lookup"><span data-stu-id="10a6c-567">Filter by port</span></span>

<span data-ttu-id="10a6c-568">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 並提供連接埠會限制對指定的連接埠提出健康狀態檢查要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-568">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="10a6c-569">這通常會用於容器環境，以公開監視服務的連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-569">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="10a6c-570">範例應用程式使用[環境變數組態提供者](xref:fundamentals/configuration/index#environment-variables-configuration-provider)來設定連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-570">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="10a6c-571">連接埠是在 *launchSettings.json* 檔案中設定，並透過環境變數傳遞至組態提供者。</span><span class="sxs-lookup"><span data-stu-id="10a6c-571">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="10a6c-572">您也必須將伺服器設定為在管理連接埠上接聽要求。</span><span class="sxs-lookup"><span data-stu-id="10a6c-572">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="10a6c-573">若要使用範例應用程式示範管理連接埠組態，請在 *Properties* 資料夾中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="10a6c-573">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="10a6c-574">範例應用程式中的下列*Properties/launchsettings.json*檔案不包含在範例應用程式的專案檔中，必須手動建立：</span><span class="sxs-lookup"><span data-stu-id="10a6c-574">The following *Properties/launchSettings.json* file in the sample app isn't included in the sample app's project files and must be created manually:</span></span>

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

<span data-ttu-id="10a6c-575">在 `Startup.ConfigureServices` 中，使用 <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> 登錄健康狀態檢查服務。</span><span class="sxs-lookup"><span data-stu-id="10a6c-575">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="10a6c-576">呼叫 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 會指定管理連接埠 (*ManagementPortStartup.cs*)：</span><span class="sxs-lookup"><span data-stu-id="10a6c-576">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=17)]

> [!NOTE]
> <span data-ttu-id="10a6c-577">您可以在程式碼中明確設定 URL 和管理連接埠，避免在範例應用程式中建立 *launchSettings.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="10a6c-577">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="10a6c-578">在 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> 建立所在的 *Program.cs* 中，新增 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> 呼叫並提供應用程式的正常回應端點和管理連接埠端點。</span><span class="sxs-lookup"><span data-stu-id="10a6c-578">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="10a6c-579">在 <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> 呼叫所在的 *ManagementPortStartup.cs* 中，明確指定管理連接埠。</span><span class="sxs-lookup"><span data-stu-id="10a6c-579">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="10a6c-580">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="10a6c-580">*Program.cs*:</span></span>
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
> <span data-ttu-id="10a6c-581">*ManagementPortStartup.cs*：</span><span class="sxs-lookup"><span data-stu-id="10a6c-581">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="10a6c-582">若要使用範例應用程式執行管理連接埠組態案例，請在命令殼層中執行來自專案資料夾的下列命令：</span><span class="sxs-lookup"><span data-stu-id="10a6c-582">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```dotnetcli
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="10a6c-583">發佈健康狀態檢查程式庫</span><span class="sxs-lookup"><span data-stu-id="10a6c-583">Distribute a health check library</span></span>

<span data-ttu-id="10a6c-584">若要發佈健康狀態檢查作為程式庫：</span><span class="sxs-lookup"><span data-stu-id="10a6c-584">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="10a6c-585">寫入健康狀態檢查，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> 介面當做獨立類別來實作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-585">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="10a6c-586">此類別可能依賴[相依性插入 (DI)](xref:fundamentals/dependency-injection)、類型啟用和[具名選項](xref:fundamentals/configuration/options)來存取組態資料。</span><span class="sxs-lookup"><span data-stu-id="10a6c-586">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   <span data-ttu-id="10a6c-587">在健康情況檢查中，`CheckHealthAsync`的邏輯：</span><span class="sxs-lookup"><span data-stu-id="10a6c-587">In the health checks logic of `CheckHealthAsync`:</span></span>

   * <span data-ttu-id="10a6c-588">方法中會使用 `data1` 和 `data2` 來執行探查的健康情況檢查邏輯。</span><span class="sxs-lookup"><span data-stu-id="10a6c-588">`data1` and `data2` are used in the method to run the probe's health check logic.</span></span>
   * <span data-ttu-id="10a6c-589">會處理 `AccessViolationException`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-589">`AccessViolationException` is handled.</span></span>

   <span data-ttu-id="10a6c-590">當發生 <xref:System.AccessViolationException> 時，會傳回 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus>，並提供 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult>，讓使用者能夠設定健康情況檢查失敗狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-590">When an <xref:System.AccessViolationException> occurs, the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.FailureStatus> is returned with the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> to allow users to configure the health checks failure status.</span></span>

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

1. <span data-ttu-id="10a6c-591">使用取用應用程式在其 `Startup.Configure` 方法中呼叫的參數，來寫入延伸模組。</span><span class="sxs-lookup"><span data-stu-id="10a6c-591">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="10a6c-592">在下列範例中，假設健康狀態檢查方法簽章如下：</span><span class="sxs-lookup"><span data-stu-id="10a6c-592">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="10a6c-593">上述簽章指出 `ExampleHealthCheck` 需要額外的資料，才能處理健康狀態檢查探查邏輯。</span><span class="sxs-lookup"><span data-stu-id="10a6c-593">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="10a6c-594">此資料會提供給委派，以在使用延伸模組登錄健康狀態檢查時，用來建立健康狀態檢查執行個體。</span><span class="sxs-lookup"><span data-stu-id="10a6c-594">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="10a6c-595">在下列範例中，呼叫者會選擇性地指定：</span><span class="sxs-lookup"><span data-stu-id="10a6c-595">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="10a6c-596">健康狀態檢查名稱 (`name`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-596">health check name (`name`).</span></span> <span data-ttu-id="10a6c-597">如果為 `null`，則會使用 `example_health_check`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-597">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="10a6c-598">健康狀態檢查的字串資料點 (`data1`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-598">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="10a6c-599">健康狀態檢查的整數資料點 (`data2`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-599">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="10a6c-600">如果為 `null`，則會使用 `1`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-600">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="10a6c-601">失敗狀態 (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-601">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="10a6c-602">預設為 `null`。</span><span class="sxs-lookup"><span data-stu-id="10a6c-602">The default is `null`.</span></span> <span data-ttu-id="10a6c-603">如果為 `null`，就會針對失敗狀態回報 [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-603">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="10a6c-604">標籤 (`IEnumerable<string>`)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-604">tags (`IEnumerable<string>`).</span></span>

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

## <a name="health-check-publisher"></a><span data-ttu-id="10a6c-605">健康狀態檢查發行者</span><span class="sxs-lookup"><span data-stu-id="10a6c-605">Health Check Publisher</span></span>

<span data-ttu-id="10a6c-606">當 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 新增至服務容器時，健康狀態檢查系統會定期執行健康狀態檢查，並呼叫 `PublishAsync` 傳回結果。</span><span class="sxs-lookup"><span data-stu-id="10a6c-606">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="10a6c-607">這對推送型健康狀態監控系統案例很有用，其預期每個處理序會定期呼叫監控系統來判斷健康狀態。</span><span class="sxs-lookup"><span data-stu-id="10a6c-607">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="10a6c-608"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 介面有單一方法：</span><span class="sxs-lookup"><span data-stu-id="10a6c-608">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="10a6c-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> 可讓您設定：</span><span class="sxs-lookup"><span data-stu-id="10a6c-609"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="10a6c-610"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; 應用程式啟動後，在執行 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實例之前所套用的初始延遲。</span><span class="sxs-lookup"><span data-stu-id="10a6c-610"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="10a6c-611">在啟動後就會套用延遲，但不會套用至後續的反覆項目。</span><span class="sxs-lookup"><span data-stu-id="10a6c-611">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="10a6c-612">預設值是五秒鐘。</span><span class="sxs-lookup"><span data-stu-id="10a6c-612">The default value is five seconds.</span></span>
* <span data-ttu-id="10a6c-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行的期間。</span><span class="sxs-lookup"><span data-stu-id="10a6c-613"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="10a6c-614">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="10a6c-614">The default value is 30 seconds.</span></span>
* <span data-ttu-id="10a6c-615"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; 如果 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> 是 `null` （預設值），健全狀況檢查發行者服務就會執行所有已註冊的健全狀況檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-615"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="10a6c-616">若要執行一部分的健康狀態檢查，請提供可篩選該組檢查的函式。</span><span class="sxs-lookup"><span data-stu-id="10a6c-616">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="10a6c-617">每個期間都會評估該述詞。</span><span class="sxs-lookup"><span data-stu-id="10a6c-617">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="10a6c-618"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; 執行所有 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實例健全狀況檢查的超時時間。</span><span class="sxs-lookup"><span data-stu-id="10a6c-618"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="10a6c-619">若要在沒有逾時的情況下執行，請使用 <xref:System.Threading.Timeout.InfiniteTimeSpan>。</span><span class="sxs-lookup"><span data-stu-id="10a6c-619">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="10a6c-620">預設值為 30 秒。</span><span class="sxs-lookup"><span data-stu-id="10a6c-620">The default value is 30 seconds.</span></span>

> [!WARNING]
> <span data-ttu-id="10a6c-621">在 ASP.NET Core 2.2 版中，設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period>並不會獲得 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作遵守；它會設定 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> 的值。</span><span class="sxs-lookup"><span data-stu-id="10a6c-621">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="10a6c-622">ASP.NET Core 3.0 已解決此問題。</span><span class="sxs-lookup"><span data-stu-id="10a6c-622">This issue has been addressed in ASP.NET Core 3.0.</span></span>

<span data-ttu-id="10a6c-623">在範例應用程式中，`ReadinessPublisher` 是一個 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作。</span><span class="sxs-lookup"><span data-stu-id="10a6c-623">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="10a6c-624">健全狀況檢查狀態會針對每個檢查記錄為下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="10a6c-624">The health check status is logged for each check as either:</span></span>

* <span data-ttu-id="10a6c-625">如果健康情況檢查狀態為 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>，則為資訊（<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>）。</span><span class="sxs-lookup"><span data-stu-id="10a6c-625">Information (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation*>) if the health checks status is <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Healthy>.</span></span>
* <span data-ttu-id="10a6c-626">如果狀態為 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> 或 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>，則為錯誤（<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>）。</span><span class="sxs-lookup"><span data-stu-id="10a6c-626">Error (<xref:Microsoft.Extensions.Logging.LoggerExtensions.LogError*>) if the status is either <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Degraded> or <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus.Unhealthy>.</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=18-27)]

<span data-ttu-id="10a6c-627">在範例應用程式的 `LivenessProbeStartup` 範例中，`StartupHostedService` 整備檢查具有 2 秒的啟動延遲，且每 30 秒會執行一次檢查。</span><span class="sxs-lookup"><span data-stu-id="10a6c-627">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="10a6c-628">為了啟用 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 實作，此範例會在[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器中將 `ReadinessPublisher` 註冊為單一服務：</span><span class="sxs-lookup"><span data-stu-id="10a6c-628">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

> [!NOTE]
> <span data-ttu-id="10a6c-629">以下因應措施可允許在已將一或多個其他託管服務新增至應用程式的情況下，將 <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> 執行個體新增至服務容器。</span><span class="sxs-lookup"><span data-stu-id="10a6c-629">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="10a6c-630">ASP.NET Core 3.0 不需要此因應措施。</span><span class="sxs-lookup"><span data-stu-id="10a6c-630">This workaround won't isn't required in ASP.NET Core 3.0.</span></span>
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
> <span data-ttu-id="10a6c-631">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) \(英文\) 隨附數個系統的發行者，包括 [Application Insights](/azure/application-insights/app-insights-overview)。</span><span class="sxs-lookup"><span data-stu-id="10a6c-631">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="10a6c-632">[AspNetCore](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks)不是由 Microsoft 維護或支援 HealthChecks。</span><span class="sxs-lookup"><span data-stu-id="10a6c-632">[AspNetCore.Diagnostics.HealthChecks](https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks) isn't maintained or supported by Microsoft.</span></span>

## <a name="restrict-health-checks-with-mapwhen"></a><span data-ttu-id="10a6c-633">利用 MapWhen 限制健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="10a6c-633">Restrict health checks with MapWhen</span></span>

<span data-ttu-id="10a6c-634">使用 <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 來有條件地將健康情況檢查端點的要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="10a6c-634">Use <xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> to conditionally branch the request pipeline for health check endpoints.</span></span>

<span data-ttu-id="10a6c-635">在下列範例中，如果收到 `api/HealthCheck` 端點的 GET 要求，`MapWhen` 會將要求管線分支以啟動健康狀態檢查中介軟體：</span><span class="sxs-lookup"><span data-stu-id="10a6c-635">In the following example, `MapWhen` branches the request pipeline to activate Health Checks Middleware if a GET request is received for the `api/HealthCheck` endpoint:</span></span>

```csharp
app.MapWhen(
    context => context.Request.Method == HttpMethod.Get.Method && 
        context.Request.Path.StartsWith("/api/HealthCheck"),
    builder => builder.UseHealthChecks());

app.UseMvc();
```

<span data-ttu-id="10a6c-636">如需詳細資訊，請參閱<xref:fundamentals/middleware/index#use-run-and-map>。</span><span class="sxs-lookup"><span data-stu-id="10a6c-636">For more information, see <xref:fundamentals/middleware/index#use-run-and-map>.</span></span>

::: moniker-end
