---
title: 設定 ASP.NET Core 身分識別
author: AdrienTorris
description: 了解 ASP.NET Core 身分識別的預設值，並了解如何設定身分識別屬性，以使用自訂的值。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-configuration
ms.openlocfilehash: 0faab001b981c79f6afa16b2a8cf80c1ef141b11
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011296"
---
# <a name="configure-aspnet-core-identity"></a>設定 ASP.NET Core 身分識別

ASP.NET Core 身分識別設定，例如密碼原則、 鎖定和 cookie 組態使用預設值。 這些設定可以覆寫在`Startup`類別。

## <a name="identity-options"></a>識別選項

[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)類別代表可用來設定身分識別系統的選項。 `IdentityOptions` 必須設定**之後**呼叫`AddIdentity`或`AddDefaultIdentity`。

### <a name="claims-identity"></a>宣告識別

[IdentityOptions.ClaimsIdentity](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity)指定[ClaimsIdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions)與下表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RoleClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | 取得或設定用於角色宣告的宣告類型。 | [ClaimTypes.Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [SecurityStampClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | 取得或設定用於安全性戳記宣告的宣告類型。 | `AspNet.Identity.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | 取得或設定用來將使用者識別碼宣告的宣告類型。 | [ClaimTypes.NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | 取得或設定用於使用者名稱宣告的宣告類型。 | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>鎖定

鎖定在中設定[PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_Identity_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_)方法：

[!code-csharp[](identity-configuration/sample/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

上述程式碼根據`Login`識別範本。 

在 設定鎖定選項`StartUp.ConfigureServices`:

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

上述程式碼集[IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)具有預設值。

驗證成功重設失敗的存取嘗試次數和重設時鐘。

[IdentityOptions.Lockout](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout)指定[LockoutOptions](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [AllowedForNewUsers](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | 決定是否新的使用者可能會鎖定。 | `true` |
| [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | 時間長度使用者遭到鎖定時的鎖定，就會發生。 | 5 分鐘 |
| [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | 失敗的存取嘗試，直到使用者遭到鎖定，如果已啟用鎖定數目。 | 5 |

### <a name="password"></a>密碼

根據預設，身分識別會需要密碼包含大寫字元、 小寫字元、 數字、 和非英數字元。 密碼必須至少六個字元。 [PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)中可設定`Startup.ConfigureServices`。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-37,50-52)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-65,84)]

::: moniker-end

[IdentityOptions.Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password)指定[PasswordOptions](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RequireDigit](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | 需要介於 0-9 密碼中的數字。 | `true` |
| [RequiredLength](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | 密碼長度下限。 | 6 |

::: moniker range=">= aspnetcore-2.0"

| [RequiredUniqueChars](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) |僅適用於 ASP.NET Core 2.0 或更新版本。<br><br> 需要密碼中的不同字元的數。 |1 |

::: moniker-end

| [RequireLowercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) |需要密碼中的小寫字元。 | `true` | |[RequireNonAlphanumeric](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) |需要密碼的非英數字元。 | `true` | |[RequireUppercase](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) |需要密碼以大寫字元。 | `true` |

### <a name="sign-in"></a>登入

下列程式碼設定`SignIn`設定 （預設值）：

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?range=29-30,44-46,50-52)] 

::: moniker-end

[IdentityOptions.SignIn](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin)指定[SignInOptions](/dotnet/api/microsoft.aspnetcore.identity.signinoptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [RequireConfirmedEmail](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | 需要一個已確認的電子郵件來登入。 | `false` |
| [RequireConfirmedPhoneNumber](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | 需要確認的電話號碼，以登入。 | `false` |

### <a name="tokens"></a>語彙基元

[IdentityOptions.Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens)指定[TokenOptions](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions)表所示的屬性。


|                                                        屬性                                                         |                                                                                      描述                                                                                      |
|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     [AuthenticatorTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider)     |                                       取得或設定`AuthenticatorTokenProvider`用來驗證兩個要素登入與驗證器。                                       |
|       [ChangeEmailTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider)       |                                     取得或設定`ChangeEmailTokenProvider`用來產生電子郵件變更確認電子郵件中所使用的權杖。                                     |
| [ChangePhoneNumberTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) |                                      取得或設定`ChangePhoneNumberTokenProvider`用來產生變更電話號碼時，使用的權杖。                                      |
| [EmailConfirmationTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) |                                             取得或設定用來產生帳戶確認電子郵件中所使用的權杖的權杖提供者。                                              |
|     [PasswordResetTokenProvider](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider)     | 取得或設定[IUserTwoFactorTokenProvider<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1)用來產生密碼重設電子郵件中所使用的權杖。 |
|                    [ProviderMap](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap)                    |                用來建構[使用者的權杖提供者](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor)具有索引鍵做為提供者的名稱。                 |

### <a name="user"></a>使用者

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[IdentityOptions.User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user)指定[UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions)表所示的屬性。

| 屬性 | 描述 | 預設 |
| -------- | ----------- | :-----: |
| [AllowedUserNameCharacters](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | 在 使用者名稱中允許的字元。 | abcdefghijklmnopqrstuvwxyz<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-._@+ |
| [RequireUniqueEmail](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | 要求每個使用者必須具有唯一的電子郵件。 | `false` |

### <a name="cookie-settings"></a>Cookie 設定

設定中的應用程式的 cookie `Startup.ConfigureServices`。 [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__)必須呼叫**之後**呼叫`AddIdentity`或`AddDefaultIdentity`。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-Configuration/Startup.cs?name=snippet_configurecookie)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?range=58-59,72-80,84)]

::: moniker-end

如需詳細資訊，請參閱 < [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions)。
