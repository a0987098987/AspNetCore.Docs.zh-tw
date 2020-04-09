---
title: 使用 MicrosoftBlazor帳戶保護ASP.NET核心 Web 大會獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 8c409651b3338c2baeae497bef43b994823a20f9
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977076"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a>使用 MicrosoftBlazor帳戶保護ASP.NET核心 Web 大會獨立應用

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

要建立使用Blazor[具有 Azure 活動目錄 (AAD)](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)的 Microsoft 帳戶進行身份驗證的 WebAssembly 獨立應用,請執行:

1. [建立 AAD 租戶與 Web 應用程式](/azure/active-directory/develop/v2-overview)

   在 Azure 門戶的**Azure 活動目錄** > **應用註冊區域註冊**AAD 應用:

   1\. 為應用提供**名稱**(例如,**Blazor用戶端 AAD**)。<br>
   2\. 在**支援帳號型態**型**態中,選擇任何組織目錄中的帳戶**。<br>
   3\. 將**重定向 URI**下拉清單設定為**Web,** 並提供`https://localhost:5001/authentication/login-callback`重定向 URI。<br>
   4\. 禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。<br>
   5\. 選取 [註冊]  。

   在**認證** > **平台設定中** > **, Web**:

   1\. 確認存在 重定`https://localhost:5001/authentication/login-callback`向**URI。**<br>
   2\. 對於**隱式授予**,選擇 Access**權杖**和**ID 權杖的**複選框。<br>
   3\. 此體驗可以接受應用的剩餘預設值。<br>
   4\. 選取 [儲存]**** 按鈕。

   記錄應用程式 ID(用戶端 ID)(`11111111-1111-1111-1111-111111111111`例如,

1. 將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample` 資料夾名稱也將成為專案名稱的一部分。

建立應用程式後,您應該能夠:

* 使用 Microsoft 帳戶登錄應用。
* 使用與獨立Blazor應用相同的方法請求 Microsoft API 的訪問權杖,前提是您已正確配置應用。 有關詳細資訊,請參閱[快速入門:設定應用程式以公開 Web API](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。

## <a name="authentication-package"></a>驗證驗證

建立應用程式以使用工作帳號或學校帳戶 ()`SingleOrg`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。 該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。

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
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。 註冊應用時,可以從 Microsoft 帳戶配置中獲取配置應用所需的值。

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
* [快速入門:使用 Microsoft 識別平台註冊應用程式](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [快速入門:設定應用程式以公開 Web API](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
