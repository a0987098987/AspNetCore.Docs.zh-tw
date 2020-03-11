---
title: 使用不含 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 瞭解如何在不 ASP.NET Core 身分識別的情況下使用 cookie 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 64f881441a7a7f9a5529cb6ee5ce81142ccd69e6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662999"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="0a8ee-103">使用不含 ASP.NET Core 身分識別的 cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="0a8ee-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="0a8ee-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="0a8ee-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0a8ee-105">ASP.NET Core 身分識別是完整且功能完整的驗證提供者，可用於建立和維護登入。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="0a8ee-106">不過，不含 ASP.NET Core 身分識別的 cookie 型驗證提供者可以使用。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-106">However, a cookie-based authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="0a8ee-107">如需詳細資訊，請參閱<xref:security/authentication/identity>。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="0a8ee-108">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a8ee-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0a8ee-109">針對範例應用程式中的示範用途，假設使用者的使用者帳戶（Maria Rodriguez）已硬式編碼到應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="0a8ee-110">使用**電子郵件**位址 `maria.rodriguez@contoso.com` 和任何密碼來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="0a8ee-111">使用者會在*頁面/帳戶/登入. cshtml .cs*檔案的 `AuthenticateUser` 方法中進行驗證。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="0a8ee-112">在真實世界的範例中，使用者會針對資料庫進行驗證。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="0a8ee-113">組態</span><span class="sxs-lookup"><span data-stu-id="0a8ee-113">Configuration</span></span>

<span data-ttu-id="0a8ee-114">如果應用程式不使用 [Microsoft.AspNetCore 中繼套件](xref:fundamentals/metapackage-app)，請在Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)中建立 AspNetCore 的套件參考套件。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="0a8ee-115">在 `Startup.ConfigureServices` 方法中，使用 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 和 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> 方法來建立驗證中介軟體服務：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="0a8ee-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> 傳遞至 `AddAuthentication` 會設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="0a8ee-117">當有多個 cookie 驗證實例，而您想要以[特定的配置進行授權](xref:security/authorization/limitingidentitybyscheme)時，`AuthenticationScheme` 會很有用。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="0a8ee-118">將設定為 [CookieAuthenticationDefaults AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) 時，會為配置提供 "cookie" 的值。`AuthenticationScheme`</span><span class="sxs-lookup"><span data-stu-id="0a8ee-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="0a8ee-119">您可以提供可區分配置的任何字串值。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="0a8ee-120">應用程式的驗證配置與應用程式的 cookie 驗證配置不同。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="0a8ee-121">未提供 cookie 驗證配置給 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>時，會使用 `CookieAuthenticationDefaults.AuthenticationScheme` （「Cookie」）。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="0a8ee-122">根據預設，驗證 cookie 的 <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> 屬性會設定為 [`true`]。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="0a8ee-123">當網站訪客未同意資料收集時，允許驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="0a8ee-124">如需詳細資訊，請參閱<xref:security/gdpr#essential-cookies>。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="0a8ee-125">在 `Startup.Configure`中，呼叫 `UseAuthentication` 和 `UseAuthorization` 以設定 `HttpContext.User` 屬性，並針對要求執行授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="0a8ee-126">呼叫 `UseAuthentication` 並 `UseAuthorization` 方法，再呼叫 `UseEndpoints`：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="0a8ee-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> 類別是用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="0a8ee-128">在服務設定中，為 `Startup.ConfigureServices` 方法中的驗證設定 `CookieAuthenticationOptions`：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="0a8ee-129">Cookie 原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="0a8ee-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="0a8ee-130">[Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)可啟用 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="0a8ee-131">將中介軟體新增至應用程式處理管線會區分順序&mdash;它只會影響在管線中註冊的下游元件。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="0a8ee-132">使用提供給 Cookie 原則中介軟體的 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>，控制 cookie 處理的全域特性，並在附加或刪除 cookie 時連結至 cookie 處理處理常式。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="0a8ee-133">預設 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> 值是 `SameSiteMode.Lax` 以允許 OAuth2 authentication。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="0a8ee-134">若要嚴格地強制執行相同的網站原則 `SameSiteMode.Strict`，請設定 `MinimumSameSitePolicy`。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="0a8ee-135">雖然這項設定會中斷 OAuth2 和其他跨原始來源驗證配置，但它會提升不依賴跨原始來源要求處理之其他類型應用程式的 cookie 安全性層級。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="0a8ee-136">`MinimumSameSitePolicy` 的 Cookie 原則中介軟體設定，可能會根據下列矩陣，影響 `CookieAuthenticationOptions` 設定中 `Cookie.SameSite` 的設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="0a8ee-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="0a8ee-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="0a8ee-138">Cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="0a8ee-138">Cookie.SameSite</span></span> | <span data-ttu-id="0a8ee-139">結果 Cookie. SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="0a8ee-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="0a8ee-140">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-140">SameSiteMode.None</span></span>     | <span data-ttu-id="0a8ee-141">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-141">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-142">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-143">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="0a8ee-144">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-144">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-145">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-146">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="0a8ee-147">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="0a8ee-148">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-148">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-149">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-150">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="0a8ee-151">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-152">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-153">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="0a8ee-154">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="0a8ee-155">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-155">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-156">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-157">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="0a8ee-158">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="0a8ee-159">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="0a8ee-160">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="0a8ee-161">建立驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="0a8ee-161">Create an authentication cookie</span></span>

