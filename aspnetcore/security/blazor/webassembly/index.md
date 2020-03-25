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
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>安全 ASP.NET Core Blazor WebAssembly

By [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Blazor WebAssembly 應用程式的保護方式與單一頁面應用程式（Spa）相同。 有數種方法可以向 Spa 驗證使用者，但最常見且完整的方法是使用以[oAuth 2.0 通訊協定](https://oauth.net/)為基礎的執行，例如[Open ID Connect （OIDC）](https://openid.net/connect/)。

## <a name="authentication-library"></a>驗證程式庫

Blazor WebAssembly 支援透過 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫使用 OIDC 來驗證和授權應用程式。 程式庫提供一組基本類型，可針對 ASP.NET Core 後端順暢地進行驗證。 程式庫整合了 ASP.NET Core 身分識別與以身分[識別伺服器](https://identityserver.io/)為基礎的 API 授權支援。 程式庫可以針對支援 OIDC 的任何協力廠商身分識別提供者（IP）進行驗證，稱為 OpenID 提供者（OP）。

Blazor WebAssembly 中的驗證支援是建置於*oidc-client*程式庫之上，這是用來處理基礎驗證通訊協定的詳細資料。

驗證 Spa 的其他選項是否存在，例如使用 SameSite cookie。 不過，Blazor WebAssembly 的工程設計會以 oAuth 和 OIDC 的形式，在 Blazor WebAssembly 應用程式中做為驗證的最佳選擇。 以[JSON Web 權杖（jwt）](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)為基礎的[權杖型驗證](xref:security/anti-request-forgery#token-based-authentication)是針對功能和安全性原因而選擇，而不是以[cookie 為基礎的驗證](xref:security/anti-request-forgery#cookie-based-authentication)：

* 使用以權杖為基礎的通訊協定提供較小的受攻擊面，因為權杖不會在所有要求中傳送。
* 伺服器端點不需要保護[跨網站偽造要求（CSRF）](xref:security/anti-request-forgery) ，因為權杖是明確傳送的。 這可讓您將 Blazor WebAssembly 應用程式與 MVC 或 Razor pages 應用程式裝載在一起。
* 權杖的許可權比 cookie 窄。 例如，除非明確地執行這類功能，否則權杖無法用來管理使用者帳戶或變更使用者的密碼。
* 權杖的存留期很短，預設為一小時，這會限制攻擊時段。 權杖也可以隨時撤銷。
* 獨立 Jwt 提供驗證程式的用戶端和伺服器保證。 例如，用戶端的方法是偵測並驗證它所收到的權杖是否合法，並在指定的驗證程式中發出。 如果協力廠商嘗試在驗證程式中途切換權杖，用戶端就可以偵測出切換的權杖，並避免使用它。
* OAuth 和 OIDC 的權杖不依賴使用者代理程式正確運作，以確保應用程式的安全。
* 以權杖為基礎的通訊協定，例如 oAuth 和 OIDC，可讓您使用相同的安全性特性集來驗證和授權託管和獨立應用程式。

## <a name="authentication-process-with-oidc"></a>使用 OIDC 的驗證程式

`Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫提供數個基本專案，以使用 OIDC 來執行驗證和授權。 就廣義而言，驗證的運作方式如下：

* 當匿名使用者選取 [登入] 按鈕或要求已套用[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性的頁面時，系統會將使用者重新導向至應用程式的登入頁面（`/authentication/login`）。
* 在登入頁面中，驗證程式庫會準備重新導向至授權端點。 授權端點位於 Blazor WebAssembly 應用程式之外，而且可以在不同的來源託管。 端點負責判斷使用者是否已驗證，以及是否要在回應中發出一或多個權杖。 驗證程式庫會提供登入回呼，以接收驗證回應。
  * 如果使用者未經過驗證，則會將使用者重新導向至基礎驗證系統，這通常是 ASP.NET Core 身分識別。
  * 如果使用者已經過驗證，則授權端點會產生適當的權杖，並將瀏覽器重新導向回登入回呼端點（`/authentication/login-callback`）。
* 當 Blazor WebAssembly 應用程式載入登入回呼端點（`/authentication/login-callback`）時，就會處理驗證回應。
  * 如果驗證程式成功完成，則會驗證使用者，並選擇性地將其傳送回給使用者要求的原始受保護 URL。
  * 如果驗證程式因任何原因而失敗，則會將使用者傳送至登入失敗頁面（`/authentication/login-failed`），並顯示錯誤。
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a>託管應用程式和協力廠商登入提供者的選項

使用協力廠商提供者驗證和授權託管的 Blazor WebAssembly 應用程式時，有數個選項可用來驗證使用者。 您選擇哪一個取決於您的案例。

如需詳細資訊，請參閱 <xref:security/authentication/social/additional-claims>。

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a>驗證使用者只呼叫受保護的協力廠商 Api

對協力廠商 API 提供者的用戶端 oAuth 流程驗證使用者：

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 在此情節中：

* 裝載應用程式的伺服器不扮演角色。
* 無法保護伺服器上的 Api。
* 應用程式只能呼叫受保護的協力廠商 Api。

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a>使用協力廠商提供者來驗證使用者，並在主機伺服器和協力廠商上呼叫受保護的 Api

使用協力廠商登入提供者來設定身分識別。 取得協力廠商 API 存取所需的權杖，並加以儲存。

當使用者登入時，身分識別會在驗證程式中收集存取權和重新整理權杖。 此時，有幾個方法可用來對協力廠商 Api 進行 API 呼叫。

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a>使用伺服器存取權杖來取出協力廠商存取權杖

使用伺服器上產生的存取權杖，從伺服器 API 端點抓取協力廠商存取權杖。 從該處，使用協力廠商存取權杖，直接從用戶端上的身分識別呼叫協力廠商 API 資源。

我們不建議採用這種方法。 這種方法需要將協力廠商存取權杖視為針對公用用戶端所產生。 在 oAuth 詞彙中，公用應用程式不會有用戶端密碼，因為它無法受信任而無法安全地儲存秘密，而且會為機密用戶端產生存取權杖。 機密用戶端是具有用戶端密碼的用戶端，並假設能夠安全地儲存秘密。

* 協力廠商存取權杖可能會被授與額外的範圍來執行敏感性作業，這是根據協力廠商針對較受信任的用戶端發出權杖的事實。
* 同樣地，重新整理權杖不應發給不受信任的用戶端，因為這樣做會讓用戶端無限制存取，除非有其他限制。

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a>從用戶端對伺服器 API 進行 API 呼叫，以便呼叫協力廠商 Api

從用戶端對伺服器 API 進行 API 呼叫。 從伺服器中，取出協力廠商 API 資源的存取權杖，併發出任何需要的呼叫。

雖然這種方法需要透過伺服器額外的網路躍點來呼叫協力廠商 API，但最終會導致更安全的體驗：

* 伺服器可以儲存重新整理權杖，並確保應用程式不會失去協力廠商資源的存取權。
* 應用程式無法從可能包含更多敏感性許可權的伺服器洩漏存取權杖。
