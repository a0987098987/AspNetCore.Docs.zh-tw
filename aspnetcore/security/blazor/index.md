---
<span data-ttu-id="72c14-101">標題： ' ASP.NET Core Blazor 驗證和授權 ' 作者： guardrex 描述：「瞭解 Blazor 驗證和授權案例」。</span><span class="sxs-lookup"><span data-stu-id="72c14-101">title: 'ASP.NET Core Blazor authentication and authorization' author: guardrex description: 'Learn about Blazor authentication and authorization scenarios.'</span></span>
<span data-ttu-id="72c14-102">monikerRange： ' >= aspnetcore-3.1 ' ms-chap： riande ms. custom： mvc ms. date： 05/19/2020 no-loc：</span><span class="sxs-lookup"><span data-stu-id="72c14-102">monikerRange: '>= aspnetcore-3.1' ms.author: riande ms.custom: mvc ms.date: 05/19/2020 no-loc:</span></span>
- <span data-ttu-id="72c14-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="72c14-103">'Blazor'</span></span>
- <span data-ttu-id="72c14-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="72c14-104">'Identity'</span></span>
- <span data-ttu-id="72c14-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="72c14-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="72c14-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="72c14-106">'Razor'</span></span>
- <span data-ttu-id="72c14-107">' SignalR ' uid： security/blazor/index</span><span class="sxs-lookup"><span data-stu-id="72c14-107">'SignalR' uid: security/blazor/index</span></span>

---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a><span data-ttu-id="72c14-108">ASP.NET Core Blazor 驗證和授權</span><span class="sxs-lookup"><span data-stu-id="72c14-108">ASP.NET Core Blazor authentication and authorization</span></span>

