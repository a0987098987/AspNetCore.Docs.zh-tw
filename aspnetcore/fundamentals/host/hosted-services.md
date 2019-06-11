---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 2dbb1a84a380ab06a4be7ecf628799a070afc9e3
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692510"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="52ccb-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="52ccb-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="52ccb-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="52ccb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="52ccb-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」  。</span><span class="sxs-lookup"><span data-stu-id="52ccb-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="52ccb-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="52ccb-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="52ccb-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="52ccb-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="52ccb-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="52ccb-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="52ccb-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="52ccb-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="52ccb-110">範圍服務可以使用相依性插入。</span><span class="sxs-lookup"><span data-stu-id="52ccb-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="52ccb-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="52ccb-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="52ccb-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="52ccb-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="52ccb-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="52ccb-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="52ccb-114">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="52ccb-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="52ccb-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="52ccb-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="52ccb-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="52ccb-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="52ccb-117">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="52ccb-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="52ccb-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="52ccb-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="52ccb-119">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="52ccb-119">Worker Service template</span></span>

<span data-ttu-id="52ccb-120">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="52ccb-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="52ccb-121">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="52ccb-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="52ccb-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52ccb-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="52ccb-123">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="52ccb-123">Create a new project.</span></span>
1. <span data-ttu-id="52ccb-124">選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="52ccb-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="52ccb-125">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="52ccb-125">Select **Next**.</span></span>
1. <span data-ttu-id="52ccb-126">在 [專案名稱]  欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="52ccb-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="52ccb-127">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="52ccb-127">Select **Create**.</span></span>
1. <span data-ttu-id="52ccb-128">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中，確認選取 [.NET Core]  和 [ASP.NET Core 3.0]  。</span><span class="sxs-lookup"><span data-stu-id="52ccb-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="52ccb-129">選取 [背景工作服務]  範本。</span><span class="sxs-lookup"><span data-stu-id="52ccb-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="52ccb-130">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="52ccb-130">Select **Create**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="52ccb-131">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="52ccb-131">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="52ccb-132">從命令殼層以 [dotnet new](/dotnet/core/tools/dotnet-new) 命令使用背景工作服務 (`worker`) 範本。</span><span class="sxs-lookup"><span data-stu-id="52ccb-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="52ccb-133">在下列範例中，已建立名為 `ContosoWorkerService` 的背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="52ccb-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="52ccb-134">當命令執行時，會自動建立 `ContosoWorkerService` 應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="52ccb-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="52ccb-135">Package</span><span class="sxs-lookup"><span data-stu-id="52ccb-135">Package</span></span>

<span data-ttu-id="52ccb-136">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="52ccb-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="52ccb-137">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="52ccb-137">IHostedService interface</span></span>

<span data-ttu-id="52ccb-138">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="52ccb-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="52ccb-139">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="52ccb-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="52ccb-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="52ccb-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="52ccb-141">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="52ccb-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="52ccb-142">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="52ccb-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="52ccb-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="52ccb-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="52ccb-144">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="52ccb-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="52ccb-145">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="52ccb-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="52ccb-146">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="52ccb-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="52ccb-147">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="52ccb-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="52ccb-148">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="52ccb-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="52ccb-149">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="52ccb-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="52ccb-150">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="52ccb-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="52ccb-151">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="52ccb-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="52ccb-152">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="52ccb-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="52ccb-153">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="52ccb-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="52ccb-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="52ccb-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="52ccb-155">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="52ccb-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="52ccb-156">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="52ccb-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="52ccb-157">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="52ccb-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="52ccb-158">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="52ccb-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="52ccb-159">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="52ccb-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="52ccb-160">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="52ccb-160">Timed background tasks</span></span>

<span data-ttu-id="52ccb-161">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="52ccb-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="52ccb-162">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="52ccb-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="52ccb-163">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="52ccb-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="52ccb-164">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="52ccb-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="52ccb-165">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="52ccb-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="52ccb-166">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="52ccb-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="52ccb-167">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="52ccb-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="52ccb-168">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="52ccb-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="52ccb-169">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="52ccb-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="52ccb-170">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="52ccb-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="52ccb-171">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="52ccb-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="52ccb-172">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="52ccb-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="52ccb-173">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="52ccb-173">Queued background tasks</span></span>

<span data-ttu-id="52ccb-174">背景工作佇列是以 .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([暫時排定為針對 ASP.NET Core 3.0 內建](https://github.com/aspnet/Hosting/issues/1280)) 為基礎：</span><span class="sxs-lookup"><span data-stu-id="52ccb-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="52ccb-175">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="52ccb-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="52ccb-176">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="52ccb-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="52ccb-177">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="52ccb-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="52ccb-178">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="52ccb-178">In the Index page model class:</span></span>

* <span data-ttu-id="52ccb-179">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="52ccb-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="52ccb-180">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="52ccb-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="52ccb-181">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="52ccb-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="52ccb-182">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="52ccb-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="52ccb-183">在索引頁面上選取 [新增工作]  按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="52ccb-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="52ccb-184">將會呼叫 `QueueBackgroundWorkItem` 以從佇列清除工作項目：</span><span class="sxs-lookup"><span data-stu-id="52ccb-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="52ccb-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="52ccb-185">Additional resources</span></span>

* [<span data-ttu-id="52ccb-186">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="52ccb-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
