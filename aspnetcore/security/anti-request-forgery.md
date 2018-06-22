---
title: 防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core
author: steve-smith
description: 了解如何防止攻擊，其中惡意網站可能會影響用戶端瀏覽器和應用程式之間的互動的 web 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
uid: security/anti-request-forgery
ms.openlocfilehash: a00bd4ff4b265a19766e54e6ad6b97b870df56c5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279593"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="1343e-103">防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1343e-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="1343e-104">由[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1343e-104">By [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1343e-105">跨站台要求偽造 (也稱為 XSRF 或 CSRF，唸成 *，請參閱衝浪*) 是 web 裝載的應用程式，惡意的 web 應用程式可能會影響用戶端瀏覽器和信任的 web 應用程式之間的互動攻擊瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1343e-105">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="1343e-106">這些攻擊可能會因為網頁瀏覽器傳送到網站的某些類型的驗證權杖會自動與每個要求。</span><span class="sxs-lookup"><span data-stu-id="1343e-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="1343e-107">這種攻擊形式就是所謂*單鍵攻擊*或*工作階段乘載*使用者因為攻擊利用先前的驗證工作階段。</span><span class="sxs-lookup"><span data-stu-id="1343e-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="1343e-108">CSRF 攻擊的範例：</span><span class="sxs-lookup"><span data-stu-id="1343e-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="1343e-109">使用者登入`www.good-banking-site.com`使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="1343e-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="1343e-110">伺服器會驗證使用者，並會發出包含驗證 cookie 的回應。</span><span class="sxs-lookup"><span data-stu-id="1343e-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="1343e-111">站台是很容易遭受攻擊，因為它信任它會接收一個有效的驗證 cookie 的任何要求。</span><span class="sxs-lookup"><span data-stu-id="1343e-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="1343e-112">使用者造訪惡意網站， `www.bad-crook-site.com`。</span><span class="sxs-lookup"><span data-stu-id="1343e-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="1343e-113">惡意的站台， `www.bad-crook-site.com`，包含 HTML 表單與下列類似：</span><span class="sxs-lookup"><span data-stu-id="1343e-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="1343e-114">請注意，表單的`action`文章易受攻擊的站台，不供惡意網站。</span><span class="sxs-lookup"><span data-stu-id="1343e-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="1343e-115">這是 CSRF 的 「 跨網站 」 部分。</span><span class="sxs-lookup"><span data-stu-id="1343e-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="1343e-116">使用者會選取 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="1343e-116">The user selects the submit button.</span></span> <span data-ttu-id="1343e-117">瀏覽器提出要求，並會自動包含所要求的網域，驗證 cookie `www.good-banking-site.com`。</span><span class="sxs-lookup"><span data-stu-id="1343e-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="1343e-118">執行要求`www.good-banking-site.com`與使用者的驗證內容的伺服器，而且可以執行已驗證的使用者可以執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="1343e-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="1343e-119">除了指定案例中，使用者在選取按鈕送出表單，惡意網站無法：</span><span class="sxs-lookup"><span data-stu-id="1343e-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="1343e-120">執行自動送出表單的指令碼。</span><span class="sxs-lookup"><span data-stu-id="1343e-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="1343e-121">傳送 AJAX 要求送出表單。</span><span class="sxs-lookup"><span data-stu-id="1343e-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="1343e-122">隱藏使用 CSS 的表單。</span><span class="sxs-lookup"><span data-stu-id="1343e-122">Hide the form using CSS.</span></span>

<span data-ttu-id="1343e-123">這些替代案例不需要任何動作或以外一開始瀏覽惡意網站使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="1343e-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="1343e-124">使用 HTTPS，不會防止 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="1343e-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="1343e-125">惡意的站台可以傳送`https://www.good-banking-site.com/`要求很容易就可以傳送不安全的要求。</span><span class="sxs-lookup"><span data-stu-id="1343e-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="1343e-126">部分攻擊的目標回應至 GET 要求的端點在此情況下之影像標記可以用來執行此動作。</span><span class="sxs-lookup"><span data-stu-id="1343e-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="1343e-127">這種形式的攻擊常會論壇網站允許映像，但封鎖 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="1343e-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="1343e-128">變更狀態的 GET 要求，其中會變更變數或資源，應用程式很容易受到惡意的攻擊。</span><span class="sxs-lookup"><span data-stu-id="1343e-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="1343e-129">**變更狀態的 GET 要求是不安全的。最佳做法是永遠不會變更的 GET 要求的狀態。**</span><span class="sxs-lookup"><span data-stu-id="1343e-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="1343e-130">針對 web 應用程式都使用 cookie 進行驗證，因為可能 CSRF 攻擊︰</span><span class="sxs-lookup"><span data-stu-id="1343e-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="1343e-131">瀏覽器儲存 web 應用程式所發出的 cookie。</span><span class="sxs-lookup"><span data-stu-id="1343e-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="1343e-132">預存的 cookie 會包含已驗證的使用者工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="1343e-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="1343e-133">瀏覽器傳送的所有 cookie 相關聯 web 應用程式網域每個要求，不論在瀏覽器內已產生的應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="1343e-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="1343e-134">不過，不限制 CSRF 攻擊利用 cookie。</span><span class="sxs-lookup"><span data-stu-id="1343e-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="1343e-135">比方說，也是很容易遭受基本和摘要式驗證。</span><span class="sxs-lookup"><span data-stu-id="1343e-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="1343e-136">瀏覽器使用者登入基本或摘要式驗證之後，自動傳送直到工作階段的認證&dagger;結束。</span><span class="sxs-lookup"><span data-stu-id="1343e-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="1343e-137">&dagger;在此內容中*工作階段*指的是用戶端工作階段期間會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1343e-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="1343e-138">它是不相關的伺服器端工作階段或[ASP.NET Core 工作階段中介軟體](xref:fundamentals/app-state)。</span><span class="sxs-lookup"><span data-stu-id="1343e-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="1343e-139">使用者可以防範 CSRF 弱點，採取預防措施：</span><span class="sxs-lookup"><span data-stu-id="1343e-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="1343e-140">登出 web 應用程式使用它們。</span><span class="sxs-lookup"><span data-stu-id="1343e-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="1343e-141">定期清除瀏覽器 cookie。</span><span class="sxs-lookup"><span data-stu-id="1343e-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="1343e-142">不過，CSRF 弱點基本上都是使用 web 應用程式，終端使用者的問題。</span><span class="sxs-lookup"><span data-stu-id="1343e-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="1343e-143">驗證基本概念</span><span class="sxs-lookup"><span data-stu-id="1343e-143">Authentication fundamentals</span></span>

<span data-ttu-id="1343e-144">受歡迎的形式的驗證 cookie 為基礎的驗證。</span><span class="sxs-lookup"><span data-stu-id="1343e-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="1343e-145">權杖型驗證系統會受歡迎情況看出，在不斷增加，特別是針對單一頁面應用程式 (SPAs)。</span><span class="sxs-lookup"><span data-stu-id="1343e-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="1343e-146">Cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="1343e-146">Cookie-based authentication</span></span>

<span data-ttu-id="1343e-147">當使用者使用其使用者名稱和密碼，它們被發行包含可以用於驗證和授權的驗證票證的權杖。</span><span class="sxs-lookup"><span data-stu-id="1343e-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="1343e-148">權杖是以伴隨著每個要求的用戶端的 cookie 會儲存。</span><span class="sxs-lookup"><span data-stu-id="1343e-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="1343e-149">產生和驗證此 cookie 是由 Cookie 驗證中介軟體執行。</span><span class="sxs-lookup"><span data-stu-id="1343e-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="1343e-150">[中介軟體](xref:fundamentals/middleware/index)序列化經過加密的 cookie 的使用者主體。</span><span class="sxs-lookup"><span data-stu-id="1343e-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="1343e-151">在後續要求中中, 介軟體驗證 cookie、 主體，會重新建立並指派主體[使用者](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)屬性[HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)。</span><span class="sxs-lookup"><span data-stu-id="1343e-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="1343e-152">權杖型驗證</span><span class="sxs-lookup"><span data-stu-id="1343e-152">Token-based authentication</span></span>

<span data-ttu-id="1343e-153">當使用者進行驗證時，它們被發行的權杖 （不 antiforgery 語彙基元）。</span><span class="sxs-lookup"><span data-stu-id="1343e-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="1343e-154">權杖包含使用者資訊的形式[宣告](/dotnet/framework/security/claims-based-identity-model)或指向維護應用程式中的使用者狀態的應用程式參考語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="1343e-155">當使用者嘗試存取需要驗證的資源時，權杖會傳送至應用程式與其他授權標頭的持有人權杖的格式。</span><span class="sxs-lookup"><span data-stu-id="1343e-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="1343e-156">這可讓應用程式的無狀態。</span><span class="sxs-lookup"><span data-stu-id="1343e-156">This makes the app stateless.</span></span> <span data-ttu-id="1343e-157">在每個後續的要求語彙基元被傳入要求的伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="1343e-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="1343e-158">這個語彙基元不*加密*; 它有*編碼*。</span><span class="sxs-lookup"><span data-stu-id="1343e-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="1343e-159">在伺服器上，語彙基元解碼成存取其資訊。</span><span class="sxs-lookup"><span data-stu-id="1343e-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="1343e-160">若要傳送的語彙基元在後續的要求，將儲存在瀏覽器的本機儲存體的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="1343e-161">如果瀏覽器的本機儲存體中儲存的語彙基元，不會擔心 CSRF 弱點。</span><span class="sxs-lookup"><span data-stu-id="1343e-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="1343e-162">CSRF 是考量當權杖儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="1343e-162">CSRF is a concern when the token is stored in a cookie.</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="1343e-163">裝載在一個網域的多個應用程式</span><span class="sxs-lookup"><span data-stu-id="1343e-163">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="1343e-164">共用裝載環境中受到劫持、 登入 CSRF 和其他攻擊。</span><span class="sxs-lookup"><span data-stu-id="1343e-164">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="1343e-165">雖然`example1.contoso.net`和`example2.contoso.net`是不同的主機，在主機之間沒有隱含的信任關係`*.contoso.net`網域。</span><span class="sxs-lookup"><span data-stu-id="1343e-165">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="1343e-166">這個隱含的信任關係可讓可能不受信任的主機會影響彼此的 cookie （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。</span><span class="sxs-lookup"><span data-stu-id="1343e-166">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="1343e-167">不共用網域就可以阻止利用相同的網域上裝載的應用程式之間的信任的 cookie 的攻擊。</span><span class="sxs-lookup"><span data-stu-id="1343e-167">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="1343e-168">當每個應用程式裝載於自己的網域時，就會利用沒有隱含的 cookie 信任關係。</span><span class="sxs-lookup"><span data-stu-id="1343e-168">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="1343e-169">ASP.NET Core antiforgery 組態</span><span class="sxs-lookup"><span data-stu-id="1343e-169">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="1343e-170">ASP.NET Core 實作 antiforgery 使用[ASP.NET Core 資料保護](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="1343e-170">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="1343e-171">資料保護堆疊必須設定為在伺服器陣列中運作。</span><span class="sxs-lookup"><span data-stu-id="1343e-171">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="1343e-172">請參閱[設定資料保護](xref:security/data-protection/configuration/overview)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1343e-172">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="1343e-173">在 ASP.NET Core 2.0 或更新版本， [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)會 antiforgery token 插入至 HTML 表單元素。</span><span class="sxs-lookup"><span data-stu-id="1343e-173">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="1343e-174">Razor 檔案中的下列標記會自動產生 antiforgery 語彙基元：</span><span class="sxs-lookup"><span data-stu-id="1343e-174">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="1343e-175">同樣地， [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)依預設會產生 antiforgery 語彙基元，如果表單的方法不是 GET。</span><span class="sxs-lookup"><span data-stu-id="1343e-175">Similarily, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="1343e-176">自動產生 antiforgery 語彙基元的 HTML 表單元素，會發生情況時`<form>`標記包含`method="post"`屬性和下列任何一個條件成立：</span><span class="sxs-lookup"><span data-stu-id="1343e-176">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

  * <span data-ttu-id="1343e-177">動作屬性是空的 (`action=""`)。</span><span class="sxs-lookup"><span data-stu-id="1343e-177">The action attribute is empty (`action=""`).</span></span>
  * <span data-ttu-id="1343e-178">動作屬性不提供 (`<form method="post">`)。</span><span class="sxs-lookup"><span data-stu-id="1343e-178">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="1343e-179">您可以停用自動產生 antiforgery 語彙基元的 HTML 表單項目：</span><span class="sxs-lookup"><span data-stu-id="1343e-179">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="1343e-180">明確停用 antiforgery token 搭配`asp-antiforgery`屬性：</span><span class="sxs-lookup"><span data-stu-id="1343e-180">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="1343e-181">Form 項目是選擇向外標記協助程式使用標記協助程式[！ 退出符號](xref:mvc/views/tag-helpers/intro#opt-out):</span><span class="sxs-lookup"><span data-stu-id="1343e-181">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="1343e-182">移除`FormTagHelper`從檢視。</span><span class="sxs-lookup"><span data-stu-id="1343e-182">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="1343e-183">`FormTagHelper`可以從檢視移除 Razor 檢視中加入下列指示詞：</span><span class="sxs-lookup"><span data-stu-id="1343e-183">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="1343e-184">[Razor 頁面](xref:razor-pages/index)XSRF/CSRF 會自動受到保護。</span><span class="sxs-lookup"><span data-stu-id="1343e-184">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="1343e-185">如需詳細資訊，請參閱[XSRF/CSRF 和 Razor 頁面](xref:razor-pages/index#xsrf)。</span><span class="sxs-lookup"><span data-stu-id="1343e-185">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="1343e-186">防禦 CSRF 攻擊的常見方法是使用*同步器 Token 模式*(STP)。</span><span class="sxs-lookup"><span data-stu-id="1343e-186">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="1343e-187">當使用者要求網頁，以與表單資料，請使用 STP:</span><span class="sxs-lookup"><span data-stu-id="1343e-187">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="1343e-188">伺服器傳送至用戶端的目前使用者的身分識別相關聯的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-188">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="1343e-189">用戶端上一步將權杖傳送至伺服器以供驗證。</span><span class="sxs-lookup"><span data-stu-id="1343e-189">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="1343e-190">如果伺服器收到不符合已驗證的使用者的身分識別的語彙基元，則會拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="1343e-190">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="1343e-191">權杖的唯一和無法預期。</span><span class="sxs-lookup"><span data-stu-id="1343e-191">The token is unique and unpredictable.</span></span> <span data-ttu-id="1343e-192">語彙基元也可用來確保適當排序的一系列的要求 (例如，確保要求序列的： 第 1 頁&ndash;第 2 頁&ndash;頁面 3)。</span><span class="sxs-lookup"><span data-stu-id="1343e-192">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 &ndash; page 2 &ndash; page 3).</span></span> <span data-ttu-id="1343e-193">所有的表單中 ASP.NET Core MVC 和 Razor 頁面範本產生 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-193">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="1343e-194">下列檢視範例組產生 antiforgery 語彙基元：</span><span class="sxs-lookup"><span data-stu-id="1343e-194">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="1343e-195">明確地加入 antiforgery 語彙基元`<form>`項目，而不使用標記協助程式的 HTML helper [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span><span class="sxs-lookup"><span data-stu-id="1343e-195">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [@Html.AntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="1343e-196">在每一個上述的情況下，ASP.NET Core 加入隱藏的表單欄位，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1343e-196">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="1343e-197">ASP.NET Core 包含三個[篩選](xref:mvc/controllers/filters)使用 antiforgery 語彙基元：</span><span class="sxs-lookup"><span data-stu-id="1343e-197">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="1343e-198">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="1343e-198">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="1343e-199">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="1343e-199">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="1343e-200">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="1343e-200">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="1343e-201">Antiforgery 選項</span><span class="sxs-lookup"><span data-stu-id="1343e-201">Antiforgery options</span></span>

<span data-ttu-id="1343e-202">自訂[antiforgery 選項](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1343e-202">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

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

| <span data-ttu-id="1343e-203">選項</span><span class="sxs-lookup"><span data-stu-id="1343e-203">Option</span></span> | <span data-ttu-id="1343e-204">描述</span><span class="sxs-lookup"><span data-stu-id="1343e-204">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="1343e-205">Cookie</span><span class="sxs-lookup"><span data-stu-id="1343e-205">Cookie</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="1343e-206">決定用來建立 antiforgery cookie 的設定。</span><span class="sxs-lookup"><span data-stu-id="1343e-206">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="1343e-207">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="1343e-207">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | <span data-ttu-id="1343e-208">Cookie 的網域。</span><span class="sxs-lookup"><span data-stu-id="1343e-208">The domain of the cookie.</span></span> <span data-ttu-id="1343e-209">預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="1343e-209">Defaults to `null`.</span></span> <span data-ttu-id="1343e-210">這個屬性已經過時，未來版本將移除。</span><span class="sxs-lookup"><span data-stu-id="1343e-210">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1343e-211">建議的替代做法是 Cookie.Domain。</span><span class="sxs-lookup"><span data-stu-id="1343e-211">The recommended alternative is Cookie.Domain.</span></span> |
| [<span data-ttu-id="1343e-212">CookieName</span><span class="sxs-lookup"><span data-stu-id="1343e-212">CookieName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | <span data-ttu-id="1343e-213">Cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="1343e-213">The name of the cookie.</span></span> <span data-ttu-id="1343e-214">如果未設定，系統會產生唯一的名稱開頭[DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (」。AspNetCore.Antiforgery。")。</span><span class="sxs-lookup"><span data-stu-id="1343e-214">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="1343e-215">這個屬性已經過時，未來版本將移除。</span><span class="sxs-lookup"><span data-stu-id="1343e-215">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1343e-216">建議的替代做法是 Cookie.Name。</span><span class="sxs-lookup"><span data-stu-id="1343e-216">The recommended alternative is Cookie.Name.</span></span> |
| [<span data-ttu-id="1343e-217">CookiePath</span><span class="sxs-lookup"><span data-stu-id="1343e-217">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | <span data-ttu-id="1343e-218">在 cookie 上設定的路徑。</span><span class="sxs-lookup"><span data-stu-id="1343e-218">The path set on the cookie.</span></span> <span data-ttu-id="1343e-219">這個屬性已經過時，未來版本將移除。</span><span class="sxs-lookup"><span data-stu-id="1343e-219">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1343e-220">建議的替代做法是 Cookie.Path。</span><span class="sxs-lookup"><span data-stu-id="1343e-220">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="1343e-221">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="1343e-221">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="1343e-222">Antiforgery 系統用來呈現 antiforgery 語彙基元，在檢視中的隱藏的表單欄位的名稱。</span><span class="sxs-lookup"><span data-stu-id="1343e-222">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="1343e-223">HeaderName</span><span class="sxs-lookup"><span data-stu-id="1343e-223">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="1343e-224">Antiforgery 系統所使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="1343e-224">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="1343e-225">如果`null`，系統會考慮只表單資料。</span><span class="sxs-lookup"><span data-stu-id="1343e-225">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="1343e-226">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="1343e-226">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="1343e-227">指定 antiforgery 系統是否需要 SSL。</span><span class="sxs-lookup"><span data-stu-id="1343e-227">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="1343e-228">如果`true`，非 SSL 要求失敗。</span><span class="sxs-lookup"><span data-stu-id="1343e-228">If `true`, non-SSL requests fail.</span></span> <span data-ttu-id="1343e-229">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="1343e-229">Defaults to `false`.</span></span> <span data-ttu-id="1343e-230">這個屬性已經過時，未來版本將移除。</span><span class="sxs-lookup"><span data-stu-id="1343e-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="1343e-231">建議的替代做法是將設定 Cookie.SecurePolicy。</span><span class="sxs-lookup"><span data-stu-id="1343e-231">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="1343e-232">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="1343e-232">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="1343e-233">指定是否要隱藏產生`X-Frame-Options`標頭。</span><span class="sxs-lookup"><span data-stu-id="1343e-233">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="1343e-234">根據預設，會產生標頭，其值為"Sameorigin 所"。</span><span class="sxs-lookup"><span data-stu-id="1343e-234">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="1343e-235">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="1343e-235">Defaults to `false`.</span></span> |

<span data-ttu-id="1343e-236">如需詳細資訊，請參閱[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。</span><span class="sxs-lookup"><span data-stu-id="1343e-236">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="1343e-237">使用 IAntiforgery 設定 antiforgery 功能</span><span class="sxs-lookup"><span data-stu-id="1343e-237">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="1343e-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)提供 API 來設定 antiforgery 功能。</span><span class="sxs-lookup"><span data-stu-id="1343e-238">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="1343e-239">`IAntiforgery` 可以要求在`Configure`方法`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="1343e-239">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="1343e-240">下列範例會使用產生 antiforgery 的語彙基元，並在回應中傳送為 （使用預設角度命名慣例在本主題稍後所述） cookie 中介軟體應用程式的首頁上：</span><span class="sxs-lookup"><span data-stu-id="1343e-240">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

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

### <a name="require-antiforgery-validation"></a><span data-ttu-id="1343e-241">需要 antiforgery 驗證</span><span class="sxs-lookup"><span data-stu-id="1343e-241">Require antiforgery validation</span></span>

<span data-ttu-id="1343e-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)是動作篩選條件可以套用到個別的動作控制站，或全域。</span><span class="sxs-lookup"><span data-stu-id="1343e-242">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="1343e-243">套用此篩選條件的動作提出的要求會遭到封鎖，除非要求包含有效的 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-243">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

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

<span data-ttu-id="1343e-244">`ValidateAntiForgeryToken`屬性裝飾，包括 HTTP GET 要求的動作方法要求需要 token。</span><span class="sxs-lookup"><span data-stu-id="1343e-244">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it decorates, including HTTP GET requests.</span></span> <span data-ttu-id="1343e-245">如果`ValidateAntiForgeryToken`屬性套用跨應用程式的控制站，可以使用覆寫`IgnoreAntiforgeryToken`屬性。</span><span class="sxs-lookup"><span data-stu-id="1343e-245">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="1343e-246">ASP.NET Core 不支援 GET 要求中自動加入 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-246">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="1343e-247">Antiforgery 的語彙基元的不安全的 HTTP 方法只會自動進行驗證</span><span class="sxs-lookup"><span data-stu-id="1343e-247">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="1343e-248">ASP.NET Core 應用程式不會產生 antiforgery 權杖之安全的 HTTP 方法 （GET、 HEAD、 選項和追蹤）。</span><span class="sxs-lookup"><span data-stu-id="1343e-248">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="1343e-249">而不是廣泛套用`ValidateAntiForgeryToken`屬性，然後將它與覆寫`IgnoreAntiforgeryToken`屬性[AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)屬性可以使用。</span><span class="sxs-lookup"><span data-stu-id="1343e-249">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="1343e-250">這個屬性的運作方式和`ValidateAntiForgeryToken`屬性，不同之處在於它不需要使用下列的 HTTP 方法所提出之要求的語彙基元：</span><span class="sxs-lookup"><span data-stu-id="1343e-250">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="1343e-251">GET</span><span class="sxs-lookup"><span data-stu-id="1343e-251">GET</span></span>
* <span data-ttu-id="1343e-252">HEAD</span><span class="sxs-lookup"><span data-stu-id="1343e-252">HEAD</span></span>
* <span data-ttu-id="1343e-253">選項</span><span class="sxs-lookup"><span data-stu-id="1343e-253">OPTIONS</span></span>
* <span data-ttu-id="1343e-254">TRACE</span><span class="sxs-lookup"><span data-stu-id="1343e-254">TRACE</span></span>

<span data-ttu-id="1343e-255">我們建議使用`AutoValidateAntiforgeryToken`廣泛的非 API 」 案例。</span><span class="sxs-lookup"><span data-stu-id="1343e-255">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="1343e-256">這可確保依預設受到保護後動作。</span><span class="sxs-lookup"><span data-stu-id="1343e-256">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="1343e-257">替代方案是根據預設，忽略 antiforgery 語彙基元，除非`ValidateAntiForgeryToken`套用至個別動作方法。</span><span class="sxs-lookup"><span data-stu-id="1343e-257">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="1343e-258">它比較可能在此案例中剩餘的 POST 動作方法的受保護的錯誤，讓應用程式受到 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="1343e-258">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="1343e-259">所有文章應該都傳送 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-259">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="1343e-260">應用程式開發介面不需要傳送非 cookie 權杖的一部分的自動機制。</span><span class="sxs-lookup"><span data-stu-id="1343e-260">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="1343e-261">實作可能取決於用戶端程式碼實作。</span><span class="sxs-lookup"><span data-stu-id="1343e-261">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="1343e-262">某些範例如下所示：</span><span class="sxs-lookup"><span data-stu-id="1343e-262">Some examples are shown below:</span></span>

<span data-ttu-id="1343e-263">類別層級的範例：</span><span class="sxs-lookup"><span data-stu-id="1343e-263">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="1343e-264">全域範例：</span><span class="sxs-lookup"><span data-stu-id="1343e-264">Global example:</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="1343e-265">覆寫全域或控制器 antiforgery 屬性</span><span class="sxs-lookup"><span data-stu-id="1343e-265">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="1343e-266">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)篩選條件可用於不需要指定的 「 動作 」 （或稱 「 控制器 」） antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-266">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="1343e-267">套用時，此篩選條件會覆寫`ValidateAntiForgeryToken`和`AutoValidateAntiforgeryToken`（全域或控制站上），在較高層級指定的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="1343e-267">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

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

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="1343e-268">在驗證之後，重新整理權杖</span><span class="sxs-lookup"><span data-stu-id="1343e-268">Refresh tokens after authentication</span></span>

<span data-ttu-id="1343e-269">將使用者重新導向至檢視或 Razor 頁面驗證使用者之後，應該重新整理語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-269">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="1343e-270">JavaScript、 AJAX 和 SPAs</span><span class="sxs-lookup"><span data-stu-id="1343e-270">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="1343e-271">在傳統的 HTML 型應用程式，antiforgery 權杖會傳遞至使用隱藏的表單欄位的伺服器。</span><span class="sxs-lookup"><span data-stu-id="1343e-271">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="1343e-272">在現代 JavaScript 為基礎的應用程式和 SPAs，許多要求以程式設計的方式。</span><span class="sxs-lookup"><span data-stu-id="1343e-272">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="1343e-273">這些 AJAX 要求可以使用其他技術 （例如要求標頭或 cookie） 傳送語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-273">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="1343e-274">Cookie 用來儲存驗證權杖，並驗證伺服器上的應用程式開發介面要求，CSRF 有潛在的問題。</span><span class="sxs-lookup"><span data-stu-id="1343e-274">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="1343e-275">如果本機儲存體用來儲存的語彙基元，CSRF 的弱點可能會可能會降低，因為從本機儲存體的值不會自動傳送到每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="1343e-275">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="1343e-276">因此，使用本機儲存體來儲存 antiforgery 語彙基元，在用戶端傳送權杖，因為要求標頭是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="1343e-276">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="1343e-277">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1343e-277">JavaScript</span></span>

<span data-ttu-id="1343e-278">檢視與使用 JavaScript，權杖可以建立使用從檢視內的服務。</span><span class="sxs-lookup"><span data-stu-id="1343e-278">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="1343e-279">插入[Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)到檢視並呼叫服務[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span><span class="sxs-lookup"><span data-stu-id="1343e-279">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="1343e-280">這個方法不需要直接處理從伺服器中設定 cookie，或從用戶端讀取。</span><span class="sxs-lookup"><span data-stu-id="1343e-280">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="1343e-281">上述範例使用 JavaScript AJAX 張貼標頭讀取隱藏的欄位值。</span><span class="sxs-lookup"><span data-stu-id="1343e-281">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="1343e-282">JavaScript 也可以存取 cookie 中的語彙基元，並使用 cookie 的內容建立的語彙基元值的標頭。</span><span class="sxs-lookup"><span data-stu-id="1343e-282">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="1343e-283">假設指令碼中呼叫的標頭傳送權杖要求`X-CSRF-TOKEN`，antiforgery 服務設定為尋找`X-CSRF-TOKEN`標頭：</span><span class="sxs-lookup"><span data-stu-id="1343e-283">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="1343e-284">下列範例使用 JavaScript，請使用適當的標頭 AJAX 要求：</span><span class="sxs-lookup"><span data-stu-id="1343e-284">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

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

### <a name="angularjs"></a><span data-ttu-id="1343e-285">AngularJS</span><span class="sxs-lookup"><span data-stu-id="1343e-285">AngularJS</span></span>

<span data-ttu-id="1343e-286">AngularJS 會到位址 CSRF 慣例。</span><span class="sxs-lookup"><span data-stu-id="1343e-286">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="1343e-287">如果伺服器傳送的 cookie 名稱`XSRF-TOKEN`，AngularJS`$http`服務將 cookie 的值加入標頭將要求傳送到伺服器時。</span><span class="sxs-lookup"><span data-stu-id="1343e-287">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="1343e-288">此程序是自動的。</span><span class="sxs-lookup"><span data-stu-id="1343e-288">This process is automatic.</span></span> <span data-ttu-id="1343e-289">標頭，不需要明確設定。</span><span class="sxs-lookup"><span data-stu-id="1343e-289">The header doesn't need to be set explicitly.</span></span> <span data-ttu-id="1343e-290">標頭名稱`X-XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="1343e-290">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="1343e-291">伺服器應該偵測此標頭，並驗證其內容。</span><span class="sxs-lookup"><span data-stu-id="1343e-291">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="1343e-292">適用於 ASP.NET Core 應用程式開發介面使用這個慣例：</span><span class="sxs-lookup"><span data-stu-id="1343e-292">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="1343e-293">設定您的應用程式提供語彙基元在呼叫 cookie `XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="1343e-293">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="1343e-294">設定 antiforgery 服務，以尋找名為標頭`X-XSRF-TOKEN`。</span><span class="sxs-lookup"><span data-stu-id="1343e-294">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="1343e-295">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1343e-295">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="1343e-296">擴充 antiforgery</span><span class="sxs-lookup"><span data-stu-id="1343e-296">Extend antiforgery</span></span>

<span data-ttu-id="1343e-297">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員以便在往返過程中每個語彙基元的其他資料來擴充反 CSRF 系統的行為。</span><span class="sxs-lookup"><span data-stu-id="1343e-297">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="1343e-298">[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)每次呼叫方法會產生欄位語彙基元，並傳回值內嵌在產生的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="1343e-298">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="1343e-299">實作者可能傳回的時間戳記、 nonce 或任何其他值，然後呼叫[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)驗證語彙基元時驗證這項資料。</span><span class="sxs-lookup"><span data-stu-id="1343e-299">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="1343e-300">用戶端的使用者名稱已內嵌在產生的語彙基元，因此不需要加入這項資訊。</span><span class="sxs-lookup"><span data-stu-id="1343e-300">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="1343e-301">如果語彙基元包含補充資料但不是`IAntiForgeryAdditionalDataProvider`是設定，未驗證的補充資料。</span><span class="sxs-lookup"><span data-stu-id="1343e-301">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1343e-302">其他資源</span><span class="sxs-lookup"><span data-stu-id="1343e-302">Additional resources</span></span>

* <span data-ttu-id="1343e-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)(OWASP)。</span><span class="sxs-lookup"><span data-stu-id="1343e-303">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
