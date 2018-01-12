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
# <a name="configure-identity"></a>設定身分識別

ASP.NET Core 身分識別密碼原則鎖定的時間，您可以覆寫輕鬆地在應用程式中的 cookie 設定像是應用程式中擁有通用行為`Startup`類別。

## <a name="passwords-policy"></a>密碼原則

根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字和非英數字元。 另外還有一些其他的限制。 若要簡化密碼限制，請修改`ConfigureServices`方法`Startup`應用程式的類別。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 加入`RequiredUniqueChars`屬性。 否則，選項會從 ASP.NET Core 相同 1.x。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

`IdentityOptions.Password`具有下列屬性：

| 屬性                | 描述                       | 預設 |
| ----------------------- | --------------------------------- | ------- |
| `RequireDigit`          | 需要介於 0-9 的密碼。 | true |
| `RequiredLength`        | 密碼長度下限。 | 6 |
| `RequireNonAlphanumeric`| 需要密碼中的非英數字元。 | true |
| `RequireUppercase`      | 需要密碼中的大寫字元。 | true |
| `RequireLowercase`      | 需要密碼中的小寫字元。 | true |
| `RequiredUniqueChars`   | 需要密碼中不同的字元的數。 | 1 |


## <a name="users-lockout"></a>使用者的鎖定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

`IdentityOptions.Lockout`具有下列屬性：

| 屬性                | 描述                       | 預設 |
| ----------------------- | --------------------------------- | ------- |
| `DefaultLockoutTimeSpan` | 等待時間總數使用者遭到鎖定時鎖定。  | 5 分鐘  |
| `MaxFailedAccessAttempts` | 失敗的存取嘗試，直到使用者鎖定，如果已啟用鎖定數目。  | 5 |
| `AllowedForNewUsers` | 決定是否新的使用者可能會鎖定。  | true |

## <a name="sign-in-settings"></a>登入設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

`IdentityOptions.SignIn`具有下列屬性：

| 屬性                | 描述                       | 預設 |
| ----------------------- | --------------------------------- | ------- |
| `RequireConfirmedEmail` | 需要登入的確認電子郵件。 | False  |
| `RequireConfirmedPhoneNumber` |  需要確認的電話號碼登入。 | False  |

## <a name="user-validation-settings"></a>使用者驗證設定

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

`IdentityOptions.User`具有下列屬性：

| 屬性                | 描述                       | 預設 |
| ----------------------- | --------------------------------- | ------- |
| `RequireUniqueEmail`  | 需要每位使用者擁有唯一的電子郵件。 | False  |
| `AllowedUserNameCharacters`  | 使用者名稱中允許的字元數。 | abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+ |


## <a name="applications-cookie-settings"></a>應用程式的 cookie 設定

類似的密碼原則的所有應用程式的 cookie 設定可以變更在`Startup`類別。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

在下`ConfigureServices`中`Startup`類別，您可以設定應用程式的 cookie。

[!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

`CookieAuthenticationOptions`具有下列屬性：

| 屬性                | 描述                       | 預設 |
| ----------------------- | --------------------------------- | ------- |
| `Cookie.Name`  | Cookie 的名稱。  | .AspNetCore.Cookies。  |
| `Cookie.HttpOnly`  | 當 true 時，不能從用戶端指令碼存取 cookie。  |  true |
| `ExpireTimeSpan`  | 控制儲存在 cookie 中的驗證票證的時間會維持有效從其建立點。  | 14 天  |
| `LoginPath`  | 未經授權的使用者時，則會重新導向至登入此路徑。 | / 帳戶/登入  |
| `LogoutPath`  | 在使用者登出時，則會重新導向到這個路徑。  | / 帳戶/登出  |
| `AccessDeniedPath`  | 當使用者失敗時的授權檢查時，則會重新導向到這個路徑。  |   |
| `SlidingExpiration`  | 若為 true，將會與新的到期時間，當目前 cookie 是多個到期視窗的中間發出新的 cookie。  | / 帳戶/AccessDenied |
| `ReturnUrlParameter`  | 判斷當 401 未授權的狀態的程式碼變更為 302 重新導向至登入路徑時，由中介軟體附加的查詢字串參數的名稱。  |  true |
| `AuthenticationScheme`  | 這只是適用於 ASP.NET Core 相關 1.x。 特定的驗證配置的邏輯名稱。 |  |
| `AutomaticAuthenticate`  | 這個旗標僅適用於 ASP.NET Core 相關 1.x。 若為 true，cookie 驗證應執行每個要求，並嘗試驗證，重新建構建立它的任何序列化的主體。  |  |