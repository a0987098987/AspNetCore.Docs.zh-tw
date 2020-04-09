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
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>在 ASP.NET Core 中使用託管服務的背景工作

由[李歡](https://github.com/huan086)

::: moniker range=">= aspnetcore-3.0"

在 ASP.NET Core 中，背景工作可實作為「託管服務」**。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 作用[網域服務的託管服務](xref:fundamentals/dependency-injection#service-lifetimes)。 作用域服務可以使用[依賴項注入 (DI)。](xref:fundamentals/dependency-injection)
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)([如何下載](xref:index#how-to-download-a-sample))

## <a name="worker-service-template"></a>背景工作服務範本

ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。 從輔助服務樣本建立的應用程式在其專案檔中指定輔助角色 SDK:

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

使用範本作為裝載服務應用程式的基礎：

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a>Package

基於輔助服務範本的應用使用`Microsoft.NET.Sdk.Worker`SDK,並且具有對[Microsoft.擴展.託管](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)包的顯式包引用。 例如,請參閱範例應用程式的專案檔 (*背景任務範例.csproj*)。

對於使用 SDK`Microsoft.NET.Sdk.Web`的 Web 應用,從共用框架隱式引用[Microsoft.擴展.託管](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)包。 不需要應用的專案檔中的顯式包引用。

## <a name="ihostedservice-interface"></a>IHostedService 介面

介面<xref:Microsoft.Extensions.Hosting.IHostedService>為主機管理的物件定義兩種方法:

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。 `StartAsync`在*之前呼叫*:

  * 套用的要求處理導管已`Startup.Configure`設定 。
  * 伺服器已啟動[,I應用程式終身。應用程式啟動](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)已觸發。

  可以更改默認行為,以便在配置並`StartAsync``ApplicationStarted`調用應用管道後託管服務運行。 要更改默認行為,請調用`VideosWatcher``ConfigureWebHostDefaults`後添加託管服務(在以下示例中),

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

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host#shutdown-timeout>。
  * 使用 Web 主機時，關機逾時會裝載組態設定。 如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#shutdown-timeout>。

託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。 如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。

## <a name="backgroundservice-base-class"></a>背景服務基類

<xref:Microsoft.Extensions.Hosting.BackgroundService>是實現長時間運行<xref:Microsoft.Extensions.Hosting.IHostedService>的基類。

呼叫[同步(取消權杖)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*)來執行後台服務。 實現返回表示後台<xref:System.Threading.Tasks.Task>服務的整個存留期的 。 在[ExecuteAsync 變為非同步](https://github.com/dotnet/extensions/issues/2149)之前,不會啟動任何`await`進一步的服務,例如透過呼叫 。 避免長時間執行,從而阻止`ExecuteAsync`在中進行初始化工作。 [StopAsync(取消權杖)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*)中的主機塊`ExecuteAsync`等待 完成。

調用[I 託管服務.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*)時觸發取消權杖。 觸發取消權杖`ExecuteAsync`時,應及時完成 。以正常關閉服務。 否則,服務在關機超時時不正常關閉。 有關詳細資訊,請參閱[I託管服務介面](#ihostedservice-interface)部分。

## <a name="timed-background-tasks"></a>計時背景工作

計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。 此計時器會觸發工作的 `DoWork` 方法。 計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<xref:System.Threading.Timer>不會等待以前的執行`DoWork`完成,因此顯示的方法可能並不適合每個方案。 [互鎖.增量](xref:System.Threading.Interlocked.Increment*)用於將執行計數器增量為原子操作,從而確保多個線程不會同時`executionCount`更新 。

該服務使用`AddHostedService`擴充`IHostBuilder.ConfigureServices`方法在 (*Program.cs*) 註冊:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>在背景工作中使用範圍服務

要在[後台服務](#backgroundservice-base-class)中使用[作用域服務](xref:fundamentals/dependency-injection#service-lifetimes),請建立作用域。 根據預設，不會針對託管服務建立任何範圍。

範圍背景工作服務包含背景工作的邏輯。 在下例中︰

* 該服務是異步的。 `DoWork` 方法會傳回 `Task`。 出於展示目的,`DoWork`該方法等待延遲 10 秒。
* <xref:Microsoft.Extensions.Logging.ILogger>注入到服務中。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

託管服務創建一個作用域來解決作用域後台任務服務以調用其`DoWork`方法。 `DoWork`傳`Task`回在中等待`ExecuteAsync`的 。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

服務在`IHostBuilder.ConfigureServices`*(Program.cs*) 註冊。 託管服務使用`AddHostedService`擴充方法註冊:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>排入佇列背景工作

背景4.x(<xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*>[暫定為內建ASP.NET核心](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

在以下`QueueHostedService`範例中:

* 方法`BackgroundProcessing`傳回`Task`在中等待`ExecuteAsync`的 。
* 佇列中的後台任務在`BackgroundProcessing`中 取消排隊和執行。
* 在服務停止`StopAsync`之前,正在等待工作項。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

`MonitorLoop`選擇金鑰時,`w`服務都會處理託管服務的佇列 :

* `IBackgroundTaskQueue`注入到服務中`MonitorLoop`。
* `IBackgroundTaskQueue.QueueBackgroundWorkItem`調用 以對工作項進行排隊。
* 工作項模擬長時間執行的背景:
  * 執行三個 5`Task.Delay`秒延遲 ()。
  * 如果`try-catch`任務被<xref:System.OperationCanceledException>取消 ,語句將捕獲。

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

服務在`IHostBuilder.ConfigureServices`*(Program.cs*) 註冊。 託管服務使用`AddHostedService`擴充方法註冊:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

`MontiorLoop``Program.Main`啟動:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

在 ASP.NET Core 中，背景工作可實作為「託管服務」**。 託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 本主題提供三個託管服務範例：

* 在計時器上執行的背景工作。
* 作用[網域服務的託管服務](xref:fundamentals/dependency-injection#service-lifetimes)。 範圍服務可以使用[相依性性性 (DI)](xref:fundamentals/dependency-injection)
* 以循序方式執行的排入佇列背景工作。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/)([如何下載](xref:index#how-to-download-a-sample))

## <a name="package"></a>Package

參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，或新增 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) 套件的套件參考。

## <a name="ihostedservice-interface"></a>IHostedService 介面

託管服務會實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 此介面針對主機所管理的物件定義兩種方法：

* [StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` 包含啟動背景工作的邏輯。 使用[Web 主機](xref:fundamentals/host/web-host)`StartAsync`時,將在伺服器啟動和[IApplicationLifetime.應用程式啟動](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*)後調用。 使用[泛型主機](xref:fundamentals/host/generic-host)時`StartAsync`, 在觸`ApplicationStarted`發之前 調用。

* [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; 當主機執行正常關機程序時觸發。 `StopAsync` 包含用來結束背景工作的邏輯。 實作 <xref:System.IDisposable> 和 [完成項 (解構函式)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) 以處置任何非受控的資源。

  取消權杖有五秒的逾時預設值，以表示關機程序應該不再順利。 在權杖上要求取消時：

  * 應終止應用程式正在執行的任何剩餘背景作業。
  * 在 `StopAsync` 中呼叫的任何方法應立即傳回。

  不過，不會在要求取消後直接放棄工作&mdash;呼叫者會等待所有工作完成。

  如果應用程式意外關閉 (例如，應用程式的處理序失敗)，可能不會呼叫 `StopAsync`。 因此，任何在 `StopAsync` 中所呼叫方法或所執行作業可能不會發生。

  若要延長預設的五秒鐘關機逾時，請設定：

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> (使用泛型主機時)。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host#shutdown-timeout>。
  * 使用 Web 主機時，關機逾時會裝載組態設定。 如需詳細資訊，請參閱 <xref:fundamentals/host/web-host#shutdown-timeout>。

託管服務會在應用程式啟動時隨即啟動，然後在應用程式關閉時正常關閉。 如果在背景工作執行期間擲回錯誤，即使未呼叫 `StopAsync`，也應該呼叫 `Dispose`。

## <a name="timed-background-tasks"></a>計時背景工作

計時背景工作使用 [System.Threading.Timer](xref:System.Threading.Timer) 類別。 此計時器會觸發工作的 `DoWork` 方法。 計時器已在 `StopAsync` 停用，並會在處置服務容器時於 `Dispose` 上進行處置：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<xref:System.Threading.Timer>不會等待以前的執行`DoWork`完成,因此顯示的方法可能並不適合每個方案。

服務是在 `Startup.ConfigureServices` 中使用 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>在背景工作中使用範圍服務

要在 中使用[作用域服務](xref:fundamentals/dependency-injection#service-lifetimes),`IHostedService`請建立作用域。 根據預設，不會針對託管服務建立任何範圍。

範圍背景工作服務包含背景工作的邏輯。 在下列範例中，<xref:Microsoft.Extensions.Logging.ILogger> 會插入至服務：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

託管服務會建立範圍來解析範圍背景工作服務，以呼叫其 `DoWork` 方法：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>排入佇列背景工作

背景任務佇列基於 .NET 框架 4.x(<xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*>[暫定為 ASP.NET 核心](https://github.com/aspnet/Hosting/issues/1280)內建):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

在 `QueueHostedService` 中，佇列中的背景工作會從佇列中清除，並作為 [BackgroundService](#backgroundservice-base-class) 執行，這是實作長時間執行 `IHostedService` 的基底類別：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

這些服務會在 `Startup.ConfigureServices` 中註冊。 `IHostedService` 實作是以 `AddHostedService` 擴充方法註冊：

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

在索引頁面模型類別中：

* 將 `IBackgroundTaskQueue` 插入至建構函式並指派給 `Queue`。
* 插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> 並指派給 `_serviceScopeFactory`。 處理站會用來建立 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 的執行個體，可用來在範圍內建立服務。 建立範圍，以便使用應用程式的 `AppDbContext` ([具範圍服務](xref:fundamentals/dependency-injection#service-lifetimes))，在 `IBackgroundTaskQueue` (單一服務) 中寫入資料庫記錄。

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

在索引頁面上選取 [新增工作]**** 按鈕時，就會執行 `OnPostAddTask` 方法。 `QueueBackgroundWorkItem`呼叫 以對工作項目進行佇列:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [在微服務中使用 IHostedService 和 BackgroundService 類別實作背景工作](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [在 Azure 應用服務中使用 Web 工作執行後臺工作](/azure/app-service/webjobs-create)
* <xref:System.Threading.Timer>
