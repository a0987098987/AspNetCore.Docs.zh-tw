---
title: Blazor WebAssembly使用 Azure Active Directory 保護 ASP.NET Core 託管應用程式
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
uid: blazor/security/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: d2732819dd8f18da1f99965bb91e5eb3670ff4db
ms.sourcegitcommit: 384833762c614851db653b841cc09fbc944da463
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86445186"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-azure-active-directory"></a>Blazor WebAssembly使用 Azure Active Directory 保護 ASP.NET Core 託管應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

本文說明如何建立使用[Azure Active Directory （AAD）](https://azure.microsoft.com/services/active-directory/)進行驗證的[ Blazor WebAssembly 託管應用程式](xref:blazor/hosting-models#blazor-webassembly)。

## <a name="register-apps-in-aad-and-create-solution"></a>在 AAD 中註冊應用程式並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

遵循[快速入門：設定租](/azure/active-directory/develop/quickstart-create-new-tenant)使用者中的指導方針，在 AAD 中建立租使用者。

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指引，為*伺服器 API 應用*程式註冊 AAD 應用程式，然後執行下列動作：

1. 在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。
1. 提供應用程式的**名稱**（例如** Blazor Server AAD**）。
1. 選擇**支援的帳戶類型**。 在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。
1. 在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。
1. 停用 **[授與系統**  >  **管理員同意 openid 和 offline_access 許可權**] 核取方塊。
1. 選取 [註冊]。

記錄下列資訊：

* *伺服器 API 應用程式*應用程式（用戶端）識別碼（例如， `41451fa7-82d9-4673-8fa5-69eff5a761fd` ）
* 目錄（租使用者）識別碼（例如， `e86c78e2-8bb4-4c41-aefd-918e0565a45e` ）
* AAD 主要/發行者/租使用者網域（例如， `contoso.onmicrosoft.com` ）：在已註冊的應用程式之 Azure 入口網站的 [**商標**] 分頁中，此網域可作為 [**發行者] 網域**。

在 [ **API 許可權**] 中，移除 [ **Microsoft Graph**  >  **使用者]。 [讀取**] 許可權，因為應用程式不需要登入或使用者設定檔存取權。

在中**公開 API**：

1. 選取 [新增範圍]。
1. 選取 [儲存並繼續]  。
1. 提供**範圍名稱**（例如， `API.Access` ）。
1. 提供系統**管理員同意顯示名稱**（例如 `Access API` ）。
1. 提供系統**管理員同意描述**（例如 `Allows the app to access server app API endpoints.` ）。
1. 確認 [**狀態**] 設定為 [**已啟用**]。
1. 選取 [新增範圍]。

記錄下列資訊：

* 應用程式識別碼 URI （例如， `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd` 、 `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` 或您提供的自訂值）
* 預設範圍（例如， `API.Access` ）

應用程式識別碼 URI 可能需要用戶端應用程式中的特殊設定，如本主題稍後的[存取權杖範圍](#access-token-scopes)一節中所述。

### <a name="register-a-client-app"></a>註冊用戶端應用程式

請遵循[快速入門：使用 Microsoft 身分識別平臺註冊應用程式](/azure/active-directory/develop/quickstart-register-app)和後續的 Azure AAD 主題中的指導方針，為*用戶端應用程式*註冊 AAD 應用程式，然後執行下列動作：

1. 在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。
1. 提供應用程式的**名稱**（例如** Blazor 用戶端 AAD**）。
1. 選擇**支援的帳戶類型**。 在此體驗中，您可以**只選取此組織目錄中的帳戶**（單一租使用者）。
1. 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行之應用程式的預設埠是5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，在 [**調試**程式] 面板的伺服器應用程式屬性中，可以找到應用程式的隨機產生埠。 由於應用程式目前不存在，且 IIS Express 埠未知，請在建立應用程式之後返回此步驟，並更新重新導向 URI。 [[建立應用程式](#create-the-app)] 區段中會出現一個批註，提醒 IIS Express 使用者更新重新導向 URI。
1. 停用 **[授與系統**  >  **管理員同意 openid 和 offline_access 許可權**] 核取方塊。
1. 選取 [註冊]。

記錄*用戶端應用*程式（用戶端）識別碼（例如 `4369008b-21fa-427c-abaa-9b53bf58e538` ）。

在 [**驗證**  >  **平臺**設定]  >  **Web**：

1. 確認的重新**導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存]**** 按鈕。

在 [ **API 許可權**] 中：

1. 確認應用程式已**Microsoft Graph**  >  **使用者。讀取**許可權。
1. 選取 [**新增許可權**]，後面接著 [**我的 api**]。
1. 從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如， ** Blazor Server AAD**）。
1. 開啟 [ **API**清單]。
1. 啟用 API 的存取權（例如， `API.Access` ）。
1. 選取 [新增權限]。
1. 選取 [為 **{租使用者名稱} 授與系統管理員同意**] 按鈕。 選取 [是] 以確認。

### <a name="create-the-app"></a>建立應用程式

在空的資料夾中，將下列命令中的預留位置取代為先前記錄的資訊，並在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -o {APP NAME} --tenant-id "{TENANT ID}"
```

| 預留位置                  | Azure 入口網站名稱                                     | 範例                                |
| ---------------------------- | ----------------------------------------------------- | -------------------------------------- |
| `{APP NAME}`                 | &mdash;                                               | `BlazorSample`                         |
| `{CLIENT APP CLIENT ID}`     | *用戶端應用*程式的應用程式（用戶端）識別碼          | `4369008b-21fa-427c-abaa-9b53bf58e538` |
| `{DEFAULT SCOPE}`            | 領域名稱                                            | `API.Access`                           |
| `{SERVER API APP CLIENT ID}` | *伺服器 API 應用*程式的應用程式（用戶端）識別碼      | `41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{SERVER API APP ID URI}`    | 應用程式識別碼 URI （[請參閱附注](#access-token-scopes)） | `41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{TENANT DOMAIN}`            | 主要/發行者/租使用者網域                       | `contoso.onmicrosoft.com`              |
| `{TENANT ID}`                | 目錄 (租用戶) 識別碼                                 | `e86c78e2-8bb4-4c41-aefd-918e0565a45e` |

使用選項指定的輸出位置會 `-o|--output` 建立專案資料夾（如果不存在），並成為應用程式名稱的一部分。

> [!NOTE]
> 將應用程式識別碼 URI 傳遞給 `app-id-uri` 選項，但請注意，在用戶端應用程式中可能需要進行設定變更，如[存取權杖範圍](#access-token-scopes)一節中所述。

> [!NOTE]
> 在 Azure 入口網站中，*用戶端應用程式的***驗證**  >  **平臺**  >  設定**Web**重新  >  **導向 URI**會針對使用預設設定在 Kestrel 伺服器上執行的應用程式，設定為埠5001。
>
> 如果*用戶端應用程式*是在隨機 IIS Express 埠上執行，則可以在 [**調試**程式] 面板的*伺服器 API 應用程式*屬性中找到應用程式的埠。
>
> 如果未在*用戶端應用程式的*已知埠之前設定埠，請回到 Azure 入口網站中的*用戶端應用程式*註冊，並使用正確的埠更新重新導向 URI。

## <a name="server-app-configuration"></a>伺服器應用程式設定

*本節適用于解決方案的 **`Server`** 應用程式。*

### <a name="authentication-package"></a>驗證套件

封裝會提供驗證和授權呼叫 ASP.NET Core Web Api 的支援 [`Microsoft.AspNetCore.Authentication.AzureAD.UI`](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI/) ：

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
  Version="3.1.4" />
```

### <a name="authentication-service-support"></a>驗證服務支援

方法會在 `AddAuthentication` 應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。 <xref:Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions.AddAzureADBearer%2A>方法會設定 JWT 持有人處理常式中所需的特定參數，以驗證 Azure Active Directory 所發出的權杖：

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A>並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> 確定：

* 應用程式會嘗試剖析並驗證傳入要求的權杖。
* 嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a>使用者. Identity 。檔案名

根據預設，伺服器應用程式 API 會填入 `User.Identity.Name` 來自宣告類型的值（例如 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` `2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com` ）。

若要將應用程式設定為從宣告類型接收值 `name` ，請 <xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType?displayProperty=nameWithType> <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> 在中設定的 `Startup.ConfigureServices` ：

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>應用程式設定

檔案 `appsettings.json` 包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式：

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{SERVER API APP CLIENT ID}",
  }
}
```

範例：

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "contoso.onmicrosoft.com",
    "TenantId": "e86c78e2-8bb4-4c41-aefd-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
  }
}
```

### <a name="weatherforecast-controller"></a>WeatherForecast 控制器

WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至控制器。 請**務必**瞭解：

* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)此 api 控制器中的屬性是保護此 api 免于未經授權存取的唯一做法。
* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)應用程式中所使用的屬性 Blazor WebAssembly 只會作為應用程式的提示，使用者應該獲得授權，應用程式才能正確運作。

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

*本節適用于解決方案的 **`Client`** 應用程式。*

### <a name="authentication-package"></a>驗證套件

建立應用程式以使用工作或學校帳戶（）時 `SingleOrg` ，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（）的套件參考 [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) 。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

[`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)封裝可傳遞會將 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 套件新增至應用程式。

