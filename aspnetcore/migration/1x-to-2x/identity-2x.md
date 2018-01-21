---
title: "移轉的驗證和身分識別，ASP.NET Core 2.0"
author: scottaddie
description: "本文概述了最常見的步驟移轉 ASP.NET Core 1.x 驗證和身分識別為 ASP.NET Core 2.0。"
ms.author: scaddie
manager: wpickett
ms.date: 10/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 72ad31438a344fb5fa2b357c709b923b8077e742
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a>移轉的驗證和身分識別，ASP.NET Core 2.0

由[Scott Addie](https://github.com/scottaddie)和[Hao 是一隻](https://github.com/HaoK)

ASP.NET Core 2.0 具有新的模型來驗證和[識別](xref:security/authentication/identity)可簡化使用服務的組態。 使用驗證或識別的 ASP.NET Core 1.x 應用程式可以更新為使用新的模型，如下所述。

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a>驗證中介軟體和服務
1.x 專案中設定驗證透過中介軟體。 中介軟體方法會叫用每個您想要支援的驗證配置。

下例會 1.x 設定 Facebook 驗證在中使用識別*Startup.cs*:

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

在 2.0 專案中，驗證是透過服務設定。 在中註冊每個驗證配置`ConfigureServices`方法*Startup.cs*。 `UseIdentity`方法取代`UseAuthentication`。

下例會 2.0 設定 Facebook 驗證在中使用識別*Startup.cs*:

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

`UseAuthentication`方法會將單一驗證中介軟體元件，負責自動驗證和遠端驗證要求的處理。 它會取代所有個別的中介軟體元件與單一、 共同的中介軟體元件。

以下是每個主要的驗證配置 2.0 的移轉指示。

### <a name="cookie-based-authentication"></a>Cookie 為基礎的驗證
選取其中一個兩個選項，並進行必要的變更在*Startup.cs*:

1. 使用 cookie 識別
    - 取代`UseIdentity`與`UseAuthentication`中`Configure`方法：

        ```csharp
        app.UseAuthentication();
        ```

    - 叫用`AddIdentity`方法中的`ConfigureServices`方法，將 cookie 驗證服務。
    - （選擇性） 叫用`ConfigureApplicationCookie`或`ConfigureExternalCookie`方法中的`ConfigureServices`調整識別 cookie 設定的方法。

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. 身分識別不使用 cookie
    - 取代`UseCookieAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - 叫用`AddAuthentication`和`AddCookie`方法`ConfigureServices`方法：

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

### <a name="jwt-bearer-authentication"></a>JWT Bearer 驗證
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
在 1.x`AutomaticAuthenticate`和`AutomaticChallenge`屬性[AuthenticationOptions](https://docs.microsoft.com/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底類別要用來將一種驗證配置。 沒有強制這個好方法。

在 2.0 中，這兩個屬性有為個別的屬性移除`AuthenticationOptions`執行個體。 可在設定`AddAuthentication`方法呼叫內`ConfigureServices`方法*Startup.cs*:

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

在上述程式碼片段中，預設配置設定為`CookieAuthenticationDefaults.AuthenticationScheme`(稱為"Cookie")。

或者，使用的多載的版本`AddAuthentication`方法來設定多個屬性。 在下列多載的方法範例中，預設配置設定為`CookieAuthenticationDefaults.AuthenticationScheme`。 驗證配置，或者可以指定在您的個人`[Authorize]`屬性或授權原則。

```csharp
services.AddAuthentication(options => 
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

如果下列條件的其中一種情況，請在 2.0 中定義的預設配置：
- 您想要自動登入的使用者
- 您使用`[Authorize]`屬性或授權原則，而不指定結構描述

此規則的例外是`AddIdentity`方法。 這個方法會將 cookie 加入適合您和設定預設的驗證和應用程式 cookie 挑戰配置`IdentityConstants.ApplicationScheme`。 此外，它將預設登入的配置設定為外部 cookie `IdentityConstants.ExternalScheme`。

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a>使用 HttpContext 驗證延伸模組
`IAuthenticationManager`介面是 1.x 驗證系統的主要進入點。 它已取代為一組新的`HttpContext`中的擴充方法`Microsoft.AspNetCore.Authentication`命名空間。

例如，1.x 專案參考`Authentication`屬性：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

在 2.0 專案中，匯入`Microsoft.AspNetCore.Authentication`命名空間，並刪除`Authentication`屬性參考：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a>Windows 驗證 (HTTP.sys / IISIntegration)
有兩種變化，Windows 驗證：
1. 主應用程式只允許已驗證的使用者
2. 主應用程式允許匿名和已驗證的使用者

上述第一種變異不會受到 2.0 的變更。

2.0 的變更會影響上述第二種變化。 例如，您可能會允許匿名使用者到您的應用程式在 IIS 或[HTTP.sys](xref:fundamentals/servers/weblistener)圖層在控制器層級但授權使用者。 在此案例中，設定的預設配置為`IISDefaults.AuthenticationScheme`中`ConfigureServices`方法*Startup.cs*:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

若未設定的預設配置據以防止授權要求挑戰無法運作。

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a>IdentityCookieOptions 執行個體
2.0 變更的副作用是切換到使用名為選項，而不是 cookie 的選項執行個體。 移除自訂身分識別 cookie 結構描述名稱的功能。

比方說，1.x 專案使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)傳遞`IdentityCookieOptions`參數插入*AccountController.cs*。 從提供的執行個體來存取外部的 cookie 驗證配置：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

上述建構函式的資料隱碼變成不需要在 2.0 專案中，而`_externalCookieScheme`刪除欄位，可以：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

`IdentityConstants.ExternalScheme`常數可以直接使用：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a>新增 IdentityUser POCO 導覽屬性
基底的 Entity Framework (EF) 核心導覽屬性`IdentityUser`POCO （純舊 CLR 物件） 已經移除。 如果 1.x 專案使用這些屬性，以手動方式將它們加回 2.0 的專案：

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

若要執行 EF 核心移轉時，請避免重複的外部索引鍵，將下列內容加入您`IdentityDbContext`類別`OnModelCreating`方法 (之後`base.OnModelCreating();`呼叫):

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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
同步方法`GetExternalAuthenticationSchemes`改用非同步版本中移除。 1.x 專案中有下列的程式碼*ManageController.cs*:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

這個方法會出現在*Login.cshtml*太：

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

在 2.0 專案中，使用`GetExternalAuthenticationSchemesAsync`方法：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

在*Login.cshtml*、`AuthenticationScheme`中存取屬性`foreach`迴圈變為`Name`:

[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a>ManageLoginsViewModel 屬性變更
A`ManageLoginsViewModel`物件用於`ManageLogins`動作*ManageController.cs*。 1.x 專案中，物件的`OtherLogins`屬性傳回型別是`IList<AuthenticationDescription>`。 這個傳回類型需要先匯入`Microsoft.AspNetCore.Http.Authentication`:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

在 2.0 專案中，傳回型別變更為`IList<AuthenticationScheme>`。 這個新的傳回型別必須替換`Microsoft.AspNetCore.Http.Authentication`匯入`Microsoft.AspNetCore.Authentication`匯入。

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a>其他資源
如需其他詳細資料和討論，請參閱[Auth 2.0 討論](https://github.com/aspnet/Security/issues/1338)GitHub 上的問題。
