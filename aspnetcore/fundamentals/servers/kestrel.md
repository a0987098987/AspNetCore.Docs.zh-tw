---
title: "ASP.NET Core kestrel web 伺服器實作"
author: tdykstra
description: "ASP.NET Core 根據 libuv 介紹 Kestrel，跨平台 web 伺服器。"
keywords: "ASP.NET Core，Kestrel，libuv，url 前置詞"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a961d46d7804b7ac7e570692fe42727feae3d5c9
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a>ASP.NET Core Kestrel web 伺服器實作的簡介

由[Tom Dykstra](https://github.com/tdykstra)， [Chris Ross](https://github.com/Tratcher)，和[Stephen Halter](https://twitter.com/halter73)

Kestrel 是跨平台[適用於 ASP.NET Core web 伺服器](index.md)根據[libuv](https://github.com/libuv/libuv)，跨平台非同步 I/O 文件庫。 Kestrel 是 ASP.NET Core 專案範本中的預設隨附的 web 伺服器。 

Kestrel 支援下列功能：

  * HTTPS
  * 用來啟用不透明升級[WebSockets](https://github.com/aspnet/websockets)
  * Nginx 背後的高效能的 Unix 通訊端 

Kestrel 都支援所有平台和.NET Core 支援的版本。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[檢視或下載 2.x 的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[檢視或下載 1.x 的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a>使用 Kestrel 透過反向 proxy 的時機

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

需要邊緣部署 （公開流量從網際網路） 的安全性考量的反向 proxy。 Kestrel 的 1.x 版對於攻擊的防禦並不充足。 其中包括但不限制於適當的逾時、 大小上限，以及並行連線限制。

---

您有多個共用相同的 IP 和連接埠將單一伺服器上執行的應用程式時需要的反向 proxy 的案例。 不會直接使用 Kestrel 因為 Kestrel 不支援共用相同的 IP 和多個處理序之間的連接埠。 當您設定 Kestrel 通訊埠上接聽時，它會處理該連接埠，無論主機標頭的所有流量。 反向 proxy 可以共用連接埠，然後必須轉送給 Kestrel 上唯一的 IP 和連接埠。

即使反向 proxy 伺服器不是必要的使用其中一種可能很不錯的選擇對於其他原因：

* 它會限制公開的介面區。
* 它提供選擇性額外的組態並提供深層防禦。
* 它可能與現有基礎結構更好的整合。
* 它可簡化負載平衡和 SSL 設定。 只有您的反向 proxy 伺服器需要 SSL 憑證，且該伺服器與通訊使用一般 HTTP 內部網路上的應用程式伺服器。

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a>如何在 ASP.NET Core 應用程式中使用 Kestrel

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/)封裝包含在[Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)。

依預設，ASP.NET Core 專案範本使用 Kestrel。 在*Program.cs*，範本程式碼會呼叫`CreateDefaultBuilder`，而它會呼叫[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)背後的運作原理。

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

如果您要設定 Kestrel 選項，請呼叫`UseKestrel`中*Program.cs*如下列範例所示：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

安裝[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet 封裝。

呼叫[UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)上的擴充方法`WebHostBuilder`中您`Main`方法，並指定任何[Kestrel 選項](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)需要下一節中所示。

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a>Kestrel 選項

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Kestrel web 伺服器的條件約束組態選項，在網際網路對向部署時特別有用。 以下是一些您可以設定的限制：

- 用戶端連線數目上限
- 要求主體大小上限
- 要求主體資料速率下限

您設定這些條件約束與其他人在`Limits`屬性[KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)類別。 `Limits`屬性會保留的執行個體[KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)類別。 

**最大用戶端連線**

同時開啟的 TCP 連線的數目上限可以設定整個應用程式，以下列程式碼：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

沒有個別的限制已經從 HTTP 或 HTTPS 升級到另一個通訊協定 （例如，在 Websocket 要求） 的連線。 升級連線之後，它不會納入`MaxConcurrentConnections`限制。 

最大連接數目是無限制 (null) 的預設值。

**要求主體大小上限**

預設的最大要求本文大小是 30,000,000 位元組，大約 28.6 MB。 

若要覆寫中的 ASP.NET Core MVC 應用程式的限制的建議的方式是使用[RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)動作方法上的屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下是範例，示範如何設定整個應用程式的每個要求的條件約束：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

您可以覆寫特定中介軟體中的要求上的設定：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
如果您嘗試將應用程式已開始讀取要求之後，在要求上設定的限制，則會擲回例外狀況。 沒有`IsReadOnly`屬性會告訴您，如果`MaxRequestBodySize`屬性處於唯讀狀態，這表示它已經太遲設定的限制。

**最小要求主體資料的速率**

如果資料是中指定的速率位元組/秒 kestrel 會檢查每秒。 如果速率低於最小值、 連接逾時。在寬限期是時間的 Kestrel 可讓用戶端來增加其傳送速率的最小值量在這段期間，不會檢查速率。 在寬限期內可協助避免中斷連接一開始是以低速由於 TCP 緩慢開始傳送資料。

預設最小頻率為 240 位元組/秒、 5 秒的寬限期內。

回應也適用於最小的速率。 程式碼以設定 要求限制和回應限制為相同，但是具有`RequestBody`或`Response`中的屬性和介面名稱。 

以下是範例，示範如何設定中的最小的資料速率*Program.cs*:

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

您可以設定每個要求的速率中介軟體中,：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

如需其他 Kestrel 選項的資訊，請參閱下列類別：

* [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Kestrel 選項的相關資訊，請參閱[KestrelServerOptions 類別](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions)。

---

### <a name="endpoint-configuration"></a>端點組態

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

根據預設 ASP.NET Core 繫結至`http://localhost:5000`。 您設定 URL 前置詞和連接埠上接聽，藉由呼叫 Kestrel`Listen`或`ListenUnixSocket`方法`KestrelServerOptions`。 (`UseUrls`、`urls`命令列引數，並同樣 ASPNETCORE_URLS 環境變數，但沒有註明的限制[本文稍後](#useurls-limitations)。)

**繫結至 TCP 通訊端**

`Listen`方法繫結至 TCP 通訊端，而且選項 lambda，可讓您設定的 SSL 憑證：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

請注意如何這個範例會設定 SSL 特定端點使用[ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)。 若要設定其他 Kestrel 設定之特定端點設定，您可以使用相同的 API。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

**繫結至 Unix 通訊端**

您可以接聽 Unix 通訊端上以改善效能與 Nginx，本範例所示：

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

**連接埠 0**

如果您指定連接埠號碼 0，Kestrel 動態繫結至可用的通訊埠。 下列範例會示範如何判斷哪些實際上是 Kestrel 在執行階段繫結的連接埠：

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

**UseUrls 限制**

您可以設定端點，藉由呼叫`UseUrls`方法或使用`urls`命令列引數或 ASPNETCORE_URLS 環境變數。 這些方法會很有用，如果您希望程式碼以使用 Kestrel 以外的伺服器。 不過，請注意這些限制：

* 您無法使用 SSL，使用這些方法。
* 如果您同時使用`Listen`方法和`UseUrls`、`Listen`端點覆寫`UseUrls`端點。

**適用於 IIS 的端點組態**

如果您使用 IIS 時，IIS 的 URL 繫結覆寫任何您藉由呼叫所設定的繫結`Listen`或`UseUrls`。 如需詳細資訊，請參閱[簡介 ASP.NET 核心模組](aspnet-core-module.md)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

根據預設 ASP.NET Core 繫結至`http://localhost:5000`。 您可以設定 URL 前置詞和 Kestrel 接聽所使用的連接埠`UseUrls`擴充方法，`urls`命令列引數或 ASP.NET Core 組態系統。 如需有關這些方法的詳細資訊，請參閱[主控](../../fundamentals/hosting.md)。 當您使用 IIS 的反向 proxy，URL 繫結如何運作的詳細資訊，請參閱[ASP.NET 核心模組](aspnet-core-module.md)。 

---

### <a name="url-prefixes"></a>URL 前置詞

如果您呼叫`UseUrls`或使用`urls`命令列引數或 ASPNETCORE_URLS 環境變數，URL 前置詞可以是下列任一形式。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

只有 HTTP URL 的前置詞是有效的;當您設定 URL 繫結使用 kestrel 不支援 SSL `UseUrls`。

* IPv4 位址與連接埠號碼

  ```
  http://65.55.39.10:80/
  ```

  0.0.0.0 是一個特殊情況，將繫結至所有 IPv4 位址。


* IPv6 位址的連接埠號碼

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  [::] 就 IPv6 相當於 IPv4 0.0.0.0。


* 主機名稱與連接埠號碼

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  主機名稱，*，和 +，不是特殊。 任何不是可辨識的 IP 位址或"localhost"的項目會繫結的所有 IPv4 和 IPv6 Ip。 如果您要將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式相同的連接埠，請使用[HTTP.sys](httpsys.md)或如 IIS、 Nginx 或 Apache 反向 proxy 伺服器。

* "Localhost"的連接埠號碼的連接埠號碼或回送 IP 名稱

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  當`localhost`指定，則 Kestrel 嘗試繫結到 IPv4 和 IPv6 的回送介面。 如果要求的通訊埠，在任一個回送介面上的另一個服務使用 Kestrel 無法啟動。 如果任一個回送介面，就無法使用任何其他原因 （大部分通常是因為不支援 IPv6 的時候），Kestrel 記錄警告。 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IPv4 位址與連接埠號碼

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  0.0.0.0 是一個特殊情況，將繫結至所有 IPv4 位址。


* IPv6 位址的連接埠號碼

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  [::] 就 IPv6 相當於 IPv4 0.0.0.0。


* 主機名稱與連接埠號碼

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  主機名稱， \*，並不是特殊 +。 任何不是可辨識的 IP 位址或"localhost"的項目繫結至所有 IPv4 和 IPv6 Ip。 如果您要將不同的主機名稱繫結至不同的 ASP.NET Core 應用程式相同的連接埠，請使用[WebListener](weblistener.md)或如 IIS、 Nginx 或 Apache 反向 proxy 伺服器。

* "Localhost"的連接埠號碼的連接埠號碼或回送 IP 名稱

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  當`localhost`指定，則 Kestrel 嘗試繫結到 IPv4 和 IPv6 的回送介面。 如果要求的通訊埠，在任一個回送介面上的另一個服務使用 Kestrel 無法啟動。 如果任一個回送介面，就無法使用任何其他原因 （大部分通常是因為不支援 IPv6 的時候），Kestrel 記錄警告。 

* Unix 通訊端

  ```
  http://unix:/run/dan-live.sock  
  ```

**連接埠 0**

如果您指定連接埠號碼 0，Kestrel 動態繫結至可用的通訊埠。 繫結至連接埠 0 允許任何主機名稱或 IP 除了`localhost`名稱。

下列範例會示範如何判斷哪些實際上是 Kestrel 在執行階段繫結的連接埠：

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

**SSL 的 URL 前置詞**

務必包含 URL 前置詞與`https:`如果您呼叫`UseHttps`擴充方法，如下所示。

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
> 無法在相同的連接埠上裝載 HTTPS 和 HTTP。

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
