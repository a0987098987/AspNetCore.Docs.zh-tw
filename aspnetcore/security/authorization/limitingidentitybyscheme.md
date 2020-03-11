---
title: 在 ASP.NET Core 中使用特定配置進行授權
author: rick-anderson
description: 本文說明如何在使用多個驗證方法時，將身分識別限制為特定的配置。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: a3be2b8171c146beef7e62c8f7e55883ca5dc687
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661816"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a>在 ASP.NET Core 中使用特定配置進行授權

在某些情況下，例如單頁應用程式（Spa），通常會使用多個驗證方法。 例如，應用程式可能會使用以 cookie 為基礎的驗證來登入，並針對 JavaScript 要求進行 JWT 持有人驗證。 在某些情況下，應用程式可能會有多個驗證處理常式實例。 例如，兩個 cookie 處理常式，其中一個包含基本身分識別，而另一個則是在觸發多重要素驗證（MFA）時建立。 因為使用者要求的作業需要額外的安全性，所以可能會觸發 MFA。 如需在使用者要求需要 MFA 的資源時強制執行 MFA 的詳細資訊，請參閱 GitHub 問題[保護區段與 mfa](https://github.com/dotnet/AspNetCore.Docs/issues/15791#issuecomment-580464195)。

驗證架構會在驗證期間設定驗證服務時命名。 例如：

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

在上述程式碼中，已新增兩個驗證處理常式：一個用於 cookie，一個用於持有人。

>[!NOTE]
>指定預設配置會導致 `HttpContext.User` 屬性設定為該身分識別。 如果不想要該行為，請叫用 `AddAuthentication`的無參數形式來停用它。

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a>選取具有 [授權] 屬性的配置

在授權的時候，應用程式會指出要使用的處理常式。 藉由將以逗號分隔的驗證架構清單傳遞給 `[Authorize]`，選取應用程式將授權的處理常式。 不論是否已設定預設值，`[Authorize]` 屬性都會指定要使用的驗證配置或配置。 例如：

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

在上述範例中，cookie 和持有人處理常式都會執行，而且有機會建立和附加目前使用者的身分識別。 藉由僅指定單一配置，會執行對應的處理常式。

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

在上述程式碼中，只有具有「持有人」配置的處理常式才會執行。 系統會忽略任何以 cookie 為基礎的身分識別。

## <a name="selecting-the-scheme-with-policies"></a>選取具有原則的配置

如果您想要在[原則](xref:security/authorization/policies)中指定所需的配置，可以在新增原則時設定 `AuthenticationSchemes` 集合：

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

在上述範例中，"Over18" 原則只會針對「持有人」處理常式所建立的身分識別來執行。 藉由設定 `[Authorize]` 屬性的 `Policy` 屬性來使用原則：

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a>使用多個驗證配置

某些應用程式可能需要支援多種類型的驗證。 例如，您的應用程式可能會從 Azure Active Directory 和使用者資料庫驗證使用者。 另一個範例是從 Active Directory 同盟服務和 Azure Active Directory B2C 驗證使用者的應用程式。 在此情況下，應用程式應該接受來自數個簽發者的 JWT 持有人權杖。

新增您想要接受的所有驗證配置。 例如，`Startup.ConfigureServices` 中的下列程式碼會新增具有不同簽發者的兩個 JWT 持有人驗證配置：

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
> 只有一個 JWT 持有人驗證會向 `JwtBearerDefaults.AuthenticationScheme`的預設驗證配置進行註冊。 額外的驗證必須使用唯一的驗證配置進行註冊。

下一個步驟是更新預設授權原則，以接受這兩種驗證配置。 例如：

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

當預設的授權原則遭到覆寫時，可以使用控制器中的 `[Authorize]` 屬性。 然後，控制器會接受第一個或第二個簽發者所發出的 JWT 要求。

::: moniker-end
