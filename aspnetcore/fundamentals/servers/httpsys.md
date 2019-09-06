---
title: ASP.NET Core 中的 HTTP.sys 網頁伺服器實作
author: guardrex
description: 深入了解 HTTP.sys，這是 Windows 上的 ASP.NET Core 網頁伺服器。 HTTP.sys 建置在 HTTP.sys 核心模式驅動程式之上，是 Kestrel 的替代方式，可以用來直接連線到網際網路而不使用 IIS。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: b9adbdd83b3c4e1eeaadcf99fa3ee6cb41f67f8e
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310512"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>ASP.NET Core 中的 HTTP.sys 網頁伺服器實作

作者：[Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher) 和 [Luke Latham](https://github.com/guardrex)

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) 是只在 Windows 上執行的 [ASP.NET Core 網頁伺服器](xref:fundamentals/servers/index)。 HTTP.sys 是 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器的替代方式，提供了一些 Kestrel 未提供的功能。

> [!IMPORTANT]
> HTTP.sys 與 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)不相容，且不能與 IIS 或 IIS Express 搭配使用。

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

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

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

::: moniker range="< aspnetcore-3.0"

使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)（[nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)）時，不需要專案檔中的套件參考。 若不是使用 `Microsoft.AspNetCore.App` 中繼套件，請將套件參考加入 [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)。

::: moniker-end

建置主機時，呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 擴充方法，並指定任何必要的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions>。 下列範例會將選項設定為它們的預設值：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Program.cs?name=snippet1&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Program.cs?name=snippet1&highlight=4-12)]

::: moniker-end

其他的 HTTP.sys 設定則透過[登錄設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)處理。

   **HTTP.sys 選項**

::: moniker range=">= aspnetcore-3.0"

