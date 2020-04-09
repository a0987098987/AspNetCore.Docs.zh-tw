---
title: 使用認證庫保護BlazorASP.NET核心 Web 大會獨立應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: 893fff10df37e1c2be549604f4cb83cd20049108
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977037"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>使用認證庫保護BlazorASP.NET核心 Web 大會獨立應用

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

*對於 Azure 活動目錄 (AAD) 和 Azure 活動目錄 B2C (AAD B2C),不要遵循本主題中的指南。請參閱此目錄節點中的 AAD 和 AAD B2C 主題。*

要建立Blazor`Microsoft.AspNetCore.Components.WebAssembly.Authentication`使用函式庫的 WebAssembly 獨立應用,請在命令外殼中執行以下命令:

```dotnetcli
dotnet new blazorwasm -au Individual
```

要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample` 資料夾名稱也將成為專案名稱的一部分。

在視覺化工作室[中,建立BlazorWeb 組裝應用](xref:blazor/get-started)。 使用**應用程式商店使用者帳戶套用內**選項將**認證**設定為**單個使用者帳戶**。

## <a name="authentication-package"></a>驗證驗證

創建應用以使用個人使用者帳戶時,應用會自動在應用的專案檔中接收`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包的包引用。 該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。

如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。

## <a name="authentication-service-support"></a>認證服務支援

使用`AddOidcAuthentication``Microsoft.AspNetCore.Components.WebAssembly.Authentication`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。 此方法設置應用與標識提供者 (IP) 互動所需的所有服務。

*Program.cs*:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

使用開放 ID 連接 (OIDC) 為獨立應用提供身份驗證支援。 該方法`AddOidcAuthentication`接受回調以配置使用 OIDC 對應用進行身份驗證所需的參數。 配置應用所需的值可以從符合 OIDC 的 IP 獲得。 註冊應用時獲取值,這些值通常發生在其連線門戶中。

## <a name="access-token-scopes"></a>存取權杖範圍

WebAssemblyBlazor範本不會自動配置應用以請求安全 API 的訪問權杖。 要將權杖預先設定登入串流的一部分,請將作用網域`OidcProviderOptions`加入預設的權限的功能的功能的功能:

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
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
> options.ProviderOptions.DefaultScopes.Add(
>     "{API CLIENT ID OR CUSTOM VALUE}/{SCOPE NAME}");
> ```

如需詳細資訊，請參閱 <xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens>。

## <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

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
