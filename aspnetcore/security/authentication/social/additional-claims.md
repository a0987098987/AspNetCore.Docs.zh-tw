---
title: 在 ASP.NET Core 中保存外部提供者的其他宣告和權杖
author: guardrex
description: 瞭解如何從外部提供者建立額外的宣告和權杖。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/01/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: cdf263df8d1aa17ea3820a16ecbd10abce9d683d
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925160"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="96e13-103">在 ASP.NET Core 中保存外部提供者的其他宣告和權杖</span><span class="sxs-lookup"><span data-stu-id="96e13-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="96e13-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="96e13-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="96e13-105">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="96e13-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="96e13-106">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="96e13-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="96e13-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96e13-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96e13-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="96e13-108">Prerequisites</span></span>

<span data-ttu-id="96e13-109">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="96e13-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="96e13-110">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="96e13-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="96e13-111">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="96e13-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="96e13-112">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="96e13-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="96e13-113">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="96e13-113">Set the client ID and client secret</span></span>

<span data-ttu-id="96e13-114">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="96e13-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="96e13-115">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="96e13-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="96e13-116">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="96e13-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="96e13-117">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="96e13-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="96e13-118">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="96e13-119">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="96e13-120">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="96e13-121">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="96e13-122">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="96e13-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="96e13-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="96e13-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="96e13-124">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="96e13-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="96e13-125">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="96e13-125">Establish the authentication scope</span></span>

<span data-ttu-id="96e13-126">藉由指定 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>，指定要從提供者抓取的許可權清單。</span><span class="sxs-lookup"><span data-stu-id="96e13-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="96e13-127">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="96e13-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="96e13-128">提供者</span><span class="sxs-lookup"><span data-stu-id="96e13-128">Provider</span></span>  | <span data-ttu-id="96e13-129">`Scope`</span><span class="sxs-lookup"><span data-stu-id="96e13-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="96e13-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="96e13-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="96e13-131">Google</span><span class="sxs-lookup"><span data-stu-id="96e13-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="96e13-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="96e13-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="96e13-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="96e13-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="96e13-134">在範例應用程式中，當在 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 時，架構會自動新增 Google 的 `userinfo.profile` 範圍。</span><span class="sxs-lookup"><span data-stu-id="96e13-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="96e13-135">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="96e13-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="96e13-136">在下列範例中，會新增 Google `https://www.googleapis.com/auth/user.birthday.read` 範圍，以取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="96e13-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="96e13-137">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="96e13-137">Map user data keys and create claims</span></span>

