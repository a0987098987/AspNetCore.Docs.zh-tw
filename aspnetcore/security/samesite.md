---
title: 在 ASP.NET Core 中使用 SameSite cookie
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 中使用 SameSite cookie
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite
ms.openlocfilehash: b344ed8f539979210980b3421659207edd513f32
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146429"
---
# <a name="work-with-samesite-cookies-in-aspnet-core"></a><span data-ttu-id="f16b0-103">在 ASP.NET Core 中使用 SameSite cookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-103">Work with SameSite cookies in ASP.NET Core</span></span>

<span data-ttu-id="f16b0-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f16b0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f16b0-105">[SameSite](https://tools.ietf.org/html/draft-west-first-party-cookies-07)是一種[IETF](https://ietf.org/about/)草稿，其設計目的是要針對跨網站偽造要求（CSRF）攻擊提供一些保護。</span><span class="sxs-lookup"><span data-stu-id="f16b0-105">[SameSite](https://tools.ietf.org/html/draft-west-first-party-cookies-07) is an [IETF](https://ietf.org/about/) draft designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="f16b0-106">[SameSite 2019 草稿](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00)：</span><span class="sxs-lookup"><span data-stu-id="f16b0-106">The [SameSite 2019 draft](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):</span></span>

* <span data-ttu-id="f16b0-107">預設會將 cookie 視為 `SameSite=Lax`。</span><span class="sxs-lookup"><span data-stu-id="f16b0-107">Treats cookies as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="f16b0-108">說明明確判斷提示 `SameSite=None` 以啟用跨網站傳遞的 cookie 應標示為 `Secure`。</span><span class="sxs-lookup"><span data-stu-id="f16b0-108">States cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span>

<span data-ttu-id="f16b0-109">`Lax` 適用于大部分的應用程式 cookie。</span><span class="sxs-lookup"><span data-stu-id="f16b0-109">`Lax` works for most app cookies.</span></span> <span data-ttu-id="f16b0-110">某些形式的驗證，例如[OpenID connect](https://openid.net/connect/) （OIDC）和[WS-同盟](https://auth0.com/docs/protocols/ws-fed)，預設為以 POST 為基礎的重新導向。</span><span class="sxs-lookup"><span data-stu-id="f16b0-110">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="f16b0-111">以 POST 為基礎的重新導向會觸發 SameSite 瀏覽器保護，因此這些元件已停用 SameSite。</span><span class="sxs-lookup"><span data-stu-id="f16b0-111">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="f16b0-112">大部分的[OAuth](https://oauth.net/)登入都不會受到影響，因為要求的流動方式不同。</span><span class="sxs-lookup"><span data-stu-id="f16b0-112">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="f16b0-113">`None` 參數會導致用戶端的相容性問題，而此標準會實作為先前的2016草稿標準（例如 iOS 12）。</span><span class="sxs-lookup"><span data-stu-id="f16b0-113">The `None` parameter causes compatibility problems with clients that implemented the prior 2016 draft standard (for example, iOS 12).</span></span> <span data-ttu-id="f16b0-114">請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-114">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="f16b0-115">每個發出 cookie 的 ASP.NET Core 元件都必須決定是否適合 SameSite。</span><span class="sxs-lookup"><span data-stu-id="f16b0-115">Each ASP.NET Core component that emits cookies needs to decide if SameSite is appropriate.</span></span>

## <a name="api-usage-with-samesite"></a><span data-ttu-id="f16b0-116">使用 SameSite 的 API 使用方式</span><span class="sxs-lookup"><span data-stu-id="f16b0-116">API usage with SameSite</span></span>

<span data-ttu-id="f16b0-117">SameSite 會將預設值[附加](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)至 `Unspecified`，這表示 cookie 不會加入任何屬性，而且用戶端將會使用其預設行為（較寬鬆的新瀏覽器，也就是不需要）。</span><span class="sxs-lookup"><span data-stu-id="f16b0-117">[HttpContext.Response.Cookies.Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) defaults to `Unspecified`, meaning no SameSite attribute added to the cookie and the client will use its default behavior (Lax for new browsers, None for old ones).</span></span> <span data-ttu-id="f16b0-118">下列程式碼顯示如何將 cookie SameSite 值變更為 `SameSiteMode.Lax`：</span><span class="sxs-lookup"><span data-stu-id="f16b0-118">The following code shows how to change the cookie SameSite value to `SameSiteMode.Lax`:</span></span>

[!code-csharp[](samesite/sample/Pages/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="f16b0-119">發出 cookie 的所有 ASP.NET Core 元件都會使用適用于其案例的設定來覆寫先前的預設值。</span><span class="sxs-lookup"><span data-stu-id="f16b0-119">All ASP.NET Core components that emit cookies override the preceding defaults with settings appropriate for their scenarios.</span></span> <span data-ttu-id="f16b0-120">先前已覆寫的預設值尚未變更。</span><span class="sxs-lookup"><span data-stu-id="f16b0-120">The overridden preceding default values haven't changed.</span></span>

| <span data-ttu-id="f16b0-121">元件</span><span class="sxs-lookup"><span data-stu-id="f16b0-121">Component</span></span> | <span data-ttu-id="f16b0-122">Cookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-122">cookie</span></span> | <span data-ttu-id="f16b0-123">Default</span><span class="sxs-lookup"><span data-stu-id="f16b0-123">Default</span></span> |
| ------------- | ------------- |
| <xref:Microsoft.AspNetCore.Http.CookieBuilder> | <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite> | `Unspecified` |
| <xref:Microsoft.AspNetCore.Http.HttpContext.Session>  | [<span data-ttu-id="f16b0-124">SessionOptions. Cookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-124">SessionOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Builder.SessionOptions.Cookie) |`Lax` |
| <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.CookieTempDataProvider>  | [<span data-ttu-id="f16b0-125">CookieTempDataProviderOptions. Cookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-125">CookieTempDataProviderOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Mvc.CookieTempDataProviderOptions.Cookie) | `Lax` |
| <xref:Microsoft.AspNetCore.Antiforgery.IAntiforgery> | [<span data-ttu-id="f16b0-126">AntiforgeryOptions. Cookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-126">AntiforgeryOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions.Cookie)| `Strict` |
| [<span data-ttu-id="f16b0-127">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="f16b0-127">Cookie Authentication</span></span>](xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*) | [<span data-ttu-id="f16b0-128">Cookieauthenticationoptions.authenticationtype. Cookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-128">CookieAuthenticationOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.CookieName) | `Lax` |
| <xref:Microsoft.Extensions.DependencyInjection.TwitterExtensions.AddTwitter*> | [<span data-ttu-id="f16b0-129">TwitterOptions.StateCookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-129">TwitterOptions.StateCookie </span></span>](xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterOptions.StateCookie) | `Lax`  |
| <span data-ttu-id="f16b0-130"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [RemoteAuthenticationOptions. CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None`</span><span class="sxs-lookup"><span data-stu-id="f16b0-130"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [RemoteAuthenticationOptions.CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None`</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.OpenIdConnectExtensions.AddOpenIdConnect*> | [<span data-ttu-id="f16b0-131">OpenIdConnectOptions.NonceCookie</span><span class="sxs-lookup"><span data-stu-id="f16b0-131">OpenIdConnectOptions.NonceCookie</span></span>](xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.OpenIdConnectOptions.NonceCookie)| `None` |
| [<span data-ttu-id="f16b0-132">HttpCoNtext. Cookie. Append</span><span class="sxs-lookup"><span data-stu-id="f16b0-132">HttpContext.Response.Cookies.Append</span></span>](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) | <xref:Microsoft.AspNetCore.Http.CookieOptions> | `Unspecified` |

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="f16b0-133">ASP.NET Core 3.1 和更新版本提供下列 SameSite 支援：</span><span class="sxs-lookup"><span data-stu-id="f16b0-133">ASP.NET Core 3.1 and later provides the following SameSite support:</span></span>

* <span data-ttu-id="f16b0-134">重新定義發出 `SameSiteMode.None` 的行為 `SameSite=None`</span><span class="sxs-lookup"><span data-stu-id="f16b0-134">Redefines the behavior of `SameSiteMode.None` to emit `SameSite=None`</span></span>
* <span data-ttu-id="f16b0-135">加入新的值 `SameSiteMode.Unspecified` 以省略 SameSite 屬性。</span><span class="sxs-lookup"><span data-stu-id="f16b0-135">Adds a new value `SameSiteMode.Unspecified` to omit the SameSite attribute.</span></span>
* <span data-ttu-id="f16b0-136">所有 cookie Api 預設為 `Unspecified`。</span><span class="sxs-lookup"><span data-stu-id="f16b0-136">All cookies APIs default to `Unspecified`.</span></span> <span data-ttu-id="f16b0-137">某些使用 cookie 的元件會針對其案例設定更具體的值。</span><span class="sxs-lookup"><span data-stu-id="f16b0-137">Some components that use cookies set values more specific to their scenarios.</span></span> <span data-ttu-id="f16b0-138">如需範例，請參閱上表。</span><span class="sxs-lookup"><span data-stu-id="f16b0-138">See the table above for examples.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f16b0-139">在 ASP.NET Core 3.0 和更新版本中，SameSite 預設值已變更，以避免與不一致的用戶端預設值發生衝突。</span><span class="sxs-lookup"><span data-stu-id="f16b0-139">In ASP.NET Core 3.0 and later the SameSite defaults were changed to avoid conflicting with inconsistent client defaults.</span></span> <span data-ttu-id="f16b0-140">下列 Api 已將預設值從 `SameSiteMode.Lax ` 變更為 `-1`，以避免發出這些 cookie 的 SameSite 屬性：</span><span class="sxs-lookup"><span data-stu-id="f16b0-140">The following APIs have changed the default from `SameSiteMode.Lax ` to `-1` to avoid emitting a SameSite attribute for these cookies:</span></span>

* <span data-ttu-id="f16b0-141"><xref:Microsoft.AspNetCore.Http.CookieOptions> 用於[HttpCoNtext. Response. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)</span><span class="sxs-lookup"><span data-stu-id="f16b0-141"><xref:Microsoft.AspNetCore.Http.CookieOptions> used with [HttpContext.Response.Cookies.Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)</span></span>
* <span data-ttu-id="f16b0-142"><xref:Microsoft.AspNetCore.Http.CookieBuilder> 用來做為 `CookieOptions` 的 factory</span><span class="sxs-lookup"><span data-stu-id="f16b0-142"><xref:Microsoft.AspNetCore.Http.CookieBuilder>  used as a factory for `CookieOptions`</span></span>
* [<span data-ttu-id="f16b0-143">CookiePolicyOptions.MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f16b0-143">CookiePolicyOptions.MinimumSameSitePolicy</span></span>](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)

::: moniker-end

## <a name="history-and-changes"></a><span data-ttu-id="f16b0-144">歷程記錄和變更</span><span class="sxs-lookup"><span data-stu-id="f16b0-144">History and changes</span></span>

<span data-ttu-id="f16b0-145">SameSite 支援首次使用[2016 draft 標準](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)在2.0 的 ASP.NET Core 中執行。</span><span class="sxs-lookup"><span data-stu-id="f16b0-145">SameSite support was first implemented in ASP.NET Core in 2.0 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span> <span data-ttu-id="f16b0-146">2016標準是加入宣告。</span><span class="sxs-lookup"><span data-stu-id="f16b0-146">The 2016 standard was opt-in.</span></span> <span data-ttu-id="f16b0-147">ASP.NET Core 依預設將數個 cookie 設定為 `Lax` 來加入宣告。</span><span class="sxs-lookup"><span data-stu-id="f16b0-147">ASP.NET Core opted-in by setting several cookies to `Lax` by default.</span></span> <span data-ttu-id="f16b0-148">在驗證發生數個[問題](https://github.com/aspnet/Announcements/issues/318)之後，大部分 SameSite 的使用量都會[停用](https://github.com/aspnet/Announcements/issues/348)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-148">After encountering several [issues](https://github.com/aspnet/Announcements/issues/318) with authentication, most SameSite usage was [disabled](https://github.com/aspnet/Announcements/issues/348).</span></span>

<span data-ttu-id="f16b0-149">[修補程式](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)于2019年11月發行，從2016標準更新為2019標準。</span><span class="sxs-lookup"><span data-stu-id="f16b0-149">[Patches](https://devblogs.microsoft.com/dotnet/net-core-November-2019/) were issued in November 2019 to update from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="f16b0-150">[SameSite 規格的2019草稿](https://github.com/aspnet/Announcements/issues/390)：</span><span class="sxs-lookup"><span data-stu-id="f16b0-150">The [2019 draft of the SameSite specification](https://github.com/aspnet/Announcements/issues/390):</span></span>

* <span data-ttu-id="f16b0-151">與2016草稿**不**相容。</span><span class="sxs-lookup"><span data-stu-id="f16b0-151">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="f16b0-152">如需詳細資訊，請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-152">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="f16b0-153">指定預設會將 cookie 視為 `SameSite=Lax`。</span><span class="sxs-lookup"><span data-stu-id="f16b0-153">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="f16b0-154">指定明確判斷提示 `SameSite=None` 以啟用跨網站傳遞的 cookie，應該標示為 `Secure`。</span><span class="sxs-lookup"><span data-stu-id="f16b0-154">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span> <span data-ttu-id="f16b0-155">`None` 是退出宣告的新專案。</span><span class="sxs-lookup"><span data-stu-id="f16b0-155">`None` is a new entry to opt out.</span></span>
* <span data-ttu-id="f16b0-156">發行給 ASP.NET Core 2.1、2.2 和3.0 的修補程式支援。</span><span class="sxs-lookup"><span data-stu-id="f16b0-156">Is supported by patches issued for ASP.NET Core 2.1, 2.2, and 3.0.</span></span> <span data-ttu-id="f16b0-157">ASP.NET Core 3.1 有額外的 SameSite 支援。</span><span class="sxs-lookup"><span data-stu-id="f16b0-157">ASP.NET Core 3.1 has additional SameSite support.</span></span>
* <span data-ttu-id="f16b0-158">排定預設會在[2020 年2月](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)由 [Chrome](https://chromestatus.com/feature/5088147346030592) 啟用。</span><span class="sxs-lookup"><span data-stu-id="f16b0-158">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="f16b0-159">瀏覽器已開始移至2019中的此標準。</span><span class="sxs-lookup"><span data-stu-id="f16b0-159">Browsers started moving to this standard in 2019.</span></span>

## <a name="apis-impacted-by-the-change-from-the-2016-samesite-draft-standard-to-the-2019-draft-standard"></a><span data-ttu-id="f16b0-160">受 2016 SameSite draft standard 變更為 2019 draft standard 的 Api</span><span class="sxs-lookup"><span data-stu-id="f16b0-160">APIs impacted by the change from the 2016 SameSite draft standard to the 2019 draft standard</span></span>

* [<span data-ttu-id="f16b0-161">SameSiteMode</span><span class="sxs-lookup"><span data-stu-id="f16b0-161">Http.SameSiteMode</span></span>](xref:Microsoft.AspNetCore.Http.SameSiteMode)
* [<span data-ttu-id="f16b0-162">Cookieoptions.secure. SameSite</span><span class="sxs-lookup"><span data-stu-id="f16b0-162">CookieOptions.SameSite</span></span>](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [<span data-ttu-id="f16b0-163">CookieBuilder. SameSite</span><span class="sxs-lookup"><span data-stu-id="f16b0-163">CookieBuilder.SameSite</span></span>](xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite)
* [<span data-ttu-id="f16b0-164">CookiePolicyOptions.MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="f16b0-164">CookiePolicyOptions.MinimumSameSitePolicy</span></span>](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)
* <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
* <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite?displayProperty=fullName>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="f16b0-165">支援舊版瀏覽器</span><span class="sxs-lookup"><span data-stu-id="f16b0-165">Supporting older browsers</span></span>

<span data-ttu-id="f16b0-166">2016 SameSite 標準規定必須將未知的值視為 `SameSite=Strict` 值。</span><span class="sxs-lookup"><span data-stu-id="f16b0-166">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="f16b0-167">從支援 2016 SameSite standard 的舊版瀏覽器存取的應用程式，可能會在取得值為 `None`的 SameSite 屬性時中斷。</span><span class="sxs-lookup"><span data-stu-id="f16b0-167">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="f16b0-168">如果 Web 應用程式想要支援較舊的瀏覽器，則必須執行瀏覽器偵測。</span><span class="sxs-lookup"><span data-stu-id="f16b0-168">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="f16b0-169">ASP.NET Core 不會實作為瀏覽器偵測，因為使用者代理程式的值會高度變動並經常變更。</span><span class="sxs-lookup"><span data-stu-id="f16b0-169">ASP.NET Core doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span> <span data-ttu-id="f16b0-170"><xref:Microsoft.AspNetCore.CookiePolicy> 中的擴充點可讓您插入使用者代理程式特定的邏輯。</span><span class="sxs-lookup"><span data-stu-id="f16b0-170">An extension point in <xref:Microsoft.AspNetCore.CookiePolicy> allows plugging in User-Agent specific logic.</span></span>

<span data-ttu-id="f16b0-171">在 `Startup.Configure`中，新增呼叫 <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> 的程式碼，然後再呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 或*任何*寫入 cookie 的方法：</span><span class="sxs-lookup"><span data-stu-id="f16b0-171">In `Startup.Configure`, add code that calls <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or *any* method that writes cookies:</span></span>

[!code-csharp[](samesite/sample/Startup.cs?name=snippet5&highlight=18-19)]

<span data-ttu-id="f16b0-172">在 `Startup.ConfigureServices`中，新增類似下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f16b0-172">In `Startup.ConfigureServices`, add code similar to the following:</span></span>

::: moniker range="= aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup.cs?name=snippet)]

::: moniker-end

<span data-ttu-id="f16b0-173">在上述範例中，`MyUserAgentDetectionLib.DisallowsSameSiteNone` 是使用者提供的程式庫，可偵測使用者代理程式是否不支援 SameSite `None`：</span><span class="sxs-lookup"><span data-stu-id="f16b0-173">In the preceding sample, `MyUserAgentDetectionLib.DisallowsSameSiteNone` is a user supplied library that detects if the user agent doesn't support SameSite `None`:</span></span>

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet2)]

<span data-ttu-id="f16b0-174">下列程式碼顯示範例 `DisallowsSameSiteNone` 方法：</span><span class="sxs-lookup"><span data-stu-id="f16b0-174">The following code shows a sample `DisallowsSameSiteNone` method:</span></span>

> [!WARNING]
> <span data-ttu-id="f16b0-175">下列程式碼僅供示範之用：</span><span class="sxs-lookup"><span data-stu-id="f16b0-175">The following code is for demonstration only:</span></span>
> * <span data-ttu-id="f16b0-176">不應該將它視為已完成。</span><span class="sxs-lookup"><span data-stu-id="f16b0-176">It should not be considered complete.</span></span>
> * <span data-ttu-id="f16b0-177">不會維護或支援。</span><span class="sxs-lookup"><span data-stu-id="f16b0-177">It is not maintained or supported.</span></span>

[!code-csharp[](samesite/sample/Startup31.cs?name=snippetX)]

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="f16b0-178">測試應用程式的 SameSite 問題</span><span class="sxs-lookup"><span data-stu-id="f16b0-178">Test apps for SameSite problems</span></span>

<span data-ttu-id="f16b0-179">與遠端網站（例如透過協力廠商登入）互動的應用程式需要：</span><span class="sxs-lookup"><span data-stu-id="f16b0-179">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="f16b0-180">測試多個瀏覽器的互動。</span><span class="sxs-lookup"><span data-stu-id="f16b0-180">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="f16b0-181">套用本檔中討論的[CookiePolicy 瀏覽器偵測和緩和措施](#sob)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-181">Apply the [CookiePolicy browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="f16b0-182">使用可選擇新 SameSite 行為的用戶端版本來測試 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f16b0-182">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="f16b0-183">Chrome、Firefox 和 Chromium Edge 全都具有可用於測試的新加入宣告功能旗標。</span><span class="sxs-lookup"><span data-stu-id="f16b0-183">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="f16b0-184">在您的應用程式套用 SameSite 修補程式之後，請使用較舊的用戶端版本（尤其是 Safari）進行測試。</span><span class="sxs-lookup"><span data-stu-id="f16b0-184">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="f16b0-185">如需詳細資訊，請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-185">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="f16b0-186">使用 Chrome 進行測試</span><span class="sxs-lookup"><span data-stu-id="f16b0-186">Test with Chrome</span></span>

<span data-ttu-id="f16b0-187">Chrome 78 + 會提供誤導的結果，因為它有暫時的緩和措施。</span><span class="sxs-lookup"><span data-stu-id="f16b0-187">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="f16b0-188">Chrome 78 + 暫時緩和可讓 cookie 少於兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="f16b0-188">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="f16b0-189">已啟用適當測試旗標的 Chrome 76 或77提供更精確的結果。</span><span class="sxs-lookup"><span data-stu-id="f16b0-189">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="f16b0-190">若要測試新的 SameSite 行為，請切換 `chrome://flags/#same-site-by-default-cookies` 為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="f16b0-190">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="f16b0-191">舊版 Chrome （75和更低版本）會回報為失敗，並出現新的 `None` 設定。</span><span class="sxs-lookup"><span data-stu-id="f16b0-191">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="f16b0-192">請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-192">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="f16b0-193">Google 不會提供舊版的 chrome 版本。</span><span class="sxs-lookup"><span data-stu-id="f16b0-193">Google does not make older chrome versions available.</span></span> <span data-ttu-id="f16b0-194">請遵循[下載 Chromium](https://www.chromium.org/getting-involved/download-chromium)的指示來測試舊版 Chrome。</span><span class="sxs-lookup"><span data-stu-id="f16b0-194">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="f16b0-195">請勿從搜尋舊版 chrome 所提供的**連結下載 Chrome** 。</span><span class="sxs-lookup"><span data-stu-id="f16b0-195">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="f16b0-196">Chromium 76 Win64</span><span class="sxs-lookup"><span data-stu-id="f16b0-196">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="f16b0-197">Chromium 74 Win64</span><span class="sxs-lookup"><span data-stu-id="f16b0-197">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a><span data-ttu-id="f16b0-198">使用 Safari 進行測試</span><span class="sxs-lookup"><span data-stu-id="f16b0-198">Test with Safari</span></span>

<span data-ttu-id="f16b0-199">Safari 12 會嚴格實作為先前的草稿，並在新的 `None` 值在 cookie 中失敗。</span><span class="sxs-lookup"><span data-stu-id="f16b0-199">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="f16b0-200">透過此檔中[支援較舊流覽](#sob)器的瀏覽器偵測程式碼，可避免 `None`。</span><span class="sxs-lookup"><span data-stu-id="f16b0-200">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="f16b0-201">使用 MSAL、ADAL 或您使用的任何程式庫，測試 Safari 12、Safari 13 和以 WebKit 為基礎的 OS 樣式登入。</span><span class="sxs-lookup"><span data-stu-id="f16b0-201">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="f16b0-202">問題取決於基礎作業系統版本。</span><span class="sxs-lookup"><span data-stu-id="f16b0-202">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="f16b0-203">OSX Mojave （10.14）和 iOS 12 已知具有新 SameSite 行為的相容性問題。</span><span class="sxs-lookup"><span data-stu-id="f16b0-203">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="f16b0-204">將 OS 升級至 OSX Catalina （10.15）或 iOS 13 會修正問題。</span><span class="sxs-lookup"><span data-stu-id="f16b0-204">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="f16b0-205">Safari 目前沒有加入宣告旗標來測試新的規格行為。</span><span class="sxs-lookup"><span data-stu-id="f16b0-205">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="f16b0-206">使用 Firefox 進行測試</span><span class="sxs-lookup"><span data-stu-id="f16b0-206">Test with Firefox</span></span>

<span data-ttu-id="f16b0-207">在 [`about:config`] 頁面上選擇 [`network.cookie.sameSite.laxByDefault`] 功能旗標，即可在版本 68 + 上測試新標準的 Firefox 支援。</span><span class="sxs-lookup"><span data-stu-id="f16b0-207">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="f16b0-208">尚未報告舊版 Firefox 的相容性問題。</span><span class="sxs-lookup"><span data-stu-id="f16b0-208">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-browser"></a><span data-ttu-id="f16b0-209">使用 Edge 瀏覽器進行測試</span><span class="sxs-lookup"><span data-stu-id="f16b0-209">Test with Edge browser</span></span>

<span data-ttu-id="f16b0-210">Edge 支援舊的 SameSite 標準。</span><span class="sxs-lookup"><span data-stu-id="f16b0-210">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="f16b0-211">Edge 版本44沒有任何已知的新標準相容性問題。</span><span class="sxs-lookup"><span data-stu-id="f16b0-211">Edge version 44 doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="f16b0-212">使用 Edge 進行測試（Chromium）</span><span class="sxs-lookup"><span data-stu-id="f16b0-212">Test with Edge (Chromium)</span></span>

<span data-ttu-id="f16b0-213">在 [`edge://flags/#same-site-by-default-cookies`] 頁面上設定 SameSite 旗標。</span><span class="sxs-lookup"><span data-stu-id="f16b0-213">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="f16b0-214">Edge Chromium 未發現相容性問題。</span><span class="sxs-lookup"><span data-stu-id="f16b0-214">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="f16b0-215">使用 Electron 進行測試</span><span class="sxs-lookup"><span data-stu-id="f16b0-215">Test with Electron</span></span>

<span data-ttu-id="f16b0-216">Electron 的版本包含舊版的 Chromium。</span><span class="sxs-lookup"><span data-stu-id="f16b0-216">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="f16b0-217">例如，小組所使用的 Electron 版本是 Chromium 66，這會展現較舊的行為。</span><span class="sxs-lookup"><span data-stu-id="f16b0-217">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="f16b0-218">您必須使用您的產品所使用的 Electron 版本來執行自己的相容性測試。</span><span class="sxs-lookup"><span data-stu-id="f16b0-218">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="f16b0-219">請參閱下一節中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="f16b0-219">See [Supporting older browsers](#sob) in the following section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f16b0-220">其他資源</span><span class="sxs-lookup"><span data-stu-id="f16b0-220">Additional resources</span></span>

* [<span data-ttu-id="f16b0-221">Chromium Blog：開發人員：準備開始新的 SameSite = None;安全 Cookie 設定</span><span class="sxs-lookup"><span data-stu-id="f16b0-221">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="f16b0-222">SameSite cookie 說明</span><span class="sxs-lookup"><span data-stu-id="f16b0-222">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)
* [<span data-ttu-id="f16b0-223">2019年11月修補程式</span><span class="sxs-lookup"><span data-stu-id="f16b0-223">November 2019 Patches</span></span>](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)
