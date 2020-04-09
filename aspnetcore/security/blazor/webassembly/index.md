---
title: 安全ASP.NET核心Blazor網路組裝
author: guardrex
description: 瞭解如何將 WebAssemby 應用程式作為單頁應用程式 (SPA) 保護Blazor。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: be286d770cd8d6e5cf7885b91be8654f74ffd743
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80538976"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="e0441-103">安全ASP.NET核心Blazor網路組裝</span><span class="sxs-lookup"><span data-stu-id="e0441-103">Secure ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="e0441-104">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="e0441-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor<span data-ttu-id="e0441-105">Web組裝應用的安全方式與單頁應用程式 (SPA) 相同。</span><span class="sxs-lookup"><span data-stu-id="e0441-105"> WebAssembly apps are secured in the same manner as Single Page Applications (SPAs).</span></span> <span data-ttu-id="e0441-106">有幾種方法可以對使用者進行對SP的身份驗證,但最常見和全面的方法是基於[OAuth 2.0協定的](https://oauth.net/)實現,如[開放ID連接 (OIDC)。](https://openid.net/connect/)</span><span class="sxs-lookup"><span data-stu-id="e0441-106">There are several approaches for authenticating users to SPAs, but the most common and comprehensive approach is to use an implementation based on the [OAuth 2.0 protocol](https://oauth.net/), such as [Open ID Connect (OIDC)](https://openid.net/connect/).</span></span>

## <a name="authentication-library"></a><span data-ttu-id="e0441-107">認證庫</span><span class="sxs-lookup"><span data-stu-id="e0441-107">Authentication library</span></span>

Blazor<span data-ttu-id="e0441-108">WebAssembly 支援`Microsoft.AspNetCore.Components.WebAssembly.Authentication`透過庫使用 OIDC 對應用進行身份驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="e0441-108"> WebAssembly supports authenticating and authorizing apps using OIDC via the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library.</span></span> <span data-ttu-id="e0441-109">該庫提供了一組基元,用於針對ASP.NET核心後端無縫驗證。</span><span class="sxs-lookup"><span data-stu-id="e0441-109">The library provides a set of primitives for seamlessly authenticating against ASP.NET Core backends.</span></span> <span data-ttu-id="e0441-110">該庫將ASP.NET核心識別與建構在[識別伺服器](https://identityserver.io/)之上的API授權支援整合。</span><span class="sxs-lookup"><span data-stu-id="e0441-110">The library integrates ASP.NET Core Identity with API authorization support built on top of [Identity Server](https://identityserver.io/).</span></span> <span data-ttu-id="e0441-111">庫可以針對支援 OIDC 的任何第三方識別提供者 (IP) 進行身份驗證,後者稱為 OpenID 提供者 (OP)。</span><span class="sxs-lookup"><span data-stu-id="e0441-111">The library can authenticate against any third-party Identity Provider (IP) that supports OIDC, which are called OpenID Providers (OP).</span></span>

<span data-ttu-id="e0441-112">WebAssemblyBlazor中的 認證支援建立在*oidc-client.js*庫之上,該庫用於處理基礎身份驗證協定的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e0441-112">The authentication support in Blazor WebAssembly is built on top of the *oidc-client.js* library, which is used to handle the underlying authentication protocol details.</span></span>

<span data-ttu-id="e0441-113">存在用於驗證 SPA 的其他選項,例如使用 SameSite Cookie。</span><span class="sxs-lookup"><span data-stu-id="e0441-113">Other options for authenticating SPAs exist, such as the use of SameSite cookies.</span></span> <span data-ttu-id="e0441-114">但是,WebAssemblyBlazor的工程設計在 OAuth 和 OIDCBlazor上確定,作為 Web 組裝應用中身份驗證的最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="e0441-114">However, the engineering design of Blazor WebAssembly is settled on OAuth and OIDC as the best option for authentication in Blazor WebAssembly apps.</span></span> <span data-ttu-id="e0441-115">基於[JSON Web 權杖 (JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)的[基於權杖的身份驗證](xref:security/anti-request-forgery#token-based-authentication)選擇於[基於 Cookie 的身份驗證](xref:security/anti-request-forgery#cookie-based-authentication),原因包括功能和安全:</span><span class="sxs-lookup"><span data-stu-id="e0441-115">[Token-based authentication](xref:security/anti-request-forgery#token-based-authentication) based on [JSON Web Tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) was chosen over [cookie-based authentication](xref:security/anti-request-forgery#cookie-based-authentication) for functional and security reasons:</span></span>

* <span data-ttu-id="e0441-116">使用基於權杖的協定可提供較小的攻擊表面積,因為權杖不會在所有請求中發送。</span><span class="sxs-lookup"><span data-stu-id="e0441-116">Using a token-based protocol offers a smaller attack surface area, as the tokens aren't sent in all requests.</span></span>
* <span data-ttu-id="e0441-117">伺服器終結點不需要保護防止[跨網站請求偽造 (CSRF),](xref:security/anti-request-forgery)因為權杖是顯式發送的。</span><span class="sxs-lookup"><span data-stu-id="e0441-117">Server endpoints don't require protection against [Cross-Site Request Forgery (CSRF)](xref:security/anti-request-forgery) because the tokens are sent explicitly.</span></span> <span data-ttu-id="e0441-118">這允許您與 MVC 或 Razor 頁面應用Blazor一起託管 Web 組裝應用。</span><span class="sxs-lookup"><span data-stu-id="e0441-118">This allows you to host Blazor WebAssembly apps alongside MVC or Razor pages apps.</span></span>
* <span data-ttu-id="e0441-119">令牌的許可權比 Cookie 窄。</span><span class="sxs-lookup"><span data-stu-id="e0441-119">Tokens have narrower permissions than cookies.</span></span> <span data-ttu-id="e0441-120">例如,令牌不能用於管理使用者帳戶或更改使用者的密碼,除非顯式實現此類功能。</span><span class="sxs-lookup"><span data-stu-id="e0441-120">For example, tokens can't be used to manage the user account or change a user's password unless such functionality is explicitly implemented.</span></span>
* <span data-ttu-id="e0441-121">令牌的存留期很短,默認情況下為 1 小時,這限制了攻擊視窗。</span><span class="sxs-lookup"><span data-stu-id="e0441-121">Tokens have a short lifetime, one hour by default, which limits the attack window.</span></span> <span data-ttu-id="e0441-122">令牌也可以在任何時候被吊銷。</span><span class="sxs-lookup"><span data-stu-id="e0441-122">Tokens can also be revoked at any time.</span></span>
* <span data-ttu-id="e0441-123">自包含的 JWT 向用戶端和伺服器提供有關身份驗證過程的保證。</span><span class="sxs-lookup"><span data-stu-id="e0441-123">Self-contained JWTs offer guarantees to the client and server about the authentication process.</span></span> <span data-ttu-id="e0441-124">例如,用戶端具有檢測和驗證其接收的權杖是否合法並作為給定身份驗證過程的一部分發出的方法。</span><span class="sxs-lookup"><span data-stu-id="e0441-124">For example, a client has the means to detect and validate that the tokens it receives are legitimate and were emitted as part of a given authentication process.</span></span> <span data-ttu-id="e0441-125">如果第三方嘗試在身份驗證過程中切換令牌,用戶端可以檢測交換權杖並避免使用它。</span><span class="sxs-lookup"><span data-stu-id="e0441-125">If a third party attempts to switch a token in the middle of the authentication process, the client can detect the switched token and avoid using it.</span></span>
* <span data-ttu-id="e0441-126">具有 OAuth 和 OIDC 的權杖不依賴於使用者代理行為正確,以確保應用的安全。</span><span class="sxs-lookup"><span data-stu-id="e0441-126">Tokens with OAuth and OIDC don't rely on the user agent behaving correctly to ensure that the app is secure.</span></span>
* <span data-ttu-id="e0441-127">基於權杖的協定(如OAuth和OIDC)允許對具有相同安全特徵集的託管和獨立應用進行身份驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="e0441-127">Token-based protocols, such as OAuth and OIDC, allow for authenticating and authorizing hosted and standalone apps with the same set of security characteristics.</span></span>

## <a name="authentication-process-with-oidc"></a><span data-ttu-id="e0441-128">OIDC 的身分驗證程序</span><span class="sxs-lookup"><span data-stu-id="e0441-128">Authentication process with OIDC</span></span>

<span data-ttu-id="e0441-129">該`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫提供了幾個基元,以便使用 OIDC 實現身份驗證和授權。</span><span class="sxs-lookup"><span data-stu-id="e0441-129">The `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library offers several primitives to implement authentication and authorization using OIDC.</span></span> <span data-ttu-id="e0441-130">從廣義上講,身份驗證的工作原理如下:</span><span class="sxs-lookup"><span data-stu-id="e0441-130">In broad terms, authentication works as follows:</span></span>

* <span data-ttu-id="e0441-131">當匿名使用者選擇登錄按鈕或請求應用該屬性的[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)頁面時,使用者將重定向到應用的登錄頁 ()。`/authentication/login`</span><span class="sxs-lookup"><span data-stu-id="e0441-131">When an anonymous user selects the login button or requests a page with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute applied, the user is redirected to the app's login page (`/authentication/login`).</span></span>
* <span data-ttu-id="e0441-132">在登錄頁中,身份驗證庫準備重定向到授權終結點。</span><span class="sxs-lookup"><span data-stu-id="e0441-132">In the login page, the authentication library prepares for a redirect to the authorization endpoint.</span></span> <span data-ttu-id="e0441-133">授權終結點位於BlazorWebAssembly 應用之外,可以託管在單獨的源源。</span><span class="sxs-lookup"><span data-stu-id="e0441-133">The authorization endpoint is outside of the Blazor WebAssembly app and can be hosted at a separate origin.</span></span> <span data-ttu-id="e0441-134">終結點負責確定使用者是否經過身份驗證,並負責在回應中發出一個或多個令牌。</span><span class="sxs-lookup"><span data-stu-id="e0441-134">The endpoint is responsible for determining whether the user is authenticated and for issuing one or more tokens in response.</span></span> <span data-ttu-id="e0441-135">身份驗證庫提供登錄回調以接收身份驗證回應。</span><span class="sxs-lookup"><span data-stu-id="e0441-135">The authentication library provides a login callback to receive the authentication response.</span></span>
  * <span data-ttu-id="e0441-136">如果使用者未經過身份驗證,則使用者將重定向到基礎身份驗證系統,該身份驗證系統通常ASP.NET核心標識。</span><span class="sxs-lookup"><span data-stu-id="e0441-136">If the user isn't authenticated, the user is redirected to the underlying authentication system, which is usually ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="e0441-137">如果用戶已經過身份驗證,則授權終結點將生成相應的令牌,並將瀏覽器重定向回登錄回調終結點 ()。`/authentication/login-callback`</span><span class="sxs-lookup"><span data-stu-id="e0441-137">If the user was already authenticated, the authorization endpoint generates the appropriate tokens and redirects the browser back to the login callback endpoint (`/authentication/login-callback`).</span></span>
* <span data-ttu-id="e0441-138">當BlazorWebAssembly 應用載入登入回`/authentication/login-callback`檔時 (), 身份驗證回應將處理。</span><span class="sxs-lookup"><span data-stu-id="e0441-138">When the Blazor WebAssembly app loads the login callback endpoint (`/authentication/login-callback`), the authentication response is processed.</span></span>
  * <span data-ttu-id="e0441-139">如果身份驗證過程成功完成,則對使用者進行身份驗證,並選擇性地將發送回使用者請求的原始受保護 URL。</span><span class="sxs-lookup"><span data-stu-id="e0441-139">If the authentication process completes successfully, the user is authenticated and optionally sent back to the original protected URL that the user requested.</span></span>
  * <span data-ttu-id="e0441-140">如果身份驗證過程由於任何原因失敗,則使用者將發送到登錄失敗頁 (),`/authentication/login-failed`並顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="e0441-140">If the authentication process fails for any reason, the user is sent to the login failed page (`/authentication/login-failed`), and an error is displayed.</span></span>

## <a name="support-prerendering-with-authentication"></a><span data-ttu-id="e0441-141">支援透過身份驗證預像</span><span class="sxs-lookup"><span data-stu-id="e0441-141">Support prerendering with authentication</span></span>

<span data-ttu-id="e0441-142">在遵循託管BlazorWebAssembly 應用主題中的指南後,使用以下說明建立具有以下功能的應用:</span><span class="sxs-lookup"><span data-stu-id="e0441-142">After following the guidance in one of the hosted Blazor WebAssembly app topics, use the following instructions to create an app that:</span></span>

* <span data-ttu-id="e0441-143">預呈現不需要授權的路徑。</span><span class="sxs-lookup"><span data-stu-id="e0441-143">Prerenders paths for which authorization isn't required.</span></span>
* <span data-ttu-id="e0441-144">不預渲染需要授權的路徑。</span><span class="sxs-lookup"><span data-stu-id="e0441-144">Doesn't prerender paths for which authorization is required.</span></span>

<span data-ttu-id="e0441-145">在用戶端應用的`Program`類別 *(Program.cs),* 將公用服務註冊分解為單獨的方法`ConfigureCommonServices`(例如:</span><span class="sxs-lookup"><span data-stu-id="e0441-145">In the Client app's `Program` class (*Program.cs*), factor common service registrations into a separate method (for example, `ConfigureCommonServices`):</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.RootComponents.Add<App>("app");

        services.AddBaseAddressHttpClient();
        services.Add...;

        ConfigureCommonServices(builder.Services);

        await builder.Build().RunAsync();
    }

    public static void ConfigureCommonServices(IServiceCollection services)
    {
        // Common service registrations
    }
}
```

<span data-ttu-id="e0441-146">在「伺服器」應用中,`Startup.ConfigureServices`註冊以下附加服務:</span><span class="sxs-lookup"><span data-stu-id="e0441-146">In the Server app's `Startup.ConfigureServices`, register the following additional services:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Server;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddRazorPages();
    services.AddScoped<AuthenticationStateProvider, ServerAuthenticationStateProvider>();
    services.AddScoped<SignOutSessionStateManager>();

    Client.Program.ConfigureCommonServices(services);
}
```

<span data-ttu-id="e0441-147">在「伺服器」應用程式的方法`Startup.Configure`中,取代為`endpoints.MapFallbackToFile("index.html")``endpoints.MapFallbackToPage("/_Host")`:</span><span class="sxs-lookup"><span data-stu-id="e0441-147">In the Server app's `Startup.Configure` method, replace `endpoints.MapFallbackToFile("index.html")` with `endpoints.MapFallbackToPage("/_Host")`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

<span data-ttu-id="e0441-148">在「伺服器」應用中,如果*頁面*資料夾不存在,則創建該資料夾。</span><span class="sxs-lookup"><span data-stu-id="e0441-148">In the Server app, create a *Pages* folder if it doesn't exist.</span></span> <span data-ttu-id="e0441-149">在「伺服器」應用的 *「頁面」* 資料夾中創建 *_Host.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="e0441-149">Create a *_Host.cshtml* page inside the Server app's *Pages* folder.</span></span> <span data-ttu-id="e0441-150">將用戶端應用*wwwroot/index.html*檔案中的內容貼上*到 「頁面/_Host.cshtml」* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="e0441-150">Paste the contents from the Client app's *wwwroot/index.html* file into the *Pages/_Host.cshtml* file.</span></span> <span data-ttu-id="e0441-151">更新檔案的內容:</span><span class="sxs-lookup"><span data-stu-id="e0441-151">Update the file's contents:</span></span>

* <span data-ttu-id="e0441-152">將 `@page "_Host"` 新增到檔案的頂端。</span><span class="sxs-lookup"><span data-stu-id="e0441-152">Add `@page "_Host"` to the top of the file.</span></span>
* <span data-ttu-id="e0441-153">將`<app>Loading...</app>`標記取代為以下內容:</span><span class="sxs-lookup"><span data-stu-id="e0441-153">Replace the `<app>Loading...</app>` tag with the following:</span></span>

  ```cshtml
  <app>
      @if (!HttpContext.Request.Path.StartsWithSegments("/authentication"))
      {
          <component type="typeof(Wasm.Authentication.Client.App)" render-mode="Static" />
      }
      else
      {
          <text>Loading...</text>
      }
  </app>
  ```
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a><span data-ttu-id="e0441-154">託管應用和第三方登入者的選項</span><span class="sxs-lookup"><span data-stu-id="e0441-154">Options for hosted apps and third-party login providers</span></span>

<span data-ttu-id="e0441-155">使用第三方提供者驗證和授權Blazor託管 WebAssembly 應用時,可以使用多種選項進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="e0441-155">When authenticating and authorizing a hosted Blazor WebAssembly app with a third-party provider, there are several options available for authenticating the user.</span></span> <span data-ttu-id="e0441-156">您選擇的一個取決於您的方案。</span><span class="sxs-lookup"><span data-stu-id="e0441-156">Which one you choose depends on your scenario.</span></span>

<span data-ttu-id="e0441-157">如需詳細資訊，請參閱 <xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="e0441-157">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a><span data-ttu-id="e0441-158">將使用者驗證為僅呼叫受保護的第三方 API</span><span class="sxs-lookup"><span data-stu-id="e0441-158">Authenticate users to only call protected third party APIs</span></span>

<span data-ttu-id="e0441-159">透過用戶端 OAuth 串流對第三方 API 提供程式進行身份驗證:</span><span class="sxs-lookup"><span data-stu-id="e0441-159">Authenticate the user with a client-side OAuth flow against the third-party API provider:</span></span>

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 <span data-ttu-id="e0441-160">在此情節中：</span><span class="sxs-lookup"><span data-stu-id="e0441-160">In this scenario:</span></span>

* <span data-ttu-id="e0441-161">託管應用的伺服器不扮演角色。</span><span class="sxs-lookup"><span data-stu-id="e0441-161">The server hosting the app doesn't play a role.</span></span>
* <span data-ttu-id="e0441-162">無法保護伺服器上的 API。</span><span class="sxs-lookup"><span data-stu-id="e0441-162">APIs on the server can't be protected.</span></span>
* <span data-ttu-id="e0441-163">應用只能調用受保護的第三方 API。</span><span class="sxs-lookup"><span data-stu-id="e0441-163">The app can only call protected third-party APIs.</span></span>

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a><span data-ttu-id="e0441-164">使用第三方提供者對使用者進行身份驗證,並在主機伺服器和第三方上調用受保護的 API</span><span class="sxs-lookup"><span data-stu-id="e0441-164">Authenticate users with a third-party provider and call protected APIs on the host server and the third party</span></span>

<span data-ttu-id="e0441-165">使用第三方登錄提供程式配置標識。</span><span class="sxs-lookup"><span data-stu-id="e0441-165">Configure Identity with a third-party login provider.</span></span> <span data-ttu-id="e0441-166">獲取第三方 API 訪問所需的權杖並儲存它們。</span><span class="sxs-lookup"><span data-stu-id="e0441-166">Obtain the tokens required for third-party API access and store them.</span></span>

<span data-ttu-id="e0441-167">當用戶登錄時,標識將收集訪問和刷新權杖,作為身份驗證過程的一部分。</span><span class="sxs-lookup"><span data-stu-id="e0441-167">When a user logs in, Identity collects access and refresh tokens as part of the authentication process.</span></span> <span data-ttu-id="e0441-168">此時,有幾種方法可以對第三方 API 進行 API 調用。</span><span class="sxs-lookup"><span data-stu-id="e0441-168">At that point, there are a couple of approaches available for making API calls to third-party APIs.</span></span>

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a><span data-ttu-id="e0441-169">使用伺服器存取權杖取得三方存取權杖</span><span class="sxs-lookup"><span data-stu-id="e0441-169">Use a server access token to retrieve the third-party access token</span></span>

<span data-ttu-id="e0441-170">使用伺服器上生成的訪問權杖從伺服器 API 終結點檢索第三方存取權杖。</span><span class="sxs-lookup"><span data-stu-id="e0441-170">Use the access token generated on the server to retrieve the third-party access token from a server API endpoint.</span></span> <span data-ttu-id="e0441-171">從那裡,使用第三方訪問權杖直接從用戶端上的標識調用第三方 API 資源。</span><span class="sxs-lookup"><span data-stu-id="e0441-171">From there, use the third-party access token to call third-party API resources directly from Identity on the client.</span></span>

<span data-ttu-id="e0441-172">我們不建議此方法。</span><span class="sxs-lookup"><span data-stu-id="e0441-172">We don't recommend this approach.</span></span> <span data-ttu-id="e0441-173">此方法需要將第三方訪問權杖視為為公共用戶端生成的權杖。</span><span class="sxs-lookup"><span data-stu-id="e0441-173">This approach requires treating the third-party access token as if it were generated for a public client.</span></span> <span data-ttu-id="e0441-174">在 OAuth 術語中,公共應用沒有用戶端機密,因為它不能被信任安全地儲存機密,並且訪問令牌是為機密用戶端生成的。</span><span class="sxs-lookup"><span data-stu-id="e0441-174">In OAuth terms, the public app doesn't have a client secret because it can't be trusted to store secrets safely, and the access token is produced for a confidential client.</span></span> <span data-ttu-id="e0441-175">機密用戶端是具有客戶端機密且假定能夠安全地儲存機密的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e0441-175">A confidential client is a client that has a client secret and is assumed to be able to safely store secrets.</span></span>

* <span data-ttu-id="e0441-176">第三方訪問權杖可能會被授予其他作用域,以便根據第三方為更受信任的用戶端發出權杖來執行敏感操作。</span><span class="sxs-lookup"><span data-stu-id="e0441-176">The third-party access token might be granted additional scopes to perform sensitive operations based on the fact that the third-party emitted the token for a more trusted client.</span></span>
* <span data-ttu-id="e0441-177">同樣,刷新令牌不應頒發給不受信任的客戶端,因為這樣做可以授予用戶端無限制的訪問許可權,除非實施其他限制。</span><span class="sxs-lookup"><span data-stu-id="e0441-177">Similarly, refresh tokens shouldn't be issued to a client that isn't trusted, as doing so gives the client unlimited access unless other restrictions are put into place.</span></span>

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a><span data-ttu-id="e0441-178">從用戶端對伺服器 API 進行 API 呼叫,以便呼叫第三方 API</span><span class="sxs-lookup"><span data-stu-id="e0441-178">Make API calls from the client to the server API in order to call third-party APIs</span></span>

<span data-ttu-id="e0441-179">從用戶端對伺服器 API 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e0441-179">Make an API call from the client to the server API.</span></span> <span data-ttu-id="e0441-180">從伺服器中檢索第三方 API 資源的訪問權杖,併發出任何必要的調用。</span><span class="sxs-lookup"><span data-stu-id="e0441-180">From the server, retrieve the access token for the third-party API resource and issue whatever call is necessary.</span></span>

<span data-ttu-id="e0441-181">雖然此方法需要透過伺服器進行額外的網路跳機來調用第三方 API,但它最終會導致更安全的體驗:</span><span class="sxs-lookup"><span data-stu-id="e0441-181">While this approach requires an extra network hop through the server to call a third-party API, it ultimately results in a safer experience:</span></span>

* <span data-ttu-id="e0441-182">伺服器可以存儲刷新權杖,並確保應用不會失去對第三方資源的訪問許可權。</span><span class="sxs-lookup"><span data-stu-id="e0441-182">The server can store refresh tokens and ensure that the app doesn't lose access to third-party resources.</span></span>
* <span data-ttu-id="e0441-183">應用無法從可能包含更敏感許可權的伺服器洩漏訪問令牌。</span><span class="sxs-lookup"><span data-stu-id="e0441-183">The app can't leak access tokens from the server that might contain more sensitive permissions.</span></span>
