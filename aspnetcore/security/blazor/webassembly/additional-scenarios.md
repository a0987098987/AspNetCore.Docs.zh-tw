---
<span data-ttu-id="a921e-101">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-101">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-103">'Blazor'</span></span>
- <span data-ttu-id="a921e-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-104">'Identity'</span></span>
- <span data-ttu-id="a921e-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-106">'Razor'</span></span>
- <span data-ttu-id="a921e-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="a921e-108">ASP.NET Core Blazor WebAssembly 其他安全性案例</span><span class="sxs-lookup"><span data-stu-id="a921e-108">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="a921e-109">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="a921e-109">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

## <a name="attach-tokens-to-outgoing-requests"></a><span data-ttu-id="a921e-110">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="a921e-110">Attach tokens to outgoing requests</span></span>

<span data-ttu-id="a921e-111">此 `AuthorizationMessageHandler` 服務可與搭配使用 `HttpClient` ，將存取權杖附加至傳出要求。</span><span class="sxs-lookup"><span data-stu-id="a921e-111">The `AuthorizationMessageHandler` service can be used with `HttpClient` to attach access tokens to outgoing requests.</span></span> <span data-ttu-id="a921e-112">您可以使用現有的服務來取得權杖 `IAccessTokenProvider` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-112">Tokens are acquired using the existing `IAccessTokenProvider` service.</span></span> <span data-ttu-id="a921e-113">如果無法取得權杖， `AccessTokenNotAvailableException` 就會擲回。</span><span class="sxs-lookup"><span data-stu-id="a921e-113">If a token can't be acquired, an `AccessTokenNotAvailableException` is thrown.</span></span> <span data-ttu-id="a921e-114">`AccessTokenNotAvailableException`具有 `Redirect` 方法，可以用來將使用者導覽至識別提供者，以取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-114">`AccessTokenNotAvailableException` has a `Redirect` method that can be used to navigate the user to the identity provider to acquire a new token.</span></span> <span data-ttu-id="a921e-115">`AuthorizationMessageHandler`可以使用方法，透過授權的 url、範圍和傳回 URL 來設定 `ConfigureHandler` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-115">The `AuthorizationMessageHandler` can be configured with the authorized URLs, scopes, and return URL using the `ConfigureHandler` method.</span></span>

<span data-ttu-id="a921e-116">在下列範例中，會 `AuthorizationMessageHandler` `HttpClient` 在 `Program.Main` （*Program.cs*）中設定：</span><span class="sxs-lookup"><span data-stu-id="a921e-116">In the following example, `AuthorizationMessageHandler` configures an `HttpClient` in `Program.Main` (*Program.cs*):</span></span>

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddTransient(sp =>
{
    return new HttpClient(sp.GetRequiredService<AuthorizationMessageHandler>()
        .ConfigureHandler(
            new [] { "https://www.example.com/base" },
            scopes: new[] { "example.read", "example.write" }))
        {
            BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
        };
});
```

<span data-ttu-id="a921e-117">為了方便起見， `BaseAddressAuthorizationMessageHandler` 會包含以應用程式基底位址預先設定為授權 URL 的。</span><span class="sxs-lookup"><span data-stu-id="a921e-117">For convenience, a `BaseAddressAuthorizationMessageHandler` is included that's preconfigured with the app base address as an authorized URL.</span></span> <span data-ttu-id="a921e-118">已啟用驗證的 Blazor WebAssembly 範本現在會 <xref:System.Net.Http.IHttpClientFactory> 在伺服器 API 專案中使用，以設定 <xref:System.Net.Http.HttpClient> 具有下列專案的 `BaseAddressAuthorizationMessageHandler` ：</span><span class="sxs-lookup"><span data-stu-id="a921e-118">The authentication-enabled Blazor WebAssembly templates now use <xref:System.Net.Http.IHttpClientFactory> in the Server API project to set up an <xref:System.Net.Http.HttpClient> with the `BaseAddressAuthorizationMessageHandler`:</span></span>

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddHttpClient("BlazorWithIdentity.ServerAPI", 
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
        .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("BlazorWithIdentity.ServerAPI"));
```

<span data-ttu-id="a921e-119">在上述範例中，會使用建立用戶端，而在對 `CreateClient` <xref:System.Net.Http.HttpClient> 伺服器專案提出要求時，會提供包含存取權杖的實例。</span><span class="sxs-lookup"><span data-stu-id="a921e-119">Where the client is created with `CreateClient` in the preceding example, the <xref:System.Net.Http.HttpClient> is supplied instances that include access tokens when making requests to the server project.</span></span>

<span data-ttu-id="a921e-120">接著會使用設定的，透過 <xref:System.Net.Http.HttpClient> 簡單模式來提出授權的要求 `try-catch` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-120">The configured <xref:System.Net.Http.HttpClient> is then used to make authorized requests using a simple `try-catch` pattern.</span></span>

<span data-ttu-id="a921e-121">`FetchData`元件（*Pages/FetchData. razor*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-121">`FetchData` component (*Pages/FetchData.razor*):</span></span>

```csharp
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject HttpClient Client

...

protected override async Task OnInitializedAsync()
{
    try
    {
        forecasts = 
            await Client.GetFromJsonAsync<WeatherForecast[]>("WeatherForecast");
    }
    catch (AccessTokenNotAvailableException exception)
    {
        exception.Redirect();
    }
}
```

## <a name="typed-httpclient"></a><span data-ttu-id="a921e-122">具類型的 HttpClient</span><span class="sxs-lookup"><span data-stu-id="a921e-122">Typed HttpClient</span></span>

<span data-ttu-id="a921e-123">您可以定義具型別用戶端，以處理單一類別內的所有 HTTP 和權杖取得顧慮。</span><span class="sxs-lookup"><span data-stu-id="a921e-123">A typed client can be defined that handles all of the HTTP and token acquisition concerns within a single class.</span></span>

<span data-ttu-id="a921e-124">*WeatherForecastClient.cs*：</span><span class="sxs-lookup"><span data-stu-id="a921e-124">*WeatherForecastClient.cs*:</span></span>

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

