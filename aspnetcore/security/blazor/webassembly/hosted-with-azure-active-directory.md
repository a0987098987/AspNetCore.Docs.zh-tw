---
title: 使用 AzureBlazor活動目錄保護ASP.NET核心 Web 元件託管應用
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 8fec9f585f42469665cf29069674a199e1626629
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977128"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>使用 AzureBlazor活動目錄保護ASP.NET核心 Web 元件託管應用

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]



本文介紹如何建立使用[Azure 活動目錄 (AAD)](https://azure.microsoft.com/services/active-directory/)進行身份認證的[BlazorWebAssembly 託管應用](xref:blazor/hosting-models#blazor-webassembly)。

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>在 AAD B2C 中註冊應用並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

按照[「快速入門」中的指南:設置租戶](/azure/active-directory/develop/quickstart-create-new-tenant)以在 AAD 中創建租戶。

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

按照['快速入門'中的指南操作:將應用程式註冊到 Microsoft 識別平台](/azure/active-directory/develop/quickstart-register-app)和後續 Azure AAD 主題,以在 Azure 門戶的**Azure 活動目錄** > **應用註冊**區域註冊 伺服器*API 應用*的 AAD 應用:

1. 選取 [新增註冊]  。
1. 為應用提供**名稱**(例如,**Blazor伺服器 AAD**)。
1. 選擇**支援的帳號型態 。** 對於此體驗,您只能選擇**此組織目錄中的帳戶**(單個租戶)。
1. 在這種情況下,*伺服器 API 應用*不需要重定向**URI,** 因此請將下拉清單設定為**Web,** 並且不輸入重定向 URI。
1. 禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。
1. 選取 [註冊]  。

在**API 許可權**中,刪除 Microsoft**圖形** > **使用者.讀取**許可權,因為應用不需要登錄或存取設定檔。

在**公開 API 中**:

1. 選擇 **「添加範圍**」。
1. 選取 [儲存並繼續]****。
1. 提供**範圍名稱**(例如`API.Access`,
1. 提供**管理員同意顯示名稱**(例如`Access API`,
1. 提供**管理員同意說明**(例如`Allows the app to access server app API endpoints.`,
1. 確認**狀態**設定為**啟用**。
1. 選擇 **「添加範圍**」。

記錄以下資訊:

* *伺服器 API 應用程式*應用程式代碼(用戶端代碼)(例如`11111111-1111-1111-1111-111111111111`)
* 套用 ID`https://contoso.onmicrosoft.com/11111111-1111-1111-1111-111111111111`URI(例如,`api://11111111-1111-1111-1111-111111111111`或您提供的自訂值 )
* 目錄 ID(租戶代碼)(`222222222-2222-2222-2222-222222222222`例如 )
* AAD 租戶域(例如`contoso.onmicrosoft.com`)
* 默認作用域(例如), `API.Access`

### <a name="register-a-client-app"></a>註冊用戶端應用程式

按照[「快速入門」中的指南操作:將應用程式註冊到 Microsoft 標識平台](/azure/active-directory/develop/quickstart-register-app)和後續 Azure AAD 主題,以在 Azure 門戶的**Azure 活動目錄** > **應用註冊**區域註冊*客戶端應用*的 AAD 應用:

1. 選取 [新增註冊]  。
1. 為應用提供**名稱**(例如,**Blazor用戶端 AAD**)。
1. 選擇**支援的帳號型態 。** 對於此體驗,您只能選擇**此組織目錄中的帳戶**(單個租戶)。
1. 將**重定向 URI**下拉清單設定為**Web,** 並提供`https://localhost:5001/authentication/login-callback`重定向 URI。
1. 禁用 **「許可權** > **授予管理員集中打開」。和offline_access權限**複選框。
1. 選取 [註冊]  。

在**認證** > **平台設定中** > **, Web**:

1. 確認存在 重定`https://localhost:5001/authentication/login-callback`向**URI。**
1. 對於**隱式授予**,選擇 Access**權杖**和**ID 權杖的**複選框。
1. 此體驗可以接受應用的剩餘預設值。
1. 選取 [儲存]**** 按鈕。

在**API 權限**中:

1. 確認應用具有**Microsoft 圖形** > **使用者.讀取**許可權。
1. 選擇 **「新增」** 後跟**我的 API 的權限**。
1. 從 **「名稱」** 列中選擇*伺服器 API 應用*(例如,**Blazor伺服器 AAD**)。
1. 開啟**API**清單。
1. 啟用對 API 的訪問(`API.Access`例如,
1. 選取 [新增權限]****。
1. 選擇 **[TENANT 名稱] 按鈕的管理員內容**。 選取 [是] **** 加以確認。

記錄*客戶端應用*應用程式ID(用戶端ID)(`33333333-3333-3333-3333-333333333333`例如。

### <a name="create-the-app"></a>建立應用程式

將以下指令中的佔位符替換為前面記錄的資訊,並在命令 shell 中執行該指令:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{DOMAIN}" -ho --tenant-id "{TENANT ID}"
```

要指定輸出位置(如果不存在,則創建項目資料夾)請在命令中包含具有路徑的輸出選項(例如。 `-o BlazorSample` 資料夾名稱也將成為專案名稱的一部分。

> [!NOTE]
> 將應用 ID`app-id-uri`URI 傳遞給該選項,但請注意客戶端應用中可能需要更改配置,這在[Access 權杖作用域](#access-token-scopes)部分中對此進行了說明。

## <a name="server-app-configuration"></a>伺服器應用設定

*本節涉及解決方案的 **「伺服器」** 應用。*

### <a name="authentication-package"></a>驗證驗證

認證與授權呼叫ASP.NET核心 Web API 的支援`Microsoft.AspNetCore.Authentication.AzureAD.UI`由:

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
    Version="3.1.0" />
```

### <a name="authentication-service-support"></a>認證服務支援

該方法`AddAuthentication`在應用中設置身份驗證服務,並將 JWT 承載處理程式配置為預設身份驗證方法。 該方法`AddAzureADBearer`在 JWT 承載處理程式中設定驗證 Azure 活動目錄發出的權杖所需的特定參數:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

`UseAuthentication`並確保`UseAuthorization`:

* 應用嘗試對傳入請求解析和驗證權杖。
* 嘗試在沒有適當認證的權利訪問受保護資源的任何請求都失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="useridentityname"></a>User.Identity.Name

預設情況下,伺服器應用`User.Identity.Name`API`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`使用 宣告類型中的值填充`2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com`(例如,

要將應用配置為從`name`聲明類型接收值,請<xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions>配置`Startup.ConfigureServices`中的[令牌驗證參數。](xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType)

```csharp
services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>應用程式設定

*appsettings.json*檔包含用於配置用於驗證存取權杖的JWT承載處理程式的選項。

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

### <a name="weatherforecast-controller"></a>天氣預報控制器

天氣預報控制器 (*控制器/天氣預報控制器.cs)* 公開受保護的`[Authorize]`API, 該屬性應用於控制器。 **請務必**瞭解:

* 此`[Authorize]`API 控制器中的屬性是保護此 API 免受未經授權的訪問的唯一原因。
* Blazor WebAssembly 應用中`[Authorize]`使用的 屬性僅用作應用的提示,即應授權使用者使應用正常工作。

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

## <a name="client-app-configuration"></a>用戶端應用設定

*本節涉及解決方案的**用戶端**應用。*

### <a name="authentication-package"></a>驗證驗證

建立應用程式以使用工作帳號或學校帳戶 ()`SingleOrg`時,應用程式會自動接收 Microsoft[身份驗證函式庫](/azure/active-directory/develop/msal-overview)()`Microsoft.Authentication.WebAssembly.Msal`的套件參考 。 該包提供一組基元,可幫助應用對使用者進行身份驗證,並獲取令牌來調用受保護的 API。

如果向應用程式加入認證,則手動將套件加入到應用程式的項目檔中:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
    Version="{VERSION}" />
```

在前面`{VERSION}`的包引用中替換`Microsoft.AspNetCore.Blazor.Templates`<xref:blazor/get-started>為 本文中顯示的包版本。

包`Microsoft.Authentication.WebAssembly.Msal`會臨時將`Microsoft.AspNetCore.Components.WebAssembly.Authentication`包添加到應用。

### <a name="authentication-service-support"></a>認證服務支援

使用`AddMsalAuthentication``Microsoft.Authentication.WebAssembly.Msal`包提供的擴充方法在服務容器中註冊對使用者進行身份驗證的支援。 此方法設置應用與標識提供者 (IP) 互動所需的所有服務。

*Program.cs*:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    var authentication = options.ProviderOptions.Authentication;
    authentication.Authority = "https://login.microsoftonline.com/{TENANT ID}";
    authentication.ClientId = "{CLIENT ID}";
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

該方法`AddMsalAuthentication`接受回調以配置驗證應用所需的參數。 註冊應用時,可以從 Azure 門戶 AAD 配置中獲取配置應用所需的值。

### <a name="access-token-scopes"></a>存取權杖範圍

預設存取權杖範圍表示存取權杖的清單,這些作用網域是:

* 默認情況下包含在登錄請求中。
* 用於在身份驗證后立即預配訪問權杖。

根據 Azure 活動目錄規則,所有作用域必須屬於同一應用。 根據需要為其他 API 應用添加其他作用域:

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

### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a>套用元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>重定到登入元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>登入元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>擷取資料元件

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>執行應用程式

從「伺服器」專案運行應用。 使用 Visual Studio 時,在**解決方案資源管理器**中選擇「伺服器」專案,然後選擇工具列中的 **「運行」** 按鈕,或從 **「調試」** 選單啟動應用。

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* [要求其他存取權杖](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* <xref:security/authentication/azure-active-directory/index>
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
