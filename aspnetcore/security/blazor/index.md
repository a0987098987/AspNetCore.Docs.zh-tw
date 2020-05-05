---
title: ASP.NET Core Blazor驗證和授權
author: guardrex
description: 瞭解Blazor驗證和授權案例。
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
uid: security/blazor/index
ms.openlocfilehash: d55880265ed1ceedf8f115412e5ac47309521239
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82772891"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="ab889-103">ASP.NET Core Blazor驗證和授權</span><span class="sxs-lookup"><span data-stu-id="ab889-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="ab889-104">作者： [Steve Sanderson](https://github.com/SteveSandersonMS)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ab889-104">By [Steve Sanderson](https://github.com/SteveSandersonMS) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

> [!NOTE]
> <span data-ttu-id="ab889-105">如需本文中適用于Blazor WebAssembly 的指導方針，需要 ASP.NET Core Blazor WebAssembly 範本3.2 版或更新版本。</span><span class="sxs-lookup"><span data-stu-id="ab889-105">For the guidance in this article that applies to Blazor WebAssembly, the ASP.NET Core Blazor WebAssembly template version 3.2 or later is required.</span></span> <span data-ttu-id="ab889-106">如果您不是使用 Visual Studio 16.6 版 Preview 2 或更新版本，請Blazor遵循中<xref:blazor/get-started>的指引來取得最新的 WebAssembly 範本。</span><span class="sxs-lookup"><span data-stu-id="ab889-106">If you aren't using Visual Studio version 16.6 Preview 2 or later, obtain the latest Blazor WebAssembly template by following the guidance in <xref:blazor/get-started>.</span></span>

<span data-ttu-id="ab889-107">ASP.NET Core 支援在應用程式中Blazor設定和管理安全性。</span><span class="sxs-lookup"><span data-stu-id="ab889-107">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="ab889-108">伺服器和Blazor WebAssembly 應用Blazor程式之間的安全性案例不同。</span><span class="sxs-lookup"><span data-stu-id="ab889-108">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="ab889-109">因為Blazor伺服器應用程式是在伺服器上執行，所以授權檢查能夠判斷：</span><span class="sxs-lookup"><span data-stu-id="ab889-109">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="ab889-110">要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。</span><span class="sxs-lookup"><span data-stu-id="ab889-110">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="ab889-111">適用於應用程式和元件之特定區域的存取規則。</span><span class="sxs-lookup"><span data-stu-id="ab889-111">Access rules for areas of the app and components.</span></span>

Blazor<span data-ttu-id="ab889-112">WebAssembly 應用程式會在用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="ab889-112"> WebAssembly apps run on the client.</span></span> <span data-ttu-id="ab889-113">授權「僅」\*\* 會被用來決定要顯示的 UI 選項。</span><span class="sxs-lookup"><span data-stu-id="ab889-113">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="ab889-114">由於用戶端檢查可由使用者修改或略過，因此Blazor WebAssembly 應用程式無法強制執行授權存取規則。</span><span class="sxs-lookup"><span data-stu-id="ab889-114">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="ab889-115">頁面授權慣例不適用於可路由Razor的元件。 [ Razor ](xref:security/authorization/razor-pages-authorization)</span><span class="sxs-lookup"><span data-stu-id="ab889-115">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="ab889-116">如果無法路由Razor傳送的元件[內嵌在頁面中](xref:blazor/integrate-components#render-components-from-a-page-or-view)，頁面的授權慣例會間接影響此Razor元件以及頁面內容的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ab889-116">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

> [!NOTE]
> <span data-ttu-id="ab889-117"><xref:Microsoft.AspNetCore.Identity.SignInManager%601>元件<xref:Microsoft.AspNetCore.Identity.UserManager%601>中Razor不支援和。</span><span class="sxs-lookup"><span data-stu-id="ab889-117"><xref:Microsoft.AspNetCore.Identity.SignInManager%601> and <xref:Microsoft.AspNetCore.Identity.UserManager%601> aren't supported in Razor components.</span></span>

## <a name="authentication"></a><span data-ttu-id="ab889-118">驗證</span><span class="sxs-lookup"><span data-stu-id="ab889-118">Authentication</span></span>

Blazor<span data-ttu-id="ab889-119">會使用現有的 ASP.NET Core 驗證機制來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="ab889-119"> uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="ab889-120">確切的機制取決於應用程式Blazor的主控方式、 Blazor WebAssembly 或Blazor伺服器。</span><span class="sxs-lookup"><span data-stu-id="ab889-120">The exact mechanism depends on how the Blazor app is hosted, Blazor WebAssembly or Blazor Server.</span></span>

### <a name="blazor-webassembly-authentication"></a>Blazor<span data-ttu-id="ab889-121">WebAssembly authentication</span><span class="sxs-lookup"><span data-stu-id="ab889-121"> WebAssembly authentication</span></span>

<span data-ttu-id="ab889-122">在Blazor WebAssembly apps 中，可以略過驗證檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="ab889-122">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="ab889-123">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="ab889-123">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="ab889-124">新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="ab889-124">Add the following:</span></span>

* <span data-ttu-id="ab889-125">AspNetCore 的套件參考。應用程式的專案檔的[授權](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)。</span><span class="sxs-lookup"><span data-stu-id="ab889-125">A package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>
* <span data-ttu-id="ab889-126">應用`Microsoft.AspNetCore.Components.Authorization`程式的 *_Imports razor*檔案的命名空間。</span><span class="sxs-lookup"><span data-stu-id="ab889-126">The `Microsoft.AspNetCore.Components.Authorization` namespace to the app's *_Imports.razor* file.</span></span>

<span data-ttu-id="ab889-127">若要處理驗證，內建或自訂`AuthenticationStateProvider`服務的執行將在下列各節中討論。</span><span class="sxs-lookup"><span data-stu-id="ab889-127">To handle authentication, implementation of a built-in or custom `AuthenticationStateProvider` service is covered in the following sections.</span></span>

<span data-ttu-id="ab889-128">如需建立應用程式和設定的詳細資訊<xref:security/blazor/webassembly/index>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="ab889-128">For more information on creating apps and configuration, see <xref:security/blazor/webassembly/index>.</span></span>

### <a name="blazor-server-authentication"></a>Blazor<span data-ttu-id="ab889-129">伺服器驗證</span><span class="sxs-lookup"><span data-stu-id="ab889-129"> Server authentication</span></span>

Blazor<span data-ttu-id="ab889-130">伺服器應用程式會透過使用SignalR建立的即時連線來運作。</span><span class="sxs-lookup"><span data-stu-id="ab889-130"> Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="ab889-131">建立連接時，會處理[以為基礎之應用SignalR程式中的驗證](xref:signalr/authn-and-authz)。</span><span class="sxs-lookup"><span data-stu-id="ab889-131">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="ab889-132">驗證可以是以 Cookie 或其他持有人權杖為基礎。</span><span class="sxs-lookup"><span data-stu-id="ab889-132">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="ab889-133">如需建立應用程式和設定的詳細資訊<xref:security/blazor/server/index>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="ab889-133">For more information on creating apps and configuration, see <xref:security/blazor/server/index>.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="ab889-134">AuthenticationStateProvider 服務</span><span class="sxs-lookup"><span data-stu-id="ab889-134">AuthenticationStateProvider service</span></span>

<span data-ttu-id="ab889-135">內`AuthenticationStateProvider`建服務會從 ASP.NET Core 的`HttpContext.User`取得驗證狀態資料。</span><span class="sxs-lookup"><span data-stu-id="ab889-135">The built-in `AuthenticationStateProvider` service obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="ab889-136">這是驗證狀態與現有 ASP.NET Core 驗證機制整合的方式。</span><span class="sxs-lookup"><span data-stu-id="ab889-136">This is how authentication state integrates with existing ASP.NET Core authentication mechanisms.</span></span>

<span data-ttu-id="ab889-137">`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。</span><span class="sxs-lookup"><span data-stu-id="ab889-137">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="ab889-138">您通常不會直接使用 `AuthenticationStateProvider`。</span><span class="sxs-lookup"><span data-stu-id="ab889-138">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="ab889-139">請使用本文稍後所述的[AuthorizeView 元件](#authorizeview-component)或工作[\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter)方法。</span><span class="sxs-lookup"><span data-stu-id="ab889-139">Use the [AuthorizeView component](#authorizeview-component) or [Task\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="ab889-140">使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。</span><span class="sxs-lookup"><span data-stu-id="ab889-140">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="ab889-141">`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="ab889-141">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

```razor
@page "/"
@using System.Security.Claims
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<h3>ClaimsPrincipal Data</h3>

<button @onclick="GetClaimsPrincipalData">Get ClaimsPrincipal Data</button>

<p>@_authMessage</p>

@if (_claims.Count() > 0)
{
    <ul>
        @foreach (var claim in _claims)
        {
            <li>@claim.Type &ndash; @claim.Value</li>
        }
    </ul>
}

<p>@_surnameMessage</p>

@code {
    private string _authMessage;
    private string _surnameMessage;
    private IEnumerable<Claim> _claims = Enumerable.Empty<Claim>();

    private async Task GetClaimsPrincipalData()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            _authMessage = $"{user.Identity.Name} is authenticated.";
            _claims = user.Claims;
            _surnameMessage = 
                $"Surname: {user.FindFirst(c => c.Type == ClaimTypes.Surname)?.Value}";
        }
        else
        {
            _authMessage = "The user is NOT authenticated.";
        }
    }
}
```

<span data-ttu-id="ab889-142">如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="ab889-142">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="ab889-143">如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="ab889-143">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="ab889-144">實作自訂 AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="ab889-144">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="ab889-145">如果應用程式需要自訂提供者， `AuthenticationStateProvider`請執行`GetAuthenticationStateAsync`並覆寫：</span><span class="sxs-lookup"><span data-stu-id="ab889-145">If the app requires a custom provider, implement `AuthenticationStateProvider` and override `GetAuthenticationStateAsync`:</span></span>

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

public class CustomAuthStateProvider : AuthenticationStateProvider
{
    public override Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, "mrfibuli"),
        }, "Fake authentication type");

        var user = new ClaimsPrincipal(identity);

        return Task.FromResult(new AuthenticationState(user));
    }
}
```

<span data-ttu-id="ab889-146">在Blazor WebAssembly 應用程式中， `CustomAuthStateProvider`服務會在`Main` *Program.cs*中註冊：</span><span class="sxs-lookup"><span data-stu-id="ab889-146">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="ab889-147">在Blazor伺服器應用程式中， `CustomAuthStateProvider`服務會在中`Startup.ConfigureServices`註冊：</span><span class="sxs-lookup"><span data-stu-id="ab889-147">In a Blazor Server app, the `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="ab889-148">使用上述`CustomAuthStateProvider`範例中的，所有使用者都會使用 username `mrfibuli`進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ab889-148">Using the `CustomAuthStateProvider` in the preceding example, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="ab889-149">將驗證狀態公開為階層式參數</span><span class="sxs-lookup"><span data-stu-id="ab889-149">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="ab889-150">如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：</span><span class="sxs-lookup"><span data-stu-id="ab889-150">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

