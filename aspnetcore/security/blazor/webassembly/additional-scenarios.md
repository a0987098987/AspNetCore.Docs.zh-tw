---
title: ASP.NET Core Blazor WebAssembly 其他安全性案例
author: guardrex
description: 瞭解如何設定 Blazor WebAssembly 以進行其他安全性案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: 52ca2cc3187eceb318f6eb38189ed7f408d5a61b
ms.sourcegitcommit: b0062f29cba2e5c21b95cf89eaf435ba830d11a3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84776406"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>ASP.NET Core Blazor WebAssembly 其他安全性案例

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

## <a name="attach-tokens-to-outgoing-requests"></a>將權杖附加到連出要求

此 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> 服務可與搭配使用 <xref:System.Net.Http.HttpClient> ，將存取權杖附加至傳出要求。 您可以使用現有的服務來取得權杖 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.IAccessTokenProvider> 。 如果無法取得權杖， <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException> 就會擲回。 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException>具有 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException.Redirect%2A> 方法，可以用來將使用者導覽至識別提供者，以取得新的權杖。 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler>可以使用方法，透過授權的 url、範圍和傳回 URL 來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> 。

使用下列其中一種方法來設定連出要求的訊息處理常式：

* [自訂 AuthorizationMessageHandler 類別](#custom-authorizationmessagehandler-class)（*建議*）
* [設定 AuthorizationMessageHandler](#configure-authorizationmessagehandler)

### <a name="custom-authorizationmessagehandler-class"></a>自訂 AuthorizationMessageHandler 類別

在下列範例中，自訂類別 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> 會擴充，可用於設定 <xref:System.Net.Http.HttpClient> ：

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class CustomAuthorizationMessageHandler : AuthorizationMessageHandler
{
    public CustomAuthorizationMessageHandler(IAccessTokenProvider provider, 
        NavigationManager navigationManager)
        : base(provider, navigationManager)
    {
        ConfigureHandler(
            authorizedUrls: new[] { "https://www.example.com/base" },
            scopes: new[] { "example.read", "example.write" });
    }
}
```

在 `Program.Main` （*Program.cs*）中， <xref:System.Net.Http.HttpClient> 會使用自訂授權訊息處理常式來設定：

```csharp
builder.Services.AddTransient<CustomAuthorizationMessageHandler>();

builder.Services.AddHttpClient("ServerAPI",
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
        .AddHttpMessageHandler<CustomAuthorizationMessageHandler>();
```

已設定的 <xref:System.Net.Http.HttpClient> 會用來透過[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)模式提出授權的要求。 使用建立用戶端的位置 <xref:System.Net.Http.IHttpClientFactory.CreateClient%2A> （[Microsoft Extensions. Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/)套件），會在對 <xref:System.Net.Http.HttpClient> 伺服器 API 提出要求時，提供包含存取權杖的實例：

```razor
@inject IHttpClientFactory ClientFactory

...

@code {
    private ExampleType[] examples;

    protected override async Task OnInitializedAsync()
    {
        try
        {
            var client = ClientFactory.CreateClient("ServerAPI");

            examples = 
                await client.GetFromJsonAsync<ExampleType[]>("{API METHOD}");

            ...
        }
        catch (AccessTokenNotAvailableException exception)
        {
            exception.Redirect();
        }
        
    }
}
```

### <a name="configure-authorizationmessagehandler"></a>設定 AuthorizationMessageHandler

在下列範例中，會 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> <xref:System.Net.Http.HttpClient> 在 `Program.Main` （*Program.cs*）中設定：

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddTransient(sp =>
{
    return new HttpClient(sp.GetRequiredService<AuthorizationMessageHandler>()
        .ConfigureHandler(
            authorizedUrls: new [] { "https://www.example.com/base" },
            scopes: new[] { "example.read", "example.write" }))
        {
            BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
        };
});
```

為了方便起見， <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.BaseAddressAuthorizationMessageHandler> 會包含以應用程式基底位址預先設定為授權 URL 的。 已啟用驗證的 Blazor WebAssembly 範本現在會 <xref:System.Net.Http.IHttpClientFactory> 在伺服器 API 專案中使用（[Microsoft Extensions. Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/)套件），以設定 <xref:System.Net.Http.HttpClient> 具有下列專案的 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.BaseAddressAuthorizationMessageHandler> ：

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddHttpClient("ServerAPI", 
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
        .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("ServerAPI"));
```

在上述範例中，會使用建立用戶端，而在對 <xref:System.Net.Http.IHttpClientFactory.CreateClient%2A> <xref:System.Net.Http.HttpClient> 伺服器專案提出要求時，會提供包含存取權杖的實例。

已設定的 <xref:System.Net.Http.HttpClient> 會用來透過[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)模式提出授權的要求：

```razor
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject HttpClient Client

...

protected override async Task OnInitializedAsync()
{
    private ExampleType[] examples;

    try
    {
        examples = 
            await Client.GetFromJsonAsync<ExampleType[]>("{API METHOD}");

        ...
    }
    catch (AccessTokenNotAvailableException exception)
    {
        exception.Redirect();
    }
}
```

## <a name="typed-httpclient"></a>具類型的 HttpClient

您可以定義具型別用戶端，以處理單一類別內的所有 HTTP 和權杖取得顧慮。

*WeatherForecastClient.cs*：

```csharp
using System.Net.Http;
using System.Net.Http.Json;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using static {APP ASSEMBLY}.Data;

public class WeatherForecastClient
{
    private readonly HttpClient client;
 
    public WeatherForecastClient(HttpClient client)
    {
        this.client = client;
    }
 
    public async Task<WeatherForecast[]> GetForecastAsync()
    {
        var forecasts = new WeatherForecast[0];

        try
        {
            forecasts = await client.GetFromJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        catch (AccessTokenNotAvailableException exception)
        {
            exception.Redirect();
        }

        return forecasts;
    }
}
```

預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱（例如， `using static BlazorSample.Data;` ）。

`Program.Main`（*Program.cs*）：

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddHttpClient<WeatherForecastClient>(
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();
```

`FetchData`元件（*Pages/FetchData. razor*）：

```razor
@inject WeatherForecastClient Client

...

protected override async Task OnInitializedAsync()
{
    forecasts = await Client.GetForecastAsync();
}
```

## <a name="configure-the-httpclient-handler"></a>設定 HttpClient 處理常式

您可以 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> 針對輸出 HTTP 要求進一步設定處理常式。

`Program.Main`（*Program.cs*）：

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => client.BaseAddress = new Uri("https://www.example.com/base"))
    .AddHttpMessageHandler(sp => sp.GetRequiredService<AuthorizationMessageHandler>()
    .ConfigureHandler(new [] { "https://www.example.com/base" },
        scopes: new[] { "example.read", "example.write" }));
```

## <a name="unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client"></a>在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求

如果 Blazor WebAssembly 應用程式通常會使用安全的預設值 <xref:System.Net.Http.HttpClient> ，應用程式也可以藉由設定名為的來進行未經驗證或未經授權的 Web API 要求 <xref:System.Net.Http.HttpClient> ：

`Program.Main`（*Program.cs*）：

```csharp
builder.Services.AddHttpClient("ServerAPI.NoAuthenticationClient", 
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

先前的註冊是除了現有的安全預設註冊之外 <xref:System.Net.Http.HttpClient> 。

元件會 <xref:System.Net.Http.HttpClient> 從 <xref:System.Net.Http.IHttpClientFactory> （[Microsoft Extensions. Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/)封裝）建立，以提出未經驗證或未經授權的要求：

```razor
@inject IHttpClientFactory ClientFactory

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var client = ClientFactory.CreateClient("ServerAPI.NoAuthenticationClient");

        forecasts = await client.GetFromJsonAsync<WeatherForecast[]>(
            "WeatherForecastNoAuthentication");
    }
}
```

> [!NOTE]
> 先前範例的伺服器 API 中的控制器 `WeatherForecastNoAuthenticationController` 不會以 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性標記。

## <a name="request-additional-access-tokens"></a>要求其他存取權杖

呼叫可以手動取得存取權杖 `IAccessTokenProvider.RequestAccessToken` 。

在下列範例中，應用程式需要額外的 Azure Active Directory （AAD） Microsoft Graph API 範圍，才能讀取使用者資料和傳送郵件。 在 Azure AAD 入口網站中新增 Microsoft Graph API 許可權之後，用戶端應用程式中會設定額外的範圍。

`Program.Main`（*Program.cs*）：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...

    options.ProviderOptions.AdditionalScopesToConsent.Add(
        "https://graph.microsoft.com/Mail.Send");
    options.ProviderOptions.AdditionalScopesToConsent.Add(
        "https://graph.microsoft.com/User.Read");
}
```

`IAccessTokenProvider.RequestToken`方法提供多載，可讓應用程式使用一組指定的範圍來布建存取權杖。

在 Razor 元件中：

```razor
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject IAccessTokenProvider TokenProvider

...

var tokenResult = await TokenProvider.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });

if (tokenResult.TryGetToken(out var token))
{
    ...
}
```

<xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenResult.TryGetToken%2A?displayProperty=nameWithType>傳回

* `true`包含 `token` 使用的。
* `false`如果未抓取權杖，則為。

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a>具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage

在 WebAssembly 應用程式的 WebAssembly 上執行時 Blazor ， [HttpClient](xref:fundamentals/http-requests)和 <xref:System.Net.Http.HttpRequestMessage> 可以用來自訂要求。 例如，您可以指定 HTTP 方法和要求標頭。 下列元件會對 `POST` 伺服器上的 To Do LIST API 端點提出要求，並顯示回應主體：

```razor
@page "/todorequest"
@using System.Net.Http
@using System.Net.Http.Headers
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject HttpClient Http
@inject IAccessTokenProvider TokenProvider

<h1>ToDo Request</h1>

<button @onclick="PostRequest">Submit POST request</button>

<p>Response body returned by the server:</p>

<p>@_responseBody</p>

@code {
    private string _responseBody;

    private async Task PostRequest()
    {
        var requestMessage = new HttpRequestMessage()
        {
            Method = new HttpMethod("POST"),
            RequestUri = new Uri("https://localhost:10000/api/TodoItems"),
            Content =
                JsonContent.Create(new TodoItem
                {
                    Name = "My New Todo Item",
                    IsComplete = false
                })
        };

        var tokenResult = await TokenProvider.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            requestMessage.Headers.Authorization =
                new AuthenticationHeaderValue("Bearer", token.Value);

            requestMessage.Content.Headers.TryAddWithoutValidation(
                "x-custom-header", "value");

            var response = await Http.SendAsync(requestMessage);
            var responseStatusCode = response.StatusCode;

            _responseBody = await response.Content.ReadAsStringAsync();
        }
    }

    public class TodoItem
    {
        public long Id { get; set; }
        public string Name { get; set; }
        public bool IsComplete { get; set; }
    }
}
```

.NET WebAssembly 的執行會 <xref:System.Net.Http.HttpClient> 使用[WindowOrWorkerGlobalScope。 fetch （）](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch)。 Fetch 可讓您設定數個[要求特有的選項](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。 

您可以使用 <xref:System.Net.Http.HttpRequestMessage> 下表所示的擴充方法來設定 HTTP 提取要求選項。

| 擴充方法 | Fetch 要求屬性 |
| --- | --- |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> | [憑證](https://developer.mozilla.org/docs/Web/API/Request/credentials) |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCache%2A> | [高速](https://developer.mozilla.org/docs/Web/API/Request/cache) |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestMode%2A> | [mode](https://developer.mozilla.org/docs/Web/API/Request/mode) |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestIntegrity%2A> | [完整性](https://developer.mozilla.org/docs/Web/API/Request/integrity) |

您可以使用更泛型的擴充方法來設定其他選項 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestOption%2A> 。
 
HTTP 回應通常會在 Blazor WebAssembly 應用程式中進行緩衝處理，以啟用回應內容的同步讀取支援。 若要啟用回應串流的支援，請 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserResponseStreamingEnabled%2A> 在要求上使用擴充方法。

若要在跨原始來源要求中包含認證，請使用 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> 擴充方法：

```csharp
requestMessage.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
```

如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。

在 CORS 要求上傳送認證（授權 cookie/標頭）時， `Authorization` cors 原則必須允許標頭。

下列原則包含的設定：

* 要求來源（ `http://localhost:5000` 、 `https://localhost:5001` ）。
* Any 方法（動詞）。
* `Content-Type`和 `Authorization` 標頭。 若要允許自訂標頭（例如 `x-custom-header` ），請在呼叫時列出標頭 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 。
* 用戶端 JavaScript 程式碼（ `credentials` 屬性設為）所設定的認證 `include` 。

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

