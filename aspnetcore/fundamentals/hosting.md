---
title: "裝載於 ASP.NET Core |Microsoft 文件"
author: ardalis
description: "在 ASP.NET Core web 主機簡介。"
keywords: "ASP.NET Core web 主機，IWebHost"
ms.author: riande
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4e45311d-8d56-46e2-b99d-6f65b648a277
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0725f3d2c130068094792e5ba9e17481155e4294
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="introduction-to-hosting-in-aspnet-core"></a>在 ASP.NET Core 中裝載的簡介

由[Steve Smith](http://ardalis.com)

若要執行 ASP.NET Core 應用程式，您必須設定並啟動使用主機`WebHostBuilder`。

## <a name="what-is-a-host"></a>什麼是主應用程式？

ASP.NET Core 應用程式需要*主機*要在其中執行。 主機必須實作`IWebHost`介面，以便公開的功能和服務的集合，而`Start`方法。 主機通常會建立使用的執行個體`WebHostBuilder`，會建立並傳回`WebHost`執行個體。 `WebHost`參照的伺服器將會處理要求。 深入了解[伺服器](servers/index.md)。

### <a name="what-is-the-difference-between-a-host-and-a-server"></a>在主機與伺服器之間的差異為何？

負責應用程式啟動和生命週期管理的主機。 伺服器是負責接受 HTTP 要求。 主機負責的部分包括確保應用程式的服務和伺服器可供使用且已正確設定。 您可以將主機視為伺服器的包裝函式。 主機已設定為使用特定的伺服器。伺服器不會知道其主機。

## <a name="setting-up-a-host"></a>設定主機

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

建立使用的執行個體的主機`WebHostBuilder`。 這通常是在您的應用程式進入點： `public static void Main` (在專案範本位於*Program.cs*檔案)。 一般*Program.cs*，所示，將示範如何使用`WebHostBuilder`建置主應用程式。

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=14,15,16,17,18,19,20,21)]

`WebHostBuilder`負責建立主機將啟動載入應用程式伺服器。 `WebHostBuilder`需要您提供伺服器實作`IServer`(`UseKestrel`上述程式碼中)。 `UseKestrel`指定應用程式會使用 Kestrel 伺服器。

伺服器的*內容的根*決定搜尋內容的檔案，就像 MVC 檢視檔案。 預設內容的根是從中執行應用程式的資料夾。

> [!NOTE]
> 指定`Directory.GetCurrentDirectory`為內容的根會用於 web 專案的根資料夾為應用程式的內容的根應用程式啟動時從這個資料夾 (例如，呼叫`dotnet run`web 專案資料夾中)。 這是使用 Visual Studio 中的預設值和`dotnet new`範本。

若要使用 IIS 做為反向 proxy，呼叫`UseIISIntegration`建置主應用程式的一部分。 

請注意，`UseIISIntegration`不設定*伺服器*、 like`UseKestrel`沒有。 若要使用 IIS 與 ASP.NET Core，您必須同時指定`UseKestrel`和`UseIISIntegration`。 `UseKestrel`建立網頁伺服器及裝載應用程式。 `UseIISIntegration`檢查 IIS/iis Express 所使用的環境變數，並設定要接聽的連接埠等要使用的標頭的設定。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

建立使用的執行個體的主機`WebHostBuilder`。 這通常是在您的應用程式進入點： `public static void Main` (在專案範本位於*Program.cs*檔案)。 一般*Program.cs*，如下所示，呼叫`CreateDefaultbuilder`建置主應用程式：

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

`CreateDefaultbuilder`建立的執行個體`WebHostBuilder`建置 bootstraps 應用程式伺服器的主機。 主應用程式需要[伺服器實作 IServer](servers/index.md)。 內建的伺服器是[Kestrel](servers/kestrel.md)和[HTTP.sys](servers/httpsys.md);`CreateDefaultbuilder`預設都會使用 Kestrel。

`CreateDefaultbuilder`會執行除了與網頁伺服器設定 Kestrel 安裝工作：

* 若要設定內容的根`Directory.GetCurrentDirectory`。
* 載入組態：
  * *appsettings.json*
  * *appsettings。\<EnvironmentName >.json*。
  * 在開發環境中執行的應用程式時的使用者密碼
  * 環境變數
  * 提供的命令列引數
* 使用篩選的記錄組態區段中指定的規則來設定主控台和偵錯輸出的記錄。
* 可讓 IIS 整合。
* 在開發環境中執行的應用程式時，請新增 [開發人員的例外狀況] 頁面。

