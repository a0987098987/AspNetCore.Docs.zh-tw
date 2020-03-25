---
title: 安全 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何以單一頁面應用程式（Spa）保護 Blazor WebAssemlby 應用程式的安全。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: 652d4c61110f786396d9d5af4f131b817c40e333
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219242"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="cbd86-103">安全 ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="cbd86-103">Secure ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="cbd86-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="cbd86-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor<span data-ttu-id="cbd86-105"> WebAssembly 應用程式的保護方式與單一頁面應用程式（Spa）相同。</span><span class="sxs-lookup"><span data-stu-id="cbd86-105"> WebAssembly apps are secured in the same manner as Single Page Applications (SPAs).</span></span> <span data-ttu-id="cbd86-106">有數種方法可以向 Spa 驗證使用者，但最常見且完整的方法是使用以[oAuth 2.0 通訊協定](https://oauth.net/)為基礎的執行，例如[Open ID Connect （OIDC）](https://openid.net/connect/)。</span><span class="sxs-lookup"><span data-stu-id="cbd86-106">There are several approaches for authenticating users to SPAs, but the most common and comprehensive approach is to use an implementation based on the [oAuth 2.0 protocol](https://oauth.net/), such as [Open ID Connect (OIDC)](https://openid.net/connect/).</span></span>

## <a name="authentication-library"></a><span data-ttu-id="cbd86-107">驗證程式庫</span><span class="sxs-lookup"><span data-stu-id="cbd86-107">Authentication library</span></span>

Blazor<span data-ttu-id="cbd86-108"> WebAssembly 支援透過 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫使用 OIDC 來驗證和授權應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbd86-108"> WebAssembly supports authenticating and authorizing apps using OIDC via the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library.</span></span> <span data-ttu-id="cbd86-109">程式庫提供一組基本類型，可針對 ASP.NET Core 後端順暢地進行驗證。</span><span class="sxs-lookup"><span data-stu-id="cbd86-109">The library provides a set of primitives for seamlessly authenticating against ASP.NET Core backends.</span></span> <span data-ttu-id="cbd86-110">程式庫整合了 ASP.NET Core 身分識別與以身分[識別伺服器](https://identityserver.io/)為基礎的 API 授權支援。</span><span class="sxs-lookup"><span data-stu-id="cbd86-110">The library integrates ASP.NET Core Identity with API authorization support built on top of [Identity Server](https://identityserver.io/).</span></span> <span data-ttu-id="cbd86-111">程式庫可以針對支援 OIDC 的任何協力廠商身分識別提供者（IP）進行驗證，稱為 OpenID 提供者（OP）。</span><span class="sxs-lookup"><span data-stu-id="cbd86-111">The library can authenticate against any third-party Identity Provider (IP) that supports OIDC, which are called OpenID Providers (OP).</span></span>

<span data-ttu-id="cbd86-112">Blazor WebAssembly 中的驗證支援是建置於*oidc-client*程式庫之上，這是用來處理基礎驗證通訊協定的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="cbd86-112">The authentication support in Blazor WebAssembly is built on top of the *oidc-client.js* library, which is used to handle the underlying authentication protocol details.</span></span>

