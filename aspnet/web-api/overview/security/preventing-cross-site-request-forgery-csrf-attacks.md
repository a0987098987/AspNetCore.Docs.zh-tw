---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: 防止跨網站要求偽造 (CSRF) 攻擊，ASP.NET MVC 中
author: MikeWasson
description: 描述跨網站要求偽造 (CSRF) 攻擊，以及如何在 ASP.NET Web MVC 中實作防 CSRF 量值。
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: c88975d1c205e9d0733bfb4c710b92bc8fdaaa7a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911493"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>防止跨網站要求偽造 (CSRF) 攻擊，在 ASP.NET MVC 應用程式
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

跨站台要求偽造 (CSRF) 攻擊是惡意網站用來傳送使用者目前登入的有弱點網站要求

CSRF 攻擊的範例如下：

1. 使用者登入`www.example.com`使用表單驗證。
2. 伺服器會驗證使用者。 來自伺服器的回應包含驗證 cookie。
3. 而不需要登出，使用者會造訪惡意的網站。 此惡意的網站包含 HTML 格式如下： 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    請注意，張貼至易受攻擊的站台，不必惡意網站的表單動作。 這是 CSRF 的 「 跨站台 」 部分。
4. 使用者按一下 [提交] 按鈕。 瀏覽器會包含與要求的驗證 cookie。
5. 要求使用者的驗證內容，以在伺服器上執行，而且可以執行的任何已驗證的使用者能夠執行的動作項目。

雖然此範例中所需的使用者可以按一下 [表單] 按鈕，惡意的頁面無法輕鬆地執行自動送出表單的指令碼一樣。 此外，使用 SSL 不會防止 CSRF 攻擊中，因為惡意的網站可傳送"https://"要求。

一般而言，CSRF 攻擊是可能對網站使用 cookie 進行驗證，因為瀏覽器會將所有相關的 cookie 傳送至目的地網站。 不過，不一定要利用 cookie CSRF 攻擊。 例如，基本和摘要式驗證也是易受攻擊。 之後使用者登入基本或摘要式驗證。 瀏覽器會自動傳送認證，直到工作階段結束為止。

## <a name="anti-forgery-tokens"></a>防偽語彙基元

為了協助防止 CSRF 攻擊，ASP.NET MVC 會使用防偽權杖，也稱為*要求驗證權杖*。

1. 用戶端要求包含表單的 HTML 網頁。
2. 伺服器包含兩個權杖回應中。 一個語彙基元會傳送 cookie。 其他會放在隱藏的表單欄位。 使攻擊者無法猜測值時，會隨機產生權杖。
3. 當用戶端會提交表單時，它必須傳送這兩個權杖回傳至伺服器。 用戶端傳送的 cookie 語彙基元為 cookie，並將傳送表單語彙基元，在表單資料。 （瀏覽器用戶端會自動執行此使用者送出表單時。）
4. 如果要求不包含這兩個語彙基元，伺服器就不允許要求。

HTML 表單與隱藏的表單語彙基元的範例如下：

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

防偽語彙基元運作，因為惡意的頁面無法讀取使用者的權杖，因為同源原則。 ([同源原則](http://www.w3.org/Security/wiki/Same_Origin_Policy)防止存取彼此的內容的兩個不同站台上裝載的文件。 因此在先前範例中，惡意的頁面可以將要求傳送至 example.com，但它無法讀取回應）。

若要防止 CSRF 攻擊，瀏覽器以無訊息方式傳送任何驗證通訊協定搭配使用防偽權杖的認證之後在使用者登入。 這包括以 cookie 為基礎的驗證通訊協定，例如表單驗證，以及例如基本和摘要式的驗證通訊協定。

您應該要求 （POST、 PUT、 DELETE） 的任何 nonsafe 方法防偽語彙基元。 此外，請確定安全的方法 （GET、 HEAD） 沒有任何副作用。 此外，如果您啟用跨網域支援，例如 CORS 或 JSONP，甚至是安全的方法，例如 GET 是可能容易遭受 CSRF 攻擊，讓攻擊者讀取潛在的敏感資料。

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>在 ASP.NET MVC 中的防偽語彙基元

若要新增至 Razor pages 的防偽語彙基元，請使用**HtmlHelper.AntiForgeryToken** helper 方法：

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

這個方法會將隱藏的表單欄位，而且也會設定 cookie 語彙基元。

## <a name="anti-csrf-and-ajax"></a>防 CSRF 和 AJAX

表單語彙基元可能會有問題的 AJAX 要求，因為 AJAX 要求可能會傳送 JSON 資料，非 HTML 表單資料。 一個解決方案是自訂的 HTTP 標頭中傳送權杖。 下列程式碼使用 Razor 語法來產生權杖，並再將權杖新增至 AJAX 要求。 藉由呼叫在伺服器上產生語彙基元**AntiForgery.GetTokens**。

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

當您處理要求時，請從要求標頭擷取權杖。 然後呼叫**AntiForgery.Validate**方法來驗證權杖。 **驗證**方法擲回例外狀況，如果權杖無效。

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
