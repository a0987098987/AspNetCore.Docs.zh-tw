---
<span data-ttu-id="08a79-101">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-101">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-102">'Blazor'</span></span>
- <span data-ttu-id="08a79-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-103">'Identity'</span></span>
- <span data-ttu-id="08a79-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-105">'Razor'</span></span>
- <span data-ttu-id="08a79-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-106">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="08a79-107">ASP.NET Core Blazor 路由</span><span class="sxs-lookup"><span data-stu-id="08a79-107">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="08a79-108">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="08a79-108">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="08a79-109">瞭解如何路由傳送要求，以及如何使用 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件在應用程式中建立導覽連結 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="08a79-109">Learn how to route requests and how to use the <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="08a79-110">ASP.NET Core 端點路由整合</span><span class="sxs-lookup"><span data-stu-id="08a79-110">ASP.NET Core endpoint routing integration</span></span>

Blazor<span data-ttu-id="08a79-111">伺服器已整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="08a79-111"> Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="08a79-112">ASP.NET Core 應用程式已設定為接受中的互動式元件連入 <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> 連線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="08a79-112">An ASP.NET Core app is configured to accept incoming connections for interactive components with <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="08a79-113">最常見的設定是將所有要求路由傳送至 Razor 頁面，做為伺服器應用程式伺服器端部分的主機 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="08a79-113">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="08a79-114">依照慣例，[*主機*] 頁面通常會命名為 *_Host. cshtml*。</span><span class="sxs-lookup"><span data-stu-id="08a79-114">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="08a79-115">主機檔案中指定的路由稱為「回溯*路由*」，因為它在路由比對中以低優先順序運作。</span><span class="sxs-lookup"><span data-stu-id="08a79-115">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="08a79-116">當其他路由不相符時，就會考慮回退路由。</span><span class="sxs-lookup"><span data-stu-id="08a79-116">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="08a79-117">這可讓應用程式使用其他控制器和頁面，而不會干擾 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="08a79-117">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="08a79-118">路由範本</span><span class="sxs-lookup"><span data-stu-id="08a79-118">Route templates</span></span>

<span data-ttu-id="08a79-119">此 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件可讓您使用指定的路由來路由傳送至每個元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-119">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component enables routing to each component with a specified route.</span></span> <span data-ttu-id="08a79-120">此 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件會出現在*應用程式 razor*檔案中：</span><span class="sxs-lookup"><span data-stu-id="08a79-120">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component appears in the *App.razor* file:</span></span>

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="08a79-121">編譯含有指示詞的*razor*檔案時 `@page` ，會提供 <xref:Microsoft.AspNetCore.Components.RouteAttribute> 指定路由範本的所產生類別。</span><span class="sxs-lookup"><span data-stu-id="08a79-121">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Components.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="08a79-122">在執行時間， <xref:Microsoft.AspNetCore.Components.RouteView> 元件：</span><span class="sxs-lookup"><span data-stu-id="08a79-122">At runtime, the <xref:Microsoft.AspNetCore.Components.RouteView> component:</span></span>

* <span data-ttu-id="08a79-123">從接收， <xref:Microsoft.AspNetCore.Components.RouteData> <xref:Microsoft.AspNetCore.Components.Routing.Router> 連同任何想要的參數。</span><span class="sxs-lookup"><span data-stu-id="08a79-123">Receives the <xref:Microsoft.AspNetCore.Components.RouteData> from the <xref:Microsoft.AspNetCore.Components.Routing.Router> along with any desired parameters.</span></span>
* <span data-ttu-id="08a79-124">使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-124">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="08a79-125">您可以選擇性地指定 <xref:Microsoft.AspNetCore.Components.RouteView.DefaultLayout> 具有版面配置類別的參數，以用於未指定版面配置的元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-125">You can optionally specify a <xref:Microsoft.AspNetCore.Components.RouteView.DefaultLayout> parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="08a79-126">預設 Blazor 範本會指定 `MainLayout` 元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-126">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="08a79-127">*MainLayout*是在範本專案的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="08a79-127">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="08a79-128">如需版面配置的詳細資訊，請參閱 <xref:blazor/layouts> 。</span><span class="sxs-lookup"><span data-stu-id="08a79-128">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="08a79-129">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-129">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="08a79-130">下列元件會回應和的要求 `/BlazorRoute` `/DifferentBlazorRoute` ：</span><span class="sxs-lookup"><span data-stu-id="08a79-130">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> <span data-ttu-id="08a79-131">若要讓 Url 正確解析，應用程式必須 `<base>` 在其*wwwroot/index.html*檔（WebAssembly）中包含標記， Blazor 或在屬性（）中指定應用程式基底路徑的*Pages/_Host. cshtml*檔案（ Blazor 伺服器） `href` `<base href="/">` 。</span><span class="sxs-lookup"><span data-stu-id="08a79-131">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="08a79-132">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/index#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="08a79-132">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="08a79-133">在找不到內容時提供自訂內容</span><span class="sxs-lookup"><span data-stu-id="08a79-133">Provide custom content when content isn't found</span></span>

