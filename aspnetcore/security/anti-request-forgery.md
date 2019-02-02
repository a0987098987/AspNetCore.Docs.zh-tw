---
title: ASP.NET Core 中的防止跨網站要求偽造 (XSRF/CSRF) 攻擊
author: steve-smith
description: 了解如何防止攻擊，惡意網站可能會影響用戶端瀏覽器與應用程式之間的互動的 web 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/anti-request-forgery
ms.openlocfilehash: 6e140717834b901e12ef7863fd07b983b0c55107
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667657"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="1eba9-103">ASP.NET Core 中的防止跨網站要求偽造 (XSRF/CSRF) 攻擊</span><span class="sxs-lookup"><span data-stu-id="1eba9-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="1eba9-104">藉由[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1eba9-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1eba9-105">跨網站偽造要求 (也稱為 XSRF 或 CSRF，唸成 *，請參閱上網*) 會讓惡意的 web 應用程式可能會影響用戶端瀏覽器和信任的 web 應用程式之間的互動的 web 應用程式對抗攻擊瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1eba9-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="1eba9-106">這些攻擊可能會因為網頁瀏覽器會將某些類型的驗證權杖會自動隨著每個要求傳送至網站。</span><span class="sxs-lookup"><span data-stu-id="1eba9-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="1eba9-107">這種形式的攻擊，也就是*單鍵攻擊*或是*工作階段乘載*因為攻擊會利用使用者先前的驗證工作階段。</span><span class="sxs-lookup"><span data-stu-id="1eba9-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="1eba9-108">CSRF 攻擊的範例：</span><span class="sxs-lookup"><span data-stu-id="1eba9-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="1eba9-109">使用者登入`www.good-banking-site.com`使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="1eba9-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="1eba9-110">伺服器會驗證使用者，並會發出包含驗證 cookie 的回應。</span><span class="sxs-lookup"><span data-stu-id="1eba9-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="1eba9-111">站台是很容易遭受攻擊，因為它所信任的任何要求，它會收到包含有效的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="1eba9-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="1eba9-112">使用者瀏覽惡意網站， `www.bad-crook-site.com`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="1eba9-113">惡意的網站， `www.bad-crook-site.com`，包含 HTML 表單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1eba9-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="1eba9-114">請注意，表單的`action`貼文到有弱點網站，而非惡意的網站。</span><span class="sxs-lookup"><span data-stu-id="1eba9-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="1eba9-115">這是 CSRF 的 「 跨站台 」 部分。</span><span class="sxs-lookup"><span data-stu-id="1eba9-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="1eba9-116">使用者選取 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1eba9-116">The user selects the submit button.</span></span> <span data-ttu-id="1eba9-117">瀏覽器提出要求，並會自動包含所要求的網域，驗證 cookie `www.good-banking-site.com`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="1eba9-118">執行要求`www.good-banking-site.com`與使用者的驗證內容的伺服器，而且可以執行已驗證的使用者可以執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="1eba9-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="1eba9-119">除了指定案例中，使用者選取按鈕以送出表單時，惡意網站可能：</span><span class="sxs-lookup"><span data-stu-id="1eba9-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="1eba9-120">執行自動送出表單的指令碼。</span><span class="sxs-lookup"><span data-stu-id="1eba9-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="1eba9-121">傳送的 AJAX 要求送出表單。</span><span class="sxs-lookup"><span data-stu-id="1eba9-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="1eba9-122">隱藏使用 CSS 的形式。</span><span class="sxs-lookup"><span data-stu-id="1eba9-122">Hide the form using CSS.</span></span>

<span data-ttu-id="1eba9-123">這些替代案例不需要任何動作或一開始瀏覽惡意的網站以外使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="1eba9-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="1eba9-124">使用 HTTPS 不會防止 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="1eba9-125">惡意網站可以傳送 `https://www.good-banking-site.com/` 要求只一樣，它可以傳送不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="1eba9-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="1eba9-126">某些攻擊會鎖定在此情況下之影像標記可用來執行動作的回應 GET 要求的端點。</span><span class="sxs-lookup"><span data-stu-id="1eba9-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="1eba9-127">這種形式的攻擊會讓映像，但封鎖 JavaScript 的論壇網站上。</span><span class="sxs-lookup"><span data-stu-id="1eba9-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="1eba9-128">變更狀態的 GET 要求，其中會改變變數或資源，應用程式容易受到惡意攻擊的影響。</span><span class="sxs-lookup"><span data-stu-id="1eba9-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="1eba9-129">**變更狀態的 GET 要求是不安全。最佳做法是永遠不會變更在 GET 要求的狀態。**</span><span class="sxs-lookup"><span data-stu-id="1eba9-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="1eba9-130">CSRF 攻擊是可能對 web 應用程式使用 cookie 進行驗證，因為：</span><span class="sxs-lookup"><span data-stu-id="1eba9-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="1eba9-131">瀏覽器儲存 web 應用程式所發出的 cookie。</span><span class="sxs-lookup"><span data-stu-id="1eba9-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="1eba9-132">預存的 cookie 會包含已驗證的使用者工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="1eba9-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="1eba9-133">瀏覽器傳送的所有 cookie 與網域相關聯 web 應用程式無論在瀏覽器內產生應用程式的要求方式的每個要求。</span><span class="sxs-lookup"><span data-stu-id="1eba9-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="1eba9-134">不過，CSRF 攻擊不會限制為 cookie 的全部功能。</span><span class="sxs-lookup"><span data-stu-id="1eba9-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="1eba9-135">例如，基本和摘要式驗證也是易受攻擊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="1eba9-136">瀏覽器使用基本或摘要式驗證的使用者登入之後，自動傳送到工作階段認證&dagger;結束。</span><span class="sxs-lookup"><span data-stu-id="1eba9-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="1eba9-137">&dagger;在此情況下，*工作階段*指的用戶端工作階段期間會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1eba9-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="1eba9-138">它是不相關的伺服器端工作階段或[ASP.NET Core 工作階段中介軟體](xref:fundamentals/app-state)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="1eba9-139">使用者可以防範 CSRF 弱點採取預防措施：</span><span class="sxs-lookup"><span data-stu-id="1eba9-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="1eba9-140">從 web 應用程式時使用它們完成登入。</span><span class="sxs-lookup"><span data-stu-id="1eba9-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="1eba9-141">定期清除瀏覽器 cookie。</span><span class="sxs-lookup"><span data-stu-id="1eba9-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="1eba9-142">不過，CSRF 弱點基本上都是 web 應用程式，而非由終端使用者的問題。</span><span class="sxs-lookup"><span data-stu-id="1eba9-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="1eba9-143">驗證基本概念</span><span class="sxs-lookup"><span data-stu-id="1eba9-143">Authentication fundamentals</span></span>

<span data-ttu-id="1eba9-144">熱門的形式的驗證 cookie 為基礎的驗證。</span><span class="sxs-lookup"><span data-stu-id="1eba9-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="1eba9-145">權杖型驗證系統越來越大受歡迎，特別是針對單一頁面應用程式 (Spa)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="1eba9-146">以 cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="1eba9-146">Cookie-based authentication</span></span>

<span data-ttu-id="1eba9-147">當使用者驗證使用使用者名稱和密碼時，會核發給這些包含可以用於驗證和授權的驗證票證的權杖。</span><span class="sxs-lookup"><span data-stu-id="1eba9-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="1eba9-148">權杖會儲存為隨附於每個要求的用戶端的 cookie 可讓。</span><span class="sxs-lookup"><span data-stu-id="1eba9-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="1eba9-149">產生和驗證此 cookie 是由 Cookie 驗證中介軟體執行的。</span><span class="sxs-lookup"><span data-stu-id="1eba9-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="1eba9-150">[中介軟體](xref:fundamentals/middleware/index)序列化的加密 cookie 中的使用者主體。</span><span class="sxs-lookup"><span data-stu-id="1eba9-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="1eba9-151">在後續的要求中, 介軟體驗證 cookie、 重新建立主體，並將指派主體[使用者](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)屬性[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="1eba9-152">權杖型驗證</span><span class="sxs-lookup"><span data-stu-id="1eba9-152">Token-based authentication</span></span>

<span data-ttu-id="1eba9-153">當驗證使用者時，會核發給這些語彙基元 （不 antiforgery 權杖）。</span><span class="sxs-lookup"><span data-stu-id="1eba9-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="1eba9-154">權杖包含使用者資訊的形式[宣告](/dotnet/framework/security/claims-based-identity-model)或指向維護應用程式中的使用者狀態的應用程式的參考語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1eba9-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="1eba9-155">當使用者嘗試存取需要驗證的資源時，就會將權杖傳送至應用程式與其他授權標頭的持有人權杖的格式。</span><span class="sxs-lookup"><span data-stu-id="1eba9-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="1eba9-156">這可讓應用程式的無狀態。</span><span class="sxs-lookup"><span data-stu-id="1eba9-156">This makes the app stateless.</span></span> <span data-ttu-id="1eba9-157">在每個後續的要求中，權杖會傳遞要求中的伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="1eba9-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="1eba9-158">這個語彙基元未*加密*; 它具有*編碼*。</span><span class="sxs-lookup"><span data-stu-id="1eba9-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="1eba9-159">在伺服器上，此語彙基元解碼成存取其資訊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="1eba9-160">在後續要求中傳送的權杖，會在瀏覽器的本機儲存體中儲存權杖。</span><span class="sxs-lookup"><span data-stu-id="1eba9-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="1eba9-161">如果瀏覽器的本機儲存體中儲存的語彙基元，則不會擔心 CSRF 的弱點可能會。</span><span class="sxs-lookup"><span data-stu-id="1eba9-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="1eba9-162">CSRF 權杖儲存在 cookie 中時的考量。</span><span class="sxs-lookup"><span data-stu-id="1eba9-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="1eba9-163">裝載在一個網域的多個應用程式</span><span class="sxs-lookup"><span data-stu-id="1eba9-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="1eba9-164">共用的裝載環境包括工作階段攔截、 登入 CSRF 和其他攻擊變得更加容易。</span><span class="sxs-lookup"><span data-stu-id="1eba9-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="1eba9-165">雖然`example1.contoso.net`並`example2.contoso.net`是不同的主控件，在主機之間沒有隱含的信任關係`*.contoso.net`網域。</span><span class="sxs-lookup"><span data-stu-id="1eba9-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="1eba9-166">這個隱含的信任關係可讓可能不受信任的主機會影響彼此的 cookie （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。</span><span class="sxs-lookup"><span data-stu-id="1eba9-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="1eba9-167">不會共用網域可防止惡意探索應用程式裝載於相同的網域之間的信任的 cookie 的攻擊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="1eba9-168">當每個應用程式裝載在自己的網域中時，會利用沒有隱含的 cookie 信任關係。</span><span class="sxs-lookup"><span data-stu-id="1eba9-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="1eba9-169">ASP.NET Core antiforgery 組態</span><span class="sxs-lookup"><span data-stu-id="1eba9-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="1eba9-170">ASP.NET Core 會實作使用 antiforgery [ASP.NET Core 資料保護](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="1eba9-171">資料保護堆疊必須設定為伺服器陣列中。</span><span class="sxs-lookup"><span data-stu-id="1eba9-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="1eba9-172">請參閱[設定資料保護](xref:security/data-protection/configuration/overview)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="1eba9-173">在 ASP.NET Core 2.0 或更新版本中， [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery 權杖插入 HTML 表單項目。</span><span class="sxs-lookup"><span data-stu-id="1eba9-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="1eba9-174">Razor 檔案中的以下標記會自動產生的防偽語彙基元：</span><span class="sxs-lookup"><span data-stu-id="1eba9-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="1eba9-175">同樣地， [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)依預設會產生 antiforgery 權杖，如果表單的方法不是 GET。</span><span class="sxs-lookup"><span data-stu-id="1eba9-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="1eba9-176">自動產生的防偽語彙基元的 HTML 表單項目發生時`<form>`標記包含`method="post"`屬性和下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="1eba9-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="1eba9-177">動作屬性是空的 (`action=""`)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="1eba9-178">未提供的 action 屬性 (`<form method="post">`)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="1eba9-179">您可以停用自動產生的防偽語彙基元，為 HTML 表單項目：</span><span class="sxs-lookup"><span data-stu-id="1eba9-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="1eba9-180">明確停用使用防偽權杖`asp-antiforgery`屬性：</span><span class="sxs-lookup"><span data-stu-id="1eba9-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="1eba9-181">表單項目是選擇外的標籤協助程式使用標籤協助程式[！ 退出符號](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="1eba9-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="1eba9-182">移除`FormTagHelper`從檢視。</span><span class="sxs-lookup"><span data-stu-id="1eba9-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="1eba9-183">`FormTagHelper`可以從檢視移除，藉由將下列指示詞新增至 Razor 檢視：</span><span class="sxs-lookup"><span data-stu-id="1eba9-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="1eba9-184">[Razor 頁面](xref:razor-pages/index)從 XSRF/CSRF 會自動施以保護。</span><span class="sxs-lookup"><span data-stu-id="1eba9-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="1eba9-185">如需詳細資訊，請參閱 < [XSRF/CSRF 和 Razor Pages](xref:razor-pages/index#xsrf)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="1eba9-186">對抗 CSRF 攻擊的最常見方法是使用*同步器 Token 模式*(spanning tree Protocol)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="1eba9-187">當使用者要求包含表單資料的頁面時，會使用 spanning tree Protocol:</span><span class="sxs-lookup"><span data-stu-id="1eba9-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="1eba9-188">伺服器會傳送至用戶端的目前使用者的身分識別相關聯的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1eba9-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="1eba9-189">用戶端上一步將權杖傳送至伺服器進行驗證。</span><span class="sxs-lookup"><span data-stu-id="1eba9-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="1eba9-190">如果伺服器收到不符合已驗證的使用者的身分識別的權杖，則會拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="1eba9-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="1eba9-191">權杖的唯一且無法預測。</span><span class="sxs-lookup"><span data-stu-id="1eba9-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="1eba9-192">語彙基元也可用來確保適當排序的一系列的要求 (例如，確保要求序列的： 第 1 頁&ndash;第 2 頁&ndash;頁面 3)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="1eba9-193">所有的 ASP.NET Core MVC 和 Razor 頁面範本中的表單產生防偽語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1eba9-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="1eba9-194">下列配對檢視範例產生的防偽語彙基元：</span><span class="sxs-lookup"><span data-stu-id="1eba9-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="1eba9-195">明確地將加入的 antiforgery 權杖`<form>`而不使用標籤協助程式與 HTML 協助程式元素[ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="1eba9-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="1eba9-196">在每個上述所有情況下，ASP.NET Core 會將隱藏的表單欄位，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1eba9-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="1eba9-197">ASP.NET Core 包含三個[篩選器](xref:mvc/controllers/filters)使用防偽權杖：</span><span class="sxs-lookup"><span data-stu-id="1eba9-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="1eba9-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="1eba9-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="1eba9-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="1eba9-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="1eba9-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="1eba9-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="1eba9-201">Antiforgery 選項</span><span class="sxs-lookup"><span data-stu-id="1eba9-201">Antiforgery options</span></span>

<span data-ttu-id="1eba9-202">來自訂[antiforgery 選項](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)在`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1eba9-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="1eba9-203">&dagger;設定 antiforgery`Cookie`屬性使用的屬性[CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder)類別。</span><span class="sxs-lookup"><span data-stu-id="1eba9-203">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="1eba9-204">選項</span><span class="sxs-lookup"><span data-stu-id="1eba9-204">Option</span></span> | <span data-ttu-id="1eba9-205">描述</span><span class="sxs-lookup"><span data-stu-id="1eba9-205">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="1eba9-206">Cookie</span><span class="sxs-lookup"><span data-stu-id="1eba9-206">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="1eba9-207">決定用來建立防偽 cookie 的設定。</span><span class="sxs-lookup"><span data-stu-id="1eba9-207">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="1eba9-208">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="1eba9-208">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="1eba9-209">防偽系統用來呈現檢視中的防偽語彙基元的隱藏的表單欄位名稱。</span><span class="sxs-lookup"><span data-stu-id="1eba9-209">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="1eba9-210">HeaderName</span><span class="sxs-lookup"><span data-stu-id="1eba9-210">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="1eba9-211">防偽系統所使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="1eba9-211">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="1eba9-212">如果`null`，系統會考慮只表單資料。</span><span class="sxs-lookup"><span data-stu-id="1eba9-212">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="1eba9-213">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="1eba9-213">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="1eba9-214">指定是否要隱藏產生`X-Frame-Options`標頭。</span><span class="sxs-lookup"><span data-stu-id="1eba9-214">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="1eba9-215">根據預設，標頭會產生含有"Sameorigin 所"的值。</span><span class="sxs-lookup"><span data-stu-id="1eba9-215">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="1eba9-216">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-216">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="1eba9-217">選項</span><span class="sxs-lookup"><span data-stu-id="1eba9-217">Option</span></span> | <span data-ttu-id="1eba9-218">描述</span><span class="sxs-lookup"><span data-stu-id="1eba9-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="1eba9-219">Cookie</span><span class="sxs-lookup"><span data-stu-id="1eba9-219">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="1eba9-220">決定用來建立防偽 cookie 的設定。</span><span class="sxs-lookup"><span data-stu-id="1eba9-220">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="1eba9-221">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="1eba9-221">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="1eba9-222">Cookie 的網域。</span><span class="sxs-lookup"><span data-stu-id="1eba9-222">The domain of the cookie.</span></span> <span data-ttu-id="1eba9-223">預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-223">Defaults to `null`.</span></span> <span data-ttu-id="1eba9-224">這個屬性已經過時，將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="1eba9-224">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1eba9-225">建議的替代做法是 Cookie.Domain。</span><span class="sxs-lookup"><span data-stu-id="1eba9-225">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="1eba9-226">CookieName</span><span class="sxs-lookup"><span data-stu-id="1eba9-226">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="1eba9-227">Cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="1eba9-227">The name of the cookie.</span></span> <span data-ttu-id="1eba9-228">如果未設定，系統會產生唯一的名稱開頭[DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) ("。AspNetCore.Antiforgery。")。</span><span class="sxs-lookup"><span data-stu-id="1eba9-228">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="1eba9-229">這個屬性已經過時，將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="1eba9-229">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1eba9-230">建議的替代做法是 Cookie.Name。</span><span class="sxs-lookup"><span data-stu-id="1eba9-230">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="1eba9-231">CookiePath</span><span class="sxs-lookup"><span data-stu-id="1eba9-231">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="1eba9-232">在 cookie 上設定的路徑。</span><span class="sxs-lookup"><span data-stu-id="1eba9-232">The path set on the cookie.</span></span> <span data-ttu-id="1eba9-233">這個屬性已經過時，將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="1eba9-233">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1eba9-234">建議的替代做法是 Cookie.Path。</span><span class="sxs-lookup"><span data-stu-id="1eba9-234">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="1eba9-235">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="1eba9-235">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="1eba9-236">防偽系統用來呈現檢視中的防偽語彙基元的隱藏的表單欄位名稱。</span><span class="sxs-lookup"><span data-stu-id="1eba9-236">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="1eba9-237">HeaderName</span><span class="sxs-lookup"><span data-stu-id="1eba9-237">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="1eba9-238">防偽系統所使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="1eba9-238">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="1eba9-239">如果`null`，系統會考慮只表單資料。</span><span class="sxs-lookup"><span data-stu-id="1eba9-239">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="1eba9-240">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="1eba9-240">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="1eba9-241">指定防偽系統是否需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="1eba9-241">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="1eba9-242">如果`true`，非 HTTPS 的要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="1eba9-242">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="1eba9-243">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-243">Defaults to `false`.</span></span> <span data-ttu-id="1eba9-244">這個屬性已經過時，將在未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="1eba9-244">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1eba9-245">建議的替代做法是將 Cookie.SecurePolicy。</span><span class="sxs-lookup"><span data-stu-id="1eba9-245">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="1eba9-246">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="1eba9-246">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="1eba9-247">指定是否要隱藏產生`X-Frame-Options`標頭。</span><span class="sxs-lookup"><span data-stu-id="1eba9-247">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="1eba9-248">根據預設，標頭會產生含有"Sameorigin 所"的值。</span><span class="sxs-lookup"><span data-stu-id="1eba9-248">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="1eba9-249">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-249">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="1eba9-250">如需詳細資訊，請參閱 < [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-250">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="1eba9-251">使用 IAntiforgery 設定 antiforgery 功能</span><span class="sxs-lookup"><span data-stu-id="1eba9-251">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="1eba9-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)提供的 API 來設定 antiforgery 的功能。</span><span class="sxs-lookup"><span data-stu-id="1eba9-252">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="1eba9-253">`IAntiforgery` 可在要求`Configure`方法的`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="1eba9-253">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="1eba9-254">下列範例會使用從應用程式的首頁上的中介軟體，以產生 antiforgery 權杖，並在回應中傳送 cookie （使用預設 Angular 命名慣例本主題稍後所述）：</span><span class="sxs-lookup"><span data-stu-id="1eba9-254">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="1eba9-255">需要防偽驗證</span><span class="sxs-lookup"><span data-stu-id="1eba9-255">Require antiforgery validation</span></span>

<span data-ttu-id="1eba9-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)是動作篩選條件可以套用至個別動作、 控制器或全域。</span><span class="sxs-lookup"><span data-stu-id="1eba9-256">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="1eba9-257">除非要求包含有效的防偽語彙基元，則會封鎖對 套用此篩選條件的動作的要求。</span><span class="sxs-lookup"><span data-stu-id="1eba9-257">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="1eba9-258">`ValidateAntiForgeryToken`屬性裝飾，包括 HTTP GET 要求至動作方法要求需要的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1eba9-258">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="1eba9-259">如果`ValidateAntiForgeryToken`屬性會套用跨應用程式的控制站，可以使用覆寫`IgnoreAntiforgeryToken`屬性。</span><span class="sxs-lookup"><span data-stu-id="1eba9-259">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="1eba9-260">ASP.NET Core 不支援自動將 antiforgery 權杖新增至 GET 要求。</span><span class="sxs-lookup"><span data-stu-id="1eba9-260">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="1eba9-261">會自動進行驗證不安全的 HTTP 方法只 antiforgery 的權杖</span><span class="sxs-lookup"><span data-stu-id="1eba9-261">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="1eba9-262">ASP.NET Core 應用程式不會產生安全的 HTTP 方法 （GET、 HEAD、 選項和追蹤） 的防偽語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1eba9-262">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="1eba9-263">而不是廣泛套用`ValidateAntiForgeryToken`屬性，然後將它與覆寫`IgnoreAntiforgeryToken`屬性， [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)屬性可用。</span><span class="sxs-lookup"><span data-stu-id="1eba9-263">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="1eba9-264">此屬性運作方式完全相同`ValidateAntiForgeryToken`屬性，不同之處在於它不需要使用下列的 HTTP 方法所提出之要求的權杖：</span><span class="sxs-lookup"><span data-stu-id="1eba9-264">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="1eba9-265">GET</span><span class="sxs-lookup"><span data-stu-id="1eba9-265">GET</span></span>
* <span data-ttu-id="1eba9-266">HEAD</span><span class="sxs-lookup"><span data-stu-id="1eba9-266">HEAD</span></span>
* <span data-ttu-id="1eba9-267">選項</span><span class="sxs-lookup"><span data-stu-id="1eba9-267">OPTIONS</span></span>
* <span data-ttu-id="1eba9-268">TRACE</span><span class="sxs-lookup"><span data-stu-id="1eba9-268">TRACE</span></span>

<span data-ttu-id="1eba9-269">我們建議使用`AutoValidateAntiforgeryToken`廣泛用於非 API 案例。</span><span class="sxs-lookup"><span data-stu-id="1eba9-269">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="1eba9-270">這可確保預設受到保護後動作。</span><span class="sxs-lookup"><span data-stu-id="1eba9-270">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="1eba9-271">替代方法是忽略 antiforgery 權杖依預設，除非`ValidateAntiForgeryToken`套用至個別動作方法。</span><span class="sxs-lookup"><span data-stu-id="1eba9-271">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="1eba9-272">它比較可能在此案例中的 POST 動作方法來保留受保護的誤離開應用程式容易遭受 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-272">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="1eba9-273">所有貼文應該傳送的防偽語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1eba9-273">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="1eba9-274">Api 不需要傳送非 cookie 權杖的一部分的自動機制。</span><span class="sxs-lookup"><span data-stu-id="1eba9-274">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="1eba9-275">實作可能取決於用戶端程式碼實作。</span><span class="sxs-lookup"><span data-stu-id="1eba9-275">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="1eba9-276">一些範例如下所示：</span><span class="sxs-lookup"><span data-stu-id="1eba9-276">Some examples are shown below:</span></span>

<span data-ttu-id="1eba9-277">類別層級的範例：</span><span class="sxs-lookup"><span data-stu-id="1eba9-277">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="1eba9-278">全域的範例：</span><span class="sxs-lookup"><span data-stu-id="1eba9-278">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="1eba9-279">覆寫全域或控制器 antiforgery 屬性</span><span class="sxs-lookup"><span data-stu-id="1eba9-279">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="1eba9-280">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)篩選器可用來消除對指定的 「 動作 」 （或稱 「 控制器 」） 的 antiforgery 權杖。</span><span class="sxs-lookup"><span data-stu-id="1eba9-280">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="1eba9-281">此篩選器套用時，覆寫`ValidateAntiForgeryToken`和`AutoValidateAntiforgeryToken`（全域或控制站上），指定在較高層級的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="1eba9-281">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="1eba9-282">在驗證之後，重新整理權杖</span><span class="sxs-lookup"><span data-stu-id="1eba9-282">Refresh tokens after authentication</span></span>

<span data-ttu-id="1eba9-283">使用者驗證的使用者重新導向至檢視表或 Razor 頁面 頁面之後，就應該重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="1eba9-283">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="1eba9-284">JavaScript、 AJAX 和 Spa</span><span class="sxs-lookup"><span data-stu-id="1eba9-284">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="1eba9-285">在傳統的 HTML 型應用程式，antiforgery 權杖會傳遞至使用隱藏的表單欄位的伺服器。</span><span class="sxs-lookup"><span data-stu-id="1eba9-285">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="1eba9-286">在現代化的 JavaScript 應用程式和 Spa，會以程式設計方式提出許多要求。</span><span class="sxs-lookup"><span data-stu-id="1eba9-286">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="1eba9-287">這些 AJAX 要求可能會使用其他技術 （例如要求標頭或 cookie） 傳送的權杖。</span><span class="sxs-lookup"><span data-stu-id="1eba9-287">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="1eba9-288">如果 cookie 用來儲存驗證權杖，並驗證伺服器上的 API 要求，CSRF 是潛在的問題。</span><span class="sxs-lookup"><span data-stu-id="1eba9-288">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="1eba9-289">如果本機儲存體來儲存權杖，可能會降低 CSRF 的弱點可能會因為從本機儲存體的值不自動傳送至每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="1eba9-289">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="1eba9-290">若要將 antiforgery 權杖儲存在用戶端和傳送權杖做為要求標頭是建議的方法，因此，使用本機儲存體。</span><span class="sxs-lookup"><span data-stu-id="1eba9-290">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="1eba9-291">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1eba9-291">JavaScript</span></span>

<span data-ttu-id="1eba9-292">使用 JavaScript 與檢視，可以建立權杖使用檢視內的服務。</span><span class="sxs-lookup"><span data-stu-id="1eba9-292">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="1eba9-293">插入[Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)服務到檢視，然後呼叫[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="1eba9-293">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="1eba9-294">這種方法就不需要直接處理伺服器上設定 cookie，或從用戶端讀取。</span><span class="sxs-lookup"><span data-stu-id="1eba9-294">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="1eba9-295">上述範例中使用 JavaScript 針對 POST 的 AJAX 標頭讀取隱藏的欄位值。</span><span class="sxs-lookup"><span data-stu-id="1eba9-295">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="1eba9-296">JavaScript 也可以存取 cookie 中的權杖，並使用 cookie 的內容權杖的值建立的標頭。</span><span class="sxs-lookup"><span data-stu-id="1eba9-296">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="1eba9-297">假設指令碼呼叫的標頭中傳送的權杖要求`X-CSRF-TOKEN`，將 antiforgery 服務設定為尋找`X-CSRF-TOKEN`標頭：</span><span class="sxs-lookup"><span data-stu-id="1eba9-297">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="1eba9-298">下列範例會使用 JavaScript 來提出 AJAX 要求使用適當的標頭：</span><span class="sxs-lookup"><span data-stu-id="1eba9-298">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="1eba9-299">AngularJS</span><span class="sxs-lookup"><span data-stu-id="1eba9-299">AngularJS</span></span>

<span data-ttu-id="1eba9-300">AngularJS 使用位址 CSRF 慣例。</span><span class="sxs-lookup"><span data-stu-id="1eba9-300">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="1eba9-301">如果伺服器會傳送具有名稱的 cookie `XSRF-TOKEN`，AngularJS`$http`服務將 cookie 的值加入標頭將要求傳送到伺服器時。</span><span class="sxs-lookup"><span data-stu-id="1eba9-301">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="1eba9-302">此程序是自動的。</span><span class="sxs-lookup"><span data-stu-id="1eba9-302">This process is automatic.</span></span> <span data-ttu-id="1eba9-303">標頭不需要明確設定的用戶端中。</span><span class="sxs-lookup"><span data-stu-id="1eba9-303">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="1eba9-304">標頭名稱`X-XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-304">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="1eba9-305">伺服器應該偵測此標頭，並驗證其內容。</span><span class="sxs-lookup"><span data-stu-id="1eba9-305">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="1eba9-306">ASP.NET Core api，才能使用此慣例，在您的應用程式啟動：</span><span class="sxs-lookup"><span data-stu-id="1eba9-306">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="1eba9-307">設定您的應用程式提供的權杖在 cookie 中稱為`XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-307">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="1eba9-308">將 antiforgery 服務設定為尋找標頭`X-XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="1eba9-308">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="1eba9-309">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1eba9-309">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="1eba9-310">擴充 antiforgery</span><span class="sxs-lookup"><span data-stu-id="1eba9-310">Extend antiforgery</span></span>

<span data-ttu-id="1eba9-311">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員在每個權杖中的反覆存取其他資料來擴充防 CSRF 系統的行為。</span><span class="sxs-lookup"><span data-stu-id="1eba9-311">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="1eba9-312">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)每次呼叫方法會產生欄位的語彙基元，並傳回值內嵌在產生的權杖。</span><span class="sxs-lookup"><span data-stu-id="1eba9-312">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="1eba9-313">實作者可能傳回的時間戳記、 nonce 或任何其他值，然後呼叫[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)來驗證權杖時驗證這項資料。</span><span class="sxs-lookup"><span data-stu-id="1eba9-313">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="1eba9-314">用戶端的使用者名稱已內嵌在產生的權杖中，這樣就不需要加入這項資訊。</span><span class="sxs-lookup"><span data-stu-id="1eba9-314">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="1eba9-315">如果語彙基元包含補充資料，但不是`IAntiForgeryAdditionalDataProvider`是設定，未驗證的補充資料。</span><span class="sxs-lookup"><span data-stu-id="1eba9-315">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1eba9-316">其他資源</span><span class="sxs-lookup"><span data-stu-id="1eba9-316">Additional resources</span></span>

* <span data-ttu-id="1eba9-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[開啟 Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP)。</span><span class="sxs-lookup"><span data-stu-id="1eba9-317">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
