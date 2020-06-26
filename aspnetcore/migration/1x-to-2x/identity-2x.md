---
title: 將驗證遷移 Identity 到 ASP.NET Core 2。0
author: scottaddie
description: 本文概述遷移 ASP.NET Core 1.x 驗證和 ASP.NET Core 2.0 的最常見步驟 Identity 。
ms.author: scaddie
ms.date: 06/21/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: dacf6fa7191f51f36b9ba65a90746a26f958fc03
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408665"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a>將驗證遷移 Identity 到 ASP.NET Core 2。0

由[Scott Addie](https://github.com/scottaddie)和[Hao Kung](https://github.com/HaoK)

ASP.NET Core 2.0 具有用於驗證的新模型，並 [Identity](xref:security/authentication/identity) 使用服務來簡化設定。 使用驗證的 ASP.NET Core 1.x 應用程式，或 Identity 可以更新為使用新的模型，如下所述。

## <a name="update-namespaces"></a>更新命名空間

在1.x 中，在 `IdentityRole` `IdentityUser` 命名空間中找到如和的類別 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 。

在2.0 中， <xref:Microsoft.AspNetCore.Identity> 命名空間會成為這類類別的新首頁。 使用預設程式 Identity 代碼時，受影響的類別包括 `ApplicationUser` 和 `Startup` 。 調整您 `using` 的語句，以解決受影響的參考。

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>驗證中介軟體和服務

在1.x 專案中，驗證是透過中介軟體來設定。 系統會針對您想要支援的每個驗證配置叫用中介軟體方法。

下列1.x 範例會使用 Startup.cs 中的來設定 Facebook 驗證 Identity ： *Startup.cs*

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

在2.0 專案中，會透過服務來設定驗證。 每個驗證配置都會在 `ConfigureServices` *Startup.cs*的方法中註冊。 `UseIdentity`方法已由取代 `UseAuthentication` 。

下列2.0 範例會使用 Startup.cs 中的來設定 Facebook 驗證 Identity ： *Startup.cs*

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

`UseAuthentication`方法會新增單一驗證中介軟體元件，負責自動驗證和處理遠端驗證要求。 它會以單一通用中介軟體元件取代所有個別中介軟體元件。

以下是每個主要驗證配置的2.0 遷移指示。

### <a name="cookie-based-authentication"></a>以 Cookie 為基礎的驗證

選取下列兩個選項的其中一個，並在*Startup.cs*中進行必要的變更：

1. 使用 cookie 搭配Identity
    - `UseIdentity` `UseAuthentication` 在方法中，將取代為 `Configure` ：

        ```csharp
        app.UseAuthentication();
        ```

    - 叫 `AddIdentity` 用方法中的方法 `ConfigureServices` ，以新增 cookie 驗證服務。
    - （選擇性） `ConfigureApplicationCookie` `ConfigureExternalCookie` 在方法中叫用或方法， `ConfigureServices` 以調整 Identity cookie 設定。

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. 不使用 cookieIdentity
    - `UseCookieAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

        ```csharp
        app.UseAuthentication();
        ```

    - 叫 `AddAuthentication` `AddCookie` 用方法中的和方法 `ConfigureServices` ：

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

在*Startup.cs*中進行下列變更：
- `UseJwtBearerAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

    ```csharp
    app.UseAuthentication();
    ```

- `AddJwtBearer`在方法中叫用方法 `ConfigureServices` ：

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    此程式碼片段不會使用 Identity ，因此應該藉由傳遞至方法來設定預設配置 `JwtBearerDefaults.AuthenticationScheme` `AddAuthentication` 。

### <a name="openid-connect-oidc-authentication"></a>OpenID Connect （OIDC）驗證

在*Startup.cs*中進行下列變更：

- `UseOpenIdConnectAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

    ```csharp
    app.UseAuthentication();
    ```

- `AddOpenIdConnect`在方法中叫用方法 `ConfigureServices` ：

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

- `PostLogoutRedirectUri`將動作中的屬性取代 `OpenIdConnectOptions` 為 `SignedOutRedirectUri` ：

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a>Facebook 驗證

在*Startup.cs*中進行下列變更：
- `UseFacebookAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

    ```csharp
    app.UseAuthentication();
    ```

- `AddFacebook`在方法中叫用方法 `ConfigureServices` ：

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a>Google 驗證

在*Startup.cs*中進行下列變更：
- `UseGoogleAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

    ```csharp
    app.UseAuthentication();
    ```

- `AddGoogle`在方法中叫用方法 `ConfigureServices` ：

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a>Microsoft 帳戶驗證

如需 Microsoft 帳戶驗證的詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/14455)。

在*Startup.cs*中進行下列變更：
- `UseMicrosoftAccountAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

    ```csharp
    app.UseAuthentication();
    ```

- `AddMicrosoftAccount`在方法中叫用方法 `ConfigureServices` ：

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a>Twitter 驗證

在*Startup.cs*中進行下列變更：
- `UseTwitterAuthentication`將方法中的方法呼叫取代 `Configure` 為 `UseAuthentication` ：

    ```csharp
    app.UseAuthentication();
    ```

- `AddTwitter`在方法中叫用方法 `ConfigureServices` ：

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a>設定預設驗證配置

在1.x 中， `AutomaticAuthenticate` `AutomaticChallenge` [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基類的和屬性是要在單一驗證配置上設定。 沒有任何好方法可以強制執行此作業。

在2.0 中，這兩個屬性已移除為個別實例上的屬性 `AuthenticationOptions` 。 您可以在 Startup.cs 的方法中，于方法呼叫中設定它們 `AddAuthentication` `ConfigureServices` ： *Startup.cs*

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

在上述程式碼片段中，預設配置會設定為 `CookieAuthenticationDefaults.AuthenticationScheme` （"cookie"）。

或者，使用方法的多載版本 `AddAuthentication` 來設定一個以上的屬性。 在下列多載方法範例中，預設配置設定為 `CookieAuthenticationDefaults.AuthenticationScheme` 。 您也可以在個別 `[Authorize]` 屬性或授權原則內指定驗證配置。

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

如果下列其中一個條件成立，請在2.0 中定義預設配置：
- 您想要讓使用者自動登入
- 您可以使用 `[Authorize]` 屬性或授權原則，而不需要指定配置

此規則的例外狀況是 `AddIdentity` 方法。 這個方法會為您新增 cookie，並將預設的驗證和挑戰配置設定為應用程式 cookie `IdentityConstants.ApplicationScheme` 。 此外，它會將預設登入配置設定為外部 cookie `IdentityConstants.ExternalScheme` 。

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>使用 HttpCoNtext 驗證延伸模組

`IAuthenticationManager`介面是 1. x 驗證系統的主要進入點。 它已由命名空間中的一組新 `HttpContext` 擴充方法取代 `Microsoft.AspNetCore.Authentication` 。

例如，1. x 個專案會參考 `Authentication` 屬性：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

在2.0 專案中，匯入 `Microsoft.AspNetCore.Authentication` 命名空間，並刪除 `Authentication` 屬性參考：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows 驗證（HTTP.sys/IISIntegration）

Windows 驗證有兩種變化：

* 主機只允許已驗證的使用者。 這項差異不會受到2.0 變更的影響。
* 主機允許匿名和已驗證的使用者。 這項變化會受到2.0 變更的影響。 例如，應用程式應該允許[IIS](xref:host-and-deploy/iis/index)或[HTTP.sys](xref:fundamentals/servers/httpsys)層的匿名使用者，但在控制站層級授權使用者。 在此案例中，請在方法中設定預設配置 `Startup.ConfigureServices` 。

  針對[IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)，將預設配置設為 `IISDefaults.AuthenticationScheme` ：

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  針對[HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)，將預設配置設為 `HttpSysDefaults.AuthenticationScheme` ：

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  無法設定預設配置，會導致授權（挑戰）要求無法使用下列例外狀況：

  > `System.InvalidOperationException`：未指定 authenticationScheme，而且找不到任何 DefaultChallengeScheme。

如需詳細資訊，請參閱 <xref:security/authentication/windowsauth> 。

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions 實例

2.0 變更的副作用是，切換為使用已命名的選項，而不是 cookie 選項實例。 Identity已移除自訂 cookie 配置名稱的功能。

例如，1.x 專案會使用函式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)將參數傳遞至 `IdentityCookieOptions` *AccountController.cs*和*ManageController.cs*。 外部 cookie 驗證配置會從提供的實例進行存取：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

上述的函式插入在2.0 專案中會變得不必要，而且 `_externalCookieScheme` 可以刪除欄位：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

1.x 專案使用 `_externalCookieScheme` 欄位，如下所示：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

在2.0 專案中，將上述程式碼取代為下列程式碼。 `IdentityConstants.ExternalScheme`常數可以直接使用。

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

藉 `SignOutAsync` 由匯入下列命名空間來解析新增的呼叫：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>新增 IdentityUser POCO 導覽屬性

基底 `IdentityUser` POCO （簡單的 CLR 物件）的 Entity Framework （EF） Core 導覽屬性已經移除。 如果您的1.x 專案使用這些屬性，請手動將其新增回2.0 專案：

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

若要在執行 EF Core 遷移時避免重複的外鍵，請將下列內容新增至 `IdentityDbContext` 類別的 `OnModelCreating` 方法（在 `base.OnModelCreating();` 呼叫之後）：

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

已移除同步方法， `GetExternalAuthenticationSchemes` 以改用非同步版本。 1.x 專案在 controller */ManageController*中具有下列程式碼：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

這個方法也會出現在*Views/Account/Login 中。 cshtml* ：

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

在2.0 專案中，請使用 <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> 方法。 *ManageController.cs*中的變更與下列程式碼類似：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

在*登入. cshtml*中， `AuthenticationScheme` 迴圈中存取的屬性會 `foreach` 變更為 `Name` ：

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel 屬性變更

`ManageLoginsViewModel`物件用於 `ManageLogins` *ManageController.cs*的動作中。 在1.x 專案中，物件的屬性傳回 `OtherLogins` 型別為 `IList<AuthenticationDescription>` 。 此傳回類型需要匯入 `Microsoft.AspNetCore.Http.Authentication` ：

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

在2.0 專案中，傳回型別會變更為 `IList<AuthenticationScheme>` 。 這個新的傳回型別需要以匯入取代匯 `Microsoft.AspNetCore.Http.Authentication` 入 `Microsoft.AspNetCore.Authentication` 。

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>其他資源

如需詳細資訊，請參閱 GitHub 上的[Auth 2.0 問題討論](https://github.com/aspnet/Security/issues/1338)。
