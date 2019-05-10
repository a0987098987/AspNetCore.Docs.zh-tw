---
title: 設定 ASP.NET Core 身分識別
author: AdrienTorris
description: 了解 ASP.NET Core 身分識別的預設值，並了解如何設定身分識別屬性，以使用自訂的值。
ms.author: riande
ms.date: 02/11/2019
uid: security/authentication/identity-configuration
ms.openlocfilehash: 823182bed2cb953e07f9374d135868aeb2be9c60
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892345"
---
# <a name="configure-aspnet-core-identity"></a><span data-ttu-id="41f9a-103">設定 ASP.NET Core 身分識別</span><span class="sxs-lookup"><span data-stu-id="41f9a-103">Configure ASP.NET Core Identity</span></span>

<span data-ttu-id="41f9a-104">ASP.NET Core 身分識別設定，例如密碼原則、 鎖定和 cookie 組態使用預設值。</span><span class="sxs-lookup"><span data-stu-id="41f9a-104">ASP.NET Core Identity uses default values for settings such as password policy, lockout, and cookie configuration.</span></span> <span data-ttu-id="41f9a-105">這些設定可以覆寫在`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="41f9a-105">These settings can be overridden in the `Startup` class.</span></span>

## <a name="identity-options"></a><span data-ttu-id="41f9a-106">識別選項</span><span class="sxs-lookup"><span data-stu-id="41f9a-106">Identity options</span></span>

<span data-ttu-id="41f9a-107">[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)類別代表可用來設定身分識別系統的選項。</span><span class="sxs-lookup"><span data-stu-id="41f9a-107">The [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) class represents the options that can be used to configure the Identity system.</span></span> <span data-ttu-id="41f9a-108">`IdentityOptions` 必須設定**之後**呼叫`AddIdentity`或`AddDefaultIdentity`。</span><span class="sxs-lookup"><span data-stu-id="41f9a-108">`IdentityOptions` must be set **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

### <a name="claims-identity"></a><span data-ttu-id="41f9a-109">宣告識別</span><span class="sxs-lookup"><span data-stu-id="41f9a-109">Claims Identity</span></span>

<span data-ttu-id="41f9a-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)指定[ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)與下表所示的屬性。</span><span class="sxs-lookup"><span data-stu-id="41f9a-110">[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) specifies the [ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) with the properties shown in the following table.</span></span>

| <span data-ttu-id="41f9a-111">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-111">Property</span></span> | <span data-ttu-id="41f9a-112">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-112">Description</span></span> | <span data-ttu-id="41f9a-113">預設</span><span class="sxs-lookup"><span data-stu-id="41f9a-113">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="41f9a-114">RoleClaimType</span><span class="sxs-lookup"><span data-stu-id="41f9a-114">RoleClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | <span data-ttu-id="41f9a-115">取得或設定用於角色宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="41f9a-115">Gets or sets the claim type used for a role claim.</span></span> | [<span data-ttu-id="41f9a-116">ClaimTypes.Role</span><span class="sxs-lookup"><span data-stu-id="41f9a-116">ClaimTypes.Role</span></span>](/dotnet/api/system.security.claims.claimtypes.role) |
| [<span data-ttu-id="41f9a-117">SecurityStampClaimType</span><span class="sxs-lookup"><span data-stu-id="41f9a-117">SecurityStampClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | <span data-ttu-id="41f9a-118">取得或設定用於安全性戳記宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="41f9a-118">Gets or sets the claim type used for the security stamp claim.</span></span> | `AspNet.Identity.SecurityStamp` |
| [<span data-ttu-id="41f9a-119">UserIdClaimType</span><span class="sxs-lookup"><span data-stu-id="41f9a-119">UserIdClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | <span data-ttu-id="41f9a-120">取得或設定用來將使用者識別碼宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="41f9a-120">Gets or sets the claim type used for the user identifier claim.</span></span> | [<span data-ttu-id="41f9a-121">ClaimTypes.NameIdentifier</span><span class="sxs-lookup"><span data-stu-id="41f9a-121">ClaimTypes.NameIdentifier</span></span>](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [<span data-ttu-id="41f9a-122">UserNameClaimType</span><span class="sxs-lookup"><span data-stu-id="41f9a-122">UserNameClaimType</span></span>](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | <span data-ttu-id="41f9a-123">取得或設定用於使用者名稱宣告的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="41f9a-123">Gets or sets the claim type used for the user name claim.</span></span> | [<span data-ttu-id="41f9a-124">ClaimTypes.Name</span><span class="sxs-lookup"><span data-stu-id="41f9a-124">ClaimTypes.Name</span></span>](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a><span data-ttu-id="41f9a-125">鎖定</span><span class="sxs-lookup"><span data-stu-id="41f9a-125">Lockout</span></span>

<span data-ttu-id="41f9a-126">鎖定在中設定[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)方法：</span><span class="sxs-lookup"><span data-stu-id="41f9a-126">Lockout is set in the [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) method:</span></span>

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="41f9a-127">上述程式碼根據`Login`識別範本。</span><span class="sxs-lookup"><span data-stu-id="41f9a-127">The preceding code is based on the `Login` Identity template.</span></span> 

<span data-ttu-id="41f9a-128">在 設定鎖定選項`StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="41f9a-128">Lockout options are set in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

