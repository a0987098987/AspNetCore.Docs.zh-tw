---
title: 在 ASP.NET Core 中使用託管服務的背景工作
author: guardrex
description: 了解如何在 ASP.NET Core 中使用託管服務實作背景工作。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688113"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>在 ASP.NET Core 中使用託管服務的背景工作

作者：[Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

在 ASP.NET Core 中，背景工作可實作為「託管服務」。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。 已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)。
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式有兩個版本：

* Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。 本主題中顯示的範例程式碼是來自 Web 主機版本的範例。 如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。
* 泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。 如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。

## <a name="worker-service-template"></a>背景工作服務範本

ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。 使用範本作為裝載服務應用程式的基礎：

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a>套件

針對 ASP.NET Core 應用程式，會隱含地新增對[Microsoft Extensions](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)的套件參考。

## <a name="ihostedservice-interface"></a>IHostedService 介面

<xref:Microsoft.Extensions.Hosting.IHostedService>介面會針對主機所管理的物件定義兩種方法：

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。 `StartAsync`會*在之前*呼叫：

  * 應用程式的要求處理管線已設定（`Startup.Configure`）。
  * 伺服器已啟動並[IApplicationLifetime。 ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)會觸發。

  您可以變更預設行為，以便在應用程式的管線`StartAsync`已設定且`ApplicationStarted`呼叫之後，託管服務才會執行。 若要變更預設行為，請在呼叫`VideosWatcher` `ConfigureWebHostDefaults`之後新增託管服務（在下列範例中）：

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

## <a name="backgroundservice"></a>BackgroundService

`BackgroundService`是用來進行長時間<xref:Microsoft.Extensions.Hosting.IHostedService>執行的基類。 `BackgroundService``ExecuteAsync(CancellationToken stoppingToken)`提供抽象方法以包含服務的邏輯。 呼叫[IHostedService. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時，會觸發。`stoppingToken` 這個方法`Task`的實作為傳回，代表背景服務的整個存留期。

此外，*也可以選擇性地*覆寫在`IHostedService`上定義的方法，以執行服務的啟動和關閉程式碼：

* `StopAsync(CancellationToken cancellationToken)`&ndash; 當應用程式主機執行正常關機`StopAsync`時，會呼叫。 當主機決定強制終止服務時，會發出信號。`cancellationToken` 如果覆寫這個方法，您**必須**呼叫（和`await`）基類方法，以確保服務正常關閉。
* `StartAsync(CancellationToken cancellationToken)`&ndash; 呼叫`StartAsync`來啟動背景服務。 如果啟動進程中斷，就會發出信號。`cancellationToken` 此實`Task`作為傳回，代表服務的啟動進程。 在`Task`完成之前，不會再啟動任何進一步的服務。 如果覆寫這個方法，您**必須**呼叫（和`await`）基類方法，以確保服務能正確啟動。

## <a name="timed-background-tasks"></a>計時背景工作

計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。 此計時器會觸發工作的 `DoWork` 方法。 計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中以`AddHostedService`擴充方法註冊：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>在背景工作中使用範圍服務

若要在中`BackgroundService`使用[範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)，請建立範圍。 根據預設，不會針對託管服務建立任何範圍。

範圍背景工作服務包含背景工作的邏輯。 在以下範例中：

* 服務是非同步。 `DoWork` 方法會傳回 `Task`。 基於示範目的，會在`DoWork`方法中等待10秒的延遲。
* <xref:Microsoft.Extensions.Logging.ILogger>會插入服務中。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

託管服務會建立範圍來解析已設定範圍的背景工作服務，以`DoWork`呼叫其方法。 `DoWork`傳回，其等候于`ExecuteAsync`： `Task`

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。 託管服務會向`AddHostedService`擴充方法註冊：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>排入佇列背景工作

背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

在下列`QueueHostedService`範例中：

* 方法會傳回，它`ExecuteAsync`會在中等待。 `Task` `BackgroundProcessing`
* 佇列中的背景工作會在中`BackgroundProcessing`進行清除並執行。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

每當在輸入裝置上選取索引`MonitorLoop` 鍵時，服務就會處理託管服務`w`的佇列工作：

* 會插入`MonitorLoop`服務中。 `IBackgroundTaskQueue`
* `IBackgroundTaskQueue.QueueBackgroundWorkItem`呼叫以將工作專案排入佇列。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

服務會在（ `IHostBuilder.ConfigureServices` *Program.cs*）中註冊。 託管服務會向`AddHostedService`擴充方法註冊：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在 ASP.NET Core 中，背景工作可實作為「託管服務」。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 啟動[具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes)的託管服務。 已設定範圍的服務可以使用相依性[插入（DI）](xref:fundamentals/dependency-injection)
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

範例應用程式有兩個版本：

* Web 主機 &ndash; Web 主機對於裝載 Web 應用程式非常有用。 本主題中顯示的範例程式碼是來自 Web 主機版本的範例。 如需詳細資訊，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。
* 泛型主機 &ndash; 泛型主機是 ASP.NET Core 2.1 的新功能。 如需詳細資訊，請參閱[泛型主機](xref:fundamentals/host/generic-host)主題。

## <a name="package"></a>套件

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

背景工作佇列是以 .net <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 4.x （[暫時排程為 ASP.NET Core 的內建](https://github.com/aspnet/Hosting/issues/1280)）為基礎：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 <xref:Microsoft.Extensions.Hosting.BackgroundService> 執行，這是實作長時間執行 `IHostedService` 的基底類別：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

在索引頁面模型類別中：

* 將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。
* 插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。 處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。 建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

在索引頁面上選取 [新增工作] 按鈕時，就會執行 `OnPostAddTask` 方法。 `QueueBackgroundWorkItem`呼叫以將工作專案排入佇列：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
