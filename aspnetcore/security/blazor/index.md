---
title: ASP.NET核心Blazor認證與授權
author: guardrex
description: 瞭解Blazor身份驗證和授權方案。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: 04bbf20d1d848edfa98e719f316b790c812bfd95
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80501317"
---
# <a name="aspnet-core-opno-locblazor-authentication-and-authorization"></a><span data-ttu-id="a6646-103">ASP.NET核心Blazor認證與授權</span><span class="sxs-lookup"><span data-stu-id="a6646-103">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="a6646-104">由[史蒂夫·桑德森](https://github.com/SteveSandersonMS)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a6646-104">By [Steve Sanderson](https://github.com/SteveSandersonMS) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

> [!NOTE]
> <span data-ttu-id="a6646-105">對於本文中適用於BlazorWeb 裝配的指導,需要 ASP.NET 核心BlazorWeb 組裝範本版本 3.2 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="a6646-105">For the guidance in this article that applies to Blazor WebAssembly, the ASP.NET Core Blazor WebAssembly template version 3.2 or later is required.</span></span> <span data-ttu-id="a6646-106">如果您不使用 Visual Studio 版本 16.6 預覽版或Blazor更高版本,請使用<xref:blazor/get-started>中的指南獲取最新的 Web 組裝範本。</span><span class="sxs-lookup"><span data-stu-id="a6646-106">If you aren't using Visual Studio version 16.6 Preview 2 or later, obtain the latest Blazor WebAssembly template by following the guidance in <xref:blazor/get-started>.</span></span>

<span data-ttu-id="a6646-107">ASP.NET核心支援Blazor應用程式中安全性的配置和管理。</span><span class="sxs-lookup"><span data-stu-id="a6646-107">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="a6646-108">伺服器和BlazorBlazorWeb 組裝應用之間的安全方案不同。</span><span class="sxs-lookup"><span data-stu-id="a6646-108">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="a6646-109">由於Blazor伺服器應用在伺服器上運行,因此授權檢查能夠確定:</span><span class="sxs-lookup"><span data-stu-id="a6646-109">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="a6646-110">要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。</span><span class="sxs-lookup"><span data-stu-id="a6646-110">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="a6646-111">適用於應用程式和元件之特定區域的存取規則。</span><span class="sxs-lookup"><span data-stu-id="a6646-111">Access rules for areas of the app and components.</span></span>