如需詳細資訊，請參閱 <xref:security/cors> 和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。

## <a name="handle-token-request-errors"></a>處理權杖要求錯誤

當單一頁面應用程式（SPA）使用 Open ID Connect （OIDC）來驗證使用者時，驗證狀態會在 SPA 和 Identity 提供者（IP）中的本機維護，其格式為使用者提供其認證所設定的會話 cookie。

IP 為使用者發出的權杖通常會在短時間內有效，大約一小時，因此用戶端應用程式必須定期提取新的權杖。 否則，在授與的權杖過期之後，使用者會被登出。 在大多數情況下，OIDC 用戶端可以布建新的權杖，而不需要使用者重新驗證，因為它會保留在 IP 內的驗證狀態或「會話」。

在某些情況下，用戶端無法在沒有使用者互動的情況下取得權杖，例如，基於某些原因，使用者明確地登出了 IP。 如果使用者造訪和登出，就會發生這種情況 `https://login.microsoftonline.com` 。在這些情況下，應用程式不會立即得知使用者是否已登出。用戶端持有的任何權杖可能不再有效。 此外，用戶端無法在目前權杖過期之後，不需要使用者互動就布建新權杖。

這些案例並不是以權杖為基礎的驗證所特有。 它們屬於 Spa 的本質。 如果移除驗證 cookie，使用 cookie 的 SPA 也無法呼叫伺服器 API。