<span data-ttu-id="08a79-134"><xref:Microsoft.AspNetCore.Components.Routing.Router>如果找不到所要求路由的內容，此元件可讓應用程式指定自訂內容。</span><span class="sxs-lookup"><span data-stu-id="08a79-134">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="08a79-135">在*應用程式的 razor*檔案中，于元件的範本參數中設定自訂內容 <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> <xref:Microsoft.AspNetCore.Components.Routing.Router> ：</span><span class="sxs-lookup"><span data-stu-id="08a79-135">In the *App.razor* file, set custom content in the <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> template parameter of the <xref:Microsoft.AspNetCore.Components.Routing.Router> component:</span></span>

```razor
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="08a79-136">標記的內容 `<NotFound>` 可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-136">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="08a79-137">若要將預設版面配置套用至 <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> 內容，請參閱 <xref:blazor/layouts> 。</span><span class="sxs-lookup"><span data-stu-id="08a79-137">To apply a default layout to <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="08a79-138">從多個元件路由至元件</span><span class="sxs-lookup"><span data-stu-id="08a79-138">Route to components from multiple assemblies</span></span>

<span data-ttu-id="08a79-139">使用 <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> 參數來指定 <xref:Microsoft.AspNetCore.Components.Routing.Router> 搜尋可路由的元件時，要考慮的元件的其他元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-139">Use the <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> parameter to specify additional assemblies for the <xref:Microsoft.AspNetCore.Components.Routing.Router> component to consider when searching for routable components.</span></span> <span data-ttu-id="08a79-140">除了指定的元件以外，還會考慮指定的元件 `AppAssembly` 。</span><span class="sxs-lookup"><span data-stu-id="08a79-140">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="08a79-141">在下列範例中， `Component1` 是在參考的類別庫中定義的可路由元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-141">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="08a79-142">下列 <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> 範例會產生的路由支援 `Component1` ：</span><span class="sxs-lookup"><span data-stu-id="08a79-142">The following <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> example results in routing support for `Component1`:</span></span>

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="08a79-143">路由參數</span><span class="sxs-lookup"><span data-stu-id="08a79-143">Route parameters</span></span>

<span data-ttu-id="08a79-144">路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：</span><span class="sxs-lookup"><span data-stu-id="08a79-144">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="08a79-145">不支援選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="08a79-145">Optional parameters aren't supported.</span></span> <span data-ttu-id="08a79-146">`@page`在上述範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="08a79-146">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="08a79-147">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-147">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="08a79-148">第二個指示詞 `@page` 會採用 `{text}` route 參數，並將值指派給 `Text` 屬性。</span><span class="sxs-lookup"><span data-stu-id="08a79-148">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="08a79-149">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="08a79-149">Route constraints</span></span>

<span data-ttu-id="08a79-150">路由條件約束會將路由區段上的類型比對強制套用至元件。</span><span class="sxs-lookup"><span data-stu-id="08a79-150">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="08a79-151">在下列範例中，對元件的路由 `Users` 只會符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="08a79-151">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="08a79-152">`Id`要求 URL 上有路由區段。</span><span class="sxs-lookup"><span data-stu-id="08a79-152">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="08a79-153">`Id`區段是一個整數（ `int` ）。</span><span class="sxs-lookup"><span data-stu-id="08a79-153">The `Id` segment is an integer (`int`).</span></span>

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="08a79-154">下表所示的路由條件約束可供使用。</span><span class="sxs-lookup"><span data-stu-id="08a79-154">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="08a79-155">若為符合不因文化特性而異的路由條件約束，請參閱表格下方的警告以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="08a79-155">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="08a79-156">條件約束</span><span class="sxs-lookup"><span data-stu-id="08a79-156">Constraint</span></span> | <span data-ttu-id="08a79-157">範例</span><span class="sxs-lookup"><span data-stu-id="08a79-157">Example</span></span>           | <span data-ttu-id="08a79-158">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="08a79-158">Example Matches</span></span>                                                                  | <span data-ttu-id="08a79-159">非變異值</span><span class="sxs-lookup"><span data-stu-id="08a79-159">Invariant</span></span><br><span data-ttu-id="08a79-160">culture</span><span class="sxs-lookup"><span data-stu-id="08a79-160">culture</span></span><br><span data-ttu-id="08a79-161">比對</span><span class="sxs-lookup"><span data-stu-id="08a79-161">matching</span></span> |
| ---
<span data-ttu-id="08a79-162">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-162">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-163">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-163">'Blazor'</span></span>
- <span data-ttu-id="08a79-164">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-164">'Identity'</span></span>
- <span data-ttu-id="08a79-165">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-165">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-166">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-166">'Razor'</span></span>
- <span data-ttu-id="08a79-167">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-167">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-168">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-168">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-169">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-169">'Blazor'</span></span>
- <span data-ttu-id="08a79-170">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-170">'Identity'</span></span>
- <span data-ttu-id="08a79-171">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-171">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-172">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-172">'Razor'</span></span>
- <span data-ttu-id="08a79-173">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-173">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-174">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-174">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-175">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-175">'Blazor'</span></span>
- <span data-ttu-id="08a79-176">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-176">'Identity'</span></span>
- <span data-ttu-id="08a79-177">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-177">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-178">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-178">'Razor'</span></span>
- <span data-ttu-id="08a79-179">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-179">'SignalR' uid:</span></span> 

<span data-ttu-id="08a79-180">----- |---標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-180">----- | --- title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-181">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-181">'Blazor'</span></span>
- <span data-ttu-id="08a79-182">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-182">'Identity'</span></span>
- <span data-ttu-id="08a79-183">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-183">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-184">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-184">'Razor'</span></span>
- <span data-ttu-id="08a79-185">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-185">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-186">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-186">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-187">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-187">'Blazor'</span></span>
- <span data-ttu-id="08a79-188">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-188">'Identity'</span></span>
- <span data-ttu-id="08a79-189">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-189">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-190">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-190">'Razor'</span></span>
- <span data-ttu-id="08a79-191">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-191">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-192">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-192">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-193">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-193">'Blazor'</span></span>
- <span data-ttu-id="08a79-194">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-194">'Identity'</span></span>
- <span data-ttu-id="08a79-195">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-195">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-196">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-196">'Razor'</span></span>
- <span data-ttu-id="08a79-197">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-197">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-198">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-198">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-199">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-199">'Blazor'</span></span>
- <span data-ttu-id="08a79-200">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-200">'Identity'</span></span>
- <span data-ttu-id="08a79-201">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-201">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-202">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-202">'Razor'</span></span>
- <span data-ttu-id="08a79-203">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-203">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-204">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-204">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-205">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-205">'Blazor'</span></span>
- <span data-ttu-id="08a79-206">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-206">'Identity'</span></span>
- <span data-ttu-id="08a79-207">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-207">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-208">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-208">'Razor'</span></span>
- <span data-ttu-id="08a79-209">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-209">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-210">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-210">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-211">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-211">'Blazor'</span></span>
- <span data-ttu-id="08a79-212">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-212">'Identity'</span></span>
- <span data-ttu-id="08a79-213">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-213">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-214">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-214">'Razor'</span></span>
- <span data-ttu-id="08a79-215">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-215">'SignalR' uid:</span></span> 

<span data-ttu-id="08a79-216">--------- |---標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-216">--------- | --- title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-217">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-217">'Blazor'</span></span>
- <span data-ttu-id="08a79-218">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-218">'Identity'</span></span>
- <span data-ttu-id="08a79-219">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-219">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-220">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-220">'Razor'</span></span>
- <span data-ttu-id="08a79-221">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-221">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-222">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-222">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-223">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-223">'Blazor'</span></span>
- <span data-ttu-id="08a79-224">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-224">'Identity'</span></span>
- <span data-ttu-id="08a79-225">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-225">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-226">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-226">'Razor'</span></span>
- <span data-ttu-id="08a79-227">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-227">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-228">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-228">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-229">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-229">'Blazor'</span></span>
- <span data-ttu-id="08a79-230">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-230">'Identity'</span></span>
- <span data-ttu-id="08a79-231">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-231">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-232">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-232">'Razor'</span></span>
- <span data-ttu-id="08a79-233">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-233">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-234">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-234">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-235">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-235">'Blazor'</span></span>
- <span data-ttu-id="08a79-236">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-236">'Identity'</span></span>
- <span data-ttu-id="08a79-237">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-237">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-238">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-238">'Razor'</span></span>
- <span data-ttu-id="08a79-239">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-239">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-240">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-240">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-241">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-241">'Blazor'</span></span>
- <span data-ttu-id="08a79-242">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-242">'Identity'</span></span>
- <span data-ttu-id="08a79-243">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-243">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-244">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-244">'Razor'</span></span>
- <span data-ttu-id="08a79-245">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-245">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-246">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-246">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-247">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-247">'Blazor'</span></span>
- <span data-ttu-id="08a79-248">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-248">'Identity'</span></span>
- <span data-ttu-id="08a79-249">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-249">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-250">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-250">'Razor'</span></span>
- <span data-ttu-id="08a79-251">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-251">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-252">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-252">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-253">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-253">'Blazor'</span></span>
- <span data-ttu-id="08a79-254">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-254">'Identity'</span></span>
- <span data-ttu-id="08a79-255">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-255">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-256">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-256">'Razor'</span></span>
- <span data-ttu-id="08a79-257">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-257">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-258">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-258">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-259">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-259">'Blazor'</span></span>
- <span data-ttu-id="08a79-260">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-260">'Identity'</span></span>
- <span data-ttu-id="08a79-261">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-261">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-262">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-262">'Razor'</span></span>
- <span data-ttu-id="08a79-263">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-263">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-264">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-264">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-265">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-265">'Blazor'</span></span>
- <span data-ttu-id="08a79-266">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-266">'Identity'</span></span>
- <span data-ttu-id="08a79-267">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-267">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-268">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-268">'Razor'</span></span>
- <span data-ttu-id="08a79-269">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-269">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-270">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-270">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-271">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-271">'Blazor'</span></span>
- <span data-ttu-id="08a79-272">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-272">'Identity'</span></span>
- <span data-ttu-id="08a79-273">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-273">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-274">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-274">'Razor'</span></span>
- <span data-ttu-id="08a79-275">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-275">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-276">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-276">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-277">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-277">'Blazor'</span></span>
- <span data-ttu-id="08a79-278">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-278">'Identity'</span></span>
- <span data-ttu-id="08a79-279">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-279">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-280">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-280">'Razor'</span></span>
- <span data-ttu-id="08a79-281">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-281">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-282">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-282">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-283">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-283">'Blazor'</span></span>
- <span data-ttu-id="08a79-284">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-284">'Identity'</span></span>
- <span data-ttu-id="08a79-285">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-285">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-286">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-286">'Razor'</span></span>
- <span data-ttu-id="08a79-287">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-287">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-288">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-288">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-289">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-289">'Blazor'</span></span>
- <span data-ttu-id="08a79-290">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-290">'Identity'</span></span>
- <span data-ttu-id="08a79-291">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-291">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-292">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-292">'Razor'</span></span>
- <span data-ttu-id="08a79-293">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-293">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-294">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-294">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-295">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-295">'Blazor'</span></span>
- <span data-ttu-id="08a79-296">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-296">'Identity'</span></span>
- <span data-ttu-id="08a79-297">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-297">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-298">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-298">'Razor'</span></span>
- <span data-ttu-id="08a79-299">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-299">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-300">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-300">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-301">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-301">'Blazor'</span></span>
- <span data-ttu-id="08a79-302">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-302">'Identity'</span></span>
- <span data-ttu-id="08a79-303">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-303">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-304">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-304">'Razor'</span></span>
- <span data-ttu-id="08a79-305">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-305">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-306">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-306">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-307">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-307">'Blazor'</span></span>
- <span data-ttu-id="08a79-308">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-308">'Identity'</span></span>
- <span data-ttu-id="08a79-309">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-309">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-310">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-310">'Razor'</span></span>
- <span data-ttu-id="08a79-311">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-311">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-312">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-312">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-313">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-313">'Blazor'</span></span>
- <span data-ttu-id="08a79-314">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-314">'Identity'</span></span>
- <span data-ttu-id="08a79-315">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-315">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-316">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-316">'Razor'</span></span>
- <span data-ttu-id="08a79-317">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-317">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-318">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-318">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-319">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-319">'Blazor'</span></span>
- <span data-ttu-id="08a79-320">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-320">'Identity'</span></span>
- <span data-ttu-id="08a79-321">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-321">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-322">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-322">'Razor'</span></span>
- <span data-ttu-id="08a79-323">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-323">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-324">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-324">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-325">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-325">'Blazor'</span></span>
- <span data-ttu-id="08a79-326">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-326">'Identity'</span></span>
- <span data-ttu-id="08a79-327">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-327">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-328">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-328">'Razor'</span></span>
- <span data-ttu-id="08a79-329">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-329">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-330">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-330">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-331">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-331">'Blazor'</span></span>
- <span data-ttu-id="08a79-332">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-332">'Identity'</span></span>
- <span data-ttu-id="08a79-333">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-333">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-334">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-334">'Razor'</span></span>
- <span data-ttu-id="08a79-335">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-335">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-336">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-336">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-337">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-337">'Blazor'</span></span>
- <span data-ttu-id="08a79-338">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-338">'Identity'</span></span>
- <span data-ttu-id="08a79-339">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-339">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-340">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-340">'Razor'</span></span>
- <span data-ttu-id="08a79-341">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-341">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-342">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-342">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-343">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-343">'Blazor'</span></span>
- <span data-ttu-id="08a79-344">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-344">'Identity'</span></span>
- <span data-ttu-id="08a79-345">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-345">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-346">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-346">'Razor'</span></span>
- <span data-ttu-id="08a79-347">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-347">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-348">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-348">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-349">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-349">'Blazor'</span></span>
- <span data-ttu-id="08a79-350">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-350">'Identity'</span></span>
- <span data-ttu-id="08a79-351">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-351">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-352">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-352">'Razor'</span></span>
- <span data-ttu-id="08a79-353">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-353">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-354">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-354">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-355">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-355">'Blazor'</span></span>
- <span data-ttu-id="08a79-356">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-356">'Identity'</span></span>
- <span data-ttu-id="08a79-357">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-357">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-358">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-358">'Razor'</span></span>
- <span data-ttu-id="08a79-359">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-359">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-360">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-360">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-361">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-361">'Blazor'</span></span>
- <span data-ttu-id="08a79-362">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-362">'Identity'</span></span>
- <span data-ttu-id="08a79-363">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-363">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-364">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-364">'Razor'</span></span>
- <span data-ttu-id="08a79-365">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-365">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-366">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-366">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-367">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-367">'Blazor'</span></span>
- <span data-ttu-id="08a79-368">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-368">'Identity'</span></span>
- <span data-ttu-id="08a79-369">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-369">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-370">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-370">'Razor'</span></span>
- <span data-ttu-id="08a79-371">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-371">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-372">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-372">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-373">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-373">'Blazor'</span></span>
- <span data-ttu-id="08a79-374">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-374">'Identity'</span></span>
- <span data-ttu-id="08a79-375">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-375">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-376">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-376">'Razor'</span></span>
- <span data-ttu-id="08a79-377">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-377">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-378">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-378">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-379">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-379">'Blazor'</span></span>
- <span data-ttu-id="08a79-380">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-380">'Identity'</span></span>
- <span data-ttu-id="08a79-381">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-381">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-382">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-382">'Razor'</span></span>
- <span data-ttu-id="08a79-383">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-383">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-384">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-384">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-385">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-385">'Blazor'</span></span>
- <span data-ttu-id="08a79-386">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-386">'Identity'</span></span>
- <span data-ttu-id="08a79-387">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-387">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-388">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-388">'Razor'</span></span>
- <span data-ttu-id="08a79-389">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-389">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-390">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-390">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-391">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-391">'Blazor'</span></span>
- <span data-ttu-id="08a79-392">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-392">'Identity'</span></span>
- <span data-ttu-id="08a79-393">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-393">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-394">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-394">'Razor'</span></span>
- <span data-ttu-id="08a79-395">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-395">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-396">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-396">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-397">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-397">'Blazor'</span></span>
- <span data-ttu-id="08a79-398">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-398">'Identity'</span></span>
- <span data-ttu-id="08a79-399">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-399">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-400">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-400">'Razor'</span></span>
- <span data-ttu-id="08a79-401">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-401">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-402">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-402">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-403">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-403">'Blazor'</span></span>
- <span data-ttu-id="08a79-404">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-404">'Identity'</span></span>
- <span data-ttu-id="08a79-405">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-405">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-406">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-406">'Razor'</span></span>
- <span data-ttu-id="08a79-407">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-407">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-408">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-408">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-409">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-409">'Blazor'</span></span>
- <span data-ttu-id="08a79-410">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-410">'Identity'</span></span>
- <span data-ttu-id="08a79-411">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-411">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-412">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-412">'Razor'</span></span>
- <span data-ttu-id="08a79-413">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-413">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-414">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-414">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-415">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-415">'Blazor'</span></span>
- <span data-ttu-id="08a79-416">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-416">'Identity'</span></span>
- <span data-ttu-id="08a79-417">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-417">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-418">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-418">'Razor'</span></span>
- <span data-ttu-id="08a79-419">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-419">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-420">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-420">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-421">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-421">'Blazor'</span></span>
- <span data-ttu-id="08a79-422">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-422">'Identity'</span></span>
- <span data-ttu-id="08a79-423">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-423">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-424">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-424">'Razor'</span></span>
- <span data-ttu-id="08a79-425">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-425">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-426">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-426">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-427">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-427">'Blazor'</span></span>
- <span data-ttu-id="08a79-428">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-428">'Identity'</span></span>
- <span data-ttu-id="08a79-429">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-429">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-430">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-430">'Razor'</span></span>
- <span data-ttu-id="08a79-431">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-431">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-432">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-432">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-433">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-433">'Blazor'</span></span>
- <span data-ttu-id="08a79-434">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-434">'Identity'</span></span>
- <span data-ttu-id="08a79-435">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-435">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-436">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-436">'Razor'</span></span>
- <span data-ttu-id="08a79-437">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-437">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-438">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-438">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-439">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-439">'Blazor'</span></span>
- <span data-ttu-id="08a79-440">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-440">'Identity'</span></span>
- <span data-ttu-id="08a79-441">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-441">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-442">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-442">'Razor'</span></span>
- <span data-ttu-id="08a79-443">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-443">'SignalR' uid:</span></span> 

<span data-ttu-id="08a79-444">---------------------------------------- |：---標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者：毫秒。自訂： ms。日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-444">---------------------------------------- | :--- title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-445">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-445">'Blazor'</span></span>
- <span data-ttu-id="08a79-446">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-446">'Identity'</span></span>
- <span data-ttu-id="08a79-447">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-447">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-448">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-448">'Razor'</span></span>
- <span data-ttu-id="08a79-449">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-449">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-450">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-450">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-451">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-451">'Blazor'</span></span>
- <span data-ttu-id="08a79-452">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-452">'Identity'</span></span>
- <span data-ttu-id="08a79-453">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-453">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-454">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-454">'Razor'</span></span>
- <span data-ttu-id="08a79-455">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-455">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-456">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-456">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-457">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-457">'Blazor'</span></span>
- <span data-ttu-id="08a79-458">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-458">'Identity'</span></span>
- <span data-ttu-id="08a79-459">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-459">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-460">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-460">'Razor'</span></span>
- <span data-ttu-id="08a79-461">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-461">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-462">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-462">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-463">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-463">'Blazor'</span></span>
- <span data-ttu-id="08a79-464">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-464">'Identity'</span></span>
- <span data-ttu-id="08a79-465">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-465">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-466">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-466">'Razor'</span></span>
- <span data-ttu-id="08a79-467">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-467">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-468">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-468">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-469">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-469">'Blazor'</span></span>
- <span data-ttu-id="08a79-470">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-470">'Identity'</span></span>
- <span data-ttu-id="08a79-471">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-471">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-472">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-472">'Razor'</span></span>
- <span data-ttu-id="08a79-473">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-473">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-474">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-474">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-475">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-475">'Blazor'</span></span>
- <span data-ttu-id="08a79-476">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-476">'Identity'</span></span>
- <span data-ttu-id="08a79-477">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-477">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-478">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-478">'Razor'</span></span>
- <span data-ttu-id="08a79-479">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-479">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-480">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-480">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-481">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-481">'Blazor'</span></span>
- <span data-ttu-id="08a79-482">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-482">'Identity'</span></span>
- <span data-ttu-id="08a79-483">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-483">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-484">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-484">'Razor'</span></span>
- <span data-ttu-id="08a79-485">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-485">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-486">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-486">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-487">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-487">'Blazor'</span></span>
- <span data-ttu-id="08a79-488">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-488">'Identity'</span></span>
- <span data-ttu-id="08a79-489">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-489">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-490">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-490">'Razor'</span></span>
- <span data-ttu-id="08a79-491">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-491">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-492">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-492">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-493">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-493">'Blazor'</span></span>
- <span data-ttu-id="08a79-494">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-494">'Identity'</span></span>
- <span data-ttu-id="08a79-495">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-495">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-496">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-496">'Razor'</span></span>
- <span data-ttu-id="08a79-497">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-497">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-498">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-498">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-499">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-499">'Blazor'</span></span>
- <span data-ttu-id="08a79-500">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-500">'Identity'</span></span>
- <span data-ttu-id="08a79-501">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-501">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-502">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-502">'Razor'</span></span>
- <span data-ttu-id="08a79-503">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-503">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-504">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-504">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-505">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-505">'Blazor'</span></span>
- <span data-ttu-id="08a79-506">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-506">'Identity'</span></span>
- <span data-ttu-id="08a79-507">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-507">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-508">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-508">'Razor'</span></span>
- <span data-ttu-id="08a79-509">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-509">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-510">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-510">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-511">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-511">'Blazor'</span></span>
- <span data-ttu-id="08a79-512">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-512">'Identity'</span></span>
- <span data-ttu-id="08a79-513">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-513">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-514">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-514">'Razor'</span></span>
- <span data-ttu-id="08a79-515">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-515">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-516">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-516">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-517">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-517">'Blazor'</span></span>
- <span data-ttu-id="08a79-518">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-518">'Identity'</span></span>
- <span data-ttu-id="08a79-519">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-519">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-520">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-520">'Razor'</span></span>
- <span data-ttu-id="08a79-521">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-521">'SignalR' uid:</span></span> 

<span data-ttu-id="08a79-522">---------------: | |`bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  |否 | |`datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                |是 | |`decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             |是 | |`double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           |是 | |`float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           |是 | |`guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` |否 | |`int`      | `{id:int}`        | `123456789`, `-123456789`                                                        |是 | |`long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        |是 |</span><span class="sxs-lookup"><span data-stu-id="08a79-522">---------------: | | `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | No                               | | `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Yes                              | | `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Yes                              | | `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Yes                              | | `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Yes                              | | `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | No                               | | `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Yes                              | | `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Yes                              |</span></span>

> [!WARNING]
> <span data-ttu-id="08a79-523">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 <xref:System.DateTime>) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="08a79-523">Route constraints that verify the URL and are converted to a CLR type (such as `int` or <xref:System.DateTime>) always use the invariant culture.</span></span> <span data-ttu-id="08a79-524">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="08a79-524">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="08a79-525">包含點的 Url 路由</span><span class="sxs-lookup"><span data-stu-id="08a79-525">Routing with URLs that contain dots</span></span>

<span data-ttu-id="08a79-526">在 Blazor [伺服器應用程式] 中， *_Host*中的預設路由是 `/` （ `@page "/"` ）。</span><span class="sxs-lookup"><span data-stu-id="08a79-526">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="08a79-527">包含點（）的要求 URL `.` 不符合預設路由，因為 URL 會顯示要求檔案。</span><span class="sxs-lookup"><span data-stu-id="08a79-527">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="08a79-528">如果 Blazor 靜態檔案不存在，應用程式會傳回*404-找不*到的回應。</span><span class="sxs-lookup"><span data-stu-id="08a79-528">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="08a79-529">若要使用包含點的路由，請使用下列路由範本來設定 *_Host. cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="08a79-529">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="08a79-530">此 `"/{**path}"` 範本包含：</span><span class="sxs-lookup"><span data-stu-id="08a79-530">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="08a79-531">雙星號*catch-all*語法（ `**` ）可跨多個資料夾界限捕捉路徑，而不需要編碼正斜線（ `/` ）。</span><span class="sxs-lookup"><span data-stu-id="08a79-531">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="08a79-532">`path`路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="08a79-532">`path` route parameter name.</span></span>

> [!NOTE]
> <span data-ttu-id="08a79-533">*Catch-all*參數語法（ `*` / `**` ）在**not** Razor 元件（*razor*）中不受支援。</span><span class="sxs-lookup"><span data-stu-id="08a79-533">*Catch-all* parameter syntax (`*`/`**`) is **not** supported in Razor components (*.razor*).</span></span>

<span data-ttu-id="08a79-534">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="08a79-534">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="08a79-535">NavLink 元件</span><span class="sxs-lookup"><span data-stu-id="08a79-535">NavLink component</span></span>

<span data-ttu-id="08a79-536"><xref:Microsoft.AspNetCore.Components.Routing.NavLink>建立導覽連結時，請使用元件來取代 HTML 超連結元素（ `<a>` ）。</span><span class="sxs-lookup"><span data-stu-id="08a79-536">Use a <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="08a79-537"><xref:Microsoft.AspNetCore.Components.Routing.NavLink>元件的行為類似 `<a>` 元素，但它會根據 `active` 其是否 `href` 符合目前的 URL 來切換 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="08a79-537">A <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="08a79-538">`active`類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。</span><span class="sxs-lookup"><span data-stu-id="08a79-538">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="08a79-539">下列 `NavMenu` 元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列，示範如何使用 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件：</span><span class="sxs-lookup"><span data-stu-id="08a79-539">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use <xref:Microsoft.AspNetCore.Components.Routing.NavLink> components:</span></span>

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="08a79-540">有兩個 <xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch> 選項可供您指派給 `Match` 元素的屬性 `<NavLink>` ：</span><span class="sxs-lookup"><span data-stu-id="08a79-540">There are two <xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch> options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="08a79-541"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.All?displayProperty=nameWithType>： <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 當符合整個目前的 URL 時，就會作用中。</span><span class="sxs-lookup"><span data-stu-id="08a79-541"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.All?displayProperty=nameWithType>: The <xref:Microsoft.AspNetCore.Components.Routing.NavLink> is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="08a79-542"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.Prefix?displayProperty=nameWithType>（*預設值*）： <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 當它符合目前 URL 的任何前置詞時，就會作用中。</span><span class="sxs-lookup"><span data-stu-id="08a79-542"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.Prefix?displayProperty=nameWithType> (*default*): The <xref:Microsoft.AspNetCore.Components.Routing.NavLink> is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="08a79-543">在上述範例中，Home 會 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> `href=""` 符合 home URL，而且只會 `active` 在應用程式的預設基底路徑 URL （例如，）接收 CSS 類別 `https://localhost:5001/` 。</span><span class="sxs-lookup"><span data-stu-id="08a79-543">In the preceding example, the Home <xref:Microsoft.AspNetCore.Components.Routing.NavLink> `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="08a79-544">第二個會在 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> `active` 使用者造訪具有前置詞的任何 URL 時收到類別 `MyComponent` （例如， `https://localhost:5001/MyComponent` 和 `https://localhost:5001/MyComponent/AnotherSegment` ）。</span><span class="sxs-lookup"><span data-stu-id="08a79-544">The second <xref:Microsoft.AspNetCore.Components.Routing.NavLink> receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="08a79-545">其他 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件屬性會傳遞至呈現的錨點標記。</span><span class="sxs-lookup"><span data-stu-id="08a79-545">Additional <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="08a79-546">在下列範例中， <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件包含 `target` 屬性：</span><span class="sxs-lookup"><span data-stu-id="08a79-546">In the following example, the <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component includes the `target` attribute:</span></span>

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="08a79-547">會轉譯下列 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="08a79-547">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="08a79-548">URI 和流覽狀態協助程式</span><span class="sxs-lookup"><span data-stu-id="08a79-548">URI and navigation state helpers</span></span>

