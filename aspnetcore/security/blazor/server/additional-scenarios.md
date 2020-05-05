---
title: ASP.NET Core Blazor Server 其他安全性案例
author: guardrex
description: 瞭解如何設定Blazor伺服器以進行其他安全性案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/server/additional-scenarios
ms.openlocfilehash: 95e9e57889fdbb5270f895874c9b8148ae4ca48d
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82772800"
---
# <a name="aspnet-core-blazor-server-additional-security-scenarios"></a>ASP.NET Core Blazor Server 其他安全性案例

By [Javier Calvarro Nelson](https://github.com/javiercn)

## <a name="pass-tokens-to-a-blazor-server-app"></a>將權杖傳遞至Blazor伺服器應用程式

您可以使用本節所Razor述的方法Blazor ，將伺服器應用程式中元件外部可用的權杖傳遞給元件。 如需範例程式碼，包括`Startup.ConfigureServices`完整的範例，請參閱將[權杖傳遞給伺服器Blazor端應用程式](https://github.com/javiercn/blazor-server-aad-sample)。

驗證服務器Blazor應用程式的方式與一般Razor頁面或 MVC 應用程式一樣。 布建權杖並將其儲存至驗證 cookie。 例如：

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

定義可**scoped**在Blazor應用程式內使用的範圍權杖提供者服務，以從相依性[插入（DI）](xref:blazor/dependency-injection)解析權杖：

```csharp
public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

在`Startup.ConfigureServices`中，為新增服務：

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

在 *_Host 的 cshtml*檔案中，建立和實例， `InitialApplicationState`並將它當做參數傳遞給應用程式：

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

在`App`元件（*razor*）中，解析服務，並使用參數中的資料進行初始化：

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
