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
# <a name="aspnet-core-blazor-webassembly-additional-security-scenarios"></a><span data-ttu-id="222fa-102">ASP.NET核心 Blazor WebAssembly 其他安全方案</span><span class="sxs-lookup"><span data-stu-id="222fa-102">ASP.NET Core Blazor WebAssembly additional security scenarios</span></span>

<span data-ttu-id="222fa-103">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="222fa-103">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

## <a name="request-additional-access-tokens"></a><span data-ttu-id="222fa-104">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="222fa-104">Request additional access tokens</span></span>

<span data-ttu-id="222fa-105">大多數應用僅需要訪問權杖才能與其使用的受保護資源進行交互。</span><span class="sxs-lookup"><span data-stu-id="222fa-105">Most apps only require an access token to interact with the protected resources that they use.</span></span> <span data-ttu-id="222fa-106">在某些情況下,應用可能需要多個權杖才能與兩個或多個資源進行交互。</span><span class="sxs-lookup"><span data-stu-id="222fa-106">In some scenarios, an app might require more than one token in order to interact with two or more resources.</span></span>

<span data-ttu-id="222fa-107">在下面的範例中,應用需要其他 Azure 活動目錄 (AAD) Microsoft 圖形 API 作用域來讀取使用者數據和發送郵件。</span><span class="sxs-lookup"><span data-stu-id="222fa-107">In the following example, additional Azure Active Directory (AAD) Microsoft Graph API scopes are required by an app to read user data and send mail.</span></span> <span data-ttu-id="222fa-108">在 Azure AAD 門戶中添加 Microsoft 圖形 API 許可權後`Program.Main`,其他作用域在用戶端應用 *(Program.cs)* 中配置。</span><span class="sxs-lookup"><span data-stu-id="222fa-108">After adding the Microsoft Graph API permissions in the Azure AAD portal, the additional scopes are configured in the Client app (`Program.Main`, *Program.cs*):</span></span>

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

<span data-ttu-id="222fa-109">該方法`IAccessTokenProvider.RequestToken`提供重載,允許應用使用一組給定的範圍預配令牌,如下例所示:</span><span class="sxs-lookup"><span data-stu-id="222fa-109">The `IAccessTokenProvider.RequestToken` method provides an overload that allows an app to provision a token with a given set of scopes, as seen in the following example:</span></span>

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

<span data-ttu-id="222fa-110">`TryGetToken`返回:</span><span class="sxs-lookup"><span data-stu-id="222fa-110">`TryGetToken` returns:</span></span>

* <span data-ttu-id="222fa-111">`true`與`token`使用。</span><span class="sxs-lookup"><span data-stu-id="222fa-111">`true` with the `token` for use.</span></span>
* <span data-ttu-id="222fa-112">`false`如果未檢索令牌。</span><span class="sxs-lookup"><span data-stu-id="222fa-112">`false` if the token isn't retrieved.</span></span>

## <a name="handle-token-request-errors"></a><span data-ttu-id="222fa-113">處理權杖要求錯誤</span><span class="sxs-lookup"><span data-stu-id="222fa-113">Handle token request errors</span></span>

<span data-ttu-id="222fa-114">當單頁應用程式 (SPA) 使用開放 ID 連接 (OIDC) 對使用者進行身份驗證時,身份驗證狀態將在 SPA 中本地和標識提供者 (IP) 中以會話 Cookie 的形式維護,該狀態是使用者提供認證而設定的。</span><span class="sxs-lookup"><span data-stu-id="222fa-114">When a Single Page Application (SPA) authenticates a user using Open ID Connect (OIDC), the authentication state is maintained locally within the SPA and in the Identity Provider (IP) in the form of a session cookie that's set as a result of the user providing their credentials.</span></span>

<span data-ttu-id="222fa-115">IP 為使用者發出的權杖通常在短時間內有效,通常大約一小時,因此用戶端應用必須定期獲取新令牌。</span><span class="sxs-lookup"><span data-stu-id="222fa-115">The tokens that the IP emits for the user typically are valid for short periods of time, about one hour normally, so the client app must regularly fetch new tokens.</span></span> <span data-ttu-id="222fa-116">否則,使用者將在授予的權杖過期後註銷。</span><span class="sxs-lookup"><span data-stu-id="222fa-116">Otherwise, the user would be logged-out after the granted tokens expire.</span></span> <span data-ttu-id="222fa-117">在大多數情況下,由於IP中的身份驗證狀態或"工作階段",OIDC客戶端能夠預配新權杖,而無需使用者再次進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="222fa-117">In most cases, OIDC clients are able to provision new tokens without requiring the user to authenticate again thanks to the authentication state or "session" that is kept within the IP.</span></span>