當應用程式對受保護的資源執行 API 呼叫時，您必須注意下列事項：

* 若要布建新的存取權杖以呼叫 API，使用者可能需要再次進行驗證。
* 即使用戶端的權杖看似有效，對伺服器的呼叫可能會失敗，因為使用者已撤銷權杖。

當應用程式要求權杖時，會有兩種可能的結果：

* 要求成功，且應用程式具有有效的權杖。
* 要求失敗，且應用程式必須再次驗證使用者，才能取得新的權杖。

當令牌要求失敗時，您必須決定是否要在執行重新導向之前，先儲存任何目前的狀態。 有數種方法存在，並增加複雜性層級：

* 將目前的頁面狀態儲存在會話儲存體中。 在[OnInitializedAsync 生命週期事件](xref:blazor/lifecycle#component-initialization-methods)（ <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> ）期間，檢查是否可以還原狀態，再繼續進行。
* 新增查詢字串參數，並使用它來通知應用程式它需要重新序列化先前儲存的狀態。
* 新增具有唯一識別碼的查詢字串參數，以將資料儲存在會話儲存體中，而不會有風險與其他專案衝突。

下列範例示範如何執行：

* 在重新導向至登入頁面之前保留狀態。
* 使用查詢字串參數，在驗證之後復原先前的狀態。

```razor
<EditForm Model="User" @onsubmit="OnSaveAsync">
    <label>User
        <InputText @bind-Value="User.Name" />
    </label>
    <label>Last name
        <InputText @bind-Value="User.LastName" />
    </label>
</EditForm>

@code {
    public class Profile
    {
        public string Name { get; set; }
        public string LastName { get; set; }
    }

    public Profile User { get; set; } = new Profile();

    protected async override Task OnInitializedAsync()
    {
        var currentQuery = new Uri(Navigation.Uri).Query;

        if (currentQuery.Contains("state=resumeSavingProfile"))
        {
            User = await JS.InvokeAsync<Profile>("sessionStorage.getState", 
                "resumeSavingProfile");
        }
    }

    public async Task OnSaveAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var resumeUri = Navigation.Uri + $"?state=resumeSavingProfile";

        var tokenResult = await AuthenticationService.RequestAccessToken(
            new AccessTokenRequestOptions
            {
                ReturnUrl = resumeUri
            });

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            await httpClient.PostAsJsonAsync("Save", User);
        }
        else
        {
            await JS.InvokeVoidAsync("sessionStorage.setState", 
                "resumeSavingProfile", User);
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }
    }
}
```

## <a name="save-app-state-before-an-authentication-operation"></a>在驗證操作之前儲存應用程式狀態

在驗證作業期間，某些情況下，您會想要在瀏覽器重新導向至 IP 之前，先儲存應用程式狀態。 當您使用狀態容器，而且想要在驗證成功之後還原狀態時，就可能發生這種情況。 您可以使用自訂驗證狀態物件來保留應用程式特定狀態或其參考，並在驗證作業成功完成後還原該狀態。 下列範例示範方法。

狀態容器類別是在應用程式中建立，其中具有屬性來保存應用程式的狀態值。 在下列範例中，容器是用來維護預設範本 `Counter` 元件（*Pages/counter razor*）的計數器值。 序列化和還原序列化容器的方法是以為基礎 <xref:System.Text.Json> 。

```csharp
using System.Text.Json;

public class StateContainer
{
    public int CounterValue { get; set; }

    public string GetStateForLocalStorage()
    {
        return JsonSerializer.Serialize(this);
    }

    public void SetStateFromLocalStorage(string locallyStoredState)
    {
        var deserializedState = 
            JsonSerializer.Deserialize<StateContainer>(locallyStoredState);

        CounterValue = deserializedState.CounterValue;
    }
}
```

`Counter`元件會使用狀態容器來維護 `currentCount` 元件以外的值：

```razor
@page "/counter"
@inject StateContainer State

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    protected override void OnInitialized()
    {
        if (State.CounterValue > 0)
        {
            currentCount = State.CounterValue;
        }
    }

    private void IncrementCount()
    {
        currentCount++;
        State.CounterValue = currentCount;
    }
}
```

從建立 `ApplicationAuthenticationState` <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticationState> 。 提供 `Id` 屬性，做為本機儲存狀態的識別碼。

*ApplicationAuthenticationState.cs*：

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class ApplicationAuthenticationState : RemoteAuthenticationState
{
    public string Id { get; set; }
}
```

此 `Authentication` 元件（*Pages/Authentication razor*）會使用本機會話儲存體搭配序列化和還原序列化方法，來儲存及還原應用程式的狀態 `StateContainer` ， `GetStateForLocalStorage` 以及 `SetStateFromLocalStorage` ：

```razor
@page "/authentication/{action}"
@inject IJSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action"
                             TAuthenticationState="ApplicationAuthenticationState"
                             AuthenticationState="AuthenticationState"
                             OnLogInSucceeded="RestoreState"
                             OnLogOutSucceeded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public ApplicationAuthenticationState AuthenticationState { get; set; } =
        new ApplicationAuthenticationState();

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn,
            Action) ||
            RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogOut,
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();

            await JS.InvokeVoidAsync("sessionStorage.setItem",
                AuthenticationState.Id, State.GetStateForLocalStorage());
        }
    }

    private async Task RestoreState(ApplicationAuthenticationState state)
    {
        if (state.Id != null)
        {
            var locallyStoredState = await JS.InvokeAsync<string>(
                "sessionStorage.getItem", state.Id);

            if (locallyStoredState != null)
            {
                State.SetStateFromLocalStorage(locallyStoredState);
                await JS.InvokeVoidAsync("sessionStorage.removeItem", state.Id);
            }
        }
    }
}
```

這個範例會使用 Azure Active Directory （AAD）進行驗證。 在 `Program.Main` （*Program.cs*）中：

* `ApplicationAuthenticationState`已設定為 Microsoft 驗證 Library （MSAL） `RemoteAuthenticationState` 類型。
* 狀態容器會在服務容器中註冊。

```csharp
builder.Services.AddMsalAuthentication<ApplicationAuthenticationState>(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
});

