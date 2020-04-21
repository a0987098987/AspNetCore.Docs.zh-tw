該`FetchData`元件展示如何:

* 預配訪問權杖。
* 使用存取權杖在 *「伺服器」* 應用中呼叫受保護的資源 API。

該`@attribute [Authorize]`指令向 Blazor WebAssembly 授權系統指示用戶必須獲得授權才能存取此元件。 *用戶端*應用中的屬性不會阻止在沒有適當認證的情況下呼叫伺服器上的 API。 *伺服器*應用還必須在適當的終結`[Authorize]`點 上使用才能正確保護它們。

`AuthenticationService.RequestAccessToken();`負責請求可添加到調用 API 的請求中的訪問權杖。 如果令牌被緩存,或者服務能夠在沒有使用者交互的情況下預配新的訪問令牌,則令牌請求將成功。 否則,令牌請求將失敗。

為了獲得要包含在請求中的實際權杖,應用必須通過調用`tokenResult.TryGetToken(out var token)`來檢查請求是否成功。 

如果請求成功,則令牌變數將填充訪問令牌。 權杖`Value`屬性公開要包含在請求標頭中的`Authorization`文字字串。

如果請求失敗,因為如果沒有使用者交互無法預配權杖,則令牌結果包含重定向 URL。 導航到此 URL 會將使用者帶到登錄頁,並在身份驗證成功後返回當前頁面。

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
            forecasts = await httpClient.GetFromJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

關於詳細資訊,請參閱[在身份認證操作之前儲存應用狀態](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation)。
