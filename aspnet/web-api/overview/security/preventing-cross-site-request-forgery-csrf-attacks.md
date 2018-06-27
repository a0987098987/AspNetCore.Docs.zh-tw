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
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a><span data-ttu-id="a30f2-103">防止跨網站要求偽造 (CSRF) 攻擊，ASP.NET Web API 中</span><span class="sxs-lookup"><span data-stu-id="a30f2-103">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a30f2-104">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a30f2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a30f2-105">跨站台要求偽造 (CSRF) 攻擊是其中惡意網站將要求傳送至易受攻擊的站台，使用者目前登入</span><span class="sxs-lookup"><span data-stu-id="a30f2-105">Cross-Site Request Forgery (CSRF) is an attack where a malicious site sends a request to a vulnerable site where the user is currently logged in</span></span>

<span data-ttu-id="a30f2-106">CSRF 攻擊的範例如下：</span><span class="sxs-lookup"><span data-stu-id="a30f2-106">Here is an example of a CSRF attack:</span></span>

1. <span data-ttu-id="a30f2-107">使用者登入`www.example.com`使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="a30f2-107">A user logs into `www.example.com` using forms authentication.</span></span>
2. <span data-ttu-id="a30f2-108">伺服器會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="a30f2-108">The server authenticates the user.</span></span> <span data-ttu-id="a30f2-109">伺服器的回應包含驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="a30f2-109">The response from the server includes an authentication cookie.</span></span>
3. <span data-ttu-id="a30f2-110">沒有登出，使用者會造訪惡意網站。</span><span class="sxs-lookup"><span data-stu-id="a30f2-110">Without logging out, the user visits a malicious web site.</span></span> <span data-ttu-id="a30f2-111">這個惡意網站包含 HTML 格式如下：</span><span class="sxs-lookup"><span data-stu-id="a30f2-111">This malicious site contains the following HTML form:</span></span> 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    <span data-ttu-id="a30f2-112">請注意，張貼至容易遭受站台，惡意網站的表單動作。</span><span class="sxs-lookup"><span data-stu-id="a30f2-112">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="a30f2-113">這是 CSRF 的 「 跨網站 」 部分。</span><span class="sxs-lookup"><span data-stu-id="a30f2-113">This is the "cross-site" part of CSRF.</span></span>
4. <span data-ttu-id="a30f2-114">使用者按一下 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a30f2-114">The user clicks the submit button.</span></span> <span data-ttu-id="a30f2-115">瀏覽器包含與要求的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="a30f2-115">The browser includes the authentication cookie with the request.</span></span>
5. <span data-ttu-id="a30f2-116">要求使用者驗證內容，以在伺服器上執行，並可以執行已驗證的使用者允許進行的任何動作。</span><span class="sxs-lookup"><span data-stu-id="a30f2-116">The request runs on the server with the user's authentication context, and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="a30f2-117">雖然這個範例需要使用者按一下 [表單] 按鈕，惡意的頁面無法輕鬆地執行自動送出表單的指令碼一樣。</span><span class="sxs-lookup"><span data-stu-id="a30f2-117">Although this example requires the user to click the form button, the malicious page could just as easily run a script that submits the form automatically.</span></span> <span data-ttu-id="a30f2-118">此外，使用 SSL 無法防止 CSRF 攻擊中，因為惡意網站可以傳送"https://"要求。</span><span class="sxs-lookup"><span data-stu-id="a30f2-118">Moreover, using SSL does not prevent a CSRF attack, because the malicious site can send an "https://" request.</span></span>

<span data-ttu-id="a30f2-119">一般而言，CSRF 攻擊可能會對網站使用 cookie 進行驗證，因為瀏覽器傳送到目標網站的所有相關的 cookie。</span><span class="sxs-lookup"><span data-stu-id="a30f2-119">Typically, CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="a30f2-120">不過，不限於利用 cookie CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="a30f2-120">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="a30f2-121">比方說，也是很容易遭受基本和摘要式驗證。</span><span class="sxs-lookup"><span data-stu-id="a30f2-121">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="a30f2-122">之後使用者登入基本或摘要式驗證。</span><span class="sxs-lookup"><span data-stu-id="a30f2-122">After a user logs in with Basic or Digest authentication.</span></span> <span data-ttu-id="a30f2-123">瀏覽器會自動傳送認證到工作階段結束為止。</span><span class="sxs-lookup"><span data-stu-id="a30f2-123">the browser automatically sends the credentials until the session ends.</span></span>

