---
title: 使用Identity伺服器保護Blazor ASP.NET Core WebAssembly 託管應用程式
author: guardrex
description: 從使用 IdentityServer 後Blazor端的 Visual Studio 中，建立具有驗證的新[IdentityServer](https://identityserver.io/)託管應用程式
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: bf2298618e922df412e0742177afd390c4116388
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768115"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-identity-server"></a>使用Identity伺服器保護Blazor ASP.NET Core WebAssembly 託管應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

若要在 Visual Studio Blazor中建立新的託管應用程式，以使用[IdentityServer](https://identityserver.io/)來驗證使用者和 API 呼叫：

1. 使用 Visual Studio 建立新** Blazor的 WebAssembly**應用程式。 如需詳細資訊，請參閱<xref:blazor/get-started>。
1. 在 [**建立新Blazor的應用程式**] 對話方塊中，選取 [**驗證**] 區段中的 [**變更**]。
1. 選取 [**個別使用者帳戶**]，後面接著 **[確定]**。
1. 選取 [ **Advanced** ] 區段中的 [ **ASP.NET Core 託管**] 核取方塊。
1. 選取 [建立]**** 按鈕。

若要在命令 shell 中建立應用程式，請執行下列命令：

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。 資料夾名稱也會成為專案名稱的一部分。

## <a name="server-app-configuration"></a>伺服器應用程式設定

下列各節說明當包含驗證支援時，專案的新增功能。

### <a name="startup-class"></a>啟始類別

`Startup`類別具有下列新增專案：

* 在 `Startup.ConfigureServices` 中：

  * Identity:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>(options => 
            options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer 使用額外<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>的協助程式方法，在 IdentityServer 上設定一些預設的 ASP.NET Core 慣例：

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * 使用其他<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper 方法進行驗證，以設定應用程式來驗證 IdentityServer 所產生的 JWT 權杖：

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* 在 `Startup.Configure` 中：

  * 驗證中介軟體會負責驗證要求認證，並在要求內容上設定使用者：

    ```csharp
    app.UseAuthentication();
    ```

  * 公開 Open ID Connect （OIDC）端點的 IdentityServer 中介軟體：

    ```csharp
    app.UseIdentityServer();
    ```

  * 驗證和授權中介軟體：

    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> Helper 方法會設定 ASP.NET Core 案例的[IdentityServer](https://identityserver.io/) 。 IdentityServer 是功能強大且可擴充的架構，可處理應用程式的安全性考慮。 IdentityServer 會在最常見的案例中公開不必要的複雜性。 因此，會提供一組慣例和設定選項，讓我們考慮一個良好的起點。 一旦您的驗證需要變更，IdentityServer 的完整功能仍然可以自訂驗證以符合應用程式的需求。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> Helper 方法會將應用程式的原則配置設定為預設驗證處理常式。 此原則設定為允許Identity處理路由至Identity URL 空間`/Identity`中任何子路徑的所有要求。 會<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>處理所有其他要求。 此外，這個方法也會：

* 向 IdentityServer `{APPLICATION NAME}API`註冊具有預設範圍的`{APPLICATION NAME}API`API 資源。
* 設定 JWT 持有人權杖中介軟體，以驗證 IdentityServer 針對應用程式所簽發的權杖。

### <a name="weatherforecastcontroller"></a>WeatherForecastController

在`WeatherForecastController` （*控制器/WeatherForecastController*）中， [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性會套用至類別。 屬性會指出使用者必須根據預設原則來存取資源。 預設的授權原則會設定為使用預設的驗證配置，這是由<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>設定為先前所述的原則配置。 Helper 方法會將<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>設定為應用程式要求的預設處理常式。

### <a name="applicationdbcontext"></a>[ApplicationdbcoNtext]

`ApplicationDbContext`在（*Data/[applicationdbcoNtext]*）中，會使用<xref:Microsoft.EntityFrameworkCore.DbContext>相同的， Identity因為它會擴充<xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601>以包含 IdentityServer 的架構。 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。

若要取得資料庫架構的完整控制權，請從Identity <xref:Microsoft.EntityFrameworkCore.DbContext>其中一個可用的類別繼承，並透過在Identity `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` `OnModelCreating`方法中呼叫來設定內容以包含架構。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

在`OidcConfigurationController` （*控制器/OidcConfigurationController*）中，會布建用戶端端點來提供 OIDC 參數。

### <a name="app-settings-files"></a>應用程式佈建檔案

在專案根目錄的應用程式佈建檔案（*appsettings*）中， `IdentityServer`區段會描述已設定的用戶端清單。 在下列範例中，有一個用戶端。 用戶端名稱會對應至應用程式名稱，並依照慣例對應至 OAuth `ClientId`參數。 此設定檔會指出正在設定的應用程式類型。 此設定檔會在內部使用，以驅動可簡化伺服器設定程式的慣例。 <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "{APP ASSEMBLY}.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

## <a name="client-app-configuration"></a>用戶端應用程式設定

### <a name="authentication-package"></a>驗證套件

建立應用程式以使用個別使用者帳戶（`Individual`）時，應用程式會在應用程式的專案檔中`Microsoft.AspNetCore.Components.WebAssembly.Authentication`自動接收套件的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。

### <a name="api-authorization-support"></a>API 授權支援

驗證使用者的支援是由`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件內提供的擴充方法插入服務容器中。 這個方法會設定應用程式與現有授權系統互動所需的所有服務。

```csharp
builder.Services.AddApiAuthorization();
```

根據預設，它會從`_configuration/{client-id}`依照慣例載入應用程式的設定。 依照慣例，用戶端識別碼會設定為應用程式的元件名稱。 您可以使用選項呼叫多載，將此 URL 變更為指向不同的端點。

### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay 元件

元件（*shared/LoginDisplay* `MainLayout` ）會在元件（*shared/MainLayout*）中轉譯，並管理下列行為： `LoginDisplay`

* 針對已驗證的使用者：
  * 顯示目前的使用者名稱。
  * 提供 ASP.NET Core Identity中 [使用者設定檔] 頁面的連結。
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

從伺服器專案執行應用程式。 使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/webassembly/additional-scenarios>