<span data-ttu-id="96e13-138">在提供者的選項中，為外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> 或 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>，以讓應用程式識別在登入時進行讀取。</span><span class="sxs-lookup"><span data-stu-id="96e13-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="96e13-139">如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="96e13-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="96e13-140">範例應用程式會在 Google 使用者資料的 `locale` 和 `picture` 金鑰中，建立地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告：</span><span class="sxs-lookup"><span data-stu-id="96e13-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="96e13-141">在 <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*> 中，<xref:Microsoft.AspNetCore.Identity.IdentityUser> （`ApplicationUser`）已使用 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> 登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="96e13-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="96e13-142">在登入程式期間，<xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> 中可用使用者資料的 @no__t 1 宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="96e13-143">在範例應用程式中，`OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入 `ApplicationUser` 的地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告，包括 <xref:System.Security.Claims.ClaimTypes.GivenName> 的宣告：</span><span class="sxs-lookup"><span data-stu-id="96e13-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="96e13-144">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="96e13-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="96e13-145">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="96e13-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="96e13-146">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="96e13-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="96e13-147">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="96e13-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="96e13-148">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="96e13-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="96e13-149">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="96e13-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="96e13-150">使用自訂的 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> 來進行 Cookie 驗證中介軟體的 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>，以在要求之間儲存身分識別。</span><span class="sxs-lookup"><span data-stu-id="96e13-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="96e13-151">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="96e13-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="96e13-152">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="96e13-152">Save the access token</span></span>

<span data-ttu-id="96e13-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義在成功授權之後，是否應將存取和重新整理權杖儲存在 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>。</span><span class="sxs-lookup"><span data-stu-id="96e13-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="96e13-154">`SaveTokens` 預設設為 `false`，以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="96e13-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="96e13-155">範例應用程式會將 `SaveTokens` 的值設定為 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> 中的 `true`：</span><span class="sxs-lookup"><span data-stu-id="96e13-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="96e13-156">當 `OnPostConfirmationAsync` 執行時，請在 `ApplicationUser` 的 `AuthenticationProperties` 中，儲存來自外部提供者的存取權杖（[ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)）。</span><span class="sxs-lookup"><span data-stu-id="96e13-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="96e13-157">範例應用程式會在*Account/ExternalLogin*中，將存取權杖儲存在 `OnPostConfirmationAsync` （新的使用者註冊）和 `OnGetCallbackAsync` （先前註冊的使用者）中：</span><span class="sxs-lookup"><span data-stu-id="96e13-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="96e13-158">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="96e13-158">How to add additional custom tokens</span></span>

<span data-ttu-id="96e13-159">為了示範如何新增自訂權杖（儲存為 `SaveTokens` 的一部分），範例應用程式會為 `TicketCreated` 的[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)新增 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>，其中包含目前的 <xref:System.DateTime>：</span><span class="sxs-lookup"><span data-stu-id="96e13-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="96e13-160">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="96e13-160">Creating and adding claims</span></span>

<span data-ttu-id="96e13-161">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="96e13-162">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="96e13-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="96e13-163">使用者可以藉由從 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 衍生並執行抽象的 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法來定義自訂動作。</span><span class="sxs-lookup"><span data-stu-id="96e13-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="96e13-164">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="96e13-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="96e13-165">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="96e13-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="96e13-166">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的所有宣告動作。</span><span class="sxs-lookup"><span data-stu-id="96e13-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="96e13-167">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="96e13-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="96e13-169">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="96e13-169">Sample app output</span></span>

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

<span data-ttu-id="96e13-170">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="96e13-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="96e13-171">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="96e13-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="96e13-172">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="96e13-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96e13-173">必要條件</span><span class="sxs-lookup"><span data-stu-id="96e13-173">Prerequisites</span></span>

<span data-ttu-id="96e13-174">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="96e13-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="96e13-175">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="96e13-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="96e13-176">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="96e13-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="96e13-177">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="96e13-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="96e13-178">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="96e13-178">Set the client ID and client secret</span></span>

<span data-ttu-id="96e13-179">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="96e13-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="96e13-180">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="96e13-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="96e13-181">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="96e13-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="96e13-182">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="96e13-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="96e13-183">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="96e13-184">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="96e13-185">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="96e13-186">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="96e13-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="96e13-187">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="96e13-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="96e13-188">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="96e13-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="96e13-189">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="96e13-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="96e13-190">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="96e13-190">Establish the authentication scope</span></span>

<span data-ttu-id="96e13-191">藉由指定 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>，指定要從提供者抓取的許可權清單。</span><span class="sxs-lookup"><span data-stu-id="96e13-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="96e13-192">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="96e13-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="96e13-193">提供者</span><span class="sxs-lookup"><span data-stu-id="96e13-193">Provider</span></span>  | <span data-ttu-id="96e13-194">`Scope`</span><span class="sxs-lookup"><span data-stu-id="96e13-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="96e13-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="96e13-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="96e13-196">Google</span><span class="sxs-lookup"><span data-stu-id="96e13-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="96e13-197">Microsoft</span><span class="sxs-lookup"><span data-stu-id="96e13-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="96e13-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="96e13-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="96e13-199">在範例應用程式中，當在 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> 上呼叫 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 時，架構會自動新增 Google 的 `userinfo.profile` 範圍。</span><span class="sxs-lookup"><span data-stu-id="96e13-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="96e13-200">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="96e13-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="96e13-201">在下列範例中，會新增 Google `https://www.googleapis.com/auth/user.birthday.read` 範圍，以取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="96e13-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="96e13-202">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="96e13-202">Map user data keys and create claims</span></span>

