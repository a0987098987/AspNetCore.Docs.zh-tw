---
title: 使用 Azure Active Directory B2C 保護Blazor ASP.NET Core WebAssembly 託管應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 6f049906da4a1aa87f293f5ad1af19dad44f477a
ms.sourcegitcommit: 6d271f4b4c3cd1e82267f51d9bfb6de221c394fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150041"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a>使用 Azure Active Directory B2C 保護Blazor ASP.NET Core WebAssembly 託管應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

本文說明如何建立Blazor WebAssembly 獨立應用程式，以使用[Azure Active Directory （AAD） B2C](/azure/active-directory-b2c/overview)進行驗證。

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>在 AAD B2C 中註冊應用程式並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

請遵循教學課程[：建立 Azure Active Directory B2C 租](/azure/active-directory-b2c/tutorial-create-tenant)使用者中的指引來建立 AAD B2C 租使用者，並記錄下列資訊：

* AAD B2C 實例（ `https://contoso.b2clogin.com/`例如，包含尾端斜線的）
* AAD B2C 租使用者網域（例如， `contoso.onmicrosoft.com`）

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

請遵循教學課程[：在 Azure Active Directory B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications)中的指導方針，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中，為*伺服器 API 應用*程式註冊 AAD 應用程式：

1. 選取 [新增註冊]  。
1. 提供應用程式的**名稱**（例如， ** Blazor伺服器 AAD B2C**）。
1. 針對**支援的帳戶類型**，選取**任何組織目錄中的帳戶或任何識別提供者。用於驗證 Azure AD B2C 的使用者。** （多租使用者）此體驗。
1. 在此案例中，*伺服器 API 應用程式*不需要重新**導向 uri** ，因此，請將下拉式關閉設定為 [ **Web** ]，而不要輸入 [重新導向 uri]。
1. 確認**許可權** > **授與系統管理員收到給 openid，並已啟用 offline_access 許可權**。
1. 選取 [註冊]  。

在中**公開 API**：

1. 選取 [**新增範圍**]。
1. 選取 [儲存並繼續]  。
1. 提供**範圍名稱**（例如， `API.Access`）。
1. 提供系統**管理員同意顯示名稱**（例如`Access API`）。
1. 提供系統**管理員同意描述**（例如`Allows the app to access server app API endpoints.`）。
1. 確認 [**狀態**] 設定為 [**已啟用**]。
1. 選取 [**新增領域**]。

記錄下列資訊：

* *伺服器 API 應用程式*應用程式識別碼（用戶端識別碼）（例如`11111111-1111-1111-1111-111111111111`，）
* 應用程式識別碼 URI （ `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111` `api://11111111-1111-1111-1111-111111111111`例如，、或您提供的自訂值）
* 目錄識別碼（租使用者識別碼）（例如， `222222222-2222-2222-2222-222222222222`）
* *伺服器 API 應用程式*應用程式識別碼 URI （ `https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`例如，Azure 入口網站可能會將值預設為用戶端識別碼）
* 預設範圍（例如， `API.Access`）

### <a name="register-a-client-app"></a>註冊用戶端應用程式

請依照教學課程[：在 Azure Active Directory B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications)中的指導方針，在 Azure 入口網站的**Azure Active Directory** > **應用程式註冊**區域中，為*用戶端應用程式*註冊 AAD 應用程式：

1. 選取 [新增註冊]  。
1. 提供應用程式的**名稱**（例如， ** Blazor用戶端 AAD B2C**）。
1. 針對**支援的帳戶類型**，選取**任何組織目錄中的帳戶或任何識別提供者。用於驗證 Azure AD B2C 的使用者。** （多租使用者）此體驗。
1. 將 [重新**導向 uri** ] 下拉式設定保留為 [ **Web**]，並提供`https://localhost:5001/authentication/login-callback`的 [重新導向 uri]。
1. 確認**許可權** > **授與系統管理員收到給 openid，並已啟用 offline_access 許可權**。
1. 選取 [註冊]  。

在 [**驗證** > **平臺** > 設定]**Web**：

1. 確認的重新**導向 URI** `https://localhost:5001/authentication/login-callback`存在。
1. 針對 **[隱含授**與]，選取 [**存取權杖**] 和 [**識別碼權杖**] 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存]**** 按鈕。

在 [ **API 許可權**] 中：

1. 確認應用程式已**Microsoft Graph** > **使用者。讀取**許可權。
1. 選取 [**新增許可權**]，後面接著 [**我的 api**]。
1. 從 [**名稱**] 資料行中選取*伺服器 API 應用程式*（例如， ** Blazor [伺服器 AAD B2C**]）。
1. 開啟 [ **API**清單]。
1. 啟用 API 的存取權（例如， `API.Access`）。
1. 選取 [新增權限]  。
1. 選取 [**授與 {租使用者名稱} 的系統管理員內容**] 按鈕。 選取 [是] **** 加以確認。

在**Home** > **Azure AD B2C** > **使用者流程**：

