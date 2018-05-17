---
title: 設定 ASP.NET Core 身分識別
author: AdrienTorris
description: 了解 ASP.NET Core 識別預設值，並了解如何設定 Identity 屬性以使用自訂值。
manager: wpickett
ms.author: scaddie
ms.date: 03/06/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-configuration
ms.openlocfilehash: 511c39db2bb4d3b215a1037c52f6c4f89b48ff7d
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="configure-aspnet-core-identity"></a>設定 ASP.NET Core 身分識別

ASP.NET Core 身分識別會使用預設組態的密碼原則、 鎖定的時間和 cookie 設定等設定。 這些設定會覆寫在應用程式之`Startup`類別。

## <a name="identity-options"></a>識別選項

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)類別代表可以用來設定身分識別系統的選項。

### <a name="claims-identity"></a>宣告識別

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)指定[ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | 取得或設定用於角色宣告的宣告型別。 | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | 取得或設定用於安全性戳記宣告的宣告型別。 | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | 取得或設定用於使用者的識別項宣告的宣告型別。 | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | 取得或設定用於使用者名稱宣告的宣告型別。 | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>鎖定

鎖定使用者一段時間之後指定失敗的存取嘗試次數 (預設： 5 存取嘗試失敗後的 5 分鐘鎖定)。 驗證成功重設失敗的存取嘗試次數和重設時鐘。

下列範例顯示的預設值：

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,39-42,50-52)]