<span data-ttu-id="72c14-109">作者： [Steve Sanderson](https://github.com/SteveSandersonMS)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="72c14-109">By [Steve Sanderson](https://github.com/SteveSandersonMS) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="72c14-110">ASP.NET Core 支援在應用程式中設定和管理安全性 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="72c14-110">ASP.NET Core supports the configuration and management of security in Blazor apps.</span></span>

<span data-ttu-id="72c14-111">Blazor伺服器和 Blazor WebAssembly 應用程式之間的安全性案例不同。</span><span class="sxs-lookup"><span data-stu-id="72c14-111">Security scenarios differ between Blazor Server and Blazor WebAssembly apps.</span></span> <span data-ttu-id="72c14-112">因為 Blazor 伺服器應用程式是在伺服器上執行，所以授權檢查能夠判斷：</span><span class="sxs-lookup"><span data-stu-id="72c14-112">Because Blazor Server apps run on the server, authorization checks are able to determine:</span></span>

* <span data-ttu-id="72c14-113">要提供給使用者的 UI 選項 (例如，使用者可以使用哪些功能表項目)。</span><span class="sxs-lookup"><span data-stu-id="72c14-113">The UI options presented to a user (for example, which menu entries are available to a user).</span></span>
* <span data-ttu-id="72c14-114">適用於應用程式和元件之特定區域的存取規則。</span><span class="sxs-lookup"><span data-stu-id="72c14-114">Access rules for areas of the app and components.</span></span>

Blazor<span data-ttu-id="72c14-115">WebAssembly 應用程式會在用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="72c14-115"> WebAssembly apps run on the client.</span></span> <span data-ttu-id="72c14-116">授權「僅」\*\* 會被用來決定要顯示的 UI 選項。</span><span class="sxs-lookup"><span data-stu-id="72c14-116">Authorization is *only* used to determine which UI options to show.</span></span> <span data-ttu-id="72c14-117">由於用戶端檢查可由使用者修改或略過，因此 Blazor WebAssembly 應用程式無法強制執行授權存取規則。</span><span class="sxs-lookup"><span data-stu-id="72c14-117">Since client-side checks can be modified or bypassed by a user, a Blazor WebAssembly app can't enforce authorization access rules.</span></span>

<span data-ttu-id="72c14-118">[ Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization)不適用於可路由的 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="72c14-118">[Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization) don't apply to routable Razor components.</span></span> <span data-ttu-id="72c14-119">如果無法路由傳送的 Razor 元件[內嵌在頁面中](xref:blazor/integrate-components#render-components-from-a-page-or-view)，頁面的授權慣例會間接影響此 Razor 元件以及頁面內容的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="72c14-119">If a non-routable Razor component is [embedded in a page](xref:blazor/integrate-components#render-components-from-a-page-or-view), the page's authorization conventions indirectly affect the Razor component along with the rest of the page's content.</span></span>

> [!NOTE]
> <span data-ttu-id="72c14-120"><xref:Microsoft.AspNetCore.Identity.SignInManager%601><xref:Microsoft.AspNetCore.Identity.UserManager%601>元件中不支援和 Razor 。</span><span class="sxs-lookup"><span data-stu-id="72c14-120"><xref:Microsoft.AspNetCore.Identity.SignInManager%601> and <xref:Microsoft.AspNetCore.Identity.UserManager%601> aren't supported in Razor components.</span></span>

## <a name="authentication"></a><span data-ttu-id="72c14-121">驗證</span><span class="sxs-lookup"><span data-stu-id="72c14-121">Authentication</span></span>

Blazor<span data-ttu-id="72c14-122">會使用現有的 ASP.NET Core 驗證機制來建立使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="72c14-122"> uses the existing ASP.NET Core authentication mechanisms to establish the user's identity.</span></span> <span data-ttu-id="72c14-123">確切的機制取決於 Blazor 應用程式的主控方式、 Blazor WebAssembly 或 Blazor 伺服器。</span><span class="sxs-lookup"><span data-stu-id="72c14-123">The exact mechanism depends on how the Blazor app is hosted, Blazor WebAssembly or Blazor Server.</span></span>

### <a name="blazor-webassembly-authentication"></a>Blazor<span data-ttu-id="72c14-124">WebAssembly authentication</span><span class="sxs-lookup"><span data-stu-id="72c14-124"> WebAssembly authentication</span></span>

<span data-ttu-id="72c14-125">在 Blazor WebAssembly apps 中，可以略過驗證檢查，因為使用者可以修改所有的用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="72c14-125">In Blazor WebAssembly apps, authentication checks can be bypassed because all client-side code can be modified by users.</span></span> <span data-ttu-id="72c14-126">這同樣也適用於所有的用戶端應用程式技術，包括 JavaScript SPA 架構或任何作業系統的原生應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c14-126">The same is true for all client-side app technologies, including JavaScript SPA frameworks or native apps for any operating system.</span></span>

<span data-ttu-id="72c14-127">新增下列內容：</span><span class="sxs-lookup"><span data-stu-id="72c14-127">Add the following:</span></span>

* <span data-ttu-id="72c14-128">AspNetCore 的套件參考。應用程式的專案檔的[授權](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/)。</span><span class="sxs-lookup"><span data-stu-id="72c14-128">A package reference for [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) to the app's project file.</span></span>
* <span data-ttu-id="72c14-129">`Microsoft.AspNetCore.Components.Authorization`應用程式的 *_Imports razor*檔案的命名空間。</span><span class="sxs-lookup"><span data-stu-id="72c14-129">The `Microsoft.AspNetCore.Components.Authorization` namespace to the app's *_Imports.razor* file.</span></span>

<span data-ttu-id="72c14-130">若要處理驗證，內建或自訂服務的執行 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 將在下列各節中討論。</span><span class="sxs-lookup"><span data-stu-id="72c14-130">To handle authentication, implementation of a built-in or custom <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> service is covered in the following sections.</span></span>

<span data-ttu-id="72c14-131">如需建立應用程式和設定的詳細資訊，請參閱 <xref:security/blazor/webassembly/index> 。</span><span class="sxs-lookup"><span data-stu-id="72c14-131">For more information on creating apps and configuration, see <xref:security/blazor/webassembly/index>.</span></span>

### <a name="blazor-server-authentication"></a>Blazor<span data-ttu-id="72c14-132">伺服器驗證</span><span class="sxs-lookup"><span data-stu-id="72c14-132"> Server authentication</span></span>

Blazor<span data-ttu-id="72c14-133">伺服器應用程式會透過使用建立的即時連線來運作 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="72c14-133"> Server apps operate over a real-time connection that's created using SignalR.</span></span> <span data-ttu-id="72c14-134">建立連接時，會處理[以為 SignalR 基礎之應用程式中的驗證](xref:signalr/authn-and-authz)。</span><span class="sxs-lookup"><span data-stu-id="72c14-134">[Authentication in SignalR-based apps](xref:signalr/authn-and-authz) is handled when the connection is established.</span></span> <span data-ttu-id="72c14-135">驗證可以是以 Cookie 或其他持有人權杖為基礎。</span><span class="sxs-lookup"><span data-stu-id="72c14-135">Authentication can be based on a cookie or some other bearer token.</span></span>

<span data-ttu-id="72c14-136">如需建立應用程式和設定的詳細資訊，請參閱 <xref:security/blazor/server/index> 。</span><span class="sxs-lookup"><span data-stu-id="72c14-136">For more information on creating apps and configuration, see <xref:security/blazor/server/index>.</span></span>

## <a name="authenticationstateprovider-service"></a><span data-ttu-id="72c14-137">AuthenticationStateProvider 服務</span><span class="sxs-lookup"><span data-stu-id="72c14-137">AuthenticationStateProvider service</span></span>

<span data-ttu-id="72c14-138">內建服務會 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 從 ASP.NET Core 的取得驗證狀態資料 `HttpContext.User` 。</span><span class="sxs-lookup"><span data-stu-id="72c14-138">The built-in <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> service obtains authentication state data from ASP.NET Core's `HttpContext.User`.</span></span> <span data-ttu-id="72c14-139">這是驗證狀態與現有 ASP.NET Core 驗證機制整合的方式。</span><span class="sxs-lookup"><span data-stu-id="72c14-139">This is how authentication state integrates with existing ASP.NET Core authentication mechanisms.</span></span>

<span data-ttu-id="72c14-140"><xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 是 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> 元件與 <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> 元件用來取得驗證狀態的基礎服務。</span><span class="sxs-lookup"><span data-stu-id="72c14-140"><xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> is the underlying service used by the <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> component and <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> component to get the authentication state.</span></span>

<span data-ttu-id="72c14-141">您通常不會直接使用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider>。</span><span class="sxs-lookup"><span data-stu-id="72c14-141">You don't typically use <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> directly.</span></span> <span data-ttu-id="72c14-142">請使用此文章稍後所述的 [AuthorizeView 元件](#authorizeview-component)或 [Task\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) 方法。</span><span class="sxs-lookup"><span data-stu-id="72c14-142">Use the [AuthorizeView component](#authorizeview-component) or [Task\<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter) approaches described later in this article.</span></span> <span data-ttu-id="72c14-143">使用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 的主要缺點，在於系統不會在基礎驗證狀態資料變更時自動通知該元件。</span><span class="sxs-lookup"><span data-stu-id="72c14-143">The main drawback to using <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> directly is that the component isn't notified automatically if the underlying authentication state data changes.</span></span>

<span data-ttu-id="72c14-144"><xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 服務可以提供目前使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="72c14-144">The <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> service can provide the current user's <xref:System.Security.Claims.ClaimsPrincipal> data, as shown in the following example:</span></span>

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
            <li>@claim.Type: @claim.Value</li>
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

<span data-ttu-id="72c14-145">如果 `user.Identity.IsAuthenticated` 為 `true`，且由於使用者為 <xref:System.Security.Claims.ClaimsPrincipal>，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="72c14-145">If `user.Identity.IsAuthenticated` is `true` and because the user is a <xref:System.Security.Claims.ClaimsPrincipal>, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="72c14-146">如需有關相依性插入 (DI) 與服務的詳細資訊，請參閱 <xref:blazor/dependency-injection> 與 <xref:fundamentals/dependency-injection>。</span><span class="sxs-lookup"><span data-stu-id="72c14-146">For more information on dependency injection (DI) and services, see <xref:blazor/dependency-injection> and <xref:fundamentals/dependency-injection>.</span></span>

## <a name="implement-a-custom-authenticationstateprovider"></a><span data-ttu-id="72c14-147">實作自訂 AuthenticationStateProvider</span><span class="sxs-lookup"><span data-stu-id="72c14-147">Implement a custom AuthenticationStateProvider</span></span>

<span data-ttu-id="72c14-148">如果應用程式需要自訂提供者，請執行 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 並覆寫 `GetAuthenticationStateAsync` ：</span><span class="sxs-lookup"><span data-stu-id="72c14-148">If the app requires a custom provider, implement <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> and override `GetAuthenticationStateAsync`:</span></span>

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

<span data-ttu-id="72c14-149">在 Blazor WebAssembly 應用程式中， `CustomAuthStateProvider` 服務會在 `Main` *Program.cs*中註冊：</span><span class="sxs-lookup"><span data-stu-id="72c14-149">In a Blazor WebAssembly app, the `CustomAuthStateProvider` service is registered in `Main` of *Program.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

builder.Services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="72c14-150">在 Blazor 伺服器應用程式中， `CustomAuthStateProvider` 服務會在中註冊 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="72c14-150">In a Blazor Server app, the `CustomAuthStateProvider` service is registered in `Startup.ConfigureServices`:</span></span>

```csharp
using Microsoft.AspNetCore.Components.Authorization;

...

services.AddScoped<AuthenticationStateProvider, CustomAuthStateProvider>();
```

<span data-ttu-id="72c14-151">使用 `CustomAuthStateProvider` 上述範例中的，所有使用者都會使用 username 進行驗證 `mrfibuli` 。</span><span class="sxs-lookup"><span data-stu-id="72c14-151">Using the `CustomAuthStateProvider` in the preceding example, all users are authenticated with the username `mrfibuli`.</span></span>

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a><span data-ttu-id="72c14-152">將驗證狀態公開為階層式參數</span><span class="sxs-lookup"><span data-stu-id="72c14-152">Expose the authentication state as a cascading parameter</span></span>

<span data-ttu-id="72c14-153">如果程式邏輯需要驗證狀態資料，例如執行使用者所觸發的動作時，請藉由定義類型的串聯參數來取得驗證狀態資料 `Task<` <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> `>` ：</span><span class="sxs-lookup"><span data-stu-id="72c14-153">If authentication state data is required for procedural logic, such as when performing an action triggered by the user, obtain the authentication state data by defining a cascading parameter of type `Task<`<xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>`>`:</span></span>

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

<span data-ttu-id="72c14-154">如果 `user.Identity.IsAuthenticated` 為 `true`，系統便可以列舉宣告，並評估角色中的成員資格。</span><span class="sxs-lookup"><span data-stu-id="72c14-154">If `user.Identity.IsAuthenticated` is `true`, claims can be enumerated and membership in roles evaluated.</span></span>

<span data-ttu-id="72c14-155">`Task<` <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> `>` 使用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> 元件中的和元件 `App` （*app.config*）來設定串聯參數：</span><span class="sxs-lookup"><span data-stu-id="72c14-155">Set up the `Task<`<xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>`>` cascading parameter using the <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> and <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> components in the `App` component (*App.razor*):</span></span>

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

<span data-ttu-id="72c14-156">在 Blazor WebAssembly 應用程式中，將選項和授權的服務新增至 `Program.Main` ：</span><span class="sxs-lookup"><span data-stu-id="72c14-156">In a Blazor WebAssembly App, add services for options and authorization to `Program.Main`:</span></span>

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

<span data-ttu-id="72c14-157">在 Blazor 伺服器應用程式中，選項和授權的服務已存在，因此不需要採取進一步的動作。</span><span class="sxs-lookup"><span data-stu-id="72c14-157">In a Blazor Server app, services for options and authorization are already present, so no further action is required.</span></span>

## <a name="authorization"></a><span data-ttu-id="72c14-158">授權</span><span class="sxs-lookup"><span data-stu-id="72c14-158">Authorization</span></span>

<span data-ttu-id="72c14-159">在使用者被驗證之後，系統便會套用「授權」\*\* 規則以控制使用者可以執行的動作。</span><span class="sxs-lookup"><span data-stu-id="72c14-159">After a user is authenticated, *authorization* rules are applied to control what the user can do.</span></span>

<span data-ttu-id="72c14-160">存取權通常會依據下列情況來授與或拒絕：</span><span class="sxs-lookup"><span data-stu-id="72c14-160">Access is typically granted or denied based on whether:</span></span>

* <span data-ttu-id="72c14-161">使用者已通過驗證 (已登入)。</span><span class="sxs-lookup"><span data-stu-id="72c14-161">A user is authenticated (signed in).</span></span>
* <span data-ttu-id="72c14-162">使用者屬於某個「角色」\*\*。</span><span class="sxs-lookup"><span data-stu-id="72c14-162">A user is in a *role*.</span></span>
* <span data-ttu-id="72c14-163">使用者具有「宣告」\*\*。</span><span class="sxs-lookup"><span data-stu-id="72c14-163">A user has a *claim*.</span></span>
* <span data-ttu-id="72c14-164">已滿足某個「原則」\*\*。</span><span class="sxs-lookup"><span data-stu-id="72c14-164">A *policy* is satisfied.</span></span>

<span data-ttu-id="72c14-165">這些概念都與 ASP.NET Core MVC 或 Pages 應用程式中的相同 Razor 。</span><span class="sxs-lookup"><span data-stu-id="72c14-165">Each of these concepts is the same as in an ASP.NET Core MVC or Razor Pages app.</span></span> <span data-ttu-id="72c14-166">如需 ASP.NET Core 安全性的詳細資訊，請參閱[ASP.NET Core 安全性和 Identity ](xref:security/index)底下的文章。</span><span class="sxs-lookup"><span data-stu-id="72c14-166">For more information on ASP.NET Core security, see the articles under [ASP.NET Core Security and Identity](xref:security/index).</span></span>

## <a name="authorizeview-component"></a><span data-ttu-id="72c14-167">AuthorizeView 元件</span><span class="sxs-lookup"><span data-stu-id="72c14-167">AuthorizeView component</span></span>

<span data-ttu-id="72c14-168"><xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> 元件會根據使用者是否獲得授權以查看某個 UI 來選擇性地顯示該 UI。</span><span class="sxs-lookup"><span data-stu-id="72c14-168">The <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> component selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="72c14-169">此方法可讓您只需要向使用者「顯示」\*\* 資料，而不需要在程序性邏輯中使用該使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="72c14-169">This approach is useful when you only need to *display* data for the user and don't need to use the user's identity in procedural logic.</span></span>

<span data-ttu-id="72c14-170">該元件會公開 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> 類型的 `context` 變數，您可以使用它來存取已登入使用者的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="72c14-170">The component exposes a `context` variable of type <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>, which you can use to access information about the signed-in user:</span></span>

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

<span data-ttu-id="72c14-171">您也可以在使用者未經授權的情況下，提供不同的顯示內容：</span><span class="sxs-lookup"><span data-stu-id="72c14-171">You can also supply different content for display if the user isn't authenticated:</span></span>

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

<span data-ttu-id="72c14-172"><xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView>元件可以在 `NavMenu` 元件（*Shared/navmenu.cshtml*）中使用，以顯示 `<li>...</li>` [NavLink 元件](xref:blazor/routing#navlink-component)（）的清單專案（） <xref:Microsoft.AspNetCore.Components.Routing.NavLink> ，但請注意，此方法只會從轉譯的輸出中移除清單專案。</span><span class="sxs-lookup"><span data-stu-id="72c14-172">The <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> component can be used in the `NavMenu` component (*Shared/NavMenu.razor*) to display a list item (`<li>...</li>`) for a [NavLink component](xref:blazor/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), but note that this approach only removes the list item from the rendered output.</span></span> <span data-ttu-id="72c14-173">它不會防止使用者流覽至元件。</span><span class="sxs-lookup"><span data-stu-id="72c14-173">It doesn't prevent the user from navigating to the component.</span></span>

<span data-ttu-id="72c14-174">`<Authorized>`和標記的內容 `<NotAuthorized>` 可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="72c14-174">The content of `<Authorized>` and `<NotAuthorized>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="72c14-175">授權情況 (例如控制 UI 選項或存取的角色或原則) 已涵蓋於[授權](#authorization)一節。</span><span class="sxs-lookup"><span data-stu-id="72c14-175">Authorization conditions, such as roles or policies that control UI options or access, are covered in the [Authorization](#authorization) section.</span></span>

<span data-ttu-id="72c14-176">如果未指定授權條件，<xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> 會使用預設的原則，並將：</span><span class="sxs-lookup"><span data-stu-id="72c14-176">If authorization conditions aren't specified, <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> uses a default policy and treats:</span></span>

* <span data-ttu-id="72c14-177">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-177">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="72c14-178">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-178">Unauthenticated (signed-out) users as unauthorized.</span></span>

### <a name="role-based-and-policy-based-authorization"></a><span data-ttu-id="72c14-179">角色型和原則型授權</span><span class="sxs-lookup"><span data-stu-id="72c14-179">Role-based and policy-based authorization</span></span>

<span data-ttu-id="72c14-180"><xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> 元件支援「角色型」\*\* 或「原則型」\*\* 授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-180">The <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> component supports *role-based* or *policy-based* authorization.</span></span>

<span data-ttu-id="72c14-181">針對角色型授權，請使用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Roles> 參數：</span><span class="sxs-lookup"><span data-stu-id="72c14-181">For role-based authorization, use the <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Roles> parameter:</span></span>

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

<span data-ttu-id="72c14-182">如需詳細資訊，請參閱<xref:security/authorization/roles>。</span><span class="sxs-lookup"><span data-stu-id="72c14-182">For more information, see <xref:security/authorization/roles>.</span></span>

<span data-ttu-id="72c14-183">針對原則型授權，請使用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Policy> 參數：</span><span class="sxs-lookup"><span data-stu-id="72c14-183">For policy-based authorization, use the <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Policy> parameter:</span></span>

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

<span data-ttu-id="72c14-184">宣告型授權是特殊案例的原則型授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-184">Claims-based authorization is a special case of policy-based authorization.</span></span> <span data-ttu-id="72c14-185">例如，您可以定義要求使用者具備特定宣告的原則。</span><span class="sxs-lookup"><span data-stu-id="72c14-185">For example, you can define a policy that requires users to have a certain claim.</span></span> <span data-ttu-id="72c14-186">如需詳細資訊，請參閱<xref:security/authorization/policies>。</span><span class="sxs-lookup"><span data-stu-id="72c14-186">For more information, see <xref:security/authorization/policies>.</span></span>

<span data-ttu-id="72c14-187">這些 Api 可以在 Blazor 伺服器或 Blazor WebAssembly 應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="72c14-187">These APIs can be used in either Blazor Server or Blazor WebAssembly apps.</span></span>

<span data-ttu-id="72c14-188">如果未指定 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Roles> 和 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Policy>，<xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> 便會使用預設原則。</span><span class="sxs-lookup"><span data-stu-id="72c14-188">If neither <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Roles> nor <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView.Policy> is specified, <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> uses the default policy.</span></span>

### <a name="content-displayed-during-asynchronous-authentication"></a><span data-ttu-id="72c14-189">在非同步驗證期間所顯示的內容</span><span class="sxs-lookup"><span data-stu-id="72c14-189">Content displayed during asynchronous authentication</span></span>

Blazor<span data-ttu-id="72c14-190">允許以*非同步方式*判斷驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="72c14-190"> allows for authentication state to be determined *asynchronously*.</span></span> <span data-ttu-id="72c14-191">這種方法的主要案例是在 Blazor WebAssembly 應用程式中，對外部端點提出驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="72c14-191">The primary scenario for this approach is in Blazor WebAssembly apps that make a request to an external endpoint for authentication.</span></span>

<span data-ttu-id="72c14-192">在驗證期間，<xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> 預設不會顯示任何內容。</span><span class="sxs-lookup"><span data-stu-id="72c14-192">While authentication is in progress, <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> displays no content by default.</span></span> <span data-ttu-id="72c14-193">若要在驗證發生時顯示內容，請使用 `<Authorizing>` 元素：</span><span class="sxs-lookup"><span data-stu-id="72c14-193">To display content while authentication occurs, use the `<Authorizing>` element:</span></span>

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

<span data-ttu-id="72c14-194">這種方法通常不適用於 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="72c14-194">This approach isn't normally applicable to Blazor Server apps.</span></span> Blazor<span data-ttu-id="72c14-195">伺服器應用程式會在狀態建立後立即知道驗證狀態。</span><span class="sxs-lookup"><span data-stu-id="72c14-195"> Server apps know the authentication state as soon as the state is established.</span></span> <span data-ttu-id="72c14-196"><xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeViewCore.Authorizing>內容可以在 Blazor 伺服器應用程式的元件中提供 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> ，但永遠不會顯示內容。</span><span class="sxs-lookup"><span data-stu-id="72c14-196"><xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeViewCore.Authorizing> content can be provided in a Blazor Server app's <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> component, but the content is never displayed.</span></span>

## <a name="authorize-attribute"></a><span data-ttu-id="72c14-197">[Authorize] 屬性</span><span class="sxs-lookup"><span data-stu-id="72c14-197">[Authorize] attribute</span></span>

<span data-ttu-id="72c14-198">[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性可以在元件中使用 Razor ：</span><span class="sxs-lookup"><span data-stu-id="72c14-198">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute can be used in Razor components:</span></span>

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!IMPORTANT]
> <span data-ttu-id="72c14-199">僅 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 在透過 `@page` 路由器到達的元件上使用 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="72c14-199">Only use [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) on `@page` components reached via the Blazor Router.</span></span> <span data-ttu-id="72c14-200">授權僅會以路由的層面執行，且「不」\*\* 適用於在頁面內轉譯的子元件。</span><span class="sxs-lookup"><span data-stu-id="72c14-200">Authorization is only performed as an aspect of routing and *not* for child components rendered within a page.</span></span> <span data-ttu-id="72c14-201">若要授權在頁面內顯示特定組件，請改為使用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView>。</span><span class="sxs-lookup"><span data-stu-id="72c14-201">To authorize the display of specific parts within a page, use <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView> instead.</span></span>

<span data-ttu-id="72c14-202">[`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)屬性也支援以角色為基礎或以原則為基礎的授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-202">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute also supports role-based or policy-based authorization.</span></span> <span data-ttu-id="72c14-203">針對角色型授權，請使用 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Roles> 參數：</span><span class="sxs-lookup"><span data-stu-id="72c14-203">For role-based authorization, use the <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Roles> parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

<span data-ttu-id="72c14-204">針對原則型授權，請使用 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Policy> 參數：</span><span class="sxs-lookup"><span data-stu-id="72c14-204">For policy-based authorization, use the <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Policy> parameter:</span></span>

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

<span data-ttu-id="72c14-205">如果 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Roles> <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Policy> 未指定或，則會 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 使用預設原則，其預設會視為：</span><span class="sxs-lookup"><span data-stu-id="72c14-205">If neither <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Roles> nor <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute.Policy> is specified, [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) uses the default policy, which by default is to treat:</span></span>

* <span data-ttu-id="72c14-206">已驗證 (已登入) 的使用者視為已授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-206">Authenticated (signed-in) users as authorized.</span></span>
* <span data-ttu-id="72c14-207">未驗證 (已登出) 的使用者視為未經授權。</span><span class="sxs-lookup"><span data-stu-id="72c14-207">Unauthenticated (signed-out) users as unauthorized.</span></span>

## <a name="customize-unauthorized-content-with-the-router-component"></a><span data-ttu-id="72c14-208">搭配 Router 元件自訂未經授權的內容</span><span class="sxs-lookup"><span data-stu-id="72c14-208">Customize unauthorized content with the Router component</span></span>

<span data-ttu-id="72c14-209"><xref:Microsoft.AspNetCore.Components.Routing.Router>元件與 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> 元件結合，可讓應用程式在下列情況指定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="72c14-209">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component, in conjunction with the <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> component, allows the app to specify custom content if:</span></span>

* <span data-ttu-id="72c14-210">找不到內容。</span><span class="sxs-lookup"><span data-stu-id="72c14-210">Content isn't found.</span></span>
* <span data-ttu-id="72c14-211">使用者無法通過套用 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 至元件的條件。</span><span class="sxs-lookup"><span data-stu-id="72c14-211">The user fails an [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) condition applied to the component.</span></span> <span data-ttu-id="72c14-212">屬性 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 一節中會涵蓋[ `[Authorize]` ](#authorize-attribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="72c14-212">The [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is covered in the [`[Authorize]` attribute](#authorize-attribute) section.</span></span>
* <span data-ttu-id="72c14-213">正在進行非同步驗證。</span><span class="sxs-lookup"><span data-stu-id="72c14-213">Asynchronous authentication is in progress.</span></span>

<span data-ttu-id="72c14-214">在預設的 Blazor 伺服器專案範本中， `App` 元件（*razor*）會示範如何設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="72c14-214">In the default Blazor Server project template, the `App` component (*App.razor*) demonstrates how to set custom content:</span></span>

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

<span data-ttu-id="72c14-215">`<NotFound>`、 `<NotAuthorized>` 和標記的內容 `<Authorizing>` 可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="72c14-215">The content of `<NotFound>`, `<NotAuthorized>`, and `<Authorizing>` tags can include arbitrary items, such as other interactive components.</span></span>

<span data-ttu-id="72c14-216">如果 `<NotAuthorized>` 未指定元素，會 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> 使用下列回溯訊息：</span><span class="sxs-lookup"><span data-stu-id="72c14-216">If the `<NotAuthorized>` element isn't specified, the <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeRouteView> uses the following fallback message:</span></span>

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a><span data-ttu-id="72c14-217">關於驗證狀態變更的通知</span><span class="sxs-lookup"><span data-stu-id="72c14-217">Notification about authentication state changes</span></span>

<span data-ttu-id="72c14-218">如果應用程式判斷基礎驗證狀態資料已變更（例如，使用者登出或其他使用者已變更其角色），[自訂 AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider)可以選擇性地叫用 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider.NotifyAuthenticationStateChanged%2A> 基類上的方法 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> 。</span><span class="sxs-lookup"><span data-stu-id="72c14-218">If the app determines that the underlying authentication state data has changed (for example, because the user signed out or another user has changed their roles), a [custom AuthenticationStateProvider](#implement-a-custom-authenticationstateprovider) can optionally invoke the method <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider.NotifyAuthenticationStateChanged%2A> on the <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> base class.</span></span> <span data-ttu-id="72c14-219">這會通知驗證狀態資料的取用者 (例如 <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView>) 使用新資料來重新轉譯。</span><span class="sxs-lookup"><span data-stu-id="72c14-219">This notifies consumers of the authentication state data (for example, <xref:Microsoft.AspNetCore.Components.Authorization.AuthorizeView>) to rerender using the new data.</span></span>

## <a name="procedural-logic"></a><span data-ttu-id="72c14-220">程序性邏輯</span><span class="sxs-lookup"><span data-stu-id="72c14-220">Procedural logic</span></span>

<span data-ttu-id="72c14-221">如果應用程式需要檢查授權規則做為程式邏輯的一部分，請使用類型的串聯參數 `Task<` <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> `>` 來取得使用者的 <xref:System.Security.Claims.ClaimsPrincipal> 。</span><span class="sxs-lookup"><span data-stu-id="72c14-221">If the app is required to check authorization rules as part of procedural logic, use a cascaded parameter of type `Task<`<xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>`>` to obtain the user's <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="72c14-222">`Task<`<xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>`>`可以與其他服務（例如 `IAuthorizationService` ）結合，以評估原則。</span><span class="sxs-lookup"><span data-stu-id="72c14-222">`Task<`<xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>`>` can be combined with other services, such as `IAuthorizationService`, to evaluate policies.</span></span>

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
> <span data-ttu-id="72c14-223">在 Blazor WebAssembly 應用程式元件中，新增 <xref:Microsoft.AspNetCore.Authorization> 和 <xref:Microsoft.AspNetCore.Components.Authorization> 命名空間：</span><span class="sxs-lookup"><span data-stu-id="72c14-223">In a Blazor WebAssembly app component, add the <xref:Microsoft.AspNetCore.Authorization> and <xref:Microsoft.AspNetCore.Components.Authorization> namespaces:</span></span>
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```
>
> <span data-ttu-id="72c14-224">這些命名空間可以藉由將它們新增至應用程式的 *_Imports razor*檔案來全域提供。</span><span class="sxs-lookup"><span data-stu-id="72c14-224">These namespaces can be provided globally by adding them to the app's *_Imports.razor* file.</span></span>

## <a name="troubleshoot-errors"></a><span data-ttu-id="72c14-225">針對錯誤進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="72c14-225">Troubleshoot errors</span></span>

<span data-ttu-id="72c14-226">常見錯誤：</span><span class="sxs-lookup"><span data-stu-id="72c14-226">Common errors:</span></span>

* <span data-ttu-id="72c14-227">**授權需要類型為 Task 的串聯參數 \<AuthenticationState> 。請考慮使用 CascadingAuthenticationState 來提供此。**</span><span class="sxs-lookup"><span data-stu-id="72c14-227">**Authorization requires a cascading parameter of type Task\<AuthenticationState>. Consider using CascadingAuthenticationState to supply this.**</span></span>

* <span data-ttu-id="72c14-228">**`null`接收的值`authenticationStateTask`**</span><span class="sxs-lookup"><span data-stu-id="72c14-228">**`null` value is received for `authenticationStateTask`**</span></span>

<span data-ttu-id="72c14-229">專案可能不是使用 Blazor 已啟用驗證的伺服器範本所建立。</span><span class="sxs-lookup"><span data-stu-id="72c14-229">It's likely that the project wasn't created using a Blazor Server template with authentication enabled.</span></span> <span data-ttu-id="72c14-230">將 `<CascadingAuthenticationState>` UI 樹狀結構的某些部分（例如，在 `App` 元件（*razor*）中）包裝起來，如下所示：</span><span class="sxs-lookup"><span data-stu-id="72c14-230">Wrap a `<CascadingAuthenticationState>` around some part of the UI tree, for example in the `App` component (*App.razor*) as follows:</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

<span data-ttu-id="72c14-231"><xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> `Task<` <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState> `>` 會提供串聯式參數，然後再從基礎 <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> DI 服務接收。</span><span class="sxs-lookup"><span data-stu-id="72c14-231">The <xref:Microsoft.AspNetCore.Components.Authorization.CascadingAuthenticationState> supplies the `Task<`<xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationState>`>` cascading parameter, which in turn it receives from the underlying <xref:Microsoft.AspNetCore.Components.Authorization.AuthenticationStateProvider> DI service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72c14-232">其他資源</span><span class="sxs-lookup"><span data-stu-id="72c14-232">Additional resources</span></span>

* <xref:security/index>
* <xref:security/authentication/windowsauth>
* <span data-ttu-id="72c14-233">[卓越 Blazor ：驗證](https://github.com/AdrienTorris/awesome-blazor#authentication)社區範例連結</span><span class="sxs-lookup"><span data-stu-id="72c14-233">[Awesome Blazor: Authentication](https://github.com/AdrienTorris/awesome-blazor#authentication) community sample links</span></span>
