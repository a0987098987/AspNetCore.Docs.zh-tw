---
title: .NET 泛型主機
author: guardrex
description: 了解 .NET 中的泛型主機，其負責啟動應用程式以及管理存留期。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: 15ce81a4226921ce053096751d7678ada36235c0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728969"
---
# <a name="net-generic-host"></a>.NET 泛型主機

作者：[Luke Latham](https://github.com/guardrex)

.NET 應用程式會設定並啟動「主機」。 主機負責應用程式啟動和存留期管理。 本主題包含 ASP.NET Core 泛型主機 ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder))，該主機適用於裝載未處理 HTTP 要求的應用程式。 如需 Web 主機 ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) 的說明，請參閱 [Web 主機](xref:fundamentals/host/web-host)主題。

泛型主機的目標是將 HTTP 管線與 Web 主機 API 分離，以允許更廣泛的主機陣列案例。 傳訊、背景工作及其他以泛型主機為基礎的非 HTTP 工作負載都可利用跨領域功能，例如設定、相依性插入 (DI) 和記錄。

泛型主機是 ASP.NET Core 2.1 的新功能，不適用於虛擬主機案例。 針對虛擬主機案例，請使用 [Web 主機](xref:fundamentals/host/web-host)。 泛型主機目前正在開發中，未來版本將取代 Web 主機並成為 HTTP 和非 HTTP 案例中的主要主機 API。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例應用程式時，請使用「外部或整合式終端機」。 請勿在 `internalConsole` 中執行範例。

若要在 Visual Studio Code 中設定主控台：

1. 開啟 *.vscode/launch.json* 檔案。
1. 在 [.NET Core Launch (console)] \(.NET Core 啟動 (主控台)\) 設定中，尋找 **console** 項目。 將此值設定為 `externalTerminal` 或 `integratedTerminal`。

## <a name="introduction"></a>簡介

泛型主機程式庫位於 [Microsoft.Extensions.Hosting 命名空間](/dotnet/api/microsoft.extensions.hosting)，並由 [Microsoft.Extensions.Hosting NuGet 套件](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/)提供。 [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) 中繼套件包含 `Microsoft.Extensions.Hosting` 套件。

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 是程式碼執行的進入點。 每個 `IHostedService` 實作會依 [ConfigureServices 中的服務註冊](#configureservices)順序執行。 當主機啟動時，會在每個 `IHostedService` 上呼叫 [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync)；當主機順利關閉時，則會依反向註冊順序呼叫 [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync)。

## <a name="set-up-a-host"></a>設定主機

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) 是程式庫和應用程式用來初始化、建置及執行主機的主要元件：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>主機組態

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) 依賴下列方法來設定主機組態值：

* 設定產生器
* 擴充方法組態

### <a name="configuration-builder"></a>組態產生器

主機產生器組態是透過在 [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) 實作上呼叫 [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) 來建立。 `ConfigureHostConfiguration` 使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 來建立主機的 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)。 設定產生器會初始化 [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment)，以供應用程式的建置程序使用。 `ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。 主機使用設定最後一個值的任何選項。

*hostsettings.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

使用 `ConfigureHostConfiguration` 的 `HostBuilder` 組態範例：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 擴充方法目前無法剖析 [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) 傳回的組態區段 (例如 `.AddConfiguration(Configuration.GetSection("section"))`)。 `GetSection` 方法會將組態索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:environment`)。 `AddConfiguration` 方法預期索引鍵要符合 `HostBuilder` 索引鍵 (例如 `environment`)。 索引鍵上的區段名稱存在可避免區段的值設定主機。 這個問題將在近期的版本中解決。 如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。

### <a name="extension-method-configuration"></a>擴充方法組態

在 `IHostBuilder` 實作上呼叫擴充方法可設定內容根目錄和環境。

#### <a name="content-root"></a>內容根目錄

此設定可決定主機開始搜尋內容檔案的位置。

**索引鍵**：contentRoot  
**類型**：*string*  
**預設值**：預設為應用程式組件所在的資料夾。  
**設定使用**：`UseContentRoot`  
**環境變數**：`ASPNETCORE_CONTENTROOT`

如果路徑不存在，就無法啟動主機。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>環境

設定應用程式的[環境](xref:fundamentals/environments)。

**索引鍵**：environment  
**類型**：*string*  
**預設值**：Production  
**設定使用**：`UseEnvironment`  
**環境變數**：`ASPNETCORE_ENVIRONMENT`