<span data-ttu-id="a921e-125">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-125">`Program.Main` (*Program.cs*):</span></span>

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddHttpClient<WeatherForecastClient>(
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();
```

<span data-ttu-id="a921e-126">`FetchData`元件（*Pages/FetchData. razor*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-126">`FetchData` component (*Pages/FetchData.razor*):</span></span>

```razor
@inject WeatherForecastClient Client

...

protected override async Task OnInitializedAsync()
{
    forecasts = await Client.GetForecastAsync();
}
```

## <a name="configure-the-httpclient-handler"></a><span data-ttu-id="a921e-127">設定 HttpClient 處理常式</span><span class="sxs-lookup"><span data-stu-id="a921e-127">Configure the HttpClient handler</span></span>

<span data-ttu-id="a921e-128">您可以 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> 針對輸出 HTTP 要求進一步設定處理常式。</span><span class="sxs-lookup"><span data-stu-id="a921e-128">The handler can be further configured with <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> for outbound HTTP requests.</span></span>

<span data-ttu-id="a921e-129">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-129">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => client.BaseAddress = new Uri("https://www.example.com/base"))
    .AddHttpMessageHandler(sp => sp.GetRequiredService<AuthorizationMessageHandler>()
    .ConfigureHandler(new [] { "https://www.example.com/base" },
        scopes: new[] { "example.read", "example.write" }));
```

## <a name="unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client"></a><span data-ttu-id="a921e-130">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="a921e-130">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>

<span data-ttu-id="a921e-131">如果 Blazor WebAssembly 應用程式通常會使用安全的預設值 <xref:System.Net.Http.HttpClient> ，應用程式也可以藉由設定名為的來進行未經驗證或未經授權的 Web API 要求 <xref:System.Net.Http.HttpClient> ：</span><span class="sxs-lookup"><span data-stu-id="a921e-131">If the Blazor WebAssembly app ordinarily uses a secure default <xref:System.Net.Http.HttpClient>, the app can also make unauthenticated or unauthorized web API requests by configuring a named <xref:System.Net.Http.HttpClient>:</span></span>

<span data-ttu-id="a921e-132">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-132">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddHttpClient("ServerAPI.NoAuthenticationClient", 
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

<span data-ttu-id="a921e-133">先前的註冊是除了現有的安全預設註冊之外 <xref:System.Net.Http.HttpClient> 。</span><span class="sxs-lookup"><span data-stu-id="a921e-133">The preceding registration is in addition to the existing secure default <xref:System.Net.Http.HttpClient> registration.</span></span>

<span data-ttu-id="a921e-134">元件會從建立， <xref:System.Net.Http.HttpClient> <xref:System.Net.Http.IHttpClientFactory> 以提出未經驗證或未經授權的要求：</span><span class="sxs-lookup"><span data-stu-id="a921e-134">A component creates the <xref:System.Net.Http.HttpClient> from the <xref:System.Net.Http.IHttpClientFactory> to make unauthenticated or unauthorized requests:</span></span>

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
> <span data-ttu-id="a921e-135">先前範例的伺服器 API 中的控制器 `WeatherForecastNoAuthenticationController` 不會以 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性標記。</span><span class="sxs-lookup"><span data-stu-id="a921e-135">The controller in the server API, `WeatherForecastNoAuthenticationController` for the preceding example, isn't marked with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute.</span></span>

## <a name="request-additional-access-tokens"></a><span data-ttu-id="a921e-136">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="a921e-136">Request additional access tokens</span></span>

<span data-ttu-id="a921e-137">呼叫可以手動取得存取權杖 `IAccessTokenProvider.RequestAccessToken` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-137">Access tokens can be manually obtained by calling `IAccessTokenProvider.RequestAccessToken`.</span></span>

<span data-ttu-id="a921e-138">在下列範例中，應用程式需要額外的 Azure Active Directory （AAD） Microsoft Graph API 範圍，才能讀取使用者資料和傳送郵件。</span><span class="sxs-lookup"><span data-stu-id="a921e-138">In the following example, additional Azure Active Directory (AAD) Microsoft Graph API scopes are required by an app to read user data and send mail.</span></span> <span data-ttu-id="a921e-139">在 Azure AAD 入口網站中新增 Microsoft Graph API 許可權之後，用戶端應用程式中會設定額外的範圍。</span><span class="sxs-lookup"><span data-stu-id="a921e-139">After adding the Microsoft Graph API permissions in the Azure AAD portal, the additional scopes are configured in the Client app.</span></span>

<span data-ttu-id="a921e-140">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-140">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="a921e-141">`IAccessTokenProvider.RequestToken`方法提供多載，可讓應用程式使用一組指定的範圍來布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-141">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision an access token with a given set of scopes.</span></span>

<span data-ttu-id="a921e-142">在 Razor 元件中：</span><span class="sxs-lookup"><span data-stu-id="a921e-142">In a Razor component:</span></span>

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

<span data-ttu-id="a921e-143">`TryGetToken`傳回</span><span class="sxs-lookup"><span data-stu-id="a921e-143">`TryGetToken` returns:</span></span>

* <span data-ttu-id="a921e-144">`true`包含 `token` 使用的。</span><span class="sxs-lookup"><span data-stu-id="a921e-144">`true` with the `token` for use.</span></span>
* <span data-ttu-id="a921e-145">`false`如果未抓取權杖，則為。</span><span class="sxs-lookup"><span data-stu-id="a921e-145">`false` if the token isn't retrieved.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="a921e-146">具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="a921e-146">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="a921e-147">在 WebAssembly 應用程式的 WebAssembly 上執行時 Blazor ， [HttpClient](xref:fundamentals/http-requests)和 <xref:System.Net.Http.HttpRequestMessage> 可以用來自訂要求。</span><span class="sxs-lookup"><span data-stu-id="a921e-147">When running on WebAssembly in a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> can be used to customize requests.</span></span> <span data-ttu-id="a921e-148">例如，您可以指定 HTTP 方法和要求標頭。</span><span class="sxs-lookup"><span data-stu-id="a921e-148">For example, you can specify the HTTP method and request headers.</span></span> <span data-ttu-id="a921e-149">下列元件會對 `POST` 伺服器上的 To Do LIST API 端點提出要求，並顯示回應主體：</span><span class="sxs-lookup"><span data-stu-id="a921e-149">The following component makes a `POST` request to a To Do List API endpoint on the server and shows the response body:</span></span>

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

<span data-ttu-id="a921e-150">.NET WebAssembly 的執行會 `HttpClient` 使用[WindowOrWorkerGlobalScope。 fetch （）](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch)。</span><span class="sxs-lookup"><span data-stu-id="a921e-150">.NET WebAssembly's implementation of `HttpClient` uses [WindowOrWorkerGlobalScope.fetch()](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch).</span></span> <span data-ttu-id="a921e-151">Fetch 可讓您設定數個[要求特有的選項](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="a921e-151">Fetch allows configuring several [request-specific options](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span> 

<span data-ttu-id="a921e-152">您可以使用 `HttpRequestMessage` 下表所示的擴充方法來設定 HTTP 提取要求選項。</span><span class="sxs-lookup"><span data-stu-id="a921e-152">HTTP fetch request options can be configured with `HttpRequestMessage` extension methods shown in the following table.</span></span>

| <span data-ttu-id="a921e-153">`HttpRequestMessage`擴充方法</span><span class="sxs-lookup"><span data-stu-id="a921e-153">`HttpRequestMessage` extension method</span></span> | <span data-ttu-id="a921e-154">Fetch 要求屬性</span><span class="sxs-lookup"><span data-stu-id="a921e-154">Fetch request property</span></span> |
| ---
<span data-ttu-id="a921e-155">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-155">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-156">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-156">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-157">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-157">'Blazor'</span></span>
- <span data-ttu-id="a921e-158">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-158">'Identity'</span></span>
- <span data-ttu-id="a921e-159">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-159">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-160">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-160">'Razor'</span></span>
- <span data-ttu-id="a921e-161">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-161">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-162">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-162">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-163">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-163">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-164">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-164">'Blazor'</span></span>
- <span data-ttu-id="a921e-165">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-165">'Identity'</span></span>
- <span data-ttu-id="a921e-166">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-166">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-167">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-167">'Razor'</span></span>
- <span data-ttu-id="a921e-168">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-168">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-169">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-169">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-170">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-170">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-171">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-171">'Blazor'</span></span>
- <span data-ttu-id="a921e-172">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-172">'Identity'</span></span>
- <span data-ttu-id="a921e-173">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-173">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-174">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-174">'Razor'</span></span>
- <span data-ttu-id="a921e-175">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-175">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-176">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-176">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-177">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-177">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-178">'Blazor'</span></span>
- <span data-ttu-id="a921e-179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-179">'Identity'</span></span>
- <span data-ttu-id="a921e-180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-180">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-181">'Razor'</span></span>
- <span data-ttu-id="a921e-182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-182">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-183">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-183">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-184">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-184">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-185">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-185">'Blazor'</span></span>
- <span data-ttu-id="a921e-186">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-186">'Identity'</span></span>
- <span data-ttu-id="a921e-187">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-187">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-188">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-188">'Razor'</span></span>
- <span data-ttu-id="a921e-189">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-189">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-190">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-190">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-191">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-191">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-192">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-192">'Blazor'</span></span>
- <span data-ttu-id="a921e-193">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-193">'Identity'</span></span>
- <span data-ttu-id="a921e-194">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-194">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-195">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-195">'Razor'</span></span>
- <span data-ttu-id="a921e-196">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-196">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-197">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-197">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-198">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-198">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-199">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-199">'Blazor'</span></span>
- <span data-ttu-id="a921e-200">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-200">'Identity'</span></span>
- <span data-ttu-id="a921e-201">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-201">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-202">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-202">'Razor'</span></span>
- <span data-ttu-id="a921e-203">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-203">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-204">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-204">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-205">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-205">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-206">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-206">'Blazor'</span></span>
- <span data-ttu-id="a921e-207">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-207">'Identity'</span></span>
- <span data-ttu-id="a921e-208">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-208">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-209">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-209">'Razor'</span></span>
- <span data-ttu-id="a921e-210">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-210">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-211">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-211">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-212">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-212">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-213">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-213">'Blazor'</span></span>
- <span data-ttu-id="a921e-214">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-214">'Identity'</span></span>
- <span data-ttu-id="a921e-215">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-215">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-216">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-216">'Razor'</span></span>
- <span data-ttu-id="a921e-217">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-217">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-218">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-218">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-219">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-219">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-220">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-220">'Blazor'</span></span>
- <span data-ttu-id="a921e-221">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-221">'Identity'</span></span>
- <span data-ttu-id="a921e-222">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-222">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-223">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-223">'Razor'</span></span>
- <span data-ttu-id="a921e-224">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-224">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-225">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-225">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-226">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-226">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-227">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-227">'Blazor'</span></span>
- <span data-ttu-id="a921e-228">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-228">'Identity'</span></span>
- <span data-ttu-id="a921e-229">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-229">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-230">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-230">'Razor'</span></span>
- <span data-ttu-id="a921e-231">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-231">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-232">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-232">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-233">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-233">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-234">'Blazor'</span></span>
- <span data-ttu-id="a921e-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-235">'Identity'</span></span>
- <span data-ttu-id="a921e-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-237">'Razor'</span></span>
- <span data-ttu-id="a921e-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-238">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-239">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-239">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-240">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-240">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-241">'Blazor'</span></span>
- <span data-ttu-id="a921e-242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-242">'Identity'</span></span>
- <span data-ttu-id="a921e-243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-243">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-244">'Razor'</span></span>
- <span data-ttu-id="a921e-245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-245">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-246">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-246">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-247">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-247">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-248">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-248">'Blazor'</span></span>
- <span data-ttu-id="a921e-249">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-249">'Identity'</span></span>
- <span data-ttu-id="a921e-250">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-250">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-251">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-251">'Razor'</span></span>
- <span data-ttu-id="a921e-252">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-252">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-253">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-253">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-254">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-254">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-255">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-255">'Blazor'</span></span>
- <span data-ttu-id="a921e-256">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-256">'Identity'</span></span>
- <span data-ttu-id="a921e-257">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-257">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-258">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-258">'Razor'</span></span>
- <span data-ttu-id="a921e-259">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-259">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-260">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-260">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-261">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-261">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-262">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-262">'Blazor'</span></span>
- <span data-ttu-id="a921e-263">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-263">'Identity'</span></span>
- <span data-ttu-id="a921e-264">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-264">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-265">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-265">'Razor'</span></span>
- <span data-ttu-id="a921e-266">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-266">'SignalR' uid:</span></span> 

<span data-ttu-id="a921e-267">------------------- |---標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-267">------------------- | --- title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-268">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-268">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-269">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-269">'Blazor'</span></span>
- <span data-ttu-id="a921e-270">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-270">'Identity'</span></span>
- <span data-ttu-id="a921e-271">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-271">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-272">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-272">'Razor'</span></span>
- <span data-ttu-id="a921e-273">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-273">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-274">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-274">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-275">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-275">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-276">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-276">'Blazor'</span></span>
- <span data-ttu-id="a921e-277">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-277">'Identity'</span></span>
- <span data-ttu-id="a921e-278">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-278">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-279">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-279">'Razor'</span></span>
- <span data-ttu-id="a921e-280">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-280">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-281">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-281">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-282">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-282">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-283">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-283">'Blazor'</span></span>
- <span data-ttu-id="a921e-284">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-284">'Identity'</span></span>
- <span data-ttu-id="a921e-285">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-285">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-286">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-286">'Razor'</span></span>
- <span data-ttu-id="a921e-287">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-287">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-288">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-288">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-289">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-289">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-290">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-290">'Blazor'</span></span>
- <span data-ttu-id="a921e-291">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-291">'Identity'</span></span>
- <span data-ttu-id="a921e-292">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-292">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-293">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-293">'Razor'</span></span>
- <span data-ttu-id="a921e-294">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-294">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-295">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-295">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-296">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-296">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-297">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-297">'Blazor'</span></span>
- <span data-ttu-id="a921e-298">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-298">'Identity'</span></span>
- <span data-ttu-id="a921e-299">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-299">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-300">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-300">'Razor'</span></span>
- <span data-ttu-id="a921e-301">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-301">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-302">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-302">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-303">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-303">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-304">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-304">'Blazor'</span></span>
- <span data-ttu-id="a921e-305">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-305">'Identity'</span></span>
- <span data-ttu-id="a921e-306">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-306">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-307">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-307">'Razor'</span></span>
- <span data-ttu-id="a921e-308">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-308">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-309">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-309">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-310">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-310">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-311">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-311">'Blazor'</span></span>
- <span data-ttu-id="a921e-312">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-312">'Identity'</span></span>
- <span data-ttu-id="a921e-313">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-313">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-314">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-314">'Razor'</span></span>
- <span data-ttu-id="a921e-315">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-315">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-316">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-316">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-317">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-317">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-318">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-318">'Blazor'</span></span>
- <span data-ttu-id="a921e-319">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-319">'Identity'</span></span>
- <span data-ttu-id="a921e-320">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-320">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-321">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-321">'Razor'</span></span>
- <span data-ttu-id="a921e-322">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-322">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-323">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-323">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-324">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-324">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-325">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-325">'Blazor'</span></span>
- <span data-ttu-id="a921e-326">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-326">'Identity'</span></span>
- <span data-ttu-id="a921e-327">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-327">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-328">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-328">'Razor'</span></span>
- <span data-ttu-id="a921e-329">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-329">'SignalR' uid:</span></span> 

<span data-ttu-id="a921e-330">----------- | |`SetBrowserRequestCredentials`         |  [認證](https://developer.mozilla.org/docs/Web/API/Request/credentials)| `SetBrowserRequestCache` |               | 快[取 | |](https://developer.mozilla.org/docs/Web/API/Request/cache)`SetBrowserRequestMode`                |  [模式](https://developer.mozilla.org/docs/Web/API/Request/mode)| `SetBrowserRequestIntegrity` |           | [完整性](https://developer.mozilla.org/docs/Web/API/Request/integrity) |</span><span class="sxs-lookup"><span data-stu-id="a921e-330">----------- | | `SetBrowserRequestCredentials`        | [credentials](https://developer.mozilla.org/docs/Web/API/Request/credentials) | | `SetBrowserRequestCache`              | [cache](https://developer.mozilla.org/docs/Web/API/Request/cache) | | `SetBrowserRequestMode`               | [mode](https://developer.mozilla.org/docs/Web/API/Request/mode) | | `SetBrowserRequestIntegrity`          | [integrity](https://developer.mozilla.org/docs/Web/API/Request/integrity) |</span></span>

<span data-ttu-id="a921e-331">您可以使用更泛型的擴充方法來設定其他選項 `SetBrowserRequestOption` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-331">You can set additional options using the more generic `SetBrowserRequestOption` extension method.</span></span>
 
<span data-ttu-id="a921e-332">HTTP 回應通常會在 Blazor WebAssembly 應用程式中進行緩衝處理，以啟用回應內容的同步讀取支援。</span><span class="sxs-lookup"><span data-stu-id="a921e-332">The HTTP response is typically buffered in a Blazor WebAssembly app to enable support for sync reads on the response content.</span></span> <span data-ttu-id="a921e-333">若要啟用回應串流的支援，請 `SetBrowserResponseStreamingEnabled` 在要求上使用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a921e-333">To enable support for response streaming, use the `SetBrowserResponseStreamingEnabled` extension method on the request.</span></span>

<span data-ttu-id="a921e-334">若要在跨原始來源要求中包含認證，請使用 `SetBrowserRequestCredentials` 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="a921e-334">To include credentials in a cross-origin request, use the `SetBrowserRequestCredentials` extension method:</span></span>

```csharp
requestMessage.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
```

<span data-ttu-id="a921e-335">如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="a921e-335">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="a921e-336">在 CORS 要求上傳送認證（授權 cookie/標頭）時， `Authorization` cors 原則必須允許標頭。</span><span class="sxs-lookup"><span data-stu-id="a921e-336">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="a921e-337">下列原則包含的設定：</span><span class="sxs-lookup"><span data-stu-id="a921e-337">The following policy includes configuration for:</span></span>

* <span data-ttu-id="a921e-338">要求來源（ `http://localhost:5000` 、 `https://localhost:5001` ）。</span><span class="sxs-lookup"><span data-stu-id="a921e-338">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="a921e-339">Any 方法（動詞）。</span><span class="sxs-lookup"><span data-stu-id="a921e-339">Any method (verb).</span></span>
* <span data-ttu-id="a921e-340">`Content-Type`和 `Authorization` 標頭。</span><span class="sxs-lookup"><span data-stu-id="a921e-340">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="a921e-341">若要允許自訂標頭（例如 `x-custom-header` ），請在呼叫時列出標頭 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 。</span><span class="sxs-lookup"><span data-stu-id="a921e-341">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="a921e-342">用戶端 JavaScript 程式碼（ `credentials` 屬性設為）所設定的認證 `include` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-342">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="a921e-343">如需詳細資訊，請參閱 <xref:security/cors> 和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。</span><span class="sxs-lookup"><span data-stu-id="a921e-343">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="handle-token-request-errors"></a><span data-ttu-id="a921e-344">處理權杖要求錯誤</span><span class="sxs-lookup"><span data-stu-id="a921e-344">Handle token request errors</span></span>

<span data-ttu-id="a921e-345">當單一頁面應用程式（SPA）使用 Open ID Connect （OIDC）來驗證使用者時，驗證狀態會在 SPA 和 Identity 提供者（IP）中的本機維護，其格式為使用者提供其認證所設定的會話 cookie。</span><span class="sxs-lookup"><span data-stu-id="a921e-345">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="a921e-346">IP 為使用者發出的權杖通常會在短時間內有效，大約一小時，因此用戶端應用程式必須定期提取新的權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-346">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="a921e-347">否則，在授與的權杖過期之後，使用者會被登出。</span><span class="sxs-lookup"><span data-stu-id="a921e-347">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="a921e-348">在大多數情況下，OIDC 用戶端可以布建新的權杖，而不需要使用者重新驗證，因為它會保留在 IP 內的驗證狀態或「會話」。</span><span class="sxs-lookup"><span data-stu-id="a921e-348">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="a921e-349">在某些情況下，用戶端無法在沒有使用者互動的情況下取得權杖，例如，基於某些原因，使用者明確地登出了 IP。</span><span class="sxs-lookup"><span data-stu-id="a921e-349">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="a921e-350">如果使用者造訪和登出，就會發生這種情況 `https://login.microsoftonline.com` 。在這些情況下，應用程式不會立即得知使用者是否已登出。用戶端持有的任何權杖可能不再有效。</span><span class="sxs-lookup"><span data-stu-id="a921e-350">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="a921e-351">此外，用戶端無法在目前權杖過期之後，不需要使用者互動就布建新權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-351">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="a921e-352">這些案例並不是以權杖為基礎的驗證所特有。</span><span class="sxs-lookup"><span data-stu-id="a921e-352">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="a921e-353">它們屬於 Spa 的本質。</span><span class="sxs-lookup"><span data-stu-id="a921e-353">They are part of the nature of SPAs.</span></span> <span data-ttu-id="a921e-354">如果移除驗證 cookie，使用 cookie 的 SPA 也無法呼叫伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="a921e-354">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="a921e-355">當應用程式對受保護的資源執行 API 呼叫時，您必須注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="a921e-355">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="a921e-356">若要布建新的存取權杖以呼叫 API，使用者可能需要再次進行驗證。</span><span class="sxs-lookup"><span data-stu-id="a921e-356">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="a921e-357">即使用戶端的權杖看似有效，對伺服器的呼叫可能會失敗，因為使用者已撤銷權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-357">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="a921e-358">當應用程式要求權杖時，會有兩種可能的結果：</span><span class="sxs-lookup"><span data-stu-id="a921e-358">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="a921e-359">要求成功，且應用程式具有有效的權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-359">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="a921e-360">要求失敗，且應用程式必須再次驗證使用者，才能取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-360">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="a921e-361">當令牌要求失敗時，您必須決定是否要在執行重新導向之前，先儲存任何目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-361">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="a921e-362">有數種方法存在，並增加複雜性層級：</span><span class="sxs-lookup"><span data-stu-id="a921e-362">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="a921e-363">將目前的頁面狀態儲存在會話儲存體中。</span><span class="sxs-lookup"><span data-stu-id="a921e-363">Store the current page state in session storage.</span></span> <span data-ttu-id="a921e-364">在期間 `OnInitializeAsync` ，檢查是否可以還原狀態，再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="a921e-364">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="a921e-365">新增查詢字串參數，並使用它來通知應用程式它需要重新序列化先前儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-365">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="a921e-366">新增具有唯一識別碼的查詢字串參數，以將資料儲存在會話儲存體中，而不會有風險與其他專案衝突。</span><span class="sxs-lookup"><span data-stu-id="a921e-366">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="a921e-367">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="a921e-367">The following example shows how to:</span></span>

* <span data-ttu-id="a921e-368">在重新導向至登入頁面之前保留狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-368">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="a921e-369">使用查詢字串參數，在驗證之後復原先前的狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-369">Recover the previous state afterward authentication using the query string parameter.</span></span>

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

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="a921e-370">在驗證操作之前儲存應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="a921e-370">Save app state before an authentication operation</span></span>

<span data-ttu-id="a921e-371">在驗證作業期間，某些情況下，您會想要在瀏覽器重新導向至 IP 之前，先儲存應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-371">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="a921e-372">當您使用類似狀態容器的情況，而且您想要在驗證成功之後還原狀態時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="a921e-372">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="a921e-373">您可以使用自訂驗證狀態物件來保留應用程式特定狀態或其參考，並在驗證作業成功完成後還原該狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-373">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="a921e-374">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-374">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/authentication/{action}"
@inject JSRuntime JS
@inject StateContainer State
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorViewCore Action="@Action" 
    AuthenticationState="AuthenticationState" OnLoginSucceded="RestoreState" 
    OnLogoutSucceded="RestoreState" />

@code {
    [Parameter]
    public string Action { get; set; }

    public class ApplicationAuthenticationState : RemoteAuthenticationState
    {
        public string Id { get; set; }
    }

    protected async override Task OnInitializedAsync()
    {
        if (RemoteAuthenticationActions.IsAction(RemoteAuthenticationActions.LogIn, 
            Action))
        {
            AuthenticationState.Id = Guid.NewGuid().ToString();
            await JS.InvokeVoidAsync("sessionStorage.setKey", 
                AuthenticationState.Id, State.Store());
        }
    }

    public async Task RestoreState(ApplicationAuthenticationState state)
    {
        var stored = await JS.InvokeAsync<string>("sessionStorage.getKey", 
            state.Id);
        State.FromStore(stored);
    }

    public ApplicationAuthenticationState AuthenticationState { get; set; } = 
        new ApplicationAuthenticationState();
}
```

## <a name="customize-app-routes"></a><span data-ttu-id="a921e-375">自訂應用程式路由</span><span class="sxs-lookup"><span data-stu-id="a921e-375">Customize app routes</span></span>

<span data-ttu-id="a921e-376">根據預設，連結 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 庫會使用下表所示的路由來代表不同的驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="a921e-376">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="a921e-377">路由</span><span class="sxs-lookup"><span data-stu-id="a921e-377">Route</span></span>                            | <span data-ttu-id="a921e-378">目的</span><span class="sxs-lookup"><span data-stu-id="a921e-378">Purpose</span></span> |
| ---
<span data-ttu-id="a921e-379">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-379">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-380">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-380">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-381">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-381">'Blazor'</span></span>
- <span data-ttu-id="a921e-382">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-382">'Identity'</span></span>
- <span data-ttu-id="a921e-383">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-383">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-384">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-384">'Razor'</span></span>
- <span data-ttu-id="a921e-385">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-385">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-386">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-386">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-387">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-387">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-388">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-388">'Blazor'</span></span>
- <span data-ttu-id="a921e-389">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-389">'Identity'</span></span>
- <span data-ttu-id="a921e-390">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-390">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-391">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-391">'Razor'</span></span>
- <span data-ttu-id="a921e-392">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-392">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-393">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-393">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-394">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-394">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-395">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-395">'Blazor'</span></span>
- <span data-ttu-id="a921e-396">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-396">'Identity'</span></span>
- <span data-ttu-id="a921e-397">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-397">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-398">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-398">'Razor'</span></span>
- <span data-ttu-id="a921e-399">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-399">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-400">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-400">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-401">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-401">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-402">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-402">'Blazor'</span></span>
- <span data-ttu-id="a921e-403">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-403">'Identity'</span></span>
- <span data-ttu-id="a921e-404">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-404">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-405">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-405">'Razor'</span></span>
- <span data-ttu-id="a921e-406">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-406">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-407">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-407">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-408">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-408">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-409">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-409">'Blazor'</span></span>
- <span data-ttu-id="a921e-410">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-410">'Identity'</span></span>
- <span data-ttu-id="a921e-411">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-411">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-412">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-412">'Razor'</span></span>
- <span data-ttu-id="a921e-413">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-413">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-414">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-414">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-415">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-415">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-416">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-416">'Blazor'</span></span>
- <span data-ttu-id="a921e-417">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-417">'Identity'</span></span>
- <span data-ttu-id="a921e-418">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-418">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-419">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-419">'Razor'</span></span>
- <span data-ttu-id="a921e-420">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-420">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-421">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-421">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-422">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-422">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-423">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-423">'Blazor'</span></span>
- <span data-ttu-id="a921e-424">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-424">'Identity'</span></span>
- <span data-ttu-id="a921e-425">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-425">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-426">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-426">'Razor'</span></span>
- <span data-ttu-id="a921e-427">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-427">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-428">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-428">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-429">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-429">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-430">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-430">'Blazor'</span></span>
- <span data-ttu-id="a921e-431">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-431">'Identity'</span></span>
- <span data-ttu-id="a921e-432">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-432">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-433">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-433">'Razor'</span></span>
- <span data-ttu-id="a921e-434">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-434">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-435">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-435">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-436">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-436">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-437">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-437">'Blazor'</span></span>
- <span data-ttu-id="a921e-438">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-438">'Identity'</span></span>
- <span data-ttu-id="a921e-439">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-439">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-440">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-440">'Razor'</span></span>
- <span data-ttu-id="a921e-441">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-441">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-442">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-442">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-443">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-443">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-444">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-444">'Blazor'</span></span>
- <span data-ttu-id="a921e-445">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-445">'Identity'</span></span>
- <span data-ttu-id="a921e-446">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-446">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-447">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-447">'Razor'</span></span>
- <span data-ttu-id="a921e-448">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-448">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-449">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-449">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-450">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-450">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-451">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-451">'Blazor'</span></span>
- <span data-ttu-id="a921e-452">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-452">'Identity'</span></span>
- <span data-ttu-id="a921e-453">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-453">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-454">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-454">'Razor'</span></span>
- <span data-ttu-id="a921e-455">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-455">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-456">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-456">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-457">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-457">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-458">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-458">'Blazor'</span></span>
- <span data-ttu-id="a921e-459">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-459">'Identity'</span></span>
- <span data-ttu-id="a921e-460">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-460">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-461">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-461">'Razor'</span></span>
- <span data-ttu-id="a921e-462">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-462">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-463">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-463">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-464">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-464">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-465">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-465">'Blazor'</span></span>
- <span data-ttu-id="a921e-466">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-466">'Identity'</span></span>
- <span data-ttu-id="a921e-467">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-467">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-468">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-468">'Razor'</span></span>
- <span data-ttu-id="a921e-469">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-469">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-470">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-470">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-471">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-471">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-472">'Blazor'</span></span>
- <span data-ttu-id="a921e-473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-473">'Identity'</span></span>
- <span data-ttu-id="a921e-474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-474">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-475">'Razor'</span></span>
- <span data-ttu-id="a921e-476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-476">'SignalR' uid:</span></span> 

<span data-ttu-id="a921e-477">---------------- |---標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-477">---------------- | --- title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-478">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-478">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-479">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-479">'Blazor'</span></span>
- <span data-ttu-id="a921e-480">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-480">'Identity'</span></span>
- <span data-ttu-id="a921e-481">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-481">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-482">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-482">'Razor'</span></span>
- <span data-ttu-id="a921e-483">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-483">'SignalR' uid:</span></span> 

<span data-ttu-id="a921e-484">---- | |`authentication/login`           |觸發登入作業。</span><span class="sxs-lookup"><span data-stu-id="a921e-484">---- | | `authentication/login`           | Triggers a sign-in operation.</span></span> <span data-ttu-id="a921e-485">| |`authentication/login-callback`  |處理任何登入作業的結果。</span><span class="sxs-lookup"><span data-stu-id="a921e-485">| | `authentication/login-callback`  | Handles the result of any sign-in operation.</span></span> <span data-ttu-id="a921e-486">| |`authentication/login-failed`    |當登入作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a921e-486">| | `authentication/login-failed`    | Displays error messages when the sign-in operation fails for some reason.</span></span> <span data-ttu-id="a921e-487">| |`authentication/logout`          |觸發登出作業。</span><span class="sxs-lookup"><span data-stu-id="a921e-487">| | `authentication/logout`          | Triggers a sign-out operation.</span></span> <span data-ttu-id="a921e-488">| |`authentication/logout-callback` |處理登出作業的結果。</span><span class="sxs-lookup"><span data-stu-id="a921e-488">| | `authentication/logout-callback` | Handles the result of a sign-out operation.</span></span> <span data-ttu-id="a921e-489">| |`authentication/logout-failed`   |當登出作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a921e-489">| | `authentication/logout-failed`   | Displays error messages when the sign-out operation fails for some reason.</span></span> <span data-ttu-id="a921e-490">| |`authentication/logged-out`      |表示使用者已成功登出。</span><span class="sxs-lookup"><span data-stu-id="a921e-490">| | `authentication/logged-out`      | Indicates that the user has successfully logout.</span></span> <span data-ttu-id="a921e-491">| |`authentication/profile`         |觸發操作以編輯使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="a921e-491">| | `authentication/profile`         | Triggers an operation to edit the user profile.</span></span> <span data-ttu-id="a921e-492">| |`authentication/register`        |觸發操作以註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="a921e-492">| | `authentication/register`        | Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="a921e-493">上表中顯示的路由可透過來設定 `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-493">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="a921e-494">設定選項以提供自訂路由時，請確認應用程式具有處理每個路徑的路由。</span><span class="sxs-lookup"><span data-stu-id="a921e-494">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="a921e-495">在下列範例中，所有路徑的前面都會加上 `/security` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-495">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="a921e-496">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-496">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="a921e-497">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-497">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="a921e-498">如果需求會呼叫完全不同的路徑，請如先前所述設定路由，並 `RemoteAuthenticatorView` 使用明確的 action 參數來呈現：</span><span class="sxs-lookup"><span data-stu-id="a921e-498">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="a921e-499">如果您選擇這樣做，則可以將 UI 分成不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="a921e-499">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="a921e-500">自訂驗證使用者介面</span><span class="sxs-lookup"><span data-stu-id="a921e-500">Customize the authentication user interface</span></span>

<span data-ttu-id="a921e-501">`RemoteAuthenticatorView`針對每個驗證狀態包含一組預設的 UI 元件。</span><span class="sxs-lookup"><span data-stu-id="a921e-501">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="a921e-502">您可以藉由傳入自訂來自訂每個狀態 `RenderFragment` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-502">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="a921e-503">若要在初始登入程式期間自訂顯示的文字，可以變更，如下所示 `RemoteAuthenticatorView` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-503">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="a921e-504">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="a921e-504">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

<span data-ttu-id="a921e-505">`RemoteAuthenticatorView`有一個片段，可用於下表所示的每個驗證路由。</span><span class="sxs-lookup"><span data-stu-id="a921e-505">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="a921e-506">路由</span><span class="sxs-lookup"><span data-stu-id="a921e-506">Route</span></span>                            | <span data-ttu-id="a921e-507">片段</span><span class="sxs-lookup"><span data-stu-id="a921e-507">Fragment</span></span>                |
| ---
<span data-ttu-id="a921e-508">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-508">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-509">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-509">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-510">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-510">'Blazor'</span></span>
- <span data-ttu-id="a921e-511">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-511">'Identity'</span></span>
- <span data-ttu-id="a921e-512">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-512">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-513">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-513">'Razor'</span></span>
- <span data-ttu-id="a921e-514">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-514">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-515">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-515">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-516">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-516">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-517">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-517">'Blazor'</span></span>
- <span data-ttu-id="a921e-518">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-518">'Identity'</span></span>
- <span data-ttu-id="a921e-519">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-519">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-520">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-520">'Razor'</span></span>
- <span data-ttu-id="a921e-521">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-521">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-522">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-522">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-523">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-523">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-524">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-524">'Blazor'</span></span>
- <span data-ttu-id="a921e-525">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-525">'Identity'</span></span>
- <span data-ttu-id="a921e-526">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-526">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-527">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-527">'Razor'</span></span>
- <span data-ttu-id="a921e-528">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-528">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-529">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-529">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-530">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-530">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-531">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-531">'Blazor'</span></span>
- <span data-ttu-id="a921e-532">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-532">'Identity'</span></span>
- <span data-ttu-id="a921e-533">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-533">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-534">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-534">'Razor'</span></span>
- <span data-ttu-id="a921e-535">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-535">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-536">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-536">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-537">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-537">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-538">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-538">'Blazor'</span></span>
- <span data-ttu-id="a921e-539">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-539">'Identity'</span></span>
- <span data-ttu-id="a921e-540">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-540">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-541">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-541">'Razor'</span></span>
- <span data-ttu-id="a921e-542">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-542">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-543">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-543">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-544">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-544">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-545">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-545">'Blazor'</span></span>
- <span data-ttu-id="a921e-546">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-546">'Identity'</span></span>
- <span data-ttu-id="a921e-547">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-547">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-548">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-548">'Razor'</span></span>
- <span data-ttu-id="a921e-549">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-549">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-550">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-550">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-551">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-551">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-552">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-552">'Blazor'</span></span>
- <span data-ttu-id="a921e-553">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-553">'Identity'</span></span>
- <span data-ttu-id="a921e-554">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-554">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-555">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-555">'Razor'</span></span>
- <span data-ttu-id="a921e-556">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-556">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-557">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-557">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-558">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-558">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-559">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-559">'Blazor'</span></span>
- <span data-ttu-id="a921e-560">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-560">'Identity'</span></span>
- <span data-ttu-id="a921e-561">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-561">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-562">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-562">'Razor'</span></span>
- <span data-ttu-id="a921e-563">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-563">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-564">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-564">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-565">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-565">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-566">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-566">'Blazor'</span></span>
- <span data-ttu-id="a921e-567">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-567">'Identity'</span></span>
- <span data-ttu-id="a921e-568">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-568">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-569">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-569">'Razor'</span></span>
- <span data-ttu-id="a921e-570">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-570">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-571">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-571">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-572">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-572">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-573">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-573">'Blazor'</span></span>
- <span data-ttu-id="a921e-574">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-574">'Identity'</span></span>
- <span data-ttu-id="a921e-575">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-575">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-576">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-576">'Razor'</span></span>
- <span data-ttu-id="a921e-577">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-577">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-578">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-578">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-579">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-579">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-580">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-580">'Blazor'</span></span>
- <span data-ttu-id="a921e-581">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-581">'Identity'</span></span>
- <span data-ttu-id="a921e-582">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-582">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-583">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-583">'Razor'</span></span>
- <span data-ttu-id="a921e-584">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-584">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-585">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-585">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-586">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-586">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-587">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-587">'Blazor'</span></span>
- <span data-ttu-id="a921e-588">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-588">'Identity'</span></span>
- <span data-ttu-id="a921e-589">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-589">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-590">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-590">'Razor'</span></span>
- <span data-ttu-id="a921e-591">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-591">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-592">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-592">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-593">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-593">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-594">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-594">'Blazor'</span></span>
- <span data-ttu-id="a921e-595">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-595">'Identity'</span></span>
- <span data-ttu-id="a921e-596">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-596">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-597">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-597">'Razor'</span></span>
- <span data-ttu-id="a921e-598">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-598">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-599">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-599">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-600">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-600">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-601">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-601">'Blazor'</span></span>
- <span data-ttu-id="a921e-602">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-602">'Identity'</span></span>
- <span data-ttu-id="a921e-603">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-603">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-604">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-604">'Razor'</span></span>
- <span data-ttu-id="a921e-605">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-605">'SignalR' uid:</span></span> 

<span data-ttu-id="a921e-606">---------------- |---標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-606">---------------- | --- title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-607">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-607">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-608">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-608">'Blazor'</span></span>
- <span data-ttu-id="a921e-609">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-609">'Identity'</span></span>
- <span data-ttu-id="a921e-610">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-610">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-611">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-611">'Razor'</span></span>
- <span data-ttu-id="a921e-612">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-612">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-613">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-613">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-614">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-614">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-615">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-615">'Blazor'</span></span>
- <span data-ttu-id="a921e-616">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-616">'Identity'</span></span>
- <span data-ttu-id="a921e-617">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-617">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-618">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-618">'Razor'</span></span>
- <span data-ttu-id="a921e-619">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-619">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-620">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-620">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-621">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-621">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-622">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-622">'Blazor'</span></span>
- <span data-ttu-id="a921e-623">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-623">'Identity'</span></span>
- <span data-ttu-id="a921e-624">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-624">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-625">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-625">'Razor'</span></span>
- <span data-ttu-id="a921e-626">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-626">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-627">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-627">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-628">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-628">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-629">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-629">'Blazor'</span></span>
- <span data-ttu-id="a921e-630">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-630">'Identity'</span></span>
- <span data-ttu-id="a921e-631">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-631">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-632">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-632">'Razor'</span></span>
- <span data-ttu-id="a921e-633">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-633">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-634">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-634">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-635">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-635">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-636">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-636">'Blazor'</span></span>
- <span data-ttu-id="a921e-637">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-637">'Identity'</span></span>
- <span data-ttu-id="a921e-638">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-638">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-639">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-639">'Razor'</span></span>
- <span data-ttu-id="a921e-640">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-640">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-641">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-641">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-642">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-642">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-643">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-643">'Blazor'</span></span>
- <span data-ttu-id="a921e-644">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-644">'Identity'</span></span>
- <span data-ttu-id="a921e-645">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-645">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-646">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-646">'Razor'</span></span>
- <span data-ttu-id="a921e-647">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-647">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-648">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-648">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-649">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-649">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-650">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-650">'Blazor'</span></span>
- <span data-ttu-id="a921e-651">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-651">'Identity'</span></span>
- <span data-ttu-id="a921e-652">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-652">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-653">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-653">'Razor'</span></span>
- <span data-ttu-id="a921e-654">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-654">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-655">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-655">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-656">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-656">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-657">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-657">'Blazor'</span></span>
- <span data-ttu-id="a921e-658">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-658">'Identity'</span></span>
- <span data-ttu-id="a921e-659">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-659">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-660">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-660">'Razor'</span></span>
- <span data-ttu-id="a921e-661">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-661">'SignalR' uid:</span></span> 

-
<span data-ttu-id="a921e-662">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-662">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="a921e-663">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a921e-663">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a921e-664">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a921e-664">'Blazor'</span></span>
- <span data-ttu-id="a921e-665">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a921e-665">'Identity'</span></span>
- <span data-ttu-id="a921e-666">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a921e-666">'Let's Encrypt'</span></span>
- <span data-ttu-id="a921e-667">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a921e-667">'Razor'</span></span>
- <span data-ttu-id="a921e-668">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a921e-668">'SignalR' uid:</span></span> 

<span data-ttu-id="a921e-669">------------ | | `authentication/login`           | `<LoggingIn>`           | | `authentication/login-callback`  | `<CompletingLoggingIn>` | | `authentication/login-failed`    | `<LogInFailed>`         | | `authentication/logout`          | `<LogOut>`              | | `authentication/logout-callback` | `<CompletingLogOut>`    | | `authentication/logout-failed`   | `<LogOutFailed>`        | | `authentication/logged-out`      | `<LogOutSucceeded>`     | | `authentication/profile`         | `<UserProfile>`         | | `authentication/register`        | `<Registering>`         |</span><span class="sxs-lookup"><span data-stu-id="a921e-669">------------ | | `authentication/login`           | `<LoggingIn>`           | | `authentication/login-callback`  | `<CompletingLoggingIn>` | | `authentication/login-failed`    | `<LogInFailed>`         | | `authentication/logout`          | `<LogOut>`              | | `authentication/logout-callback` | `<CompletingLogOut>`    | | `authentication/logout-failed`   | `<LogOutFailed>`        | | `authentication/logged-out`      | `<LogOutSucceeded>`     | | `authentication/profile`         | `<UserProfile>`         | | `authentication/register`        | `<Registering>`         |</span></span>

## <a name="customize-the-user"></a><span data-ttu-id="a921e-670">自訂使用者</span><span class="sxs-lookup"><span data-stu-id="a921e-670">Customize the user</span></span>

<span data-ttu-id="a921e-671">可以自訂系結至應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="a921e-671">Users bound to the app can be customized.</span></span> <span data-ttu-id="a921e-672">在下列範例中，所有已驗證的使用者都會收到 `amr` 每個使用者驗證方法的宣告。</span><span class="sxs-lookup"><span data-stu-id="a921e-672">In the following example, all authenticated users receive an `amr` claim for each of the user's authentication methods.</span></span>

<span data-ttu-id="a921e-673">建立擴充類別的類別 `RemoteUserAccount` ：</span><span class="sxs-lookup"><span data-stu-id="a921e-673">Create a class that extends the `RemoteUserAccount` class:</span></span>

```csharp
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class CustomUserAccount : RemoteUserAccount
{
    [JsonPropertyName("amr")]
    public string[] AuthenticationMethod { get; set; }
}
```

<span data-ttu-id="a921e-674">建立可延伸的 factory `AccountClaimsPrincipalFactory<TAccount>` ：</span><span class="sxs-lookup"><span data-stu-id="a921e-674">Create a factory that extends `AccountClaimsPrincipalFactory<TAccount>`:</span></span>

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

<span data-ttu-id="a921e-675">`CustomAccountFactory`為使用中的驗證提供者註冊。</span><span class="sxs-lookup"><span data-stu-id="a921e-675">Register the `CustomAccountFactory` for the authentication provider in use.</span></span> <span data-ttu-id="a921e-676">下列任何一項註冊都是有效的：</span><span class="sxs-lookup"><span data-stu-id="a921e-676">Any of the following registrations are valid:</span></span> 

* <span data-ttu-id="a921e-677">`AddOidcAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a921e-677">`AddOidcAuthentication`:</span></span>

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

* <span data-ttu-id="a921e-678">`AddMsalAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a921e-678">`AddMsalAuthentication`:</span></span>

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
  
* <span data-ttu-id="a921e-679">`AddApiAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="a921e-679">`AddApiAuthorization`:</span></span>

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

## <a name="support-prerendering-with-authentication"></a><span data-ttu-id="a921e-680">支援使用驗證來進行預呈現</span><span class="sxs-lookup"><span data-stu-id="a921e-680">Support prerendering with authentication</span></span>

<span data-ttu-id="a921e-681">遵循其中一個託管 WebAssembly 應用程式主題中的指導方針之後 Blazor ，請使用下列指示來建立應用程式：</span><span class="sxs-lookup"><span data-stu-id="a921e-681">After following the guidance in one of the hosted Blazor WebAssembly app topics, use the following instructions to create an app that:</span></span>

* <span data-ttu-id="a921e-682">不需要授權的 Prerenders 路徑。</span><span class="sxs-lookup"><span data-stu-id="a921e-682">Prerenders paths for which authorization isn't required.</span></span>
* <span data-ttu-id="a921e-683">不需要授權的已呈現路徑。</span><span class="sxs-lookup"><span data-stu-id="a921e-683">Doesn't prerender paths for which authorization is required.</span></span>

<span data-ttu-id="a921e-684">在用戶端應用程式的 `Program` 類別（*Program.cs*）中，將常見的服務註冊因素劃分為不同的方法（例如， `ConfigureCommonServices` ）：</span><span class="sxs-lookup"><span data-stu-id="a921e-684">In the Client app's `Program` class (*Program.cs*), factor common service registrations into a separate method (for example, `ConfigureCommonServices`):</span></span>

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

<span data-ttu-id="a921e-685">在伺服器應用程式的中 `Startup.ConfigureServices` ，註冊下列其他服務：</span><span class="sxs-lookup"><span data-stu-id="a921e-685">In the Server app's `Startup.ConfigureServices`, register the following additional services:</span></span>

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

<span data-ttu-id="a921e-686">在伺服器應用程式的 `Startup.Configure` 方法中，將取代 `endpoints.MapFallbackToFile("index.html")` 為 `endpoints.MapFallbackToPage("/_Host")` ：</span><span class="sxs-lookup"><span data-stu-id="a921e-686">In the Server app's `Startup.Configure` method, replace `endpoints.MapFallbackToFile("index.html")` with `endpoints.MapFallbackToPage("/_Host")`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

<span data-ttu-id="a921e-687">在伺服器應用程式中，建立*Pages*資料夾（如果不存在）。</span><span class="sxs-lookup"><span data-stu-id="a921e-687">In the Server app, create a *Pages* folder if it doesn't exist.</span></span> <span data-ttu-id="a921e-688">在伺服器應用程式的*Pages*資料夾內建立 *_Host 的 cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="a921e-688">Create a *_Host.cshtml* page inside the Server app's *Pages* folder.</span></span> <span data-ttu-id="a921e-689">將用戶端應用程式*wwwroot/index.html*檔的內容貼到*Pages/_Host. cshtml*檔案中。</span><span class="sxs-lookup"><span data-stu-id="a921e-689">Paste the contents from the Client app's *wwwroot/index.html* file into the *Pages/_Host.cshtml* file.</span></span> <span data-ttu-id="a921e-690">更新檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="a921e-690">Update the file's contents:</span></span>

* <span data-ttu-id="a921e-691">將 `@page "_Host"` 新增到檔案的頂端。</span><span class="sxs-lookup"><span data-stu-id="a921e-691">Add `@page "_Host"` to the top of the file.</span></span>
* <span data-ttu-id="a921e-692">將標記取代為 `<app>Loading...</app>` 下列內容：</span><span class="sxs-lookup"><span data-stu-id="a921e-692">Replace the `<app>Loading...</app>` tag with the following:</span></span>

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
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a><span data-ttu-id="a921e-693">託管應用程式和協力廠商登入提供者的選項</span><span class="sxs-lookup"><span data-stu-id="a921e-693">Options for hosted apps and third-party login providers</span></span>

<span data-ttu-id="a921e-694">Blazor使用協力廠商提供者驗證和授權託管 WebAssembly 應用程式時，有數個選項可用來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="a921e-694">When authenticating and authorizing a hosted Blazor WebAssembly app with a third-party provider, there are several options available for authenticating the user.</span></span> <span data-ttu-id="a921e-695">您選擇哪一個取決於您的案例。</span><span class="sxs-lookup"><span data-stu-id="a921e-695">Which one you choose depends on your scenario.</span></span>

<span data-ttu-id="a921e-696">如需詳細資訊，請參閱<xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="a921e-696">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a><span data-ttu-id="a921e-697">驗證使用者只呼叫受保護的協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="a921e-697">Authenticate users to only call protected third party APIs</span></span>

<span data-ttu-id="a921e-698">對協力廠商 API 提供者的用戶端 OAuth 流程驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="a921e-698">Authenticate the user with a client-side OAuth flow against the third-party API provider:</span></span>

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 <span data-ttu-id="a921e-699">在此情節中：</span><span class="sxs-lookup"><span data-stu-id="a921e-699">In this scenario:</span></span>

* <span data-ttu-id="a921e-700">裝載應用程式的伺服器不扮演角色。</span><span class="sxs-lookup"><span data-stu-id="a921e-700">The server hosting the app doesn't play a role.</span></span>
* <span data-ttu-id="a921e-701">無法保護伺服器上的 Api。</span><span class="sxs-lookup"><span data-stu-id="a921e-701">APIs on the server can't be protected.</span></span>
* <span data-ttu-id="a921e-702">應用程式只能呼叫受保護的協力廠商 Api。</span><span class="sxs-lookup"><span data-stu-id="a921e-702">The app can only call protected third-party APIs.</span></span>

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a><span data-ttu-id="a921e-703">使用協力廠商提供者來驗證使用者，並在主機伺服器和協力廠商上呼叫受保護的 Api</span><span class="sxs-lookup"><span data-stu-id="a921e-703">Authenticate users with a third-party provider and call protected APIs on the host server and the third party</span></span>

<span data-ttu-id="a921e-704">Identity使用協力廠商登入提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="a921e-704">Configure Identity with a third-party login provider.</span></span> <span data-ttu-id="a921e-705">取得協力廠商 API 存取所需的權杖，並加以儲存。</span><span class="sxs-lookup"><span data-stu-id="a921e-705">Obtain the tokens required for third-party API access and store them.</span></span>

<span data-ttu-id="a921e-706">當使用者登入時，會 Identity 收集存取權並重新整理權杖，做為驗證程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="a921e-706">When a user logs in, Identity collects access and refresh tokens as part of the authentication process.</span></span> <span data-ttu-id="a921e-707">此時，有幾個方法可用來對協力廠商 Api 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a921e-707">At that point, there are a couple of approaches available for making API calls to third-party APIs.</span></span>

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a><span data-ttu-id="a921e-708">使用伺服器存取權杖來取出協力廠商存取權杖</span><span class="sxs-lookup"><span data-stu-id="a921e-708">Use a server access token to retrieve the third-party access token</span></span>

<span data-ttu-id="a921e-709">使用伺服器上產生的存取權杖，從伺服器 API 端點抓取協力廠商存取權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-709">Use the access token generated on the server to retrieve the third-party access token from a server API endpoint.</span></span> <span data-ttu-id="a921e-710">從該處，使用協力廠商存取權杖，直接從用戶端上呼叫協力廠商 API 資源 Identity 。</span><span class="sxs-lookup"><span data-stu-id="a921e-710">From there, use the third-party access token to call third-party API resources directly from Identity on the client.</span></span>

<span data-ttu-id="a921e-711">我們不建議採用這種方法。</span><span class="sxs-lookup"><span data-stu-id="a921e-711">We don't recommend this approach.</span></span> <span data-ttu-id="a921e-712">這種方法需要將協力廠商存取權杖視為針對公用用戶端所產生。</span><span class="sxs-lookup"><span data-stu-id="a921e-712">This approach requires treating the third-party access token as if it were generated for a public client.</span></span> <span data-ttu-id="a921e-713">在 OAuth 詞彙中，公用應用程式不會有用戶端密碼，因為它無法受信任而無法安全地儲存秘密，而且會為機密用戶端產生存取權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-713">In OAuth terms, the public app doesn't have a client secret because it can't be trusted to store secrets safely, and the access token is produced for a confidential client.</span></span> <span data-ttu-id="a921e-714">機密用戶端是具有用戶端密碼的用戶端，並假設能夠安全地儲存秘密。</span><span class="sxs-lookup"><span data-stu-id="a921e-714">A confidential client is a client that has a client secret and is assumed to be able to safely store secrets.</span></span>

* <span data-ttu-id="a921e-715">協力廠商存取權杖可能會被授與額外的範圍來執行敏感性作業，這是根據協力廠商針對較受信任的用戶端發出權杖的事實。</span><span class="sxs-lookup"><span data-stu-id="a921e-715">The third-party access token might be granted additional scopes to perform sensitive operations based on the fact that the third-party emitted the token for a more trusted client.</span></span>
* <span data-ttu-id="a921e-716">同樣地，重新整理權杖不應發給不受信任的用戶端，因為這樣做會讓用戶端無限制存取，除非有其他限制。</span><span class="sxs-lookup"><span data-stu-id="a921e-716">Similarly, refresh tokens shouldn't be issued to a client that isn't trusted, as doing so gives the client unlimited access unless other restrictions are put into place.</span></span>

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a><span data-ttu-id="a921e-717">從用戶端對伺服器 API 進行 API 呼叫，以便呼叫協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="a921e-717">Make API calls from the client to the server API in order to call third-party APIs</span></span>

<span data-ttu-id="a921e-718">從用戶端對伺服器 API 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a921e-718">Make an API call from the client to the server API.</span></span> <span data-ttu-id="a921e-719">從伺服器中，取出協力廠商 API 資源的存取權杖，併發出任何需要的呼叫。</span><span class="sxs-lookup"><span data-stu-id="a921e-719">From the server, retrieve the access token for the third-party API resource and issue whatever call is necessary.</span></span>

<span data-ttu-id="a921e-720">雖然這種方法需要透過伺服器額外的網路躍點來呼叫協力廠商 API，但最終會導致更安全的體驗：</span><span class="sxs-lookup"><span data-stu-id="a921e-720">While this approach requires an extra network hop through the server to call a third-party API, it ultimately results in a safer experience:</span></span>

* <span data-ttu-id="a921e-721">伺服器可以儲存重新整理權杖，並確保應用程式不會失去協力廠商資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="a921e-721">The server can store refresh tokens and ensure that the app doesn't lose access to third-party resources.</span></span>
* <span data-ttu-id="a921e-722">應用程式無法從可能包含更多敏感性許可權的伺服器洩漏存取權杖。</span><span class="sxs-lookup"><span data-stu-id="a921e-722">The app can't leak access tokens from the server that might contain more sensitive permissions.</span></span>

## <a name="use-open-id-connect-oidc-v20-endpoints"></a><span data-ttu-id="a921e-723">使用 Open ID Connect （OIDC） v2.0 端點</span><span class="sxs-lookup"><span data-stu-id="a921e-723">Use Open ID Connect (OIDC) v2.0 endpoints</span></span>

<span data-ttu-id="a921e-724">驗證程式庫和 Blazor 範本會使用 OPEN ID Connect （OIDC） v1.0 端點。</span><span class="sxs-lookup"><span data-stu-id="a921e-724">The authentication library and Blazor templates use Open ID Connect (OIDC) v1.0 endpoints.</span></span> <span data-ttu-id="a921e-725">若要使用 v2.0 端點，請設定 JWT 持有人 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority?displayProperty=nameWithType> 選項。</span><span class="sxs-lookup"><span data-stu-id="a921e-725">To use a v2.0 endpoint, configure the JWT Bearer <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority?displayProperty=nameWithType> option.</span></span> <span data-ttu-id="a921e-726">在下列範例中，會將區段附加至屬性，以針對 v2.0 設定 AAD `v2.0` `Authority` ：</span><span class="sxs-lookup"><span data-stu-id="a921e-726">In the following example, AAD is configured for v2.0 by appending a `v2.0` segment to the `Authority` property:</span></span>

```csharp
builder.Services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, 
    options =>
    {
        options.Authority += "/v2.0";
    });
```

<span data-ttu-id="a921e-727">或者，您也可以在應用程式設定（*appsettings*）檔案中進行設定：</span><span class="sxs-lookup"><span data-stu-id="a921e-727">Alternatively, the setting can be made in the app settings (*appsettings.json*) file:</span></span>

```json
{
  "Local": {
    "Authority": "https://login.microsoftonline.com/common/oauth2/v2.0/",
    ...
  }
}
```

<span data-ttu-id="a921e-728">如果在區段上對授權單位的追蹤不適合應用程式的 OIDC 提供者（例如使用非 AAD 提供者），請 `Authority` 直接設定屬性。</span><span class="sxs-lookup"><span data-stu-id="a921e-728">If tacking on a segment to the authority isn't appropriate for the app's OIDC provider, such as with non-AAD providers, set the `Authority` property directly.</span></span> <span data-ttu-id="a921e-729">請在 `JwtBearerOptions` 應用程式佈建檔中，以金鑰設定或的屬性 `Authority` 。</span><span class="sxs-lookup"><span data-stu-id="a921e-729">Either set the property in `JwtBearerOptions` or in the app settings file with the `Authority` key.</span></span>

<span data-ttu-id="a921e-730">針對 v2.0 端點，識別碼權杖中的宣告清單會變更。</span><span class="sxs-lookup"><span data-stu-id="a921e-730">The list of claims in the ID token changes for v2.0 endpoints.</span></span> <span data-ttu-id="a921e-731">如需詳細資訊，請參閱[為何要更新至 Microsoft 身分識別平臺（v2.0）？](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison)。</span><span class="sxs-lookup"><span data-stu-id="a921e-731">For more information, see [Why update to Microsoft identity platform (v2.0)?](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison).</span></span>
