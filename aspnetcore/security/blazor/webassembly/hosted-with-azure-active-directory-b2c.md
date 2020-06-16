---
title: Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 託管應用程式
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
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: f7b53c8043b53912bcca67bae868e3b516ad2e0d
ms.sourcegitcommit: b0062f29cba2e5c21b95cf89eaf435ba830d11a3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84776458"
---
# <a name="secure-an-aspnet-core-blazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a>Blazor使用 Azure Active Directory B2C 保護 ASP.NET Core WebAssembly 託管應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

本文說明如何建立 Blazor WebAssembly 獨立應用程式，以使用[AZURE ACTIVE DIRECTORY （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證。

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>在 AAD B2C 中註冊應用程式並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

依照[教學課程：建立 Azure Active Directory B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者中的指引來建立 AAD B2C 租使用者。

記錄下列資訊：

* AAD B2C 實例（例如， `https://contoso.b2clogin.com/` 包含尾端斜線的）
* AAD B2C 租使用者網域（例如， `contoso.onmicrosoft.com` ）

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

請遵循教學課程[：在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*伺服器 API 應用*程式註冊 AAD 應用程式，然後執行下列動作：

1. 在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。
1. 提供應用程式的**名稱**（例如， ** Blazor 伺服器 AAD B2C**）。
1. 針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**
1. 在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。
1. 確認**Permissions**  >  已啟用 [許可權]，授與系統**管理員同意 openid 和 offline_access 的許可權**。
1. 選取 [註冊]。

記錄下列資訊：

* *伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如， `11111111-1111-1111-1111-111111111111` ）
* 目錄識別碼（租使用者識別碼）（例如， `222222222-2222-2222-2222-222222222222` ）
* AAD 租使用者網域（例如， `contoso.onmicrosoft.com` ）：在已註冊的應用程式之 Azure 入口網站的 [**商標**] 分頁中，網域會以**發行者網域**的形式提供。

在中**公開 API**：

1. 選取 [新增範圍]。
1. 選取 [儲存並繼續]  。
1. 提供**範圍名稱**（例如， `API.Access` ）。
1. 提供系統**管理員同意顯示名稱**（例如 `Access API` ）。
1. 提供系統**管理員同意描述**（例如 `Allows the app to access server app API endpoints.` ）。
1. 確認 [**狀態**] 設定為 [**已啟用**]。
1. 選取 [新增範圍]。

記錄下列資訊：

* 應用程式識別碼 URI （例如， `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111` 、 `api://11111111-1111-1111-1111-111111111111` 或您提供的自訂值）
* 預設範圍（例如， `API.Access` ）

### <a name="register-a-client-app"></a>註冊用戶端應用程式

請遵循教學課程[：再次在 Azure Active Directory B2C 中註冊應用程式中](/azure/active-directory-b2c/tutorial-register-applications)的指導方針，為*用戶端應用程式*註冊 AAD 應用程式，然後執行下列動作：

1. 在**Azure Active Directory**  >  **應用程式註冊**中，選取 [**新增註冊**]。
1. 提供應用程式的**名稱**（例如， ** Blazor 用戶端 AAD B2C**）。
1. 針對**支援的帳戶類型**，請選取 [多租使用者] 選項： [**任何組織目錄中的帳戶] 或 [任何身分識別提供者]。用於驗證 Azure AD B2C 的使用者。**
1. 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行之應用程式的預設埠是5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，在 [**調試**程式] 面板的伺服器應用程式屬性中，可以找到應用程式的隨機產生埠。 由於應用程式目前不存在，且 IIS Express 埠未知，請在建立應用程式之後返回此步驟，並更新重新導向 URI。 [[建立應用程式](#create-the-app)] 區段中會出現一個批註，提醒 IIS Express 使用者更新重新導向 URI。
1. 確認**Permissions**  >  已啟用 [許可權]，授與系統**管理員同意 openid 和 offline_access 的許可權**。
1. 選取 [註冊]。

記錄應用程式識別碼（用戶端識別碼）（例如 `11111111-1111-1111-1111-111111111111` ）。

在 [**驗證**  >  **平臺**設定]  >  **Web**：

1. 確認的重新**導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存]**** 按鈕。

在 [ **API 許可權**] 中：

1. 選取 [**新增許可權**]，後面接著 [**我的 api**]。
1. 從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如，[ ** Blazor 伺服器 AAD B2C**]）。
1. 開啟 [ **API**清單]。
1. 啟用 API 的存取權（例如， `API.Access` ）。
1. 選取 [新增權限]。
1. 選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。 選取 [是]  確認。

在**Home**  >  **Azure AD B2C**  >  **使用者流程**：

[建立註冊和登入使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows)

至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` `LoginDisplay` 元件（*Shared/LoginDisplay*）中的。

記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin` ）。

### <a name="create-the-app"></a>建立應用程式

以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。 資料夾名稱也會成為專案名稱的一部分。

> [!NOTE]
> 將應用程式識別碼 URI 傳遞給 `app-id-uri` 選項，但請注意，在用戶端應用程式中可能需要進行設定變更，如[存取權杖範圍](#access-token-scopes)一節中所述。
>
> 此外，由裝載的範本所設定的範圍 Blazor 可能會重複應用程式識別碼 URI 主機。 確認 `DefaultAccessTokenScopes` 在 `Program.Main` *用戶端應用程式*的（*Program.cs*）中為集合設定的範圍是正確的。

> [!NOTE]
> 在 Azure 入口網站中，*用戶端應用程式的***驗證**  >  **平臺**  >  設定**Web**重新  >  **導向 URI**會針對使用預設設定在 Kestrel 伺服器上執行的應用程式，設定為埠5001。
>
> 如果*用戶端應用程式*是在隨機 IIS Express 埠上執行，則可以在 [**調試**程式] 面板的*伺服器應用程式*屬性中找到應用程式的埠。
>
> 如果未在*用戶端應用程式的*已知埠之前設定埠，請回到 Azure 入口網站中的*用戶端應用程式*註冊，並使用正確的埠更新重新導向 URI。

## <a name="server-app-configuration"></a>伺服器應用程式設定

*本節適用于解決方案的**伺服器**應用程式。*

### <a name="authentication-package"></a>驗證套件

對 ASP.NET Core Web Api 的驗證和授權呼叫的支援，是由[AspNetCore. AzureADB2C. UI](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI/)套件所提供：

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureADB2C.UI" 
  Version="3.1.4" />
```

### <a name="authentication-service-support"></a>驗證服務支援

方法會在 `AddAuthentication` 應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。 <xref:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions.AddAzureADB2CBearer%2A>方法會設定 JWT 持有人處理常式中所需的特定參數，以驗證 Azure Active Directory B2C 所發出的權杖：

```csharp
services.AddAuthentication(AzureADB2CDefaults.BearerAuthenticationScheme)
    .AddAzureADB2CBearer(options => Configuration.Bind("AzureAdB2C", options));
```

<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A>並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> 確定：

* 應用程式會嘗試剖析並驗證傳入要求的權杖。
* 嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a>使用者. Identity 。檔案名

根據預設， `User.Identity.Name` 不會填入。

若要將應用程式設定為從宣告類型接收值 `name` ，請在中設定的[TokenValidationParameters。 NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> `Startup.ConfigureServices` ：

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADB2CDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>應用程式設定

檔案*上的appsettings.js*包含設定用來驗證存取權杖之 JWT 持有人處理常式的選項。

```json
{
  "AzureAdB2C": {
    "Instance": "https://{TENANT}.b2clogin.com/",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "Domain": "{TENANT DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

範例：

```json
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com/",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "Domain": "contoso.onmicrosoft.com",
    "SignUpSignInPolicyId": "B2C_1_signupsignin1",
  }
}
```

### <a name="weatherforecast-controller"></a>WeatherForecast 控制器

WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至控制器。 請**務必**瞭解：

* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)此 api 控制器中的屬性是保護此 api 免于未經授權存取的唯一做法。
* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)WebAssembly 應用程式中所使用的屬性 Blazor 只會作為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。

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

當您建立應用程式以使用個別 B2C 帳戶（ `IndividualB2C` ）時，應用程式會自動接收[microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（[WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

[WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)套件可轉移會將[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)套件新增至應用程式中的。

### <a name="authentication-service-support"></a>驗證服務支援

<xref:System.Net.Http.HttpClient>加入實例的支援，其中包含對伺服器專案提出要求時的存取權杖。

*Program.cs*：

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱（例如， `BlazorSample.ServerAPI` ）。

驗證使用者的支援是在服務容器中註冊，並使用 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> [WebAssembly. Msal](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)套件所提供的擴充方法。 這個方法會設定應用程式與 Identity 提供者（IP）互動所需的服務。

*Program.cs*：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

Configuration 是由*wwwroot/appsettings.json*檔案提供：

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{TENANT DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": false
  }
}
```

範例：

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": false
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

* [要求其他存取權杖](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加到連出要求](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)


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

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/webassembly/additional-scenarios>
* [在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/authentication/azure-ad-b2c>
* [教學課程：建立 Azure Active Directory B2C 租用戶](/azure/active-directory-b2c/tutorial-create-tenant)
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
