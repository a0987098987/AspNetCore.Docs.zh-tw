---
title: Overview of ASP.NET Core Authentication
author: mjrousos
description: Learn about authentication in ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/04/2019
uid: security/authentication/index
ms.openlocfilehash: 40b2fb59b96486435a2ec0a7d69bee5ab4a814d2
ms.sourcegitcommit: 76d7fff62014c3db02564191ab768acea00f1b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2019
ms.locfileid: "74852710"
---
# <a name="overview-of-aspnet-core-authentication"></a><span data-ttu-id="2bb2b-103">Overview of ASP.NET Core authentication</span><span class="sxs-lookup"><span data-stu-id="2bb2b-103">Overview of ASP.NET Core authentication</span></span>

<span data-ttu-id="2bb2b-104">By [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="2bb2b-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="2bb2b-105">Authentication is the process of determining a user's identity.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-105">Authentication is the process of determining a user's identity.</span></span> <span data-ttu-id="2bb2b-106">[Authorization](xref:security/authorization/introduction) is the process of determining whether a user has access to a resource.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-106">[Authorization](xref:security/authorization/introduction) is the process of determining whether a user has access to a resource.</span></span> <span data-ttu-id="2bb2b-107">In ASP.NET Core, authentication is handled by the `IAuthenticationService`, which is used by authentication [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="2bb2b-107">In ASP.NET Core, authentication is handled by the `IAuthenticationService`, which is used by authentication [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="2bb2b-108">The authentication service uses registered authentication handlers to complete authentication-related actions.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-108">The authentication service uses registered authentication handlers to complete authentication-related actions.</span></span> <span data-ttu-id="2bb2b-109">Examples of authentication-related actions include:</span><span class="sxs-lookup"><span data-stu-id="2bb2b-109">Examples of authentication-related actions include:</span></span>

* <span data-ttu-id="2bb2b-110">Authenticating a user.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-110">Authenticating a user.</span></span>
* <span data-ttu-id="2bb2b-111">Responding when an unauthenticated user tries to access a restricted resource.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-111">Responding when an unauthenticated user tries to access a restricted resource.</span></span>

<span data-ttu-id="2bb2b-112">The registered authentication handlers and their configuration options are called "schemes".</span><span class="sxs-lookup"><span data-stu-id="2bb2b-112">The registered authentication handlers and their configuration options are called "schemes".</span></span>

<span data-ttu-id="2bb2b-113">Authentication schemes are specified by registering authentication services in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2bb2b-113">Authentication schemes are specified by registering authentication services in `Startup.ConfigureServices`:</span></span>

* <span data-ttu-id="2bb2b-114">By calling a scheme-specific extension method after a call to `services.AddAuthentication` (such as `AddJwtBearer` or `AddCookie`, for example).</span><span class="sxs-lookup"><span data-stu-id="2bb2b-114">By calling a scheme-specific extension method after a call to `services.AddAuthentication` (such as `AddJwtBearer` or `AddCookie`, for example).</span></span> <span data-ttu-id="2bb2b-115">這些擴充方法會使用[AuthenticationBuilder](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*)來註冊具有適當設定的配置。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-115">These extension methods use [AuthenticationBuilder.AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) to register schemes with appropriate settings.</span></span>
* <span data-ttu-id="2bb2b-116">較不常用，方法是直接呼叫[AuthenticationBuilder. AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) 。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-116">Less commonly, by calling [AuthenticationBuilder.AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) directly.</span></span>

<span data-ttu-id="2bb2b-117">例如，下列程式碼會註冊 cookie 和 JWT 持有人驗證配置的驗證服務和處理常式：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-117">For example, the following code registers authentication services and handlers for cookie and JWT bearer authentication schemes:</span></span>

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options => Configuration.Bind("JwtSettings", options))
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme, options => Configuration.Bind("CookieSettings", options));
```

<span data-ttu-id="2bb2b-118">`AddAuthentication` 參數 `JwtBearerDefaults.AuthenticationScheme` 是在不要求特定配置時，預設所要使用的配置名稱。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-118">The `AddAuthentication` parameter `JwtBearerDefaults.AuthenticationScheme` is the name of the scheme to use by default when a specific scheme isn't requested.</span></span>

<span data-ttu-id="2bb2b-119">如果使用多個配置，授權原則（或授權屬性）可以[指定它們所依賴的驗證配置（或配置）](xref:security/authorization/limitingidentitybyscheme)來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-119">If multiple schemes are used, authorization policies (or authorization attributes) can [specify the authentication scheme (or schemes)](xref:security/authorization/limitingidentitybyscheme) they depend on to authenticate the user.</span></span> <span data-ttu-id="2bb2b-120">在上述範例中，您可以藉由指定名稱來使用 cookie 驗證配置（`CookieAuthenticationDefaults.AuthenticationScheme` 預設為，雖然呼叫 `AddCookie`時，可以提供不同的名稱）。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-120">In the example above, the cookie authentication scheme could be used by specifying its name (`CookieAuthenticationDefaults.AuthenticationScheme` by default, though a different name could be provided when calling `AddCookie`).</span></span>

<span data-ttu-id="2bb2b-121">在某些情況下，`AddAuthentication` 的呼叫會由其他擴充方法自動進行。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-121">In some cases, the call to `AddAuthentication` is automatically made by other extension methods.</span></span> <span data-ttu-id="2bb2b-122">例如，使用 ASP.NET Core 身分[識別](xref:security/authentication/identity)時，會在內部呼叫 `AddAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-122">For example, when using [ASP.NET Core Identity](xref:security/authentication/identity), `AddAuthentication` is called internally.</span></span>

