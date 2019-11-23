---
title: 啟用 ASP.NET Core 中的跨原始來源要求（CORS）
author: rick-anderson
description: 瞭解 CORS 如何作為標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始來源要求。
ms.author: riande
ms.custom: mvc
ms.date: 10/13/2019
uid: security/cors
ms.openlocfilehash: 3a51d365626c858ad48298a1108e37eba9050fe7
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391300"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>啟用 ASP.NET Core 中的跨原始來源要求（CORS）

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。 如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。

[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：

* 是一種 W3C 標準，可讓伺服器放寬相同的來源原則。
* **不**是安全性功能，CORS 放寬安全性。 藉由允許 CORS，API 不會更安全。 如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。
* 允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。
* 比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>相同原始來源

如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。

這兩個 Url 具有相同的來源：

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些 Url 的來源不同于前兩個 Url：

* `https://example.net` &ndash; 不同的網域
* `https://www.example.com/foo.html` &ndash; 不同的子域
* `http://example.com/foo.html` &ndash; 不同的配置
* `https://example.com:9000/foo.html` &ndash; 不同的埠

在比較原始來源時，Internet Explorer 不會考慮此埠。

## <a name="cors-with-named-policy-and-middleware"></a>具有已命名原則和中介軟體的 CORS

CORS 中介軟體會處理跨原始來源要求。 下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

上述程式碼：

* 將原則名稱設定為 "\_myAllowSpecificOrigins"。 原則名稱是任意的。
* 呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，以啟用 CORS。
* 使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)呼叫 <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>。 Lambda 會採用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。 這篇文章稍後會[說明](#cors-policy-options)設定選項`WithOrigins`，例如。

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> 方法呼叫會將 CORS 服務新增至應用程式的服務容器：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 方法可以連鎖方法，如下列程式碼所示：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

注意： URL**不**能包含尾端斜線（`/`）。 如果 URL 以 `/`終止，則比較會傳回 `false` 而且不會傳回任何標頭。

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a>將 CORS 原則套用至所有端點

下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> 透過端點路由，必須設定 CORS 中介軟體，以便在 `UseRouting` 和 `UseEndpoints`的呼叫之間執行。 不正確的設定會導致中介軟體停止正常運作。

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：
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
注意：必須在 `UseMvc`之前呼叫 `UseCors`。

::: moniker-end

請參閱[在 Razor Pages、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。

如需測試上述程式碼的指示，請參閱[測試 CORS](#test) 。

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a>使用端點路由來啟用 Cors

使用端點路由，可以使用一組 `RequireCors` 的擴充方法，針對每個端點來啟用 CORS。

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

同樣地，您也可以針對所有控制器啟用 CORS：

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a>啟用具有屬性的 CORS

[&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。 `[EnableCors]` 屬性會啟用所選結束點的 CORS，而不是所有結束點。

使用 `[EnableCors]` 指定預設原則，並 `[EnableCors("{Policy String}")]` 指定原則。

`[EnableCors]` 屬性可以套用至：

* Razor 頁面 `PageModel`
* 控制器
* 控制器動作方法

您可以使用 `[EnableCors]` 屬性，將不同的原則套用至控制器/頁面模型/動作。 當 `[EnableCors]` 屬性套用至控制器/頁面模型/動作方法，而且在中介軟體中啟用 CORS 時，就會套用這兩個原則。 我們建議您不要結合原則。 使用 `[EnableCors]` 屬性或中介軟體，而不是同時在相同的應用程式中。

下列程式碼會將不同的原則套用至每個方法：

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

下列程式碼會建立 CORS 預設原則和名為 `"AnotherPolicy"`的原則：

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>停用 CORS

[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性會停用控制器/頁面模型/動作的 CORS。

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS 原則選項

本節說明可在 CORS 原則中設定的各種選項：

* [設定允許的原始來源](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨原始來源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預檢到期時間](#set-the-preflight-expiration-time)

`Startup.ConfigureServices`會呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>。 針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。

## <a name="set-the-allowed-origins"></a>設定允許的原始來源

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許所有來源的 CORS 要求搭配任何配置（`http` 或 `https`）。 `AllowAnyOrigin` 並不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> 指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，可能會導致跨網站偽造要求。 當應用程式已設定這兩種方法時，CORS 服務會傳回不正確 CORS 回應。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> 指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，可能會導致跨網站偽造要求。 針對安全的應用程式，如果用戶端必須授權自己來存取伺服器資源，請指定來源的確切清單。

::: moniker-end

`AllowAnyOrigin` 會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

::: moniker range=">= aspnetcore-2.0"

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 會將原則的 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> 屬性設為函式，以便在評估是否允許來源時，讓原始來源符合已設定的萬用字元網域。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>：

* 允許任何 HTTP 方法：
* 會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

此設定會影響預檢要求和 `Access-Control-Request-Headers` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

::: moniker range=">= aspnetcore-2.2"

只有在 `Access-Control-Request-Headers` 中傳送的標頭完全符合 `WithHeaders`中所述的標頭時，才可以將 CORS 中介軟體原則符合 `WithHeaders` 指定的特定標頭。

例如，假設有一個應用程式設定如下：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中介軟體會拒絕具有下列要求標頭的預檢要求，因為 `Content-Language` （[HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)）未列在 `WithHeaders`中：

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。 因此，瀏覽器不會嘗試跨原始來源要求。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS 中介軟體一律允許傳送 `Access-Control-Request-Headers` 中的四個標頭，而不論 c 中設定的值為何。 此標頭清單包含：

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

例如，假設有一個應用程式設定如下：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中介軟體會以下列要求標頭成功回應預檢要求，因為 `Content-Language` 一律會列入允許清單：

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。 如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

預設可用的回應標頭為：

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 規格會呼叫這些標頭*簡單的回應標頭*。 若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>跨原始來源要求中的認證

認證需要在 CORS 要求中進行特殊處理。 根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。 認證包括 cookie 和 HTTP 驗證配置。 若要使用跨原始來源要求傳送認證，用戶端必須將 `XMLHttpRequest.withCredentials` 設定為 `true`。

直接使用 `XMLHttpRequest`：

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

使用 jQuery：

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

使用[FETCH API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)：

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

伺服器必須允許認證。 若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。

如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。

允許跨原始來源認證會有安全性風險。 另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS 規格也指出，如果 `Access-Control-Allow-Credentials` 標頭存在，設定 `"*"` （所有來源）的來源會無效。

### <a name="preflight-requests"></a>預檢要求

針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。 此要求稱為*預檢要求*。 當下列條件成立時，瀏覽器可以略過預檢要求：

* 要求方法為 GET、HEAD 或 POST。
* 應用程式不會設定 `Accept`、`Accept-Language`、`Content-Language`、`Content-Type`或 `Last-Event-ID`以外的要求標頭。
* 如果設定了 `Content-Type` 標頭，就會有下列其中一個值：
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

針對用戶端要求設定的要求標頭規則會套用至應用程式所設定的標頭，方法是呼叫 `XMLHttpRequest` 物件上的 `setRequestHeader`。 CORS 規格會呼叫這些標頭的*作者要求標頭*。 此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent`、`Host`或 `Content-Length`。

以下是預檢要求的範例：

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

預先飛行要求會使用 HTTP OPTIONS 方法。 其中包含兩個特殊標頭：

* `Access-Control-Request-Method`：將用於實際要求的 HTTP 方法。
* `Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。 如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent`。

CORS 預檢要求可能會包含 `Access-Control-Request-Headers` 標頭，這會向伺服器指出與實際要求一起傳送的標頭。

若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

瀏覽器在 `Access-Control-Request-Headers`中的設定方式並不完全一致。 如果您將標頭設定為不是 `"*"` （或使用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>）以外的任何專案，您應該至少包含 `Accept`、`Content-Type`和 `Origin`，加上您想要支援的任何自訂標頭。

以下是預檢要求的回應範例（假設伺服器允許此要求）：

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

回應包含 `Access-Control-Allow-Methods` 標頭，其中會列出允許的方法，並可選擇是否使用 `Access-Control-Allow-Headers` 標頭，其中會列出允許的標頭。 如果預檢要求成功，瀏覽器就會傳送實際的要求。

如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。 因此，瀏覽器不會嘗試跨原始來源要求。

### <a name="set-the-preflight-expiration-time"></a>設定預檢到期時間

`Access-Control-Max-Age` 標頭會指定可以快取對預檢要求回應的時間長度。 若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS 的運作方式

本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求中會發生什麼事。

* CORS**不**是安全性功能。 CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。
  * 例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。
* 藉由允許 CORS，您的 API 不會更安全。
  * 而是由用戶端（瀏覽器）強制執行 CORS。 伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。 例如，下列任何一項工具都會顯示伺服器回應：
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * 網頁瀏覽器，方法是在網址列中輸入 URL。
* 這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)要求的方式，否則會禁止。
  * 瀏覽器（不含 CORS）無法執行跨原始來源要求。 在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。 JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。 允許跨原始來源載入腳本。

[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。 如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。 不需要自訂 JavaScript 程式碼來啟用 CORS。

以下是跨原始來源要求的範例。 `Origin` 標頭會提供提出要求之網站的網域。 `Origin` 標頭是必要的，而且必須與主機不同。

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

如果伺服器允許此要求，它會在回應中設定 `Access-Control-Allow-Origin` 標頭。 此標頭的值會比對要求中的 `Origin` 標頭，或為 `"*"`的萬用字元值，這表示允許任何來源：

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

如果回應未包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。 具體而言，瀏覽器不允許此要求。 即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。

<a name="test"></a>

## <a name="test-cors"></a>測試 CORS

測試 CORS：

1. [建立 API 專案](xref:tutorials/first-web-api)。 或者，您可以[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。
1. 使用本檔中的其中一種方法來啟用 CORS。 例如：

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");` 應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。

1. 建立 web 應用程式專案（Razor Pages 或 MVC）。 此範例會使用 Razor Pages。 您可以在與 API 專案相同的方案中建立 web 應用程式。
1. 將下列反白顯示的程式碼新增至*Index. cshtml*檔案：

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. 在上述程式碼中，將 `url: 'https://<web app>.azurewebsites.net/api/values/1',` 取代為已部署應用程式的 URL。
1. 部署 API 專案。 例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。
1. 從桌面執行 Razor Pages 或 MVC 應用程式，然後按一下 [**測試**] 按鈕。 使用 F12 工具來檢查錯誤訊息。
1. 從 `WithOrigins` 移除 localhost 來源，並部署應用程式。 或者，使用不同的埠來執行用戶端應用程式。 例如，從 Visual Studio 執行。
1. 使用用戶端應用程式進行測試。 CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。 使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。 視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：

   * 使用 Microsoft Edge：

     **SEC7120： [CORS] 來源 `https://localhost:44375` 在跨原始來源資源的存取控制-允許來源回應標頭中找不到 `https://localhost:44375` `https://webapi.azurewebsites.net/api/values/1`**

   * 使用 Chrome：

     **來源 `https://localhost:44375` 的 `https://webapi.azurewebsites.net/api/values/1` 存取 XMLHttpRequest 已被 CORS 原則封鎖：要求的資源上沒有任何「存取控制-允許來源」標頭。**
     
具有 CORS 功能的端點可以使用工具（例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。 使用工具時，`Origin` 標頭所指定之要求的來源，必須與接收要求的主機不同。 如果要求不是根據 `Origin` 標頭的值而*跨原始來源*：

* CORS 中介軟體不需要處理要求。
* 回應中不會傳回 CORS 標頭。

## <a name="additional-resources"></a>其他資源

* [跨原始來源資源分享（CORS）](https://developer.mozilla.org/docs/Web/HTTP/CORS)