伺服器的*內容的根*決定搜尋內容的檔案，就像 MVC 檢視檔案。 預設內容的根是從中執行應用程式的資料夾。

> [!NOTE]
> 指定`Directory.GetCurrentDirectory`為內容的根會用於 web 專案的根資料夾為應用程式的內容的根應用程式啟動時從這個資料夾 (例如，呼叫`dotnet run`web 專案資料夾中)。 這是使用 Visual Studio 中的預設值和`dotnet new`範本。

當您使用 IIS 的反向 proxy 時，ASP.NET Core 自動呼叫`UseIISIntegration`建置主應用程式的一部分。 如需詳細資訊，請參閱[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)。

請注意，`UseIISIntegration`不設定*伺服器*、 like`UseKestrel`沒有。 `UseKestrel`建立網頁伺服器及裝載應用程式。 `UseIISIntegration`檢查 IIS/iis Express 所使用的環境變數，並設定要接聽的連接埠等要使用的標頭的設定。

---

設定主機 （和 ASP.NET Core 應用程式） 的最簡單的實作會包含只伺服器和應用程式的要求管線的組態：

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
    })
    .Build();

host.Run();
```

> [!NOTE]
> 當設定主機，您可以提供`Configure`和`ConfigureServices`方法，而不是，或除了指定`Startup`類別 (其中也必須定義這些方法中-請參閱[應用程式啟動](startup.md))。 多個呼叫`ConfigureServices`會將附加至另一個，則為呼叫`Configure`或`UseStartup`將會取代先前的設定。

## <a name="configuring-a-host"></a>主控件設定

`WebHostBuilder`提供方法來設定大部分可用的設定值，這也可以設定直接使用主機`UseSetting`和相關聯的金鑰。

### <a name="host-configuration-values"></a>主機組態值

**擷取啟動錯誤**`bool`

索引鍵： `captureStartupErrors`。 預設值為 `false`。 當`false`，啟動導致主機結束期間發生的錯誤。 當`true`，主機會擷取任何例外狀況，從`Startup`類別，並嘗試啟動伺服器。 它會顯示錯誤頁面 （泛型，或詳細，根據詳細的錯誤設定下, 面） 為每個要求。 使用設定`CaptureStartupErrors`方法。

注意： 您的應用程式執行時的 Kestrel 和 IIS，預設行為是以擷取啟動錯誤。 

```csharp
new WebHostBuilder()
    .CaptureStartupErrors(true)
   ```

**內容根**`string`

索引鍵： `contentRoot`。 預設應用程式組件 （適用於 Kestrel; 所在的資料夾IIS 預設會使用 web 專案根目錄）。 此設定可決定 ASP.NET Core 會開始搜尋的內容檔案，例如 MVC 檢視。 也做為基底路徑[Web 根目錄設定](#web-root-setting)。 使用設定`UseContentRoot`方法。 路徑必須存在，或主機將無法啟動。

```csharp
new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
   ```

**詳細錯誤**`bool`

索引鍵： `detailedErrors`。 預設值為 `false`。 當`true`（或當環境設定為 「 開發 」），應用程式會顯示啟動例外狀況，而不是一般錯誤網頁的詳細資料。 使用設定`UseSetting`。

```csharp
new WebHostBuilder()
    .UseSetting("detailedErrors", "true")
```

當設定詳細的錯誤為`false`擷取啟動錯誤，且`true`，以伺服器的每個要求的回應會顯示一般錯誤頁面。

![一般錯誤頁面](hosting/_static/generic-error-page.png)

當設定詳細的錯誤為`true`擷取啟動錯誤，且`true`，以伺服器的每個要求的回應會顯示詳細的錯誤頁面。

![詳細的錯誤頁面](hosting/_static/detailed-error-page.png)

**環境**`string`

索引鍵： `environment`。 預設為 「 生產 」。 可能會設定為任何值。 架構定義的值包括 「 開發 」、 「 預備 」 及 「 生產 」。 值不區分大小寫。 請參閱[使用多個環境](environments.md)。 使用設定`UseEnvironment`方法。

```csharp
new WebHostBuilder()
    .UseEnvironment("Development")
```

> [!NOTE]
> 根據預設，環境會從讀取`ASPNETCORE_ENVIRONMENT`環境變數。 當使用 Visual Studio，可能會設定環境變數*launchSettings.json*檔案。

<a id="server-urls"></a>

**伺服器 Url**`string`

索引鍵： `urls`。 設定為分號 （;） 分隔清單的 URL 前置詞應該回應伺服器。 例如，`http://localhost:123`。 網域/主機名稱可以取代"\*"，表示伺服器應接聽任何 IP 位址的要求，或裝載使用指定的連接埠和通訊協定 (例如，`http://*:5000`或`https://*:5001`)。 通訊協定 (`http://`或`https://`) 必須包含以每個 URL。 前置詞解譯所設定的伺服器。支援的格式而異的伺服器之間。

