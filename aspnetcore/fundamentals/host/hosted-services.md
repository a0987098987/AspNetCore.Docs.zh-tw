---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc8f7fa00436a847ab1d1ba0976fb5e3899576ee
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312124"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="0a2e2-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="0a2e2-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="0a2e2-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a2e2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0a2e2-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="0a2e2-106">託管服務是具有背景工作邏輯的類別，能夠實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="0a2e2-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="0a2e2-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="0a2e2-109">啟動範圍服務的託管服務。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="0a2e2-110">範圍服務可以使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="0a2e2-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="0a2e2-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a2e2-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0a2e2-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="0a2e2-114">Web 主機：Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="0a2e2-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="0a2e2-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="0a2e2-117">泛型主機：泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="0a2e2-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="0a2e2-119">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="0a2e2-119">IHostedService interface</span></span>

<span data-ttu-id="0a2e2-120">託管服務會實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="0a2e2-121">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="0a2e2-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="0a2e2-123">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-123">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="0a2e2-124">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-124">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="0a2e2-125">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-125">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="0a2e2-126">`StopAsync` 包含用來結束背景工作和處置任何受控資源的邏輯。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-126">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="0a2e2-127">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-127">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="0a2e2-128">託管服務會在應用程式啟動時啟動一次，然後應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-128">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="0a2e2-129">若實作 [IDisposable](/dotnet/api/system.idisposable)，就可以在處置服務容器時處置資源。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-129">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="0a2e2-130">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-130">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="0a2e2-131">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="0a2e2-131">Timed background tasks</span></span>

<span data-ttu-id="0a2e2-132">計時背景工作使用 [System.Threading.Timer](/dotnet/api/system.threading.timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-132">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="0a2e2-133">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-133">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="0a2e2-134">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-134">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="0a2e2-135">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-135">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="0a2e2-136">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="0a2e2-136">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="0a2e2-137">若要在 `IHostedService` 內使用範圍服務，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-137">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="0a2e2-138">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-138">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="0a2e2-139">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-139">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="0a2e2-140">在下列範例中，[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-140">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="0a2e2-141">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-141">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="0a2e2-142">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-142">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0a2e2-143">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-143">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="0a2e2-144">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="0a2e2-144">Queued background tasks</span></span>

<span data-ttu-id="0a2e2-145">背景工作佇列是以 .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([暫時排定為針對 ASP.NET Core 3.0 內建](https://github.com/aspnet/Hosting/issues/1280)) 為基礎：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-145">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="0a2e2-146">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice) 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-146">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](/dotnet/api/microsoft.extensions.hosting.backgroundservice), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=16,20)]

<span data-ttu-id="0a2e2-147">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-147">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0a2e2-148">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-148">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="0a2e2-149">在索引頁面模型類別中，`IBackgroundTaskQueue` 已插入至建構函式，並指派給 `Queue`：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-149">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="0a2e2-150">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="0a2e2-150">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="0a2e2-151">將會呼叫 `QueueBackgroundWorkItem` 以從佇列清除工作項目：</span><span class="sxs-lookup"><span data-stu-id="0a2e2-151">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="0a2e2-152">其他資源</span><span class="sxs-lookup"><span data-stu-id="0a2e2-152">Additional resources</span></span>

* [<span data-ttu-id="0a2e2-153">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="0a2e2-153">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="0a2e2-154">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="0a2e2-154">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
