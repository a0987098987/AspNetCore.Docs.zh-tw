---
title: 使用沒有 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 了解如何使用沒有 ASP.NET Core 身分識別的 cookie 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622737"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="78020-103">使用沒有 ASP.NET Core 身分識別的 cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="78020-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="78020-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="78020-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="78020-105">ASP.NET Core Identity 是完整且功能完整的驗證提供者來建立及維護的登入。</span><span class="sxs-lookup"><span data-stu-id="78020-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="78020-106">不過，您可以使用沒有 ASP.NET Core 身分識別的 cookie 為基礎的驗證驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="78020-106">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="78020-107">如需詳細資訊，請參閱 <xref:security/authentication/identity>。</span><span class="sxs-lookup"><span data-stu-id="78020-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="78020-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78020-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="78020-109">基於示範目的，範例應用程式中，假設使用者林麗莉 Rodriguez 的使用者帳戶會是硬式編碼到應用程式。</span><span class="sxs-lookup"><span data-stu-id="78020-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="78020-110">使用**電子郵件**使用者名稱`maria.rodriguez@contoso.com`和任何登入使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="78020-110">Use the **Email** user name `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="78020-111">使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="78020-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="78020-112">在真實世界範例中，您會驗證使用者，對資料庫。</span><span class="sxs-lookup"><span data-stu-id="78020-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="78020-113">組態</span><span class="sxs-lookup"><span data-stu-id="78020-113">Configuration</span></span>

<span data-ttu-id="78020-114">如果應用程式不會使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，建立的專案檔中的套件參考[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)封裝。</span><span class="sxs-lookup"><span data-stu-id="78020-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="78020-115">在 `Startup.ConfigureServices`方法，建立驗證中介軟體服務<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>和<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>方法：</span><span class="sxs-lookup"><span data-stu-id="78020-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="78020-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> 傳遞至`AddAuthentication`設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="78020-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="78020-117">`AuthenticationScheme` 當有多個執行個體的 cookie 驗證，而且您想要時非常有用[特定的結構描述的授權](xref:security/authorization/limitingidentitybyscheme)。</span><span class="sxs-lookup"><span data-stu-id="78020-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="78020-118">設定`AuthenticationScheme`要[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme)配置會提供 [Cookie] 的值。</span><span class="sxs-lookup"><span data-stu-id="78020-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="78020-119">您可以提供區分配置任何字串值。</span><span class="sxs-lookup"><span data-stu-id="78020-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="78020-120">應用程式的驗證配置與不同應用程式的 cookie 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="78020-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="78020-121">當 cookie 驗證配置不提供給<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>，它會使用`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie")。</span><span class="sxs-lookup"><span data-stu-id="78020-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="78020-122">驗證 cookie<xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential>屬性設定為`true`預設。</span><span class="sxs-lookup"><span data-stu-id="78020-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="78020-123">當網站訪客未同意資料收集時，允許驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="78020-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="78020-124">如需詳細資訊，請參閱 <xref:security/gdpr#essential-cookies>。</span><span class="sxs-lookup"><span data-stu-id="78020-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="78020-125">在 `Startup.Configure`方法中，呼叫`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="78020-125">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="78020-126">呼叫`UseAuthentication`方法之前呼叫`UseMvcWithDefaultRoute`或`UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="78020-126">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="78020-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>類別用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="78020-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="78020-128">設定`CookieAuthenticationOptions`中的驗證服務組態中`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="78020-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="78020-129">Cookie 原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="78020-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="78020-130">[Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)啟用 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="78020-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="78020-131">將中介軟體新增至應用程式處理管線是區分順序&mdash;它只會影響在管線中的已註冊的下游元件。</span><span class="sxs-lookup"><span data-stu-id="78020-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="78020-132">使用<xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>Cookie 原則中介軟體，來控制全域性質的 cookie 處理和攔截到 cookie 處理處理常式，當您附加或刪除 cookie 時提供。</span><span class="sxs-lookup"><span data-stu-id="78020-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="78020-133">預設值<xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy>值是`SameSiteMode.Lax`允許 OAuth2 驗證。</span><span class="sxs-lookup"><span data-stu-id="78020-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="78020-134">嚴格強制執行的相同站台原則`SameSiteMode.Strict`，將`MinimumSameSitePolicy`。</span><span class="sxs-lookup"><span data-stu-id="78020-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="78020-135">雖然 OAuth2 和其他跨原始來源的驗證配置，這項設定會中斷，它的權限提高 cookie 的用戶端進行跨原始來源要求處理的應用程式的其他類型的安全性層級。</span><span class="sxs-lookup"><span data-stu-id="78020-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="78020-136">Cookie 原則中介軟體設定`MinimumSameSitePolicy`可能會影響的設定`Cookie.SameSite`在`CookieAuthenticationOptions`根據下面的矩陣圖的設定。</span><span class="sxs-lookup"><span data-stu-id="78020-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="78020-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="78020-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="78020-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="78020-138">Cookie.SameSite</span></span> | <span data-ttu-id="78020-139">結果 Cookie.SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="78020-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="78020-140">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="78020-140">SameSiteMode.None</span></span>     | <span data-ttu-id="78020-141">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="78020-141">SameSiteMode.None</span></span><br><span data-ttu-id="78020-142">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="78020-143">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="78020-144">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="78020-144">SameSiteMode.None</span></span><br><span data-ttu-id="78020-145">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="78020-146">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="78020-147">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="78020-148">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="78020-148">SameSiteMode.None</span></span><br><span data-ttu-id="78020-149">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="78020-150">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="78020-151">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="78020-152">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="78020-153">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="78020-154">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="78020-155">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="78020-155">SameSiteMode.None</span></span><br><span data-ttu-id="78020-156">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="78020-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="78020-157">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="78020-158">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="78020-159">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="78020-160">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="78020-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="78020-161">建立驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="78020-161">Create an authentication cookie</span></span>

