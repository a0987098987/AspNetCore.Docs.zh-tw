---
title: 保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖
author: guardrex
description: 了解如何建立額外的宣告並從外部提供者的權杖。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: e18287e5a4171b3f7a6daa122111448b8447c1da
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874849"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖

作者：[Luke Latham](https://github.com/guardrex)

將 ASP.NET Core 應用程式可以建立額外的宣告和外部驗證提供者，例如 Facebook、 Google、 Microsoft 及 Twitter 的語彙基元。 每個提供者會顯示其平台上，使用者的不同資訊但接收，以及將使用者資料轉換成其他宣告的模式相同。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

決定哪些應用程式中支援的外部驗證提供者。 每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端祕密。 如需詳細資訊，請參閱 <xref:security/authentication/social/index>。 範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。

## <a name="set-the-client-id-and-client-secret"></a>設定用戶端識別碼和用戶端祕密

OAuth 驗證提供者會建立信任關係，使用用戶端識別碼和用戶端祕密應用程式。 用戶端識別碼和用戶端密碼值會針對應用程式外部驗證提供者所使用的提供者註冊應用程式時。 應用程式使用每個外部提供者必須獨立設定，提供者的用戶端識別碼和用戶端祕密。 如需詳細資訊，請參閱適用於您案例的外部驗證提供者主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Google 驗證](xref:security/authentication/google-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他驗證提供者](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

範例應用程式設定 Google 驗證提供者用戶端識別碼和由 Google 提供的用戶端祕密：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>建立驗證範圍

指定從提供者擷取所指定的權限清單<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>。 常見的外部提供者的驗證領域會出現在下表中。

| 提供者  | 範圍                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

在範例應用程式，Google`userinfo.profile`由架構自動新增範圍時<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>上呼叫<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>。 如果應用程式需要其他範圍，請將它們加入選項。 在下列範例中，Google`https://www.googleapis.com/auth/user.birthday.read`以擷取使用者的生日新增範圍：

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>將使用者資料的索引鍵對應，並建立宣告

在 提供者的選項，指定<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>或<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>每個索引鍵/子機碼的外部提供者的 JSON 使用者資料，來讀取登入的應用程式身分識別。 如需有關宣告類型的詳細資訊，請參閱<xref:System.Security.Claims.ClaimTypes>。

範例應用程式會建立地區設定 (`urn:google:locale`) 和圖片 (`urn:google:picture`) 來自宣告`locale`和`picture`Google 使用者資料中的索引鍵：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

在  <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>，則<xref:Microsoft.AspNetCore.Identity.IdentityUser>(`ApplicationUser`) 登入應用程式與<xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>。 登入程序期間<xref:Microsoft.AspNetCore.Identity.UserManager%601>可以儲存`ApplicationUser`宣告的使用者資料可從<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。

範例應用程式中`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) 建立地區設定 (`urn:google:locale`) 和圖片 (`urn:google:picture`) 為帶正負號的宣告中`ApplicationUser`，包括宣告<xref:System.Security.Claims.ClaimTypes.GivenName>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

根據預設，使用者的宣告會儲存在驗證 cookie。 如果驗證 cookie 太大，它可能會造成失敗，因為應用程式：

* 瀏覽器偵測到的 cookie 標頭太長。
* 太大而要求的整體大小。

如果需要處理使用者要求大量的使用者資料：

* 限制只有 app 所需的處理要求的使用者宣告的大小與數量。
* 使用自訂<xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>Cookie 驗證中介軟體的<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>跨要求儲存身分識別。 保留大量的伺服器上的身分識別資訊，同時只傳送給用戶端的小型工作階段識別碼索引鍵。

## <a name="save-the-access-token"></a>儲存存取權杖

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義存取和重新整理權杖是否應該儲存在<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功授權之後。 `SaveTokens` 設定為`false`預設情況下，以減少最終的驗證 cookie 的大小。

範例應用程式設定的值`SaveTokens`要`true`在<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

當`OnPostConfirmationAsync`執行時，儲存的存取權杖 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 從外部提供者`ApplicationUser`的`AuthenticationProperties`。

範例應用程式會將儲存存取權杖`OnPostConfirmationAsync`（新的使用者註冊） 和`OnGetCallbackAsync`（先前已註冊的使用者） 中*Account/ExternalLogin.cshtml.cs*:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>如何新增額外的自訂權杖

若要示範如何新增自訂權杖，它會儲存為一部分`SaveTokens`，範例應用程式會新增<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>與目前<xref:System.DateTime>如[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)的`TicketCreated`:

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a>建立並新增宣告

此架構提供常見的動作和建立，並將宣告新增至集合的擴充方法。 如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。

使用者可以定義自訂動作，藉由衍生自<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction>並實作抽象<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>方法。

如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。

## <a name="removal-of-claim-actions-and-claims"></a>移除宣告動作和宣告

[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)移除所有宣告的動作指定<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>從集合。 [（ClaimActionCollection，String） ClaimActionCollectionMapExtensions.DeleteClaim](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)刪除之宣告的指定<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>來自身分識別。 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主要搭配[OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc)移除通訊協定產生宣告。

## <a name="sample-app-output"></a>範例應用程式輸出

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="additional-resources"></a>其他資源

* [工程 SocialSample app aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash;連結的範例應用程式位於[aspnet/AspNetCore GitHub 存放庫的](https://github.com/aspnet/AspNetCore)`master`工程的分支。 `master`分支包含 ASP.NET Core 的下一個版本進行開發的程式碼。 若要查看範例應用程式的 ASP.NET Core 的發行版本的版本，請使用**分支**下拉式清單來選取發行分支 (例如`release/2.2`)。