builder.Services.AddSingleton<StateContainer>();
```

## <a name="customize-app-routes"></a>自訂應用程式路由

根據預設， [AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)會使用下表所示的路由來代表不同的驗證狀態。

| 路由                            | 目的 |
| -------------------------------- | ------- |
| `authentication/login`           | 觸發登入作業。 |
| `authentication/login-callback`  | 處理任何登入作業的結果。 |
| `authentication/login-failed`    | 當登入作業因某些原因而失敗時，會顯示錯誤訊息。 |
| `authentication/logout`          | 觸發登出作業。 |
| `authentication/logout-callback` | 處理登出作業的結果。 |
| `authentication/logout-failed`   | 當登出作業因某些原因而失敗時，會顯示錯誤訊息。 |
| `authentication/logged-out`      | 表示使用者已成功登出。 |
| `authentication/profile`         | 觸發操作以編輯使用者設定檔。 |
| `authentication/register`        | 觸發操作以註冊新的使用者。 |

上表中顯示的路由可透過來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticationOptions%601.AuthenticationPaths%2A?displayProperty=nameWithType> 。 設定選項以提供自訂路由時，請確認應用程式具有處理每個路徑的路由。

在下列範例中，所有路徑的前面都會加上 `/security` 。

`Authentication`元件（*Pages/Authentication. razor*）：

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main`（*Program.cs*）：

