---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/host/hosted-services
ms.openlocfilehash: d3f409170eedd281fd7608c4b9835bf9443c49b0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78666198"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="b6de8-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="b6de8-104">由[李歡](https://github.com/huan086)</span><span class="sxs-lookup"><span data-stu-id="b6de8-104">By [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b6de8-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」\*\*。</span><span class="sxs-lookup"><span data-stu-id="b6de8-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="b6de8-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="b6de8-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="b6de8-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="b6de8-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="b6de8-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="b6de8-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="b6de8-109">作用[網域服務的託管服務](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="b6de8-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="b6de8-110">作用域服務可以使用[依賴項注入 (DI)。](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="b6de8-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="b6de8-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="b6de8-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="b6de8-112">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6de8-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="b6de8-113">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="b6de8-113">Worker Service template</span></span>

<span data-ttu-id="b6de8-114">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="b6de8-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="b6de8-115">從輔助服務樣本建立的應用程式在其專案檔中指定輔助角色 SDK:</span><span class="sxs-lookup"><span data-stu-id="b6de8-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="b6de8-116">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="b6de8-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="b6de8-117">Package</span><span class="sxs-lookup"><span data-stu-id="b6de8-117">Package</span></span>

<span data-ttu-id="b6de8-118">基於輔助服務範本的應用使用`Microsoft.NET.Sdk.Worker`SDK,並且具有對[Microsoft.擴展.託管](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)包的顯式包引用。</span><span class="sxs-lookup"><span data-stu-id="b6de8-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="b6de8-119">例如,請參閱範例應用程式的專案檔 (*背景任務範例.csproj*)。</span><span class="sxs-lookup"><span data-stu-id="b6de8-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="b6de8-120">對於使用 SDK`Microsoft.NET.Sdk.Web`的 Web 應用,從共用框架隱式引用[Microsoft.擴展.託管](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)包。</span><span class="sxs-lookup"><span data-stu-id="b6de8-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="b6de8-121">不需要應用的專案檔中的顯式包引用。</span><span class="sxs-lookup"><span data-stu-id="b6de8-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="b6de8-122">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="b6de8-122">IHostedService interface</span></span>

<span data-ttu-id="b6de8-123">介面<xref:Microsoft.Extensions.Hosting.IHostedService>為主機管理的物件定義兩種方法:</span><span class="sxs-lookup"><span data-stu-id="b6de8-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="b6de8-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="b6de8-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="b6de8-125">`StartAsync`在*之前呼叫*:</span><span class="sxs-lookup"><span data-stu-id="b6de8-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="b6de8-126">套用的要求處理導管已`Startup.Configure`設定 。</span><span class="sxs-lookup"><span data-stu-id="b6de8-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="b6de8-127">伺服器已啟動[,I應用程式終身。應用程式啟動](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)已觸發。</span><span class="sxs-lookup"><span data-stu-id="b6de8-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="b6de8-128">可以更改默認行為,以便在配置並`StartAsync``ApplicationStarted`調用應用管道後託管服務運行。</span><span class="sxs-lookup"><span data-stu-id="b6de8-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="b6de8-129">要更改默認行為,請調用`VideosWatcher``ConfigureWebHostDefaults`後添加託管服務(在以下示例中),</span><span class="sxs-lookup"><span data-stu-id="b6de8-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="b6de8-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="b6de8-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="b6de8-131">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="b6de8-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="b6de8-132">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="b6de8-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="b6de8-133">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="b6de8-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="b6de8-134">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="b6de8-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="b6de8-135">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="b6de8-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="b6de8-136">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="b6de8-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="b6de8-137">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="b6de8-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="b6de8-138">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="b6de8-139">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="b6de8-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="b6de8-140">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="b6de8-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="b6de8-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="b6de8-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="b6de8-142">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="b6de8-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="b6de8-143">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="b6de8-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="b6de8-144">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="b6de8-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="b6de8-145">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="b6de8-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="b6de8-146">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="b6de8-147">背景服務基類</span><span class="sxs-lookup"><span data-stu-id="b6de8-147">BackgroundService base class</span></span>

<span data-ttu-id="b6de8-148"><xref:Microsoft.Extensions.Hosting.BackgroundService>是實現長時間運行<xref:Microsoft.Extensions.Hosting.IHostedService>的基類。</span><span class="sxs-lookup"><span data-stu-id="b6de8-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="b6de8-149">呼叫[同步(取消權杖)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*)來執行後台服務。</span><span class="sxs-lookup"><span data-stu-id="b6de8-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="b6de8-150">實現返回表示後台<xref:System.Threading.Tasks.Task>服務的整個存留期的 。</span><span class="sxs-lookup"><span data-stu-id="b6de8-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="b6de8-151">在[ExecuteAsync 變為非同步](https://github.com/dotnet/extensions/issues/2149)之前,不會啟動任何`await`進一步的服務,例如透過呼叫 。</span><span class="sxs-lookup"><span data-stu-id="b6de8-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/dotnet/extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="b6de8-152">避免長時間執行,從而阻止`ExecuteAsync`在中進行初始化工作。</span><span class="sxs-lookup"><span data-stu-id="b6de8-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="b6de8-153">[StopAsync(取消權杖)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*)中的主機塊`ExecuteAsync`等待 完成。</span><span class="sxs-lookup"><span data-stu-id="b6de8-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="b6de8-154">調用[I 託管服務.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時觸發取消權杖。</span><span class="sxs-lookup"><span data-stu-id="b6de8-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="b6de8-155">觸發取消權杖`ExecuteAsync`時,應及時完成 。以正常關閉服務。</span><span class="sxs-lookup"><span data-stu-id="b6de8-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="b6de8-156">否則,服務在關機超時時不正常關閉。</span><span class="sxs-lookup"><span data-stu-id="b6de8-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="b6de8-157">有關詳細資訊,請參閱[I託管服務介面](#ihostedservice-interface)部分。</span><span class="sxs-lookup"><span data-stu-id="b6de8-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="b6de8-158">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-158">Timed background tasks</span></span>

<span data-ttu-id="b6de8-159">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="b6de8-159">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="b6de8-160">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="b6de8-160">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="b6de8-161">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="b6de8-161">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<span data-ttu-id="b6de8-162"><xref:System.Threading.Timer>不會等待以前的執行`DoWork`完成,因此顯示的方法可能並不適合每個方案。</span><span class="sxs-lookup"><span data-stu-id="b6de8-162">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span> <span data-ttu-id="b6de8-163">[互鎖.增量](xref:System.Threading.Interlocked.Increment*)用於將執行計數器增量為原子操作,從而確保多個線程不會同時`executionCount`更新 。</span><span class="sxs-lookup"><span data-stu-id="b6de8-163">[Interlocked.Increment](xref:System.Threading.Interlocked.Increment*) is used to increment the execution counter as an atomic operation, which ensures that multiple threads don't update `executionCount` concurrently.</span></span>

<span data-ttu-id="b6de8-164">該服務使用`AddHostedService`擴充`IHostBuilder.ConfigureServices`方法在 (*Program.cs*) 註冊:</span><span class="sxs-lookup"><span data-stu-id="b6de8-164">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="b6de8-165">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="b6de8-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="b6de8-166">要在[後台服務](#backgroundservice-base-class)中使用[作用域服務](xref:fundamentals/dependency-injection#service-lifetimes),請建立作用域。</span><span class="sxs-lookup"><span data-stu-id="b6de8-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="b6de8-167">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="b6de8-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="b6de8-168">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="b6de8-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="b6de8-169">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="b6de8-169">In the following example:</span></span>

* <span data-ttu-id="b6de8-170">該服務是異步的。</span><span class="sxs-lookup"><span data-stu-id="b6de8-170">The service is asynchronous.</span></span> <span data-ttu-id="b6de8-171">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-171">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="b6de8-172">出於展示目的,`DoWork`該方法等待延遲 10 秒。</span><span class="sxs-lookup"><span data-stu-id="b6de8-172">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="b6de8-173"><xref:Microsoft.Extensions.Logging.ILogger>注入到服務中。</span><span class="sxs-lookup"><span data-stu-id="b6de8-173">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="b6de8-174">託管服務創建一個作用域來解決作用域後台任務服務以調用其`DoWork`方法。</span><span class="sxs-lookup"><span data-stu-id="b6de8-174">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="b6de8-175">`DoWork`傳`Task`回在中等待`ExecuteAsync`的 。</span><span class="sxs-lookup"><span data-stu-id="b6de8-175">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="b6de8-176">服務在`IHostBuilder.ConfigureServices`*(Program.cs*) 註冊。</span><span class="sxs-lookup"><span data-stu-id="b6de8-176">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="b6de8-177">託管服務使用`AddHostedService`擴充方法註冊:</span><span class="sxs-lookup"><span data-stu-id="b6de8-177">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="b6de8-178">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-178">Queued background tasks</span></span>

<span data-ttu-id="b6de8-179">背景4.x(<xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*>[暫定為內建ASP.NET核心](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="b6de8-179">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="b6de8-180">在以下`QueueHostedService`範例中:</span><span class="sxs-lookup"><span data-stu-id="b6de8-180">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="b6de8-181">方法`BackgroundProcessing`傳回`Task`在中等待`ExecuteAsync`的 。</span><span class="sxs-lookup"><span data-stu-id="b6de8-181">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="b6de8-182">佇列中的後台任務在`BackgroundProcessing`中 取消排隊和執行。</span><span class="sxs-lookup"><span data-stu-id="b6de8-182">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="b6de8-183">在服務停止`StopAsync`之前,正在等待工作項。</span><span class="sxs-lookup"><span data-stu-id="b6de8-183">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="b6de8-184">`MonitorLoop`選擇金鑰時,`w`服務都會處理託管服務的佇列 :</span><span class="sxs-lookup"><span data-stu-id="b6de8-184">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="b6de8-185">`IBackgroundTaskQueue`注入到服務中`MonitorLoop`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-185">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="b6de8-186">`IBackgroundTaskQueue.QueueBackgroundWorkItem`調用 以對工作項進行排隊。</span><span class="sxs-lookup"><span data-stu-id="b6de8-186">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="b6de8-187">工作項模擬長時間執行的背景:</span><span class="sxs-lookup"><span data-stu-id="b6de8-187">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="b6de8-188">執行三個 5`Task.Delay`秒延遲 ()。</span><span class="sxs-lookup"><span data-stu-id="b6de8-188">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="b6de8-189">如果`try-catch`任務被<xref:System.OperationCanceledException>取消 ,語句將捕獲。</span><span class="sxs-lookup"><span data-stu-id="b6de8-189">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="b6de8-190">服務在`IHostBuilder.ConfigureServices`*(Program.cs*) 註冊。</span><span class="sxs-lookup"><span data-stu-id="b6de8-190">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="b6de8-191">託管服務使用`AddHostedService`擴充方法註冊:</span><span class="sxs-lookup"><span data-stu-id="b6de8-191">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

<span data-ttu-id="b6de8-192">`MontiorLoop``Program.Main`啟動:</span><span class="sxs-lookup"><span data-stu-id="b6de8-192">`MontiorLoop` is started in `Program.Main`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b6de8-193">在 ASP.NET Core 中，背景工作可實作為「託管服務」\*\*。</span><span class="sxs-lookup"><span data-stu-id="b6de8-193">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="b6de8-194">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="b6de8-194">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="b6de8-195">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="b6de8-195">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="b6de8-196">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="b6de8-196">Background task that runs on a timer.</span></span>
* <span data-ttu-id="b6de8-197">作用[網域服務的託管服務](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="b6de8-197">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="b6de8-198">範圍服務可以使用[相依性性性 (DI)](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="b6de8-198">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="b6de8-199">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="b6de8-199">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="b6de8-200">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6de8-200">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="b6de8-201">Package</span><span class="sxs-lookup"><span data-stu-id="b6de8-201">Package</span></span>

<span data-ttu-id="b6de8-202">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="b6de8-202">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="b6de8-203">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="b6de8-203">IHostedService interface</span></span>

<span data-ttu-id="b6de8-204">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="b6de8-204">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="b6de8-205">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="b6de8-205">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="b6de8-206">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="b6de8-206">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="b6de8-207">使用[Web 主機](xref:fundamentals/host/web-host)`StartAsync`時,將在伺服器啟動和[IApplicationLifetime.應用程式啟動](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)後調用。</span><span class="sxs-lookup"><span data-stu-id="b6de8-207">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="b6de8-208">使用[泛型主機](xref:fundamentals/host/generic-host)時`StartAsync`, 在觸`ApplicationStarted`發之前 調用。</span><span class="sxs-lookup"><span data-stu-id="b6de8-208">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="b6de8-209">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="b6de8-209">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="b6de8-210">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="b6de8-210">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="b6de8-211">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="b6de8-211">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="b6de8-212">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="b6de8-212">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="b6de8-213">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="b6de8-213">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="b6de8-214">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="b6de8-214">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="b6de8-215">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="b6de8-215">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="b6de8-216">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="b6de8-216">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="b6de8-217">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-217">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="b6de8-218">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="b6de8-218">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="b6de8-219">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="b6de8-219">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="b6de8-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="b6de8-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="b6de8-221">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="b6de8-221">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="b6de8-222">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="b6de8-222">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="b6de8-223">如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="b6de8-223">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="b6de8-224">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="b6de8-224">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="b6de8-225">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-225">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="b6de8-226">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-226">Timed background tasks</span></span>

<span data-ttu-id="b6de8-227">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="b6de8-227">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="b6de8-228">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="b6de8-228">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="b6de8-229">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="b6de8-229">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="b6de8-230"><xref:System.Threading.Timer>不會等待以前的執行`DoWork`完成,因此顯示的方法可能並不適合每個方案。</span><span class="sxs-lookup"><span data-stu-id="b6de8-230">The <xref:System.Threading.Timer> doesn't wait for previous executions of `DoWork` to finish, so the approach shown might not be suitable for every scenario.</span></span>

<span data-ttu-id="b6de8-231">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="b6de8-231">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="b6de8-232">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="b6de8-232">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="b6de8-233">要在 中使用[作用域服務](xref:fundamentals/dependency-injection#service-lifetimes),`IHostedService`請建立作用域。</span><span class="sxs-lookup"><span data-stu-id="b6de8-233">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="b6de8-234">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="b6de8-234">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="b6de8-235">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="b6de8-235">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="b6de8-236">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="b6de8-236">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="b6de8-237">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="b6de8-237">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="b6de8-238">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="b6de8-238">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6de8-239">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="b6de8-239">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="b6de8-240">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-240">Queued background tasks</span></span>

<span data-ttu-id="b6de8-241">背景任務佇列基於 .NET 框架 4.x(<xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*>[暫定為 ASP.NET 核心](https://github.com/aspnet/Hosting/issues/1280)內建):</span><span class="sxs-lookup"><span data-stu-id="b6de8-241">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="b6de8-242">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 [BackgroundService](#backgroundservice-base-class) 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="b6de8-242">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="b6de8-243">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="b6de8-243">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b6de8-244">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="b6de8-244">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="b6de8-245">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="b6de8-245">In the Index page model class:</span></span>

* <span data-ttu-id="b6de8-246">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-246">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="b6de8-247">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="b6de8-247">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="b6de8-248">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="b6de8-248">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="b6de8-249">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="b6de8-249">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="b6de8-250">在索引頁面上選取 [新增工作]\*\*\*\* 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="b6de8-250">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="b6de8-251">`QueueBackgroundWorkItem`呼叫 以對工作項目進行佇列:</span><span class="sxs-lookup"><span data-stu-id="b6de8-251">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b6de8-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="b6de8-252">Additional resources</span></span>

* [<span data-ttu-id="b6de8-253">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-253">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="b6de8-254">在 Azure 應用服務中使用 Web 工作執行後臺工作</span><span class="sxs-lookup"><span data-stu-id="b6de8-254">Run background tasks with WebJobs in Azure App Service</span></span>](/azure/app-service/webjobs-create)
* <xref:System.Threading.Timer>