<span data-ttu-id="08a79-549">用於 <xref:Microsoft.AspNetCore.Components.NavigationManager> 在 c # 程式碼中處理 uri 和導覽。</span><span class="sxs-lookup"><span data-stu-id="08a79-549">Use <xref:Microsoft.AspNetCore.Components.NavigationManager> to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="08a79-550"><xref:Microsoft.AspNetCore.Components.NavigationManager>提供下表所示的事件和方法。</span><span class="sxs-lookup"><span data-stu-id="08a79-550"><xref:Microsoft.AspNetCore.Components.NavigationManager> provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="08a79-551">成員</span><span class="sxs-lookup"><span data-stu-id="08a79-551">Member</span></span> | <span data-ttu-id="08a79-552">描述</span><span class="sxs-lookup"><span data-stu-id="08a79-552">Description</span></span> |
| ---
<span data-ttu-id="08a79-553">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-553">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-554">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-554">'Blazor'</span></span>
- <span data-ttu-id="08a79-555">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-555">'Identity'</span></span>
- <span data-ttu-id="08a79-556">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-556">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-557">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-557">'Razor'</span></span>
- <span data-ttu-id="08a79-558">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-558">'SignalR' uid:</span></span> 

<span data-ttu-id="08a79-559">--- |---標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-559">--- | --- title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-560">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-560">'Blazor'</span></span>
- <span data-ttu-id="08a79-561">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-561">'Identity'</span></span>
- <span data-ttu-id="08a79-562">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-562">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-563">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-563">'Razor'</span></span>
- <span data-ttu-id="08a79-564">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-564">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-565">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-565">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-566">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-566">'Blazor'</span></span>
- <span data-ttu-id="08a79-567">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-567">'Identity'</span></span>
- <span data-ttu-id="08a79-568">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-568">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-569">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-569">'Razor'</span></span>
- <span data-ttu-id="08a79-570">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-570">'SignalR' uid:</span></span> 

