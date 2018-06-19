---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 89e595fb6ef38745d7377fdaaf1780c64320efe3
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2018
ms.locfileid: "29374687"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="a2808-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="a2808-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="a2808-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a2808-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a2808-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="a2808-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a2808-106">託管服務是具有背景工作邏輯的類別，能夠實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。</span><span class="sxs-lookup"><span data-stu-id="a2808-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="a2808-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="a2808-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a2808-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="a2808-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a2808-109">啟動範圍服務的託管服務。</span><span class="sxs-lookup"><span data-stu-id="a2808-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="a2808-110">範圍服務可以使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="a2808-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="a2808-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="a2808-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a2808-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2808-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/2.x) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a2808-113">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="a2808-113">IHostedService interface</span></span>

<span data-ttu-id="a2808-114">託管服務會實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。</span><span class="sxs-lookup"><span data-stu-id="a2808-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="a2808-115">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="a2808-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a2808-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - 在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) 之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="a2808-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="a2808-117">`StartAsync` 包含用來啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a2808-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="a2808-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="a2808-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a2808-119">`StopAsync` 包含用來結束背景工作和處置任何受控資源的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a2808-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="a2808-120">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="a2808-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="a2808-121">託管服務是在應用程式啟動時啟動一次，且在應用程式關閉時正常關閉的單一服務。</span><span class="sxs-lookup"><span data-stu-id="a2808-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="a2808-122">若實作 [IDisposable](/dotnet/api/system.idisposable)，就可以在處置服務容器時處置資源。</span><span class="sxs-lookup"><span data-stu-id="a2808-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="a2808-123">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="a2808-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a2808-124">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="a2808-124">Timed background tasks</span></span>

<span data-ttu-id="a2808-125">計時背景工作使用 [System.Threading.Timer](/dotnet/api/system.threading.timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="a2808-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="a2808-126">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="a2808-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a2808-127">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="a2808-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="a2808-128">服務會在 `Startup.ConfigureServices` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="a2808-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a2808-129">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="a2808-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a2808-130">若要在 `IHostedService` 內使用範圍服務，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="a2808-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="a2808-131">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="a2808-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a2808-132">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="a2808-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a2808-133">在下列範例中，[ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="a2808-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a2808-134">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="a2808-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="a2808-135">這些服務會在 `Startup.ConfigureServices` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="a2808-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a2808-136">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="a2808-136">Queued background tasks</span></span>

<span data-ttu-id="a2808-137">背景工作佇列是以 .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([暫時排定為針對 ASP.NET Core 2.2 內建](https://github.com/aspnet/Hosting/issues/1280)) 為基礎：</span><span class="sxs-lookup"><span data-stu-id="a2808-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a2808-138">在 `QueueHostedService` 中，佇列中的背景工作 (`workItem`) 會從佇列清除並執行：</span><span class="sxs-lookup"><span data-stu-id="a2808-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="a2808-139">這些服務會在 `Startup.ConfigureServices` 中註冊：</span><span class="sxs-lookup"><span data-stu-id="a2808-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="a2808-140">在索引頁面模型類別中，`IBackgroundTaskQueue` 已插入至建構函式，並指派給 `Queue`：</span><span class="sxs-lookup"><span data-stu-id="a2808-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a2808-141">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="a2808-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="a2808-142">將會呼叫 `QueueBackgroundWorkItem` 以從佇列清除工作項目：</span><span class="sxs-lookup"><span data-stu-id="a2808-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="a2808-143">其他資源</span><span class="sxs-lookup"><span data-stu-id="a2808-143">Additional resources</span></span>

* [<span data-ttu-id="a2808-144">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="a2808-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="a2808-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="a2808-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