<span data-ttu-id="78020-162">若要建立 cookie 保留使用者資訊，請建構<xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="78020-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="78020-163">使用者資訊會序列化並儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="78020-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="78020-164">建立<xref:System.Security.Claims.ClaimsIdentity>任何具有必要<xref:System.Security.Claims.Claim>s 和呼叫<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>使用者來登入：</span><span class="sxs-lookup"><span data-stu-id="78020-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="78020-165">`SignInAsync` 建立加密的 cookie，並將它新增至目前回應。</span><span class="sxs-lookup"><span data-stu-id="78020-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="78020-166">如果`AuthenticationScheme`未指定，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="78020-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="78020-167">ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。</span><span class="sxs-lookup"><span data-stu-id="78020-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="78020-168">裝載多部電腦上的應用程式，負載平衡的應用程式，或使用 web 伺服陣列[設定資料保護](xref:security/data-protection/configuration/overview)使用相同的金鑰環及應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="78020-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="78020-169">登出</span><span class="sxs-lookup"><span data-stu-id="78020-169">Sign out</span></span>

<span data-ttu-id="78020-170">若要將目前的使用者登出並刪除其 cookie，呼叫<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span><span class="sxs-lookup"><span data-stu-id="78020-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="78020-171">如果`CookieAuthenticationDefaults.AuthenticationScheme`（或 [Cookie]） 不做為配置 (例如，"ContosoCookie 」)，提供設定的驗證提供者時所使用的配置。</span><span class="sxs-lookup"><span data-stu-id="78020-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="78020-172">否則，會使用預設的配置。</span><span class="sxs-lookup"><span data-stu-id="78020-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="78020-173">回應後端的變更</span><span class="sxs-lookup"><span data-stu-id="78020-173">React to back-end changes</span></span>

