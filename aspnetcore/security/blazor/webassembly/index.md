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
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>安全ASP.NET核心Blazor網路組裝

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

BlazorWeb組裝應用的安全方式與單頁應用程式 (SPA) 相同。 有幾種方法可以對使用者進行對SP的身份驗證,但最常見和全面的方法是基於[OAuth 2.0協定的](https://oauth.net/)實現,如[開放ID連接 (OIDC)。](https://openid.net/connect/)

## <a name="authentication-library"></a>認證庫

BlazorWebAssembly 支援`Microsoft.AspNetCore.Components.WebAssembly.Authentication`透過庫使用 OIDC 對應用進行身份驗證和授權。 該庫提供了一組基元,用於針對ASP.NET核心後端無縫驗證。 該庫將ASP.NET核心識別與建構在[識別伺服器](https://identityserver.io/)之上的API授權支援整合。 庫可以針對支援 OIDC 的任何第三方識別提供者 (IP) 進行身份驗證,後者稱為 OpenID 提供者 (OP)。

WebAssemblyBlazor中的 認證支援建立在*oidc-client.js*庫之上,該庫用於處理基礎身份驗證協定的詳細資訊。

存在用於驗證 SPA 的其他選項,例如使用 SameSite Cookie。 但是,WebAssemblyBlazor的工程設計在 OAuth 和 OIDCBlazor上確定,作為 Web 組裝應用中身份驗證的最佳選擇。 基於[JSON Web 權杖 (JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)的[基於權杖的身份驗證](xref:security/anti-request-forgery#token-based-authentication)選擇於[基於 Cookie 的身份驗證](xref:security/anti-request-forgery#cookie-based-authentication),原因包括功能和安全:

* 使用基於權杖的協定可提供較小的攻擊表面積,因為權杖不會在所有請求中發送。
* 伺服器終結點不需要保護防止[跨網站請求偽造 (CSRF),](xref:security/anti-request-forgery)因為權杖是顯式發送的。 這允許您與 MVC 或 Razor 頁面應用Blazor一起託管 Web 組裝應用。
* 令牌的許可權比 Cookie 窄。 例如,令牌不能用於管理使用者帳戶或更改使用者的密碼,除非顯式實現此類功能。
* 令牌的存留期很短,默認情況下為 1 小時,這限制了攻擊視窗。 令牌也可以在任何時候被吊銷。
* 自包含的 JWT 向用戶端和伺服器提供有關身份驗證過程的保證。 例如,用戶端具有檢測和驗證其接收的權杖是否合法並作為給定身份驗證過程的一部分發出的方法。 如果第三方嘗試在身份驗證過程中切換令牌,用戶端可以檢測交換權杖並避免使用它。
* 具有 OAuth 和 OIDC 的權杖不依賴於使用者代理行為正確,以確保應用的安全。
* 基於權杖的協定(如OAuth和OIDC)允許對具有相同安全特徵集的託管和獨立應用進行身份驗證和授權。

## <a name="authentication-process-with-oidc"></a>OIDC 的身分驗證程序

該`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫提供了幾個基元,以便使用 OIDC 實現身份驗證和授權。 從廣義上講,身份驗證的工作原理如下:

* 當匿名使用者選擇登錄按鈕或請求應用該屬性的[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)頁面時,使用者將重定向到應用的登錄頁 ()。`/authentication/login`
* 在登錄頁中,身份驗證庫準備重定向到授權終結點。 授權終結點位於BlazorWebAssembly 應用之外,可以託管在單獨的源源。 終結點負責確定使用者是否經過身份驗證,並負責在回應中發出一個或多個令牌。 身份驗證庫提供登錄回調以接收身份驗證回應。
  * 如果使用者未經過身份驗證,則使用者將重定向到基礎身份驗證系統,該身份驗證系統通常ASP.NET核心標識。
  * 如果用戶已經過身份驗證,則授權終結點將生成相應的令牌,並將瀏覽器重定向回登錄回調終結點 ()。`/authentication/login-callback`
* 當BlazorWebAssembly 應用載入登入回`/authentication/login-callback`檔時 (), 身份驗證回應將處理。
  * 如果身份驗證過程成功完成,則對使用者進行身份驗證,並選擇性地將發送回使用者請求的原始受保護 URL。
  * 如果身份驗證過程由於任何原因失敗,則使用者將發送到登錄失敗頁 (),`/authentication/login-failed`並顯示錯誤。

## <a name="support-prerendering-with-authentication"></a>支援透過身份驗證預像

在遵循託管BlazorWebAssembly 應用主題中的指南後,使用以下說明建立具有以下功能的應用:

* 預呈現不需要授權的路徑。
* 不預渲染需要授權的路徑。

在用戶端應用的`Program`類別 *(Program.cs),* 將公用服務註冊分解為單獨的方法`ConfigureCommonServices`(例如:

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

在「伺服器」應用中,`Startup.ConfigureServices`註冊以下附加服務:

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

在「伺服器」應用程式的方法`Startup.Configure`中,取代為`endpoints.MapFallbackToFile("index.html")``endpoints.MapFallbackToPage("/_Host")`:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

在「伺服器」應用中,如果*頁面*資料夾不存在,則創建該資料夾。 在「伺服器」應用的 *「頁面」* 資料夾中創建 *_Host.cshtml*頁面。 將用戶端應用*wwwroot/index.html*檔案中的內容貼上*到 「頁面/_Host.cshtml」* 檔案中。 更新檔案的內容:

* 將 `@page "_Host"` 新增到檔案的頂端。
* 將`<app>Loading...</app>`標記取代為以下內容:

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
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a>託管應用和第三方登入者的選項

使用第三方提供者驗證和授權Blazor託管 WebAssembly 應用時,可以使用多種選項進行身份驗證。 您選擇的一個取決於您的方案。

如需詳細資訊，請參閱 <xref:security/authentication/social/additional-claims>。

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a>將使用者驗證為僅呼叫受保護的第三方 API

透過用戶端 OAuth 串流對第三方 API 提供程式進行身份驗證:

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 在此情節中：

* 託管應用的伺服器不扮演角色。
* 無法保護伺服器上的 API。
* 應用只能調用受保護的第三方 API。

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a>使用第三方提供者對使用者進行身份驗證,並在主機伺服器和第三方上調用受保護的 API

使用第三方登錄提供程式配置標識。 獲取第三方 API 訪問所需的權杖並儲存它們。

當用戶登錄時,標識將收集訪問和刷新權杖,作為身份驗證過程的一部分。 此時,有幾種方法可以對第三方 API 進行 API 調用。

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a>使用伺服器存取權杖取得三方存取權杖

使用伺服器上生成的訪問權杖從伺服器 API 終結點檢索第三方存取權杖。 從那裡,使用第三方訪問權杖直接從用戶端上的標識調用第三方 API 資源。

我們不建議此方法。 此方法需要將第三方訪問權杖視為為公共用戶端生成的權杖。 在 OAuth 術語中,公共應用沒有用戶端機密,因為它不能被信任安全地儲存機密,並且訪問令牌是為機密用戶端生成的。 機密用戶端是具有客戶端機密且假定能夠安全地儲存機密的用戶端。

* 第三方訪問權杖可能會被授予其他作用域,以便根據第三方為更受信任的用戶端發出權杖來執行敏感操作。
* 同樣,刷新令牌不應頒發給不受信任的客戶端,因為這樣做可以授予用戶端無限制的訪問許可權,除非實施其他限制。

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a>從用戶端對伺服器 API 進行 API 呼叫,以便呼叫第三方 API

從用戶端對伺服器 API 進行 API 呼叫。 從伺服器中檢索第三方 API 資源的訪問權杖,併發出任何必要的調用。

雖然此方法需要透過伺服器進行額外的網路跳機來調用第三方 API,但它最終會導致更安全的體驗:

* 伺服器可以存儲刷新權杖,並確保應用不會失去對第三方資源的訪問許可權。
* 應用無法從可能包含更敏感許可權的伺服器洩漏訪問令牌。
