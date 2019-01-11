---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: guardrex
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: af1f330f2afa340ef98a6b4bd5008859f4b0f914
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637907"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 Kestrel 網頁伺服器實作

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)

::: moniker range="<= aspnetcore-1.1"

如需本主題的 1.1 版，請下載 [ASP.NET Core (1.1 版，PDF) 中的 Kestrel 網頁伺服器實作](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Kestrel_1.1.pdf)。

::: moniker-end

Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。 Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。

Kestrel 支援下列案例：

::: moniker range=">= aspnetcore-2.2"

* HTTPS
* 用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級
* Nginx 背後的高效能 Unix 通訊端
* HTTP/2 (macOS 上除外&dagger;)

&dagger;未來版本的 macOS 上將會支援 HTTP/2。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* HTTPS
* 用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級
* Nginx 背後的高效能 Unix 通訊端

::: moniker-end

.NET Core 支援的所有平台和版本都支援 Kestrel。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a>HTTP/2 支援

如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式使用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：

* 作業系統&dagger;
  * Windows Server 2016/Windows 10 或更新版本&Dagger;
  * Linux 含 OpenSSL 1.0.2 或更新版本 (例如 Ubuntu 16.04 或更新版本)
* 目標 Framework：.NET Core 2.2 或更新版本
* [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線
* TLS 1.2 或更新版本連線

&dagger;未來版本的 macOS 上將會支援 HTTP/2。
&Dagger;Kestrel 在 Windows Server 2012 R2 與 Windows 8.1 對 HTTP/2 的支援有限。 支援有限的原因是這些作業系統上的支援 TLS 密碼編譯套件清單有限。 可能需要使用橢圓曲線數位簽章演算法 (ECDSA) 產生的憑證來保護 TLS 連線。

如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。

預設會停用 HTTP/2。 如需組態的詳細資訊，請參閱 [Kestrel 選項](#kestrel-options)和 [ListenOptions. 通訊協定](#listenoptionsprotocols)一節。

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>何時搭配使用 Kestrel 與反向 Proxy

您可以單獨使用 Kestrel，或與 [Internet Information Services (IIS)](https://www.iis.net/)、[Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/) 等「反向 Proxy 伺服器」搭配使用。 反向 Proxy 伺服器會從網路接收 HTTP 要求，然後轉送到 Kestrel。

Kestrel 用作邊緣 (網際網路對應) 網頁伺服器：

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

Kestrel 用於反向 Proxy 組態中：

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

不論組態是否具有反向 Proxy 伺服器，對於從網際網路接收要求的 ASP.NET Core 2.1 或更新版本的應用程式來說，都是支援的裝載組態。

Kestrel 用作不需要反向 Proxy 伺服器的 Edge Server 時，不支援在多個處理序之間共用相同的 IP 和連接埠。 當 Kestrel 設定為接聽連接埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的 `Host` 標頭為何。 可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。

即使不需要反向 Proxy 伺服器，使用反向 Proxy 伺服器也是不錯的選擇。

反向 Proxy：

* 可以限制它所主控之應用程式的公開介面區。
* 提供額外的組態和防禦層。
* 能夠與現有基礎結構更好地整合。
* 簡化負載平衡和安全通訊 (HTTPS) 組態。 只有反向 Proxy 伺服器需要 X.509 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器通訊。

> [!WARNING]
> 裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>如何在 ASP.NET Core 應用程式中使用 Kestrel

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 套件包含在 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中 (ASP.NET Core 2.1 或更新版本)。

ASP.NET Core 專案範本預設會使用 Kestrel。 在 *Program.cs* 中，範本程式碼會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)，而後者會呼叫場景背後的 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel)。

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker range=">= aspnetcore-2.2"

若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請使用 `ConfigureKestrel`：

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

若要在呼叫 `CreateDefaultBuilder` 之後提供額外的設定，請呼叫 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel)：

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

## <a name="kestrel-options"></a>Kestrel 選項

Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。

請在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 類別的 [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) 屬性中設定條件約束。 `Limits` 屬性會保留 [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) 類別的執行個體。

