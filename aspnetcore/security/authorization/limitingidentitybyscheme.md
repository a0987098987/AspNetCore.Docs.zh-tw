---
title: "授權與特定的結構描述中的 ASP.NET Core"
author: rick-anderson
description: "本文說明如何使用多個驗證方法時，限制特定的結構描述的識別。"
ms.author: riande
manager: wpickett
ms.date: 10/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 099dba1a4235ef62ea298748645b99e2d6d12d44
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="authorize-with-a-specific-scheme"></a>授權與特定的結構描述

在某些情況下，例如單一頁面應用程式 (SPAs)，它會使用多個驗證方法。 例如，應用程式可能會使用 cookie 基本驗證來登入和 JWT bearer 驗證進行 JavaScript 要求。 在某些情況下，應用程式可能會有多個執行個體的驗證處理常式。 例如，兩個位置其中一個包含基本的身分識別的 cookie 處理常式，另一個會建立時已觸發多因素驗證 (MFA)。 因為使用者要求的作業需要額外的安全性，可能會觸發 MFA。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

在驗證期間設定的驗證服務時，會命名為驗證配置。 例如: 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

在上述程式碼中，已經加入兩個驗證處理常式： 一個用於 cookie，另一個用於承載。

>[!NOTE]
>指定預設的配置會導致`HttpContext.User`屬性設定為該身分識別。 如果不需要該行為，請停用它叫用的無參數形式`AddAuthentication`。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在驗證期間驗證 middlewares 設定時，會命名為驗證配置。 例如: 

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

在上述程式碼中，已經加入兩個驗證 middlewares： 一個用於 cookie，另一個用於承載。

>[!NOTE]
>指定預設的配置會導致`HttpContext.User`屬性設定為該身分識別。 如果不需要該行為，請停用它藉由設定`AuthenticationOptions.AutomaticAuthenticate`屬性`false`。

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>選取配置和授權屬性

在授權應用程式會指出要使用的處理常式。 選取的應用程式將授權藉由傳遞驗證配置的逗號分隔清單的處理常式`[Authorize]`。 `[Authorize]`屬性指定的配置，來使用，不論預設值設定的驗證配置。 例如: 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

在上述範例中，cookie 和承載的處理常式會執行，並有機會建立並附加目前使用者的身分識別。 藉由指定單一配置，會執行對應的處理常式。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

在上述程式碼，只使用"Bearer"配置處理常式會執行。 以 cookie 為基礎的所有識別都會被都忽略。

## <a name="selecting-the-scheme-with-policies"></a>選取原則的配置

如果您想要指定在所需的配置[原則](xref:security/authorization/policies)，您可以設定`AuthenticationSchemes`集合加入您的原則時：

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

在上述範例中，"Over18"原則只會執行比對"Bearer"的處理常式所建立的識別。 使用此原則設定`[Authorize]`屬性的`Policy`屬性：

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