<p>@_authMessage</p>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private string _authMessage;

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            _authMessage = $"{user.Identity.Name} is authenticated.";
        }
        else
        {
            _authMessage = "The user is NOT authenticated.";
        }
    }
}
```

<span data-ttu-id="ab889-151">如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="ab889-151">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="ab889-152">`Task<AuthenticationState>` `AuthorizeRouteView`使用`App`元件中的和`CascadingAuthenticationState`元件（*app.config*）來設定串聯參數：</span><span class="sxs-lookup"><span data-stu-id="ab889-152">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the `App` component (*App.razor*):</span></span>

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="ab889-153">在Blazor WebAssembly 應用程式中，將選項和授權的服務`Program.Main`新增至：</span><span class="sxs-lookup"><span data-stu-id="ab889-153">In a Blazor WebAssembly App, add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

<span data-ttu-id="ab889-154">在Blazor伺服器應用程式中，選項和授權的服務已存在，因此不需要採取進一步的動作。</span><span class="sxs-lookup"><span data-stu-id="ab889-154">In a Blazor Server app, services for options and authorization are already present, so no further action is required.</span></span>

## <a name="authorization"></a><span data-ttu-id="ab889-155">授權</span><span class="sxs-lookup"><span data-stu-id="ab889-155">Authorization</span></span>

<span data-ttu-id="ab889-156">在使用者被驗證之後，系統便會套用「授權」\*\* 規則以控制使用者可以執行的動作。</span><span class="sxs-lookup"><span data-stu-id="ab889-156">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="ab889-157">存取權通常會依據下列情況來授與或拒絕：</span><span class="sxs-lookup"><span data-stu-id="ab889-157">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="ab889-158">使用者已通過驗證 (已登入)。</span><span class="sxs-lookup"><span data-stu-id="ab889-158">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="ab889-159">使用者屬於某個「角色」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ab889-159">A user is in a *role*.</span></span>
* <span data-ttu-id="ab889-160">使用者具有「宣告」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ab889-160">A user has a *claim*.</span></span>
* <span data-ttu-id="ab889-161">已滿足某個「原則」\*\*。</span><span class="sxs-lookup"><span data-stu-id="ab889-161">A *policy* is satisfied.</span></span>

<span data-ttu-id="ab889-162">這些概念都與 ASP.NET Core MVC 或Razor Pages 應用程式中的相同。</span><span class="sxs-lookup"><span data-stu-id="ab889-162">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="ab889-163">如需 ASP.NET Core 安全性的詳細資訊，請參閱[ASP.NET Core 安全性和Identity](xref:security/index)底下的文章。</span><span class="sxs-lookup"><span data-stu-id="ab889-163">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="ab889-164">AuthorizeView 元件</span><span class="sxs-lookup"><span data-stu-id="ab889-164">AuthorizeView component</span></span>

<span data-ttu-id="ab889-165">`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。</span><span class="sxs-lookup"><span data-stu-id="ab889-165">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="ab889-166">此方法可讓您只需要向使用者「顯示」\*\* 資料，而不需要在程序性邏輯中使用該使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="ab889-166">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="ab889-167">該元件會公開 `AuthenticationState` 類型的 `context` 變數，您可以使用它來存取已登入使用者的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="ab889-167">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="ab889-168">您也可以在使用者未經授權的情況下，提供不同的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="ab889-168">You can also supply different content for display if the user isn't authenticated:</span></span>

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

