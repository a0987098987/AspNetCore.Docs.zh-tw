---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205707"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="9d39a-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="9d39a-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="9d39a-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9d39a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9d39a-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="9d39a-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="9d39a-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="9d39a-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9d39a-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="9d39a-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="9d39a-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="9d39a-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="9d39a-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="9d39a-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9d39a-110">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="9d39a-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="9d39a-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="9d39a-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="9d39a-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9d39a-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9d39a-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="9d39a-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="9d39a-114">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="9d39a-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="9d39a-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="9d39a-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="9d39a-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="9d39a-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="9d39a-117">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="9d39a-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="9d39a-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="9d39a-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="9d39a-119">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="9d39a-119">Worker Service template</span></span>

<span data-ttu-id="9d39a-120">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="9d39a-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="9d39a-121">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="9d39a-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d39a-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d39a-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="9d39a-123">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="9d39a-123">Create a new project.</span></span>
1. <span data-ttu-id="9d39a-124">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="9d39a-125">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-125">Select **Next**.</span></span>
1. <span data-ttu-id="9d39a-126">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="9d39a-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="9d39a-127">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-127">Select **Create**.</span></span>
1. <span data-ttu-id="9d39a-128">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="9d39a-129">選取 [背景工作服務] 範本。</span><span class="sxs-lookup"><span data-stu-id="9d39a-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="9d39a-130">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d39a-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9d39a-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="9d39a-132">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="9d39a-132">Create a new project.</span></span>
1. <span data-ttu-id="9d39a-133">在提要欄位中選取 [ **.Net Core** ] 底下的 [**應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="9d39a-134">選取  **ASP.NET Core**下的 背景**工作角色**。</span><span class="sxs-lookup"><span data-stu-id="9d39a-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="9d39a-135">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-135">Select **Next**.</span></span>
1. <span data-ttu-id="9d39a-136">針對 [**目標 Framework**] 選取 [ **.net Core 3.0** ]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="9d39a-137">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-137">Select **Next**.</span></span>
1. <span data-ttu-id="9d39a-138">在 [**專案名稱**] 欄位中提供名稱。</span><span class="sxs-lookup"><span data-stu-id="9d39a-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="9d39a-139">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="9d39a-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9d39a-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="9d39a-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9d39a-141">從命令殼層以 [dotnet new](/dotnet/core/tools/dotnet-new) 命令使用背景工作服務 (`worker`) 範本。</span><span class="sxs-lookup"><span data-stu-id="9d39a-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="9d39a-142">在下列範例中，已建立名為 `ContosoWorker` 的背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d39a-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="9d39a-143">當命令執行時，會自動建立 `ContosoWorker` 應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9d39a-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="9d39a-144">Package</span><span class="sxs-lookup"><span data-stu-id="9d39a-144">Package</span></span>

<span data-ttu-id="9d39a-145">針對 ASP.NET Core 應用程式，會隱含地新增對[Microsoft Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="9d39a-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="9d39a-146">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="9d39a-146">IHostedService interface</span></span>

<span data-ttu-id="9d39a-147"><xref:Microsoft.Extensions.Hosting.IHostedService>介面會針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="9d39a-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="9d39a-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d39a-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="9d39a-149">`StartAsync`會*在之前*呼叫：</span><span class="sxs-lookup"><span data-stu-id="9d39a-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="9d39a-150">應用程式的要求處理管線已設定（`Startup.Configure`）。</span><span class="sxs-lookup"><span data-stu-id="9d39a-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="9d39a-151">伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="9d39a-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="9d39a-152">您可以變更預設行為，以便在應用程式的管線`StartAsync`已設定且`ApplicationStarted`呼叫之後，託管服務才會執行。</span><span class="sxs-lookup"><span data-stu-id="9d39a-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="9d39a-153">若要變更預設行為，請在呼叫`VideosWatcher` `ConfigureWebHostDefaults`之後新增託管服務（在下列範例中）：</span><span class="sxs-lookup"><span data-stu-id="9d39a-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="9d39a-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="9d39a-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="9d39a-155">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d39a-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="9d39a-156">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="9d39a-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="9d39a-157">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="9d39a-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="9d39a-158">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="9d39a-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="9d39a-159">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="9d39a-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="9d39a-160">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="9d39a-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="9d39a-161">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="9d39a-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="9d39a-162">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="9d39a-163">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="9d39a-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="9d39a-164">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="9d39a-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="9d39a-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="9d39a-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="9d39a-166">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="9d39a-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="9d39a-167">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="9d39a-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="9d39a-168">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="9d39a-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="9d39a-169">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="9d39a-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="9d39a-170">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="9d39a-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="9d39a-171">BackgroundService</span></span>

<span data-ttu-id="9d39a-172">`BackgroundService`是用來進行長時間<xref:Microsoft.Extensions.Hosting.IHostedService>執行的基類。</span><span class="sxs-lookup"><span data-stu-id="9d39a-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="9d39a-173">`BackgroundService`定義背景作業的兩個方法：</span><span class="sxs-lookup"><span data-stu-id="9d39a-173">`BackgroundService` defines two methods for background operations:</span></span>

* <span data-ttu-id="9d39a-174">`ExecuteAsync(CancellationToken stoppingToken)`&ndash; 在啟動<xref:Microsoft.Extensions.Hosting.IHostedService>時呼叫。 `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="9d39a-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync` Called when the <xref:Microsoft.Extensions.Hosting.IHostedService> starts.</span></span> <span data-ttu-id="9d39a-175">此執行應該會`Task`傳回，代表執行長時間執行之作業的存留期。</span><span class="sxs-lookup"><span data-stu-id="9d39a-175">The implementation should return a `Task` that represents the lifetime of the long running operations performed.</span></span> <span data-ttu-id="9d39a-176">呼叫`stoppingToken` [IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時所觸發的。</span><span class="sxs-lookup"><span data-stu-id="9d39a-176">The `stoppingToken` Triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span>
* <span data-ttu-id="9d39a-177">`StopAsync(CancellationToken stoppingToken)`&ndash; 當應用程式主機執行正常關機時`StopAsync` ，就會觸發。</span><span class="sxs-lookup"><span data-stu-id="9d39a-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` is triggered when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="9d39a-178">`stoppingToken`指出關閉程式應該不會再正常。</span><span class="sxs-lookup"><span data-stu-id="9d39a-178">The `stoppingToken` indicates that the shutdown process should no longer be graceful.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="9d39a-179">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="9d39a-179">Timed background tasks</span></span>

<span data-ttu-id="9d39a-180">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="9d39a-180">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="9d39a-181">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="9d39a-181">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="9d39a-182">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="9d39a-182">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="9d39a-183">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中以`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="9d39a-183">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="9d39a-184">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="9d39a-184">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="9d39a-185">若要在中`BackgroundService`使用[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="9d39a-185">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="9d39a-186">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="9d39a-186">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="9d39a-187">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d39a-187">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="9d39a-188">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="9d39a-188">In the following example:</span></span>

* <span data-ttu-id="9d39a-189">服務是非同步。</span><span class="sxs-lookup"><span data-stu-id="9d39a-189">The service is asynchronous.</span></span> <span data-ttu-id="9d39a-190">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-190">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="9d39a-191">基於示範目的，會在`DoWork`方法中等待10秒的延遲。</span><span class="sxs-lookup"><span data-stu-id="9d39a-191">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="9d39a-192"><xref:Microsoft.Extensions.Logging.ILogger>會插入服務中。</span><span class="sxs-lookup"><span data-stu-id="9d39a-192">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="9d39a-193">託管服務會建立範圍來解析已設定範圍的背景工作服務，以`DoWork`呼叫其方法。</span><span class="sxs-lookup"><span data-stu-id="9d39a-193">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="9d39a-194">`DoWork`傳回，其等候于`ExecuteAsync`： `Task`</span><span class="sxs-lookup"><span data-stu-id="9d39a-194">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="9d39a-195">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="9d39a-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="9d39a-196">託管服務會向`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="9d39a-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="9d39a-197">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="9d39a-197">Queued background tasks</span></span>

<span data-ttu-id="9d39a-198">背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="9d39a-198">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="9d39a-199">在下列`QueueHostedService`範例中：</span><span class="sxs-lookup"><span data-stu-id="9d39a-199">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="9d39a-200">方法會傳回，它`ExecuteAsync`會在中等待。 `Task` `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="9d39a-200">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="9d39a-201">佇列中的背景工作會在中`BackgroundProcessing`進行清除並執行。</span><span class="sxs-lookup"><span data-stu-id="9d39a-201">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="9d39a-202">每當在輸入裝置上選取索引`MonitorLoop` 鍵時，服務就會處理託管服務`w`的佇列工作：</span><span class="sxs-lookup"><span data-stu-id="9d39a-202">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="9d39a-203">會插入`MonitorLoop`服務中。 `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="9d39a-203">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="9d39a-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem`呼叫以將工作專案排入佇列。</span><span class="sxs-lookup"><span data-stu-id="9d39a-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="9d39a-205">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="9d39a-205">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="9d39a-206">託管服務會向`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="9d39a-206">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9d39a-207">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="9d39a-207">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="9d39a-208">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="9d39a-208">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9d39a-209">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="9d39a-209">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="9d39a-210">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="9d39a-210">Background task that runs on a timer.</span></span>
* <span data-ttu-id="9d39a-211">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="9d39a-211">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="9d39a-212">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="9d39a-212">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="9d39a-213">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="9d39a-213">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="9d39a-214">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9d39a-214">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9d39a-215">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="9d39a-215">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="9d39a-216">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="9d39a-216">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="9d39a-217">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="9d39a-217">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="9d39a-218">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="9d39a-218">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="9d39a-219">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="9d39a-219">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="9d39a-220">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="9d39a-220">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="9d39a-221">Package</span><span class="sxs-lookup"><span data-stu-id="9d39a-221">Package</span></span>

<span data-ttu-id="9d39a-222">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="9d39a-222">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="9d39a-223">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="9d39a-223">IHostedService interface</span></span>

<span data-ttu-id="9d39a-224">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="9d39a-224">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="9d39a-225">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="9d39a-225">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="9d39a-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d39a-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="9d39a-227">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-227">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="9d39a-228">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-228">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="9d39a-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="9d39a-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="9d39a-230">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d39a-230">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="9d39a-231">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="9d39a-231">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="9d39a-232">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="9d39a-232">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="9d39a-233">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="9d39a-233">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="9d39a-234">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="9d39a-234">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="9d39a-235">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="9d39a-235">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="9d39a-236">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="9d39a-236">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="9d39a-237">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-237">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="9d39a-238">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="9d39a-238">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="9d39a-239">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="9d39a-239">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="9d39a-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="9d39a-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="9d39a-241">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="9d39a-241">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="9d39a-242">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="9d39a-242">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="9d39a-243">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="9d39a-243">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="9d39a-244">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="9d39a-244">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="9d39a-245">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-245">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="9d39a-246">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="9d39a-246">Timed background tasks</span></span>

<span data-ttu-id="9d39a-247">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="9d39a-247">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="9d39a-248">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="9d39a-248">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="9d39a-249">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="9d39a-249">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="9d39a-250">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="9d39a-250">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="9d39a-251">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="9d39a-251">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="9d39a-252">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="9d39a-252">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="9d39a-253">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="9d39a-253">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="9d39a-254">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d39a-254">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="9d39a-255">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="9d39a-255">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="9d39a-256">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="9d39a-256">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="9d39a-257">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="9d39a-257">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9d39a-258">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="9d39a-258">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="9d39a-259">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="9d39a-259">Queued background tasks</span></span>

<span data-ttu-id="9d39a-260">背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="9d39a-260">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="9d39a-261">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="9d39a-261">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="9d39a-262">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="9d39a-262">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="9d39a-263">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="9d39a-263">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="9d39a-264">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="9d39a-264">In the Index page model class:</span></span>

* <span data-ttu-id="9d39a-265">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-265">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="9d39a-266">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="9d39a-266">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="9d39a-267">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="9d39a-267">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="9d39a-268">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="9d39a-268">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="9d39a-269">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="9d39a-269">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="9d39a-270">`QueueBackgroundWorkItem`呼叫以將工作專案排入佇列：</span><span class="sxs-lookup"><span data-stu-id="9d39a-270">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9d39a-271">其他資源</span><span class="sxs-lookup"><span data-stu-id="9d39a-271">Additional resources</span></span>

* [<span data-ttu-id="9d39a-272">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="9d39a-272">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
