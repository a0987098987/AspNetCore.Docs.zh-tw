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
- SignalR
uid: security/blazor/server/additional-scenarios
ms.openlocfilehash: 1a3e5a215daedbb9b97c1924275701915806983e
ms.sourcegitcommit: 56861af66bb364a5d60c3c72d133d854b4cf292d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82206386"
---
# <a name="aspnet-core-blazor-server-additional-security-scenarios"></a><span data-ttu-id="b843a-103">ASP.NET Core Blazor Server 其他安全性案例</span><span class="sxs-lookup"><span data-stu-id="b843a-103">ASP.NET Core Blazor Server additional security scenarios</span></span>

<span data-ttu-id="b843a-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="b843a-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

## <a name="pass-tokens-to-a-blazor-server-app"></a><span data-ttu-id="b843a-105">將權杖傳遞至Blazor伺服器應用程式</span><span class="sxs-lookup"><span data-stu-id="b843a-105">Pass tokens to a Blazor Server app</span></span>

<span data-ttu-id="b843a-106">您可以使用本節所述的方法， Blazor將伺服器應用程式中 Razor 元件以外的可用權杖傳遞給元件。</span><span class="sxs-lookup"><span data-stu-id="b843a-106">Tokens available outside of the Razor components in a Blazor Server app can be passed to components with the approach described in this section.</span></span> <span data-ttu-id="b843a-107">如需範例程式碼，包括`Startup.ConfigureServices`完整的範例，請參閱將[權杖傳遞給伺服器Blazor端應用程式](https://github.com/javiercn/blazor-server-aad-sample)。</span><span class="sxs-lookup"><span data-stu-id="b843a-107">For sample code, including a complete `Startup.ConfigureServices` example, see the [Passing tokens to a server-side Blazor application](https://github.com/javiercn/blazor-server-aad-sample).</span></span>

<span data-ttu-id="b843a-108">驗證服務器Blazor應用程式的方式與一般 RAZOR PAGES 或 MVC 應用程式一樣。</span><span class="sxs-lookup"><span data-stu-id="b843a-108">Authenticate the Blazor Server app as you would with a regular Razor Pages or MVC app.</span></span> <span data-ttu-id="b843a-109">布建權杖並將其儲存至驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="b843a-109">Provision and save the tokens to the authentication cookie.</span></span> <span data-ttu-id="b843a-110">例如：</span><span class="sxs-lookup"><span data-stu-id="b843a-110">For example:</span></span>

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

<span data-ttu-id="b843a-111">定義一個類別，以使用存取和重新整理權杖來傳入初始應用程式狀態：</span><span class="sxs-lookup"><span data-stu-id="b843a-111">Define a class to pass in the initial app state with the access and refresh tokens:</span></span>

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="b843a-112">定義可**scoped**在Blazor應用程式內使用的範圍權杖提供者服務，以從相依性[插入（DI）](xref:blazor/dependency-injection)解析權杖：</span><span class="sxs-lookup"><span data-stu-id="b843a-112">Define a **scoped** token provider service that can be used within the Blazor app to resolve the tokens from [dependency injection (DI)](xref:blazor/dependency-injection):</span></span>

```csharp
public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="b843a-113">在`Startup.ConfigureServices`中，為新增服務：</span><span class="sxs-lookup"><span data-stu-id="b843a-113">In `Startup.ConfigureServices`, add services for:</span></span>

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

<span data-ttu-id="b843a-114">在 *_Host 的 cshtml*檔案中，建立和實例， `InitialApplicationState`並將它當做參數傳遞給應用程式：</span><span class="sxs-lookup"><span data-stu-id="b843a-114">In the *_Host.cshtml* file, create and instance of `InitialApplicationState` and pass it as a parameter to the app:</span></span>

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

<span data-ttu-id="b843a-115">在`App`元件（*razor*）中，解析服務，並使用參數中的資料進行初始化：</span><span class="sxs-lookup"><span data-stu-id="b843a-115">In the `App` component (*App.razor*), resolve the service and initialize it with the data from the parameter:</span></span>

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

<span data-ttu-id="b843a-116">在提出安全 API 要求的服務中，插入權杖提供者，並取出權杖以呼叫 API：</span><span class="sxs-lookup"><span data-stu-id="b843a-116">In the service that makes a secure API request, inject the token provider and retrieve the token to call the API:</span></span>

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
