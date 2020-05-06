---
title: 在 ASP.NET Core 中保存外部提供者的其他宣告和權杖
author: rick-anderson
description: 瞭解如何從外部提供者建立額外的宣告和權杖。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 6acc1d78bf5cc39fd69329bad1cff0fbe52d9358
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82769026"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="8203d-103">在 ASP.NET Core 中保存外部提供者的其他宣告和權杖</span><span class="sxs-lookup"><span data-stu-id="8203d-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8203d-104">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="8203d-104">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="8203d-105">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="8203d-105">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="8203d-106">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8203d-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8203d-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="8203d-107">Prerequisites</span></span>

<span data-ttu-id="8203d-108">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="8203d-108">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="8203d-109">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="8203d-109">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="8203d-110">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="8203d-110">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="8203d-111">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="8203d-111">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="8203d-112">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="8203d-112">Set the client ID and client secret</span></span>

<span data-ttu-id="8203d-113">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="8203d-113">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="8203d-114">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="8203d-114">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="8203d-115">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="8203d-115">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="8203d-116">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="8203d-116">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="8203d-117">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-117">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="8203d-118">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-118">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="8203d-119">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-119">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="8203d-120">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-120">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="8203d-121">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="8203d-121">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="8203d-122">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="8203d-122">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="8203d-123">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="8203d-123">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="8203d-124">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="8203d-124">Establish the authentication scope</span></span>

<span data-ttu-id="8203d-125">藉由指定， <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>指定要從提供者抓取的許可權清單。</span><span class="sxs-lookup"><span data-stu-id="8203d-125">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="8203d-126">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="8203d-126">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="8203d-127">提供者</span><span class="sxs-lookup"><span data-stu-id="8203d-127">Provider</span></span>  | <span data-ttu-id="8203d-128">影響範圍</span><span class="sxs-lookup"><span data-stu-id="8203d-128">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="8203d-129">Facebook</span><span class="sxs-lookup"><span data-stu-id="8203d-129">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="8203d-130">Google</span><span class="sxs-lookup"><span data-stu-id="8203d-130">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="8203d-131">Microsoft</span><span class="sxs-lookup"><span data-stu-id="8203d-131">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="8203d-132">Twitter</span><span class="sxs-lookup"><span data-stu-id="8203d-132">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="8203d-133">在範例應用程式中，當`userinfo.profile`在上呼叫時<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> ，架構會自動新增 Google 的範圍<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>。</span><span class="sxs-lookup"><span data-stu-id="8203d-133">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="8203d-134">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="8203d-134">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="8203d-135">在下列範例中，會新增`https://www.googleapis.com/auth/user.birthday.read` Google 領域來取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="8203d-135">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="8203d-136">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="8203d-136">Map user data keys and create claims</span></span>

