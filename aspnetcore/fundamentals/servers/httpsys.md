---
title: "ASP.NET Core HTTP.sys web 伺服器實作"
author: rick-anderson
description: "導入了 HTTP.sys，適用於在 Windows 上的 ASP.NET Core web 伺服器。 建置在 Http.Sys 核心模式驅動程式，HTTP.sys 會是 Kestrel 用於 IIS 沒有網際網路直接連線的替代方式。"
keywords: "ASP.NET Core,HttpSys,HTTP.sys,HttpListener,url 前置詞的 SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: d3f9eb4943ed62b674d6bb2ab1b275b0a3c02343
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core HTTP.sys web 伺服器實作

由[Tom Dykstra](https://github.com/tdykstra)和[Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> 本主題只適用於 ASP.NET Core 2.0 和更新版本。 在舊版的 ASP.NET Core，HTTP.sys 會命名為[WebListener](xref:fundamentals/servers/weblistener)。

HTTP.sys 是[適用於 ASP.NET Core web 伺服器](index.md)只能在 Windows 上執行。 此系統建置[Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)。 HTTP.sys 是替代[Kestrel](kestrel.md) ，提供了一些 Kestel 沒有的功能。 **HTTP.sys 不能與 IIS 或 IIS Express，並不相容於[ASP.NET 核心模組](aspnet-core-module.md)。**

HTTP.sys 支援下列功能：

- [Windows 驗證](xref:security/authentication/windowsauth)
- 連接埠共用
- SNI 使用 HTTPS
- HTTP/2 5060，TLS (Windows 10)
- 直接檔案傳輸
- 回應快取
- WebSockets (Windows 8)

支援的 Windows 版本：

- Windows 7 和 Windows Server 2008 R2 和更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>使用 HTTP.sys 的時機

HTTP.sys 可用於部署您要公開，直接向網際網路伺服器而不使用 IIS。

![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

由於它建置在 Http.Sys 中，HTTP.sys 不需要保護以對抗攻擊反向 proxy 伺服器。 Http.Sys 是成熟的技術，可保護免於許多種類的攻擊，並提供強固性、 安全性及功能完整的 web 伺服器的延展性。 為 HTTP 接聽程式在 Http.Sys 之上，IIS 本身的執行。 

當您需要的功能不適用於 Kestrel，例如 Windows 驗證時，HTTP.sys 會是內部部署的理想選擇。

![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>如何使用 HTTP.sys

以下是針對主機作業系統和您的 ASP.NET Core 應用程式安裝工作的概觀。

### <a name="configure-windows-server"></a>設定 Windows Server

* 安裝新版的.NET 應用程式所需，例如[.NET Core](https://www.microsoft.com/net/download/core)或[.NET Framework](https://www.microsoft.com/net/download/framework)。

* __'Asverify'__ 繫結至 HTTP.sys，以及設定 SSL 憑證的 URL 前置詞

   如果您不要 __'asverify'__ URL 前置詞，在 Windows 中的，您必須以系統管理員權限執行您的應用程式。 唯一的例外是如果您將繫結使用 HTTP (不是 HTTPS) 連接埠號碼大於 1024; localhost在此情況下，系統管理員權限不需要。

   如需詳細資訊，請參閱[如何 __'asverify'__ 前置詞及設定 SSL](#preregister-url-prefixes-and-configure-ssl)本文稍後。

* 開啟防火牆連接埠來允許流量到達 HTTP.sys。

   您可以使用*netsh.exe*或[PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)。

另外還有[Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>設定您的 ASP.NET Core 應用程式使用 HTTP.sys

* 如果您使用，則需要任何封裝安裝[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage。 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)封裝包含在 metapackage。

* 呼叫`UseHttpSys`上的擴充方法`WebHostBuilder`中您`Main`方法，並指定任何[HTTP.sys 選項](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)，您需要如下列範例所示：

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>設定 HTTP.sys 選項

以下是一些 HTTP.sys 設定，您可以設定的限制。

**最大用戶端連線**

同時開啟的 TCP 連線的數目上限可以設定整個應用程式中的下列程式碼*Program.cs*:

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

最大連接數目是無限制 (null) 的預設值。

**要求主體大小上限**

預設的最大要求本文大小是 30,000,000 位元組，大約 28.6 MB。

若要覆寫中的 ASP.NET Core MVC 應用程式的限制的建議的方式是使用[RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)動作方法上的屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

以下是範例，示範如何設定整個應用程式的每個要求的條件約束：

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

您可以覆寫中的特定要求上設定*Startup.cs*:

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
如果您嘗試將應用程式已開始讀取要求之後，在要求上設定的限制，則會擲回例外狀況。 沒有`IsReadOnly`屬性會告訴您，如果`MaxRequestBodySize`屬性處於唯讀狀態，這表示它已經太遲設定的限制。

HTTP.sys 中的其他選項的相關資訊，請參閱[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)。 

### <a name="configure-urls-and-ports-to-listen-on"></a>設定 Url 和連接埠上接聽 

根據預設 ASP.NET Core 繫結至`http://localhost:5000`。 若要設定 URL 前置詞和連接埠，您可以使用`UseUrls`擴充方法，`urls`命令列引數、 ASPNETCORE_URLS 環境變數，或`UrlPrefixes`屬性[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)。 下列程式碼範例使用`UrlPrefixes`。

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

優點`UrlPrefixes`會收到錯誤訊息立即如果您嘗試新增的格式錯誤的前置詞。 優點`UseUrls`(與共用`urls`和 ASPNETCORE_URLS) 是您可以更輕鬆地切換 Kestrel 和 HTTP.sys 之間。

如果您同時使用`UseUrls`(或`urls`或 ASPNETCORE_URLS) 和`UrlPrefixes`中的設定`UrlPrefixes`覆寫中的`UseUrls`。 如需詳細資訊，請參閱[裝載](xref:fundamentals/hosting)。

HTTP.sys 會使用[HTTP 伺服器 API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。

> [!NOTE]
> 請確定您指定在相同的前置詞字串`UseUrls`或`UrlPrefixes`，您預先註冊的伺服器上。 

### <a name="dont-use-iis"></a>不使用 IIS

請確定您的應用程式未設定為執行 IIS 或 IIS Express。

在 Visual Studio 中的預設啟動設定檔為 IIS Express。 若要執行專案為主控台應用程式，手動變更選取的設定檔，如下列螢幕擷取畫面所示。

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>__'Asverify'__ URL 前置詞及設定 SSL

IIS 和 HTTP.sys 依賴接聽要求的基礎 Http.Sys 核心模式驅動程式，並執行初始處理。 在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。 不過，您需要自行設定 Http.Sys。 內建的工具，也就是執行*netsh.exe*。 

與*netsh.exe*就可以保留 URL 首碼，並指派 SSL 憑證。 此工具需要系統管理權限。

下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

下列範例會示範如何將 SSL 憑證指派：

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

以下是參考文件*netsh.exe*:

* [Netsh 命令都會使用超文字傳輸通訊協定 (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix 字串](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

下列資源提供數種案例的詳細的指示。 指 HttpListener 的發行項同樣適用於 HTTP.sys，同時根據 Http.Sys。

* [如何： 使用 SSL 憑證設定連接埠](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS 通訊-HttpListener 基礎代管與用戶端憑證](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)這是協力廠商架構部落格和相當老舊，但仍有有用的資訊。
* [如何： 逐步解說使用 HttpListener 或 Http 伺服器 unmanaged 程式碼 （c + +） 做為 SSL 簡單伺服器](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)這也是較舊的部落格包含有用資訊。

以下是一些協力廠商工具可以更輕鬆地使用比*netsh.exe*命令列。 這些不是所提供，或經由 Microsoft 背書。 這些工具執行系統管理員身分根據預設，由於*netsh.exe*本身需要系統管理員權限。

* [http.sys 管理員](http://httpsysmanager.codeplex.com/)清單提供 UI，並設定 SSL 憑證和選項保留的前置詞及憑證信任清單。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)可讓您列出或設定 SSL 憑證和 URL 前置詞。 UI 會更精簡比 http.sys 管理員，並公開更多組態選項，但否則它會提供類似的功能。 它無法建立新的憑證信任清單 (CTL)，但可以將現有的指派。

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [這篇文章的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [HTTP.sys 原始程式碼](https://github.com/aspnet/HttpSysServer/)
* [裝載](xref:fundamentals/hosting)
