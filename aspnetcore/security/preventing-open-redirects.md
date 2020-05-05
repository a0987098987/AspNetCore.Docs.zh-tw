---
title: 防止 ASP.NET Core 中的開啟重新導向攻擊
author: ardalis
description: 示範如何防止對 ASP.NET Core 應用程式的開啟重新導向攻擊
ms.author: riande
ms.date: 07/07/2017
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/preventing-open-redirects
ms.openlocfilehash: ad4c9806146567b6ef1f5e78eaeca96cb649c1af
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82774388"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="0b99d-103">防止 ASP.NET Core 中的開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="0b99d-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="0b99d-104">重新導向至透過要求（例如 querystring 或表單資料）所指定 URL 的 web 應用程式可能會遭到篡改，以將使用者重新導向至外部的惡意 URL。</span><span class="sxs-lookup"><span data-stu-id="0b99d-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="0b99d-105">這種篡改稱為開放式重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="0b99d-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="0b99d-106">每當您的應用程式邏輯重新導向至指定的 URL 時，您都必須確認重新導向 URL 未遭到篡改。</span><span class="sxs-lookup"><span data-stu-id="0b99d-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="0b99d-107">ASP.NET Core 具有內建功能，可協助保護應用程式不受開啟的重新導向（也稱為開放式重新導向）攻擊。</span><span class="sxs-lookup"><span data-stu-id="0b99d-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="0b99d-108">什麼是開啟的重新導向攻擊？</span><span class="sxs-lookup"><span data-stu-id="0b99d-108">What is an open redirect attack?</span></span>

<span data-ttu-id="0b99d-109">Web 應用程式通常會在使用者存取需要驗證的資源時，將他們重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0b99d-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="0b99d-110">重新導向通常會包含`returnUrl` querystring 參數，讓使用者可以在成功登入之後，傳回給原始要求的 URL。</span><span class="sxs-lookup"><span data-stu-id="0b99d-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="0b99d-111">使用者驗證之後，系統會將他們重新導向至原先要求的 URL。</span><span class="sxs-lookup"><span data-stu-id="0b99d-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="0b99d-112">因為在要求的 querystring 中指定了目的地 URL，所以惡意使用者可能會篡改 querystring。</span><span class="sxs-lookup"><span data-stu-id="0b99d-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="0b99d-113">遭篡改的 querystring 可能會允許網站將使用者重新導向至外部的惡意網站。</span><span class="sxs-lookup"><span data-stu-id="0b99d-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="0b99d-114">這項技術稱為「開啟重新導向」（或「重新導向」）攻擊。</span><span class="sxs-lookup"><span data-stu-id="0b99d-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="0b99d-115">範例攻擊</span><span class="sxs-lookup"><span data-stu-id="0b99d-115">An example attack</span></span>

<span data-ttu-id="0b99d-116">惡意使用者可能會開發攻擊，以允許惡意使用者存取使用者的認證或機密資訊。</span><span class="sxs-lookup"><span data-stu-id="0b99d-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="0b99d-117">若要開始攻擊，惡意使用者說服使用者按一下您網站登入頁面的連結，並將`returnUrl` querystring 值新增至 URL。</span><span class="sxs-lookup"><span data-stu-id="0b99d-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="0b99d-118">例如，請考慮中`contoso.com`的應用程式，其中包含的登`http://contoso.com/Account/LogOn?returnUrl=/Home/About`入頁面。</span><span class="sxs-lookup"><span data-stu-id="0b99d-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="0b99d-119">攻擊會遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0b99d-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="0b99d-120">使用者按一下的惡意連結`http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` （第二個 URL 是 "contoso**1**.com"，而不是 "contoso.com"）。</span><span class="sxs-lookup"><span data-stu-id="0b99d-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="0b99d-121">使用者登入成功。</span><span class="sxs-lookup"><span data-stu-id="0b99d-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="0b99d-122">使用者會被重新導向（由網站）到`http://contoso1.com/Account/LogOn` （看起來與實際網站完全類似的惡意網站）。</span><span class="sxs-lookup"><span data-stu-id="0b99d-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="0b99d-123">使用者再次登入（提供惡意網站的認證），然後重新導向至實際網站。</span><span class="sxs-lookup"><span data-stu-id="0b99d-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="0b99d-124">使用者可能認為第一次嘗試登入失敗，而第二次嘗試成功。</span><span class="sxs-lookup"><span data-stu-id="0b99d-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="0b99d-125">使用者很可能會不知道他們的認證會受到危害。</span><span class="sxs-lookup"><span data-stu-id="0b99d-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![開啟重新導向攻擊程式](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="0b99d-127">除了登入頁面以外，有些網站會提供重新導向頁面或端點。</span><span class="sxs-lookup"><span data-stu-id="0b99d-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="0b99d-128">假設您的應用程式有一個頁面， `/Home/Redirect`其中包含開啟的重新導向。</span><span class="sxs-lookup"><span data-stu-id="0b99d-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="0b99d-129">例如，攻擊者可能會建立電子郵件中的連結，以前往`[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`。</span><span class="sxs-lookup"><span data-stu-id="0b99d-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="0b99d-130">一般使用者會查看 URL，並從您的網站名稱開始查看。</span><span class="sxs-lookup"><span data-stu-id="0b99d-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="0b99d-131">信任它，他們會按一下連結。</span><span class="sxs-lookup"><span data-stu-id="0b99d-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="0b99d-132">然後，開啟的重新導向會將使用者傳送至網路釣魚網站，看起來與您的內容相同，而且使用者可能會登入他們認為您的網站。</span><span class="sxs-lookup"><span data-stu-id="0b99d-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="0b99d-133">防止開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="0b99d-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="0b99d-134">開發 web 應用程式時，將所有使用者提供的資料視為不受信任。</span><span class="sxs-lookup"><span data-stu-id="0b99d-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="0b99d-135">如果您的應用程式具有可根據 URL 內容重新導向使用者的功能，請確定這類重新導向只會在您的應用程式中本機完成（或至已知的 URL，而不是查詢字串中可能提供的任何 URL）。</span><span class="sxs-lookup"><span data-stu-id="0b99d-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="0b99d-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="0b99d-136">LocalRedirect</span></span>

<span data-ttu-id="0b99d-137">使用來自`LocalRedirect`基類`Controller`的 helper 方法：</span><span class="sxs-lookup"><span data-stu-id="0b99d-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="0b99d-138">`LocalRedirect`如果指定非本機 URL，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0b99d-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="0b99d-139">否則，它的`Redirect`行為就像方法一樣。</span><span class="sxs-lookup"><span data-stu-id="0b99d-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="0b99d-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="0b99d-140">IsLocalUrl</span></span>

<span data-ttu-id="0b99d-141">重新導向之前，請使用[IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)方法來測試 url：</span><span class="sxs-lookup"><span data-stu-id="0b99d-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="0b99d-142">下列範例顯示如何在重新導向之前檢查 URL 是否為本機。</span><span class="sxs-lookup"><span data-stu-id="0b99d-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="0b99d-143">`IsLocalUrl`方法可保護使用者不小心重新導向至惡意網站。</span><span class="sxs-lookup"><span data-stu-id="0b99d-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="0b99d-144">當您需要本機 URL 的情況下提供非本機 URL 時，您可以記錄所提供的 URL 詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0b99d-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="0b99d-145">記錄重新導向 Url 可能有助於診斷重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="0b99d-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