## <a name="anti-forgery-tokens"></a><span data-ttu-id="a30f2-124">防偽語彙基元</span><span class="sxs-lookup"><span data-stu-id="a30f2-124">Anti-Forgery Tokens</span></span>

<span data-ttu-id="a30f2-125">為了避免 CSRF 攻擊，ASP.NET MVC 會使用防偽語彙基元，也稱為*要求驗證權杖*。</span><span class="sxs-lookup"><span data-stu-id="a30f2-125">To help prevent CSRF attacks, ASP.NET MVC uses anti-forgery tokens, also called *request verification tokens*.</span></span>

1. <span data-ttu-id="a30f2-126">用戶端會要求包含表單的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="a30f2-126">The client requests an HTML page that contains a form.</span></span>
2. <span data-ttu-id="a30f2-127">伺服器回應中包含兩個語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-127">The server includes two tokens in the response.</span></span> <span data-ttu-id="a30f2-128">為 cookie 會傳送一個 token。</span><span class="sxs-lookup"><span data-stu-id="a30f2-128">One token is sent as a cookie.</span></span> <span data-ttu-id="a30f2-129">其他會放置在隱藏的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="a30f2-129">The other is placed in a hidden form field.</span></span> <span data-ttu-id="a30f2-130">如此對手無法猜測值時，會隨機產生語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-130">The tokens are generated randomly so that an adversary cannot guess the values.</span></span>
3. <span data-ttu-id="a30f2-131">當用戶端提交表單時，它必須傳回給伺服器傳送這兩個語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-131">When the client submits the form, it must send both tokens back to the server.</span></span> <span data-ttu-id="a30f2-132">用戶端傳送的 cookie 語彙基元當做 cookie，並將傳送表單資料內的表單語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-132">The client sends the cookie token as a cookie, and it sends the form token inside the form data.</span></span> <span data-ttu-id="a30f2-133">（瀏覽器用戶端會自動執行此使用者送出表單時。）</span><span class="sxs-lookup"><span data-stu-id="a30f2-133">(A browser client automatically does this when the user submits the form.)</span></span>
4. <span data-ttu-id="a30f2-134">如果要求不包含這兩個語彙基元，伺服器就不允許要求。</span><span class="sxs-lookup"><span data-stu-id="a30f2-134">If a request does not include both tokens, the server disallows the request.</span></span>

<span data-ttu-id="a30f2-135">HTML 表單，以隱藏的表單語彙基元的範例如下：</span><span class="sxs-lookup"><span data-stu-id="a30f2-135">Here is an example of an HTML form with a hidden form token:</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

<span data-ttu-id="a30f2-136">防偽語彙基元運作，因為惡意的頁面無法讀取使用者的權杖，因為相同原始原則。</span><span class="sxs-lookup"><span data-stu-id="a30f2-136">Anti-forgery tokens work because the malicious page cannot read the user's tokens, due to same-origin policies.</span></span> <span data-ttu-id="a30f2-137">([相同原始原則](http://www.w3.org/Security/wiki/Same_Origin_Policy)防止存取彼此的內容的兩個不同站台上裝載的文件。</span><span class="sxs-lookup"><span data-stu-id="a30f2-137">([Same-origin policies](http://www.w3.org/Security/wiki/Same_Origin_Policy) prevent documents hosted on two different sites from accessing each other's content.</span></span> <span data-ttu-id="a30f2-138">因此在先前範例中，惡意的頁面可以將要求傳送至 example.com，但它無法讀取回應）。</span><span class="sxs-lookup"><span data-stu-id="a30f2-138">So in the earlier example, the malicious page can send requests to example.com, but it cannot read the response.)</span></span>

