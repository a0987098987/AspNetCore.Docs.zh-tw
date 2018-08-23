---
title: 啟用 ASP.NET Core 中的跨源要求 (CORS)
author: rick-anderson
description: 了解如何為標準，以允許或拒絕在 ASP.NET Core 應用程式的跨原始要求的 CORS。
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2018
ms.locfileid: "41834714"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>啟用 ASP.NET Core 中的跨源要求 (CORS)

藉由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)

瀏覽器安全性可防止網頁對另一個網域提出 AJAX 要求。 這項限制稱為*同源原則*，可防止惡意網站從另一個網站讀取敏感性資料。 不過，有時候您可能想要讓跨原始來源要求對您的 web API 的其他站台。

[跨原始資源共用](http://www.w3.org/TR/cors/)(CORS) 是 W3C 標準，可讓伺服器放寬同源原則。 使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。 CORS 可較為安全且更有彈性，比早期技術這類[JSONP](https://wikipedia.org/wiki/JSONP)。 本主題說明如何在 ASP.NET Core 應用程式中啟用 CORS。

## <a name="what-is-same-origin"></a>什麼是 「 相同原始 」？

如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原點。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

這些兩個 Url 有相同的來源：

* `http://example.com/foo.html`

* `http://example.com/bar.html`

這些 Url 會有兩個不同的來源，比上一個：

* `http://example.net` -不同的網域

* `http://www.example.com/foo.html` -不同的子網域

* `https://example.com/foo.html` -不同的配置

* `http://example.com:9000/foo.html` -不同的連接埠

> [!NOTE]
> 比較原始來源時，Internet Explorer 不會視為連接埠。

## <a name="enable-cors"></a>啟用 CORS

::: moniker range="<= aspnetcore-1.1"

若要設定您的應用程式的 CORS 新增`Microsoft.AspNetCore.Cors`封裝至您的專案。

::: moniker-end

呼叫[AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)在`Startup.ConfigureServices`:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>與中介軟體啟用 CORS

若要啟用 CORS，請將 CORS 中介軟體新增至要求管線使用`UseCors`擴充方法。 CORS 中介軟體必須在前面定義的端點在您的應用程式中要支援跨原始來源要求 (例如，任何呼叫之前`UseMvc`)。

新增 CORS 中介軟體使用時，就可以指定跨源原則[CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors)類別。 執行這項作業的方法有兩種。 第一個是呼叫`UseCors`使用 lambda:

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**注意︰** 必須指定不含尾端斜線的 URL (`/`)。 如果 URL 終止`/`，比較會傳回`false`且會傳回不含標頭。

Lambda 會採用`CorsPolicyBuilder`物件。 您會發現一份[組態選項](#cors-policy-options)本主題稍後的。 在此範例中，該原則可讓跨源要求，從`http://example.com`和其他來源。

CorsPolicyBuilder 有 fluent API，因此您可以在方法呼叫鏈結：

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

第二種方法是定義一或多個具名的 CORS 原則，並再依名稱選取的原則，在執行階段。

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

此範例會新增一個名為"AllowSpecificOrigin 」 的 CORS 原則。 若要選取原則，將名稱傳遞給`UseCors`。

## <a name="enabling-cors-in-mvc"></a>在 MVC 中啟用 CORS

您也可以使用 MVC 套用每個動作，每個控制站，或適用於所有控制站的特定 CORS。 使用 MVC 啟用 CORS 時，會使用相同的 CORS 服務，但不 CORS 中介軟體。

### <a name="per-action"></a>每個動作

若要指定特定動作的 CORS 原則新增`[EnableCors]`屬性的動作。 指定原則名稱。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>每個控制站

若要指定特定的控制站的 CORS 原則新增`[EnableCors]`屬性至控制器類別。 指定原則名稱。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>全域

您可以啟用 CORS 全域所有控制站新增`CorsAuthorizationFilterFactory`至全域篩選集合的篩選器：

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

優先順序是： 全域控制器的動作。 動作層級原則優先於控制器層級原則，以及控制器層級原則的優先順序高於全域原則。

### <a name="disable-cors"></a>停用 CORS

若要停用控制器或動作的 CORS，請使用`[DisableCors]`屬性。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS 原則選項

本章節描述您可以設定的 CORS 原則中的各種選項。

* [設定允許的來源](#set-the-allowed-origins)

* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)

* [設定允許的要求標頭](#set-the-allowed-request-headers)

* [設定公開的回應標頭](#set-the-exposed-response-headers)

* [跨原始來源要求中的認證](#credentials-in-cross-origin-requests)

* [設定預檢到期時間](#set-the-preflight-expiration-time)

如需一些選項，可能會很有幫助讀取[如何 CORS 運作](#how-cors-works)第一次。

### <a name="set-the-allowed-origins"></a>設定允許的來源

若要允許一或多個特定的來源：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

若要允許所有來源：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

請仔細考慮，才能允許來自任何來源的要求。 這表示，幾乎任何網站可以對進行 AJAX 呼叫您的 API。

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

若要允許所有的 HTTP 方法：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

這會影響事前要求和存取控制-允許-方法標頭。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

CORS 預檢要求可能會包含存取控制-access-control-request-headers 標標頭，列出應用程式設定的 HTTP 標頭 (所謂 「 撰寫要求標頭 」)。

列入允許清單特定的標頭：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

若要允許所有撰寫要求標頭：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

瀏覽器不會在它們如何設定存取控制-access-control-request-headers 標完全一致的。 如果您將設定標頭的任何項目以外的其他 「 * 」，您應該至少包含在 「 accept 」，"content-type"、"origin"，以及您想要支援的任何自訂標頭。

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

根據預設，瀏覽器不會將所有應用程式的回應標頭公開。 (請參閱[ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header)。)是預設可用的回應標頭如下：

* Cache-Control

* 內容語言

* Content-Type

* 到期

* 上次修改

* Pragma

CORS 規格會呼叫這些*簡單的回應標頭*。 若要讓其他標頭可供應用程式：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>跨原始來源要求中的認證

認證需要在 CORS 要求的特殊處理。 根據預設，瀏覽器不會傳送任何與跨原始要求的認證。 認證包含 cookie，以及 HTTP 驗證配置。 若要傳送與跨原始要求的認證，用戶端必須 XMLHttpRequest.withCredentials 設為 true。

直接使用 XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

在 jQuery 中：

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

此外，伺服器必須允許的認證。 若要允許跨原始來源的認證：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

現在的 HTTP 回應將包含存取控制-允許-認證標頭，它會告訴瀏覽器伺服器允許跨原始來源要求認證。

如果瀏覽器傳送認證，但是回應沒有包含有效的存取控制-允許-認證標頭，瀏覽器不會公開 （expose） 的應用程式的回應，而且 AJAX 要求失敗。

允許跨原始來源的認證時要小心。 網站，以在另一個網域可以傳送給使用者不知情的情況下代表的使用者上的應用程式的登入的使用者的認證。 CORS 規格也會指出該設定來源，則`"*"`（所有原始網域） 是無效的如果`Access-Control-Allow-Credentials`標頭已存在。

### <a name="set-the-preflight-expiration-time"></a>設定預檢到期時間

存取控制-最大壽命標頭會指定多久可以快取預檢要求的回應。 若要設定此標頭：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>CORS 的運作方式

本章節描述 CORS 要求的 HTTP 訊息層級中發生的動作。 請務必了解，這樣可以正確地設定和非預期的行為發生時偵錯的 CORS 原則，CORS 的運作方式。

CORS 規格引進了數個新的 HTTP 標頭啟用跨源要求。 如果瀏覽器支援 CORS，它會設定自動跨原始來源要求這些標頭。 若要啟用 CORS，不需要自訂 JavaScript 程式碼。

以下是跨原始要求的範例。 `Origin`標頭提供網站提出要求的網域：

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

如果伺服器允許的要求，它會在回應中設定的存取控制-允許-原始標頭。 此標頭的值符合 Origin 標頭從要求中，或者是萬用字元值"*"，表示允許任何來源：

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

如果回應不包含存取控制-允許-原始標頭，AJAX 要求將會失敗。 具體而言，瀏覽器不允許要求。 即使伺服器會傳回成功的回應，瀏覽器不提供回應給用戶端應用程式。

### <a name="preflight-requests"></a>預檢要求

對於某些 CORS 要求，瀏覽器會傳送其他要求，稱為 「 預檢要求，」 後才會傳送實際要求的資源。 如果下列條件成立，則瀏覽器可以略過預檢要求：

* 要求方法是 GET、 HEAD 或 POST、 和

* 應用程式不會設定 Accept、 Accept-language、 內容語言、 以外任何要求標頭內容類型或最後一個事件識別碼，以及

* Content-type 標頭 (如果設定) 是下列其中之一：

  * application/x-www-form-urlencoded

  * multipart/form-data

  * text/plain

要求標頭的相關規則適用於應用程式會藉由呼叫 setRequestHeader XMLHttpRequest 物件上設定的標頭。 （CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於設定瀏覽器，例如使用者代理程式、 主機或 Content-length 標頭。

預檢要求的範例如下：

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

事前要求使用 HTTP OPTIONS; 方法。 它包含兩個特殊標頭：

* 存取控制-要求的方法： 將實際的要求使用 HTTP 方法。

* 存取控制-access-control-request-headers 標： 實際的要求設定的應用程式的要求標頭的清單。 （同樣地，這不包括瀏覽器設定的標頭。）

以下是範例回應，假設伺服器允許的要求：

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

此回應包含存取控制-允許-方法標頭，其中列出允許的方法，並選擇性地存取控制-允許-標頭的標頭，它會列出允許的標頭。 如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。
