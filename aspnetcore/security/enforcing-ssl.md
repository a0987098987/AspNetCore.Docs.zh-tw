---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 示範如何要求 HTTPS/TLS 中的 ASP.NET Core web 應用程式。
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: a4ab91ef23a798c919a23a44f5a050bd3c09d56a
ms.sourcegitcommit: d99a8554c91f626cf5e466911cf504dcbff0e02e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2018
ms.locfileid: "39356684"
---
# <a name="enforce-https-in-aspnet-core"></a>強制使用 ASP.NET Core 中的 HTTPS

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文件說明如何：

* 需要 HTTPS 進行的所有要求。
* 將所有 HTTP 要求重新都導向至 HTTPS。

> [!WARNING]
> 請勿**未**使用[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)接收機密資訊的 Web Api 上。 `RequireHttpsAttribute` 若要從 HTTP 至 HTTPS 的瀏覽器重新導向，會使用 HTTP 狀態碼。 API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。 此類用戶端可能會透過 HTTP 傳送資訊。 Web Api 應執行下列之一：
>
> * 不在 HTTP 上接聽。
> * 關閉與狀態碼 400 （不正確的要求） 的連線，並不會提供要求。

<a name="require"></a>
## <a name="require-https"></a>需要 HTTPS

::: moniker range=">= aspnetcore-2.1"

我們建議所有的 ASP.NET Core web 應用程式呼叫 HTTPS 重新導向中介軟體 ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) 到所有的 HTTP 要求重新導向至 HTTPS。

下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

上述反白顯示的程式碼：

* 使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`)。 生產環境應用程式應該呼叫[UseHsts](#hsts)。
* 使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443)。

下列程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

上述反白顯示的程式碼：

* 設定組[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)到`Status307TemporaryRedirect`，這是預設值。 生產環境應用程式應該呼叫[UseHsts](#hsts)。
* 設定 HTTPS 連接埠為 5001。 預設值為 443。

下列機制會自動設定連接埠：

* 中介軟體可以探索透過連接埠[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)當符合下列條件：
  - Kestrel 或 HTTP.sys 可直接使用 HTTPS 端點 （也適用於 Visual Studio Code 的偵錯工具執行應用程式）。
  - 只有**一個 HTTPS 連接埠**應用程式使用。
* 使用 visual Studio:
  - IIS Express 已啟用 HTTPS。
  - *launchSettings.json*設定`sslPort`適用於 IIS Express。

> [!NOTE]
> 應用程式執行 （例如 IIS、 IIS Express），在反向 proxy 後方時`IServerAddressesFeature`無法使用。 您必須手動設定連接埠。 當未設定連接埠時，不是重新導向要求。

可以藉由設定設定的連接埠[https_port Web 主機組態設定](xref:fundamentals/host/web-host#https-port):

**索引鍵**: https_port**型別**:*字串*
**預設**： 未設定預設值。
**使用設定**: `UseSetting` 
**環境變數**: `<PREFIX_>HTTPS_PORT` (前置詞是`ASPNETCORE_`使用 Web 主機時。)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> 可以間接設定的連接埠，藉由設定 URL`ASPNETCORE_URLS`環境變數。 環境變數設定的伺服器，，，然後在中介軟體間接探索透過 HTTPS 連接埠`IServerAddressesFeature`。

如果已設定任何連接埠：

* 要求未重新導向。
* 中介軟體會記錄警告。

> [!NOTE]
> 除了使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 是使用 URL 重寫中介軟體 (`AddRedirectToHttps`)。 `AddRedirectToHttps` 也可以設定的狀態碼和連接埠重新導向為執行時。 如需詳細資訊，請參閱 < [URL 重寫中介軟體](xref:fundamentals/url-rewriting)。
>
> 當重新導向至 HTTPS，而不需要額外的重新導向規則，我們建議使用 HTTPS 重新導向中介軟體 (`UseHttpsRedirection`) 本主題中所述。

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

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP Strict Transport 安全性通訊協定 (HSTS)

每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過使用特殊的回應標頭的 web 應用程式所指定的選擇加入的安全性增強功能。 支援的瀏覽器收到此標頭之後該瀏覽器會防止任何通訊透過 HTTP 傳送至指定的網域，並改為將會透過 HTTPS 傳送的所有通訊。 它也會防止 HTTPS 點選提示上的瀏覽器。

ASP.NET Core 2.1 或更新版本會實作 HSTS 與`UseHsts`擴充方法。 下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` 不建議在開發過程中因為 HSTS 標頭是高可快取瀏覽器。 根據預設，`UseHsts`排除本機回送位址。

下列程式碼範例：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* 設定 Strict 傳輸安全性標頭的預先載入的參數。 預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。 請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。
* 可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。 
* 明確設定為 60 天的 Strict-傳輸層安全性標頭的最大壽命參數。 如果未設定，預設值為 30 天。 請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。
* 新增`example.com`的主機，以排除清單。

`UseHsts` 排除下列 「 回送 」 主控件：

* `localhost` : IPv4 回送位址。
* `127.0.0.1` : IPv4 回送位址。
* `[::1]` : IPv6 回送位址。

上述範例顯示如何新增其他主機。
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>選擇退出的 HTTPS 上建立專案

（從 Visual Studio 或 dotnet 命令列） 的 ASP.NET Core 2.1 或更新版本的 web 應用程式範本可讓[HTTPS 重新導向](#require)並[HSTS](#hsts)。 對於不需要 HTTPS 的部署，您可以選擇退出的 HTTPS。 比方說，就不需要其中 HTTPS 處理外部邊緣，每個節點上使用 HTTPS 的某些後端服務。

若要退出 HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

取消核取**設定為使用 HTTPS**核取方塊。

![實體圖表](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

使用 `--no-https` 選項。 例如

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>如何設定適用於 Docker 的開發人員憑證

請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。

::: moniker-end
