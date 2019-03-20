---
title: 將驗證和身分識別移轉至 ASP.NET Core 2.0
author: scottaddie
description: 本文概述了最常見的步驟移轉 ASP.NET Core 1.x 驗證和身分識別為 ASP.NET Core 2.0。
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: d11d41c82236436096660a24df81a3df4da0fb8e
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265362"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>將驗證和身分識別移轉至 ASP.NET Core 2.0

藉由[Scott Addie](https://github.com/scottaddie)和[Hao 是一隻](https://github.com/HaoK)

ASP.NET Core 2.0 具有新的模型來驗證和[識別](xref:security/authentication/identity)可簡化使用服務的組態。 使用驗證或識別的 ASP.NET Core 1.x 應用程式可以更新為使用新的模型，如下所述。

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>驗證中介軟體和服務

在 1.x 專案中，驗證已透過中介軟體。 中介軟體的方法會叫用每個您想要支援的驗證配置。

下列範例中，1.x 設定 Facebook 驗證使用中身分識別*Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

在 2.0 專案中，驗證是透過服務設定。 在中註冊每個驗證配置`ConfigureServices`方法*Startup.cs*。 `UseIdentity`方法會取代`UseAuthentication`。

下例 2.0 設定 Facebook 驗證使用中身分識別*Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

`UseAuthentication`方法會將單一驗證中介軟體元件，負責自動驗證和遠端驗證要求的處理。 它會取代所有個別的中介軟體元件的單一、 共同的中介軟體元件。

以下是每個主要的驗證配置的 2.0 移轉指示。

### <a name="cookie-based-authentication"></a>以 cookie 為基礎的驗證

選取其中一個，在兩個選項，並進行必要的變更，在*Startup.cs*:

1. 使用身分識別的 cookie
    - 取代`UseIdentity`具有`UseAuthentication`在`Configure`方法：

        ```csharp
        app.UseAuthentication();
        ```

    - 叫用`AddIdentity`方法中的`ConfigureServices`將 cookie 驗證服務的方法。
    - （選擇性） 叫用`ConfigureApplicationCookie`或是`ConfigureExternalCookie`方法中的`ConfigureServices`調整的身分識別的 cookie 設定的方法。

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. 使用沒有身分識別的 cookie
    - 取代`UseCookieAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

        ```csharp
        app.UseAuthentication();
        ```

    - 叫用`AddAuthentication`並`AddCookie`中的方法`ConfigureServices`方法：

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a>JWT 持有人驗證

在變更下列*Startup.cs*:
- 取代`UseJwtBearerAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddJwtBearer`方法中的`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    此程式碼片段不會使用身分識別，因此應該藉由傳遞設定的預設配置`JwtBearerDefaults.AuthenticationScheme`至`AddAuthentication`方法。

### <a name="openid-connect-oidc-authentication"></a>OpenID Connect (OIDC) 驗證

在變更下列*Startup.cs*:

- 取代`UseOpenIdConnectAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddOpenIdConnect`方法中的`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a>Facebook 驗證

在變更下列*Startup.cs*:
- 取代`UseFacebookAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddFacebook`方法中的`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google 驗證

在變更下列*Startup.cs*:
- 取代`UseGoogleAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddGoogle`方法中的`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Microsoft 帳戶驗證

在變更下列*Startup.cs*:
- 取代`UseMicrosoftAccountAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddMicrosoftAccount`方法中的`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Twitter 驗證

在變更下列*Startup.cs*:
- 取代`UseTwitterAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddTwitter`方法中的`ConfigureServices`方法：

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>設定預設的驗證配置

在 1.x`AutomaticAuthenticate`和`AutomaticChallenge`的屬性[AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底類別要設定一種驗證配置。 發生不好的方法，強制執行。

在 2.0 中，這兩個屬性有為個別的屬性移除`AuthenticationOptions`執行個體。 在設定這些`AddAuthentication`方法呼叫內`ConfigureServices`方法*Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

在上述程式碼片段中，預設配置設為`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie")。

或者，使用的多載的版本`AddAuthentication`方法來設定多個屬性。 在下列的多載的方法範例中，預設配置設為`CookieAuthenticationDefaults.AuthenticationScheme`。 驗證配置或者在您的個人內指定`[Authorize]`屬性或授權原則。

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

如果下列條件之一成立，請在 2.0 中定義的預設配置：
- 您希望使用者會自動登入
- 您使用`[Authorize]`屬性或授權原則，而不指定結構描述

此規則的例外是`AddIdentity`方法。 這個方法會將 cookie 加入寄件者和預設值進行驗證，而且挑戰應用程式 cookie 配置的集`IdentityConstants.ApplicationScheme`。 此外，它將預設登入的配置設定為外部 cookie `IdentityConstants.ExternalScheme`。

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>使用 HttpContext 驗證延伸模組

`IAuthenticationManager`介面是 1.x 驗證系統的主要進入點。 它已取代為一組新的`HttpContext`中的擴充方法`Microsoft.AspNetCore.Authentication`命名空間。

比方說，1.x 專案參考`Authentication`屬性：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

在 2.0 專案中，匯入`Microsoft.AspNetCore.Authentication`命名空間，並刪除`Authentication`屬性參考：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows 驗證 (HTTP.sys / IISIntegration)

有兩種變化的 Windows 驗證：
1. 主應用程式只允許已驗證的使用者
2. 主機允許匿名和已驗證的使用者

上述第一種變化不會受到 2.0 的變更。

上述第二種變化會受到 2.0 的變更。 例如，您可能會允許匿名使用者到您的應用程式在 IIS 或[HTTP.sys](xref:fundamentals/servers/httpsys)層但授權的使用者，在控制器層級。 在此案例中，設定為預設配置`IISDefaults.AuthenticationScheme`在`Startup.ConfigureServices`方法：

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

若未設定的預設配置據以防止授權要求，要求無法運作。

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions 執行個體

2.0 變更的副作用是切換到使用名為選項，而不是 cookie 選項執行個體。 不再能夠自訂身分識別的 cookie 配置名稱。

比方說，1.x 專案使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)傳遞`IdentityCookieOptions`參數插入*AccountController.cs*。 從提供的執行個體來存取外部的 cookie 驗證配置：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

上述建構函式插入將不再需要在 2.0 專案中，而`_externalCookieScheme`欄位可以刪除：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme`常數可以直接使用：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>加入 POCO IdentityUser 導覽屬性

Entity Framework (EF) Core 導覽屬性的基底`IdentityUser`POCO （純舊 CLR 物件） 已移除。 如果您的 1.x 專案會使用這些屬性，以手動方式將他們新增回 2.0 的專案：

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

若要執行 EF Core 移轉時，請避免重複的外部索引鍵，將下列內容加入您`IdentityDbContext`類別`OnModelCreating`方法 (之後`base.OnModelCreating();`呼叫):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a>取代 GetExternalAuthenticationSchemes

同步方法`GetExternalAuthenticationSchemes`移除是因為希望的非同步版本。 1.x 專案中有下列的程式碼*ManageController.cs*:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

這個方法會出現在*Login.cshtml*太：

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

在 2.0 專案中，使用`GetExternalAuthenticationSchemesAsync`方法：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

在  *Login.cshtml*，則`AuthenticationScheme`中存取屬性`foreach`迴圈變更為`Name`:

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel 屬性變更

A`ManageLoginsViewModel`中使用物件`ManageLogins`動作*ManageController.cs*。 在 1.x 專案中，該物件的`OtherLogins`屬性傳回型別是`IList<AuthenticationDescription>`。 這個傳回型別需要先匯入`Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

在 2.0 專案中，傳回的型別變更為  `IList<AuthenticationScheme>`。 這個新的傳回型別必須替換`Microsoft.AspNetCore.Http.Authentication`匯入具有`Microsoft.AspNetCore.Authentication`匯入。

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>其他資源

如需其他詳細資料和討論，請參閱[Auth 2.0 討論](https://github.com/aspnet/Security/issues/1338)GitHub 上的提出問題。
