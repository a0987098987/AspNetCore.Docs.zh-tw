---
title: 強制執行中的 ASP.NET Core HTTPS
author: rick-anderson
description: 示範如何要求 HTTPS/TLS 中 ASP.NET Core web 應用程式。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: edc69443455677ba80ebb0a73e193d4d6741e470
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>強制執行中的 ASP.NET Core HTTPS

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文件說明如何：

- 所有要求需要 HTTPS。
- 將所有 HTTP 要求重新都導向至 HTTPS。

> [!WARNING]
> 請勿**不**使用`RequireHttpsAttribute`上接收機密資訊的 Web Api。 `RequireHttpsAttribute` 從 HTTP 至 HTTPS 的瀏覽器重新導向會使用 HTTP 狀態碼。 API 用戶端可能不了解，或是遵循從 HTTP 重新導向至 HTTPS。 這類用戶端可能會透過 HTTP 傳送資訊。 Web 應用程式開發介面應執行下列之一：
>
>* 不在 HTTP 上接聽。
>* 關閉與狀態碼 400 （不正確的要求） 的連線，並不會處理要求。

<a name="require"></a>
## <a name="require-https"></a>需要 HTTPS

::: moniker range=">= aspnetcore-2.1"
我們建議所有 ASP.NET Core web 應用程式呼叫`UseHttpsRedirection`將所有 HTTP 要求重新都導向至 HTTPS。 如果`UseHsts`稱為應用程式，並在它之前必須先呼叫`UseHttpsRedirection`。

下列程式碼會呼叫`UseHttpsRedirection`中`Startup`類別：

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]


下列程式碼範例：

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

* 設定`RedirectStatusCode`。
* 5001 設定 HTTPS 連接埠。

::: moniker-end


::: moniker range="< aspnetcore-2.1"

[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute)用來要求 HTTPS。 `[RequireHttpsAttribute]` 可以裝飾控制器或方法，或可以全域套用。 若要全域套用的屬性，加入下列程式碼加入`ConfigureServices`中`Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

上述的反白顯示程式碼需要的所有要求都使用`HTTPS`; 因此，HTTP 要求會被忽略。 而下列反白顯示的程式碼會將所有 HTTP 要求都重新導向至 HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

如需詳細資訊，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。

全域使用 HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) 是安全性最佳作法。 將`[RequireHttps]`屬性套用至所有控制器，不會比全域使用 HTTPS 來的安全。 您無法保證`[RequireHttps]`加入新的控制器和 Razor 頁面時，屬性會套用。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP 嚴格的傳輸安全性通訊協定 (HSTS)

每個[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)， [HTTP 嚴格的傳輸安全性 (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)是透過使用特殊的回應標頭的 web 應用程式所指定的選擇加入的安全性增強功能。 一旦支援的瀏覽器收到此標頭該瀏覽器將會避免任何透過 HTTP 傳送到指定的網域進行通訊，並改為將透過 HTTPS 傳送的所有通訊。 它也會防止 HTTPS 的點選連結的瀏覽器的提示。

2.1 或更新版本的 ASP.NET Core 實作與 HSTS`UseHsts`擴充方法。 下列程式碼會呼叫`UseHsts`應用程式不在[開發模式](xref:fundamentals/environments):

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` 不建議在開發因為 HSTS 標頭是高度可快取的瀏覽器。 根據預設，UseHsts 排除本機回送位址。

下列程式碼範例：

[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* 設定 Strict 傳輸安全性標頭的預先載入的參數。 預先載入不屬於[RFC HSTS 規格](https://tools.ietf.org/html/rfc6797)，但是要預先載入 HSTS 上全新安裝的站台的網頁瀏覽器支援。 請參閱 [https://hstspreload.org/](https://hstspreload.org/) 以取得詳細資訊。
* 可讓[includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)，這會套用到主機的子網域的 HSTS 原則。 
* 明確設定為 60 天的 Strict 傳輸安全性標頭的保留時間上限參數。 如果沒有設定，預設值為 30 天。 請參閱[保留時間上限指示詞](https://tools.ietf.org/html/rfc6797#section-6.1.1)如需詳細資訊。
* 新增`example.com`的主機，以排除清單。

`UseHsts` 排除下列回送主機：

* `localhost` : IPv4 回送位址。
* `127.0.0.1` : IPv4 回送位址。
* `[::1]` : IPv6 回送位址。

上述範例顯示如何新增其他主機。
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>選擇不使用 HTTPS 的專案建立

ASP.NET 核心 2.1 和更新版本 （從 Visual Studio 或 dotnet 命令列） 的 web 應用程式範本可讓[HTTPS 的重新導向](#require)和[HSTS](#hsts)。 對於不需要 HTTPS 的部署，您可以選擇不使用的 HTTPS。 例如，不需要其中 HTTPS 處理外部在邊緣，每個節點使用 HTTPS 的某些後端服務。

若要退出 HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

取消核取**設定以進行 HTTPS**核取方塊。

![實體圖表](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

使用 `--no-https` 選項。 例如

```cli
dotnet new razor --no-https
```

------

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a>安裝 Docker 的開發人員憑證

請參閱[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6199)。

::: moniker-end
