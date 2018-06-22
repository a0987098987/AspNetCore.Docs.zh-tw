---
title: 使用沒有 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 需使用沒有 ASP.NET Core 身分識別的 cookie 驗證的說明
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 2064e4d6406ce3ca3ce28732f113e8c81447aace
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275602"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="4d5a5-103">使用沒有 ASP.NET Core 身分識別的 cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="4d5a5-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="4d5a5-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d5a5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4d5a5-105">您在先前的驗證主題中，看到[ASP.NET Core 識別](xref:security/authentication/identity)是完整的功能完整的驗證提供者來建立及維護的登入。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="4d5a5-106">不過，您可以使用 cookie 架構驗證情況下使用您自己的自訂驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="4d5a5-107">您可以做為獨立驗證提供者，而 ASP.NET Core 識別不使用 cookie 為基礎的驗證。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="4d5a5-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d5a5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4d5a5-109">為了示範之用的範例應用程式中，假設使用者 Maria Rodriguez 的使用者帳戶是在應用程式的硬式編碼。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="4d5a5-110">使用電子郵件使用者名稱 」maria.rodriguez@contoso.com"和任何登入使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-110">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="4d5a5-111">使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="4d5a5-112">在真實世界範例中，您會驗證使用者，對資料庫。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-112">In a real-world example, the user would be authenticated against a database.</span></span>

