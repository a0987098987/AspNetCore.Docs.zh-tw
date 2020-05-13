---
title: Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 059947888653c05a062ec5e5849d8087cd01f475
ms.sourcegitcommit: 1250c90c8d87c2513532be5683640b65bfdf9ddb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83153603"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

若要建立 Blazor 使用[AZURE ACTIVE DIRECTORY （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 WebAssembly 獨立應用程式：

1. 遵循下列主題中的指導方針，在 Azure 入口網站中建立租使用者並註冊 web 應用程式：

   * [建立 AAD B2C 租](/azure/active-directory-b2c/tutorial-create-tenant) &ndash; 使用者記錄下列資訊：

     1 \。 AAD B2C 實例（例如， `https://contoso.b2clogin.com/` 包含尾端斜線的）<br>
     2 \。 AAD B2C 租使用者網域（例如， `contoso.onmicrosoft.com` ）

   * [註冊 web 應用程式](/azure/active-directory-b2c/tutorial-register-applications) &ndash;在應用程式註冊期間進行下列選擇：

     1 \。 將 [ **Web 應用程式/WEB API** ] 設定為 **[是]**。<br>
     2 \。 將 [**允許隱含流程**] 設為 **[是]**。<br>
     3 \。 新增的 [**回復 URL** ] `https://localhost:5001/authentication/login-callback` 。

     記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111` ）。

   * [建立使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows) &ndash;建立註冊和登入使用者流程。

     至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` `LoginDisplay` 元件（*Shared/LoginDisplay*）中的。

     記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin` ）。

1. 以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。 資料夾名稱也會成為專案名稱的一部分。

## <a name="authentication-package"></a>驗證套件

建立應用程式以使用個別 B2C 帳戶（ `IndividualB2C` ）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（）的套件參考 `Microsoft.Authentication.WebAssembly.Msal` 。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

`{VERSION}`將前述套件參考中的取代為發行 `Microsoft.AspNetCore.Blazor.Templates` 項中所顯示的套件版本 <xref:blazor/get-started> 。

`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。

## <a name="authentication-service-support"></a>驗證服務支援

使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 `AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal` 。 這個方法會設定應用程式與 Identity 提供者（IP）互動所需的所有服務。

*Program.cs*：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
});
```

`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。

Configuration 是由*wwwroot/appsettings*檔案所提供：

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

範例：

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd"
  }
}
```

## <a name="access-token-scopes"></a>存取權杖範圍

BlazorWebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。 若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍 `MsalProviderOptions` ：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> 如果 Azure 入口網站提供範圍 URI，且應用程式在收到來自 API 的*401 未經授權*回應時擲回**未處理的例外**狀況，請嘗試使用不包含配置和主機的範圍 uri。 例如，Azure 入口網站可能會提供下列其中一個範圍 URI 格式：
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> 提供不含配置和主機的範圍 URI：
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

如需詳細資訊，請參閱*其他案例*文章的下列章節：

* [要求其他存取權杖](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加到連出要求](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

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

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/webassembly/additional-scenarios>
* [在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/authentication/azure-ad-b2c>
* [教學課程：建立 Azure Active Directory B2C 租用戶](/azure/active-directory-b2c/tutorial-create-tenant)
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
