---
title: "ASP.NET Core 中的 HTTP.sys 網頁伺服器實作"
author: rick-anderson
description: "導入了 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 建置在 Http.Sys 核心模式驅動程式之上，HTTP.sys 是 Kestrel 的替代方式，可以用於直接連線到網際網路而不使用 IIS。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 HTTP.sys 網頁伺服器實作

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> 本主題只適用於 ASP.NET Core 2.0 和更新版本。 在舊版的 ASP.NET Core 中，HTTP.sys 名為 [WebListener](xref:fundamentals/servers/weblistener)。

HTTP.sys 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](index.md)。 它建置在 [Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)之上。 HTTP.sys 是 [Kestrel](kestrel.md) 的替代方式，提供了一些 Kestel 沒有的功能。 **HTTP.sys 不能與 IIS 或 IIS Express 搭配使用，因為它與 [ASP.NET Core 模組](aspnet-core-module.md)不相容。**

HTTP.sys 支援下列功能：

- [Windows 驗證](xref:security/authentication/windowsauth)
- 連接埠共用
- 使用 SNI 的 HTTPS
- HTTP/2 over TLS (Windows 10)
- 直接檔案傳輸
- 回應快取
- WebSocket (Windows 8)

支援的 Windows 版本：

- Windows 7 與 Windows Server 2008 R2 和更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>使用 HTTP.sys 的時機

HTTP.sys 可用於您需要直接向網際網路公開伺服器而不使用 IIS 的部署。

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

由於它建置在 Http.Sys 之上，HTTP.sys 不需要反向 Proxy 伺服器以抵禦攻擊。 Http.Sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的強固性、安全性及延展性。 IIS 本身在 Http.Sys 之上以 HTTP 接聽程式的形式執行。 

當您需要的功能不適用於 Kestrel 時，例如 Windows 驗證，HTTP.sys 會是內部部署的理想選擇。

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>如何使用 HTTP.sys

以下是針對主機作業系統和您的 ASP.NET Core 應用程式的安裝工作概觀。

### <a name="configure-windows-server"></a>設定 Windows Server

* 安裝您應用程式所需的 .NET 版本，例如 [.NET Core](https://www.microsoft.com/net/download/core) 或 [.NET Framework](https://www.microsoft.com/net/download/framework)。

* 預先註冊 URL 前置詞以繫結至 HTTP.sys，然後設定 SSL 憑證

   如果您不在 Windows 預先註冊 URL 前置詞，則必須以系統管理員權限執行您的應用程式。 唯一的例外是如果您使用 HTTP (不是 HTTPS) 繫結至 localhost，且使用大於 1024 的連接埠號碼；在此情況下不需要系統管理員權限。

   如需詳細資料，請參閱本文稍後的[如何預先註冊前置詞和設定 SSL](#preregister-url-prefixes-and-configure-ssl)。

* 開啟防火牆連接埠來允許流量到達 HTTP.sys。

   您可以使用 *netsh.exe* 或 [PowerShell Cmdlet](https://technet.microsoft.com/library/jj554906)。

另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>設定 ASP.NET Core 應用程式以使用 HTTP.sys

* 如果您使用 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 中繼套件，則不需要安裝任何套件。 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) 套件包含在中繼套件裡。

* 在您的 `Main` 方法中，對 `WebHostBuilder` 呼叫 `UseHttpSys` 擴充方法，並指定需要的任何 [HTTP.sys 選項](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)，如下列範例所示：

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>設定 HTTP.sys 選項

以下是您可以設定的一些 HTTP.sys 設定和限制。

**用戶端連線數目上限**

可以使用 *Program.cs* 中的下列程式碼，針對整個應用程式設定同時開啟的 TCP 連線數目上限：

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

連線數目上限預設為無限制 (null)。

**要求主體大小上限**

預設的要求主體大小上限是 30,000,000 個位元組，大約 28.6MB。

要覆寫 ASP.NET Core MVC 應用程式中的限制，建議的方式是使用 action 方法上的 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下範例會示範如何設定整個應用程式、每個要求的條件約束：

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

您可以在 *Startup.cs* 中覆寫特定要求的設定：

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
如果您嘗試在應用程式已開始讀取要求之後，設定要求的限制，則會擲回例外狀況。 有一個 `IsReadOnly` 屬性會告訴您 `MaxRequestBodySize` 屬性處於唯讀狀態，這表示要設定限制已經太遲。

如需其他 HTTP.sys 選項的資訊，請參閱 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)。 

### <a name="configure-urls-and-ports-to-listen-on"></a>設定要接聽的 URL 和連接埠 

ASP.NET Core 預設會繫結至 `http://localhost:5000`。 若要設定 URL 前置詞和連接埠，您可以使用 `UseUrls` 擴充方法、`urls` 命令列引數、ASPNETCORE_URLS 環境變數，或 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) 的 `UrlPrefixes` 屬性。 下列程式碼範例使用 `UrlPrefixes`。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