```csharp
builder.Services.AddApiAuthorization(options => { 
    options.AuthenticationPaths.LogInPath = "security/login";
    options.AuthenticationPaths.LogInCallbackPath = "security/login-callback";
    options.AuthenticationPaths.LogInFailedPath = "security/login-failed";
    options.AuthenticationPaths.LogOutPath = "security/logout";
    options.AuthenticationPaths.LogOutCallbackPath = "security/logout-callback";
    options.AuthenticationPaths.LogOutFailedPath = "security/logout-failed";
    options.AuthenticationPaths.LogOutSucceededPath = "security/logged-out";
    options.AuthenticationPaths.ProfilePath = "security/profile";
    options.AuthenticationPaths.RegisterPath = "security/register";
});
```

如果需求會呼叫完全不同的路徑，請如先前所述設定路由，並 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> 使用明確的 action 參數來呈現：

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

如果您選擇這樣做，則可以將 UI 分成不同的頁面。

## <a name="customize-the-authentication-user-interface"></a>自訂驗證使用者介面

<xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView>針對每個驗證狀態包含一組預設的 UI 元件。 您可以藉由傳入自訂來自訂每個狀態 <xref:Microsoft.AspNetCore.Components.RenderFragment> 。 若要在初始登入程式期間自訂顯示的文字，可以變更，如下所示 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> 。

