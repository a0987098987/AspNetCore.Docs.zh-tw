---
title: ASP.NET Core Blazor WebAssembly 其他安全性案例
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: ccb512392341e3eea33f4ab45740b7900f7b63f9
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989470"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>ASP.NET Core Blazor WebAssembly 其他安全性案例

By [Javier Calvarro Nelson](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a>處理權杖要求錯誤

當單一頁面應用程式（SPA）使用 Open ID Connect （OIDC）來驗證使用者時，驗證狀態會在 SPA 和身分識別提供者（IP）中，以會話 cookie 的形式保留在本機，並以使用者提供其憑證.

IP 為使用者發出的權杖通常會在短時間內有效，大約一小時，因此用戶端應用程式必須定期提取新的權杖。 否則，在授與的權杖過期之後，使用者會被登出。 在大多數情況下，OIDC 用戶端可以布建新的權杖，而不需要使用者重新驗證，因為它會保留在 IP 內的驗證狀態或「會話」。

在某些情況下，用戶端無法在沒有使用者互動的情況下取得權杖，例如，基於某些原因，使用者明確地登出了 IP。 當使用者造訪 `https://login.microsoftonline.com` 並登出時，就會發生這種情況。在這些情況下，應用程式不會立即得知使用者是否已登出。用戶端持有的任何權杖可能不再有效。 此外，用戶端無法在目前權杖過期之後，不需要使用者互動就布建新權杖。

這些案例並不是以權杖為基礎的驗證所特有。 它們屬於 Spa 的本質。 如果移除驗證 cookie，使用 cookie 的 SPA 也無法呼叫伺服器 API。

當應用程式對受保護的資源執行 API 呼叫時，您必須注意下列事項：

* 若要布建新的存取權杖以呼叫 API，使用者可能需要再次進行驗證。
* 即使用戶端的權杖看似有效，對伺服器的呼叫可能會失敗，因為使用者已撤銷權杖。

當應用程式要求權杖時，會有兩種可能的結果：

* 要求成功，且應用程式具有有效的權杖。
* 要求失敗，且應用程式必須再次驗證使用者，才能取得新的權杖。

當令牌要求失敗時，您必須決定是否要在執行重新導向之前，先儲存任何目前的狀態。 有數種方法存在，並增加複雜性層級：

* 將目前的頁面狀態儲存在會話儲存體中。 在 `OnInitializeAsync`期間，檢查是否可以還原狀態，再繼續進行。
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
            await httpClient.PostJsonAsync("Save", User);
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

在驗證作業期間，某些情況下，您會想要在瀏覽器重新導向至 IP 之前，先儲存應用程式狀態。 當您使用類似狀態容器的情況，而且您想要在驗證成功之後還原狀態時，就可能發生這種情況。 您可以使用自訂驗證狀態物件來保留應用程式特定狀態或其參考，並在驗證作業成功完成後還原該狀態。

`Authentication` 元件（*Pages/Authentication. razor*）：

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

## <a name="request-additional-access-tokens"></a>要求其他存取權杖

大部分的應用程式只需要存取權杖，即可與他們使用的受保護資源互動。 在某些情況下，應用程式可能需要一個以上的權杖，才能與兩個或多個資源互動。 `IAccessTokenProvider.RequestToken` 方法提供多載，可讓應用程式使用一組指定的範圍來布建權杖，如下列範例所示：

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a>自訂應用程式路由

根據預設，`Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫會使用下表所示的路由來代表不同的驗證狀態。

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

上表中顯示的路由可透過 `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`來設定。 設定選項以提供自訂路由時，請確認應用程式具有處理每個路徑的路由。

在下列範例中，所有路徑的前面都會加上 `/security`。

`Authentication` 元件（*Pages/Authentication. razor*）：

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main` （*Program.cs*）：

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

如果需求會呼叫完全不同的路徑，請設定前面所述的路由，並使用明確的 action 參數轉譯 `RemoteAuthenticatorView`：

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

如果您選擇這樣做，則可以將 UI 分成不同的頁面。

## <a name="customize-the-authentication-user-interface"></a>自訂驗證使用者介面

`RemoteAuthenticatorView` 包含每個驗證狀態的一組預設 UI 部分。 您可以藉由傳入自訂 `RenderFragment`來自訂每個狀態。 若要在初始登入程式期間自訂顯示的文字，可以變更 `RemoteAuthenticatorView`，如下所示。

`Authentication` 元件（*Pages/Authentication. razor*）：

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

`RemoteAuthenticatorView` 有一個片段，可用於下表所示的每個驗證路由。

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
