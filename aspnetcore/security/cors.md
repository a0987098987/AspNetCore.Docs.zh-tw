---
title: 啟用 ASP.NET Core 中的跨原始來源要求（CORS）
author: rick-anderson
description: 瞭解 CORS 如何作為標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始來源要求。
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/cors
ms.openlocfilehash: 0a2be31092ab491e23ab9de9be676b5b4d3963ee
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86060276"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>啟用 ASP.NET Core 中的跨原始來源要求（CORS）

::: moniker range=">= aspnetcore-3.0"

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)

本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。 如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/docs/Web/HTTP/CORS)。

[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：

* 是一種 W3C 標準，可讓伺服器放寬相同的來源原則。
* **不**是安全性功能，CORS 放寬安全性。 藉由允許 CORS，API 不會更安全。 如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。
* 允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。
* 比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="same-origin"></a>相同的來源

如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。

這兩個 Url 具有相同的來源：

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些 Url 的來源不同于前兩個 Url：

* `https://example.net`：不同的網域
* `https://www.example.com/foo.html`：不同的子域
* `http://example.com/foo.html`：不同的配置
* `https://example.com:9000/foo.html`：不同的埠

## <a name="enable-cors"></a>啟用 CORS

有三種方式可啟用 CORS：

* 在中介軟體中，使用[已命名的原則](#np)或[預設原則](#dp)。
* 使用[端點路由](#ecors)。
* 具有[[EnableCors]](#attr)屬性的。

將[[EnableCors]](#attr)屬性與已命名的原則搭配使用，可在限制支援 CORS 的端點時提供最精細的控制。

> [!WARNING]
> <xref:Owin.CorsExtensions.UseCors%2A>在使用之前，必須先呼叫 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A> `UseResponseCaching` 。

下列各節將詳細說明每一種方法。

<a name="np"></a>

## <a name="cors-with-named-policy-and-middleware"></a>具有已命名原則和中介軟體的 CORS

CORS 中介軟體會處理跨原始來源要求。 下列程式碼會將 CORS 原則套用至具有指定來源的所有應用程式端點：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=3,9,32)]

上述程式碼：

* 將原則名稱設定為 `_myAllowSpecificOrigins` 。 原則名稱是任意的。
* 呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，並指定 `_myAllowSpecificOrigins` CORS 原則。 `UseCors`新增 CORS 中介軟體。 對的呼叫 `UseCors` 必須放在之後 `UseRouting` （但之前） `UseAuthorization` 。 如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#middleware-order)。
* <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的呼叫。 Lambda 會接受 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。 [Configuration options](#cors-policy-options) `WithOrigins` 這篇文章稍後會說明設定選項，例如。
* 啟用 `_myAllowSpecificOrigins` 所有控制器端點的 CORS 原則。 請參閱[端點路由](#ecors)以將 CORS 原則套用至特定端點。
* 使用回應快取[中介軟體](xref:performance/caching/middleware)時，請 <xref:Owin.CorsExtensions.UseCors%2A> 在之前呼叫 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A> 。

使用端點路由，**必須**將 CORS 中介軟體設定為在呼叫和之間執行 `UseRouting` `UseEndpoints` 。

如需測試程式碼的相關指示，請參閱[測試 CORS](#testc) ，類似于上述程式碼。

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將 CORS 服務新增至應用程式的服務容器：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet2)]

如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以連結，如下列程式碼所示：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup2.cs?name=snippet)]

注意：指定的 URL**不**能包含尾端斜線（ `/` ）。 如果 URL 以結束 `/` ，則比較會 `false` 傳回，而且不會傳回任何標頭。

<a name="dp"></a>

### <a name="cors-with-default-policy-and-middleware"></a>具有預設原則和中介軟體的 CORS

下列反白顯示的程式碼會啟用預設 CORS 原則：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupDefaultPolicy.cs?name=snippet2&highlight=7,29)]

上述程式碼會將預設 CORS 原則套用至所有控制器端點。

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a>使用端點路由來啟用 Cors

