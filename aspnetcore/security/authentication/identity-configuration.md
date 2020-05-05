---
title: 設定 ASP.NET CoreIdentity
author: AdrienTorris
description: 瞭解 ASP.NET Core Identity預設值，並瞭解如何設定Identity屬性以使用自訂值。
ms.author: riande
ms.date: 02/11/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity-configuration
ms.openlocfilehash: b88f2627eabc536f2d3b8e677020a67bfd1a40ba
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775644"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="4d398-103">設定 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="4d398-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="4d398-104">ASP.NET Core 身分識別會使用設定的預設值，例如密碼原則、鎖定和 cookie 設定。</span><span class="sxs-lookup"><span data-stu-id="4d398-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="4d398-105">這些設定可以在`Startup`類別中覆寫。</span><span class="sxs-lookup"><span data-stu-id="4d398-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="4d398-106">身分識別選項</span><span class="sxs-lookup"><span data-stu-id="4d398-106">Identity options</span></span>

<span data-ttu-id="4d398-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)類別代表可以用來設定身分識別系統的選項。</span><span class="sxs-lookup"><span data-stu-id="4d398-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="4d398-108">`IdentityOptions`呼叫`AddIdentity`或`AddDefaultIdentity`**之後**，必須設定。</span><span class="sxs-lookup"><span data-stu-id="4d398-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="4d398-109">宣告身分識別</span><span class="sxs-lookup"><span data-stu-id="4d398-109">Claims Identity</span></span>

