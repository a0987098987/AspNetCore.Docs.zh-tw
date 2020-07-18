---
title: 設定 ASP.NET CoreIdentity
author: AdrienTorris
description: 瞭解 ASP.NET Core Identity 預設值，並瞭解如何設定 Identity 屬性以使用自訂值。
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/identity-configuration
ms.openlocfilehash: 262101594d57ad63bb0bac4da3cefa8d8d380908
ms.sourcegitcommit: 3544941682869734ea0113e24e02ed0ec9e1a9ec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2020
ms.locfileid: "86464523"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="bc012-103">設定 ASP.NET CoreIdentity</span><span class="sxs-lookup"><span data-stu-id="bc012-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="bc012-104">ASP.NET Core 會 Identity 使用密碼原則、鎖定和 cookie 設定等設定的預設值。</span><span class="sxs-lookup"><span data-stu-id="bc012-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="bc012-105">這些設定可以在類別中覆寫 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="bc012-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a>Identity<span data-ttu-id="bc012-106">選項</span><span class="sxs-lookup"><span data-stu-id="bc012-106"> options</span></span>

<span data-ttu-id="bc012-107">[ Identity Options](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)類別代表可以用來設定系統的選項 Identity 。</span><span class="sxs-lookup"><span data-stu-id="bc012-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="bc012-108">`IdentityOptions`呼叫或**之後**，必須 `AddIdentity` 設定 `AddDefaultIdentity` 。</span><span class="sxs-lookup"><span data-stu-id="bc012-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="bc012-109">退款Identity</span><span class="sxs-lookup"><span data-stu-id="bc012-109">Claims Identity</span></span>

<span data-ttu-id="bc012-110">[ Identity Options。宣告 Identity ](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)會使用下表所示的屬性來指定[宣告 Identity 選項](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)。</span><span class="sxs-lookup"><span data-stu-id="bc012-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="bc012-111">屬性</span><span class="sxs-lookup"><span data-stu-id="bc012-111">Property</span></span> | <span data-ttu-id="bc012-112">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-112">Description</span></span> | <span data-ttu-id="bc012-113">預設</span><span class="sxs-lookup"><span data-stu-id="bc012-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bc012-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="bc012-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="bc012-115">取得或設定用於角色宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="bc012-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="bc012-116">Claimtype。角色</span><span class="sxs-lookup"><span data-stu-id="bc012-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="bc012-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="bc012-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="bc012-118">取得或設定用於安全性戳記宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="bc012-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="bc012-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="bc012-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="bc012-120">取得或設定用於使用者識別碼宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="bc012-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="bc012-121">Claimtype. NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="bc012-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="bc012-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="bc012-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="bc012-123">取得或設定用於使用者名稱宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="bc012-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="bc012-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="bc012-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="bc012-125">鎖定</span><span class="sxs-lookup"><span data-stu-id="bc012-125">Lockout</span></span>

