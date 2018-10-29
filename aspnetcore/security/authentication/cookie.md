---
title: 使用沒有 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 說明的使用沒有 ASP.NET Core 身分識別的 cookie 驗證
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 8add7559557d505397c3be8d8a48aa2e9d9e45e8
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207416"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="7e52a-103">使用沒有 ASP.NET Core 身分識別的 cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="7e52a-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="7e52a-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7e52a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7e52a-105">當您在先前的驗證主題中所見[ASP.NET Core Identity](xref:security/authentication/identity)是完整且功能完整的驗證提供者來建立及維護的登入。</span><span class="sxs-lookup"><span data-stu-id="7e52a-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="7e52a-106">不過，您可能要使用您自己的自訂驗證邏輯使用有時 cookie 型驗證。</span><span class="sxs-lookup"><span data-stu-id="7e52a-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="7e52a-107">您可以使用以 cookie 為基礎的驗證作為獨立驗證提供者沒有 ASP.NET Core 身分識別。</span><span class="sxs-lookup"><span data-stu-id="7e52a-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="7e52a-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7e52a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7e52a-109">基於示範目的，範例應用程式中，假設使用者林麗莉 Rodriguez 的使用者帳戶會是硬式編碼到應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e52a-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="7e52a-110">使用電子郵件使用者名稱 」maria.rodriguez@contoso.com」 和任何登入使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="7e52a-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="7e52a-111">使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="7e52a-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="7e52a-112">在真實世界範例中，您會驗證使用者，對資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e52a-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="7e52a-113">如需移轉以 cookie 為基礎的驗證，從 ASP.NET Core 1.x 至 2.0，請參閱[移轉驗證和 ASP.NET Core 2.0 主題 （Cookie 型驗證） 的身分識別](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="7e52a-114">若要使用 ASP.NET Core 身分識別，請參閱[身分識別簡介](xref:security/authentication/identity)主題。</span><span class="sxs-lookup"><span data-stu-id="7e52a-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="7e52a-115">組態</span><span class="sxs-lookup"><span data-stu-id="7e52a-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e52a-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7e52a-117">如果應用程式不會使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，建立的專案檔中的套件參考[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)封裝 (版本 2.1.0 或更新版本）。</span><span class="sxs-lookup"><span data-stu-id="7e52a-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="7e52a-118">在 `ConfigureServices`方法，建立驗證中介軟體服務`AddAuthentication`和`AddCookie`方法：</span><span class="sxs-lookup"><span data-stu-id="7e52a-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="7e52a-119">`AuthenticationScheme` 傳遞至`AddAuthentication`設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="7e52a-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="7e52a-120">`AuthenticationScheme` 當有多個執行個體的 cookie 驗證，而且您想要時非常有用[特定的結構描述的授權](xref:security/authorization/limitingidentitybyscheme)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="7e52a-121">設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="7e52a-122">您可以提供區分配置任何字串值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="7e52a-123">應用程式的驗證配置與不同應用程式的 cookie 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="7e52a-123">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="7e52a-124">當 cookie 驗證配置不提供給<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>，它會使用[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookie")。</span><span class="sxs-lookup"><span data-stu-id="7e52a-124">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookies").</span></span>

<span data-ttu-id="7e52a-125">在 `Configure`方法，請使用`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="7e52a-125">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="7e52a-126">呼叫`UseAuthentication`方法之前呼叫`UseMvcWithDefaultRoute`或`UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="7e52a-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="7e52a-127">**AddCookie 選項**</span><span class="sxs-lookup"><span data-stu-id="7e52a-127">**AddCookie Options**</span></span>

