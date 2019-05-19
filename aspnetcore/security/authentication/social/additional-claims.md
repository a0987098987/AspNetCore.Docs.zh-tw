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
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="cbfb2-103">保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖</span><span class="sxs-lookup"><span data-stu-id="cbfb2-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="cbfb2-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="cbfb2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cbfb2-105">將 ASP.NET Core 應用程式可以建立額外的宣告和外部驗證提供者，例如 Facebook、 Google、 Microsoft 及 Twitter 的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="cbfb2-106">每個提供者會顯示其平台上，使用者的不同資訊但接收，以及將使用者資料轉換成其他宣告的模式相同。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="cbfb2-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cbfb2-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbfb2-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="cbfb2-108">Prerequisites</span></span>

<span data-ttu-id="cbfb2-109">決定哪些應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="cbfb2-110">每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="cbfb2-111">如需詳細資訊，請參閱 <xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="cbfb2-112">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-112">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="cbfb2-113">設定用戶端識別碼和用戶端祕密</span><span class="sxs-lookup"><span data-stu-id="cbfb2-113">Set the client ID and client secret</span></span>

<span data-ttu-id="cbfb2-114">OAuth 驗證提供者會建立信任關係，使用用戶端識別碼和用戶端祕密應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="cbfb2-115">用戶端識別碼和用戶端密碼值會針對應用程式外部驗證提供者所使用的提供者註冊應用程式時。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="cbfb2-116">應用程式使用每個外部提供者必須獨立設定，提供者的用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="cbfb2-117">如需詳細資訊，請參閱適用於您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="cbfb2-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="cbfb2-118">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="cbfb2-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="cbfb2-119">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="cbfb2-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="cbfb2-120">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="cbfb2-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="cbfb2-121">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="cbfb2-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="cbfb2-122">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="cbfb2-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="cbfb2-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="cbfb2-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="cbfb2-124">範例應用程式設定 Google 驗證提供者用戶端識別碼和由 Google 提供的用戶端祕密：</span><span class="sxs-lookup"><span data-stu-id="cbfb2-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="cbfb2-125">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="cbfb2-125">Establish the authentication scope</span></span>

<span data-ttu-id="cbfb2-126">指定從提供者擷取所指定的權限清單<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="cbfb2-127">常見的外部提供者的驗證領域會出現在下表中。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="cbfb2-128">提供者</span><span class="sxs-lookup"><span data-stu-id="cbfb2-128">Provider</span></span>  | <span data-ttu-id="cbfb2-129">範圍</span><span class="sxs-lookup"><span data-stu-id="cbfb2-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="cbfb2-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="cbfb2-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="cbfb2-131">Google</span><span class="sxs-lookup"><span data-stu-id="cbfb2-131">Google</span></span>    | `https://www.googleapis.com/auth/userinfo.profile`               |
| <span data-ttu-id="cbfb2-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="cbfb2-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="cbfb2-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="cbfb2-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="cbfb2-134">在範例應用程式，Google`userinfo.profile`由架構自動新增範圍時<xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*>上呼叫<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-134">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="cbfb2-135">如果應用程式需要其他範圍，請將它們加入選項。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-135">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="cbfb2-136">在下列範例中，Google`https://www.googleapis.com/auth/user.birthday.read`以擷取使用者的生日新增範圍：</span><span class="sxs-lookup"><span data-stu-id="cbfb2-136">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="cbfb2-137">將使用者資料的索引鍵對應，並建立宣告</span><span class="sxs-lookup"><span data-stu-id="cbfb2-137">Map user data keys and create claims</span></span>

