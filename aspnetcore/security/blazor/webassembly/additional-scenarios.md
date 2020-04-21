---
title: ASP.NET核心BlazorWeb 組裝其他安全方案
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/additional-scenarios
ms.openlocfilehash: 314a7b54ab87295b8ca814f5e369942ae911407e
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661587"
---
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a>ASP.NET核心 Blazor WebAssembly 其他安全方案

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="request-additional-access-tokens"></a>要求其他存取權杖

大多數應用僅需要訪問權杖才能與其使用的受保護資源進行交互。 在某些情況下,應用可能需要多個權杖才能與兩個或多個資源進行交互。

在下面的範例中,應用需要其他 Azure 活動目錄 (AAD) Microsoft 圖形 API 作用域來讀取使用者數據和發送郵件。 在 Azure AAD 門戶中添加 Microsoft 圖形 API 許可權後`Program.Main`,其他作用域在用戶端應用 *(Program.cs)* 中配置。

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

該方法`IAccessTokenProvider.RequestToken`提供重載,允許應用使用一組給定的範圍預配令牌,如下例所示:

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
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

`TryGetToken`返回:

* `true`與`token`使用。
* `false`如果未檢索令牌。

## <a name="handle-token-request-errors"></a>處理權杖要求錯誤

當單頁應用程式 (SPA) 使用開放 ID 連接 (OIDC) 對使用者進行身份驗證時,身份驗證狀態將在 SPA 中本地和標識提供者 (IP) 中以會話 Cookie 的形式維護,該狀態是使用者提供認證而設定的。

IP 為使用者發出的權杖通常在短時間內有效,通常大約一小時,因此用戶端應用必須定期獲取新令牌。 否則,使用者將在授予的權杖過期後註銷。 在大多數情況下,由於IP中的身份驗證狀態或"工作階段",OIDC客戶端能夠預配新權杖,而無需使用者再次進行身份驗證。

在某些情況下,客戶端在沒有使用者交互的情況下無法獲取令牌,例如,由於某種原因,使用者顯式註銷了 IP。 如果用戶訪問`https://login.microsoftonline.com`並註銷,將發生此情況。在這些情況下,應用不會立即知道使用者已註銷。用戶端持有的任何權杖可能不再有效。 此外,在當前權杖過期後,用戶端無法預配沒有使用者互動的新權杖。

這些方案不特定於基於令牌的身份驗證。 它們是 SPA 性質的一部分。 如果刪除身份驗證 Cookie,則使用 Cookie 的 SPA 也無法呼叫伺服器 API。

當應用對受保護的資源執行 API 呼叫時,您必須注意以下事項:

* 要預配新的訪問權杖以調用 API,可能需要使用者再次進行身份驗證。
* 即使用戶端具有看似有效的權杖,對伺服器的調用也可能失敗,因為令牌已被使用者吊銷。

當應用請求權杖時,有兩種可能的結果:

* 請求成功,並且應用具有有效的權杖。
* 請求失敗,應用必須再次對使用者進行身份驗證才能獲得新令牌。

當權杖請求失敗時,您需要在執行重定向之前決定是否要儲存任何當前狀態。 存在幾種方法,複雜性越來越高:

* 將當前頁面狀態存儲在會話存儲中。 在`OnInitializeAsync`期間,檢查是否可以在繼續之前恢復狀態。
* 添加查詢字串參數,並將其用作向應用發出信號,使其需要重新補充以前保存的狀態的一種方式。
* 添加具有唯一標識符的查詢字串參數,將數據存儲在會話存儲中,而不會冒與其他項發生衝突的風險。

下列範例示範如何執行：

* 在重定向到登錄頁之前保留狀態。
* 使用查詢字串參數恢復以前的狀態后身份驗證。

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

## <a name="save-app-state-before-an-authentication-operation"></a>在認證操作之前儲存套用狀態

在身份驗證操作期間,在某些情況下,您希望在瀏覽器重定向到 IP 之前保存應用狀態。 當您使用狀態容器之類的內容,並且希望在身份驗證成功后還原狀態時,情況可能如此。 可以使用自定義身份驗證狀態物件保留特定於應用的狀態或對它的引用,並在身份驗證操作成功完成後還原該狀態。

`Authentication`元件 (*頁 / 身份認證. razor) :*

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

## <a name="customize-app-routes"></a>自訂應用路由

預設情況下,`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫使用下表中顯示的路由來表示不同的身份驗證狀態。

| 路由                            | 目的 |
| -------------------------------- | ------- |
| `authentication/login`           | 觸發登錄操作。 |
| `authentication/login-callback`  | 處理任何登錄操作的結果。 |
| `authentication/login-failed`    | 當登錄操作由於某種原因失敗時顯示錯誤消息。 |
| `authentication/logout`          | 觸發註銷操作。 |
| `authentication/logout-callback` | 處理註銷操作的結果。 |
| `authentication/logout-failed`   | 當註銷操作由於某種原因失敗時顯示錯誤消息。 |
| `authentication/logged-out`      | 指示使用者已成功註銷。 |
| `authentication/profile`         | 觸發操作以編輯使用者配置檔。 |
| `authentication/register`        | 觸發操作以註冊新使用者。 |

上表中顯示的路由可通過`RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`進行配置。 設置選項以提供自定義路由時,請確認應用具有處理每個路徑的路由。

在下面的範例中,所有路徑都預定了`/security`。

`Authentication`元件 (*頁 / 身份認證. razor) :*

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

`Program.Main`*(Program.cs*):

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

如果要求對完全不同的路徑進行調用,請按照前面所述設置路由,並`RemoteAuthenticatorView`呈現顯式操作參數:

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

如果選擇這樣做,則可以將UI分解為不同的頁面。

## <a name="customize-the-authentication-user-interface"></a>自訂身份驗證使用者介面

`RemoteAuthenticatorView`包括每個身份驗證狀態的預設 UI 部分集。 每個狀態都可以通過傳入自定義`RenderFragment`來自定義。 在初始登入時自訂顯示的文字,可以按以下的`RemoteAuthenticatorView`變更 。

`Authentication`元件 (*頁 / 身份認證. razor) :*

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

有`RemoteAuthenticatorView`一個片段,可以按下表中顯示的每個身份驗證路由使用。

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
