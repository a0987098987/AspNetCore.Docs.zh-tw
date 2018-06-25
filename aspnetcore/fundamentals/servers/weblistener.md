---
title: ASP.NET Core 中的 WebListener 網頁伺服器實作
author: rick-anderson
description: 了解 WebListener，這是 Windows 上 ASP.NET Core 的網頁伺服器，可用於直接連線到網際網路而無需 IIS。
ms.author: riande
ms.date: 03/13/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 68aea99d6ce6af12655ef5fdb13130e9279e448a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274865"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 WebListener 網頁伺服器實作

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> 本主題只適用於 ASP.NET Core 1.x。 在 ASP.NET Core 2.0 中，WebListener 名為 [HTTP.sys](httpsys.md)。

WebListener 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](index.md)。 它建置在 [Http.Sys 核心模式驅動程式](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)之上。 WebListener 是 [Kestrel](kestrel.md) 的替代方式，可以用於直接連線到網際網路，而不需依賴 IIS 作為反向 Proxy 伺服器。 事實上，**WebListener 不能與 IIS 或 IIS Express 搭配使用，因為它與 [ASP.NET Core 模組](aspnet-core-module.md)不相容。**

雖然 WebListener 是針對 ASP.NET Core 所開發，但它可以透過 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 套件直接用於任何 .NET Core 或 .NET Framework 應用程式。

WebListener 支援下列功能：

- [Windows 驗證](xref:security/authentication/windowsauth)
- 連接埠共用
- 使用 SNI 的 HTTPS
- HTTP/2 over TLS (Windows 10)
- 直接檔案傳輸
- 回應快取
- WebSocket (Windows 8)

支援的 Windows 版本：

- Windows 7 與 Windows Server 2008 R2 和更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>WebListener 使用時機

WebListener 可用於您需要直接向網際網路公開伺服器而不使用 IIS 的部署。

![Weblistener 直接與網際網路通訊](weblistener/_static/weblistener-to-internet.png)

由於它建置在 Http.Sys 之上，WebListener 不需要反向 Proxy 伺服器以抵禦攻擊。 Http.Sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。 IIS 本身在 Http.Sys 之上以 HTTP 接聽程式的形式執行。 

當您需要它所提供的其中一個功能無法使用 Kestrel 來取得時，WebListener 也是不錯的內部部署選擇。

