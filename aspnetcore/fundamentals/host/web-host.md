---
title: ASP.NET Core Web 主機
author: guardrex
description: 深入了解 ASP.NET Core 中的 Web 主機，其負責啟動應用程式以及管理存留期。
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 8b72376ae4cb608c4df0cf516288188cff862b36
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894240"
---
# <a name="aspnet-core-web-host"></a>ASP.NET Core Web 主機

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET Core 應用程式會設定並啟動「主機」。 主機負責應用程式啟動和存留期管理。 至少，主機會設定伺服器和要求處理管線。 本主題涵蓋 ASP.NET Core Web 主機 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder))，這對裝載 Web 應用程式很實用。 如需 .NET 泛型主機 ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) 的說明，請參閱 <xref:fundamentals/host/generic-host>。

## <a name="set-up-a-host"></a>設定主機

::: moniker range=">= aspnetcore-2.0"

使用 ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)) 的執行個體建立主機。 這通常在應用程式的進入點執行，也就是 `Main` 方法。 在專案範本中，`Main` 位於 *Program.cs*。 一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

`CreateDefaultBuilder` 會執行下列工作：

* 將 [Kestrel](xref:fundamentals/servers/kestrel) 設定為網頁伺服器，並使用應用程式主機組態提供者設定伺服器。 如需 Kestrel 預設選項，請參閱 <xref:fundamentals/servers/kestrel#kestrel-options>。
* 設定 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory) 所傳回路徑的內容根目錄。
* 從下列項目載入[主機組態](#host-configuration-values)：
  * 前面加上 `ASPNETCORE_` 的環境變數 (例如，`ASPNETCORE_ENVIRONMENT`)。
  * 命令列引數。
* 從下列項目載入應用程式組態：
  * *appsettings.json*。
  * *appsettings.{Environment}.json*
  * 應用程式在使用輸入組件的 `Development` 環境中執行時的[使用者密碼](xref:security/app-secrets)。
  * 環境變數。
  * 命令列引數。
* 設定主控台和偵錯輸出的[記錄](xref:fundamentals/logging/index)。 記錄包含 *appsettings.json* 或 *appsettings.{Environment}.json* 檔案的記錄組態區段中指定的[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)規則。
* 在 IIS 背後執行時，啟用 [IIS 整合](xref:host-and-deploy/iis/index)。 設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)時伺服器接聽的基底路徑和連接埠。 此模組會在 IIS 和 Kestrel 之間建立反向 Proxy。 也會設定應用程式以[擷取啟動錯誤](#capture-startup-errors)。 如需 IIS 預設選項，請參閱 <xref:host-and-deploy/iis/index#iis-options>。
* 如果應用程式的環境是「開發」，請將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。 如需詳細資訊，請參閱[範圍驗證](#scope-validation)。

`CreateDefaultBuilder` 所定義的組態可以透過 [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration)、[ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)，以及 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 的其他方法和擴充方法加以覆寫及擴增。 數個範例如下：

* [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) 用來指定應用程式的其他 `IConfiguration`。 下列 `ConfigureAppConfiguration` 呼叫會新增委派，以在 *appsettings.xml* 檔案中包含應用程式組態。 可能會多次呼叫 `ConfigureAppConfiguration`。 請注意，此組態不適用於主機 (例如，伺服器 URL 或環境)。 請參閱[主機組態值](#host-configuration-values)一節。

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* 下列 `ConfigureLogging` 呼叫會新增委派，將最低的記錄層級 ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) 設定為 [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel)。 此設定會覆寫 `CreateDefaultBuilder` 所設定之 *ppsettings.Development.json* (`LogLevel.Debug`) 和 *appsettings.Production.json* (`LogLevel.Error`) 中的設定。 可能會多次呼叫 `ConfigureLogging`。

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* 下列 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) 呼叫會覆寫當 `CreateDefaultBuilder` 設定 Kestrel 時建立的預設 [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) 30,000,000 位元組：

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。 從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。 這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。

如需應用程式組態的詳細資訊，請參閱 <xref:fundamentals/configuration/index>。

> [!NOTE]
> 作為使用靜態 `CreateDefaultBuilder` 方法的替代做法，ASP.NET Core 2.x 支援從 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 建立主機的方法。 如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

使用 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 的執行個體建立主機。 建立主機通常在應用程式的進入點執行，也就是 `Main` 方法。 在專案範本中，`Main` 位於 *Program.cs*：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

`WebHostBuilder` 需要[實作 IServer 的伺服器](xref:fundamentals/servers/index)。 內建的伺服器是 [Kestrel](xref:fundamentals/servers/kestrel) 和 [HTTP.sys](xref:fundamentals/servers/httpsys) (在 ASP.NET Core 2.0 版之前，HTTP.sys 稱為 [WebListener](xref:fundamentals/servers/weblistener))。 在此範例中，[UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)會指定 Kestrel 伺服器。

「內容根目錄」會決定主機搜尋內容檔案 (例如 MVC 檢視檔案) 的位置。 [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1) 會取得 `UseContentRoot` 的預設內容根目錄。 從專案的根資料夾啟動應用程式時，會使用專案的根資料夾作為內容根目錄。 這是 [Visual Studio](https://www.visualstudio.com/) 和 [dotnet 新範本](/dotnet/core/tools/dotnet-new)中使用的預設值。

若要使用 IIS 作為反向 Proxy，請在建置主機時呼叫 [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)。 `UseIISIntegration` 不會像 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)那樣設定「伺服器」。 `UseIISIntegration` 會設定使用 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)以在 Kestrel 和 IIS 之間建立反向 Proxy 時，伺服器接聽的基底路徑和連接埠。 若要搭配使用 IIS 與 ASP.NET Core，必須指定 `UseKestrel` 和 `UseIISIntegration`。 `UseIISIntegration` 只會在 IIS 或 IIS Express 背後執行時啟動。 如需詳細資訊，請參閱 <xref:fundamentals/servers/aspnet-core-module> 與 <xref:host-and-deploy/aspnet-core-module>。