-
<span data-ttu-id="08a79-571">標題： ' ASP.NET Core Blazor 路由 ' 作者：描述： monikerRange： ms。作者： ms. 自訂： ms. 日期：無-loc：</span><span class="sxs-lookup"><span data-stu-id="08a79-571">title: 'ASP.NET Core Blazor routing' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="08a79-572">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="08a79-572">'Blazor'</span></span>
- <span data-ttu-id="08a79-573">'Identity'</span><span class="sxs-lookup"><span data-stu-id="08a79-573">'Identity'</span></span>
- <span data-ttu-id="08a79-574">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="08a79-574">'Let's Encrypt'</span></span>
- <span data-ttu-id="08a79-575">'Razor'</span><span class="sxs-lookup"><span data-stu-id="08a79-575">'Razor'</span></span>
- <span data-ttu-id="08a79-576">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="08a79-576">'SignalR' uid:</span></span> 

<span data-ttu-id="08a79-577">------ | |<xref:Microsoft.AspNetCore.Components.NavigationManager.Uri> |取得目前的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="08a79-577">------ | | <xref:Microsoft.AspNetCore.Components.NavigationManager.Uri> | Gets the current absolute URI.</span></span> <span data-ttu-id="08a79-578">| |<xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> |取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="08a79-578">| | <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> | Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="08a79-579">通常會 <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> 對應至 `href` `<base>` *wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host. cshtml* （Server）中檔元素的屬性 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="08a79-579">Typically, <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="08a79-580">| |<xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A> |導覽至指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="08a79-580">| | <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A> | Navigates to the specified URI.</span></span> <span data-ttu-id="08a79-581">如果 `forceLoad` 為 `true` ：</span><span class="sxs-lookup"><span data-stu-id="08a79-581">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="08a79-582">已略過用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="08a79-582">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="08a79-583">瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</span><span class="sxs-lookup"><span data-stu-id="08a79-583">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> <span data-ttu-id="08a79-584">| |<xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged> |導覽位置變更時引發的事件。</span><span class="sxs-lookup"><span data-stu-id="08a79-584">| | <xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged> | An event that fires when the navigation location has changed.</span></span> <span data-ttu-id="08a79-585">| |<xref:Microsoft.AspNetCore.Components.NavigationManager.ToAbsoluteUri%2A> |將相對 URI 轉換為絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="08a79-585">| | <xref:Microsoft.AspNetCore.Components.NavigationManager.ToAbsoluteUri%2A> | Converts a relative URI into an absolute URI.</span></span> <span data-ttu-id="08a79-586">| |<span style="word-break:normal;word-wrap:normal"><xref:Microsoft.AspNetCore.Components.NavigationManager.ToBaseRelativePath%2A></span> |假設基底 URI （例如先前傳回的 URI <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> ），會將絕對 uri 轉換成相對於基底 uri 前置詞的 uri。</span><span class="sxs-lookup"><span data-stu-id="08a79-586">| | <span style="word-break:normal;word-wrap:normal"><xref:Microsoft.AspNetCore.Components.NavigationManager.ToBaseRelativePath%2A></span> | Given a base URI (for example, a URI previously returned by <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri>), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="08a79-587">下列元件 `Counter` 會在選取按鈕時，流覽至應用程式的元件：</span><span class="sxs-lookup"><span data-stu-id="08a79-587">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```razor
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```

