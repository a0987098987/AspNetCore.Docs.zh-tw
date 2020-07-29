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
ms.openlocfilehash: 5c999b426742cf75b1997f5b40223e2dda112901
ms.sourcegitcommit: 1b89fc58114a251926abadfd5c69c120f1ba12d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87160296"
---
# <a name="configure-aspnet-core-no-locidentity"></a>設定 ASP.NET CoreIdentity

ASP.NET Core 會 Identity 使用密碼原則、鎖定和 cookie 設定等設定的預設值。 這些設定可以在類別中覆寫 `Startup` 。

## <a name="no-locidentity-options"></a>Identity選項

[ Identity Options](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)類別代表可以用來設定系統的選項 Identity 。 `IdentityOptions`呼叫或**之後**，必須 `AddIdentity` 設定 `AddDefaultIdentity` 。

### <a name="claims-no-locidentity"></a>退款Identity

[ Identity Options。宣告 Identity ](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)會使用下表所示的屬性來指定[宣告 Identity 選項](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | 取得或設定用於角色宣告的宣告類型。 | [Claimtype。角色](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | 取得或設定用於安全性戳記宣告的宣告類型。 | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | 取得或設定用於使用者識別碼宣告的宣告類型。 | [Claimtype. NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | 取得或設定用於使用者名稱宣告的宣告類型。 | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>鎖定

鎖定是在[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)方法中設定：

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

上述程式碼是以範本為基礎 `Login` Identity 。 

鎖定選項設定于 `StartUp.ConfigureServices` ：

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

上述程式碼會使用預設值來設定[ Identity 選項](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 。

成功的驗證會重設失敗的存取嘗試計數，並重設時鐘。

[ Identity 選項。鎖定](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)會使用資料表中顯示的屬性來指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) 。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | 決定是否可以鎖定新的使用者。 | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | 發生鎖定時使用者鎖定的時間量。 | 5 分鐘 |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | 在使用者遭到鎖定之前，如果已啟用鎖定，則嘗試存取失敗的次數。 | 5 |

### <a name="password"></a>密碼

根據預設， Identity 密碼必須包含大寫字元、小寫字元、數位和非英數位元。 密碼長度至少必須為6個字元。

密碼是使用下列設定：

* <xref:Microsoft.AspNetCore.Identity.PasswordOptions>在中 `Startup.ConfigureServices` 。
* [ `[StringLength]` ](xref:System.ComponentModel.DataAnnotations.StringLengthAttribute) `Password` 如果 Identity [scaffold 至應用程式](xref:security/authentication/scaffold-identity)，則為屬性的屬性。 `InputModel``Password`在下列檔案中可找到屬性：
  * `Areas/Identity/Pages/Account/Register.cshtml.cs`
  * `Areas/Identity/Pages/Account/ResetPassword.cshtml.cs`

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

[ Identity Options。 Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)會使用資料表中顯示的屬性來指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) 。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | 在密碼中需要介於0-9 之間的數位。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | 密碼的最小長度。 | 6 |
| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | 需要密碼中的小寫字元。 | `true` |
| [RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | 密碼中需要非英數位元。 | `true` |
| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | 僅適用于 ASP.NET Core 2.0 或更新版本。<br><br> 需要密碼中的相異字元數。 | 1 |
| [RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | 需要密碼中的大寫字元。 | `true` |

### <a name="sign-in"></a>登入

下列程式碼會設定 `SignIn` 設定（預設值）：

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

[登入] 會使用資料表中顯示的屬性[ Identity 來指定](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) [SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) 。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | 需要已確認的電子郵件才能登入。 | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | 需要確認的電話號碼才能登入。 | `false` |

### <a name="tokens"></a>權杖

[ Identity 選項。權杖](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)會使用資料表中顯示的屬性來指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) 。

| 屬性 | 說明 |
| -------- | ----------- |
| [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | 取得或設定，用 `AuthenticatorTokenProvider` 來驗證驗證器的雙因素登入。 |
| [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | 取得或設定， `ChangeEmailTokenProvider` 用來產生電子郵件變更確認電子郵件中使用的權杖。 |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | 取得或設定， `ChangePhoneNumberTokenProvider` 用來產生變更電話號碼時使用的權杖。 |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | 取得或設定權杖提供者，用來產生帳戶確認電子郵件中使用的權杖。 |
| [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | 取得或設定用來產生密碼重設電子郵件中使用之權杖的[IUserTwoFactorTokenProvider \<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) 。 |
| [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | 用來以用來作為提供者名稱的金鑰來建立[使用者權杖提供者](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)。 |

### <a name="user"></a>User

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[ Identity Options。使用者](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)指定具有資料表中所顯示內容的[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) 。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | 使用者名稱中允許的字元。 | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | 要求每位使用者都有唯一的電子郵件。 | `false` |

### <a name="cookie-settings"></a>Cookie 設定

在中設定應用程式的 cookie `Startup.ConfigureServices` 。 呼叫或**之後**，必須呼叫[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) `AddIdentity` `AddDefaultIdentity` 。

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

如需詳細資訊，請參閱[cookieauthenticationoptions.authenticationtype](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)。

## <a name="password-hasher-options"></a>密碼 Hasher 選項

<xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions>取得和設定密碼雜湊的選項。

| 選項 | 說明 |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> | 雜湊新密碼時所使用的相容性模式。 預設為 <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3>。 雜湊密碼的第一個位元組（稱為*格式標記*）會指定用來雜湊密碼的雜湊演算法版本。 根據雜湊來驗證密碼時，方法會根據 <xref:Microsoft.AspNetCore.Identity.PasswordHasher`1.VerifyHashedPassword*> 第一個位元組來選取正確的演算法。 無論使用哪一個演算法版本來雜湊密碼，用戶端都可以進行驗證。 設定相容性模式會影響*新密碼*的雜湊。 |
| <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> | 使用 PBKDF2 來對密碼進行雜湊處理時所使用的反覆運算次數。 只有當設定為時，才會使用這個值 <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.CompatibilityMode> <xref:Microsoft.AspNetCore.Identity.PasswordHasherCompatibilityMode.IdentityV3> 。 值必須是正整數，且預設值為 `10000` 。 |

在下列範例中， <xref:Microsoft.AspNetCore.Identity.PasswordHasherOptions.IterationCount> 會設定為 `12000` 中的 `Startup.ConfigureServices` ：

```csharp
// using Microsoft.AspNetCore.Identity;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```

## <a name="globally-require-all-users-to-be-authenticated"></a>全域要求所有使用者都必須經過驗證

[!INCLUDE[](~/includes/requireAuth.md)]