---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: 防止跨網站要求偽造 (CSRF) 攻擊，ASP.NET Web API 中的 |Microsoft 文件
author: MikeWasson
description: 描述跨網站要求偽造 (CSRF) 攻擊，以及如何實作 ASP.NET Web API 中的反 CSRF 量值。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5e7b24c697e0bb37f388341abd89609c76f6b64c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961233"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>防止跨網站要求偽造 (CSRF) 攻擊，ASP.NET Web API 中
====================
由[Mike Wasson](https://github.com/MikeWasson)

跨站台要求偽造 (CSRF) 攻擊是其中惡意網站將要求傳送至易受攻擊的站台，使用者目前登入

CSRF 攻擊的範例如下：

1. 使用者登入`www.example.com`使用表單驗證。
2. 伺服器會驗證使用者。 伺服器的回應包含驗證 cookie。
3. 沒有登出，使用者會造訪惡意網站。 這個惡意網站包含 HTML 格式如下： 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    請注意，張貼至容易遭受站台，惡意網站的表單動作。 這是 CSRF 的 「 跨網站 」 部分。
4. 使用者按一下 [提交] 按鈕。 瀏覽器包含與要求的驗證 cookie。
5. 要求使用者驗證內容，以在伺服器上執行，並可以執行已驗證的使用者允許進行的任何動作。

雖然這個範例需要使用者按一下 [表單] 按鈕，惡意的頁面無法輕鬆地執行自動送出表單的指令碼一樣。 此外，使用 SSL 無法防止 CSRF 攻擊中，因為惡意網站可以傳送"https://"要求。

一般而言，CSRF 攻擊可能會對網站使用 cookie 進行驗證，因為瀏覽器傳送到目標網站的所有相關的 cookie。 不過，不限於利用 cookie CSRF 攻擊。 比方說，也是很容易遭受基本和摘要式驗證。 之後使用者登入基本或摘要式驗證。 瀏覽器會自動傳送認證到工作階段結束為止。

## <a name="anti-forgery-tokens"></a>防偽語彙基元

為了避免 CSRF 攻擊，ASP.NET MVC 會使用防偽語彙基元，也稱為*要求驗證權杖*。

1. 用戶端會要求包含表單的 HTML 網頁。
2. 伺服器回應中包含兩個語彙基元。 為 cookie 會傳送一個 token。 其他會放置在隱藏的表單欄位。 如此對手無法猜測值時，會隨機產生語彙基元。
3. 當用戶端提交表單時，它必須傳回給伺服器傳送這兩個語彙基元。 用戶端傳送的 cookie 語彙基元當做 cookie，並將傳送表單資料內的表單語彙基元。 （瀏覽器用戶端會自動執行此使用者送出表單時。）
4. 如果要求不包含這兩個語彙基元，伺服器就不允許要求。

HTML 表單，以隱藏的表單語彙基元的範例如下：

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

防偽語彙基元運作，因為惡意的頁面無法讀取使用者的權杖，因為相同原始原則。 ([相同原始原則](http://www.w3.org/Security/wiki/Same_Origin_Policy)防止存取彼此的內容的兩個不同站台上裝載的文件。 因此在先前範例中，惡意的頁面可以將要求傳送至 example.com，但它無法讀取回應）。

若要避免 CSRF 攻擊，使用任何驗證通訊協定防偽語彙基元，瀏覽器以無訊息方式傳送認證之後使用者登入。 這包括 cookie 為基礎的驗證通訊協定，例如表單驗證，以及例如基本和摘要式的驗證通訊協定。

您應該要求 （POST、 PUT、 DELETE） 的任何 nonsafe 方法防偽語彙基元。 此外，請確定安全的方法 （GET、 HEAD），並沒有任何副作用。 此外，如果您啟用跨網域支援，例如 CORS 或 JSONP，諸如取得更安全的方法是可能容易遭受 CSRF 攻擊，讓攻擊者讀取可能含有機密資料。

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>ASP.NET MVC 中的防偽語彙基元

若要加入 Razor 頁面防偽語彙基元，請使用**HtmlHelper.AntiForgeryToken** helper 方法：

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

這個方法會加入隱藏的表單欄位，也會設定的 cookie 語彙基元。

## <a name="anti-csrf-and-ajax"></a>反 CSRF 和 AJAX

表單語彙基元可能 AJAX 要求的問題，因為 AJAX 要求可能會傳送 JSON 資料，HTML 表單資料。 一個解決方案是將自訂 HTTP 標頭中傳送的語彙基元。 下列程式碼來產生權杖，會使用 Razor 語法，然後將權杖加入至 AJAX 要求。 藉由呼叫在伺服器上產生的語彙基元**AntiForgery.GetTokens**。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

當您處理要求時，請從要求標頭中擷取語彙基元。 然後呼叫**AntiForgery.Validate**方法以驗證語彙基元。 **驗證**方法擲回例外狀況，如果語彙基元無效。

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