使用時，以每個端點為基礎啟用 CORS `RequireCors` ，目前**不**支援[自動預檢要求](#apf)。 如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/aspnetcore/issues/20709)和[使用端點路由測試 CORS 和 [HttpOptions]](#tcer)。

使用端點路由，可以使用一組擴充方法，針對每個端點來啟用 CORS <xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPt.cs?name=snippet2&highlight=3,7-15,32,40,43)]

在上述程式碼中：

* `app.UseCors`啟用 CORS 中介軟體。 因為尚未設定預設原則，所以 `app.UseCors()` 單獨不會啟用 CORS。
* `/echo`和控制器端點允許使用指定的原則進行跨原始來源要求。
* `/echo2`和 Razor 頁面端點**不**允許跨原始來源要求，因為未指定預設原則。

[[DisableCors]](#dc) **屬性不會停用**端點路由所啟用的 CORS `RequireCors` 。

如需測試類似上述程式碼的指示，請參閱[使用端點路由和 [HttpOptions] 測試 CORS](#tcer) 。

<a name="attr"></a>

## <a name="enable-cors-with-attributes"></a>啟用具有屬性的 CORS

使用[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性啟用 CORS，並將已命名的原則套用至只有需要 CORS 的端點可提供最精細的控制。

[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。 `[EnableCors]`屬性會啟用所選端點的 CORS，而不是所有端點：

* `[EnableCors]`指定預設原則。
* `[EnableCors("{Policy String}")]`指定已命名的原則。

`[EnableCors]`屬性可套用至：

* Razor本頁`PageModel`
* 控制器
* 控制器動作方法

您可以使用屬性，將不同的原則套用至控制器、頁面模型或動作方法 `[EnableCors]` 。 當 `[EnableCors]` 屬性套用至控制器、頁面模型或動作方法，而且在中介軟體中啟用 CORS 時，會套用**這兩個**原則。 **我們建議您不要結合原則。使用** `[EnableCors]` **屬性或中介軟體，而不是同時在相同的應用程式中。**

下列程式碼會將不同的原則套用至每個方法：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

下列程式碼會建立兩個 CORS 原則：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup3.cs?name=snippet&highlight=12-28,44)]

針對限制 CORS 要求的最精細控制：

* 使用 `[EnableCors("MyPolicy")]` 具有已命名原則的。
* 不要定義預設原則。
* 請勿使用[端點路由](#ecors)。

下一節中的程式碼會符合前述清單。

如需測試程式碼的相關指示，請參閱[測試 CORS](#testc) ，類似于上述程式碼。

<a name="dc"></a>

### <a name="disable-cors"></a>停用 CORS

[[DisableCors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) **屬性不會停用**[端點路由](#ecors)已啟用的 CORS。

下列程式碼會定義 CORS 原則 `"MyPolicy"` ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTestMyPolicy.cs?name=snippet)]

下列程式碼會停用動作的 CORS `GetValues2` ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet&highlight=1,23)]

上述程式碼：

* 不會使用[端點路由](#ecors)來啟用 CORS。
* 不會定義[預設的 CORS 原則](#dp)。
* 使用[[EnableCors （"MyPolicy"）]](#attr)來啟用 `"MyPolicy"` 控制器的 CORS 原則。
* 停用方法的 CORS `GetValues2` 。

如需測試上述程式碼的指示，請參閱[測試 CORS](#testc) 。

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS 原則選項

本節說明可在 CORS 原則中設定的各種選項：

* [設定允許的原始來源](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨原始來源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預檢到期時間](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>會在中呼叫 `Startup.ConfigureServices` 。 針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。

## <a name="set-the-allowed-origins"></a>設定允許的原始來源

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>：允許使用任何配置（或）來自所有來源的 CORS 要求 `http` `https` 。 `AllowAnyOrigin`不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。

> [!NOTE]
> 指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，而且可能會導致跨網站偽造要求。 當應用程式已設定這兩種方法時，CORS 服務會傳回不正確 CORS 回應。

`AllowAnyOrigin`會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>：將原則的屬性設定為函式 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> ，以便在評估是否允許來源時，允許來源符合已設定的萬用字元網域。

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet)]

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* 允許任何 HTTP 方法：
* 會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

若要允許在 CORS 要求中傳送特定標頭（稱為[author 要求標頭](https://xhr.spec.whatwg.org/#request)），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

若要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

`AllowAnyHeader`會影響預檢要求和[存取控制要求](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)標頭標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

`WithHeaders`只有在 `Access-Control-Request-Headers` 完全符合中所述標頭的情況下傳送的標頭時，才可以將 CORS 中介軟體原則與指定的特定標頭比對 `WithHeaders` 。

例如，假設有一個應用程式設定如下：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet4)]

CORS 中介軟體會拒絕具有下列要求標頭的預檢要求，因為 `Content-Language` （[HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)）未列于 `WithHeaders` ：

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。 因此，瀏覽器不會嘗試跨原始來源要求。

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。 如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

預設可用的回應標頭為：

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 規格會呼叫這些標頭*簡單的回應標頭*。 若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet5)]
### <a name="credentials-in-cross-origin-requests"></a>跨原始來源要求中的認證

認證需要在 CORS 要求中進行特殊處理。 根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。 認證包括 cookie 和 HTTP 驗證配置。 若要使用跨原始來源要求傳送認證，用戶端必須將設 `XMLHttpRequest.withCredentials` 為 `true` 。

`XMLHttpRequest`直接使用：

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

使用[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)：

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

伺服器必須允許認證。 若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet6)]

HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。

如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。

允許跨原始來源認證會有安全性風險。 另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS 規格也會指出如果標頭存在，將原始來源設定為 `"*"` （所有來源）是不正確 `Access-Control-Allow-Credentials` 。

<a name="pref"></a>

## <a name="preflight-requests"></a>預檢要求

針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的[選項](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)要求。 此要求稱為[預檢要求](https://developer.mozilla.org/docs/Glossary/Preflight_request)。 如果下列**所有**條件都成立，瀏覽器就可以略過預檢要求：

* 要求方法為 GET、HEAD 或 POST。
* 應用程式不會設定、、、或以外的要求標頭 `Accept` `Accept-Language` `Content-Language` `Content-Type` `Last-Event-ID` 。
* `Content-Type`如果設定了標頭，則會有下列其中一個值：
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

針對用戶端要求設定的要求標頭規則會套用至應用程式透過在物件上呼叫所設定的標頭 `setRequestHeader` `XMLHttpRequest` 。 CORS 規格會呼叫這些標頭的[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)。 此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent` 、 `Host` 或 `Content-Length` 。

以下是範例回應，類似于此檔的[測試 CORS](#testc)一節中 **[Put test]** 按鈕所提出的預檢要求。

```
General:
Request URL: https://cors3.azurewebsites.net/api/values/5
Request Method: OPTIONS
Status Code: 204 No Content

Response Headers:
Access-Control-Allow-Methods: PUT,DELETE,GET
Access-Control-Allow-Origin: https://cors1.azurewebsites.net
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f8...8;Path=/;HttpOnly;Domain=cors1.azurewebsites.net
Vary: Origin

Request Headers:
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Access-Control-Request-Method: PUT
Connection: keep-alive
Host: cors3.azurewebsites.net
Origin: https://cors1.azurewebsites.net
Referer: https://cors1.azurewebsites.net/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0
```

預檢要求會使用[HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)方法。 其中可能包含下列標頭：

* [存取控制-要求-方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)：將用於實際要求的 HTTP 方法。
* [存取控制-要求-標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)：應用程式在實際要求上設定的要求標頭清單。 如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent` 。
* [存取控制-允許方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

如果預檢要求遭到拒絕，應用程式會傳回 `200 OK` 回應，但不會設定 CORS 標頭。 因此，瀏覽器不會嘗試跨原始來源要求。 如需拒絕預檢要求的範例，請參閱本檔的[測試 CORS](#testc)一節。

使用 F12 工具時，主控台應用程式會顯示類似下列其中一項的錯誤，視瀏覽器而定：

* Firefox：跨原始來源要求已封鎖：相同的來源原則不允許讀取位於的遠端資源 `https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5` 。 （原因： CORS 要求未成功）。 [深入了解](https://developer.mozilla.org/docs/Web/HTTP/CORS/Errors/CORSDidNotSucceed)
* 以 Chromium 為基礎：從來源 ' ' 提取 ' ' 的存取權已 https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5 https://cors3.azurewebsites.net 遭 CORS 原則封鎖：對預檢要求的回應未通過存取控制檢查：要求的資源上沒有任何「存取控制-允許-來源」標頭。 如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。

若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

若要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

瀏覽器的設定方式並不一致 `Access-Control-Request-Headers` 。 如果其中一項：

* 標頭會設定為以外的任何專案`"*"`
* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>會呼叫：至少包含 `Accept` 、 `Content-Type` 和 `Origin` ，再加上您想要支援的任何自訂標頭。

<a name="apf"></a>

### <a name="automatic-preflight-request-code"></a>自動預檢要求碼

套用 CORS 原則時：

* `app.UseCors`在中呼叫以全域方式 `Startup.Configure` 。
* 使用 `[EnableCors]` 屬性。

ASP.NET Core 回應預檢 OPTIONS 要求。

使用時，以每個端點為基礎啟用 CORS `RequireCors` ，目前**不**支援自動預檢要求。

本檔的[測試 CORS](#testc)一節會示範此行為。

<a name="pro"></a>

### <a name="httpoptions-attribute-for-preflight-requests"></a>[HttpOptions] 預檢要求的屬性

以適當的原則啟用 CORS 時，ASP.NET Core 通常會自動回應 CORS 預檢要求。 在某些情況下，這可能不是這種情況。 例如，搭配使用[CORS 與端點路由](#ecors)。

下列程式碼會使用[[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute)屬性來建立選項要求的端點：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet&highlight=5-17)]

如需測試上述程式碼的指示，請參閱[使用端點路由和 [HttpOptions] 測試 CORS](#tcer) 。

### <a name="set-the-preflight-expiration-time"></a>設定預檢到期時間

`Access-Control-Max-Age`標頭會指定快取回應要求的時間長度。 若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*> ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet7)]
<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS 的運作方式

本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)要求中會發生什麼事。

* CORS**不**是安全性功能。 CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。
  * 例如，惡意執行者可以對您的網站使用[跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。
* 藉由允許 CORS，API 不會更安全。
  * 而是由用戶端（瀏覽器）強制執行 CORS。 伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。 例如，下列任何一項工具都會顯示伺服器回應：
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * 網頁瀏覽器，方法是在網址列中輸入 URL。
* 這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)要求的方式，否則會禁止。
  * 沒有 CORS 的瀏覽器無法執行跨原始來源要求。 在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。 JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。 允許跨原始來源載入腳本。

[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。 如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。 不需要自訂 JavaScript 程式碼來啟用 CORS。

已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)上的 [ [PUT test] 按鈕](https://cors3.azurewebsites.net/test)

以下是從 [[值](https://cors3.azurewebsites.net/)] [測試] 按鈕到的跨原始來源要求的範例 `https://cors1.azurewebsites.net/api/values` 。 `Origin`標頭：

* 提供提出要求的網站網域。
* 是必要的，而且必須與主機不同。

**一般標頭**

```
Request URL: https://cors1.azurewebsites.net/api/values
Request Method: GET
Status Code: 200 OK
```

**回應標頭**

```
Content-Encoding: gzip
Content-Type: text/plain; charset=utf-8
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors1.azurewebsites.net
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Powered-By: ASP.NET
```

**要求標頭**

```
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
Host: cors1.azurewebsites.net
Origin: https://cors3.azurewebsites.net
Referer: https://cors3.azurewebsites.net/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0 ...
```

在 `OPTIONS` 要求中，伺服器會在回應中設定**回應標頭** `Access-Control-Allow-Origin: {allowed origin}` 標頭。 例如，已部署的[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) [Delete [EnableCors]](https://cors1.azurewebsites.net/test?number=2)按鈕 `OPTIONS` 要求包含下列標頭：

**一般標頭**

```
Request URL: https://cors3.azurewebsites.net/api/TodoItems2/MyDelete2/5
Request Method: OPTIONS
Status Code: 204 No Content
```

**回應標頭**

```
Access-Control-Allow-Headers: Content-Type,x-custom-header
Access-Control-Allow-Methods: PUT,DELETE,GET,OPTIONS
Access-Control-Allow-Origin: https://cors1.azurewebsites.net
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors3.azurewebsites.net
Vary: Origin
X-Powered-By: ASP.NET
```

**要求標頭**

```
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Access-Control-Request-Headers: content-type
Access-Control-Request-Method: DELETE
Connection: keep-alive
Host: cors3.azurewebsites.net
Origin: https://cors1.azurewebsites.net
Referer: https://cors1.azurewebsites.net/test?number=2
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0
```

在上述**回應標頭**中，伺服器會在回應中設定[存取控制-允許來源](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)標頭。 `https://cors1.azurewebsites.net`此標頭的值符合 `Origin` 要求的標頭。

如果 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> 呼叫，則 `Access-Control-Allow-Origin: *` 會傳回萬用字元值。 `AllowAnyOrigin`允許任何來源。

如果回應不包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。 具體而言，瀏覽器不允許此要求。 即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。

<a name="options"></a>

### <a name="display-options-requests"></a>顯示選項要求

根據預設，Chrome 和 Edge 瀏覽器不會在 F12 工具的 [網路] 索引標籤上顯示 [選項要求]。 若要在這些瀏覽器中顯示選項要求：

* `chrome://flags/#out-of-blink-cors` 或 `edge://flags/#out-of-blink-cors`
* 停用旗標。
* 後.

Firefox 預設會顯示選項要求。

## <a name="cors-in-iis"></a>IIS 中的 CORS

部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。 若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。

<a name="testc"></a>

## <a name="test-cors"></a>測試 CORS

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)包含測試 CORS 的程式碼。 請參閱[如何下載](xref:index#how-to-download-a-sample)。 範例是已加入頁面的 API 專案 Razor ：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTest2.cs?name=snippet2)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors)。

以下 `ValuesController` 提供測試的端點：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet)]

[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs)是由Rick.Doc的[RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet 封裝所提供，並顯示路由資訊。

使用下列其中一種方法來測試前面的範例程式碼：

* 使用在上部署的範例應用程式 [https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/) 。 不需要下載範例。
* `dotnet run`使用的預設 URL 來執行範例 `https://localhost:5001` 。
* 從 Visual Studio 執行範例，並將的埠設定為44398，以取得的 URL `https://localhost:44398` 。

使用具有 F12 工具的瀏覽器：

* 選取 [**值**] 按鈕，並查看 [**網路**] 索引標籤中的標頭。
* 選取 [ **PUT test** ] 按鈕。 如需顯示選項要求的指示，請參閱[顯示選項要求](#options)。 **PUT 測試**會建立兩個要求，一個選項預檢要求和 PUT 要求。
* 選取 [] **`GetValues2 [DisableCors]`** 按鈕以觸發失敗的 CORS 要求。 如檔中所述，回應會傳回200成功，但不會進行 CORS 要求。 選取 [**主控台**] 索引標籤，以查看 CORS 錯誤。 視瀏覽器而定，會顯示類似下列的錯誤：

     `'https://cors1.azurewebsites.net/api/values/GetValues2'`CORS 原則已封鎖從原始來源提取的存取權 `'https://cors3.azurewebsites.net'` ：要求的資源上沒有任何「存取控制-允許來源」標頭。 如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。
     
已啟用 CORS 的端點可以使用工具（例如，[捲曲](https://curl.haxx.se/)、 [Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。 使用工具時，標頭所指定的要求原點必須與 `Origin` 接收要求的主機不同。 如果要求不是根據標頭的值而*跨原始來源* `Origin` ：

* CORS 中介軟體不需要處理要求。
* 回應中不會傳回 CORS 標頭。

下列命令會使用 `curl` 來發出具有下列資訊的選項要求：

```bash
curl -X OPTIONS https://cors3.azurewebsites.net/api/TodoItems2/5 -i
```

<!--
curl come with Git. Add to path variable
C:\Program Files\Git\mingw64\bin\
-->

<a name="tcer"></a>

### <a name="test-cors-with-endpoint-routing-and-httpoptions"></a>使用端點路由和 [HttpOptions] 測試 CORS

使用時，以每個端點為基礎啟用 CORS `RequireCors` ，目前**不**支援[自動預檢要求](#apf)。 請考慮使用[端點路由來啟用 CORS](#ecors)的下列程式碼：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPointBugTest.cs?name=snippet2)]

以下 `TodoItems1Controller` 提供測試的端點：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems1Controller.cs?name=snippet2)]

從已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的 [[測試] 頁面](https://cors1.azurewebsites.net/test?number=1)測試上述程式碼。

**Delete [EnableCors]** 和**GET [EnableCors]** 按鈕成功，因為端點有 `[EnableCors]` 並回應預檢要求。 其他端點失敗。 [**取得**] 按鈕會失敗，因為[JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js)會傳送：

```javascript
 headers: {
      "Content-Type": "x-custom-header"
 },
```

以下 `TodoItems2Controller` 提供類似的端點，但包含明確的程式碼來回應選項要求：

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet2)]

從已部署範例的 [[測試] 頁面](https://cors1.azurewebsites.net/test?number=2)測試上述程式碼。 在 [**控制器**] 下拉式清單中，選取 [**預檢**]，然後**設定 [控制器**]。 所有對端點的 CORS 呼叫都會 `TodoItems2Controller` 成功。

## <a name="additional-resources"></a>其他資源

* [跨原始來源資源分享 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [IIS CORS 模組使用者入門](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。

瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。 這種限制稱為「*相同來源原則*」。 相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。 有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。 如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/docs/Web/HTTP/CORS)。

[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：

* 是一種 W3C 標準，可讓伺服器放寬相同的來源原則。
* **不**是安全性功能，CORS 放寬安全性。 藉由允許 CORS，API 不會更安全。 如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。
* 允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。
* 比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="same-origin"></a>相同的來源

如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。

這兩個 Url 具有相同的來源：

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些 Url 的來源不同于前兩個 Url：

* `https://example.net`：不同的網域
* `https://www.example.com/foo.html`：不同的子域
* `http://example.com/foo.html`：不同的配置
* `https://example.com:9000/foo.html`：不同的埠

在比較原始來源時，Internet Explorer 不會考慮此埠。

## <a name="cors-with-named-policy-and-middleware"></a>具有已命名原則和中介軟體的 CORS

CORS 中介軟體會處理跨原始來源要求。 下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

上述程式碼：

* 將原則名稱設定為 " \_ myAllowSpecificOrigins"。 原則名稱是任意的。
* 呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，以啟用 CORS。
* <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的呼叫。 Lambda 會接受 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。 [Configuration options](#cors-policy-options) `WithOrigins` 這篇文章稍後會說明設定選項，例如。

<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將 CORS 服務新增至應用程式的服務容器：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以連鎖方法，如下列程式碼所示：

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

注意： URL**不**能包含尾端斜線（ `/` ）。 如果 URL 以結束 `/` ，則比較會 `false` 傳回，而且不會傳回任何標頭。

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
注意： `UseCors` 必須在之前呼叫 `UseMvc` 。

請參閱[在頁面 Razor 、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。

如需測試程式碼的相關指示，請參閱[測試 CORS](#test) ，類似于上述程式碼。

## <a name="enable-cors-with-attributes"></a>啟用具有屬性的 CORS

[ &lbrack; EnableCors &rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。 `[EnableCors]`屬性會啟用所選結束點的 CORS，而不是所有結束點。

使用 `[EnableCors]` 指定預設原則，並 `[EnableCors("{Policy String}")]` 指定原則。

`[EnableCors]`屬性可套用至：

* Razor本頁`PageModel`
* 控制器
* 控制器動作方法

您可以使用屬性，將不同的原則套用至控制器/頁面模型/動作 `[EnableCors]` 。 當 `[EnableCors]` 屬性套用至控制器/頁面模型/動作方法，且在中介軟體中啟用 CORS 時，會套用**這兩個**原則。 我們建議您**不要**結合原則。 請使用 `[EnableCors]` 屬性或中介軟體，**而不是兩者**。 使用時 `[EnableCors]` ，請勿**not**定義預設原則。

下列程式碼會將不同的原則套用至每個方法：

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

下列程式碼會建立 CORS 預設原則和名為的原則 `"AnotherPolicy"` ：

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>停用 CORS

[ &lbrack; DisableCors &rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性會停用控制器/頁面模型/動作的 CORS。

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS 原則選項

本節說明可在 CORS 原則中設定的各種選項：

* [設定允許的原始來源](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨原始來源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預檢到期時間](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>會在中呼叫 `Startup.ConfigureServices` 。 針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。

## <a name="set-the-allowed-origins"></a>設定允許的原始來源

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>：允許使用任何配置（或）來自所有來源的 CORS 要求 `http` `https` 。 `AllowAnyOrigin`不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。

> [!NOTE]
> 指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，而且可能會導致跨網站偽造要求。 針對安全的應用程式，如果用戶端必須授權自己來存取伺服器資源，請指定來源的確切清單。

`AllowAnyOrigin`會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>：將原則的屬性設定為函式 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> ，以便在評估是否允許來源時，允許來源符合已設定的萬用字元網域。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* 允許任何 HTTP 方法：
* 會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

此設定會影響預檢要求和 `Access-Control-Request-Headers` 標頭。 如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。

CORS 中介軟體一律允許傳送中的四個標頭， `Access-Control-Request-Headers` 而不論 c 中設定的值為何。 此標頭清單包含：

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

例如，假設有一個應用程式設定如下：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中介軟體會以下列要求標頭成功回應預檢要求，因為 `Content-Language` 一律允許：

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。 如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

預設可用的回應標頭為：

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 規格會呼叫這些標頭*簡單的回應標頭*。 若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*> ：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>跨原始來源要求中的認證

認證需要在 CORS 要求中進行特殊處理。 根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。 認證包括 cookie 和 HTTP 驗證配置。 若要使用跨原始來源要求傳送認證，用戶端必須將設 `XMLHttpRequest.withCredentials` 為 `true` 。

`XMLHttpRequest`直接使用：

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

使用[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)：

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

伺服器必須允許認證。 若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*> ：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。

如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。

允許跨原始來源認證會有安全性風險。 另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS 規格也會指出如果標頭存在，將原始來源設定為 `"*"` （所有來源）是不正確 `Access-Control-Allow-Credentials` 。

### <a name="preflight-requests"></a>預檢要求

針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。 此要求稱為*預檢要求*。 當下列條件成立時，瀏覽器可以略過預檢要求：

* 要求方法為 GET、HEAD 或 POST。
* 應用程式不會設定、、、或以外的要求標頭 `Accept` `Accept-Language` `Content-Language` `Content-Type` `Last-Event-ID` 。
* `Content-Type`如果設定了標頭，則會有下列其中一個值：
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

針對用戶端要求設定的要求標頭規則會套用至應用程式透過在物件上呼叫所設定的標頭 `setRequestHeader` `XMLHttpRequest` 。 CORS 規格會呼叫這些標頭的*作者要求標頭*。 此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent` 、 `Host` 或 `Content-Length` 。

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
* `Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。 如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent` 。

<!-- I think this needs to be removed, was put here accidently -->

以適當的原則啟用 CORS 時，ASP.NET Core 通常會自動回應 CORS 預檢要求。 請參閱[預檢要求的 [HttpOptions] 屬性](#pro)。

CORS 預檢要求可能會包含 `Access-Control-Request-Headers` 標頭，這會向伺服器指出與實際要求一起傳送的標頭。

若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

瀏覽器的設定方式並不完全一致 `Access-Control-Request-Headers` 。 如果您將標頭設定為 `"*"` （或使用）以外的任何專案 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*> ，您應該至少包含 `Accept` 、 `Content-Type` 和，再 `Origin` 加上您想要支援的任何自訂標頭。

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

回應 `Access-Control-Allow-Methods` 會包含標頭，其中會列出允許的方法，並選擇性地列出 `Access-Control-Allow-Headers` 標頭，其中會列出允許的標頭。 如果預檢要求成功，瀏覽器就會傳送實際的要求。

如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。 因此，瀏覽器不會嘗試跨原始來源要求。

### <a name="set-the-preflight-expiration-time"></a>設定預檢到期時間

`Access-Control-Max-Age`標頭會指定快取回應要求的時間長度。 若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*> ：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS 的運作方式

本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)要求中會發生什麼事。

* CORS**不**是安全性功能。 CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。
  * 例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。
* 藉由允許 CORS，您的 API 不會更安全。
  * 而是由用戶端（瀏覽器）強制執行 CORS。 伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。 例如，下列任何一項工具都會顯示伺服器回應：
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HttpClient](/dotnet/csharp/tutorials/console-webapiclient)
    * 網頁瀏覽器，方法是在網址列中輸入 URL。
* 這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)要求的方式，否則會禁止。
  * 瀏覽器（不含 CORS）無法執行跨原始來源要求。 在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。 JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。 允許跨原始來源載入腳本。

[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。 如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。 不需要自訂 JavaScript 程式碼來啟用 CORS。

以下是跨原始來源要求的範例。 `Origin`標頭會提供提出要求之網站的網域。 `Origin`標頭是必要的，而且必須與主機不同。

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

如果伺服器允許此要求，它會 `Access-Control-Allow-Origin` 在回應中設定標頭。 此標頭的值 `Origin` 會比對要求中的標頭，或為萬用字元值 `"*"` ，表示允許任何來源：

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

如果回應不包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。 具體而言，瀏覽器不允許此要求。 即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。

<a name="test"></a>

## <a name="test-cors"></a>測試 CORS

測試 CORS：

1. [建立 API 專案](xref:tutorials/first-web-api)。 或者，您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。
1. 使用本檔中的其中一種方法來啟用 CORS。 例如：

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。

1. 建立 web 應用程式專案（ Razor 頁面或 MVC）。 此範例會使用 Razor 頁面。 您可以在與 API 專案相同的方案中建立 web 應用程式。
1. 將下列反白顯示的程式碼新增至*Index. cshtml*檔案：

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. 在上述程式碼中，將取代為已 `url: 'https://<web app>.azurewebsites.net/api/values/1',` 部署應用程式的 URL。
1. 部署 API 專案。 例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。
1. Razor從桌面執行頁面或 MVC 應用程式，然後按一下 [**測試**] 按鈕。 使用 F12 工具來檢查錯誤訊息。
1. 從移除 localhost 來源 `WithOrigins` ，並部署應用程式。 或者，使用不同的埠來執行用戶端應用程式。 例如，從 Visual Studio 執行。
1. 使用用戶端應用程式進行測試。 CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。 使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。 視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：

   * 使用 Microsoft Edge：

     **SEC7120： [CORS] 來源在 `https://localhost:44375` `https://localhost:44375` 的跨原始來源資源的存取控制-允許來源回應標頭中找不到`https://webapi.azurewebsites.net/api/values/1`**

   * 使用 Chrome：

     **`https://webapi.azurewebsites.net/api/values/1` `https://localhost:44375` CORS 原則已封鎖從來源存取 XMLHttpRequest：要求的資源上沒有任何「存取控制-允許來源」標頭。**
     
具有 CORS 功能的端點可以使用工具（例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。 使用工具時，標頭所指定的要求原點必須與 `Origin` 接收要求的主機不同。 如果要求不是根據標頭的值而*跨原始來源* `Origin` ：

* CORS 中介軟體不需要處理要求。
* 回應中不會傳回 CORS 標頭。

## <a name="cors-in-iis"></a>IIS 中的 CORS

部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。 若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。

## <a name="additional-resources"></a>其他資源

* [跨原始來源資源分享 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [IIS CORS 模組使用者入門](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end