```csharp
new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

在 ASP.NET Core 2.0 中，Kestrel 有它自己的端點設定應用程式開發介面，而且不支援`https://`中`urls`字串。 如需詳細資訊，請參閱[簡介 Kestrel](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration)。

**啟動組件**`string`

索引鍵： `startupAssembly`。 決定要搜尋的組件`Startup`類別。 使用設定`UseStartup`方法。 可能會改為參照特定的型別使用`WebHostBuilder.UseStartup<StartupType>`。 若為多個`UseStartup`呼叫的方法，最後一個的優先順序。

```csharp
new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

<a name=web-root-setting></a>

**Web 根目錄**`string`

索引鍵： `webroot`。 如果未指定預設值是`(Content Root Path)\wwwroot`，如果它存在。 如果此路徑不存在，則會使用任何作業檔案提供者。 使用設定`UseWebRoot`。

```csharp
new WebHostBuilder()
    .UseWebRoot("public")
```

### <a name="overriding-configuration"></a>覆寫設定

使用[組態](configuration.md)設定主應用程式所使用的組態值。 這些值可能會隨後覆寫。 這指定使用`UseConfiguration`。

```csharp
public static void Main(string[] args)
{
    var config = new ConfigurationBuilder()
        .AddJsonFile("hosting.json", optional: true)
        .AddCommandLine(args)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .Configure(app =>
        {
            app.Run(async (context) => await context.Response.WriteAsync("Hi!"));
        })
        .Build();

    host.Run();
}
```

在上述範例中，命令列引數可能會傳入設定主控件，或設定選擇性地指定在*hosting.json*檔案。 若要指定特定 URL 上所執行的主機，您可以傳入所要的值從命令提示字元：

```console
dotnet run --urls "http://*:5000"
```

`Run`方法會啟動 web 應用程式，並且封鎖呼叫執行緒，直到主機已關閉。

```csharp
host.Run();
```

您可以藉由呼叫非封鎖方式執行主機其`Start`方法：

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

將 Url 的清單`Start`方法，它會接聽指定的 Url:

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

以下是有效的 URL 格式取決於您要使用的伺服器。 如需詳細資訊，請參閱[伺服器 Url](#server-urls)稍早在本文章。

> [!NOTE]
> `UseConfiguration`擴充方法不是目前可以剖析所傳回的組態區段`GetSection`(例如， `.UseConfiguration(Configuration.GetSection("section"))`。 `GetSection`方法篩選到要求的區段之組態機碼，但會保留的區段名稱索引鍵 (例如， `section:urls`， `section:environment`)。 `UseConfiguration`方法預期要比對的索引鍵`WebHostBuilder`索引鍵 (例如， `urls`， `environment`)。 索引鍵的區段名稱存在主控件設定防止區段的值。 這個問題將在近期的版本中解決。 如需詳細資訊和因應措施，請參閱[傳遞到 WebHostBuilder.UseConfiguration 組態區段會使用完整金鑰](https://github.com/aspnet/Hosting/issues/839)。

### <a name="ordering-importance"></a>排序重要性

`WebHostBuilder`如果設定第一次從某些環境變數，讀取設定。 這些環境變數都必須使用格式`ASPNETCORE_{configurationKey}`，所以若要設定 Url 的範例則伺服器會接聽預設，會設定`ASPNETCORE_URLS`。

您可以指定覆寫這些環境變數值的任何組態 (使用`UseConfiguration`) 或明確地將此值設定 (使用`UseUrls`執行個體)。 主機會使用任何選項設定的值上一次。 基於這個理由，`UseIISIntegration`必須出現在之後`UseUrls`，因為它會取代 URL 動態 IIS 所提供的其中一個。 如果您想要以程式設計方式設定一個值，預設 URL，但允許它會覆寫的組態，您可以設定主應用程式，如下所示：

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseUrls("http://*:1000") // default URL
    .UseConfiguration(config) // override from command line
    .UseKestrel()
    .Build();
```

## <a name="additional-resources"></a>其他資源

* [發行到使用 IIS 的 Windows](../publishing/iis.md)
* [發行至使用 Nginx Linux](../publishing/linuxproduction.md)
* [若要使用 Apache 的 Linux 發行](../publishing/apache-proxy.md)
* [在 Windows 服務的主機](xref:hosting/windows-service)

