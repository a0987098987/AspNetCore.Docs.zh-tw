---
title: "ASP.NET Core WebListener web 伺服器實作"
author: rick-anderson
description: "導入了 WebListener，適用於在 Windows 上的 ASP.NET Core web 伺服器。 建置在 Http.Sys 核心模式驅動程式，WebListener 是可以用於 IIS 沒有網際網路直接連線的 Kestrel 的替代方式。"
keywords: "ASP.NET Core、 WebListener、 HttpListener、 url 前置詞，SSL"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: f1abb3558546cd907c78b44d9353d9c9f1f5aff1
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET Core WebListener web 伺服器實作

由[Tom Dykstra](https://github.com/tdykstra)和[Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> 本主題只適用於 ASP.NET Core 1.x。 在 ASP.NET Core 2.0 中，名為 WebListener [HTTP.sys](httpsys.md)。

WebListener 是[適用於 ASP.NET Core web 伺服器](index.md)只能在 Windows 上執行。 此系統建置[Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)。 WebListener 是替代[Kestrel](kestrel.md) ，可用來直接連線到網際網路不需依賴 IIS 做為反向 proxy 伺服器。 事實上， **WebListener 不能與 IIS 或 IIS Express，並不相容於[ASP.NET 核心模組](aspnet-core-module.md)。**

雖然 WebListener 針對 ASP.NET Core 開發，它可以用任何.NET Core 或.NET Framework 應用程式透過直接在[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 封裝。

WebListener 支援下列功能：

- [Windows 驗證](xref:security/authentication/windowsauth)
- 連接埠共用
- SNI 使用 HTTPS
- HTTP/2 5060，TLS (Windows 10)
- 直接檔案傳輸
- 回應快取
- WebSockets (Windows 8)

支援的 Windows 版本：

- Windows 7 和 Windows Server 2008 R2 和更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>何時使用 WebListener

WebListener 可用於部署您要公開，直接向網際網路伺服器而不使用 IIS。

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

由於它建置在 Http.Sys，WebListener 不需要保護以對抗攻擊反向 proxy 伺服器。 Http.Sys 是成熟的技術，可保護免於許多種類的攻擊，並提供強固性、 安全性及功能完整的 web 伺服器的延展性。 為 HTTP 接聽程式在 Http.Sys 之上，IIS 本身的執行。 

WebListener 也是不錯的選擇內部部署，當您需要的功能，您無法使用 Kestrel 來取得它所提供的其中一個。

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>如何使用 WebListener

以下是針對主機作業系統和您的 ASP.NET Core 應用程式安裝工作的概觀。

### <a name="configure-windows-server"></a>設定 Windows Server

* 安裝新版的.NET 應用程式所需，例如[.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe)或.NET Framework 4.5.1。

* __'Asverify'__ 繫結至 WebListener，以及設定 SSL 憑證的 URL 前置詞

   如果您不要 __'asverify'__ URL 前置詞，在 Windows 中的，您必須以系統管理員權限執行您的應用程式。 唯一的例外是如果您將繫結使用 HTTP (不是 HTTPS) 連接埠號碼大於 1024; localhost在此情況下，系統管理員權限不需要的。

   如需詳細資訊，請參閱[如何 __'asverify'__ 前置詞及設定 SSL](#preregister-url-prefixes-and-configure-ssl)本文稍後。

* 開啟防火牆連接埠來允許流量到達 WebListener。

   您可以使用 netsh.exe 或[PowerShell cmdlet](https://technet.microsoft.com/library/jj554906)。

另外還有[Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。

### <a name="configure-your-aspnet-core-application"></a>設定 ASP.NET Core 應用程式

* 安裝 NuGet 套件[Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/)。 這樣也會安裝[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/)做為相依性。

* 呼叫`UseWebListener`上的擴充方法[WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)中您`Main`方法，並指定任何 WebListener[選項](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)和[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)需要如下列範例所示：

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* 設定 Url 和連接埠上接聽 

  根據預設 ASP.NET Core 繫結至`http://localhost:5000`。 若要設定 URL 前置詞和連接埠，您可以使用`UseURLs`擴充方法，`urls`命令列引數或 ASP.NET Core 組態系統。 如需詳細資訊，請參閱[主控](../../fundamentals/hosting.md)。

  網頁接聽程式會使用[Http.Sys 前置詞字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。 沒有前置詞字串格式需求 WebListener 特有的。

  > [!NOTE]
  > 請確定您指定在相同的前置詞字串`UseUrls`，您預先註冊的伺服器上。 

* 請確定您的應用程式未設定為執行 IIS 或 IIS Express。

  在 Visual Studio 中的預設啟動設定檔為 IIS Express。  若要執行為主控台應用程式的專案您必須手動變更選取的設定檔，如下列螢幕擷取畫面所示。

  ![選取主控台應用程式設定檔](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>如何使用 ASP.NET Core 之外 WebListener

* 安裝[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 封裝。

* [__'Asverify'__ 繫結至 WebListener，以及設定 SSL 憑證的 URL 前置詞](#preregister-url-prefixes-and-configure-ssl)以用於 ASP.NET Core 一樣。

另外還有[Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。


以下是示範 WebListener 使用 ASP.NET Core 之外的程式碼範例：

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a>__'Asverify'__ URL 前置詞及設定 SSL

IIS 和 WebListener 依賴接聽要求的基礎 Http.Sys 核心模式驅動程式，並執行初始處理。 在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。 不過，如果您使用 WebListener 您需要自行設定 Http.Sys。 也就是執行 netsh.exe 內建的工具。 

您必須使用 netsh.exe 的最常見的工作會保留 URL 首碼，並指派 SSL 憑證。

NetSh.exe 不是使用適用於初學者簡單工具。 下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最低限度：

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

下列範例會示範如何將 SSL 憑證指派：

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

以下是正式的參考文件：

* [Netsh 命令都會使用超文字傳輸通訊協定 (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix 字串](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

下列資源提供數種案例的詳細的指示。 參考的文件`HttpListener`同樣適用於`WebListener`，因為兩者都以 Http.Sys。

* [如何： 使用 SSL 憑證設定連接埠](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS 通訊-HttpListener 基礎代管與用戶端憑證](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)這是協力廠商架構部落格和相當老舊，但仍有有用的資訊。
* [如何： 逐步解說使用 HttpListener 或 Http 伺服器 unmanaged 程式碼 （c + +） 做為 SSL 簡單伺服器](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)這也是較舊的部落格包含有用資訊。
* [如何設定 SSL 使用.NET 核心 WebListener？](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

以下是一些可以比使用 netsh.exe 命令列的協力廠商工具。 這些不是所提供，或經由 Microsoft 背書。 這些工具執行系統管理員身分根據預設，由於 netsh.exe 本身需要系統管理員權限。

* [http.sys 管理員](http://httpsysmanager.codeplex.com/)清單提供 UI，並設定 SSL 憑證和選項保留的前置詞及憑證信任清單。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)可讓您列出或設定 SSL 憑證和 URL 前置詞。 UI 會更精簡比 http.sys 管理員，並公開更多組態選項，但否則它會提供類似的功能。 它無法建立新的憑證信任清單 (CTL)，但可以將現有的指派。

產生自我簽署的 SSL 憑證，Microsoft 提供的命令列工具： [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968)和 PowerShell 指令程式[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)。 另外還有第三方 UI 工具，可讓您更輕鬆地產生自我簽署的 SSL 憑證：

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [這篇文章的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener 原始程式碼](https://github.com/aspnet/HttpSysServer/)
* [裝載](../hosting.md)
