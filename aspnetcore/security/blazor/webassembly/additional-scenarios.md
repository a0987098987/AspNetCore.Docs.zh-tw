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
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="97db6-102">ASP.NET Core Blazor WebAssembly 其他安全性案例</span><span class="sxs-lookup"><span data-stu-id="97db6-102">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="97db6-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="97db6-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="handle-token-request-errors"></a><span data-ttu-id="97db6-104">處理權杖要求錯誤</span><span class="sxs-lookup"><span data-stu-id="97db6-104">Handle token request errors</span></span>

<span data-ttu-id="97db6-105">當單一頁面應用程式（SPA）使用 Open ID Connect （OIDC）來驗證使用者時，驗證狀態會在 SPA 和身分識別提供者（IP）中，以會話 cookie 的形式保留在本機，並以使用者提供其憑證.</span><span class="sxs-lookup"><span data-stu-id="97db6-105">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="97db6-106">IP 為使用者發出的權杖通常會在短時間內有效，大約一小時，因此用戶端應用程式必須定期提取新的權杖。</span><span class="sxs-lookup"><span data-stu-id="97db6-106">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="97db6-107">否則，在授與的權杖過期之後，使用者會被登出。</span><span class="sxs-lookup"><span data-stu-id="97db6-107">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="97db6-108">在大多數情況下，OIDC 用戶端可以布建新的權杖，而不需要使用者重新驗證，因為它會保留在 IP 內的驗證狀態或「會話」。</span><span class="sxs-lookup"><span data-stu-id="97db6-108">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="97db6-109">在某些情況下，用戶端無法在沒有使用者互動的情況下取得權杖，例如，基於某些原因，使用者明確地登出了 IP。</span><span class="sxs-lookup"><span data-stu-id="97db6-109">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="97db6-110">當使用者造訪 `https://login.microsoftonline.com` 並登出時，就會發生這種情況。在這些情況下，應用程式不會立即得知使用者是否已登出。用戶端持有的任何權杖可能不再有效。</span><span class="sxs-lookup"><span data-stu-id="97db6-110">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="97db6-111">此外，用戶端無法在目前權杖過期之後，不需要使用者互動就布建新權杖。</span><span class="sxs-lookup"><span data-stu-id="97db6-111">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="97db6-112">這些案例並不是以權杖為基礎的驗證所特有。</span><span class="sxs-lookup"><span data-stu-id="97db6-112">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="97db6-113">它們屬於 Spa 的本質。</span><span class="sxs-lookup"><span data-stu-id="97db6-113">They are part of the nature of SPAs.</span></span> <span data-ttu-id="97db6-114">如果移除驗證 cookie，使用 cookie 的 SPA 也無法呼叫伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="97db6-114">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="97db6-115">當應用程式對受保護的資源執行 API 呼叫時，您必須注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="97db6-115">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="97db6-116">若要布建新的存取權杖以呼叫 API，使用者可能需要再次進行驗證。</span><span class="sxs-lookup"><span data-stu-id="97db6-116">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="97db6-117">即使用戶端的權杖看似有效，對伺服器的呼叫可能會失敗，因為使用者已撤銷權杖。</span><span class="sxs-lookup"><span data-stu-id="97db6-117">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="97db6-118">當應用程式要求權杖時，會有兩種可能的結果：</span><span class="sxs-lookup"><span data-stu-id="97db6-118">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="97db6-119">要求成功，且應用程式具有有效的權杖。</span><span class="sxs-lookup"><span data-stu-id="97db6-119">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="97db6-120">要求失敗，且應用程式必須再次驗證使用者，才能取得新的權杖。</span><span class="sxs-lookup"><span data-stu-id="97db6-120">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="97db6-121">當令牌要求失敗時，您必須決定是否要在執行重新導向之前，先儲存任何目前的狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-121">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="97db6-122">有數種方法存在，並增加複雜性層級：</span><span class="sxs-lookup"><span data-stu-id="97db6-122">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="97db6-123">將目前的頁面狀態儲存在會話儲存體中。</span><span class="sxs-lookup"><span data-stu-id="97db6-123">Store the current page state in session storage.</span></span> <span data-ttu-id="97db6-124">在 `OnInitializeAsync`期間，檢查是否可以還原狀態，再繼續進行。</span><span class="sxs-lookup"><span data-stu-id="97db6-124">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="97db6-125">新增查詢字串參數，並使用它來通知應用程式它需要重新序列化先前儲存的狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-125">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="97db6-126">新增具有唯一識別碼的查詢字串參數，以將資料儲存在會話儲存體中，而不會有風險與其他專案衝突。</span><span class="sxs-lookup"><span data-stu-id="97db6-126">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="97db6-127">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="97db6-127">The following example shows how to:</span></span>

* <span data-ttu-id="97db6-128">在重新導向至登入頁面之前保留狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-128">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="97db6-129">使用查詢字串參數，在驗證之後復原先前的狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-129">Recover the previous state afterward authentication using the query string parameter.</span></span>

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

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="97db6-130">在驗證操作之前儲存應用程式狀態</span><span class="sxs-lookup"><span data-stu-id="97db6-130">Save app state before an authentication operation</span></span>