<span data-ttu-id="8203d-137">在提供者的選項中，針對<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>外部<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定或，以讓應用程式識別在登入時讀取。</span><span class="sxs-lookup"><span data-stu-id="8203d-137">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="8203d-138">如需宣告類型的詳細資訊， <xref:System.Security.Claims.ClaimTypes>請參閱。</span><span class="sxs-lookup"><span data-stu-id="8203d-138">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="8203d-139">範例應用程式會從 Google`urn:google:locale`使用者資料中的`urn:google:picture` `locale`和`picture`金鑰建立地區設定（）和圖片（）宣告：</span><span class="sxs-lookup"><span data-stu-id="8203d-139">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="8203d-140">在`Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`中， <xref:Microsoft.AspNetCore.Identity.IdentityUser>會`ApplicationUser`使用<xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>將（）登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="8203d-140">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="8203d-141">在登入程式期間， <xref:Microsoft.AspNetCore.Identity.UserManager%601>可以儲存可從`ApplicationUser`取得之使用者資料的宣告<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。</span><span class="sxs-lookup"><span data-stu-id="8203d-141">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="8203d-142">在範例應用程式中`OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入的地區設定`urn:google:locale`（）和圖片`urn:google:picture`（）宣告，包括`ApplicationUser` <xref:System.Security.Claims.ClaimTypes.GivenName>下列各項的聲明：</span><span class="sxs-lookup"><span data-stu-id="8203d-142">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="8203d-143">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="8203d-143">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="8203d-144">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="8203d-144">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="8203d-145">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="8203d-145">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="8203d-146">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="8203d-146">The overall size of the request is too large.</span></span>

<span data-ttu-id="8203d-147">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="8203d-147">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="8203d-148">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="8203d-148">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="8203d-149">使用 Cookie 驗證<xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>中介軟體的自訂來<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>儲存要求之間的身分識別。</span><span class="sxs-lookup"><span data-stu-id="8203d-149">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="8203d-150">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="8203d-150">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="8203d-151">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="8203d-151">Save the access token</span></span>

<span data-ttu-id="8203d-152"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>定義在成功授權之後， <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>是否應將存取和重新整理權杖儲存在中。</span><span class="sxs-lookup"><span data-stu-id="8203d-152"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="8203d-153">`SaveTokens`預設會設定`false`為，以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="8203d-153">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="8203d-154">範例應用程式會將的值`SaveTokens`設定`true`為<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>中的：</span><span class="sxs-lookup"><span data-stu-id="8203d-154">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="8203d-155">執行`OnPostConfirmationAsync`時，從的外部提供`ApplicationUser`者儲存存取權杖（ `AuthenticationProperties` [ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)）。</span><span class="sxs-lookup"><span data-stu-id="8203d-155">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="8203d-156">範例應用程式會在*Account/ExternalLogin*中，將存取權杖儲存`OnGetCallbackAsync`在`OnPostConfirmationAsync` （新的使用者註冊）和（先前註冊的使用者）中：</span><span class="sxs-lookup"><span data-stu-id="8203d-156">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="8203d-157">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="8203d-157">How to add additional custom tokens</span></span>

<span data-ttu-id="8203d-158">為了示範如何新增會儲存為一部分的`SaveTokens`自訂權杖，範例應用程式會在的[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)中<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>加入具有目前<xref:System.DateTime>的`TicketCreated`：</span><span class="sxs-lookup"><span data-stu-id="8203d-158">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="8203d-159">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="8203d-159">Creating and adding claims</span></span>

<span data-ttu-id="8203d-160">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="8203d-160">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="8203d-161">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="8203d-161">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="8203d-162">使用者可以自訂動作，方法是<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction>從衍生並執行<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>抽象方法。</span><span class="sxs-lookup"><span data-stu-id="8203d-162">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="8203d-163">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="8203d-163">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="8203d-164">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="8203d-164">Removal of claim actions and claims</span></span>

<span data-ttu-id="8203d-165">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除指定<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>的的所有宣告動作。</span><span class="sxs-lookup"><span data-stu-id="8203d-165">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="8203d-166">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>指定的宣告。</span><span class="sxs-lookup"><span data-stu-id="8203d-166">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="8203d-167"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="8203d-167"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="8203d-168">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="8203d-168">Sample app output</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8203d-169">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="8203d-169">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="8203d-170">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="8203d-170">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="8203d-171">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8203d-171">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8203d-172">必要條件</span><span class="sxs-lookup"><span data-stu-id="8203d-172">Prerequisites</span></span>

<span data-ttu-id="8203d-173">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="8203d-173">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="8203d-174">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="8203d-174">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="8203d-175">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="8203d-175">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="8203d-176">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="8203d-176">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="8203d-177">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="8203d-177">Set the client ID and client secret</span></span>

<span data-ttu-id="8203d-178">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="8203d-178">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="8203d-179">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="8203d-179">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="8203d-180">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="8203d-180">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="8203d-181">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="8203d-181">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="8203d-182">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-182">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="8203d-183">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-183">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="8203d-184">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-184">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="8203d-185">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="8203d-185">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="8203d-186">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="8203d-186">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="8203d-187">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="8203d-187">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="8203d-188">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="8203d-188">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="8203d-189">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="8203d-189">Establish the authentication scope</span></span>

<span data-ttu-id="8203d-190">藉由指定， <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>指定要從提供者抓取的許可權清單。</span><span class="sxs-lookup"><span data-stu-id="8203d-190">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="8203d-191">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="8203d-191">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="8203d-192">提供者</span><span class="sxs-lookup"><span data-stu-id="8203d-192">Provider</span></span>  | <span data-ttu-id="8203d-193">影響範圍</span><span class="sxs-lookup"><span data-stu-id="8203d-193">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="8203d-194">Facebook</span><span class="sxs-lookup"><span data-stu-id="8203d-194">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="8203d-195">Google</span><span class="sxs-lookup"><span data-stu-id="8203d-195">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="8203d-196">Microsoft</span><span class="sxs-lookup"><span data-stu-id="8203d-196">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="8203d-197">Twitter</span><span class="sxs-lookup"><span data-stu-id="8203d-197">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="8203d-198">在範例應用程式中，當`userinfo.profile`在上呼叫時<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> ，架構會自動新增 Google 的範圍<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>。</span><span class="sxs-lookup"><span data-stu-id="8203d-198">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="8203d-199">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="8203d-199">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="8203d-200">在下列範例中，會新增`https://www.googleapis.com/auth/user.birthday.read` Google 領域來取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="8203d-200">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="8203d-201">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="8203d-201">Map user data keys and create claims</span></span>