<span data-ttu-id="7e52a-128">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)類別用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="7e52a-128">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="7e52a-129">選項</span><span class="sxs-lookup"><span data-stu-id="7e52a-129">Option</span></span> | <span data-ttu-id="7e52a-130">描述</span><span class="sxs-lookup"><span data-stu-id="7e52a-130">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="7e52a-131">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="7e52a-131">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-132">提供要找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ForbidAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-132">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="7e52a-133">預設值是 `/Account/AccessDenied`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-133">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="7e52a-134">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="7e52a-134">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-135">若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證服務所建立的任何宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="7e52a-135">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="7e52a-136">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="7e52a-136">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-137">其中提供 cookie 的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-137">The domain name where the cookie is served.</span></span> <span data-ttu-id="7e52a-138">根據預設，這是要求的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-138">By default, this is the host name of the request.</span></span> <span data-ttu-id="7e52a-139">瀏覽器只在要求中，cookie 傳送相符的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-139">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="7e52a-140">您可能想要調整該項目可在網域中擁有任何主機可用的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-140">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="7e52a-141">例如，若要設定 cookie 網域`.contoso.com`並提供給`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-141">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="7e52a-142">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="7e52a-142">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-143">取得或設定 cookie 的存留時間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-143">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="7e52a-144">目前這個選項不會生效，就會變成過時的 ASP.NET Core 2.1 +。</span><span class="sxs-lookup"><span data-stu-id="7e52a-144">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="7e52a-145">使用`ExpireTimeSpan`設定 cookie 的到期時間的選項。</span><span class="sxs-lookup"><span data-stu-id="7e52a-145">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="7e52a-146">如需詳細資訊，請參閱 <<c0> [ 釐清行為 CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-146">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="7e52a-147">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="7e52a-147">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-148">旗標，指出是否 cookie 應該僅供伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="7e52a-148">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="7e52a-149">此值變更為`false`允許用戶端指令碼，以存取 cookie，可能會用來開啟您的應用程式應有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。</span><span class="sxs-lookup"><span data-stu-id="7e52a-149">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="7e52a-150">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-150">The default value is `true`.</span></span> |
| [<span data-ttu-id="7e52a-151">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="7e52a-151">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-152">設定 cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-152">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="7e52a-153">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="7e52a-153">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-154">用來隔離在相同的主機名稱上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e52a-154">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="7e52a-155">如果您在執行的應用程式`/app1`想要將 cookie 限制為該應用程式，請設定`CookiePath`屬性設`/app1`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-155">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="7e52a-156">如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e52a-156">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="7e52a-157">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="7e52a-157">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-158">表示瀏覽器是否應該允許 cookie 附加至相同站台要求 (`SameSiteMode.Strict`) 或跨網站要求使用安全的 HTTP 方法和相同站台要求 (`SameSiteMode.Lax`)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-158">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="7e52a-159">當設定為`SameSiteMode.None`，未設定的 cookie 標頭值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-159">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="7e52a-160">請注意， [Cookie 原則中介軟體](#cookie-policy-middleware)可能會覆寫您所提供的值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-160">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="7e52a-161">若要支援 OAuth 驗證，預設值是`SameSiteMode.Lax`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-161">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="7e52a-162">如需詳細資訊，請參閱 < [SameSite cookie 原則因為損毀的 OAuth 驗證](https://github.com/aspnet/Security/issues/1231)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-162">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="7e52a-163">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="7e52a-163">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-164">旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-164">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="7e52a-165">預設值是 `CookieSecurePolicy.SameAsRequest`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-165">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="7e52a-166">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="7e52a-166">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-167">設定組`DataProtectionProvider`用來建立預設`TicketDataFormat`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-167">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="7e52a-168">如果`TicketDataFormat`屬性設定，`DataProtectionProvider`選項不會使用。</span><span class="sxs-lookup"><span data-stu-id="7e52a-168">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="7e52a-169">如果未提供，則會使用應用程式的預設資料保護提供者。</span><span class="sxs-lookup"><span data-stu-id="7e52a-169">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="7e52a-170">事件</span><span class="sxs-lookup"><span data-stu-id="7e52a-170">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-171">處理常式會呼叫提供者，讓應用程式控制項的特定處理時間點上的方法。</span><span class="sxs-lookup"><span data-stu-id="7e52a-171">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="7e52a-172">如果`Events`不提供的預設執行個體提供呼叫方法時，不做任何動作。</span><span class="sxs-lookup"><span data-stu-id="7e52a-172">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="7e52a-173">EventsType</span><span class="sxs-lookup"><span data-stu-id="7e52a-173">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-174">若要取得做為服務類型`Events`而非屬性的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7e52a-174">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="7e52a-175">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="7e52a-175">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-176">`TimeSpan`之後儲存在 cookie 內的驗證票證已過期。</span><span class="sxs-lookup"><span data-stu-id="7e52a-176">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="7e52a-177">`ExpireTimeSpan` 會加入至目前的時間來建立票證的到期時間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-177">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="7e52a-178">`ExpiredTimeSpan`值一律會移到加密的 AuthTicket 由伺服器進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7e52a-178">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="7e52a-179">它也可能會進入[Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)標頭，但是只有`IsPersistent`設定。</span><span class="sxs-lookup"><span data-stu-id="7e52a-179">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="7e52a-180">若要設定`IsPersistent`要`true`，設定[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)傳遞至`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-180">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="7e52a-181">預設值`ExpireTimeSpan`為 14 天。</span><span class="sxs-lookup"><span data-stu-id="7e52a-181">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="7e52a-182">LoginPath</span><span class="sxs-lookup"><span data-stu-id="7e52a-182">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-183">提供要找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ChallengeAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-183">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="7e52a-184">產生 401 的目前 URL 加入至`LoginPath`所命名的查詢字串參數為`ReturnUrlParameter`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-184">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="7e52a-185">一次要求`LoginPath`授與新登入的身分識別，`ReturnUrlParameter`值用來將瀏覽器重新導向回到原始的未經授權的狀態程式碼的 URL。</span><span class="sxs-lookup"><span data-stu-id="7e52a-185">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="7e52a-186">預設值是 `/Account/Login`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-186">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="7e52a-187">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="7e52a-187">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-188">如果`LogoutPath`提供，以處理常式中，則該路徑的要求重新導向的值為基礎`ReturnUrlParameter`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-188">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="7e52a-189">預設值是 `/Account/Logout`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-189">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="7e52a-190">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="7e52a-190">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-191">判斷由 302 已找到 （重新導向 URL） 回應的處理常式會附加查詢字串參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-191">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="7e52a-192">`ReturnUrlParameter` 在要求抵達時，會使用`LoginPath`或`LogoutPath`執行登入或登出動作後，瀏覽器傳回至原始 URL。</span><span class="sxs-lookup"><span data-stu-id="7e52a-192">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="7e52a-193">預設值是 `ReturnUrl`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-193">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="7e52a-194">SessionStore</span><span class="sxs-lookup"><span data-stu-id="7e52a-194">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-195">選擇性的容器，用來儲存跨要求的身分識別。</span><span class="sxs-lookup"><span data-stu-id="7e52a-195">An optional container used to store identity across requests.</span></span> <span data-ttu-id="7e52a-196">使用時，只有工作階段識別碼會傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="7e52a-196">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="7e52a-197">`SessionStore` 可用來降低大型的身分識別的潛在問題。</span><span class="sxs-lookup"><span data-stu-id="7e52a-197">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="7e52a-198">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="7e52a-198">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-199">旗標，指出是否要以動態方式發出新的更新的到期時間的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-199">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="7e52a-200">這可能會發生的任何位置的目前 cookie 到期期間超過 50%過期的要求。</span><span class="sxs-lookup"><span data-stu-id="7e52a-200">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="7e52a-201">新的到期日往前移動目前的日期加上`ExpireTimespan`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-201">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="7e52a-202">[絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-202">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="7e52a-203">絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。</span><span class="sxs-lookup"><span data-stu-id="7e52a-203">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="7e52a-204">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-204">The default value is `true`.</span></span> |
| [<span data-ttu-id="7e52a-205">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="7e52a-205">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-206">`TicketDataFormat`用來保護且取消保護身分識別和其他屬性儲存在 cookie 值中。</span><span class="sxs-lookup"><span data-stu-id="7e52a-206">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="7e52a-207">如果未提供，`TicketDataFormat`會使用建立[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-207">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="7e52a-208">驗證</span><span class="sxs-lookup"><span data-stu-id="7e52a-208">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="7e52a-209">檢查的選項都是有效的方法。</span><span class="sxs-lookup"><span data-stu-id="7e52a-209">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="7e52a-210">設定`CookieAuthenticationOptions`中的驗證服務組態中`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="7e52a-210">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e52a-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7e52a-212">ASP.NET Core 1.x 使用 cookie[中介軟體](xref:fundamentals/middleware/index)，序列化的加密 cookie 的使用者主體。</span><span class="sxs-lookup"><span data-stu-id="7e52a-212">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="7e52a-213">在後續的要求，對 cookie 進行驗證，以及主體會重新建立並指派給`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="7e52a-213">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="7e52a-214">安裝[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)在專案中的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7e52a-214">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="7e52a-215">此套件包含的 cookie 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7e52a-215">This package contains the cookie middleware.</span></span>

<span data-ttu-id="7e52a-216">使用`UseCookieAuthentication`方法中的`Configure`方法，在您*Startup.cs*檔案之後，再`UseMvc`或`UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="7e52a-216">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="7e52a-217">**CookieAuthenticationOptions 選項**</span><span class="sxs-lookup"><span data-stu-id="7e52a-217">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="7e52a-218">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)類別用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="7e52a-218">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="7e52a-219">選項</span><span class="sxs-lookup"><span data-stu-id="7e52a-219">Option</span></span> | <span data-ttu-id="7e52a-220">描述</span><span class="sxs-lookup"><span data-stu-id="7e52a-220">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="7e52a-221">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="7e52a-221">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-222">設定驗證配置。</span><span class="sxs-lookup"><span data-stu-id="7e52a-222">Sets the authentication scheme.</span></span> <span data-ttu-id="7e52a-223">`AuthenticationScheme` 當有多個執行個體的驗證，而且您想要時非常有用[特定的結構描述的授權](xref:security/authorization/limitingidentitybyscheme)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-223">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="7e52a-224">設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-224">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="7e52a-225">您可以提供區分配置任何字串值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-225">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="7e52a-226">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="7e52a-226">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-227">設定值，表示 cookie 驗證，應該在每個要求上執行，並且在嘗試驗證，並重新建構它建立任何序列化的主體。</span><span class="sxs-lookup"><span data-stu-id="7e52a-227">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="7e52a-228">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="7e52a-228">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-229">如果為 true，則驗證中介軟體會處理自動的挑戰。</span><span class="sxs-lookup"><span data-stu-id="7e52a-229">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="7e52a-230">如果為 false，驗證中介軟體只會改變明確指出時的回應`AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-230">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="7e52a-231">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="7e52a-231">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-232">若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證中介軟體所建立的任何宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="7e52a-232">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="7e52a-233">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="7e52a-233">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-234">其中提供 cookie 的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-234">The domain name where the cookie is served.</span></span> <span data-ttu-id="7e52a-235">根據預設，這是要求的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="7e52a-235">By default, this is the host name of the request.</span></span> <span data-ttu-id="7e52a-236">瀏覽器，只是要比對的主機名稱的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-236">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="7e52a-237">您可能想要調整該項目可在網域中擁有任何主機可用的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-237">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="7e52a-238">例如，若要設定 cookie 網域`.contoso.com`並提供給`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-238">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="7e52a-239">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="7e52a-239">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-240">旗標，指出是否 cookie 應該僅供伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="7e52a-240">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="7e52a-241">此值變更為`false`允許用戶端指令碼，以存取 cookie，可能會用來開啟您的應用程式應有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。</span><span class="sxs-lookup"><span data-stu-id="7e52a-241">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="7e52a-242">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-242">The default value is `true`.</span></span> |
| [<span data-ttu-id="7e52a-243">CookiePath</span><span class="sxs-lookup"><span data-stu-id="7e52a-243">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-244">用來隔離在相同的主機名稱上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e52a-244">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="7e52a-245">如果您在執行的應用程式`/app1`想要將 cookie 限制為該應用程式，請設定`CookiePath`屬性設`/app1`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-245">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="7e52a-246">如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e52a-246">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="7e52a-247">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="7e52a-247">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-248">旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-248">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="7e52a-249">預設值是 `CookieSecurePolicy.SameAsRequest`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-249">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="7e52a-250">描述</span><span class="sxs-lookup"><span data-stu-id="7e52a-250">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-251">其他資訊提供給應用程式的驗證類型的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7e52a-251">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="7e52a-252">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="7e52a-252">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-253">`TimeSpan`之後驗證票證已過期。</span><span class="sxs-lookup"><span data-stu-id="7e52a-253">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="7e52a-254">它會新增至目前的時間來建立票證的到期時間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-254">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="7e52a-255">若要使用`ExpireTimeSpan`，您必須設定`IsPersistent`要`true`中`AuthenticationProperties`傳遞至`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-255">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="7e52a-256">預設值為 14 天。</span><span class="sxs-lookup"><span data-stu-id="7e52a-256">The default value is 14 days.</span></span> |
| [<span data-ttu-id="7e52a-257">slidingExpiration</span><span class="sxs-lookup"><span data-stu-id="7e52a-257">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="7e52a-258">旗標，指出是否 cookie 的到期日重設時超過一半的`ExpireTimeSpan`經過這段間隔。</span><span class="sxs-lookup"><span data-stu-id="7e52a-258">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="7e52a-259">新的 exipiration 時間往前移動目前的日期加上`ExpireTimespan`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-259">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="7e52a-260">[絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-260">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="7e52a-261">絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。</span><span class="sxs-lookup"><span data-stu-id="7e52a-261">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="7e52a-262">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-262">The default value is `true`.</span></span> |

<span data-ttu-id="7e52a-263">設定`CookieAuthenticationOptions`Cookie 驗證中介軟體中`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="7e52a-263">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="7e52a-264">Cookie 原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="7e52a-264">Cookie Policy Middleware</span></span>

<span data-ttu-id="7e52a-265">[Cookie 原則中介軟體](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)可讓應用程式中的 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="7e52a-265">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="7e52a-266">將中介軟體新增至應用程式處理管線是敏感; 的順序它只會影響已註冊後，管線中的元件。</span><span class="sxs-lookup"><span data-stu-id="7e52a-266">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="7e52a-267">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)提供給 Cookie 原則中介軟體可讓您控制全域性質的 cookie 處理和攔截到的 cookie 處理處理常式，當您附加或刪除 cookie 時。</span><span class="sxs-lookup"><span data-stu-id="7e52a-267">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="7e52a-268">屬性</span><span class="sxs-lookup"><span data-stu-id="7e52a-268">Property</span></span> | <span data-ttu-id="7e52a-269">描述</span><span class="sxs-lookup"><span data-stu-id="7e52a-269">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="7e52a-270">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="7e52a-270">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="7e52a-271">會影響是否 cookie 必須 HttpOnly，這是旗標，指出是否 cookie 應該僅供伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="7e52a-271">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="7e52a-272">預設值是 `HttpOnlyPolicy.None`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-272">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="7e52a-273">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="7e52a-273">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="7e52a-274">會影響的 cookie 相同站台屬性 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="7e52a-274">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="7e52a-275">預設值是 `SameSiteMode.Lax`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-275">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="7e52a-276">此選項可供 ASP.NET Core 2.0 +。</span><span class="sxs-lookup"><span data-stu-id="7e52a-276">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="7e52a-277">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="7e52a-277">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="7e52a-278">呼叫時，附加 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-278">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="7e52a-279">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="7e52a-279">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="7e52a-280">刪除 cookie 時，會呼叫它。</span><span class="sxs-lookup"><span data-stu-id="7e52a-280">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="7e52a-281">安全</span><span class="sxs-lookup"><span data-stu-id="7e52a-281">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="7e52a-282">會影響是否 cookie 必須是安全。</span><span class="sxs-lookup"><span data-stu-id="7e52a-282">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="7e52a-283">預設值是 `CookieSecurePolicy.None`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-283">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="7e52a-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + 只)</span><span class="sxs-lookup"><span data-stu-id="7e52a-284">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="7e52a-285">預設值`MinimumSameSitePolicy`值是`SameSiteMode.Lax`允許 OAuth2 驗證。</span><span class="sxs-lookup"><span data-stu-id="7e52a-285">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="7e52a-286">嚴格強制執行的相同站台原則`SameSiteMode.Strict`，將`MinimumSameSitePolicy`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-286">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="7e52a-287">雖然 OAuth2 和其他跨原始來源的驗證配置，這項設定會中斷，它的權限提高 cookie 的用戶端進行跨原始來源要求處理的應用程式的其他類型的安全性層級。</span><span class="sxs-lookup"><span data-stu-id="7e52a-287">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="7e52a-288">Cookie 原則中介軟體設定`MinimumSameSitePolicy`可能會影響您設定`Cookie.SameSite`在`CookieAuthenticationOptions`根據下面的矩陣圖的設定。</span><span class="sxs-lookup"><span data-stu-id="7e52a-288">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="7e52a-289">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="7e52a-289">MinimumSameSitePolicy</span></span> | <span data-ttu-id="7e52a-290">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="7e52a-290">Cookie.SameSite</span></span> | <span data-ttu-id="7e52a-291">結果 Cookie.SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="7e52a-291">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="7e52a-292">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7e52a-292">SameSiteMode.None</span></span>     | <span data-ttu-id="7e52a-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7e52a-293">SameSiteMode.None</span></span><br><span data-ttu-id="7e52a-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="7e52a-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="7e52a-296">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7e52a-296">SameSiteMode.None</span></span><br><span data-ttu-id="7e52a-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="7e52a-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7e52a-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-299">SameSiteMode.Lax</span></span>      | <span data-ttu-id="7e52a-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7e52a-300">SameSiteMode.None</span></span><br><span data-ttu-id="7e52a-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="7e52a-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="7e52a-303">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-303">SameSiteMode.Lax</span></span><br><span data-ttu-id="7e52a-304">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-304">SameSiteMode.Lax</span></span><br><span data-ttu-id="7e52a-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-305">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="7e52a-306">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-306">SameSiteMode.Strict</span></span>   | <span data-ttu-id="7e52a-307">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="7e52a-307">SameSiteMode.None</span></span><br><span data-ttu-id="7e52a-308">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="7e52a-308">SameSiteMode.Lax</span></span><br><span data-ttu-id="7e52a-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-309">SameSiteMode.Strict</span></span> | <span data-ttu-id="7e52a-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-310">SameSiteMode.Strict</span></span><br><span data-ttu-id="7e52a-311">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-311">SameSiteMode.Strict</span></span><br><span data-ttu-id="7e52a-312">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="7e52a-312">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="7e52a-313">建立驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="7e52a-313">Create an authentication cookie</span></span>

<span data-ttu-id="7e52a-314">若要建立保留使用者資訊的 cookie，您必須建構[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-314">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="7e52a-315">使用者資訊會序列化並儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="7e52a-315">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e52a-316">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-316">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7e52a-317">建立[ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)任何具有必要[宣告](/dotnet/api/system.security.claims.claim)s 和呼叫[Addtoroleasync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)使用者來登入：</span><span class="sxs-lookup"><span data-stu-id="7e52a-317">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e52a-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7e52a-319">呼叫[Addtoroleasync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)使用者來登入：</span><span class="sxs-lookup"><span data-stu-id="7e52a-319">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="7e52a-320">`SignInAsync` 建立加密的 cookie，並將它新增至目前回應。</span><span class="sxs-lookup"><span data-stu-id="7e52a-320">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="7e52a-321">如果您未指定`AuthenticationScheme`，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="7e52a-321">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="7e52a-322">實際上，使用的加密是 ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系統。</span><span class="sxs-lookup"><span data-stu-id="7e52a-322">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="7e52a-323">如果您裝載多部機器、 負載平衡的應用程式，或使用 web 伺服陣列上的應用程式，則您必須[設定資料保護](xref:security/data-protection/configuration/overview)使用相同的金鑰環及應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="7e52a-323">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="7e52a-324">登出</span><span class="sxs-lookup"><span data-stu-id="7e52a-324">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e52a-325">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-325">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7e52a-326">若要將目前的使用者登出並刪除其 cookie，呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="7e52a-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e52a-327">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-327">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7e52a-328">若要將目前的使用者登出並刪除其 cookie，呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="7e52a-328">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="7e52a-329">如果您未使用`CookieAuthenticationDefaults.AuthenticationScheme`（或"Cookie"） 做為配置 (例如，"ContosoCookie 」)，提供設定的驗證提供者時所使用的配置。</span><span class="sxs-lookup"><span data-stu-id="7e52a-329">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="7e52a-330">否則，會使用預設的配置。</span><span class="sxs-lookup"><span data-stu-id="7e52a-330">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="7e52a-331">回應後端的變更</span><span class="sxs-lookup"><span data-stu-id="7e52a-331">React to back-end changes</span></span>

<span data-ttu-id="7e52a-332">一旦建立 cookie，它會變成身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="7e52a-332">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="7e52a-333">即使您停用使用者，在您的後端系統中，cookie 驗證系統一無所知，與使用者保持登入，只要其 cookie 為有效。</span><span class="sxs-lookup"><span data-stu-id="7e52a-333">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="7e52a-334">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)事件，在 ASP.NET Core 2.x 或[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) 1.x 可用來攔截和覆寫 cookie 身分識別驗證的 ASP.NET Core 中的方法。</span><span class="sxs-lookup"><span data-stu-id="7e52a-334">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="7e52a-335">此方法可減輕存取應用程式的已撤銷使用者的風險。</span><span class="sxs-lookup"><span data-stu-id="7e52a-335">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="7e52a-336">Cookie 驗證的其中一個方法根據追蹤的使用者資料庫已變更時。</span><span class="sxs-lookup"><span data-stu-id="7e52a-336">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="7e52a-337">如果資料庫沒有已變更，因為使用者的 cookie 的發出，則不需要重新驗證使用者，如果其 cookie 仍然有效。</span><span class="sxs-lookup"><span data-stu-id="7e52a-337">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="7e52a-338">若要實作此案例中，資料庫中實作`IUserRepository`會針對此範例中，儲存`LastChanged`值。</span><span class="sxs-lookup"><span data-stu-id="7e52a-338">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="7e52a-339">當任何使用者會在資料庫中，更新`LastChanged`值設定為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-339">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="7e52a-340">若要以基礎資料庫變更時，使其失效的 cookie`LastChanged`值，請建立與 cookie`LastChanged`宣告包含目前`LastChanged`從資料庫的值：</span><span class="sxs-lookup"><span data-stu-id="7e52a-340">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e52a-341">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-341">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7e52a-342">若要實作的覆寫`ValidatePrincipal`事件，一種方法具有下列簽章，您可以從衍生類別中的寫入[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="7e52a-342">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="7e52a-343">範例看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="7e52a-343">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="7e52a-344">在 cookie 中的服務註冊期間註冊的事件執行個體`ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="7e52a-344">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="7e52a-345">提供已設定領域的服務註冊您`CustomCookieAuthenticationEvents`類別：</span><span class="sxs-lookup"><span data-stu-id="7e52a-345">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e52a-346">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-346">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7e52a-347">若要實作的覆寫`ValidateAsync`事件時，寫入具有下列簽章的方法：</span><span class="sxs-lookup"><span data-stu-id="7e52a-347">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="7e52a-348">ASP.NET Core 身分識別的一部分實作這項檢查其[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)。</span><span class="sxs-lookup"><span data-stu-id="7e52a-348">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="7e52a-349">範例看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="7e52a-349">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="7e52a-350">註冊的事件中的 cookie 驗證組態期間`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="7e52a-350">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="7e52a-351">更新的使用者名稱的情況下，請考慮&mdash;並不會影響以任何方式的安全性決策。</span><span class="sxs-lookup"><span data-stu-id="7e52a-351">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="7e52a-352">如果您想要非破壞性的方式更新使用者主體，呼叫`context.ReplacePrincipal`並設定`context.ShouldRenew`屬性設`true`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-352">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="7e52a-353">此處所述的方法是在每個要求時觸發。</span><span class="sxs-lookup"><span data-stu-id="7e52a-353">The approach described here is triggered on every request.</span></span> <span data-ttu-id="7e52a-354">這會導致應用程式對大量的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="7e52a-354">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="7e52a-355">永續性 cookie</span><span class="sxs-lookup"><span data-stu-id="7e52a-355">Persistent cookies</span></span>

<span data-ttu-id="7e52a-356">您可能想要在瀏覽器工作階段之間保存的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-356">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="7e52a-357">這個持續性，才應該啟用明確的使用者同意的情況的 「 記住我 」 核取方塊上登入或類似的機制。</span><span class="sxs-lookup"><span data-stu-id="7e52a-357">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="7e52a-358">下列程式碼片段會建立身分識別和對應的 cookie，透過瀏覽器結束時仍然有效。</span><span class="sxs-lookup"><span data-stu-id="7e52a-358">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="7e52a-359">會遵守任何先前設定的滑動逾期設定。</span><span class="sxs-lookup"><span data-stu-id="7e52a-359">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="7e52a-360">如果 cookie 已過期的瀏覽器關閉時，瀏覽器在重新啟動之後，就會清除的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-360">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e52a-361">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-361">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="7e52a-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)類別位於`Microsoft.AspNetCore.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e52a-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="7e52a-364">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)類別位於`Microsoft.AspNetCore.Http.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-364">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="7e52a-365">絕對的 cookie 到期日</span><span class="sxs-lookup"><span data-stu-id="7e52a-365">Absolute cookie expiration</span></span>

<span data-ttu-id="7e52a-366">您可以設定絕對到期時間與`ExpiresUtc`。</span><span class="sxs-lookup"><span data-stu-id="7e52a-366">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="7e52a-367">您也必須設定`IsPersistent`; 否則即為`ExpiresUtc`會被忽略，並建立單一工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="7e52a-367">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="7e52a-368">當`ExpiresUtc`上設定`SignInAsync`，它會覆寫的值`ExpireTimeSpan`選擇`CookieAuthenticationOptions`，如果設定。</span><span class="sxs-lookup"><span data-stu-id="7e52a-368">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="7e52a-369">下列程式碼片段會建立身分識別和對應的 cookie 可持續 20 分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="7e52a-369">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="7e52a-370">這會忽略任何先前設定的滑動逾期設定。</span><span class="sxs-lookup"><span data-stu-id="7e52a-370">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7e52a-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7e52a-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7e52a-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="additional-resources"></a><span data-ttu-id="7e52a-373">其他資源</span><span class="sxs-lookup"><span data-stu-id="7e52a-373">Additional resources</span></span>

* [<span data-ttu-id="7e52a-374">Auth 2.0 變更 / 移轉公告</span><span class="sxs-lookup"><span data-stu-id="7e52a-374">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="7e52a-375">以原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="7e52a-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