<span data-ttu-id="222fa-118">在某些情況下,客戶端在沒有使用者交互的情況下無法獲取令牌,例如,由於某種原因,使用者顯式註銷了 IP。</span><span class="sxs-lookup"><span data-stu-id="222fa-118">There are some cases in which the client can't get a token without user interaction, for example, when for some reason the user explicitly logs out from the IP.</span></span> <span data-ttu-id="222fa-119">如果用戶訪問`https://login.microsoftonline.com`並註銷,將發生此情況。在這些情況下,應用不會立即知道使用者已註銷。用戶端持有的任何權杖可能不再有效。</span><span class="sxs-lookup"><span data-stu-id="222fa-119">This scenario occurs if a user visits `https://login.microsoftonline.com` and logs out. In these scenarios, the app doesn't know immediately that the user has logged out. Any token that the client holds might no longer be valid.</span></span> <span data-ttu-id="222fa-120">此外,在當前權杖過期後,用戶端無法預配沒有使用者互動的新權杖。</span><span class="sxs-lookup"><span data-stu-id="222fa-120">Also, the client isn't able to provision a new token without user interaction after the current token expires.</span></span>

<span data-ttu-id="222fa-121">這些方案不特定於基於令牌的身份驗證。</span><span class="sxs-lookup"><span data-stu-id="222fa-121">These scenarios aren't specific to token-based authentication.</span></span> <span data-ttu-id="222fa-122">它們是 SPA 性質的一部分。</span><span class="sxs-lookup"><span data-stu-id="222fa-122">They are part of the nature of SPAs.</span></span> <span data-ttu-id="222fa-123">如果刪除身份驗證 Cookie,則使用 Cookie 的 SPA 也無法呼叫伺服器 API。</span><span class="sxs-lookup"><span data-stu-id="222fa-123">An SPA using cookies also fails to call a server API if the authentication cookie is removed.</span></span>

<span data-ttu-id="222fa-124">當應用對受保護的資源執行 API 呼叫時,您必須注意以下事項:</span><span class="sxs-lookup"><span data-stu-id="222fa-124">When an app performs API calls to protected resources, you must be aware of the following:</span></span>

* <span data-ttu-id="222fa-125">要預配新的訪問權杖以調用 API,可能需要使用者再次進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="222fa-125">To provision a new access token to call the API, the user might be required to authenticate again.</span></span>
* <span data-ttu-id="222fa-126">即使用戶端具有看似有效的權杖,對伺服器的調用也可能失敗,因為令牌已被使用者吊銷。</span><span class="sxs-lookup"><span data-stu-id="222fa-126">Even if the client has a token that seems to be valid, the call to the server might fail because the token was revoked by the user.</span></span>

<span data-ttu-id="222fa-127">當應用請求權杖時,有兩種可能的結果:</span><span class="sxs-lookup"><span data-stu-id="222fa-127">When the app requests a token, there are two possible outcomes:</span></span>

* <span data-ttu-id="222fa-128">請求成功,並且應用具有有效的權杖。</span><span class="sxs-lookup"><span data-stu-id="222fa-128">The request succeeds, and the app has a valid token.</span></span>
* <span data-ttu-id="222fa-129">請求失敗,應用必須再次對使用者進行身份驗證才能獲得新令牌。</span><span class="sxs-lookup"><span data-stu-id="222fa-129">The request fails, and the app must authenticate the user again to obtain a new token.</span></span>

<span data-ttu-id="222fa-130">當權杖請求失敗時,您需要在執行重定向之前決定是否要儲存任何當前狀態。</span><span class="sxs-lookup"><span data-stu-id="222fa-130">When a token request fails, you need to decide whether you want to save any current state before you perform a redirection.</span></span> <span data-ttu-id="222fa-131">存在幾種方法,複雜性越來越高:</span><span class="sxs-lookup"><span data-stu-id="222fa-131">Several approaches exist with increasing levels of complexity:</span></span>

* <span data-ttu-id="222fa-132">將當前頁面狀態存儲在會話存儲中。</span><span class="sxs-lookup"><span data-stu-id="222fa-132">Store the current page state in session storage.</span></span> <span data-ttu-id="222fa-133">在`OnInitializeAsync`期間,檢查是否可以在繼續之前恢復狀態。</span><span class="sxs-lookup"><span data-stu-id="222fa-133">During `OnInitializeAsync`, check if state can be restored before continuing.</span></span>
* <span data-ttu-id="222fa-134">添加查詢字串參數,並將其用作向應用發出信號,使其需要重新補充以前保存的狀態的一種方式。</span><span class="sxs-lookup"><span data-stu-id="222fa-134">Add a query string parameter and use that as a way to signal the app that it needs to re-hydrate the previously saved state.</span></span>
* <span data-ttu-id="222fa-135">添加具有唯一標識符的查詢字串參數,將數據存儲在會話存儲中,而不會冒與其他項發生衝突的風險。</span><span class="sxs-lookup"><span data-stu-id="222fa-135">Add a query string parameter with a unique identifier to store data in session storage without risking collisions with other items.</span></span>

