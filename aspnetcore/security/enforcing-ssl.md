---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 了解如何在 ASP.NET Core web 應用程式需要 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 8d48877153d6d75348e29299c669125904236de8
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692599"
---
# <a name="enforce-https-in-aspnet-core"></a>強制使用 ASP.NET Core 中的 HTTPS

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文件說明如何：

* 需要 HTTPS 進行的所有要求。
* 將所有 HTTP 要求重新都導向至 HTTPS。

沒有可用的 API 可以防止用戶端第一次要求中傳送機密資料。

> [!WARNING]
> 請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。 `RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。 API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。 此類用戶端可能會透過 HTTP 傳送資訊。 Web Api 應執行下列之一：
>
> * 不在 HTTP 上接聽。
> * 關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。

## <a name="require-https"></a>需要 HTTPS

::: moniker range=">= aspnetcore-2.1"

我們建議您的生產環境 ASP.NET Core web 應用程式呼叫：

* HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 將 HTTP 要求重新導向至 HTTPS。
* HSTS 中介軟體 ([UseHsts](#http-strict-transport-security-protocol-hsts)) 傳送給用戶端的 HTTP Strict Transport Security 通訊協定 (HSTS) 標頭。

> [!NOTE]
> 在反向 proxy 組態中部署的應用程式允許 proxy 處理連線安全性 (HTTPS)。 如果 proxy 也會處理 HTTPS 重新導向，則不需要使用 HTTPS 重新導向中介軟體。 如果 proxy 伺服器也會負責編寫 HSTS 標頭 (例如[原生 HSTS 支援 IIS 10.0 (1709) 或更新版本](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support))，HSTS 中介軟體不需要應用程式。 如需詳細資訊，請參閱 <<c0> [ 退出的 HTTPS/HSTS 專案建立](#opt-out-of-httpshsts-on-project-creation)。

### <a name="usehttpsredirection"></a>UseHttpsRedirection

下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

上述反白顯示的程式碼：

* 使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect))。
* 使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) 但覆寫`ASPNETCORE_HTTPS_PORT`環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)。

我們建議使用暫時重新導向，而不是永久重新導向。 連結快取，會在開發環境中造成不穩定的行為。 如果您想要傳送的永久重新導向狀態碼，在非開發環境中應用程式時，請參閱[設定在生產環境中的永久重新導向](#configure-permanent-redirects-in-production)一節。 我們建議您使用[HSTS](#http-strict-transport-security-protocol-hsts)來只保護資源的用戶端通知要求應傳送至 （只在生產環境） 中的應用程式。

### <a name="port-configuration"></a>連接埠組態

將不安全的要求重新導向至 HTTPS 連接埠必須適用於中介軟體。 如果沒有連接埠可用：

* 不會發生重新導向至 HTTPS。
* 中介軟體會記錄警告 「 無法判定重新導向的 https 連接埠。 」

指定 HTTPS 連接埠，使用下列方法之一：

* 設定[HttpsRedirectionOptions.HttpsPort](#options)。
* 設定`ASPNETCORE_HTTPS_PORT`環境變數或[https_port Web 主機組態設定](xref:fundamentals/host/web-host#https-port):

  **索引鍵**: `https_port`  
  **類型**：*string*  
  **預設**：未設定預設值。  
  **設定使用**：`UseSetting`  
  **環境變數**:`<PREFIX_>HTTPS_PORT` (前置詞`ASPNETCORE_`使用時[Web 主機](xref:fundamentals/host/web-host)。)

  設定時<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>在`Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* 表示具有安全的配置使用的連接埠`ASPNETCORE_URLS`環境變數。 環境變數設定伺服器。 中介軟體間接探索透過 HTTPS 連接埠<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>。 這個方法不適用於反向 proxy 的部署。
