---
title: ASP.NET Core Blazor WebAssembly 其他安全性案例
author: guardrex
description: 瞭解如何設定Blazor WebAssembly 以進行其他安全性案例。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: e69b598431027aa540227b87dedfd091057a1af4
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768165"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="44818-103">ASP.NET Core Blazor WebAssembly 其他安全性案例</span><span class="sxs-lookup"><span data-stu-id="44818-103">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="44818-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="44818-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="attach-tokens-to-outgoing-requests"></a><span data-ttu-id="44818-105">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="44818-105">Attach tokens to outgoing requests</span></span>

<span data-ttu-id="44818-106">此`AuthorizationMessageHandler`服務可與`HttpClient`搭配使用，將存取權杖附加至傳出要求。</span><span class="sxs-lookup"><span data-stu-id="44818-106">The `AuthorizationMessageHandler` service can be used with `HttpClient` to attach access tokens to outgoing requests.</span></span> <span data-ttu-id="44818-107">您可以使用現有`IAccessTokenProvider`的服務來取得權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-107">Tokens are acquired using the existing `IAccessTokenProvider` service.</span></span> <span data-ttu-id="44818-108">如果無法取得權杖， `AccessTokenNotAvailableException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="44818-108">If a token can't be acquired, an `AccessTokenNotAvailableException` is thrown.</span></span> <span data-ttu-id="44818-109">`AccessTokenNotAvailableException`具有`Redirect`方法，可以用來將使用者導覽至識別提供者，以取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-109">`AccessTokenNotAvailableException` has a `Redirect` method that can be used to navigate the user to the identity provider to acquire a new token.</span></span> <span data-ttu-id="44818-110">`AuthorizationMessageHandler`可以使用`ConfigureHandler`方法，透過授權的 url、範圍和傳回 URL 來設定。</span><span class="sxs-lookup"><span data-stu-id="44818-110">The `AuthorizationMessageHandler` can be configured with the authorized URLs, scopes, and return URL using the `ConfigureHandler` method.</span></span>

<span data-ttu-id="44818-111">在下列範例中， `AuthorizationMessageHandler`會`HttpClient`在（ `Program.Main` *Program.cs*）中設定：</span><span class="sxs-lookup"><span data-stu-id="44818-111">In the following example, `AuthorizationMessageHandler` configures an `HttpClient` in `Program.Main` (*Program.cs*):</span></span>