設定主機 (和 ASP.NET Core 應用程式) 的最小實作，包含指定伺服器和應用程式要求管線的組態：

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

::: moniker-end

設定主機時，可以提供 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) 和 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) 方法。 如果指定 `Startup` 類別，它必須定義 `Configure` 方法。 如需詳細資訊，請參閱<xref:fundamentals/startup>。 多次呼叫 `ConfigureServices` 會彼此附加。 對 `WebHostBuilder` 多次呼叫 `Configure` 或 `UseStartup` 則會取代先前的設定。

## <a name="host-configuration-values"></a>主機組態值

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 依賴下列方法來設定主機組態值：

* 主機建立器組態，其中包含 `ASPNETCORE_{configurationKey}` 格式的環境變數。 例如，`ASPNETCORE_ENVIRONMENT`。
* [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) 和 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 之類的延伸模組 (請參閱[覆寫組態](#override-configuration)一節)。
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) 和相關聯的索引鍵。 使用 `UseSetting` 設定值時，此值會設定為字串，而不論其類型為何。

主機使用設定最後一個值的任何選項。 如需詳細資訊，請參閱下一節的[覆寫組態](#override-configuration)。

### <a name="application-key-name"></a>應用程式索引鍵 (名稱)

在主機建構期間呼叫 [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) 或 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) 時，會自動設定 [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) 屬性。 該值會設定為包含應用程式進入點的組件名稱。 若要明確設定該值，請使用 [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)：

**索引鍵**：applicationName  
**類型**：*string*  
**預設**：包含應用程式進入點的組件名稱。  
**設定使用**：`UseSetting`  
**環境變數**：`ASPNETCORE_APPLICATIONKEY`

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a>擷取啟動錯誤

此設定會控制啟動錯誤的擷取。

**索引鍵**：captureStartupErrors  
**類型**：*bool* (`true` 或 `1`)  
**預設值**：預設為 `false`，除非應用程式執行時在 IIS 背後有 Kestrel，此時預設值即為 `true`。  
**設定使用**：`CaptureStartupErrors`  
**環境變數**：`ASPNETCORE_CAPTURESTARTUPERRORS`

當它為 `false` 時，啟動期間發生的錯誤會導致主機結束。 當它為 `true` 時，主機會擷取啟動期間的例外狀況，並嘗試啟動伺服器。

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a>內容根目錄

此設定可決定 ASP.NET Core 開始搜尋內容檔案 (例如 MVC 檢視) 的位置。 

**索引鍵**：contentRoot  
**類型**：*string*  
**預設值**：預設為應用程式組件所在的資料夾。  
**設定使用**：`UseContentRoot`  
**環境變數**：`ASPNETCORE_CONTENTROOT`

內容根目錄也作為 [Web 根目錄設定](#web-root)的基底路徑。 如果路徑不存在，就無法啟動主機。

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a>詳細錯誤

決定是否應該擷取詳細的錯誤。

**索引鍵**：detailedErrors  
**類型**：*bool* (`true` 或 `1`)  
**預設值**：false  
**設定使用**：`UseSetting`  
**環境變數**：`ASPNETCORE_DETAILEDERRORS`

啟用時 (或當 <a href="#environment">Environment</a> 設定為 `Development` 時)，應用程式會擷取詳細例外狀況。

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a>環境

設定應用程式的環境。

**索引鍵**：environment  
**類型**：*string*  
**預設值**：Production  
**設定使用**：`UseEnvironment`  
**環境變數**：`ASPNETCORE_ENVIRONMENT`

環境可以設定為任何值。 架構定義的值包括 `Development`、`Staging` 和 `Production`。 值不區分大小寫。 根據預設，*Environment* 是從 `ASPNETCORE_ENVIRONMENT` 環境變數讀取。 使用 [Visual Studio](https://www.visualstudio.com/) 時，可能會在 *launchSettings.json* 檔案設定環境變數。 如需詳細資訊，請參閱<xref:fundamentals/environments>。

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a>裝載啟動組件

設定應用程式的裝載啟動組件。

**索引鍵**：hostingStartupAssemblies  
**類型**：*string*  
**預設值**：空字串  
**設定使用**：`UseSetting`  
**環境變數**：`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

在啟動時載入的裝載啟動組件字串，以分號分隔。

雖然設定值會預設為空字串，但裝載啟動組件一律會包含應用程式的組件。 提供裝載啟動組件時，它們會新增至應用程式的組件，以便在應用程式在啟動時建置其通用服務時載入。

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a>偏好裝載 URL

表示主機是否應接聽使用 `WebHostBuilder` 設定的 URL，而不是 `IServer` 實作所設定的 URL。

**索引鍵**：preferHostingUrls  
**類型**：*bool* (`true` 或 `1`)  
**預設值**：true  
**設定使用**：`PreferHostingUrls`  
**環境變數**：`ASPNETCORE_PREFERHOSTINGURLS`

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a>防止裝載啟動

可防止自動載入裝載啟動組件，包括應用程式組件所設定的裝載啟動組件。 如需詳細資訊，請參閱<xref:fundamentals/configuration/platform-specific-configuration>。

**索引鍵**preventHostingStartup  
**類型**：*bool* (`true` 或 `1`)  
**預設值**：false  
**設定使用**：`UseSetting`  
**環境變數**：`ASPNETCORE_PREVENTHOSTINGSTARTUP`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a>伺服器 URL

表示伺服器應該接聽要求的 IP 位址或主機位址，以及連接埠和通訊協定。

**索引鍵**：urls  
**類型**：*string*  
**預設值**：http://localhost:5000  
**設定使用**：`UseUrls`  
**環境變數**：`ASPNETCORE_URLS`

設定為伺服器應該回應的 URL 前置清單，並以分號 (;) 分隔。 例如，`http://localhost:123`。 使用 "\*"，表示伺服器應接聽任何 IP 位址或主機名稱上的要求，並使用指定的連接埠和通訊協定 (例如，`http://*:5000`)。 通訊協定 (`http://` 或 `https://`) 必須包含在每個 URL 中。 伺服器之間支援的格式有所不同。

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

Kestrel 有它自己的端點設定 API。 如需詳細資訊，請參閱<xref:fundamentals/servers/kestrel#endpoint-configuration>。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a>關機逾時

指定等待 Web 主機關機的時間長度。

**索引鍵**：shutdownTimeoutSeconds  
**類型**：*int*  
**預設值**：5  
**設定使用**：`UseShutdownTimeout`  
**環境變數**：`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

雖然索引鍵接受 *int* 與 `UseSetting` (例如 `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)，[UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) 擴充方法會採用 [TimeSpan](/dotnet/api/system.timespan)。

在逾時期間，裝載會：

* 觸發 [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)。
* 嘗試停止託管的服務，並記錄無法停止之服務的任何錯誤。

如果在所有的託管服務停止之前逾時期限已到期，則應用程式關閉時，會停止任何剩餘的作用中服務。 即使服務尚未完成處理也會停止。 如果服務需要更多時間才能停止，請增加逾時。

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a>啟動組件

決定要搜尋 `Startup` 類別的組件。

**索引鍵**：startupAssembly  
**類型**：*string*  
**預設值**：應用程式的組件  
**設定使用**：`UseStartup`  
**環境變數**：`ASPNETCORE_STARTUPASSEMBLY`

可以參考依名稱 (`string`) 或類型 (`TStartup`) 的組件。 如果呼叫多個 `UseStartup` 方法，最後一個將會優先。

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a>Web 根目錄

設定應用程式靜態資產的相對路徑。

**索引鍵**：webroot  
**類型**：*string*  
**預設值**：如果未指定，則預設值為 "(Content Root)/wwwroot"，如果該路徑存在的話。 如果路徑不存在，則會使用無作業檔案提供者。  
**設定使用**：`UseWebRoot`  
**環境變數**：`ASPNETCORE_WEBROOT`

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a>覆寫組態

使用 [Configuration](xref:fundamentals/configuration/index) 來設定 Web 主機。 在下列範例中，主機組態選擇性指定於 *hostsettings.json* 檔案中。 從 *hostsettings.json* 檔案載入的任何組態，可以用命令列引數加以覆寫。 建置的組態 (在 `config` 中) 用以使用 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機。 `IWebHostBuilder` 設定會新增到應用程式的組態，但反之則不然 &mdash;`ConfigureAppConfiguration` 並不會影響 `IWebHostBuilder` 組態。

::: moniker range=">= aspnetcore-2.0"

首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

*hostsettings.json*：

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

首先使用 *hostsettings.json* 組態來覆寫 `UseUrls` 所提供的組態，其次是命令列引數組態：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

*hostsettings.json*：

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 擴充方法目前無法剖析 `GetSection` 傳回的組態區段 (例如 `.UseConfiguration(Configuration.GetSection("section"))`。 `GetSection` 方法會將設定索引鍵篩選到要求的區段，但保留索引鍵上的區段名稱 (例如 `section:urls`、`section:environment`)。 `UseConfiguration` 方法預期索引鍵要符合 `WebHostBuilder` 索引鍵 (例如 `urls`、`environment`)。 索引鍵上的區段名稱存在可避免區段的值設定主機。 這個問題將在近期的版本中解決。 如需詳細資訊和因應措施，請參閱 [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (將設定區段傳遞到 WebHostBuilder.UseConfiguration 會使用完整索引鍵)。
>
> `UseConfiguration` 只會將索引鍵從提供的 `IConfiguration` 複製到主機建立器的組態。 因此，為 JSON、INI 和 XML 設定檔設定 `reloadOnChange: true` 沒有任何作用。

若要指定主機在特定 URL 上執行，所要的值可以在執行 [dotnet run](/dotnet/core/tools/dotnet-run) 時從命令提示字元傳入。 命令列引數會覆寫 *hostsettings.json* 檔案的 `urls` 值，且伺服器會接聽連接埠 8080：

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>管理主機

::: moniker range=">= aspnetcore-2.0"

**執行**

`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：

```csharp
host.Run();
```

**Start**

藉由呼叫 `Start` 方法，以非封鎖方式執行主機：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

應用程式可以使用預先設定的 `CreateDefaultBuilder` 預設值，使用便利的靜態方法初始化並啟動新的主機。 這些方法會啟動伺服器而無主控台輸出，且在使用 [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) 時等候中斷 (Ctrl-C/SIGINT 或 SIGTERM)：

**Start(RequestDelegate app)**

使用 `RequestDelegate` 啟動：

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應 `WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。 應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。

**Start(string url, RequestDelegate app)**

使用 URL 和 `RequestDelegate` 來啟動：

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

產生與 **Start(RequestDelegate app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。

**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**

使用 `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 的執行個體來使用路由中介軟體：

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

使用下列瀏覽器要求與範例：

| 要求                                    | 回應                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello, Martin!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | 擲回例外狀況，字串為 "ooops!" |
| `http://localhost:5000/throw`              | 擲回例外狀況，字串為 "Un oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。 應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。

**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**

使用 URL 和 `IRouteBuilder` 的執行個體：

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

產生與 **Start(Action&lt;IRouteBuilder&gt; routeBuilder)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。

**StartWith(Action&lt;IApplicationBuilder&gt; app)**

提供委派以設定 `IApplicationBuilder`：

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

在瀏覽器中提出要求給 `http://localhost:5000`，以收到 "Hello World!" 回應 `WaitForShutdown` 會封鎖，直到發出中斷 (Ctrl-C/SIGINT 或 SIGTERM)。 應用程式會顯示 `Console.WriteLine` 訊息並等候按鍵動作以便結束。

**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**

提供 URL 和委派以設定 `IApplicationBuilder`：

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

產生與 **StartWith(Action&lt;IApplicationBuilder&gt; app)** 相同的結果，除了應用程式會在 `http://localhost:8080` 回應。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

**執行**

`Run` 方法會啟動 Web 應用程式，並且封鎖呼叫執行緒，直到主機關閉為止：

```csharp
host.Run();
```

**Start**

藉由呼叫 `Start` 方法，以非封鎖方式執行主機：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

如果 URL 的清單傳遞至 `Start` 方法，它會接聽指定的 URL：

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

::: moniker-end

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment 介面

[IHostingEnvironment 介面](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 Web 裝載環境相關資訊。 使用[建構函式插入](xref:fundamentals/dependency-injection)以取得 `IHostingEnvironment`，才能使用其屬性和擴充方法：

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

[以慣例為基礎的方法](xref:fundamentals/environments#environment-based-startup-class-and-methods)可用來根據環境在啟動時設定應用程式。 或者，將 `IHostingEnvironment` 插入至 `Startup` 建構函式，以便用於 `ConfigureServices`：

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> 除了 `IsDevelopment` 擴充方法，`IHostingEnvironment` 也提供 `IsStaging`、`IsProduction` 和 `IsEnvironment(string environmentName)` 方法。 如需詳細資訊，請參閱<xref:fundamentals/environments>。

`IHostingEnvironment` 服務也可直接插入至 `Configure` 方法，以便設定處理管線：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

建立自訂[中介軟體](xref:fundamentals/middleware/index#writing-middleware)時，`IHostingEnvironment` 可以插入至 `Invoke` 方法：

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime 介面

[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) 允許啟動後和關機活動。 在介面上的三個屬性是用來註冊定義啟動和關閉事件之 `Action` 方法的取消語彙基元。

| 取消語彙基元    | 觸發時機&#8230; |
| --------------------- | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | 已完全啟動主機。 |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | 主機正在完成正常關機程序。 應該處理所有要求。 關機封鎖，直到完成此事件。 |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | 主機正在執行正常關機程序。 可能仍在處理要求。 關機封鎖，直到完成此事件。 |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) 會要求應用程式終止。 當呼叫類別的 `Shutdown` 方法時，下列類別使用 `StopApplication` 來順利關閉應用程式：

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

## <a name="scope-validation"></a>範圍驗證

::: moniker range=">= aspnetcore-2.0"

如果應用程式的環境是「開發」，[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 會將 [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) 設定為 `true`。

::: moniker-end

當 `ValidateScopes` 設定為 `true` 時，預設服務提供者會執行檢查以確認：

* 範圍服務不是直接或間接由根服務提供者解析。
* 範圍服務不是直接或間接插入至單一服務。

根服務提供者會在呼叫 [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) 時建立。 當提供者啟動應用程式時，根服務提供者的存留期與應用程式/伺服器的存留期一致，並會在應用程式關閉時處置。

範圍服務會由建立這些服務的容器處置。 若是在根容器中建立範圍服務，因為當應用程式/伺服器關機時，服務只會由根容器處理，所以服務的存留期會提升為單一服務等級。 當呼叫 `BuildServiceProvider` 時，驗證服務範圍會攔截到這些情況。

若要一律驗證範圍，包括在實際執行環境中，請使用主機產生器上的 [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) 來設定 [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions)：

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a>針對 System.ArgumentException 進行疑難排解

**下列情況僅在 ASP.NET Core 2.0 應用程式未呼叫 `UseStartup` 或 `Configure` 時，適用於該應用程式。**

可以藉由將 `IStartup` 直接插入至相依性插入容器來建置主機，而不是呼叫 `UseStartup` 或 `Configure`：

```csharp
services.AddSingleton<IStartup, Startup>();
```

如果主機以此方式建置，可能會發生下列錯誤：

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

發生此錯誤的原因是，必須具有應用程式名稱 (目前組件的名稱) 才可掃描 `HostingStartupAttributes`。 如果應用程式以手動方式將 `IStartup` 插入至相依性插入容器，請將下列呼叫新增至 `WebHostBuilder`，並指定組件名稱：

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

或者，將虛設的 `Configure` 新增至 `WebHostBuilder`，以自動設定應用程式名稱：

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

如需詳細資訊，請參閱 [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) (公告：已移除 Microsoft.Extensions.PlatformAbstractions (註解)) 和 [StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
