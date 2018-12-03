---
title: ASP.NET Core 中的 HTTP.sys 網頁伺服器實作
author: guardrex
description: 深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f5ab1a3cbd1020a5ab2bd64a81b5782fd116f069
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450641"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 HTTP.sys 網頁伺服器實作

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)

> [!NOTE]
> 本主題適用於 ASP.NET Core 2.0 或更新版本。 在舊版的 ASP.NET Core 中，HTTP.sys 名為 [WebListener](xref:fundamentals/servers/weblistener)。

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。 HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 的替代方式，提供了一些 Kestrel 未提供的功能。

> [!IMPORTANT]
> HTTP.sys 與 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。

HTTP.sys 支援下列功能：

* [Windows 驗證](xref:security/authentication/windowsauth)
* 連接埠共用
* 使用 SNI 的 HTTPS
* 透過 TLS 的 HTTP/2 (Windows 10 或更新版本)
* 直接檔案傳輸
* 回應快取
* WebSocket (Windows 8 或更新版本)

支援的 Windows 版本：

* Windows 7 或更新版本
* Windows Server 2008 R2 或更新版本

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>使用 HTTP.sys 的時機

HTTP.sys 在下列部署環境中非常有用：

* 需要直接向網際網路公開伺服器而不使用 IIS。

  ![HTTP.sys 直接與網際網路通訊](httpsys/_static/httpsys-to-internet.png)

* 內部部署需要的功能無法在 Kestrel 中使用，例如 [Windows 驗證](xref:security/authentication/windowsauth)。

  ![HTTP.sys 直接與內部網路通訊](httpsys/_static/httpsys-to-internal.png)

HTTP.sys 是成熟的技術，可抵禦許多種類的攻擊，並提供功能完整之網頁伺服器的穩固性、安全性及延展性。 IIS 本身在 HTTP.sys 之上以 HTTP 接聽程式的形式執行。

## <a name="http2-support"></a>HTTP/2 支援

