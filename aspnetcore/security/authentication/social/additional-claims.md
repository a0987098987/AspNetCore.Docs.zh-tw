---
title: 在 ASP.NET Core 中保存外部提供者的其他宣告和權杖
author: guardrex
description: 瞭解如何從外部提供者建立額外的宣告和權杖。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 44b3e72085e6265319b53b548f7f7ddde2adbd14
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828577"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="0c1f2-103">在 ASP.NET Core 中保存外部提供者的其他宣告和權杖</span><span class="sxs-lookup"><span data-stu-id="0c1f2-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="0c1f2-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0c1f2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0c1f2-105">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="0c1f2-106">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="0c1f2-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c1f2-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c1f2-108">必要條件：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-108">Prerequisites</span></span>

<span data-ttu-id="0c1f2-109">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="0c1f2-110">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="0c1f2-111">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="0c1f2-112">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="0c1f2-113">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="0c1f2-113">Set the client ID and client secret</span></span>

<span data-ttu-id="0c1f2-114">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="0c1f2-115">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="0c1f2-116">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="0c1f2-117">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="0c1f2-118">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="0c1f2-119">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="0c1f2-120">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="0c1f2-121">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="0c1f2-122">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="0c1f2-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="0c1f2-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="0c1f2-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="0c1f2-124">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="0c1f2-125">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="0c1f2-125">Establish the authentication scope</span></span>

<span data-ttu-id="0c1f2-126">藉由指定 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>，指定要從提供者取出的許可權清單。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="0c1f2-127">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="0c1f2-128">Provider</span><span class="sxs-lookup"><span data-stu-id="0c1f2-128">Provider</span></span>  | <span data-ttu-id="0c1f2-129">範圍</span><span class="sxs-lookup"><span data-stu-id="0c1f2-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="0c1f2-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="0c1f2-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="0c1f2-131">Google</span><span class="sxs-lookup"><span data-stu-id="0c1f2-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="0c1f2-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="0c1f2-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="0c1f2-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="0c1f2-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="0c1f2-134">在範例應用程式中，當您在 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>上呼叫 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 時，架構會自動新增 Google 的 `userinfo.profile` 範圍。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="0c1f2-135">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="0c1f2-136">在下列範例中，會新增 Google `https://www.googleapis.com/auth/user.birthday.read` 範圍，以取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="0c1f2-137">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="0c1f2-137">Map user data keys and create claims</span></span>

<span data-ttu-id="0c1f2-138">在提供者的選項中，為外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> 或 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>，讓應用程式識別能夠在登入時讀取。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="0c1f2-139">如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="0c1f2-140">範例應用程式會在 Google 使用者資料的 `locale` 和 `picture` 金鑰中，建立地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="0c1f2-141">在 `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`中，<xref:Microsoft.AspNetCore.Identity.IdentityUser> （`ApplicationUser`）會使用 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-141">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="0c1f2-142">在登入程式期間，<xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>所提供使用者資料的 `ApplicationUser` 宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="0c1f2-143">在範例應用程式中，`OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入 `ApplicationUser`的地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告，包括 <xref:System.Security.Claims.ClaimTypes.GivenName>的宣告：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="0c1f2-144">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="0c1f2-145">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="0c1f2-146">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="0c1f2-147">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="0c1f2-148">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="0c1f2-149">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="0c1f2-150">使用 Cookie 驗證中介軟體的 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> 的自訂 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>，在要求之間儲存身分識別。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="0c1f2-151">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="0c1f2-152">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="0c1f2-152">Save the access token</span></span>

<span data-ttu-id="0c1f2-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義在成功授權之後，是否應將存取和重新整理權杖儲存在 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> 中。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="0c1f2-154">`SaveTokens` 預設會設定為 `false`，以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="0c1f2-155">範例應用程式會將 `SaveTokens` 的值設定為 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>中 `true`：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="0c1f2-156">當 `OnPostConfirmationAsync` 執行時，請在 `ApplicationUser`的 `AuthenticationProperties`中儲存外部提供者的存取權杖（[ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)）。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="0c1f2-157">範例應用程式會將存取權杖儲存在*Account/ExternalLogin*中的 `OnPostConfirmationAsync` （新的使用者註冊）和 `OnGetCallbackAsync` （先前註冊的使用者）中：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="0c1f2-158">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="0c1f2-158">How to add additional custom tokens</span></span>

<span data-ttu-id="0c1f2-159">為了示範如何新增會儲存為 `SaveTokens`一部分的自訂權杖，範例應用程式會針對 `TicketCreated`的[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) ，新增具有目前 <xref:System.DateTime> 的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="0c1f2-160">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="0c1f2-160">Creating and adding claims</span></span>

<span data-ttu-id="0c1f2-161">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="0c1f2-162">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="0c1f2-163">使用者可以藉由衍生自 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 並執行抽象的 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法，來定義自訂動作。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="0c1f2-164">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="0c1f2-165">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="0c1f2-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="0c1f2-166">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的所有宣告動作。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="0c1f2-167">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="0c1f2-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="0c1f2-169">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="0c1f2-169">Sample app output</span></span>

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

<span data-ttu-id="0c1f2-170">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-170">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="0c1f2-171">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-171">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="0c1f2-172">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0c1f2-172">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c1f2-173">必要條件：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-173">Prerequisites</span></span>

<span data-ttu-id="0c1f2-174">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-174">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="0c1f2-175">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-175">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="0c1f2-176">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-176">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="0c1f2-177">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-177">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="0c1f2-178">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="0c1f2-178">Set the client ID and client secret</span></span>

