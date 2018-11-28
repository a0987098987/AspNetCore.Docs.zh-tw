---
title: 啟用 ASP.NET Core 中的跨源要求 (CORS)
author: rick-anderson
description: 了解如何為標準，以允許或拒絕在 ASP.NET Core 應用程式的跨原始要求的 CORS。
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458539"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>啟用 ASP.NET Core 中的跨源要求 (CORS)

藉由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)

瀏覽器安全性可防止網頁對不同的網域服務之 web 網頁提出要求。 這項限制稱為*同源原則*。 同源原則會防止惡意網站從另一個網站讀取敏感性資料。 有時候，您可能要允許其他站台會將跨原始來源要求對您的應用程式。

[跨原始資源共用](https://www.w3.org/TR/cors/)(CORS) 是 W3C 標準，可讓伺服器放寬同源原則。 使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。 CORS 可較為安全且更有彈性，比早期的技術，例如[JSONP](https://wikipedia.org/wiki/JSONP)。 本主題說明如何在 ASP.NET Core 應用程式中啟用 CORS。

## <a name="same-origin"></a>婞鎏磭懘

兩個 Url 有相同的原點，如果它們有相同的配置、 主機和連接埠 ([RFC 6454](https://tools.ietf.org/html/rfc6454))。

這些兩個 Url 有相同的來源：

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些 Url 會有不同的來源，比先前的兩個 Url:

* `https://example.net` &ndash; 不同的網域
* `https://www.example.com/foo.html` &ndash; 不同的子網域
* `http://example.com/foo.html` &ndash; 不同的配置
* `https://example.com:9000/foo.html` &ndash; 不同的連接埠

> [!NOTE]
> 比較原始來源時，Internet Explorer 不會視為連接埠。

## <a name="register-cors-services"></a>註冊的 CORS 服務

::: moniker range=">= aspnetcore-2.1"

參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)封裝。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或新增的套件參考[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)封裝。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

新增的套件參考[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)封裝。

::: moniker-end

呼叫<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>在`Startup.ConfigureServices`將 CORS 服務加入至應用程式的服務容器：

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>啟用 CORS

註冊之後的 CORS 服務，使用下列其中一個方法來啟用 CORS 的 ASP.NET Core 應用程式中：

* [CORS 中介軟體](#enable-cors-with-cors-middleware)&ndash;套用 CORS 原則全域透過中介軟體應用程式。
* [在 MVC 中的 CORS](#enable-cors-in-mvc) &ndash;套用 CORS 原則，每個動作，或每個控制站。 不使用 CORS 中介軟體。

### <a name="enable-cors-with-cors-middleware"></a>啟用 CORS 與 CORS 中介軟體

CORS 中介軟體會處理應用程式的跨原始來源要求。 若要啟用 CORS 中介軟體在要求處理管線中，呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>擴充方法`Startup.Configure`。

CORS 中介軟體必須在前面定義的端點在您的應用程式中要支援跨原始來源要求 (例如，再呼叫`UseMvc`MVC/Razor 頁面中介軟體)。

A*跨源原則*新增 CORS 中介軟體使用時，可以指定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>類別。 有兩個方法定義的 CORS 原則：

* 呼叫`UseCors`使用 lambda:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  Lambda 會採用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>物件。 [組態選項](#cors-policy-options)，例如`WithOrigins`，本主題稍後所述。 在上述範例中，該原則可讓跨源要求，從`https://example.com`和其他來源。

  必須指定不含尾端斜線的 URL (`/`)。 如果 URL 終止`/`，比較傳回`false`並傳回不含標頭。

  `CorsPolicyBuilder` 有 fluent API，因此您可以在方法呼叫鏈結：

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* 定義一或多個具名的 CORS 原則，並在執行階段的名稱以選取原則。 下列範例會將名為使用者定義的 CORS 原則*AllowSpecificOrigin*。 若要選取原則，將名稱傳遞給`UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>啟用在 MVC 中的 CORS

您也可以使用 MVC 套用特定 CORS 原則，每個動作，或每個控制器。 您可以使用 MVC 啟用 CORS，會使用已註冊的 CORS 服務。 不使用 CORS 中介軟體。

### <a name="per-action"></a>每個動作

若要指定特定動作的 CORS 原則，請新增[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性的動作。 指定原則名稱。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>每個控制站

若要指定特定的控制站的 CORS 原則，請新增[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性至控制器類別。 指定原則名稱。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

優先順序是：

1. action
1. 控制器

### <a name="disable-cors"></a>停用 CORS

若要停用控制器或動作的 CORS，請使用[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性：

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>CORS 原則選項

本章節描述您可以設定的 CORS 原則中的各種選項。 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>方法呼叫`Startup.ConfigureServices`。

* [設定允許的來源](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨原始來源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預檢到期時間](#set-the-preflight-expiration-time)

如需一些選項，可能會很有幫助讀取[運作方式的 CORS](#how-cors-works)區段第一次。

### <a name="set-the-allowed-origins"></a>設定允許的來源

ASP.NET Core MVC 中的 CORS 中介軟體有幾種方式可以指定允許的來源：

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; 可讓您指定一個或多個 Url。 URL 可能包含配置、 主機名稱和連接埠，但不含任何路徑資訊。 例如， `https://example.com` 。 必須指定不含尾端斜線的 URL (`/`)。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許來自任何配置的所有原始網域的 CORS 要求 (`http`或`https`)。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  請仔細考慮，才能允許來自任何來源的要求。 允許來自任何來源的要求表示*任何網站*可以將跨原始來源要求對您的應用程式。

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > 指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。 這兩種方法以設定應用程式時，CORS 服務就會傳回 CORS 回應無效。

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > 指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。 請考慮指定確切的原始來源清單，如果用戶端，必須獲得授權存取伺服器資源本身。

  ::: moniker-end

  此設定會影響預檢要求，`Access-Control-Allow-Origin`標頭。 如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。

::: moniker range=">= aspnetcore-2.0"

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 設定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>原則以允許符合設定含萬用字元網域，如果原始來源允許在評估時的原始來源的函式的屬性。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

若要允許所有的 HTTP 方法，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

此設定會影響預檢要求，`Access-Control-Allow-Methods`標頭。 如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

若要允許特定的標頭傳送在 CORS 要求中，呼叫*撰寫要求標頭*，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允許所有 author 要求標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

此設定會影響預檢要求，`Access-Control-Request-Headers`標頭。 如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。

::: moniker range=">= aspnetcore-2.2"

CORS 中介軟體原則相符項目所指定的特定標頭`WithHeaders`傳送的標頭時才有可能`Access-Control-Request-Headers`完全符合的標頭中所述`WithHeaders`。

比方說，假設應用程式設定，如下所示：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中介軟體會拒絕具有以下要求標頭的預檢要求，因為`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 中未列出`WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

應用程式會傳回*200 確定*回應但不會傳送回 CORS 標頭。 因此，在瀏覽器不會嘗試跨原始來源要求。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS 中介軟體一律允許四個中的標頭`Access-Control-Request-Headers`傳送不論 CorsPolicy.Headers 中設定的值。 此標頭的清單包括：

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

比方說，假設應用程式設定，如下所示：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中介軟體成功回應下列要求標頭的預檢要求因為`Content-Language`總是列入允許清單：

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

根據預設，瀏覽器不會將所有應用程式的回應標頭公開。 如需詳細資訊，請參閱 < [W3C 跨原始資源共用 （術語）： 簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

是預設可用的回應標頭如下：

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 規格會呼叫這些標頭*簡單的回應標頭*。 若要讓其他標頭使用應用程式，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>跨原始來源要求中的認證

認證需要在 CORS 要求的特殊處理。 根據預設，瀏覽器不會傳送具有跨原始要求的認證。 認證包含 cookie 與 HTTP 驗證配置。 若要傳送與跨原始要求的認證，用戶端必須將`XMLHttpRequest.withCredentials`至`true`。

使用`XMLHttpRequest`直接：

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

在 jQuery 中：

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

此外，伺服器必須允許的認證。 若要允許跨原始來源的認證，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 回應包含`Access-Control-Allow-Credentials`標頭，它會告訴瀏覽器伺服器允許跨原始來源要求認證。

如果瀏覽器傳送認證，但是回應沒有包含有效`Access-Control-Allow-Credentials`標頭，瀏覽器不會公開應用程式，回應及跨原始來源要求會失敗。

允許跨原始來源的認證時要小心。 網站，以在另一個網域可以傳送給使用者不知情的情況下代表的使用者上的應用程式的登入使用者的認證。

CORS 規格也會指出該設定來源，則`"*"`（所有原始網域） 是無效的如果`Access-Control-Allow-Credentials`標頭已存在。

### <a name="preflight-requests"></a>預檢要求

對於某些 CORS 要求，瀏覽器會傳送其他要求，再進行實際的要求。 此要求會呼叫*預檢要求*。 如果下列條件成立，則瀏覽器可以略過預檢要求：

* 要求方法是 GET、 HEAD 或 POST。
* 應用程式不會設定要求標頭以外`Accept`， `Accept-Language`， `Content-Language`， `Content-Type`，或`Last-Event-ID`。
* `Content-Type`標頭，如果設定，有下列值之一：
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

在要求標頭的規則集套用至應用程式設定所呼叫的標頭的用戶端要求`setRequestHeader`上`XMLHttpRequest`物件。 CORS 規格會呼叫這些標頭*撰寫要求標頭*。 規則不適用於瀏覽器可以設定，例如標頭`User-Agent`， `Host`，或`Content-Length`。

預檢要求的範例如下：

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

事前要求使用 HTTP OPTIONS; 方法。 它包含兩個特殊標頭：

* `Access-Control-Request-Method`: 將會用於實際要求的 HTTP 方法。
* `Access-Control-Request-Headers`： 一份應用程式設定實際要求的要求標頭。 如稍早所述，這不包含標頭，以瀏覽器設定，例如`User-Agent`。

CORS 預檢要求可能包括`Access-Control-Request-Headers`標頭，會向伺服器指出傳送實際要求的標頭。

若要允許特定的標頭，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允許所有 author 要求標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

瀏覽器未在設定的方式完全一致`Access-Control-Request-Headers`。 如果您將設定標頭的任何項目以外`"*"`(或使用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)，您應該至少包含`Accept`， `Content-Type`，和`Origin`，再加上您想要支援的任何自訂標頭。

以下是範例回應預檢要求 （假設伺服器允許的要求）：

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

回應包括`Access-Control-Allow-Methods`標頭，其中列出允許的方法和 （選擇性）`Access-Control-Allow-Headers`標頭，它會列出允許的標頭。 如果預檢要求成功，則瀏覽器會傳送實際要求。

如果預檢要求遭到拒絕，應用程式會傳回*200 確定*回應但不會傳送回 CORS 標頭。 因此，在瀏覽器不會嘗試跨原始來源要求。

### <a name="set-the-preflight-expiration-time"></a>設定預檢到期時間

`Access-Control-Max-Age`標頭會指定多久可以快取預檢要求的回應。 若要設定此標頭，呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a>CORS 的運作方式

本章節描述 CORS 要求的 HTTP 訊息層級中發生的動作。 請務必了解，這樣可以正確地設定和非預期的行為發生時偵錯的 CORS 原則，CORS 的運作方式。

CORS 規格引進了數個新的 HTTP 標頭啟用跨源要求。 如果瀏覽器支援 CORS，它會設定自動跨原始來源要求這些標頭。 若要啟用 CORS，不需要自訂 JavaScript 程式碼。

以下是跨原始要求的範例。 `Origin`標頭提供網站提出要求的網域：

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

如果伺服器允許的要求，它會設定`Access-Control-Allow-Origin`標頭回應中的。 此標頭的值可能是符合`Origin`要求標頭或萬用字元值`"*"`，允許任何來源的意義：

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

如果回應不包含`Access-Control-Allow-Origin`標頭，跨原始來源要求就會失敗。 具體而言，瀏覽器不允許要求。 即使伺服器會傳回成功的回應，瀏覽器不提供回應給用戶端應用程式。

## <a name="additional-resources"></a>其他資源

* [跨原始資源共用 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
