---
title: 使用 Azure Active Directory B2C 保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: bb03ef1e6d216cfc06e2b91919c64f92f2ef634e
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219268"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>使用 Azure Active Directory B2C 保護 ASP.NET Core Blazor WebAssembly 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

若要建立使用[Azure Active Directory （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 Blazor WebAssembly 獨立應用程式：

1. 遵循下列主題中的指導方針，在 Azure 入口網站中建立租使用者並註冊 web 應用程式：

   * [建立 AAD B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者 &ndash; 記錄下列資訊：

     1\. AAD B2C 實例（例如，`https://contoso.b2clogin.com/`，其中包含尾端斜線）<br>
     2\. AAD B2C 租使用者網域（例如，`contoso.onmicrosoft.com`）

   * [註冊 web 應用程式](/azure/active-directory-b2c/tutorial-register-applications)&ndash; 在應用程式註冊期間進行下列選擇：

     1\. 將 [ **Web 應用程式/WEB API** ] 設定為 **[是]** 。<br>
     2\. 將 [**允許隱含流程**] 設為 **[是]** 。<br>
     3\. 新增 `https://localhost:5001/authentication/login-callback`的 [**回復 URL** ]。

     記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）。

   * [建立使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows)&ndash; 建立註冊和登入使用者流程。

     至少選取 **應用程式宣告** > **顯示名稱**使用者屬性，以在 `LoginDisplay` 元件（*Shared/LoginDisplay*）中填入 `context.User.Identity.Name`。

     記錄為應用程式建立的註冊和登入使用者流程名稱（例如，`B2C_1_signupsignin`）。

1. 以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。 資料夾名稱也會成為專案名稱的一部分。

## <a name="authentication-package"></a>驗證套件

當您建立應用程式以使用個別 B2C 帳戶（`IndividualB2C`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。

`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。

## <a name="authentication-service-support"></a>驗證服務支援

驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。 這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。

*Program.cs*：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = 
        "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}";
    authentication.ClientId = "{CLIENT ID}";
    authentication.ValidateAuthority = false;
});
```

`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。 若要布建權杖做為登入流程的一部分，請將範圍新增至 `MsalProviderOptions`的預設存取權杖範圍：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{API ID URI}/{SCOPE}");
});
```

## <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>LoginDisplay 元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/authentication/azure-ad-b2c>
* [教學課程：建立 Azure Active Directory B2C 租用戶](/azure/active-directory-b2c/tutorial-create-tenant)
