---
title: Blazor WebAssembly使用驗證程式庫保護 ASP.NET Core 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/08/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 02960e6c7d70be3ea1be3ed9e2280e5b5847c926
ms.sourcegitcommit: f7873c02c1505c99106cbc708f37e18fc0a496d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147682"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-the-authentication-library"></a>Blazor WebAssembly使用驗證程式庫保護 ASP.NET Core 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

*若為 Azure Active Directory （AAD）和 Azure Active Directory B2C （AAD B2C），請不要遵循本主題中的指導方針。請參閱此目錄節點中的 AAD 和 AAD B2C 主題。*

若要建立 Blazor WebAssembly 使用程式庫的獨立應用程式 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) ，請遵循您選擇工具的指引。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

若要 Blazor WebAssembly 使用驗證機制建立新的專案：

1. 選擇 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中的** Blazor WebAssembly 應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更**]。

1. 使用 [**將使用者帳戶儲存在應用程式中**] 選項選取**個別的使用者帳戶**，以使用 ASP.NET Core 的系統將使用者儲存在應用程式中 [Identity](xref:security/authentication/identity) 。

# <a name="visual-studio-code--net-core-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

Blazor WebAssembly使用空白資料夾中的驗證機制來建立新的專案。 指定 `Individual` 驗證機制，並 `-au|--auth` 選擇使用 ASP.NET Core 的系統將使用者儲存在應用程式內 [Identity](xref:security/authentication/identity) ：

```dotnetcli
dotnet new blazorwasm -au Individual -o {APP NAME}
```

| 預留位置  | 範例        |
| ------------ | -------------- |
| `{APP NAME}` | `BlazorSample` |

使用選項指定的輸出位置會 `-o|--output` 建立專案資料夾（如果不存在），並成為應用程式名稱的一部分。

如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

若要 Blazor WebAssembly 使用驗證機制建立新的專案：

1. 在 [**設定新的 Blazor WebAssembly 應用程式**] 步驟上，從 [**驗證**] 下拉式選單選取 [**個別驗證（應用程式內）** ]。

1. 應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 [Identity](xref:security/authentication/identity) 。

---

## <a name="authentication-package"></a>驗證套件

建立應用程式以使用個別使用者帳戶時，應用程式會 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 在應用程式的專案檔中自動接收套件的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="3.2.0" />
```

## <a name="authentication-service-support"></a>驗證服務支援

使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 <xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A> [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 。 這個方法會設定應用程式與 Identity 提供者（IP）互動所需的服務。

`Program.cs`:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    builder.Configuration.Bind("Local", options.ProviderOptions);
});
```

設定是由檔案所提供 `wwwroot/appsettings.json` ：

```json
{
    "Local": {
        "Authority": "{AUTHORITY}",
        "ClientId": "{CLIENT ID}"
    }
}
```

獨立應用程式的驗證支援是使用 Open ID Connect （OIDC）提供。 <xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A>方法會接受回呼來設定使用 OIDC 驗證應用程式所需的參數。 設定應用程式所需的值可從符合 OIDC 規範的 IP 取得。 當您註冊應用程式時，請取得這些值，這通常會發生在其線上入口網站中。

## <a name="access-token-scopes"></a>存取權杖範圍

Blazor WebAssembly範本不會自動將應用程式設定為要求安全 API 的存取權杖。 若要在登入流程中布建存取權杖，請將範圍新增至的預設權杖範圍 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.OidcProviderOptions> ：

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

如需詳細資訊，請參閱*其他案例*文章的下列章節：

* [要求其他存取權杖](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加到連出要求](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>LoginDisplay 元件

`LoginDisplay`元件（ `Shared/LoginDisplay.razor` ）會在 `MainLayout` 元件（）中轉譯 `Shared/MainLayout.razor` ，並管理下列行為：

* 針對已驗證的使用者：
  * 顯示目前的使用者名稱。
  * 提供用來登出應用程式的按鈕。
* 若為匿名使用者，則提供登入的選項。

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
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

## <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:blazor/security/webassembly/additional-scenarios>
* [在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
