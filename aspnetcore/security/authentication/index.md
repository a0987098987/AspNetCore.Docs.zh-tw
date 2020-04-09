---
title: ASP.NET核心身份驗證概述
author: mjrousos
description: 瞭解ASP.NET核心中的身份驗證。
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2020
uid: security/authentication/index
ms.openlocfilehash: 404904ecfa30d1fe7e47f0daaa423ddd6f1b06e8
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79434326"
---
# <a name="overview-of-aspnet-core-authentication"></a><span data-ttu-id="b70e8-103">ASP.NET核心身份驗證概述</span><span class="sxs-lookup"><span data-stu-id="b70e8-103">Overview of ASP.NET Core authentication</span></span>

<span data-ttu-id="b70e8-104">由[邁克·盧梭斯](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="b70e8-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="b70e8-105">身份驗證是確定使用者身份的過程。</span><span class="sxs-lookup"><span data-stu-id="b70e8-105">Authentication is the process of determining a user's identity.</span></span> <span data-ttu-id="b70e8-106">[授權](xref:security/authorization/introduction)是確定使用者是否有權訪問資源的過程。</span><span class="sxs-lookup"><span data-stu-id="b70e8-106">[Authorization](xref:security/authorization/introduction) is the process of determining whether a user has access to a resource.</span></span> <span data-ttu-id="b70e8-107">在ASP.NET核心中,身份驗證由`IAuthenticationService`由身份驗證[中間件](xref:fundamentals/middleware/index)使用。</span><span class="sxs-lookup"><span data-stu-id="b70e8-107">In ASP.NET Core, authentication is handled by the `IAuthenticationService`, which is used by authentication [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="b70e8-108">身份驗證服務使用已註冊的身份驗證處理程式完成與身份驗證相關的操作。</span><span class="sxs-lookup"><span data-stu-id="b70e8-108">The authentication service uses registered authentication handlers to complete authentication-related actions.</span></span> <span data-ttu-id="b70e8-109">與認證相關的操作的範例包括:</span><span class="sxs-lookup"><span data-stu-id="b70e8-109">Examples of authentication-related actions include:</span></span>

* <span data-ttu-id="b70e8-110">驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="b70e8-110">Authenticating a user.</span></span>
* <span data-ttu-id="b70e8-111">當未經身份驗證的用戶嘗試訪問受限資源時回應。</span><span class="sxs-lookup"><span data-stu-id="b70e8-111">Responding when an unauthenticated user tries to access a restricted resource.</span></span>

<span data-ttu-id="b70e8-112">註冊的身份驗證處理程式及其配置選項稱為"方案"。</span><span class="sxs-lookup"><span data-stu-id="b70e8-112">The registered authentication handlers and their configuration options are called "schemes".</span></span>

<span data-ttu-id="b70e8-113">認證專案通過在 中`Startup.ConfigureServices`註冊身份驗證服務來指定:</span><span class="sxs-lookup"><span data-stu-id="b70e8-113">Authentication schemes are specified by registering authentication services in `Startup.ConfigureServices`:</span></span>

* <span data-ttu-id="b70e8-114">以在呼叫後呼叫特定於機制的延伸方法`services.AddAuthentication`(`AddJwtBearer`例如`AddCookie`, 或 。</span><span class="sxs-lookup"><span data-stu-id="b70e8-114">By calling a scheme-specific extension method after a call to `services.AddAuthentication` (such as `AddJwtBearer` or `AddCookie`, for example).</span></span> <span data-ttu-id="b70e8-115">這些擴充方法使用[身份驗證生成器.AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*)註冊具有適當設置的方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-115">These extension methods use [AuthenticationBuilder.AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) to register schemes with appropriate settings.</span></span>
* <span data-ttu-id="b70e8-116">不太常見,通過直接調用[身份驗證生成器.AddScheme。](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*)</span><span class="sxs-lookup"><span data-stu-id="b70e8-116">Less commonly, by calling [AuthenticationBuilder.AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) directly.</span></span>

<span data-ttu-id="b70e8-117">例如,以下代碼註冊 Cookie 和 JWT 承載身份驗證方案的身份驗證服務和處理程式:</span><span class="sxs-lookup"><span data-stu-id="b70e8-117">For example, the following code registers authentication services and handlers for cookie and JWT bearer authentication schemes:</span></span>

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options => Configuration.Bind("JwtSettings", options))
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme, options => Configuration.Bind("CookieSettings", options));
```

<span data-ttu-id="b70e8-118">參數`AddAuthentication``JwtBearerDefaults.AuthenticationScheme`是默認情況下在未請求特定方案時要使用的方案的名稱。</span><span class="sxs-lookup"><span data-stu-id="b70e8-118">The `AddAuthentication` parameter `JwtBearerDefaults.AuthenticationScheme` is the name of the scheme to use by default when a specific scheme isn't requested.</span></span>

<span data-ttu-id="b70e8-119">如果使用多個方案,授權策略(或授權屬性)可以指定它們所依賴的[身份驗證方案(或方案)](xref:security/authorization/limitingidentitybyscheme)以對使用者進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="b70e8-119">If multiple schemes are used, authorization policies (or authorization attributes) can [specify the authentication scheme (or schemes)](xref:security/authorization/limitingidentitybyscheme) they depend on to authenticate the user.</span></span> <span data-ttu-id="b70e8-120">在上面的示例中,Cookie 身份驗證方案可以通過指定其`CookieAuthenticationDefaults.AuthenticationScheme`名稱( 預設情況下,儘管`AddCookie`在調用 時可能提供不同名稱)來使用。</span><span class="sxs-lookup"><span data-stu-id="b70e8-120">In the example above, the cookie authentication scheme could be used by specifying its name (`CookieAuthenticationDefaults.AuthenticationScheme` by default, though a different name could be provided when calling `AddCookie`).</span></span>

<span data-ttu-id="b70e8-121">在某些情況下,調用`AddAuthentication`是由其他擴展方法自動進行的。</span><span class="sxs-lookup"><span data-stu-id="b70e8-121">In some cases, the call to `AddAuthentication` is automatically made by other extension methods.</span></span> <span data-ttu-id="b70e8-122">例如,在使用[ASP.NET 核心標識](xref:security/authentication/identity)時,`AddAuthentication`在內部調用。</span><span class="sxs-lookup"><span data-stu-id="b70e8-122">For example, when using [ASP.NET Core Identity](xref:security/authentication/identity), `AddAuthentication` is called internally.</span></span>

<span data-ttu-id="b70e8-123">透過在應用程式的 呼叫`Startup.Configure`裝置,<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>將新增身份認證的`IApplicationBuilder`中間件 。</span><span class="sxs-lookup"><span data-stu-id="b70e8-123">The Authentication middleware is added in `Startup.Configure` by calling the <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> extension method on the app's `IApplicationBuilder`.</span></span> <span data-ttu-id="b70e8-124">調用`UseAuthentication`註冊使用以前註冊的身份驗證方案的中間件。</span><span class="sxs-lookup"><span data-stu-id="b70e8-124">Calling `UseAuthentication` registers the middleware which uses the previously registered authentication schemes.</span></span> <span data-ttu-id="b70e8-125">在`UseAuthentication`依賴於使用者經過身份驗證的任何中間件之前調用。</span><span class="sxs-lookup"><span data-stu-id="b70e8-125">Call `UseAuthentication` before any middleware that depends on users being authenticated.</span></span> <span data-ttu-id="b70e8-126">使用終結點路由時,調用`UseAuthentication`必須轉到:</span><span class="sxs-lookup"><span data-stu-id="b70e8-126">When using endpoint routing, the call to `UseAuthentication` must go:</span></span>

* <span data-ttu-id="b70e8-127">之後`UseRouting`,以便路由資訊可用於身份驗證決策。</span><span class="sxs-lookup"><span data-stu-id="b70e8-127">After `UseRouting`, so that route information is available for authentication decisions.</span></span>
* <span data-ttu-id="b70e8-128">在`UseEndpoints`之前,以便在訪問終結點之前對用戶進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="b70e8-128">Before `UseEndpoints`, so that users are authenticated before accessing the endpoints.</span></span>

## <a name="authentication-concepts"></a><span data-ttu-id="b70e8-129">驗證概念</span><span class="sxs-lookup"><span data-stu-id="b70e8-129">Authentication Concepts</span></span>

### <a name="authentication-scheme"></a><span data-ttu-id="b70e8-130">驗證配置</span><span class="sxs-lookup"><span data-stu-id="b70e8-130">Authentication scheme</span></span>

<span data-ttu-id="b70e8-131">認證專案是對應於:</span><span class="sxs-lookup"><span data-stu-id="b70e8-131">An authentication scheme is a name which corresponds to:</span></span>

* <span data-ttu-id="b70e8-132">驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="b70e8-132">An authentication handler.</span></span>
* <span data-ttu-id="b70e8-133">用於配置處理程式的特定實例的選項。</span><span class="sxs-lookup"><span data-stu-id="b70e8-133">Options for configuring that specific instance of the handler.</span></span>

<span data-ttu-id="b70e8-134">方案作為引用關聯處理程序的身份驗證、質詢和禁止行為的機制非常有用。</span><span class="sxs-lookup"><span data-stu-id="b70e8-134">Schemes are useful as a mechanism for referring to the authentication, challenge, and forbid behaviors of the associated handler.</span></span> <span data-ttu-id="b70e8-135">例如,授權策略可以使用方案名稱來指定應使用哪些身份驗證方案(或方案)對使用者進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="b70e8-135">For example, an authorization policy can use scheme names to specify which authentication scheme (or schemes) should be used to authenticate the user.</span></span> <span data-ttu-id="b70e8-136">配置身份驗證時,通常指定預設身份驗證方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-136">When configuring authentication, it's common to specify the default authentication scheme.</span></span> <span data-ttu-id="b70e8-137">除非資源請求特定方案,否則將使用預設方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-137">The default scheme is used unless a resource requests a specific scheme.</span></span> <span data-ttu-id="b70e8-138">也可以:</span><span class="sxs-lookup"><span data-stu-id="b70e8-138">It's also possible to:</span></span>

* <span data-ttu-id="b70e8-139">指定用於身份驗證、質詢和禁止操作的不同預設方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-139">Specify different default schemes to use for authenticate, challenge, and forbid actions.</span></span>
* <span data-ttu-id="b70e8-140">使用[策略方案](xref:security/authentication/policyschemes)將多個方案合併為一個方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-140">Combine multiple schemes into one using [policy schemes](xref:security/authentication/policyschemes).</span></span>

### <a name="authentication-handler"></a><span data-ttu-id="b70e8-141">驗證處理常式</span><span class="sxs-lookup"><span data-stu-id="b70e8-141">Authentication handler</span></span>

<span data-ttu-id="b70e8-142">認證處理者:</span><span class="sxs-lookup"><span data-stu-id="b70e8-142">An authentication handler:</span></span>

* <span data-ttu-id="b70e8-143">是實現方案行為的類型。</span><span class="sxs-lookup"><span data-stu-id="b70e8-143">Is a type that implements the behavior of a scheme.</span></span>
* <span data-ttu-id="b70e8-144">衍生或<xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler><xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>。</span><span class="sxs-lookup"><span data-stu-id="b70e8-144">Is derived from <xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler> or <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>.</span></span>
* <span data-ttu-id="b70e8-145">對用戶進行身份驗證負有主要責任。</span><span class="sxs-lookup"><span data-stu-id="b70e8-145">Has the primary responsibility to authenticate users.</span></span>

<span data-ttu-id="b70e8-146">根據身份驗證配置與傳入的要求上下文,身份驗證處理程式:</span><span class="sxs-lookup"><span data-stu-id="b70e8-146">Based on the authentication scheme's configuration and the incoming request context, authentication handlers:</span></span>

* <span data-ttu-id="b70e8-147">如果<xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket>身份驗證成功,則構造表示用戶標識的物件。</span><span class="sxs-lookup"><span data-stu-id="b70e8-147">Construct <xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket> objects representing the user's identity if authentication is successful.</span></span>
* <span data-ttu-id="b70e8-148">如果身份驗證不成功,則返回"無結果"或"失敗"。</span><span class="sxs-lookup"><span data-stu-id="b70e8-148">Return 'no result' or 'failure' if authentication is unsuccessful.</span></span>
* <span data-ttu-id="b70e8-149">在使用者嘗試存取資源時,具有質詢和禁止操作的方法:</span><span class="sxs-lookup"><span data-stu-id="b70e8-149">Have methods for challenge and forbid actions for when users attempt to access resources:</span></span>
  * <span data-ttu-id="b70e8-150">它們未經授權訪問(禁止)。</span><span class="sxs-lookup"><span data-stu-id="b70e8-150">They are unauthorized to access (forbid).</span></span>
  * <span data-ttu-id="b70e8-151">當他們未經身份驗證時(質詢)。</span><span class="sxs-lookup"><span data-stu-id="b70e8-151">When they are unauthenticated (challenge).</span></span>

### <a name="authenticate"></a><span data-ttu-id="b70e8-152">Authenticate</span><span class="sxs-lookup"><span data-stu-id="b70e8-152">Authenticate</span></span>

<span data-ttu-id="b70e8-153">身份驗證方案的身份驗證操作負責根據請求上下文構造使用者的身份。</span><span class="sxs-lookup"><span data-stu-id="b70e8-153">An authentication scheme's authenticate action is responsible for constructing the user's identity based on request context.</span></span> <span data-ttu-id="b70e8-154">它返回指示<xref:Microsoft.AspNetCore.Authentication.AuthenticateResult>身份驗證是否成功,如果是,則返回身份驗證票證中的用戶標識。</span><span class="sxs-lookup"><span data-stu-id="b70e8-154">It returns an <xref:Microsoft.AspNetCore.Authentication.AuthenticateResult> indicating whether authentication was successful and, if so, the user's identity in an authentication ticket.</span></span> <span data-ttu-id="b70e8-155">請參閱＜<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync%2A>＞。</span><span class="sxs-lookup"><span data-stu-id="b70e8-155">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync%2A>.</span></span> <span data-ttu-id="b70e8-156">認證範例包括:</span><span class="sxs-lookup"><span data-stu-id="b70e8-156">Authenticate examples include:</span></span>

* <span data-ttu-id="b70e8-157">從 Cookie 構造用戶標識的 Cookie 身份驗證方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-157">A cookie authentication scheme constructing the user's identity from cookies.</span></span>
* <span data-ttu-id="b70e8-158">JWT 承載方案可反序列化和驗證 JWT 承載權杖以建構使用者的身份。</span><span class="sxs-lookup"><span data-stu-id="b70e8-158">A JWT bearer scheme deserializing and validating a JWT bearer token to construct the user's identity.</span></span>

### <a name="challenge"></a><span data-ttu-id="b70e8-159">挑戰</span><span class="sxs-lookup"><span data-stu-id="b70e8-159">Challenge</span></span>

<span data-ttu-id="b70e8-160">當未經身份驗證的使用者請求需要身份驗證的終結點時,授權會調用身份驗證質詢。</span><span class="sxs-lookup"><span data-stu-id="b70e8-160">An authentication challenge is invoked by Authorization when an unauthenticated user requests an endpoint that requires authentication.</span></span> <span data-ttu-id="b70e8-161">例如,當匿名使用者請求受限資源或單擊登錄連結時,將發出身份驗證質詢。</span><span class="sxs-lookup"><span data-stu-id="b70e8-161">An authentication challenge is issued, for example, when an anonymous user requests a restricted resource or clicks on a login link.</span></span> <span data-ttu-id="b70e8-162">授權使用指定的身份驗證方案調用質詢,或者在未指定任何身份驗證方案時調用預設值。</span><span class="sxs-lookup"><span data-stu-id="b70e8-162">Authorization invokes a challenge using the specified authentication scheme(s), or the default if none is specified.</span></span> <span data-ttu-id="b70e8-163">請參閱＜<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync%2A>＞。</span><span class="sxs-lookup"><span data-stu-id="b70e8-163">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync%2A>.</span></span> <span data-ttu-id="b70e8-164">認證質詢範例包括:</span><span class="sxs-lookup"><span data-stu-id="b70e8-164">Authentication challenge examples include:</span></span>

* <span data-ttu-id="b70e8-165">Cookie 身份驗證方案將使用者重定向到登錄頁。</span><span class="sxs-lookup"><span data-stu-id="b70e8-165">A cookie authentication scheme redirecting the user to a login page.</span></span>
* <span data-ttu-id="b70e8-166">返回帶標頭的 401`www-authenticate: bearer`結果的 JWT 承載方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-166">A JWT bearer scheme returning a 401 result with a `www-authenticate: bearer` header.</span></span>

<span data-ttu-id="b70e8-167">質詢操作應讓使用者知道使用什麼身份驗證機制來訪問請求的資源。</span><span class="sxs-lookup"><span data-stu-id="b70e8-167">A challenge action should let the user know what authentication mechanism to use to access the requested resource.</span></span>

### <a name="forbid"></a><span data-ttu-id="b70e8-168">禁止</span><span class="sxs-lookup"><span data-stu-id="b70e8-168">Forbid</span></span>

<span data-ttu-id="b70e8-169">當經過身份驗證的用戶嘗試訪問不允許其訪問的資源時,授權調用身份驗證方案的禁止操作。</span><span class="sxs-lookup"><span data-stu-id="b70e8-169">An authentication scheme's forbid action is called by Authorization when an authenticated user attempts to access a resource they are not permitted to access.</span></span> <span data-ttu-id="b70e8-170">請參閱＜<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync%2A>＞。</span><span class="sxs-lookup"><span data-stu-id="b70e8-170">See <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync%2A>.</span></span> <span data-ttu-id="b70e8-171">禁止身份驗證的範例包括:</span><span class="sxs-lookup"><span data-stu-id="b70e8-171">Authentication forbid examples include:</span></span>
* <span data-ttu-id="b70e8-172">禁止將用戶重定向到指示訪問許可權的頁面的 Cookie 身份驗證方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-172">A cookie authentication scheme redirecting the user to a page indicating access was forbidden.</span></span>
* <span data-ttu-id="b70e8-173">返回 403 結果的 JWT 承載計劃。</span><span class="sxs-lookup"><span data-stu-id="b70e8-173">A JWT bearer scheme returning a 403 result.</span></span>
* <span data-ttu-id="b70e8-174">自定義身份驗證方案重定向到使用者可以請求訪問資源的頁面。</span><span class="sxs-lookup"><span data-stu-id="b70e8-174">A custom authentication scheme redirecting to a page where the user can request access to the resource.</span></span>

<span data-ttu-id="b70e8-175">禁止操作可以讓使用者知道:</span><span class="sxs-lookup"><span data-stu-id="b70e8-175">A forbid action can let the user know:</span></span>

* <span data-ttu-id="b70e8-176">它們經過身份驗證。</span><span class="sxs-lookup"><span data-stu-id="b70e8-176">They are authenticated.</span></span>
* <span data-ttu-id="b70e8-177">不允許他們訪問請求的資源。</span><span class="sxs-lookup"><span data-stu-id="b70e8-177">They aren't permitted to access the requested resource.</span></span>

<span data-ttu-id="b70e8-178">有關挑戰和禁止之間的差異,請參閱以下連結:</span><span class="sxs-lookup"><span data-stu-id="b70e8-178">See the following links for differences between challenge and forbid:</span></span>

* <span data-ttu-id="b70e8-179">[使用操作資源處理程式挑戰和禁止](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler)。</span><span class="sxs-lookup"><span data-stu-id="b70e8-179">[Challenge and forbid with an operational resource handler](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler).</span></span>
* <span data-ttu-id="b70e8-180">[挑戰與關閉的差異](xref:security/authorization/secure-data#challenge)。</span><span class="sxs-lookup"><span data-stu-id="b70e8-180">[Differences between challenge and forbid](xref:security/authorization/secure-data#challenge).</span></span>

## <a name="authentication-providers-per-tenant"></a><span data-ttu-id="b70e8-181">每個租戶的身份驗證提供者</span><span class="sxs-lookup"><span data-stu-id="b70e8-181">Authentication providers per tenant</span></span>

<span data-ttu-id="b70e8-182">ASP.NET核心框架沒有用於多租戶身份驗證的內置解決方案。</span><span class="sxs-lookup"><span data-stu-id="b70e8-182">ASP.NET Core framework does not have a built-in solution for multi-tenant authentication.</span></span>
<span data-ttu-id="b70e8-183">雖然客戶當然可以使用內建功能編寫一個功能,但我們建議客戶為此查看[Orchard Core。](https://www.orchardcore.net/)</span><span class="sxs-lookup"><span data-stu-id="b70e8-183">While it's certainly possible for customers to write one, using the built-in features, we recommend customers to look into [Orchard Core](https://www.orchardcore.net/) for this purpose.</span></span>

<span data-ttu-id="b70e8-184">果園核心是:</span><span class="sxs-lookup"><span data-stu-id="b70e8-184">Orchard Core is:</span></span>

* <span data-ttu-id="b70e8-185">使用 ASP.NET Core 構建的開源模組化和多租戶應用框架。</span><span class="sxs-lookup"><span data-stu-id="b70e8-185">An open-source modular and multi-tenant app framework built with ASP.NET Core.</span></span>
* <span data-ttu-id="b70e8-186">構建在應用框架之上的內容管理系統 (CMS)。</span><span class="sxs-lookup"><span data-stu-id="b70e8-186">A content management system (CMS) built on top of that app framework.</span></span>

<span data-ttu-id="b70e8-187">有關每個租戶的身份驗證提供程式的範例,請參閱[Orchard Core](https://github.com/OrchardCMS/OrchardCore)源。</span><span class="sxs-lookup"><span data-stu-id="b70e8-187">See the [Orchard Core](https://github.com/OrchardCMS/OrchardCore) source for an example of authentication providers per tenant.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b70e8-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="b70e8-188">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authentication/policyschemes>
* <xref:security/authorization/secure-data>
