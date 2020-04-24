---
title: 使用 Microsoft 帳戶Blazor保護 ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 58fd7e6caaffa4c2a293cdddef80def66b8e0680
ms.sourcegitcommit: 4f91da9ce4543b39dba5e8920a9500d3ce959746
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82138570"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a>使用 Microsoft 帳戶Blazor保護 ASP.NET Core WebAssembly 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

若要建立Blazor WebAssembly 獨立應用程式，以使用[具有 Azure Active Directory （AAD）的 Microsoft 帳戶](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)進行驗證：

1. [建立 AAD 租使用者和 web 應用程式](/azure/active-directory/develop/v2-overview)

   在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**] 區域中註冊 AAD 應用程式：

   1 \。 提供應用程式的**名稱**（例如** Blazor用戶端 AAD**）。<br>
   2 \。 在 [**支援的帳戶類型**] 中，選取 [**任何組織目錄中的帳戶**]。<br>
   3 \。 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供`https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。<br>
   4 \。 停用 [授與系統**管理員收到給 openid 和 offline_access 許可權**] 核取方塊的**許可權** > 。<br>
   5 \。 選取 [註冊]  。

   在 [**驗證** > **平臺** > 設定]**Web**：

   1 \。 確認的重新**導向 URI** `https://localhost:5001/authentication/login-callback`存在。<br>
   2 \。 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。<br>
   3 \。 此體驗可接受應用程式的其餘預設值。<br>
   4 \。 選取 [儲存]**** 按鈕。

   記錄應用程式識別碼（用戶端識別碼）（例如`11111111-1111-1111-1111-111111111111`）。

1. 以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。 資料夾名稱也會成為專案名稱的一部分。

建立應用程式之後，您應該能夠：

* 使用 Microsoft 帳戶登入應用程式。
* 如果您已正確設定應用程式，請使用與獨立Blazor應用程式相同的方法，來要求 Microsoft api 的存取權杖。 如需詳細資訊，請參閱[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。

## <a name="authentication-package"></a>驗證套件

建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。

`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件新增至應用程式。

## <a name="authentication-service-support"></a>驗證服務支援

使用`AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal`封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援。 這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。

*Program.cs*：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
});
```

`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。

Configuration 是由*wwwroot/appsettings*檔案所提供：

```json
{
    "AzureAd": {
        "Authority": "https://login.microsoftonline.com/common",
        "ClientId": "{CLIENT ID}"
    }
}
```

範例：

```json
{
    "AzureAd": {
        "Authority": "https://login.microsoftonline.com/common",
        "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd"
    }
}
```

## <a name="access-token-scopes"></a>存取權杖範圍

Blazor WebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。 若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍`MsalProviderOptions`：

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

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/webassembly/additional-scenarios>
* [快速入門：使用 Microsoft 身分識別平台來註冊應用程式](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [快速入門：設定應用程式以公開 web Api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