* 在開發中，請在中設定的 HTTPS URL *launchsettings.json*。 使用 IIS Express 時，請啟用 HTTPS。
* 設定向外公開 edge 部署的 HTTPS URL 端點[Kestrel](xref:fundamentals/servers/kestrel)伺服器或[HTTP.sys](xref:fundamentals/servers/httpsys)伺服器。 只有**一個 HTTPS 連接埠**應用程式使用。 中介軟體會探索透過連接埠<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>。

> [!NOTE]
> 在反向 proxy 組態中，執行應用程式時<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>無法使用。 使用本節中所述的其他方法的其中一個連接埠設定。

使用 Kestrel 或 HTTP.sys 時做為向外公開邊緣伺服器，Kestrel 或 HTTP.sys 必須設定為接聽兩者：

* 會在重新導向用戶端的安全連接埠 (通常，在生產環境和開發 5001 443)。
* 不安全的連接埠 (通常，在生產環境中為 80) 與開發中的 5000。

為了讓應用程式用戶端收到不安全的要求，並重新導向至安全的連接埠的用戶端必須能夠使用不安全的連接埠。

如需詳細資訊，請參閱 < [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)或<xref:fundamentals/servers/httpsys>。

### <a name="deployment-scenarios"></a>部署案例

用戶端與伺服器之間的任何防火牆也必須開啟流量的通訊連接埠。

如果要求轉送的反向 proxy 設定，使用[轉送標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)之前呼叫 HTTPS 重新導向中介軟體。 轉送標頭中介軟體更新`Request.Scheme`，並使用`X-Forwarded-Proto`標頭。 中介軟體允許重新導向 Uri 和其他安全性原則才能正常運作。 轉送標頭中介軟體不使用時後, 端應用程式可能不會收到正確的配置和得到的重新導向迴圈。 常見的使用者錯誤訊息是發生太多的重新導向。

部署至 Azure App Service 時，請依照下列中的指導方針[教學課程：將現有的自訂 SSL 憑證繫結至 Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl)。

### <a name="options"></a>選項

下列醒目提示程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

呼叫`AddHttpsRedirection`時，才需要變更的值`HttpsPort`或`RedirectStatusCode`。

上述反白顯示的程式碼：

* 設定組[HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*)到<xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>，這是預設值。 使用的欄位<xref:Microsoft.AspNetCore.Http.StatusCodes>類別的工作分派`RedirectStatusCode`。
* 設定 HTTPS 連接埠為 5001。 預設值為 443。

#### <a name="configure-permanent-redirects-in-production"></a>在生產環境中設定永久重新導向

中介軟體會預設為傳送[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)與所有重新導向。 如果您想要傳送的永久重新導向狀態碼，在非開發環境中應用程式時，包裝中介軟體選項的組態中的非開發環境的條件式檢查。

設定時`IWebHostBuilder`中*Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>HTTPS 重新導向中介軟體的替代方法

