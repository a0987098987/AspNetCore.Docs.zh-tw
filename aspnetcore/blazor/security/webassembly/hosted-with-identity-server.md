---
title: Blazor WebAssembly使用伺服器保護 ASP.NET Core 託管應用 Identity 程式
author: guardrex
description: Blazor從使用[IdentityServer](https://identityserver.io/)後端的 Visual Studio 中，建立具有驗證的新託管應用程式
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/hosted-with-identity-server
ms.openlocfilehash: b0c6cbd814a23afabbbf9a0d943d28125ac1f61c
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944576"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-identity-server"></a>Blazor WebAssembly使用伺服器保護 ASP.NET Core 託管應用 Identity 程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

本文說明如何建立新的 Blazor 託管應用程式，以使用[IdentityServer](https://identityserver.io/)來驗證使用者和 API 呼叫。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio 中：

1. 建立新的 **Blazor WebAssembly** 應用程式。
1. 在 [**建立新的 Blazor 應用程式**] 對話方塊中，選取 [**驗證**] 區段中的 [**變更**]。
1. 選取 [**個別使用者帳戶**]，後面接著 **[確定]**。
1. 選取 [ **Advanced** ] 區段中的 [ **ASP.NET Core 託管**] 核取方塊。
1. 選取 [建立]**** 按鈕。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

若要在命令 shell 中建立應用程式，請執行下列命令：

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。 資料夾名稱也會成為專案名稱的一部分。

---

## <a name="server-app-configuration"></a>伺服器應用程式設定

下列各節說明當包含驗證支援時，專案的新增功能。

### <a name="startup-class"></a>啟始類別

`Startup`類別具有下列新增專案。

* 在 `Startup.ConfigureServices` 中：

  * ASP.NET Core Identity ：

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>(options => 
            options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer 使用額外 <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> 的協助程式方法，在 IdentityServer 上設定預設的 ASP.NET Core 慣例：

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * 使用其他 helper 方法進行驗證，以設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 應用程式來驗證 IdentityServer 所產生的 JWT 權杖：

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* 在 `Startup.Configure` 中：

  * IdentityServer 中介軟體會公開 Open ID Connect （OIDC）端點：

    ```csharp
    app.UseIdentityServer();
    ```

  * 驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：

    ```csharp
    app.UseAuthentication();
    ```

  * 授權中介軟體可啟用授權功能：

    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>Helper 方法會設定 ASP.NET Core 案例的[IdentityServer](https://identityserver.io/) 。 IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。 IdentityServer 會在最常見的案例中公開不必要的複雜性。 因此，會提供一組慣例和設定選項，讓我們考慮一個良好的起點。 一旦您的驗證需要變更，就可以使用 IdentityServer 的完整功能，自訂驗證以符合應用程式的需求。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>Helper 方法會將應用程式的原則配置設定為預設驗證處理常式。 此原則設定為允許 Identity 處理路由至 URL 空間中任何子路徑的所有要求 Identity `/Identity` 。 會 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 處理所有其他要求。 此外，這個方法也會：

* 向 `{APPLICATION NAME}API` IdentityServer 註冊具有預設範圍的 API 資源 `{APPLICATION NAME}API` 。
* 設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。

### <a name="weatherforecastcontroller"></a>WeatherForecastController

在 `WeatherForecastController` （ `Controllers/WeatherForecastController.cs` ）中， [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性會套用至類別。 屬性會指出使用者必須根據預設原則來存取資源。 預設的授權原則會設定為使用預設的驗證配置，這是由所設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> 。 Helper 方法會將設定 <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> 為應用程式要求的預設處理常式。

### <a name="applicationdbcontext"></a>[ApplicationdbcoNtext]

在 `ApplicationDbContext` （ `Data/ApplicationDbContext.cs` ）中，會 <xref:Microsoft.EntityFrameworkCore.DbContext> 擴充 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 以包含 IdentityServer 的架構。 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。

若要取得資料庫架構的完整控制權，請從其中一個可用的類別繼承， Identity <xref:Microsoft.EntityFrameworkCore.DbContext> 並透過 Identity `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` 在方法中呼叫來設定內容以包含架構 <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> 。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

在 `OidcConfigurationController` （ `Controllers/OidcConfigurationController.cs` ）中，會布建用戶端端點來提供 OIDC 參數。

### <a name="app-settings-files"></a>應用程式佈建檔案

在專案根目錄的應用程式佈建檔案（ `appsettings.json` ）中， `IdentityServer` 區段會描述已設定的用戶端清單。 在下列範例中，有一個用戶端。 用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId` 參數。 此設定檔會指出正在設定的應用程式類型。 此設定檔會在內部使用，以驅動可簡化伺服器設定程式的慣例。 <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "{APP ASSEMBLY}.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱（例如， `BlazorSample.Client` ）。

## <a name="client-app-configuration"></a>用戶端應用程式設定

### <a name="authentication-package"></a>驗證套件

建立應用程式以使用個別使用者帳戶（）時 `Individual` ，應用程式會 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 在應用程式的專案檔中自動接收套件的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="3.2.0" />
```

### <a name="api-authorization-support"></a>API 授權支援

驗證使用者的支援是由套件內提供的擴充方法插入服務容器中 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 。 這個方法會設定應用程式所需的服務，以與現有的授權系統互動。

```csharp
builder.Services.AddApiAuthorization();
```

根據預設，應用程式的設定會依照慣例從載入 `_configuration/{client-id}` 。 依照慣例，用戶端識別碼會設定為應用程式的元件名稱。 您可以使用選項呼叫多載，將此 URL 變更為指向不同的端點。

### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay 元件

`LoginDisplay`元件（ `Shared/LoginDisplay.razor` ）會在 `MainLayout` 元件（）中轉譯 `Shared/MainLayout.razor` ，並管理下列行為：

* 針對已驗證的使用者：
  * 顯示目前的使用者名稱。
  * 提供 ASP.NET Core 中 [使用者設定檔] 頁面的連結 Identity 。
  * 提供用來登出應用程式的按鈕。
* 匿名使用者：
  * 提供註冊的選項。
  * 提供登入的選項。

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>FetchData 元件

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>執行應用程式

從伺服器專案執行應用程式。 使用 Visual Studio 時，您可以：

* 將工具列中的 [**啟始專案**] 下拉式清單設定為*伺服器 API 應用程式*，然後選取 [**執行**] 按鈕。
* 在**方案總管**中選取伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。

## <a name="name-and-role-claim-with-api-authorization"></a>具有 API 授權的名稱和角色宣告

### <a name="custom-user-factory"></a>自訂使用者工廠

在用戶端應用程式中，建立自訂的使用者 factory。 Identity伺服器會在單一宣告中以 JSON 陣列的形式傳送多個角色 `role` 。 單一角色會當做宣告中的字串值來傳送。 Factory 會 `role` 針對每個使用者角色建立個別的宣告。

`CustomUserFactory.cs`:

```csharp
using System.Linq;
using System.Security.Claims;
using System.Text.Json;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;

public class CustomUserFactory
    : AccountClaimsPrincipalFactory<RemoteUserAccount>
{
    public CustomUserFactory(IAccessTokenProviderAccessor accessor)
        : base(accessor)
    {
    }

    public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
        RemoteUserAccount account,
        RemoteAuthenticationUserOptions options)
    {
        var user = await base.CreateUserAsync(account, options);

        if (user.Identity.IsAuthenticated)
        {
            var identity = (ClaimsIdentity)user.Identity;
            var roleClaims = identity.FindAll(identity.RoleClaimType);

            if (roleClaims != null && roleClaims.Any())
            {
                foreach (var existingClaim in roleClaims)
                {
                    identity.RemoveClaim(existingClaim);
                }

                var rolesElem = account.AdditionalProperties[identity.RoleClaimType];

                if (rolesElem is JsonElement roles)
                {
                    if (roles.ValueKind == JsonValueKind.Array)
                    {
                        foreach (var role in roles.EnumerateArray())
                        {
                            identity.AddClaim(new Claim(options.RoleClaim, role.GetString()));
                        }
                    }
                    else
                    {
                        identity.AddClaim(new Claim(options.RoleClaim, roles.GetString()));
                    }
                }
            }
        }

        return user;
    }
}
```

在用戶端應用程式中，于（）中註冊 factory `Program.Main` `Program.cs` ：

```csharp
builder.Services.AddApiAuthorization()
    .AddAccountClaimsPrincipalFactory<CustomUserFactory>();
```

在伺服器應用程式中，呼叫產生器 <xref:Microsoft.AspNetCore.Identity.IdentityBuilder.AddRoles*> 上的 Identity ，這會新增角色相關服務：

```csharp
using Microsoft.AspNetCore.Identity;

...

services.AddDefaultIdentity<ApplicationUser>(options => 
    options.SignIn.RequireConfirmedAccount = true)
    .AddRoles<IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

### <a name="configure-identity-server"></a>設定 Identity 伺服器

請使用下列**其中一**種方法：

* [API 授權選項](#api-authorization-options)
* [設定檔服務](#profile-service)

#### <a name="api-authorization-options"></a>API 授權選項

在伺服器應用程式中：

* 設定 Identity 伺服器將 `name` 和宣告放 `role` 入識別碼權杖和存取權杖中。
* 防止 JWT 權杖處理常式中的角色的預設對應。

```csharp
using System.IdentityModel.Tokens.Jwt;
using System.Linq;

...

services.AddIdentityServer()
    .AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options => {
        options.IdentityResources["openid"].UserClaims.Add("name");
        options.ApiResources.Single().UserClaims.Add("name");
        options.IdentityResources["openid"].UserClaims.Add("role");
        options.ApiResources.Single().UserClaims.Add("role");
    });

JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Remove("role");
```

#### <a name="profile-service"></a>設定檔服務

在伺服器應用程式中，建立一個 `ProfileService` 執行。

`ProfileService.cs`:

```csharp
using IdentityModel;
using IdentityServer4.Models;
using IdentityServer4.Services;
using System.Threading.Tasks;

public class ProfileService : IProfileService
{
    public ProfileService()
    {
    }

    public Task GetProfileDataAsync(ProfileDataRequestContext context)
    {
        var nameClaim = context.Subject.FindAll(JwtClaimTypes.Name);
        context.IssuedClaims.AddRange(nameClaim);

        var roleClaims = context.Subject.FindAll(JwtClaimTypes.Role);
        context.IssuedClaims.AddRange(roleClaims);

        return Task.CompletedTask;
    }

    public Task IsActiveAsync(IsActiveContext context)
    {
        return Task.CompletedTask;
    }
}
```

在伺服器應用程式中，在中註冊設定檔服務 `Startup.ConfigureServices` ：

```csharp
using IdentityServer4.Services;

...

services.AddTransient<IProfileService, ProfileService>();
```

### <a name="use-authorization-mechanisms"></a>使用授權機制

在用戶端應用程式中，元件授權方法會在此時運作。 元件中的任何授權機制都可以使用角色來授權使用者：

* [ `AuthorizeView` 元件](xref:blazor/security/index#authorizeview-component)（範例： `<AuthorizeView Roles="admin">` ）
* [ `[Authorize]` attribute](xref:blazor/security/index#authorize-attribute)指示詞（ <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ）（範例： `@attribute [Authorize(Roles = "admin")]` ）
* 程式[邏輯](xref:blazor/security/index#procedural-logic)（範例： `if (user.IsInRole("admin")) { ... }` ）

  支援多個角色測試：

  ```csharp
  if (user.IsInRole("admin") && user.IsInRole("developer"))
  {
      ...
  }
  ```

`User.Identity.Name`會在用戶端應用程式中填入使用者的使用者名稱，這通常是其登入電子郵件地址。

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* [部署至 Azure App Service](xref:security/authentication/identity/spa#deploy-to-production)
* [從 Key Vault 匯入憑證（Azure 檔）](/azure/app-service/configure-ssl-certificate#import-a-certificate-from-key-vault)
* <xref:blazor/security/webassembly/additional-scenarios>
* [在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