<span data-ttu-id="bc012-126">鎖定是在[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)方法中設定：</span><span class="sxs-lookup"><span data-stu-id="bc012-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="bc012-127">上述程式碼是以範本為基礎 `Login` Identity 。</span><span class="sxs-lookup"><span data-stu-id="bc012-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="bc012-128">鎖定選項設定于 `StartUp.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="bc012-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="bc012-129">上述程式碼會使用預設值來設定[ Identity 選項](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="bc012-130">成功的驗證會重設失敗的存取嘗試計數，並重設時鐘。</span><span class="sxs-lookup"><span data-stu-id="bc012-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="bc012-131">[ Identity 選項。鎖定](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)會使用資料表中顯示的屬性來指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bc012-132">屬性</span><span class="sxs-lookup"><span data-stu-id="bc012-132">Property</span></span> | <span data-ttu-id="bc012-133">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-133">Description</span></span> | <span data-ttu-id="bc012-134">預設</span><span class="sxs-lookup"><span data-stu-id="bc012-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bc012-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="bc012-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="bc012-136">決定是否可以鎖定新的使用者。</span><span class="sxs-lookup"><span data-stu-id="bc012-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="bc012-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="bc012-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="bc012-138">發生鎖定時使用者鎖定的時間量。</span><span class="sxs-lookup"><span data-stu-id="bc012-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="bc012-139">5 分鐘</span><span class="sxs-lookup"><span data-stu-id="bc012-139">5 minutes</span></span> |
| [<span data-ttu-id="bc012-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="bc012-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="bc012-141">在使用者遭到鎖定之前，如果已啟用鎖定，則嘗試存取失敗的次數。</span><span class="sxs-lookup"><span data-stu-id="bc012-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="bc012-142">5</span><span class="sxs-lookup"><span data-stu-id="bc012-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="bc012-143">密碼</span><span class="sxs-lookup"><span data-stu-id="bc012-143">Password</span></span>

<span data-ttu-id="bc012-144">根據預設， Identity 密碼必須包含大寫字元、小寫字元、數位和非英數位元。</span><span class="sxs-lookup"><span data-stu-id="bc012-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="bc012-145">密碼長度至少必須為6個字元。</span><span class="sxs-lookup"><span data-stu-id="bc012-145">Passwords must be at least six characters long.</span></span>

<span data-ttu-id="bc012-146">密碼是使用下列設定：</span><span class="sxs-lookup"><span data-stu-id="bc012-146">Passwords are configured with:</span></span>

* <span data-ttu-id="bc012-147"><xref:Microsoft.AspNetCore.Identity.PasswordOptions>在中 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="bc012-147"><xref:Microsoft.AspNetCore.Identity.PasswordOptions> in `Startup.ConfigureServices`.</span></span>
* <span data-ttu-id="bc012-148">[ `[StringLength]` ](xref:System.ComponentModel.DataAnnotations.StringLengthAttribute) `Password` 如果 Identity [scaffold 至應用程式](xref:security/authentication/scaffold-identity)，則為屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="bc012-148">[`[StringLength]` attributes](xref:System.ComponentModel.DataAnnotations.StringLengthAttribute) of `Password` properties if Identity is [scaffolded into the app](xref:security/authentication/scaffold-identity).</span></span> <span data-ttu-id="bc012-149">`InputModel``Password`在下列檔案中可找到屬性：</span><span class="sxs-lookup"><span data-stu-id="bc012-149">`InputModel` `Password` properties are found in the following files:</span></span>
  * `Areas/Identity/Pages/Account/Register.cshtml.cs`
  * `Areas/Identity/Pages/Account/ResetPassword.cshtml.cs`

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

<span data-ttu-id="bc012-150">[ Identity Options。 Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)會使用資料表中顯示的屬性來指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-150">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bc012-151">屬性</span><span class="sxs-lookup"><span data-stu-id="bc012-151">Property</span></span> | <span data-ttu-id="bc012-152">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-152">Description</span></span> | <span data-ttu-id="bc012-153">預設</span><span class="sxs-lookup"><span data-stu-id="bc012-153">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bc012-154">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="bc012-154">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="bc012-155">在密碼中需要介於0-9 之間的數位。</span><span class="sxs-lookup"><span data-stu-id="bc012-155">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="bc012-156">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="bc012-156">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="bc012-157">密碼的最小長度。</span><span class="sxs-lookup"><span data-stu-id="bc012-157">The minimum length of the password.</span></span> | <span data-ttu-id="bc012-158">6</span><span class="sxs-lookup"><span data-stu-id="bc012-158">6</span></span> |
| [<span data-ttu-id="bc012-159">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="bc012-159">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="bc012-160">需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="bc012-160">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="bc012-161">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="bc012-161">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="bc012-162">密碼中需要非英數位元。</span><span class="sxs-lookup"><span data-stu-id="bc012-162">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="bc012-163">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="bc012-163">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="bc012-164">僅適用于 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="bc012-164">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="bc012-165">需要密碼中的相異字元數。</span><span class="sxs-lookup"><span data-stu-id="bc012-165">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="bc012-166">1</span><span class="sxs-lookup"><span data-stu-id="bc012-166">1</span></span> |
| [<span data-ttu-id="bc012-167">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="bc012-167">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="bc012-168">需要密碼中的大寫字元。</span><span class="sxs-lookup"><span data-stu-id="bc012-168">Requires an uppercase character in the password.</span></span> | `true` |

### <a name="sign-in"></a><span data-ttu-id="bc012-169">登入</span><span class="sxs-lookup"><span data-stu-id="bc012-169">Sign-in</span></span>

<span data-ttu-id="bc012-170">下列程式碼會設定 `SignIn` 設定（預設值）：</span><span class="sxs-lookup"><span data-stu-id="bc012-170">The following code sets `SignIn` settings (to default values):</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

<span data-ttu-id="bc012-171">[登入] 會使用資料表中顯示的屬性[ Identity 來指定](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-171">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bc012-172">屬性</span><span class="sxs-lookup"><span data-stu-id="bc012-172">Property</span></span> | <span data-ttu-id="bc012-173">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-173">Description</span></span> | <span data-ttu-id="bc012-174">預設</span><span class="sxs-lookup"><span data-stu-id="bc012-174">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bc012-175">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="bc012-175">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="bc012-176">需要已確認的電子郵件才能登入。</span><span class="sxs-lookup"><span data-stu-id="bc012-176">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="bc012-177">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="bc012-177">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="bc012-178">需要確認的電話號碼才能登入。</span><span class="sxs-lookup"><span data-stu-id="bc012-178">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="bc012-179">權杖</span><span class="sxs-lookup"><span data-stu-id="bc012-179">Tokens</span></span>

<span data-ttu-id="bc012-180">[ Identity 選項。權杖](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)會使用資料表中顯示的屬性來指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-180">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bc012-181">屬性</span><span class="sxs-lookup"><span data-stu-id="bc012-181">Property</span></span> | <span data-ttu-id="bc012-182">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-182">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="bc012-183">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bc012-183">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | <span data-ttu-id="bc012-184">取得或設定，用 `AuthenticatorTokenProvider` 來驗證驗證器的雙因素登入。</span><span class="sxs-lookup"><span data-stu-id="bc012-184">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span> |
| [<span data-ttu-id="bc012-185">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bc012-185">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | <span data-ttu-id="bc012-186">取得或設定， `ChangeEmailTokenProvider` 用來產生電子郵件變更確認電子郵件中使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="bc012-186">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span> |
| [<span data-ttu-id="bc012-187">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bc012-187">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | <span data-ttu-id="bc012-188">取得或設定， `ChangePhoneNumberTokenProvider` 用來產生變更電話號碼時使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="bc012-188">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span> |
| [<span data-ttu-id="bc012-189">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bc012-189">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | <span data-ttu-id="bc012-190">取得或設定權杖提供者，用來產生帳戶確認電子郵件中使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="bc012-190">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span> |
| [<span data-ttu-id="bc012-191">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="bc012-191">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | <span data-ttu-id="bc012-192">取得或設定用來產生密碼重設電子郵件中使用之權杖的[IUserTwoFactorTokenProvider \<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-192">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
| [<span data-ttu-id="bc012-193">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="bc012-193">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | <span data-ttu-id="bc012-194">用來以用來作為提供者名稱的金鑰來建立[使用者權杖提供者](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)。</span><span class="sxs-lookup"><span data-stu-id="bc012-194">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span> |

### <a name="user"></a><span data-ttu-id="bc012-195">User</span><span class="sxs-lookup"><span data-stu-id="bc012-195">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="bc012-196">[ Identity Options。使用者](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)指定具有資料表中所顯示內容的[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) 。</span><span class="sxs-lookup"><span data-stu-id="bc012-196">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="bc012-197">屬性</span><span class="sxs-lookup"><span data-stu-id="bc012-197">Property</span></span> | <span data-ttu-id="bc012-198">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-198">Description</span></span> | <span data-ttu-id="bc012-199">預設</span><span class="sxs-lookup"><span data-stu-id="bc012-199">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="bc012-200">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="bc012-200">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="bc012-201">使用者名稱中允許的字元。</span><span class="sxs-lookup"><span data-stu-id="bc012-201">Allowed characters in the username.</span></span> | <span data-ttu-id="bc012-202">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="bc012-202">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="bc012-203">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="bc012-203">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="bc012-204">0123456789</span><span class="sxs-lookup"><span data-stu-id="bc012-204">0123456789</span></span><br><span data-ttu-id="bc012-205">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="bc012-205">-.\_@+</span></span> |
| [<span data-ttu-id="bc012-206">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="bc012-206">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="bc012-207">要求每位使用者都有唯一的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="bc012-207">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="bc012-208">Cookie 設定</span><span class="sxs-lookup"><span data-stu-id="bc012-208">Cookie settings</span></span>

<span data-ttu-id="bc012-209">在中設定應用程式的 cookie `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="bc012-209">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bc012-210">呼叫或**之後**，必須呼叫[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) `AddIdentity` `AddDefaultIdentity` 。</span><span class="sxs-lookup"><span data-stu-id="bc012-210">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

