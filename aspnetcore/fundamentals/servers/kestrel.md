---
title: ASP.NET Core 中的 Kestrel 網頁伺服器實作
author: rick-anderson
description: 了解 Kestrel，這是 ASP.NET Core 的跨平台網頁伺服器。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1c5d229614e6d6ca6889d19a5f3dc145da01bc04
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555322"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 Kestrel 網頁伺服器實作

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)

Kestrel 是 [ASP.NET Core 的跨平台網頁伺服器](xref:fundamentals/servers/index)。 Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。

Kestrel 支援下列功能：

* HTTPS
* 用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級
* Nginx 背後的高效能 Unix 通訊端

.NET Core 支援的所有平台和版本都支援 Kestrel。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>何時搭配使用 Kestrel 與反向 Proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

不論設定是否具有反向 Proxy 伺服器，對於 ASP.NET Core 2.0 或更新版本的應用程式，其中之一都是有效且支援的裝載設定。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果應用程式只接受來自內部網路的要求，就可直接使用 Kestrel 作為應用程式的伺服器。

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

如果將應用程式公開到網際網路，請使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

基於安全性考量，必須有反向 Proxy 才能進行邊緣部署 (公開到網際網路中的流量)。 Kestrel 的 1.x 版沒有針對攻擊的完整防禦補充，例如適當的逾時、大小上限，以及並行連線限制。

---

反向 Proxy 存在的情況如下：有多個在單一伺服器上執行的應用程式共用相同的 IP 和連接埠。 Kestrel 不支援這種情況，因為 Kestrel 不支援在多個處理序之間共用相同的 IP 和連接埠。 當 Kestrel 設定為接聽通訊埠時，Kestrel 會處理該連接埠的所有流量，而不論要求的主機標頭。 可以共用連接埠的反向 Proxy 能夠在唯一的 IP 和連接埠上轉送要求給 Kestrel。

即使不需要反向 Proxy 伺服器，反向 Proxy 伺服器也是不錯的選擇：

* 它可以限制它所主控之應用程式的公開介面區。
* 它提供額外的組態和防禦層。
* 它能夠與現有基礎結構更好地整合。
* 它簡化負載平衡和 SSL 組態。 只有反向 Proxy 伺服器需要 SSL 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器通訊。

> [!WARNING]
> 如果未使用反向 Proxy 並啟用主機篩選，則必須啟用[主機篩選](#host-filtering)。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>如何在 ASP.NET Core 應用程式中使用 Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 套件包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)裡。

ASP.NET Core 專案範本預設會使用 Kestrel。 在 *Program.cs* 中，範本程式碼會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)，而後者會呼叫場景背後的 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel)。

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

安裝 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 套件。

在 `Main` 方法中，於 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) 上呼叫 [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) 擴充方法，並指定需要的任何 [Kestrel 選項](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)，如下一節所示。

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel 選項

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。 可以自訂幾個重要限制：

* 用戶端連線數目上限
* 要求主體大小上限
* 要求主體資料速率下限

請在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 類別的 [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) 屬性中設定這些和其他條件約束。 `Limits` 屬性會保留 [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) 類別的執行個體。

**用戶端連線數目上限**

[MaxConcurrentConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[MaxConcurrentUpgradedConnections](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3)]

已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。 升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=4)]

連線數目上限預設為無限制 (null)。

**要求主體大小上限**

[MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6 MB。

要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下範例會示範如何設定應用程式、每個要求的條件約束：

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

您可以在中介軟體中覆寫特定要求的設定：

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。 有一個 `IsReadOnly` 屬性會指出 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。

**要求主體資料速率下限**

[MinRequestBodyDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[MinResponseDataRate](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。 如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。 寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。

預設速率下限為 240 個位元組/秒，寬限期為 5 秒。

速率下限也適用於回應。 除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。

以下範例示範如何在 *Program.cs* 中設定資料速率下限：

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-7)]

您可以在中介軟體中設定每個要求的速率：

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

如需其他 Kestrel 選項和限制的資訊，請參閱：

* [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

如需 Kestrel 選項和限制的資訊，請參閱：

* [KestrelServerOptions 類別](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

---

### <a name="endpoint-configuration"></a>端點組態

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
ASP.NET Core 預設會繫結至 `http://localhost:5000`。 在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 上呼叫 [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 或 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 方法，以設定 Kestrel 的 URL 前置詞和連接埠。 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制。

`urls` 主機組態索引鍵必須來自主機組態，而不是應用程式組態。 將 `urls` 索引鍵和值新增至 *appsettings.json* 並不會影響主機組態，因為在從組態檔讀取組態時，主機已完全初始化。 不過，*appsettings.json* 中的 `urls` 索引鍵可以搭配主機產生器上的 [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) 來設定主機：

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 預設會繫結至：

* `http://localhost:5000`
* `https://localhost:5001` (當有本機開發憑證存在時)

開發憑證會建立於：

* 已安裝 [.NET Core SDK](/dotnet/core/sdk) 時。
* [dev-certs 工具](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs)用來建立憑證。

有些瀏覽器需要您授與明確的權限給瀏覽器，才能信任本機開發憑證。

ASP.NET Core 2.1 和更新版本的專案範本會將應用程式設定為預設於 HTTPS 上執行，並包含 [HTTPS 重新導向和 HSTS 支援](xref:security/enforcing-ssl)。

在 [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) 上呼叫 [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 或 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 方法，以設定 Kestrel 的 URL 前置詞和連接埠。

`UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵和 `ASPNETCORE_URLS` 環境變數同樣有效，但卻有本節稍後註明的限制 (針對 HTTPS 端點組態必須有預設憑證可用)。

ASP.NET Core 2.1 `KestrelServerOptions` 組態：

**ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)**  
指定組態 `Action` 以針對每個指定端點執行。 呼叫 `ConfigureEndpointDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。

**ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)**  
指定組態 `Action` 以針對每個 HTTPS 端點執行。 呼叫 `ConfigureHttpsDefaults` 多次會以最後一個指定的 `Action` 取代之前的 `Action`。

**Configure(IConfiguration)**  
建立組態載入器，設定採用 [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) 作為輸入的 Kestrel。 組態的範圍必須限於 Kestrel 的組態區段。

**ListenOptions.UseHttps**  
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

使用這些方法提供的值可以是一或多個 HTTP 和 HTTPS 端點 (如果有預設憑證可用則為 HTTPS)。 將值設定為以分號分隔的清單 (例如，`"Urls": "http://localhost:8000;http://localhost:8001"`)。

*從組態取代預設憑證*

[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 預設會呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 以載入 Kestrel 組態。 Kestrel 可以使用預設的 HTTPS 應用程式設定組態結構描述。 設定多個端點，包括 URL 和要使用的憑證－從磁碟上的檔案，或是從憑證存放區。

在下列 *appsettings.json* 範例中：

* 將 **AllowInvalid** 設定為 `true`，允許使用無效的憑證 (例如，自我簽署憑證)。
* 任何未指定憑證 (接下來範例中的 **HttpsDefaultCert**) 的 HTTPS 端點會回復為 [憑證] >[預設] 下定義的憑證或開發憑證。

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
* `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 會傳回 `KestrelConfigurationLoader` 與 `.Endpoint(string name, options => { })` 方法，此方法可用來補充已設定的端點設定：

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  您也可以直接存取 `KestrelServerOptions.ConfigurationLoader` 來保持反覆運算現有的載入器，例如 `WebHost.CreatedDeafaultBuilder` 提供的載入器。

* 每個端點的組態區段可用於 `Endpoint` 方法的選項，因此可讀取自訂組態。
* 可以藉由使用另一個區段再次呼叫 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 而載入多個組態。 只會使用最後一個組態，除非在先前的執行個體上已明確呼叫 `Load`。 中繼套件不會呼叫 `Load`，如此可能會取代其預設組態區段。
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

SNI 支援需要在目標架構 `netcoreapp2.1` 上執行。 在 `netcoreapp2.0` 和 `net461` 上，會叫用回呼，但 `name` 一律為 `null`。 如果用戶端不在 TLS 信號交換中提供主機名稱參數，則 `name` 也是 `null`。

```csharp
WebHost.CreateDefaultBuilder()
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
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
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

**繫結至 TCP 通訊端**

[Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) 方法繫結至 TCP 通訊端，而選項 Lambda 則允許 SSL 憑證組態：

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

範例使用 [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions) 來設定端點的 SSL。 若要設定特定端點的其他 Kestrel 設定，請使用相同的 API。

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

**繫結至 Unix 通訊端**

使用 [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) 接聽 UNIX 通訊端以改善 Nginx 的效能，如此範例所示：

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

**連接埠 0**

指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。 下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：

```console
Now listening on: http://127.0.0.1:48508
```

**UseUrls、-url 命令列引數、urls 主機組態索引鍵和 ASPNETCORE_URLS 環境變數限制**

使用下列方法來設定端點：

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
* `--urls` 命令列引數
* `urls` 主機組態索引鍵
* `ASPNETCORE_URLS` 環境變數

要讓程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。 不過，請注意下列限制：

* SSL 無法搭配這些方法使用，除非在 HTTPS 端點組態中提供預設憑證 (例如，使用 `KestrelServerOptions` 組態或組態檔，如本主題稍早所示)。
* 當同時使用 `Listen` 和 `UseUrls` 方法時，`Listen` 端點會覆寫 `UseUrls` 端點。

**IIS 端點組態**

使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `Listen` 或 `UseUrls` 設定。 如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)主題。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

ASP.NET Core 預設會繫結至 `http://localhost:5000`。 使用下列各項設定 URL 前置詞和 Kestrel 的連接埠：

* [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) 擴充方法
* `--urls` 命令列引數
* `urls` 主機組態索引鍵
* ASP.NET Core 組態系統，包括 `ASPNETCORE_URLS` 環境變數

如需這些方法的詳細資訊，請參閱[裝載](xref:fundamentals/host/index)。

**IIS 端點組態**

使用 IIS 時，IIS 覆寫繫結的 URL 繫結是由 `UseUrls` 設定。 如需詳細資訊，請參閱 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)主題。

---

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a>傳輸組態

隨著 ASP.NET Core 2.1 的發行，Kestrel 的預設傳輸不再根據 Libuv，而是改為根據受控通訊端。 對於升級到 2.1 的 ASP.NET Core 2.0 應用程式而言，這是一項重大變更，它們會呼叫 [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) 並相依於下列任一套件：

* [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (直接套件參考)
* [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

對於使用 `Microsoft.AspNetCore.App` 中繼套件且需要使用 Libuv 的 ASP.NET Core 2.1 或更新版本專案：

* 將 [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) 套件的相依性新增至應用程式的專案檔中：

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                    Version="2.1.0" />
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

::: moniker-end

### <a name="url-prefixes"></a>URL 前置詞

使用 `UseUrls`、`--urls` 命令列引數、`urls` 主機組態索引鍵或 `ASPNETCORE_URLS` 環境變數時，URL 前置詞可以採用下列任一格式。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

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
  > 如果未使用反向 Proxy 並啟用主機篩選，請啟用[主機篩選](#host-filtering)。

* 主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。 如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。 如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* IPv4 位址與連接埠號碼

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  `0.0.0.0` 是繫結至所有 IPv4 位址的特殊情況。

* IPv6 位址與連接埠號碼

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  `[::]` 是相當於 IPv4 `0.0.0.0` 的 IPv6 對等項目。

* 主機名稱與連接埠號碼

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  主機名稱、`*` 和 `+` 並不特殊。 不是可辨識的 IP 位址或 `localhost` 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。 若要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [WebListener](xref:fundamentals/servers/weblistener) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。

* 主機 `localhost` 名稱與連接埠號碼，或回送 IP 與連接埠號碼

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  如果指定 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。 如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。 如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。

* UNIX 通訊端

  ```
  http://unix:/run/dan-live.sock
  ```

**連接埠 0**

指定連接埠號碼 `0` 時，Kestrel 會動態繫結至可用的連接埠。 除了 `localhost`，任何主機名稱或 IP 都允許繫結至連接埠 `0`。

當應用程式執行時，主控台視窗輸出會指出可以連線到應用程式的動態連接埠：

```console
Now listening on: http://127.0.0.1:48508
```

**SSL 的 URL 前置詞**

如果呼叫 `UseHttps` 擴充方法，請務必使用 `https:` 包含 URL 前置詞：

```csharp
var host = new WebHostBuilder()
    .UseKestrel(options =>
    {
        options.UseHttps("testCert.pfx", "testPassword");
    })
   .UseUrls("http://localhost:5000", "https://localhost:5001")
   .UseContentRoot(Directory.GetCurrentDirectory())
   .UseStartup<Startup>()
   .Build();
```

> [!NOTE]
> HTTPS 和 HTTP 無法裝載在相同的連接埠上。

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

---

## <a name="host-filtering"></a>主機篩選

雖然 Kestrel 根據前置詞來支援組態，例如 `http://example.com:5000`，Kestrel 大多會忽略主機名稱。 主機 `localhost` 是特殊情況，用來繫結到回送位址。 任何非明確 IP 位址的主機，會繫結至所有公用 IP 位址。 此資訊完全不用來驗證要求 `Host` 標頭。

有兩種因應措施：

* 反向 proxy 後方的主機，並使用主機標頭篩選。 這是 Kestrel 在 ASP.NET Core 1.x 中唯一支援的案例。
* 請使用中介軟體，依 `Host` 標頭篩選要求。 範例中介軟體如下：

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

在 `Startup.Configure` 中註冊前面的 `HostFilteringMiddleware`。 請注意，[中介軟體註冊的順序](xref:fundamentals/middleware/index#ordering)很重要。 註冊應該在註冊中介軟體註冊 (例如 `app.UseExceptionHandler`) 之後立即發生。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

前面的中介軟體預期在 *appsettings.\<EnvironmentName>.json* 中有 `AllowedHosts` 索引鍵。 此索引鍵的值是以分號分隔的主機名稱清單，不含連接埠號碼。 將 `AllowedHosts` 索引鍵值配對包含在 *appsettings.Production.json*中：

```json
{
  "AllowedHosts": "example.com"
}
```

*appsettings.Development.json* (localhost 組態檔)：

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a>其他資源

* [強制使用 HTTPS](xref:security/enforcing-ssl)
* [Kestrel 原始程式碼](https://github.com/aspnet/KestrelHttpServer)