### <a name="authentication-service-support"></a>驗證服務支援

<xref:System.Net.Http.HttpClient>加入實例的支援，其中包含對伺服器專案提出要求時的存取權杖。

`Program.cs`:

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
        client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddScoped(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱（例如， `BlazorSample.ServerAPI` ）。

使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) 。 這個方法會設定應用程式與 Identity 提供者（IP）互動所需的服務。

`Program.cs`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

設定是由檔案所提供 `wwwroot/appsettings.json` ：

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/{TENANT ID}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": true
  }
}
```

範例：

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/e86c78e2-...-918e0565a45e",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": true
  }
}
```

### <a name="access-token-scopes"></a>存取權杖範圍

預設存取權杖範圍代表存取權杖範圍的清單，如下所示：

* 預設包含在登入要求中。
* 用來在驗證之後立即布建存取權杖。

所有範圍都必須屬於相同的應用程式，每個 Azure Active Directory 規則。 您可以視需要為其他 API 應用程式新增額外的範圍：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

如需詳細資訊，請參閱*其他案例*文章的下列章節：

* [要求其他存取權杖](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加到連出要求](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)


### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

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

從伺服器專案執行應用程式。 使用 Visual Studio 時，您可以：

* 將工具列中的 [**啟始專案**] 下拉式清單設定為*伺服器 API 應用程式*，然後選取 [**執行**] 按鈕。
* 在**方案總管**中選取伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:blazor/security/webassembly/additional-scenarios>
* [在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:blazor/security/webassembly/aad-groups-roles>
* <xref:security/authentication/azure-active-directory/index>
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