<span data-ttu-id="ab889-169">`AuthorizeView`元件可以在`NavMenu`元件（*Shared/navmenu.cshtml*）中用來顯示的清單專案（`<li>...</li>`） `NavLink`，但請注意，此方法只會從轉譯的輸出中移除清單專案。</span><span class="sxs-lookup"><span data-stu-id="ab889-169">The `AuthorizeView` component can be used in the `NavMenu` component (*Shared/NavMenu.razor*) to display a list item (`<li>...</li>`) for a `NavLink`, but note that this approach only removes the list item from the rendered output.</span></span> <span data-ttu-id="ab889-170">它不會防止使用者流覽至元件。</span><span class="sxs-lookup"><span data-stu-id="ab889-170">It doesn't prevent the user from navigating to the component.</span></span>

<span data-ttu-id="ab889-171">`<Authorized>`和`<NotAuthorized>`標記的內容可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="ab889-171">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="ab889-172">授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。</span><span class="sxs-lookup"><span data-stu-id="ab889-172">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="ab889-173">如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：</span><span class="sxs-lookup"><span data-stu-id="ab889-173">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="ab889-174">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-174">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="ab889-175">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-175">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="ab889-176">角色型和原則型授權</span><span class="sxs-lookup"><span data-stu-id="ab889-176">Role-based and policy-based authorization</span></span>