除了使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。 `AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向為執行時。 如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。

當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。

::: moniker-end

::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)用來要求 HTTPS。 `[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。 若要全域套用的屬性，新增下列程式碼`ConfigureServices`在`Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

上述反白顯示的程式碼會要求所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。 而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。 中介軟體也允許應用程式執行時重新導向設定的狀態碼或狀態碼和連接埠。

全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。 將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。 您無法保證`[RequireHttps]`新增新的控制器和 Razor 頁面時，屬性會套用。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP Strict Transport 安全性通訊協定 (HSTS)

每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過回應標頭使用的 web 應用程式所指定的選擇加入的安全性增強功能。 當[瀏覽器支援 HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)收到此標頭：

* 瀏覽器會儲存可防止傳送的任何通訊透過 HTTP 的定義域的組態。 瀏覽器會強制透過 HTTPS 的所有通訊。
* 瀏覽器會防止使用者使用不受信任或不正確的憑證。 瀏覽器會停用允許使用者暫時信任此種憑證的提示。

因為 HSTS 會強制執行用戶端就會有一些限制：

* 用戶端必須支援 HSTS。
* HSTS 需要至少一個成功的 HTTPS 要求建立 HSTS 原則。
* 應用程式必須檢查每個 HTTP 要求並重新導向或拒絕的 HTTP 要求。

ASP.NET Core 2.1 或更新版本會實作 HSTS 與`UseHsts`擴充方法。 下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` 不建議在開發過程中因為 HSTS 設定高度快取瀏覽器。 根據預設，`UseHsts`排除本機回送位址。

生產環境中實作 HTTPS 第一次，設定初始[HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)小的值，使用其中一種<xref:System.TimeSpan>方法。 從設定值時數不超過一天的萬一您需要還原為 HTTP，HTTPS 基礎結構。 確定在 HTTPS 設定的持續性後，增加 HSTS 最大壽命值;常用的值為一年。

下列程式碼範例：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* 設定 Strict 傳輸安全性標頭的預先載入的參數。 預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。 請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。
* 可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。
* 明確設定為 60 天的 Strict 傳輸安全性標頭的最大壽命參數。 如果未設定，預設值為 30 天。 請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。
* 新增`example.com`的主機，以排除清單。

`UseHsts` 排除下列 「 回送 」 主控件：

* `localhost`：IPv4 回送位址。
* `127.0.0.1`：IPv4 回送位址。
* `[::1]`：IPv6 回送位址。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>選擇退出的 HTTPS/HSTS 專案建立

在某些後端服務案例中公開邊緣的網路處理連線安全性的位置，設定每個節點的連線安全性並非必要條件。 Web 應用程式從 Visual Studio 中，或從範本產生[dotnet 新](/dotnet/core/tools/dotnet-new)命令啟用[HTTPS 重新導向](#require-https)並[HSTS](#http-strict-transport-security-protocol-hsts)。 對於不需要這些案例的部署，您可以選擇退出的 HTTPS/HSTS 從範本建立應用程式。

若要退出 HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

取消核取**設定為使用 HTTPS**核取方塊。

![顯示 HTTPS 核取方塊取消選取 [設定新的 ASP.NET Core Web 應用程式] 對話方塊。](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

使用 `--no-https` 選項。 例如

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>信任 ASP.NET Core HTTPS 開發憑證，在 Windows 和 macOS 上

.NET core SDK 包含 HTTPS 開發憑證。 憑證會安裝為初次執行體驗的一部分。 比方說，`dotnet --info`會產生類似下列輸出：

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

安裝 .NET Core SDK 會將 ASP.NET Core HTTPS 開發憑證安裝至本機使用者憑證存放區。 已安裝的憑證，但不是受信任。 若要信任的憑證執行一次性的步驟，以執行 dotnet`dev-certs`工具：

```console
dotnet dev-certs https --trust
```

下列命令會提供 `dev-certs` 工具的說明：

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>如何設定適用於 Docker 的開發人員憑證

請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6199)。

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>信任適用於 Linux 的 Windows 子系統的 HTTPS 憑證

Windows for Linux 子系統 (WSL) 會產生 HTTPS 的自我簽署的憑證。若要設定信任 WSL 憑證的 Windows 憑證存放區：

* 執行下列命令以匯出 WSL 產生憑證： `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* 在 WSL 視窗中，執行下列命令： `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  上述命令會設定環境變數，因此 Linux 會使用 Windows 信任的憑證。

## <a name="additional-information"></a>其他資訊

* <xref:host-and-deploy/proxy-load-balancer>
* [Linux 上使用 Apache 裝載 ASP.NET Core:HTTPS 設定](xref:host-and-deploy/linux-apache#https-configuration)
* [Linux 上使用 Nginx 裝載 ASP.NET Core:HTTPS 設定](xref:host-and-deploy/linux-nginx#https-configuration)
* [如何在 IIS 上設定 SSL](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [OWASP HSTS 瀏覽器支援](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