如果符合下列基本需求，則可以針對 ASP.NET Core 應用程式啟用 [HTTP/2](https://httpwg.org/specs/rfc7540.html)：

* Windows Server 2016/Windows 10 或更新版本
* [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 連線
* TLS 1.2 或更新版本連線

::: moniker range=">= aspnetcore-2.2"

如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/2`。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

如果已建立 HTTP/2 連線，[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) 會報告 `HTTP/1.1`。

::: moniker-end

HTTP/2 預設為啟用。 如果 HTTP/2 連線尚未建立，連線會退為 HTTP/1.1。 Windows 的未來版本會推出 HTTP/2 設定旗標，包括使用 HTTP.sys 來停用 HTTP/2 的功能。

## <a name="kernel-mode-authentication-with-kerberos"></a>使用 Kerberos 的核心模式驗證

HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。 Kerberos 和 HTTP.sys 不支援使用者模式驗證。 必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。 請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。

## <a name="how-to-use-httpsys"></a>如何使用 HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>設定 ASP.NET Core 應用程式使用 HTTP.sys

1. 使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) (ASP.NET Core 2.1 或更新版本) 時，專案檔中不需要套件參考。 若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。

2. 建置 Web 主機時，呼叫 [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 擴充方法，並指定任何必要的 [HTTP.sys 選項](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)：

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。

   **HTTP.sys 選項**

   | 屬性 | 描述 | 預設 |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | 控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。 | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | 允許匿名要求。 | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | 指定允許的驗證配置。 處置接聽程式之前可隨時修改。 值是由 [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。 | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | 針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。 回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。 它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。 | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | 可同時接受的數目上限。 | 5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | 可接受的同時連線數量上限。 使用 `-1` 為無限多個。 使用 `null` 以使用登錄之整個電腦的設定。 | `null`<br>(無限制) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | 請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。 | 30000000 位元組<br>(~28.6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | 可以加入佇列的最大要求數目。 | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | 指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。 | `false`<br>(正常完成) |
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | 公開 HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 設定，這也可在登錄中設定。 API 連結可提供包括預設值在內每個設定的詳細資訊：<ul><li>[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</li><li>[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; 要求實體內容到達的允許時間。</li><li>[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</li><li>[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; 允許連線閒置的時間。</li><li>[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; 回應的最低傳送速率。</li><li>[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | 指定 [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) 以向 HTTP.sys 註冊。 最實用的是 [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add)，可用來將前置詞加入集合。 處置接聽程式之前可隨時修改這些內容。 |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   任何要求所允許的大小上限 (以位元組為單位)。 當設定為 `null` 時，要求主體大小上限為無限制。 此限制對升級連線不會有任何影響，因為其一律為無限制。

   在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方法，是在動作方法上使用 [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 屬性：

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。 `IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。

   如果應用程式應針對每個要求覆寫 [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize)，則使用 [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature)：

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. 如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。

   在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。 若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：

   ![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>設定 Windows Server

1. 如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。

   * **.NET Core** &ndash; 如果應用程式需要 .NET Core，請從 [.NET All Downloads](https://www.microsoft.com/net/download/all) (.NET 所有下載) 取得並執行 .NET Core 安裝程式。
   * **.NET Framework** &ndash; 如果應用程式要求 .NET Framework，請參閱 [.NET Framework：安裝指南](/dotnet/framework/install/)以尋找安裝指示。 安裝必要的 .NET Framework。 您可在 [.NET All Downloads](https://www.microsoft.com/net/download/all) (.NET 所有下載) 找到最新的 .NET Framework 安裝程式。

2. 設定要應用程式的 URL 和連接埠。

   ASP.NET Core 預設會繫結至 `http://localhost:5000`。 若要設定 URL 前置詞和連接埠，選項包括使用：

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * `urls` 命令列引數
   * `ASPNETCORE_URLS` 環境變數
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   下列程式碼範例示範如何使用 [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)：

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=11)]

   `UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。

   `UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。 因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。 如需 `UseUrls`、`urls` 和 `ASPNETCORE_URLS` 的詳細資訊，請參閱[在 ASP.NET Core 中代管](xref:fundamentals/host/index)主題。

   HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。

   > [!WARNING]
   > 請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。 最上層萬用字元繫結可能暴露您的應用程式安全性弱點。 這對強式與弱式萬用字元皆適用。 請使用明確主機名稱，而非萬用字元。 若您擁有整個父網域 (與具弱點的 `*.com` 相對) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 就沒有此安全性風險。 如需詳細資訊，請參閱 [rfc7230 5.4 節](https://tools.ietf.org/html/rfc7230#section-5.4)。

3. 預先註冊 URL 前置詞以繫結至 HTTP.sys，然後設定 x.509 憑證。

   如果 URL 前置詞並未在 Windows 中預先註冊，請以系統管理員權限執行應用程式。 唯一的例外狀況是使用 HTTP (不是 HTTPS) 透過大於 1024 的連接埠號碼繫結至 localhost。 在此情況下，不需要系統管理員權限。

   1. 用來設定 HTTP.sys 的內建工具是 *netsh.exe*。 *netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。 此工具需要系統管理員權限。

      下列範例示範保留連接埠 80 和 443 的 URL 前置詞的命令：

      ```console
      netsh http add urlacl url=http://+:80/ user=Users
      netsh http add urlacl url=https://+:443/ user=Users
      ```

      下列範例會示範如何指派 X.509 憑證：

      ```console
      netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}"
      ```

      以下是 *netsh.exe* 的參考文件：

      * [超文字傳輸通訊協定 (HTTP) 的 Netsh 命令](https://technet.microsoft.com/library/cc725882.aspx)
      * [UrlPrefix 字串](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

   2. 如有需要，可建立自我簽署的 X.509 憑證。

      [!INCLUDE [How to make an X.509 cert](../../includes/make-x509-cert.md)]

4. 開啟防火牆連接埠來允許流量到達 HTTP.sys。 使用 *netsh.exe* 或 [PowerShell Cmdlets](https://technet.microsoft.com/library/jj554906)。

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="additional-resources"></a>其他資源

* [HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)
* [aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