<span data-ttu-id="222fa-136">下列範例示範如何執行：</span><span class="sxs-lookup"><span data-stu-id="222fa-136">The following example shows how to:</span></span>

* <span data-ttu-id="222fa-137">在重定向到登錄頁之前保留狀態。</span><span class="sxs-lookup"><span data-stu-id="222fa-137">Preserve state before redirecting to the login page.</span></span>
* <span data-ttu-id="222fa-138">使用查詢字串參數恢復以前的狀態后身份驗證。</span><span class="sxs-lookup"><span data-stu-id="222fa-138">Recover the previous state afterward authentication using the query string parameter.</span></span>

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

## <a name="save-app-state-before-an-authentication-operation"></a><span data-ttu-id="222fa-139">在認證操作之前儲存套用狀態</span><span class="sxs-lookup"><span data-stu-id="222fa-139">Save app state before an authentication operation</span></span>

<span data-ttu-id="222fa-140">在身份驗證操作期間,在某些情況下,您希望在瀏覽器重定向到 IP 之前保存應用狀態。</span><span class="sxs-lookup"><span data-stu-id="222fa-140">During an authentication operation, there are cases where you want to save the app state before the browser is redirected to the IP.</span></span> <span data-ttu-id="222fa-141">當您使用狀態容器之類的內容,並且希望在身份驗證成功后還原狀態時,情況可能如此。</span><span class="sxs-lookup"><span data-stu-id="222fa-141">This can be the case when you are using something like a state container and you want to restore the state after the authentication succeeds.</span></span> <span data-ttu-id="222fa-142">可以使用自定義身份驗證狀態物件保留特定於應用的狀態或對它的引用,並在身份驗證操作成功完成後還原該狀態。</span><span class="sxs-lookup"><span data-stu-id="222fa-142">You can use a custom authentication state object to preserve app-specific state or a reference to it and restore that state once the authentication operation successfully completes.</span></span>

<span data-ttu-id="222fa-143">`Authentication`元件 (*頁 / 身份認證. razor) :*</span><span class="sxs-lookup"><span data-stu-id="222fa-143">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

## <a name="customize-app-routes"></a><span data-ttu-id="222fa-144">自訂應用路由</span><span class="sxs-lookup"><span data-stu-id="222fa-144">Customize app routes</span></span>

<span data-ttu-id="222fa-145">預設情況下,`Microsoft.AspNetCore.Components.WebAssembly.Authentication`庫使用下表中顯示的路由來表示不同的身份驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="222fa-145">By default, the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library uses the routes shown in the following table for representing different authentication states.</span></span>

| <span data-ttu-id="222fa-146">路由</span><span class="sxs-lookup"><span data-stu-id="222fa-146">Route</span></span>                            | <span data-ttu-id="222fa-147">目的</span><span class="sxs-lookup"><span data-stu-id="222fa-147">Purpose</span></span> |
| -------------------------------- | ------- |
| `authentication/login`           | <span data-ttu-id="222fa-148">觸發登錄操作。</span><span class="sxs-lookup"><span data-stu-id="222fa-148">Triggers a sign-in operation.</span></span> |
| `authentication/login-callback`  | <span data-ttu-id="222fa-149">處理任何登錄操作的結果。</span><span class="sxs-lookup"><span data-stu-id="222fa-149">Handles the result of any sign-in operation.</span></span> |
| `authentication/login-failed`    | <span data-ttu-id="222fa-150">當登錄操作由於某種原因失敗時顯示錯誤消息。</span><span class="sxs-lookup"><span data-stu-id="222fa-150">Displays error messages when the sign-in operation fails for some reason.</span></span> |
| `authentication/logout`          | <span data-ttu-id="222fa-151">觸發註銷操作。</span><span class="sxs-lookup"><span data-stu-id="222fa-151">Triggers a sign-out operation.</span></span> |
| `authentication/logout-callback` | <span data-ttu-id="222fa-152">處理註銷操作的結果。</span><span class="sxs-lookup"><span data-stu-id="222fa-152">Handles the result of a sign-out operation.</span></span> |
| `authentication/logout-failed`   | <span data-ttu-id="222fa-153">當註銷操作由於某種原因失敗時顯示錯誤消息。</span><span class="sxs-lookup"><span data-stu-id="222fa-153">Displays error messages when the sign-out operation fails for some reason.</span></span> |
| `authentication/logged-out`      | <span data-ttu-id="222fa-154">指示使用者已成功註銷。</span><span class="sxs-lookup"><span data-stu-id="222fa-154">Indicates that the user has successfully logout.</span></span> |
| `authentication/profile`         | <span data-ttu-id="222fa-155">觸發操作以編輯使用者配置檔。</span><span class="sxs-lookup"><span data-stu-id="222fa-155">Triggers an operation to edit the user profile.</span></span> |
| `authentication/register`        | <span data-ttu-id="222fa-156">觸發操作以註冊新使用者。</span><span class="sxs-lookup"><span data-stu-id="222fa-156">Triggers an operation to register a new user.</span></span> |