`Authentication`元件（*Pages/Authentication. razor*）：

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action">
    <LoggingIn>
        You are about to be redirected to https://login.microsoftonline.com.
    </LoggingIn>
</RemoteAuthenticatorView>

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView>有一個片段，可用於下表所示的每個驗證路由。

| 路由                            | 片段                |
| -------------------------------- | ----------------------- |
| `authentication/login`           | `<LoggingIn>`           |
| `authentication/login-callback`  | `<CompletingLoggingIn>` |
| `authentication/login-failed`    | `<LogInFailed>`         |
| `authentication/logout`          | `<LogOut>`              |
| `authentication/logout-callback` | `<CompletingLogOut>`    |
| `authentication/logout-failed`   | `<LogOutFailed>`        |
| `authentication/logged-out`      | `<LogOutSucceeded>`     |
| `authentication/profile`         | `<UserProfile>`         |
| `authentication/register`        | `<Registering>`         |

## <a name="customize-the-user"></a>自訂使用者

可以自訂系結至應用程式的使用者。 在下列範例中，所有已驗證的使用者都會收到 `amr` 每個使用者驗證方法的宣告。

建立擴充類別的類別 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteUserAccount> ：

```csharp
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class CustomUserAccount : RemoteUserAccount
{
    [JsonPropertyName("amr")]
    public string[] AuthenticationMethod { get; set; }
}
```

