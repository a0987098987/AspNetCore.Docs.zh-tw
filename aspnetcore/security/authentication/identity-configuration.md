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
# <a name="configure-identity"></a>設定身分識別

ASP.NET Core 識別有一些您可以覆寫輕鬆地在應用程式中的預設行為`Startup`類別。

## <a name="passwords-policy"></a>密碼原則

根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字和非英數字元。 另外還有一些其他的限制。 如果您想要簡化密碼限制，您可以`Startup`應用程式的類別。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 加入`RequiredUniqueChars`屬性。 否則，選項會從 ASP.NET Core 相同 1.x。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`具有下列屬性：
* `RequireDigit`： 需要密碼中的 0-9 之間的數字。 預設為 true。
* `RequiredLength`： 最小密碼長度。 預設為 6。
* `RequireNonAlphanumeric`： 需要非英數字元的密碼。 預設為 true。
* `RequireUppercase`： 需要密碼中的大寫字元。 預設為 true。
* `RequireLowercase`： 需要密碼中的小寫字元。 預設為 true。
* `RequiredUniqueChars`： 需要密碼中不同的字元的數。 預設值為 1。


## <a name="users-lockout"></a>使用者的鎖定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`具有下列屬性：
* `DefaultLockoutTimeSpan`： 的時間量的使用者便會鎖定發生鎖定時。 預設為 5 分鐘。
* `MaxFailedAccessAttempts`： 失敗的存取嘗試次數直到使用者鎖定，如果已啟用鎖定。 預設為 5。
* `AllowedForNewUsers`： 決定是否新的使用者可能會鎖定。預設為 true。


## <a name="sign-in-settings"></a>登入設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`具有下列屬性：
* `RequireConfirmedEmail`： 需要登入的確認電子郵件。 預設為 false。
* `RequireConfirmedPhoneNumber`： 需要確認的電話號碼登入。 預設為 false。


## <a name="user-validation-settings"></a>使用者驗證設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`具有下列屬性：
* `RequireUniqueEmail`： 需要每位使用者擁有唯一的電子郵件。 預設為 false。
* `AllowedUserNameCharacters`： 允許的使用者名稱中的字元。 預設為 abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+。

## <a name="applications-cookie-settings"></a>應用程式的 cookie 設定

類似的密碼原則的所有應用程式的 cookie 設定可以變更在`Startup`類別。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

在下`ConfigureServices`中`Startup`類別，您可以設定應用程式的 cookie。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

--- 

`CookieAuthenticationOptions`具有下列屬性：
* `Cookie.Name`: Cookie 名稱。 預設值。AspNetCore.Cookies。
* `Cookie.HttpOnly`： 若為 true，不能從用戶端指令碼存取 cookie。 預設為 true。
* `ExpireTimeSpan`： 控制儲存在 cookie 中的驗證票證的時間會維持有效從其建立點。 預設為 14 天。
* `LoginPath`： 未經授權的使用者時，則會重新導向至登入此路徑。 預設為/帳戶/登入。
* `LogoutPath`： 當使用者登出時，則會重新導向到這個路徑。 預設為/帳戶/登出。
* `AccessDeniedPath`： 當使用者失敗時的授權檢查，則會重新導向到這個路徑。 預設為/帳戶/AccessDenied。
* `SlidingExpiration`： 若為 true，將會與新的到期時間，超過正中間時，透過過期視窗目前 cookie 時發出新的 cookie。 預設為 true。
* `ReturnUrlParameter`: ReturnUrlParameter 判斷當 401 未授權的狀態的程式碼變更為 302 重新導向至登入路徑時，由中介軟體附加的查詢字串參數的名稱。
* `AuthenticationScheme`： 這只是適用於 ASP.NET Core 相關 1.x。 特定的驗證配置的邏輯名稱。
* `AutomaticAuthenticate`： 此旗標僅適用於 ASP.NET Core 相關 1.x。 若為 true，cookie 驗證應執行每個要求，並嘗試驗證，重新建構建立它的任何序列化的主體。

