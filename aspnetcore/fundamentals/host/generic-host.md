---
title: .NET 泛型主機
author: guardrex
description: 了解 .NET 中的泛型主機，其負責啟動應用程式以及管理存留期。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/30/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 9943c9dd2d6dd67a79186ee880b181a5915d06be
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505709"
---
# <a name="net-generic-host"></a>.NET 泛型主機

作者：[Luke Latham](https://github.com/guardrex)

.NET Core 應用程式會設定並啟動「主機」。 主機負責應用程式啟動和存留期管理。 本主題的內容涵蓋 ASP.NET Core 泛型主機 (<xref:Microsoft.Extensions.Hosting.HostBuilder>)，其對於裝載不會處理 HTTP 要求的應用程式相當實用。 如需 Web 主機 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>) 的說明，請參閱 <xref:fundamentals/host/web-host>。

泛型主機的目標是將 HTTP 管線與 Web 主機 API 分離，以允許更廣泛的主機陣列案例。 傳訊、背景工作及其他以泛型主機為基礎的非 HTTP 工作負載都可利用跨領域功能，例如設定、相依性插入 (DI) 和記錄。

泛型主機是 ASP.NET Core 2.1 的新功能，不適用於虛擬主機案例。 針對虛擬主機案例，請使用 [Web 主機](xref:fundamentals/host/web-host)。 泛型主機目前正在開發中，未來版本將取代 Web 主機並成為 HTTP 和非 HTTP 案例中的主要主機 API。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例應用程式時，請使用「外部或整合式終端機」。 請勿在 `internalConsole` 中執行範例。

若要在 Visual Studio Code 中設定主控台：

1. 開啟 *.vscode/launch.json* 檔案。
1. 在 [.NET Core Launch (console)] \(.NET Core 啟動 (主控台)\) 設定中，尋找 **console** 項目。 將此值設定為 `externalTerminal` 或 `integratedTerminal`。

## <a name="introduction"></a>簡介

可使用位於 <xref:Microsoft.Extensions.Hosting> 命名空間的泛型主機程式庫，且該程式庫由 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) 套件提供。 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本) 包含 `Microsoft.Extensions.Hosting` 套件。

