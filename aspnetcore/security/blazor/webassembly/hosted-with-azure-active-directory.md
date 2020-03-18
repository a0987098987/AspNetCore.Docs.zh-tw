---
title: 使用 Azure Active Directory 保護 Blazor WebAssembly 託管應用程式的 ASP.NET Core
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 2ddbc9791ec9b31d55c9c6017d9d6d5be5c8dec8
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434482"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>使用 Azure Active Directory 保護 Blazor WebAssembly 託管應用程式的 ASP.NET Core

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



本文說明如何建立使用[Azure Active Directory （AAD）](https://azure.microsoft.com/services/active-directory/)進行驗證的[Blazor WebAssembly 託管應用程式](xref:blazor/hosting-models#blazor-webassembly)。

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>在 AAD B2C 中註冊應用程式並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

遵循[快速入門：設定租](/azure/active-directory/develop/quickstart-create-new-tenant)使用者中的指導方針，在 AAD 中建立租使用者。

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中，為*伺服器 API 應用*程式註冊 AAD 應用程式：

1. 選取 [新增註冊]。
1. 提供應用程式的**名稱**（例如， **Blazor Server AAD**）。
1. 選擇**支援的帳戶類型**。 在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。
1. 在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。
1. 停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。
1. 選取 [註冊]。

在 [ **API 許可權**] 中，移除**Microsoft Graph** > **使用者。讀取**許可權，因為應用程式不需要登入或 uer 設定檔存取權。

在中**公開 API**：

1. 選取 [新增範圍]。
1. 選取 [儲存並繼續]。
1. 提供**範圍名稱**（例如，`API.Access`）。
1. 提供系統**管理員同意顯示名稱**（例如，`Access API`）。
1. 提供系統**管理員同意描述**（例如，`Allows the app to access server app API endpoints.`）。
1. 確認 [**狀態**] 設定為 [**已啟用**]。
1. 選取 [**新增領域**]。

記錄下列資訊：

* *伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111`）
* 目錄識別碼（租使用者識別碼）（例如 `222222222-2222-2222-2222-222222222222`）
* AAD 租使用者網域（例如 `contoso.onmicrosoft.com`）
* 預設範圍（例如 `API.Access`）

### <a name="register-a-client-app"></a>註冊用戶端應用程式

請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中註冊*用戶端應用*程式的 AAD 應用程式：

1. 選取 [新增註冊]。
1. 提供應用程式的**名稱**（例如， **Blazor 用戶端 AAD**）。
1. 選擇**支援的帳戶類型**。 在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。
1. 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供 `https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。
1. 停用**許可權** > **將系統管理員收到授與 openid 並 offline_access 許可權** 核取方塊。
1. 選取 [註冊]。

在 [**驗證** > **平臺**設定 > **Web**]：

1. 確認 `https://localhost:5001/authentication/login-callback` 的重新**導向 URI**存在。
1. 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存] 按鈕。

在 [ **API 許可權**] 中：

1. 確認應用程式已**Microsoft Graph** > **使用者。讀取**許可權。
1. 選取 [**新增許可權**]，後面接著 [**我的 api**]。
1. 從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如， **Blazor Server AAD**）。
1. 開啟 [ **API**清單]。
1. 啟用 API 的存取權（例如，`API.Access`）。
1. 選取 [新增權限]。
1. 選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。 選取 [是] 加以確認。

記錄*用戶端應用*程式識別碼（用戶端識別碼）（例如 `33333333-3333-3333-3333-333333333333`）。

### <a name="create-the-app"></a>建立應用程式

以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP CLIENT ID}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。 資料夾名稱也會成為專案名稱的一部分。

> [!NOTE]
> 如需預設存取權杖範圍的重要設定變更，請參閱[驗證服務支援](#Authentication service support)一節。 從範本建立*用戶端應用程式*之後，必須手動變更 Blazor WebAssembly 範本所提供的值。

## <a name="server-app-configuration"></a>伺服器應用程式設定

*本節適用于解決方案的**伺服器**應用程式。*

### <a name="authentication-package"></a>驗證套件

`Microsoft.AspNetCore.Authentication.AzureAD.UI`提供驗證和授權呼叫 ASP.NET Core Web Api 的支援：

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>驗證服務支援

`AddAuthentication` 方法會在應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。 `AddAzureADBearer` 方法會設定 JWT 持有人處理常式中的特定參數，以驗證 Azure Active Directory 所發出的權杖：

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication` 和 `UseAuthorization` 確保：

* 應用程式會嘗試剖析並驗證傳入要求的權杖。
* 嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="app-settings"></a>應用程式設定

*Appsettings*包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{API CLIENT ID}",
  }
}
```

### <a name="weatherforecast-controller"></a>WeatherForecast 控制器

WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 `[Authorize]` 屬性套用至控制器。 請**務必**瞭解：

* 此 API 控制器中的 `[Authorize]` 屬性是保護此 API 免于未經授權存取的唯一做法。
* Blazor WebAssembly 應用程式中使用的 `[Authorize]` 屬性僅做為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>用戶端應用程式設定

*本節適用于解決方案的**用戶端**應用程式。*

### <a name="authentication-package"></a>驗證套件

當您建立應用程式以使用工作或學校帳戶（`SingleOrg`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。

`Microsoft.Authentication.WebAssembly.Msal` 套件可傳遞將 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件新增至應用程式。

### <a name="authentication-service-support"></a>驗證服務支援

驗證使用者的支援是以 `Microsoft.Authentication.WebAssembly.Msal` 套件所提供的 `AddMsalAuthentication` 擴充方法，在服務容器中註冊。 這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。

*Program.cs*：

產生*用戶端應用程式*時，預設存取權杖範圍的格式為 `api://{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}`。 **移除範圍值的 `api://` 部分。** 此問題將在未來的預覽版本中解決。

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}");
});
```

> [!NOTE]
> 預設存取權杖範圍的格式必須是 `{SERVER API APP CLIENT ID}/{DEFAULT SCOPE}` （例如，`11111111-1111-1111-1111-111111111111/API.Access`）。 如果將配置或配置和主機提供給範圍設定（如 Azure 入口網站中所示），則*用戶端應用程式*會在收到來自*伺服器 API 應用程式*的*401 未經授權*回應時，擲回未處理的例外狀況。

`AddMsalAuthentication` 方法會接受回呼，以設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

預設存取權杖範圍代表存取權杖範圍的清單，如下所示：

* 預設包含在登入要求中。
* 用來在驗證之後立即布建存取權杖。

所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。 您可以視需要為其他 API 應用程式新增額外的範圍：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add(
        "{SERVER API APP CLIENT ID}/{SCOPE}");
});
```

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

### <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay 元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>FetchData 元件

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>執行應用程式

從伺服器專案執行應用程式。 使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/authentication/azure-active-directory/index>
