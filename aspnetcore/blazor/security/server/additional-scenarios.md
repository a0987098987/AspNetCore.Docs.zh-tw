---
title: ASP.NET Core Blazor Server 其他安全性案例
author: guardrex
description: 瞭解如何設定 Blazor Server 其他安全性案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/04/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/server/additional-scenarios
ms.openlocfilehash: 46de9a22dec540b8dfda7583b7a3c5c2dcbbc549
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85402321"
---
# <a name="aspnet-core-blazor-server-additional-security-scenarios"></a>ASP.NET Core Blazor Server 其他安全性案例

By [Javier Calvarro Nelson](https://github.com/javiercn)

## <a name="pass-tokens-to-a-blazor-server-app"></a>將權杖傳遞給 Blazor Server 應用程式

您 Razor Blazor Server 可以使用本節所述的方法，將應用程式中元件外部可用的權杖傳遞給元件。 如需範例程式碼，包括完整的 `Startup.ConfigureServices` 範例，請參閱將[權杖傳遞給伺服器端 Blazor 應用程式](https://github.com/javiercn/blazor-server-aad-sample)。

Blazor Server以一般 Razor 頁面或 MVC 應用程式的方式驗證應用程式。 布建權杖並將其儲存至驗證 cookie。 例如：

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

定義一個類別，以使用存取和重新整理權杖來傳入初始應用程式狀態：

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

定義可在應用程式內使用的**範圍**權杖提供者服務， Blazor 以從相依性[插入（DI）](xref:blazor/fundamentals/dependency-injection)解析權杖：

```csharp
public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

在中 `Startup.ConfigureServices` ，為新增服務：

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

在檔案中 `_Host.cshtml` ，建立和實例， `InitialApplicationState` 並將它當做參數傳遞給應用程式：

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

在 `App` 元件（ `App.razor` ）中，解析服務，並使用參數中的資料進行初始化：

```razor
@inject TokenProvider TokenProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokenProvider.AccessToken = InitialState.AccessToken;
        TokenProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

在提出安全 API 要求的服務中，插入權杖提供者，並取出權杖以呼叫 API：

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="set-the-authentication-scheme"></a>設定驗證配置

對於使用多個驗證中介軟體，因而有多個驗證配置的應用程式，使用的配置 Blazor 可以在的端點設定中明確設定 `Startup.Configure` 。 下列範例會設定 Azure Active Directory 配置：

```csharp
endpoints.MapBlazorHub().RequireAuthorization(
    new AuthorizeAttribute 
    {
        AuthenticationSchemes = AzureADDefaults.AuthenticationScheme
    });
```

## <a name="use-open-id-connect-oidc-v20-endpoints"></a>使用 Open ID Connect （OIDC） v2.0 端點

驗證程式庫和 Blazor 範本會使用 OPEN ID Connect （OIDC） v1.0 端點。 若要使用 v2.0 端點，請 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority?displayProperty=nameWithType> 在中設定選項 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions> ：

```csharp
services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, 
    options =>
    {
        options.Authority += "/v2.0";
    }
```

或者，您也可以在應用程式設定（）檔案中進行設定 `appsettings.json` ：

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/common/oauth2/v2.0/",
    ...
  }
}
```

如果在區段上對授權單位的追蹤不適合應用程式的 OIDC 提供者（例如使用非 AAD 提供者），請 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> 直接設定屬性。 請在 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions> 應用程式佈建檔中，以金鑰設定或的屬性 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> 。

### <a name="code-changes"></a>程式碼變更

* 針對 v2.0 端點，識別碼權杖中的宣告清單會變更。 如需詳細資訊，請參閱[為何要更新至 Microsoft 身分識別平臺（v2.0）？](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison) 在 Azure 檔中。
* 因為在 v2.0 端點的範圍 Uri 中指定了資源，所以請移除 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Resource?displayProperty=nameWithType> 中的屬性設定 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions> ：

  ```csharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options => 
      {
          ...
          options.Resource = "...";    // REMOVE THIS LINE
          ...
      }
      ```

  For more information, see [Scopes, not resources](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison#scopes-not-resources) in the Azure documentation.

### App ID URI

* When using v2.0 endpoints, APIs define an *`App ID URI`*, which is meant to represent a unique identifier for the API.
* All scopes include the App ID URI as a prefix, and v2.0 endpoints emit access tokens with the App ID URI as the audience.
* When using V2.0 endpoints, the client ID configured in the Server API changes from the API Application ID (Client ID) to the App ID URI.

`appsettings.json`:

```json
{
  "AzureAd": {
    ...
    "ClientId": "https://{TENANT}.onmicrosoft.com/{APP NAME}"
    ...
  }
}
```

您可以在 OIDC 提供者應用程式註冊描述中找到要使用的應用程式識別碼 URI。