<span data-ttu-id="ab889-177">`AuthorizeView` 元件支援「角色型」\*\* 或「原則型」\*\* 授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-177">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="ab889-178">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="ab889-178">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="ab889-179">如需詳細資訊，請參閱<xref:security/authorization/roles>。</span><span class="sxs-lookup"><span data-stu-id="ab889-179">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="ab889-180">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="ab889-180">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="ab889-181">宣告型授權是特殊案例的原則型授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-181">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="ab889-182">例如，您可以定義要求使用者具備特定宣告的原則。</span><span class="sxs-lookup"><span data-stu-id="ab889-182">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="ab889-183">如需詳細資訊，請參閱<xref:security/authorization/policies>。</span><span class="sxs-lookup"><span data-stu-id="ab889-183">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="ab889-184">這些 Api 可以在Blazor伺服器或Blazor WebAssembly 應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="ab889-184">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="ab889-185">如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。</span><span class="sxs-lookup"><span data-stu-id="ab889-185">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="ab889-186">在非同步驗證期間所顯示的內容</span><span class="sxs-lookup"><span data-stu-id="ab889-186">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="ab889-187">允許以*非同步方式*判斷驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="ab889-187"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="ab889-188">這種方法的主要案例是在Blazor WebAssembly 應用程式中，對外部端點提出驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="ab889-188">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="ab889-189">在驗證期間，`AuthorizeView` 預設不會顯示任何內容。</span><span class="sxs-lookup"><span data-stu-id="ab889-189">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="ab889-190">若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：</span><span class="sxs-lookup"><span data-stu-id="ab889-190">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

