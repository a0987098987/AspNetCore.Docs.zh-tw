---
title: 保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖
author: guardrex
description: 了解如何建立額外的宣告並從外部提供者的權杖。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: dc8b3e32141466a12e4eff0c86d2d4bed689afe5
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206353"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖

作者：[Luke Latham](https://github.com/guardrex)

將 ASP.NET Core 應用程式可以建立額外的宣告和外部驗證提供者，例如 Facebook、 Google、 Microsoft 及 Twitter 的語彙基元。 每個提供者會顯示其平台上，使用者的不同資訊但接收，以及將使用者資料轉換成其他宣告的模式相同。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisite"></a>必要條件

決定哪些應用程式中支援的外部驗證提供者。 每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端祕密。 如需詳細資訊，請參閱<xref:security/authentication/social/index>。 [範例應用程式](#sample-app-instructions)會使用[Google 驗證提供者](xref:security/authentication/google-logins)。

## <a name="authentication-provider-configuration"></a>驗證提供者組態

### <a name="set-the-client-id-and-client-secret"></a>設定用戶端識別碼和用戶端祕密

OAuth 驗證提供者會建立信任關係，使用用戶端識別碼和用戶端祕密應用程式。 用戶端識別碼和用戶端密碼值會針對應用程式外部驗證提供者所使用的提供者註冊應用程式時。 應用程式使用每個外部提供者必須獨立設定，提供者的用戶端識別碼和用戶端祕密。 如需詳細資訊，請參閱適用於您案例的外部驗證提供者主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Google 驗證](xref:security/authentication/google-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他驗證提供者](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

範例應用程式設定 Google 驗證提供者用戶端識別碼和由 Google 提供的用戶端祕密：

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

### <a name="establish-the-authentication-scope"></a>建立驗證範圍

指定從提供者擷取所指定的權限清單<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>。 常見的外部提供者的驗證領域會出現在下表中。

| 提供者  | 範圍                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/plus.login`                     |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

範例應用程式會新增 Google`plus.login`要求 Google + 登入權限的範圍：

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

### <a name="map-user-data-keys-and-create-claims"></a>將使用者資料的索引鍵對應，並建立宣告

在 提供者的選項，指定<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>每個索引鍵的外部提供者的 JSON 使用者資料，來讀取登入的應用程式身分識別。 如需有關宣告類型的詳細資訊，請參閱<xref:System.Security.Claims.ClaimTypes>。

範例應用程式會建立<xref:System.Security.Claims.ClaimTypes.Gender>來自宣告`gender`Google 使用者資料中的索引鍵：

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

在  <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>，則<xref:Microsoft.AspNetCore.Identity.IdentityUser>(`ApplicationUser`) 登入應用程式與<xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>。 登入程序期間<xref:Microsoft.AspNetCore.Identity.UserManager`1>可以儲存`ApplicationUser`宣告的使用者資料可從<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。

範例應用程式中`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) 建立<xref:System.Security.Claims.ClaimTypes.Gender>宣告為帶正負號的`ApplicationUser`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

### <a name="save-the-access-token"></a>儲存存取權杖

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義存取和重新整理權杖是否應該儲存在<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功授權之後。 `SaveTokens` 設定為`false`預設情況下，以減少最終的驗證 cookie 的大小。

範例應用程式設定的值`SaveTokens`要`true`在<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

當`OnPostConfirmationAsync`執行時，儲存的存取權杖 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 從外部提供者`ApplicationUser`的`AuthenticationProperties`。

範例應用程式會將儲存存取權杖：

* `OnPostConfirmationAsync` &ndash; 執行新的使用者註冊。
* `OnGetCallbackAsync` &ndash; 當先前已註冊的使用者登入應用程式時執行。

*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

### <a name="how-to-add-additional-custom-tokens"></a>如何新增額外的自訂權杖

若要示範如何新增自訂權杖，它會儲存為一部分`SaveTokens`，範例應用程式會新增<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>與目前<xref:System.DateTime>如[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)的`TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a>範例應用程式的指示

範例應用程式示範如何：

* 從 Google 取得使用者的性別和儲存性別宣告的值。
* 將 Google 存取權杖儲存在使用者的`AuthenticationProperties`。

若要使用範例應用程式：

1. 註冊應用程式，並取得有效的用戶端識別碼和 Google 驗證的用戶端祕密。 如需詳細資訊，請參閱<xref:security/authentication/google-logins>。
1. 提供用戶端識別碼和應用程式中的用戶端祕密<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>的`Startup.ConfigureServices`。
1. 執行應用程式，並要求我宣告頁面。 當使用者未登入時，應用程式重新導向至 Google。 使用 Google 登入。 Google 使用者重新導向回到應用程式 (`/Home/MyClaims`)。 使用者經過驗證，而且我的宣告頁面載入。 性別宣告是底下**使用者宣告**從 Google 取得的值。 存取權杖會出現在**驗證屬性**。

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    b36a7b09-9135-4810-b7a5-78697ff23e99
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    username@gmail.com
AspNet.Identity.SecurityStamp
    29G2TB881ATCUQFJSRFG1S0QJ0OOAWVT
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/gender
    female
http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
    Google

Authentication Properties

.Token.access_token
    bv42.Dgw...GQMv9ArLPs
.Token.token_type
    Bearer
.Token.expires_at
    2018-08-27T19:08:00.0000000+00:00
.Token.TicketCreated
    8/27/2018 6:08:00 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.issued
    Mon, 27 Aug 2018 18:08:05 GMT
.expires
    Mon, 10 Sep 2018 18:08:05 GMT
```