<span data-ttu-id="96e13-203">在提供者的選項中，為外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> 或 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>，以讓應用程式識別在登入時進行讀取。</span><span class="sxs-lookup"><span data-stu-id="96e13-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="96e13-204">如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="96e13-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="96e13-205">範例應用程式會在 Google 使用者資料的 `locale` 和 `picture` 金鑰中，建立地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告：</span><span class="sxs-lookup"><span data-stu-id="96e13-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="96e13-206">在 <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*> 中，<xref:Microsoft.AspNetCore.Identity.IdentityUser> （`ApplicationUser`）已使用 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> 登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="96e13-206">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="96e13-207">在登入程式期間，<xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> 中可用使用者資料的 @no__t 1 宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="96e13-208">在範例應用程式中，`OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入 `ApplicationUser` 的地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告，包括 <xref:System.Security.Claims.ClaimTypes.GivenName> 的宣告：</span><span class="sxs-lookup"><span data-stu-id="96e13-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="96e13-209">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="96e13-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="96e13-210">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="96e13-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="96e13-211">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="96e13-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="96e13-212">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="96e13-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="96e13-213">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="96e13-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="96e13-214">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="96e13-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="96e13-215">使用自訂的 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> 來進行 Cookie 驗證中介軟體的 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>，以在要求之間儲存身分識別。</span><span class="sxs-lookup"><span data-stu-id="96e13-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="96e13-216">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="96e13-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="96e13-217">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="96e13-217">Save the access token</span></span>

<span data-ttu-id="96e13-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義在成功授權之後，是否應將存取和重新整理權杖儲存在 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>。</span><span class="sxs-lookup"><span data-stu-id="96e13-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="96e13-219">`SaveTokens` 預設設為 `false`，以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="96e13-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="96e13-220">範例應用程式會將 `SaveTokens` 的值設定為 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> 中的 `true`：</span><span class="sxs-lookup"><span data-stu-id="96e13-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="96e13-221">當 `OnPostConfirmationAsync` 執行時，請在 `ApplicationUser` 的 `AuthenticationProperties` 中，儲存來自外部提供者的存取權杖（[ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)）。</span><span class="sxs-lookup"><span data-stu-id="96e13-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="96e13-222">範例應用程式會在*Account/ExternalLogin*中，將存取權杖儲存在 `OnPostConfirmationAsync` （新的使用者註冊）和 `OnGetCallbackAsync` （先前註冊的使用者）中：</span><span class="sxs-lookup"><span data-stu-id="96e13-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="96e13-223">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="96e13-223">How to add additional custom tokens</span></span>

<span data-ttu-id="96e13-224">為了示範如何新增自訂權杖（儲存為 `SaveTokens` 的一部分），範例應用程式會為 `TicketCreated` 的[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)新增 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>，其中包含目前的 <xref:System.DateTime>：</span><span class="sxs-lookup"><span data-stu-id="96e13-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="96e13-225">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="96e13-225">Creating and adding claims</span></span>

<span data-ttu-id="96e13-226">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="96e13-227">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="96e13-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="96e13-228">使用者可以藉由從 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 衍生並執行抽象的 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法來定義自訂動作。</span><span class="sxs-lookup"><span data-stu-id="96e13-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="96e13-229">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="96e13-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="96e13-230">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="96e13-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="96e13-231">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的所有宣告動作。</span><span class="sxs-lookup"><span data-stu-id="96e13-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="96e13-232">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="96e13-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="96e13-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="96e13-234">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="96e13-234">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="96e13-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="96e13-235">Additional resources</span></span>

* <span data-ttu-id="96e13-236">[aspnet/AspNetCore 工程 SocialSample 應用程式](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample)&ndash; 連結的範例應用程式位於[Aspnet/AspNetCore GitHub](https://github.com/aspnet/AspNetCore)存放庫的 @no__t 3 工程分支。</span><span class="sxs-lookup"><span data-stu-id="96e13-236">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="96e13-237">在下一版的 ASP.NET Core 中，`master` 分支包含作用中開發的程式碼。</span><span class="sxs-lookup"><span data-stu-id="96e13-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="96e13-238">若要查看 ASP.NET Core 發行版本的範例應用程式版本，請使用 [**分支**] 下拉式清單來選取發行分支（例如 `release/{X.Y}`）。</span><span class="sxs-lookup"><span data-stu-id="96e13-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
