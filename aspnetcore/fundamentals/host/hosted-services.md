---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688113"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="12766-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="12766-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="12766-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="12766-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="12766-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="12766-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="12766-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="12766-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="12766-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="12766-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="12766-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="12766-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="12766-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="12766-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="12766-110">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="12766-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="12766-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="12766-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="12766-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12766-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="12766-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="12766-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="12766-114">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="12766-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="12766-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="12766-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="12766-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="12766-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="12766-117">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="12766-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="12766-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="12766-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="12766-119">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="12766-119">Worker Service template</span></span>

<span data-ttu-id="12766-120">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="12766-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="12766-121">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="12766-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="12766-122">套件</span><span class="sxs-lookup"><span data-stu-id="12766-122">Package</span></span>

<span data-ttu-id="12766-123">針對 ASP.NET Core 應用程式，會隱含地新增對[Microsoft Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="12766-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="12766-124">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="12766-124">IHostedService interface</span></span>

<span data-ttu-id="12766-125"><xref:Microsoft.Extensions.Hosting.IHostedService>介面會針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="12766-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="12766-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="12766-127">`StartAsync`會*在之前*呼叫：</span><span class="sxs-lookup"><span data-stu-id="12766-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="12766-128">應用程式的要求處理管線已設定（`Startup.Configure`）。</span><span class="sxs-lookup"><span data-stu-id="12766-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="12766-129">伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="12766-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="12766-130">您可以變更預設行為，以便在應用程式的管線`StartAsync`已設定且`ApplicationStarted`呼叫之後，託管服務才會執行。</span><span class="sxs-lookup"><span data-stu-id="12766-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="12766-131">若要變更預設行為，請在呼叫`VideosWatcher` `ConfigureWebHostDefaults`之後新增託管服務（在下列範例中）：</span><span class="sxs-lookup"><span data-stu-id="12766-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* <span data-ttu-id="12766-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="12766-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="12766-133">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="12766-134">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="12766-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="12766-135">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="12766-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="12766-136">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="12766-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="12766-137">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="12766-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="12766-138">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="12766-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="12766-139">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="12766-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="12766-140">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="12766-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="12766-141">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="12766-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="12766-142">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="12766-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="12766-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="12766-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="12766-144">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="12766-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="12766-145">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="12766-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="12766-146">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="12766-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="12766-147">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="12766-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="12766-148">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="12766-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="12766-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="12766-149">BackgroundService</span></span>

<span data-ttu-id="12766-150">`BackgroundService`是用來進行長時間<xref:Microsoft.Extensions.Hosting.IHostedService>執行的基類。</span><span class="sxs-lookup"><span data-stu-id="12766-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="12766-151">`BackgroundService``ExecuteAsync(CancellationToken stoppingToken)`提供抽象方法以包含服務的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="12766-152">呼叫[IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時，會觸發。`stoppingToken`</span><span class="sxs-lookup"><span data-stu-id="12766-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="12766-153">這個方法`Task`的實作為傳回，代表背景服務的整個存留期。</span><span class="sxs-lookup"><span data-stu-id="12766-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="12766-154">此外，*也可以選擇性地*覆寫在`IHostedService`上定義的方法，以執行服務的啟動和關閉程式碼：</span><span class="sxs-lookup"><span data-stu-id="12766-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="12766-155">`StopAsync(CancellationToken cancellationToken)`&ndash; 當應用程式主機執行正常關機`StopAsync`時，會呼叫。</span><span class="sxs-lookup"><span data-stu-id="12766-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="12766-156">當主機決定強制終止服務時，會發出信號。`cancellationToken`</span><span class="sxs-lookup"><span data-stu-id="12766-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="12766-157">如果覆寫這個方法，您**必須**呼叫（和`await`）基類方法，以確保服務正常關閉。</span><span class="sxs-lookup"><span data-stu-id="12766-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="12766-158">`StartAsync(CancellationToken cancellationToken)`&ndash; 呼叫`StartAsync`來啟動背景服務。</span><span class="sxs-lookup"><span data-stu-id="12766-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="12766-159">如果啟動進程中斷，就會發出信號。`cancellationToken`</span><span class="sxs-lookup"><span data-stu-id="12766-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="12766-160">此實`Task`作為傳回，代表服務的啟動進程。</span><span class="sxs-lookup"><span data-stu-id="12766-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="12766-161">在`Task`完成之前，不會再啟動任何進一步的服務。</span><span class="sxs-lookup"><span data-stu-id="12766-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="12766-162">如果覆寫這個方法，您**必須**呼叫（和`await`）基類方法，以確保服務能正確啟動。</span><span class="sxs-lookup"><span data-stu-id="12766-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="12766-163">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="12766-163">Timed background tasks</span></span>

<span data-ttu-id="12766-164">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="12766-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="12766-165">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="12766-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="12766-166">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="12766-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="12766-167">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中以`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="12766-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="12766-168">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="12766-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="12766-169">若要在中`BackgroundService`使用[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="12766-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="12766-170">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="12766-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="12766-171">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="12766-172">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="12766-172">In the following example:</span></span>

* <span data-ttu-id="12766-173">服務是非同步。</span><span class="sxs-lookup"><span data-stu-id="12766-173">The service is asynchronous.</span></span> <span data-ttu-id="12766-174">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="12766-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="12766-175">基於示範目的，會在`DoWork`方法中等待10秒的延遲。</span><span class="sxs-lookup"><span data-stu-id="12766-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="12766-176"><xref:Microsoft.Extensions.Logging.ILogger>會插入服務中。</span><span class="sxs-lookup"><span data-stu-id="12766-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="12766-177">託管服務會建立範圍來解析已設定範圍的背景工作服務，以`DoWork`呼叫其方法。</span><span class="sxs-lookup"><span data-stu-id="12766-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="12766-178">`DoWork`傳回，其等候于`ExecuteAsync`： `Task`</span><span class="sxs-lookup"><span data-stu-id="12766-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="12766-179">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="12766-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="12766-180">託管服務會向`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="12766-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="12766-181">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="12766-181">Queued background tasks</span></span>

<span data-ttu-id="12766-182">背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="12766-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="12766-183">在下列`QueueHostedService`範例中：</span><span class="sxs-lookup"><span data-stu-id="12766-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="12766-184">方法會傳回，它`ExecuteAsync`會在中等待。 `Task` `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="12766-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="12766-185">佇列中的背景工作會在中`BackgroundProcessing`進行清除並執行。</span><span class="sxs-lookup"><span data-stu-id="12766-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="12766-186">每當在輸入裝置上選取索引`MonitorLoop` 鍵時，服務就會處理託管服務`w`的佇列工作：</span><span class="sxs-lookup"><span data-stu-id="12766-186">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="12766-187">會插入`MonitorLoop`服務中。 `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="12766-187">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="12766-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem`呼叫以將工作專案排入佇列。</span><span class="sxs-lookup"><span data-stu-id="12766-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="12766-189">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="12766-189">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="12766-190">託管服務會向`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="12766-190">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="12766-191">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="12766-191">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="12766-192">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="12766-192">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="12766-193">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="12766-193">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="12766-194">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="12766-194">Background task that runs on a timer.</span></span>
* <span data-ttu-id="12766-195">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="12766-195">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="12766-196">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="12766-196">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="12766-197">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="12766-197">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="12766-198">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12766-198">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="12766-199">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="12766-199">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="12766-200">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="12766-200">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="12766-201">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="12766-201">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="12766-202">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="12766-202">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="12766-203">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="12766-203">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="12766-204">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="12766-204">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="12766-205">套件</span><span class="sxs-lookup"><span data-stu-id="12766-205">Package</span></span>

<span data-ttu-id="12766-206">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="12766-206">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="12766-207">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="12766-207">IHostedService interface</span></span>

<span data-ttu-id="12766-208">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="12766-208">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="12766-209">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="12766-209">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="12766-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="12766-211">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="12766-211">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="12766-212">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="12766-212">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="12766-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="12766-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="12766-214">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-214">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="12766-215">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="12766-215">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="12766-216">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="12766-216">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="12766-217">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="12766-217">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="12766-218">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="12766-218">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="12766-219">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="12766-219">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="12766-220">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="12766-220">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="12766-221">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="12766-221">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="12766-222">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="12766-222">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="12766-223">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="12766-223">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="12766-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="12766-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="12766-225">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="12766-225">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="12766-226">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="12766-226">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="12766-227">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="12766-227">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="12766-228">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="12766-228">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="12766-229">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="12766-229">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="12766-230">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="12766-230">Timed background tasks</span></span>

<span data-ttu-id="12766-231">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="12766-231">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="12766-232">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="12766-232">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="12766-233">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="12766-233">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="12766-234">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="12766-234">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="12766-235">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="12766-235">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="12766-236">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="12766-236">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="12766-237">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="12766-237">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="12766-238">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="12766-238">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="12766-239">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="12766-239">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="12766-240">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="12766-240">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="12766-241">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="12766-241">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="12766-242">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="12766-242">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="12766-243">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="12766-243">Queued background tasks</span></span>

<span data-ttu-id="12766-244">背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="12766-244">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="12766-245">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="12766-245">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="12766-246">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="12766-246">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="12766-247">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="12766-247">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="12766-248">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="12766-248">In the Index page model class:</span></span>

* <span data-ttu-id="12766-249">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="12766-249">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="12766-250">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="12766-250">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="12766-251">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="12766-251">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="12766-252">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="12766-252">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="12766-253">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="12766-253">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="12766-254">`QueueBackgroundWorkItem`呼叫以將工作專案排入佇列：</span><span class="sxs-lookup"><span data-stu-id="12766-254">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="12766-255">其他資源</span><span class="sxs-lookup"><span data-stu-id="12766-255">Additional resources</span></span>

* [<span data-ttu-id="12766-256">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="12766-256">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