### <a name="maximum-client-connections"></a>用戶端連線數目上限

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。 升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

連線數目上限預設為無限制 (null)。

### <a name="maximum-request-body-size"></a>要求主體大小上限

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。

要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下範例會示範如何設定應用程式、每個要求的條件約束：

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

您可以在中介軟體中覆寫特定要求的設定：

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。 有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。

### <a name="minimum-request-body-data-rate"></a>要求主體資料速率下限

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。 如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。 寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。

預設速率下限為 240 個位元組/秒，寬限期為 5 秒。

速率下限也適用於回應。 除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。

以下範例示範如何在 *Program.cs* 中設定資料速率下限：

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

::: moniker-end

您可以覆寫中介軟體中每個要求的最小速率限制：

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

::: moniker range=">= aspnetcore-2.2"

因為通訊協定對要求多工的支援，所以 HTTP/2 不支援以每一要求基礎修改速率限制，進而使先前範例中所參考的所有速率功能都不會出現在 HTTP/2 要求的 `HttpContext.Features` 中。 透過 `KestrelServerOptions.Limits` 設定的全伺服器速率限制皆仍套用至 HTTP/1.x 及 HTTP/2 連線。

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a>每個連線的資料流數目上限

`Http2.MaxStreamsPerConnection` 會限制每個 HTTP/2 連線的同時要求資料流數目。 超出的資料流會被拒絕。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

預設值為 100。

### <a name="header-table-size"></a>標頭表格大小

HPACK 解碼器可解壓縮 HTTP/2 連線的 HTTP 標頭。 `Http2.HeaderTableSize` 會限制 HPACK 解碼器所使用的標頭壓縮表格大小。 這個值是以八位元提供，而且必須大於零 (0)。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

預設值為 4096。

### <a name="maximum-frame-size"></a>框架大小上限

`Http2.MaxFrameSize` 表示要接收的 HTTP/2 連線框架承載大小上限。 這個值是以八位元提供，而且必須介於 2^14 (16,384) 到 2^24-1 (16,777,215) 之間。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

預設值為 2^14 (16,384)。

### <a name="maximum-request-header-size"></a>要求標頭大小上限

`Http2.MaxRequestHeaderFieldSize` 以八位元表示要求標頭值的允許大小上限。 此限制皆共同套用至已壓縮及未壓縮代表中的名稱與值。 此值必須大於零 (0)。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

預設值為 8,192。

### <a name="initial-connection-window-size"></a>初始連線視窗大小

`Http2.InitialConnectionWindowSize` 會以位元組表示伺服器緩衝每個連線之所有要求 (資料流) 單次彙總的要求內容資料上限。 要求也皆受 `Http2.InitialStreamWindowSize` 所限制。 此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

預設值為 128 KB (131,072)。

### <a name="initial-stream-window-size"></a>初始資料流視窗大小

`Http2.InitialStreamWindowSize` 會以位元組表示每個要求 (資料流) 單次伺服器緩衝的要求內容資料上限。 要求也皆受 `Http2.InitialStreamWindowSize` 所限制。 此值必須大於或等於 65,535，且小於 2^31 (2,147,483,648)。

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

預設值為 96 KB (98,304)。

::: moniker-end

如需其他 Kestrel 選項和限制的資訊，請參閱：

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

## <a name="endpoint-configuration"></a>端點組態

ASP.NET Core 預設會繫結至：

* `http://localhost:5000`
* `https://localhost:5001` (當有本機開發憑證存在時)

開發憑證會建立於：

