---
title: 在 ASP.NET 核心中開啟跨源要求 (CORS)
author: rick-anderson
description: 瞭解 CORS 如何作為在 ASP.NET 酷應用中允許或拒絕跨源請求的標準。
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
uid: security/cors
ms.openlocfilehash: 56a339d9018f619af38aecc6f4c2ff40c3c43d2f
ms.sourcegitcommit: 3d07e21868dafc503530ecae2cfa18a7490b58a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642690"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>在 ASP.NET 核心中開啟跨源要求 (CORS)

::: moniker range=">= aspnetcore-3.0"

由[里克·安德森](https://twitter.com/RickAndMSFT)和[柯克·拉金](https://twitter.com/serpent5)

本文演示如何在ASP.NET核心應用中啟用 CORS。

流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。 此限制稱為*同源原則*。 同源策略可防止惡意網站從其他網站讀取敏感數據。 有時,您可能希望允許其他網站向你的應用發出交叉源請求。 有關詳細資訊,請參閱[Mozilla CORS 文章](https://developer.mozilla.org/docs/Web/HTTP/CORS)。

[跨源資源分享](https://www.w3.org/TR/cors/)(CORS):

* 是允許伺服器放鬆同源策略的 W3C 標準。
* **CORS 不是**安全功能,可放鬆安全性。 通過允許 CORS,API 並不更安全。 有關詳細資訊,請參閱[CORS 的工作原理](#how-cors)。
* 允許伺服器顯式允許某些跨源請求,同時拒絕其他請求。
* 比早期的技術(如[JSONP](/dotnet/framework/wcf/samples/jsonp))更安全、更靈活。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)([如何下載](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>同一源

如果兩個 URL 具有相同的方案、主機和埠[(RFC 6454),](https://tools.ietf.org/html/rfc6454)則它們具有相同的源源。

這兩個網址 有相同的來源:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些網址與前兩個網址具有不同的來源:

* `https://example.net`&ndash;不同的網域
* `https://www.example.com/foo.html`&ndash;不同的子域
* `http://example.com/foo.html`&ndash;不同的機制
* `https://example.com:9000/foo.html`&ndash;不同的連接埠

## <a name="enable-cors"></a>啟用 CORS

有三種方法可以啟用 CORS:

* 在中間件中使用[命名政策](#np)或[預設政策](#dp)。
* 使用[終結點路由](#ecors)。
* 使用[[啟用 Cors]](#attr)屬性。

將[[EnableCors]](#attr)屬性與命名策略一起使用,可在限制支援 CORS 的終結點時提供最佳控制。

每種方法都詳見以下各節。

<a name="np"></a>

## <a name="cors-with-named-policy-and-middleware"></a>包含命名政策與中間件的 CORS

CORS 中間件處理跨源請求。 以下代碼將 CORS 政策應用於具有指定來源的所有應用終結點:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=3,9,31)]

上述程式碼：

* 將策略名稱設定到`_myAllowSpecificOrigins`。 策略名稱是任意的。
* 調用<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>擴充方法`_myAllowSpecificOrigins`並指定 CORS 策略。 `UseCors`添加 CORS 中間件。 呼叫`UseCors`後`UseRouting`必須發出`UseAuthorization`,但之前 。 有關詳細資訊,請參閱[中間件訂單](xref:fundamentals/middleware/index#middleware-order)。
* 使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的調用。 lambda 取<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>一個物件。 [配置選項](#cors-policy-options)(`WithOrigins`如 )在本文的後面部分介紹。
* 為所有控制器`_myAllowSpecificOrigins`終結點啟用 CORS 策略。 請參閱[終結點路由](#ecors),將 CORS 策略應用於特定終結點。

使用端點路由時,***必須***將 CORS 中間件配置為`UseRouting`在`UseEndpoints`調用和 之間執行。

有關測試代碼的說明,請參閱[測試 CORS,](#testc)這些說明與前面的代碼類似。

方法<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>呼叫將 CORS 服務新增到應用的服務容器:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet2)]

關於詳細資訊,請參考此文件中的[CORS 政策選項](#cpo)。

這些方法<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>可以連結,如以下代碼所示:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup2.cs?name=snippet)]

注意: 指定的網址**不能**包含尾`/`隨斜槓 ( 。 如果 URL`/`終止與`false`,則比較返回,並且不返回任何標頭。

<a name="dp"></a>

### <a name="cors-with-default-policy-and-middleware"></a>具有預設政策和中間件的 CORS

以下突顯的代碼啟用預設 CORS 政策:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupDefaultPolicy.cs?name=snippet2&highlight=7,29)]

前面的代碼將預設的 CORS 策略應用於所有控制器終結點。

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a>使用端點路由啟用 Cors

使用`RequireCors`目前使用每個的終結點的 CORS***不支援***[自動預取要求](#apf)。 有關詳細資訊,請參閱此[GitHub 問題和](https://github.com/dotnet/aspnetcore/issues/20709)[使用終結點路由和 [HttpOptions] 測試 CORS。](#tcer)

使用端點路由,可以使用擴充<xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*>方法集,按終結點啟用 CORS:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPt.cs?name=snippet2&highlight=3,7-15,32,40,43)]

在上述程式碼中：

* `app.UseCors`啟用 CORS 中間件。 由於尚未設定預設政策,`app.UseCors()`因此單獨無法啟用 CORS。
* `/echo`和控制器終結點允許使用指定的策略進行交叉源請求。
* `/echo2`和 Razor Pages 終結點***不允許***跨源請求,因為未指定預設策略。

[[禁用 Cors]](#dc)屬性***不會***停用由 終結`RequireCors`點路由啟用的 CORS。

有關測試代碼的指令,請參閱[使用端點路由和 [HttpOptions] 測試 CORS。](#tcer)

<a name="attr"></a>

## <a name="enable-cors-with-attributes"></a>使用屬性啟用 CORS

使用[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性啟用 CORS 並將命名策略應用於僅對需要 CORS 的終結點提供最佳控制。

[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供了全域應用 CORS 的替代方法。 該`[EnableCors]`屬性為選擇的終結點啟用 CORS,而不是所有終結點:

* `[EnableCors]`指定預設策略。
* `[EnableCors("{Policy String}")]`指定命名策略。

該`[EnableCors]`屬性可應用於:

* 剃刀頁面`PageModel`
* 控制器
* 控制器操作方法

可以將不同的策略應用於具有屬性的`[EnableCors]`控制器、頁面模型或操作方法。 當該`[EnableCors]`屬性應用於控制器、頁面模型或操作方法,並在中間件中啟用 CORS 時,將應用***這兩個***策略。 ***我們建議不要合併策略。使用相同的應用程式的屬性***`[EnableCors]`***或中間件,而不是兩者在同一應用中。***

以下代碼對每種方法應用不同的原則:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

以下代碼建立兩個 CORS 政策:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup3.cs?name=snippet&highlight=12-28,44)]

為了最好地控制限制 CORS 請求:

* 與`[EnableCors("MyPolicy")]`命名策略一起使用。
* 不要定義預設策略。
* 不要使用[終結點路由](#ecors)。

下一節中的代碼與前面的清單一應合。

有關測試代碼的說明,請參閱[測試 CORS,](#testc)這些說明與前面的代碼類似。

<a name="dc"></a>

### <a name="disable-cors"></a>關閉 CORS

[[禁用 Cors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性***不會***停用終結點[路由](#ecors)已啟用的 CORS。

以下代碼定義 CORS`"MyPolicy"`政策 :

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTestMyPolicy.cs?name=snippet)]

以下代碼關閉操作的`GetValues2`CORS:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet&highlight=1,23)]

上述程式碼：

* 不支援帶[終結點路由](#ecors)的 CORS。
* 不定義[預設的 CORS 政策](#dp)。
* 使用[[啟用 Cors("我的策略")]](#attr)`"MyPolicy"`為控制器啟用 CORS 策略。
* 禁用方法的`GetValues2`CORS。

有關測試上述代碼的說明,請參閱[測試 CORS。](#testc)

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS 政策選項

本節介紹可在 CORS 策略中設定的各種選項:

* [設定允許的原點](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預先過期時間](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>在`Startup.ConfigureServices`中 呼叫 。 對於某些選項,首先閱讀[「CORS 的工作原理」](#how-cors)部分可能會有所説明。

## <a name="set-the-allowed-origins"></a>設定允許的原點

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>&ndash;允許使用任何方案 (`http``https`或 ) 從所有來源請求 CORS 請求。 `AllowAnyOrigin`不安全,因為*任何網站*都可以向應用發出交叉源請求。

> [!NOTE]
> 指定`AllowAnyOrigin``AllowCredentials`和 是不安全的配置,可能會導致跨網站請求偽造。 當應用同時配置這兩種方法時,CORS 服務返回無效的 CORS 回應。

`AllowAnyOrigin`影響預檢請求和`Access-Control-Allow-Origin`標頭。 有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash;將<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略的屬性設為一個函數,允許源在評估是否允許原點時匹配配置的通配符域。

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet)]

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* 允許任何 HTTP 方法:
* 影響印前請求和`Access-Control-Allow-Methods`標頭。 有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

要允許在 CORS 請求中傳送特定標頭,請呼叫作者[要求標頭](https://xhr.spec.whatwg.org/#request),呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers),請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>呼叫 :

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

`AllowAnyHeader`影響預取要求與[存取控制要求標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)。 有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。

僅當中`Access-Control-Request-Headers`發送的標頭與`WithHeaders`中 所述標頭`WithHeaders`完全匹配時 ,才能與 指定的特定標頭匹配 CORS 中間件策略。

例如,請考慮配置如下的應用:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet4)]

CORS 中介件拒絕具有以下請求標頭的預檢要求,`Content-Language`因為 ([標題名稱. Content 語言](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 未在`WithHeaders`中列出:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

該應用程式返回*200 OK*回應,但不會將 CORS 標頭髮送回。 因此,瀏覽器不會嘗試跨源請求。

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

默認情況下,瀏覽器不會向應用公開所有響應標頭。 有關詳細資訊,請參閱[W3C 跨源資源分享(術語):簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

預設情況下可用的回應標頭是:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 規範呼叫這些標頭*為簡單回應標頭*。 要使其他標頭可供應用使用,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet5)]
### <a name="credentials-in-cross-origin-requests"></a>跨源要求中的認證

憑據需要在 CORS 請求中特殊處理。 默認情況下,瀏覽器不會發送具有跨源請求的憑據。 認證包括 Cookie 和 HTTP 驗證方案。 要使用跨源要求傳送認證,客戶端必須設定`XMLHttpRequest.withCredentials``true`為 。

直接`XMLHttpRequest`使用:

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

使用[擷取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

伺服器必須允許憑據。 要允許跨源認證,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet6)]

HTTP 回應包括`Access-Control-Allow-Credentials`一個標頭,該標頭告訴瀏覽器伺服器允許跨源請求的憑據。

如果瀏覽器發送憑據,但回應不包含有效的`Access-Control-Allow-Credentials`標頭,則瀏覽器不會向應用公開回應,並且跨源請求將失敗。

允許跨源憑據存在安全風險。 另一個域中的網站可以在使用者不知情的情況下代表使用者向應用發送登錄使用者的憑據。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS 規範還規定,如果標頭存在`"*"``Access-Control-Allow-Credentials`, 則將原點設置為(所有源)無效。

<a name="pref"></a>

## <a name="preflight-requests"></a>預先要求

對於某些 CORS 請求,瀏覽器在發出實際請求之前會發送其他[OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)請求。 此要求為[預先要求](https://developer.mozilla.org/docs/Glossary/Preflight_request)。 如果以下***所有條件都***為 true,瀏覽器可以跳過預檢請求:

* 請求方法是 GET、頭或 POST。
* 套用不設定`Accept`要求 標頭以外`Accept-Language``Content-Language``Content-Type`的`Last-Event-ID`、、、、 或 。
* 標頭`Content-Type`(如果已設定)具有以下值之一:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

為用戶端請求設置的請求標頭規則適用於應用通過調用`setRequestHeader`物件設置`XMLHttpRequest`的 標頭。 CORS 規範呼叫這些標頭[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)。 這個規則不適用於瀏覽器可以設定的標頭,例如`User-Agent` `Host` `Content-Length` 。

下面是與本文件[「測試」](#testc)部分中的 **[放置測試]** 按鈕所做的預檢請求類似的範例回應。

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

預檢請求使用[HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)方法。 它可能包括以下標頭:

* [存取控制-請求方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method):將用於實際請求的 HTTP 方法。
* [存取控制-請求-標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers):應用在實際請求上設置的請求標頭的清單。 如前所述,這不包括瀏覽器設定的標頭,如`User-Agent`。
* [存取控制-允許方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

如果預檢請求被拒絕,應用將返回回應`200 OK`,但不會設置 CORS 標頭。 因此,瀏覽器不會嘗試跨源請求。 有關被拒絕的印前請求的範例,請參閱本文件的[「測試 CORS」](#testc)部分。

使用 F12 工具,主控台應用程式會顯示類似於以下錯誤之一的錯誤,具體取決於瀏覽器:

* Firefox: 跨源請求被阻止: 同一源策略不允許讀`https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5`取 中的遠端資源。 (原因:CORS 請求未成功)。 [深入了解](https://developer.mozilla.org/docs/Web/HTTP/CORS/Errors/CORSDidNotSucceed)
* 基於鉻:CORS 策略阻止了以https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5『 來自https://cors3.azurewebsites.net源' 獲取的訪問:對印前請求的回應未通過訪問控制檢查:請求的資源上不存在"訪問-控制-允許-原點"標頭。 如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。

要允許特定標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers),請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>呼叫 :

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

瀏覽器的設置`Access-Control-Request-Headers`方式不一致。 如果任一:

* 標頭設定為`"*"`
* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>呼叫:`Accept`至少`Content-Type`包括`Origin`, 和, 以及要支援的任何自訂標頭。

<a name="apf"></a>

### <a name="automatic-preflight-request-code"></a>自動預先要求代碼

應用 CORS 策略時,

* 透過 呼`app.UseCors``Startup.Configure`叫 在全球範圍內呼叫 。
* 使用`[EnableCors]`屬性。

ASP.NET核心回應預檢選項請求。

使用`RequireCors`目前基於每個終結點啟用 CORS***不支援***自動預檢請求。

本文件的[「測試 CORS」](#testc)部分演示了此行為。

<a name="pro"></a>

### <a name="httpoptions-attribute-for-preflight-requests"></a>預先要求的 [HttpOptions] 屬性

當使用適當的策略啟用 CORS 時,ASP.NET核心通常會自動回應 CORS 預檢請求。 在某些情況下,情況可能並非如此。 例如,將[CORS 與終結點路由一起使用](#ecors)。

以下代碼使用[[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute)屬性為 OPTIONS 請求建立終結點:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet&highlight=5-17)]

有關測試上述代碼的說明,請參閱[使用終結點路由和 [HttpOptions] 測試 CORS。](#tcer)

### <a name="set-the-preflight-expiration-time"></a>設定預先過期時間

標頭`Access-Control-Max-Age`指定對預檢請求的回應可以緩存多長時間。 要設定這個標頭,請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>呼叫 :

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet7)]
<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS 的工作原理

本節介紹在 HTTP 消息級別發生的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)請求中發生的情況。

* CORS**不是**一個安全功能。 CORS 是一種 W3C 標準,允許伺服器放鬆同源策略。
  * 例如,惡意參與者可能會對您的網站使用[跨網站腳本 (XSS),](xref:security/cross-site-scripting)並對其啟用 CORS 的網站執行跨網站請求以竊取資訊。
* 通過允許 CORS,API 並不更安全。
  * 由用戶端(瀏覽器)來強制實施 CORS。 伺服器執行請求並返回回應,返回錯誤的是用戶端並阻止回應。 例如,以下任何工具將顯示伺服器回應:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HTTPClient](/dotnet/csharp/tutorials/console-webapiclient)
    * 通過在位址列中輸入 URL 來訪問 Web 瀏覽器。
* 這是伺服器允許瀏覽器執行跨源[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)請求的一種方式,否則將禁止這樣做。
  * 沒有 CORS 的瀏覽器無法執行跨源請求。 在 CORS 之前[,JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)曾用於規避此限制。 JSONP 不使用 XHR,`<script>`它使用 標記來接收回應。 允許跨源載入腳本。

[CORS 規範](https://www.w3.org/TR/cors/)引入了幾個支援跨源請求的新 HTTP 標頭。 如果瀏覽器支援 CORS,它將自動為跨源請求設置這些標頭。 啟用 CORS 不需要自訂 JavaScript 代碼。

已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的[PUT 測試按鈕](https://cors3.azurewebsites.net/test)

下面是從[「值](https://cors3.azurewebsites.net/)」測試`https://cors1.azurewebsites.net/api/values`按鈕到的跨源請求的範例。 標頭`Origin`:

* 提供發出請求的網站的域。
* 是必需的,並且必須與主機不同。

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

在`OPTIONS`請求中,伺服器在回應中設置**回應**`Access-Control-Allow-Origin: {allowed origin}`標頭標頭。 例如,已部署[的範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) [,刪除 [啟用 Cors]](https://cors1.azurewebsites.net/test?number=2)按鈕`OPTIONS`請求包含以下標頭:

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

在前面的**回應標頭中**,伺服器在回應中設置[訪問控制允許源](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)標頭。 此`https://cors1.azurewebsites.net`標頭的值與請求中的`Origin`標頭匹配。

如果<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>呼叫`Access-Control-Allow-Origin: *`, 則傳回的通配符值。 `AllowAnyOrigin`允許任何來源。

如果回應不包括標頭,`Access-Control-Allow-Origin`則跨源請求將失敗。 具體來說,瀏覽器不允許請求。 即使伺服器返回成功的回應,瀏覽器也不會使回應對用戶端應用可用。

<a name="options"></a>

### <a name="display-options-requests"></a>顯示選項要求

默認情況下,Chrome 和邊緣瀏覽器不會在 F12 工具的網路選項卡上顯示選項請求。 要在這些瀏覽器中顯示選項請求,

* `chrome://flags/#out-of-blink-cors` 或 `edge://flags/#out-of-blink-cors`
* 禁用標誌。
* 重新啟動。

默認情況下,Firefox 會顯示選項請求。

## <a name="cors-in-iis"></a>IIS 的 CORS

部署到IIS時,如果伺服器未配置為允許匿名訪問,則CORS必須在Windows身份驗證之前運行。 為了支援此專案,需要為應用程式安裝和設定[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。

<a name="testc"></a>

## <a name="test-cors"></a>測試 CORS

[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)具有用於測試 CORS 的代碼。 請參閱[如何下載](xref:index#how-to-download-a-sample)。 該範例為新增 Razor 頁面的 API 專案:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTest2.cs?name=snippet2)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`應僅用於測試類似於[下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors)的範例應用。

下面`ValuesController`提供測試的終結點:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet)]

[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs)由[Rick.Docs.sample.RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet 包提供,並顯示路線資訊。

使用以下方法之一測試前面的範例碼:

* 在中使用部署的範例[https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/)應用。 無需下載示例。
* `dotnet run`使用`https://localhost:5001`的預設網址 執行範例。
* 從 Visual Studio 運行範例,埠設定為 44398,`https://localhost:44398`用於 URL。

將瀏覽器與 F12 工具一起使用:

* 選擇「**值」** 按鈕並檢視 **「網路」** 選項卡中的標頭。
* 選擇**PUT 測試**按鈕。 關於顯示 OPTIONS 要求的說明,請參考[顯示選項要求](#options)。 **PUT 測試**創建兩個請求,一個選項預檢請求和 PUT 請求。
* 選擇按鈕**`GetValues2 [DisableCors]`** 以觸發失敗的 CORS 請求。 如文檔中所述,回應返回 200 成功,但未發出 CORS 請求。 選擇 **「控制台」** 選項卡以檢視 CORS 錯誤。 根據瀏覽器的不同,將顯示類似於以下內容的錯誤:

     CORS`'https://cors1.azurewebsites.net/api/values/GetValues2'`策略 阻止`'https://cors3.azurewebsites.net'`了從 源提取的訪問:請求的資源上不存在"訪問-控制-允許源"標頭。 如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。
     
支援 CORS 的終結點可以使用工具進行測試,例如[捲曲](https://curl.haxx.se/)[、Fiddler](https://www.telerik.com/fiddler)或[Postman。](https://www.getpostman.com/) 使用工具時,`Origin`標頭指定的請求的來源必須不同於接收請求的主機。 如果要求不是基於標頭的值*的跨源*: `Origin`

* CORS 中間件無需處理請求。
* 回應中未返回 CORS 標頭。

以下指令用於`curl`送出帶有資訊的 OPTIONS 請求:

```bash
curl -X OPTIONS https://cors3.azurewebsites.net/api/TodoItems2/5 -i
```

<!--
curl come with Git. Add to path variable
C:\Program Files\Git\mingw64\bin\
-->

<a name="tcer"></a>

### <a name="test-cors-with-endpoint-routing-and-httpoptions"></a>使用端點路由與 [httpOptions] 測試 CORS

使用`RequireCors`目前使用每個的終結點的 CORS***不支援***[自動預取要求](#apf)。 請考慮以下使用[終結點路由啟用 CORS 的代碼](#ecors):

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPointBugTest.cs?name=snippet2)]

以下內容`TodoItems1Controller`提供測試的終結點:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems1Controller.cs?name=snippet2)]

從已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的[測試頁](https://cors1.azurewebsites.net/test?number=1)測試前面的代碼。

**刪除 [啟用 Cors]** 和**GET [啟用Cors]** 按鈕成功,因為`[EnableCors]`端點具有並回應預檢請求。 其他終結點失敗。 **GET**按鈕失敗,因為[JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js)傳送:

```javascript
 headers: {
      "Content-Type": "x-custom-header"
 },
```

以下內容`TodoItems2Controller`提供類似的終結點,但包括用於回應 OPTIONS 請求的顯式代碼:

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet2)]

從已部署範例的[測試頁](https://cors1.azurewebsites.net/test?number=2)測試前面的代碼。 在**控制器**下拉清單中,選擇 **「預檢**」,然後**設定控制器**。 對`TodoItems2Controller`終結點的所有 CORS 調用都成功。

## <a name="additional-resources"></a>其他資源

* [跨原始來源資源分享 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [開始使用 IIS CORS 模組](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文演示如何在ASP.NET核心應用中啟用 CORS。

流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。 此限制稱為*同源原則*。 同源策略可防止惡意網站從其他網站讀取敏感數據。 有時,您可能希望允許其他網站向你的應用發出交叉源請求。 有關詳細資訊,請參閱[Mozilla CORS 文章](https://developer.mozilla.org/docs/Web/HTTP/CORS)。

[跨源資源分享](https://www.w3.org/TR/cors/)(CORS):

* 是允許伺服器放鬆同源策略的 W3C 標準。
* **CORS 不是**安全功能,可放鬆安全性。 通過允許 CORS,API 並不更安全。 有關詳細資訊,請參閱[CORS 的工作原理](#how-cors)。
* 允許伺服器顯式允許某些跨源請求,同時拒絕其他請求。
* 比早期的技術(如[JSONP](/dotnet/framework/wcf/samples/jsonp))更安全、更靈活。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample)([如何下載](xref:index#how-to-download-a-sample))

## <a name="same-origin"></a>同一源

如果兩個 URL 具有相同的方案、主機和埠[(RFC 6454),](https://tools.ietf.org/html/rfc6454)則它們具有相同的源源。

這兩個網址 有相同的來源:

* `https://example.com/foo.html`
* `https://example.com/bar.html`

這些網址與前兩個網址具有不同的來源:

* `https://example.net`&ndash;不同的網域
* `https://www.example.com/foo.html`&ndash;不同的子域
* `http://example.com/foo.html`&ndash;不同的機制
* `https://example.com:9000/foo.html`&ndash;不同的連接埠

在比較源時,Internet Explorer 不考慮埠。

## <a name="cors-with-named-policy-and-middleware"></a>包含命名政策與中間件的 CORS

CORS 中間件處理跨源請求。 以下代碼支援具有指定來源的整個應用的 CORS:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

上述程式碼：

* 將策略名稱設為"myAllow\_特定原點" 策略名稱是任意的。
* 呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>式擴充方法,該方法啟用 CORS。
* 使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的調用。 lambda 取<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>一個物件。 [配置選項](#cors-policy-options)(`WithOrigins`如 )在本文的後面部分介紹。

方法<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>呼叫將 CORS 服務新增到應用的服務容器:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

關於詳細資訊,請參考此文件中的[CORS 政策選項](#cpo)。

該方法<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>可以連結方法,如以下代碼所示:

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

注意: 網址**不得**包含`/`尾隨斜槓 ( 。 如果 URL`/`終止與`false`,則比較返回,並且不返回任何標頭。

以下代碼透過 CORS 的裝置將 CORS 政策應用於所有應用終結點:
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
注意:`UseCors`必須在`UseMvc`之前 呼叫 。

請參閱[在 Razor 頁面、控制器和操作方法中啟用 CORS,](#ecors)以便在頁面/控制器/操作級別應用 CORS 策略。

有關測試代碼的說明,請參閱[測試 CORS,](#test)這些說明與前面的代碼類似。

## <a name="enable-cors-with-attributes"></a>使用屬性啟用 CORS

[啟用 Cors&rbrack;屬性提供了全域應用 CORS 的替代方法。 &lbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) 該`[EnableCors]`屬性為選定的端點啟用 CORS,而不是所有端點。

指定`[EnableCors]`預設策略和`[EnableCors("{Policy String}")]`指定策略。

該`[EnableCors]`屬性可應用於:

* 剃刀頁面`PageModel`
* 控制器
* 控制器操作方法

您可以使用`[EnableCors]`屬性對控制器/頁面模型/操作應用不同的策略。 當該`[EnableCors]`屬性應用於控制器/頁面模型/操作方法,並在中間件中啟用 CORS 時,將應用***這兩個***策略。 我們建議***不要***合併策略。 使用屬性`[EnableCors]`或中間件,***而不是兩者**。 使用`[EnableCors]`時 **,不要**定義預設策略。

以下代碼對每種方法應用不同的原則:

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

以下代碼建立 CORS 預設政策與名為`"AnotherPolicy"`的策略 :

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a>關閉 CORS

[禁用 Cors&rbrack;屬性禁用控制器/頁面模型/操作的 CORS。 &lbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)

<a name="cpo"></a>

## <a name="cors-policy-options"></a>CORS 政策選項

本節介紹可在 CORS 策略中設定的各種選項:

* [設定允許的原點](#set-the-allowed-origins)
* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)
* [設定允許的要求標頭](#set-the-allowed-request-headers)
* [設定公開的回應標頭](#set-the-exposed-response-headers)
* [跨源要求中的認證](#credentials-in-cross-origin-requests)
* [設定預先過期時間](#set-the-preflight-expiration-time)

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>在`Startup.ConfigureServices`中 呼叫 。 對於某些選項,首先閱讀[「CORS 的工作原理」](#how-cors)部分可能會有所説明。

## <a name="set-the-allowed-origins"></a>設定允許的原點

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>&ndash;允許使用任何方案 (`http``https`或 ) 從所有來源請求 CORS 請求。 `AllowAnyOrigin`不安全,因為*任何網站*都可以向應用發出交叉源請求。

> [!NOTE]
> 指定`AllowAnyOrigin``AllowCredentials`和 是不安全的配置,可能會導致跨網站請求偽造。 對於安全應用,如果客戶端必須授權自己訪問伺服器資源,請指定確切的來源清單。

`AllowAnyOrigin`影響預檢請求和`Access-Control-Allow-Origin`標頭。 有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash;將<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略的屬性設為一個函數,允許源在評估是否允許原點時匹配配置的通配符域。

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

* 允許任何 HTTP 方法:
* 影響印前請求和`Access-Control-Allow-Methods`標頭。 有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

要允許在 CORS 請求中傳送特定標頭,請呼叫作者*要求標頭*,呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

要允許所有作者要求標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

此設置會影響預檢請求和`Access-Control-Request-Headers`標頭。 有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。

無論 CorsPolicy.header 中配置`Access-Control-Request-Headers`的值 如何,CORS 中間件始終允許發送 中的四個標頭。 這個標頭清單包括:

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

例如,請考慮配置如下的應用:

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中間件使用以下請求標頭成功回應預檢請求,`Content-Language`因為 始終被列入白名單:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

默認情況下,瀏覽器不會向應用公開所有響應標頭。 有關詳細資訊,請參閱[W3C 跨源資源分享(術語):簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。

預設情況下可用的回應標頭是:

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 規範呼叫這些標頭*為簡單回應標頭*。 要使其他標頭可供應用使用,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>跨源要求中的認證

憑據需要在 CORS 請求中特殊處理。 默認情況下,瀏覽器不會發送具有跨源請求的憑據。 認證包括 Cookie 和 HTTP 驗證方案。 要使用跨源要求傳送認證,客戶端必須設定`XMLHttpRequest.withCredentials``true`為 。

直接`XMLHttpRequest`使用:

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

使用[擷取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API):

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

伺服器必須允許憑據。 要允許跨源認證,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 回應包括`Access-Control-Allow-Credentials`一個標頭,該標頭告訴瀏覽器伺服器允許跨源請求的憑據。

如果瀏覽器發送憑據,但回應不包含有效的`Access-Control-Allow-Credentials`標頭,則瀏覽器不會向應用公開回應,並且跨源請求將失敗。

允許跨源憑據存在安全風險。 另一個域中的網站可以在使用者不知情的情況下代表使用者向應用發送登錄使用者的憑據。 <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

CORS 規範還規定,如果標頭存在`"*"``Access-Control-Allow-Credentials`, 則將原點設置為(所有源)無效。

### <a name="preflight-requests"></a>預先要求

對於某些 CORS 請求,瀏覽器在發出實際請求之前發送其他請求。 此要求為*預先要求*。 如果以下條件為 true,瀏覽器可以跳過預檢請求:

* 請求方法是 GET、頭或 POST。
* 套用不設定`Accept`要求 標頭以外`Accept-Language``Content-Language``Content-Type`的`Last-Event-ID`、、、、 或 。
* 標頭`Content-Type`(如果已設定)具有以下值之一:
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

為用戶端請求設置的請求標頭規則適用於應用通過調用`setRequestHeader`物件設置`XMLHttpRequest`的 標頭。 CORS 規範呼叫這些標頭*作者要求標頭*。 這個規則不適用於瀏覽器可以設定的標頭,例如`User-Agent` `Host` `Content-Length` 。

以下是預檢要求的範例:

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

飛行前請求使用 HTTP OPTIONS 方法。 它包括兩個特殊的標頭:

* `Access-Control-Request-Method`:將用於實際請求的 HTTP 方法。
* `Access-Control-Request-Headers`:應用在實際請求上設置的請求標頭的清單。 如前所述,這不包括瀏覽器設定的標頭,如`User-Agent`。

<!-- I think this needs to be removed, was put here accidently -->

當使用適當的策略啟用 CORS 時,ASP.NET核心通常會自動回應 CORS 預檢請求。 [有關預檢要求,請參閱 [HttpOptions] 屬性](#pro)。

CORS 預檢請求可能包含一`Access-Control-Request-Headers`個 標頭,該標頭向伺服器指示與實際請求一起發送的標頭。

要允許特定標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

要允許所有作者要求標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

流覽器`Access-Control-Request-Headers`的設置 方式並不完全一致。 如果將標頭設置為 (或`"*"`<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>使用 ) 以外的任何內容,則`Accept`至少`Content-Type``Origin`應包括和 ,以及要支援的任何自定義標頭。

以下是對預檢請求的範例回應(假設伺服器允許該請求):

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

回應包括列出`Access-Control-Allow-Methods`允許的方法的標頭,並選擇一`Access-Control-Allow-Headers`個 標頭,其中列出了允許的標頭。 如果預檢請求成功,瀏覽器將發送實際請求。

如果預檢請求被拒絕,應用將返回*200 OK*回應,但不會將 CORS 標頭髮送回。 因此,瀏覽器不會嘗試跨源請求。

### <a name="set-the-preflight-expiration-time"></a>設定預先過期時間

標頭`Access-Control-Max-Age`指定對預檢請求的回應可以緩存多長時間。 要設定這個標頭,請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>呼叫 :

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a>CORS 的工作原理

本節介紹在 HTTP 消息級別發生的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)請求中發生的情況。

* CORS**不是**一個安全功能。 CORS 是一種 W3C 標準,允許伺服器放鬆同源策略。
  * 例如,惡意參與者可以使用[防止跨網站腳本 (XSS)](xref:security/cross-site-scripting)攻擊您的網站,並對其啟用 CORS 的網站執行跨網站請求以竊取資訊。
* 通過允許 CORS,您的 API 並不更安全。
  * 由用戶端(瀏覽器)來強制實施 CORS。 伺服器執行請求並返回回應,返回錯誤的是用戶端並阻止回應。 例如,以下任何工具將顯示伺服器回應:
    * [Fiddler](https://www.telerik.com/fiddler)
    * [Postman](https://www.getpostman.com/)
    * [.NET HTTPClient](/dotnet/csharp/tutorials/console-webapiclient)
    * 通過在位址列中輸入 URL 來訪問 Web 瀏覽器。
* 這是伺服器允許瀏覽器執行跨源[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)請求的一種方式,否則將禁止這樣做。
  * 瀏覽器(沒有 CORS)不能執行交叉源請求。 在 CORS 之前[,JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)曾用於規避此限制。 JSONP 不使用 XHR,`<script>`它使用 標記來接收回應。 允許跨源載入腳本。

[CORS 規範](https://www.w3.org/TR/cors/)引入了幾個支援跨源請求的新 HTTP 標頭。 如果瀏覽器支援 CORS,它將自動為跨源請求設置這些標頭。 啟用 CORS 不需要自訂 JavaScript 代碼。

下面是跨源請求的示例。 標頭`Origin`提供發出請求的網站的域。 標頭`Origin`是必需的,並且必須與主機不同。

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

如果伺服器允許請求,它將在`Access-Control-Allow-Origin`回應中設置標頭。 此標頭的值與請求中的`Origin`標頭匹配,或者是通配符`"*"`值 ,這意味著允許任何源:

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

如果回應不包括標頭,`Access-Control-Allow-Origin`則跨源請求將失敗。 具體來說,瀏覽器不允許請求。 即使伺服器返回成功的回應,瀏覽器也不會使回應對用戶端應用可用。

<a name="test"></a>

## <a name="test-cors"></a>測試 CORS

測試 CORS:

1. [建立 API 專案](xref:tutorials/first-web-api)。 或您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。
1. 使用本文件中的一種方法啟用 CORS。 例如：

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > `WithOrigins("https://localhost:<port>");`應僅用於測試類似於[下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)的範例應用。

1. 創建 Web 應用專案(剃鬚頁面或 MVC)。 該示例使用剃刀頁。 您可以在與 API 專案相同的解決方案中創建 Web 應用。
1. 將以下突顯的代碼加入*到 Index.cshtml*檔:

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. 在前面的代碼中,替換為`url: 'https://<web app>.azurewebsites.net/api/values/1',`已部署應用的 URL。
1. 部署 API 專案。 例如,[部署到 Azure](xref:host-and-deploy/azure-apps/index)。
1. 從桌面運行剃刀頁面或 MVC 應用,然後單擊 **「測試**」按鈕。 使用 F12 工具檢視錯誤訊息。
1. 從`WithOrigins`中刪除本地主機源並部署應用。 或者,使用其他埠運行客戶端應用。 例如,從可視化工作室運行。
1. 使用用戶端應用進行測試。 CORS 失敗傳回錯誤,但錯誤消息對 JavaScript 不可用。 使用 F12 工具中的主控台選項卡檢視錯誤。 根據瀏覽器的不同,您會收到類似於以下內容的錯誤(在 F12 工具控制台中):

   * 使用微軟邊緣:

     **SEC7120: [CORS]`https://localhost:44375`在`https://localhost:44375`「訪問 -控制-允許-原點」回應標頭中找不到跨源資源`https://webapi.azurewebsites.net/api/values/1`**

   * 使用鉻:

     **CORS 策略阻止了`https://webapi.azurewebsites.net/api/values/1`從`https://localhost:44375`源 處對 XMLHTTPRequest 的訪問:請求的資源上不存在"訪問-控制-允許源"標頭。**
     
支援 CORS 的終結點可以使用工具進行測試,例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)。 使用工具時,`Origin`標頭指定的請求的來源必須不同於接收請求的主機。 如果要求不是基於標頭的值*的跨源*: `Origin`

* CORS 中間件無需處理請求。
* 回應中未返回 CORS 標頭。

## <a name="cors-in-iis"></a>IIS 的 CORS

部署到IIS時,如果伺服器未配置為允許匿名訪問,則CORS必須在Windows身份驗證之前運行。 為了支援此專案,需要為應用程式安裝和設定[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。

## <a name="additional-resources"></a>其他資源

* [跨原始來源資源分享 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [開始使用 IIS CORS 模組](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end