環境可以設定為任何值。 架構定義的值包括 `Development`、`Staging` 和 `Production`。 值不區分大小寫。 根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。 使用 [Visual Studio](https://www.visualstudio.com/) 時，可能會在 *launchSettings.json* 檔案設定環境變數。 如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

應用程式產生器設定是透過在 [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) 實作上呼叫 [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) 來建立。 `ConfigureAppConfiguration` 使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 來建立應用程式的 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)。 `ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。 應用程式使用設定最後一個值的任何選項。 您可以從後續作業的 [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) 及 [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services) 中存取 `ConfigureAppConfiguration` 所建立的設定。

使用 `ConfigureAppConfiguration` 的應用程式組態範例：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) 擴充方法目前無法剖析 [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) 傳回的組態區段 (例如 `.AddConfiguration(Configuration.GetSection("section"))`)。 `GetSection` 方法會將組態索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:Logging:LogLevel:Default`)。 `AddConfiguration` 方法預期完全符合組態索引鍵 (例如 `Logging:LogLevel:Default`)。 存在於索引鍵上的區段名稱可避免區段的值設定應用程式。 這個問題將在近期的版本中解決。 如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) 會將服務新增至應用程式的[相依性插入](xref:fundamentals/dependency-injection)容器。 `ConfigureServices` 可以多次呼叫，其結果是累加的。

託管服務是具有背景工作邏輯的類別，能夠實作 [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) 介面。 如需詳細資訊，請參閱[搭配託管服務的背景工作](xref:fundamentals/host/hosted-services)主題。

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用 `AddHostedService` 擴充方法，將存留期事件 `LifetimeEventsHostedService` 和計時背景工作 `TimedHostedService` 等服務新增至應用程式：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) 會新增用來設定所提供 [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder) 的委派。 `ConfigureLogging` 可以多次呼叫，其結果是累加的。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) 會接聽 `Ctrl+C`/SIGINT 或 SIGTERM，並呼叫 [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) 以啟動關機程序。 `UseConsoleLifetime` 會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。 [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) 會預先註冊為預設存留期實作。 系統會使用最後一個註冊的存留期。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>容器設定

為支援插入其他容器，主機可以接受 [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1)。 提供處理站不是 DI 容器註冊的一部分，而是用來建立實體 DI 容器的主機內建功能。 [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) 會覆寫用來建立應用程式服務提供者的預設處理站。

自訂容器設定是由 [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) 方法管理。 `ConfigureContainer` 提供在基礎主機 API 上設定容器的強型別體驗。 `ConfigureContainer` 可以多次呼叫，其結果是累加的。

建立應用程式的服務容器：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

提供服務容器處理站：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

使用處理站，並設定應用程式的自訂服務容器：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>擴充性

主機擴充性是透過 `IHostBuilder` 上的擴充方法執行。 下列範例示範擴充方法如何使用 [RabbitMQ](https://www.rabbitmq.com/) 擴充 `IHostBuilder` 實作。 此擴充方法 (位於應用程式中的其他位置) 會註冊 RabbitMQ `IHostedService`：

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>管理主機

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) 實作都負責啟動及停止服務容器中已註冊的 `IHostedService` 實作。

### <a name="run"></a>執行

[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止：

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 `Task`：

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) 會啟用主控台支援、建置和啟動主機，以及等候 `Ctrl+C`/SIGINT 或 SIGTERM 關機。

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Start 和 StopAsync

[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) 會同步啟動主機。

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) 會嘗試在提供的逾時內停止主機。

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync 和 StopAsync

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) 會啟動應用程式。

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) 會停止應用程式。

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>WaitForShutdown

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) 是透過 [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime) 觸發，例如 [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (接聽 `Ctrl+C`/SIGINT 或 SIGTERM)。 `WaitForShutdown` 會呼叫 [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync)。

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) 會傳回透過指定語彙基元觸發關機時所完成的 `Task`，並呼叫 [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync)。

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>外部控制

主機的外部控制可以使用可從外部呼叫的方法來達成：

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) 是在 [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) 開始時呼叫，並等到完成後再繼續進行。 這可用來將啟動延遲到外部事件發出訊號為止。

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment 介面

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) 提供應用程式主控環境的相關資訊。 使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime 介面

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) 允許啟動後和關機活動，包括順利關機要求。 在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。

| 取消語彙基元 | 觸發時機&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | 已完全啟動主機。 |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | 主機正在完成正常關機程序。 應該處理所有要求。 關機封鎖，直到完成此事件。 |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | 主機正在執行正常關機程序。 可能仍在處理要求。 關機封鎖，直到完成此事件。 |

建構函式將 `IApplicationLifetime` 服務插入任何類別。 [範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用建構函式插入 `LifetimeEventsHostedService` 類別 (`IHostedService`實作) 來註冊事件。

*LifetimeEventsHostedService.cs*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。 當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="additional-resources"></a>其他資源

* [使用託管服務的背景工作](xref:fundamentals/host/hosted-services)
* [GitHub 上的主控存放庫範例](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
