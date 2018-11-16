---
title: 保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖
author: guardrex
description: 了解如何建立額外的宣告並從外部提供者的權杖。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/additional-claims
ms.openlocfilehash: 9a24ac138950ef2bedac48f506655d06520137cf
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708357"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="7499b-103">保存其他的宣告與 ASP.NET Core 中的外部提供者的權杖</span><span class="sxs-lookup"><span data-stu-id="7499b-103">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

<span data-ttu-id="7499b-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7499b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7499b-105">將 ASP.NET Core 應用程式可以建立額外的宣告和外部驗證提供者，例如 Facebook、 Google、 Microsoft 及 Twitter 的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="7499b-105">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="7499b-106">每個提供者會顯示其平台上，使用者的不同資訊但接收，以及將使用者資料轉換成其他宣告的模式相同。</span><span class="sxs-lookup"><span data-stu-id="7499b-106">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="7499b-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7499b-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7499b-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="7499b-108">Prerequisites</span></span>

<span data-ttu-id="7499b-109">決定哪些應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="7499b-109">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="7499b-110">每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="7499b-110">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="7499b-111">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="7499b-111">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="7499b-112">[範例應用程式](#sample-app-instructions)會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="7499b-112">The [sample app](#sample-app-instructions) uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="7499b-113">設定用戶端識別碼和用戶端祕密</span><span class="sxs-lookup"><span data-stu-id="7499b-113">Set the client ID and client secret</span></span>

<span data-ttu-id="7499b-114">OAuth 驗證提供者會建立信任關係，使用用戶端識別碼和用戶端祕密應用程式。</span><span class="sxs-lookup"><span data-stu-id="7499b-114">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="7499b-115">用戶端識別碼和用戶端密碼值會針對應用程式外部驗證提供者所使用的提供者註冊應用程式時。</span><span class="sxs-lookup"><span data-stu-id="7499b-115">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="7499b-116">應用程式使用每個外部提供者必須獨立設定，提供者的用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="7499b-116">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="7499b-117">如需詳細資訊，請參閱適用於您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="7499b-117">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="7499b-118">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="7499b-118">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="7499b-119">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="7499b-119">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="7499b-120">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="7499b-120">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="7499b-121">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="7499b-121">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="7499b-122">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="7499b-122">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="7499b-123">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="7499b-123">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="7499b-124">範例應用程式設定 Google 驗證提供者用戶端識別碼和由 Google 提供的用戶端祕密：</span><span class="sxs-lookup"><span data-stu-id="7499b-124">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,6)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="7499b-125">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="7499b-125">Establish the authentication scope</span></span>

<span data-ttu-id="7499b-126">指定從提供者擷取所指定的權限清單<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>。</span><span class="sxs-lookup"><span data-stu-id="7499b-126">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="7499b-127">常見的外部提供者的驗證領域會出現在下表中。</span><span class="sxs-lookup"><span data-stu-id="7499b-127">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="7499b-128">提供者</span><span class="sxs-lookup"><span data-stu-id="7499b-128">Provider</span></span>  | <span data-ttu-id="7499b-129">範圍</span><span class="sxs-lookup"><span data-stu-id="7499b-129">Scope</span></span>                                                            |
| --------- | ---------------------------------------------------------------- |
| <span data-ttu-id="7499b-130">Facebook</span><span class="sxs-lookup"><span data-stu-id="7499b-130">Facebook</span></span>  | `https://www.facebook.com/dialog/oauth`                          |
| <span data-ttu-id="7499b-131">Google</span><span class="sxs-lookup"><span data-stu-id="7499b-131">Google</span></span>    | `https://www.googleapis.com/auth/plus.login`                     |
| <span data-ttu-id="7499b-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="7499b-132">Microsoft</span></span> | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| <span data-ttu-id="7499b-133">Twitter</span><span class="sxs-lookup"><span data-stu-id="7499b-133">Twitter</span></span>   | `https://api.twitter.com/oauth/authenticate`                     |