建立可延伸的 factory <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccountClaimsPrincipalFactory%601> ：

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;

public class CustomAccountFactory 
    : AccountClaimsPrincipalFactory<CustomUserAccount>
{
    public CustomAccountFactory(NavigationManager navigationManager, 
        IAccessTokenProviderAccessor accessor) : base(accessor)
    {
    }
  
    public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
        CustomUserAccount account, RemoteAuthenticationUserOptions options)
    {
        var initialUser = await base.CreateUserAsync(account, options);
        
        if (initialUser.Identity.IsAuthenticated)
        {
            foreach (var value in account.AuthenticationMethod)
            {
                ((ClaimsIdentity)initialUser.Identity)
                    .AddClaim(new Claim("amr", value));
            }
        }
           
        return initialUser;
    }
}
```

`CustomAccountFactory`為使用中的驗證提供者註冊。 下列任何一項註冊都是有效的： 

* <xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A>:

  ```csharp
  using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

  ...

  builder.Services.AddOidcAuthentication<RemoteAuthenticationState, 
      CustomUserAccount>(options =>
  {
      ...
  })
  .AddAccountClaimsPrincipalFactory<RemoteAuthenticationState, 
      CustomUserAccount, CustomAccountFactory>();
  ```

* <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>:

  ```csharp
  using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

  ...

  builder.Services.AddMsalAuthentication<RemoteAuthenticationState, 
      CustomUserAccount>(options =>
  {
      ...
  })
  .AddAccountClaimsPrincipalFactory<RemoteAuthenticationState, 
      CustomUserAccount, CustomAccountFactory>();
  ```
  
* <xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddApiAuthorization%2A>:

  ```csharp
  using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

  ...

  builder.Services.AddApiAuthorization<RemoteAuthenticationState, 
      CustomUserAccount>(options =>
  {
      ...
  })
  .AddAccountClaimsPrincipalFactory<RemoteAuthenticationState, 
      CustomUserAccount, CustomAccountFactory>();
  ```

## <a name="support-prerendering-with-authentication"></a>支援使用驗證來進行預呈現

遵循其中一個託管 WebAssembly 應用程式主題中的指導方針之後 Blazor ，請使用下列指示來建立應用程式：

* 不需要授權的 Prerenders 路徑。
* 不需要授權的已呈現路徑。

在用戶端應用程式的 `Program` 類別（*Program.cs*）中，將常見的服務註冊因素劃分為不同的方法（例如， `ConfigureCommonServices` ）：

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.RootComponents.Add<App>("app");

        builder.Services.AddTransient(new HttpClient 
        {
            BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
        });

        services.Add...;

        ConfigureCommonServices(builder.Services);

        await builder.Build().RunAsync();
    }

    public static void ConfigureCommonServices(IServiceCollection services)
    {
        // Common service registrations
    }
}
```

在伺服器應用程式的中 `Startup.ConfigureServices` ，註冊下列其他服務：

```csharp
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Server;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddRazorPages();
    services.AddScoped<AuthenticationStateProvider, 
        ServerAuthenticationStateProvider>();
    services.AddScoped<SignOutSessionStateManager>();

    Client.Program.ConfigureCommonServices(services);
}
```

在伺服器應用程式的 `Startup.Configure` 方法中，取代[端點。具有端點的 MapFallbackToFile （"index.html"）](xref:Microsoft.AspNetCore.Builder.StaticFilesEndpointRouteBuilderExtensions.MapFallbackToFile%2A) [。MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A)：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

在伺服器應用程式中，建立*Pages*資料夾（如果不存在）。 在伺服器應用程式的*Pages*資料夾內建立 *_Host 的 cshtml*頁面。 將用戶端應用程式的*wwwroot/index.html*檔案中的內容貼到*Pages/_Host. cshtml*檔案中。 更新檔案的內容：

* 將 `@page "_Host"` 新增到檔案的頂端。
* 將標記取代為 `<app>Loading...</app>` 下列內容：

  ```cshtml
  <app>
      @if (!HttpContext.Request.Path.StartsWithSegments("/authentication"))
      {
          <component type="typeof(Wasm.Authentication.Client.App)" 
              render-mode="Static" />
      }
      else
      {
          <text>Loading...</text>
      }
  </app>
  ```
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a>託管應用程式和協力廠商登入提供者的選項