<span data-ttu-id="08a79-588">下列元件會藉由設定來處理位置已變更事件 <xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="08a79-588">The following component handles a location changed event by setting <xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged?displayProperty=nameWithType>.</span></span> <span data-ttu-id="08a79-589">`HandleLocationChanged`當架構呼叫時，方法會解除掛鉤 `Dispose` 。</span><span class="sxs-lookup"><span data-stu-id="08a79-589">The `HandleLocationChanged` method is unhooked when `Dispose` is called by the framework.</span></span> <span data-ttu-id="08a79-590">Unhooking 方法允許元件的垃圾收集。</span><span class="sxs-lookup"><span data-stu-id="08a79-590">Unhooking the method permits garbage collection of the component.</span></span>

```razor
@implements IDisposable
@inject NavigationManager NavigationManager

...

protected override void OnInitialized()
{
    NavigationManager.LocationChanged += HandleLocationChanged;
}

private void HandleLocationChanged(object sender, LocationChangedEventArgs e)
{
    ...
}

public void Dispose()
{
    NavigationManager.LocationChanged -= HandleLocationChanged;
}
```

<span data-ttu-id="08a79-591"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs>提供事件的下列相關資訊：</span><span class="sxs-lookup"><span data-stu-id="08a79-591"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs> provides the following information about the event:</span></span>

* <span data-ttu-id="08a79-592"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>：新位置的 URL。</span><span class="sxs-lookup"><span data-stu-id="08a79-592"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>: The URL of the new location.</span></span>
* <span data-ttu-id="08a79-593"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>：如果是，則會 `true` Blazor 攔截瀏覽器的導覽。</span><span class="sxs-lookup"><span data-stu-id="08a79-593"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>: If `true`, Blazor intercepted the navigation from the browser.</span></span> <span data-ttu-id="08a79-594">如果 `false` <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A?displayProperty=nameWithType> 為，則會造成導覽。</span><span class="sxs-lookup"><span data-stu-id="08a79-594">If `false`, <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A?displayProperty=nameWithType> caused the navigation to occur.</span></span>

<span data-ttu-id="08a79-595">如需有關元件處置的詳細資訊，請參閱 <xref:blazor/lifecycle#component-disposal-with-idisposable> 。</span><span class="sxs-lookup"><span data-stu-id="08a79-595">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>