<span data-ttu-id="8203d-202">在提供者的選項中，針對<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>外部<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定或，以讓應用程式識別在登入時讀取。</span><span class="sxs-lookup"><span data-stu-id="8203d-202">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="8203d-203">如需宣告類型的詳細資訊， <xref:System.Security.Claims.ClaimTypes>請參閱。</span><span class="sxs-lookup"><span data-stu-id="8203d-203">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="8203d-204">範例應用程式會從 Google`urn:google:locale`使用者資料中的`urn:google:picture` `locale`和`picture`金鑰建立地區設定（）和圖片（）宣告：</span><span class="sxs-lookup"><span data-stu-id="8203d-204">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="8203d-205">在`Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`中， <xref:Microsoft.AspNetCore.Identity.IdentityUser>會`ApplicationUser`使用<xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>將（）登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="8203d-205">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="8203d-206">在登入程式期間， <xref:Microsoft.AspNetCore.Identity.UserManager%601>可以儲存可從`ApplicationUser`取得之使用者資料的宣告<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。</span><span class="sxs-lookup"><span data-stu-id="8203d-206">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="8203d-207">在範例應用程式中`OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入的地區設定`urn:google:locale`（）和圖片`urn:google:picture`（）宣告，包括`ApplicationUser` <xref:System.Security.Claims.ClaimTypes.GivenName>下列各項的聲明：</span><span class="sxs-lookup"><span data-stu-id="8203d-207">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="8203d-208">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="8203d-208">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="8203d-209">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="8203d-209">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="8203d-210">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="8203d-210">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="8203d-211">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="8203d-211">The overall size of the request is too large.</span></span>

<span data-ttu-id="8203d-212">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="8203d-212">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="8203d-213">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="8203d-213">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="8203d-214">使用 Cookie 驗證<xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>中介軟體的自訂來<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>儲存要求之間的身分識別。</span><span class="sxs-lookup"><span data-stu-id="8203d-214">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="8203d-215">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="8203d-215">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="8203d-216">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="8203d-216">Save the access token</span></span>

<span data-ttu-id="8203d-217"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>定義在成功授權之後， <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>是否應將存取和重新整理權杖儲存在中。</span><span class="sxs-lookup"><span data-stu-id="8203d-217"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="8203d-218">`SaveTokens`預設會設定`false`為，以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="8203d-218">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="8203d-219">範例應用程式會將的值`SaveTokens`設定`true`為<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>中的：</span><span class="sxs-lookup"><span data-stu-id="8203d-219">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="8203d-220">執行`OnPostConfirmationAsync`時，從的外部提供`ApplicationUser`者儲存存取權杖（ `AuthenticationProperties` [ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)）。</span><span class="sxs-lookup"><span data-stu-id="8203d-220">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="8203d-221">範例應用程式會在*Account/ExternalLogin*中，將存取權杖儲存`OnGetCallbackAsync`在`OnPostConfirmationAsync` （新的使用者註冊）和（先前註冊的使用者）中：</span><span class="sxs-lookup"><span data-stu-id="8203d-221">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="8203d-222">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="8203d-222">How to add additional custom tokens</span></span>

<span data-ttu-id="8203d-223">為了示範如何新增會儲存為一部分的`SaveTokens`自訂權杖，範例應用程式會在的[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)中<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>加入具有目前<xref:System.DateTime>的`TicketCreated`：</span><span class="sxs-lookup"><span data-stu-id="8203d-223">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="8203d-224">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="8203d-224">Creating and adding claims</span></span>

<span data-ttu-id="8203d-225">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="8203d-225">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="8203d-226">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="8203d-226">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="8203d-227">使用者可以自訂動作，方法是<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction>從衍生並執行<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>抽象方法。</span><span class="sxs-lookup"><span data-stu-id="8203d-227">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="8203d-228">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="8203d-228">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="8203d-229">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="8203d-229">Removal of claim actions and claims</span></span>

<span data-ttu-id="8203d-230">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除指定<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>的的所有宣告動作。</span><span class="sxs-lookup"><span data-stu-id="8203d-230">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="8203d-231">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>指定的宣告。</span><span class="sxs-lookup"><span data-stu-id="8203d-231">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="8203d-232"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="8203d-232"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="8203d-233">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="8203d-233">Sample app output</span></span>

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

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8203d-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="8203d-234">Additional resources</span></span>

* <span data-ttu-id="8203d-235">[dotnet/AspNetCore 工程 SocialSample 應用程式](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash;連結的範例應用程式位於[dotnet/AspNetCore GitHub](https://github.com/dotnet/AspNetCore) `master`存放庫的工程分支上。</span><span class="sxs-lookup"><span data-stu-id="8203d-235">[dotnet/AspNetCore engineering SocialSample app](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [dotnet/AspNetCore GitHub repo's](https://github.com/dotnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="8203d-236">`master`分支包含適用于下一個版本之 ASP.NET Core 的主動式開發程式碼。</span><span class="sxs-lookup"><span data-stu-id="8203d-236">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="8203d-237">若要查看 ASP.NET Core 發行版本的範例應用程式版本，請使用 [**分支**] 下拉式清單來選取發行分支（例如`release/{X.Y}`）。</span><span class="sxs-lookup"><span data-stu-id="8203d-237">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
