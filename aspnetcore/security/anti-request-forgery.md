---
title: "防止跨網站要求偽造 (XSRF/CSRF) 攻擊中 ASP.NET Core"
author: steve-smith
ms.author: riande
description: "防止跨網站要求偽造 (XSRF/CSRF) 攻擊中 ASP.NET Core"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: 3831bf737186d10eb1b298f5ec2da1fd33ebedd9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="76774-103">防止跨網站要求偽造 (XSRF/CSRF) 攻擊中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76774-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="76774-104">[Steve Smith](https://ardalis.com/)， [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="76774-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="76774-105">防偽防止何種攻擊？</span><span class="sxs-lookup"><span data-stu-id="76774-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="76774-106">跨站台要求偽造 (也稱為 XSRF 或 CSRF，唸成*，請參閱衝浪*) 是 web 裝載的應用程式讓惡意網站可能會影響用戶端瀏覽器和信任的網站之間的互動攻擊該瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="76774-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="76774-107">這些攻擊都可能因為 web 瀏覽器傳送到網站的某些類型的驗證權杖會自動與每個要求。</span><span class="sxs-lookup"><span data-stu-id="76774-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="76774-108">這種形式的攻擊也稱為的*單鍵攻擊*或*工作階段乘載*，因為使用者的攻擊會利用先前的驗證工作階段。</span><span class="sxs-lookup"><span data-stu-id="76774-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="76774-109">CSRF 攻擊的範例：</span><span class="sxs-lookup"><span data-stu-id="76774-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="76774-110">使用者登入`www.example.com`，使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="76774-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="76774-111">伺服器會驗證使用者，並會發出包含驗證 cookie 的回應。</span><span class="sxs-lookup"><span data-stu-id="76774-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="76774-112">使用者造訪惡意網站。</span><span class="sxs-lookup"><span data-stu-id="76774-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="76774-113">惡意的站台包含 HTML 表單如下所示：</span><span class="sxs-lookup"><span data-stu-id="76774-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="76774-114">請注意，張貼至容易遭受站台，惡意網站的表單動作。</span><span class="sxs-lookup"><span data-stu-id="76774-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="76774-115">這是 CSRF 的 「 跨網站 」 部分。</span><span class="sxs-lookup"><span data-stu-id="76774-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="76774-116">使用者按一下 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="76774-116">The user clicks the submit button.</span></span> <span data-ttu-id="76774-117">瀏覽器會自動包含所要求的網域 （在此情況下很容易遭受站台） 要求的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="76774-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="76774-118">要求使用者驗證內容與在伺服器上執行，並可以執行已驗證的使用者允許進行的任何動作。</span><span class="sxs-lookup"><span data-stu-id="76774-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="76774-119">這個範例會要求使用者可以按一下 [表單] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="76774-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="76774-120">惡意的頁面可以：</span><span class="sxs-lookup"><span data-stu-id="76774-120">The malicious page could:</span></span>

* <span data-ttu-id="76774-121">執行自動送出表單的指令碼。</span><span class="sxs-lookup"><span data-stu-id="76774-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="76774-122">以 AJAX 要求中傳送的表單提交。</span><span class="sxs-lookup"><span data-stu-id="76774-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="76774-123">Css 使用隱藏的表單。</span><span class="sxs-lookup"><span data-stu-id="76774-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="76774-124">使用 SSL 不會防止 CSRF 攻擊，惡意的站台可以傳送`https://`要求。</span><span class="sxs-lookup"><span data-stu-id="76774-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="76774-125">部分攻擊目標站台端點回應`GET`要求，在其中案例之影像標記可以用來執行 （這種攻擊是常見的允許映像，但會封鎖 JavaScript 論壇網站） 的動作。</span><span class="sxs-lookup"><span data-stu-id="76774-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="76774-126">變更狀態時，使用的應用程式`GET`要求而受到損害受到惡意的攻擊。</span><span class="sxs-lookup"><span data-stu-id="76774-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="76774-127">CSRF 攻擊可能會進行對網站使用 cookie 進行驗證，因為瀏覽器傳送到目標網站的所有相關的 cookie。</span><span class="sxs-lookup"><span data-stu-id="76774-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="76774-128">不過，不限於利用 cookie CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="76774-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="76774-129">比方說，也是很容易遭受基本和摘要式驗證。</span><span class="sxs-lookup"><span data-stu-id="76774-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="76774-130">在使用者登入基本或摘要式驗證後，瀏覽器將會自動傳送認證到工作階段結束為止。</span><span class="sxs-lookup"><span data-stu-id="76774-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="76774-131">注意： 在此情況下，*工作階段*指的是用戶端工作階段期間會驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="76774-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="76774-132">它是不相關的伺服器端工作階段或[工作階段中介軟體](xref:fundamentals/app-state)。</span><span class="sxs-lookup"><span data-stu-id="76774-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="76774-133">使用者可以防範 CSRF 缺失：</span><span class="sxs-lookup"><span data-stu-id="76774-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="76774-134">記錄遠離網站時使用它們完成。</span><span class="sxs-lookup"><span data-stu-id="76774-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="76774-135">定期清除其瀏覽器的 cookie。</span><span class="sxs-lookup"><span data-stu-id="76774-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="76774-136">不過，CSRF 弱點基本上都是使用 web 應用程式，終端使用者的問題。</span><span class="sxs-lookup"><span data-stu-id="76774-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="76774-137">ASP.NET Core MVC 如何解決 CSRF？</span><span class="sxs-lookup"><span data-stu-id="76774-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="76774-138">ASP.NET Core 實作反 request 偽造使用[ASP.NET Core 資料保護堆疊](xref:security/data-protection/introduction)。</span><span class="sxs-lookup"><span data-stu-id="76774-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="76774-139">ASP.NET Core 資料保護必須設定為在伺服器陣列中運作。</span><span class="sxs-lookup"><span data-stu-id="76774-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="76774-140">請參閱[設定資料保護](xref:security/data-protection/configuration/overview)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="76774-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="76774-141">ASP.NET Core 反 request 偽造預設資料保護設定</span><span class="sxs-lookup"><span data-stu-id="76774-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="76774-142">在 ASP.NET Core MVC 2.0 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)插入防偽語彙基元的 HTML 表單元素。</span><span class="sxs-lookup"><span data-stu-id="76774-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="76774-143">例如，下列標記 Razor 檔案中的將會自動產生防偽語彙基元：</span><span class="sxs-lookup"><span data-stu-id="76774-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="76774-144">自動產生的防偽語彙基元的 HTML 表單元素，會發生情況時：</span><span class="sxs-lookup"><span data-stu-id="76774-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="76774-145">`form`標記包含`method="post"`屬性 AND</span><span class="sxs-lookup"><span data-stu-id="76774-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="76774-146">動作屬性是空的。</span><span class="sxs-lookup"><span data-stu-id="76774-146">The action attribute is empty.</span></span> <span data-ttu-id="76774-147">( `action=""`) 或</span><span class="sxs-lookup"><span data-stu-id="76774-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="76774-148">未提供的 action 屬性。</span><span class="sxs-lookup"><span data-stu-id="76774-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="76774-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="76774-149">(`<form method="post">`)</span></span>

<span data-ttu-id="76774-150">您可以停用自動產生的防偽語彙基元的 HTML 表單項目：</span><span class="sxs-lookup"><span data-stu-id="76774-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="76774-151">明確停用`asp-antiforgery`。</span><span class="sxs-lookup"><span data-stu-id="76774-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="76774-152">例如</span><span class="sxs-lookup"><span data-stu-id="76774-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="76774-153">使用標記協助程式選擇超出標記協助程式的表單項目[！ 退出符號](xref:mvc/views/tag-helpers/intro#opt-out)。</span><span class="sxs-lookup"><span data-stu-id="76774-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="76774-154">移除`FormTagHelper`從檢視。</span><span class="sxs-lookup"><span data-stu-id="76774-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="76774-155">您可以移除`FormTagHelper`從下列指示詞加入 Razor 檢視的檢視：</span><span class="sxs-lookup"><span data-stu-id="76774-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="76774-156">[Razor 頁面](xref:mvc/razor-pages/index)XSRF/CSRF 會自動受到保護。</span><span class="sxs-lookup"><span data-stu-id="76774-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="76774-157">您不需要撰寫任何額外的程式碼。</span><span class="sxs-lookup"><span data-stu-id="76774-157">You don't have to write any additional code.</span></span> <span data-ttu-id="76774-158">請參閱[XSRF/CSRF 和 Razor 頁面](xref:mvc/razor-pages/index#xsrf)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="76774-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="76774-159">防禦 CSRF 攻擊的常見方法是同步器 token 模式 (STP)。</span><span class="sxs-lookup"><span data-stu-id="76774-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="76774-160">STP 是使用者要求表單資料的頁面時使用的技術。</span><span class="sxs-lookup"><span data-stu-id="76774-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="76774-161">伺服器傳送至用戶端的目前使用者的身分識別相關聯的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="76774-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="76774-162">用戶端上一步將權杖傳送至伺服器以供驗證。</span><span class="sxs-lookup"><span data-stu-id="76774-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="76774-163">如果伺服器收到不符合已驗證的使用者的身分識別的語彙基元，則會拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="76774-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="76774-164">權杖的唯一和無法預期。</span><span class="sxs-lookup"><span data-stu-id="76774-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="76774-165">語彙基元也可用以確保適當排序的一系列的要求 （確保第 1 頁位於第 2 頁位於第 3 頁）。</span><span class="sxs-lookup"><span data-stu-id="76774-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="76774-166">ASP.NET Core MVC 範本中的所有表單都產生 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="76774-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="76774-167">檢視邏輯下列兩個範例會產生 antiforgery 語彙基元：</span><span class="sxs-lookup"><span data-stu-id="76774-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="76774-168">您可以明確加入 antiforgery 語彙基元``<form>``項目，而不使用標記協助程式的 HTML helper ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="76774-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="76774-169">在每一個上述的情況下，ASP.NET Core 將會加入隱藏的表單欄位，如下所示：</span><span class="sxs-lookup"><span data-stu-id="76774-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="76774-170">ASP.NET Core 包含三個[篩選](xref:mvc/controllers/filters)使用 antiforgery 語彙基元： ``ValidateAntiForgeryToken``， ``AutoValidateAntiforgeryToken``，和``IgnoreAntiforgeryToken``。</span><span class="sxs-lookup"><span data-stu-id="76774-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="76774-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="76774-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="76774-172">``ValidateAntiForgeryToken``是動作篩選條件可以套用到個別的動作控制站，或全域。</span><span class="sxs-lookup"><span data-stu-id="76774-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="76774-173">除非要求包含有效的 antiforgery 語彙基元，將會封鎖套用此篩選條件的動作提出的要求。</span><span class="sxs-lookup"><span data-stu-id="76774-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="76774-174">``ValidateAntiForgeryToken``屬性需要裝飾，包括要求至動作方法的語彙基元`HTTP GET`要求。</span><span class="sxs-lookup"><span data-stu-id="76774-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="76774-175">如果您將它套用廣泛，您可以覆寫它與``IgnoreAntiforgeryToken``屬性。</span><span class="sxs-lookup"><span data-stu-id="76774-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="76774-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="76774-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="76774-177">ASP.NET Core 應用程式通常不產生 antiforgery 的語彙基元的安全 HTTP 方法 （GET、 HEAD、 選項和追蹤）。</span><span class="sxs-lookup"><span data-stu-id="76774-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="76774-178">而不是廣泛套用``ValidateAntiForgeryToken``屬性，然後將它與覆寫``IgnoreAntiforgeryToken``屬性，您可以使用``AutoValidateAntiforgeryToken``屬性。</span><span class="sxs-lookup"><span data-stu-id="76774-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="76774-179">這個屬性的運作方式和``ValidateAntiForgeryToken``屬性，不同之處在於它不需要使用下列的 HTTP 方法所提出之要求的語彙基元：</span><span class="sxs-lookup"><span data-stu-id="76774-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="76774-180">GET</span><span class="sxs-lookup"><span data-stu-id="76774-180">GET</span></span>
* <span data-ttu-id="76774-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="76774-181">HEAD</span></span>
* <span data-ttu-id="76774-182">選項</span><span class="sxs-lookup"><span data-stu-id="76774-182">OPTIONS</span></span>
* <span data-ttu-id="76774-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="76774-183">TRACE</span></span>

<span data-ttu-id="76774-184">我們建議您改用``AutoValidateAntiforgeryToken``廣泛的非 API 」 案例。</span><span class="sxs-lookup"><span data-stu-id="76774-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="76774-185">這可確保您張貼的動作依預設受到保護。</span><span class="sxs-lookup"><span data-stu-id="76774-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="76774-186">替代方案是根據預設，忽略 antiforgery 語彙基元，除非``ValidateAntiForgeryToken``套用至個別動作方法。</span><span class="sxs-lookup"><span data-stu-id="76774-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="76774-187">很可能在此案例中將 POST 動作方法的左未受保護，讓您的應用程式受到 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="76774-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="76774-188">即使匿名文章應該傳送 antiforgery 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="76774-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="76774-189">注意： 應用程式開發介面沒有自動化的機制來傳送非 cookie 權杖的一部分。您的實作可能取決於您的用戶端程式碼實作。</span><span class="sxs-lookup"><span data-stu-id="76774-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="76774-190">某些範例如下所示。</span><span class="sxs-lookup"><span data-stu-id="76774-190">Some examples are shown below.</span></span>


<span data-ttu-id="76774-191">範例 （類別層級）：</span><span class="sxs-lookup"><span data-stu-id="76774-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="76774-192">範例 （全域）：</span><span class="sxs-lookup"><span data-stu-id="76774-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="76774-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="76774-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="76774-194">``IgnoreAntiforgeryToken``篩選器可用來消除 antiforgery 的權杖，才能在指定的 「 動作 」 （或稱 「 控制器 」） 的需要。</span><span class="sxs-lookup"><span data-stu-id="76774-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="76774-195">此篩選器套用時，將會覆寫``ValidateAntiForgeryToken``及/或``AutoValidateAntiforgeryToken``（全域或控制站上），在較高層級指定的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="76774-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="76774-196">JavaScript、 AJAX 和 SPAs</span><span class="sxs-lookup"><span data-stu-id="76774-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="76774-197">傳統的 HTML 型應用程式，antiforgery 權杖會傳遞至使用隱藏的表單欄位的伺服器。</span><span class="sxs-lookup"><span data-stu-id="76774-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="76774-198">在現代 JavaScript 為基礎的應用程式和單一頁面應用程式 (SPAs) 中，許多要求以程式設計的方式。</span><span class="sxs-lookup"><span data-stu-id="76774-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="76774-199">這些 AJAX 要求可以使用其他技術 （例如要求標頭或 cookie） 傳送語彙基元。</span><span class="sxs-lookup"><span data-stu-id="76774-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="76774-200">如果，使用 cookie 來儲存驗證權杖，並驗證伺服器上的應用程式開發介面要求 CSRF 將會有潛在的問題。</span><span class="sxs-lookup"><span data-stu-id="76774-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="76774-201">不過，如果本機儲存體用來儲存的語彙基元，CSRF 的弱點可能會可能會降低，因為從本機儲存體的值不會自動傳送到每個新要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="76774-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="76774-202">因此，使用本機儲存體來儲存 antiforgery 語彙基元，在用戶端傳送權杖，因為要求標頭是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="76774-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="76774-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="76774-203">AngularJS</span></span>

<span data-ttu-id="76774-204">AngularJS 會到位址 CSRF 慣例。</span><span class="sxs-lookup"><span data-stu-id="76774-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="76774-205">如果伺服器傳送的 cookie 名稱``XSRF-TOKEN``，Angular``$http``服務會將加入的值從這個 cookie 標頭將要求傳送到此伺服器時。</span><span class="sxs-lookup"><span data-stu-id="76774-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="76774-206">此程序是自動的。您不需要明確設定的標頭。</span><span class="sxs-lookup"><span data-stu-id="76774-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="76774-207">標頭名稱``X-XSRF-TOKEN``。</span><span class="sxs-lookup"><span data-stu-id="76774-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="76774-208">伺服器應該偵測此標頭，並驗證其內容。</span><span class="sxs-lookup"><span data-stu-id="76774-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="76774-209">適用於 ASP.NET Core 應用程式開發介面使用這個慣例：</span><span class="sxs-lookup"><span data-stu-id="76774-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="76774-210">設定您的應用程式提供語彙基元在呼叫 cookie``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="76774-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="76774-211">設定 antiforgery 服務，以尋找名為標頭``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="76774-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="76774-212">[檢視範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample)。</span><span class="sxs-lookup"><span data-stu-id="76774-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="76774-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="76774-213">JavaScript</span></span>

<span data-ttu-id="76774-214">使用 JavaScript 與檢視，您可以建立使用從您的檢視中的服務權杖。</span><span class="sxs-lookup"><span data-stu-id="76774-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="76774-215">若要這樣做，您將插入`Microsoft.AspNetCore.Antiforgery.IAntiforgery`到檢視並呼叫服務`GetAndStoreTokens`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="76774-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="76774-216">這個方法不需要直接處理從伺服器中設定 cookie，或從用戶端讀取。</span><span class="sxs-lookup"><span data-stu-id="76774-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="76774-217">JavaScript 可以也存取 cookie 中所提供的權杖，然後再使用 cookie 的內容來建立標頭的語彙基元值，如下所示。</span><span class="sxs-lookup"><span data-stu-id="76774-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="76774-218">接著，假設您建構指令碼就會要求呼叫標頭中傳送的語彙基元``X-CSRF-TOKEN``，antiforgery 服務設定為尋找``X-CSRF-TOKEN``標頭：</span><span class="sxs-lookup"><span data-stu-id="76774-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="76774-219">下列範例會使用 jQuery 將 AJAX 要求具有適當的標頭：</span><span class="sxs-lookup"><span data-stu-id="76774-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="76774-220">設定 Antiforgery</span><span class="sxs-lookup"><span data-stu-id="76774-220">Configuring Antiforgery</span></span>

<span data-ttu-id="76774-221">`IAntiforgery`提供 API 來設定 antiforgery 系統。</span><span class="sxs-lookup"><span data-stu-id="76774-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="76774-222">可以要求在`Configure`方法`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="76774-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="76774-223">下列範例會使用產生 antiforgery 的語彙基元，並在回應中傳送為 （使用預設角度命名慣例上面所述） cookie 中介軟體應用程式的首頁上：</span><span class="sxs-lookup"><span data-stu-id="76774-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="76774-224">選項</span><span class="sxs-lookup"><span data-stu-id="76774-224">Options</span></span>

<span data-ttu-id="76774-225">您可以自訂[antiforgery 選項](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary)中`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="76774-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="76774-226">選項</span><span class="sxs-lookup"><span data-stu-id="76774-226">Option</span></span>        | <span data-ttu-id="76774-227">描述</span><span class="sxs-lookup"><span data-stu-id="76774-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="76774-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="76774-228">CookieDomain</span></span>  | <span data-ttu-id="76774-229">Cookie 的網域。</span><span class="sxs-lookup"><span data-stu-id="76774-229">The domain of the cookie.</span></span> <span data-ttu-id="76774-230">預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="76774-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="76774-231">CookieName</span><span class="sxs-lookup"><span data-stu-id="76774-231">CookieName</span></span>    | <span data-ttu-id="76774-232">Cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="76774-232">The name of the cookie.</span></span> <span data-ttu-id="76774-233">如果未設定，系統會產生唯一的名稱開頭`DefaultCookiePrefix`(」。AspNetCore.Antiforgery。")。</span><span class="sxs-lookup"><span data-stu-id="76774-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="76774-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="76774-234">CookiePath</span></span>    | <span data-ttu-id="76774-235">在 cookie 上設定的路徑。</span><span class="sxs-lookup"><span data-stu-id="76774-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="76774-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="76774-236">FormFieldName</span></span> | <span data-ttu-id="76774-237">Antiforgery 系統用來呈現 antiforgery 語彙基元，在檢視中的隱藏的表單欄位的名稱。</span><span class="sxs-lookup"><span data-stu-id="76774-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="76774-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="76774-238">HeaderName</span></span>    | <span data-ttu-id="76774-239">Antiforgery 系統所使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="76774-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="76774-240">如果`null`，系統會考慮只表單資料。</span><span class="sxs-lookup"><span data-stu-id="76774-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="76774-241">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="76774-241">RequireSsl</span></span>    | <span data-ttu-id="76774-242">指定 antiforgery 系統是否需要 SSL。</span><span class="sxs-lookup"><span data-stu-id="76774-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="76774-243">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="76774-243">Defaults to `false`.</span></span> <span data-ttu-id="76774-244">如果`true`，非 SSL 要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="76774-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="76774-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="76774-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="76774-246">指定是否要隱藏產生`X-Frame-Options`標頭。</span><span class="sxs-lookup"><span data-stu-id="76774-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="76774-247">根據預設，會產生標頭，其值為"Sameorigin 所"。</span><span class="sxs-lookup"><span data-stu-id="76774-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="76774-248">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="76774-248">Defaults to `false`.</span></span> |

<span data-ttu-id="76774-249">Https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions 如需詳細資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="76774-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="76774-250">擴充 Antiforgery</span><span class="sxs-lookup"><span data-stu-id="76774-250">Extending Antiforgery</span></span>

<span data-ttu-id="76774-251">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員擴充以便在往返過程中每個語彙基元的其他資料的防 XSRF 系統行為。</span><span class="sxs-lookup"><span data-stu-id="76774-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="76774-252">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_)每次呼叫方法會產生欄位語彙基元，並傳回值內嵌在產生的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="76774-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="76774-253">實作者可能傳回的時間戳記、 nonce 或任何其他值，然後呼叫[ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_)驗證語彙基元時驗證這項資料。</span><span class="sxs-lookup"><span data-stu-id="76774-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="76774-254">用戶端的使用者名稱已內嵌在產生的語彙基元，因此不需要加入這項資訊。</span><span class="sxs-lookup"><span data-stu-id="76774-254">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="76774-255">如果語彙基元包含補充資料但不是`IAntiForgeryAdditionalDataProvider`已經過設定，未驗證的補充資料。</span><span class="sxs-lookup"><span data-stu-id="76774-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="76774-256">Fundamentals</span><span class="sxs-lookup"><span data-stu-id="76774-256">Fundamentals</span></span>

<span data-ttu-id="76774-257">CSRF 攻擊依賴傳送每個要求對該網域與定義域相關聯的 cookie 的預設瀏覽器的行為。</span><span class="sxs-lookup"><span data-stu-id="76774-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="76774-258">這些 cookie 會儲存在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="76774-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="76774-259">它們通常會包含已驗證的使用者工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="76774-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="76774-260">受歡迎的形式的驗證 cookie 為基礎的驗證。</span><span class="sxs-lookup"><span data-stu-id="76774-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="76774-261">權杖型驗證系統已以受歡迎情況看出，成長特別是針對 SPAs 和其他 「 智慧型用戶端 」 案例。</span><span class="sxs-lookup"><span data-stu-id="76774-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="76774-262">Cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="76774-262">Cookie-based authentication</span></span>

<span data-ttu-id="76774-263">一旦使用者已使用使用者名稱和密碼驗證時，它們被發行可用來識別及驗證他們已經過驗證的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="76774-263">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="76774-264">權杖是以伴隨著每個要求的用戶端的 cookie 會儲存。</span><span class="sxs-lookup"><span data-stu-id="76774-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="76774-265">產生和驗證此 cookie 是由 cookie 驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="76774-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="76774-266">ASP.NET Core 提供 cookie[中介軟體](../fundamentals/middleware.md)的序列化經過加密的 cookie 的使用者主體，然後在後續要求中，驗證 cookie，會重新建立主體，並將其指派給`User`屬性`HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="76774-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="76774-267">使用 cookie 時，驗證 cookie 只是一個容器的表單驗證票證。</span><span class="sxs-lookup"><span data-stu-id="76774-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="76774-268">票證傳遞做為表單驗證 cookie，隨著每項要求的值，並為表單驗證，在伺服器上，用來識別已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="76774-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="76774-269">當使用者登入系統時，使用者工作階段會在伺服器端上建立並儲存在資料庫或其他持續性存放區。</span><span class="sxs-lookup"><span data-stu-id="76774-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="76774-270">系統會產生指向實際的工作階段資料存放區中的工作階段金鑰，並為用戶端端 cookie 傳送出去。</span><span class="sxs-lookup"><span data-stu-id="76774-270">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="76774-271">使用者要求的資源需要授權的任何時間時，網頁伺服器會檢查此工作階段金鑰。</span><span class="sxs-lookup"><span data-stu-id="76774-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="76774-272">系統會檢查相關聯的使用者工作階段是否擁有存取所要求的資源的權限。</span><span class="sxs-lookup"><span data-stu-id="76774-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="76774-273">若是如此，要求將會繼續。</span><span class="sxs-lookup"><span data-stu-id="76774-273">If so, the request continues.</span></span> <span data-ttu-id="76774-274">否則，要求會傳回為未獲授權。</span><span class="sxs-lookup"><span data-stu-id="76774-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="76774-275">這種方法，使用 cookie，以讓應用程式看起來似乎可設定狀態，因為它是可以 「 記住 」 的使用者之前驗證與伺服器。</span><span class="sxs-lookup"><span data-stu-id="76774-275">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="76774-276">使用者語彙基元</span><span class="sxs-lookup"><span data-stu-id="76774-276">User tokens</span></span>

<span data-ttu-id="76774-277">權杖型驗證不會儲存在伺服器上的工作階段。</span><span class="sxs-lookup"><span data-stu-id="76774-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="76774-278">相反地，當使用者登入它們正在發行的權杖 （不 antiforgery 語彙基元）。</span><span class="sxs-lookup"><span data-stu-id="76774-278">Instead, when a user is logged in they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="76774-279">這個語彙基元包含所有已驗證權杖所需的資料。</span><span class="sxs-lookup"><span data-stu-id="76774-279">This token holds all the data that's required to validate the token.</span></span> <span data-ttu-id="76774-280">它也包含使用者資訊，請在表單的[宣告](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model)。</span><span class="sxs-lookup"><span data-stu-id="76774-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="76774-281">當使用者想要存取需要驗證的伺服器資源時，權杖會傳送到其他授權標頭形式 Bearer {t} 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="76774-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="76774-282">這可讓應用程式無狀態因為在每個後續的要求語彙基元，在要求中傳遞伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="76774-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="76774-283">這個語彙基元不*加密*; 而是*編碼*。</span><span class="sxs-lookup"><span data-stu-id="76774-283">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="76774-284">在伺服器端權杖可以解碼存取權杖中的原始資訊。</span><span class="sxs-lookup"><span data-stu-id="76774-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="76774-285">若要在後續要求中傳送的語彙基元，您可以儲存它在瀏覽器的本機儲存體，或在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="76774-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="76774-286">您不必擔心 XSRF 弱點，如果您的權杖會儲存在本機儲存體中，但它會是問題，如果語彙基元，儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="76774-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="76774-287">在一個網域中裝載多個應用程式</span><span class="sxs-lookup"><span data-stu-id="76774-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="76774-288">即使`example1.cloudapp.net`和`example2.cloudapp.net`是不同的主控件下的所有主機之間沒有隱含的信任關係`*.cloudapp.net`網域。</span><span class="sxs-lookup"><span data-stu-id="76774-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="76774-289">這個隱含的信任關係可讓可能不受信任的主機會影響彼此的 cookie （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。</span><span class="sxs-lookup"><span data-stu-id="76774-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="76774-290">ASP.NET Core 執行階段提供某些風險降低，因為即使惡意的子網域可將覆寫工作階段語彙基元會無法產生使用者的有效的欄位語彙基元欄位語彙基元中，內嵌使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="76774-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="76774-291">不過，這類環境中裝載時的內建的防 XSRF 常式仍無法防止工作階段攔截或登入 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="76774-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="76774-292">共用的裝載環境包括 vunerable 劫持、 登入 CSRF，和其他攻擊。</span><span class="sxs-lookup"><span data-stu-id="76774-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="76774-293">其他資源</span><span class="sxs-lookup"><span data-stu-id="76774-293">Additional Resources</span></span>

* <span data-ttu-id="76774-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))上[開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)(OWASP)。</span><span class="sxs-lookup"><span data-stu-id="76774-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
