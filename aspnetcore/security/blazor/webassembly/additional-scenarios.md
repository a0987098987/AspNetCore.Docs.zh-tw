---
<span data-ttu-id="597c4-101">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-101">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-103">'Blazor'</span></span>
- <span data-ttu-id="597c4-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-104">'Identity'</span></span>
- <span data-ttu-id="597c4-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-106">'Razor'</span></span>
- <span data-ttu-id="597c4-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="597c4-108">ASP.NET Core Blazor WebAssembly 其他安全性案例</span><span class="sxs-lookup"><span data-stu-id="597c4-108">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="597c4-109">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="597c4-109">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

## <a name="attach-tokens-to-outgoing-requests"></a><span data-ttu-id="597c4-110">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="597c4-110">Attach tokens to outgoing requests</span></span>

<span data-ttu-id="597c4-111">此 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> 服務可與搭配使用 <xref:System.Net.Http.HttpClient> ，將存取權杖附加至傳出要求。</span><span class="sxs-lookup"><span data-stu-id="597c4-111">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> service can be used with <xref:System.Net.Http.HttpClient> to attach access tokens to outgoing requests.</span></span> <span data-ttu-id="597c4-112">您可以使用現有的服務來取得權杖 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.IAccessTokenProvider> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-112">Tokens are acquired using the existing <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.IAccessTokenProvider> service.</span></span> <span data-ttu-id="597c4-113">如果無法取得權杖， <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException> 就會擲回。</span><span class="sxs-lookup"><span data-stu-id="597c4-113">If a token can't be acquired, an <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException> is thrown.</span></span> <span data-ttu-id="597c4-114"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException>具有 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException.Redirect%2A> 方法，可以用來將使用者導覽至識別提供者，以取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-114"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException> has a <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenNotAvailableException.Redirect%2A> method that can be used to navigate the user to the identity provider to acquire a new token.</span></span> <span data-ttu-id="597c4-115"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler>可以使用方法，透過授權的 url、範圍和傳回 URL 來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-115">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> can be configured with the authorized URLs, scopes, and return URL using the <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> method.</span></span>