<span data-ttu-id="cbd86-113">驗證 Spa 的其他選項是否存在，例如使用 SameSite cookie。</span><span class="sxs-lookup"><span data-stu-id="cbd86-113">Other options for authenticating SPAs exist, such as the use of SameSite cookies.</span></span> <span data-ttu-id="cbd86-114">不過，Blazor WebAssembly 的工程設計會以 oAuth 和 OIDC 的形式，在 Blazor WebAssembly 應用程式中做為驗證的最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="cbd86-114">However, the engineering design of Blazor WebAssembly is settled on oAuth and OIDC as the best option for authentication in Blazor WebAssembly apps.</span></span> <span data-ttu-id="cbd86-115">以[JSON Web 權杖（jwt）](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)為基礎的[權杖型驗證](xref:security/anti-request-forgery#token-based-authentication)是針對功能和安全性原因而選擇，而不是以[cookie 為基礎的驗證](xref:security/anti-request-forgery#cookie-based-authentication)：</span><span class="sxs-lookup"><span data-stu-id="cbd86-115">[Token-based authentication](xref:security/anti-request-forgery#token-based-authentication) based on [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) was chosen over [cookie-based authentication](xref:security/anti-request-forgery#cookie-based-authentication) for functional and security reasons:</span></span>

* <span data-ttu-id="cbd86-116">使用以權杖為基礎的通訊協定提供較小的受攻擊面，因為權杖不會在所有要求中傳送。</span><span class="sxs-lookup"><span data-stu-id="cbd86-116">Using a token-based protocol offers a smaller attack surface area, as the tokens aren't sent in all requests.</span></span>
* <span data-ttu-id="cbd86-117">伺服器端點不需要保護[跨網站偽造要求（CSRF）](xref:security/anti-request-forgery) ，因為權杖是明確傳送的。</span><span class="sxs-lookup"><span data-stu-id="cbd86-117">Server endpoints don't require protection against [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) because the tokens are sent explicitly.</span></span> <span data-ttu-id="cbd86-118">這可讓您將 Blazor WebAssembly 應用程式與 MVC 或 Razor pages 應用程式裝載在一起。</span><span class="sxs-lookup"><span data-stu-id="cbd86-118">This allows you to host Blazor WebAssembly apps alongside MVC or Razor pages apps.</span></span>
* <span data-ttu-id="cbd86-119">權杖的許可權比 cookie 窄。</span><span class="sxs-lookup"><span data-stu-id="cbd86-119">Tokens have narrower permissions than cookies.</span></span> <span data-ttu-id="cbd86-120">例如，除非明確地執行這類功能，否則權杖無法用來管理使用者帳戶或變更使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="cbd86-120">For example, tokens can't be used to manage the user account or change a user's password unless such functionality is explicitly implemented.</span></span>
* <span data-ttu-id="cbd86-121">權杖的存留期很短，預設為一小時，這會限制攻擊時段。</span><span class="sxs-lookup"><span data-stu-id="cbd86-121">Tokens have a short lifetime, one hour by default, which limits the attack window.</span></span> <span data-ttu-id="cbd86-122">權杖也可以隨時撤銷。</span><span class="sxs-lookup"><span data-stu-id="cbd86-122">Tokens can also be revoked at any time.</span></span>
* <span data-ttu-id="cbd86-123">獨立 Jwt 提供驗證程式的用戶端和伺服器保證。</span><span class="sxs-lookup"><span data-stu-id="cbd86-123">Self-contained JWTs offer guarantees to the client and server about the authentication process.</span></span> <span data-ttu-id="cbd86-124">例如，用戶端的方法是偵測並驗證它所收到的權杖是否合法，並在指定的驗證程式中發出。</span><span class="sxs-lookup"><span data-stu-id="cbd86-124">For example, a client has the means to detect and validate that the tokens it receives are legitimate and were emitted as part of a given authentication process.</span></span> <span data-ttu-id="cbd86-125">如果協力廠商嘗試在驗證程式中途切換權杖，用戶端就可以偵測出切換的權杖，並避免使用它。</span><span class="sxs-lookup"><span data-stu-id="cbd86-125">If a third party attempts to switch a token in the middle of the authentication process, the client can detect the switched token and avoid using it.</span></span>
* <span data-ttu-id="cbd86-126">OAuth 和 OIDC 的權杖不依賴使用者代理程式正確運作，以確保應用程式的安全。</span><span class="sxs-lookup"><span data-stu-id="cbd86-126">Tokens with oAuth and OIDC don't rely on the user agent behaving correctly to ensure that the app is secure.</span></span>
* <span data-ttu-id="cbd86-127">以權杖為基礎的通訊協定，例如 oAuth 和 OIDC，可讓您使用相同的安全性特性集來驗證和授權託管和獨立應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbd86-127">Token-based protocols, such as oAuth and OIDC, allow for authenticating and authorizing hosted and standalone apps with the same set of security characteristics.</span></span>

## <a name="authentication-process-with-oidc"></a><span data-ttu-id="cbd86-128">使用 OIDC 的驗證程式</span><span class="sxs-lookup"><span data-stu-id="cbd86-128">Authentication process with OIDC</span></span>

<span data-ttu-id="cbd86-129">`Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫提供數個基本專案，以使用 OIDC 來執行驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="cbd86-129">The `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library offers several primitives to implement authentication and authorization using OIDC.</span></span> <span data-ttu-id="cbd86-130">就廣義而言，驗證的運作方式如下：</span><span class="sxs-lookup"><span data-stu-id="cbd86-130">In broad terms, authentication works as follows:</span></span>

* <span data-ttu-id="cbd86-131">當匿名使用者選取 [登入] 按鈕或要求已套用[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性的頁面時，系統會將使用者重新導向至應用程式的登入頁面（`/authentication/login`）。</span><span class="sxs-lookup"><span data-stu-id="cbd86-131">When an anonymous user selects the login button or requests a page with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute applied, the user is redirected to the app's login page (`/authentication/login`).</span></span>
* <span data-ttu-id="cbd86-132">在登入頁面中，驗證程式庫會準備重新導向至授權端點。</span><span class="sxs-lookup"><span data-stu-id="cbd86-132">In the login page, the authentication library prepares for a redirect to the authorization endpoint.</span></span> <span data-ttu-id="cbd86-133">授權端點位於 Blazor WebAssembly 應用程式之外，而且可以在不同的來源託管。</span><span class="sxs-lookup"><span data-stu-id="cbd86-133">The authorization endpoint is outside of the Blazor WebAssembly app and can be hosted at a separate origin.</span></span> <span data-ttu-id="cbd86-134">端點負責判斷使用者是否已驗證，以及是否要在回應中發出一或多個權杖。</span><span class="sxs-lookup"><span data-stu-id="cbd86-134">The endpoint is responsible for determining whether the user is authenticated and for issuing one or more tokens in response.</span></span> <span data-ttu-id="cbd86-135">驗證程式庫會提供登入回呼，以接收驗證回應。</span><span class="sxs-lookup"><span data-stu-id="cbd86-135">The authentication library provides a login callback to receive the authentication response.</span></span>
  * <span data-ttu-id="cbd86-136">如果使用者未經過驗證，則會將使用者重新導向至基礎驗證系統，這通常是 ASP.NET Core 身分識別。</span><span class="sxs-lookup"><span data-stu-id="cbd86-136">If the user isn't authenticated, the user is redirected to the underlying authentication system, which is usually ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="cbd86-137">如果使用者已經過驗證，則授權端點會產生適當的權杖，並將瀏覽器重新導向回登入回呼端點（`/authentication/login-callback`）。</span><span class="sxs-lookup"><span data-stu-id="cbd86-137">If the user was already authenticated, the authorization endpoint generates the appropriate tokens and redirects the browser back to the login callback endpoint (`/authentication/login-callback`).</span></span>
* <span data-ttu-id="cbd86-138">當 Blazor WebAssembly 應用程式載入登入回呼端點（`/authentication/login-callback`）時，就會處理驗證回應。</span><span class="sxs-lookup"><span data-stu-id="cbd86-138">When the Blazor WebAssembly app loads the login callback endpoint (`/authentication/login-callback`), the authentication response is processed.</span></span>
  * <span data-ttu-id="cbd86-139">如果驗證程式成功完成，則會驗證使用者，並選擇性地將其傳送回給使用者要求的原始受保護 URL。</span><span class="sxs-lookup"><span data-stu-id="cbd86-139">If the authentication process completes successfully, the user is authenticated and optionally sent back to the original protected URL that the user requested.</span></span>
  * <span data-ttu-id="cbd86-140">如果驗證程式因任何原因而失敗，則會將使用者傳送至登入失敗頁面（`/authentication/login-failed`），並顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="cbd86-140">If the authentication process fails for any reason, the user is sent to the login failed page (`/authentication/login-failed`), and an error is displayed.</span></span>
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a><span data-ttu-id="cbd86-141">託管應用程式和協力廠商登入提供者的選項</span><span class="sxs-lookup"><span data-stu-id="cbd86-141">Options for hosted apps and third-party login providers</span></span>

<span data-ttu-id="cbd86-142">使用協力廠商提供者驗證和授權託管的 Blazor WebAssembly 應用程式時，有數個選項可用來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="cbd86-142">When authenticating and authorizing a hosted Blazor WebAssembly app with a third-party provider, there are several options available for authenticating the user.</span></span> <span data-ttu-id="cbd86-143">您選擇哪一個取決於您的案例。</span><span class="sxs-lookup"><span data-stu-id="cbd86-143">Which one you choose depends on your scenario.</span></span>

<span data-ttu-id="cbd86-144">如需詳細資訊，請參閱 <xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="cbd86-144">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a><span data-ttu-id="cbd86-145">驗證使用者只呼叫受保護的協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="cbd86-145">Authenticate users to only call protected third party APIs</span></span>

<span data-ttu-id="cbd86-146">對協力廠商 API 提供者的用戶端 oAuth 流程驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="cbd86-146">Authenticate the user with a client-side oAuth flow against the third-party API provider:</span></span>

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 <span data-ttu-id="cbd86-147">在此情節中：</span><span class="sxs-lookup"><span data-stu-id="cbd86-147">In this scenario:</span></span>

* <span data-ttu-id="cbd86-148">裝載應用程式的伺服器不扮演角色。</span><span class="sxs-lookup"><span data-stu-id="cbd86-148">The server hosting the app doesn't play a role.</span></span>
* <span data-ttu-id="cbd86-149">無法保護伺服器上的 Api。</span><span class="sxs-lookup"><span data-stu-id="cbd86-149">APIs on the server can't be protected.</span></span>
* <span data-ttu-id="cbd86-150">應用程式只能呼叫受保護的協力廠商 Api。</span><span class="sxs-lookup"><span data-stu-id="cbd86-150">The app can only call protected third-party APIs.</span></span>

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a><span data-ttu-id="cbd86-151">使用協力廠商提供者來驗證使用者，並在主機伺服器和協力廠商上呼叫受保護的 Api</span><span class="sxs-lookup"><span data-stu-id="cbd86-151">Authenticate users with a third-party provider and call protected APIs on the host server and the third party</span></span>

<span data-ttu-id="cbd86-152">使用協力廠商登入提供者來設定身分識別。</span><span class="sxs-lookup"><span data-stu-id="cbd86-152">Configure Identity with a third-party login provider.</span></span> <span data-ttu-id="cbd86-153">取得協力廠商 API 存取所需的權杖，並加以儲存。</span><span class="sxs-lookup"><span data-stu-id="cbd86-153">Obtain the tokens required for third-party API access and store them.</span></span>

<span data-ttu-id="cbd86-154">當使用者登入時，身分識別會在驗證程式中收集存取權和重新整理權杖。</span><span class="sxs-lookup"><span data-stu-id="cbd86-154">When a user logs in, Identity collects access and refresh tokens as part of the authentication process.</span></span> <span data-ttu-id="cbd86-155">此時，有幾個方法可用來對協力廠商 Api 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="cbd86-155">At that point, there are a couple of approaches available for making API calls to third-party APIs.</span></span>

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a><span data-ttu-id="cbd86-156">使用伺服器存取權杖來取出協力廠商存取權杖</span><span class="sxs-lookup"><span data-stu-id="cbd86-156">Use a server access token to retrieve the third-party access token</span></span>

<span data-ttu-id="cbd86-157">使用伺服器上產生的存取權杖，從伺服器 API 端點抓取協力廠商存取權杖。</span><span class="sxs-lookup"><span data-stu-id="cbd86-157">Use the access token generated on the server to retrieve the third-party access token from a server API endpoint.</span></span> <span data-ttu-id="cbd86-158">從該處，使用協力廠商存取權杖，直接從用戶端上的身分識別呼叫協力廠商 API 資源。</span><span class="sxs-lookup"><span data-stu-id="cbd86-158">From there, use the third-party access token to call third-party API resources directly from Identity on the client.</span></span>

<span data-ttu-id="cbd86-159">我們不建議採用這種方法。</span><span class="sxs-lookup"><span data-stu-id="cbd86-159">We don't recommend this approach.</span></span> <span data-ttu-id="cbd86-160">這種方法需要將協力廠商存取權杖視為針對公用用戶端所產生。</span><span class="sxs-lookup"><span data-stu-id="cbd86-160">This approach requires treating the third-party access token as if it were generated for a public client.</span></span> <span data-ttu-id="cbd86-161">在 oAuth 詞彙中，公用應用程式不會有用戶端密碼，因為它無法受信任而無法安全地儲存秘密，而且會為機密用戶端產生存取權杖。</span><span class="sxs-lookup"><span data-stu-id="cbd86-161">In oAuth terms, the public app doesn't have a client secret because it can't be trusted to store secrets safely, and the access token is produced for a confidential client.</span></span> <span data-ttu-id="cbd86-162">機密用戶端是具有用戶端密碼的用戶端，並假設能夠安全地儲存秘密。</span><span class="sxs-lookup"><span data-stu-id="cbd86-162">A confidential client is a client that has a client secret and is assumed to be able to safely store secrets.</span></span>

* <span data-ttu-id="cbd86-163">協力廠商存取權杖可能會被授與額外的範圍來執行敏感性作業，這是根據協力廠商針對較受信任的用戶端發出權杖的事實。</span><span class="sxs-lookup"><span data-stu-id="cbd86-163">The third-party access token might be granted additional scopes to perform sensitive operations based on the fact that the third-party emitted the token for a more trusted client.</span></span>
* <span data-ttu-id="cbd86-164">同樣地，重新整理權杖不應發給不受信任的用戶端，因為這樣做會讓用戶端無限制存取，除非有其他限制。</span><span class="sxs-lookup"><span data-stu-id="cbd86-164">Similarly, refresh tokens shouldn't be issued to a client that isn't trusted, as doing so gives the client unlimited access unless other restrictions are put into place.</span></span>

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a><span data-ttu-id="cbd86-165">從用戶端對伺服器 API 進行 API 呼叫，以便呼叫協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="cbd86-165">Make API calls from the client to the server API in order to call third-party APIs</span></span>

<span data-ttu-id="cbd86-166">從用戶端對伺服器 API 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="cbd86-166">Make an API call from the client to the server API.</span></span> <span data-ttu-id="cbd86-167">從伺服器中，取出協力廠商 API 資源的存取權杖，併發出任何需要的呼叫。</span><span class="sxs-lookup"><span data-stu-id="cbd86-167">From the server, retrieve the access token for the third-party API resource and issue whatever call is necessary.</span></span>

<span data-ttu-id="cbd86-168">雖然這種方法需要透過伺服器額外的網路躍點來呼叫協力廠商 API，但最終會導致更安全的體驗：</span><span class="sxs-lookup"><span data-stu-id="cbd86-168">While this approach requires an extra network hop through the server to call a third-party API, it ultimately results in a safer experience:</span></span>

* <span data-ttu-id="cbd86-169">伺服器可以儲存重新整理權杖，並確保應用程式不會失去協力廠商資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="cbd86-169">The server can store refresh tokens and ensure that the app doesn't lose access to third-party resources.</span></span>
* <span data-ttu-id="cbd86-170">應用程式無法從可能包含更多敏感性許可權的伺服器洩漏存取權杖。</span><span class="sxs-lookup"><span data-stu-id="cbd86-170">The app can't leak access tokens from the server that might contain more sensitive permissions.</span></span>