<span data-ttu-id="7499b-134">範例應用程式會新增 Google`plus.login`要求 Google + 登入權限的範圍：</span><span class="sxs-lookup"><span data-stu-id="7499b-134">The sample app adds the Google `plus.login` scope to request Google+ sign in permissions:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=7)]

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="7499b-135">將使用者資料的索引鍵對應，並建立宣告</span><span class="sxs-lookup"><span data-stu-id="7499b-135">Map user data keys and create claims</span></span>

<span data-ttu-id="7499b-136">在 提供者的選項，指定<xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*>每個索引鍵的外部提供者的 JSON 使用者資料，來讀取登入的應用程式身分識別。</span><span class="sxs-lookup"><span data-stu-id="7499b-136">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> for each key in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="7499b-137">如需有關宣告類型的詳細資訊，請參閱<xref:System.Security.Claims.ClaimTypes>。</span><span class="sxs-lookup"><span data-stu-id="7499b-137">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="7499b-138">範例應用程式會建立<xref:System.Security.Claims.ClaimTypes.Gender>來自宣告`gender`Google 使用者資料中的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="7499b-138">The sample app creates a <xref:System.Security.Claims.ClaimTypes.Gender> claim from the `gender` key in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=8)]

<span data-ttu-id="7499b-139">在  <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>，則<xref:Microsoft.AspNetCore.Identity.IdentityUser>(`ApplicationUser`) 登入應用程式與<xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>。</span><span class="sxs-lookup"><span data-stu-id="7499b-139">In <xref:Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync*>, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager`1.SignInAsync*>.</span></span> <span data-ttu-id="7499b-140">登入程序期間<xref:Microsoft.AspNetCore.Identity.UserManager`1>可以儲存`ApplicationUser`宣告的使用者資料可從<xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>。</span><span class="sxs-lookup"><span data-stu-id="7499b-140">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager`1> can store an `ApplicationUser` claim for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="7499b-141">範例應用程式中`OnPostConfirmationAsync`(*Account/ExternalLogin.cshtml.cs*) 建立<xref:System.Security.Claims.ClaimTypes.Gender>宣告為帶正負號的`ApplicationUser`:</span><span class="sxs-lookup"><span data-stu-id="7499b-141">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes a <xref:System.Security.Claims.ClaimTypes.Gender> claim for the signed in `ApplicationUser`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=30-31)]

## <a name="save-the-access-token"></a><span data-ttu-id="7499b-142">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="7499b-142">Save the access token</span></span>

<span data-ttu-id="7499b-143"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> 定義存取和重新整理權杖是否應該儲存在<xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties>成功授權之後。</span><span class="sxs-lookup"><span data-stu-id="7499b-143"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="7499b-144">`SaveTokens` 設定為`false`預設情況下，以減少最終的驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="7499b-144">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="7499b-145">範例應用程式設定的值`SaveTokens`要`true`在<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span><span class="sxs-lookup"><span data-stu-id="7499b-145">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=9)]

<span data-ttu-id="7499b-146">當`OnPostConfirmationAsync`執行時，儲存的存取權杖 ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 從外部提供者`ApplicationUser`的`AuthenticationProperties`。</span><span class="sxs-lookup"><span data-stu-id="7499b-146">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="7499b-147">範例應用程式會將儲存存取權杖：</span><span class="sxs-lookup"><span data-stu-id="7499b-147">The sample app saves the access token in:</span></span>

* <span data-ttu-id="7499b-148">`OnPostConfirmationAsync` &ndash; 執行新的使用者註冊。</span><span class="sxs-lookup"><span data-stu-id="7499b-148">`OnPostConfirmationAsync` &ndash; Executes for new user registration.</span></span>
* <span data-ttu-id="7499b-149">`OnGetCallbackAsync` &ndash; 當先前已註冊的使用者登入應用程式時執行。</span><span class="sxs-lookup"><span data-stu-id="7499b-149">`OnGetCallbackAsync` &ndash; Executes when a previously registered user signs into the app.</span></span>

<span data-ttu-id="7499b-150">*Account/ExternalLogin.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7499b-150">*Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=34-35)]

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnGetCallbackAsync&highlight=31-32)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="7499b-151">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="7499b-151">How to add additional custom tokens</span></span>

<span data-ttu-id="7499b-152">若要示範如何新增自訂權杖，它會儲存為一部分`SaveTokens`，範例應用程式會新增<xref:Microsoft.AspNetCore.Authentication.AuthenticationToken>與目前<xref:System.DateTime>如[AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*)的`TicketCreated`:</span><span class="sxs-lookup"><span data-stu-id="7499b-152">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/AdditionalClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=10-21)]

## <a name="sample-app-instructions"></a><span data-ttu-id="7499b-153">範例應用程式的指示</span><span class="sxs-lookup"><span data-stu-id="7499b-153">Sample app instructions</span></span>

<span data-ttu-id="7499b-154">範例應用程式示範如何：</span><span class="sxs-lookup"><span data-stu-id="7499b-154">The sample app demonstrates how to:</span></span>

* <span data-ttu-id="7499b-155">從 Google 取得使用者的性別和儲存性別宣告的值。</span><span class="sxs-lookup"><span data-stu-id="7499b-155">Obtain the user's gender from Google and store a gender claim with the value.</span></span>
* <span data-ttu-id="7499b-156">將 Google 存取權杖儲存在使用者的`AuthenticationProperties`。</span><span class="sxs-lookup"><span data-stu-id="7499b-156">Store the Google access token in the user's `AuthenticationProperties`.</span></span>

<span data-ttu-id="7499b-157">若要使用範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="7499b-157">To use the sample app:</span></span>

1. <span data-ttu-id="7499b-158">註冊應用程式，並取得有效的用戶端識別碼和 Google 驗證的用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="7499b-158">Register the app and obtain a valid client ID and client secret for Google authentication.</span></span> <span data-ttu-id="7499b-159">如需詳細資訊，請參閱<xref:security/authentication/google-logins>。</span><span class="sxs-lookup"><span data-stu-id="7499b-159">For more information, see <xref:security/authentication/google-logins>.</span></span>
1. <span data-ttu-id="7499b-160">提供用戶端識別碼和應用程式中的用戶端祕密<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>的`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="7499b-160">Provide the client ID and client secret to the app in the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> of `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="7499b-161">執行應用程式，並要求我宣告頁面。</span><span class="sxs-lookup"><span data-stu-id="7499b-161">Run the app and request the My Claims page.</span></span> <span data-ttu-id="7499b-162">當使用者未登入時，應用程式重新導向至 Google。</span><span class="sxs-lookup"><span data-stu-id="7499b-162">When the user isn't signed in, the app redirects to Google.</span></span> <span data-ttu-id="7499b-163">使用 Google 登入。</span><span class="sxs-lookup"><span data-stu-id="7499b-163">Sign in with Google.</span></span> <span data-ttu-id="7499b-164">Google 使用者重新導向回到應用程式 (`/Home/MyClaims`)。</span><span class="sxs-lookup"><span data-stu-id="7499b-164">Google redirects the user back to the app (`/Home/MyClaims`).</span></span> <span data-ttu-id="7499b-165">使用者經過驗證，而且我的宣告頁面載入。</span><span class="sxs-lookup"><span data-stu-id="7499b-165">The user is authenticated, and the My Claims page is loaded.</span></span> <span data-ttu-id="7499b-166">性別宣告是底下**使用者宣告**從 Google 取得的值。</span><span class="sxs-lookup"><span data-stu-id="7499b-166">The gender claim is present under **User Claims** with the value obtained from Google.</span></span> <span data-ttu-id="7499b-167">存取權杖會出現在**驗證屬性**。</span><span class="sxs-lookup"><span data-stu-id="7499b-167">The access token appears in the **Authentication Properties**.</span></span>

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

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]
