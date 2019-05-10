---
title: 防止 ASP.NET Core 中的開啟重新導向攻擊
author: ardalis
description: 示範如何防止開啟重新導向攻擊將 ASP.NET Core 應用程式
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891615"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="8a3e0-103">防止 ASP.NET Core 中的開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="8a3e0-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="8a3e0-104">重新導向至指定透過例如查詢字串或表單資料要求 URL 的 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="8a3e0-105">這種竄改稱為開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="8a3e0-106">每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL 未遭竄改。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="8a3e0-107">ASP.NET Core 具有內建的功能，以協助防止開啟重新導向 （也就是開啟重新導向） 攻擊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="8a3e0-108">什麼是開啟重新導向攻擊？</span><span class="sxs-lookup"><span data-stu-id="8a3e0-108">What is an open redirect attack?</span></span>

<span data-ttu-id="8a3e0-109">Web 應用程式經常將使用者重新導向至登入頁面存取需要驗證的資源時。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="8a3e0-110">重新導向通常包含`returnUrl`querystring 參數，讓使用者可以傳回給原始要求的 URL 之後使用者已成功登入。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="8a3e0-111">使用者驗證之後，它們會被重新導向至其原先要求的 URL。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="8a3e0-112">因為要求的 querystring 中指定的目的地 URL，則惡意使用者無法修改查詢字串。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="8a3e0-113">遭竄改的查詢字串可能會允許將使用者重新導向至外部，惡意網站的站台。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="8a3e0-114">這項技術稱為開啟重新導向 （或重新導向） 攻擊。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="8a3e0-115">範例攻擊</span><span class="sxs-lookup"><span data-stu-id="8a3e0-115">An example attack</span></span>

<span data-ttu-id="8a3e0-116">惡意使用者可以開發目的是讓惡意使用者的存取權的使用者認證或機密資訊的攻擊。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="8a3e0-117">若要開始的攻擊，惡意使用者說服使用者按一下與您的網站登入頁面的連結`returnUrl`新增至 URL 的查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="8a3e0-118">例如，請考慮在應用程式`contoso.com`，其中包含登入頁面`http://contoso.com/Account/LogOn?returnUrl=/Home/About`。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="8a3e0-119">攻擊會遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="8a3e0-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="8a3e0-120">使用者按下惡意連結`http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn`(第二個 URL 是"contoso**1**.com"，不是"contoso.com")。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="8a3e0-121">使用者成功登入。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="8a3e0-122">將使用者重新導向 （由站台） `http://contoso1.com/Account/LogOn` （看起來像真正的站台完全惡意網站）。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="8a3e0-123">在使用者登入一次 （讓惡意網站他們的認證） 並被重新導向到真網站。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="8a3e0-124">使用者可能會認為他們第一次嘗試登入失敗，且其第二次嘗試成功。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="8a3e0-125">使用者很可能並不會知道他們的認證遭到入侵。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![開啟重新導向攻擊程序](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="8a3e0-127">除了登入頁面中，有些網站會提供重新導向頁面或端點。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="8a3e0-128">假設您的應用程式有開啟的重新導向的頁面`/Home/Redirect`。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="8a3e0-129">攻擊者建立，比方說，連至電子郵件中的連結`[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="8a3e0-130">一般使用者將看看 URL，並看看它開始與您的網站名稱。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="8a3e0-131">信任的會讓他們按一下連結。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="8a3e0-132">開啟重新導向至網路釣魚網站，看起來與您相同，然後會傳送使用者和使用者可能登入他們認為是您的網站。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="8a3e0-133">防止開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="8a3e0-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="8a3e0-134">在開發 web 應用程式時，將使用者提供的所有資料視為不受信任。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="8a3e0-135">如果您的應用程式 URL 的內容為基礎的使用者重新導向的功能，請確定，這類的重新導向是只在本機完成應用程式內 （或已知的 URL，不含任何可能會在查詢字串中提供的 URL）。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="8a3e0-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="8a3e0-136">LocalRedirect</span></span>

<span data-ttu-id="8a3e0-137">使用`LocalRedirect`自基底的 helper 方法`Controller`類別：</span><span class="sxs-lookup"><span data-stu-id="8a3e0-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="8a3e0-138">`LocalRedirect` 如果指定非本機 URL，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="8a3e0-139">否則，它的行為就像`Redirect`方法。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="8a3e0-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="8a3e0-140">IsLocalUrl</span></span>

<span data-ttu-id="8a3e0-141">使用[IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)之前，先測試 Url 重新導向的方法：</span><span class="sxs-lookup"><span data-stu-id="8a3e0-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="8a3e0-142">下列範例示範如何檢查 URL 是否本機，再重新導向。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="8a3e0-143">`IsLocalUrl`方法可防止使用者被不小心重新導向至惡意網站。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="8a3e0-144">您可以記錄的情況下，您必須是本機的 URL 提供給非本機 URL 時所提供的 URL 的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="8a3e0-145">記錄重新導向 Url 可能有助於診斷重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8a3e0-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