```csharp
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

<span data-ttu-id="44818-112">為了方便起見， `BaseAddressAuthorizationMessageHandler`會包含以應用程式基底位址預先設定為授權 URL 的。</span><span class="sxs-lookup"><span data-stu-id="44818-112">For convenience, a `BaseAddressAuthorizationMessageHandler` is included that's preconfigured with the app base address as an authorized URL.</span></span> <span data-ttu-id="44818-113">已啟用驗證的 Blazor WebAssembly 範本現在會使用[IHttpClientFactory](https://docs.microsoft.com/aspnet/core/fundamentals/http-requests)來設定`HttpClient`具有下列內容`BaseAddressAuthorizationMessageHandler`的：</span><span class="sxs-lookup"><span data-stu-id="44818-113">The authentication-enabled Blazor WebAssembly templates now use [IHttpClientFactory](https://docs.microsoft.com/aspnet/core/fundamentals/http-requests) to set up an `HttpClient` with the `BaseAddressAuthorizationMessageHandler`:</span></span>

```csharp
builder.Services.AddHttpClient("BlazorWithIdentityApp1.ServerAPI", 
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
        .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddTransient(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("BlazorWithIdentityApp1.ServerAPI"));
```

<span data-ttu-id="44818-114">在上述範例中，會`CreateClient`使用建立用戶端，而`HttpClient`在對伺服器專案提出要求時，會提供包含存取權杖的實例。</span><span class="sxs-lookup"><span data-stu-id="44818-114">Where the client is created with `CreateClient` in the preceding example, the `HttpClient` is supplied instances that include access tokens when making requests to the server project.</span></span>

<span data-ttu-id="44818-115">接著會`HttpClient`使用設定的，透過簡單`try-catch`模式來提出授權的要求。</span><span class="sxs-lookup"><span data-stu-id="44818-115">The configured `HttpClient` is then used to make authorized requests using a simple `try-catch` pattern.</span></span> <span data-ttu-id="44818-116">下列`FetchData`元件會要求氣象預報資料：</span><span class="sxs-lookup"><span data-stu-id="44818-116">The following `FetchData` component requests weather forecast data:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    try
    {
        forecasts = 
            await Http.GetFromJsonAsync<WeatherForecast[]>("WeatherForecast");
    }
    catch (AccessTokenNotAvailableException exception)
    {
        exception.Redirect();
    }
}
```

<span data-ttu-id="44818-117">或者，您可以定義具型別用戶端，以處理單一類別內所有的 HTTP 和權杖取得考慮：</span><span class="sxs-lookup"><span data-stu-id="44818-117">Alternatively, you can define a typed client that handles all of the HTTP and token acquisition concerns within a single class:</span></span>

<span data-ttu-id="44818-118">*WeatherClient.cs*：</span><span class="sxs-lookup"><span data-stu-id="44818-118">*WeatherClient.cs*:</span></span>

```csharp
public class WeatherClient
{
    private readonly HttpClient httpClient;
 
    public WeatherClient(HttpClient httpClient)
    {
        this.httpClient = httpClient;
    }
 
    public async Task<IEnumerable<WeatherForecast>> GetWeatherForeacasts()
    {
        IEnumerable<WeatherForecast> forecasts = new WeatherForecast[0];

        try
        {
            forecasts = await httpClient.GetFromJsonAsync<WeatherForecast[]>(
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

<span data-ttu-id="44818-119">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="44818-119">*Program.cs*:</span></span>

```csharp
builder.Services.AddHttpClient<WeatherClient>(
    client => client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();
```

<span data-ttu-id="44818-120">*FetchData razor*：</span><span class="sxs-lookup"><span data-stu-id="44818-120">*FetchData.razor*:</span></span>

```razor
@inject WeatherClient WeatherClient

...

protected override async Task OnInitializedAsync()
{
    forecasts = await WeatherClient.GetWeatherForeacasts();
}
```

## <a name="request-additional-access-tokens"></a><span data-ttu-id="44818-121">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="44818-121">Request additional access tokens</span></span>

<span data-ttu-id="44818-122">呼叫`IAccessTokenProvider.RequestAccessToken`可以手動取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-122">Access tokens can be manually obtained by calling `IAccessTokenProvider.RequestAccessToken`.</span></span>

<span data-ttu-id="44818-123">在下列範例中，應用程式需要額外的 Azure Active Directory （AAD） Microsoft Graph API 範圍，才能讀取使用者資料和傳送郵件。</span><span class="sxs-lookup"><span data-stu-id="44818-123">In the following example, additional Azure Active Directory (AAD) Microsoft Graph API scopes are required by an app to read user data and send mail.</span></span> <span data-ttu-id="44818-124">在 Azure AAD 入口網站中新增 Microsoft Graph API 許可權之後，會在用戶端應用程式（`Program.Main`， *Program.cs*）中設定額外的範圍：</span><span class="sxs-lookup"><span data-stu-id="44818-124">After adding the Microsoft Graph API permissions in the Azure AAD portal, the additional scopes are configured in the Client app (`Program.Main`, *Program.cs*):</span></span>

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

<span data-ttu-id="44818-125">`IAccessTokenProvider.RequestToken`方法提供多載，可讓應用程式使用一組指定的範圍來布建存取權杖，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="44818-125">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision an access token with a given set of scopes, as seen in the following example:</span></span>

```csharp
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

<span data-ttu-id="44818-126">`TryGetToken`傳回</span><span class="sxs-lookup"><span data-stu-id="44818-126">`TryGetToken` returns:</span></span>

* <span data-ttu-id="44818-127">`true``token`包含使用的。</span><span class="sxs-lookup"><span data-stu-id="44818-127">`true` with the `token` for use.</span></span>
* <span data-ttu-id="44818-128">`false`如果未抓取權杖，則為。</span><span class="sxs-lookup"><span data-stu-id="44818-128">`false` if the token isn't retrieved.</span></span>

## <a name="httpclient-and-httprequestmessage-with-fetch-api-request-options"></a><span data-ttu-id="44818-129">具有 Fetch API 要求選項的 HttpClient 和 HttpRequestMessage</span><span class="sxs-lookup"><span data-stu-id="44818-129">HttpClient and HttpRequestMessage with Fetch API request options</span></span>

<span data-ttu-id="44818-130">在 Blazor WebAssembly 應用程式的 WebAssembly 上執行時[HttpClient](xref:fundamentals/http-requests) ，您<xref:System.Net.Http.HttpRequestMessage>可以使用 HttpClient 和來自訂要求。</span><span class="sxs-lookup"><span data-stu-id="44818-130">When running on WebAssembly in a Blazor WebAssembly app, [HttpClient](xref:fundamentals/http-requests) and <xref:System.Net.Http.HttpRequestMessage> can be used to customize requests.</span></span> <span data-ttu-id="44818-131">例如，您可以指定 HTTP 方法和要求標頭。</span><span class="sxs-lookup"><span data-stu-id="44818-131">For example, you can specify the HTTP method and request headers.</span></span> <span data-ttu-id="44818-132">下列範例會對伺服器`POST`上的 To DO List API 端點提出要求，並顯示回應主體：</span><span class="sxs-lookup"><span data-stu-id="44818-132">The following example makes a `POST` request to a To Do List API endpoint on the server and shows the response body:</span></span>

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

<span data-ttu-id="44818-133">.NET WebAssembly 的執行會`HttpClient`使用[WindowOrWorkerGlobalScope。 fetch （）](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch)。</span><span class="sxs-lookup"><span data-stu-id="44818-133">.NET WebAssembly's implementation of `HttpClient` uses [WindowOrWorkerGlobalScope.fetch()](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch).</span></span> <span data-ttu-id="44818-134">Fetch 可讓您設定數個[要求特有的選項](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="44818-134">Fetch allows configuring several [request-specific options](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span> 

<span data-ttu-id="44818-135">您可以使用`HttpRequestMessage`下表所示的擴充方法來設定 HTTP 提取要求選項。</span><span class="sxs-lookup"><span data-stu-id="44818-135">HTTP fetch request options can be configured with `HttpRequestMessage` extension methods shown in the following table.</span></span>

| <span data-ttu-id="44818-136">`HttpRequestMessage`擴充方法</span><span class="sxs-lookup"><span data-stu-id="44818-136">`HttpRequestMessage` extension method</span></span> | <span data-ttu-id="44818-137">Fetch 要求屬性</span><span class="sxs-lookup"><span data-stu-id="44818-137">Fetch request property</span></span> |
| ------------------------------------- | ---------------------- |
| `SetBrowserRequestCredentials`        | [<span data-ttu-id="44818-138">憑證</span><span class="sxs-lookup"><span data-stu-id="44818-138">credentials</span></span>](https://developer.mozilla.org/docs/Web/API/Request/credentials) |
| `SetBrowserRequestCache`              | [<span data-ttu-id="44818-139">高速</span><span class="sxs-lookup"><span data-stu-id="44818-139">cache</span></span>](https://developer.mozilla.org/docs/Web/API/Request/cache) |
| `SetBrowserRequestMode`               | [<span data-ttu-id="44818-140">mode</span><span class="sxs-lookup"><span data-stu-id="44818-140">mode</span></span>](https://developer.mozilla.org/docs/Web/API/Request/mode) |
| `SetBrowserRequestIntegrity`          | [<span data-ttu-id="44818-141">完整性</span><span class="sxs-lookup"><span data-stu-id="44818-141">integrity</span></span>](https://developer.mozilla.org/docs/Web/API/Request/integrity) |

<span data-ttu-id="44818-142">您可以使用更泛型`SetBrowserRequestOption`的擴充方法來設定其他選項。</span><span class="sxs-lookup"><span data-stu-id="44818-142">You can set additional options using the more generic `SetBrowserRequestOption` extension method.</span></span>
 
<span data-ttu-id="44818-143">HTTP 回應通常會在 Blazor WebAssembly 應用程式中進行緩衝處理，以啟用回應內容的同步讀取支援。</span><span class="sxs-lookup"><span data-stu-id="44818-143">The HTTP response is typically buffered in a Blazor WebAssembly app to enable support for sync reads on the response content.</span></span> <span data-ttu-id="44818-144">若要啟用回應串流的支援，請`SetBrowserResponseStreamingEnabled`在要求上使用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="44818-144">To enable support for response streaming, use the `SetBrowserResponseStreamingEnabled` extension method on the request.</span></span>

<span data-ttu-id="44818-145">若要在跨原始來源要求中包含認證，請`SetBrowserRequestCredentials`使用擴充方法：</span><span class="sxs-lookup"><span data-stu-id="44818-145">To include credentials in a cross-origin request, use the `SetBrowserRequestCredentials` extension method:</span></span>

```csharp
requestMessage.SetBrowserRequestCredentials(BrowserRequestCredentials.Include);
```

<span data-ttu-id="44818-146">如需有關提取 API 選項的詳細資訊，請參閱[MDN web 檔： WindowOrWorkerGlobalScope。 Fetch （）:P arameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)。</span><span class="sxs-lookup"><span data-stu-id="44818-146">For more information on Fetch API options, see [MDN web docs: WindowOrWorkerGlobalScope.fetch():Parameters](https://developer.mozilla.org/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters).</span></span>

<span data-ttu-id="44818-147">在 CORS 要求上傳送認證（授權 cookie/標頭）時`Authorization` ，cors 原則必須允許標頭。</span><span class="sxs-lookup"><span data-stu-id="44818-147">When sending credentials (authorization cookies/headers) on CORS requests, the `Authorization` header must be allowed by the CORS policy.</span></span>

<span data-ttu-id="44818-148">下列原則包含的設定：</span><span class="sxs-lookup"><span data-stu-id="44818-148">The following policy includes configuration for:</span></span>

* <span data-ttu-id="44818-149">要求來源（`http://localhost:5000`、 `https://localhost:5001`）。</span><span class="sxs-lookup"><span data-stu-id="44818-149">Request origins (`http://localhost:5000`, `https://localhost:5001`).</span></span>
* <span data-ttu-id="44818-150">Any 方法（動詞）。</span><span class="sxs-lookup"><span data-stu-id="44818-150">Any method (verb).</span></span>
* <span data-ttu-id="44818-151">`Content-Type`和`Authorization`標頭。</span><span class="sxs-lookup"><span data-stu-id="44818-151">`Content-Type` and `Authorization` headers.</span></span> <span data-ttu-id="44818-152">若要允許自訂標頭（例如`x-custom-header`），請在呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>時列出標頭。</span><span class="sxs-lookup"><span data-stu-id="44818-152">To allow a custom header (for example, `x-custom-header`), list the header when calling <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>.</span></span>
* <span data-ttu-id="44818-153">用戶端 JavaScript 程式碼（`credentials`屬性設為`include`）所設定的認證。</span><span class="sxs-lookup"><span data-stu-id="44818-153">Credentials set by client-side JavaScript code (`credentials` property set to `include`).</span></span>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization, "x-custom-header")
    .AllowCredentials());
```

<span data-ttu-id="44818-154">如需詳細資訊， <xref:security/cors>請參閱和範例應用程式的 HTTP 要求測試器元件（*Components/HTTPRequestTester*）。</span><span class="sxs-lookup"><span data-stu-id="44818-154">For more information, see <xref:security/cors> and the sample app's HTTP Request Tester component (*Components/HTTPRequestTester.razor*).</span></span>

## <a name="handle-token-request-errors"></a><span data-ttu-id="44818-155">處理權杖要求錯誤</span><span class="sxs-lookup"><span data-stu-id="44818-155">Handle token request errors</span></span>

<span data-ttu-id="44818-156">當單一頁面應用程式（SPA）使用 Open ID Connect （OIDC）來驗證使用者時，驗證狀態會在 SPA 和識別提供者（IP）中的本機維護，其格式為使用者提供其認證時所設定的會話 cookie。</span><span class="sxs-lookup"><span data-stu-id="44818-156">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="44818-157">IP 為使用者發出的權杖通常會在短時間內有效，大約一小時，因此用戶端應用程式必須定期提取新的權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-157">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="44818-158">否則，在授與的權杖過期之後，使用者會被登出。</span><span class="sxs-lookup"><span data-stu-id="44818-158">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="44818-159">在大多數情況下，OIDC 用戶端可以布建新的權杖，而不需要使用者重新驗證，因為它會保留在 IP 內的驗證狀態或「會話」。</span><span class="sxs-lookup"><span data-stu-id="44818-159">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="44818-160">在某些情況下，用戶端無法在沒有使用者互動的情況下取得權杖，例如，基於某些原因，使用者明確地登出了 IP。</span><span class="sxs-lookup"><span data-stu-id="44818-160">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="44818-161">如果使用者造訪`https://login.microsoftonline.com`和登出，就會發生這種情況。在這些情況下，應用程式不會立即得知使用者是否已登出。用戶端持有的任何權杖可能不再有效。</span><span class="sxs-lookup"><span data-stu-id="44818-161">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="44818-162">此外，用戶端無法在目前權杖過期之後，不需要使用者互動就布建新權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-162">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="44818-163">這些案例並不是以權杖為基礎的驗證所特有。</span><span class="sxs-lookup"><span data-stu-id="44818-163">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="44818-164">它們屬於 Spa 的本質。</span><span class="sxs-lookup"><span data-stu-id="44818-164">They are part of the nature of SPAs.</span></span> <span data-ttu-id="44818-165">如果移除驗證 cookie，使用 cookie 的 SPA 也無法呼叫伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="44818-165">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="44818-166">當應用程式對受保護的資源執行 API 呼叫時，您必須注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="44818-166">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="44818-167">若要布建新的存取權杖以呼叫 API，使用者可能需要再次進行驗證。</span><span class="sxs-lookup"><span data-stu-id="44818-167">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="44818-168">即使用戶端的權杖看似有效，對伺服器的呼叫可能會失敗，因為使用者已撤銷權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-168">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="44818-169">當應用程式要求權杖時，會有兩種可能的結果：</span><span class="sxs-lookup"><span data-stu-id="44818-169">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="44818-170">要求成功，且應用程式具有有效的權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-170">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="44818-171">要求失敗，且應用程式必須再次驗證使用者，才能取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-171">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="44818-172">當令牌要求失敗時，您必須決定是否要在執行重新導向之前，先儲存任何目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-172">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="44818-173">有數種方法存在，並增加複雜性層級：</span><span class="sxs-lookup"><span data-stu-id="44818-173">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="44818-174">將目前的頁面狀態儲存在會話儲存體中。</span><span class="sxs-lookup"><span data-stu-id="44818-174">Store the current page state in session storage.</span></span> <span data-ttu-id="44818-175">在`OnInitializeAsync`期間，檢查是否可以還原狀態，再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="44818-175">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="44818-176">新增查詢字串參數，並使用它來通知應用程式它需要重新序列化先前儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-176">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="44818-177">新增具有唯一識別碼的查詢字串參數，以將資料儲存在會話儲存體中，而不會有風險與其他專案衝突。</span><span class="sxs-lookup"><span data-stu-id="44818-177">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="44818-178">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="44818-178">The following example shows how to:</span></span>

* <span data-ttu-id="44818-179">在重新導向至登入頁面之前保留狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-179">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="44818-180">使用查詢字串參數，在驗證之後復原先前的狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-180">Recover the previous state afterward authentication using the query string parameter.</span></span>

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

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="44818-181">在驗證操作之前儲存應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="44818-181">Save app state before an authentication operation</span></span>

<span data-ttu-id="44818-182">在驗證作業期間，某些情況下，您會想要在瀏覽器重新導向至 IP 之前，先儲存應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-182">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="44818-183">當您使用類似狀態容器的情況，而且您想要在驗證成功之後還原狀態時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="44818-183">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="44818-184">您可以使用自訂驗證狀態物件來保留應用程式特定狀態或其參考，並在驗證作業成功完成後還原該狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-184">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="44818-185">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="44818-185">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

## <a name="customize-app-routes"></a><span data-ttu-id="44818-186">自訂應用程式路由</span><span class="sxs-lookup"><span data-stu-id="44818-186">Customize app routes</span></span>

<span data-ttu-id="44818-187">根據預設，連結`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫會使用下表所示的路由來代表不同的驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-187">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="44818-188">路由</span><span class="sxs-lookup"><span data-stu-id="44818-188">Route</span></span>                            | <span data-ttu-id="44818-189">目的</span><span class="sxs-lookup"><span data-stu-id="44818-189">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="44818-190">觸發登入作業。</span><span class="sxs-lookup"><span data-stu-id="44818-190">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="44818-191">處理任何登入作業的結果。</span><span class="sxs-lookup"><span data-stu-id="44818-191">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="44818-192">當登入作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="44818-192">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="44818-193">觸發登出作業。</span><span class="sxs-lookup"><span data-stu-id="44818-193">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="44818-194">處理登出作業的結果。</span><span class="sxs-lookup"><span data-stu-id="44818-194">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="44818-195">當登出作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="44818-195">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="44818-196">表示使用者已成功登出。</span><span class="sxs-lookup"><span data-stu-id="44818-196">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="44818-197">觸發操作以編輯使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="44818-197">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="44818-198">觸發操作以註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="44818-198">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="44818-199">上表中顯示的路由可透過來設定`RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`。</span><span class="sxs-lookup"><span data-stu-id="44818-199">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="44818-200">設定選項以提供自訂路由時，請確認應用程式具有處理每個路徑的路由。</span><span class="sxs-lookup"><span data-stu-id="44818-200">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="44818-201">在下列範例中，所有路徑的前面都會加`/security`上。</span><span class="sxs-lookup"><span data-stu-id="44818-201">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="44818-202">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="44818-202">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="44818-203">`Program.Main`（*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="44818-203">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="44818-204">如果需求會呼叫完全不同的路徑，請如先前所述設定路由，並`RemoteAuthenticatorView`使用明確的 action 參數來呈現：</span><span class="sxs-lookup"><span data-stu-id="44818-204">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="44818-205">如果您選擇這樣做，則可以將 UI 分成不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="44818-205">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="44818-206">自訂驗證使用者介面</span><span class="sxs-lookup"><span data-stu-id="44818-206">Customize the authentication user interface</span></span>

<span data-ttu-id="44818-207">`RemoteAuthenticatorView`針對每個驗證狀態包含一組預設的 UI 元件。</span><span class="sxs-lookup"><span data-stu-id="44818-207">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="44818-208">您可以藉由傳入自訂`RenderFragment`來自訂每個狀態。</span><span class="sxs-lookup"><span data-stu-id="44818-208">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="44818-209">若要在初始登入程式期間自訂顯示的文字， `RemoteAuthenticatorView`可以變更，如下所示。</span><span class="sxs-lookup"><span data-stu-id="44818-209">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="44818-210">`Authentication`元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="44818-210">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

<span data-ttu-id="44818-211">`RemoteAuthenticatorView`有一個片段，可用於下表所示的每個驗證路由。</span><span class="sxs-lookup"><span data-stu-id="44818-211">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="44818-212">路由</span><span class="sxs-lookup"><span data-stu-id="44818-212">Route</span></span>                            | <span data-ttu-id="44818-213">片段</span><span class="sxs-lookup"><span data-stu-id="44818-213">Fragment</span></span>                |
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

## <a name="customize-the-user"></a><span data-ttu-id="44818-214">自訂使用者</span><span class="sxs-lookup"><span data-stu-id="44818-214">Customize the user</span></span>

<span data-ttu-id="44818-215">可以自訂系結至應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="44818-215">Users bound to the app can be customized.</span></span> <span data-ttu-id="44818-216">在下列範例中，所有已驗證的使用者`amr`都會收到每個使用者驗證方法的宣告。</span><span class="sxs-lookup"><span data-stu-id="44818-216">In the following example, all authenticated users receive an `amr` claim for each of the user's authentication methods.</span></span>

<span data-ttu-id="44818-217">建立擴充`RemoteUserAccount`類別的類別：</span><span class="sxs-lookup"><span data-stu-id="44818-217">Create a class that extends the `RemoteUserAccount` class:</span></span>

```csharp
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class OidcAccount : RemoteUserAccount
{
    [JsonPropertyName("amr")]
    public string[] AuthenticationMethod { get; set; }
}
```

<span data-ttu-id="44818-218">建立可延伸`AccountClaimsPrincipalFactory<TAccount>`的 factory：</span><span class="sxs-lookup"><span data-stu-id="44818-218">Create a factory that extends `AccountClaimsPrincipalFactory<TAccount>`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication.Internal;

public class CustomAccountFactory 
    : AccountClaimsPrincipalFactory<OidcAccount>
{
    public CustomAccountFactory(NavigationManager navigationManager, 
        IAccessTokenProviderAccessor accessor) : base(accessor)
    {
    }
  
    public async override ValueTask<ClaimsPrincipal> CreateUserAsync(
        OidcAccount account, RemoteAuthenticationUserOptions options)
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

<span data-ttu-id="44818-219">註冊服務以使用`CustomAccountFactory`：</span><span class="sxs-lookup"><span data-stu-id="44818-219">Register services to use the `CustomAccountFactory`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

...

builder.Services.AddApiAuthorization<RemoteAuthenticationState, OidcAccount>()
    .AddAccountClaimsPrincipalFactory<RemoteAuthenticationState, OidcAccount, 
        CustomAccountFactory>();
```

## <a name="support-prerendering-with-authentication"></a><span data-ttu-id="44818-220">支援使用驗證來進行預呈現</span><span class="sxs-lookup"><span data-stu-id="44818-220">Support prerendering with authentication</span></span>

<span data-ttu-id="44818-221">遵循其中一個託管Blazor WebAssembly 應用程式主題中的指導方針之後，請使用下列指示來建立應用程式：</span><span class="sxs-lookup"><span data-stu-id="44818-221">After following the guidance in one of the hosted Blazor WebAssembly app topics, use the following instructions to create an app that:</span></span>

* <span data-ttu-id="44818-222">不需要授權的 Prerenders 路徑。</span><span class="sxs-lookup"><span data-stu-id="44818-222">Prerenders paths for which authorization isn't required.</span></span>
* <span data-ttu-id="44818-223">不需要授權的已呈現路徑。</span><span class="sxs-lookup"><span data-stu-id="44818-223">Doesn't prerender paths for which authorization is required.</span></span>

<span data-ttu-id="44818-224">在用戶端應用程式`Program`的類別（*Program.cs*）中，將常見的服務註冊因素劃分為不同的`ConfigureCommonServices`方法（例如，）：</span><span class="sxs-lookup"><span data-stu-id="44818-224">In the Client app's `Program` class (*Program.cs*), factor common service registrations into a separate method (for example, `ConfigureCommonServices`):</span></span>

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

<span data-ttu-id="44818-225">在伺服器應用程式的`Startup.ConfigureServices`中，註冊下列其他服務：</span><span class="sxs-lookup"><span data-stu-id="44818-225">In the Server app's `Startup.ConfigureServices`, register the following additional services:</span></span>

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

<span data-ttu-id="44818-226">在伺服器應用程式的`Startup.Configure`方法中， `endpoints.MapFallbackToFile("index.html")`將`endpoints.MapFallbackToPage("/_Host")`取代為：</span><span class="sxs-lookup"><span data-stu-id="44818-226">In the Server app's `Startup.Configure` method, replace `endpoints.MapFallbackToFile("index.html")` with `endpoints.MapFallbackToPage("/_Host")`:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllers();
    endpoints.MapFallbackToPage("/_Host");
});
```

<span data-ttu-id="44818-227">在伺服器應用程式中，建立*Pages*資料夾（如果不存在）。</span><span class="sxs-lookup"><span data-stu-id="44818-227">In the Server app, create a *Pages* folder if it doesn't exist.</span></span> <span data-ttu-id="44818-228">在伺服器應用程式的*Pages*資料夾內建立 *_Host 的 cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="44818-228">Create a *_Host.cshtml* page inside the Server app's *Pages* folder.</span></span> <span data-ttu-id="44818-229">將用戶端應用程式*wwwroot/index.html*檔的內容貼到*Pages/_Host. cshtml*檔案中。</span><span class="sxs-lookup"><span data-stu-id="44818-229">Paste the contents from the Client app's *wwwroot/index.html* file into the *Pages/_Host.cshtml* file.</span></span> <span data-ttu-id="44818-230">更新檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="44818-230">Update the file's contents:</span></span>

* <span data-ttu-id="44818-231">將 `@page "_Host"` 新增到檔案的頂端。</span><span class="sxs-lookup"><span data-stu-id="44818-231">Add `@page "_Host"` to the top of the file.</span></span>
* <span data-ttu-id="44818-232">將`<app>Loading...</app>`標記取代為下列內容：</span><span class="sxs-lookup"><span data-stu-id="44818-232">Replace the `<app>Loading...</app>` tag with the following:</span></span>

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
  
## <a name="options-for-hosted-apps-and-third-party-login-providers"></a><span data-ttu-id="44818-233">託管應用程式和協力廠商登入提供者的選項</span><span class="sxs-lookup"><span data-stu-id="44818-233">Options for hosted apps and third-party login providers</span></span>

<span data-ttu-id="44818-234">使用協力廠商提供者Blazor驗證和授權託管 WebAssembly 應用程式時，有數個選項可用來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="44818-234">When authenticating and authorizing a hosted Blazor WebAssembly app with a third-party provider, there are several options available for authenticating the user.</span></span> <span data-ttu-id="44818-235">您選擇哪一個取決於您的案例。</span><span class="sxs-lookup"><span data-stu-id="44818-235">Which one you choose depends on your scenario.</span></span>

<span data-ttu-id="44818-236">如需詳細資訊，請參閱<xref:security/authentication/social/additional-claims>。</span><span class="sxs-lookup"><span data-stu-id="44818-236">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>

### <a name="authenticate-users-to-only-call-protected-third-party-apis"></a><span data-ttu-id="44818-237">驗證使用者只呼叫受保護的協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="44818-237">Authenticate users to only call protected third party APIs</span></span>

<span data-ttu-id="44818-238">對協力廠商 API 提供者的用戶端 OAuth 流程驗證使用者：</span><span class="sxs-lookup"><span data-stu-id="44818-238">Authenticate the user with a client-side OAuth flow against the third-party API provider:</span></span>

 ```csharp
 builder.services.AddOidcAuthentication(options => { ... });
 ```
 
 <span data-ttu-id="44818-239">在此情節中：</span><span class="sxs-lookup"><span data-stu-id="44818-239">In this scenario:</span></span>

* <span data-ttu-id="44818-240">裝載應用程式的伺服器不扮演角色。</span><span class="sxs-lookup"><span data-stu-id="44818-240">The server hosting the app doesn't play a role.</span></span>
* <span data-ttu-id="44818-241">無法保護伺服器上的 Api。</span><span class="sxs-lookup"><span data-stu-id="44818-241">APIs on the server can't be protected.</span></span>
* <span data-ttu-id="44818-242">應用程式只能呼叫受保護的協力廠商 Api。</span><span class="sxs-lookup"><span data-stu-id="44818-242">The app can only call protected third-party APIs.</span></span>

### <a name="authenticate-users-with-a-third-party-provider-and-call-protected-apis-on-the-host-server-and-the-third-party"></a><span data-ttu-id="44818-243">使用協力廠商提供者來驗證使用者，並在主機伺服器和協力廠商上呼叫受保護的 Api</span><span class="sxs-lookup"><span data-stu-id="44818-243">Authenticate users with a third-party provider and call protected APIs on the host server and the third party</span></span>

<span data-ttu-id="44818-244">使用Identity協力廠商登入提供者進行設定。</span><span class="sxs-lookup"><span data-stu-id="44818-244">Configure Identity with a third-party login provider.</span></span> <span data-ttu-id="44818-245">取得協力廠商 API 存取所需的權杖，並加以儲存。</span><span class="sxs-lookup"><span data-stu-id="44818-245">Obtain the tokens required for third-party API access and store them.</span></span>

<span data-ttu-id="44818-246">當使用者登入時，會Identity收集存取權並重新整理權杖，做為驗證程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="44818-246">When a user logs in, Identity collects access and refresh tokens as part of the authentication process.</span></span> <span data-ttu-id="44818-247">此時，有幾個方法可用來對協力廠商 Api 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="44818-247">At that point, there are a couple of approaches available for making API calls to third-party APIs.</span></span>

#### <a name="use-a-server-access-token-to-retrieve-the-third-party-access-token"></a><span data-ttu-id="44818-248">使用伺服器存取權杖來取出協力廠商存取權杖</span><span class="sxs-lookup"><span data-stu-id="44818-248">Use a server access token to retrieve the third-party access token</span></span>

<span data-ttu-id="44818-249">使用伺服器上產生的存取權杖，從伺服器 API 端點抓取協力廠商存取權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-249">Use the access token generated on the server to retrieve the third-party access token from a server API endpoint.</span></span> <span data-ttu-id="44818-250">從該處，使用協力廠商存取權杖，直接從Identity用戶端上呼叫協力廠商 API 資源。</span><span class="sxs-lookup"><span data-stu-id="44818-250">From there, use the third-party access token to call third-party API resources directly from Identity on the client.</span></span>

<span data-ttu-id="44818-251">我們不建議採用這種方法。</span><span class="sxs-lookup"><span data-stu-id="44818-251">We don't recommend this approach.</span></span> <span data-ttu-id="44818-252">這種方法需要將協力廠商存取權杖視為針對公用用戶端所產生。</span><span class="sxs-lookup"><span data-stu-id="44818-252">This approach requires treating the third-party access token as if it were generated for a public client.</span></span> <span data-ttu-id="44818-253">在 OAuth 詞彙中，公用應用程式不會有用戶端密碼，因為它無法受信任而無法安全地儲存秘密，而且會為機密用戶端產生存取權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-253">In OAuth terms, the public app doesn't have a client secret because it can't be trusted to store secrets safely, and the access token is produced for a confidential client.</span></span> <span data-ttu-id="44818-254">機密用戶端是具有用戶端密碼的用戶端，並假設能夠安全地儲存秘密。</span><span class="sxs-lookup"><span data-stu-id="44818-254">A confidential client is a client that has a client secret and is assumed to be able to safely store secrets.</span></span>

* <span data-ttu-id="44818-255">協力廠商存取權杖可能會被授與額外的範圍來執行敏感性作業，這是根據協力廠商針對較受信任的用戶端發出權杖的事實。</span><span class="sxs-lookup"><span data-stu-id="44818-255">The third-party access token might be granted additional scopes to perform sensitive operations based on the fact that the third-party emitted the token for a more trusted client.</span></span>
* <span data-ttu-id="44818-256">同樣地，重新整理權杖不應發給不受信任的用戶端，因為這樣做會讓用戶端無限制存取，除非有其他限制。</span><span class="sxs-lookup"><span data-stu-id="44818-256">Similarly, refresh tokens shouldn't be issued to a client that isn't trusted, as doing so gives the client unlimited access unless other restrictions are put into place.</span></span>

#### <a name="make-api-calls-from-the-client-to-the-server-api-in-order-to-call-third-party-apis"></a><span data-ttu-id="44818-257">從用戶端對伺服器 API 進行 API 呼叫，以便呼叫協力廠商 Api</span><span class="sxs-lookup"><span data-stu-id="44818-257">Make API calls from the client to the server API in order to call third-party APIs</span></span>

<span data-ttu-id="44818-258">從用戶端對伺服器 API 進行 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="44818-258">Make an API call from the client to the server API.</span></span> <span data-ttu-id="44818-259">從伺服器中，取出協力廠商 API 資源的存取權杖，併發出任何需要的呼叫。</span><span class="sxs-lookup"><span data-stu-id="44818-259">From the server, retrieve the access token for the third-party API resource and issue whatever call is necessary.</span></span>

<span data-ttu-id="44818-260">雖然這種方法需要透過伺服器額外的網路躍點來呼叫協力廠商 API，但最終會導致更安全的體驗：</span><span class="sxs-lookup"><span data-stu-id="44818-260">While this approach requires an extra network hop through the server to call a third-party API, it ultimately results in a safer experience:</span></span>

* <span data-ttu-id="44818-261">伺服器可以儲存重新整理權杖，並確保應用程式不會失去協力廠商資源的存取權。</span><span class="sxs-lookup"><span data-stu-id="44818-261">The server can store refresh tokens and ensure that the app doesn't lose access to third-party resources.</span></span>
* <span data-ttu-id="44818-262">應用程式無法從可能包含更多敏感性許可權的伺服器洩漏存取權杖。</span><span class="sxs-lookup"><span data-stu-id="44818-262">The app can't leak access tokens from the server that might contain more sensitive permissions.</span></span>
