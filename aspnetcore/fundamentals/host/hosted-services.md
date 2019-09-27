---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 5a29952c4e50edb953fa03c6ea1a1ae27b728bb0
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317726"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="eba6d-103">在 ASP.NET Core 中使用託管服務的背景工作</span><span class="sxs-lookup"><span data-stu-id="eba6d-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="eba6d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="eba6d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="eba6d-105">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="eba6d-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="eba6d-106">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="eba6d-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="eba6d-107">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="eba6d-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="eba6d-108">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="eba6d-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="eba6d-109">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="eba6d-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="eba6d-110">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="eba6d-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="eba6d-111">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="eba6d-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="eba6d-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eba6d-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="eba6d-113">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="eba6d-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="eba6d-114">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="eba6d-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="eba6d-115">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="eba6d-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="eba6d-116">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="eba6d-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="eba6d-117">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="eba6d-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="eba6d-118">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="eba6d-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="eba6d-119">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="eba6d-119">Worker Service template</span></span>

<span data-ttu-id="eba6d-120">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="eba6d-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="eba6d-121">使用範本作為裝載服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="eba6d-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eba6d-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eba6d-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="eba6d-123">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="eba6d-123">Create a new project.</span></span>
1. <span data-ttu-id="eba6d-124">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="eba6d-125">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-125">Select **Next**.</span></span>
1. <span data-ttu-id="eba6d-126">在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。</span><span class="sxs-lookup"><span data-stu-id="eba6d-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="eba6d-127">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-127">Select **Create**.</span></span>
1. <span data-ttu-id="eba6d-128">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="eba6d-129">選取 [背景工作服務] 範本。</span><span class="sxs-lookup"><span data-stu-id="eba6d-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="eba6d-130">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eba6d-131">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="eba6d-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="eba6d-132">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="eba6d-132">Create a new project.</span></span>
1. <span data-ttu-id="eba6d-133">在提要欄位中選取 [ **.Net Core** ] 底下的 [**應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="eba6d-134">選取  **ASP.NET Core**下的 背景**工作角色**。</span><span class="sxs-lookup"><span data-stu-id="eba6d-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="eba6d-135">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-135">Select **Next**.</span></span>
1. <span data-ttu-id="eba6d-136">針對 [**目標 Framework**] 選取 [ **.net Core 3.0** ]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="eba6d-137">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-137">Select **Next**.</span></span>
1. <span data-ttu-id="eba6d-138">在 [**專案名稱**] 欄位中提供名稱。</span><span class="sxs-lookup"><span data-stu-id="eba6d-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="eba6d-139">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="eba6d-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="eba6d-140">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="eba6d-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="eba6d-141">從命令殼層以 [dotnet new](/dotnet/core/tools/dotnet-new) 命令使用背景工作服務 (`worker`) 範本。</span><span class="sxs-lookup"><span data-stu-id="eba6d-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="eba6d-142">在下列範例中，已建立名為 `ContosoWorker` 的背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="eba6d-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="eba6d-143">當命令執行時，會自動建立 `ContosoWorker` 應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="eba6d-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="eba6d-144">套件</span><span class="sxs-lookup"><span data-stu-id="eba6d-144">Package</span></span>

<span data-ttu-id="eba6d-145">針對 ASP.NET Core 應用程式，會隱含地新增對[Microsoft Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="eba6d-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="eba6d-146">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="eba6d-146">IHostedService interface</span></span>

<span data-ttu-id="eba6d-147"><xref:Microsoft.Extensions.Hosting.IHostedService>介面會針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="eba6d-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="eba6d-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="eba6d-149">`StartAsync`會*在之前*呼叫：</span><span class="sxs-lookup"><span data-stu-id="eba6d-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="eba6d-150">應用程式的要求處理管線已設定（`Startup.Configure`）。</span><span class="sxs-lookup"><span data-stu-id="eba6d-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="eba6d-151">伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。</span><span class="sxs-lookup"><span data-stu-id="eba6d-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="eba6d-152">您可以變更預設行為，以便在應用程式的管線`StartAsync`已設定且`ApplicationStarted`呼叫之後，託管服務才會執行。</span><span class="sxs-lookup"><span data-stu-id="eba6d-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="eba6d-153">若要變更預設行為，請在呼叫`VideosWatcher` `ConfigureWebHostDefaults`之後新增託管服務（在下列範例中）：</span><span class="sxs-lookup"><span data-stu-id="eba6d-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="eba6d-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="eba6d-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="eba6d-155">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="eba6d-156">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="eba6d-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="eba6d-157">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="eba6d-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="eba6d-158">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="eba6d-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="eba6d-159">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="eba6d-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="eba6d-160">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="eba6d-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="eba6d-161">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="eba6d-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="eba6d-162">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="eba6d-163">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="eba6d-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="eba6d-164">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="eba6d-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="eba6d-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="eba6d-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="eba6d-166">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="eba6d-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="eba6d-167">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="eba6d-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="eba6d-168">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="eba6d-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="eba6d-169">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="eba6d-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="eba6d-170">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="eba6d-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="eba6d-171">BackgroundService</span></span>

<span data-ttu-id="eba6d-172">`BackgroundService`是用來進行長時間<xref:Microsoft.Extensions.Hosting.IHostedService>執行的基類。</span><span class="sxs-lookup"><span data-stu-id="eba6d-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="eba6d-173">`BackgroundService``ExecuteAsync(CancellationToken stoppingToken)`提供抽象方法以包含服務的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-173">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="eba6d-174">呼叫[IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時，會觸發。`stoppingToken`</span><span class="sxs-lookup"><span data-stu-id="eba6d-174">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="eba6d-175">這個方法`Task`的實作為傳回，代表背景服務的整個存留期。</span><span class="sxs-lookup"><span data-stu-id="eba6d-175">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="eba6d-176">此外，*也可以選擇性地*覆寫在`IHostedService`上定義的方法，以執行服務的啟動和關閉程式碼：</span><span class="sxs-lookup"><span data-stu-id="eba6d-176">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="eba6d-177">`StopAsync(CancellationToken cancellationToken)`&ndash; 當應用程式主機執行正常關機`StopAsync`時，會呼叫。</span><span class="sxs-lookup"><span data-stu-id="eba6d-177">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="eba6d-178">當主機決定強制終止服務時，會發出信號。`cancellationToken`</span><span class="sxs-lookup"><span data-stu-id="eba6d-178">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="eba6d-179">如果覆寫這個方法，您**必須**呼叫（和`await`）基類方法，以確保服務正常關閉。</span><span class="sxs-lookup"><span data-stu-id="eba6d-179">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="eba6d-180">`StartAsync(CancellationToken cancellationToken)`&ndash; 呼叫`StartAsync`來啟動背景服務。</span><span class="sxs-lookup"><span data-stu-id="eba6d-180">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="eba6d-181">如果啟動進程中斷，就會發出信號。`cancellationToken`</span><span class="sxs-lookup"><span data-stu-id="eba6d-181">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="eba6d-182">此實`Task`作為傳回，代表服務的啟動進程。</span><span class="sxs-lookup"><span data-stu-id="eba6d-182">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="eba6d-183">在`Task`完成之前，不會再啟動任何進一步的服務。</span><span class="sxs-lookup"><span data-stu-id="eba6d-183">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="eba6d-184">如果覆寫這個方法，您**必須**呼叫（和`await`）基類方法，以確保服務能正確啟動。</span><span class="sxs-lookup"><span data-stu-id="eba6d-184">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="eba6d-185">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="eba6d-185">Timed background tasks</span></span>

<span data-ttu-id="eba6d-186">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="eba6d-186">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="eba6d-187">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="eba6d-187">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="eba6d-188">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="eba6d-188">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="eba6d-189">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中以`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="eba6d-189">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="eba6d-190">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="eba6d-190">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="eba6d-191">若要在中`BackgroundService`使用[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。</span><span class="sxs-lookup"><span data-stu-id="eba6d-191">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="eba6d-192">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="eba6d-192">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="eba6d-193">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-193">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="eba6d-194">在以下範例中：</span><span class="sxs-lookup"><span data-stu-id="eba6d-194">In the following example:</span></span>

* <span data-ttu-id="eba6d-195">服務是非同步。</span><span class="sxs-lookup"><span data-stu-id="eba6d-195">The service is asynchronous.</span></span> <span data-ttu-id="eba6d-196">`DoWork` 方法會傳回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-196">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="eba6d-197">基於示範目的，會在`DoWork`方法中等待10秒的延遲。</span><span class="sxs-lookup"><span data-stu-id="eba6d-197">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="eba6d-198"><xref:Microsoft.Extensions.Logging.ILogger>會插入服務中。</span><span class="sxs-lookup"><span data-stu-id="eba6d-198">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="eba6d-199">託管服務會建立範圍來解析已設定範圍的背景工作服務，以`DoWork`呼叫其方法。</span><span class="sxs-lookup"><span data-stu-id="eba6d-199">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="eba6d-200">`DoWork`傳回，其等候于`ExecuteAsync`： `Task`</span><span class="sxs-lookup"><span data-stu-id="eba6d-200">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="eba6d-201">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="eba6d-201">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="eba6d-202">託管服務會向`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="eba6d-202">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="eba6d-203">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="eba6d-203">Queued background tasks</span></span>

<span data-ttu-id="eba6d-204">背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="eba6d-204">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="eba6d-205">在下列`QueueHostedService`範例中：</span><span class="sxs-lookup"><span data-stu-id="eba6d-205">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="eba6d-206">方法會傳回，它`ExecuteAsync`會在中等待。 `Task` `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="eba6d-206">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="eba6d-207">佇列中的背景工作會在中`BackgroundProcessing`進行清除並執行。</span><span class="sxs-lookup"><span data-stu-id="eba6d-207">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="eba6d-208">每當在輸入裝置上選取索引`MonitorLoop` 鍵時，服務就會處理託管服務`w`的佇列工作：</span><span class="sxs-lookup"><span data-stu-id="eba6d-208">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="eba6d-209">會插入`MonitorLoop`服務中。 `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="eba6d-209">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="eba6d-210">`IBackgroundTaskQueue.QueueBackgroundWorkItem`呼叫以將工作專案排入佇列。</span><span class="sxs-lookup"><span data-stu-id="eba6d-210">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="eba6d-211">服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。</span><span class="sxs-lookup"><span data-stu-id="eba6d-211">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="eba6d-212">託管服務會向`AddHostedService`擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="eba6d-212">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="eba6d-213">在 ASP.NET Core 中，背景工作可實作為「託管服務」。</span><span class="sxs-lookup"><span data-stu-id="eba6d-213">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="eba6d-214">託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="eba6d-214">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="eba6d-215">本主題提供三個託管服務範例：</span><span class="sxs-lookup"><span data-stu-id="eba6d-215">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="eba6d-216">在計時器上執行的背景工作。</span><span class="sxs-lookup"><span data-stu-id="eba6d-216">Background task that runs on a timer.</span></span>
* <span data-ttu-id="eba6d-217">啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。</span><span class="sxs-lookup"><span data-stu-id="eba6d-217">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="eba6d-218">已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)</span><span class="sxs-lookup"><span data-stu-id="eba6d-218">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="eba6d-219">以循序方式執行的排入佇列背景工作。</span><span class="sxs-lookup"><span data-stu-id="eba6d-219">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="eba6d-220">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eba6d-220">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="eba6d-221">範例應用程式有兩個版本：</span><span class="sxs-lookup"><span data-stu-id="eba6d-221">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="eba6d-222">Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。</span><span class="sxs-lookup"><span data-stu-id="eba6d-222">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="eba6d-223">本主題中顯示的範例程式碼是來自 Web 主機版本的範例。</span><span class="sxs-lookup"><span data-stu-id="eba6d-223">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="eba6d-224">如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。</span><span class="sxs-lookup"><span data-stu-id="eba6d-224">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="eba6d-225">泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。</span><span class="sxs-lookup"><span data-stu-id="eba6d-225">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="eba6d-226">如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。</span><span class="sxs-lookup"><span data-stu-id="eba6d-226">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="eba6d-227">套件</span><span class="sxs-lookup"><span data-stu-id="eba6d-227">Package</span></span>

<span data-ttu-id="eba6d-228">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="eba6d-228">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="eba6d-229">IHostedService 介面</span><span class="sxs-lookup"><span data-stu-id="eba6d-229">IHostedService interface</span></span>

<span data-ttu-id="eba6d-230">託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。</span><span class="sxs-lookup"><span data-stu-id="eba6d-230">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="eba6d-231">此介面針對主機所管理的物件定義兩種方法：</span><span class="sxs-lookup"><span data-stu-id="eba6d-231">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="eba6d-232">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-232">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="eba6d-233">使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-233">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="eba6d-234">使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-234">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="eba6d-235">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。</span><span class="sxs-lookup"><span data-stu-id="eba6d-235">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="eba6d-236">`StopAsync` 包含用來結束背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-236">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="eba6d-237">實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。</span><span class="sxs-lookup"><span data-stu-id="eba6d-237">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="eba6d-238">取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。</span><span class="sxs-lookup"><span data-stu-id="eba6d-238">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="eba6d-239">在權杖上要求取消時：</span><span class="sxs-lookup"><span data-stu-id="eba6d-239">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="eba6d-240">應終止應用程式正在執行的任何剩餘背景作業。</span><span class="sxs-lookup"><span data-stu-id="eba6d-240">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="eba6d-241">在 `StopAsync` 中呼叫的任何方法應立即傳回。</span><span class="sxs-lookup"><span data-stu-id="eba6d-241">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="eba6d-242">不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。</span><span class="sxs-lookup"><span data-stu-id="eba6d-242">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="eba6d-243">如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-243">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="eba6d-244">因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。</span><span class="sxs-lookup"><span data-stu-id="eba6d-244">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="eba6d-245">若要延長預設的五秒鐘關機逾時，請設定：</span><span class="sxs-lookup"><span data-stu-id="eba6d-245">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="eba6d-246"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。</span><span class="sxs-lookup"><span data-stu-id="eba6d-246"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="eba6d-247">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="eba6d-247">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="eba6d-248">使用 Web 主機時，關機逾時會裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="eba6d-248">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="eba6d-249">如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。</span><span class="sxs-lookup"><span data-stu-id="eba6d-249">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="eba6d-250">託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。</span><span class="sxs-lookup"><span data-stu-id="eba6d-250">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="eba6d-251">如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-251">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="eba6d-252">計時背景工作</span><span class="sxs-lookup"><span data-stu-id="eba6d-252">Timed background tasks</span></span>

<span data-ttu-id="eba6d-253">計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。</span><span class="sxs-lookup"><span data-stu-id="eba6d-253">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="eba6d-254">此計時器會觸發工作的 `DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="eba6d-254">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="eba6d-255">計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：</span><span class="sxs-lookup"><span data-stu-id="eba6d-255">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="eba6d-256">服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="eba6d-256">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="eba6d-257">在背景工作中使用範圍服務</span><span class="sxs-lookup"><span data-stu-id="eba6d-257">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="eba6d-258">若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。</span><span class="sxs-lookup"><span data-stu-id="eba6d-258">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="eba6d-259">根據預設，不會針對託管服務建立任何範圍。</span><span class="sxs-lookup"><span data-stu-id="eba6d-259">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="eba6d-260">範圍背景工作服務包含背景工作的邏輯。</span><span class="sxs-lookup"><span data-stu-id="eba6d-260">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="eba6d-261">在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：</span><span class="sxs-lookup"><span data-stu-id="eba6d-261">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="eba6d-262">託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：</span><span class="sxs-lookup"><span data-stu-id="eba6d-262">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="eba6d-263">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="eba6d-263">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="eba6d-264">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="eba6d-264">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="eba6d-265">排入佇列背景工作</span><span class="sxs-lookup"><span data-stu-id="eba6d-265">Queued background tasks</span></span>

<span data-ttu-id="eba6d-266">背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：</span><span class="sxs-lookup"><span data-stu-id="eba6d-266">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="eba6d-267">在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：</span><span class="sxs-lookup"><span data-stu-id="eba6d-267">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="eba6d-268">這些服務會在 `Startup.ConfigureServices` 中註冊。</span><span class="sxs-lookup"><span data-stu-id="eba6d-268">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="eba6d-269">`IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：</span><span class="sxs-lookup"><span data-stu-id="eba6d-269">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="eba6d-270">在索引頁面模型類別中：</span><span class="sxs-lookup"><span data-stu-id="eba6d-270">In the Index page model class:</span></span>

* <span data-ttu-id="eba6d-271">將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-271">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="eba6d-272">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。</span><span class="sxs-lookup"><span data-stu-id="eba6d-272">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="eba6d-273">處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。</span><span class="sxs-lookup"><span data-stu-id="eba6d-273">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="eba6d-274">建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。</span><span class="sxs-lookup"><span data-stu-id="eba6d-274">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="eba6d-275">在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。</span><span class="sxs-lookup"><span data-stu-id="eba6d-275">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="eba6d-276">`QueueBackgroundWorkItem`呼叫以將工作專案排入佇列：</span><span class="sxs-lookup"><span data-stu-id="eba6d-276">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="eba6d-277">其他資源</span><span class="sxs-lookup"><span data-stu-id="eba6d-277">Additional resources</span></span>

* [<span data-ttu-id="eba6d-278">在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作</span><span class="sxs-lookup"><span data-stu-id="eba6d-278">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
