---
title: 強制使用 ASP.NET Core 中的 HTTPS
author: rick-anderson
description: 了解如何在 ASP.NET Core web 應用程式需要 HTTPS/TLS。
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325597"
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

<a name="require"></a>

## <a name="require-https"></a>需要 HTTPS

::: moniker range=">= aspnetcore-2.1"

我們建議您所有的生產環境 ASP.NET Core web 應用程式呼叫：

* HTTPS 重新導向中介軟體 ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) 到所有的 HTTP 要求重新導向至 HTTPS。
* [UseHsts](#hsts)，HTTP Strict Transport 安全性通訊協定 (HSTS)。

### <a name="usehttpsredirection"></a>UseHttpsRedirection

下列程式碼會呼叫`UseHttpsRedirection`在`Startup`類別：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

上述反白顯示的程式碼：

* 使用預設[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect))。
* 使用預設[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) 但覆寫`ASPNETCORE_HTTPS_PORT`環境變數或[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)。

我們建議使用暫時重新導向，而不是永久重新導向，因為連結快取可能在開發環境中造成不穩定的行為。 我們建議您使用[HSTS](#hsts)來只保護資源的用戶端通知要求應傳送至 （只在生產環境） 中的應用程式。

> [!WARNING]
> 連接埠必須是適用於中介軟體重新導向至 HTTPS。 如果沒有連接埠可用，則不會發生重新導向至 HTTPS。 HTTPS 連接埠可以使用任何下列方式指定：
>
> * 設定`HttpsRedirectionOptions.HttpsPort`。
> * 設定 `ASPNETCORE_HTTPS_PORT` 環境變數。
> * 在開發中，請在中設定的 HTTPS URL *launchsettings.json*。
> * 設定的 HTTPS URL 端點[Kestrel](xref:fundamentals/servers/kestrel)或是[HTTP.sys](xref:fundamentals/servers/httpsys)。
>
> 使用 Kestrel 或 HTTP.sys 時做為向外公開邊緣伺服器，Kestrel 或 HTTP.sys 必須設定為接聽兩者：
>
> * 會在重新導向用戶端的安全連接埠 (通常，在生產環境和開發 5001 443)。
> * 不安全的連接埠 (通常，在生產環境中為 80) 與開發中的 5000。
>
> 為了讓應用程式用戶端收到不安全的要求，並將它重新導向至安全的連接埠必須能夠使用不安全的連接埠。
>
> 用戶端與伺服器之間的任何防火牆也必須有流量開啟連接埠。
>
> 如需詳細資訊，請參閱 < [Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)或<xref:fundamentals/servers/httpsys>。

下列醒目提示程式碼會呼叫[AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection)設定中介軟體選項：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

呼叫`AddHttpsRedirection`時，才需要變更的值`HttpsPort`或`RedirectStatusCode`。

上述反白顯示的程式碼：

* 設定組[HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode)要[Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)，這是預設值。 使用的欄位[StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes)類別的工作分派`RedirectStatusCode`。
* 設定 HTTPS 連接埠為 5001。 預設值為 443。

下列機制會自動設定連接埠：

* 中介軟體可以探索透過連接埠[IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature)當符合下列條件：

  * Kestrel 或 HTTP.sys 可直接使用 HTTPS 端點 （也適用於 Visual Studio Code 的偵錯工具執行應用程式）。
  * 只有**一個 HTTPS 連接埠**應用程式使用。

* 使用 visual Studio:

  * IIS Express 已啟用 HTTPS。
  * *launchSettings.json*設定`sslPort`適用於 IIS Express。

> [!NOTE]
> 應用程式執行 （例如 IIS、 IIS Express），在反向 proxy 後方時`IServerAddressesFeature`無法使用。 您必須手動設定連接埠。 當未設定連接埠時，不是重新導向要求。

可以藉由設定設定的連接埠[https_port Web 主機組態設定](xref:fundamentals/host/web-host#https-port):

**索引鍵**: https_port  
**類型**：*string*  
**預設**： 未設定預設值。  
**設定使用**：`UseSetting`  
**環境變數**: `<PREFIX_>HTTPS_PORT` (前置詞是`ASPNETCORE_`使用 Web 主機時。)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> 可以間接設定的連接埠，藉由設定 URL`ASPNETCORE_URLS`環境變數。 環境變數設定的伺服器，，，然後在中介軟體間接探索透過 HTTPS 連接埠`IServerAddressesFeature`。

如果已設定任何連接埠：

* 要求未重新導向。
* 中介軟體會記錄警告 「 無法判定重新導向的 https 連接埠。 」

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

針對生產環境中實作 HTTPS 第一次，初始 HSTS 將值設定為較小的值。 從設定值時數不超過一天的萬一您需要還原為 HTTP，HTTPS 基礎結構。 確定在 HTTPS 設定的持續性後，增加 HSTS 最大壽命值;常用的值為一年。 

下列程式碼範例：

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* 設定 Strict 傳輸安全性標頭的預先載入的參數。 預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。 請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。
* 可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，套用 HSTS 原則來裝載子網域。
* 明確設定為 60 天的 Strict 傳輸安全性標頭的最大壽命參數。 如果未設定，預設值為 30 天。 請參閱[最大壽命指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。
* 新增`example.com`的主機，以排除清單。

`UseHsts` 排除下列 「 回送 」 主控件：

* `localhost` : IPv4 回送位址。
* `127.0.0.1` : IPv4 回送位址。
* `[::1]` : IPv6 回送位址。

上述範例顯示如何新增其他主機。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a>選擇退出的 HTTPS/HSTS 專案建立

在某些後端服務案例中公開邊緣的網路處理連線安全性的位置，設定每個節點的連線安全性並非必要條件。 Web 應用程式從 Visual Studio 中，或從範本產生[dotnet 新](/dotnet/core/tools/dotnet-new)命令啟用[HTTPS 重新導向](#require)並[HSTS](#hsts)。 對於不需要這些案例的部署，您可以選擇退出的 HTTPS/HSTS 從範本建立應用程式。

若要退出 HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

取消核取**設定為使用 HTTPS**核取方塊。

![實體圖表](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

使用 `--no-https` 選項。 例如

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>如何設定適用於 Docker 的開發人員憑證

請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。

::: moniker-end

## <a name="additional-information"></a>其他資訊

* [OWASP HSTS 瀏覽器支援](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
