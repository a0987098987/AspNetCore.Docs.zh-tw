---
title: 使用 AzureBlazor活動目錄 B2C 保護 ASP.NET核心 Web 元件獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-azure-active-directory-b2c
ms.openlocfilehash: 0734bad2d4281eb856783a362ef8c608a303c17a
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977050"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-azure-active-directory-b2c"></a>使用 AzureBlazor活動目錄 B2C 保護 ASP.NET核心 Web 元件獨立應用

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

要建立Blazor[Azure 活動目錄 (AAD) B2C](/azure/active-directory-b2c/overview)進行身份驗證的 Web 大會獨立應用,請執行:

1. 按照以下主題中的指南創建租戶並在 Azure 門戶中註冊 Web 應用:

   * [建立 AAD B2C 租戶](/azure/active-directory-b2c/tutorial-create-tenant)&ndash;記錄以下資訊:

     1\. AAD B2C 實體(`https://contoso.b2clogin.com/`例如 ,包括尾隨斜杠 )<br>
     2\. AAD B2C 租戶域`contoso.onmicrosoft.com`(例如 ,

   * [註冊 Web 應用程式](/azure/active-directory-b2c/tutorial-register-applications)&ndash;在應用程式註冊期間進行以下選擇:

     1\. 將**Web 應用/Web API**設定為 **"是**"。<br>
     2\. 將**允許隱式流**設置為 **"是**"。<br>
     3\. 添加`https://localhost:5001/authentication/login-callback`的**回覆 URL。**

     記錄應用程式 ID(用戶端 ID)(`11111111-1111-1111-1111-111111111111`例如,

   * [創建用戶流](/azure/active-directory-b2c/tutorial-create-user-flows)&ndash;創建註冊和登錄使用者流。

     至少,選擇**應用程式聲明** > **顯示名稱**使用者屬性以`context.User.Identity.Name``LoginDisplay`填充 元件中的 (*共用/ LoginDisplay.razor)。*

     記錄為應用創建的註冊和登錄使用者流名稱(例如。 `B2C_1_signupsignin`

1. 將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:

   ```dotnetcli
   dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --client-id "{CLIENT ID}" --domain "{DOMAIN}" -ssp "{SIGN UP OR SIGN IN POLICY}"
   ```

   要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample` 資料夾名稱也將成為專案名稱的一部分。

## <a name="authentication-package"></a>驗證驗證

建立使用單個 B2C 帳號 ()`IndividualB2C`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。 該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。

如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。

包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。

## <a name="authentication-service-support"></a>認證服務支援

使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。 此方法設置應用與標識提供者 (IP) 互動所需的所有服務。

*Program.cs*:

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

該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。 註冊應用時,可以從 Azure 門戶 AAD 配置中獲取配置應用所需的值。

## <a name="access-token-scopes"></a>存取權杖範圍

WebAssemblyBlazor範本不會自動配置應用以請求安全 API 的訪問權杖。 要將權杖預先設定登入串流的一部分,請將作用網域`MsalProviderOptions`加入預設存取權杖的作用區中:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> 如果 Azure 門戶提供作用域 URI,並且應用在收到來自 API 的*401 未授權*回應時**引發未處理的異常**,請嘗試使用不包括方案和主機的範圍 URI。 例如,Azure 門戶可以提供以下作用域 URI 格式之一:
>
> * `https://{ORGANIZATION}.onmicrosoft.com/{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
> * `api://{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}`
>
> 提供無方案和主機的範圍 URI:
>
> ```csharp
> options.ProviderOptions.DefaultAccessTokenScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。

## <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a>套用元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>重定到登入元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>登入元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* [要求其他存取權杖](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-ad-b2c>
* [教學課程：建立 Azure Active Directory B2C 租用戶](/azure/active-directory-b2c/tutorial-create-tenant)
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