<span data-ttu-id="4d5a5-113">如需有關移轉 cookie 為基礎的驗證，從 ASP.NET Core 詳細 1.x 為 2.0，請參閱[移轉的驗證和 ASP.NET Core 2.0 主題 （Cookie 架構驗證） 的身分識別](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-113">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrate Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

<span data-ttu-id="4d5a5-114">若要使用 ASP.NET Core 身分識別，請參閱[識別簡介](xref:security/authentication/identity)主題。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-114">To use ASP.NET Core Identity, see the [Introduction to Identity](xref:security/authentication/identity) topic.</span></span>

## <a name="configuration"></a><span data-ttu-id="4d5a5-115">組態</span><span class="sxs-lookup"><span data-stu-id="4d5a5-115">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d5a5-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="4d5a5-117">如果應用程式不會使用[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，在專案檔中建立封裝參考[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)封裝 (版本 2.1.0 或更新版本）。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-117">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package (version 2.1.0 or later).</span></span>

<span data-ttu-id="4d5a5-118">在`ConfigureServices`方法，建立驗證中介軟體服務與`AddAuthentication`和`AddCookie`方法：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-118">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="4d5a5-119">`AuthenticationScheme` 傳遞至`AddAuthentication`設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-119">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="4d5a5-120">`AuthenticationScheme` 有多個執行個體的驗證 cookie，而且您想要時相當實用[授權特定的結構描述與](xref:security/authorization/limitingidentitybyscheme)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-120">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="4d5a5-121">設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-121">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="4d5a5-122">您可以提供區別配置任何字串值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-122">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="4d5a5-123">在`Configure`方法，請使用`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-123">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="4d5a5-124">呼叫`UseAuthentication`方法之前先呼叫`UseMvcWithDefaultRoute`或`UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="4d5a5-124">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="4d5a5-125">**AddCookie 選項**</span><span class="sxs-lookup"><span data-stu-id="4d5a5-125">**AddCookie Options**</span></span>

<span data-ttu-id="4d5a5-126">[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)類別用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-126">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="4d5a5-127">選項</span><span class="sxs-lookup"><span data-stu-id="4d5a5-127">Option</span></span> | <span data-ttu-id="4d5a5-128">描述</span><span class="sxs-lookup"><span data-stu-id="4d5a5-128">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="4d5a5-129">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="4d5a5-129">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-130">提供找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ForbidAsync`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-130">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="4d5a5-131">預設值是 `/Account/AccessDenied`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-131">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="4d5a5-132">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="4d5a5-132">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-133">若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證服務所建立的任何宣告上的屬性。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-133">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="4d5a5-134">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="4d5a5-134">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-135">其中提供 cookie 的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-135">The domain name where the cookie is served.</span></span> <span data-ttu-id="4d5a5-136">根據預設，這是要求的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-136">By default, this is the host name of the request.</span></span> <span data-ttu-id="4d5a5-137">瀏覽器只在要求中，cookie 傳送相符的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-137">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="4d5a5-138">您可能想要調整這在您的網域中有任何主機可用的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-138">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="4d5a5-139">例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-139">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="4d5a5-140">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="4d5a5-140">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-141">取得或設定 cookie 的壽命。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-141">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="4d5a5-142">目前這個選項沒有 ops，就會變成過時中 ASP.NET Core 2.1 +。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-142">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="4d5a5-143">使用`ExpireTimeSpan`選項可用來設定的 cookie 到期。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-143">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="4d5a5-144">如需詳細資訊，請參閱[釐清 CookieAuthenticationOptions.Cookie.Expiration 行為](https://github.com/aspnet/Security/issues/1293)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-144">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="4d5a5-145">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="4d5a5-145">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-146">旗標，指出是否 cookie 應該只能由伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-146">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="4d5a5-147">變更此值為`false`允許存取 cookie 的用戶端指令碼，可能會用來開啟您的應用程式應該有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-147">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="4d5a5-148">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-148">The default value is `true`.</span></span> |
| [<span data-ttu-id="4d5a5-149">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="4d5a5-149">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-150">設定 cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-150">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="4d5a5-151">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="4d5a5-151">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-152">用來隔離在相同的主機名稱上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-152">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="4d5a5-153">如果您有在執行的應用程式`/app1`想要限制該應用程式的 cookie，請設定`CookiePath`屬性`/app1`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-153">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="4d5a5-154">如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-154">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="4d5a5-155">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="4d5a5-155">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-156">表示瀏覽器是否應該允許 cookie 附加至相同站台要求只 (`SameSiteMode.Strict`) 或跨網站要求使用安全的 HTTP 方法和相同站台要求 (`SameSiteMode.Lax`)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-156">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="4d5a5-157">當設定為`SameSiteMode.None`，未設定的 cookie 標頭值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-157">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="4d5a5-158">請注意， [Cookie 原則中介軟體](#cookie-policy-middleware)可能會覆寫您所提供的值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-158">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="4d5a5-159">若要支援 OAuth 驗證，預設值是`SameSiteMode.Lax`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-159">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="4d5a5-160">如需詳細資訊，請參閱[因為 SameSite cookie 原則而中斷的 OAuth 驗證](https://github.com/aspnet/Security/issues/1231)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-160">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="4d5a5-161">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="4d5a5-161">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-162">旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)、 HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或為要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-162">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="4d5a5-163">預設值是 `CookieSecurePolicy.SameAsRequest`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-163">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="4d5a5-164">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="4d5a5-164">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-165">設定`DataProtectionProvider`用來建立預設`TicketDataFormat`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-165">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="4d5a5-166">如果`TicketDataFormat`設定屬性，則`DataProtectionProvider`選項不會使用。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-166">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="4d5a5-167">如果未提供，則會使用應用程式的預設資料保護提供者。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-167">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="4d5a5-168">事件</span><span class="sxs-lookup"><span data-stu-id="4d5a5-168">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-169">此處理常式呼叫方法上提供的應用程式控制項在特定處理的點上的提供者。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-169">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="4d5a5-170">如果`Events`不提供的預設執行個體提供，呼叫的方法時，不做任何動作。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-170">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="4d5a5-171">EventsType</span><span class="sxs-lookup"><span data-stu-id="4d5a5-171">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-172">若要取得做為服務類型`Events`而不是屬性的執行個體。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-172">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="4d5a5-173">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="4d5a5-173">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-174">`TimeSpan`之後儲存在 cookie 內的驗證票證已過期。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-174">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="4d5a5-175">`ExpireTimeSpan` 會加入至目前的時間來建立票證的到期時間。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-175">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="4d5a5-176">`ExpiredTimeSpan`值一律會移到加密伺服器所驗證的 AuthTicket。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-176">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="4d5a5-177">它也可能會進入[Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)標頭，但是只有`IsPersistent`設定。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-177">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="4d5a5-178">若要設定`IsPersistent`至`true`，設定[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)傳遞至`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-178">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="4d5a5-179">預設值`ExpireTimeSpan`為 14 天。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-179">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="4d5a5-180">LoginPath</span><span class="sxs-lookup"><span data-stu-id="4d5a5-180">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-181">提供找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ChallengeAsync`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-181">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="4d5a5-182">產生 401 的目前 URL 加入至`LoginPath`所命名的查詢字串參數為`ReturnUrlParameter`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-182">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="4d5a5-183">一次要求`LoginPath`授與新的登入身分，`ReturnUrlParameter`值會用來將瀏覽器重新導向回到造成原始未授權的狀態碼的 URL。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-183">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="4d5a5-184">預設值是 `/Account/Login`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-184">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="4d5a5-185">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="4d5a5-185">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-186">如果`LogoutPath`提供，加入處理常式，則該路徑的要求重新導向的值為基礎`ReturnUrlParameter`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-186">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="4d5a5-187">預設值是 `/Account/Logout`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-187">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="4d5a5-188">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="4d5a5-188">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-189">決定後面 302 發現 （重新導向 URL） 回應的處理常式附加的查詢字串參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-189">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="4d5a5-190">`ReturnUrlParameter` 在要求抵達時，會使用`LoginPath`或`LogoutPath`執行登入或登出動作後，傳回瀏覽器的原始 url。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-190">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="4d5a5-191">預設值是 `ReturnUrl`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-191">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="4d5a5-192">SessionStore</span><span class="sxs-lookup"><span data-stu-id="4d5a5-192">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-193">選擇性的容器，用來儲存跨要求的身分識別。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-193">An optional container used to store identity across requests.</span></span> <span data-ttu-id="4d5a5-194">使用時，工作階段識別碼會傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-194">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="4d5a5-195">`SessionStore` 可用來降低大型的身分識別的潛在問題。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-195">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="4d5a5-196">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="4d5a5-196">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-197">旗標，指出是否要以動態方式發出新的更新的到期時間的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-197">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="4d5a5-198">這可能會發生在任何位置的目前 cookie 的逾期期限已超過 50%過期的要求。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-198">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="4d5a5-199">新的到期日會向前移至目前日期加上`ExpireTimespan`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-199">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="4d5a5-200">[絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-200">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="4d5a5-201">絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-201">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="4d5a5-202">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-202">The default value is `true`.</span></span> |
| [<span data-ttu-id="4d5a5-203">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="4d5a5-203">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-204">`TicketDataFormat`用來保護且取消保護身分識別，並且會儲存在 cookie 值中的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-204">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="4d5a5-205">如果未提供，`TicketDataFormat`使用建立[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-205">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="4d5a5-206">驗證</span><span class="sxs-lookup"><span data-stu-id="4d5a5-206">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="4d5a5-207">檢查的選項都有效的方法。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-207">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="4d5a5-208">設定`CookieAuthenticationOptions`中驗證的服務組態中`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-208">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d5a5-209">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-209">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="4d5a5-210">ASP.NET Core 1.x 使用 cookie[中介軟體](xref:fundamentals/middleware/index)，序列化為使用者主體已加密的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-210">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="4d5a5-211">在後續要求中，對 cookie 進行驗證，以及主體已重新建立並指派給`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-211">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="4d5a5-212">安裝[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)專案中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-212">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="4d5a5-213">此套件包含 cookie 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-213">This package contains the cookie middleware.</span></span>

<span data-ttu-id="4d5a5-214">使用`UseCookieAuthentication`方法中的`Configure`方法在您*Startup.cs*檔案之後，再`UseMvc`或`UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="4d5a5-214">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

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

<span data-ttu-id="4d5a5-215">**CookieAuthenticationOptions 選項**</span><span class="sxs-lookup"><span data-stu-id="4d5a5-215">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="4d5a5-216">[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)類別用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-216">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="4d5a5-217">選項</span><span class="sxs-lookup"><span data-stu-id="4d5a5-217">Option</span></span> | <span data-ttu-id="4d5a5-218">描述</span><span class="sxs-lookup"><span data-stu-id="4d5a5-218">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="4d5a5-219">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="4d5a5-219">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-220">設定驗證配置。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-220">Sets the authentication scheme.</span></span> <span data-ttu-id="4d5a5-221">`AuthenticationScheme` 有多個執行個體的驗證，而且您想要時相當實用[授權特定的結構描述與](xref:security/authorization/limitingidentitybyscheme)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-221">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="4d5a5-222">設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-222">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="4d5a5-223">您可以提供區別配置任何字串值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-223">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="4d5a5-224">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="4d5a5-224">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-225">設定值，指出應該執行每個要求的 cookie 驗證，並嘗試驗證，重新建構建立它的任何序列化的主體。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-225">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="4d5a5-226">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="4d5a5-226">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-227">如果為 true，驗證中介軟體會處理自動挑戰。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-227">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="4d5a5-228">如果為 false，驗證中介軟體僅改變回應明確指出時`AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-228">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="4d5a5-229">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="4d5a5-229">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-230">若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證中介軟體所建立的任何宣告上的屬性。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-230">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="4d5a5-231">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="4d5a5-231">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-232">其中提供 cookie 的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-232">The domain name where the cookie is served.</span></span> <span data-ttu-id="4d5a5-233">根據預設，這是要求的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-233">By default, this is the host name of the request.</span></span> <span data-ttu-id="4d5a5-234">瀏覽器，只是要比對的主機名稱的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-234">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="4d5a5-235">您可能想要調整這在您的網域中有任何主機可用的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-235">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="4d5a5-236">例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-236">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="4d5a5-237">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="4d5a5-237">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-238">旗標，指出是否 cookie 應該只能由伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-238">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="4d5a5-239">變更此值為`false`允許存取 cookie 的用戶端指令碼，可能會用來開啟您的應用程式應該有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-239">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="4d5a5-240">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-240">The default value is `true`.</span></span> |
| [<span data-ttu-id="4d5a5-241">CookiePath</span><span class="sxs-lookup"><span data-stu-id="4d5a5-241">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-242">用來隔離在相同的主機名稱上執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-242">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="4d5a5-243">如果您有在執行的應用程式`/app1`想要限制該應用程式的 cookie，請設定`CookiePath`屬性`/app1`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-243">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="4d5a5-244">如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-244">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="4d5a5-245">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="4d5a5-245">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-246">旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)、 HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或為要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-246">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="4d5a5-247">預設值是 `CookieSecurePolicy.SameAsRequest`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-247">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="4d5a5-248">描述</span><span class="sxs-lookup"><span data-stu-id="4d5a5-248">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-249">其他資訊才可以提供給應用程式的驗證類型的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-249">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="4d5a5-250">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="4d5a5-250">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-251">`TimeSpan`之後驗證票證已過期。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-251">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="4d5a5-252">它會加入到目前的時間來建立票證的到期時間。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-252">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="4d5a5-253">若要使用`ExpireTimeSpan`，您必須設定`IsPersistent`至`true`中`AuthenticationProperties`傳遞至`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-253">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="4d5a5-254">預設值為 14 天。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-254">The default value is 14 days.</span></span> |
| [<span data-ttu-id="4d5a5-255">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="4d5a5-255">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="4d5a5-256">旗標，指出是否 cookie 的到期日重設時超過一半的`ExpireTimeSpan`間隔。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-256">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="4d5a5-257">新的 exipiration 時間往前移動到目前日期加上`ExpireTimespan`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-257">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="4d5a5-258">[絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-258">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="4d5a5-259">絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-259">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="4d5a5-260">預設值是 `true`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-260">The default value is `true`.</span></span> |

<span data-ttu-id="4d5a5-261">設定`CookieAuthenticationOptions`Cookie 驗證中介軟體中`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-261">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="4d5a5-262">Cookie 的原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="4d5a5-262">Cookie Policy Middleware</span></span>

<span data-ttu-id="4d5a5-263">[Cookie 的原則中介軟體](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)可讓應用程式中的 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-263">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="4d5a5-264">將中介軟體新增到應用程式處理管線是順序機密;它只會影響註冊之後，管線中的元件。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-264">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="4d5a5-265">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) Cookie 原則中介軟體提供讓您控制 cookie 處理處理常式的全域性質的 cookie 處理和攔截 cookie 被附加或刪除時。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-265">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="4d5a5-266">屬性</span><span class="sxs-lookup"><span data-stu-id="4d5a5-266">Property</span></span> | <span data-ttu-id="4d5a5-267">描述</span><span class="sxs-lookup"><span data-stu-id="4d5a5-267">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="4d5a5-268">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="4d5a5-268">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="4d5a5-269">會影響是否 cookie 必須 HttpOnly，這是旗標，指出是否 cookie 應該只能由伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-269">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="4d5a5-270">預設值是 `HttpOnlyPolicy.None`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-270">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="4d5a5-271">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="4d5a5-271">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="4d5a5-272">會影響 cookie 的相同站台屬性 （請參閱下文）。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-272">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="4d5a5-273">預設值是 `SameSiteMode.Lax`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-273">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="4d5a5-274">此選項適用於 ASP.NET Core 2.0 +。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-274">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="4d5a5-275">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="4d5a5-275">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="4d5a5-276">Cookie 會附加時呼叫。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-276">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="4d5a5-277">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="4d5a5-277">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="4d5a5-278">刪除 cookie 時呼叫。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-278">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="4d5a5-279">安全</span><span class="sxs-lookup"><span data-stu-id="4d5a5-279">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="4d5a5-280">會影響是否 cookie 必須是安全的。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-280">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="4d5a5-281">預設值是 `CookieSecurePolicy.None`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-281">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="4d5a5-282">**MinimumSameSitePolicy** （ASP.NET 2.0 + 僅限核心）</span><span class="sxs-lookup"><span data-stu-id="4d5a5-282">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="4d5a5-283">預設值`MinimumSameSitePolicy`值是`SameSiteMode.Lax`允許 OAuth2 驗證。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-283">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="4d5a5-284">嚴格地強制執行的相同站台原則`SameSiteMode.Strict`，將`MinimumSameSitePolicy`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-284">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="4d5a5-285">雖然這項設定中斷 OAuth2 和其他的跨原始驗證配置時，它會模仿不要依賴跨原始要求處理的應用程式的其他類型的 cookie 安全性層級。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-285">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="4d5a5-286">Cookie 的原則中介軟體設定`MinimumSameSitePolicy`可能會影響您設定`Cookie.SameSite`中`CookieAuthenticationOptions`根據矩陣下方的設定。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-286">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="4d5a5-287">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="4d5a5-287">MinimumSameSitePolicy</span></span> | <span data-ttu-id="4d5a5-288">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="4d5a5-288">Cookie.SameSite</span></span> | <span data-ttu-id="4d5a5-289">結果 Cookie.SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="4d5a5-289">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="4d5a5-290">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4d5a5-290">SameSiteMode.None</span></span>     | <span data-ttu-id="4d5a5-291">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4d5a5-291">SameSiteMode.None</span></span><br><span data-ttu-id="4d5a5-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-292">SameSiteMode.Lax</span></span><br><span data-ttu-id="4d5a5-293">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-293">SameSiteMode.Strict</span></span> | <span data-ttu-id="4d5a5-294">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4d5a5-294">SameSiteMode.None</span></span><br><span data-ttu-id="4d5a5-295">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-295">SameSiteMode.Lax</span></span><br><span data-ttu-id="4d5a5-296">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-296">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4d5a5-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-297">SameSiteMode.Lax</span></span>      | <span data-ttu-id="4d5a5-298">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4d5a5-298">SameSiteMode.None</span></span><br><span data-ttu-id="4d5a5-299">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-299">SameSiteMode.Lax</span></span><br><span data-ttu-id="4d5a5-300">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-300">SameSiteMode.Strict</span></span> | <span data-ttu-id="4d5a5-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="4d5a5-302">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-302">SameSiteMode.Lax</span></span><br><span data-ttu-id="4d5a5-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-303">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="4d5a5-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-304">SameSiteMode.Strict</span></span>   | <span data-ttu-id="4d5a5-305">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="4d5a5-305">SameSiteMode.None</span></span><br><span data-ttu-id="4d5a5-306">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="4d5a5-306">SameSiteMode.Lax</span></span><br><span data-ttu-id="4d5a5-307">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-307">SameSiteMode.Strict</span></span> | <span data-ttu-id="4d5a5-308">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-308">SameSiteMode.Strict</span></span><br><span data-ttu-id="4d5a5-309">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-309">SameSiteMode.Strict</span></span><br><span data-ttu-id="4d5a5-310">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="4d5a5-310">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="4d5a5-311">建立驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="4d5a5-311">Create an authentication cookie</span></span>

<span data-ttu-id="4d5a5-312">若要建立的保留使用者資訊的 cookie，您必須建構[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-312">To create a cookie holding user information, you must construct a [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="4d5a5-313">使用者資訊會序列化並儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-313">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d5a5-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="4d5a5-315">建立[ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)與任何所需[宣告](/dotnet/api/system.security.claims.claim)s 和呼叫[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)登入使用者：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-315">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d5a5-316">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-316">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="4d5a5-317">呼叫[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)登入使用者：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-317">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="4d5a5-318">`SignInAsync` 建立加密的 cookie，並將它加入至目前回應。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-318">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="4d5a5-319">如果您未指定`AuthenticationScheme`，則會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-319">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="4d5a5-320">實際上，使用的加密是 ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系統。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-320">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="4d5a5-321">如果您裝載的多部電腦、 負載平衡的應用程式，或使用 web 伺服陣列上的應用程式，則您必須[設定資料保護](xref:security/data-protection/configuration/overview)使用相同的索引鍵環形和應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-321">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="4d5a5-322">登出</span><span class="sxs-lookup"><span data-stu-id="4d5a5-322">Sign out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d5a5-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="4d5a5-324">若要登出目前的使用者，並刪除其 cookie，請呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="4d5a5-324">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d5a5-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="4d5a5-326">若要登出目前的使用者，並刪除其 cookie，請呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="4d5a5-326">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="4d5a5-327">如果您不使用`CookieAuthenticationDefaults.AuthenticationScheme`（也"稱為 Cookie"） 做為配置 (例如，"ContosoCookie")，提供設定的驗證提供者時所使用的配置。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-327">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="4d5a5-328">否則，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-328">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="4d5a5-329">回應端的變更</span><span class="sxs-lookup"><span data-stu-id="4d5a5-329">React to back-end changes</span></span>

<span data-ttu-id="4d5a5-330">一旦建立 cookie 時，它會變成身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-330">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="4d5a5-331">即使您停用後端系統中的使用者，cookie 驗證系統並不了解，以及使用者保持登入，只要其 cookie 為有效。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-331">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="4d5a5-332">[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)中 ASP.NET Core 事件 2.x 或[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1)中 ASP.NET Core 1.x 可以用來攔截並覆寫 cookie 身分識別的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-332">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="4d5a5-333">這種方法可減少已撤銷使用者存取應用程式的風險。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-333">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="4d5a5-334">驗證 cookie 的其中一個方法根據追蹤的使用者資料庫已變更時。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-334">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="4d5a5-335">如果資料庫沒有已變更，因為使用者的 cookie 發出，但是沒有需要重新驗證使用者，其 cookie 是否仍然有效。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-335">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="4d5a5-336">若要實作此案例中，資料庫中，這實作於`IUserRepository`會針對此範例中，儲存`LastChanged`值。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-336">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="4d5a5-337">在資料庫中，更新任何使用者時`LastChanged`值設定為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-337">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="4d5a5-338">若要讓 cookie 失效時的資料庫變更為基礎`LastChanged`值，請建立與 cookie`LastChanged`宣告包含目前`LastChanged`從資料庫的值：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-338">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d5a5-339">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-339">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4d5a5-340">若要實作的覆寫`ValidatePrincipal`事件，將您從衍生類別中的下列簽章的方法寫入[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="4d5a5-340">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="4d5a5-341">範例如下所示：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-341">An example looks like the following:</span></span>

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

<span data-ttu-id="4d5a5-342">在 cookie 中的服務登錄期間註冊事件執行個體`ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-342">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="4d5a5-343">提供已設定領域的服務註冊您`CustomCookieAuthenticationEvents`類別：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-343">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d5a5-344">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-344">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4d5a5-345">若要實作的覆寫`ValidateAsync`事件，將具有下列簽章的方法寫入：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-345">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="4d5a5-346">ASP.NET Core 身分識別的一部分實作這項檢查其[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-346">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="4d5a5-347">範例如下所示：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-347">An example looks like the following:</span></span>

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

<span data-ttu-id="4d5a5-348">在 cookie 驗證組態設定期間註冊事件`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="4d5a5-348">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

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

<span data-ttu-id="4d5a5-349">請考慮在使用者的名稱更新的情況下&mdash;並不會影響安全性，以任何方式的決策。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-349">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="4d5a5-350">如果您想要非破壞性的方式更新使用者的主體，呼叫`context.ReplacePrincipal`並設定`context.ShouldRenew`屬性`true`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-350">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="4d5a5-351">此處所述的方法是在每個要求觸發的。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-351">The approach described here is triggered on every request.</span></span> <span data-ttu-id="4d5a5-352">這會導致應用程式對效能帶來負面影響。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-352">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="4d5a5-353">永續性 cookie</span><span class="sxs-lookup"><span data-stu-id="4d5a5-353">Persistent cookies</span></span>

<span data-ttu-id="4d5a5-354">您可能想要在瀏覽器工作階段之間保存的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-354">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="4d5a5-355">此持續性，才應該啟用明確的使用者同意 「 記住我 核取方塊上登入或類似的機制。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-355">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="4d5a5-356">下列程式碼片段會建立身分識別和對應未透過瀏覽器封閉區段的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-356">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="4d5a5-357">會接受任何先前設定的滑動逾期設定。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-357">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="4d5a5-358">如果 cookie 過期時就會關閉瀏覽器，瀏覽器之後重新啟動時，就會清除 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-358">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d5a5-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="4d5a5-360">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)類別位於`Microsoft.AspNetCore.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-360">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d5a5-361">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-361">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="4d5a5-362">[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)類別位於`Microsoft.AspNetCore.Http.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-362">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="4d5a5-363">絕對的 cookie 到期</span><span class="sxs-lookup"><span data-stu-id="4d5a5-363">Absolute cookie expiration</span></span>

<span data-ttu-id="4d5a5-364">您可以設定與絕對到期時間`ExpiresUtc`。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-364">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="4d5a5-365">您也必須設定`IsPersistent`，否則`ExpiresUtc`會被忽略，並建立單一工作階段 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-365">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="4d5a5-366">當`ExpiresUtc`上設定`SignInAsync`，它會覆寫的值`ExpireTimeSpan`選項`CookieAuthenticationOptions`，如果設定。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-366">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="4d5a5-367">下列程式碼片段會建立身分識別和對應的 cookie 會持續 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-367">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="4d5a5-368">這會忽略任何先前設定的滑動逾期設定。</span><span class="sxs-lookup"><span data-stu-id="4d5a5-368">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d5a5-369">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-369">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d5a5-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d5a5-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

## <a name="additional-resources"></a><span data-ttu-id="4d5a5-371">其他資源</span><span class="sxs-lookup"><span data-stu-id="4d5a5-371">Additional resources</span></span>

* [<span data-ttu-id="4d5a5-372">Auth 2.0 變更 / 移轉公告</span><span class="sxs-lookup"><span data-stu-id="4d5a5-372">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="4d5a5-373">以配置限制身分識別</span><span class="sxs-lookup"><span data-stu-id="4d5a5-373">Limit identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="4d5a5-374">宣告式授權</span><span class="sxs-lookup"><span data-stu-id="4d5a5-374">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="4d5a5-375">以原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="4d5a5-375">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