<span data-ttu-id="97db6-131">在驗證作業期間，某些情況下，您會想要在瀏覽器重新導向至 IP 之前，先儲存應用程式狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-131">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="97db6-132">當您使用類似狀態容器的情況，而且您想要在驗證成功之後還原狀態時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="97db6-132">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="97db6-133">您可以使用自訂驗證狀態物件來保留應用程式特定狀態或其參考，並在驗證作業成功完成後還原該狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-133">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="97db6-134">`Authentication` 元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="97db6-134">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

## <a name="request-additional-access-tokens"></a><span data-ttu-id="97db6-135">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="97db6-135">Request additional access tokens</span></span>

<span data-ttu-id="97db6-136">大部分的應用程式只需要存取權杖，即可與他們使用的受保護資源互動。</span><span class="sxs-lookup"><span data-stu-id="97db6-136">Most apps only require an access token to interact with the protected resources that they use.</span></span> <span data-ttu-id="97db6-137">在某些情況下，應用程式可能需要一個以上的權杖，才能與兩個或多個資源互動。</span><span class="sxs-lookup"><span data-stu-id="97db6-137">In some scenarios, an app might require more than one token in order to interact with two or more resources.</span></span> <span data-ttu-id="97db6-138">`IAccessTokenProvider.RequestToken` 方法提供多載，可讓應用程式使用一組指定的範圍來布建權杖，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="97db6-138">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision a token with a given set of scopes, as seen in the following example:</span></span>

```csharp
var tokenResult = await AuthenticationService.RequestAccessToken(
    new AccessTokenRequestOptions
    {
        Scopes = new[] { "https://graph.microsoft.com/Mail.Send", 
            "https://graph.microsoft.com/User.Read" }
    });
```

## <a name="customize-app-routes"></a><span data-ttu-id="97db6-139">自訂應用程式路由</span><span class="sxs-lookup"><span data-stu-id="97db6-139">Customize app routes</span></span>

<span data-ttu-id="97db6-140">根據預設，`Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫會使用下表所示的路由來代表不同的驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-140">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="97db6-141">路由</span><span class="sxs-lookup"><span data-stu-id="97db6-141">Route</span></span>                            | <span data-ttu-id="97db6-142">目的</span><span class="sxs-lookup"><span data-stu-id="97db6-142">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="97db6-143">觸發登入作業。</span><span class="sxs-lookup"><span data-stu-id="97db6-143">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="97db6-144">處理任何登入作業的結果。</span><span class="sxs-lookup"><span data-stu-id="97db6-144">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="97db6-145">當登入作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="97db6-145">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="97db6-146">觸發登出作業。</span><span class="sxs-lookup"><span data-stu-id="97db6-146">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="97db6-147">處理登出作業的結果。</span><span class="sxs-lookup"><span data-stu-id="97db6-147">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="97db6-148">當登出作業因某些原因而失敗時，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="97db6-148">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="97db6-149">表示使用者已成功登出。</span><span class="sxs-lookup"><span data-stu-id="97db6-149">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="97db6-150">觸發操作以編輯使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="97db6-150">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="97db6-151">觸發操作以註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="97db6-151">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="97db6-152">上表中顯示的路由可透過 `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`來設定。</span><span class="sxs-lookup"><span data-stu-id="97db6-152">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="97db6-153">設定選項以提供自訂路由時，請確認應用程式具有處理每個路徑的路由。</span><span class="sxs-lookup"><span data-stu-id="97db6-153">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="97db6-154">在下列範例中，所有路徑的前面都會加上 `/security`。</span><span class="sxs-lookup"><span data-stu-id="97db6-154">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="97db6-155">`Authentication` 元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="97db6-155">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="97db6-156">`Program.Main` （*Program.cs*）：</span><span class="sxs-lookup"><span data-stu-id="97db6-156">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="97db6-157">如果需求會呼叫完全不同的路徑，請設定前面所述的路由，並使用明確的 action 參數轉譯 `RemoteAuthenticatorView`：</span><span class="sxs-lookup"><span data-stu-id="97db6-157">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="97db6-158">如果您選擇這樣做，則可以將 UI 分成不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="97db6-158">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="97db6-159">自訂驗證使用者介面</span><span class="sxs-lookup"><span data-stu-id="97db6-159">Customize the authentication user interface</span></span>

<span data-ttu-id="97db6-160">`RemoteAuthenticatorView` 包含每個驗證狀態的一組預設 UI 部分。</span><span class="sxs-lookup"><span data-stu-id="97db6-160">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="97db6-161">您可以藉由傳入自訂 `RenderFragment`來自訂每個狀態。</span><span class="sxs-lookup"><span data-stu-id="97db6-161">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="97db6-162">若要在初始登入程式期間自訂顯示的文字，可以變更 `RemoteAuthenticatorView`，如下所示。</span><span class="sxs-lookup"><span data-stu-id="97db6-162">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="97db6-163">`Authentication` 元件（*Pages/Authentication. razor*）：</span><span class="sxs-lookup"><span data-stu-id="97db6-163">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

<span data-ttu-id="97db6-164">`RemoteAuthenticatorView` 有一個片段，可用於下表所示的每個驗證路由。</span><span class="sxs-lookup"><span data-stu-id="97db6-164">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="97db6-165">路由</span><span class="sxs-lookup"><span data-stu-id="97db6-165">Route</span></span>                            | <span data-ttu-id="97db6-166">片段</span><span class="sxs-lookup"><span data-stu-id="97db6-166">Fragment</span></span>                |
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
