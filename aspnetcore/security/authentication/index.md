---
title: ASP.NET Core 驗證的總覽
author: mjrousos
description: 深入瞭解 ASP.NET Core 中的驗證。
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/index
ms.openlocfilehash: a230e1ae85a54ddf16900b2ee7ed4a18d45e4ea2
ms.sourcegitcommit: 1b89fc58114a251926abadfd5c69c120f1ba12d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2020
ms.locfileid: "87160202"
---
# <a name="overview-of-aspnet-core-authentication"></a>ASP.NET Core 驗證的總覽

由[Mike Rousos](https://github.com/mjrousos)

驗證是判斷使用者身分識別的程式。 「授權」（ [Authorization](xref:security/authorization/introduction) ）是判斷使用者是否有權存取資源的程式。 在 ASP.NET Core 中，驗證是由 `IAuthenticationService` 驗證[中介軟體](xref:fundamentals/middleware/index)所使用的來處理。 驗證服務會使用已註冊的驗證處理常式來完成驗證相關的動作。 驗證相關動作的範例包括：

* 驗證使用者。
* 當未驗證的使用者嘗試存取受限制的資源時回應。

已註冊的驗證處理常式及其設定選項稱為「配置」。

驗證配置是藉由在中註冊驗證服務來指定 `Startup.ConfigureServices` ：

* 在呼叫（例如或）之後呼叫配置特定的擴充方法 `services.AddAuthentication` `AddJwtBearer` `AddCookie` 。 這些擴充方法會使用[AuthenticationBuilder](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*)來註冊具有適當設定的配置。
* 較不常用，方法是直接呼叫[AuthenticationBuilder. AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*) 。

例如，下列程式碼會註冊 cookie 和 JWT 持有人驗證配置的驗證服務和處理常式：

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options => Configuration.Bind("JwtSettings", options))
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme, options => Configuration.Bind("CookieSettings", options));
```

`AddAuthentication`參數 `JwtBearerDefaults.AuthenticationScheme` 是未要求特定配置時，預設要使用的配置名稱。

如果使用多個配置，授權原則（或授權屬性）可以[指定它們所依賴的驗證配置（或配置）](xref:security/authorization/limitingidentitybyscheme)來驗證使用者。 在上述範例中，您可以藉由指定名稱來使用 cookie 驗證配置（ `CookieAuthenticationDefaults.AuthenticationScheme` 根據預設，雖然呼叫時可能會提供不同的名稱 `AddCookie` ）。

在某些情況下，對的呼叫會 `AddAuthentication` 由其他擴充方法自動進行。 例如，使用[ASP.NET Core Identity ](xref:security/authentication/identity)時， `AddAuthentication` 會在內部呼叫。

藉 `Startup.Configure` 由在 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> 應用程式的上呼叫擴充方法，即可在中新增驗證中介軟體 `IApplicationBuilder` 。 呼叫會 `UseAuthentication` 註冊使用先前註冊之驗證配置的中介軟體。 在 `UseAuthentication` 相依于要驗證之使用者的任何中介軟體之前呼叫。 使用端點路由時，對的呼叫 `UseAuthentication` 必須：

* 之後 `UseRouting` ，就可以使用路由資訊來進行驗證決策。
* 在之前 `UseEndpoints` ，會先驗證使用者，然後再存取端點。

## <a name="authentication-concepts"></a>驗證概念

### <a name="authentication-scheme"></a>驗證配置

驗證配置是對應至的名稱：

* 驗證處理常式。
* 用於設定該特定處理常式實例的選項。

配置可做為一種機制，用來參考相關處理常式的驗證、挑戰和禁止行為。 例如，授權原則可以使用配置名稱來指定應該使用哪一種驗證配置（或配置）來驗證使用者。 設定驗證時，通常會指定預設的驗證配置。 除非資源要求特定的配置，否則會使用預設配置。 也可以：

* 指定要用於驗證、挑戰和禁止動作的不同預設配置。
* 使用[原則](xref:security/authentication/policyschemes)配置，將多個配置結合成一個。

### <a name="authentication-handler"></a>驗證處理常式

驗證處理常式：

* 是實作為配置行為的類型。
* 衍生自 <xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler> 或 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1> 。
* 主要負責驗證使用者。

根據驗證配置的設定和連入要求內容，驗證處理常式：

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket>如果驗證成功，則代表使用者身分識別的結構物件。
* 如果驗證失敗，則傳回「沒有結果」或「失敗」。
* 在使用者嘗試存取資源時，有挑戰和禁止動作的方法：
  * 他們未經授權存取（禁止）。
  * 未經驗證時（挑戰）。

### <a name="authenticate"></a>Authenticate

驗證配置的驗證動作會負責根據要求內容來建立使用者的身分識別。 它會傳回 <xref:Microsoft.AspNetCore.Authentication.AuthenticateResult> ，指出驗證是否成功，以及使用者在驗證票證中的身分識別。 請參閱＜ <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync%2A> ＞。 驗證範例包括：

* Cookie 驗證配置會從 cookie 來建立使用者的身分識別。
* JWT 持有人配置會還原序列化和驗證 JWT 持有人權杖，以建立使用者的身分識別。

### <a name="challenge"></a>挑戰

當未驗證的使用者要求需要驗證的端點時，授權就會叫用驗證挑戰。 例如，當匿名使用者要求受限制的資源，或按一下登入連結時，就會發出驗證挑戰。 授權會使用指定的驗證配置，或預設值（如果未指定）來叫用挑戰。 請參閱＜ <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync%2A> ＞。 驗證挑戰範例包括：

* Cookie 驗證配置會將使用者重新導向至登入頁面。
* JWT 持有人配置會傳回具有標頭的401結果 `www-authenticate: bearer` 。

挑戰動作應讓使用者知道要使用哪種驗證機制來存取要求的資源。

### <a name="forbid"></a>禁止

當已驗證的使用者嘗試存取不允許存取的資源時，授權會呼叫驗證配置的禁止動作。 請參閱＜ <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync%2A> ＞。 驗證禁止的範例包括：
* Cookie 驗證配置會將使用者重新導向至表示禁止存取的頁面。
* JWT 持有人配置傳回403結果。
* 自訂驗證配置會重新導向至頁面，讓使用者可以在其中要求資源的存取權。

禁止動作可讓使用者知道：

* 它們會經過驗證。
* 不允許他們存取要求的資源。

請參閱下列連結，以瞭解挑戰與禁止的差異：

* [操作資源處理常式的挑戰和禁止](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler)。
* [挑戰與禁止之間的差異](xref:security/authorization/secure-data#challenge)。

## <a name="authentication-providers-per-tenant"></a>每個租使用者的驗證提供者

ASP.NET Core framework 沒有內建解決方案可進行多租使用者驗證。
雖然客戶可以使用內建功能來撰寫，但我們建議客戶查看[Orchard Core](https://www.orchardcore.net/)以達成此目的。

Orchard 核心為：

* 以 ASP.NET Core 建立的開放原始碼模組化和多租使用者應用程式架構。
* 以該應用程式架構為基礎的內容管理系統（CMS）。

如需每個租使用者的驗證提供者範例，請參閱[Orchard 核心](https://github.com/OrchardCMS/OrchardCore)來源。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authentication/policyschemes>
* <xref:security/authorization/secure-data>
* [全域要求已驗證的使用者](xref:security/authorization/secure-data#rau)