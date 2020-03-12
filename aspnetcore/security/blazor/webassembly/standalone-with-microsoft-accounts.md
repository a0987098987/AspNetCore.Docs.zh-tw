---
title: 使用 Microsoft 帳戶保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-microsoft-accounts
ms.openlocfilehash: 6883af3486256e7c6905626d8da09e8ae0c4a896
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083822"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-microsoft-accounts"></a>使用 Microsoft 帳戶保護 ASP.NET Core Blazor WebAssembly 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

若要建立 Blazor WebAssembly 獨立應用程式，以使用[具有 Azure Active Directory （AAD）的 Microsoft 帳戶](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)進行驗證：

1. [建立 AAD 租使用者和 web 應用程式](/azure/active-directory/develop/v2-overview)

   在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中註冊 AAD 應用程式：

   1\. 提供應用程式的**名稱**（例如， **Blazor 用戶端 AAD**）。<br>
   2\. 在 [**支援的帳戶類型**] 中，選取 [**任何組織目錄中的帳戶**]。<br>
   3\. 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供 `https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。<br>
   4\. 停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。<br>
   5\. 選取 [註冊]。

   在 [**驗證** > **平臺**設定 > **Web**]：

   1\. 確認 `https://localhost:5001/authentication/login-callback` 的重新**導向 URI**存在。<br>
   2\. 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。<br>
   3\. 此體驗可接受應用程式的其餘預設值。<br>
   4\. 選取 [儲存] 按鈕。

   記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）。

1. 以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

   ```dotnetcli
   dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" --tenant-id "common"
   ```

   若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。 資料夾名稱也會成為專案名稱的一部分。

建立應用程式之後，您應該能夠：

* 使用 Microsoft 帳戶登入應用程式。
* 如果您已正確設定應用程式，請使用與獨立 Blazor 應用程式相同的方法，來要求 Microsoft Api 的存取權杖。 如需詳細資訊，請參閱[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。

## <a name="authentication-package"></a>驗證套件

當您建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

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
    authentication.Authority = "{AUTHORITY}";
    authentication.ClientId = "{CLIENT ID}";
});
```

`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Microsoft 帳戶設定取得設定應用程式所需的值。

## <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

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

* [快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app#register-a-new-application-using-the-azure-portal)
* [快速入門：設定應用程式以公開 web Api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)
