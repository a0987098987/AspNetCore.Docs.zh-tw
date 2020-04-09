---
title: ASP.NET核心身份驗證概述
author: mjrousos
description: 瞭解ASP.NET核心中的身份驗證。
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2020
uid: security/authentication/index
ms.openlocfilehash: 404904ecfa30d1fe7e47f0daaa423ddd6f1b06e8
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79434326"
---
# <a name="overview-of-aspnet-core-authentication"></a>ASP.NET核心身份驗證概述

由[邁克·盧梭斯](https://github.com/mjrousos)

身份驗證是確定使用者身份的過程。 [授權](xref:security/authorization/introduction)是確定使用者是否有權訪問資源的過程。 在ASP.NET核心中,身份驗證由`IAuthenticationService`由身份驗證[中間件](xref:fundamentals/middleware/index)使用。 身份驗證服務使用已註冊的身份驗證處理程式完成與身份驗證相關的操作。 與認證相關的操作的範例包括:

* 驗證使用者。
* 當未經身份驗證的用戶嘗試訪問受限資源時回應。

註冊的身份驗證處理程式及其配置選項稱為"方案"。

認證專案通過在 中`Startup.ConfigureServices`註冊身份驗證服務來指定:

* 以在呼叫後呼叫特定於機制的延伸方法`services.AddAuthentication`(`AddJwtBearer`例如`AddCookie`, 或 。 這些擴充方法使用[身份驗證生成器.AddScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*)註冊具有適當設置的方案。
* 不太常見,通過直接調用[身份驗證生成器.AddScheme。](xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder.AddScheme*)

例如,以下代碼註冊 Cookie 和 JWT 承載身份驗證方案的身份驗證服務和處理程式:

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options => Configuration.Bind("JwtSettings", options))
    .AddCookie(CookieAuthenticationDefaults.AuthenticationScheme, options => Configuration.Bind("CookieSettings", options));
```

參數`AddAuthentication``JwtBearerDefaults.AuthenticationScheme`是默認情況下在未請求特定方案時要使用的方案的名稱。

如果使用多個方案,授權策略(或授權屬性)可以指定它們所依賴的[身份驗證方案(或方案)](xref:security/authorization/limitingidentitybyscheme)以對使用者進行身份驗證。 在上面的示例中,Cookie 身份驗證方案可以通過指定其`CookieAuthenticationDefaults.AuthenticationScheme`名稱( 預設情況下,儘管`AddCookie`在調用 時可能提供不同名稱)來使用。

在某些情況下,調用`AddAuthentication`是由其他擴展方法自動進行的。 例如,在使用[ASP.NET 核心標識](xref:security/authentication/identity)時,`AddAuthentication`在內部調用。

透過在應用程式的 呼叫`Startup.Configure`裝置,<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>將新增身份認證的`IApplicationBuilder`中間件 。 調用`UseAuthentication`註冊使用以前註冊的身份驗證方案的中間件。 在`UseAuthentication`依賴於使用者經過身份驗證的任何中間件之前調用。 使用終結點路由時,調用`UseAuthentication`必須轉到:

* 之後`UseRouting`,以便路由資訊可用於身份驗證決策。
* 在`UseEndpoints`之前,以便在訪問終結點之前對用戶進行身份驗證。

## <a name="authentication-concepts"></a>驗證概念

### <a name="authentication-scheme"></a>驗證配置

認證專案是對應於:

* 驗證處理常式。
* 用於配置處理程式的特定實例的選項。

方案作為引用關聯處理程序的身份驗證、質詢和禁止行為的機制非常有用。 例如,授權策略可以使用方案名稱來指定應使用哪些身份驗證方案(或方案)對使用者進行身份驗證。 配置身份驗證時,通常指定預設身份驗證方案。 除非資源請求特定方案,否則將使用預設方案。 也可以:

* 指定用於身份驗證、質詢和禁止操作的不同預設方案。
* 使用[策略方案](xref:security/authentication/policyschemes)將多個方案合併為一個方案。

### <a name="authentication-handler"></a>驗證處理常式

認證處理者:

* 是實現方案行為的類型。
* 衍生或<xref:Microsoft.AspNetCore.Authentication.IAuthenticationHandler><xref:Microsoft.AspNetCore.Authentication.AuthenticationHandler`1>。
* 對用戶進行身份驗證負有主要責任。

根據身份驗證配置與傳入的要求上下文,身份驗證處理程式:

* 如果<xref:Microsoft.AspNetCore.Authentication.AuthenticationTicket>身份驗證成功,則構造表示用戶標識的物件。
* 如果身份驗證不成功,則返回"無結果"或"失敗"。
* 在使用者嘗試存取資源時,具有質詢和禁止操作的方法:
  * 它們未經授權訪問(禁止)。
  * 當他們未經身份驗證時(質詢)。

### <a name="authenticate"></a>Authenticate

身份驗證方案的身份驗證操作負責根據請求上下文構造使用者的身份。 它返回指示<xref:Microsoft.AspNetCore.Authentication.AuthenticateResult>身份驗證是否成功,如果是,則返回身份驗證票證中的用戶標識。 請參閱＜<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync%2A>＞。 認證範例包括:

* 從 Cookie 構造用戶標識的 Cookie 身份驗證方案。
* JWT 承載方案可反序列化和驗證 JWT 承載權杖以建構使用者的身份。

### <a name="challenge"></a>挑戰

當未經身份驗證的使用者請求需要身份驗證的終結點時,授權會調用身份驗證質詢。 例如,當匿名使用者請求受限資源或單擊登錄連結時,將發出身份驗證質詢。 授權使用指定的身份驗證方案調用質詢,或者在未指定任何身份驗證方案時調用預設值。 請參閱＜<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync%2A>＞。 認證質詢範例包括:

* Cookie 身份驗證方案將使用者重定向到登錄頁。
* 返回帶標頭的 401`www-authenticate: bearer`結果的 JWT 承載方案。

質詢操作應讓使用者知道使用什麼身份驗證機制來訪問請求的資源。

### <a name="forbid"></a>禁止

當經過身份驗證的用戶嘗試訪問不允許其訪問的資源時,授權調用身份驗證方案的禁止操作。 請參閱＜<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync%2A>＞。 禁止身份驗證的範例包括:
* 禁止將用戶重定向到指示訪問許可權的頁面的 Cookie 身份驗證方案。
* 返回 403 結果的 JWT 承載計劃。
* 自定義身份驗證方案重定向到使用者可以請求訪問資源的頁面。

禁止操作可以讓使用者知道:

* 它們經過身份驗證。
* 不允許他們訪問請求的資源。

有關挑戰和禁止之間的差異,請參閱以下連結:

* [使用操作資源處理程式挑戰和禁止](xref:security/authorization/resourcebased#challenge-and-forbid-with-an-operational-resource-handler)。
* [挑戰與關閉的差異](xref:security/authorization/secure-data#challenge)。

## <a name="authentication-providers-per-tenant"></a>每個租戶的身份驗證提供者

ASP.NET核心框架沒有用於多租戶身份驗證的內置解決方案。
雖然客戶當然可以使用內建功能編寫一個功能,但我們建議客戶為此查看[Orchard Core。](https://www.orchardcore.net/)

果園核心是:

* 使用 ASP.NET Core 構建的開源模組化和多租戶應用框架。
* 構建在應用框架之上的內容管理系統 (CMS)。

有關每個租戶的身份驗證提供程式的範例,請參閱[Orchard Core](https://github.com/OrchardCMS/OrchardCore)源。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authentication/policyschemes>
* <xref:security/authorization/secure-data>
