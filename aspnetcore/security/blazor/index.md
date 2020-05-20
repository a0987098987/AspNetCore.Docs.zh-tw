---
<span data-ttu-id="f6e95-101">標題： ' ASP.NET Core Blazor 驗證和授權 ' 作者：描述：「瞭解 Blazor 驗證和授權案例」。</span><span class="sxs-lookup"><span data-stu-id="f6e95-101">title: 'ASP.NET Core Blazor authentication and authorization' author: description: 'Learn about Blazor authentication and authorization scenarios.'</span></span>
<span data-ttu-id="f6e95-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="f6e95-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="f6e95-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="f6e95-103">'Blazor'</span></span>
- <span data-ttu-id="f6e95-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="f6e95-104">'Identity'</span></span>
- <span data-ttu-id="f6e95-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="f6e95-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="f6e95-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="f6e95-106">'Razor'</span></span>
- <span data-ttu-id="f6e95-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="f6e95-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="f6e95-108">ASP.NET Core Blazor 驗證和授權</span><span class="sxs-lookup"><span data-stu-id="f6e95-108">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="f6e95-109">作者： [Steve Sanderson](https://github.com/SteveSandersonMS)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f6e95-109">By [Steve Sanderson](https://github.com/SteveSandersonMS) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f6e95-110">ASP.NET Core 支援在應用程式中設定和管理安全性 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-110">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="f6e95-111">Blazor伺服器和 Blazor WebAssembly 應用程式之間的安全性案例不同。</span><span class="sxs-lookup"><span data-stu-id="f6e95-111">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="f6e95-112">因為 Blazor 伺服器應用程式是在伺服器上執行，所以授權檢查能夠判斷：</span><span class="sxs-lookup"><span data-stu-id="f6e95-112">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="f6e95-113">要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。</span><span class="sxs-lookup"><span data-stu-id="f6e95-113">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="f6e95-114">適用於應用程式和元件之特定區域的存取規則。</span><span class="sxs-lookup"><span data-stu-id="f6e95-114">Access rules for areas of the app and components.</span></span>

Blazor<span data-ttu-id="f6e95-115">WebAssembly 應用程式會在用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="f6e95-115"> WebAssembly apps run on the client.</span></span> <span data-ttu-id="f6e95-116">授權「僅」\*\* 會被用來決定要顯示的 UI 選項。</span><span class="sxs-lookup"><span data-stu-id="f6e95-116">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="f6e95-117">由於用戶端檢查可由使用者修改或略過，因此 Blazor WebAssembly 應用程式無法強制執行授權存取規則。</span><span class="sxs-lookup"><span data-stu-id="f6e95-117">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="f6e95-118">[ Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization)不適用於可路由的 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-118">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="f6e95-119">如果無法路由傳送的 Razor 元件[內嵌在頁面中](xref:blazor/integrate-components#render-components-from-a-page-or-view)，頁面的授權慣例會間接影響此 Razor 元件以及頁面內容的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="f6e95-119">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

> [!NOTE]
> <span data-ttu-id="f6e95-120"><xref:Microsoft.AspNetCore.Identity.SignInManager%601><xref:Microsoft.AspNetCore.Identity.UserManager%601>元件中不支援和 Razor 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-120"><xref:Microsoft.AspNetCore.Identity.SignInManager%601> and <xref:Microsoft.AspNetCore.Identity.UserManager%601> aren't supported in Razor components.</span></span>

## <a name="authentication"></a><span data-ttu-id="f6e95-121">驗證</span><span class="sxs-lookup"><span data-stu-id="f6e95-121">Authentication</span></span>

Blazor<span data-ttu-id="f6e95-122">會使用現有的 ASP.NET Core 驗證機制來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="f6e95-122"> uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="f6e95-123">確切的機制取決於 Blazor 應用程式的主控方式、 Blazor WebAssembly 或 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6e95-123">The exact mechanism depends on how the Blazor app is hosted, Blazor WebAssembly or Blazor Server.</span></span>

### <a name="blazor-webassembly-authentication"></a>Blazor<span data-ttu-id="f6e95-124">WebAssembly authentication</span><span class="sxs-lookup"><span data-stu-id="f6e95-124"> WebAssembly authentication</span></span>

<span data-ttu-id="f6e95-125">在 Blazor WebAssembly apps 中，可以略過驗證檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="f6e95-125">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="f6e95-126">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6e95-126">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="f6e95-127">新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="f6e95-127">Add the following:</span></span>

* <span data-ttu-id="f6e95-128">AspNetCore 的套件參考。應用程式的專案檔的[授權](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)。</span><span class="sxs-lookup"><span data-stu-id="f6e95-128">A package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>
* <span data-ttu-id="f6e95-129">`Microsoft.AspNetCore.Components.Authorization`應用程式的 *_Imports razor*檔案的命名空間。</span><span class="sxs-lookup"><span data-stu-id="f6e95-129">The `Microsoft.AspNetCore.Components.Authorization` namespace to the app's *_Imports.razor* file.</span></span>

<span data-ttu-id="f6e95-130">若要處理驗證，內建或自訂服務的執行 `AuthenticationStateProvider` 將在下列各節中討論。</span><span class="sxs-lookup"><span data-stu-id="f6e95-130">To handle authentication, implementation of a built-in or custom `AuthenticationStateProvider` service is covered in the following sections.</span></span>

<span data-ttu-id="f6e95-131">如需建立應用程式和設定的詳細資訊，請參閱 <xref:security/blazor/webassembly/index> 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-131">For more information on creating apps and configuration, see <xref:security/blazor/webassembly/index>.</span></span>

### <a name="blazor-server-authentication"></a>Blazor<span data-ttu-id="f6e95-132">伺服器驗證</span><span class="sxs-lookup"><span data-stu-id="f6e95-132"> Server authentication</span></span>

Blazor<span data-ttu-id="f6e95-133">伺服器應用程式會透過使用建立的即時連線來運作 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-133"> Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="f6e95-134">建立連接時，會處理[以為 SignalR 基礎之應用程式中的驗證](xref:signalr/authn-and-authz)。</span><span class="sxs-lookup"><span data-stu-id="f6e95-134">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="f6e95-135">驗證可以是以 Cookie 或其他持有人權杖為基礎。</span><span class="sxs-lookup"><span data-stu-id="f6e95-135">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="f6e95-136">如需建立應用程式和設定的詳細資訊，請參閱 <xref:security/blazor/server/index> 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-136">For more information on creating apps and configuration, see <xref:security/blazor/server/index>.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="f6e95-137">AuthenticationStateProvider 服務</span><span class="sxs-lookup"><span data-stu-id="f6e95-137">AuthenticationStateProvider service</span></span>

<span data-ttu-id="f6e95-138">內建服務會 `AuthenticationStateProvider` 從 ASP.NET Core 的取得驗證狀態資料 `HttpContext.User` 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-138">The built-in `AuthenticationStateProvider` service obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="f6e95-139">這是驗證狀態與現有 ASP.NET Core 驗證機制整合的方式。</span><span class="sxs-lookup"><span data-stu-id="f6e95-139">This is how authentication state integrates with existing ASP.NET Core authentication mechanisms.</span></span>

<span data-ttu-id="f6e95-140">`AuthenticationStateProvider` 是 `AuthorizeView` 元件與 `CascadingAuthenticationState` 元件用來取得驗證狀態的基礎服務。</span><span class="sxs-lookup"><span data-stu-id="f6e95-140">`AuthenticationStateProvider` is the underlying service used by the `AuthorizeView` component and `CascadingAuthenticationState` component to get the authentication state.</span></span>

<span data-ttu-id="f6e95-141">您通常不會直接使用 `AuthenticationStateProvider`。</span><span class="sxs-lookup"><span data-stu-id="f6e95-141">You don't typically use `AuthenticationStateProvider` directly.</span></span> <span data-ttu-id="f6e95-142">請使用本文稍後所述的[AuthorizeView 元件](#authorizeview-component)或工作[ \< AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter)方法。</span><span class="sxs-lookup"><span data-stu-id="f6e95-142">Use the [AuthorizeView component](#authorizeview-component) or [Task\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="f6e95-143">使用 `AuthenticationStateProvider` 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-143">The main drawback to using `AuthenticationStateProvider` directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="f6e95-144">`AuthenticationStateProvider` 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="f6e95-144">The `AuthenticationStateProvider` service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

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

<span data-ttu-id="f6e95-145">如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="f6e95-145">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="f6e95-146">如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="f6e95-146">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="f6e95-147">實作自訂 AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="f6e95-147">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="f6e95-148">如果應用程式需要自訂提供者，請執行 `AuthenticationStateProvider` 並覆寫 `GetAuthenticationStateAsync` ：</span><span class="sxs-lookup"><span data-stu-id="f6e95-148">If the app requires a custom provider, implement `AuthenticationStateProvider` and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="f6e95-149">在 Blazor WebAssembly 應用程式中， `CustomAuthStateProvider` 服務會在 `Main` *Program.cs*中註冊：</span><span class="sxs-lookup"><span data-stu-id="f6e95-149">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="f6e95-150">在 Blazor 伺服器應用程式中， `CustomAuthStateProvider` 服務會在中註冊 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="f6e95-150">In a Blazor Server app, the `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="f6e95-151">使用 `CustomAuthStateProvider` 上述範例中的，所有使用者都會使用 username 進行驗證 `mrfibuli` 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-151">Using the `CustomAuthStateProvider` in the preceding example, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="f6e95-152">將驗證狀態公開為階層式參數</span><span class="sxs-lookup"><span data-stu-id="f6e95-152">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="f6e95-153">如果驗證狀態資料需要程序性邏輯 (例如在執行由使用者觸發的動作時)，請透過定義 `Task<AuthenticationState>` 類型的階層式參數來取得驗證狀態資料：</span><span class="sxs-lookup"><span data-stu-id="f6e95-153">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<AuthenticationState>`:</span></span>

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

<span data-ttu-id="f6e95-154">如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="f6e95-154">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="f6e95-155">`Task<AuthenticationState>`使用 `AuthorizeRouteView` `CascadingAuthenticationState` 元件中的和元件 `App` （*app.config*）來設定串聯參數：</span><span class="sxs-lookup"><span data-stu-id="f6e95-155">Set up the `Task<AuthenticationState>` cascading parameter using the `AuthorizeRouteView` and `CascadingAuthenticationState` components in the `App` component (*App.razor*):</span></span>

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

<span data-ttu-id="f6e95-156">在 Blazor WebAssembly 應用程式中，將選項和授權的服務新增至 `Program.Main` ：</span><span class="sxs-lookup"><span data-stu-id="f6e95-156">In a Blazor WebAssembly App, add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

<span data-ttu-id="f6e95-157">在 Blazor 伺服器應用程式中，選項和授權的服務已存在，因此不需要採取進一步的動作。</span><span class="sxs-lookup"><span data-stu-id="f6e95-157">In a Blazor Server app, services for options and authorization are already present, so no further action is required.</span></span>

## <a name="authorization"></a><span data-ttu-id="f6e95-158">授權</span><span class="sxs-lookup"><span data-stu-id="f6e95-158">Authorization</span></span>

<span data-ttu-id="f6e95-159">在使用者被驗證之後，系統便會套用「授權」\*\* 規則以控制使用者可以執行的動作。</span><span class="sxs-lookup"><span data-stu-id="f6e95-159">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="f6e95-160">存取權通常會依據下列情況來授與或拒絕：</span><span class="sxs-lookup"><span data-stu-id="f6e95-160">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="f6e95-161">使用者已通過驗證 (已登入)。</span><span class="sxs-lookup"><span data-stu-id="f6e95-161">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="f6e95-162">使用者屬於某個「角色」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6e95-162">A user is in a *role*.</span></span>
* <span data-ttu-id="f6e95-163">使用者具有「宣告」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6e95-163">A user has a *claim*.</span></span>
* <span data-ttu-id="f6e95-164">已滿足某個「原則」\*\*。</span><span class="sxs-lookup"><span data-stu-id="f6e95-164">A *policy* is satisfied.</span></span>

<span data-ttu-id="f6e95-165">這些概念都與 ASP.NET Core MVC 或 Pages 應用程式中的相同 Razor 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-165">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="f6e95-166">如需 ASP.NET Core 安全性的詳細資訊，請參閱[ASP.NET Core 安全性和 Identity ](xref:security/index)底下的文章。</span><span class="sxs-lookup"><span data-stu-id="f6e95-166">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="f6e95-167">AuthorizeView 元件</span><span class="sxs-lookup"><span data-stu-id="f6e95-167">AuthorizeView component</span></span>

<span data-ttu-id="f6e95-168">`AuthorizeView` 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。</span><span class="sxs-lookup"><span data-stu-id="f6e95-168">The `AuthorizeView` component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="f6e95-169">此方法可讓您只需要向使用者「顯示」\*\* 資料，而不需要在程序性邏輯中使用該使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="f6e95-169">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="f6e95-170">該元件會公開 `AuthenticationState` 類型的 `context` 變數，您可以使用它來存取已登入使用者的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="f6e95-170">The component exposes a `context` variable of type `AuthenticationState`, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="f6e95-171">您也可以在使用者未經授權的情況下，提供不同的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="f6e95-171">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="f6e95-172">`AuthorizeView`元件可以在 `NavMenu` 元件（*Shared/navmenu.cshtml*）中用來顯示的清單專案（ `<li>...</li>` ） `NavLink` ，但請注意，此方法只會從轉譯的輸出中移除清單專案。</span><span class="sxs-lookup"><span data-stu-id="f6e95-172">The `AuthorizeView` component can be used in the `NavMenu` component (*Shared/NavMenu.razor*) to display a list item (`<li>...</li>`) for a `NavLink`, but note that this approach only removes the list item from the rendered output.</span></span> <span data-ttu-id="f6e95-173">它不會防止使用者流覽至元件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-173">It doesn't prevent the user from navigating to the component.</span></span>

<span data-ttu-id="f6e95-174">`<Authorized>`和標記的內容 `<NotAuthorized>` 可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-174">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="f6e95-175">授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。</span><span class="sxs-lookup"><span data-stu-id="f6e95-175">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="f6e95-176">如果未指定授權條件，`AuthorizeView` 會使用預設的原則，並將：</span><span class="sxs-lookup"><span data-stu-id="f6e95-176">If authorization conditions aren't specified, `AuthorizeView` uses a default policy and treats:</span></span>

* <span data-ttu-id="f6e95-177">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-177">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="f6e95-178">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-178">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="f6e95-179">角色型和原則型授權</span><span class="sxs-lookup"><span data-stu-id="f6e95-179">Role-based and policy-based authorization</span></span>

<span data-ttu-id="f6e95-180">`AuthorizeView` 元件支援「角色型」\*\* 或「原則型」\*\* 授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-180">The `AuthorizeView` component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="f6e95-181">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="f6e95-181">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="f6e95-182">如需詳細資訊，請參閱<xref:security/authorization/roles>。</span><span class="sxs-lookup"><span data-stu-id="f6e95-182">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="f6e95-183">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="f6e95-183">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="f6e95-184">宣告型授權是特殊案例的原則型授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-184">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="f6e95-185">例如，您可以定義要求使用者具備特定宣告的原則。</span><span class="sxs-lookup"><span data-stu-id="f6e95-185">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="f6e95-186">如需詳細資訊，請參閱<xref:security/authorization/policies>。</span><span class="sxs-lookup"><span data-stu-id="f6e95-186">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="f6e95-187">這些 Api 可以在 Blazor 伺服器或 Blazor WebAssembly 應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="f6e95-187">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="f6e95-188">如果未指定 `Roles` 和 `Policy`，`AuthorizeView` 便會使用預設原則。</span><span class="sxs-lookup"><span data-stu-id="f6e95-188">If neither `Roles` nor `Policy` is specified, `AuthorizeView` uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="f6e95-189">在非同步驗證期間所顯示的內容</span><span class="sxs-lookup"><span data-stu-id="f6e95-189">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="f6e95-190">允許以*非同步方式*判斷驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="f6e95-190"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="f6e95-191">這種方法的主要案例是在 Blazor WebAssembly 應用程式中，對外部端點提出驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="f6e95-191">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="f6e95-192">在驗證期間，`AuthorizeView` 預設不會顯示任何內容。</span><span class="sxs-lookup"><span data-stu-id="f6e95-192">While authentication is in progress, `AuthorizeView` displays no content by default.</span></span> <span data-ttu-id="f6e95-193">若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：</span><span class="sxs-lookup"><span data-stu-id="f6e95-193">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="f6e95-194">這種方法通常不適用於 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6e95-194">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="f6e95-195">伺服器應用程式會在狀態建立後立即知道驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="f6e95-195"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="f6e95-196">`Authorizing`內容可以在 Blazor 伺服器應用程式的元件中提供 `AuthorizeView` ，但永遠不會顯示內容。</span><span class="sxs-lookup"><span data-stu-id="f6e95-196">`Authorizing` content can be provided in a Blazor Server app's `AuthorizeView` component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="f6e95-197">[Authorize] 屬性</span><span class="sxs-lookup"><span data-stu-id="f6e95-197">[Authorize] attribute</span></span>

<span data-ttu-id="f6e95-198">`[Authorize]`屬性可以在元件中使用 Razor ：</span><span class="sxs-lookup"><span data-stu-id="f6e95-198">The `[Authorize]` attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="f6e95-199">僅 `[Authorize]` 在透過 `@page` 路由器到達的元件上使用 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-199">Only use `[Authorize]` on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="f6e95-200">授權僅會以路由的層面執行，且「不」\*\* 適用於在頁面內轉譯的子元件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-200">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="f6e95-201">若要授權在頁面內顯示特定組件，請改為使用 `AuthorizeView`。</span><span class="sxs-lookup"><span data-stu-id="f6e95-201">To authorize the display of specific parts within a page, use `AuthorizeView` instead.</span></span>

<span data-ttu-id="f6e95-202">`[Authorize]` 屬性也支援角色型或原則型授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-202">The `[Authorize]` attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="f6e95-203">針對角色型授權，請使用 `Roles` 參數：</span><span class="sxs-lookup"><span data-stu-id="f6e95-203">For role-based authorization, use the `Roles` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="f6e95-204">針對原則型授權，請使用 `Policy` 參數：</span><span class="sxs-lookup"><span data-stu-id="f6e95-204">For policy-based authorization, use the `Policy` parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="f6e95-205">如果未指定 `Roles` 與 `Policy`，`[Authorize]` 便會使用預設原則，它預設會將：</span><span class="sxs-lookup"><span data-stu-id="f6e95-205">If neither `Roles` nor `Policy` is specified, `[Authorize]` uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="f6e95-206">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-206">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="f6e95-207">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="f6e95-207">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="f6e95-208">搭配 Router 元件自訂未經授權的內容</span><span class="sxs-lookup"><span data-stu-id="f6e95-208">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="f6e95-209">`Router`元件與 `AuthorizeRouteView` 元件結合，可讓應用程式在下列情況指定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="f6e95-209">The `Router` component, in conjunction with the `AuthorizeRouteView` component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="f6e95-210">找不到內容。</span><span class="sxs-lookup"><span data-stu-id="f6e95-210">Content isn't found.</span></span>
* <span data-ttu-id="f6e95-211">使用者無法滿足套用至元件的 `[Authorize]` 條件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-211">The user fails an `[Authorize]` condition applied to the component.</span></span> <span data-ttu-id="f6e95-212">屬性 `[Authorize]` 一節中會涵蓋[ `[Authorize]` ](#authorize-attribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="f6e95-212">The `[Authorize]` attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="f6e95-213">正在進行非同步驗證。</span><span class="sxs-lookup"><span data-stu-id="f6e95-213">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="f6e95-214">在預設的 Blazor 伺服器專案範本中， `App` 元件（*razor*）會示範如何設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="f6e95-214">In the default Blazor Server project template, the `App` component (*App.razor*) demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="f6e95-215">`<NotFound>`、 `<NotAuthorized>` 和標記的內容 `<Authorizing>` 可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="f6e95-215">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="f6e95-216">如果 `<NotAuthorized>` 未指定元素，會 `AuthorizeRouteView` 使用下列回溯訊息：</span><span class="sxs-lookup"><span data-stu-id="f6e95-216">If the `<NotAuthorized>` element isn't specified, the `AuthorizeRouteView` uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="f6e95-217">關於驗證狀態變更的通知</span><span class="sxs-lookup"><span data-stu-id="f6e95-217">Notification about authentication state changes</span></span>

<span data-ttu-id="f6e95-218">如果應用程式判斷基礎驗證狀態資料已變更（例如，使用者登出或其他使用者已變更其角色），[自訂 AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider)可以選擇性地叫用 `NotifyAuthenticationStateChanged` 基類上的方法 `AuthenticationStateProvider` 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-218">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a [custom AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider) can optionally invoke the method `NotifyAuthenticationStateChanged` on the `AuthenticationStateProvider` base class.</span></span> <span data-ttu-id="f6e95-219">這會通知驗證狀態資料的取用者 (例如 `AuthorizeView`) 使用新資料來重新轉譯。</span><span class="sxs-lookup"><span data-stu-id="f6e95-219">This notifies consumers of the authentication state data (for example, `AuthorizeView`) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="f6e95-220">程序性邏輯</span><span class="sxs-lookup"><span data-stu-id="f6e95-220">Procedural logic</span></span>

<span data-ttu-id="f6e95-221">如果作為程序性邏輯的一部分，應用程式必須檢查授權規則，請使用 `Task<AuthenticationState>` 類型的階層式參數來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal>。</span><span class="sxs-lookup"><span data-stu-id="f6e95-221">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<AuthenticationState>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="f6e95-222">`Task<AuthenticationState>` 可以與其他服務 (例如 `IAuthorizationService`) 結合來評估原則。</span><span class="sxs-lookup"><span data-stu-id="f6e95-222">`Task<AuthenticationState>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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
> <span data-ttu-id="f6e95-223">在 Blazor WebAssembly 應用程式元件中，新增 `Microsoft.AspNetCore.Authorization` 和 `Microsoft.AspNetCore.Components.Authorization` 命名空間：</span><span class="sxs-lookup"><span data-stu-id="f6e95-223">In a Blazor WebAssembly app component, add the `Microsoft.AspNetCore.Authorization` and `Microsoft.AspNetCore.Components.Authorization` namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```
>
> <span data-ttu-id="f6e95-224">這些命名空間可以藉由將它們新增至應用程式的 *_Imports razor*檔案來全域提供。</span><span class="sxs-lookup"><span data-stu-id="f6e95-224">These namespaces can be provided globally by adding them to the app's *_Imports.razor* file.</span></span>

## <a name="authorization-in-blazor-webassembly-apps"></a><span data-ttu-id="f6e95-225">WebAssembly apps 中的授權 Blazor</span><span class="sxs-lookup"><span data-stu-id="f6e95-225">Authorization in Blazor WebAssembly apps</span></span>

<span data-ttu-id="f6e95-226">在 Blazor WebAssembly apps 中，可以略過授權檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="f6e95-226">In Blazor WebAssembly apps, authorization checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="f6e95-227">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6e95-227">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="f6e95-228">**請一律在由您用戶端應用程式所存取之任何 API 端點內的伺服器上執行授權檢查。**</span><span class="sxs-lookup"><span data-stu-id="f6e95-228">**Always perform authorization checks on the server within any API endpoints accessed by your client-side app.**</span></span>

<span data-ttu-id="f6e95-229">如需詳細資訊，請參閱底下的文章 <xref:security/blazor/webassembly/index> 。</span><span class="sxs-lookup"><span data-stu-id="f6e95-229">For more information, see the articles under <xref:security/blazor/webassembly/index>.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="f6e95-230">針對錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="f6e95-230">Troubleshoot errors</span></span>

<span data-ttu-id="f6e95-231">常見錯誤：</span><span class="sxs-lookup"><span data-stu-id="f6e95-231">Common errors:</span></span>

* <span data-ttu-id="f6e95-232">**授權需要類型為 Task AuthenticationState> 的串聯參數 \< 。請考慮使用 CascadingAuthenticationState 來提供此。**</span><span class="sxs-lookup"><span data-stu-id="f6e95-232">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="f6e95-233">**`null`接收的值`authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="f6e95-233">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="f6e95-234">專案可能不是使用 Blazor 已啟用驗證的伺服器範本所建立。</span><span class="sxs-lookup"><span data-stu-id="f6e95-234">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="f6e95-235">將 `<CascadingAuthenticationState>` UI 樹狀結構的某些部分（例如，在 `App` 元件（*razor*）中）包裝起來，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f6e95-235">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in the `App` component (*App.razor*) as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="f6e95-236">`CascadingAuthenticationState` 會提供 `Task<AuthenticationState>` 階層式參數，它接著會接收自底層 `AuthenticationStateProvider` DI 服務。</span><span class="sxs-lookup"><span data-stu-id="f6e95-236">The `CascadingAuthenticationState` supplies the `Task<AuthenticationState>` cascading parameter, which in turn it receives from the underlying `AuthenticationStateProvider` DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f6e95-237">其他資源</span><span class="sxs-lookup"><span data-stu-id="f6e95-237">Additional resources</span></span>

* <xref:security/index>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="f6e95-238">[卓越 Blazor ：驗證](https://github.com/AdrienTorris/awesome-blazor#authentication)社區範例連結</span><span class="sxs-lookup"><span data-stu-id="f6e95-238">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
