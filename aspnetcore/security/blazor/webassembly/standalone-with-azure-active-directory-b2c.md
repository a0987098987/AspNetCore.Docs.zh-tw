---
title: Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 911eeffbe632d8f285d54ae78b62f17851205e83
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84214957"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

若要建立 Blazor 使用[AZURE ACTIVE DIRECTORY （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證的 WebAssembly 獨立應用程式：

遵循下列主題中的指導方針，在 Azure 入口網站中建立租使用者並註冊 web 應用程式：

[建立 AAD B2C 租使用者](/azure/active-directory-b2c/tutorial-create-tenant)

記錄下列資訊：

* AAD B2C 實例（例如， `https://contoso.b2clogin.com/` 包含尾端斜線的）。
* AAD B2C 租使用者網域（例如 `contoso.onmicrosoft.com` ）。

請遵循教學課程[：再次在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*用戶端應用程式*註冊 AAD 應用程式，然後執行下列動作：

1. 在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。
1. 提供應用程式的**名稱**（例如， ** Blazor 獨立 AAD B2C**）。
1. 針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**
1. 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行之應用程式的預設埠是5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，應用程式的隨機產生埠可以在 [**調試**程式] 面板的 [屬性] 中找到。 由於應用程式目前不存在，且 IIS Express 埠未知，請在建立應用程式之後返回此步驟，並更新重新導向 URI。 本主題稍後會出現一個批註，提醒 IIS Express 使用者更新重新導向 URI。
1. 確認**許可權**  >  **授與系統管理員收到給 openid，並已啟用 offline_access 許可權**。
1. 選取 [註冊]。

記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111` ）。

在 [**驗證**  >  **平臺**設定]  >  **Web**：

1. 確認的重新**導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存]**** 按鈕。

在**Home**  >  **Azure AD B2C**  >  **使用者流程**：

[建立註冊和登入使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows)

至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` `LoginDisplay` 元件（*Shared/LoginDisplay*）中的。

記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin` ）。

以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{TENANT DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。 資料夾名稱也會成為專案名稱的一部分。

> [!NOTE]
> 在 Azure 入口網站中，應用程式的**驗證**  >  **平臺**  >  設定**Web**重新  >  **導向 URI**會針對使用預設設定在 Kestrel 伺服器上執行的應用程式進行通訊埠5001。
>
> 如果應用程式是在隨機 IIS Express 埠上執行，則可以在 [**調試**程式] 面板的 [屬性] 中找到應用程式的埠。
>
> 如果先前未使用應用程式的已知埠設定埠，請回到 Azure 入口網站中的應用程式註冊，並使用正確的埠更新重新導向 URI。

建立應用程式之後，您應該能夠：

* 使用 AAD 使用者帳戶登入應用程式。
* 要求 Microsoft Api 的存取權杖。 如需詳細資訊，請參閱：
  * [存取權杖範圍](#access-token-scopes)
  * [快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。

## <a name="authentication-package"></a>驗證套件

當您建立應用程式以使用個別 B2C 帳戶（ `IndividualB2C` ）時，應用程式會自動接收[microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（[WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

[WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)套件可轉移會將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)套件新增至應用程式中的。

## <a name="authentication-service-support"></a>驗證服務支援

驗證使用者的支援是在服務容器中註冊，並使用 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> [WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)套件所提供的擴充方法。 這個方法會設定應用程式與 Identity 提供者（IP）互動所需的所有服務。

*Program.cs*：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
});
```

<xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 AAD 設定取得設定應用程式所需的值。

Configuration 是由*wwwroot/appsettings*檔案所提供：

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT ID}",
    "ValidateAuthority": false
  }
}
```

範例：

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "ValidateAuthority": false
  }
}
```

## <a name="access-token-scopes"></a>存取權杖範圍

BlazorWebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。 若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍 <xref:Microsoft.Authentication.WebAssembly.Msal.Models.MsalProviderOptions> ：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

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
