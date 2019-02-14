---
title: ASP.NET Core 中的特定結構描述的授權
author: rick-anderson
description: 這篇文章說明如何使用多個驗證方法時，限制至特定的結構描述的身分識別。
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248195"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>ASP.NET Core 中的特定結構描述的授權

在某些情況下，例如單一頁面應用程式 (Spa)，它會使用多個驗證方法。 比方說，應用程式可能會使用以 cookie 為基礎的驗證來登入和 JWT 持有人驗證 JavaScript 要求。 在某些情況下，應用程式可能有多個執行個體的驗證處理常式。 例如，兩個，其中包含基本的身分識別的 cookie 處理常式，另一個被建立時已觸發 multi-factor authentication (MFA)。 因為使用者要求的作業需要額外的安全性，可能會觸發 MFA。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

驗證服務設定期間驗證時，名為一種驗證配置。 例如: 

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

在上述程式碼中，已新增兩個驗證處理常式： 一個用於 cookie，一個用於持有人。

>[!NOTE]
>指定預設結構描述會導致`HttpContext.User`屬性設定為該身分識別。 如果不需要該行為，將它停用藉由叫用的無參數形式`AddAuthentication`。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在驗證期間設定驗證中介軟體時，會命名為驗證配置。 例如: 

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

在上述程式碼中，已新增兩個驗證中介軟體： 一個用於 cookie，一個用於持有人。

>[!NOTE]
>指定預設結構描述會導致`HttpContext.User`屬性設定為該身分識別。 如果不需要該行為，將它停用藉由設定`AuthenticationOptions.AutomaticAuthenticate`屬性設`false`。

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>選取 Authorize 屬性結構描述

在 授權應用程式會指出要使用的處理常式。 選取與應用程式會藉由傳遞以逗號分隔清單中的驗證配置授權的處理常式`[Authorize]`。 `[Authorize]`屬性會指定驗證配置，或是不論預設值設定使用的配置。 例如: 

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

在上述範例中，將 cookie 與 持有人的處理常式會執行，並有機會建立並附加目前使用者的身分識別。 藉由指定單一配置，會執行對應的處理常式。

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

在上述程式碼中，只有具有"Bearer"配置處理常式會執行。 任何以 cookie 為基礎的身分識別會被忽略。

## <a name="selecting-the-scheme-with-policies"></a>選取的配置原則

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

在上述範例中，"Over18"原則只會針對執行 「 持有人 」 處理常式所建立的身分識別。 使用原則，只要`[Authorize]`屬性的`Policy`屬性：

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>使用多個驗證配置

某些應用程式可能需要支援多種類型的驗證。 例如，您的應用程式可能會驗證使用者從 Azure Active Directory 和使用者資料庫。 另一個範例是驗證使用者，從 Active Directory Federation Services 與 Azure Active Directory B2C 的應用程式。 在此情況下，應用程式應該接受從數個簽發者的 JWT 持有人權杖。

新增所有您想要接受的驗證配置。 例如，下列程式碼中`Startup.ConfigureServices`新增兩個不同的簽發者使用 JWT 持有人驗證配置：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> 只有一個 JWT 持有人驗證已註冊的預設驗證配置`JwtBearerDefaults.AuthenticationScheme`。 註冊以唯一的驗證配置需要額外的驗證。

下一個步驟是更新預設的授權原則，以接受兩個驗證配置。 例如: 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

因為預設的授權原則覆寫時，就可以使用`[Authorize]`控制器中的屬性。 然後，控制器會接受要求的第一個或第二個簽發者所簽發的 jwt。

::: moniker-end
