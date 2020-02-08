---
title: 在 ASP.NET Core 中強制使用 HTTPS
author: rick-anderson
description: 瞭解如何在 ASP.NET Core web 應用程式中要求使用 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 59883a8165040fa58edb2f6cf22d4d6b3abf6f3e
ms.sourcegitcommit: 80286715afb93c4d13c931b008016d6086c0312b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074545"
---
# <a name="enforce-https-in-aspnet-core"></a>在 ASP.NET Core 中強制使用 HTTPS

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

本檔說明如何：

* 所有要求都需要 HTTPS。
* 將所有 HTTP 要求都重新導向至 HTTPS。

沒有 API 可防止用戶端在第一次要求時傳送敏感性資料。

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a>API 專案
>
> 請勿**在**接收機密資訊的 Web api 上使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 。 `RequireHttpsAttribute` 會使用 HTTP 狀態碼，將瀏覽器從 HTTP 重新導向至 HTTPS。 API 用戶端可能無法瞭解或遵循從 HTTP 到 HTTPS 的重新導向。 這類用戶端可能會透過 HTTP 傳送資訊。 Web Api 應為：
>
> * 不接聽 HTTP。
> * 關閉具有狀態碼400（不正確的要求）的連接，而不提供要求。
>
> ## <a name="hsts-and-api-projects"></a>HSTS 和 API 專案
>
> 預設 API 專案不包含[HSTS](#hsts) ，因為 HSTS 通常是瀏覽器唯一的指令。 其他呼叫端（例如電話或桌面應用程式）**不會**遵守指令。 即使在瀏覽器中，透過 HTTP 對 API 進行的單一驗證呼叫也會有不安全網路的風險。 安全的方法是將 API 專案設定為僅接聽並透過 HTTPS 回應。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a>API 專案
>
> 請勿**在**接收機密資訊的 Web api 上使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) 。 `RequireHttpsAttribute` 會使用 HTTP 狀態碼，將瀏覽器從 HTTP 重新導向至 HTTPS。 API 用戶端可能無法瞭解或遵循從 HTTP 到 HTTPS 的重新導向。 這類用戶端可能會透過 HTTP 傳送資訊。 Web Api 應為：
>
> * 不接聽 HTTP。
> * 關閉具有狀態碼400（不正確的要求）的連接，而不提供要求。

::: moniker-end

## <a name="require-https"></a>需要 HTTPS

我們建議生產 ASP.NET Core web 應用程式使用：

