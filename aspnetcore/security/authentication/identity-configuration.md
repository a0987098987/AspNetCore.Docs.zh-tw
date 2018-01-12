---
title: "設定 ASP.NET Core 身分識別"
author: AdrienTorris
description: "了解 ASP.NET Core 識別預設值，並設定要使用自訂值的各種識別屬性。"
keywords: "ASP.NET Core，身分識別驗證安全性"
ms.author: scaddie
manager: wpickett
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: ac204cb89aac1f90adc64c4f0bec4e946cb8c4d9
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="configure-identity"></a><span data-ttu-id="17894-104">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="17894-104">Configure Identity</span></span>

<span data-ttu-id="17894-105">ASP.NET Core 身分識別密碼原則鎖定的時間，您可以覆寫輕鬆地在應用程式中的 cookie 設定像是應用程式中擁有通用行為`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="17894-105">ASP.NET Core Identity has common behaviors in applications such as password policy, lockout time, and cookie settings that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="17894-106">密碼原則</span><span class="sxs-lookup"><span data-stu-id="17894-106">Passwords policy</span></span>

<span data-ttu-id="17894-107">根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字和非英數字元。</span><span class="sxs-lookup"><span data-stu-id="17894-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="17894-108">另外還有一些其他的限制。</span><span class="sxs-lookup"><span data-stu-id="17894-108">There are also some other restrictions.</span></span> <span data-ttu-id="17894-109">若要簡化密碼限制，請修改`ConfigureServices`方法`Startup`應用程式的類別。</span><span class="sxs-lookup"><span data-stu-id="17894-109">To simplify password restrictions, modify the `ConfigureServices` method of the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="17894-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="17894-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="17894-111">ASP.NET Core 2.0 加入`RequiredUniqueChars`屬性。</span><span class="sxs-lookup"><span data-stu-id="17894-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="17894-112">否則，選項會從 ASP.NET Core 相同 1.x。</span><span class="sxs-lookup"><span data-stu-id="17894-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="17894-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="17894-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="17894-114">`IdentityOptions.Password`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="17894-114">`IdentityOptions.Password` has the following properties:</span></span>

| <span data-ttu-id="17894-115">屬性</span><span class="sxs-lookup"><span data-stu-id="17894-115">Property</span></span>                | <span data-ttu-id="17894-116">描述</span><span class="sxs-lookup"><span data-stu-id="17894-116">Description</span></span>                       | <span data-ttu-id="17894-117">預設</span><span class="sxs-lookup"><span data-stu-id="17894-117">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | <span data-ttu-id="17894-118">需要介於 0-9 的密碼。</span><span class="sxs-lookup"><span data-stu-id="17894-118">Requires a number between 0-9 in the password.</span></span> | <span data-ttu-id="17894-119">true</span><span class="sxs-lookup"><span data-stu-id="17894-119">true</span></span> |
| `RequiredLength`        | <span data-ttu-id="17894-120">密碼長度下限。</span><span class="sxs-lookup"><span data-stu-id="17894-120">The minimum length of the password.</span></span> | <span data-ttu-id="17894-121">6</span><span class="sxs-lookup"><span data-stu-id="17894-121">6</span></span> |
| `RequireNonAlphanumeric`| <span data-ttu-id="17894-122">需要密碼中的非英數字元。</span><span class="sxs-lookup"><span data-stu-id="17894-122">Requires a non-alphanumeric character in the password.</span></span> | <span data-ttu-id="17894-123">true</span><span class="sxs-lookup"><span data-stu-id="17894-123">true</span></span> |
| `RequireUppercase`      | <span data-ttu-id="17894-124">需要密碼中的大寫字元。</span><span class="sxs-lookup"><span data-stu-id="17894-124">Requires an upper case character in the password.</span></span> | <span data-ttu-id="17894-125">true</span><span class="sxs-lookup"><span data-stu-id="17894-125">true</span></span> |
| `RequireLowercase`      | <span data-ttu-id="17894-126">需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="17894-126">Requires a lower case character in the password.</span></span> | <span data-ttu-id="17894-127">true</span><span class="sxs-lookup"><span data-stu-id="17894-127">true</span></span> |
| `RequiredUniqueChars`   | <span data-ttu-id="17894-128">需要密碼中不同的字元的數。</span><span class="sxs-lookup"><span data-stu-id="17894-128">Requires the number of distinct characters in the password.</span></span> | <span data-ttu-id="17894-129">1</span><span class="sxs-lookup"><span data-stu-id="17894-129">1</span></span> |


## <a name="users-lockout"></a><span data-ttu-id="17894-130">使用者的鎖定</span><span class="sxs-lookup"><span data-stu-id="17894-130">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="17894-131">`IdentityOptions.Lockout`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="17894-131">`IdentityOptions.Lockout` has the following properties:</span></span>

| <span data-ttu-id="17894-132">屬性</span><span class="sxs-lookup"><span data-stu-id="17894-132">Property</span></span>                | <span data-ttu-id="17894-133">描述</span><span class="sxs-lookup"><span data-stu-id="17894-133">Description</span></span>                       | <span data-ttu-id="17894-134">預設</span><span class="sxs-lookup"><span data-stu-id="17894-134">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | <span data-ttu-id="17894-135">等待時間總數使用者遭到鎖定時鎖定。</span><span class="sxs-lookup"><span data-stu-id="17894-135">The amount of time a user is locked out when a lockout occurs.</span></span>  | <span data-ttu-id="17894-136">5 分鐘</span><span class="sxs-lookup"><span data-stu-id="17894-136">5 minutes</span></span>  |
| `MaxFailedAccessAttempts` | <span data-ttu-id="17894-137">失敗的存取嘗試，直到使用者鎖定，如果已啟用鎖定數目。</span><span class="sxs-lookup"><span data-stu-id="17894-137">The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span>  | <span data-ttu-id="17894-138">5</span><span class="sxs-lookup"><span data-stu-id="17894-138">5</span></span> |
| `AllowedForNewUsers` | <span data-ttu-id="17894-139">決定是否新的使用者可能會鎖定。</span><span class="sxs-lookup"><span data-stu-id="17894-139">Determines if a new user can be locked out.</span></span>  | <span data-ttu-id="17894-140">true</span><span class="sxs-lookup"><span data-stu-id="17894-140">true</span></span> |

## <a name="sign-in-settings"></a><span data-ttu-id="17894-141">登入設定</span><span class="sxs-lookup"><span data-stu-id="17894-141">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="17894-142">`IdentityOptions.SignIn`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="17894-142">`IdentityOptions.SignIn` has the following properties:</span></span>

| <span data-ttu-id="17894-143">屬性</span><span class="sxs-lookup"><span data-stu-id="17894-143">Property</span></span>                | <span data-ttu-id="17894-144">描述</span><span class="sxs-lookup"><span data-stu-id="17894-144">Description</span></span>                       | <span data-ttu-id="17894-145">預設</span><span class="sxs-lookup"><span data-stu-id="17894-145">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | <span data-ttu-id="17894-146">需要登入的確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="17894-146">Requires a confirmed email to sign in.</span></span> | <span data-ttu-id="17894-147">False</span><span class="sxs-lookup"><span data-stu-id="17894-147">false</span></span>  |
| `RequireConfirmedPhoneNumber` |  <span data-ttu-id="17894-148">需要確認的電話號碼登入。</span><span class="sxs-lookup"><span data-stu-id="17894-148">Requires a confirmed phone number to sign in.</span></span> | <span data-ttu-id="17894-149">False</span><span class="sxs-lookup"><span data-stu-id="17894-149">false</span></span>  |

## <a name="user-validation-settings"></a><span data-ttu-id="17894-150">使用者驗證設定</span><span class="sxs-lookup"><span data-stu-id="17894-150">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="17894-151">`IdentityOptions.User`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="17894-151">`IdentityOptions.User` has the following properties:</span></span>

| <span data-ttu-id="17894-152">屬性</span><span class="sxs-lookup"><span data-stu-id="17894-152">Property</span></span>                | <span data-ttu-id="17894-153">描述</span><span class="sxs-lookup"><span data-stu-id="17894-153">Description</span></span>                       | <span data-ttu-id="17894-154">預設</span><span class="sxs-lookup"><span data-stu-id="17894-154">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | <span data-ttu-id="17894-155">需要每位使用者擁有唯一的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="17894-155">Requires each User to have a unique email.</span></span> | <span data-ttu-id="17894-156">False</span><span class="sxs-lookup"><span data-stu-id="17894-156">false</span></span>  |
| `AllowedUserNameCharacters`  | <span data-ttu-id="17894-157">使用者名稱中允許的字元數。</span><span class="sxs-lookup"><span data-stu-id="17894-157">Allowed characters in the username.</span></span> | <span data-ttu-id="17894-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span><span class="sxs-lookup"><span data-stu-id="17894-158">abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+</span></span> |


## <a name="applications-cookie-settings"></a><span data-ttu-id="17894-159">應用程式的 cookie 設定</span><span class="sxs-lookup"><span data-stu-id="17894-159">Application's cookie settings</span></span>

<span data-ttu-id="17894-160">類似的密碼原則的所有應用程式的 cookie 設定可以變更在`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="17894-160">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="17894-161">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="17894-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="17894-162">在下`ConfigureServices`中`Startup`類別，您可以設定應用程式的 cookie。</span><span class="sxs-lookup"><span data-stu-id="17894-162">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="17894-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="17894-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