<span data-ttu-id="78020-174">一旦建立 cookie，cookie 就會是身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="78020-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="78020-175">如果使用者帳戶已停用後端系統中：</span><span class="sxs-lookup"><span data-stu-id="78020-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="78020-176">應用程式的 cookie 驗證系統會繼續處理要求的驗證 cookie 為基礎。</span><span class="sxs-lookup"><span data-stu-id="78020-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="78020-177">只要驗證 cookie 為有效，則使用者會保持登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="78020-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="78020-178"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*>事件可用來攔截和覆寫 cookie 身分識別的驗證。</span><span class="sxs-lookup"><span data-stu-id="78020-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="78020-179">驗證每個要求的 cookie，可降低已撤銷使用者存取應用程式的風險。</span><span class="sxs-lookup"><span data-stu-id="78020-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="78020-180">Cookie 驗證的其中一個方法根據追蹤的使用者資料庫變更時。</span><span class="sxs-lookup"><span data-stu-id="78020-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="78020-181">如果資料庫沒有已變更，因為使用者的 cookie 的發出，則不需要重新驗證使用者，如果其 cookie 仍然有效。</span><span class="sxs-lookup"><span data-stu-id="78020-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="78020-182">在範例應用程式中實作資料庫`IUserRepository`，並將儲存`LastChanged`值。</span><span class="sxs-lookup"><span data-stu-id="78020-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="78020-183">在資料庫中，更新使用者時`LastChanged`值設定為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="78020-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="78020-184">若要以基礎資料庫變更時，使其失效的 cookie`LastChanged`值，請建立與 cookie`LastChanged`宣告包含目前`LastChanged`從資料庫的值：</span><span class="sxs-lookup"><span data-stu-id="78020-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="78020-185">若要實作的覆寫`ValidatePrincipal`事件，一種方法具有下列簽章，衍生自的類別中的寫入<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span><span class="sxs-lookup"><span data-stu-id="78020-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="78020-186">以下是範例實作`CookieAuthenticationEvents`:</span><span class="sxs-lookup"><span data-stu-id="78020-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="78020-187">在 cookie 中的服務註冊期間註冊的事件執行個體`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="78020-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="78020-188">提供[範圍服務登錄](xref:fundamentals/dependency-injection#service-lifetimes)針對您`CustomCookieAuthenticationEvents`類別：</span><span class="sxs-lookup"><span data-stu-id="78020-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="78020-189">更新的使用者名稱的情況下，請考慮&mdash;並不會影響以任何方式的安全性決策。</span><span class="sxs-lookup"><span data-stu-id="78020-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="78020-190">如果您想要非破壞性的方式更新使用者主體，呼叫`context.ReplacePrincipal`並設定`context.ShouldRenew`屬性設`true`。</span><span class="sxs-lookup"><span data-stu-id="78020-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="78020-191">此處所述的方法是在每個要求時觸發。</span><span class="sxs-lookup"><span data-stu-id="78020-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="78020-192">驗證每個要求上的所有使用者的驗證 cookie，可能會導致應用程式對大量的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="78020-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="78020-193">永續性 cookie</span><span class="sxs-lookup"><span data-stu-id="78020-193">Persistent cookies</span></span>

<span data-ttu-id="78020-194">您可能想要在瀏覽器工作階段之間保存的 cookie。</span><span class="sxs-lookup"><span data-stu-id="78020-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="78020-195">這個持續性，才應該啟用明確的使用者同意的情況與 「 還記得我 」 核取方塊在登入或類似的機制。</span><span class="sxs-lookup"><span data-stu-id="78020-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="78020-196">下列程式碼片段會建立身分識別和對應的 cookie，透過瀏覽器結束時仍然有效。</span><span class="sxs-lookup"><span data-stu-id="78020-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="78020-197">會遵守任何先前設定的滑動逾期設定。</span><span class="sxs-lookup"><span data-stu-id="78020-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="78020-198">如果 cookie 已過期的瀏覽器關閉時，瀏覽器在重新啟動之後，就會清除的 cookie。</span><span class="sxs-lookup"><span data-stu-id="78020-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="78020-199">設定<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent>要`true`在<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span><span class="sxs-lookup"><span data-stu-id="78020-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="78020-200">絕對的 cookie 到期日</span><span class="sxs-lookup"><span data-stu-id="78020-200">Absolute cookie expiration</span></span>

<span data-ttu-id="78020-201">可以使用設定絕對到期時間<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>。</span><span class="sxs-lookup"><span data-stu-id="78020-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="78020-202">若要建立永續性 cookie，`IsPersistent`也必須設定。</span><span class="sxs-lookup"><span data-stu-id="78020-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="78020-203">否則，cookie 會建立具有工作階段為基礎的存留期，而且可能過期之前或之後，它會保存驗證票證。</span><span class="sxs-lookup"><span data-stu-id="78020-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="78020-204">當`ExpiresUtc`設定，它會覆寫的值<xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan>選擇<xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>，如果設定。</span><span class="sxs-lookup"><span data-stu-id="78020-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="78020-205">下列程式碼片段會建立身分識別和對應的 cookie 可持續 20 分鐘的時間。</span><span class="sxs-lookup"><span data-stu-id="78020-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="78020-206">這會忽略任何先前設定的滑動逾期設定。</span><span class="sxs-lookup"><span data-stu-id="78020-206">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

## <a name="additional-resources"></a><span data-ttu-id="78020-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="78020-207">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="78020-208">以原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="78020-208">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