[建立註冊和登入使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows)

至少選取 [**應用程式宣告** > **顯示名稱**] 使用者屬性，以填入`context.User.Identity.Name` `LoginDisplay`元件（*Shared/LoginDisplay*）中的。

記錄下列資訊：

* 記錄*用戶端應用*程式識別碼（用戶端識別碼）（例如`33333333-3333-3333-3333-333333333333`）。
* 記錄為應用程式建立的註冊和登入使用者流程名稱（例如， `B2C_1_signupsignin`）。

### <a name="create-the-app"></a>建立應用程式

以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho -ssp "{SIGN UP OR SIGN IN POLICY}" --tenant-id "{TENANT ID}"
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如`-o BlazorSample`）的 output 選項。 資料夾名稱也會成為專案名稱的一部分。

> [!NOTE]
> 將應用程式識別碼 URI 傳遞給`app-id-uri`選項，但請注意，在用戶端應用程式中可能需要進行設定變更，如[存取權杖範圍](#access-token-scopes)一節中所述。

## <a name="server-app-configuration"></a>伺服器應用程式設定

*本節適用于解決方案的**伺服器**應用程式。*

### <a name="authentication-package"></a>驗證套件

驗證和授權呼叫 ASP.NET Core Web Api 的支援是由所`Microsoft.AspNetCore.Authentication.AzureADB2C.UI`提供：

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureADB2C.UI" 
    Version="{VERSION}" />
```

### <a name="authentication-service-support"></a>驗證服務支援

`AddAuthentication`方法會在應用程式內設定驗證服務，並將 JWT 持有人處理常式設為預設的驗證方法。 `AddAzureADB2CBearer`方法會設定 JWT 持有人處理常式中所需的特定參數，以驗證 Azure Active Directory B2C 所發出的權杖：

```csharp
services.AddAuthentication(AzureADB2CDefaults.BearerAuthenticationScheme)
    .AddAzureADB2CBearer(options => Configuration.Bind("AzureAdB2C", options));
```

`UseAuthentication`並`UseAuthorization`確定：

* 應用程式會嘗試剖析並驗證傳入要求的權杖。
* 嘗試存取受保護資源而沒有適當認證的任何要求都會失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a>User.Identity.Name

根據預設， `User.Identity.Name`不會填入。

若要將應用程式設定為從`name`宣告類型接收值，請<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions>在中`Startup.ConfigureServices`設定的[TokenValidationParameters。 NameClaimType](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType) ：

```csharp
services.Configure<JwtBearerOptions>(
    AzureADB2CDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>應用程式設定

*Appsettings*包含的選項可設定用來驗證存取權杖的 JWT 持有人處理常式。

```json
{
  "AzureAd": {
    "Instance": "https://{ORGANIZATION}.b2clogin.com/",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "Domain": "{DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

範例：

```json
{
  "AzureAd": {
    "Instance": "https://contoso.b2clogin.com/",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "Domain": "contoso.onmicrosoft.com",
    "SignUpSignInPolicyId": "B2C_1_signupsignin1",
  }
}
```

### <a name="weatherforecast-controller"></a>WeatherForecast 控制器

WeatherForecast 控制器（*控制器/WeatherForecastController*）會公開受保護的 API，並將`[Authorize]`屬性套用至控制器。 請**務必**瞭解：

* 此`[Authorize]` api 控制器中的屬性是保護此 api 免于未經授權存取的唯一做法。
* WebAssembly 應用程式中所使用的`[Authorize]`屬性只會作為應用程式的提示，使用者應該獲得授權，應用程式才能正常運作。 Blazor

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

建立應用程式以使用個別 B2C 帳戶（`IndividualB2C`）時，應用程式會自動接收[Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview)（`Microsoft.Authentication.WebAssembly.Msal`）的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

將`{VERSION}`前述套件參考中的取代為發行項中`Microsoft.AspNetCore.Blazor.Templates` <xref:blazor/get-started>所顯示的套件版本。

`Microsoft.Authentication.WebAssembly.Msal`封裝可傳遞會將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`套件新增至應用程式。

### <a name="authentication-service-support"></a>驗證服務支援

加入`HttpClient`實例的支援，其中包含對伺服器專案提出要求時的存取權杖。

*Program.cs*：

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
        client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

使用`AddMsalAuthentication` `Microsoft.Authentication.WebAssembly.Msal`封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援。 這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。

*Program.cs*：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

`AddMsalAuthentication`方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

Configuration 是由*wwwroot/appsettings*檔案所提供：

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
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

從伺服器專案執行應用程式。 使用 Visual Studio 時，請選取**方案總管**中的伺服器專案，然後選取工具列中的 [**執行**] 按鈕，或從 [**調試**程式] 功能表啟動應用程式。

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/webassembly/additional-scenarios>
* <xref:security/authentication/azure-ad-b2c>
* [教學課程：建立 Azure Active Directory B2C 租用戶](/azure/active-directory-b2c/tutorial-create-tenant)
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
