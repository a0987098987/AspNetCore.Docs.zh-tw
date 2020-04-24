---
title: 保護 ASP.NET Core Blazor WebAssembly
author: guardrex
description: 瞭解如何以單一Blazor頁面應用程式（spa）保護 WebAssemlby 應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/22/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/index
ms.openlocfilehash: dba70afdb7a0a50da7ded38437860b52d2ce5f88
ms.sourcegitcommit: 7bb14d005155a5044c7902a08694ee8ccb20c113
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82110888"
---
# <a name="secure-aspnet-core-opno-locblazor-webassembly"></a>保護 ASP.NET Core Blazor WebAssembly

By [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

> [!NOTE]
> 本文中的指導方針適用于 ASP.NET Core 3.2 Preview 4。 本主題將在4月24日星期五更新為涵蓋 Preview 5。

BlazorWebAssembly apps 的保護方式與單一頁面應用程式（Spa）相同。 有數種方法可以向 Spa 驗證使用者，但最常見且完整的方法是使用以[OAuth 2.0 通訊協定](https://oauth.net/)為基礎的執行，例如[Open ID Connect （OIDC）](https://openid.net/connect/)。

## <a name="authentication-library"></a>驗證程式庫

BlazorWebAssembly 支援透過連結`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫使用 OIDC 來驗證和授權應用程式。 程式庫提供一組基本類型，可針對 ASP.NET Core 後端順暢地進行驗證。 程式庫整合了 ASP.NET Core 身分識別與以身分[識別伺服器](https://identityserver.io/)為基礎的 API 授權支援。 程式庫可以針對支援 OIDC 的任何協力廠商身分識別提供者（IP）進行驗證，稱為 OpenID 提供者（OP）。

WebAssembly 中Blazor的驗證支援是建置於*oidc-client*程式庫之上，用來處理基礎驗證通訊協定的詳細資料。

驗證 Spa 的其他選項是否存在，例如使用 SameSite cookie。 不過， Blazor WebAssembly 的工程設計是以 OAUTH 和 OIDC 作為 WebAssembly 應用程式中Blazor驗證的最佳選項來進行。 以[JSON Web 權杖（jwt）](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)為基礎的[權杖型驗證](xref:security/anti-request-forgery#token-based-authentication)是針對功能和安全性原因而選擇，而不是以[cookie 為基礎的驗證](xref:security/anti-request-forgery#cookie-based-authentication)：

* 使用以權杖為基礎的通訊協定提供較小的受攻擊面，因為權杖不會在所有要求中傳送。
* 伺服器端點不需要保護[跨網站偽造要求（CSRF）](xref:security/anti-request-forgery) ，因為權杖是明確傳送的。 這可讓您將Blazor WebAssembly 應用程式與 MVC 或 Razor pages 應用程式裝載在一起。
* 權杖的許可權比 cookie 窄。 例如，除非明確地執行這類功能，否則權杖無法用來管理使用者帳戶或變更使用者的密碼。
* 權杖的存留期很短，預設為一小時，這會限制攻擊時段。 權杖也可以隨時撤銷。
* 獨立 Jwt 提供驗證程式的用戶端和伺服器保證。 例如，用戶端的方法是偵測並驗證它所收到的權杖是否合法，並在指定的驗證程式中發出。 如果協力廠商嘗試在驗證程式中途切換權杖，用戶端就可以偵測出切換的權杖，並避免使用它。
* OAuth 和 OIDC 的權杖不依賴使用者代理程式正確運作，以確保應用程式的安全。
* 以權杖為基礎的通訊協定，例如 OAuth 和 OIDC，可讓您使用相同的安全性特性集來驗證和授權託管和獨立應用程式。

## <a name="authentication-process-with-oidc"></a>使用 OIDC 的驗證程式

連結`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫提供數個基本專案，以使用 OIDC 來執行驗證和授權。 就廣義而言，驗證的運作方式如下：

* 當匿名使用者選取 [登入] 按鈕或要求已套用[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性的頁面時，系統會將使用者重新導向至應用程式的`/authentication/login`登入頁面（）。
* 在登入頁面中，驗證程式庫會準備重新導向至授權端點。 授權端點位於Blazor WebAssembly 應用程式之外，而且可以在不同的來源託管。 端點負責判斷使用者是否已驗證，以及是否要在回應中發出一或多個權杖。 驗證程式庫會提供登入回呼，以接收驗證回應。
  * 如果使用者未經過驗證，則會將使用者重新導向至基礎驗證系統，這通常是 ASP.NET Core 身分識別。
  * 如果使用者已經過驗證，則授權端點會產生適當的權杖，並將瀏覽器重新導向回登入回呼`/authentication/login-callback`端點（）。
* 當Blazor WebAssembly 應用程式載入登入回呼端點（`/authentication/login-callback`）時，就會處理驗證回應。
  * 如果驗證程式成功完成，則會驗證使用者，並選擇性地將其傳送回給使用者要求的原始受保護 URL。
  * 如果驗證程式因任何原因而失敗，則會將使用者傳送至登入失敗頁面`/authentication/login-failed`（），並顯示錯誤。

## <a name="additional-resources"></a>其他資源

* 此*總覽*底下的文章提供有關在 WebAssembly apps Blazor中針對特定提供者驗證使用者的資訊。
* <xref:security/blazor/webassembly/additional-scenarios>
