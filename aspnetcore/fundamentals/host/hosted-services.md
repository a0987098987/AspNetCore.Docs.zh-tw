---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 49229b5db4d58f25f86425f8622d12c9107262bd
ms.sourcegitcommit: 57b85708f4cded99b8f008a69830cb104cd8e879
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2020
ms.locfileid: "75914217"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="2a8d4-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="2a8d4-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="2a8d4-104">By [Luke Latham](https://github.com/guardrex)和[Jeow Li Huan](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="2a8d4-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2a8d4-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2a8d4-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2a8d4-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2a8d4-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2a8d4-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2a8d4-110">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="2a8d4-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2a8d4-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2a8d4-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="2a8d4-113">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="2a8d4-113">Worker Service template</span></span>

<span data-ttu-id="2a8d4-114">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="2a8d4-115">從背景工作角色服務範本建立的應用程式會在其專案檔中指定背景工作角色 SDK：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="2a8d4-116">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="2a8d4-117">套件</span><span class="sxs-lookup"><span data-stu-id="2a8d4-117">Package</span></span>

<span data-ttu-id="2a8d4-118">以背景工作角色服務範本為基礎的應用程式會使用 `Microsoft.NET.Sdk.Worker` SDK，並具有對[Microsoft Extensions. 裝載](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)封裝的明確套件參考。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="2a8d4-119">例如，請參閱範例應用程式的專案檔（*BackgroundTasksSample .csproj*）。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="2a8d4-120">若是使用 `Microsoft.NET.Sdk.Web` SDK 的 web 應用程式，則會隱含地從共用架構參考[Microsoft Extensions. 裝載](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)套件。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="2a8d4-121">應用程式的專案檔中不需要明確的套件參考。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2a8d4-122">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="2a8d4-122">IHostedService interface</span></span>

<span data-ttu-id="2a8d4-123"><xref:Microsoft.Extensions.Hosting.IHostedService> 介面會針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2a8d4-124">[StartAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2a8d4-125">*在之前*呼叫 `StartAsync`：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="2a8d4-126">應用程式的要求處理管線已設定（`Startup.Configure`）。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="2a8d4-127">伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="2a8d4-128">您可以變更預設行為，以便在應用程式的管線已設定且呼叫 `ApplicationStarted` 之後，託管服務的 `StartAsync` 執行。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="2a8d4-129">若要變更預設行為，請在呼叫 `ConfigureWebHostDefaults`之後，新增託管服務（在下列範例中`VideosWatcher`）：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="2a8d4-130">當主機執行正常關機時，會觸發[StopAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2a8d4-131">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2a8d4-132">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2a8d4-133">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2a8d4-134">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2a8d4-135">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2a8d4-136">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2a8d4-137">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2a8d4-138">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2a8d4-139">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2a8d4-140">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2a8d4-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2a8d4-142">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2a8d4-143">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2a8d4-144">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2a8d4-145">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2a8d4-146">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="2a8d4-147">BackgroundService 基類</span><span class="sxs-lookup"><span data-stu-id="2a8d4-147">BackgroundService base class</span></span>

<span data-ttu-id="2a8d4-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> 是用來執行長時間執行 <xref:Microsoft.Extensions.Hosting.IHostedService>的基類。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="2a8d4-149">呼叫[ExecuteAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*)以執行背景服務。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="2a8d4-150">此實作為傳回的 <xref:System.Threading.Tasks.Task>，代表背景服務的整個存留期。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="2a8d4-151">在[ExecuteAsync 變成非同步](https://github.com/dotnet/extensions/issues/2149)（例如藉由呼叫 `await`）之前，不會再啟動任何進一步的服務。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/dotnet/extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="2a8d4-152">請避免在 `ExecuteAsync`中執行長時間的封鎖初始化工作。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="2a8d4-153">[StopAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*)中的主機區塊，等待 `ExecuteAsync` 完成。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="2a8d4-154">呼叫[IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時，會觸發解除標記。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="2a8d4-155">您的 `ExecuteAsync` 的執行應該會在引發解除標記時立即完成，以便正常地關閉服務。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="2a8d4-156">否則，服務強制會在關機時間關閉。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="2a8d4-157">如需詳細資訊，請參閱[IHostedService 介面](#ihostedservice-interface)一節。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2a8d4-158">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="2a8d4-158">Timed background tasks</span></span>

<span data-ttu-id="2a8d4-159">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-159">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2a8d4-160">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-160">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2a8d4-161">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-161">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<span data-ttu-id="2a8d4-162"><xref:System.Threading.Timer> 不會等待先前執行的 `DoWork` 完成，因此所顯示的方法可能不適用於每個案例。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-162">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span> <span data-ttu-id="2a8d4-163">[連鎖：](xref:System.Threading.Interlocked.Increment*)用來將執行計數器遞增為不可部分完成的作業，以確保多個執行緒不會同時更新 `executionCount`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-163">[Interlocked.Increment](xref:System.Threading.Interlocked.Increment*) is used to increment the execution counter as an atomic operation, which ensures that multiple threads don't update `executionCount` concurrently.</span></span>

<span data-ttu-id="2a8d4-164">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-164">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2a8d4-165">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="2a8d4-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2a8d4-166">若要使用[BackgroundService](#backgroundservice-base-class)內的[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="2a8d4-167">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2a8d4-168">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2a8d4-169">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-169">In the following example:</span></span>

* <span data-ttu-id="2a8d4-170">服務是非同步。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-170">The service is asynchronous.</span></span> <span data-ttu-id="2a8d4-171">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-171">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="2a8d4-172">基於示範目的，`DoWork` 方法會等待10秒的延遲。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-172">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="2a8d4-173"><xref:Microsoft.Extensions.Logging.ILogger> 會插入服務中。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-173">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2a8d4-174">託管服務會建立範圍來解析已設定範圍的背景工作服務，以呼叫其 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-174">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="2a8d4-175">`DoWork` 會傳回 `Task`，在 `ExecuteAsync`中等待：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-175">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="2a8d4-176">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-176">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2a8d4-177">託管服務會向 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-177">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2a8d4-178">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="2a8d4-178">Queued background tasks</span></span>

<span data-ttu-id="2a8d4-179">背景工作佇列是以 .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-179">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2a8d4-180">在下列 `QueueHostedService` 範例中：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-180">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="2a8d4-181">`BackgroundProcessing` 方法會傳回 `Task`，在 `ExecuteAsync`中等待。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-181">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="2a8d4-182">佇列中的背景工作會在 `BackgroundProcessing`中進行清除作業並執行。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-182">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="2a8d4-183">在 `StopAsync`中停止服務之前，會等待工作專案。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-183">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="2a8d4-184">每當在輸入裝置上選取 `w` 金鑰時，`MonitorLoop` 服務就會處理託管服務的佇列工作：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-184">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="2a8d4-185">`IBackgroundTaskQueue` 會插入 `MonitorLoop` 服務中。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-185">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="2a8d4-186">呼叫 `IBackgroundTaskQueue.QueueBackgroundWorkItem` 以將工作專案排入佇列。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-186">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="2a8d4-187">工作專案會模擬長時間執行的背景工作：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-187">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="2a8d4-188">會執行三個5秒的延遲（`Task.Delay`）。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-188">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="2a8d4-189">如果工作已取消，`try-catch` 語句會 <xref:System.OperationCanceledException> 補漏白。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-189">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="2a8d4-190">服務會在 `IHostBuilder.ConfigureServices` （*Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-190">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2a8d4-191">託管服務會向 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-191">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2a8d4-192">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-192">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2a8d4-193">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-193">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2a8d4-194">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-194">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2a8d4-195">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-195">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2a8d4-196">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-196">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2a8d4-197">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="2a8d4-197">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="2a8d4-198">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-198">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2a8d4-199">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2a8d4-199">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="2a8d4-200">套件</span><span class="sxs-lookup"><span data-stu-id="2a8d4-200">Package</span></span>

<span data-ttu-id="2a8d4-201">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-201">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2a8d4-202">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="2a8d4-202">IHostedService interface</span></span>

<span data-ttu-id="2a8d4-203">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-203">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2a8d4-204">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-204">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2a8d4-205">[StartAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-205">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2a8d4-206">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-206">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="2a8d4-207">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-207">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="2a8d4-208">當主機執行正常關機時，會觸發[StopAsync （CancellationToken）](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-208">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2a8d4-209">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-209">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2a8d4-210">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-210">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2a8d4-211">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-211">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2a8d4-212">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-212">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2a8d4-213">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-213">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2a8d4-214">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-214">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2a8d4-215">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-215">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2a8d4-216">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-216">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2a8d4-217">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-217">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2a8d4-218">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-218">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2a8d4-219"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-219"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2a8d4-220">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-220">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2a8d4-221">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-221">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2a8d4-222">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-222">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2a8d4-223">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-223">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2a8d4-224">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-224">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2a8d4-225">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="2a8d4-225">Timed background tasks</span></span>

<span data-ttu-id="2a8d4-226">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-226">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2a8d4-227">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-227">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2a8d4-228">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-228">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="2a8d4-229"><xref:System.Threading.Timer> 不會等待先前執行的 `DoWork` 完成，因此所顯示的方法可能不適用於每個案例。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-229">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span>

<span data-ttu-id="2a8d4-230">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-230">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2a8d4-231">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="2a8d4-231">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2a8d4-232">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-232">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="2a8d4-233">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-233">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2a8d4-234">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-234">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2a8d4-235">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-235">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2a8d4-236">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-236">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="2a8d4-237">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-237">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2a8d4-238">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-238">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2a8d4-239">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="2a8d4-239">Queued background tasks</span></span>

<span data-ttu-id="2a8d4-240">背景工作佇列是根據 .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-240">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2a8d4-241">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 [BackgroundService](#backgroundservice-base-class) 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-241">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="2a8d4-242">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-242">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2a8d4-243">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-243">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="2a8d4-244">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-244">In the Index page model class:</span></span>

* <span data-ttu-id="2a8d4-245">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-245">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="2a8d4-246">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-246">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="2a8d4-247">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-247">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="2a8d4-248">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-248">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="2a8d4-249">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="2a8d4-249">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="2a8d4-250">呼叫 `QueueBackgroundWorkItem` 以將工作專案排入佇列：</span><span class="sxs-lookup"><span data-stu-id="2a8d4-250">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2a8d4-251">其他資源</span><span class="sxs-lookup"><span data-stu-id="2a8d4-251">Additional resources</span></span>

* [<span data-ttu-id="2a8d4-252">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="2a8d4-252">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
