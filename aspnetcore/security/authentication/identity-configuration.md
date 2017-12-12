---
title: "設定 ASP.NET Core 身分識別"
author: AdrienTorris
description: "了解 ASP.NET Core 識別預設值，並設定要使用自訂值的各種識別屬性。"
keywords: "ASP.NET Core，身分識別驗證安全性"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 2861ca474e7e82da81943966394a92040ce96ab8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="configure-identity"></a><span data-ttu-id="1016b-104">設定身分識別</span><span class="sxs-lookup"><span data-stu-id="1016b-104">Configure Identity</span></span>

<span data-ttu-id="1016b-105">ASP.NET Core 識別有一些您可以覆寫輕鬆地在應用程式中的預設行為`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="1016b-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's `Startup` class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="1016b-106">密碼原則</span><span class="sxs-lookup"><span data-stu-id="1016b-106">Passwords policy</span></span>

<span data-ttu-id="1016b-107">根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字和非英數字元。</span><span class="sxs-lookup"><span data-stu-id="1016b-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, a digit, and a non-alphanumeric character.</span></span> <span data-ttu-id="1016b-108">另外還有一些其他的限制。</span><span class="sxs-lookup"><span data-stu-id="1016b-108">There are also some other restrictions.</span></span> <span data-ttu-id="1016b-109">如果您想要簡化密碼限制，您可以`Startup`應用程式的類別。</span><span class="sxs-lookup"><span data-stu-id="1016b-109">If you want to simplify password restrictions, you can do that in the `Startup` class of your application.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1016b-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1016b-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1016b-111">ASP.NET Core 2.0 加入`RequiredUniqueChars`屬性。</span><span class="sxs-lookup"><span data-stu-id="1016b-111">ASP.NET Core 2.0 added the `RequiredUniqueChars` property.</span></span> <span data-ttu-id="1016b-112">否則，選項會從 ASP.NET Core 相同 1.x。</span><span class="sxs-lookup"><span data-stu-id="1016b-112">Otherwise, the options are the same from ASP.NET Core 1.x.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1016b-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1016b-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

<span data-ttu-id="1016b-114">`IdentityOptions.Password`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1016b-114">`IdentityOptions.Password` has the following properties:</span></span>
* <span data-ttu-id="1016b-115">`RequireDigit`： 需要密碼中的 0-9 之間的數字。</span><span class="sxs-lookup"><span data-stu-id="1016b-115">`RequireDigit`: Requires a number between 0-9 in the password.</span></span> <span data-ttu-id="1016b-116">預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-116">Defaults to true.</span></span>
* <span data-ttu-id="1016b-117">`RequiredLength`： 最小密碼長度。</span><span class="sxs-lookup"><span data-stu-id="1016b-117">`RequiredLength`: The minimum length of the password.</span></span> <span data-ttu-id="1016b-118">預設為 6。</span><span class="sxs-lookup"><span data-stu-id="1016b-118">Defaults to 6.</span></span>
* <span data-ttu-id="1016b-119">`RequireNonAlphanumeric`： 需要非英數字元的密碼。</span><span class="sxs-lookup"><span data-stu-id="1016b-119">`RequireNonAlphanumeric`: Requires a non-alphanumeric character in the password.</span></span> <span data-ttu-id="1016b-120">預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-120">Defaults to true.</span></span>
* <span data-ttu-id="1016b-121">`RequireUppercase`： 需要密碼中的大寫字元。</span><span class="sxs-lookup"><span data-stu-id="1016b-121">`RequireUppercase`: Requires an upper case character in the password.</span></span> <span data-ttu-id="1016b-122">預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-122">Defaults to true.</span></span>
* <span data-ttu-id="1016b-123">`RequireLowercase`： 需要密碼中的小寫字元。</span><span class="sxs-lookup"><span data-stu-id="1016b-123">`RequireLowercase`: Requires a lower case character in the password.</span></span> <span data-ttu-id="1016b-124">預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-124">Defaults to true.</span></span>
* <span data-ttu-id="1016b-125">`RequiredUniqueChars`： 需要密碼中不同的字元的數。</span><span class="sxs-lookup"><span data-stu-id="1016b-125">`RequiredUniqueChars`: Requires the number of distinct characters in the password.</span></span> <span data-ttu-id="1016b-126">預設值為 1。</span><span class="sxs-lookup"><span data-stu-id="1016b-126">Defaults to 1.</span></span>


## <a name="users-lockout"></a><span data-ttu-id="1016b-127">使用者的鎖定</span><span class="sxs-lookup"><span data-stu-id="1016b-127">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

<span data-ttu-id="1016b-128">`IdentityOptions.Lockout`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1016b-128">`IdentityOptions.Lockout` has the following properties:</span></span>
* <span data-ttu-id="1016b-129">`DefaultLockoutTimeSpan`： 的時間量的使用者便會鎖定發生鎖定時。</span><span class="sxs-lookup"><span data-stu-id="1016b-129">`DefaultLockoutTimeSpan`: The amount of time a user is locked out when a lockout occurs.</span></span> <span data-ttu-id="1016b-130">預設為 5 分鐘。</span><span class="sxs-lookup"><span data-stu-id="1016b-130">Defaults to 5 minutes.</span></span>
* <span data-ttu-id="1016b-131">`MaxFailedAccessAttempts`： 失敗的存取嘗試次數直到使用者鎖定，如果已啟用鎖定。</span><span class="sxs-lookup"><span data-stu-id="1016b-131">`MaxFailedAccessAttempts`: The number of failed access attempts until a user is locked out, if lockout is enabled.</span></span> <span data-ttu-id="1016b-132">預設為 5。</span><span class="sxs-lookup"><span data-stu-id="1016b-132">Defaults to 5.</span></span>
* <span data-ttu-id="1016b-133">`AllowedForNewUsers`： 決定是否新的使用者可能會鎖定。預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-133">`AllowedForNewUsers`: Determines if a new user can be locked out. Defaults to true.</span></span>


## <a name="sign-in-settings"></a><span data-ttu-id="1016b-134">登入設定</span><span class="sxs-lookup"><span data-stu-id="1016b-134">Sign in settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

<span data-ttu-id="1016b-135">`IdentityOptions.SignIn`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1016b-135">`IdentityOptions.SignIn` has the following properties:</span></span>
* <span data-ttu-id="1016b-136">`RequireConfirmedEmail`： 需要登入的確認電子郵件。</span><span class="sxs-lookup"><span data-stu-id="1016b-136">`RequireConfirmedEmail`: Requires a confirmed email to sign in.</span></span> <span data-ttu-id="1016b-137">預設為 false。</span><span class="sxs-lookup"><span data-stu-id="1016b-137">Defaults to false.</span></span>
* <span data-ttu-id="1016b-138">`RequireConfirmedPhoneNumber`： 需要確認的電話號碼登入。</span><span class="sxs-lookup"><span data-stu-id="1016b-138">`RequireConfirmedPhoneNumber`: Requires a confirmed phone number to sign in.</span></span> <span data-ttu-id="1016b-139">預設為 false。</span><span class="sxs-lookup"><span data-stu-id="1016b-139">Defaults to false.</span></span>


## <a name="user-validation-settings"></a><span data-ttu-id="1016b-140">使用者驗證設定</span><span class="sxs-lookup"><span data-stu-id="1016b-140">User validation settings</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

<span data-ttu-id="1016b-141">`IdentityOptions.User`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1016b-141">`IdentityOptions.User` has the following properties:</span></span>
* <span data-ttu-id="1016b-142">`RequireUniqueEmail`： 需要每位使用者擁有唯一的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="1016b-142">`RequireUniqueEmail`: Requires each User to have a unique email.</span></span> <span data-ttu-id="1016b-143">預設為 false。</span><span class="sxs-lookup"><span data-stu-id="1016b-143">Defaults to false.</span></span>
* <span data-ttu-id="1016b-144">`AllowedUserNameCharacters`： 允許的使用者名稱中的字元。</span><span class="sxs-lookup"><span data-stu-id="1016b-144">`AllowedUserNameCharacters`: Allowed characters in the username.</span></span> <span data-ttu-id="1016b-145">預設為 abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+。</span><span class="sxs-lookup"><span data-stu-id="1016b-145">Defaults to abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+.</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="1016b-146">應用程式的 cookie 設定</span><span class="sxs-lookup"><span data-stu-id="1016b-146">Application's cookie settings</span></span>

<span data-ttu-id="1016b-147">類似的密碼原則的所有應用程式的 cookie 設定可以變更在`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="1016b-147">Like the passwords policy, all the settings of the application's cookie can be changed in the `Startup` class.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1016b-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1016b-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1016b-149">在下`ConfigureServices`中`Startup`類別，您可以設定應用程式的 cookie。</span><span class="sxs-lookup"><span data-stu-id="1016b-149">Under `ConfigureServices` in the `Startup` class, you can configure the application's cookie.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1016b-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1016b-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

