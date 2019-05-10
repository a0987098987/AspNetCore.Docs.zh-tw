---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 613227cdead1d0b62a0dead2fca9fab68fd534cc
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889003"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="247eb-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="247eb-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="247eb-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="247eb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="247eb-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="247eb-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="247eb-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="247eb-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="247eb-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="247eb-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="247eb-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="247eb-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="247eb-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="247eb-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="247eb-110">範圍服務可以使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="247eb-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="247eb-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="247eb-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="247eb-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="247eb-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="247eb-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="247eb-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="247eb-114">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="247eb-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="247eb-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="247eb-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="247eb-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="247eb-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="247eb-117">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="247eb-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="247eb-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="247eb-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="247eb-119">Package</span><span class="sxs-lookup"><span data-stu-id="247eb-119">Package</span></span>

<span data-ttu-id="247eb-120">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="247eb-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="247eb-121">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="247eb-121">IHostedService interface</span></span>

<span data-ttu-id="247eb-122">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="247eb-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="247eb-123">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="247eb-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="247eb-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="247eb-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="247eb-125">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="247eb-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="247eb-126">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="247eb-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="247eb-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="247eb-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="247eb-128">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="247eb-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="247eb-129">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="247eb-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="247eb-130">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="247eb-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="247eb-131">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="247eb-131">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="247eb-132">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="247eb-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="247eb-133">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="247eb-133">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="247eb-134">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="247eb-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="247eb-135">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="247eb-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="247eb-136">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="247eb-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="247eb-137">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="247eb-137">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="247eb-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="247eb-138"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="247eb-139">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="247eb-139">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="247eb-140">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="247eb-140">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="247eb-141">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="247eb-141">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="247eb-142">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="247eb-142">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="247eb-143">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="247eb-143">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="247eb-144">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="247eb-144">Timed background tasks</span></span>

<span data-ttu-id="247eb-145">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="247eb-145">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="247eb-146">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="247eb-146">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="247eb-147">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="247eb-147">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="247eb-148">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="247eb-148">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="247eb-149">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="247eb-149">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="247eb-150">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="247eb-150">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="247eb-151">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="247eb-151">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="247eb-152">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="247eb-152">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="247eb-153">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="247eb-153">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="247eb-154">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="247eb-154">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="247eb-155">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="247eb-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="247eb-156">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="247eb-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="247eb-157">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="247eb-157">Queued background tasks</span></span>

<span data-ttu-id="247eb-158">背景工作佇列是以 .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([暫時排定為針對 ASP.NET Core 3.0 內建](https://github.com/aspnet/Hosting/issues/1280)) 為基礎：</span><span class="sxs-lookup"><span data-stu-id="247eb-158">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="247eb-159">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="247eb-159">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="247eb-160">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="247eb-160">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="247eb-161">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="247eb-161">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="247eb-162">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="247eb-162">In the Index page model class:</span></span>

* <span data-ttu-id="247eb-163">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="247eb-163">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="247eb-164">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="247eb-164">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="247eb-165">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="247eb-165">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="247eb-166">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="247eb-166">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="247eb-167">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="247eb-167">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="247eb-168">將會呼叫 `QueueBackgroundWorkItem` 以從佇列清除工作項目：</span><span class="sxs-lookup"><span data-stu-id="247eb-168">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="247eb-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="247eb-169">Additional resources</span></span>

* [<span data-ttu-id="247eb-170">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="247eb-170">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