Blazor使用協力廠商提供者驗證和授權託管 WebAssembly 應用程式時，有數個選項可用來驗證使用者。 您選擇哪一個取決於您的案例。

如需詳細資訊，請參閱 <xref:security/authentication/social/additional-claims> 。

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a>驗證使用者只呼叫受保護的協力廠商 Api

對協力廠商 API 提供者的用戶端 OAuth 流程驗證使用者：

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 在此情節中：

* 裝載應用程式的伺服器不扮演角色。
* 無法保護伺服器上的 Api。
* 應用程式只能呼叫受保護的協力廠商 Api。

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a>使用協力廠商提供者來驗證使用者，並在主機伺服器和協力廠商上呼叫受保護的 Api

Identity使用協力廠商登入提供者進行設定。 取得協力廠商 API 存取所需的權杖，並加以儲存。

當使用者登入時，會 Identity 收集存取權並重新整理權杖，做為驗證程式的一部分。 此時，有幾個方法可用來對協力廠商 Api 進行 API 呼叫。

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a>使用伺服器存取權杖來取出協力廠商存取權杖

使用伺服器上產生的存取權杖，從伺服器 API 端點抓取協力廠商存取權杖。 從該處，使用協力廠商存取權杖，直接從用戶端上呼叫協力廠商 API 資源 Identity 。

我們不建議採用這種方法。 這種方法需要將協力廠商存取權杖視為針對公用用戶端所產生。 在 OAuth 詞彙中，公用應用程式不會有用戶端密碼，因為它無法受信任而無法安全地儲存秘密，而且會為機密用戶端產生存取權杖。 機密用戶端是具有用戶端密碼的用戶端，並假設能夠安全地儲存秘密。

* 協力廠商存取權杖可能會被授與額外的範圍來執行敏感性作業，這是根據協力廠商針對較受信任的用戶端發出權杖的事實。
* 同樣地，重新整理權杖不應發給不受信任的用戶端，因為這樣做會讓用戶端無限制存取，除非有其他限制。

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a>從用戶端對伺服器 API 進行 API 呼叫，以便呼叫協力廠商 Api

從用戶端對伺服器 API 進行 API 呼叫。 從伺服器中，取出協力廠商 API 資源的存取權杖，併發出任何需要的呼叫。

雖然這種方法需要透過伺服器額外的網路躍點來呼叫協力廠商 API，但最終會導致更安全的體驗：

* 伺服器可以儲存重新整理權杖，並確保應用程式不會失去協力廠商資源的存取權。
* 應用程式無法從可能包含更多敏感性許可權的伺服器洩漏存取權杖。

## <a name="use-open-id-connect-oidc-v20-endpoints"></a>使用 Open ID Connect （OIDC） v2.0 端點

驗證程式庫和 Blazor 範本會使用 OPEN ID Connect （OIDC） v1.0 端點。 若要使用 v2.0 端點，請設定 JWT 持有人 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority?displayProperty=nameWithType> 選項。 在下列範例中，會將區段附加至屬性，以針對 v2.0 設定 AAD `v2.0` <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority> ：

```csharp
builder.Services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, 
    options =>
    {
        options.Authority += "/v2.0";
    });
```

或者，您也可以在應用程式設定（*appsettings.js開啟*）檔案中進行設定：

```json
{
  "Local": {
    "Authority": "https://login.microsoftonline.com/common/oauth2/v2.0/",
    ...
  }
}
```

如果在區段上對授權單位的追蹤不適合應用程式的 OIDC 提供者（例如使用非 AAD 提供者），請 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> 直接設定屬性。 請 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> 使用金鑰在應用程式佈建檔案（*appsettings.js*中）設定或中的屬性 `Authority` 。

針對 v2.0 端點，識別碼權杖中的宣告清單會變更。 如需詳細資訊，請參閱[為何要更新至 Microsoft 身分識別平臺（v2.0）？](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison)。