<span data-ttu-id="1016b-151">`CookieAuthenticationOptions`具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="1016b-151">`CookieAuthenticationOptions` has the following properties:</span></span>
* <span data-ttu-id="1016b-152">`Cookie.Name`: Cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="1016b-152">`Cookie.Name`: The name of the cookie.</span></span> <span data-ttu-id="1016b-153">預設值。AspNetCore.Cookies。</span><span class="sxs-lookup"><span data-stu-id="1016b-153">Defaults to .AspNetCore.Cookies.</span></span>
* <span data-ttu-id="1016b-154">`Cookie.HttpOnly`： 若為 true，不能從用戶端指令碼存取 cookie。</span><span class="sxs-lookup"><span data-stu-id="1016b-154">`Cookie.HttpOnly`: When true, the cookie is not accessible from client-side scripts.</span></span> <span data-ttu-id="1016b-155">預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-155">Defaults to true.</span></span>
* <span data-ttu-id="1016b-156">`ExpireTimeSpan`： 控制儲存在 cookie 中的驗證票證的時間會維持有效從其建立點。</span><span class="sxs-lookup"><span data-stu-id="1016b-156">`ExpireTimeSpan`: Controls how much time the authentication ticket stored in the cookie will remain valid from the point it is created.</span></span> <span data-ttu-id="1016b-157">預設為 14 天。</span><span class="sxs-lookup"><span data-stu-id="1016b-157">Defaults to 14 days.</span></span>
* <span data-ttu-id="1016b-158">`LoginPath`： 未經授權的使用者時，則會重新導向至登入此路徑。</span><span class="sxs-lookup"><span data-stu-id="1016b-158">`LoginPath`: When a user is unauthorized, they will be redirected to this path to login.</span></span> <span data-ttu-id="1016b-159">預設為/帳戶/登入。</span><span class="sxs-lookup"><span data-stu-id="1016b-159">Defaults to /Account/Login.</span></span>
* <span data-ttu-id="1016b-160">`LogoutPath`： 當使用者登出時，則會重新導向到這個路徑。</span><span class="sxs-lookup"><span data-stu-id="1016b-160">`LogoutPath`: When a user is logged out, they will be redirected to this path.</span></span> <span data-ttu-id="1016b-161">預設為/帳戶/登出。</span><span class="sxs-lookup"><span data-stu-id="1016b-161">Defaults to /Account/Logout.</span></span>
* <span data-ttu-id="1016b-162">`AccessDeniedPath`： 當使用者失敗時的授權檢查，則會重新導向到這個路徑。</span><span class="sxs-lookup"><span data-stu-id="1016b-162">`AccessDeniedPath`: When a user fails an authorization check, they will be redirected to this path.</span></span> <span data-ttu-id="1016b-163">預設為/帳戶/AccessDenied。</span><span class="sxs-lookup"><span data-stu-id="1016b-163">Defaults to /Account/AccessDenied.</span></span>
* <span data-ttu-id="1016b-164">`SlidingExpiration`： 若為 true，將會與新的到期時間，超過正中間時，透過過期視窗目前 cookie 時發出新的 cookie。</span><span class="sxs-lookup"><span data-stu-id="1016b-164">`SlidingExpiration`: When true, a new cookie will be issued with a new expiration time when the current cookie is more than halfway through the expiration window.</span></span> <span data-ttu-id="1016b-165">預設為 true。</span><span class="sxs-lookup"><span data-stu-id="1016b-165">Defaults to true.</span></span>
* <span data-ttu-id="1016b-166">`ReturnUrlParameter`: ReturnUrlParameter 判斷當 401 未授權的狀態的程式碼變更為 302 重新導向至登入路徑時，由中介軟體附加的查詢字串參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="1016b-166">`ReturnUrlParameter`: The ReturnUrlParameter determines the name of the query string parameter which is appended by the middleware when a 401 Unauthorized status code is changed to a 302 redirect onto the login path.</span></span>
* <span data-ttu-id="1016b-167">`AuthenticationScheme`： 這只是適用於 ASP.NET Core 相關 1.x。</span><span class="sxs-lookup"><span data-stu-id="1016b-167">`AuthenticationScheme`: This is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="1016b-168">特定的驗證配置的邏輯名稱。</span><span class="sxs-lookup"><span data-stu-id="1016b-168">The logical name for a particular authentication scheme.</span></span>
* <span data-ttu-id="1016b-169">`AutomaticAuthenticate`： 此旗標僅適用於 ASP.NET Core 相關 1.x。</span><span class="sxs-lookup"><span data-stu-id="1016b-169">`AutomaticAuthenticate`: This flag is only relevant for ASP.NET Core 1.x.</span></span> <span data-ttu-id="1016b-170">若為 true，cookie 驗證應執行每個要求，並嘗試驗證，重新建構建立它的任何序列化的主體。</span><span class="sxs-lookup"><span data-stu-id="1016b-170">When true, cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