<span data-ttu-id="2bb2b-123">驗證中介軟體會在 `Startup.Configure` 中，藉由在應用程式的 `IApplicationBuilder`上呼叫 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 擴充方法來新增。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-123">The Authentication middleware is added in `Startup.Configure` by calling the <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> extension method on the app's `IApplicationBuilder`.</span></span> <span data-ttu-id="2bb2b-124">呼叫 `UseAuthentication` 會註冊使用先前註冊之驗證配置的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-124">Calling `UseAuthentication` registers the middleware which uses the previously registered authentication schemes.</span></span> <span data-ttu-id="2bb2b-125">在相依于要驗證之使用者的任何中介軟體之前，呼叫 `UseAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-125">Call `UseAuthentication` before any middleware that depends on users being authenticated.</span></span> <span data-ttu-id="2bb2b-126">使用端點路由時，`UseAuthentication` 的呼叫必須執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-126">When using endpoint routing, the call to `UseAuthentication` must go:</span></span>

* <span data-ttu-id="2bb2b-127">`UseRouting`之後，即可使用路由資訊來進行驗證決策。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-127">After `UseRouting`, so that route information is available for authentication decisions.</span></span>
* <span data-ttu-id="2bb2b-128">在 `UseEndpoints`之前，會先驗證使用者，然後再存取端點。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-128">Before `UseEndpoints`, so that users are authenticated before accessing the endpoints.</span></span>

## <a name="authentication-concepts"></a><span data-ttu-id="2bb2b-129">驗證概念</span><span class="sxs-lookup"><span data-stu-id="2bb2b-129">Authentication Concepts</span></span>

### <a name="authentication-scheme"></a><span data-ttu-id="2bb2b-130">驗證配置</span><span class="sxs-lookup"><span data-stu-id="2bb2b-130">Authentication scheme</span></span>

<span data-ttu-id="2bb2b-131">驗證配置是對應至的名稱：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-131">An authentication scheme is a name which corresponds to:</span></span>

* <span data-ttu-id="2bb2b-132">驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-132">An authentication handler.</span></span>
* <span data-ttu-id="2bb2b-133">用於設定該特定處理常式實例的選項。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-133">Options for configuring that specific instance of the handler.</span></span>

<span data-ttu-id="2bb2b-134">配置可做為一種機制，用來參考相關處理常式的驗證、挑戰和禁止行為。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-134">Schemes are useful as a mechanism for referring to the authentication, challenge, and forbid behaviors of the associated handler.</span></span> <span data-ttu-id="2bb2b-135">例如，授權原則可以使用配置名稱來指定應該使用哪一種驗證配置（或配置）來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-135">For example, an authorization policy can use scheme names to specify which authentication scheme (or schemes) should be used to authenticate the user.</span></span> <span data-ttu-id="2bb2b-136">設定驗證時，通常會指定預設的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-136">When configuring authentication, it's common to specify the default authentication scheme.</span></span> <span data-ttu-id="2bb2b-137">除非資源要求特定的配置，否則會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-137">The default scheme is used unless a resource requests a specific scheme.</span></span> <span data-ttu-id="2bb2b-138">也可以：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-138">It's also possible to:</span></span>

* <span data-ttu-id="2bb2b-139">指定要用於驗證、挑戰和禁止動作的不同預設配置。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-139">Specify different default schemes to use for authenticate, challenge, and forbid actions.</span></span>
* <span data-ttu-id="2bb2b-140">使用[原則](xref:security/authentication/policyschemes)配置，將多個配置結合成一個。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-140">Combine multiple schemes into one using [policy schemes](xref:security/authentication/policyschemes).</span></span>

### <a name="authentication-handler"></a><span data-ttu-id="2bb2b-141">驗證處理常式</span><span class="sxs-lookup"><span data-stu-id="2bb2b-141">Authentication handler</span></span>

<span data-ttu-id="2bb2b-142">驗證處理常式：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-142">An authentication handler:</span></span>

* <span data-ttu-id="2bb2b-143">是實作為配置行為的類型。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-143">Is a type that implements the behavior of a scheme.</span></span>
* <span data-ttu-id="2bb2b-144">衍生自 <xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler> 或 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-144">Is derived from <xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler> or <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span>
* <span data-ttu-id="2bb2b-145">主要負責驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-145">Has the primary responsibility to authenticate users.</span></span>

<span data-ttu-id="2bb2b-146">根據驗證配置的設定和連入要求內容，驗證處理常式：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-146">Based on the authentication scheme's configuration and the incoming request context, authentication handlers:</span></span>

* <span data-ttu-id="2bb2b-147">如果驗證成功，請建立代表使用者身分識別的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket> 物件。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-147">Construct <xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket> objects representing the user's identity if authentication is successful.</span></span>
* <span data-ttu-id="2bb2b-148">如果驗證失敗，則傳回「沒有結果」或「失敗」。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-148">Return 'no result' or 'failure' if authentication is unsuccessful.</span></span>
* <span data-ttu-id="2bb2b-149">在使用者嘗試存取資源時，有挑戰和禁止動作的方法：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-149">Have methods for challenge and forbid actions for when users attempt to access resources:</span></span>
  * <span data-ttu-id="2bb2b-150">他們未經授權存取（禁止）。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-150">They are unauthorized to access (forbid).</span></span>
  * <span data-ttu-id="2bb2b-151">未經驗證時（挑戰）。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-151">When they are unauthenticated (challenge).</span></span>

### <a name="authenticate"></a><span data-ttu-id="2bb2b-152">驗證</span><span class="sxs-lookup"><span data-stu-id="2bb2b-152">Authenticate</span></span>

<span data-ttu-id="2bb2b-153">驗證配置的驗證動作會負責根據要求內容來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-153">An authentication scheme's authenticate action is responsible for constructing the user's identity based on request context.</span></span> <span data-ttu-id="2bb2b-154">它會傳回 <xref:Microsoft.AspNetCore.Authentication.AuthenticateResult>，指出驗證是否成功，以及使用者在驗證票證中的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-154">It returns an <xref:Microsoft.AspNetCore.Authentication.AuthenticateResult> indicating whether authentication was successful and, if so, the user's identity in an authentication ticket.</span></span> <span data-ttu-id="2bb2b-155">請參閱`HttpContext.AuthenticateAsync`.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-155">See `HttpContext.AuthenticateAsync`.</span></span> <span data-ttu-id="2bb2b-156">驗證範例包括：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-156">Authenticate examples include:</span></span>

* <span data-ttu-id="2bb2b-157">Cookie 驗證配置會從 cookie 來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-157">A cookie authentication scheme constructing the user's identity from cookies.</span></span>
* <span data-ttu-id="2bb2b-158">JWT 持有人配置會還原序列化和驗證 JWT 持有人權杖，以建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-158">A JWT bearer scheme deserializing and validating a JWT bearer token to construct the user's identity.</span></span>

### <a name="challenge"></a><span data-ttu-id="2bb2b-159">挑戰</span><span class="sxs-lookup"><span data-stu-id="2bb2b-159">Challenge</span></span>

<span data-ttu-id="2bb2b-160">當未驗證的使用者要求需要驗證的端點時，授權就會叫用驗證挑戰。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-160">An authentication challenge is invoked by Authorization when an unauthenticated user requests an endpoint that requires authentication.</span></span> <span data-ttu-id="2bb2b-161">例如，當匿名使用者要求受限制的資源，或按一下登入連結時，就會發出驗證挑戰。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-161">An authentication challenge is issued, for example, when an anonymous user requests a restricted resource or clicks on a login link.</span></span> <span data-ttu-id="2bb2b-162">授權會使用指定的驗證配置，或預設值（如果未指定）來叫用挑戰。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-162">Authorization invokes a challenge using the specified authentication scheme(s), or the default if none is specified.</span></span> <span data-ttu-id="2bb2b-163">請參閱`HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-163">See `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="2bb2b-164">驗證挑戰範例包括：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-164">Authentication challenge examples include:</span></span>

