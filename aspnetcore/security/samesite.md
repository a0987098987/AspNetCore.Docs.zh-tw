---
title: 在 ASP.NET Core 中使用 SameSite cookie
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 中使用 SameSite cookie
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
- Electron
uid: security/samesite
ms.openlocfilehash: 3e3c12e17de3e12ead15c405e9339761a3f2f711
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944268"
---
# <a name="work-with-samesite-cookies-in-aspnet-core"></a><span data-ttu-id="e5d2f-103">在 ASP.NET Core 中使用 SameSite cookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-103">Work with SameSite cookies in ASP.NET Core</span></span>

<span data-ttu-id="e5d2f-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e5d2f-105">SameSite 是一種[IETF](https://ietf.org/about/)草稿標準，旨在針對跨網站偽造要求（CSRF）攻擊提供一些保護。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-105">SameSite is an [IETF](https://ietf.org/about/) draft standard designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="e5d2f-106">初稿標準已于[2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07)開始繪製，已于[2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00)更新。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-106">Originally drafted in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), the draft standard was updated in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span></span> <span data-ttu-id="e5d2f-107">更新的標準不會與前一個標準回溯相容，下列是最顯著的差異：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-107">The updated standard is not backward compatible with the previous standard, with the following being the most noticeable differences:</span></span>

* <span data-ttu-id="e5d2f-108">預設會將不含 SameSite 標頭的 cookie 視為 `SameSite=Lax` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-108">Cookies without SameSite header are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="e5d2f-109">`SameSite=None`必須用來允許跨網站 cookie 的使用。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-109">`SameSite=None` must be used to allow cross-site cookie use.</span></span>
* <span data-ttu-id="e5d2f-110">判斷提示的 cookie `SameSite=None` 也必須標記為 `Secure` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-110">Cookies that assert `SameSite=None` must also be marked as `Secure`.</span></span>
* <span data-ttu-id="e5d2f-111">使用的應用程式 [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) 可能會遇到 `sameSite=Lax` 或 cookie 的問題， `sameSite=Strict` 因為會被 `<iframe>` 視為跨網站案例。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-111">Applications that use [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) may experience issues with `sameSite=Lax` or `sameSite=Strict` cookies because `<iframe>` is treated as cross-site scenarios.</span></span>
* <span data-ttu-id="e5d2f-112">`SameSite=None` [2016 標準](https://tools.ietf.org/html/draft-west-first-party-cookies-07)不允許此值，而會導致某些實作為將這類 cookie 視為 `SameSite=Strict` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-112">The value `SameSite=None` is not allowed by the [2016 standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) and causes some implementations to treat such cookies as `SameSite=Strict`.</span></span> <span data-ttu-id="e5d2f-113">請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-113">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="e5d2f-114">此 `SameSite=Lax` 設定適用于大部分的應用程式 cookie。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-114">The `SameSite=Lax` setting works for most application cookies.</span></span> <span data-ttu-id="e5d2f-115">某些形式的驗證，例如[OpenID connect](https://openid.net/connect/) （OIDC）和[WS-同盟](https://auth0.com/docs/protocols/ws-fed)，預設為以 POST 為基礎的重新導向。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-115">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="e5d2f-116">以 POST 為基礎的重新導向會觸發 SameSite 瀏覽器保護，因此這些元件已停用 SameSite。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-116">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="e5d2f-117">大部分的[OAuth](https://oauth.net/)登入都不會受到影響，因為要求的流動方式不同。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-117">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="e5d2f-118">每個發出 cookie 的 ASP.NET Core 元件都必須決定是否適合 SameSite。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-118">Each ASP.NET Core component that emits cookies needs to decide if SameSite is appropriate.</span></span>

## <a name="samesite-and-identity"></a><span data-ttu-id="e5d2f-119">SameSite 和Identity</span><span class="sxs-lookup"><span data-stu-id="e5d2f-119">SameSite and Identity</span></span>

[!INCLUDE[](~/includes/SameSiteIdentity.md)]

## <a name="samesite-test-sample-code"></a><span data-ttu-id="e5d2f-120">SameSite 測試範例程式碼</span><span class="sxs-lookup"><span data-stu-id="e5d2f-120">SameSite test sample code</span></span>

 ::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="e5d2f-121">您可以下載並測試下列範例：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-121">The following samples can be downloaded and tested:</span></span>

| <span data-ttu-id="e5d2f-122">範例</span><span class="sxs-lookup"><span data-stu-id="e5d2f-122">Sample</span></span>               | <span data-ttu-id="e5d2f-123">Document</span><span class="sxs-lookup"><span data-stu-id="e5d2f-123">Document</span></span> |
| ----------------- | ------------ |
| [<span data-ttu-id="e5d2f-124">.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e5d2f-124">.NET Core MVC</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)  | <xref:security/samesite/mvc21> |
| <span data-ttu-id="e5d2f-125">[.NET Core Razor 頁面](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-125">[.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)</span></span>  | <xref:security/samesite/rp21> |

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e5d2f-126">您可以下載並測試下列範例：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-126">The following sample can be downloaded and tested:</span></span>


| <span data-ttu-id="e5d2f-127">範例</span><span class="sxs-lookup"><span data-stu-id="e5d2f-127">Sample</span></span>               | <span data-ttu-id="e5d2f-128">Document</span><span class="sxs-lookup"><span data-stu-id="e5d2f-128">Document</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e5d2f-129">[.NET Core Razor 頁面](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-129">[.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)</span></span>  | <xref:security/samesite/rp31> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="net-core-support-for-the-samesite-attribute"></a><span data-ttu-id="e5d2f-130">SameSite 屬性的 .NET Core 支援</span><span class="sxs-lookup"><span data-stu-id="e5d2f-130">.NET Core support for the sameSite attribute</span></span>

<span data-ttu-id="e5d2f-131">.NET Core 2.2 支援自12月2019的更新發行以來，適用于 SameSite 的 2019 draft standard。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-131">.NET Core 2.2 supports the 2019 draft standard for SameSite since the release of updates in December 2019.</span></span> <span data-ttu-id="e5d2f-132">開發人員可以使用屬性，以程式設計方式控制 sameSite 屬性的值 `HttpCookie.SameSite` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-132">Developers are able to programmatically control the value of the sameSite attribute using the `HttpCookie.SameSite` property.</span></span> <span data-ttu-id="e5d2f-133">將屬性設定為 [Strict]、[寬鬆] 或 [無] 會 `SameSite` 導致這些值在網路上使用 cookie 寫入。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-133">Setting the `SameSite` property to Strict, Lax, or None results in those values being written on the network with the cookie.</span></span> <span data-ttu-id="e5d2f-134">將其設定為等於（SameSiteMode）（-1）表示不應在具有 cookie 的網路上包含任何 sameSite 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d2f-134">Setting it equal to (SameSiteMode)(-1) indicates that no sameSite attribute should be included on the network with the cookie</span></span>

[!code-csharp[](samesite/snippets/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="e5d2f-135">.NET Core 3.0 支援已更新的 SameSite 值，並將額外的列舉值新增 `SameSiteMode.Unspecified` 至 `SameSiteMode` 列舉。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-135">.NET Core 3.0 supports the updated SameSite values and adds an extra enum value, `SameSiteMode.Unspecified` to the `SameSiteMode` enum.</span></span>
<span data-ttu-id="e5d2f-136">這個新的值表示不應使用 cookie 傳送 sameSite。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-136">This new value indicates no sameSite should be sent with the cookie.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

## <a name="december-patch-behavior-changes"></a><span data-ttu-id="e5d2f-137">12月修補程式列為變更</span><span class="sxs-lookup"><span data-stu-id="e5d2f-137">December patch behavior changes</span></span>

<span data-ttu-id="e5d2f-138">.NET Framework 和 .NET Core 2.1 的特定行為變更是 `SameSite` 屬性解讀值的方式 `None` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-138">The specific behavior change for .NET Framework and .NET Core 2.1 is how the `SameSite` property interprets the `None` value.</span></span> <span data-ttu-id="e5d2f-139">在修補程式之前，「 `None` 不要發出屬性」的值，在修補程式之後，表示「發出屬性，其值為」 `None` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-139">Before the patch a value of `None` meant "Do not emit the attribute at all", after the patch it means "Emit the attribute with a value of `None`".</span></span> <span data-ttu-id="e5d2f-140">在修補程式之後，的 `SameSite` 值 `(SameSiteMode)(-1)` 會導致不發出屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-140">After the patch a `SameSite` value of `(SameSiteMode)(-1)` causes the attribute not to be emitted.</span></span>

<span data-ttu-id="e5d2f-141">表單驗證和會話狀態 cookie 的預設 SameSite 值已從變更 `None` 為 `Lax` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-141">The default SameSite value for forms authentication and session state cookies was changed from `None` to `Lax`.</span></span>

::: moniker-end

## <a name="api-usage-with-samesite"></a><span data-ttu-id="e5d2f-142">使用 SameSite 的 API 使用方式</span><span class="sxs-lookup"><span data-stu-id="e5d2f-142">API usage with SameSite</span></span>

<span data-ttu-id="e5d2f-143">[HttpCoNtext](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)會將預設值附加至 `Unspecified` ，這表示 cookie 不會加入任何 SameSite 屬性，而且用戶端將會使用其預設行為（較寬鬆的新瀏覽器，也就是不需要）。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-143">[HttpContext.Response.Cookies.Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) defaults to `Unspecified`, meaning no SameSite attribute added to the cookie and the client will use its default behavior (Lax for new browsers, None for old ones).</span></span> <span data-ttu-id="e5d2f-144">下列程式碼顯示如何將 cookie SameSite 值變更為 `SameSiteMode.Lax` ：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-144">The following code shows how to change the cookie SameSite value to `SameSiteMode.Lax`:</span></span>

[!code-csharp[](samesite/sample/Pages/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="e5d2f-145">發出 cookie 的所有 ASP.NET Core 元件都會使用適用于其案例的設定來覆寫先前的預設值。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-145">All ASP.NET Core components that emit cookies override the preceding defaults with settings appropriate for their scenarios.</span></span> <span data-ttu-id="e5d2f-146">先前已覆寫的預設值尚未變更。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-146">The overridden preceding default values haven't changed.</span></span>

| <span data-ttu-id="e5d2f-147">元件</span><span class="sxs-lookup"><span data-stu-id="e5d2f-147">Component</span></span> | <span data-ttu-id="e5d2f-148">去</span><span class="sxs-lookup"><span data-stu-id="e5d2f-148">cookie</span></span> | <span data-ttu-id="e5d2f-149">預設</span><span class="sxs-lookup"><span data-stu-id="e5d2f-149">Default</span></span> |
| ------------- | ------------- |
| <xref:Microsoft.AspNetCore.Http.CookieBuilder> | <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite> | `Unspecified` |
| <xref:Microsoft.AspNetCore.Http.HttpContext.Session>  | [<span data-ttu-id="e5d2f-150">SessionOptions. Cookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-150">SessionOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Builder.SessionOptions.Cookie) |`Lax` |
| <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.CookieTempDataProvider>  | [<span data-ttu-id="e5d2f-151">CookieTempDataProviderOptions. Cookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-151">CookieTempDataProviderOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Mvc.CookieTempDataProviderOptions.Cookie) | `Lax` |
| <xref:Microsoft.AspNetCore.Antiforgery.IAntiforgery> | [<span data-ttu-id="e5d2f-152">AntiforgeryOptions. Cookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-152">AntiforgeryOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions.Cookie)| `Strict` |
| [<span data-ttu-id="e5d2f-153">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="e5d2f-153">Cookie Authentication</span></span>](xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*) | [<span data-ttu-id="e5d2f-154">Cookieauthenticationoptions.authenticationtype. Cookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-154">CookieAuthenticationOptions.Cookie</span></span>](xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.CookieName) | `Lax` |
| <xref:Microsoft.Extensions.DependencyInjection.TwitterExtensions.AddTwitter*> | [<span data-ttu-id="e5d2f-155">TwitterOptions.StateCookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-155">TwitterOptions.StateCookie </span></span>](xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterOptions.StateCookie) | `Lax`  |
| <span data-ttu-id="e5d2f-156"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [RemoteAuthenticationOptions.CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None`</span><span class="sxs-lookup"><span data-stu-id="e5d2f-156"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [RemoteAuthenticationOptions.CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None`</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.OpenIdConnectExtensions.AddOpenIdConnect*> | [<span data-ttu-id="e5d2f-157">OpenIdConnectOptions.NonceCookie</span><span class="sxs-lookup"><span data-stu-id="e5d2f-157">OpenIdConnectOptions.NonceCookie</span></span>](xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.OpenIdConnectOptions.NonceCookie)| `None` |
| [<span data-ttu-id="e5d2f-158">HttpCoNtext. Cookie. Append</span><span class="sxs-lookup"><span data-stu-id="e5d2f-158">HttpContext.Response.Cookies.Append</span></span>](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) | <xref:Microsoft.AspNetCore.Http.CookieOptions> | `Unspecified` |

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="e5d2f-159">ASP.NET Core 3.1 和更新版本提供下列 SameSite 支援：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-159">ASP.NET Core 3.1 and later provides the following SameSite support:</span></span>

* <span data-ttu-id="e5d2f-160">重新定義發出的行為 `SameSiteMode.None``SameSite=None`</span><span class="sxs-lookup"><span data-stu-id="e5d2f-160">Redefines the behavior of `SameSiteMode.None` to emit `SameSite=None`</span></span>
* <span data-ttu-id="e5d2f-161">加入新的值 `SameSiteMode.Unspecified` ，以省略 SameSite 屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-161">Adds a new value `SameSiteMode.Unspecified` to omit the SameSite attribute.</span></span>
* <span data-ttu-id="e5d2f-162">所有 cookie Api 都會預設為 `Unspecified` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-162">All cookies APIs default to `Unspecified`.</span></span> <span data-ttu-id="e5d2f-163">某些使用 cookie 的元件會針對其案例設定更具體的值。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-163">Some components that use cookies set values more specific to their scenarios.</span></span> <span data-ttu-id="e5d2f-164">如需範例，請參閱上表。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-164">See the table above for examples.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e5d2f-165">在 ASP.NET Core 3.0 和更新版本中，SameSite 預設值已變更，以避免與不一致的用戶端預設值發生衝突。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-165">In ASP.NET Core 3.0 and later the SameSite defaults were changed to avoid conflicting with inconsistent client defaults.</span></span> <span data-ttu-id="e5d2f-166">下列 Api 已將預設值從變更 `SameSiteMode.Lax ` 為 `-1` ，以避免發出這些 Cookie 的 SameSite 屬性：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-166">The following APIs have changed the default from `SameSiteMode.Lax ` to `-1` to avoid emitting a SameSite attribute for these cookies:</span></span>

* <span data-ttu-id="e5d2f-167"><xref:Microsoft.AspNetCore.Http.CookieOptions>用於[HttpCoNtext. Response. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-167"><xref:Microsoft.AspNetCore.Http.CookieOptions> used with [HttpContext.Response.Cookies.Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)</span></span>
* <span data-ttu-id="e5d2f-168"><xref:Microsoft.AspNetCore.Http.CookieBuilder>當做的 factory 使用`CookieOptions`</span><span class="sxs-lookup"><span data-stu-id="e5d2f-168"><xref:Microsoft.AspNetCore.Http.CookieBuilder>  used as a factory for `CookieOptions`</span></span>
* [<span data-ttu-id="e5d2f-169">CookiePolicyOptions.MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="e5d2f-169">CookiePolicyOptions.MinimumSameSitePolicy</span></span>](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)

::: moniker-end

## <a name="history-and-changes"></a><span data-ttu-id="e5d2f-170">歷程記錄和變更</span><span class="sxs-lookup"><span data-stu-id="e5d2f-170">History and changes</span></span>

<span data-ttu-id="e5d2f-171">SameSite 支援首次使用[2016 draft 標準](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)在2.0 的 ASP.NET Core 中執行。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-171">SameSite support was first implemented in ASP.NET Core in 2.0 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span> <span data-ttu-id="e5d2f-172">2016標準是加入宣告。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-172">The 2016 standard was opt-in.</span></span> <span data-ttu-id="e5d2f-173">預設會將數個 cookie 設定為，以 ASP.NET Core 加入宣告 `Lax` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-173">ASP.NET Core opted-in by setting several cookies to `Lax` by default.</span></span> <span data-ttu-id="e5d2f-174">在驗證發生數個[問題](https://github.com/aspnet/Announcements/issues/318)之後，大部分 SameSite 的使用量都會[停用](https://github.com/aspnet/Announcements/issues/348)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-174">After encountering several [issues](https://github.com/aspnet/Announcements/issues/318) with authentication, most SameSite usage was [disabled](https://github.com/aspnet/Announcements/issues/348).</span></span>

<span data-ttu-id="e5d2f-175">[修補程式](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)于2019年11月發行，從2016標準更新為2019標準。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-175">[Patches](https://devblogs.microsoft.com/dotnet/net-core-November-2019/) were issued in November 2019 to update from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="e5d2f-176">[SameSite 規格的2019草稿](https://github.com/aspnet/Announcements/issues/390)：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-176">The [2019 draft of the SameSite specification](https://github.com/aspnet/Announcements/issues/390):</span></span>

* <span data-ttu-id="e5d2f-177">與2016草稿**不**相容。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-177">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="e5d2f-178">如需詳細資訊，請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-178">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="e5d2f-179">指定 cookie 預設會視為 `SameSite=Lax` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-179">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="e5d2f-180">指定明確判斷提示 `SameSite=None` 以啟用跨網站傳遞的 cookie 應該標記為 `Secure` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-180">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should be marked as `Secure`.</span></span> <span data-ttu-id="e5d2f-181">`None`是要退出的新專案。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-181">`None` is a new entry to opt out.</span></span>
* <span data-ttu-id="e5d2f-182">發行給 ASP.NET Core 2.1、2.2 和3.0 的修補程式支援。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-182">Is supported by patches issued for ASP.NET Core 2.1, 2.2, and 3.0.</span></span> <span data-ttu-id="e5d2f-183">ASP.NET Core 3.1 有額外的 SameSite 支援。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-183">ASP.NET Core 3.1 has additional SameSite support.</span></span>
* <span data-ttu-id="e5d2f-184">排定預設會在[2020 年2月](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)啟用。 [Chrome](https://chromestatus.com/feature/5088147346030592)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-184">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="e5d2f-185">瀏覽器已開始移至2019中的此標準。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-185">Browsers started moving to this standard in 2019.</span></span>

## <a name="apis-impacted-by-the-change-from-the-2016-samesite-draft-standard-to-the-2019-draft-standard"></a><span data-ttu-id="e5d2f-186">受 2016 SameSite draft standard 變更為 2019 draft standard 的 Api</span><span class="sxs-lookup"><span data-stu-id="e5d2f-186">APIs impacted by the change from the 2016 SameSite draft standard to the 2019 draft standard</span></span>

* [<span data-ttu-id="e5d2f-187">SameSiteMode</span><span class="sxs-lookup"><span data-stu-id="e5d2f-187">Http.SameSiteMode</span></span>](xref:Microsoft.AspNetCore.Http.SameSiteMode)
* [<span data-ttu-id="e5d2f-188">Cookieoptions.secure. SameSite</span><span class="sxs-lookup"><span data-stu-id="e5d2f-188">CookieOptions.SameSite</span></span>](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [<span data-ttu-id="e5d2f-189">CookieBuilder. SameSite</span><span class="sxs-lookup"><span data-stu-id="e5d2f-189">CookieBuilder.SameSite</span></span>](xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite)
* [<span data-ttu-id="e5d2f-190">CookiePolicyOptions.MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="e5d2f-190">CookiePolicyOptions.MinimumSameSitePolicy</span></span>](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)
* <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
* <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite?displayProperty=fullName>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="e5d2f-191">支援舊版瀏覽器</span><span class="sxs-lookup"><span data-stu-id="e5d2f-191">Supporting older browsers</span></span>

<span data-ttu-id="e5d2f-192">2016 SameSite 標準規定必須將未知的值視為 `SameSite=Strict` 值。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-192">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="e5d2f-193">從支援 2016 SameSite standard 的舊版瀏覽器存取的應用程式，可能會在取得值為的 SameSite 屬性時中斷 `None` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-193">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="e5d2f-194">如果 Web 應用程式想要支援較舊的瀏覽器，則必須執行瀏覽器偵測。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-194">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="e5d2f-195">ASP.NET Core 不會實作為瀏覽器偵測，因為使用者代理程式的值會高度變動並經常變更。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-195">ASP.NET Core doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span> <span data-ttu-id="e5d2f-196">中的擴充點 <xref:Microsoft.AspNetCore.CookiePolicy> 允許插入使用者代理程式特定的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-196">An extension point in <xref:Microsoft.AspNetCore.CookiePolicy> allows plugging in User-Agent specific logic.</span></span>

<span data-ttu-id="e5d2f-197">在中 `Startup.Configure` ，新增在呼叫之前呼叫的程式碼， <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 或*任何*寫入 cookie 的方法：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-197">In `Startup.Configure`, add code that calls <xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> before calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> or *any* method that writes cookies:</span></span>

[!code-csharp[](samesite/sample/Startup.cs?name=snippet5&highlight=18-19)]

<span data-ttu-id="e5d2f-198">在中 `Startup.ConfigureServices` ，新增類似下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-198">In `Startup.ConfigureServices`, add code similar to the following:</span></span>

::: moniker range="= aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup.cs?name=snippet)]

::: moniker-end

<span data-ttu-id="e5d2f-199">在上述範例中， `MyUserAgentDetectionLib.DisallowsSameSiteNone` 是使用者提供的程式庫，可偵測使用者代理程式是否不支援 SameSite `None` ：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-199">In the preceding sample, `MyUserAgentDetectionLib.DisallowsSameSiteNone` is a user supplied library that detects if the user agent doesn't support SameSite `None`:</span></span>

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet2)]

<span data-ttu-id="e5d2f-200">下列程式碼顯示範例 `DisallowsSameSiteNone` 方法：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-200">The following code shows a sample `DisallowsSameSiteNone` method:</span></span>

> [!WARNING]
> <span data-ttu-id="e5d2f-201">下列程式碼僅供示範之用：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-201">The following code is for demonstration only:</span></span>
> * <span data-ttu-id="e5d2f-202">不應該將它視為已完成。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-202">It should not be considered complete.</span></span>
> * <span data-ttu-id="e5d2f-203">不會維護或支援。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-203">It is not maintained or supported.</span></span>

[!code-csharp[](samesite/sample/Startup31.cs?name=snippetX)]

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="e5d2f-204">測試應用程式的 SameSite 問題</span><span class="sxs-lookup"><span data-stu-id="e5d2f-204">Test apps for SameSite problems</span></span>

<span data-ttu-id="e5d2f-205">與遠端網站（例如透過協力廠商登入）互動的應用程式需要：</span><span class="sxs-lookup"><span data-stu-id="e5d2f-205">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="e5d2f-206">測試多個瀏覽器的互動。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-206">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="e5d2f-207">套用本檔中討論的[CookiePolicy 瀏覽器偵測和緩和措施](#sob)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-207">Apply the [CookiePolicy browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="e5d2f-208">使用可選擇新 SameSite 行為的用戶端版本來測試 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-208">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="e5d2f-209">Chrome、Firefox 和 Chromium Edge 全都具有可用於測試的新加入宣告功能旗標。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-209">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="e5d2f-210">在您的應用程式套用 SameSite 修補程式之後，請使用較舊的用戶端版本（尤其是 Safari）進行測試。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-210">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="e5d2f-211">如需詳細資訊，請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-211">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="e5d2f-212">使用 Chrome 進行測試</span><span class="sxs-lookup"><span data-stu-id="e5d2f-212">Test with Chrome</span></span>

<span data-ttu-id="e5d2f-213">Chrome 78 + 會提供誤導的結果，因為它有暫時的緩和措施。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-213">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="e5d2f-214">Chrome 78 + 暫時緩和可讓 cookie 少於兩分鐘。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-214">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="e5d2f-215">已啟用適當測試旗標的 Chrome 76 或77提供更精確的結果。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-215">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="e5d2f-216">若要測試新的 SameSite 行為，請切換 `chrome://flags/#same-site-by-default-cookies` 為 [**已啟用**]。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-216">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="e5d2f-217">舊版 Chrome （75和更低版本）會回報為失敗，並出現新的 `None` 設定。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-217">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="e5d2f-218">請參閱本檔中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-218">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="e5d2f-219">Google 不會提供舊版的 chrome 版本。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-219">Google does not make older chrome versions available.</span></span> <span data-ttu-id="e5d2f-220">請遵循[下載 Chromium](https://www.chromium.org/getting-involved/download-chromium)的指示來測試舊版 Chrome。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-220">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="e5d2f-221">請勿從搜尋舊版 chrome 所提供的**連結下載 Chrome** 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-221">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="e5d2f-222">Chromium 76 Win64</span><span class="sxs-lookup"><span data-stu-id="e5d2f-222">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="e5d2f-223">Chromium 74 Win64</span><span class="sxs-lookup"><span data-stu-id="e5d2f-223">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

<span data-ttu-id="e5d2f-224">從抑制的版本開始 `80.0.3975.0` ，您可以使用新的旗標來停用最寬鬆的 + POST 暫時緩和， `--enable-features=SameSiteDefaultChecksMethodRigorously` 以允許在已移除緩和功能的最終狀態中測試網站和服務。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-224">Starting in Canary version `80.0.3975.0`, the Lax+POST temporary mitigation can be disabled for testing purposes using the new flag `--enable-features=SameSiteDefaultChecksMethodRigorously` to allow testing of sites and services in the eventual end state of the feature where the mitigation has been removed.</span></span> <span data-ttu-id="e5d2f-225">如需詳細資訊，請參閱 Chromium Projects [SameSite Updates](https://www.chromium.org/updates/same-site)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-225">For more information, see The Chromium Projects [SameSite Updates](https://www.chromium.org/updates/same-site)</span></span>

### <a name="test-with-safari"></a><span data-ttu-id="e5d2f-226">使用 Safari 進行測試</span><span class="sxs-lookup"><span data-stu-id="e5d2f-226">Test with Safari</span></span>

<span data-ttu-id="e5d2f-227">Safari 12 已嚴格實作為先前的草稿，當新的 `None` 值在 cookie 中時就會失敗。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-227">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="e5d2f-228">`None`透過支援本檔中[較舊流覽](#sob)器的瀏覽器偵測程式碼來避免。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-228">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="e5d2f-229">使用 MSAL、ADAL 或您使用的任何程式庫，測試 Safari 12、Safari 13 和以 WebKit 為基礎的 OS 樣式登入。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-229">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="e5d2f-230">問題取決於基礎作業系統版本。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-230">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="e5d2f-231">OSX Mojave （10.14）和 iOS 12 已知具有新 SameSite 行為的相容性問題。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-231">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="e5d2f-232">將 OS 升級至 OSX Catalina （10.15）或 iOS 13 會修正問題。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-232">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="e5d2f-233">Safari 目前沒有加入宣告旗標來測試新的規格行為。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-233">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="e5d2f-234">使用 Firefox 進行測試</span><span class="sxs-lookup"><span data-stu-id="e5d2f-234">Test with Firefox</span></span>

<span data-ttu-id="e5d2f-235">您可以使用功能旗標在頁面上進行選擇，以在第68版上測試新標準的 Firefox 支援 `about:config` `network.cookie.sameSite.laxByDefault` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-235">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="e5d2f-236">尚未報告舊版 Firefox 的相容性問題。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-236">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-browser"></a><span data-ttu-id="e5d2f-237">使用 Edge 瀏覽器進行測試</span><span class="sxs-lookup"><span data-stu-id="e5d2f-237">Test with Edge browser</span></span>

<span data-ttu-id="e5d2f-238">Edge 支援舊的 SameSite 標準。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-238">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="e5d2f-239">Edge 版本44沒有任何已知的新標準相容性問題。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-239">Edge version 44 doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="e5d2f-240">使用 Edge 進行測試（Chromium）</span><span class="sxs-lookup"><span data-stu-id="e5d2f-240">Test with Edge (Chromium)</span></span>

<span data-ttu-id="e5d2f-241">SameSite 旗標是在頁面上設定的 `edge://flags/#same-site-by-default-cookies` 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-241">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="e5d2f-242">Edge Chromium 未發現相容性問題。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-242">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="e5d2f-243">測試方式Electron</span><span class="sxs-lookup"><span data-stu-id="e5d2f-243">Test with Electron</span></span>

<span data-ttu-id="e5d2f-244">的版本 Electron 包含較舊版本的 Chromium。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-244">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="e5d2f-245">例如，小組所使用的版本 Electron 是 Chromium 66，這會展現較舊的行為。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-245">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="e5d2f-246">您必須使用您的產品版本來執行自己的相容性測試 Electron 。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-246">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="e5d2f-247">請參閱下一節中的[支援舊版瀏覽器](#sob)。</span><span class="sxs-lookup"><span data-stu-id="e5d2f-247">See [Supporting older browsers](#sob) in the following section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5d2f-248">其他資源</span><span class="sxs-lookup"><span data-stu-id="e5d2f-248">Additional resources</span></span>

* [<span data-ttu-id="e5d2f-249">Chromium Blog：開發人員：準備開始新的 SameSite = None;安全 Cookie 設定</span><span class="sxs-lookup"><span data-stu-id="e5d2f-249">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="e5d2f-250">SameSite cookie 說明</span><span class="sxs-lookup"><span data-stu-id="e5d2f-250">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)
* [<span data-ttu-id="e5d2f-251">2019年11月修補程式</span><span class="sxs-lookup"><span data-stu-id="e5d2f-251">November 2019 Patches</span></span>](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)

 ::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

| <span data-ttu-id="e5d2f-252">範例</span><span class="sxs-lookup"><span data-stu-id="e5d2f-252">Sample</span></span>               | <span data-ttu-id="e5d2f-253">Document</span><span class="sxs-lookup"><span data-stu-id="e5d2f-253">Document</span></span> |
| ----------------- | ------------ |
| [<span data-ttu-id="e5d2f-254">.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e5d2f-254">.NET Core MVC</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)  | <xref:security/samesite/mvc21> |
| <span data-ttu-id="e5d2f-255">[.NET Core Razor 頁面](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-255">[.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)</span></span>  | <xref:security/samesite/rp21> |

::: moniker-end

 ::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="e5d2f-256">範例</span><span class="sxs-lookup"><span data-stu-id="e5d2f-256">Sample</span></span>               | <span data-ttu-id="e5d2f-257">Document</span><span class="sxs-lookup"><span data-stu-id="e5d2f-257">Document</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e5d2f-258">[.NET Core Razor 頁面](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)</span><span class="sxs-lookup"><span data-stu-id="e5d2f-258">[.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)</span></span>  | <xref:security/samesite/rp31> |

::: moniker-end