<span data-ttu-id="222fa-157">上表中顯示的路由可通過`RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`進行配置。</span><span class="sxs-lookup"><span data-stu-id="222fa-157">The routes shown in the preceding table are configurable via `RemoteAuthenticationOptions<TProviderOptions>.AuthenticationPaths`.</span></span> <span data-ttu-id="222fa-158">設置選項以提供自定義路由時,請確認應用具有處理每個路徑的路由。</span><span class="sxs-lookup"><span data-stu-id="222fa-158">When setting options to provide custom routes, confirm that the app has a route that handles each path.</span></span>

<span data-ttu-id="222fa-159">在下面的範例中,所有路徑都預定了`/security`。</span><span class="sxs-lookup"><span data-stu-id="222fa-159">In the following example, all the paths are prefixed with `/security`.</span></span>

<span data-ttu-id="222fa-160">`Authentication`元件 (*頁 / 身份認證. razor) :*</span><span class="sxs-lookup"><span data-stu-id="222fa-160">`Authentication` component (*Pages/Authentication.razor*):</span></span>

```razor
@page "/security/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code{
    [Parameter]
    public string Action { get; set; }
}
```

<span data-ttu-id="222fa-161">`Program.Main`*(Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="222fa-161">`Program.Main` (*Program.cs*):</span></span>

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

<span data-ttu-id="222fa-162">如果要求對完全不同的路徑進行調用,請按照前面所述設置路由,並`RemoteAuthenticatorView`呈現顯式操作參數:</span><span class="sxs-lookup"><span data-stu-id="222fa-162">If the requirement calls for completely different paths, set the routes as described previously and render the `RemoteAuthenticatorView` with an explicit action parameter:</span></span>

```razor
@page "/register"

<RemoteAuthenticatorView Action="@RemoteAuthenticationActions.Register" />
```

<span data-ttu-id="222fa-163">如果選擇這樣做,則可以將UI分解為不同的頁面。</span><span class="sxs-lookup"><span data-stu-id="222fa-163">You're allowed to break the UI into different pages if you choose to do so.</span></span>

## <a name="customize-the-authentication-user-interface"></a><span data-ttu-id="222fa-164">自訂身份驗證使用者介面</span><span class="sxs-lookup"><span data-stu-id="222fa-164">Customize the authentication user interface</span></span>

<span data-ttu-id="222fa-165">`RemoteAuthenticatorView`包括每個身份驗證狀態的預設 UI 部分集。</span><span class="sxs-lookup"><span data-stu-id="222fa-165">`RemoteAuthenticatorView` includes a default set of UI pieces for each authentication state.</span></span> <span data-ttu-id="222fa-166">每個狀態都可以通過傳入自定義`RenderFragment`來自定義。</span><span class="sxs-lookup"><span data-stu-id="222fa-166">Each state can be customized by passing in a custom `RenderFragment`.</span></span> <span data-ttu-id="222fa-167">在初始登入時自訂顯示的文字,可以按以下的`RemoteAuthenticatorView`變更 。</span><span class="sxs-lookup"><span data-stu-id="222fa-167">To customize the displayed text during the initial login process, can change the `RemoteAuthenticatorView` as follows.</span></span>

<span data-ttu-id="222fa-168">`Authentication`元件 (*頁 / 身份認證. razor) :*</span><span class="sxs-lookup"><span data-stu-id="222fa-168">`Authentication` component (*Pages/Authentication.razor*):</span></span>

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

<span data-ttu-id="222fa-169">有`RemoteAuthenticatorView`一個片段,可以按下表中顯示的每個身份驗證路由使用。</span><span class="sxs-lookup"><span data-stu-id="222fa-169">The `RemoteAuthenticatorView` has one fragment that can be used per authentication route shown in the following table.</span></span>

| <span data-ttu-id="222fa-170">路由</span><span class="sxs-lookup"><span data-stu-id="222fa-170">Route</span></span>                            | <span data-ttu-id="222fa-171">片段</span><span class="sxs-lookup"><span data-stu-id="222fa-171">Fragment</span></span>                |
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