![Weblistener 直接與內部網路通訊](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>WebListener 使用方法

以下是針對主機作業系統和您的 ASP.NET Core 應用程式的安裝工作概觀。

### <a name="configure-windows-server"></a>設定 Windows Server

* 安裝您應用程式所需的 .NET 版本，例如 [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) 或 .NET Framework 4.5.1。

* 預先註冊 URL 前置詞以繫結至 WebListener，然後設定 SSL 憑證

   如果您不在 Windows 預先註冊 URL 前置詞，則必須以系統管理員權限執行您的應用程式。 唯一的例外是如果您使用 HTTP (不是 HTTPS) 繫結至 localhost，且使用大於 1024 的連接埠號碼；在此情況下不需要系統管理員權限。

   如需詳細資料，請參閱本文稍後的[如何預先註冊前置詞和設定 SSL](#preregister-url-prefixes-and-configure-ssl)。

* 開啟防火牆連接埠來允許流量到達 WebListener。

   您可以使用 netsh.exe 或 [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906)。

另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。

### <a name="configure-your-aspnet-core-application"></a>設定 ASP.NET Core 應用程式

* 安裝 [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/) NuGet 套件。 這也會安裝 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) 作為相依性。

* 在您的 `Main` 方法中，對 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 呼叫 `UseWebListener` 擴充方法，並指定需要的任何 WebListener [選項](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)和[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)，如下列範例所示：

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* 設定要接聽的 URL 和連接埠 

  ASP.NET Core 預設會繫結至 `http://localhost:5000`。 若要設定 URL 前置詞和連接埠，您可以使用 `UseURLs` 擴充方法、`urls` 命令列引數或 ASP.NET Core 組態系統。 如需詳細資訊，請參閱 [在 ASP.NET Core 中代管(xref:fundamentals/host/index)。

  網頁接聽程式會使用 [Http.Sys 前置詞字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。 沒有 WebListener 特有的前置詞字串格式需求。

  > [!WARNING]
  > 請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。 最上層萬用字元繫結可能暴露您的應用程式安全性弱點。 這對強式與弱式萬用字元皆適用。 請使用明確主機名稱，而非萬用字元。 若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。 如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。

  > [!NOTE]
  > 請確定您在 `UseUrls` 中指定您於伺服器上預先註冊的相同前置詞字串。 

* 請確定您的應用程式未設定為執行 IIS 或 IIS Express。

  在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。  若要執行專案作為主控台應用程式，您必須手動變更選取的設定檔，如下列螢幕擷取畫面所示。

  ![選取主控台應用程式設定檔](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>如何在 ASP.NET Core 之外使用 WebListener

* 安裝 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet 套件。

* [預先註冊 URL 前置詞以繫結至 WebListener，然後設定 SSL 憑證](#preregister-url-prefixes-and-configure-ssl)，如同用於 ASP.NET Core 一樣。

另外還有 [Http.Sys 登錄設定](https://support.microsoft.com/kb/820129)。


以下是示範在 ASP.NET Core 之外使用 WebListener 的程式碼範例：

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>預先註冊 URL 前置詞和設定 SSL

IIS 和 WebListener 都依賴基礎 Http.Sys 核心模式驅動程式來接聽要求，以及執行初始處理。 在 IIS 中，管理 UI 可讓您很輕鬆地設定所有項目。 不過，如果您要使用 WebListener，則必須自行設定 Http.Sys。 這麼做的內建工具是 netsh.exe。 

您必須使用 netsh.exe 的最常見工作是保留 URL 前置詞和指派 SSL 憑證。

NetSh.exe 不是適用於初學者的簡單工具。 下列範例會示範保留連接埠 80 和 443 的 URL 前置詞時所需的最小值：

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

下列範例會示範如何指派 SSL 憑證：

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

以下是正式參考文件：

* [超文字傳輸通訊協定 (HTTP) 的 Netsh 命令](https://technet.microsoft.com/library/cc725882.aspx)
* [UrlPrefix 字串](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

下列資源提供數種案例的詳細指示。 提到 `HttpListener` 的文章同樣適用於 `WebListener`，因為兩者都是根據 Http.Sys。

* [如何：使用 SSL 憑證設定連接埠](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通訊 - 以 HttpListener 為基礎的裝載和用戶端憑證) 這是協力廠商部落格，相當老舊但仍有有用的資訊。
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (如何：逐步解說使用 HttpListener 或 Http Server unmanaged 程式碼 (C++) 作為 SSL 簡單伺服器) 這也是具有有用資訊的較舊部落格。
* [How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (如何使用 SSL 設定 .NET Core WebListener？)

以下是一些協力廠商工具，比起 netsh.exe 命令列可以更輕鬆地使用。 這些不是由 Microsoft 提供，或經由 Microsoft 背書。 工具預設會以系統管理員身分執行，因為 netsh.exe 本身需要系統管理員權限。

* [http.sys Manager](http://httpsysmanager.codeplex.com/) 提供 UI 以便列出和設定 SSL 憑證與選項、前置詞保留，以及憑證信任清單。 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) 可讓您列出或設定 SSL 憑證與 URL 前置詞。 UI 比 http.sys Manager 更精簡，並公開更多組態選項，但在其他方面提供類似的功能。 它無法建立新的憑證信任清單 (CTL)，但可以指派現有的憑證信任清單。

為了產生自我簽署的 SSL 憑證，Microsoft 提供命令列工具：[MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) 和 PowerShell Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)。 另外還有協力廠商 UI 工具，可讓您更輕鬆地產生自我簽署的 SSL 憑證：

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Makecert UI](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* [本文的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [WebListener 原始程式碼](https://github.com/aspnet/HttpSysServer/)
* [裝載](xref:fundamentals/host/index)