<span data-ttu-id="cbfb2-138">在 提供者的選項，指定<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>或<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*>每個索引鍵/子機碼的外部提供者的 JSON 使用者資料，來讀取登入的應用程式身分識別。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-138">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="cbfb2-139">如需有關宣告類型的詳細資訊，請參閱<xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-139">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="cbfb2-140">範例應用程式會建立地區設定 (`urn:google:locale`) 和圖片 (`urn:google:picture`) 來自宣告`locale`和`picture`Google 使用者資料中的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="cbfb2-140">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="cbfb2-141">在  <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>，則<xref:Microsoft.AspNetCore.Identity.IdentityUser>(`ApplicationUser`) 登入應用程式與<xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-141">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="cbfb2-142">登入程序期間<xref:Microsoft.AspNetCore.Identity.UserManager%601>可以儲存`ApplicationUser`宣告的使用者資料可從<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-142">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="cbfb2-143">範例應用程式中`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) 建立地區設定 (`urn:google:locale`) 和圖片 (`urn:google:picture`) 為帶正負號的宣告中`ApplicationUser`，包括宣告<xref:System.Security.Claims.ClaimTypes.GivenName>:</span><span class="sxs-lookup"><span data-stu-id="cbfb2-143">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="cbfb2-144">根據預設，使用者的宣告會儲存在驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-144">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="cbfb2-145">如果驗證 cookie 太大，它可能會造成失敗，因為應用程式：</span><span class="sxs-lookup"><span data-stu-id="cbfb2-145">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="cbfb2-146">瀏覽器偵測到的 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-146">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="cbfb2-147">太大而要求的整體大小。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-147">The overall size of the request is too large.</span></span>

<span data-ttu-id="cbfb2-148">如果需要處理使用者要求大量的使用者資料：</span><span class="sxs-lookup"><span data-stu-id="cbfb2-148">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="cbfb2-149">限制只有 app 所需的處理要求的使用者宣告的大小與數量。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-149">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="cbfb2-150">使用自訂<xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore>Cookie 驗證中介軟體的<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore>跨要求儲存身分識別。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-150">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="cbfb2-151">保留大量的伺服器上的身分識別資訊，同時只傳送給用戶端的小型工作階段識別碼索引鍵。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-151">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="cbfb2-152">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="cbfb2-152">Save the access token</span></span>

<span data-ttu-id="cbfb2-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義存取和重新整理權杖是否應該儲存在<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功授權之後。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-153"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="cbfb2-154">`SaveTokens` 設定為`false`預設情況下，以減少最終的驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-154">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="cbfb2-155">範例應用程式設定的值`SaveTokens`要`true`在<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="cbfb2-155">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="cbfb2-156">當`OnPostConfirmationAsync`執行時，儲存的存取權杖 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 從外部提供者`ApplicationUser`的`AuthenticationProperties`。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-156">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="cbfb2-157">範例應用程式會將儲存存取權杖`OnPostConfirmationAsync`（新的使用者註冊） 和`OnGetCallbackAsync`（先前已註冊的使用者） 中*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="cbfb2-157">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="cbfb2-158">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="cbfb2-158">How to add additional custom tokens</span></span>

<span data-ttu-id="cbfb2-159">若要示範如何新增自訂權杖，它會儲存為一部分`SaveTokens`，範例應用程式會新增<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>與目前<xref:System.DateTime>如[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)的`TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="cbfb2-159">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-28)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="cbfb2-160">建立並新增宣告</span><span class="sxs-lookup"><span data-stu-id="cbfb2-160">Creating and adding claims</span></span>

<span data-ttu-id="cbfb2-161">此架構提供常見的動作和建立，並將宣告新增至集合的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-161">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="cbfb2-162">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-162">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="cbfb2-163">使用者可以定義自訂動作，藉由衍生自<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction>並實作抽象<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*>方法。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-163">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="cbfb2-164">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-164">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="cbfb2-165">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="cbfb2-165">Removal of claim actions and claims</span></span>

<span data-ttu-id="cbfb2-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)移除所有宣告的動作指定<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>從集合。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-166">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="cbfb2-167">[（ClaimActionCollection，String） ClaimActionCollectionMapExtensions.DeleteClaim](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)刪除之宣告的指定<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType>來自身分識別。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-167">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="cbfb2-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> 主要搭配[OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc)移除通訊協定產生宣告。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-168"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="cbfb2-169">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="cbfb2-169">Sample app output</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="cbfb2-170">其他資源</span><span class="sxs-lookup"><span data-stu-id="cbfb2-170">Additional resources</span></span>

* <span data-ttu-id="cbfb2-171">[工程 SocialSample app aspnet/AspNetCore](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash;連結的範例應用程式位於[aspnet/AspNetCore GitHub 存放庫的](https://github.com/aspnet/AspNetCore)`master`工程的分支。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-171">[aspnet/AspNetCore engineering SocialSample app](https://github.com/aspnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample) &ndash; The linked sample app is on the [aspnet/AspNetCore GitHub repo's](https://github.com/aspnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="cbfb2-172">`master`分支包含 ASP.NET Core 的下一個版本進行開發的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-172">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="cbfb2-173">若要查看範例應用程式的 ASP.NET Core 的發行版本的版本，請使用**分支**下拉式清單來選取發行分支 (例如`release/2.2`)。</span><span class="sxs-lookup"><span data-stu-id="cbfb2-173">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/2.2`).</span></span>
