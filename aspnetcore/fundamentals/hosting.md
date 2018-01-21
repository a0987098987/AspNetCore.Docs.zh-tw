---
title: "在 ASP.NET Core 中裝載"
author: guardrex
description: "深入了解 ASP.NET Core，負責啟動與存留期管理的應用程式中的 web 主機。"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 7f6712073002b73ca4ddd7586718c81e62cacbc2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="hosting-in-aspnet-core"></a>在 ASP.NET Core 中裝載

作者：[Luke Latham](https://github.com/guardrex)

ASP.NET Core 應用程式設定和啟動*主機*。 負責應用程式啟動和生命週期管理的主機。 至少一部伺服器和要求處理管線，會設定主應用程式。

## <a name="setting-up-a-host"></a>設定主機

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

建立主應用程式使用的執行個體[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)。 這通常執行中應用程式的進入點，`Main`方法。 在專案範本中，`Main`位於*Program.cs*。 一般*Program.cs*呼叫[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)啟動主機的設定：

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

`CreateDefaultBuilder`會執行下列工作：

* 設定[Kestrel](servers/kestrel.md)做為 web 伺服器。 Kestrel 預設選項，請參閱[Kestrel 選項 > 一節中 ASP.NET Core Kestrel web 伺服器實作的](xref:fundamentals/servers/kestrel#kestrel-options)。
* 設定所傳回的路徑內容的根[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)。
* 從負載選擇性設定：
  * *appsettings.json*.
  * *appsettings.{Environment}.json*.
  * [使用者密碼](xref:security/app-secrets)的應用程式執行時`Development`環境。
  * 環境變數。
  * 命令列引數。
* 設定[記錄](xref:fundamentals/logging/index)主控台和偵錯輸出。 記錄將包含[記錄檔篩選](xref:fundamentals/logging/index#log-filtering)記錄組態區段中指定的規則*appsettings.json*或*appsettings。 {環境}.json*檔案。
* 當 IIS 背景執行，可讓[IIS integration](xref:host-and-deploy/iis/index)。 設定基底路徑和伺服器使用時聆聽的連接埠[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)。 此模組會建立 IIS 與 Kestrel 之間的反向 proxy。 也會設定應用程式[擷取啟動錯誤](#capture-startup-errors)。 對於 IIS 的預設選項，請參閱[IIS 選項 > 一節的主控件與 IIS 的 Windows 上的 ASP.NET Core](xref:host-and-deploy/iis/index#iis-options)。

*內容的根*決定主機會為內容檔案，例如 MVC 檢視的搜尋。 應用程式啟動時從專案的根資料夾，會使用專案的根資料夾，做為內容的根。 這是預設值用於[Visual Studio](https://www.visualstudio.com/)和[dotnet 新範本](/dotnet/core/tools/dotnet-new)。

如需有關應用程式組態的詳細資訊，請參閱[組態中 ASP.NET Core](xref:fundamentals/configuration/index)。

> [!NOTE]
> 做為使用靜態替代`CreateDefaultBuilder`方法，建立從主機[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)是支援的方法與 ASP.NET Core 2.x。 如需詳細資訊，請參閱 ASP.NET Core 1.x 索引標籤。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

建立主應用程式使用的執行個體[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)。 建立主控件應用程式的進入點，通常執行`Main`方法。 在專案範本中，`Main`位於*Program.cs*:

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

`WebHostBuilder`需要[伺服器實作 IServer](servers/index.md)。 內建的伺服器是[Kestrel](servers/kestrel.md)和[HTTP.sys](servers/httpsys.md) (ASP.NET Core 2.0 版本中之前, 已呼叫 HTTP.sys [WebListener](xref:fundamentals/servers/weblistener))。 在此範例中， [UseKestrel 擴充方法](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)指定 Kestrel 伺服器。

*內容的根*決定主機會為內容檔案，例如 MVC 檢視的搜尋。 取得預設內容根`UseContentRoot`由[Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1)。 應用程式啟動時從專案的根資料夾，會使用專案的根資料夾，做為內容的根。 這是預設值用於[Visual Studio](https://www.visualstudio.com/)和[dotnet 新範本](/dotnet/core/tools/dotnet-new)。

若要使用 IIS 做為反向 proxy，呼叫[UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions)建置主應用程式的一部分。 `UseIISIntegration`未設定*伺服器*、 like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1)沒有。 `UseIISIntegration`設定基底路徑和伺服器使用時聆聽的連接埠[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)建立 Kestrel 與 IIS 之間的反向 proxy。 若要使用 IIS 與 ASP.NET Core`UseKestrel`和`UseIISIntegration`必須指定。 `UseIISIntegration`只會啟動執行 IIS 或 IIS Express 後面時。 如需詳細資訊，請參閱[簡介 ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)和[ASP.NET 核心模組的組態參考](xref:host-and-deploy/aspnet-core-module)。

最簡單的實作，以設定主應用程式 （和 ASP.NET Core 應用程式） 包含指定伺服器和應用程式的要求管線的組態：

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

---

當設定主機，[設定](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1)和[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1)可以提供的方法。 如果`Startup`類別已指定，則必須定義`Configure`方法。 如需詳細資訊，請參閱[中 ASP.NET Core 應用程式啟動](startup.md)。 多個呼叫`ConfigureServices`附加至另一個。 多個呼叫`Configure`或`UseStartup`上`WebHostBuilder`取代先前的設定。

## <a name="host-configuration-values"></a>主機組態值

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)依賴下列方法來設定主機的組態值：

* 主控件產生器設定，其中包含環境變數與格式`ASPNETCORE_{configurationKey}`。 例如，`ASPNETCORE_URLS`。
* 明確的方法，例如`CaptureStartupErrors`。
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting)和相關聯的金鑰。 設定的值時`UseSetting`，此值設定為字串，不論類型為何。

主機會使用任何選項設定值上, 一次。 如需詳細資訊，請參閱[正在覆寫組態](#overriding-configuration)下一節。

### <a name="capture-startup-errors"></a>擷取啟動錯誤

此設定會控制擷取的啟動錯誤。

**索引鍵**: captureStartupErrors  
**型別**: *bool* (`true`或`1`)  
**預設**： 預設為`false`Kestrel 背後 IIS，其中預設值是以執行應用程式除非`true`。  
**使用設定**:`CaptureStartupErrors`  
**環境變數**:`ASPNETCORE_CAPTURESTARTUPERRORS`

當`false`，啟動導致主機結束期間發生的錯誤。 當`true`，主應用程式在啟動期間擷取例外狀況，並嘗試啟動伺服器。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a>內容的根

此設定可決定 ASP.NET Core 開始搜尋的內容檔案，例如 MVC 檢視的位置。 

**索引鍵**: contentRoot  
**型別**:*字串*  
**預設**： 預設為應用程式組件所在的資料夾。  
**使用設定**:`UseContentRoot`  
**環境變數**:`ASPNETCORE_CONTENTROOT`

內容的根也作為基底路徑[Web 根目錄下設定](#web-root)。 如果路徑不存在，就無法啟動主機。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a>詳細的錯誤

判斷詳細的錯誤應該擷取。

**索引鍵**: detailedErrors  
**型別**: *bool* (`true`或`1`)  
**預設**: false  
**使用設定**:`UseSetting`  
**環境變數**:`ASPNETCORE_DETAILEDERRORS`

當啟用 (或當<a href="#environment">環境</a>設`Development`)，應用程式會擷取詳細例外狀況。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a>環境

設定應用程式的環境。

**索引鍵**： 環境  
**型別**:*字串*  
**預設**： 生產環境  
**使用設定**:`UseEnvironment`  
**環境變數**:`ASPNETCORE_ENVIRONMENT`

環境可以設定為任何值。 架構定義的值包括`Development`， `Staging`，和`Production`。 值不區分大小寫。 根據預設，*環境*讀取從`ASPNETCORE_ENVIRONMENT`環境變數。 當使用[Visual Studio](https://www.visualstudio.com/)，可能會設定環境變數*launchSettings.json*檔案。 如需詳細資訊，請參閱[使用多個環境](xref:fundamentals/environments)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a>裝載啟動的組件

設定應用程式的裝載啟動組件。

**索引鍵**: hostingStartupAssemblies  
**型別**:*字串*  
**預設**： 空字串  
**使用設定**:`UseSetting`  
**環境變數**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

裝載啟動在啟動時載入的組件的字串，以分號分隔。 這項功能的新 ASP.NET Core 2.0。

雖然組態值會預設為空字串，則裝載的啟動組件永遠會包含應用程式的組件。 當未提供裝載啟動組件時，它們被加入至應用程式的組件時應用程式建置其通用服務在啟動時載入。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

這項功能已無法在 ASP.NET Core 1.x。

---

### <a name="prefer-hosting-urls"></a>偏好裝載 Url

表示主機是否應接聽設定的 Url`WebHostBuilder`而不是與`IServer`實作。

**索引鍵**: preferHostingUrls  
**型別**: *bool* (`true`或`1`)  
**預設**: true  
**使用設定**:`PreferHostingUrls`  
**環境變數**:`ASPNETCORE_PREFERHOSTINGURLS`

這項功能的新 ASP.NET Core 2.0。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

這項功能已無法在 ASP.NET Core 1.x。

---

### <a name="prevent-hosting-startup"></a>防止裝載啟動

可防止自動載入裝載啟動的組件，包括裝載應用程式的組件所設定的啟動組件。 請參閱[使用 IHostingStartup 外部組件加入應用程式功能](xref:host-and-deploy/ihostingstartup)如需詳細資訊。

**索引鍵**: preventHostingStartup  
**型別**: *bool* (`true`或`1`)  
**預設**: false  
**使用設定**:`UseSetting`  
**環境變數**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`

這項功能的新 ASP.NET Core 2.0。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

這項功能已無法在 ASP.NET Core 1.x。

---

### <a name="server-urls"></a>伺服器 Url

表示 IP 位址或連接埠和通訊協定，伺服器應該接聽之要求的主機位址。

**索引鍵**: url  
**型別**:*字串*  
**預設**: http://localhost:5000/  
**使用設定**:`UseUrls`  
**環境變數**:`ASPNETCORE_URLS`

設定為以分號分隔 （;） 應該回應伺服器的前置詞的 URL 清單。 例如，`http://localhost:123`。 使用 「\*"，表示伺服器應接聽任何 IP 位址或主機名稱使用指定的連接埠和通訊協定上的要求 (例如， `http://*:5000`)。 通訊協定 (`http://`或`https://`) 必須包含以每個 URL。 伺服器之間的不支援的格式。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

Kestrel 有它自己的端點設定應用程式開發介面。 如需詳細資訊，請參閱[在 ASP.NET Core 中實作 Kestrel 網頁伺服器](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a>關閉逾時

指定 web 主機關閉的時間量。

**索引鍵**: shutdownTimeoutSeconds  
**型別**: *int*  
**預設**: 5  
**使用設定**:`UseShutdownTimeout`  
**環境變數**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

雖然索引鍵接受*int*與`UseSetting`(例如， `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`)、`UseShutdownTimeout`擴充方法會採用`TimeSpan`。 這項功能的新 ASP.NET Core 2.0。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

這項功能已無法在 ASP.NET Core 1.x。

---

### <a name="startup-assembly"></a>啟動組件

決定要搜尋的組件`Startup`類別。

**索引鍵**: startupAssembly  
**型別**:*字串*  
**預設**： 應用程式的組件  
**使用設定**:`UseStartup`  
**環境變數**:`ASPNETCORE_STARTUPASSEMBLY`

依名稱的組件 (`string`) 或型別 (`TStartup`) 可以參考。 若為多個`UseStartup`呼叫的方法，最後一個的優先順序。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a>Web 根目錄

設定應用程式的靜態資產的相對路徑。

**索引鍵**: webroot  
**型別**:*字串*  
**預設**： 如果未指定，預設值是"(Content Root)/wwwroot"，則該路徑存在。 如果路徑不存在，則會使用任何作業檔案提供者。  
**使用設定**:`UseWebRoot`  
**環境變數**:`ASPNETCORE_WEBROOT`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a>覆寫設定

使用[組態](xref:fundamentals/configuration/index)設定主控件。 在下列範例中，主機設定 （選擇性） 指定於*hosting.json*檔案。 從載入任何組態*hosting.json*命令列引數可能會覆寫檔案。 內建的設定 (在`config`) 用來設定與主機`UseConfiguration`。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

覆寫所提供的組態`UseUrls`與*hosting.json*設定第一個命令列引數設定第二個：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

*hosting.json*:

```json
{
    urls: "http://*:5005"
}
```

覆寫所提供的組態`UseUrls`與*hosting.json*設定第一個命令列引數設定第二個：

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

---

> [!NOTE]
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)擴充方法不是目前可以剖析所傳回的組態區段`GetSection`(例如， `.UseConfiguration(Configuration.GetSection("section"))`。 `GetSection`方法篩選到要求的區段之組態機碼，但會保留的區段名稱索引鍵 (例如， `section:urls`， `section:environment`)。 `UseConfiguration`方法預期要比對的索引鍵`WebHostBuilder`索引鍵 (例如， `urls`， `environment`)。 索引鍵的區段名稱存在主控件設定防止區段的值。 這個問題將在近期的版本中解決。 如需詳細資訊和因應措施，請參閱[傳遞到 WebHostBuilder.UseConfiguration 組態區段會使用完整金鑰](https://github.com/aspnet/Hosting/issues/839)。

若要指定特定 URL 上所執行的主機，所要的值中可傳遞的命令提示字元執行時`dotnet run`。 命令列引數會覆寫`urls`值*hosting.json*檔和伺服器會接聽連接埠 8080:

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a>正在啟動主機

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**執行**

`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機已關閉：

```csharp
host.Run();
```

**Start**

主機執行的非封鎖方式，藉由呼叫其`Start`方法：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

如果 Url 的清單傳遞至`Start`方法，它會接聽指定的 Url:

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

應用程式可以初始化和啟動新的主機使用的預先設定的預設值`CreateDefaultBuilder`使用靜態便利的方法。 啟動伺服器未主控台輸出的情況下，這些方法[WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown)等候中斷 （Ctrl-C/SIGINT 或 SIGTERM）：

**開始 （RequestDelegate 應用程式）**

開頭`RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

若要在瀏覽器中提出要求`http://localhost:5000`接收"Hello World ！"的回應 `WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。 應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。

**開始 （以字串 url、 RequestDelegate 應用程式）**

URL 的開頭和`RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

會產生相同結果**開始 （RequestDelegate 應用程式）**，除了應用程式回應`http://localhost:8080`。

**Start(Action<IRouteBuilder> routeBuilder)**

使用的執行個體`IRouteBuilder`([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) 使用路由的中介軟體：

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

使用下列瀏覽器要求範例：

| 要求                                    | 回應                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Hello，Martin ！                           |
| `http://localhost:5000/buenosdias/Catrina` | Catrina 布宜諾 dias ！                    |
| `http://localhost:5000/throw/ooops!`       | 擲回例外狀況的字串"ooops ！ 」 |
| `http://localhost:5000/throw`              | 擲回例外狀況的字串"Uh 喔 ！ 」 |
| `http://localhost:5000/Sante/Kevin`        | Sante，Kevin ！                            |
| `http://localhost:5000`                    | Hello World!                             |

`WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。 應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。

**啟動 (string url，動作<IRouteBuilder>routeBuilder)**

使用 URL 和執行個體`IRouteBuilder`:

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

會產生相同結果**開始 (動作<IRouteBuilder>routeBuilder)**，除了應用程式會回應在`http://localhost:8080`。

**StartWith(Action<IApplicationBuilder> app)**

提供設定委派`IApplicationBuilder`:

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

若要在瀏覽器中提出要求`http://localhost:5000`接收"Hello World ！"的回應 `WaitForShutdown`直到發出中斷 （Ctrl-C/SIGINT 或 SIGTERM） 的區塊。 應用程式會顯示`Console.WriteLine`訊息並等候 keypress 結束。

**StartWith (string url，動作<IApplicationBuilder>應用程式)**

提供 URL，若要設定委派`IApplicationBuilder`:

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

會產生相同結果**StartWith (動作<IApplicationBuilder>應用程式)**，除了應用程式回應`http://localhost:8080`。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**執行**

`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機關機：

```csharp
host.Run();
```

**Start**

主機執行的非封鎖方式，藉由呼叫其`Start`方法：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

如果 Url 的清單傳遞至`Start`方法，它會接聽指定的 Url:


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

---

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment 介面

[IHostingEnvironment 介面](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment)提供應用程式的 web 主機環境的相關資訊。 使用[建構函式插入](xref:fundamentals/dependency-injection)取得`IHostingEnvironment`才能使用其屬性和擴充方法：

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

A[慣例為基礎的方法](xref:fundamentals/environments#startup-conventions)可用來根據環境的啟動時設定應用程式。 或者，插入`IHostingEnvironment`到`Startup`建構函式以用於`ConfigureServices`:

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
> 除了`IsDevelopment`擴充方法，`IHostingEnvironment`提供`IsStaging`， `IsProduction`，和`IsEnvironment(string environmentName)`方法。 請參閱[使用多個環境](xref:fundamentals/environments)如需詳細資訊。

`IHostingEnvironment`服務也可直接插入`Configure`設定處理管線的方法：

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

`IHostingEnvironment`可以插入到`Invoke`方法時建立自訂[中介軟體](xref:fundamentals/middleware#writing-middleware):

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

[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime)允許後啟動和關閉的活動。 在介面上的三個屬性是用來註冊的取消語彙基元`Action`定義啟動和關閉事件的方法。 另外還有`StopApplication`方法。

| 取消語彙基元    | 觸發時 &#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | 已完全啟動主機。 |
| `ApplicationStopping` | 主機正在執行正常關機程序。 可能仍在處理要求。 關機封鎖，直到完成此事件。 |
| `ApplicationStopped`  | 主應用程式即將完成正常關機程序。 應該處理所有要求。 關機封鎖，直到完成此事件。 |

| 方法            | 動作                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | 目前的應用程式要求終止。 |

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

## <a name="troubleshooting-systemargumentexception"></a>疑難排解 System.ArgumentException

**ASP.NET Core 2.0 只適用於**

主機可能會建立將，以便`IStartup`直接將相依性插入容器，而不是呼叫`UseStartup`或`Configure`:

```csharp
services.AddSingleton<IStartup, Startup>();
```

如果主應用程式建立的如此一來，可能會發生下列錯誤：

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

這是因為[applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) （目前的組件），才能掃描`HostingStartupAttributes`。 如果應用程式以手動方式插入`IStartup`到相依性插入容器中，加入下列呼叫`WebHostBuilder`具有所指定的組件名稱：

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

或者，新增假`Configure`至`WebHostBuilder`，將`applicationName`(`ApplicationKey`) 自動：

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

**請注意**： 時，這只需要 ASP.NET Core 2.0 版，而且只應用程式不會呼叫`UseStartup`或`Configure`。

如需詳細資訊，請參閱[公告： Microsoft.Extensions.PlatformAbstractions 已移除 （註解）](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938)和[StartupInjection 範例](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs)。

## <a name="additional-resources"></a>其他資源

* [ Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)
* [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)
* [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)
* [在 Windows 服務的主機](xref:host-and-deploy/windows-service)
