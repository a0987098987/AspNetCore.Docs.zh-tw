<span data-ttu-id="c2d1d-101">`FetchData` 元件顯示如何：</span><span class="sxs-lookup"><span data-stu-id="c2d1d-101">The `FetchData` component shows how to:</span></span>

* <span data-ttu-id="c2d1d-102">布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-102">Provision an access token.</span></span>
* <span data-ttu-id="c2d1d-103">使用存取權杖在*伺服器*應用程式中呼叫受保護的資源 API。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-103">Use the access token to call a protected resource API in the *Server* app.</span></span>

<span data-ttu-id="c2d1d-104">`@attribute [Authorize]` 指示詞會向 Blazor WebAssembly 授權系統表示使用者必須獲得授權，才能流覽此元件。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-104">The `@attribute [Authorize]` directive indicates to the Blazor WebAssembly authorization system that the user must be authorized in order to visit this component.</span></span> <span data-ttu-id="c2d1d-105">*用戶端*應用程式中的屬性存在，並不會防止在沒有適當認證的情況下呼叫伺服器上的 API。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-105">The presence of the attribute in the *Client* app doesn't prevent the API on the server from being called without proper credentials.</span></span> <span data-ttu-id="c2d1d-106">*伺服器*應用程式也必須在適當的端點上使用 `[Authorize]`，才能正確地加以保護。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-106">The *Server* app also must use `[Authorize]` on the appropriate endpoints to correctly protect them.</span></span>

<span data-ttu-id="c2d1d-107">`AuthenticationService.RequestAccessToken();` 會負責要求可新增至要求以呼叫 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-107">`AuthenticationService.RequestAccessToken();` takes care of requesting an access token that can be added to the request to call the API.</span></span> <span data-ttu-id="c2d1d-108">如果已快取權杖，或服務能夠在沒有使用者互動的情況下布建新的存取權杖，則權杖要求會成功。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-108">If the token is cached or the service is able to provision a new access token without user interaction, the token request succeeds.</span></span> <span data-ttu-id="c2d1d-109">否則，權杖要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-109">Otherwise, the token request fails.</span></span>

<span data-ttu-id="c2d1d-110">為了取得要包含在要求中的實際權杖，應用程式必須藉由呼叫 `tokenResult.TryGetToken(out var token)`來檢查要求是否成功。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-110">In order to obtain the actual token to include in the request, the app must check that the request succeeded by calling `tokenResult.TryGetToken(out var token)`.</span></span> 

<span data-ttu-id="c2d1d-111">如果要求成功，權杖變數就會填入存取權杖。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-111">If the request was successful, the token variable is populated with the access token.</span></span> <span data-ttu-id="c2d1d-112">Token 的 `Value` 屬性會公開要包含在 `Authorization` 要求標頭中的常值字串。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-112">The `Value` property of the token exposes the literal string to include in the `Authorization` request header.</span></span>

<span data-ttu-id="c2d1d-113">如果要求失敗，因為無法在沒有使用者互動的情況下布建權杖，權杖結果會包含重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-113">If the request failed because the token couldn't be provisioned without user interaction, the token result contains a redirect URL.</span></span> <span data-ttu-id="c2d1d-114">流覽至此 URL 會將使用者帶到登入頁面，並在成功驗證之後回到目前的頁面。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-114">Navigating to this URL takes the user to the login page and back to the current page after a successful authentication.</span></span>

```razor
@page "/fetchdata"
...
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

<span data-ttu-id="c2d1d-115">如需詳細資訊，請參閱[在驗證作業之前儲存應用程式狀態](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation)。</span><span class="sxs-lookup"><span data-stu-id="c2d1d-115">For more information, see [Save app state before an authentication operation](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).</span></span>
