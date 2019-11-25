---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/19/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: da3c2679005714a3d82de94cf3bc3c809aa3500d
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239725"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>在 ASP.NET Core 中使用託管服務的背景工作

By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)

::: moniker range=">= aspnetcore-3.0"

在 ASP.NET Core 中，背景工作可實作為「託管服務」。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。 The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="worker-service-template"></a>背景工作服務範本

ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。 An app created from the Worker Service template specifies the Worker SDK in its project file:

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

使用範本作為裝載服務應用程式的基礎：

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a>封裝

An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package. For example, see the sample app's project file (*BackgroundTasksSample.csproj*).

For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework. An explicit package reference in the app's project file isn't required.

## <a name="ihostedservice-interface"></a>IHostedService 介面

The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。 `StartAsync` is called *before*:

  * The app's request processing pipeline is configured (`Startup.Configure`).
  * The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.

  The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called. To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:

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

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。 `StopAsync` 包含用來結束背景工作的邏輯。 實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。

  取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。 在權杖上要求取消時：

  * 應終止應用程式正在執行的任何剩餘背景作業。
  * 在 `StopAsync` 中呼叫的任何方法應立即傳回。

  不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。

  如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。 因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。

  若要延長預設的五秒鐘關機逾時，請設定：

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。 如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。
  * 使用 Web 主機時，關機逾時會裝載組態設定。 如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。

託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。 如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。

## <a name="backgroundservice-base-class"></a>BackgroundService base class

<xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.

[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service. The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service. No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/aspnet/Extensions/issues/2149), such as by calling `await`. Avoid performing long, blocking initialization work in `ExecuteAsync`. The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.

The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called. Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service. Otherwise, the service ungracefully shuts down at the shutdown timeout. For more information, see the [IHostedService interface](#ihostedservice-interface) section.

## <a name="timed-background-tasks"></a>計時背景工作

計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。 此計時器會觸發工作的 `DoWork` 方法。 計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>在背景工作中使用範圍服務

To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope. 根據預設，不會針對託管服務建立任何範圍。

範圍背景工作服務包含背景工作的邏輯。 在以下範例中：

* The service is asynchronous. `DoWork` 方法會傳回 `Task`。 For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.
* An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method. `DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*). The hosted service is registered with the `AddHostedService` extension method:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>排入佇列背景工作

A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

In the following `QueueHostedService` example:

* The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.
* Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.
* Work items are awaited before the service stops in `StopAsync`.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:

* The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.
* The work item simulates a long-running background task:
  * Three 5-second delays are executed (`Task.Delay`).
  * A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*). The hosted service is registered with the `AddHostedService` extension method:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在 ASP.NET Core 中，背景工作可實作為「託管服務」。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。 The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="package"></a>封裝

參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。

## <a name="ihostedservice-interface"></a>IHostedService 介面

託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 此介面針對主機所管理的物件定義兩種方法：

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。 使用 [Web 主機](xref:fundamentals/host/web-host)時，是在啟動伺服器和觸發 [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) 之後才呼叫 `StartAsync`。 使用 [泛型主機](xref:fundamentals/host/generic-host)時，是在觸發 `ApplicationStarted` 之前呼叫 `StartAsync`。

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。 `StopAsync` 包含用來結束背景工作的邏輯。 實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。

  取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。 在權杖上要求取消時：

  * 應終止應用程式正在執行的任何剩餘背景作業。
  * 在 `StopAsync` 中呼叫的任何方法應立即傳回。

  不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。

  如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。 因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。

  若要延長預設的五秒鐘關機逾時，請設定：

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。 如需詳細資訊，請參閱<xref:fundamentals/host/generic-host#shutdown-timeout>。
  * 使用 Web 主機時，關機逾時會裝載組態設定。 如需詳細資訊，請參閱<xref:fundamentals/host/web-host#shutdown-timeout>。

託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。 如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。

## <a name="timed-background-tasks"></a>計時背景工作

計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。 此計時器會觸發工作的 `DoWork` 方法。 計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>在背景工作中使用範圍服務

若要在 `IHostedService` 內使用[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立一個範圍。 根據預設，不會針對託管服務建立任何範圍。

範圍背景工作服務包含背景工作的邏輯。 在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>排入佇列背景工作

A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 [BackgroundService](#backgroundservice-base-class) 執行，這是實作長時間執行 `IHostedService` 的基底類別：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

在索引頁面模型類別中：

* 將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。
* 插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。 處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。 建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。 `QueueBackgroundWorkItem` is called to enqueue a work item:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