`UrlPrefixes` 的優點是如果您嘗試新增格式錯誤的前置詞，您會立即收到錯誤訊息。 `UseUrls` 的優點 (與 `urls` 和 ASPNETCORE_URLS 相同) 是您可以更輕鬆地切換 Kestrel 和 HTTP.sys。

如果您同時使用 `UseUrls` (或 `urls` 或 ASPNETCORE_URLS) 和 `UrlPrefixes`，`UrlPrefixes` 中的設定會覆寫 `UseUrls` 中的設定。 如需詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。

HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。

> [!NOTE]
> 請確定您在 `UseUrls` 或 `UrlPrefixes` 中指定您在伺服器上預先註冊的相同前置詞字串。 

### <a name="dont-use-iis"></a>不使用 IIS

請確定您的應用程式未設定為執行 IIS 或 IIS Express。

在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。 若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示。

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>預先註冊 URL 前置詞及設定 SSL

IIS 和 HTTP.sys 都依賴基礎 Http.Sys 核心模式驅動程式來接聽要求，以及執行初始處理。 在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。 不過，您需要自行設定 Http.Sys。 自行設定 Http.Sys 的內建工具是 *netsh.exe*。 

使用 *netsh.exe*，您可以保留 URL 前置詞並指派 SSL 憑證。 工具需要管理權限。

下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

下列範例會示範如何指派 SSL 憑證：

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

以下是 *netsh.exe* 的參考文件：

* [超文字傳輸通訊協定 (HTTP) 的 Netsh 命令](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix 字串](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

下列資源提供數種案例的詳細指示。 提到 HttpListener 的文章同樣適用於 HTTP.sys，因為兩者都是根據 Http.Sys。

* [如何：使用 SSL 憑證設定連接埠](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通訊 - 以 HttpListener 為基礎的裝載和用戶端憑證) 這是協力廠商部落格，相當老舊但仍有有用的資訊。
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (如何：逐步解說使用 HttpListener 或 Http Server unmanaged 程式碼 (C++) 作為 SSL 簡單伺服器) 這也是具有有用資訊的較舊部落格。

以下是一些協力廠商工具，比起 *netsh.exe* 命令列可以更輕鬆地使用。 這些不是由 Microsoft 提供，或經由 Microsoft 背書。 工具預設會以系統管理員身分執行，因為 *netsh.exe* 本身需要系統管理員權限。

* [http.sys Manager](http://httpsysmanager.codeplex.com/) 提供 UI 以便列出和設定 SSL 憑證和選項、前置詞保留，以及憑證信任清單。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) 可讓您列出或設定 SSL 憑證和 URL 前置詞。 UI 比 http.sys Manager 更精簡，並公開更多組態選項，但在其他方面提供類似的功能。 它無法建立新的憑證信任清單 (CTL)，但可以指派現有的憑證信任清單。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [本文的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys 原始程式碼](https://github.com/aspnet/HttpSysServer/)
* [裝載](xref:fundamentals/hosting)