<span data-ttu-id="41f9a-129">上述程式碼集[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)具有預設值。</span><span class="sxs-lookup"><span data-stu-id="41f9a-129">The preceding code sets the [IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with default values.</span></span>

<span data-ttu-id="41f9a-130">驗證成功重設失敗的存取嘗試次數和重設時鐘。</span><span class="sxs-lookup"><span data-stu-id="41f9a-130">A successful authentication resets the failed access attempts count and resets the clock.</span></span>

<span data-ttu-id="41f9a-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)表所示的屬性。</span><span class="sxs-lookup"><span data-stu-id="41f9a-131">[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) specifies the [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="41f9a-132">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-132">Property</span></span> | <span data-ttu-id="41f9a-133">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-133">Description</span></span> | <span data-ttu-id="41f9a-134">預設</span><span class="sxs-lookup"><span data-stu-id="41f9a-134">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="41f9a-135">AllowedForNewUsers</span><span class="sxs-lookup"><span data-stu-id="41f9a-135">AllowedForNewUsers</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | <span data-ttu-id="41f9a-136">決定是否新的使用者可能會鎖定。</span><span class="sxs-lookup"><span data-stu-id="41f9a-136">Determines if a new user can be locked out.</span></span> | `true` |
| [<span data-ttu-id="41f9a-137">DefaultLockoutTimeSpan</span><span class="sxs-lookup"><span data-stu-id="41f9a-137">DefaultLockoutTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | <span data-ttu-id="41f9a-138">時間長度使用者遭到鎖定時的鎖定，就會發生。</span><span class="sxs-lookup"><span data-stu-id="41f9a-138">The amount of time a user is locked out when a lockout occurs.</span></span> | <span data-ttu-id="41f9a-139">5 分鐘</span><span class="sxs-lookup"><span data-stu-id="41f9a-139">5 minutes</span></span> |
| [<span data-ttu-id="41f9a-140">MaxFailedAccessAttempts</span><span class="sxs-lookup"><span data-stu-id="41f9a-140">MaxFailedAccessAttempts</span></span>](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | <span data-ttu-id="41f9a-141">失敗的存取嘗試，直到使用者遭到鎖定，如果已啟用鎖定數目。</span><span class="sxs-lookup"><span data-stu-id="41f9a-141">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> | <span data-ttu-id="41f9a-142">5</span><span class="sxs-lookup"><span data-stu-id="41f9a-142">5</span></span> |

### <a name="password"></a><span data-ttu-id="41f9a-143">密碼</span><span class="sxs-lookup"><span data-stu-id="41f9a-143">Password</span></span>

<span data-ttu-id="41f9a-144">根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字、 和非英數字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-144">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="41f9a-145">密碼必須至少六個字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-145">Passwords must be at least six characters long.</span></span> <span data-ttu-id="41f9a-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)中可設定`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="41f9a-146">[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) can be set in `Startup.ConfigureServices`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

<span data-ttu-id="41f9a-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)表所示的屬性。</span><span class="sxs-lookup"><span data-stu-id="41f9a-147">[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) specifies the [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) with the properties shown in the table.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="41f9a-148">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-148">Property</span></span> | <span data-ttu-id="41f9a-149">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-149">Description</span></span> | <span data-ttu-id="41f9a-150">預設</span><span class="sxs-lookup"><span data-stu-id="41f9a-150">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="41f9a-151">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="41f9a-151">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="41f9a-152">需要介於 0-9 密碼中的數字。</span><span class="sxs-lookup"><span data-stu-id="41f9a-152">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="41f9a-153">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="41f9a-153">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="41f9a-154">密碼長度下限。</span><span class="sxs-lookup"><span data-stu-id="41f9a-154">The minimum length of the password.</span></span> | <span data-ttu-id="41f9a-155">6</span><span class="sxs-lookup"><span data-stu-id="41f9a-155">6</span></span> |
| [<span data-ttu-id="41f9a-156">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="41f9a-156">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="41f9a-157">需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-157">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="41f9a-158">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="41f9a-158">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="41f9a-159">需要密碼的非英數字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-159">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="41f9a-160">RequiredUniqueChars</span><span class="sxs-lookup"><span data-stu-id="41f9a-160">RequiredUniqueChars</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | <span data-ttu-id="41f9a-161">僅適用於 ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="41f9a-161">Only applies to ASP.NET Core 2.0 or later.</span></span><br><br> <span data-ttu-id="41f9a-162">需要密碼中的不同字元的數。</span><span class="sxs-lookup"><span data-stu-id="41f9a-162">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="41f9a-163">1</span><span class="sxs-lookup"><span data-stu-id="41f9a-163">1</span></span> |
| [<span data-ttu-id="41f9a-164">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="41f9a-164">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="41f9a-165">需要密碼以大寫字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-165">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="41f9a-166">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-166">Property</span></span> | <span data-ttu-id="41f9a-167">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-167">Description</span></span> | <span data-ttu-id="41f9a-168">預設</span><span class="sxs-lookup"><span data-stu-id="41f9a-168">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="41f9a-169">RequireDigit</span><span class="sxs-lookup"><span data-stu-id="41f9a-169">RequireDigit</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | <span data-ttu-id="41f9a-170">需要介於 0-9 密碼中的數字。</span><span class="sxs-lookup"><span data-stu-id="41f9a-170">Requires a number between 0-9 in the password.</span></span> | `true` |
| [<span data-ttu-id="41f9a-171">RequiredLength</span><span class="sxs-lookup"><span data-stu-id="41f9a-171">RequiredLength</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | <span data-ttu-id="41f9a-172">密碼長度下限。</span><span class="sxs-lookup"><span data-stu-id="41f9a-172">The minimum length of the password.</span></span> | <span data-ttu-id="41f9a-173">6</span><span class="sxs-lookup"><span data-stu-id="41f9a-173">6</span></span> |
| [<span data-ttu-id="41f9a-174">RequireLowercase</span><span class="sxs-lookup"><span data-stu-id="41f9a-174">RequireLowercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | <span data-ttu-id="41f9a-175">需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-175">Requires a lowercase character in the password.</span></span> | `true` |
| [<span data-ttu-id="41f9a-176">RequireNonAlphanumeric</span><span class="sxs-lookup"><span data-stu-id="41f9a-176">RequireNonAlphanumeric</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | <span data-ttu-id="41f9a-177">需要密碼的非英數字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-177">Requires a non-alphanumeric character in the password.</span></span> | `true` |
| [<span data-ttu-id="41f9a-178">RequireUppercase</span><span class="sxs-lookup"><span data-stu-id="41f9a-178">RequireUppercase</span></span>](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | <span data-ttu-id="41f9a-179">需要密碼以大寫字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-179">Requires an uppercase character in the password.</span></span> | `true` |

::: moniker-end

### <a name="sign-in"></a><span data-ttu-id="41f9a-180">登入</span><span class="sxs-lookup"><span data-stu-id="41f9a-180">Sign-in</span></span>

<span data-ttu-id="41f9a-181">下列程式碼設定`SignIn`設定 （預設值）：</span><span class="sxs-lookup"><span data-stu-id="41f9a-181">The following code sets `SignIn` settings (to default values):</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

<span data-ttu-id="41f9a-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)指定[SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)表所示的屬性。</span><span class="sxs-lookup"><span data-stu-id="41f9a-182">[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) specifies the [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="41f9a-183">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-183">Property</span></span> | <span data-ttu-id="41f9a-184">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-184">Description</span></span> | <span data-ttu-id="41f9a-185">預設</span><span class="sxs-lookup"><span data-stu-id="41f9a-185">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="41f9a-186">RequireConfirmedEmail</span><span class="sxs-lookup"><span data-stu-id="41f9a-186">RequireConfirmedEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | <span data-ttu-id="41f9a-187">需要一個已確認的電子郵件來登入。</span><span class="sxs-lookup"><span data-stu-id="41f9a-187">Requires a confirmed email to sign in.</span></span> | `false` |
| [<span data-ttu-id="41f9a-188">RequireConfirmedPhoneNumber</span><span class="sxs-lookup"><span data-stu-id="41f9a-188">RequireConfirmedPhoneNumber</span></span>](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | <span data-ttu-id="41f9a-189">需要確認的電話號碼，以登入。</span><span class="sxs-lookup"><span data-stu-id="41f9a-189">Requires a confirmed phone number to sign in.</span></span> | `false` |

### <a name="tokens"></a><span data-ttu-id="41f9a-190">語彙基元</span><span class="sxs-lookup"><span data-stu-id="41f9a-190">Tokens</span></span>

<span data-ttu-id="41f9a-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)表所示的屬性。</span><span class="sxs-lookup"><span data-stu-id="41f9a-191">[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) specifies the [TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) with the properties shown in the table.</span></span>

|                                                        <span data-ttu-id="41f9a-192">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-192">Property</span></span>                                                         |                                                                                      <span data-ttu-id="41f9a-193">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-193">Description</span></span>                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [<span data-ttu-id="41f9a-194">AuthenticatorTokenProvider</span><span class="sxs-lookup"><span data-stu-id="41f9a-194">AuthenticatorTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       <span data-ttu-id="41f9a-195">取得或設定`AuthenticatorTokenProvider`用來驗證兩個要素登入與驗證器。</span><span class="sxs-lookup"><span data-stu-id="41f9a-195">Gets or sets the `AuthenticatorTokenProvider` used to validate two-factor sign-ins with an authenticator.</span></span>                                       |
|       [<span data-ttu-id="41f9a-196">ChangeEmailTokenProvider</span><span class="sxs-lookup"><span data-stu-id="41f9a-196">ChangeEmailTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     <span data-ttu-id="41f9a-197">取得或設定`ChangeEmailTokenProvider`用來產生電子郵件變更確認電子郵件中所使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="41f9a-197">Gets or sets the `ChangeEmailTokenProvider` used to generate tokens used in email change confirmation emails.</span></span>                                     |
| [<span data-ttu-id="41f9a-198">ChangePhoneNumberTokenProvider</span><span class="sxs-lookup"><span data-stu-id="41f9a-198">ChangePhoneNumberTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      <span data-ttu-id="41f9a-199">取得或設定`ChangePhoneNumberTokenProvider`用來產生變更電話號碼時，使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="41f9a-199">Gets or sets the `ChangePhoneNumberTokenProvider` used to generate tokens used when changing phone numbers.</span></span>                                      |
| [<span data-ttu-id="41f9a-200">EmailConfirmationTokenProvider</span><span class="sxs-lookup"><span data-stu-id="41f9a-200">EmailConfirmationTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             <span data-ttu-id="41f9a-201">取得或設定用來產生帳戶確認電子郵件中所使用的權杖的權杖提供者。</span><span class="sxs-lookup"><span data-stu-id="41f9a-201">Gets or sets the token provider used to generate tokens used in account confirmation emails.</span></span>                                              |
|     [<span data-ttu-id="41f9a-202">PasswordResetTokenProvider</span><span class="sxs-lookup"><span data-stu-id="41f9a-202">PasswordResetTokenProvider</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | <span data-ttu-id="41f9a-203">取得或設定[IUserTwoFactorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)用來產生密碼重設電子郵件中所使用的權杖。</span><span class="sxs-lookup"><span data-stu-id="41f9a-203">Gets or sets the [IUserTwoFactorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) used to generate tokens used in password reset emails.</span></span> |
|                    [<span data-ttu-id="41f9a-204">ProviderMap</span><span class="sxs-lookup"><span data-stu-id="41f9a-204">ProviderMap</span></span>](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                <span data-ttu-id="41f9a-205">用來建構[使用者的權杖提供者](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)具有索引鍵做為提供者的名稱。</span><span class="sxs-lookup"><span data-stu-id="41f9a-205">Used to construct a [User Token Provider](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) with the key used as the provider's name.</span></span>                 |

### <a name="user"></a><span data-ttu-id="41f9a-206">使用者</span><span class="sxs-lookup"><span data-stu-id="41f9a-206">User</span></span>

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

<span data-ttu-id="41f9a-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)指定[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)表所示的屬性。</span><span class="sxs-lookup"><span data-stu-id="41f9a-207">[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) specifies the [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) with the properties shown in the table.</span></span>

| <span data-ttu-id="41f9a-208">屬性</span><span class="sxs-lookup"><span data-stu-id="41f9a-208">Property</span></span> | <span data-ttu-id="41f9a-209">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-209">Description</span></span> | <span data-ttu-id="41f9a-210">預設</span><span class="sxs-lookup"><span data-stu-id="41f9a-210">Default</span></span> |
| -------- | ----------- | :-----: |
| [<span data-ttu-id="41f9a-211">AllowedUserNameCharacters</span><span class="sxs-lookup"><span data-stu-id="41f9a-211">AllowedUserNameCharacters</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | <span data-ttu-id="41f9a-212">在 使用者名稱中允許的字元。</span><span class="sxs-lookup"><span data-stu-id="41f9a-212">Allowed characters in the username.</span></span> | <span data-ttu-id="41f9a-213">abcdefghijklmnopqrstuvwxyz</span><span class="sxs-lookup"><span data-stu-id="41f9a-213">abcdefghijklmnopqrstuvwxyz</span></span><br><span data-ttu-id="41f9a-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span><span class="sxs-lookup"><span data-stu-id="41f9a-214">ABCDEFGHIJKLMNOPQRSTUVWXYZ</span></span><br><span data-ttu-id="41f9a-215">0123456789</span><span class="sxs-lookup"><span data-stu-id="41f9a-215">0123456789</span></span><br><span data-ttu-id="41f9a-216">-.\_@+</span><span class="sxs-lookup"><span data-stu-id="41f9a-216">-.\_@+</span></span> |
| [<span data-ttu-id="41f9a-217">RequireUniqueEmail</span><span class="sxs-lookup"><span data-stu-id="41f9a-217">RequireUniqueEmail</span></span>](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | <span data-ttu-id="41f9a-218">要求每個使用者必須具有唯一的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="41f9a-218">Requires each user to have a unique email.</span></span> | `false` |

### <a name="cookie-settings"></a><span data-ttu-id="41f9a-219">Cookie 設定</span><span class="sxs-lookup"><span data-stu-id="41f9a-219">Cookie settings</span></span>

<span data-ttu-id="41f9a-220">設定中的應用程式的 cookie `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="41f9a-220">Configure the app's cookie in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="41f9a-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__)必須呼叫**之後**呼叫`AddIdentity`或`AddDefaultIdentity`。</span><span class="sxs-lookup"><span data-stu-id="41f9a-221">[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) must be called **after** calling `AddIdentity` or `AddDefaultIdentity`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

<span data-ttu-id="41f9a-222">如需詳細資訊，請參閱 [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)。</span><span class="sxs-lookup"><span data-stu-id="41f9a-222">For more information, see [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).</span></span>

## <a name="password-hasher-options"></a><span data-ttu-id="41f9a-223">密碼雜湊程式選項</span><span class="sxs-lookup"><span data-stu-id="41f9a-223">Password Hasher options</span></span>

<span data-ttu-id="41f9a-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> 取得並設定密碼雜湊的選項。</span><span class="sxs-lookup"><span data-stu-id="41f9a-224"><xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions> gets and sets options for password hashing.</span></span>

| <span data-ttu-id="41f9a-225">選項</span><span class="sxs-lookup"><span data-stu-id="41f9a-225">Option</span></span> | <span data-ttu-id="41f9a-226">描述</span><span class="sxs-lookup"><span data-stu-id="41f9a-226">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | <span data-ttu-id="41f9a-227">雜湊的新密碼時所用的相容性模式。</span><span class="sxs-lookup"><span data-stu-id="41f9a-227">The compatibility mode used when hashing new passwords.</span></span> <span data-ttu-id="41f9a-228">預設值為 <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>。</span><span class="sxs-lookup"><span data-stu-id="41f9a-228">Defaults to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="41f9a-229">雜湊的密碼，並呼叫的第一個位元組*格式標記*，指定用來雜湊密碼的雜湊演算法的版本。</span><span class="sxs-lookup"><span data-stu-id="41f9a-229">The first byte of a hashed password, called a *format marker*, specifies the version of the hashing algorithm used to hash the password.</span></span> <span data-ttu-id="41f9a-230">當驗證的密碼雜湊，<xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*>方法會選取正確的演算法為基礎的第一個位元組。</span><span class="sxs-lookup"><span data-stu-id="41f9a-230">When verifying a password against a hash, the <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> method selects the correct algorithm based on the first byte.</span></span> <span data-ttu-id="41f9a-231">用戶端是能夠進行驗證而不論其中演算法版本，用來雜湊密碼。</span><span class="sxs-lookup"><span data-stu-id="41f9a-231">A client is able to authenticate regardless of which version of the algorithm was used to hash the password.</span></span> <span data-ttu-id="41f9a-232">設定相容性模式會影響的雜湊*新的密碼*。</span><span class="sxs-lookup"><span data-stu-id="41f9a-232">Setting the compatibility mode affects the hashing of *new passwords*.</span></span> |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | <span data-ttu-id="41f9a-233">使用雜湊密碼使用 PBKDF2 時的反覆運算次數。</span><span class="sxs-lookup"><span data-stu-id="41f9a-233">The number of iterations used when hashing passwords using PBKDF2.</span></span> <span data-ttu-id="41f9a-234">此值時才會使用<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode>設為<xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>。</span><span class="sxs-lookup"><span data-stu-id="41f9a-234">This value is only used when the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> is set to <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>.</span></span> <span data-ttu-id="41f9a-235">值必須是正整數，預設值為`10000`。</span><span class="sxs-lookup"><span data-stu-id="41f9a-235">The value must be a positive integer and defaults to `10000`.</span></span> |

<span data-ttu-id="41f9a-236">在下列範例中，<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount>設定為`12000`在`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="41f9a-236">In the following example, the <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> is set to `12000` in `Startup.ConfigureServices`:</span></span>

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```