* <span data-ttu-id="2bb2b-165">Cookie 驗證配置會將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-165">A cookie authentication scheme redirecting the user to a login page.</span></span>
* <span data-ttu-id="2bb2b-166">JWT 持有人配置會傳回具有 `www-authenticate: bearer` 標頭的401結果。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-166">A JWT bearer scheme returning a 401 result with a `www-authenticate: bearer` header.</span></span>

<span data-ttu-id="2bb2b-167">挑戰動作應讓使用者知道要使用哪種驗證機制來存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-167">A challenge action should let the user know what authentication mechanism to use to access the requested resource.</span></span>

### <a name="forbid"></a><span data-ttu-id="2bb2b-168">禁止</span><span class="sxs-lookup"><span data-stu-id="2bb2b-168">Forbid</span></span>

<span data-ttu-id="2bb2b-169">當已驗證的使用者嘗試存取不允許存取的資源時，授權會呼叫驗證配置的禁止動作。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-169">An authentication scheme's forbid action is called by Authorization when an authenticated user attempts to access a resource they are not permitted to access.</span></span> <span data-ttu-id="2bb2b-170">請參閱`HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="2bb2b-170">See `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="2bb2b-171">驗證禁止的範例包括：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-171">Authentication forbid examples include:</span></span>
* <span data-ttu-id="2bb2b-172">Cookie 驗證配置會將使用者重新導向至表示禁止存取的頁面。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-172">A cookie authentication scheme redirecting the user to a page indicating access was forbidden.</span></span>
* <span data-ttu-id="2bb2b-173">JWT 持有人配置傳回403結果。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-173">A JWT bearer scheme returning a 403 result.</span></span>
* <span data-ttu-id="2bb2b-174">自訂驗證配置會重新導向至頁面，讓使用者可以在其中要求資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-174">A custom authentication scheme redirecting to a page where the user can request access to the resource.</span></span>

<span data-ttu-id="2bb2b-175">禁止動作可讓使用者知道：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-175">A forbid action can let the user know:</span></span>

* <span data-ttu-id="2bb2b-176">它們會經過驗證。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-176">They are authenticated.</span></span>
* <span data-ttu-id="2bb2b-177">不允許他們存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-177">They aren't permitted to access the requested resource.</span></span>

<span data-ttu-id="2bb2b-178">請參閱下列連結，以瞭解挑戰與禁止的差異：</span><span class="sxs-lookup"><span data-stu-id="2bb2b-178">See the following links for differences between challenge and forbid:</span></span>

* <span data-ttu-id="2bb2b-179">[操作資源處理常式的挑戰和禁止](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler)。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-179">[Challenge and forbid with an operational resource handler](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler).</span></span>
* <span data-ttu-id="2bb2b-180">[挑戰與禁止之間的差異](xref:security/authorization/secure-data#challenge)。</span><span class="sxs-lookup"><span data-stu-id="2bb2b-180">[Differences between challenge and forbid](xref:security/authorization/secure-data#challenge).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bb2b-181">其他資源</span><span class="sxs-lookup"><span data-stu-id="2bb2b-181">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authentication/policyschemes>
* <xref:security/authorization/secure-data>