<span data-ttu-id="4d398-110">[IdentityOptions. ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)會使用下表所示的屬性來指定[ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="4d398-111">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-111">Property</span></span> | <span data-ttu-id="4d398-112">描述</span><span class="sxs-lookup"><span data-stu-id="4d398-112">Description</span></span> | <span data-ttu-id="4d398-113">預設</span><span class="sxs-lookup"><span data-stu-id="4d398-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="4d398-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="4d398-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="4d398-115">取得或設定用於角色宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="4d398-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="4d398-116">Claimtype。角色</span><span class="sxs-lookup"><span data-stu-id="4d398-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="4d398-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="4d398-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="4d398-118">取得或設定用於安全性戳記宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="4d398-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="4d398-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="4d398-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="4d398-120">取得或設定用於使用者識別碼宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="4d398-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="4d398-121">Claimtype. NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="4d398-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="4d398-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="4d398-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="4d398-123">取得或設定用於使用者名稱宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="4d398-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="4d398-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="4d398-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="4d398-125">鎖定</span><span class="sxs-lookup"><span data-stu-id="4d398-125">Lockout</span></span>

<span data-ttu-id="4d398-126">鎖定是在[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)方法中設定：</span><span class="sxs-lookup"><span data-stu-id="4d398-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="4d398-127">上述程式碼是以身分`Login`識別範本為基礎。</span><span class="sxs-lookup"><span data-stu-id="4d398-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="4d398-128">鎖定選項設定于`StartUp.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="4d398-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="4d398-129">上述程式碼會使用預設值來設定[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="4d398-130">成功的驗證會重設失敗的存取嘗試計數，並重設時鐘。</span><span class="sxs-lookup"><span data-stu-id="4d398-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="4d398-131">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)會使用資料表中顯示的屬性來指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="4d398-132">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-132">Property</span></span> | <span data-ttu-id="4d398-133">描述</span><span class="sxs-lookup"><span data-stu-id="4d398-133">Description</span></span> | <span data-ttu-id="4d398-134">預設</span><span class="sxs-lookup"><span data-stu-id="4d398-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="4d398-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="4d398-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="4d398-136">決定是否可以鎖定新的使用者。</span><span class="sxs-lookup"><span data-stu-id="4d398-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="4d398-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="4d398-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="4d398-138">發生鎖定時使用者鎖定的時間量。</span><span class="sxs-lookup"><span data-stu-id="4d398-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="4d398-139">5 分鐘</span><span class="sxs-lookup"><span data-stu-id="4d398-139">5 minutes</span></span> |
| [<span data-ttu-id="4d398-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="4d398-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="4d398-141">在使用者遭到鎖定之前，如果已啟用鎖定，則嘗試存取失敗的次數。</span><span class="sxs-lookup"><span data-stu-id="4d398-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="4d398-142">5</span><span class="sxs-lookup"><span data-stu-id="4d398-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="4d398-143">密碼</span><span class="sxs-lookup"><span data-stu-id="4d398-143">Password</span></span>

<span data-ttu-id="4d398-144">根據預設，身分識別需要密碼包含大寫字元、小寫字元、數位和非英數位元。</span><span class="sxs-lookup"><span data-stu-id="4d398-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="4d398-145">密碼長度至少必須為6個字元。</span><span class="sxs-lookup"><span data-stu-id="4d398-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="4d398-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)可以在中`Startup.ConfigureServices`設定。</span><span class="sxs-lookup"><span data-stu-id="4d398-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="4d398-147">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)會使用資料表中顯示的屬性來指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="4d398-148">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-148">Property</span></span> | <span data-ttu-id="4d398-149">描述</span><span class="sxs-lookup"><span data-stu-id="4d398-149">Description</span></span> | <span data-ttu-id="4d398-150">預設</span><span class="sxs-lookup"><span data-stu-id="4d398-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="4d398-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="4d398-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="4d398-152">在密碼中需要介於0-9 之間的數位。</span><span class="sxs-lookup"><span data-stu-id="4d398-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="4d398-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="4d398-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="4d398-154">密碼的最小長度。</span><span class="sxs-lookup"><span data-stu-id="4d398-154">The minimum length of the password.</span></span> | <span data-ttu-id="4d398-155">6</span><span class="sxs-lookup"><span data-stu-id="4d398-155">6</span></span> |
| [<span data-ttu-id="4d398-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="4d398-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="4d398-157">需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="4d398-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="4d398-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="4d398-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="4d398-159">密碼中需要非英數位元。</span><span class="sxs-lookup"><span data-stu-id="4d398-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="4d398-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="4d398-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="4d398-161">僅適用于 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="4d398-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="4d398-162">需要密碼中的相異字元數。</span><span class="sxs-lookup"><span data-stu-id="4d398-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="4d398-163">1</span><span class="sxs-lookup"><span data-stu-id="4d398-163">1</span></span> |
| [<span data-ttu-id="4d398-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="4d398-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="4d398-165">需要密碼中的大寫字元。</span><span class="sxs-lookup"><span data-stu-id="4d398-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="4d398-166">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-166">Property</span></span> | <span data-ttu-id="4d398-167">描述</span><span class="sxs-lookup"><span data-stu-id="4d398-167">Description</span></span> | <span data-ttu-id="4d398-168">預設</span><span class="sxs-lookup"><span data-stu-id="4d398-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="4d398-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="4d398-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="4d398-170">在密碼中需要介於0-9 之間的數位。</span><span class="sxs-lookup"><span data-stu-id="4d398-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="4d398-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="4d398-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="4d398-172">密碼的最小長度。</span><span class="sxs-lookup"><span data-stu-id="4d398-172">The minimum length of the password.</span></span> | <span data-ttu-id="4d398-173">6</span><span class="sxs-lookup"><span data-stu-id="4d398-173">6</span></span> |
| [<span data-ttu-id="4d398-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="4d398-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="4d398-175">需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="4d398-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="4d398-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="4d398-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="4d398-177">密碼中需要非英數位元。</span><span class="sxs-lookup"><span data-stu-id="4d398-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="4d398-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="4d398-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="4d398-179">需要密碼中的大寫字元。</span><span class="sxs-lookup"><span data-stu-id="4d398-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="4d398-180">登入</span><span class="sxs-lookup"><span data-stu-id="4d398-180">Sign-in</span></span>

<span data-ttu-id="4d398-181">下列程式碼會`SignIn`設定設定（預設值）：</span><span class="sxs-lookup"><span data-stu-id="4d398-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="4d398-182">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)會使用資料表中顯示的屬性來指定[SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="4d398-183">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-183">Property</span></span> | <span data-ttu-id="4d398-184">描述</span><span class="sxs-lookup"><span data-stu-id="4d398-184">Description</span></span> | <span data-ttu-id="4d398-185">預設</span><span class="sxs-lookup"><span data-stu-id="4d398-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="4d398-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="4d398-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="4d398-187">需要已確認的電子郵件才能登入。</span><span class="sxs-lookup"><span data-stu-id="4d398-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="4d398-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="4d398-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="4d398-189">需要確認的電話號碼才能登入。</span><span class="sxs-lookup"><span data-stu-id="4d398-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="4d398-190">權杖</span><span class="sxs-lookup"><span data-stu-id="4d398-190">Tokens</span></span>

<span data-ttu-id="4d398-191">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)會使用資料表中顯示的屬性來指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="4d398-192">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="4d398-193">說明</span><span class="sxs-lookup"><span data-stu-id="4d398-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="4d398-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="4d398-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="4d398-195">取得或設定， `AuthenticatorTokenProvider`用來驗證驗證器的雙因素登入。</span><span class="sxs-lookup"><span data-stu-id="4d398-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="4d398-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="4d398-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="4d398-197">取得或設定， `ChangeEmailTokenProvider`用來產生電子郵件變更確認電子郵件中使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="4d398-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="4d398-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="4d398-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="4d398-199">取得或設定， `ChangePhoneNumberTokenProvider`用來產生變更電話號碼時使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="4d398-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="4d398-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="4d398-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="4d398-201">取得或設定權杖提供者，用來產生帳戶確認電子郵件中使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="4d398-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="4d398-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="4d398-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="4d398-203">取得或設定[\<IUserTwoFactorTokenProvider TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)用來產生密碼重設電子郵件中使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="4d398-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="4d398-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="4d398-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="4d398-205">用來以用來作為提供者名稱的金鑰來建立[使用者權杖提供者](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)。</span><span class="sxs-lookup"><span data-stu-id="4d398-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="4d398-206">User</span><span class="sxs-lookup"><span data-stu-id="4d398-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="4d398-207">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)會使用資料表中顯示的屬性來指定[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="4d398-208">屬性</span><span class="sxs-lookup"><span data-stu-id="4d398-208">Property</span></span> | <span data-ttu-id="4d398-209">說明</span><span class="sxs-lookup"><span data-stu-id="4d398-209">Description</span></span> | <span data-ttu-id="4d398-210">預設</span><span class="sxs-lookup"><span data-stu-id="4d398-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="4d398-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="4d398-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="4d398-212">使用者名稱中允許的字元。</span><span class="sxs-lookup"><span data-stu-id="4d398-212">Allowed characters in the username.</span></span> | <span data-ttu-id="4d398-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="4d398-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="4d398-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="4d398-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="4d398-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="4d398-215">0123456789</span></span><br><span data-ttu-id="4d398-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="4d398-216">-.\_@+</span></span> |
| [<span data-ttu-id="4d398-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="4d398-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="4d398-218">要求每位使用者都有唯一的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="4d398-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="4d398-219">Cookie 設定</span><span class="sxs-lookup"><span data-stu-id="4d398-219">Cookie settings</span></span>

<span data-ttu-id="4d398-220">在中`Startup.ConfigureServices`設定應用程式的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4d398-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4d398-221">呼叫`AddIdentity`或`AddDefaultIdentity`**之後**，必須呼叫[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) 。</span><span class="sxs-lookup"><span data-stu-id="4d398-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="4d398-222">如需詳細資訊，請參閱[cookieauthenticationoptions.authenticationtype](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)。</span><span class="sxs-lookup"><span data-stu-id="4d398-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="4d398-223">密碼 Hasher 選項</span><span class="sxs-lookup"><span data-stu-id="4d398-223">Password Hasher options</span></span>

<span data-ttu-id="4d398-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions>取得和設定密碼雜湊的選項。</span><span class="sxs-lookup"><span data-stu-id="4d398-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="4d398-225">選項</span><span class="sxs-lookup"><span data-stu-id="4d398-225">Option</span></span> | <span data-ttu-id="4d398-226">描述</span><span class="sxs-lookup"><span data-stu-id="4d398-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="4d398-227">雜湊新密碼時所使用的相容性模式。</span><span class="sxs-lookup"><span data-stu-id="4d398-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="4d398-228">預設為 <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>。</span><span class="sxs-lookup"><span data-stu-id="4d398-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="4d398-229">雜湊密碼的第一個位元組（稱為*格式標記*）會指定用來雜湊密碼的雜湊演算法版本。</span><span class="sxs-lookup"><span data-stu-id="4d398-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="4d398-230">根據雜湊來驗證密碼時， <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*>方法會根據第一個位元組來選取正確的演算法。</span><span class="sxs-lookup"><span data-stu-id="4d398-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="4d398-231">無論使用哪一個演算法版本來雜湊密碼，用戶端都可以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="4d398-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="4d398-232">設定相容性模式會影響*新密碼*的雜湊。</span><span class="sxs-lookup"><span data-stu-id="4d398-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="4d398-233">使用 PBKDF2 來對密碼進行雜湊處理時所使用的反覆運算次數。</span><span class="sxs-lookup"><span data-stu-id="4d398-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="4d398-234">只有當設定為<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>時，才會使用這個值。</span><span class="sxs-lookup"><span data-stu-id="4d398-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="4d398-235">值必須是正整數，且預設值為`10000`。</span><span class="sxs-lookup"><span data-stu-id="4d398-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="4d398-236">在下列範例中，會<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount>設定為`12000`中`Startup.ConfigureServices`的：</span><span class="sxs-lookup"><span data-stu-id="4d398-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