<span data-ttu-id="0c1f2-179">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-179">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="0c1f2-180">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-180">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="0c1f2-181">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-181">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="0c1f2-182">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-182">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="0c1f2-183">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-183">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="0c1f2-184">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-184">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="0c1f2-185">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-185">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="0c1f2-186">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="0c1f2-186">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="0c1f2-187">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="0c1f2-187">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="0c1f2-188">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="0c1f2-188">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="0c1f2-189">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-189">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="0c1f2-190">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="0c1f2-190">Establish the authentication scope</span></span>

<span data-ttu-id="0c1f2-191">藉由指定 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>，指定要從提供者取出的許可權清單。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-191">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="0c1f2-192">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-192">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="0c1f2-193">Provider</span><span class="sxs-lookup"><span data-stu-id="0c1f2-193">Provider</span></span>  | <span data-ttu-id="0c1f2-194">範圍</span><span class="sxs-lookup"><span data-stu-id="0c1f2-194">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="0c1f2-195">Facebook</span><span class="sxs-lookup"><span data-stu-id="0c1f2-195">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="0c1f2-196">Google</span><span class="sxs-lookup"><span data-stu-id="0c1f2-196">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="0c1f2-197">Microsoft</span><span class="sxs-lookup"><span data-stu-id="0c1f2-197">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="0c1f2-198">Twitter</span><span class="sxs-lookup"><span data-stu-id="0c1f2-198">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="0c1f2-199">在範例應用程式中，當您在 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>上呼叫 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 時，架構會自動新增 Google 的 `userinfo.profile` 範圍。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-199">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="0c1f2-200">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-200">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="0c1f2-201">在下列範例中，會新增 Google `https://www.googleapis.com/auth/user.birthday.read` 範圍，以取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-201">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="0c1f2-202">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="0c1f2-202">Map user data keys and create claims</span></span>

<span data-ttu-id="0c1f2-203">在提供者的選項中，為外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> 或 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>，讓應用程式識別能夠在登入時讀取。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-203">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="0c1f2-204">如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-204">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="0c1f2-205">範例應用程式會在 Google 使用者資料的 `locale` 和 `picture` 金鑰中，建立地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-205">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="0c1f2-206">在 `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`中，<xref:Microsoft.AspNetCore.Identity.IdentityUser> （`ApplicationUser`）會使用 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-206">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="0c1f2-207">在登入程式期間，<xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>所提供使用者資料的 `ApplicationUser` 宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-207">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="0c1f2-208">在範例應用程式中，`OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入 `ApplicationUser`的地區設定（`urn:google:locale`）和圖片（`urn:google:picture`）宣告，包括 <xref:System.Security.Claims.ClaimTypes.GivenName>的宣告：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-208">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="0c1f2-209">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-209">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="0c1f2-210">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-210">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="0c1f2-211">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-211">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="0c1f2-212">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-212">The overall size of the request is too large.</span></span>

<span data-ttu-id="0c1f2-213">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-213">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="0c1f2-214">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-214">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="0c1f2-215">使用 Cookie 驗證中介軟體的 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> 的自訂 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>，在要求之間儲存身分識別。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-215">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="0c1f2-216">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-216">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="0c1f2-217">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="0c1f2-217">Save the access token</span></span>

<span data-ttu-id="0c1f2-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義在成功授權之後，是否應將存取和重新整理權杖儲存在 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> 中。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-218"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="0c1f2-219">`SaveTokens` 預設會設定為 `false`，以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-219">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="0c1f2-220">範例應用程式會將 `SaveTokens` 的值設定為 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>中 `true`：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-220">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="0c1f2-221">當 `OnPostConfirmationAsync` 執行時，請在 `ApplicationUser`的 `AuthenticationProperties`中儲存外部提供者的存取權杖（[ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)）。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-221">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="0c1f2-222">範例應用程式會將存取權杖儲存在*Account/ExternalLogin*中的 `OnPostConfirmationAsync` （新的使用者註冊）和 `OnGetCallbackAsync` （先前註冊的使用者）中：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-222">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="0c1f2-223">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="0c1f2-223">How to add additional custom tokens</span></span>

<span data-ttu-id="0c1f2-224">為了示範如何新增會儲存為 `SaveTokens`一部分的自訂權杖，範例應用程式會針對 `TicketCreated`的[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) ，新增具有目前 <xref:System.DateTime> 的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>：</span><span class="sxs-lookup"><span data-stu-id="0c1f2-224">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="0c1f2-225">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="0c1f2-225">Creating and adding claims</span></span>

<span data-ttu-id="0c1f2-226">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-226">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="0c1f2-227">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-227">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="0c1f2-228">使用者可以藉由衍生自 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 並執行抽象的 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法，來定義自訂動作。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-228">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="0c1f2-229">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-229">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="0c1f2-230">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="0c1f2-230">Removal of claim actions and claims</span></span>

<span data-ttu-id="0c1f2-231">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的所有宣告動作。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-231">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="0c1f2-232">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除給定 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 的宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-232">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="0c1f2-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-233"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="0c1f2-234">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="0c1f2-234">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="0c1f2-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="0c1f2-235">Additional resources</span></span>

* <span data-ttu-id="0c1f2-236">[dotnet/AspNetCore 工程 SocialSample 應用程式](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample)&ndash; 連結的範例應用程式位於[Dotnet/AspNetCore GitHub](https://github.com/dotnet/AspNetCore)存放庫的 `master` 工程分支上。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-236">[dotnet/AspNetCore engineering SocialSample app](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [dotnet/AspNetCore GitHub repo's](https://github.com/dotnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="0c1f2-237">在下一版的 ASP.NET Core 中，`master` 分支包含作用中開發的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-237">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="0c1f2-238">若要查看 ASP.NET Core 發行版本的範例應用程式版本，請使用 [**分支**] 下拉式清單來選取發行分支（例如 `release/{X.Y}`）。</span><span class="sxs-lookup"><span data-stu-id="0c1f2-238">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