<span data-ttu-id="ab889-191">這種方法通常不適用Blazor于伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="ab889-191">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="ab889-192">伺服器應用程式會在狀態建立後立即知道驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="ab889-192"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="ab889-193">`Authorizing`內容可以在Blazor伺服器應用程式的`AuthorizeView`元件中提供，但永遠不會顯示內容。</span><span class="sxs-lookup"><span data-stu-id="ab889-193">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="ab889-194">[Authorize] 屬性</span><span class="sxs-lookup"><span data-stu-id="ab889-194">[Authorize] attribute</span></span>

<span data-ttu-id="ab889-195">`[Authorize]`屬性可以在元件中Razor使用：</span><span class="sxs-lookup"><span data-stu-id="ab889-195">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="ab889-196">僅在`[Authorize]`透過`@page` Blazor路由器到達的元件上使用。</span><span class="sxs-lookup"><span data-stu-id="ab889-196">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="ab889-197">授權僅會以路由的層面執行，且「不」\*\* 適用於在頁面內轉譯的子元件。</span><span class="sxs-lookup"><span data-stu-id="ab889-197">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="ab889-198">若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。</span><span class="sxs-lookup"><span data-stu-id="ab889-198">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="ab889-199">`[Authorize]` 屬性也支援角色型或原則型授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-199">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="ab889-200">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="ab889-200">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="ab889-201">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="ab889-201">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="ab889-202">如果未指定 `Roles` 與 `Policy`，`[Authorize]` 便會使用預設原則，它預設會將：</span><span class="sxs-lookup"><span data-stu-id="ab889-202">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="ab889-203">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-203">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="ab889-204">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="ab889-204">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="ab889-205">搭配 Router 元件自訂未經授權的內容</span><span class="sxs-lookup"><span data-stu-id="ab889-205">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="ab889-206">`Router`元件與`AuthorizeRouteView`元件結合，可讓應用程式在下列情況指定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="ab889-206">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="ab889-207">找不到內容。</span><span class="sxs-lookup"><span data-stu-id="ab889-207">Content isn't found.</span></span>
* <span data-ttu-id="ab889-208">使用者無法滿足套用至元件的 `[Authorize]` 條件。</span><span class="sxs-lookup"><span data-stu-id="ab889-208">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="ab889-209">屬性`[Authorize]` [ `[Authorize]`一節](#authorize-attribute)中會涵蓋屬性。</span><span class="sxs-lookup"><span data-stu-id="ab889-209">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="ab889-210">正在進行非同步驗證。</span><span class="sxs-lookup"><span data-stu-id="ab889-210">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="ab889-211">在預設Blazor的伺服器專案範本中， `App`元件（*razor*）會示範如何設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="ab889-211">In the default Blazor Server project template, the `App` component (*App.razor*) demonstrates how to set custom content:</span></span>

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

<span data-ttu-id="ab889-212">`<NotFound>`、 `<NotAuthorized>`和`<Authorizing>`標記的內容可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="ab889-212">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="ab889-213">如果未`<NotAuthorized>`指定元素，會`AuthorizeRouteView`使用下列回溯訊息：</span><span class="sxs-lookup"><span data-stu-id="ab889-213">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="ab889-214">關於驗證狀態變更的通知</span><span class="sxs-lookup"><span data-stu-id="ab889-214">Notification about authentication state changes</span></span>

<span data-ttu-id="ab889-215">如果應用程式判斷基礎驗證狀態資料已變更（例如，使用者登出或其他使用者已變更其角色），[自訂 AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider)可以選擇性地叫用`NotifyAuthenticationStateChanged` `AuthenticationStateProvider`基類上的方法。</span><span class="sxs-lookup"><span data-stu-id="ab889-215">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a [custom AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider) can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="ab889-216">這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。</span><span class="sxs-lookup"><span data-stu-id="ab889-216">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="ab889-217">程序性邏輯</span><span class="sxs-lookup"><span data-stu-id="ab889-217">Procedural logic</span></span>

<span data-ttu-id="ab889-218">如果作為程序性邏輯的一部分，應用程式必須檢查授權規則，請使用 `Task<AuthenticationState>` 類型的階層式參數來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="ab889-218">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="ab889-219">`Task<AuthenticationState>` 可以與其他服務 (例如 `IAuthorizationService`) 結合來評估原則。</span><span class="sxs-lookup"><span data-stu-id="ab889-219">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="ab889-220">在Blazor WebAssembly 應用程式元件中，新增`Microsoft.AspNetCore.Authorization`和`Microsoft.AspNetCore.Components.Authorization`命名空間：</span><span class="sxs-lookup"><span data-stu-id="ab889-220">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```
>
> <span data-ttu-id="ab889-221">這些命名空間可以藉由將它們新增至應用程式的 *_Imports razor*檔案來全域提供。</span><span class="sxs-lookup"><span data-stu-id="ab889-221">These namespaces can be provided globally by adding them to the app's *_Imports.razor* file.</span></span>

## <a name="authorization-in-blazor-webassembly-apps"></a><span data-ttu-id="ab889-222">WebAssembly apps Blazor中的授權</span><span class="sxs-lookup"><span data-stu-id="ab889-222">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="ab889-223">在Blazor WebAssembly apps 中，可以略過授權檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="ab889-223">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="ab889-224">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="ab889-224">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="ab889-225">**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**</span><span class="sxs-lookup"><span data-stu-id="ab889-225">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="ab889-226">如需詳細資訊，請參閱底下<xref:security/blazor/webassembly/index>的文章。</span><span class="sxs-lookup"><span data-stu-id="ab889-226">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="ab889-227">針對錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="ab889-227">Troubleshoot errors</span></span>

<span data-ttu-id="ab889-228">常見錯誤：</span><span class="sxs-lookup"><span data-stu-id="ab889-228">Common errors:</span></span>

* <span data-ttu-id="ab889-229">**授權需要類型為 Task\<AuthenticationState> 的串聯參數。請考慮使用 CascadingAuthenticationState 來提供此。**</span><span class="sxs-lookup"><span data-stu-id="ab889-229">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="ab889-230">**`null`接收的值`authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="ab889-230">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="ab889-231">專案可能不是使用已啟用驗證的Blazor伺服器範本所建立。</span><span class="sxs-lookup"><span data-stu-id="ab889-231">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="ab889-232">`<CascadingAuthenticationState>`將 UI 樹狀結構的某些部分（例如，在`App`元件（*razor*）中）包裝起來，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ab889-232">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in the `App` component (*App.razor*) as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="ab889-233">`CascadingAuthenticationState` 會提供 `Task<AuthenticationState>` 階層式參數，它接著會接收自底層 `AuthenticationStateProvider` DI 服務。</span><span class="sxs-lookup"><span data-stu-id="ab889-233">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab889-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="ab889-234">Additional resources</span></span>

* <xref:security/index>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="ab889-235">[卓越Blazor：驗證](https://github.com/AdrienTorris/awesome-blazor#authentication)社區範例連結</span><span class="sxs-lookup"><span data-stu-id="ab889-235">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