<span data-ttu-id="17894-164">`CookieAuthenticationOptions`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="17894-164">`CookieAuthenticationOptions` has the following properties:</span></span>

| <span data-ttu-id="17894-165">屬性</span><span class="sxs-lookup"><span data-stu-id="17894-165">Property</span></span>                | <span data-ttu-id="17894-166">描述</span><span class="sxs-lookup"><span data-stu-id="17894-166">Description</span></span>                       | <span data-ttu-id="17894-167">預設</span><span class="sxs-lookup"><span data-stu-id="17894-167">Default</span></span> |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | <span data-ttu-id="17894-168">Cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="17894-168">The name of the cookie.</span></span>  | <span data-ttu-id="17894-169">.AspNetCore.Cookies。</span><span class="sxs-lookup"><span data-stu-id="17894-169">.AspNetCore.Cookies.</span></span>  |
| `Cookie.HttpOnly`  | <span data-ttu-id="17894-170">當 true 時，不能從用戶端指令碼存取 cookie。</span><span class="sxs-lookup"><span data-stu-id="17894-170">When true, the cookie is not accessible from client-side scripts.</span></span>  |  <span data-ttu-id="17894-171">true</span><span class="sxs-lookup"><span data-stu-id="17894-171">true</span></span> |
| `ExpireTimeSpan`  | <span data-ttu-id="17894-172">控制儲存在 cookie 中的驗證票證的時間會維持有效從其建立點。</span><span class="sxs-lookup"><span data-stu-id="17894-172">Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span>  | <span data-ttu-id="17894-173">14 天</span><span class="sxs-lookup"><span data-stu-id="17894-173">14 days</span></span>  |
| `LoginPath`  | <span data-ttu-id="17894-174">未經授權的使用者時，則會重新導向至登入此路徑。</span><span class="sxs-lookup"><span data-stu-id="17894-174">When a user is unauthorized, they will be redirected to this path to login.</span></span> | <span data-ttu-id="17894-175">/ 帳戶/登入</span><span class="sxs-lookup"><span data-stu-id="17894-175">/Account/Login</span></span>  |
| `LogoutPath`  | <span data-ttu-id="17894-176">在使用者登出時，則會重新導向到這個路徑。</span><span class="sxs-lookup"><span data-stu-id="17894-176">When a user is logged out, they will be redirected to this path.</span></span>  | <span data-ttu-id="17894-177">/ 帳戶/登出</span><span class="sxs-lookup"><span data-stu-id="17894-177">/Account/Logout</span></span>  |
| `AccessDeniedPath`  | <span data-ttu-id="17894-178">當使用者失敗時的授權檢查時，則會重新導向到這個路徑。</span><span class="sxs-lookup"><span data-stu-id="17894-178">When a user fails an authorization check, they will be redirected to this path.</span></span>  |   |
| `SlidingExpiration`  | <span data-ttu-id="17894-179">若為 true，將會與新的到期時間，當目前 cookie 是多個到期視窗的中間發出新的 cookie。</span><span class="sxs-lookup"><span data-stu-id="17894-179">When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span>  | <span data-ttu-id="17894-180">/ 帳戶/AccessDenied</span><span class="sxs-lookup"><span data-stu-id="17894-180">/Account/AccessDenied</span></span> |
| `ReturnUrlParameter`  | <span data-ttu-id="17894-181">判斷當 401 未授權的狀態的程式碼變更為 302 重新導向至登入路徑時，由中介軟體附加的查詢字串參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="17894-181">Determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>  |  <span data-ttu-id="17894-182">true</span><span class="sxs-lookup"><span data-stu-id="17894-182">true</span></span> |
| `AuthenticationScheme`  | <span data-ttu-id="17894-183">這只是適用於 ASP.NET Core 相關 1.x。</span><span class="sxs-lookup"><span data-stu-id="17894-183">This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="17894-184">特定的驗證配置的邏輯名稱。</span><span class="sxs-lookup"><span data-stu-id="17894-184">The logical name for a particular authentication scheme.</span></span> |  |
| `AutomaticAuthenticate`  | <span data-ttu-id="17894-185">這個旗標僅適用於 ASP.NET Core 相關 1.x。</span><span class="sxs-lookup"><span data-stu-id="17894-185">This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="17894-186">若為 true，cookie 驗證應執行每個要求，並嘗試驗證，重新建構建立它的任何序列化的主體。</span><span class="sxs-lookup"><span data-stu-id="17894-186">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>  |  |