* 用來將 HTTP 要求重新導向至 HTTPS 的 HTTPS 重新導向中介軟體（<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>）。
* HSTS 中介軟體（[UseHsts](#http-strict-transport-security-protocol-hsts)），以將 HTTP 嚴格傳輸安全性通訊協定（HSTS）標頭傳送給用戶端。

> [!NOTE]
> 部署在反向 proxy 設定中的應用程式可讓 proxy 處理連線安全性（HTTPS）。 如果 proxy 也處理 HTTPS 重新導向，則不需要使用 HTTPS 重新導向中介軟體。 如果 proxy 伺服器也處理寫入 HSTS 標頭（例如， [IIS 10.0 （1709）或更新版本中的原生 HSTS 支援](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)），應用程式就不需要 HSTS 中介軟體。 如需詳細資訊，請參閱[在專案建立時退出宣告 HTTPS/HSTS](#opt-out-of-httpshsts-on-project-creation)。

### <a name="usehttpsredirection"></a>UseHttpsRedirection

下列程式碼會呼叫 `Startup` 類別中的 `UseHttpsRedirection`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

上述反白顯示的程式碼：

* 會使用預設的[HttpsRedirectionOptions RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) （[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)）。
* 除非由 `ASPNETCORE_HTTPS_PORT` 環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)覆寫，否則會使用預設的[HttpsRedirectionOptions HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) （null）。

我們建議使用暫時重新導向，而不是永久重新導向。 連結快取在開發環境中可能會造成不穩定的行為。 如果您想要在應用程式處於非開發環境時傳送永久重新導向狀態碼，請參閱在[生產中設定永久重新導向](#configure-permanent-redirects-in-production)一節。 我們建議使用[HSTS](#http-strict-transport-security-protocol-hsts)來通知用戶端，只應將安全的資源要求傳送至應用程式（僅限生產環境）。

### <a name="port-configuration"></a>連接埠組態

中介軟體必須要有埠，才可將不安全的要求重新導向至 HTTPS。 如果沒有可用的埠：

* 不會重新導向至 HTTPS。
* 中介軟體會記錄「無法判斷重新導向的 HTTPs 埠」的警告。

使用下列任何方法來指定 HTTPS 埠：

* 設定[HttpsRedirectionOptions. HttpsPort](#options)。

::: moniker range=">= aspnetcore-3.0"

* 設定 [`https_port`[主機] 設定](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port)：

  * 在 [主機設定] 中。
  * 藉由設定 `ASPNETCORE_HTTPS_PORT` 環境變數。
  * 藉由在*appsettings*中新增最上層專案：

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* 使用[ASPNETCORE_URLS 環境變數](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls)，表示具有安全配置的埠。 環境變數會設定伺服器。 中介軟體會透過 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>間接探索 HTTPS 埠。 這種方法無法在反向 proxy 部署中使用。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* 設定 [`https_port`[主機] 設定](xref:fundamentals/host/web-host#https-port)：

  * 在 [主機設定] 中。
  * 藉由設定 `ASPNETCORE_HTTPS_PORT` 環境變數。
  * 藉由在*appsettings*中新增最上層專案：

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* 使用[ASPNETCORE_URLS 環境變數](xref:fundamentals/host/web-host#server-urls)，表示具有安全配置的埠。 環境變數會設定伺服器。 中介軟體會透過 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>間接探索 HTTPS 埠。 這種方法無法在反向 proxy 部署中使用。

::: moniker-end

* 在開發中，在*launchsettings.json*中設定 HTTPS URL。 使用 IIS Express 時啟用 HTTPS。

* 針對[Kestrel](xref:fundamentals/servers/kestrel)伺服器或[HTTP.sys](xref:fundamentals/servers/httpsys)伺服器的公眾面向邊緣部署，設定 HTTPS URL 端點。 應用程式只會使用**一個 HTTPS 埠**。 中介軟體會透過 <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>來探索埠。

> [!NOTE]
> 當應用程式在反向 proxy 設定中執行時，<xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> 無法使用。 使用本節所述的其中一種其他方法來設定埠。

### <a name="edge-deployments"></a>Edge 部署 

當 Kestrel 或 HTTP.SYS 做為公眾面向的邊緣伺服器時，Kestrel 或 HTTP.SYS 必須設定為接聽兩者：

* 重新導向用戶端的安全埠（通常是生產環境中的443和開發中的5001）。
* 不安全的埠（通常是在生產環境中為80，開發中則為5000）。

用戶端必須能夠存取不安全的埠，應用程式才能接收不安全的要求，並將用戶端重新導向至安全的埠。

如需詳細資訊，請參閱[Kestrel 端點 configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>。

### <a name="deployment-scenarios"></a>部署案例

用戶端與伺服器之間的任何防火牆，也必須開啟流量的通訊埠。

如果要求是在反向 proxy 設定中轉送，請在呼叫 HTTPS 重新導向中介軟體之前，使用[轉送的標頭中介軟體](xref:host-and-deploy/proxy-load-balancer)。 轉送的標頭中介軟體會使用 `X-Forwarded-Proto` 標頭來更新 `Request.Scheme`。 中介軟體允許重新導向 Uri 和其他安全性原則正確運作。 未使用轉送的標頭中介軟體時，後端應用程式可能不會收到正確的配置，且會在重新導向迴圈中結束。 常見的使用者錯誤訊息是發生太多次重新導向。

部署到 Azure App Service 時，請遵循教學課程[：將現有的自訂 SSL 憑證系結至 Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl)中的指引。

### <a name="options"></a>選項

下列反白顯示的程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)來設定中介軟體選項：


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


只有在變更 `HttpsPort` 或 `RedirectStatusCode`的值時，才需要呼叫 `AddHttpsRedirection`。

上述反白顯示的程式碼：

* 將[HttpsRedirectionOptions RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*)設定為 <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>，這是預設值。 使用 <xref:Microsoft.AspNetCore.Http.StatusCodes> 類別的欄位，以 `RedirectStatusCode`指派。
* 將 HTTPS 埠設定為5001。

#### <a name="configure-permanent-redirects-in-production"></a>在生產環境中設定永久重新導向

中介軟體會預設為傳送具有所有重新導向的[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) 。 如果您想要在應用程式處於非開發環境時傳送永久重新導向狀態碼，請在非開發環境的條件式檢查中包裝中介軟體選項設定。

::: moniker range=">= aspnetcore-3.0"

在*Startup.cs*中設定服務時：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
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

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

在*Startup.cs*中設定服務時：

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

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a>HTTPS 重新導向中介軟體替代方法

使用 HTTPS 重新導向中介軟體（`UseHttpsRedirection`）的替代方法是使用 URL 重寫中介軟體（`AddRedirectToHttps`）。 `AddRedirectToHttps` 也可以在重新導向執行時設定狀態碼和埠。 如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。

重新導向至 HTTPS 時，若不需要額外的重新導向規則，建議使用本主題中所述的 HTTPS 重新導向中介軟體（`UseHttpsRedirection`）。

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP 嚴格傳輸安全性通訊協定（HSTS）

根據[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP 嚴格傳輸安全性（HSTS）](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)是由 web 應用程式透過使用回應標頭所指定的加入宣告安全性增強功能。 當[支援 HSTS 的瀏覽器](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support)收到此標頭時：

* 瀏覽器會儲存網域的設定，以防止透過 HTTP 傳送任何通訊。 瀏覽器會強制所有透過 HTTPS 進行的通訊。
* 瀏覽器會防止使用者使用不受信任或不正確憑證。 瀏覽器會停用允許使用者暫時信任這類憑證的提示。

因為 HSTS 是由用戶端強制執行，所以有一些限制：

* 用戶端必須支援 HSTS。
* HSTS 至少需要一個成功的 HTTPS 要求，才能建立 HSTS 原則。
* 應用程式必須檢查每個 HTTP 要求，然後重新導向或拒絕 HTTP 要求。

ASP.NET Core 2.1 和更新版本使用 `UseHsts` 擴充方法來執行 HSTS。 當應用程式不在[開發模式](xref:fundamentals/environments)時，下列程式碼會呼叫 `UseHsts`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

`UseHsts` 不建議在開發中使用，因為瀏覽器會高度快取 HSTS 設定。 根據預設，`UseHsts` 會排除本機回送位址。

若為第一次執行 HTTPS 的生產環境，請使用其中一個 <xref:System.TimeSpan> 方法，將初始[HstsOptions](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*)設定為較小的值。 如果您需要將 HTTPS 基礎結構還原為 HTTP，請將值從小時設定為不超過一天。 在您確信 HTTPS 設定的持續性之後，請增加 HSTS 的最大壽命值;常使用的值為一年。

下列程式碼範例：


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* 設定嚴格傳輸安全性標頭的預先載入參數。 預先載入不是[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)的一部分，但 web 瀏覽器支援在全新安裝時預先載入 HSTS 網站。 如需詳細資訊，請參閱 [https://hstspreload.org/](https://hstspreload.org/)。
* 啟用[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，這會將 HSTS 原則套用至裝載子域。
* 將嚴格傳輸安全性標頭的最大壽命參數明確設定為60天。 如果未設定，則預設為30天。 如需詳細資訊，請參閱[最大壽命](https://tools.ietf.org/html/rfc6797#section-6.1.1)指示詞。
* 將 `example.com` 新增至要排除的主機清單。

`UseHsts` 排除下列回送主機：

* `localhost`： IPv4 回送位址。
* `127.0.0.1`： IPv4 回送位址。
* `[::1]`： IPv6 回送位址。

## <a name="opt-out-of-httpshsts-on-project-creation"></a>在建立專案時退出宣告 HTTPS/HSTS

在某些後端服務案例中，連線安全性會在網路的公開邊緣處理，而不需要在每個節點上設定連線安全性。 從 Visual Studio 中的範本或[dotnet new](/dotnet/core/tools/dotnet-new)命令產生的 Web 應用程式會啟用[HTTPS](#require-https)重新導向和[HSTS](#http-strict-transport-security-protocol-hsts)。 針對不需要這些案例的部署，您可以在從範本建立應用程式時退出宣告 HTTPS/HSTS。

若要退出 HTTPS/HSTS：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

取消核取 [**針對 HTTPS 設定**] 核取方塊。

::: moniker range=">= aspnetcore-3.0"

![[新增 ASP.NET Core Web 應用程式] 對話方塊，其中顯示未選取的 [設定 HTTPS] 核取方塊。](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![[新增 ASP.NET Core Web 應用程式] 對話方塊，其中顯示未選取的 [設定 HTTPS] 核取方塊。](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

使用 `--no-https` 選項。 例如：

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>信任 Windows 和 macOS 上的 ASP.NET Core HTTPS 開發憑證

.NET Core SDK 包含 HTTPS 開發憑證。 憑證會在首次執行體驗中安裝。 例如，`dotnet --info` 會產生類似下列的輸出：

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

安裝 .NET Core SDK 會將 ASP.NET Core HTTPS 開發憑證安裝至本機使用者憑證存放區。 憑證已安裝，但不受信任。 若要信任憑證，請執行 dotnet `dev-certs` 工具的一次性步驟：

```dotnetcli
dotnet dev-certs https --trust
```

下列命令會提供 `dev-certs` 工具的說明：

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>如何設定 Docker 的開發人員憑證

請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6199)。

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>從適用于 Linux 的 Windows 子系統信任 HTTPS 憑證

適用于 Linux 的 Windows 子系統（WSL）會產生 HTTPS 自我簽署憑證。若要將 Windows 憑證存放區設定為信任 WSL 憑證：

* 執行下列命令以匯出 WSL 產生的憑證： `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* 在 WSL 視窗中，執行下列命令： `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  上述命令會設定環境變數，讓 Linux 使用 Windows 受信任的憑證。

## <a name="troubleshoot-certificate-problems"></a>針對憑證問題進行疑難排解

當 ASP.NET Core 的 HTTPS 開發憑證已[安裝並受信任](#trust)，但您仍有瀏覽器警告，指出憑證不受信任時，本節會提供協助。 [Kestrel](xref:fundamentals/servers/kestrel)會使用 ASP.NET Core HTTPS 開發憑證。

### <a name="all-platforms---certificate-not-trusted"></a>所有平臺-憑證不受信任

執行下列命令：

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

關閉任何開啟的瀏覽器實例。 在應用程式中開啟新的瀏覽器視窗。 瀏覽器會快取憑證信任。

上述命令會解決大部分的瀏覽器信任問題。 如果瀏覽器仍不信任憑證，請遵循遵循的平臺特定建議。

### <a name="docker---certificate-not-trusted"></a>Docker-憑證不受信任

* 刪除*C:\Users\{USER} \AppData\Roaming\ASP.NET\Https*資料夾。
* 清除方案。 刪除 [bin] 和 [obj] 資料夾。
* 重新開機開發工具。 例如，Visual Studio、Visual Studio Code 或 Visual Studio for Mac。

### <a name="windows---certificate-not-trusted"></a>Windows-憑證不受信任

* 檢查證書存儲中的憑證。 在 [`Current User > Personal > Certificates`] 和 [`Current User > Trusted root certification authorities > Certificates`] 底下，應該會有一個 `localhost` 憑證具有 `ASP.NET Core HTTPS development certificate` 易記名稱
* 從個人和信任的根憑證授權單位移除所有找到的憑證。 請勿**移除 IIS Express** localhost 憑證。
* 執行下列命令：

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

關閉任何開啟的瀏覽器實例。 在應用程式中開啟新的瀏覽器視窗。

### <a name="os-x---certificate-not-trusted"></a>OS X-憑證不受信任

* 開啟 [KeyChain 存取]。
* 選取 [系統 keychain]。
* 檢查 localhost 憑證是否存在。
* 檢查其是否包含圖示上的 `+` 符號，以指出其是否受其信任，以供所有使用者使用。
* 從系統 keychain 中移除憑證。
* 執行下列命令：

```dotnetcli
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

關閉任何開啟的瀏覽器實例。 在應用程式中開啟新的瀏覽器視窗。

請參閱[使用 IIS Express （dotnet/AspNetCore #16892）的 HTTPS 錯誤](https://github.com/dotnet/AspNetCore/issues/16892)，以疑難排解 Visual Studio 的憑證問題。

### <a name="iis-express-ssl-certificate-used-with-visual-studio"></a>搭配 Visual Studio 使用的 IIS Express SSL 憑證

若要修正 IIS Express 憑證的問題，請從 Visual Studio 安裝程式中選取 [**修復**]。 如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/dotnet/aspnetcore/issues/16892) \(英文\)。

## <a name="additional-information"></a>其他資訊

* <xref:host-and-deploy/proxy-load-balancer>
* [在 Linux 上使用 Apache： HTTPS 設定的主機 ASP.NET Core](xref:host-and-deploy/linux-apache#https-configuration)
* [在 Linux 上使用 Nginx： HTTPS 設定的主機 ASP.NET Core](xref:host-and-deploy/linux-nginx#https-configuration)
* [如何在 IIS 上設定 SSL](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [OWASP HSTS 瀏覽器支援](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
