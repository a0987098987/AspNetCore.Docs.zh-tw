---
title: 使用識別伺服器保護BlazorASP.NET核心 Web 元件託管應用
author: guardrex
description: 使用[識別伺服器](https://identityserver.io/)Blazor後端介面的 Visual Studio 中使用身份驗證建立新託管應用
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: 4c51200159ced16132e15bb4a1f0915ca0cf5945
ms.sourcegitcommit: c9d1208e86160615b2d914cce74a839ae41297a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "81791622"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a>使用識別伺服器保護BlazorASP.NET核心 Web 元件託管應用

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

要在 Visual Blazor Studio 中創建新託管應用,使用[識別伺服器](https://identityserver.io/)對使用者和 API 呼叫進行身份驗證,

1. 使用可視化工作室創建新的**BlazorWeb 組裝**應用。 如需詳細資訊，請參閱 <xref:blazor/get-started>。
1. 在「**創建新Blazor應用」** 對話框中,在 **「身份驗證**」部分中選擇 **「更改**」。
1. 選擇**單個使用者帳戶**後跟 **「確定**」。
1. 在 **「進階」** 部分選擇 **「ASP.NET 核心託管**複選框。
1. 選取 [建立]**** 按鈕。

要在命令外殼中建立應用,請執行以下命令:

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample` 資料夾名稱也將成為專案名稱的一部分。

## <a name="server-app-configuration"></a>伺服器應用設定

以下各節介紹在包含身份驗證支援時對項目的補充。

### <a name="startup-class"></a>啟始類別

這個`Startup`類別的功能:

* 在 `Startup.ConfigureServices` 中：

  * 身分識別：

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * 識別伺服器具有附加<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>幫助器方法,在標識伺服器頂部設置一些預設ASP.NET核心約定:

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * 使用其他<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>說明程式方法進行身份驗證,該方法將應用配置為驗證 IdentityServer 產生的 JWT 權杖:

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* 在 `Startup.Configure` 中：

  * 負責驗證要求認證並在請求內容中設定使用者的身份驗證中間件:

    ```csharp
    app.UseAuthentication();
    ```

  * 公開開啟 ID 連線 (OIDC) 終結點的識別伺服器的中間件:

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>新增 Api 授權

<xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>說明器方法為ASP.NET核心方案設定[識別伺服器](https://identityserver.io/)。 IdentityServer 是一個功能強大且可擴展的框架,用於處理應用安全問題。 標識伺服器會為最常見的方案公開不必要的複雜性。 因此,提供了一組約定和配置選項,我們認為這是一個良好的起點。 一旦身份驗證需要更改,身份伺服器的全部功能仍可用於自定義身份驗證以滿足應用的要求。

### <a name="addidentityserverjwt"></a>新增身份伺服器Jwt

<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>説明程式方法將應用的策略方案配置為預設身份驗證處理程式。 該策略設定為允許標識處理路由到標識 URL`/Identity`空間中的任何子路徑的所有請求。 處理<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>所有其他請求。 此外,此方法:

* 將`{APPLICATION NAME}API`API 資源註冊為預設作用`{APPLICATION NAME}API`域 的識別伺服器。
* 配置 JWT 承載權杖中間件以驗證 IdentityServer 為應用頒發的權杖。

### <a name="weatherforecastcontroller"></a>天氣預報控制器

在`WeatherForecastController`(*控制器/天氣預報控制器.cs*)[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)中,該屬性應用於類。 該屬性指示必須根據預設策略授權使用者才能訪問資源。 默認授權策略配置為使用預設身份驗證方案,該方案由<xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>前面提到的策略方案設置。 説明程式方法<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler>配置為對應用的請求的默認處理程式。

### <a name="applicationdbcontext"></a>應用程式資料庫上下文

在`ApplicationDbContext`(*資料/ 應用程式 DbContext.cs)* 中<xref:Microsoft.EntityFrameworkCore.DbContext>,識別中<xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601>使用相同的,但擴展 為包括識別伺服器的架構除外。 <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> 衍生自 <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>。

要完全控制資料庫架構,請從其中一個可用的標識<xref:Microsoft.EntityFrameworkCore.DbContext>類繼承,並通過調`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)``OnModelCreating`用 方法將上下文配置為包括標識架構。

### <a name="oidcconfigurationcontroller"></a>Oidc 設定控制器

在`OidcConfigurationController`(*控制器/Oidc配置控制器.cs)* 中,用戶端終結點被預配以服務 OIDC 參數。

### <a name="app-settings-files"></a>套用設定檔

在專案根部的應用設定檔 (*appsettings.json*)`IdentityServer`中,該部分描述已配置的用戶端的清單。 在下面的示例中,只有一個用戶端。 用戶端名稱對應於應用名稱,並通過約定映射到 OAuth`ClientId`參數。 設定檔指示正在配置的應用類型。 配置檔在內部用於驅動簡化伺服器配置過程的約定。 <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

在開發環境應用設定檔中(*應用設定)。在專案根目錄的開發.json*`IdentityServer`中 ,該部分描述了用於對權杖進行簽名的鍵。 <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a>用戶端應用設定

### <a name="authentication-package"></a>驗證驗證

創建應用以使用個人使用者帳戶 ()`Individual`時,應用會自動在應用的專案檔中`Microsoft.AspNetCore.Components.WebAssembly.Authentication`接收 包的包引用。 該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。

如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。

### <a name="api-authorization-support"></a>API 授權支援

通過`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包內提供的擴充方法將使用者身份驗證支援插入服務容器。 此方法設置應用與現有授權系統交互所需的所有服務。

```csharp
builder.Services.AddApiAuthorization();
```

默認情況下,它按約定從`_configuration/{client-id}`載入應用的配置。 按照慣例,客戶端 ID 設置為應用的程式集名稱。 可以通過使用選項調用重載來更改此 URL 以指向單獨的終結點。

### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a>套用元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>重定到登入元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>登入元件

元件`LoginDisplay`(*共用 / LoginDisplay.razor*) 呈現在`MainLayout`元件中 ( 共用 /*MainLayout.razor*) 並管理以下行為:

* 對於經過身份驗證的使用者:
  * 顯示當前使用者名稱。
  * 提供指向ASP.NET核心標識中的使用者配置檔頁的連結。
  * 提供一個按鈕以註銷應用程式。
* 對匿名使用者:
  * 提供註冊選項。
  * 提供登錄選項。

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

### <a name="fetchdata-component"></a>擷取資料元件

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>執行應用程式

從「伺服器」專案運行應用。 使用 Visual Studio 時,在**解決方案資源管理器**中選擇「伺服器」專案,然後選擇工具列中的 **「運行」** 按鈕,或從 **「調試」** 選單啟動應用。

[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* [要求其他存取權杖](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