<xref:Microsoft.Extensions.Hosting.IHostedService> 是程式碼執行的進入點。 每個 `IHostedService` 實作會依 [ConfigureServices 中的服務註冊](#configureservices)順序執行。 當主機啟動時，會在每個 `IHostedService` 上呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>；當主機順利關閉時，則會依反向註冊順序呼叫 <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*>。

## <a name="set-up-a-host"></a>設定主機

<xref:Microsoft.Extensions.Hosting.IHostBuilder> 是程式庫和應用程式用來初始化、建置及執行主機的主要元件：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a>預設服務

下列服務會在主機初始化期間註冊：

* [環境](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [組態](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [選項](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [記錄](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>主機組態

主機組態的建立方式：

* 呼叫 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 上的擴充方法，來設定[內容根目錄](#content-root)及[環境](#environment)。
* 在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> 中從組態提供者讀取組態。

### <a name="extension-methods"></a>擴充方法

### <a name="application-key-name"></a>應用程式索引鍵 (名稱)

[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) 屬性是在主機建構期間從主機設定當中設定。 若要明確設定該值，請使用 [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)：

**索引鍵**：applicationName  
**類型**：*string*  
**預設**：包含應用程式進入點的組件名稱。  
**設定使用**：`HostBuilderContext.HostingEnvironment.ApplicationName`  
**環境變數**：`<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` 是[選擇性和使用者定義的](#configuration-builder))

### <a name="content-root"></a>內容根目錄

此設定可決定主機開始搜尋內容檔案的位置。

**索引鍵**：contentRoot  
**類型**：*string*  
**預設值**：預設為應用程式組件所在的資料夾。  
**設定使用**：`UseContentRoot`  
**環境變數**：`<PREFIX_>CONTENTROOT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configuration-builder))

如果路徑不存在，就無法啟動主機。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>環境

設定應用程式的[環境](xref:fundamentals/environments)。

**索引鍵**：environment  
**類型**：*string*  
**預設值**：Production  
**設定使用**：`UseEnvironment`  
**環境變數**：`<PREFIX_>ENVIRONMENT` (`<PREFIX_>` 是[選擇性和使用者定義的](#configuration-builder))

環境可以設定為任何值。 架構定義的值包括 `Development`、`Staging` 和 `Production`。 值不區分大小寫。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

`ConfigureHostConfiguration` 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立主機的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。 主機組態會用來將 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 初始化，使其在應用程式建置流程中可供使用。

`ConfigureHostConfiguration` 可以多次呼叫，其結果是累加的。 主機會使用指定索引鍵上最後設定值的任何選項。

主機組態會自動流向應用程式組態 ([ConfigureAppConfiguration](#configureappconfiguration) 及剩餘的所有應用程式)。

預設不會包含任何提供者。 您必須明確地指定應用程式在 `ConfigureHostConfiguration` 之中需要哪個組態提供者，包括：

* 檔案組態 (例如，來自 *hostsettings.json* 的檔案)。
* 環境變數組態。
* 命令列引數組態。
* 任何其他必要的組態提供者。

使用之後呼叫[檔案組態提供者](xref:fundamentals/configuration/index#file-configuration-provider)的 `SetBasePath`，並透過指定應用程式基底路徑，就可使用主機的檔案設定。 範例應用程式會使用 JSON 檔案，*hostsettings.json*，並呼叫 <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> 取用檔案的主機組態設定。

若要新增主機的[環境變數組態](xref:fundamentals/configuration/index#environment-variables-configuration-provider)，請在主機建立器上呼叫 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>。 `AddEnvironmentVariables` 可接受選擇性的使用者定義前置詞。 範例應用程式會使用前置詞 `PREFIX_`。 讀取環境變數時，就會移除前置詞。 在設定範例應用程式的主機時，`PREFIX_ENVIRONMENT` 的環境變數值會變成 `environment` 索引鍵的主機組態值。

在開發期間使用 [Visual Studio](https://www.visualstudio.com/) 或以 `dotnet run` 執行應用程式時，可能會在 *Properties/launchSettings.json* 檔案中設定環境變數。 在 [Visual Studio Code](https://code.visualstudio.com/) 中，可以在開發期間於 *.vscode/launch.json* 檔案中設定環境變數。 如需詳細資訊，請參閱<xref:fundamentals/environments>。

[命令列組態](xref:fundamentals/configuration/index#command-line-configuration-provider)可透過呼叫 <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> 新增。 命令列組態會在最後新增，以便命令列引數覆寫由先前組態提供者提供的組態。

*hostsettings.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

您可以使用 [applicationName](#application-key-name) 和 [contentRoot](#content-root) 索引鍵來提供其他組態。

使用 `ConfigureHostConfiguration` 的 `HostBuilder` 組態範例：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

應用程式組態的建立方式是在 <xref:Microsoft.Extensions.Hosting.IHostBuilder> 實作上呼叫 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>。 `ConfigureAppConfiguration` 會使用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 來建立應用程式的 <xref:Microsoft.Extensions.Configuration.IConfiguration>。 `ConfigureAppConfiguration` 可以多次呼叫，其結果是累加的。 應用程式會使用指定索引鍵上最後設定值的任何選項。 您可以從後續作業的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) 及 <xref:Microsoft.Extensions.Hosting.IHost.Services*> 中存取 `ConfigureAppConfiguration` 所建立的組態。

應用程式組態會自動接收由 [ConfigureHostConfiguration](#configurehostconfiguration) 提供的主機組態。

使用 `ConfigureAppConfiguration` 的應用程式組態範例：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appsettings.Development.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appsettings.Production.json*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

若要將設定檔移至輸出目錄，請在專案檔中將設定檔指定為 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items)。 範例應用程式使用下列 `<Content>` 項目，移動其 JSON 應用程式設定檔和 *hostsettings.json*：

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> 會將服務新增至應用程式的[相依性插入](xref:fundamentals/dependency-injection)容器。 `ConfigureServices` 可以多次呼叫，其結果是累加的。

託管服務是具有背景工作邏輯的類別，可實作 <xref:Microsoft.Extensions.Hosting.IHostedService> 介面。 如需詳細資訊，請參閱<xref:fundamentals/host/hosted-services>。

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用 `AddHostedService` 擴充方法，將存留期事件 `LifetimeEventsHostedService` 和計時背景工作 `TimedHostedService` 等服務新增至應用程式：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> 會新增委派以設定提供的 <xref:Microsoft.Extensions.Logging.ILoggingBuilder>。 `ConfigureLogging` 可以多次呼叫，其結果是累加的。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> 會接聽 `Ctrl+C`/SIGINT 或 SIGTERM，並呼叫 <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 以啟動關機程序。 `UseConsoleLifetime` 會解除封鎖 [RunAsync](#runasync) 和 [WaitForShutdownAsync](#waitforshutdownasync) 等延伸模組。 <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> 會預先註冊為預設存留期實作。 系統會使用最後一個註冊的存留期。

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>容器設定

為支援插入其他容器，主機可以接受 <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory`1>。 提供處理站不是 DI 容器註冊的一部分，而是用來建立實體 DI 容器的主機內建功能。 [UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) 會覆寫用來建立應用程式服務提供者的預設處理站。

自訂容器組態由 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> 方法管理。 `ConfigureContainer` 提供在基礎主機 API 上設定容器的強型別體驗。 `ConfigureContainer` 可以多次呼叫，其結果是累加的。

建立應用程式的服務容器：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

提供服務容器處理站：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

使用處理站，並設定應用程式的自訂服務容器：

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>擴充性

主機擴充性是透過 `IHostBuilder` 上的擴充方法執行。 下列範例使用 <xref:fundamentals/host/hosted-services> 中示範的 [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) 範例顯示擴充方法如何擴充 `IHostBuilder` 實作。

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

應用程式會建立 `UseHostedService` 擴充方法以註冊在 `T` 中傳遞的裝載服務：

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a>管理主機

<xref:Microsoft.Extensions.Hosting.IHost> 實作負責啟動及停止服務容器中已註冊的 `IHostedService` 實作。

### <a name="run"></a>執行

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> 會執行應用程式並封鎖呼叫執行緒，直到主機關閉為止：

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> 會執行應用程式，並傳回觸發取消語彙基元或關機時所完成的 `Task`：

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

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> 會啟用主控台支援、建置和啟動主機，以及等候 `Ctrl+C`/SIGINT 或 SIGTERM 關機。

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> 會同步啟動主機。

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> 會嘗試在提供的逾時內停止主機。

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

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 會啟動應用程式。

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> 會停止應用程式。

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> 會透過 <xref:Microsoft.Extensions.Hosting.IHostLifetime> 觸發，像是 <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (會接聽 `Ctrl+C`/SIGINT 或 SIGTERM)。 `WaitForShutdown` 會呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> 會傳回透過指定語彙基元觸發關機時所完成的 `Task`，並呼叫 <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>。

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

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> 在 <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> 開始時呼叫，並等到完成後再繼續進行。 這可用來將啟動延遲到外部事件發出訊號為止。

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment 介面

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 提供應用程式主控環境的相關資訊。 使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：

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

如需詳細資訊，請參閱<xref:fundamentals/environments>。

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime 介面

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> 允許啟動後和關機活動，包括順利關機要求。 在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。

| 取消語彙基元 | 觸發時機&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | 已完全啟動主機。 |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | 主機正在完成正常關機程序。 應該處理所有要求。 關機封鎖，直到完成此事件。 |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | 主機正在執行正常關機程序。 可能仍在處理要求。 關機封鎖，直到完成此事件。 |

建構函式將 `IApplicationLifetime` 服務插入任何類別。 [範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/)使用建構函式插入 `LifetimeEventsHostedService` 類別 (`IHostedService` 實作) 來註冊事件。

*LifetimeEventsHostedService.cs*：

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> 會要求應用程式終止。 當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：

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

* <xref:fundamentals/host/hosted-services>
* [GitHub 上的主控存放庫範例](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
