<span data-ttu-id="415ea-101">該`FetchData`元件展示如何:</span><span class="sxs-lookup"><span data-stu-id="415ea-101">The `FetchData` component shows how to:</span></span>

* <span data-ttu-id="415ea-102">預配訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="415ea-102">Provision an access token.</span></span>
* <span data-ttu-id="415ea-103">使用存取權杖在 *「伺服器」* 應用中呼叫受保護的資源 API。</span><span class="sxs-lookup"><span data-stu-id="415ea-103">Use the access token to call a protected resource API in the *Server* app.</span></span>

<span data-ttu-id="415ea-104">該`@attribute [Authorize]`指令向 Blazor WebAssembly 授權系統指示用戶必須獲得授權才能存取此元件。</span><span class="sxs-lookup"><span data-stu-id="415ea-104">The `@attribute [Authorize]` directive indicates to the Blazor WebAssembly authorization system that the user must be authorized in order to visit this component.</span></span> <span data-ttu-id="415ea-105">*用戶端*應用中的屬性不會阻止在沒有適當認證的情況下呼叫伺服器上的 API。</span><span class="sxs-lookup"><span data-stu-id="415ea-105">The presence of the attribute in the *Client* app doesn't prevent the API on the server from being called without proper credentials.</span></span> <span data-ttu-id="415ea-106">*伺服器*應用還必須在適當的終結`[Authorize]`點 上使用才能正確保護它們。</span><span class="sxs-lookup"><span data-stu-id="415ea-106">The *Server* app also must use `[Authorize]` on the appropriate endpoints to correctly protect them.</span></span>

<span data-ttu-id="415ea-107">`AuthenticationService.RequestAccessToken();`負責請求可添加到調用 API 的請求中的訪問權杖。</span><span class="sxs-lookup"><span data-stu-id="415ea-107">`AuthenticationService.RequestAccessToken();` takes care of requesting an access token that can be added to the request to call the API.</span></span> <span data-ttu-id="415ea-108">如果令牌被緩存,或者服務能夠在沒有使用者交互的情況下預配新的訪問令牌,則令牌請求將成功。</span><span class="sxs-lookup"><span data-stu-id="415ea-108">If the token is cached or the service is able to provision a new access token without user interaction, the token request succeeds.</span></span> <span data-ttu-id="415ea-109">否則,令牌請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="415ea-109">Otherwise, the token request fails.</span></span>

<span data-ttu-id="415ea-110">為了獲得要包含在請求中的實際權杖,應用必須通過調用`tokenResult.TryGetToken(out var token)`來檢查請求是否成功。</span><span class="sxs-lookup"><span data-stu-id="415ea-110">In order to obtain the actual token to include in the request, the app must check that the request succeeded by calling `tokenResult.TryGetToken(out var token)`.</span></span> 

<span data-ttu-id="415ea-111">如果請求成功,則令牌變數將填充訪問令牌。</span><span class="sxs-lookup"><span data-stu-id="415ea-111">If the request was successful, the token variable is populated with the access token.</span></span> <span data-ttu-id="415ea-112">權杖`Value`屬性公開要包含在請求標頭中的`Authorization`文字字串。</span><span class="sxs-lookup"><span data-stu-id="415ea-112">The `Value` property of the token exposes the literal string to include in the `Authorization` request header.</span></span>

<span data-ttu-id="415ea-113">如果請求失敗,因為如果沒有使用者交互無法預配權杖,則令牌結果包含重定向 URL。</span><span class="sxs-lookup"><span data-stu-id="415ea-113">If the request failed because the token couldn't be provisioned without user interaction, the token result contains a redirect URL.</span></span> <span data-ttu-id="415ea-114">導航到此 URL 會將使用者帶到登錄頁,並在身份驗證成功後返回當前頁面。</span><span class="sxs-lookup"><span data-stu-id="415ea-114">Navigating to this URL takes the user to the login page and back to the current page after a successful authentication.</span></span>

```razor
@page "/fetchdata"
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject IAccessTokenProvider AuthenticationService
@inject NavigationManager Navigation
@using {APPLICATION NAMESPACE}.Shared
@attribute [Authorize]

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var tokenResult = await AuthenticationService.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            forecasts = await httpClient.GetJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

<span data-ttu-id="415ea-115">關於詳細資訊,請參閱[在身份認證操作之前儲存應用狀態](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation)。</span><span class="sxs-lookup"><span data-stu-id="415ea-115">For more information, see [Save app state before an authentication operation](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span></span>