Blazor<span data-ttu-id="a6646-112">在用戶端上運行 Web 組裝應用。</span><span class="sxs-lookup"><span data-stu-id="a6646-112"> WebAssembly apps run on the client.</span></span> <span data-ttu-id="a6646-113">授權「僅」\*\* 會被用來決定要顯示的 UI 選項。</span><span class="sxs-lookup"><span data-stu-id="a6646-113">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="a6646-114">由於使用者可以修改或繞過客戶端檢查Blazor, 因此 WebAssembly 應用無法強制實施授權存取規則。</span><span class="sxs-lookup"><span data-stu-id="a6646-114">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="a6646-115">[剃刀頁授權約定](xref:security/authorization/razor-pages-authorization)不適用於可路由的 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="a6646-115">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="a6646-116">如果[嵌入](xref:blazor/integrate-components#render-components-from-a-page-or-view)了不可路由的 Razor 元件,則該頁的授權約定會間接影響 Razor 元件以及頁面的其他內容。</span><span class="sxs-lookup"><span data-stu-id="a6646-116">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

> [!NOTE]
> <span data-ttu-id="a6646-117"><xref:Microsoft.AspNetCore.Identity.SignInManager%601>並且<xref:Microsoft.AspNetCore.Identity.UserManager%601>在 Razor 元件中不受支援。</span><span class="sxs-lookup"><span data-stu-id="a6646-117"><xref:Microsoft.AspNetCore.Identity.SignInManager%601> and <xref:Microsoft.AspNetCore.Identity.UserManager%601> aren't supported in Razor components.</span></span>

## <a name="authentication"></a><span data-ttu-id="a6646-118">驗證</span><span class="sxs-lookup"><span data-stu-id="a6646-118">Authentication</span></span>

Blazor<span data-ttu-id="a6646-119">使用現有的ASP.NET核心身份驗證機制來建立使用者的身份。</span><span class="sxs-lookup"><span data-stu-id="a6646-119"> uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="a6646-120">確切的機制取決於Blazor應用程式的託管方式、WebBlazor組裝Blazor或 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a6646-120">The exact mechanism depends on how the Blazor app is hosted, Blazor WebAssembly or Blazor Server.</span></span>

### <a name="opno-locblazor-webassembly-authentication"></a>Blazor<span data-ttu-id="a6646-121">Web 組裝身分驗證</span><span class="sxs-lookup"><span data-stu-id="a6646-121"> WebAssembly authentication</span></span>

<span data-ttu-id="a6646-122">在BlazorWebAssembly 應用中,可以繞過身份驗證檢查,因為使用者可以修改所有用戶端代碼。</span><span class="sxs-lookup"><span data-stu-id="a6646-122">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="a6646-123">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6646-123">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="a6646-124">新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="a6646-124">Add the following:</span></span>

* <span data-ttu-id="a6646-125">[Microsoft.AspNetCore.元件的](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)包引用.對應用的專案檔的授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-125">A package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>
* <span data-ttu-id="a6646-126">`Microsoft.AspNetCore.Components.Authorization`應用 *_Imports.razor*檔的名稱空間。</span><span class="sxs-lookup"><span data-stu-id="a6646-126">The `Microsoft.AspNetCore.Components.Authorization` namespace to the app's *_Imports.razor* file.</span></span>

<span data-ttu-id="a6646-127">要處理身份驗證,以下各節將介紹內置或自定義`AuthenticationStateProvider`服務的實現。</span><span class="sxs-lookup"><span data-stu-id="a6646-127">To handle authentication, implementation of a built-in or custom `AuthenticationStateProvider` service is covered in the following sections.</span></span>

<span data-ttu-id="a6646-128">有關建立應用和設定的詳細資訊,請參閱<xref:security/blazor/webassembly/index>。</span><span class="sxs-lookup"><span data-stu-id="a6646-128">For more information on creating apps and configuration, see <xref:security/blazor/webassembly/index>.</span></span>

### <a name="opno-locblazor-server-authentication"></a>Blazor<span data-ttu-id="a6646-129">伺服器身份驗證</span><span class="sxs-lookup"><span data-stu-id="a6646-129"> Server authentication</span></span>

Blazor<span data-ttu-id="a6646-130">伺服器應用透過使用SignalR創建的即時連接運行。</span><span class="sxs-lookup"><span data-stu-id="a6646-130"> Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="a6646-131">建立連線時,將處理[SignalR基於中的應用程式中的身份驗證](xref:signalr/authn-and-authz)。</span><span class="sxs-lookup"><span data-stu-id="a6646-131">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="a6646-132">驗證可以是以 Cookie 或其他持有人權杖為基礎。</span><span class="sxs-lookup"><span data-stu-id="a6646-132">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="a6646-133">有關建立應用和設定的詳細資訊,請參閱<xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="a6646-133">For more information on creating apps and configuration, see <xref:security/blazor/server>.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="a6646-134">AuthenticationStateProvider 服務</span><span class="sxs-lookup"><span data-stu-id="a6646-134">AuthenticationStateProvider service</span></span>

<span data-ttu-id="a6646-135">內置`AuthenticationStateProvider`服務從ASP.NET Core 獲取`HttpContext.User`身份驗證狀態數據。</span><span class="sxs-lookup"><span data-stu-id="a6646-135">The built-in `AuthenticationStateProvider` service obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="a6646-136">這是身份驗證狀態與現有ASP.NET核心身份驗證機制集成的方式。</span><span class="sxs-lookup"><span data-stu-id="a6646-136">This is how authentication state integrates with existing ASP.NET Core authentication mechanisms.</span></span>

<span data-ttu-id="a6646-137">`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。</span><span class="sxs-lookup"><span data-stu-id="a6646-137">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="a6646-138">您通常不會直接使用 `AuthenticationStateProvider`。</span><span class="sxs-lookup"><span data-stu-id="a6646-138">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="a6646-139">使用本文後面介紹的[「授權檢視」元件](#authorizeview-component)或[\<任務 身份驗證狀態>](#expose-the-authentication-state-as-a-cascading-parameter)方法。</span><span class="sxs-lookup"><span data-stu-id="a6646-139">Use the [AuthorizeView component](#authorizeview-component) or [Task\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="a6646-140">使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。</span><span class="sxs-lookup"><span data-stu-id="a6646-140">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="a6646-141">`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a6646-141">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

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

<span data-ttu-id="a6646-142">如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="a6646-142">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="a6646-143">如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="a6646-143">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="a6646-144">實作自訂 AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="a6646-144">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="a6646-145">如果應用程式需要自訂提供者,則實現`AuthenticationStateProvider`並重`GetAuthenticationStateAsync`寫 :</span><span class="sxs-lookup"><span data-stu-id="a6646-145">If the app requires a custom provider, implement `AuthenticationStateProvider` and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="a6646-146">在BlazorWebAssembly 應用`CustomAuthStateProvider`中,`Main`服務註冊在*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a6646-146">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="a6646-147">在Blazor伺服器應用中`CustomAuthStateProvider`, 該服務`Startup.ConfigureServices`在中註冊:</span><span class="sxs-lookup"><span data-stu-id="a6646-147">In a Blazor Server app, the `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="a6646-148">`CustomAuthStateProvider`使用前面的示例中,所有使用者都使用使用者名`mrfibuli`進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="a6646-148">Using the `CustomAuthStateProvider` in the preceding example, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="a6646-149">將驗證狀態公開為階層式參數</span><span class="sxs-lookup"><span data-stu-id="a6646-149">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="a6646-150">如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：</span><span class="sxs-lookup"><span data-stu-id="a6646-150">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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

<span data-ttu-id="a6646-151">如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="a6646-151">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="a6646-152">`Task<AuthenticationState>``AuthorizeRouteView`使用`App`元件中的元件`CascadingAuthenticationState`*(App.razor)* 設定級聯參數 :</span><span class="sxs-lookup"><span data-stu-id="a6646-152">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the `App` component (*App.razor*):</span></span>

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

<span data-ttu-id="a6646-153">在BlazorWebAssembly 應用程式中,將選項的服務與授權`Program.Main`加入 :</span><span class="sxs-lookup"><span data-stu-id="a6646-153">In a Blazor WebAssembly App, add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

<span data-ttu-id="a6646-154">在BlazorServer 應用中,選項和授權的服務已經存在,因此無需執行進一步操作。</span><span class="sxs-lookup"><span data-stu-id="a6646-154">In a Blazor Server app, services for options and authorization are already present, so no further action is required.</span></span>

## <a name="authorization"></a><span data-ttu-id="a6646-155">授權</span><span class="sxs-lookup"><span data-stu-id="a6646-155">Authorization</span></span>

<span data-ttu-id="a6646-156">在使用者被驗證之後，系統便會套用「授權」\*\* 規則以控制使用者可以執行的動作。</span><span class="sxs-lookup"><span data-stu-id="a6646-156">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="a6646-157">存取權通常會依據下列情況來授與或拒絕：</span><span class="sxs-lookup"><span data-stu-id="a6646-157">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="a6646-158">使用者已通過驗證 (已登入)。</span><span class="sxs-lookup"><span data-stu-id="a6646-158">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="a6646-159">使用者屬於某個「角色」\*\*。</span><span class="sxs-lookup"><span data-stu-id="a6646-159">A user is in a *role*.</span></span>
* <span data-ttu-id="a6646-160">使用者具有「宣告」\*\*。</span><span class="sxs-lookup"><span data-stu-id="a6646-160">A user has a *claim*.</span></span>
* <span data-ttu-id="a6646-161">已滿足某個「原則」\*\*。</span><span class="sxs-lookup"><span data-stu-id="a6646-161">A *policy* is satisfied.</span></span>

<span data-ttu-id="a6646-162">上述概念皆和 ASP.NET Core MVC 或 Razor Pages 應用程式中的概念相同。</span><span class="sxs-lookup"><span data-stu-id="a6646-162">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="a6646-163">如需 ASP.NET Core 安全性的詳細資訊，請參閱 [ASP.NET Core 安全性與身分識別](xref:security/index)下的文章。</span><span class="sxs-lookup"><span data-stu-id="a6646-163">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="a6646-164">AuthorizeView 元件</span><span class="sxs-lookup"><span data-stu-id="a6646-164">AuthorizeView component</span></span>

<span data-ttu-id="a6646-165">`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。</span><span class="sxs-lookup"><span data-stu-id="a6646-165">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="a6646-166">此方法可讓您只需要向使用者「顯示」\*\* 資料，而不需要在程序性邏輯中使用該使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="a6646-166">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="a6646-167">該元件會公開 `AuthenticationState` 類型的 `context` 變數，您可以使用它來存取已登入使用者的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="a6646-167">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="a6646-168">您也可以在使用者未經授權的情況下，提供不同的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="a6646-168">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="a6646-169">該`AuthorizeView`元件可用於`NavMenu`元件 *(Shared/NavMenu.razor)* 來`<li>...</li>``NavLink`顯示 的 清單項 ( ),但請注意,此方法僅從呈現的輸出中刪除清單項。</span><span class="sxs-lookup"><span data-stu-id="a6646-169">The `AuthorizeView` component can be used in the `NavMenu` component (*Shared/NavMenu.razor*) to display a list item (`<li>...</li>`) for a `NavLink`, but note that this approach only removes the list item from the rendered output.</span></span> <span data-ttu-id="a6646-170">它不會阻止用戶導航到元件。</span><span class="sxs-lookup"><span data-stu-id="a6646-170">It doesn't prevent the user from navigating to the component.</span></span>

<span data-ttu-id="a6646-171">`<Authorized>`和`<NotAuthorized>`標記的內容可以包括任意專案,如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="a6646-171">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="a6646-172">授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。</span><span class="sxs-lookup"><span data-stu-id="a6646-172">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="a6646-173">如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：</span><span class="sxs-lookup"><span data-stu-id="a6646-173">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="a6646-174">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-174">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="a6646-175">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-175">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="a6646-176">角色型和原則型授權</span><span class="sxs-lookup"><span data-stu-id="a6646-176">Role-based and policy-based authorization</span></span>

<span data-ttu-id="a6646-177">`AuthorizeView` 元件支援「角色型」\*\* 或「原則型」\*\* 授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-177">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="a6646-178">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="a6646-178">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="a6646-179">如需詳細資訊，請參閱 <xref:security/authorization/roles>。</span><span class="sxs-lookup"><span data-stu-id="a6646-179">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="a6646-180">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="a6646-180">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="a6646-181">宣告型授權是特殊案例的原則型授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-181">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="a6646-182">例如，您可以定義要求使用者具備特定宣告的原則。</span><span class="sxs-lookup"><span data-stu-id="a6646-182">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="a6646-183">如需詳細資訊，請參閱 <xref:security/authorization/policies>。</span><span class="sxs-lookup"><span data-stu-id="a6646-183">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="a6646-184">這些 APIBlazor可用於Blazor伺服器或 Web 組裝應用。</span><span class="sxs-lookup"><span data-stu-id="a6646-184">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="a6646-185">如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。</span><span class="sxs-lookup"><span data-stu-id="a6646-185">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="a6646-186">在非同步驗證期間所顯示的內容</span><span class="sxs-lookup"><span data-stu-id="a6646-186">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="a6646-187">允許非*同步*確定身份驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="a6646-187"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="a6646-188">此方法的主要方案是在BlazorWebAssembly 應用中,這些應用向外部終結點發出身份驗證請求。</span><span class="sxs-lookup"><span data-stu-id="a6646-188">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="a6646-189">在驗證期間，`AuthorizeView` 預設不會顯示任何內容。</span><span class="sxs-lookup"><span data-stu-id="a6646-189">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="a6646-190">若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：</span><span class="sxs-lookup"><span data-stu-id="a6646-190">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="a6646-191">此方法通常不適用於Blazor伺服器應用。</span><span class="sxs-lookup"><span data-stu-id="a6646-191">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="a6646-192">伺服器應用在建立狀態后立即知道身份驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="a6646-192"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="a6646-193">`Authorizing`內容可以在Blazor伺服器應用`AuthorizeView`的元件中提供,但內容永遠不會顯示。</span><span class="sxs-lookup"><span data-stu-id="a6646-193">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="a6646-194">[Authorize] 屬性</span><span class="sxs-lookup"><span data-stu-id="a6646-194">[Authorize] attribute</span></span>

<span data-ttu-id="a6646-195">該`[Authorize]`屬性可用於 Razor 元件:</span><span class="sxs-lookup"><span data-stu-id="a6646-195">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="a6646-196">僅對`[Authorize]``@page`通過路由器到達的Blazor元件使用。</span><span class="sxs-lookup"><span data-stu-id="a6646-196">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="a6646-197">授權僅會以路由的層面執行，且「不」\*\* 適用於在頁面內轉譯的子元件。</span><span class="sxs-lookup"><span data-stu-id="a6646-197">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="a6646-198">若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。</span><span class="sxs-lookup"><span data-stu-id="a6646-198">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="a6646-199">`[Authorize]` 屬性也支援角色型或原則型授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-199">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="a6646-200">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="a6646-200">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="a6646-201">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="a6646-201">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="a6646-202">如果未指定 `Roles` 與 `Policy`，`[Authorize]` 便會使用預設原則，它預設會將：</span><span class="sxs-lookup"><span data-stu-id="a6646-202">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="a6646-203">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-203">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="a6646-204">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="a6646-204">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="a6646-205">搭配 Router 元件自訂未經授權的內容</span><span class="sxs-lookup"><span data-stu-id="a6646-205">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="a6646-206">此`Router`元件與`AuthorizeRouteView`元件結合使用,允許應用程式在以下情況下指定自訂內容:</span><span class="sxs-lookup"><span data-stu-id="a6646-206">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="a6646-207">找不到內容。</span><span class="sxs-lookup"><span data-stu-id="a6646-207">Content isn't found.</span></span>
* <span data-ttu-id="a6646-208">使用者無法滿足套用至元件的 `[Authorize]` 條件。</span><span class="sxs-lookup"><span data-stu-id="a6646-208">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="a6646-209">屬性部分涵蓋該`[Authorize]`屬性[`[Authorize]`](#authorize-attribute)。</span><span class="sxs-lookup"><span data-stu-id="a6646-209">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="a6646-210">正在進行非同步驗證。</span><span class="sxs-lookup"><span data-stu-id="a6646-210">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="a6646-211">在預設Blazor的伺服器項目樣本中,`App`元件 (*App.razor*) 展示如何設定自訂內容:</span><span class="sxs-lookup"><span data-stu-id="a6646-211">In the default Blazor Server project template, the `App` component (*App.razor*) demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="a6646-212">和`<Authorizing>`標`<NotFound>`記`<NotAuthorized>`的內容 可以包括任意項,如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="a6646-212">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="a6646-213">如果未指定`<NotAuthorized>`元素,`AuthorizeRouteView`則使用以下回退訊息:</span><span class="sxs-lookup"><span data-stu-id="a6646-213">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="a6646-214">關於驗證狀態變更的通知</span><span class="sxs-lookup"><span data-stu-id="a6646-214">Notification about authentication state changes</span></span>

<span data-ttu-id="a6646-215">如果應用確定基礎身份驗證狀態數據已更改(例如,由於用戶註銷或其他使用者更改了其角色),[則自定義身份驗證狀態提供者](#implement-a-custom-authenticationstateprovider)可以選擇`NotifyAuthenticationStateChanged``AuthenticationStateProvider`調用 基類上的方法。</span><span class="sxs-lookup"><span data-stu-id="a6646-215">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a [custom AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider) can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="a6646-216">這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。</span><span class="sxs-lookup"><span data-stu-id="a6646-216">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="a6646-217">程序性邏輯</span><span class="sxs-lookup"><span data-stu-id="a6646-217">Procedural logic</span></span>

<span data-ttu-id="a6646-218">如果作為程序性邏輯的一部分，應用程式必須檢查授權規則，請使用 `Task<AuthenticationState>` 類型的階層式參數來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="a6646-218">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="a6646-219">`Task<AuthenticationState>` 可以與其他服務 (例如 `IAuthorizationService`) 結合來評估原則。</span><span class="sxs-lookup"><span data-stu-id="a6646-219">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

```razor
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
> <span data-ttu-id="a6646-220">在BlazorWebAssembly 套件中`Microsoft.AspNetCore.Authorization`,`Microsoft.AspNetCore.Components.Authorization`加入與命名空間:</span><span class="sxs-lookup"><span data-stu-id="a6646-220">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```
>
> <span data-ttu-id="a6646-221">通過將這些命名空間添加到應用的 *_Imports.razor*檔案中,可以全域提供這些命名空間。</span><span class="sxs-lookup"><span data-stu-id="a6646-221">These namespaces can be provided globally by adding them to the app's *_Imports.razor* file.</span></span>

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a><span data-ttu-id="a6646-222">Web 組Blazor裝 應用程式中的授權</span><span class="sxs-lookup"><span data-stu-id="a6646-222">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="a6646-223">在BlazorWebAssembly 應用中,可以繞過授權檢查,因為使用者可以修改所有用戶端代碼。</span><span class="sxs-lookup"><span data-stu-id="a6646-223">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="a6646-224">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6646-224">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="a6646-225">**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**</span><span class="sxs-lookup"><span data-stu-id="a6646-225">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="a6646-226">有關詳細資訊,請參閱<xref:security/blazor/webassembly/index>下的文章。</span><span class="sxs-lookup"><span data-stu-id="a6646-226">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="a6646-227">針對錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="a6646-227">Troubleshoot errors</span></span>

<span data-ttu-id="a6646-228">常見錯誤：</span><span class="sxs-lookup"><span data-stu-id="a6646-228">Common errors:</span></span>

* <span data-ttu-id="a6646-229">**授權需要任務\<身份驗證狀態>類型的級聯參數。請考慮使用級聯身份驗證狀態來提供此功能。**</span><span class="sxs-lookup"><span data-stu-id="a6646-229">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="a6646-230">**`null`收到的值`authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="a6646-230">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="a6646-231">專案很可能不是使用啟用身份驗證的Blazor伺服器範本創建的。</span><span class="sxs-lookup"><span data-stu-id="a6646-231">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="a6646-232">`<CascadingAuthenticationState>`環繞 UI 樹的某些部分,`App`例如在 元件 (*App.razor*) 中,如下所示:</span><span class="sxs-lookup"><span data-stu-id="a6646-232">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in the `App` component (*App.razor*) as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="a6646-233">`CascadingAuthenticationState` 會提供 `Task<AuthenticationState>` 階層式參數，它接著會接收自底層 `AuthenticationStateProvider` DI 服務。</span><span class="sxs-lookup"><span data-stu-id="a6646-233">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6646-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="a6646-234">Additional resources</span></span>

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="a6646-235">[真棒Blazor: 驗證](https://github.com/AdrienTorris/awesome-blazor#authentication)社區範例</span><span class="sxs-lookup"><span data-stu-id="a6646-235">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