<span data-ttu-id="597c4-116">在下列範例中，會 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> <xref:System.Net.Http.HttpClient> 在 `Program.Main` （*Program.cs*）中設定：</span><span class="sxs-lookup"><span data-stu-id="597c4-116">In the following example, <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler> configures an <xref:System.Net.Http.HttpClient> in `Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="597c4-117">為了方便起見， <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.BaseAddressAuthorizationMessageHandler> 會包含以應用程式基底位址預先設定為授權 URL 的。</span><span class="sxs-lookup"><span data-stu-id="597c4-117">For convenience, a <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.BaseAddressAuthorizationMessageHandler> is included that's preconfigured with the app base address as an authorized URL.</span></span> <span data-ttu-id="597c4-118">已啟用驗證的 Blazor WebAssembly 範本現在會 <xref:System.Net.Http.IHttpClientFactory> 在伺服器 API 專案中使用，以設定 <xref:System.Net.Http.HttpClient> 具有下列專案的 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.BaseAddressAuthorizationMessageHandler> ：</span><span class="sxs-lookup"><span data-stu-id="597c4-118">The authentication-enabled Blazor WebAssembly templates now use <xref:System.Net.Http.IHttpClientFactory> in the Server API project to set up an <xref:System.Net.Http.HttpClient> with the <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.BaseAddressAuthorizationMessageHandler>:</span></span>

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

<span data-ttu-id="597c4-119">在上述範例中，會使用建立用戶端，而在對 <xref:System.Net.Http.IHttpClientFactory.CreateClient%2A> <xref:System.Net.Http.HttpClient> 伺服器專案提出要求時，會提供包含存取權杖的實例。</span><span class="sxs-lookup"><span data-stu-id="597c4-119">Where the client is created with <xref:System.Net.Http.IHttpClientFactory.CreateClient%2A> in the preceding example, the <xref:System.Net.Http.HttpClient> is supplied instances that include access tokens when making requests to the server project.</span></span>

<span data-ttu-id="597c4-120">接著會使用設定的，透過 <xref:System.Net.Http.HttpClient> 簡單的[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)模式來提出授權的要求。</span><span class="sxs-lookup"><span data-stu-id="597c4-120">The configured <xref:System.Net.Http.HttpClient> is then used to make authorized requests using a simple [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) pattern.</span></span>

<span data-ttu-id="597c4-121">`FetchData`元件（*Pages/FetchData. razor*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-121">`FetchData` component (*Pages/FetchData.razor*):</span></span>

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

## <a name="typed-httpclient"></a><span data-ttu-id="597c4-122">具類型的 HttpClient</span><span class="sxs-lookup"><span data-stu-id="597c4-122">Typed HttpClient</span></span>

<span data-ttu-id="597c4-123">您可以定義具型別用戶端，以處理單一類別內的所有 HTTP 和權杖取得顧慮。</span><span class="sxs-lookup"><span data-stu-id="597c4-123">A typed client can be defined that handles all of the HTTP and token acquisition concerns within a single class.</span></span>

<span data-ttu-id="597c4-124">*WeatherForecastClient.cs*：</span><span class="sxs-lookup"><span data-stu-id="597c4-124">*WeatherForecastClient.cs*:</span></span>

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

<span data-ttu-id="597c4-125">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-125">`Program.Main` (*Program.cs*):</span></span>

```csharp
using System.Net.Http;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddHttpClient<WeatherForecastClient>(
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();
```

<span data-ttu-id="597c4-126">`FetchData`元件（*Pages/FetchData. razor*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-126">`FetchData` component (*Pages/FetchData.razor*):</span></span>

```razor
@inject WeatherForecastClient Client

...

protected override async Task OnInitializedAsync()
{
    forecasts = await Client.GetForecastAsync();
}
```

## <a name="configure-the-httpclient-handler"></a><span data-ttu-id="597c4-127">設定 HttpClient 處理常式</span><span class="sxs-lookup"><span data-stu-id="597c4-127">Configure the HttpClient handler</span></span>

<span data-ttu-id="597c4-128">您可以 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> 針對輸出 HTTP 要求進一步設定處理常式。</span><span class="sxs-lookup"><span data-stu-id="597c4-128">The handler can be further configured with <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AuthorizationMessageHandler.ConfigureHandler%2A> for outbound HTTP requests.</span></span>

<span data-ttu-id="597c4-129">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-129">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddHttpClient<WeatherForecastClient>(client => client.BaseAddress = new Uri("https://www.example.com/base"))
    .AddHttpMessageHandler(sp => sp.GetRequiredService<AuthorizationMessageHandler>()
    .ConfigureHandler(new [] { "https://www.example.com/base" },
        scopes: new[] { "example.read", "example.write" }));
```

## <a name="unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client"></a><span data-ttu-id="597c4-130">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="597c4-130">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>

<span data-ttu-id="597c4-131">如果 Blazor WebAssembly 應用程式通常會使用安全的預設值 <xref:System.Net.Http.HttpClient> ，應用程式也可以藉由設定名為的來進行未經驗證或未經授權的 Web API 要求 <xref:System.Net.Http.HttpClient> ：</span><span class="sxs-lookup"><span data-stu-id="597c4-131">If the Blazor WebAssembly app ordinarily uses a secure default <xref:System.Net.Http.HttpClient>, the app can also make unauthenticated or unauthorized web API requests by configuring a named <xref:System.Net.Http.HttpClient>:</span></span>

<span data-ttu-id="597c4-132">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-132">`Program.Main` (*Program.cs*):</span></span>

```csharp
builder.Services.AddHttpClient("ServerAPI.NoAuthenticationClient", 
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress));
```

<span data-ttu-id="597c4-133">先前的註冊是除了現有的安全預設註冊之外 <xref:System.Net.Http.HttpClient> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-133">The preceding registration is in addition to the existing secure default <xref:System.Net.Http.HttpClient> registration.</span></span>

<span data-ttu-id="597c4-134">元件會從建立， <xref:System.Net.Http.HttpClient> <xref:System.Net.Http.IHttpClientFactory> 以提出未經驗證或未經授權的要求：</span><span class="sxs-lookup"><span data-stu-id="597c4-134">A component creates the <xref:System.Net.Http.HttpClient> from the <xref:System.Net.Http.IHttpClientFactory> to make unauthenticated or unauthorized requests:</span></span>

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
> <span data-ttu-id="597c4-135">先前範例的伺服器 API 中的控制器 `WeatherForecastNoAuthenticationController` 不會以 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性標記。</span><span class="sxs-lookup"><span data-stu-id="597c4-135">The controller in the server API, `WeatherForecastNoAuthenticationController` for the preceding example, isn't marked with the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute.</span></span>

## <a name="request-additional-access-tokens"></a><span data-ttu-id="597c4-136">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="597c4-136">Request additional access tokens</span></span>

<span data-ttu-id="597c4-137">呼叫可以手動取得存取權杖 `IAccessTokenProvider.RequestAccessToken` 。</span><span class="sxs-lookup"><span data-stu-id="597c4-137">Access tokens can be manually obtained by calling `IAccessTokenProvider.RequestAccessToken`.</span></span>

<span data-ttu-id="597c4-138">在下列範例中，應用程式需要額外的 Azure Active Directory （AAD） Microsoft Graph API 範圍，才能讀取使用者資料和傳送郵件。</span><span class="sxs-lookup"><span data-stu-id="597c4-138">In the following example, additional Azure Active Directory (AAD) Microsoft Graph API scopes are required by an app to read user data and send mail.</span></span> <span data-ttu-id="597c4-139">在 Azure AAD 入口網站中新增 Microsoft Graph API 許可權之後，用戶端應用程式中會設定額外的範圍。</span><span class="sxs-lookup"><span data-stu-id="597c4-139">After adding the Microsoft Graph API permissions in the Azure AAD portal, the additional scopes are configured in the Client app.</span></span>

<span data-ttu-id="597c4-140">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-140">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="597c4-141">`IAccessTokenProvider.RequestToken`方法提供多載，可讓應用程式使用一組指定的範圍來布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-141">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision an access token with a given set of scopes.</span></span>

<span data-ttu-id="597c4-142">在 Razor 元件中：</span><span class="sxs-lookup"><span data-stu-id="597c4-142">In a Razor component:</span></span>

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

<span data-ttu-id="597c4-143"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenResult.TryGetToken%2A?displayProperty=nameWithType>傳回</span><span class="sxs-lookup"><span data-stu-id="597c4-143"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccessTokenResult.TryGetToken%2A?displayProperty=nameWithType> returns:</span></span>

* <span data-ttu-id="597c4-144">`true`包含 `token` 使用的。</span><span class="sxs-lookup"><span data-stu-id="597c4-144">`true` with the `token` for use.</span></span>
* <span data-ttu-id="597c4-145">`false`如果未抓取權杖，則為。</span><span class="sxs-lookup"><span data-stu-id="597c4-145">`false` if the token isn't retrieved.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="597c4-146">具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="597c4-146">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="597c4-147">在 WebAssembly 應用程式的 WebAssembly 上執行時 Blazor ， [HttpClient](xref:fundamentals/http-requests)和 <xref:System.Net.Http.HttpRequestMessage> 可以用來自訂要求。</span><span class="sxs-lookup"><span data-stu-id="597c4-147">When running on WebAssembly in a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> can be used to customize requests.</span></span> <span data-ttu-id="597c4-148">例如，您可以指定 HTTP 方法和要求標頭。</span><span class="sxs-lookup"><span data-stu-id="597c4-148">For example, you can specify the HTTP method and request headers.</span></span> <span data-ttu-id="597c4-149">下列元件會對 `POST` 伺服器上的 To Do LIST API 端點提出要求，並顯示回應主體：</span><span class="sxs-lookup"><span data-stu-id="597c4-149">The following component makes a `POST` request to a To Do List API endpoint on the server and shows the response body:</span></span>

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

<span data-ttu-id="597c4-150">.NET WebAssembly 的執行會 <xref:System.Net.Http.HttpClient> 使用[WindowOrWorkerGlobalScope。 fetch （）](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch)。</span><span class="sxs-lookup"><span data-stu-id="597c4-150">.NET WebAssembly's implementation of <xref:System.Net.Http.HttpClient> uses [WindowOrWorkerGlobalScope.fetch()](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch).</span></span> <span data-ttu-id="597c4-151">Fetch 可讓您設定數個[要求特有的選項](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="597c4-151">Fetch allows configuring several [request-specific options](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span> 

<span data-ttu-id="597c4-152">您可以使用 <xref:System.Net.Http.HttpRequestMessage> 下表所示的擴充方法來設定 HTTP 提取要求選項。</span><span class="sxs-lookup"><span data-stu-id="597c4-152">HTTP fetch request options can be configured with <xref:System.Net.Http.HttpRequestMessage> extension methods shown in the following table.</span></span>

| <span data-ttu-id="597c4-153">擴充方法</span><span class="sxs-lookup"><span data-stu-id="597c4-153">Extension method</span></span> | <span data-ttu-id="597c4-154">Fetch 要求屬性</span><span class="sxs-lookup"><span data-stu-id="597c4-154">Fetch request property</span></span> |
| --- | --- |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> | [<span data-ttu-id="597c4-155">憑證</span><span class="sxs-lookup"><span data-stu-id="597c4-155">credentials</span></span>](https://developer.mozilla.org/docs/Web/API/Request/credentials) |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCache%2A> | [<span data-ttu-id="597c4-156">高速</span><span class="sxs-lookup"><span data-stu-id="597c4-156">cache</span></span>](https://developer.mozilla.org/docs/Web/API/Request/cache) |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestMode%2A> | [<span data-ttu-id="597c4-157">mode</span><span class="sxs-lookup"><span data-stu-id="597c4-157">mode</span></span>](https://developer.mozilla.org/docs/Web/API/Request/mode) |
| <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestIntegrity%2A> | [<span data-ttu-id="597c4-158">完整性</span><span class="sxs-lookup"><span data-stu-id="597c4-158">integrity</span></span>](https://developer.mozilla.org/docs/Web/API/Request/integrity) |

<span data-ttu-id="597c4-159">您可以使用更泛型的擴充方法來設定其他選項 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestOption%2A> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-159">You can set additional options using the more generic <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestOption%2A> extension method.</span></span>
 
<span data-ttu-id="597c4-160">HTTP 回應通常會在 Blazor WebAssembly 應用程式中進行緩衝處理，以啟用回應內容的同步讀取支援。</span><span class="sxs-lookup"><span data-stu-id="597c4-160">The HTTP response is typically buffered in a Blazor WebAssembly app to enable support for sync reads on the response content.</span></span> <span data-ttu-id="597c4-161">若要啟用回應串流的支援，請 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserResponseStreamingEnabled%2A> 在要求上使用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="597c4-161">To enable support for response streaming, use the <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserResponseStreamingEnabled%2A> extension method on the request.</span></span>

<span data-ttu-id="597c4-162">若要在跨原始來源要求中包含認證，請使用 <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="597c4-162">To include credentials in a cross-origin request, use the <xref:Microsoft.AspNetCore.Components.WebAssembly.Http.WebAssemblyHttpRequestMessageExtensions.SetBrowserRequestCredentials%2A> extension method:</span></span>

```csharp
requestMessage.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
```

<span data-ttu-id="597c4-163">如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="597c4-163">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="597c4-164">在 CORS 要求上傳送認證（授權 cookie/標頭）時， `Authorization` cors 原則必須允許標頭。</span><span class="sxs-lookup"><span data-stu-id="597c4-164">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="597c4-165">下列原則包含的設定：</span><span class="sxs-lookup"><span data-stu-id="597c4-165">The following policy includes configuration for:</span></span>

* <span data-ttu-id="597c4-166">要求來源（ `http://localhost:5000` 、 `https://localhost:5001` ）。</span><span class="sxs-lookup"><span data-stu-id="597c4-166">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="597c4-167">Any 方法（動詞）。</span><span class="sxs-lookup"><span data-stu-id="597c4-167">Any method (verb).</span></span>
* <span data-ttu-id="597c4-168">`Content-Type`和 `Authorization` 標頭。</span><span class="sxs-lookup"><span data-stu-id="597c4-168">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="597c4-169">若要允許自訂標頭（例如 `x-custom-header` ），請在呼叫時列出標頭 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-169">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="597c4-170">用戶端 JavaScript 程式碼（ `credentials` 屬性設為）所設定的認證 `include` 。</span><span class="sxs-lookup"><span data-stu-id="597c4-170">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="597c4-171">如需詳細資訊，請參閱 <xref:security/cors> 和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。</span><span class="sxs-lookup"><span data-stu-id="597c4-171">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="handle-token-request-errors"></a><span data-ttu-id="597c4-172">處理權杖要求錯誤</span><span class="sxs-lookup"><span data-stu-id="597c4-172">Handle token request errors</span></span>

<span data-ttu-id="597c4-173">當單一頁面應用程式（SPA）使用 Open ID Connect （OIDC）來驗證使用者時，驗證狀態會在 SPA 和 Identity 提供者（IP）中的本機維護，其格式為使用者提供其認證所設定的會話 cookie。</span><span class="sxs-lookup"><span data-stu-id="597c4-173">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="597c4-174">IP 為使用者發出的權杖通常會在短時間內有效，大約一小時，因此用戶端應用程式必須定期提取新的權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-174">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="597c4-175">否則，在授與的權杖過期之後，使用者會被登出。</span><span class="sxs-lookup"><span data-stu-id="597c4-175">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="597c4-176">在大多數情況下，OIDC 用戶端可以布建新的權杖，而不需要使用者重新驗證，因為它會保留在 IP 內的驗證狀態或「會話」。</span><span class="sxs-lookup"><span data-stu-id="597c4-176">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="597c4-177">在某些情況下，用戶端無法在沒有使用者互動的情況下取得權杖，例如，基於某些原因，使用者明確地登出了 IP。</span><span class="sxs-lookup"><span data-stu-id="597c4-177">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="597c4-178">如果使用者造訪和登出，就會發生這種情況 `https://login.microsoftonline.com` 。在這些情況下，應用程式不會立即得知使用者是否已登出。用戶端持有的任何權杖可能不再有效。</span><span class="sxs-lookup"><span data-stu-id="597c4-178">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="597c4-179">此外，用戶端無法在目前權杖過期之後，不需要使用者互動就布建新權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-179">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="597c4-180">這些案例並不是以權杖為基礎的驗證所特有。</span><span class="sxs-lookup"><span data-stu-id="597c4-180">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="597c4-181">它們屬於 Spa 的本質。</span><span class="sxs-lookup"><span data-stu-id="597c4-181">They are part of the nature of SPAs.</span></span> <span data-ttu-id="597c4-182">如果移除驗證 cookie，使用 cookie 的 SPA 也無法呼叫伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="597c4-182">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="597c4-183">當應用程式對受保護的資源執行 API 呼叫時，您必須注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="597c4-183">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="597c4-184">若要布建新的存取權杖以呼叫 API，使用者可能需要再次進行驗證。</span><span class="sxs-lookup"><span data-stu-id="597c4-184">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="597c4-185">即使用戶端的權杖看似有效，對伺服器的呼叫可能會失敗，因為使用者已撤銷權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-185">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="597c4-186">當應用程式要求權杖時，會有兩種可能的結果：</span><span class="sxs-lookup"><span data-stu-id="597c4-186">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="597c4-187">要求成功，且應用程式具有有效的權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-187">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="597c4-188">要求失敗，且應用程式必須再次驗證使用者，才能取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-188">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="597c4-189">當令牌要求失敗時，您必須決定是否要在執行重新導向之前，先儲存任何目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-189">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="597c4-190">有數種方法存在，並增加複雜性層級：</span><span class="sxs-lookup"><span data-stu-id="597c4-190">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="597c4-191">將目前的頁面狀態儲存在會話儲存體中。</span><span class="sxs-lookup"><span data-stu-id="597c4-191">Store the current page state in session storage.</span></span> <span data-ttu-id="597c4-192">在[OnInitializedAsync 生命週期事件](xref:blazor/lifecycle#component-initialization-methods)（ <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> ）期間，檢查是否可以還原狀態，再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="597c4-192">During the [OnInitializedAsync lifecycle event](xref:blazor/lifecycle#component-initialization-methods) (<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>), check if state can be restored before continuing.</span></span>
* <span data-ttu-id="597c4-193">新增查詢字串參數，並使用它來通知應用程式它需要重新序列化先前儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-193">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="597c4-194">新增具有唯一識別碼的查詢字串參數，以將資料儲存在會話儲存體中，而不會有風險與其他專案衝突。</span><span class="sxs-lookup"><span data-stu-id="597c4-194">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="597c4-195">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="597c4-195">The following example shows how to:</span></span>

* <span data-ttu-id="597c4-196">在重新導向至登入頁面之前保留狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-196">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="597c4-197">使用查詢字串參數，在驗證之後復原先前的狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-197">Recover the previous state afterward authentication using the query string parameter.</span></span>

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

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="597c4-198">在驗證操作之前儲存應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="597c4-198">Save app state before an authentication operation</span></span>

<span data-ttu-id="597c4-199">在驗證作業期間，某些情況下，您會想要在瀏覽器重新導向至 IP 之前，先儲存應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-199">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="597c4-200">當您使用類似狀態容器的情況，而且您想要在驗證成功之後還原狀態時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="597c4-200">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="597c4-201">您可以使用自訂驗證狀態物件來保留應用程式特定狀態或其參考，並在驗證作業成功完成後還原該狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-201">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="597c4-202">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-202">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

## <a name="customize-app-routes"></a><span data-ttu-id="597c4-203">自訂應用程式路由</span><span class="sxs-lookup"><span data-stu-id="597c4-203">Customize app routes</span></span>

<span data-ttu-id="597c4-204">根據預設， [AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)會使用下表所示的路由來代表不同的驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="597c4-204">By default, the [Microsoft.AspNetCore.Components.WebAssembly.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="597c4-205">路由</span><span class="sxs-lookup"><span data-stu-id="597c4-205">Route</span></span>                            | <span data-ttu-id="597c4-206">目的</span><span class="sxs-lookup"><span data-stu-id="597c4-206">Purpose</span></span> |
| ---
<span data-ttu-id="597c4-207">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-207">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-208">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-208">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-209">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-209">'Blazor'</span></span>
- <span data-ttu-id="597c4-210">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-210">'Identity'</span></span>
- <span data-ttu-id="597c4-211">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-211">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-212">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-212">'Razor'</span></span>
- <span data-ttu-id="597c4-213">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-213">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-214">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-214">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-215">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-215">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-216">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-216">'Blazor'</span></span>
- <span data-ttu-id="597c4-217">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-217">'Identity'</span></span>
- <span data-ttu-id="597c4-218">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-218">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-219">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-219">'Razor'</span></span>
- <span data-ttu-id="597c4-220">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-220">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-221">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-221">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-222">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-222">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-223">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-223">'Blazor'</span></span>
- <span data-ttu-id="597c4-224">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-224">'Identity'</span></span>
- <span data-ttu-id="597c4-225">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-225">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-226">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-226">'Razor'</span></span>
- <span data-ttu-id="597c4-227">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-227">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-228">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-228">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-229">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-229">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-230">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-230">'Blazor'</span></span>
- <span data-ttu-id="597c4-231">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-231">'Identity'</span></span>
- <span data-ttu-id="597c4-232">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-232">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-233">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-233">'Razor'</span></span>
- <span data-ttu-id="597c4-234">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-234">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-235">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-235">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-236">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-236">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-237">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-237">'Blazor'</span></span>
- <span data-ttu-id="597c4-238">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-238">'Identity'</span></span>
- <span data-ttu-id="597c4-239">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-239">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-240">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-240">'Razor'</span></span>
- <span data-ttu-id="597c4-241">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-241">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-242">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-242">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-243">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-243">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-244">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-244">'Blazor'</span></span>
- <span data-ttu-id="597c4-245">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-245">'Identity'</span></span>
- <span data-ttu-id="597c4-246">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-246">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-247">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-247">'Razor'</span></span>
- <span data-ttu-id="597c4-248">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-248">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-249">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-249">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-250">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-250">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-251">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-251">'Blazor'</span></span>
- <span data-ttu-id="597c4-252">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-252">'Identity'</span></span>
- <span data-ttu-id="597c4-253">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-253">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-254">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-254">'Razor'</span></span>
- <span data-ttu-id="597c4-255">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-255">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-256">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-256">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-257">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-257">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-258">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-258">'Blazor'</span></span>
- <span data-ttu-id="597c4-259">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-259">'Identity'</span></span>
- <span data-ttu-id="597c4-260">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-260">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-261">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-261">'Razor'</span></span>
- <span data-ttu-id="597c4-262">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-262">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-263">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-263">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-264">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-264">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-265">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-265">'Blazor'</span></span>
- <span data-ttu-id="597c4-266">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-266">'Identity'</span></span>
- <span data-ttu-id="597c4-267">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-267">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-268">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-268">'Razor'</span></span>
- <span data-ttu-id="597c4-269">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-269">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-270">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-270">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-271">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-271">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-272">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-272">'Blazor'</span></span>
- <span data-ttu-id="597c4-273">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-273">'Identity'</span></span>
- <span data-ttu-id="597c4-274">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-274">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-275">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-275">'Razor'</span></span>
- <span data-ttu-id="597c4-276">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-276">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-277">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-277">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-278">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-278">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-279">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-279">'Blazor'</span></span>
- <span data-ttu-id="597c4-280">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-280">'Identity'</span></span>
- <span data-ttu-id="597c4-281">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-281">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-282">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-282">'Razor'</span></span>
- <span data-ttu-id="597c4-283">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-283">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-284">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-284">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-285">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-285">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-286">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-286">'Blazor'</span></span>
- <span data-ttu-id="597c4-287">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-287">'Identity'</span></span>
- <span data-ttu-id="597c4-288">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-288">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-289">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-289">'Razor'</span></span>
- <span data-ttu-id="597c4-290">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-290">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-291">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-291">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-292">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-292">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-293">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-293">'Blazor'</span></span>
- <span data-ttu-id="597c4-294">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-294">'Identity'</span></span>
- <span data-ttu-id="597c4-295">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-295">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-296">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-296">'Razor'</span></span>
- <span data-ttu-id="597c4-297">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-297">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-298">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-298">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-299">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-299">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-300">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-300">'Blazor'</span></span>
- <span data-ttu-id="597c4-301">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-301">'Identity'</span></span>
- <span data-ttu-id="597c4-302">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-302">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-303">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-303">'Razor'</span></span>
- <span data-ttu-id="597c4-304">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-304">'SignalR' uid:</span></span> 

<span data-ttu-id="597c4-305">---------------- |---標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-305">---------------- | --- title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-306">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-306">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-307">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-307">'Blazor'</span></span>
- <span data-ttu-id="597c4-308">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-308">'Identity'</span></span>
- <span data-ttu-id="597c4-309">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-309">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-310">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-310">'Razor'</span></span>
- <span data-ttu-id="597c4-311">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-311">'SignalR' uid:</span></span> 

<span data-ttu-id="597c4-312">---- | |`authentication/login`           |觸發登入作業。</span><span class="sxs-lookup"><span data-stu-id="597c4-312">---- | | `authentication/login`           | Triggers a sign-in operation.</span></span> <span data-ttu-id="597c4-313">| |`authentication/login-callback`  |處理任何登入作業的結果。</span><span class="sxs-lookup"><span data-stu-id="597c4-313">| | `authentication/login-callback`  | Handles the result of any sign-in operation.</span></span> <span data-ttu-id="597c4-314">| |`authentication/login-failed`    |當登入作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="597c4-314">| | `authentication/login-failed`    | Displays error messages when the sign-in operation fails for some reason.</span></span> <span data-ttu-id="597c4-315">| |`authentication/logout`          |觸發登出作業。</span><span class="sxs-lookup"><span data-stu-id="597c4-315">| | `authentication/logout`          | Triggers a sign-out operation.</span></span> <span data-ttu-id="597c4-316">| |`authentication/logout-callback` |處理登出作業的結果。</span><span class="sxs-lookup"><span data-stu-id="597c4-316">| | `authentication/logout-callback` | Handles the result of a sign-out operation.</span></span> <span data-ttu-id="597c4-317">| |`authentication/logout-failed`   |當登出作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="597c4-317">| | `authentication/logout-failed`   | Displays error messages when the sign-out operation fails for some reason.</span></span> <span data-ttu-id="597c4-318">| |`authentication/logged-out`      |表示使用者已成功登出。</span><span class="sxs-lookup"><span data-stu-id="597c4-318">| | `authentication/logged-out`      | Indicates that the user has successfully logout.</span></span> <span data-ttu-id="597c4-319">| |`authentication/profile`         |觸發操作以編輯使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="597c4-319">| | `authentication/profile`         | Triggers an operation to edit the user profile.</span></span> <span data-ttu-id="597c4-320">| |`authentication/register`        |觸發操作以註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="597c4-320">| | `authentication/register`        | Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="597c4-321">上表中顯示的路由可透過來設定 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticationOptions%601.AuthenticationPaths%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-321">The routes shown in the preceding table are configurable via <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticationOptions%601.AuthenticationPaths%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="597c4-322">設定選項以提供自訂路由時，請確認應用程式具有處理每個路徑的路由。</span><span class="sxs-lookup"><span data-stu-id="597c4-322">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="597c4-323">在下列範例中，所有路徑的前面都會加上 `/security` 。</span><span class="sxs-lookup"><span data-stu-id="597c4-323">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="597c4-324">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-324">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="597c4-325">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-325">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="597c4-326">如果需求會呼叫完全不同的路徑，請如先前所述設定路由，並 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> 使用明確的 action 參數來呈現：</span><span class="sxs-lookup"><span data-stu-id="597c4-326">If the requirement calls for completely different paths, set the routes as described previously and render the <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="597c4-327">如果您選擇這樣做，則可以將 UI 分成不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="597c4-327">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="597c4-328">自訂驗證使用者介面</span><span class="sxs-lookup"><span data-stu-id="597c4-328">Customize the authentication user interface</span></span>

<span data-ttu-id="597c4-329"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView>針對每個驗證狀態包含一組預設的 UI 元件。</span><span class="sxs-lookup"><span data-stu-id="597c4-329"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="597c4-330">您可以藉由傳入自訂來自訂每個狀態 <xref:Microsoft.AspNetCore.Components.RenderFragment> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-330">Each state can be customized by passing in a custom <xref:Microsoft.AspNetCore.Components.RenderFragment>.</span></span> <span data-ttu-id="597c4-331">若要在初始登入程式期間自訂顯示的文字，可以變更，如下所示 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> 。</span><span class="sxs-lookup"><span data-stu-id="597c4-331">To customize the displayed text during the initial login process, can change the <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> as follows.</span></span>

<span data-ttu-id="597c4-332">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="597c4-332">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

<span data-ttu-id="597c4-333"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView>有一個片段，可用於下表所示的每個驗證路由。</span><span class="sxs-lookup"><span data-stu-id="597c4-333">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="597c4-334">路由</span><span class="sxs-lookup"><span data-stu-id="597c4-334">Route</span></span>                            | <span data-ttu-id="597c4-335">片段</span><span class="sxs-lookup"><span data-stu-id="597c4-335">Fragment</span></span>                |
| ---
<span data-ttu-id="597c4-336">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-336">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-337">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-337">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-338">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-338">'Blazor'</span></span>
- <span data-ttu-id="597c4-339">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-339">'Identity'</span></span>
- <span data-ttu-id="597c4-340">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-340">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-341">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-341">'Razor'</span></span>
- <span data-ttu-id="597c4-342">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-342">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-343">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-343">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-344">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-344">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-345">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-345">'Blazor'</span></span>
- <span data-ttu-id="597c4-346">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-346">'Identity'</span></span>
- <span data-ttu-id="597c4-347">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-347">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-348">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-348">'Razor'</span></span>
- <span data-ttu-id="597c4-349">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-349">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-350">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-350">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-351">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-351">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-352">'Blazor'</span></span>
- <span data-ttu-id="597c4-353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-353">'Identity'</span></span>
- <span data-ttu-id="597c4-354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-354">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-355">'Razor'</span></span>
- <span data-ttu-id="597c4-356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-357">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-357">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-358">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-358">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-359">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-359">'Blazor'</span></span>
- <span data-ttu-id="597c4-360">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-360">'Identity'</span></span>
- <span data-ttu-id="597c4-361">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-361">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-362">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-362">'Razor'</span></span>
- <span data-ttu-id="597c4-363">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-363">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-364">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-364">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-365">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-365">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-366">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-366">'Blazor'</span></span>
- <span data-ttu-id="597c4-367">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-367">'Identity'</span></span>
- <span data-ttu-id="597c4-368">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-368">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-369">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-369">'Razor'</span></span>
- <span data-ttu-id="597c4-370">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-370">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-371">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-371">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-372">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-372">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-373">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-373">'Blazor'</span></span>
- <span data-ttu-id="597c4-374">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-374">'Identity'</span></span>
- <span data-ttu-id="597c4-375">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-375">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-376">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-376">'Razor'</span></span>
- <span data-ttu-id="597c4-377">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-377">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-378">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-378">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-379">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-379">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-380">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-380">'Blazor'</span></span>
- <span data-ttu-id="597c4-381">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-381">'Identity'</span></span>
- <span data-ttu-id="597c4-382">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-382">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-383">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-383">'Razor'</span></span>
- <span data-ttu-id="597c4-384">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-384">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-385">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-385">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-386">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-386">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-387">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-387">'Blazor'</span></span>
- <span data-ttu-id="597c4-388">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-388">'Identity'</span></span>
- <span data-ttu-id="597c4-389">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-389">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-390">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-390">'Razor'</span></span>
- <span data-ttu-id="597c4-391">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-391">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-392">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-392">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-393">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-393">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-394">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-394">'Blazor'</span></span>
- <span data-ttu-id="597c4-395">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-395">'Identity'</span></span>
- <span data-ttu-id="597c4-396">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-396">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-397">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-397">'Razor'</span></span>
- <span data-ttu-id="597c4-398">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-398">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-399">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-399">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-400">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-400">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-401">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-401">'Blazor'</span></span>
- <span data-ttu-id="597c4-402">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-402">'Identity'</span></span>
- <span data-ttu-id="597c4-403">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-403">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-404">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-404">'Razor'</span></span>
- <span data-ttu-id="597c4-405">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-405">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-406">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-406">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-407">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-407">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-408">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-408">'Blazor'</span></span>
- <span data-ttu-id="597c4-409">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-409">'Identity'</span></span>
- <span data-ttu-id="597c4-410">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-410">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-411">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-411">'Razor'</span></span>
- <span data-ttu-id="597c4-412">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-412">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-413">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-413">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-414">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-414">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-415">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-415">'Blazor'</span></span>
- <span data-ttu-id="597c4-416">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-416">'Identity'</span></span>
- <span data-ttu-id="597c4-417">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-417">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-418">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-418">'Razor'</span></span>
- <span data-ttu-id="597c4-419">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-419">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-420">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-420">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-421">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-421">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-422">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-422">'Blazor'</span></span>
- <span data-ttu-id="597c4-423">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-423">'Identity'</span></span>
- <span data-ttu-id="597c4-424">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-424">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-425">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-425">'Razor'</span></span>
- <span data-ttu-id="597c4-426">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-426">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-427">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-427">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-428">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-428">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-429">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-429">'Blazor'</span></span>
- <span data-ttu-id="597c4-430">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-430">'Identity'</span></span>
- <span data-ttu-id="597c4-431">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-431">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-432">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-432">'Razor'</span></span>
- <span data-ttu-id="597c4-433">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-433">'SignalR' uid:</span></span> 

<span data-ttu-id="597c4-434">---------------- |---標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-434">---------------- | --- title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-435">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-435">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-436">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-436">'Blazor'</span></span>
- <span data-ttu-id="597c4-437">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-437">'Identity'</span></span>
- <span data-ttu-id="597c4-438">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-438">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-439">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-439">'Razor'</span></span>
- <span data-ttu-id="597c4-440">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-440">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-441">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-441">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-442">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-442">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-443">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-443">'Blazor'</span></span>
- <span data-ttu-id="597c4-444">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-444">'Identity'</span></span>
- <span data-ttu-id="597c4-445">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-445">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-446">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-446">'Razor'</span></span>
- <span data-ttu-id="597c4-447">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-447">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-448">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-448">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-449">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-449">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-450">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-450">'Blazor'</span></span>
- <span data-ttu-id="597c4-451">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-451">'Identity'</span></span>
- <span data-ttu-id="597c4-452">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-452">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-453">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-453">'Razor'</span></span>
- <span data-ttu-id="597c4-454">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-454">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-455">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-455">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-456">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-456">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-457">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-457">'Blazor'</span></span>
- <span data-ttu-id="597c4-458">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-458">'Identity'</span></span>
- <span data-ttu-id="597c4-459">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-459">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-460">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-460">'Razor'</span></span>
- <span data-ttu-id="597c4-461">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-461">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-462">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-462">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-463">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-463">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-464">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-464">'Blazor'</span></span>
- <span data-ttu-id="597c4-465">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-465">'Identity'</span></span>
- <span data-ttu-id="597c4-466">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-466">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-467">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-467">'Razor'</span></span>
- <span data-ttu-id="597c4-468">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-468">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-469">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-469">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-470">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-470">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-471">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-471">'Blazor'</span></span>
- <span data-ttu-id="597c4-472">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-472">'Identity'</span></span>
- <span data-ttu-id="597c4-473">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-473">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-474">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-474">'Razor'</span></span>
- <span data-ttu-id="597c4-475">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-475">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-476">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-476">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-477">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-477">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-478">'Blazor'</span></span>
- <span data-ttu-id="597c4-479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-479">'Identity'</span></span>
- <span data-ttu-id="597c4-480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-480">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-481">'Razor'</span></span>
- <span data-ttu-id="597c4-482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-483">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-483">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-484">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-484">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-485">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-485">'Blazor'</span></span>
- <span data-ttu-id="597c4-486">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-486">'Identity'</span></span>
- <span data-ttu-id="597c4-487">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-487">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-488">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-488">'Razor'</span></span>
- <span data-ttu-id="597c4-489">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-489">'SignalR' uid:</span></span> 

-
<span data-ttu-id="597c4-490">標題：「ASP.NET Core Blazor WebAssembly 其他安全性案例的作者：描述：」瞭解如何設定 Blazor WebAssembly 以取得其他安全性案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-490">title: 'ASP.NET Core Blazor WebAssembly additional security scenarios' author: description: 'Learn how to configure Blazor WebAssembly for additional security scenarios.'</span></span>
<span data-ttu-id="597c4-491">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="597c4-491">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="597c4-492">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="597c4-492">'Blazor'</span></span>
- <span data-ttu-id="597c4-493">'Identity'</span><span class="sxs-lookup"><span data-stu-id="597c4-493">'Identity'</span></span>
- <span data-ttu-id="597c4-494">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="597c4-494">'Let's Encrypt'</span></span>
- <span data-ttu-id="597c4-495">'Razor'</span><span class="sxs-lookup"><span data-stu-id="597c4-495">'Razor'</span></span>
- <span data-ttu-id="597c4-496">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="597c4-496">'SignalR' uid:</span></span> 

<span data-ttu-id="597c4-497">------------ | | `authentication/login`           | `<LoggingIn>`           | | `authentication/login-callback`  | `<CompletingLoggingIn>` | | `authentication/login-failed`    | `<LogInFailed>`         | | `authentication/logout`          | `<LogOut>`              | | `authentication/logout-callback` | `<CompletingLogOut>`    | | `authentication/logout-failed`   | `<LogOutFailed>`        | | `authentication/logged-out`      | `<LogOutSucceeded>`     | | `authentication/profile`         | `<UserProfile>`         | | `authentication/register`        | `<Registering>`         |</span><span class="sxs-lookup"><span data-stu-id="597c4-497">------------ | | `authentication/login`           | `<LoggingIn>`           | | `authentication/login-callback`  | `<CompletingLoggingIn>` | | `authentication/login-failed`    | `<LogInFailed>`         | | `authentication/logout`          | `<LogOut>`              | | `authentication/logout-callback` | `<CompletingLogOut>`    | | `authentication/logout-failed`   | `<LogOutFailed>`        | | `authentication/logged-out`      | `<LogOutSucceeded>`     | | `authentication/profile`         | `<UserProfile>`         | | `authentication/register`        | `<Registering>`         |</span></span>

## <a name="customize-the-user"></a><span data-ttu-id="597c4-498">自訂使用者</span><span class="sxs-lookup"><span data-stu-id="597c4-498">Customize the user</span></span>

<span data-ttu-id="597c4-499">可以自訂系結至應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="597c4-499">Users bound to the app can be customized.</span></span> <span data-ttu-id="597c4-500">在下列範例中，所有已驗證的使用者都會收到 `amr` 每個使用者驗證方法的宣告。</span><span class="sxs-lookup"><span data-stu-id="597c4-500">In the following example, all authenticated users receive an `amr` claim for each of the user's authentication methods.</span></span>

<span data-ttu-id="597c4-501">建立擴充類別的類別 <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteUserAccount> ：</span><span class="sxs-lookup"><span data-stu-id="597c4-501">Create a class that extends the <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteUserAccount> class:</span></span>

```csharp
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class CustomUserAccount : RemoteUserAccount
{
    [JsonPropertyName("amr")]
    public string[] AuthenticationMethod { get; set; }
}
```

<span data-ttu-id="597c4-502">建立可延伸的 factory <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccountClaimsPrincipalFactory%601> ：</span><span class="sxs-lookup"><span data-stu-id="597c4-502">Create a factory that extends <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.AccountClaimsPrincipalFactory%601>:</span></span>

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

<span data-ttu-id="597c4-503">`CustomAccountFactory`為使用中的驗證提供者註冊。</span><span class="sxs-lookup"><span data-stu-id="597c4-503">Register the `CustomAccountFactory` for the authentication provider in use.</span></span> <span data-ttu-id="597c4-504">下列任何一項註冊都是有效的：</span><span class="sxs-lookup"><span data-stu-id="597c4-504">Any of the following registrations are valid:</span></span> 

* <span data-ttu-id="597c4-505"><xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A>:</span><span class="sxs-lookup"><span data-stu-id="597c4-505"><xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddOidcAuthentication%2A>:</span></span>

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

* <span data-ttu-id="597c4-506"><xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>:</span><span class="sxs-lookup"><span data-stu-id="597c4-506"><xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>:</span></span>

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
  
* <span data-ttu-id="597c4-507"><xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddApiAuthorization%2A>:</span><span class="sxs-lookup"><span data-stu-id="597c4-507"><xref:Microsoft.Extensions.DependencyInjection.WebAssemblyAuthenticationServiceCollectionExtensions.AddApiAuthorization%2A>:</span></span>

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

## <a name="support-prerendering-with-authentication"></a><span data-ttu-id="597c4-508">支援使用驗證來進行預呈現</span><span class="sxs-lookup"><span data-stu-id="597c4-508">Support prerendering with authentication</span></span>

<span data-ttu-id="597c4-509">遵循其中一個託管 WebAssembly 應用程式主題中的指導方針之後 Blazor ，請使用下列指示來建立應用程式：</span><span class="sxs-lookup"><span data-stu-id="597c4-509">After following the guidance in one of the hosted Blazor WebAssembly app topics, use the following instructions to create an app that:</span></span>

* <span data-ttu-id="597c4-510">不需要授權的 Prerenders 路徑。</span><span class="sxs-lookup"><span data-stu-id="597c4-510">Prerenders paths for which authorization isn't required.</span></span>
* <span data-ttu-id="597c4-511">不需要授權的已呈現路徑。</span><span class="sxs-lookup"><span data-stu-id="597c4-511">Doesn't prerender paths for which authorization is required.</span></span>

<span data-ttu-id="597c4-512">在用戶端應用程式的 `Program` 類別（*Program.cs*）中，將常見的服務註冊因素劃分為不同的方法（例如， `ConfigureCommonServices` ）：</span><span class="sxs-lookup"><span data-stu-id="597c4-512">In the Client app's `Program` class (*Program.cs*), factor common service registrations into a separate method (for example, `ConfigureCommonServices`):</span></span>

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

<span data-ttu-id="597c4-513">在伺服器應用程式的中 `Startup.ConfigureServices` ，註冊下列其他服務：</span><span class="sxs-lookup"><span data-stu-id="597c4-513">In the Server app's `Startup.ConfigureServices`, register the following additional services:</span></span>

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

<span data-ttu-id="597c4-514">在伺服器應用程式的 `Startup.Configure` 方法中，取代[端點。具有端點的 MapFallbackToFile （"index. html"）](xref:Microsoft.AspNetCore.Builder.StaticFilesEndpointRouteBuilderExtensions.MapFallbackToFile%2A) [。MapFallbackToPage （"/_Host"）](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A)：</span><span class="sxs-lookup"><span data-stu-id="597c4-514">In the Server app's `Startup.Configure` method, replace [endpoints.MapFallbackToFile("index.html")](xref:Microsoft.AspNetCore.Builder.StaticFilesEndpointRouteBuilderExtensions.MapFallbackToFile%2A) with [endpoints.MapFallbackToPage("/_Host")](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A):</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

<span data-ttu-id="597c4-515">在伺服器應用程式中，建立*Pages*資料夾（如果不存在）。</span><span class="sxs-lookup"><span data-stu-id="597c4-515">In the Server app, create a *Pages* folder if it doesn't exist.</span></span> <span data-ttu-id="597c4-516">在伺服器應用程式的*Pages*資料夾內建立 *_Host 的 cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="597c4-516">Create a *_Host.cshtml* page inside the Server app's *Pages* folder.</span></span> <span data-ttu-id="597c4-517">將用戶端應用程式*wwwroot/index.html*檔的內容貼到*Pages/_Host. cshtml*檔案中。</span><span class="sxs-lookup"><span data-stu-id="597c4-517">Paste the contents from the Client app's *wwwroot/index.html* file into the *Pages/_Host.cshtml* file.</span></span> <span data-ttu-id="597c4-518">更新檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="597c4-518">Update the file's contents:</span></span>

* <span data-ttu-id="597c4-519">將 `@page "_Host"` 新增到檔案的頂端。</span><span class="sxs-lookup"><span data-stu-id="597c4-519">Add `@page "_Host"` to the top of the file.</span></span>
* <span data-ttu-id="597c4-520">將標記取代為 `<app>Loading...</app>` 下列內容：</span><span class="sxs-lookup"><span data-stu-id="597c4-520">Replace the `<app>Loading...</app>` tag with the following:</span></span>

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
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a><span data-ttu-id="597c4-521">託管應用程式和協力廠商登入提供者的選項</span><span class="sxs-lookup"><span data-stu-id="597c4-521">Options for hosted apps and third-party login providers</span></span>

<span data-ttu-id="597c4-522">Blazor使用協力廠商提供者驗證和授權託管 WebAssembly 應用程式時，有數個選項可用來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="597c4-522">When authenticating and authorizing a hosted Blazor WebAssembly app with a third-party provider, there are several options available for authenticating the user.</span></span> <span data-ttu-id="597c4-523">您選擇哪一個取決於您的案例。</span><span class="sxs-lookup"><span data-stu-id="597c4-523">Which one you choose depends on your scenario.</span></span>

<span data-ttu-id="597c4-524">如需詳細資訊，請參閱<xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="597c4-524">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a><span data-ttu-id="597c4-525">驗證使用者只呼叫受保護的協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="597c4-525">Authenticate users to only call protected third party APIs</span></span>

<span data-ttu-id="597c4-526">對協力廠商 API 提供者的用戶端 OAuth 流程驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="597c4-526">Authenticate the user with a client-side OAuth flow against the third-party API provider:</span></span>

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 <span data-ttu-id="597c4-527">在此情節中：</span><span class="sxs-lookup"><span data-stu-id="597c4-527">In this scenario:</span></span>

* <span data-ttu-id="597c4-528">裝載應用程式的伺服器不扮演角色。</span><span class="sxs-lookup"><span data-stu-id="597c4-528">The server hosting the app doesn't play a role.</span></span>
* <span data-ttu-id="597c4-529">無法保護伺服器上的 Api。</span><span class="sxs-lookup"><span data-stu-id="597c4-529">APIs on the server can't be protected.</span></span>
* <span data-ttu-id="597c4-530">應用程式只能呼叫受保護的協力廠商 Api。</span><span class="sxs-lookup"><span data-stu-id="597c4-530">The app can only call protected third-party APIs.</span></span>

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a><span data-ttu-id="597c4-531">使用協力廠商提供者來驗證使用者，並在主機伺服器和協力廠商上呼叫受保護的 Api</span><span class="sxs-lookup"><span data-stu-id="597c4-531">Authenticate users with a third-party provider and call protected APIs on the host server and the third party</span></span>

<span data-ttu-id="597c4-532">Identity使用協力廠商登入提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="597c4-532">Configure Identity with a third-party login provider.</span></span> <span data-ttu-id="597c4-533">取得協力廠商 API 存取所需的權杖，並加以儲存。</span><span class="sxs-lookup"><span data-stu-id="597c4-533">Obtain the tokens required for third-party API access and store them.</span></span>

<span data-ttu-id="597c4-534">當使用者登入時，會 Identity 收集存取權並重新整理權杖，做為驗證程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="597c4-534">When a user logs in, Identity collects access and refresh tokens as part of the authentication process.</span></span> <span data-ttu-id="597c4-535">此時，有幾個方法可用來對協力廠商 Api 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="597c4-535">At that point, there are a couple of approaches available for making API calls to third-party APIs.</span></span>

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a><span data-ttu-id="597c4-536">使用伺服器存取權杖來取出協力廠商存取權杖</span><span class="sxs-lookup"><span data-stu-id="597c4-536">Use a server access token to retrieve the third-party access token</span></span>

<span data-ttu-id="597c4-537">使用伺服器上產生的存取權杖，從伺服器 API 端點抓取協力廠商存取權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-537">Use the access token generated on the server to retrieve the third-party access token from a server API endpoint.</span></span> <span data-ttu-id="597c4-538">從該處，使用協力廠商存取權杖，直接從用戶端上呼叫協力廠商 API 資源 Identity 。</span><span class="sxs-lookup"><span data-stu-id="597c4-538">From there, use the third-party access token to call third-party API resources directly from Identity on the client.</span></span>

<span data-ttu-id="597c4-539">我們不建議採用這種方法。</span><span class="sxs-lookup"><span data-stu-id="597c4-539">We don't recommend this approach.</span></span> <span data-ttu-id="597c4-540">這種方法需要將協力廠商存取權杖視為針對公用用戶端所產生。</span><span class="sxs-lookup"><span data-stu-id="597c4-540">This approach requires treating the third-party access token as if it were generated for a public client.</span></span> <span data-ttu-id="597c4-541">在 OAuth 詞彙中，公用應用程式不會有用戶端密碼，因為它無法受信任而無法安全地儲存秘密，而且會為機密用戶端產生存取權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-541">In OAuth terms, the public app doesn't have a client secret because it can't be trusted to store secrets safely, and the access token is produced for a confidential client.</span></span> <span data-ttu-id="597c4-542">機密用戶端是具有用戶端密碼的用戶端，並假設能夠安全地儲存秘密。</span><span class="sxs-lookup"><span data-stu-id="597c4-542">A confidential client is a client that has a client secret and is assumed to be able to safely store secrets.</span></span>

* <span data-ttu-id="597c4-543">協力廠商存取權杖可能會被授與額外的範圍來執行敏感性作業，這是根據協力廠商針對較受信任的用戶端發出權杖的事實。</span><span class="sxs-lookup"><span data-stu-id="597c4-543">The third-party access token might be granted additional scopes to perform sensitive operations based on the fact that the third-party emitted the token for a more trusted client.</span></span>
* <span data-ttu-id="597c4-544">同樣地，重新整理權杖不應發給不受信任的用戶端，因為這樣做會讓用戶端無限制存取，除非有其他限制。</span><span class="sxs-lookup"><span data-stu-id="597c4-544">Similarly, refresh tokens shouldn't be issued to a client that isn't trusted, as doing so gives the client unlimited access unless other restrictions are put into place.</span></span>

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a><span data-ttu-id="597c4-545">從用戶端對伺服器 API 進行 API 呼叫，以便呼叫協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="597c4-545">Make API calls from the client to the server API in order to call third-party APIs</span></span>

<span data-ttu-id="597c4-546">從用戶端對伺服器 API 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="597c4-546">Make an API call from the client to the server API.</span></span> <span data-ttu-id="597c4-547">從伺服器中，取出協力廠商 API 資源的存取權杖，併發出任何需要的呼叫。</span><span class="sxs-lookup"><span data-stu-id="597c4-547">From the server, retrieve the access token for the third-party API resource and issue whatever call is necessary.</span></span>

<span data-ttu-id="597c4-548">雖然這種方法需要透過伺服器額外的網路躍點來呼叫協力廠商 API，但最終會導致更安全的體驗：</span><span class="sxs-lookup"><span data-stu-id="597c4-548">While this approach requires an extra network hop through the server to call a third-party API, it ultimately results in a safer experience:</span></span>

* <span data-ttu-id="597c4-549">伺服器可以儲存重新整理權杖，並確保應用程式不會失去協力廠商資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="597c4-549">The server can store refresh tokens and ensure that the app doesn't lose access to third-party resources.</span></span>
* <span data-ttu-id="597c4-550">應用程式無法從可能包含更多敏感性許可權的伺服器洩漏存取權杖。</span><span class="sxs-lookup"><span data-stu-id="597c4-550">The app can't leak access tokens from the server that might contain more sensitive permissions.</span></span>

## <a name="use-open-id-connect-oidc-v20-endpoints"></a><span data-ttu-id="597c4-551">使用 Open ID Connect （OIDC） v2.0 端點</span><span class="sxs-lookup"><span data-stu-id="597c4-551">Use Open ID Connect (OIDC) v2.0 endpoints</span></span>

<span data-ttu-id="597c4-552">驗證程式庫和 Blazor 範本會使用 OPEN ID Connect （OIDC） v1.0 端點。</span><span class="sxs-lookup"><span data-stu-id="597c4-552">The authentication library and Blazor templates use Open ID Connect (OIDC) v1.0 endpoints.</span></span> <span data-ttu-id="597c4-553">若要使用 v2.0 端點，請設定 JWT 持有人 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority?displayProperty=nameWithType> 選項。</span><span class="sxs-lookup"><span data-stu-id="597c4-553">To use a v2.0 endpoint, configure the JWT Bearer <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority?displayProperty=nameWithType> option.</span></span> <span data-ttu-id="597c4-554">在下列範例中，會將區段附加至屬性，以針對 v2.0 設定 AAD `v2.0` <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority> ：</span><span class="sxs-lookup"><span data-stu-id="597c4-554">In the following example, AAD is configured for v2.0 by appending a `v2.0` segment to the <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions.Authority> property:</span></span>

```csharp
builder.Services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, 
    options =>
    {
        options.Authority += "/v2.0";
    });
```

<span data-ttu-id="597c4-555">或者，您也可以在應用程式設定（*appsettings*）檔案中進行設定：</span><span class="sxs-lookup"><span data-stu-id="597c4-555">Alternatively, the setting can be made in the app settings (*appsettings.json*) file:</span></span>

```json
{
  "Local": {
    "Authority": "https://login.microsoftonline.com/common/oauth2/v2.0/",
    ...
  }
}
```

<span data-ttu-id="597c4-556">如果在區段上對授權單位的追蹤不適合應用程式的 OIDC 提供者（例如使用非 AAD 提供者），請 <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> 直接設定屬性。</span><span class="sxs-lookup"><span data-stu-id="597c4-556">If tacking on a segment to the authority isn't appropriate for the app's OIDC provider, such as with non-AAD providers, set the <xref:Microsoft.AspNetCore.Builder.OpenIdConnectOptions.Authority> property directly.</span></span> <span data-ttu-id="597c4-557">請在 <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> 應用程式佈建檔案（*appsettings*）中，使用金鑰設定的屬性 `Authority` 。</span><span class="sxs-lookup"><span data-stu-id="597c4-557">Either set the property in <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> or in the app settings file (*appsettings.json*) with the `Authority` key.</span></span>

<span data-ttu-id="597c4-558">針對 v2.0 端點，識別碼權杖中的宣告清單會變更。</span><span class="sxs-lookup"><span data-stu-id="597c4-558">The list of claims in the ID token changes for v2.0 endpoints.</span></span> <span data-ttu-id="597c4-559">如需詳細資訊，請參閱[為何要更新至 Microsoft 身分識別平臺（v2.0）？](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison)。</span><span class="sxs-lookup"><span data-stu-id="597c4-559">For more information, see [Why update to Microsoft identity platform (v2.0)?](/azure/active-directory/azuread-dev/azure-ad-endpoint-comparison).</span></span>