<span data-ttu-id="a30f2-139">若要避免 CSRF 攻擊，使用任何驗證通訊協定防偽語彙基元，瀏覽器以無訊息方式傳送認證之後使用者登入。</span><span class="sxs-lookup"><span data-stu-id="a30f2-139">To prevent CSRF attacks, use anti-forgery tokens with any authentication protocol where the browser silently sends credentials after the user logs in.</span></span> <span data-ttu-id="a30f2-140">這包括 cookie 為基礎的驗證通訊協定，例如表單驗證，以及例如基本和摘要式的驗證通訊協定。</span><span class="sxs-lookup"><span data-stu-id="a30f2-140">This includes cookie-based authentication protocols, such as forms authentication, as well as protocols such as Basic and Digest authentication.</span></span>

<span data-ttu-id="a30f2-141">您應該要求 （POST、 PUT、 DELETE） 的任何 nonsafe 方法防偽語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-141">You should require anti-forgery tokens for any nonsafe methods (POST, PUT, DELETE).</span></span> <span data-ttu-id="a30f2-142">此外，請確定安全的方法 （GET、 HEAD），並沒有任何副作用。</span><span class="sxs-lookup"><span data-stu-id="a30f2-142">Also, make sure that safe methods (GET, HEAD) do not have any side effects.</span></span> <span data-ttu-id="a30f2-143">此外，如果您啟用跨網域支援，例如 CORS 或 JSONP，諸如取得更安全的方法是可能容易遭受 CSRF 攻擊，讓攻擊者讀取可能含有機密資料。</span><span class="sxs-lookup"><span data-stu-id="a30f2-143">Moreover, if you enable cross-domain support, such as CORS or JSONP, then even safe methods like GET are potentially vulnerable to CSRF attacks, allowing the attacker to read potentially sensitive data.</span></span>

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a><span data-ttu-id="a30f2-144">ASP.NET MVC 中的防偽語彙基元</span><span class="sxs-lookup"><span data-stu-id="a30f2-144">Anti-Forgery Tokens in ASP.NET MVC</span></span>

<span data-ttu-id="a30f2-145">若要加入 Razor 頁面防偽語彙基元，請使用**HtmlHelper.AntiForgeryToken** helper 方法：</span><span class="sxs-lookup"><span data-stu-id="a30f2-145">To add the anti-forgery tokens to a Razor page, use the **HtmlHelper.AntiForgeryToken** helper method:</span></span>

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

<span data-ttu-id="a30f2-146">這個方法會加入隱藏的表單欄位，也會設定的 cookie 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-146">This method adds the hidden form field and also sets the cookie token.</span></span>

## <a name="anti-csrf-and-ajax"></a><span data-ttu-id="a30f2-147">反 CSRF 和 AJAX</span><span class="sxs-lookup"><span data-stu-id="a30f2-147">Anti-CSRF and AJAX</span></span>

<span data-ttu-id="a30f2-148">表單語彙基元可能 AJAX 要求的問題，因為 AJAX 要求可能會傳送 JSON 資料，HTML 表單資料。</span><span class="sxs-lookup"><span data-stu-id="a30f2-148">The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="a30f2-149">一個解決方案是將自訂 HTTP 標頭中傳送的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-149">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="a30f2-150">下列程式碼來產生權杖，會使用 Razor 語法，然後將權杖加入至 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="a30f2-150">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> <span data-ttu-id="a30f2-151">藉由呼叫在伺服器上產生的語彙基元**AntiForgery.GetTokens**。</span><span class="sxs-lookup"><span data-stu-id="a30f2-151">The tokens are generated at the server by calling **AntiForgery.GetTokens**.</span></span>

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

<span data-ttu-id="a30f2-152">當您處理要求時，請從要求標頭中擷取語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-152">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="a30f2-153">然後呼叫**AntiForgery.Validate**方法以驗證語彙基元。</span><span class="sxs-lookup"><span data-stu-id="a30f2-153">Then call the **AntiForgery.Validate** method to validate the tokens.</span></span> <span data-ttu-id="a30f2-154">**驗證**方法擲回例外狀況，如果語彙基元無效。</span><span class="sxs-lookup"><span data-stu-id="a30f2-154">The **Validate** method throws an exception if the tokens are not valid.</span></span>

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
