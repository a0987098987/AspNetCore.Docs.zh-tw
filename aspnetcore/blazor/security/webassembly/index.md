---
title: 安全 ASP.NET CoreBlazor WebAssembly
author: guardrex
description: 瞭解如何以 Blazor 單一頁面應用程式 (spa) 保護 WebAssemlby 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/01/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/index
ms.openlocfilehash: 0ff580dd7cbefdfe3121b30490f99e0235d93bc3
ms.sourcegitcommit: 14c3d111f9d656c86af36ecb786037bf214f435c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86176152"
---
# <a name="secure-aspnet-core-blazor-webassembly"></a><span data-ttu-id="7ad17-103">安全 ASP.NET CoreBlazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="7ad17-103">Secure ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="7ad17-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="7ad17-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

Blazor WebAssembly<span data-ttu-id="7ad17-105">應用程式的保護方式與單一頁面應用程式 (Spa) 相同。</span><span class="sxs-lookup"><span data-stu-id="7ad17-105"> apps are secured in the same manner as Single Page Applications (SPAs).</span></span> <span data-ttu-id="7ad17-106">有數種方法可以向 Spa 驗證使用者，但最常見且完整的方法是使用以[OAuth 2.0 通訊協定](https://oauth.net/)為基礎的執行，例如[Open ID Connect (OIDC) ](https://openid.net/connect/)。</span><span class="sxs-lookup"><span data-stu-id="7ad17-106">There are several approaches for authenticating users to SPAs, but the most common and comprehensive approach is to use an implementation based on the [OAuth 2.0 protocol](https://oauth.net/), such as [Open ID Connect (OIDC)](https://openid.net/connect/).</span></span>

## <a name="authentication-library"></a><span data-ttu-id="7ad17-107">驗證程式庫</span><span class="sxs-lookup"><span data-stu-id="7ad17-107">Authentication library</span></span>

Blazor WebAssembly<span data-ttu-id="7ad17-108">支援透過程式庫使用 OIDC 來驗證和授權應用程式 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-108"> supports authenticating and authorizing apps using OIDC via the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) library.</span></span> <span data-ttu-id="7ad17-109">程式庫提供一組基本類型，可針對 ASP.NET Core 後端順暢地進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7ad17-109">The library provides a set of primitives for seamlessly authenticating against ASP.NET Core backends.</span></span> <span data-ttu-id="7ad17-110">程式庫整合 Identity 了 ASP.NET Core，並在[ Identity 伺服器](https://identityserver.io/)之上建立 API 授權支援。</span><span class="sxs-lookup"><span data-stu-id="7ad17-110">The library integrates ASP.NET Core Identity with API authorization support built on top of [Identity Server](https://identityserver.io/).</span></span> <span data-ttu-id="7ad17-111">程式庫可以針對支援 OIDC 的任何協力廠商 Identity 提供者進行驗證 (IP) ，這稱為 OpenID 提供者 (OP) 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-111">The library can authenticate against any third-party Identity Provider (IP) that supports OIDC, which are called OpenID Providers (OP).</span></span>

<span data-ttu-id="7ad17-112">中的驗證支援 Blazor WebAssembly 是建置於程式庫之上 `oidc-client.js` ，用來處理基礎驗證通訊協定的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="7ad17-112">The authentication support in Blazor WebAssembly is built on top of the `oidc-client.js` library, which is used to handle the underlying authentication protocol details.</span></span>

<span data-ttu-id="7ad17-113">驗證 Spa 的其他選項是否存在，例如使用 SameSite cookie。</span><span class="sxs-lookup"><span data-stu-id="7ad17-113">Other options for authenticating SPAs exist, such as the use of SameSite cookies.</span></span> <span data-ttu-id="7ad17-114">不過，的工程設計 Blazor WebAssembly 是以 OAuth 和 OIDC 作為在應用程式中進行驗證的最佳選項來進行 Blazor WebAssembly 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-114">However, the engineering design of Blazor WebAssembly is settled on OAuth and OIDC as the best option for authentication in Blazor WebAssembly apps.</span></span> <span data-ttu-id="7ad17-115">以 JSON Web 權杖為基礎的[權杖型驗證](xref:security/anti-request-forgery#token-based-authentication) [ (jwt) ](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)由[cookie 型驗證](xref:security/anti-request-forgery#cookie-based-authentication)所選擇，以因應功能和安全性的理由：</span><span class="sxs-lookup"><span data-stu-id="7ad17-115">[Token-based authentication](xref:security/anti-request-forgery#token-based-authentication) based on [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) was chosen over [cookie-based authentication](xref:security/anti-request-forgery#cookie-based-authentication) for functional and security reasons:</span></span>

* <span data-ttu-id="7ad17-116">使用以權杖為基礎的通訊協定提供較小的受攻擊面，因為權杖不會在所有要求中傳送。</span><span class="sxs-lookup"><span data-stu-id="7ad17-116">Using a token-based protocol offers a smaller attack surface area, as the tokens aren't sent in all requests.</span></span>
* <span data-ttu-id="7ad17-117">伺服器端點不需要保護[跨網站偽造要求 (CSRF) ](xref:security/anti-request-forgery) ，因為權杖是以明確的形式傳送。</span><span class="sxs-lookup"><span data-stu-id="7ad17-117">Server endpoints don't require protection against [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) because the tokens are sent explicitly.</span></span> <span data-ttu-id="7ad17-118">這可讓您將 Blazor WebAssembly 應用程式與 MVC 或 Razor pages 應用程式裝載在一起。</span><span class="sxs-lookup"><span data-stu-id="7ad17-118">This allows you to host Blazor WebAssembly apps alongside MVC or Razor pages apps.</span></span>
* <span data-ttu-id="7ad17-119">權杖的許可權比 cookie 窄。</span><span class="sxs-lookup"><span data-stu-id="7ad17-119">Tokens have narrower permissions than cookies.</span></span> <span data-ttu-id="7ad17-120">例如，除非明確地執行這類功能，否則權杖無法用來管理使用者帳戶或變更使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="7ad17-120">For example, tokens can't be used to manage the user account or change a user's password unless such functionality is explicitly implemented.</span></span>
* <span data-ttu-id="7ad17-121">權杖的存留期很短，預設為一小時，這會限制攻擊時段。</span><span class="sxs-lookup"><span data-stu-id="7ad17-121">Tokens have a short lifetime, one hour by default, which limits the attack window.</span></span> <span data-ttu-id="7ad17-122">權杖也可以隨時撤銷。</span><span class="sxs-lookup"><span data-stu-id="7ad17-122">Tokens can also be revoked at any time.</span></span>
* <span data-ttu-id="7ad17-123">獨立 Jwt 提供驗證程式的用戶端和伺服器保證。</span><span class="sxs-lookup"><span data-stu-id="7ad17-123">Self-contained JWTs offer guarantees to the client and server about the authentication process.</span></span> <span data-ttu-id="7ad17-124">例如，用戶端的方法是偵測並驗證它所收到的權杖是否合法，並在指定的驗證程式中發出。</span><span class="sxs-lookup"><span data-stu-id="7ad17-124">For example, a client has the means to detect and validate that the tokens it receives are legitimate and were emitted as part of a given authentication process.</span></span> <span data-ttu-id="7ad17-125">如果協力廠商嘗試在驗證程式中途切換權杖，用戶端就可以偵測出切換的權杖，並避免使用它。</span><span class="sxs-lookup"><span data-stu-id="7ad17-125">If a third party attempts to switch a token in the middle of the authentication process, the client can detect the switched token and avoid using it.</span></span>
* <span data-ttu-id="7ad17-126">OAuth 和 OIDC 的權杖不依賴使用者代理程式正確運作，以確保應用程式的安全。</span><span class="sxs-lookup"><span data-stu-id="7ad17-126">Tokens with OAuth and OIDC don't rely on the user agent behaving correctly to ensure that the app is secure.</span></span>
* <span data-ttu-id="7ad17-127">以權杖為基礎的通訊協定，例如 OAuth 和 OIDC，可讓您使用相同的安全性特性集來驗證和授權託管和獨立應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ad17-127">Token-based protocols, such as OAuth and OIDC, allow for authenticating and authorizing hosted and standalone apps with the same set of security characteristics.</span></span>

## <a name="authentication-process-with-oidc"></a><span data-ttu-id="7ad17-128">使用 OIDC 的驗證程式</span><span class="sxs-lookup"><span data-stu-id="7ad17-128">Authentication process with OIDC</span></span>

<span data-ttu-id="7ad17-129">連結 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 庫提供數個基本專案，以使用 OIDC 來執行驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="7ad17-129">The [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) library offers several primitives to implement authentication and authorization using OIDC.</span></span> <span data-ttu-id="7ad17-130">就廣義而言，驗證的運作方式如下：</span><span class="sxs-lookup"><span data-stu-id="7ad17-130">In broad terms, authentication works as follows:</span></span>

* <span data-ttu-id="7ad17-131">當匿名使用者選取 [登入] 按鈕或要求已套用屬性的頁面時 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) ，系統會將使用者重新導向至應用程式的登入頁面， (`/authentication/login`) 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-131">When an anonymous user selects the login button or requests a page with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute applied, the user is redirected to the app's login page (`/authentication/login`).</span></span>
* <span data-ttu-id="7ad17-132">在登入頁面中，驗證程式庫會準備重新導向至授權端點。</span><span class="sxs-lookup"><span data-stu-id="7ad17-132">In the login page, the authentication library prepares for a redirect to the authorization endpoint.</span></span> <span data-ttu-id="7ad17-133">授權端點不在 Blazor WebAssembly 應用程式的外部，而且可以在不同的來源託管。</span><span class="sxs-lookup"><span data-stu-id="7ad17-133">The authorization endpoint is outside of the Blazor WebAssembly app and can be hosted at a separate origin.</span></span> <span data-ttu-id="7ad17-134">端點負責判斷使用者是否已驗證，以及是否要在回應中發出一或多個權杖。</span><span class="sxs-lookup"><span data-stu-id="7ad17-134">The endpoint is responsible for determining whether the user is authenticated and for issuing one or more tokens in response.</span></span> <span data-ttu-id="7ad17-135">驗證程式庫會提供登入回呼，以接收驗證回應。</span><span class="sxs-lookup"><span data-stu-id="7ad17-135">The authentication library provides a login callback to receive the authentication response.</span></span>
  * <span data-ttu-id="7ad17-136">如果使用者未經過驗證，則會將使用者重新導向至基礎驗證系統，這通常是 ASP.NET Core Identity 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-136">If the user isn't authenticated, the user is redirected to the underlying authentication system, which is usually ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="7ad17-137">如果使用者已經過驗證，則授權端點會產生適當的權杖，並將瀏覽器重新導向回到登入回呼端點 (`/authentication/login-callback`) 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-137">If the user was already authenticated, the authorization endpoint generates the appropriate tokens and redirects the browser back to the login callback endpoint (`/authentication/login-callback`).</span></span>
* <span data-ttu-id="7ad17-138">當 Blazor WebAssembly 應用程式載入登入回呼端點 (`/authentication/login-callback`) 時，就會處理驗證回應。</span><span class="sxs-lookup"><span data-stu-id="7ad17-138">When the Blazor WebAssembly app loads the login callback endpoint (`/authentication/login-callback`), the authentication response is processed.</span></span>
  * <span data-ttu-id="7ad17-139">如果驗證程式成功完成，則會驗證使用者，並選擇性地將其傳送回給使用者要求的原始受保護 URL。</span><span class="sxs-lookup"><span data-stu-id="7ad17-139">If the authentication process completes successfully, the user is authenticated and optionally sent back to the original protected URL that the user requested.</span></span>
  * <span data-ttu-id="7ad17-140">如果驗證程式因任何原因而失敗，則會將使用者傳送至 [登入失敗] 頁面 (`/authentication/login-failed`) ，並顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="7ad17-140">If the authentication process fails for any reason, the user is sent to the login failed page (`/authentication/login-failed`), and an error is displayed.</span></span>

## <a name="authentication-component"></a><span data-ttu-id="7ad17-141">`Authentication` 元件</span><span class="sxs-lookup"><span data-stu-id="7ad17-141">`Authentication` component</span></span>

<span data-ttu-id="7ad17-142">`Authentication`元件 (`Pages/Authentication.razor`) 會處理遠端驗證作業，並允許應用程式：</span><span class="sxs-lookup"><span data-stu-id="7ad17-142">The `Authentication` component (`Pages/Authentication.razor`) handles remote authentication operations and permits the app to:</span></span>

* <span data-ttu-id="7ad17-143">設定驗證狀態的應用程式路由。</span><span class="sxs-lookup"><span data-stu-id="7ad17-143">Configure app routes for authentication states.</span></span>
* <span data-ttu-id="7ad17-144">設定驗證狀態的 UI 內容。</span><span class="sxs-lookup"><span data-stu-id="7ad17-144">Set UI content for authentication states.</span></span>
* <span data-ttu-id="7ad17-145">管理驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="7ad17-145">Manage authentication state.</span></span>

<span data-ttu-id="7ad17-146">驗證動作（例如註冊或登入使用者）會傳遞至 Blazor 架構的 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorViewCore%601> 元件，這會保存並控制驗證作業之間的狀態。</span><span class="sxs-lookup"><span data-stu-id="7ad17-146">Authentication actions, such as registering or signing in a user, are passed to the Blazor framework's <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorViewCore%601> component, which persists and controls state across authentication operations.</span></span>

<span data-ttu-id="7ad17-147">如需詳細資訊和範例，請參閱 <xref:blazor/security/webassembly/additional-scenarios>。</span><span class="sxs-lookup"><span data-stu-id="7ad17-147">For more information and examples, see <xref:blazor/security/webassembly/additional-scenarios>.</span></span>

## <a name="authorization"></a><span data-ttu-id="7ad17-148">授權</span><span class="sxs-lookup"><span data-stu-id="7ad17-148">Authorization</span></span>

<span data-ttu-id="7ad17-149">在 Blazor WebAssembly 應用程式中，可以略過授權檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="7ad17-149">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="7ad17-150">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ad17-150">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="7ad17-151">**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**</span><span class="sxs-lookup"><span data-stu-id="7ad17-151">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

## <a name="refresh-tokens"></a><span data-ttu-id="7ad17-152">重新整理權杖</span><span class="sxs-lookup"><span data-stu-id="7ad17-152">Refresh tokens</span></span>

<span data-ttu-id="7ad17-153">重新整理權杖無法在應用程式中保護用戶端 Blazor WebAssembly 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-153">Refresh tokens can't be secured client-side in Blazor WebAssembly apps.</span></span> <span data-ttu-id="7ad17-154">因此，不應將重新整理權杖傳送至應用程式，以供直接使用。</span><span class="sxs-lookup"><span data-stu-id="7ad17-154">Therefore, refresh tokens shouldn't be sent to the app for direct use.</span></span>

<span data-ttu-id="7ad17-155">在託管解決方案中，伺服器端應用程式可以維護及使用重新整理權杖 Blazor WebAssembly 來存取協力廠商 api。</span><span class="sxs-lookup"><span data-stu-id="7ad17-155">Refresh tokens can be maintained and used by the server-side app in a Hosted Blazor WebAssembly solution to access third-party APIs.</span></span> <span data-ttu-id="7ad17-156">如需詳細資訊，請參閱 <xref:blazor/security/webassembly/additional-scenarios#authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party> 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-156">For more information, see <xref:blazor/security/webassembly/additional-scenarios#authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party>.</span></span>

## <a name="implementation-guidance"></a><span data-ttu-id="7ad17-157">實作指引</span><span class="sxs-lookup"><span data-stu-id="7ad17-157">Implementation guidance</span></span>

<span data-ttu-id="7ad17-158">此*總覽*底下的文章提供有關在 Blazor WebAssembly 應用程式中針對特定提供者驗證使用者的資訊。</span><span class="sxs-lookup"><span data-stu-id="7ad17-158">Articles under this *Overview* provide information on authenticating users in Blazor WebAssembly apps against specific providers.</span></span>

<span data-ttu-id="7ad17-159">獨立 Blazor WebAssembly 應用程式：</span><span class="sxs-lookup"><span data-stu-id="7ad17-159">Standalone Blazor WebAssembly apps:</span></span>

* [<span data-ttu-id="7ad17-160">OIDC 提供者和 WebAssembly Authentication 程式庫的一般指引</span><span class="sxs-lookup"><span data-stu-id="7ad17-160">General guidance for OIDC providers and the WebAssembly Authentication Library</span></span>](xref:blazor/security/webassembly/standalone-with-authentication-library)
* [<span data-ttu-id="7ad17-161">Microsoft 帳戶</span><span class="sxs-lookup"><span data-stu-id="7ad17-161">Microsoft Accounts</span></span>](xref:blazor/security/webassembly/standalone-with-microsoft-accounts)
* [<span data-ttu-id="7ad17-162">Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="7ad17-162">Azure Active Directory (AAD)</span></span>](xref:blazor/security/webassembly/standalone-with-azure-active-directory)
* [<span data-ttu-id="7ad17-163">Azure Active Directory (AAD) B2C</span><span class="sxs-lookup"><span data-stu-id="7ad17-163">Azure Active Directory (AAD) B2C</span></span>](xref:blazor/security/webassembly/standalone-with-azure-active-directory-b2c)

<span data-ttu-id="7ad17-164">託管 Blazor WebAssembly 應用程式：</span><span class="sxs-lookup"><span data-stu-id="7ad17-164">Hosted Blazor WebAssembly apps:</span></span>

* [<span data-ttu-id="7ad17-165">Azure Active Directory (AAD)</span><span class="sxs-lookup"><span data-stu-id="7ad17-165">Azure Active Directory (AAD)</span></span>](xref:blazor/security/webassembly/hosted-with-azure-active-directory)
* [<span data-ttu-id="7ad17-166">Azure Active Directory (AAD) B2C</span><span class="sxs-lookup"><span data-stu-id="7ad17-166">Azure Active Directory (AAD) B2C</span></span>](xref:blazor/security/webassembly/hosted-with-azure-active-directory-b2c)
* <span data-ttu-id="7ad17-167">[Identity伺服器](xref:blazor/security/webassembly/hosted-with-identity-server)</span><span class="sxs-lookup"><span data-stu-id="7ad17-167">[Identity Server](xref:blazor/security/webassembly/hosted-with-identity-server)</span></span>

<span data-ttu-id="7ad17-168">如需設定的進一步指引，請參閱 <xref:blazor/security/webassembly/additional-scenarios> 。</span><span class="sxs-lookup"><span data-stu-id="7ad17-168">For further guidance on configuration, see <xref:blazor/security/webassembly/additional-scenarios>.</span></span>