<span data-ttu-id="0a8ee-162">若要建立保留使用者資訊的 cookie，請建立 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="0a8ee-163">使用者資訊會序列化並儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="0a8ee-164">建立具有任何必要 <xref:System.Security.Claims.Claim>的 <xref:System.Security.Claims.ClaimsIdentity>，並呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> 以登入使用者：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="0a8ee-165">`SignInAsync` 會建立加密的 cookie，並將其新增至目前的回應。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="0a8ee-166">如果未指定 `AuthenticationScheme`，則會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="0a8ee-167">ASP.NET Core 的[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-167">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="0a8ee-168">若為裝載于多部電腦上的應用程式、跨應用程式的負載平衡，或使用 web 伺服陣列，請[將資料保護設定](xref:security/data-protection/configuration/overview)為使用相同的金鑰環形和應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-168">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="0a8ee-169">登出</span><span class="sxs-lookup"><span data-stu-id="0a8ee-169">Sign out</span></span>

<span data-ttu-id="0a8ee-170">若要登出目前的使用者並刪除其 cookie，請呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-170">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="0a8ee-171">如果 `CookieAuthenticationDefaults.AuthenticationScheme` （或「Cookie」）不是用來做為配置（例如，"ContosoCookie"），請提供設定驗證提供者時所使用的架構。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-171">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="0a8ee-172">否則，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-172">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="0a8ee-173">對後端變更做出回應</span><span class="sxs-lookup"><span data-stu-id="0a8ee-173">React to back-end changes</span></span>

<span data-ttu-id="0a8ee-174">一旦建立 cookie，cookie 就是身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-174">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="0a8ee-175">如果在後端系統中停用使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-175">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="0a8ee-176">應用程式的 cookie 驗證系統會繼續根據驗證 cookie 來處理要求。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-176">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="0a8ee-177">只要驗證 cookie 有效，使用者就會繼續登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-177">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="0a8ee-178"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> 事件可以用來攔截和覆寫 cookie 身分識別的驗證。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-178">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="0a8ee-179">在每個要求上驗證 cookie，可降低撤銷存取應用程式之使用者的風險。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-179">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="0a8ee-180">Cookie 驗證的一種方法是以追蹤使用者資料庫變更的時間為基礎。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-180">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="0a8ee-181">如果自使用者的 cookie 發行後，資料庫尚未變更，則不需要重新驗證使用者（如果其 cookie 仍然有效）。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-181">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="0a8ee-182">在範例應用程式中，資料庫會在 `IUserRepository` 中執行，並儲存 `LastChanged` 值。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-182">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="0a8ee-183">當資料庫中的使用者更新時，`LastChanged` 值會設為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-183">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="0a8ee-184">若要在資料庫根據 `LastChanged` 值變更時使 cookie 失效，請使用包含資料庫目前 `LastChanged` 值的 `LastChanged` 宣告來建立 cookie：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-184">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="0a8ee-185">若要為 `ValidatePrincipal` 事件執行覆寫，請在衍生自 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>的類別中，撰寫具有下列簽章的方法：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-185">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="0a8ee-186">以下是 `CookieAuthenticationEvents`的範例執行：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-186">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="0a8ee-187">在 `Startup.ConfigureServices` 方法中的 cookie 服務註冊期間註冊事件實例。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-187">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="0a8ee-188">為您的 `CustomCookieAuthenticationEvents` 類別提供[限定範圍的服務註冊](xref:fundamentals/dependency-injection#service-lifetimes)：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-188">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="0a8ee-189">考慮到使用者名稱更新的情況，&mdash;不會以任何方式影響安全性的決策。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-189">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="0a8ee-190">如果您想要以非破壞性的更新使用者主體，請呼叫 `context.ReplacePrincipal`，並將 `context.ShouldRenew` 屬性設定為 [`true`]。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-190">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="0a8ee-191">這裡所述的方法會在每個要求上觸發。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-191">The approach described here is triggered on every request.</span></span> <span data-ttu-id="0a8ee-192">在每個要求上驗證所有使用者的驗證 cookie，可能會導致應用程式的效能大幅下降。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-192">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="0a8ee-193">持續性 cookie</span><span class="sxs-lookup"><span data-stu-id="0a8ee-193">Persistent cookies</span></span>

<span data-ttu-id="0a8ee-194">您可能想要 cookie 跨瀏覽器會話保存。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-194">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="0a8ee-195">只有在登入或類似的機制上有 [記住我] 核取方塊的明確使用者同意，才能啟用此持續性。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-195">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="0a8ee-196">下列程式碼片段會建立可透過瀏覽器結束不受的身分識別和對應的 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-196">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="0a8ee-197">系統會接受先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-197">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="0a8ee-198">如果 cookie 在瀏覽器關閉時過期，瀏覽器會在重新開機後清除 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-198">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="0a8ee-199">將 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>中的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> 設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-199">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="0a8ee-200">絕對 cookie 到期日</span><span class="sxs-lookup"><span data-stu-id="0a8ee-200">Absolute cookie expiration</span></span>

<span data-ttu-id="0a8ee-201">絕對到期時間可以使用 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>來設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-201">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="0a8ee-202">若要建立持續性 cookie，也必須設定 `IsPersistent`。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-202">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="0a8ee-203">否則，會使用以會話為基礎的存留期來建立 cookie，而且可能會在它所保留的驗證票證之前或之後過期。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-203">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="0a8ee-204">設定 `ExpiresUtc` 時，它會覆寫 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>的 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> 選項值（如果已設定）。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-204">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="0a8ee-205">下列程式碼片段會建立持續20分鐘的身分識別和對應的 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-205">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="0a8ee-206">這會忽略先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-206">This ignores any sliding expiration settings previously configured.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="0a8ee-207">ASP.NET Core 身分識別是完整且功能完整的驗證提供者，可用於建立和維護登入。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-207">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="0a8ee-208">不過，您可以使用 cookie 型驗證驗證提供者，而不需要 ASP.NET Core 身分識別。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-208">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="0a8ee-209">如需詳細資訊，請參閱<xref:security/authentication/identity>。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-209">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="0a8ee-210">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a8ee-210">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0a8ee-211">針對範例應用程式中的示範用途，假設使用者的使用者帳戶（Maria Rodriguez）已硬式編碼到應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-211">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="0a8ee-212">使用**電子郵件**位址 `maria.rodriguez@contoso.com` 和任何密碼來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-212">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="0a8ee-213">使用者會在*頁面/帳戶/登入. cshtml .cs*檔案的 `AuthenticateUser` 方法中進行驗證。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-213">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="0a8ee-214">在真實世界的範例中，使用者會針對資料庫進行驗證。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-214">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="0a8ee-215">組態</span><span class="sxs-lookup"><span data-stu-id="0a8ee-215">Configuration</span></span>

<span data-ttu-id="0a8ee-216">如果應用程式不使用 [Microsoft.AspNetCore 中繼套件](xref:fundamentals/metapackage-app)，請在Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)中建立 AspNetCore 的套件參考套件。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-216">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="0a8ee-217">在 `Startup.ConfigureServices` 方法中，使用 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 和 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> 方法來建立驗證中介軟體服務：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-217">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="0a8ee-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> 傳遞至 `AddAuthentication` 會設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-218"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="0a8ee-219">當有多個 cookie 驗證實例，而您想要以[特定的配置進行授權](xref:security/authorization/limitingidentitybyscheme)時，`AuthenticationScheme` 會很有用。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-219">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="0a8ee-220">將設定為 [CookieAuthenticationDefaults AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) 時，會為配置提供 "cookie" 的值。`AuthenticationScheme`</span><span class="sxs-lookup"><span data-stu-id="0a8ee-220">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="0a8ee-221">您可以提供可區分配置的任何字串值。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-221">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="0a8ee-222">應用程式的驗證配置與應用程式的 cookie 驗證配置不同。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-222">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="0a8ee-223">未提供 cookie 驗證配置給 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>時，會使用 `CookieAuthenticationDefaults.AuthenticationScheme` （「Cookie」）。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-223">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="0a8ee-224">根據預設，驗證 cookie 的 <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> 屬性會設定為 [`true`]。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-224">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="0a8ee-225">當網站訪客未同意資料收集時，允許驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-225">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="0a8ee-226">如需詳細資訊，請參閱<xref:security/gdpr#essential-cookies>。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-226">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="0a8ee-227">在 `Startup.Configure` 方法中，呼叫 `UseAuthentication` 方法，以叫用設定 `HttpContext.User` 屬性的驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-227">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="0a8ee-228">呼叫 `UseAuthentication` 方法，再呼叫 `UseMvcWithDefaultRoute` 或 `UseMvc`：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-228">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="0a8ee-229"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> 類別是用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-229">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="0a8ee-230">在服務設定中，為 `Startup.ConfigureServices` 方法中的驗證設定 `CookieAuthenticationOptions`：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-230">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a><span data-ttu-id="0a8ee-231">Cookie 原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="0a8ee-231">Cookie Policy Middleware</span></span>

<span data-ttu-id="0a8ee-232">[Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)可啟用 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-232">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="0a8ee-233">將中介軟體新增至應用程式處理管線會區分順序&mdash;它只會影響在管線中註冊的下游元件。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-233">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="0a8ee-234">使用提供給 Cookie 原則中介軟體的 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>，控制 cookie 處理的全域特性，並在附加或刪除 cookie 時連結至 cookie 處理處理常式。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-234">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="0a8ee-235">預設 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> 值是 `SameSiteMode.Lax` 以允許 OAuth2 authentication。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-235">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="0a8ee-236">若要嚴格地強制執行相同的網站原則 `SameSiteMode.Strict`，請設定 `MinimumSameSitePolicy`。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-236">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="0a8ee-237">雖然這項設定會中斷 OAuth2 和其他跨原始來源驗證配置，但它會提升不依賴跨原始來源要求處理之其他類型應用程式的 cookie 安全性層級。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-237">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="0a8ee-238">`MinimumSameSitePolicy` 的 Cookie 原則中介軟體設定，可能會根據下列矩陣，影響 `CookieAuthenticationOptions` 設定中 `Cookie.SameSite` 的設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-238">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="0a8ee-239">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="0a8ee-239">MinimumSameSitePolicy</span></span> | <span data-ttu-id="0a8ee-240">Cookie. SameSite</span><span class="sxs-lookup"><span data-stu-id="0a8ee-240">Cookie.SameSite</span></span> | <span data-ttu-id="0a8ee-241">結果 Cookie. SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="0a8ee-241">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="0a8ee-242">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-242">SameSiteMode.None</span></span>     | <span data-ttu-id="0a8ee-243">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-243">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-244">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-244">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-245">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-245">SameSiteMode.Strict</span></span> | <span data-ttu-id="0a8ee-246">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-246">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-247">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-247">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-248">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-248">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="0a8ee-249">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-249">SameSiteMode.Lax</span></span>      | <span data-ttu-id="0a8ee-250">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-250">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-251">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-251">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-252">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-252">SameSiteMode.Strict</span></span> | <span data-ttu-id="0a8ee-253">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-254">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-254">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-255">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-255">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="0a8ee-256">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-256">SameSiteMode.Strict</span></span>   | <span data-ttu-id="0a8ee-257">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="0a8ee-257">SameSiteMode.None</span></span><br><span data-ttu-id="0a8ee-258">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="0a8ee-258">SameSiteMode.Lax</span></span><br><span data-ttu-id="0a8ee-259">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-259">SameSiteMode.Strict</span></span> | <span data-ttu-id="0a8ee-260">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-260">SameSiteMode.Strict</span></span><br><span data-ttu-id="0a8ee-261">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-261">SameSiteMode.Strict</span></span><br><span data-ttu-id="0a8ee-262">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="0a8ee-262">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-cookie"></a><span data-ttu-id="0a8ee-263">建立驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="0a8ee-263">Create an authentication cookie</span></span>

<span data-ttu-id="0a8ee-264">若要建立保留使用者資訊的 cookie，請建立 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-264">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="0a8ee-265">使用者資訊會序列化並儲存在 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-265">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="0a8ee-266">建立具有任何必要 <xref:System.Security.Claims.Claim>的 <xref:System.Security.Claims.ClaimsIdentity>，並呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> 以登入使用者：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-266">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="0a8ee-267">`SignInAsync` 會建立加密的 cookie，並將其新增至目前的回應。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-267">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="0a8ee-268">如果未指定 `AuthenticationScheme`，則會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-268">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="0a8ee-269">ASP.NET Core 的[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-269">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="0a8ee-270">若為裝載于多部電腦上的應用程式、跨應用程式的負載平衡，或使用 web 伺服陣列，請[將資料保護設定](xref:security/data-protection/configuration/overview)為使用相同的金鑰環形和應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-270">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="0a8ee-271">登出</span><span class="sxs-lookup"><span data-stu-id="0a8ee-271">Sign out</span></span>

<span data-ttu-id="0a8ee-272">若要登出目前的使用者並刪除其 cookie，請呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-272">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="0a8ee-273">如果 `CookieAuthenticationDefaults.AuthenticationScheme` （或「Cookie」）不是用來做為配置（例如，"ContosoCookie"），請提供設定驗證提供者時所使用的架構。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-273">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="0a8ee-274">否則，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-274">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="0a8ee-275">對後端變更做出回應</span><span class="sxs-lookup"><span data-stu-id="0a8ee-275">React to back-end changes</span></span>

<span data-ttu-id="0a8ee-276">一旦建立 cookie，cookie 就是身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-276">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="0a8ee-277">如果在後端系統中停用使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-277">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="0a8ee-278">應用程式的 cookie 驗證系統會繼續根據驗證 cookie 來處理要求。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-278">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="0a8ee-279">只要驗證 cookie 有效，使用者就會繼續登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-279">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="0a8ee-280"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> 事件可以用來攔截和覆寫 cookie 身分識別的驗證。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-280">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="0a8ee-281">在每個要求上驗證 cookie，可降低撤銷存取應用程式之使用者的風險。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-281">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="0a8ee-282">Cookie 驗證的一種方法是以追蹤使用者資料庫變更的時間為基礎。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-282">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="0a8ee-283">如果自使用者的 cookie 發行後，資料庫尚未變更，則不需要重新驗證使用者（如果其 cookie 仍然有效）。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-283">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="0a8ee-284">在範例應用程式中，資料庫會在 `IUserRepository` 中執行，並儲存 `LastChanged` 值。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-284">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="0a8ee-285">當資料庫中的使用者更新時，`LastChanged` 值會設為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-285">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="0a8ee-286">若要在資料庫根據 `LastChanged` 值變更時使 cookie 失效，請使用包含資料庫目前 `LastChanged` 值的 `LastChanged` 宣告來建立 cookie：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-286">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

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

<span data-ttu-id="0a8ee-287">若要為 `ValidatePrincipal` 事件執行覆寫，請在衍生自 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>的類別中，撰寫具有下列簽章的方法：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-287">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="0a8ee-288">以下是 `CookieAuthenticationEvents`的範例執行：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-288">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

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

<span data-ttu-id="0a8ee-289">在 `Startup.ConfigureServices` 方法中的 cookie 服務註冊期間註冊事件實例。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-289">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="0a8ee-290">為您的 `CustomCookieAuthenticationEvents` 類別提供[限定範圍的服務註冊](xref:fundamentals/dependency-injection#service-lifetimes)：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-290">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="0a8ee-291">考慮到使用者名稱更新的情況，&mdash;不會以任何方式影響安全性的決策。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-291">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="0a8ee-292">如果您想要以非破壞性的更新使用者主體，請呼叫 `context.ReplacePrincipal`，並將 `context.ShouldRenew` 屬性設定為 [`true`]。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-292">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="0a8ee-293">這裡所述的方法會在每個要求上觸發。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-293">The approach described here is triggered on every request.</span></span> <span data-ttu-id="0a8ee-294">在每個要求上驗證所有使用者的驗證 cookie，可能會導致應用程式的效能大幅下降。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-294">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="0a8ee-295">持續性 cookie</span><span class="sxs-lookup"><span data-stu-id="0a8ee-295">Persistent cookies</span></span>

<span data-ttu-id="0a8ee-296">您可能想要 cookie 跨瀏覽器會話保存。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-296">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="0a8ee-297">只有在登入或類似的機制上有 [記住我] 核取方塊的明確使用者同意，才能啟用此持續性。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-297">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="0a8ee-298">下列程式碼片段會建立可透過瀏覽器結束不受的身分識別和對應的 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-298">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="0a8ee-299">系統會接受先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-299">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="0a8ee-300">如果 cookie 在瀏覽器關閉時過期，瀏覽器會在重新開機後清除 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-300">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="0a8ee-301">將 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>中的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> 設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="0a8ee-301">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

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

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="0a8ee-302">絕對 cookie 到期日</span><span class="sxs-lookup"><span data-stu-id="0a8ee-302">Absolute cookie expiration</span></span>

<span data-ttu-id="0a8ee-303">絕對到期時間可以使用 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>來設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-303">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="0a8ee-304">若要建立持續性 cookie，也必須設定 `IsPersistent`。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-304">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="0a8ee-305">否則，會使用以會話為基礎的存留期來建立 cookie，而且可能會在它所保留的驗證票證之前或之後過期。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-305">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="0a8ee-306">設定 `ExpiresUtc` 時，它會覆寫 <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>的 <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> 選項值（如果已設定）。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-306">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="0a8ee-307">下列程式碼片段會建立持續20分鐘的身分識別和對應的 cookie。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-307">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="0a8ee-308">這會忽略先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="0a8ee-308">This ignores any sliding expiration settings previously configured.</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="0a8ee-309">其他資源</span><span class="sxs-lookup"><span data-stu-id="0a8ee-309">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="0a8ee-310">以原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="0a8ee-310">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