<span data-ttu-id="bc012-211">如需詳細資訊，請參閱[cookieauthenticationoptions.authenticationtype](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)。</span><span class="sxs-lookup"><span data-stu-id="bc012-211">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="bc012-212">密碼 Hasher 選項</span><span class="sxs-lookup"><span data-stu-id="bc012-212">Password Hasher options</span></span>

<span data-ttu-id="bc012-213"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions>取得和設定密碼雜湊的選項。</span><span class="sxs-lookup"><span data-stu-id="bc012-213"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="bc012-214">選項</span><span class="sxs-lookup"><span data-stu-id="bc012-214">Option</span></span> | <span data-ttu-id="bc012-215">描述</span><span class="sxs-lookup"><span data-stu-id="bc012-215">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="bc012-216">雜湊新密碼時所使用的相容性模式。</span><span class="sxs-lookup"><span data-stu-id="bc012-216">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="bc012-217">預設為 <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>。</span><span class="sxs-lookup"><span data-stu-id="bc012-217">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="bc012-218">雜湊密碼的第一個位元組（稱為*格式標記*）會指定用來雜湊密碼的雜湊演算法版本。</span><span class="sxs-lookup"><span data-stu-id="bc012-218">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="bc012-219">根據雜湊來驗證密碼時，方法會根據 <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> 第一個位元組來選取正確的演算法。</span><span class="sxs-lookup"><span data-stu-id="bc012-219">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="bc012-220">無論使用哪一個演算法版本來雜湊密碼，用戶端都可以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="bc012-220">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="bc012-221">設定相容性模式會影響*新密碼*的雜湊。</span><span class="sxs-lookup"><span data-stu-id="bc012-221">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="bc012-222">使用 PBKDF2 來對密碼進行雜湊處理時所使用的反覆運算次數。</span><span class="sxs-lookup"><span data-stu-id="bc012-222">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="bc012-223">只有當設定為時，才會使用這個值 <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3> 。</span><span class="sxs-lookup"><span data-stu-id="bc012-223">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="bc012-224">值必須是正整數，且預設值為 `10000` 。</span><span class="sxs-lookup"><span data-stu-id="bc012-224">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="bc012-225">在下列範例中， <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> 會設定為 `12000` 中的 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="bc012-225">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
