---
title: "啟用跨原始要求 (CORS)"
author: rick-anderson
description: "本文件介紹的標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始要求的 CORS。"
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 9f53ce11f1659aa3416fe4fbb94183c64ab0dab5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="enabling-cross-origin-requests-cors"></a>啟用跨原始要求 (CORS)

由[Mike Wasson](https://github.com/mikewasson)， [Shayne Boyer](https://twitter.com/spboyer)，和[Tom Dykstra](https://github.com/tdykstra)

瀏覽器安全性防止網頁 AJAX 要求到另一個網域。 這項限制稱為*相同來源原則*，並可防止惡意網站從另一個站台讀取的機密資料。 不過，有時您可能想要讓其他對您的 web API 進行跨原始要求的站台。

[跨原始資源共用](http://www.w3.org/TR/cors/)」 (CORS) 是一種 W3C 標準，可讓放寬的相同來源原則的伺服器。 使用 CORS，伺服器可以明確地允許某些跨原始要求時拒絕其他人。 CORS 是更安全且更彈性與較早的技術如[JSONP](https://wikipedia.org/wiki/JSONP)。 本主題示範如何在 ASP.NET Core 應用程式中啟用 CORS。

## <a name="what-is-same-origin"></a>什麼是 「 相同來源 」？

如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原始主機。 ([RFC 6454](http://tools.ietf.org/html/rfc6454))

這些兩個 Url 有相同的原始主機：

* `http://example.com/foo.html`

* `http://example.com/bar.html`

這些 Url 會有兩個不同來源比上一個：

* `http://example.net`為不同的網域

* `http://www.example.com/foo.html`為不同的子網域

* `https://example.com/foo.html`為不同的配置

* `http://example.com:9000/foo.html`為不同的通訊埠

> [!NOTE]
> 比較來源時，Internet Explorer 不會視為連接埠。

## <a name="setting-up-cors"></a>CORS 設定

若要設定加入您的應用程式的 CORS`Microsoft.AspNetCore.Cors`封裝至您的專案。

將 CORS 服務加入 Startup.cs 中：

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a>啟用 CORS 中介軟體

若要啟用整個應用程式的 CORS 將 CORS 中介軟體新增至您要求管線使用`UseCors`擴充方法。 請注意，CORS 中介軟體必須在任何已定義的端點之前在您想要支援跨原始要求 （例如應用程式中。 任何呼叫之前`UseMvc`)。

加入 CORS 中介軟體使用時，您可以指定跨原始原則`CorsPolicyBuilder`類別。 執行這項作業的方法有兩種。 第一個方法是呼叫 UseCors lambda:

[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

**注意：**沒有尾端斜線，必須指定的 URL (`/`)。 如果 URL 將會終止並`/`，比較會傳回`false`而且不會傳回不含標頭。

Lambda 會採用`CorsPolicyBuilder`物件。 您可以找到一份[組態選項](#cors-policy-options)本主題稍後。 在此範例中的原則，允許跨原始要求從`http://example.com`和其他來源。

請注意，CorsPolicyBuilder fluent API，因此您可以在方法呼叫鏈結：

[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

第二種方法是定義一個或多個具名的 CORS 原則，然後依名稱選取的原則，在執行階段。

[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

這個範例會將名為"AllowSpecificOrigin 」 的 CORS 原則。 若要選取的原則，將傳遞至名稱`UseCors`。

## <a name="enabling-cors-in-mvc"></a>啟用 CORS MVC 中

MVC 或者可用來套用特定的 CORS，每個動作，每個控制站，或全域的所有控制站。 使用 MVC 啟用 CORS 時使用相同的 CORS 服務，但 CORS 中介軟體不是。

### <a name="per-action"></a>每個動作

若要指定特定動作的 CORS 原則將`[EnableCors]`動作屬性。 指定原則名稱。

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a>每個控制站

若要指定將特定控制器的 CORS 原則`[EnableCors]`屬性加入控制器類別。 指定原則名稱。

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a>全域

您可以啟用 CORS 全域所有控制器加入`CorsAuthorizationFilterFactory`全域篩選集合的篩選：

[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

優先順序是： 全域控制器的動作。 動作層級原則的優先順序高於控制器層級原則和控制器層級原則會優先於全域原則。

### <a name="disable-cors"></a>停用 CORS

若要停用 CORS，控制器或動作，使用`[DisableCors]`屬性。

[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a>CORS 原則選項

本章節描述您可以設定 CORS 原則中的各種選項。

* [設定允許的來源](#set-the-allowed-origins)

* [設定允許的 HTTP 方法](#set-the-allowed-http-methods)

* [設定允許的要求標頭](#set-the-allowed-request-headers)

* [設定公開的回應標頭](#set-the-exposed-response-headers)

* [跨原始要求中的認證](#credentials-in-cross-origin-requests)

* [預檢到期時間設定](#set-the-preflight-expiration-time)

某些選項可能會很有幫助讀取[如何 CORS 運作](#how-cors-works)第一次。

### <a name="set-the-allowed-origins"></a>設定允許的來源

若要允許一或多個特定的來源：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]

若要允許所有來源：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]

請仔細考慮，才能允許來自任何來源的要求。 這表示幾乎任何網站，可以進行 AJAX 呼叫您的 api。

### <a name="set-the-allowed-http-methods"></a>設定允許的 HTTP 方法

若要允許所有的 HTTP 方法：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]

這會影響事前要求和存取控制-允許-方法標頭。

### <a name="set-the-allowed-request-headers"></a>設定允許的要求標頭

CORS 預檢要求可能會包含存取控制-頭 access-control-request-headers 標頭，列出應用程式所設定的 HTTP 標頭 (所謂的 「 撰寫要求標頭 」)。

白名單特定的標頭：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]

若要允許所有撰寫要求標頭：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]

瀏覽器不會在它們如何設定存取控制-access-control-request-headers 標完全一致的。 如果您將設定標頭的任何項目不是"*"，您至少應該包含 [接受]，「 內容型別 」 和 「 原始 」，再加上您想要支援的任何自訂標頭。

### <a name="set-the-exposed-response-headers"></a>設定公開的回應標頭

根據預設，瀏覽器不會公開所有的應用程式的回應標頭。 (請參閱[http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header)。)預設可用的回應標頭如下：

* Cache-Control

* Content-Language

* Content-Type

* 到期

* Last-Modified

* Pragma

CORS 規格會呼叫這些*簡單的回應標頭*。 若要讓其他標頭可用於應用程式：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a>跨原始要求中的認證

認證需要特殊處理 CORS 要求中。 根據預設，瀏覽器不會傳送跨原始要求的任何認證。 認證會包括 cookie，以及 HTTP 驗證配置。 若要傳送與跨原始要求的認證，用戶端必須 XMLHttpRequest.withCredentials 設定為 true。

直接使用 XMLHttpRequest:

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

JQuery： 中

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

此外，伺服器必須允許認證。 若要允許跨原始認證：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]

現在 HTTP 回應將包含存取控制-允許-認證標頭，告知瀏覽器伺服器允許跨原始要求的認證。

如果瀏覽器傳送認證，但回應未包含有效的存取控制-允許-認證標頭，瀏覽器將不會公開至應用程式中，回應和 AJAX 要求失敗。

要非常小心有關允許跨原始認證，因為這表示網站，位於另一個網域可以傳送給您的應用程式使用者的身分登入之使用者的認證不會察覺使用者。 CORS 規格也狀態該設定來源可"*"（所有原始網域） 不正確的存取控制-允許-認證標頭是否存在。

### <a name="set-the-preflight-expiration-time"></a>預檢到期時間設定

存取控制的最大年齡標頭指定可以快取預檢要求的回應的時間長度。 若要設定此標頭：

[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a>CORS 的運作方式

本章節描述 CORS 要求，在 HTTP 訊息的層級中發生的事。 請務必了解 CORS 運作方式，以便您可以正確地設定您的 CORS 原則和疑難排解如果項目不在您預期方式運作。

CORS 規格導入了幾個新的 HTTP 標頭啟用跨原始要求。 如果瀏覽器支援 CORS，它會設定這些標頭會自動針對跨原始要求。您不需要執行任何 JavaScript 程式碼中的特殊的動作。

以下是跨原始要求的範例。 「 原始 」 標頭提供提出要求的站台的網域：

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

如果伺服器允許的要求，它會在回應中設定的存取控制-允許的原始標頭。 此標頭的值符合要求，從 Origin 標頭或萬用字元值"*"，允許任何來源的意義：

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

如果回應不包含存取控制-允許的原始標頭，要求將會失敗。 具體來說，瀏覽器不允許要求。 即使伺服器會傳回成功的回應，瀏覽器不會提供回應給用戶端應用程式。

### <a name="preflight-requests"></a>預檢要求

對於某些 CORS 要求，瀏覽器會傳送其他要求，傳送實際要求的資源之前稱為 「 預檢要求，」。 如果下列條件成立，瀏覽器可以略過預檢要求：

* 要求方法為 GET、 HEAD 或 post 要求和

* 應用程式不會設定 Accept，接受語言內容語言，以外的任何要求標頭的內容類型或最後一個事件識別碼，並

* Content-type 標頭 (如果設定) 是下列其中之一：

  * application/x-www-form-urlencoded

  * multipart/form-data

  * 文字/純文字

要求標頭有關的規則適用於應用程式會藉由呼叫 setRequestHeader XMLHttpRequest 物件上設定的標頭。 （CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於可設定的瀏覽器，例如使用者代理程式、 主機、 或 Content-length 標頭。

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

事前要求會使用 OPTIONS HTTP 方法。 它包含兩個特殊標頭：

* 存取控制-要求的方法： 將實際的要求使用 HTTP 方法。

* 存取控制-access-control-request-headers 標： 實際要求設定的應用程式的要求標頭的清單。 （同樣地，這不包括瀏覽器設定的標頭）。

以下是範例回應，假設伺服器允許要求：

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

回應包括列出允許的方法，存取控制-允許-方法標頭和選擇性地存取控制-允許的標頭標頭，其中會列出允許的標頭。 如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。