確認[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync)設定`lockoutOnFailure`至`true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | 決定是否新的使用者可能會鎖定。 | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | 等待時間總數使用者遭到鎖定時鎖定。 | 5 分鐘 |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | 失敗的存取嘗試，直到使用者鎖定，如果已啟用鎖定數目。 | 5 |

### <a name="password"></a>密碼

根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字和非英數字元。 密碼必須至少為六個字元。 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)中可改變`Startup.ConfigureServices`。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

ASP.NET Core 2.0 加入[RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars)屬性。 否則，選項會與 ASP.NET Core 相同 1.x。

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

---

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | 需要介於 0-9 的密碼。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | 密碼長度下限。 | 6 |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | 僅適用於 ASP.NET Core 2.0 或更新版本。<br><br> 需要密碼中不同的字元的數。 | 1 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | 需要密碼中的小寫字元。 | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | 需要密碼中的非英數字元。 | `true` |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | 需要密碼中的大寫字元。 | `true` |

### <a name="sign-in"></a>登入

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)]

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)指定[SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | 需要登入的確認電子郵件。 | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | 需要確認的電話號碼登入。 | `false` |

### <a name="tokens"></a>語彙基元

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)表所示的屬性。


|                                                        屬性                                                         |                                                                                      描述                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       取得或設定`AuthenticatorTokenProvider`用來驗證兩個要素登入與驗證器。                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     取得或設定`ChangeEmailTokenProvider`用來產生電子郵件變更確認電子郵件中使用 token。                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      取得或設定`ChangePhoneNumberTokenProvider`用來產生語彙基元變更電話號碼時使用。                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             取得或設定用來產生用於帳戶確認電子郵件的權杖的權杖提供者。                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | 取得或設定[IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)用來產生密碼重設電子郵件中使用 token。 |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                用來建構[使用者權杖提供者](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)具有索引鍵做為提供者的名稱。                 |

### <a name="user"></a>使用者

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,48-52)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)指定[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | 使用者名稱中允許的字元數。 | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | 需要每位使用者擁有唯一的電子郵件。 | `false` |

## <a name="cookie-settings"></a>Cookie 設定

設定中的應用程式的 cookie `Startup.ConfigureServices`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

---

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)具有下列屬性：

|                                                               屬性                                                               |                                                                                                                                                           描述                                                                                                                                                            |
|--------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath)       |                                                                 通知處理常式，它應該變更傳出<em>403 禁止</em>狀態程式碼到<em>302 重新導向</em>到指定的路徑。<br><br>預設值是 `/Account/AccessDenied`。                                                                  |
|             [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme)              |                                                                                                                僅適用於 ASP.NET Core 1.x。<br><br> 特定的驗證配置的邏輯名稱。                                                                                                                |
|            [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate)             |                                                                       僅適用於 ASP.NET Core 1.x。<br><br> 若為 true，cookie 驗證應執行每個要求，並嘗試驗證，重新建構建立它的任何序列化的主體。                                                                        |
|               [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge)                |                                              僅適用於 ASP.NET Core 1.x。<br><br> 如果為 true，驗證中介軟體會處理自動挑戰。 如果為 false，驗證中介軟體僅改變回應明確指出時`AuthenticationScheme`。                                               |
|               [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer)               |                                                             取得或設定用於所建立的任何宣告的簽發者 (繼承自[AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions))。                                                             |
|                             [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain)                              |                                                                                                                                             要與 cookie 產生關聯的網域。                                                                                                                                             |
|                         [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration)                          |                 取得或設定 HTTP cookie (而不驗證 cookie) 的使用期限。 這個屬性會覆寫[ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan)。 它不應該用於 CookieAuthentication 的內容。                  |
|                           [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly)                            |                                                                                                               指出是否可供用戶端指令碼存取 cookie。<br><br>預設值是 `true`。                                                                                                                |
|                               [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name)                                |                                                                                                                            Cookie 的名稱。<br><br>預設值是 `.AspNetCore.Cookies`。                                                                                                                            |
|                               [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path)                                |                                                                                                                                                         Cookie 路徑。                                                                                                                                                         |
|                           [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite)                            |                                                                                           `SameSite` Cookie 的屬性。<br><br>預設值是[SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode)。                                                                                            |
|                       [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy)                        |                                                   [CookieSecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy)組態。<br><br>預設值是[CookieSecurePolicy.SameAsRequest](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy)。                                                    |
|                  [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain)                   |                                                                                                                      僅適用於 ASP.NET Core 1.x。<br><br> 其中提供 cookie 的網域名稱。                                                                                                                       |
|                [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly)                 |                                                                                       僅適用於 ASP.NET Core 1.x。<br><br> 旗標，指出是否 cookie 應該只能由伺服器存取。<br><br>預設值是 `true`。                                                                                        |
|                    [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath)                     |                                                                                                                  僅適用於 ASP.NET Core 1.x。<br><br> 用來隔離在相同的主機名稱上執行的應用程式。                                                                                                                   |
|                  [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure)                   | 僅適用於 ASP.NET Core 1.x。<br><br> 旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)、 HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或為要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。<br><br>預設值是 `CookieSecurePolicy.SameAsRequest`。 |
|          [CookieManager](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.cookiemanager)          |                                                                                                                         用來從要求取得 cookie 或設定回應上的元件。                                                                                                                          |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider) |                                                                             如果設定，請使用提供者[CookieAuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationhandler)資料保護。                                                                             |
|                      [描述](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description)                       |                                                                                                僅適用於 ASP.NET Core 1.x。<br><br> 其他資訊才可以提供給應用程式的驗證類型的詳細資訊。                                                                                                |
|                 [事件](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events)                 |                                                                                                      處理常式會呼叫提供者提供的應用程式控制項在何處發生處理特定點上的方法。                                                                                                       |
|                 [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype)                 |                                                            如果設定，此服務来取得的類型`Events`執行個體，而不是屬性 (繼承自[AuthenticationSchemeOptions](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions))。                                                            |
|         [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan)         |                                                                                      控制多少時間儲存在 cookie 會維持有效期限自其建立點驗證票證。<br><br>預設值為 14 天。                                                                                       |
|              [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath)              |                                                                                                       未經授權的使用者時，它們是重新導向至登入此路徑。<br><br>預設值是 `/Account/Login`。                                                                                                       |
|             [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath)             |                                                                                                            當使用者登出時，它們是重新導向到這個路徑。<br><br>預設值是 `/Account/Logout`。                                                                                                            |
|     [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter)     |                                              決定是由中介軟體附加的查詢字串參數名稱時<em>401 未授權</em>狀態碼變更為<em>302 重新導向</em>到登入路徑。<br><br>預設值是 `ReturnUrl`。                                              |
|           [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore)           |                                                                                                                              跨要求儲存識別要在其中選擇性容器。                                                                                                                               |
|      [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration)      |                                                                           若為 true，新的 cookie 會發出新的到期時間，當目前 cookie 是多個到期視窗的中間。<br><br>預設值是 `true`。                                                                           |
|       [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat)       |                                                                                                 `TicketDataFormat`用來保護且取消保護識別和儲存在 cookie 值中的其他屬性。                                                                                                  |

