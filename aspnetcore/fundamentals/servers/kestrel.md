---
title: "ASP.NET Core 中的 Kestrel 網頁伺服器實作"
author: tdykstra
description: "介紹 Kestrel，這是以 libuv 為基礎的跨平台 ASP.NET Core 網頁伺服器。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 Kestrel 網頁伺服器實作簡介

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Stephen Halter](https://twitter.com/halter73)

Kestrel 是以 [libuv](https://github.com/libuv/libuv) (跨平台非同步 I/O 程式庫) 為基礎的跨平台 [ASP.NET Core 網路伺服器](index.md)。 Kestrel 是 ASP.NET Core 專案範本中預設隨附的網頁伺服器。 

Kestrel 支援下列功能：

  * HTTPS
  * 用來啟用 [WebSockets](https://github.com/aspnet/websockets) 的不透明升級
  * Nginx 背後的高效能 Unix 通訊端 

.NET Core 支援的所有平台和版本都支援 Kestrel。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[檢視或下載 2.x 的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[檢視或下載 1.x 的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>何時搭配使用 Kestrel 與反向 Proxy

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

您可以單獨使用 Kestrel，或與 IIS、Nginx 或 Apache 等「反向 Proxy 伺服器」搭配使用。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。

![Kestrel 不使用反向 Proxy 伺服器直接與網際網路通訊](kestrel/_static/kestrel-to-internet2.png)

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

如果 Kestrel 只公開到內部網路，則不論組態是否使用反向 Proxy 伺服器，皆可使用。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如果應用程式只接受來自內部網路的要求，您可以單獨使用 Kestrel。

![Kestrel 直接與內部網路通訊](kestrel/_static/kestrel-to-internal.png)

如果將應用程式公開到網際網路，您必須使用 IIS、Nginx 或 Apache 作為「反向 Proxy 伺服器」。 反向 Proxy 伺服器會從網際網路接收 HTTP 要求，並在進行一些初步處理後，將其轉送至 Kestrel。

![Kestrel 透過 IIS、Nginx 或 Apache 等反向 Proxy 伺服器間接與網際網路通訊](kestrel/_static/kestrel-to-internet.png)

基於安全性考量，必須有反向 Proxy 才能進行邊緣部署 (公開到網際網路中的流量)。 Kestrel 的 1.x 版對於攻擊的防禦並不充足。 這包括但不限於適當的逾時、大小限制和同時連線限制。

---

需要反向 Proxy 的情況如下：您有多個在單一伺服器上執行的應用程式共用相同的 IP 和連接埠。 不能直接使用 Kestrel，因為 Kestrel 不支援在多個處理序之間共用相同的 IP 和連接埠。 當您設定 Kestrel 接聽連接埠時，它會處理該連接埠的所有流量，而不管主機標頭為何。 可以共用連接埠的反向 Proxy 則必須在唯一的 IP 和連接埠上轉送給 Kestrel。

即使不需要反向 Proxy 伺服器，基於其他原因使用該伺服器也是不錯的選擇：

* 它可以限制公開的介面區。
* 它提供選擇性的額外組態和防禦層。
* 它能夠與現有基礎結構更好地整合。
* 它簡化了負載平衡和 SSL 設定。 只有您的反向 Proxy 伺服器需要 SSL 憑證，而且該伺服器可以使用一般 HTTP 與內部網路上的應用程式伺服器通訊。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>如何在 ASP.NET Core 應用程式中使用 Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) 套件包含在 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)裡。

ASP.NET Core 專案範本預設會使用 Kestrel。 在 *Program.cs* 中，範本程式碼會呼叫 `CreateDefaultBuilder`，而後者會呼叫場景背後的 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

如果您需要設定 Kestrel 選項，請在 *Program.cs* 中呼叫 `UseKestrel`，如下列範例所示：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

安裝 [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 套件。

在您的 `Main` 方法中，於 `WebHostBuilder` 上呼叫 [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) 擴充方法，並指定需要的任何 [Kestrel 選項](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)，如下一節所示。

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel 選項

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel 網頁伺服器所含的條件約束組態選項，在網際網路對應部署方面特別有用。 以下是一些您可以設定的限制：

- 用戶端連線數目上限
- 要求主體大小上限
- 要求主體資料速率下限

您可以在 [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) 類別的 `Limits` 屬性中設定這些條件約束和其他限制。 `Limits` 屬性會保留 [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) 類別的執行個體。 

**用戶端連線數目上限**

可以使用下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

已經從 HTTP 或 HTTPS 升級為另一個通訊協定 (例如，在 WebSocket 要求中) 的連線，有其個別限制。 升級連線之後，它不會納入 `MaxConcurrentConnections` 限制。 

連線數目上限預設為無限制 (null)。

**要求主體大小上限**

預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6MB。 

要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是在動作方法上使用 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下範例會示範如何設定整個應用程式、每個要求的條件約束：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

您可以在中介軟體中覆寫特定要求的設定：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
如果您嘗試在應用程式已開始讀取要求之後，設定要求的限制，則會擲回例外狀況。 有一個 `IsReadOnly` 屬性會告訴您 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。

**要求主體資料速率下限**

如果資料是以指定的速率 (位元組/秒) 傳入，Kestrel 會每秒檢查一次。 如果速率低於下限值，則連線會逾時。寬限期是 Kestrel 提供給用戶端的時間量，以便將其傳送速率提高到下限值；在這段期間不會檢查速率。 寬限期可協助避免中斷連線，這是由於 TCP 緩慢啟動而一開始以低速傳送資料所造成。

預設速率下限為 240 個位元組/秒，寬限期為 5 秒。

速率下限也適用於回應。 除了屬性中具有 `RequestBody` 或 `Response` 以及介面名稱之外，用來設定要求限制和回應限制的程式碼都相同。 

以下範例示範如何在 *Program.cs* 中設定資料速率下限：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

您可以在中介軟體中設定每個要求的速率：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

如需其他 Kestrel 選項的資訊，請參閱下列類別：

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

如需 Kestrel 選項的資訊，請參閱 [KestrelServerOptions 類別](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)。

---

### <a name="endpoint-configuration"></a>端點組態

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 預設會繫結至 `http://localhost:5000`。 藉由在 `KestrelServerOptions` 上 呼叫 `Listen` 或 `ListenUnixSocket` 方法，您可以設定 URL 前置詞和讓 Kestrel 接聽的連接埠。 (`UseUrls`、`urls` 命令列引數和 ASPNETCORE_URLS 環境變數同樣有效，但卻有[本文稍後](#useurls-limitations)註明的限制。)

**繫結至 TCP 通訊端**

`Listen` 方法繫結至 TCP 通訊端，而選項 Lambda 可讓您設定 SSL 憑證：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

請注意這個範例如何使用 [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs) 設定特定端點的 SSL。 若要設定特定端點的其他 Kestrel 設定，您可以使用相同的 API。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**繫結至 Unix 通訊端**

您可以接聽 Unix 通訊端以改善 Nginx 的效能，如此範例所示：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**連接埠 0**

如果您指定連接埠號碼 0，Kestrel 會動態繫結至可用的連接埠。 下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls 限制**

藉由呼叫 `UseUrls` 方法，或者使用 `urls` 命令列引數或 ASPNETCORE_URLS 環境變數，您可以設定端點。 如果您希望程式碼使用 Kestrel 以外的伺服器，這些方法會很有用。 不過，請注意下列限制：

* 您無法使用 SSL 與這些方法搭配。
* 如果您同時使用 `Listen` 方法和 `UseUrls`，`Listen` 端點會覆寫 `UseUrls` 端點。

**IIS 的端點組態**

如果您使用 IIS，IIS 的 URL 繫結就會覆寫您呼叫 `Listen` 或 `UseUrls` 所設定的任何繫結。 如需詳細資訊，請參閱 [ASP.NET Core 模組簡介](aspnet-core-module.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 預設會繫結至 `http://localhost:5000`。 藉由使用 `UseUrls` 擴充方法、`urls` 命令列引數或 ASP.NET Core 組態系統，您可以設定 URL 前置詞和 Kestrel 所要接聽的連接埠。 如需這些方法的詳細資訊，請參閱[裝載](../../fundamentals/hosting.md)。 如需當您使用 IIS 作為反向 Proxy 時，URL 繫結如何運作的資訊，請參閱 [ASP.NET Core 模組](aspnet-core-module.md)。 

---

### <a name="url-prefixes"></a>URL 前置詞

如果您呼叫 `UseUrls`，或是使用 `urls` 命令列引數或 ASPNETCORE_URLS 環境變數，URL 前置詞可以採用下列任一格式。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

只有 HTTP 的 URL 前置詞才有效；當您使用 `UseUrls` 來設定 URL 繫結時，Kestrel 不支援 SSL 。

* IPv4 位址與連接埠號碼

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 是繫結至所有 IPv4 位址的特殊情況。


* IPv6 位址與連接埠號碼

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] 是相當於 IPv4 0.0.0.0 的 IPv6 對等項目。


* 主機名稱與連接埠號碼

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  主機名稱 * 和 + 並不特殊。 不是可辨識的 IP 位址或 "localhost" 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。 如果您需要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [HTTP.sys](httpsys.md) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。

* "Localhost" 與連接埠號碼，或回送 IP 與連接埠號碼

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  如果指定了 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。 如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。 如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4 位址與連接埠號碼

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 是繫結至所有 IPv4 位址的特殊情況。


* IPv6 位址與連接埠號碼

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] 是相當於 IPv4 0.0.0.0 的 IPv6 對等項目。


* 主機名稱與連接埠號碼

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  主機名稱 \* 和 + 並不特殊。 不是可辨識的 IP 位址或 "localhost" 的任何項目，都會繫結至所有 IPv4 和 IPv6 IP。 如果您需要在相同連接埠上將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式，請使用 [WebListener](weblistener.md) 或反向 Proxy 伺服器 (例如 IIS、Nginx 或 Apache)。

* "Localhost" 與連接埠號碼，或回送 IP 與連接埠號碼

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  如果指定了 `localhost`，Kestrel 會嘗試同時繫結至 IPv4 和 IPv6 回送介面。 如果所要求的連接埠在任一個回送介面上由另一個服務使用，則 Kestrel 無法啟動。 如果任一回送介面由於任何其他原因 (最常見的原因是不支援 IPv6) 無法使用，Kestrel 就會記錄警告。 

* UNIX 通訊端

  ```
  http://unix:/run/dan-live.sock  
  ```

**連接埠 0**

如果您指定連接埠號碼 0，Kestrel 會動態繫結至可用的連接埠。 除了 `localhost` 名稱，任何主機名稱或 IP 都允許繫結至連接埠 0。

下列範例示範如何判斷 Kestrel 在執行階段實際上繫結至哪一個連接埠：

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**SSL 的 URL 前置詞**

如果您呼叫 `UseHttps` 擴充方法，請務必使用 `https:` 包含 URL 前置詞，如下所示。

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

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* [2.x 的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [Kestrel 原始程式碼](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* [1.x 的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [Kestrel 原始程式碼](https://github.com/aspnet/KestrelHttpServer)

---