* 已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。
* [dev-certs 工具](xref:aspnetcore-2.1#https)用來建立憑證。

有些瀏覽器需要您授與明確的權限給瀏覽器，才能信任本機開發憑證。

ASP.NET Core 2.1 和更新版本的專案範本會將應用程式設定為預設於 HTTPS 上執行，並包含 [HTTPS 重新導向和 HSTS 支援](xref:security/enforcing-ssl)。

在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 上呼叫 [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 或 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 方法，以設定 Kestrel 的 URL 前置詞和連接埠。

`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。

ASP.NET Core 2.1 `KestrelServerOptions` 組態：

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a>ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)

指定組態 `Action` 以針對每個指定端點執行。 呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a>ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)

指定組態 `Action` 以針對每個 HTTPS 端點執行。 呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。

### <a name="configureiconfiguration"></a>Configure(IConfiguration)

建立組態載入器，設定採用 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 作為輸入的 Kestrel。 組態的範圍必須限於 Kestrel 的組態區段。

### <a name="listenoptionsusehttps"></a>ListenOptions.UseHttps

設定 Kestrel 使用 HTTPS。

`ListenOptions.UseHttps` 延伸模組：

* `UseHttps` &ndash; 設定 Kestrel 以與 HTTPS 預設憑證搭配使用。 如果未設定預設憑證，會擲回例外狀況。
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

`ListenOptions.UseHttps` 參數：

* `filename` 是憑證檔案的路徑和檔案名稱，它相對於包含應用程式內容檔案的目錄。
* `password` 是存取 X.509 憑證資料所需的密碼。
* `configureOptions` 是設定 `HttpsConnectionAdapterOptions` 的 `Action`。 傳回 `ListenOptions`。
* `storeName` 是要從中載入憑證的憑證存放區。
* `subject` 是憑證的主體名稱。
* `allowInvalid` 表示是否應該考慮無效的憑證，例如自我簽署憑證。
* `location` 是要從中載入憑證的存放區位置。
* `serverCertificate` 是 X.509 憑證。

在生產環境中，必須明確設定 HTTPS。 至少必須提供預設憑證。

支援的組態描述如下：

* 無組態
* 從組態取代預設憑證
* 變更程式碼中的預設值

*無組態*

Kestrel 會接聽 `http://localhost:5000` 和 `https://localhost:5001` (如果預設憑證可用的話)。

使用以下各項指定 URL：

* `ASPNETCORE_URLS` 環境變數。
* `--urls` 命令列引數。
* `urls` 主機組態索引鍵。
* `UseUrls` 擴充方法。

如需詳細資訊，請參閱[伺服器 URL](xref:fundamentals/host/web-host#server-urls) 和[覆寫設定](xref:fundamentals/host/web-host#override-configuration)。

使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。 將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000; http://localhost:8001"`)。

*從組態取代預設憑證*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 組態。 Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。 設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。

在下列 *appsettings.json* 範例中：

* 將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。
* 任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證]  > [預設] 下定義的憑證或開發憑證。

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

除了針對任何憑證節點使用 [路徑] 和 [密碼]，還可以使用憑證存放區欄位指定憑證。 例如，[憑證] > [預設] 憑證可以指定為：

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

結構描述附註：

* 端點名稱不區分大小寫。 例如，`HTTPS` 和 `Https` 都有效。
* `Url` 參數對每個端點而言都是必要的。 此參數的格式等同於最上層 `Urls` 組態參數，但是它限制為單一值。
* 這些端點會取代最上層 `Urls` 組態中定義的端點，而不是新增至其中。 透過 `Listen` 在程式碼中定義的端點，會與組態區段中定義的端點累計。
* `Certificate` 區段是選擇性的。 如果未指定 `Certificate` 區段，則會使用先前案例中所定義的預設值。 如果沒有預設值可供使用，伺服器就會擲回例外狀況，且無法啟動。
* `Certificate` 區段同時支援 [路徑]&ndash; [密碼] 和 [主旨]&ndash; [存放區] 憑證。
* 可以用這種方式定義任何數目的端點，只要它們不會導致連接埠衝突即可。
* `options.Configure(context.Configuration.GetSection("Kestrel"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, options => { })` 方法，此方法可用來補充已設定的端點設定：

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  您也可以直接存取 `KestrelServerOptions.ConfigurationLoader` 來保持反覆運算現有的載入器，例如 [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 提供的載入器。

* 每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。
* 可以藉由使用另一個區段再次呼叫 `options.Configure(context.Configuration.GetSection("Kestrel"))` 而載入多個組態。 只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。 中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。
* `KestrelConfigurationLoader` 會將來自 `KestrelServerOptions` 的 API 的 `Listen` 系列鏡像為 `Endpoint` 多載，所以可在相同的位置設定程式碼和設定端點。 這些多載不使用名稱，並且只使用來自組態的預設組態。

*變更程式碼中的預設值*

`ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 可以用來變更 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的預設設定，包括覆寫先前案例中指定的預設憑證。 `ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 應該在設定任何端點之前呼叫。

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

*SNI 的 Kestrel 支援*

[伺服器名稱指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可以用於在相同的 IP 位址和連接埠上裝載多個網域。 SNI 若要運作，用戶端會在 TLS 信號交換期間傳送安全工作階段的主機名稱給伺服器，讓伺服器可以提供正確的憑證。 用戶端在 TLS 信號交換之後的安全工作階段期間，會使用所提供的憑證與伺服器進行加密通訊。

Kestrel 透過 `ServerCertificateSelector` 回呼來支援 SNI。 回呼會針對每個連線叫用一次，允許應用程式檢查主機名稱並選取適當的憑證。

SNI 支援需要：

* 在目標 Framework `netcoreapp2.1` 上執行。 在 `netcoreapp2.0` 和 `net461` 上，會叫用回呼，但 `name` 一律為 `null`。 如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。
* 所有網站都在相同的 Kestrel 執行個體上執行。 在不使用反向 Proxy 的情況下，Kestrel 不支援跨多個執行個體共用 IP 位址和連接埠。

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        });
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, options) =>
        {
            options.ListenAnyIP(5005, listenOptions =>
            {
                listenOptions.UseHttps(httpsOptions =>
                {
                    var localhostCert = CertificateLoader.LoadFromStoreCert(
                        "localhost", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var exampleCert = CertificateLoader.LoadFromStoreCert(
                        "example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var subExampleCert = CertificateLoader.LoadFromStoreCert(
                        "sub.example.com", "My", StoreLocation.CurrentUser, 
                        allowInvalid: true);
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
                    certs["localhost"] = localhostCert;
                    certs["example.com"] = exampleCert;
                    certs["sub.example.com"] = subExampleCert;

                    httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                    {
                        if (name != null && certs.TryGetValue(name, out var cert))
                        {
                            return cert;
                        }

                        return exampleCert;
                    };
                });
            });
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a>繫結至 TCP 通訊端

[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 方法繫結至 TCP 通訊端，而選項 Lambda 則允許 SSL 憑證組態：

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

範例使用 [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) 來設定端點的 SSL。 若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a>繫結至 Unix 通訊端

使用 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 接聽 UNIX 通訊端以改善 Nginx 的效能，如此範例所示：

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

### <a name="port-0"></a>連接埠 0

指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。 下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a>限制

使用下列方法來設定端點：

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` 命令列引數
* `urls` 主機組態索引鍵
* `ASPNETCORE_URLS` 環境變數

要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。 不過，請注意下列限制：

* SSL 無法搭配這些方法使用，除非在 HTTPS 端點組態中提供預設憑證 (例如，使用 `KestrelServerOptions` 組態或組態檔，如本主題稍早所示)。
* 當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。

### <a name="iis-endpoint-configuration"></a>IIS 端點設定

使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。 如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主題。

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a>ListenOptions.Protocols

`Protocols` 屬性會建立在連線端點上或針對伺服器啟用的 HTTP 通訊協定 (`HttpProtocols`)。 從 `HttpProtocols` 列舉中指派一個值給 `Protocols` 屬性。

| `HttpProtocols` 列舉值 | 允許的連線通訊協定 |
| -------------------------- | ----------------------------- |
| `Http1`                    | 僅限 HTTP/1.1。 可在具有或沒有 TLS 的情況下使用。 |
| `Http2`                    | 僅限 HTTP/2。 主要在具有 TLS 的情況下使用。 只有在用戶端支援[先備知識模式](https://tools.ietf.org/html/rfc7540#section-3.4)時，才可以在沒有 TLS 的情況下使用。 |
| `Http1AndHttp2`            | HTTP/1.1 和 HTTP/2。 需要 TLS 和 [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線才能交涉 HTTP/2；否則，連線預設為 HTTP/1.1。 |

預設通訊協定為 HTTP/1.1。

HTTP/2 的 TLS 限制：

* TLS 1.2 版或更新版本
* 已停用重新交涉
* 已停用壓縮
* 暫時金鑰交換大小下限：
  * 橢圓曲線 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 最小 224 個位元
  * 有限欄位 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 最小 2048 個位元
* 加密套件未列於封鎖清單中

預設支援 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; 與 P-256 橢圓曲線 &lbrack;`FIPS186`&rbrack;。

下列範例會允許連接埠 8000 上的 HTTP/1.1 和 HTTP/2 連線。 這些連線使用提供的憑證受到 TLS 保護：

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

選擇性地建立 `IConnectionAdapter` 實作，針對每個連線來篩選特定加密的 TLS 交握：

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        });
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

從設定進行通訊協定設定

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 組態。

在下列 *appsettings.json* 範例中，會為 Kestrel 的所有端點建立預設連線通訊協定 (HTTP/1.1 和 HTTP/2)：

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

下列設定檔範例會為特定端點建立連線通訊協定：

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

程式碼中指定的通訊協定會覆寫設定所設定的值。

::: moniker-end

## <a name="transport-configuration"></a>傳輸組態

隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。 對於升級到 2.1 的 ASP.NET Core 2.0 應用程式而言，這是一項重大變更，它們會呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) 並相依於下列任一套件：

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

對於使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)且需要使用 Libuv 的 ASP.NET Core 2.1 或更新版本專案：

* 將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* 呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv)：

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

### <a name="url-prefixes"></a>URL 前置詞

使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。

只有 HTTP URL 前置詞有效。 當使用 `UseUrls` 設定 URL 繫結時，Kestrel 不支援 SSL。

* IPv4 位址與連接埠號碼

  ```
  http://65.55.39.10:80/
  ```

  `0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。

* IPv6 位址與連接埠號碼

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  `[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。

* 主機名稱與連接埠號碼

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  主機名稱 `*` 和 `+` 並不特殊。 無法辨識為有效 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。 若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。

  > [!WARNING]
  > 裝載於反向 Proxy 組態需要[主機篩選](#host-filtering)。

* 主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。 如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。 如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。

## <a name="host-filtering"></a>主機篩選

雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。 主機 `localhost` 是特殊情況，用來繫結到回送位址。 任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。 `Host` 標頭未驗證。

因應措施是使用主機篩選中介軟體。 主機篩選中介軟體係由 [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) 套件提供，隨附於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 或更新版本)。 中介軟體是由 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 新增，它會呼叫 [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering)：

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

預設停用主機篩選中介軟體。 若要啓用中介軟體，請在 *appsettings.json*/*appsettings.\<環境名稱>.json* 中定義 `AllowedHosts` 索引鍵。 此值是以分號分隔的主機名稱清單，不含連接埠號碼：

*appsettings.json*：

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> [轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)也有 [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) 選項。 在不同的案例中，轉送標頭中介軟體和主機篩選中介軟體有類似的功能。 當不保留 `Host` 標頭，卻使用反向 Proxy 伺服器或負載平衡器轉送要求時，可使用轉送標頭中介軟體設定 `AllowedHosts`。 當使用 Kestrel 作為公眾對應 Edge Server，或直接轉送 `Host` 標頭時，可使用主機篩選中介軟體設定 `AllowedHosts`。
>
> 如需轉送標頭中介軟體的詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。

## <a name="additional-resources"></a>其他資源

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [Kestrel 原始程式碼](https://github.com/aspnet/KestrelHttpServer)
* [RFC 7230：Message Syntax and Routing (訊息語法和路由) 第 5.4 節：Host (主機)](https://tools.ietf.org/html/rfc7230#section-5.4)
