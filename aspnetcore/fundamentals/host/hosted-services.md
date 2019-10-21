---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: c1fbb5ae8ffc4ee506f42df6a4cbbe845b2b903d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333663"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="59114-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="59114-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="59114-104">By [Luke Latham](https://github.com/guardrex)和[Jeow Li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="59114-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="59114-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="59114-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="59114-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="59114-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="59114-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="59114-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="59114-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="59114-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="59114-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="59114-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="59114-110">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="59114-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="59114-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="59114-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="59114-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59114-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="59114-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="59114-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="59114-114">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="59114-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="59114-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="59114-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="59114-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="59114-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="59114-117">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="59114-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="59114-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="59114-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="59114-119">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="59114-119">Worker Service template</span></span>

<span data-ttu-id="59114-120">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="59114-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="59114-121">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="59114-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="59114-122">封裝</span><span class="sxs-lookup"><span data-stu-id="59114-122">Package</span></span>

<span data-ttu-id="59114-123">針對 ASP.NET Core 應用程式，會隱含地新增對[Microsoft Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="59114-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="59114-124">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="59114-124">IHostedService interface</span></span>

<span data-ttu-id="59114-125">@No__t_0 介面會針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="59114-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="59114-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="59114-127">*在之前*呼叫 `StartAsync`：</span><span class="sxs-lookup"><span data-stu-id="59114-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="59114-128">應用程式的要求處理管線已設定（`Startup.Configure`）。</span><span class="sxs-lookup"><span data-stu-id="59114-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="59114-129">伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="59114-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="59114-130">您可以變更預設行為，以便在應用程式的管線已設定且呼叫 `ApplicationStarted` 之後，託管服務的 `StartAsync` 執行。</span><span class="sxs-lookup"><span data-stu-id="59114-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="59114-131">若要變更預設行為，請在呼叫 `ConfigureWebHostDefaults` 之後，新增託管服務（在下列範例中 `VideosWatcher`）：</span><span class="sxs-lookup"><span data-stu-id="59114-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="59114-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="59114-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="59114-133">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="59114-134">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="59114-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="59114-135">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="59114-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="59114-136">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="59114-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="59114-137">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="59114-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="59114-138">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="59114-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="59114-139">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="59114-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="59114-140">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="59114-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="59114-141">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="59114-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="59114-142">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="59114-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="59114-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="59114-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="59114-144">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="59114-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="59114-145">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="59114-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="59114-146">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="59114-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="59114-147">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="59114-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="59114-148">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="59114-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="59114-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="59114-149">BackgroundService</span></span>

<span data-ttu-id="59114-150">`BackgroundService` 是用來執行長時間執行 <xref:Microsoft.Extensions.Hosting.IHostedService> 的基類。</span><span class="sxs-lookup"><span data-stu-id="59114-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="59114-151">`BackgroundService` 提供 `ExecuteAsync(CancellationToken stoppingToken)` 的抽象方法，以包含服務的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="59114-152">呼叫[IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時，就會觸發 `stoppingToken`。</span><span class="sxs-lookup"><span data-stu-id="59114-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="59114-153">這個方法的執行會傳回代表背景服務整個存留期的 `Task`。</span><span class="sxs-lookup"><span data-stu-id="59114-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="59114-154">此外，*也可以選擇性地*覆寫在 `IHostedService` 上定義的方法，以執行服務的啟動和關閉程式碼：</span><span class="sxs-lookup"><span data-stu-id="59114-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="59114-155">當應用程式主機執行正常關機時，會呼叫 `StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="59114-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="59114-156">當主機決定強制終止服務時，就會發出 `cancellationToken` 信號。</span><span class="sxs-lookup"><span data-stu-id="59114-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="59114-157">如果覆寫這個方法，您**必須**呼叫（並 `await`）基類方法，以確保服務正常關閉。</span><span class="sxs-lookup"><span data-stu-id="59114-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="59114-158">呼叫 `StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` 來啟動背景服務。</span><span class="sxs-lookup"><span data-stu-id="59114-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="59114-159">如果啟動進程中斷，就會發出 `cancellationToken` 信號。</span><span class="sxs-lookup"><span data-stu-id="59114-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="59114-160">此實作為傳回的 `Task`，代表服務的啟動進程。</span><span class="sxs-lookup"><span data-stu-id="59114-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="59114-161">在此 `Task` 完成之前，不會再啟動任何進一步的服務。</span><span class="sxs-lookup"><span data-stu-id="59114-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="59114-162">如果覆寫這個方法，您**必須**呼叫（並 `await`）基類方法，以確保服務能正確啟動。</span><span class="sxs-lookup"><span data-stu-id="59114-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="59114-163">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="59114-163">Timed background tasks</span></span>

<span data-ttu-id="59114-164">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="59114-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="59114-165">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="59114-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="59114-166">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="59114-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="59114-167">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="59114-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="59114-168">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="59114-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="59114-169">若要在 `BackgroundService` 內使用[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="59114-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="59114-170">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="59114-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="59114-171">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="59114-172">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="59114-172">In the following example:</span></span>

* <span data-ttu-id="59114-173">服務是非同步。</span><span class="sxs-lookup"><span data-stu-id="59114-173">The service is asynchronous.</span></span> <span data-ttu-id="59114-174">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="59114-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="59114-175">基於示範目的，`DoWork` 方法會等待10秒的延遲。</span><span class="sxs-lookup"><span data-stu-id="59114-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="59114-176">@No__t_0 會插入服務中。</span><span class="sxs-lookup"><span data-stu-id="59114-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="59114-177">託管服務會建立範圍來解析已設定範圍的背景工作服務，以呼叫其 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="59114-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="59114-178">`DoWork` 會傳回 `Task`，在 `ExecuteAsync` 中等待：</span><span class="sxs-lookup"><span data-stu-id="59114-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="59114-179">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="59114-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="59114-180">託管服務會向 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="59114-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="59114-181">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="59114-181">Queued background tasks</span></span>

<span data-ttu-id="59114-182">背景工作佇列是以 .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="59114-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="59114-183">在下列 `QueueHostedService` 範例中：</span><span class="sxs-lookup"><span data-stu-id="59114-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="59114-184">@No__t_0 方法會傳回 `Task`，在 `ExecuteAsync` 中等待。</span><span class="sxs-lookup"><span data-stu-id="59114-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="59114-185">佇列中的背景工作會在 `BackgroundProcessing` 中進行清除作業並執行。</span><span class="sxs-lookup"><span data-stu-id="59114-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="59114-186">在 `StopAsync` 中停止服務之前，會等待工作專案。</span><span class="sxs-lookup"><span data-stu-id="59114-186">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="59114-187">每當在輸入裝置上選取 `w` 金鑰時，`MonitorLoop` 服務就會處理託管服務的佇列工作：</span><span class="sxs-lookup"><span data-stu-id="59114-187">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="59114-188">@No__t_0 會插入 `MonitorLoop` 服務中。</span><span class="sxs-lookup"><span data-stu-id="59114-188">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="59114-189">呼叫 `IBackgroundTaskQueue.QueueBackgroundWorkItem` 以將工作專案排入佇列。</span><span class="sxs-lookup"><span data-stu-id="59114-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="59114-190">工作專案會模擬長時間執行的背景工作：</span><span class="sxs-lookup"><span data-stu-id="59114-190">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="59114-191">會執行三個5秒的延遲（`Task.Delay`）。</span><span class="sxs-lookup"><span data-stu-id="59114-191">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="59114-192">如果工作已取消，`try-catch` 語句會 <xref:System.OperationCanceledException> 補漏白。</span><span class="sxs-lookup"><span data-stu-id="59114-192">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="59114-193">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="59114-193">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="59114-194">託管服務會向 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="59114-194">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="59114-195">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="59114-195">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="59114-196">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="59114-196">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="59114-197">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="59114-197">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="59114-198">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="59114-198">Background task that runs on a timer.</span></span>
* <span data-ttu-id="59114-199">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="59114-199">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="59114-200">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="59114-200">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="59114-201">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="59114-201">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="59114-202">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="59114-202">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="59114-203">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="59114-203">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="59114-204">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="59114-204">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="59114-205">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="59114-205">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="59114-206">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="59114-206">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="59114-207">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="59114-207">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="59114-208">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="59114-208">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="59114-209">封裝</span><span class="sxs-lookup"><span data-stu-id="59114-209">Package</span></span>

<span data-ttu-id="59114-210">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="59114-210">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="59114-211">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="59114-211">IHostedService interface</span></span>

<span data-ttu-id="59114-212">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="59114-212">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="59114-213">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="59114-213">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="59114-214">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-214">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="59114-215">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="59114-215">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="59114-216">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="59114-216">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="59114-217">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="59114-217">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="59114-218">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-218">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="59114-219">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="59114-219">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="59114-220">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="59114-220">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="59114-221">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="59114-221">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="59114-222">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="59114-222">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="59114-223">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="59114-223">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="59114-224">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="59114-224">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="59114-225">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="59114-225">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="59114-226">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="59114-226">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="59114-227">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="59114-227">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="59114-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="59114-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="59114-229">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="59114-229">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="59114-230">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="59114-230">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="59114-231">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="59114-231">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="59114-232">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="59114-232">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="59114-233">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="59114-233">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="59114-234">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="59114-234">Timed background tasks</span></span>

<span data-ttu-id="59114-235">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="59114-235">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="59114-236">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="59114-236">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="59114-237">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="59114-237">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="59114-238">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="59114-238">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="59114-239">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="59114-239">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="59114-240">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="59114-240">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="59114-241">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="59114-241">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="59114-242">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="59114-242">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="59114-243">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="59114-243">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="59114-244">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="59114-244">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="59114-245">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="59114-245">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="59114-246">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="59114-246">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="59114-247">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="59114-247">Queued background tasks</span></span>

<span data-ttu-id="59114-248">背景工作佇列是根據 .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）：</span><span class="sxs-lookup"><span data-stu-id="59114-248">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="59114-249">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="59114-249">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="59114-250">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="59114-250">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="59114-251">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="59114-251">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="59114-252">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="59114-252">In the Index page model class:</span></span>

* <span data-ttu-id="59114-253">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="59114-253">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="59114-254">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="59114-254">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="59114-255">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="59114-255">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="59114-256">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="59114-256">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="59114-257">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="59114-257">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="59114-258">呼叫 `QueueBackgroundWorkItem` 以將工作專案排入佇列：</span><span class="sxs-lookup"><span data-stu-id="59114-258">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="59114-259">其他資源</span><span class="sxs-lookup"><span data-stu-id="59114-259">Additional resources</span></span>

* [<span data-ttu-id="59114-260">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="59114-260">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