| 屬性 | 說明 | 預設 |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | 控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。 | `false` |
| [Authentication.AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | 允許匿名要求。 | `true` |
| [Authentication.Schemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | 指定允許的驗證配置。 處置接聽程式之前可隨時修改。 值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。 | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | 針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。 回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。 它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。 | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | 可同時接受的數目上限。 | 5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | 可接受的同時連線數量上限。 使用 `-1` 為無限多個。 使用 `null` 以使用登錄之整個電腦的設定。 | `null`<br>(無限制) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | 請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。 | 30000000 位元組<br>(~28.6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | 可以加入佇列的最大要求數目。 | 1000 |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | 指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。 | `false`<br>(正常完成) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | 公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。 API 連結可提供包括預設值在內每個設定的詳細資訊：<ul><li>[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</li><li>[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; 要求實體內容到達的允許時間。</li><li>[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</li><li>[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; 允許連線閒置的時間。</li><li>[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; 回應的最低傳送速率。</li><li>[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | 指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。 最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。 處置接聽程式之前可隨時修改這些內容。 |  |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| 屬性 | 說明 | 預設 |
| -------- | ----------- | :-----: |
| [AllowSynchronousIO](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.AllowSynchronousIO) | 控制是否允許 `HttpContext.Request.Body` 和 `HttpContext.Response.Body` 同步輸出/輸入。 | `true` |
| [Authentication.AllowAnonymous](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.AllowAnonymous) | 允許匿名要求。 | `true` |
| [Authentication.Schemes](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationManager.Schemes) | 指定允許的驗證配置。 處置接聽程式之前可隨時修改。 值是由 [AuthenticationSchemes enum](xref:Microsoft.AspNetCore.Server.HttpSys.AuthenticationSchemes) 提供：`Basic`、`Kerberos`、`Negotiate`、`None` 和 `NTLM`。 | `None` |
| [EnableResponseCaching](xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.EnableResponseCaching) | 針對含有合格標頭的回應嘗試[核心模式](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)快取。 回應可能不包含 `Set-Cookie`、`Vary` 或 `Pragma` 標頭。 它必須包含為 `public` 的 `Cache-Control` 標頭，且有 `shared-max-age` 或 `max-age` 值，或是 `Expires` 標頭。 | `true` |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxAccepts> | 可同時接受的數目上限。 | 5 &times; [Environment.<br>ProcessorCount](xref:System.Environment.ProcessorCount) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxConnections> | 可接受的同時連線數量上限。 使用 `-1` 為無限多個。 使用 `null` 以使用登錄之整個電腦的設定。 | `null`<br>(無限制) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize> | 請參閱 <a href="#maxrequestbodysize">MaxRequestBodySize</a> 小節。 | 30000000 位元組<br>(~28.6 MB) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.RequestQueueLimit> | 可以加入佇列的最大要求數目。 | 1000 |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ThrowWriteExceptions> | 指出若回應本文因為用戶端中斷連線而寫入失敗時，應擲回例外狀況或正常完成。 | `false`<br>(正常完成) |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.Timeouts> | 公開 HTTP.sys <xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager> 設定，這也可在登錄中設定。 API 連結可提供包括預設值在內每個設定的詳細資訊：<ul><li>[TimeoutManager.DrainEntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.DrainEntityBody) &ndash; HTTP 伺服器 API 清空持續連線上實體內容的允許時間。</li><li>[TimeoutManager.EntityBody](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.EntityBody) &ndash; 要求實體內容到達的允許時間。</li><li>[TimeoutManager.HeaderWait](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.HeaderWait) &ndash; HTTP 伺服器 API 剖析要求標頭的允許時間。</li><li>[TimeoutManager.IdleConnection](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.IdleConnection) &ndash; 允許連線閒置的時間。</li><li>[TimeoutManager.MinSendBytesPerSecond](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.MinSendBytesPerSecond) &ndash; 回應的最低傳送速率。</li><li>[TimeoutManager.RequestQueue](xref:Microsoft.AspNetCore.Server.HttpSys.TimeoutManager.RequestQueue) &ndash; 在應用程式 擷取要求之前，將要求保留於要求佇列中的允許時間。</li></ul> |  |
| <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> | 指定 <xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection> 以向 HTTP.sys 註冊。 最實用的是 [UrlPrefixCollection.Add](xref:Microsoft.AspNetCore.Server.HttpSys.UrlPrefixCollection.Add*)，可用來將前置詞加入集合。 處置接聽程式之前可隨時修改這些內容。 |  |

::: moniker-end

<a name="maxrequestbodysize"></a>

**MaxRequestBodySize**

任何要求所允許的大小上限 (以位元組為單位)。 當設定為 `null` 時，要求主體大小上限為無限制。 此限制對升級連線不會有任何影響，因為其一律為無限制。

在 ASP.NET Core MVC 應用程式中針對單一 `IActionResult` 覆寫限制的建議方式，是在動作方法上使用 <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> 屬性：

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

如果應用程式已開始讀取要求之後，才嘗試設定要求的限制，則會擲回例外狀況。 `IsReadOnly` 屬性可用來指出 `MaxRequestBodySize` 屬性是否處於唯讀狀態，代表要設定限制已經太遲。

如果應用程式應該覆寫每個要求的 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.MaxRequestBodySize>，請使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpMaxRequestBodySizeFeature>：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](httpsys/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](httpsys/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6-7)]

::: moniker-end

如果您使用 Visual Studio，請確定應用程式未設定為執行 IIS 或 IIS Express。

在 Visual Studio 中，預設啟動設定檔適用於 IIS Express。 若要執行專案作為主控台應用程式，請手動變更選取的設定檔，如下列螢幕擷取畫面所示：

![選取主控台應用程式設定檔](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>設定 Windows Server

1. 判斷要為應用程式開啟的連接埠，然後使用 [Windows 防火牆](/windows/security/threat-protection/windows-firewall/create-an-inbound-port-rule)或 [New-NetFirewallRule](/powershell/module/netsecurity/new-netfirewallrule) PowerShell Cmdlet 來開啟防火牆連接埠，以允許流量到達 HTTP.sys。 在下列命令和應用程式設定中，會使用連接埠 443。

1. 部署至 Azure VM 時，請在[網路安全性群組](/azure/virtual-machines/windows/nsg-quickstart-portal)中開啟連接埠。 在下列命令和應用程式設定中，會使用連接埠 443。

1. 視需要取得並安裝 X.509 憑證。

   在 Windows 上，請使用 [New-SelfSignedCertificate PowerShell Cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate) 來建立自我簽署的憑證。 如需不支援的範例，請參閱 [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) \(英文\)。

   將自我簽署的憑證或 CA 簽署的憑證安裝在伺服器的 [本機電腦] > 個人 存放區中。

1. 如果應用程式是[與架構相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)，請安裝 .NET Core、.NET Framework 或兩者 (如果應用程式是以 .NET Framework 為目標的 .NET Core 應用程式)。

   * **.NET Core** &ndash; 如果應用程式需要 .NET Core，請從 [.NET Core 下載](https://dotnet.microsoft.com/download)取得並執行 **.NET Core 執行階段**安裝程式。 請勿在伺服器上安裝完整的 SDK。
   * **.NET Framework** &ndash; 如果應用程式需要 .NET Framework，請參閱 [.NET Framework 安裝指南](/dotnet/framework/install/)。 安裝必要的 .NET Framework。 您可以從 [.NET Core 下載](https://dotnet.microsoft.com/download)頁面取得最新 .NET Framework 的安裝程式。

   如果應用程式是[自封式部署](/dotnet/core/deploying/#self-contained-deployments-scd)，則應用程式的部署中會包含執行階段。 不需要在伺服器上安裝任何架構。

1. 設定應用程式中的 URL 和連接埠。

   ASP.NET Core 預設會繫結至 `http://localhost:5000`。 若要設定 URL 首碼和連接埠，選項包括：

   * <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
   * `urls` 命令列引數
   * `ASPNETCORE_URLS` 環境變數
   * <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes>

   下列程式碼範例示範如何使用 <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.UrlPrefixes> 搭配位於連接埠 443 的伺服器本機 IP 位址 `10.0.0.4`：

::: moniker range=">= aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/3.x/Program.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

   [!code-csharp[](httpsys/samples_snapshot/2.x/Program.cs?highlight=6)]

::: moniker-end

   `UrlPrefixes` 的優點是針對錯誤格式的前置詞會立即產生錯誤訊息。

   `UrlPrefixes` 中的設定會覆寫 `UseUrls`/`urls`/`ASPNETCORE_URLS` 設定。 因此，`UseUrls`、`urls` 和 `ASPNETCORE_URLS` 環境變數的優點，是能更輕鬆地在 Kestrel 和 HTTP.sys 之間切換。

   HTTP.sys 使用 [HTTP Server API UrlPrefix 字串格式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)。

   > [!WARNING]
   > 請**勿**使用最上層萬用字元繫結 (`http://*:80/`與 `http://+:80`)。 最上層萬用字元繫結會導致應用程式安全性弱點。 這對強式與弱式萬用字元皆適用。 請使用明確的主機名稱或 IP 位址，而不要使用萬用字元。 若您擁有整個父網域 (相對於有弱點的 `*.com`) 的控制權，則子網域萬用字元繫結 (例如 `*.mysub.com`) 便不構成安全性風險。 如需詳細資訊，請參閱[RFC 7230：5.4 節：主機](https://tools.ietf.org/html/rfc7230#section-5.4) \(英文\)。

1. 在伺服器上預先註冊 URL 首碼。

   用來設定 HTTP.sys 的內建工具是 *netsh.exe*。 *netsh.exe* 是用來保留 URL 前置詞，並指派 X.509 憑證。 此工具需要系統管理員權限。

   使用 *netsh.exe* 工具來為應用程式註冊 URL：

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * `<URL>` &ndash; 完整的「統一資源定位器」(URL)。 請勿使用萬用字元繫結。 請使用有效的主機名稱或本機 IP 位址。 URL 必須包含結尾斜線。
   * `<USER>` &ndash; 會指定使用者或「使用者-群組」名稱。

   在以下範例中，伺服器的本機 IP 位址是 `10.0.0.4`：

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   已註冊 URL 時，此工具會以 `URL reservation successfully added` 回應。

   若要刪除已註冊的 URL，請使用 `delete urlacl` 命令：

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. 在伺服器上註冊 X.509 憑證。

   使用 *netsh.exe* 工具來為應用程式註冊憑證：

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * `<IP>` &ndash; 會指定繫結的本機 IP 位址。 請勿使用萬用字元繫結。 請使用有效的 IP 位址。
   * `<PORT>` &ndash; 會指定繫結的連接埠。
   * `<THUMBPRINT>` &ndash; X.509 憑證指紋。
   * `<GUID>` &ndash; 開發人員所產生來代表應用程式的 GUID，供參考使用。

   為了便於參考，請將 GUID 以套件標記的形式儲存在應用程式中：

   * 在 Visual Studio 中：
     * 在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [屬性]，以開啟應用程式的專案屬性。
     * 選取 [套件] 索引標籤。
     * 輸入您在 [標記] 欄位中建立的 GUID。
   * 不是使用 Visual Studio 時：
     * 開啟應用程式的專案檔。
     * 將 `<PackageTags>` 屬性搭配您所建立的 GUID 新增至新的或現有的 `<PropertyGroup>`：

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   在以下範例中：

   * 伺服器的本機 IP 位址是 `10.0.0.4`。
   * 線上隨機 GUID 產生器會提供 `appid` 值。

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   已註冊憑證時，此工具會以 `SSL Certificate successfully added` 回應。

   若要刪除憑證註冊，請使用 `delete sslcert` 命令：

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   以下是 *netsh.exe* 的參考文件：

   * [超文字傳輸通訊協定 (HTTP) 的 Netsh 命令](https://technet.microsoft.com/library/cc725882.aspx)
   * [UrlPrefix 字串](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. 執行應用程式。

   使用 HTTP (不是 HTTPS) 搭配大於 1024 的連接埠號碼來繫結至 localhost 時，不需要系統管理員權限即可執行應用程式。 針對其他設定 (例如，使用本機 IP 位址或繫結至連接埠 443)，請使用系統管理員權限來執行應用程式。

   應用程式會在伺服器的公用 IP 位址回應。 在此範例中，是從網際網路透過伺服器的公用 IP 位址 `104.214.79.47` 連線至伺服器。

   在此範例中使用的是開發憑證。 在略過瀏覽器的未受信任憑證警告之後，頁面會安全地載入。

   ![顯示已載入應用程式索引頁面的瀏覽器視窗](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxy 伺服器和負載平衡器案例

如果是 HTTP.sys 所裝載且與來自網際網路或公司網路的要求進行互動的應用程式，在裝載於 Proxy 伺服器和負載平衡器後方時，可能需要額外的組態。 如需詳細資訊，請參閱[設定 ASP.NET Core 以處理 Proxy 伺服器和負載平衡器](xref:host-and-deploy/proxy-load-balancer)。

## <a name="additional-resources"></a>其他資源

* [使用 HTTP.sys 來啟用 Windows 驗證](xref:security/authentication/windowsauth#httpsys) \(機器翻譯\)
* [HTTP 伺服器 API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx) \(英文\)
* [aspnet/HttpSysServer GitHub 存放庫 (原始程式碼)](https://github.com/aspnet/HttpSysServer/) \(英文\)
* [主機](xref:fundamentals/index#host)
* <xref:test/troubleshoot>
