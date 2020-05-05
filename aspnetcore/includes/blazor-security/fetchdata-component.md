<span data-ttu-id="45e26-101">此`FetchData`元件顯示如何：</span><span class="sxs-lookup"><span data-stu-id="45e26-101">The `FetchData` component shows how to:</span></span>

* <span data-ttu-id="45e26-102">布建存取權杖。</span><span class="sxs-lookup"><span data-stu-id="45e26-102">Provision an access token.</span></span>
* <span data-ttu-id="45e26-103">使用存取權杖在*伺服器*應用程式中呼叫受保護的資源 API。</span><span class="sxs-lookup"><span data-stu-id="45e26-103">Use the access token to call a protected resource API in the *Server* app.</span></span>

<span data-ttu-id="45e26-104">`@attribute [Authorize]`指示詞會向 Blazor WebAssembly 授權系統指出，使用者必須獲得授權，才能流覽此元件。</span><span class="sxs-lookup"><span data-stu-id="45e26-104">The `@attribute [Authorize]` directive indicates to the Blazor WebAssembly authorization system that the user must be authorized in order to visit this component.</span></span> <span data-ttu-id="45e26-105">*用戶端*應用程式中的屬性存在，並不會防止在沒有適當認證的情況下呼叫伺服器上的 API。</span><span class="sxs-lookup"><span data-stu-id="45e26-105">The presence of the attribute in the *Client* app doesn't prevent the API on the server from being called without proper credentials.</span></span> <span data-ttu-id="45e26-106">*伺服器*應用程式也必須在`[Authorize]`適當的端點上使用，才能正確地保護它們。</span><span class="sxs-lookup"><span data-stu-id="45e26-106">The *Server* app also must use `[Authorize]` on the appropriate endpoints to correctly protect them.</span></span>

<span data-ttu-id="45e26-107">`IAccessTokenProvider.RequestAccessToken();`負責要求可新增至要求以呼叫 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="45e26-107">`IAccessTokenProvider.RequestAccessToken();` takes care of requesting an access token that can be added to the request to call the API.</span></span> <span data-ttu-id="45e26-108">如果已快取權杖，或服務能夠在沒有使用者互動的情況下布建新的存取權杖，則權杖要求會成功。</span><span class="sxs-lookup"><span data-stu-id="45e26-108">If the token is cached or the service is able to provision a new access token without user interaction, the token request succeeds.</span></span> <span data-ttu-id="45e26-109">否則，權杖要求會失敗，並`AccessTokenNotAvailableException`出現錯誤，在`try-catch`語句中攔截到。</span><span class="sxs-lookup"><span data-stu-id="45e26-109">Otherwise, the token request fails with an `AccessTokenNotAvailableException` error, which is caught in a `try-catch` statement.</span></span>

<span data-ttu-id="45e26-110">為了取得要包含在要求中的實際權杖，應用程式必須藉由呼叫`tokenResult.TryGetToken(out var token)`來檢查要求是否成功。</span><span class="sxs-lookup"><span data-stu-id="45e26-110">In order to obtain the actual token to include in the request, the app must check that the request succeeded by calling `tokenResult.TryGetToken(out var token)`.</span></span> 

<span data-ttu-id="45e26-111">如果要求成功，權杖變數就會填入存取權杖。</span><span class="sxs-lookup"><span data-stu-id="45e26-111">If the request was successful, the token variable is populated with the access token.</span></span> <span data-ttu-id="45e26-112">Token `Value`的屬性會公開要包含在`Authorization`要求標頭中的常值字串。</span><span class="sxs-lookup"><span data-stu-id="45e26-112">The `Value` property of the token exposes the literal string to include in the `Authorization` request header.</span></span>

<span data-ttu-id="45e26-113">如果要求失敗，因為無法在沒有使用者互動的情況下布建權杖，權杖結果會包含重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="45e26-113">If the request failed because the token couldn't be provisioned without user interaction, the token result contains a redirect URL.</span></span> <span data-ttu-id="45e26-114">流覽至此 URL 會將使用者帶到登入頁面，並在成功驗證之後回到目前的頁面。</span><span class="sxs-lookup"><span data-stu-id="45e26-114">Navigating to this URL takes the user to the login page and back to the current page after a successful authentication.</span></span>

```razor
@page "/fetchdata"
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@using {APP NAMESPACE}.Shared
@attribute [Authorize]
@inject HttpClient Http

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        try
        {
            forecasts = await Http.GetFromJsonAsync<WeatherForecast[]>("WeatherForecast");
        }
        catch (AccessTokenNotAvailableException exception)
        {
            exception.Redirect();
        }
    }
}
```
