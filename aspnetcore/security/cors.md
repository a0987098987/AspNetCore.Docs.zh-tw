---
title: 啟用 ASP.NET Core 中的跨源要求 (CORS)
author: rick-anderson
description: 了解如何為標準，以允許或拒絕在 ASP.NET Core 應用程式的跨原始要求的 CORS。
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 2cad26d0f61519f63888a2bc399bb7e8a0f1ee04
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210128"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>啟用 ASP.NET Core 中的跨源要求 (CORS)

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。

瀏覽器安全性可防止網頁對不同的網域服務之 web 網頁提出要求。 這項限制稱為*同源原則*。 同源原則會防止惡意網站從另一個網站讀取敏感性資料。 有時候，您可能要允許其他站台會將跨原始來源要求對您的應用程式。 如需詳細資訊，請參閱 < [Mozilla CORS 文章](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。

[跨原始資源共用](https://www.w3.org/TR/cors/)(CORS):

* 是 W3C 標準，可讓伺服器放寬同源原則。
* 已**不**一項安全性功能，CORS 會放寬安全性。 API 不允許 CORS 較為安全。 如需詳細資訊，請參閱 <<c0> [ 運作方式的 CORS](#how-cors)。
* 可讓以明確允許某些跨源要求，並拒絕其他的伺服器。
* 是更安全和更有彈性，比早期的技術，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>相同原始來源

兩個 Url 有相同的原點，如果它們有相同的配置、 主機和連接埠 ([RFC 6454](https://tools.ietf.org/html/rfc6454))。

這兩個 Url 有相同的來源：

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些 Url 會有不同的來源，比先前的兩個 Url:

* `https://example.net` &ndash; 不同的網域
* `https://www.example.com/foo.html` &ndash; 不同的子網域
* `http://example.com/foo.html` &ndash; 不同的配置
* `https://example.com:9000/foo.html` &ndash; 不同的連接埠

比較原始來源時，Internet Explorer 不會考慮連接埠。

## <a name="cors-with-named-policy-and-middleware"></a>使用具名的原則和中介軟體的 CORS

CORS 中介軟體會處理跨原始來源要求。 下列程式碼會為指定的 origin 整個應用程式啟用 CORS:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

上述程式碼：

* 若要設定的原則名稱"\_myAllowSpecificOrigins"。 原則名稱是任意的。
* 呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>延伸模組方法，可讓核心。
* 呼叫<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>具有[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。 Lambda 會採用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>物件。 [組態選項](#cors-policy-options)，例如`WithOrigins`，本文稍後所述。

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將應用程式的服務容器中的 CORS 服務：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

如需詳細資訊，請參閱 < [CORS 原則選項](#cpo)本文件中。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以鏈結方法，如下列程式碼所示：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

下列醒目提示的程式碼會套用至透過 CORS 中介軟體的所有應用程式端點的 CORS 原則：

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

請參閱[Razor 頁面、 控制器及動作方法中的 啟用 CORS](#ecors)套用頁面/控制站/動作層級的 CORS 原則。

注意:

* `UseCors` 必須在 `UseMvc` 之前呼叫。
* URL 必須**未**包含斜線 (`/`)。 如果 URL 終止`/`，比較傳回`false`並傳回不含標頭。

請參閱[測試 CORS](#test)如需有關測試上述程式碼。

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a>使用屬性中啟用 CORS

[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方案。 `[EnableCors]`屬性能讓選取的結束點，而不是所有的結束點的 CORS。

使用`[EnableCors]`指定的預設原則和`[EnableCors("{Policy String}")]`指定原則。

`[EnableCors]`屬性可以套用至：

* Razor 頁面 `PageModel`
* 控制器
* 控制器動作方法

您可以將不同原則套用至控制器/頁面模型/動作與`[EnableCors]`屬性。 當`[EnableCors]`屬性會套用至控制器/頁面模型/動作方法，並啟用 CORS 中介軟體中，這兩項原則會套用。 我們建議您不要組合原則。 使用`[EnableCors]`屬性或中介軟體，不能同時在相同的應用程式中。

下列程式碼會將不同的原則套用至每個方法：

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

下列程式碼會建立 CORS 預設原則和原則，名為`"AnotherPolicy"`:

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>停用 CORS

[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)控制器/頁面模型/動作屬性停用 CORS。

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS 原則選項

本章節描述各種選項，可在 CORS 原則設定：

* [設定允許的來源](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨原始來源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預檢到期時間](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> 在中，稱為`Startup.ConfigureServices`。 如需一些選項，可能會很有幫助讀取[運作方式的 CORS](#how-cors)區段第一次。

## <a name="set-the-allowed-origins"></a>設定允許的來源

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許來自任何配置的所有原始網域的 CORS 要求 (`http`或`https`)。 `AllowAnyOrigin` 不安全因為*任何網站*可以將跨原始來源要求對應用程式。

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> 指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。 這兩種方法以設定應用程式時，CORS 服務就會傳回 CORS 回應無效。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> 指定`AllowAnyOrigin`和`AllowCredentials`是不安全的設定，可能會導致跨網站偽造要求。 安全的應用程式中，指定確切的原始來源清單，如果用戶端，必須獲得授權存取伺服器資源本身。

::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**.
-->

`AllowAnyOrigin` 會影響預檢要求，`Access-Control-Allow-Origin`標頭。 如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 設定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>屬性是可讓正在評估是否允許來源時，符合設定的萬用字元網域的原始來源的函式的原則。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>：

* 可讓任何 HTTP 方法：
* 會影響預檢要求，`Access-Control-Allow-Methods`標頭。 如需詳細資訊，請參閱 <<c0> [ 預檢要求](#preflight-requests)一節。

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

根據預設，瀏覽器不會將所有應用程式的回應標頭公開。 如需詳細資訊，請參閱[W3C 跨原始資源共用 （術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

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

使用 jQuery:

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

使用[擷取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

伺服器必須允許的認證。 若要允許跨原始來源的認證，請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 回應包含`Access-Control-Allow-Credentials`標頭，它會告訴瀏覽器伺服器允許跨原始來源要求認證。

如果瀏覽器傳送認證，但是回應沒有包含有效`Access-Control-Allow-Credentials`標頭，瀏覽器不會公開應用程式，回應及跨原始來源要求會失敗。

允許跨原始來源的認證會造成安全性風險。 網站，以在另一個網域可以傳送給使用者不知情的情況下代表的使用者上的應用程式的登入使用者的認證。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

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

* `Access-Control-Request-Method`：將會用於實際要求的 HTTP 方法。
* `Access-Control-Request-Headers`：在實際的要求設定的應用程式的要求標頭的清單。 如稍早所述，這不包含標頭，以瀏覽器設定，例如`User-Agent`。

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

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS 的運作方式

本章節描述中所發生情況[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求的 HTTP 訊息層級。

* CORS 是**不**安全的功能。 CORS 是 W3C 標準，可讓伺服器放寬同源原則。
  * 比方說，無法使用惡意執行者[防止跨網站指令碼 (XSS)](xref:security/cross-site-scripting)對您的網站並執行其已啟用 CORS 的站台的跨網站要求竊取資訊。
* 您的 API 不允許 CORS 較為安全。
  * 它由用戶端 （瀏覽器） 強制執行 CORS。 伺服器會執行要求，並傳回回應，它是發生錯誤，區塊會將回應傳回用戶端。 例如，任何下列工具會顯示伺服器回應：
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * 網頁瀏覽器的網址列中輸入 URL。
* 它可讓伺服器，以允許瀏覽器執行跨原始來源[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或是[擷取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)否則會禁止的要求。
  * （不含 CORS) 的瀏覽器無法執行跨原始來源要求。 CORS，再[JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)用來規避這項限制。 JSONP 不會使用 XHR，它會使用`<script>`接收回應的標記。 要載入的跨原始來源系統允許指令碼。

[CORS 規格](https://www.w3.org/TR/cors/)引進了數個新的 HTTP 標頭啟用跨源要求。 如果瀏覽器支援 CORS，它會設定自動跨原始來源要求這些標頭。 若要啟用 CORS，不需要自訂 JavaScript 程式碼。

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

<a name="test"></a>

## <a name="test-cors"></a>測試 CORS

若要測試 CORS:

1. [建立 API 專案](xref:tutorials/first-web-api)。 或者，您可以[下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors)。
1. 啟用 CORS，本文件中使用其中一種方法。 例如: 

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` 應該只用於測試的範例應用程式類似於[下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors)。

1. 建立 web 應用程式專案 （Razor 頁面或 MVC）。 此範例會使用 Razor 頁面。 您可以在相同的方案，為 API 專案中建立 web 應用程式。
1. 將下列反白顯示的程式碼，加入*Index.cshtml*檔案：

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. 在上述程式碼，取代`url: 'https://<web app>.azurewebsites.net/api/values/1',`以部署的應用程式的 url。
1. 部署 API 專案。 例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。
1. 從桌面執行的 Razor 頁面或 MVC 應用程式，然後按一下**測試** 按鈕。 您可以使用 F12 工具來檢閱錯誤訊息。
1. 移除從 localhost 原點`WithOrigins`部署應用程式。 或者，執行用戶端應用程式使用不同的連接埠。 例如，從 Visual Studio 執行。
1. 使用用戶端應用程式進行測試。 CORS 錯誤會傳回錯誤，但無法使用 JavaScript 的錯誤訊息。 使用 F12 工具中的 [主控台] 索引標籤，來查看錯誤。 根據瀏覽器中，您收到錯誤 （在 [F12 工具] 主控台中） 如下所示：

   * 使用 Microsoft Edge:

     **SEC7120: [CORS] 原點`https://localhost:44375`找不到`https://localhost:44375`中跨原始來源資源的存取控制-允許-原始回應標頭 `https://webapi.azurewebsites.net/api/values/1`**

   * 使用 Chrome:

     **存取在 XMLHttpRequest`https://webapi.azurewebsites.net/api/values/1`從原點`https://localhost:44375`已封鎖的 CORS 原則：要求的資源上有沒有 '存取控制-允許-原始' 標頭。**

## <a name="additional-resources"></a>其他資源

* [跨原始資源共用 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
