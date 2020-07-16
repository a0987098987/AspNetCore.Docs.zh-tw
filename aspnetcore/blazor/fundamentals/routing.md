---
title: ASP.NET Core Blazor 路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求，以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/14/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/routing
ms.openlocfilehash: 4f85c4a9803482f39446dda599f10829c9879f27
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2020
ms.locfileid: "86407758"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="87b6d-103">ASP.NET Core Blazor 路由</span><span class="sxs-lookup"><span data-stu-id="87b6d-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="87b6d-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87b6d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="87b6d-105">瞭解如何路由傳送要求，以及如何使用 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件在應用程式中建立導覽連結 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-105">Learn how to route requests and how to use the <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="87b6d-106">ASP.NET Core 端點路由整合</span><span class="sxs-lookup"><span data-stu-id="87b6d-106">ASP.NET Core endpoint routing integration</span></span>

Blazor Server<span data-ttu-id="87b6d-107">已整合至[ASP.NET Core 端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="87b6d-107"> is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="87b6d-108">ASP.NET Core 應用程式已設定為接受中的互動式元件連入 <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> 連線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="87b6d-109">最常見的設定是將所有要求路由傳送至 Razor 頁面，作為應用程式伺服器端部分的主機 Blazor Server 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="87b6d-110">依照慣例，*主機*頁面通常會命名為 `_Host.cshtml` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-110">By convention, the *host* page is usually named `_Host.cshtml`.</span></span> <span data-ttu-id="87b6d-111">主機檔案中指定的路由稱為「回溯*路由*」，因為它在路由比對中以低優先順序運作。</span><span class="sxs-lookup"><span data-stu-id="87b6d-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="87b6d-112">當其他路由不相符時，就會考慮回退路由。</span><span class="sxs-lookup"><span data-stu-id="87b6d-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="87b6d-113">這可讓應用程式使用其他控制器和頁面，而不會干擾 Blazor Server 應用程式。</span><span class="sxs-lookup"><span data-stu-id="87b6d-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

<span data-ttu-id="87b6d-114">如需針對 <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A> 非根 URL 伺服器裝載設定的詳細資訊，請參閱 <xref:blazor/host-and-deploy/index#app-base-path> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-114">For information on configuring <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage%2A> for non-root URL server hosting, see <xref:blazor/host-and-deploy/index#app-base-path>.</span></span>

## <a name="route-templates"></a><span data-ttu-id="87b6d-115">路由範本</span><span class="sxs-lookup"><span data-stu-id="87b6d-115">Route templates</span></span>

<span data-ttu-id="87b6d-116">此 <xref:Microsoft.AspNetCore.Components.Routing.Router> 元件可讓您使用指定的路由來路由傳送至每個元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-116">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component enables routing to each component with a specified route.</span></span> <span data-ttu-id="87b6d-117"><xref:Microsoft.AspNetCore.Components.Routing.Router>元件會出現在檔案中 `App.razor` ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-117">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component appears in the `App.razor` file:</span></span>

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

<span data-ttu-id="87b6d-118">當編譯具有指示詞的檔案時 `.razor` `@page` ，會提供所產生的類別來 <xref:Microsoft.AspNetCore.Components.RouteAttribute> 指定路由範本。</span><span class="sxs-lookup"><span data-stu-id="87b6d-118">When a `.razor` file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Components.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="87b6d-119">在執行時間， <xref:Microsoft.AspNetCore.Components.RouteView> 元件：</span><span class="sxs-lookup"><span data-stu-id="87b6d-119">At runtime, the <xref:Microsoft.AspNetCore.Components.RouteView> component:</span></span>

* <span data-ttu-id="87b6d-120">從接收， <xref:Microsoft.AspNetCore.Components.RouteData> <xref:Microsoft.AspNetCore.Components.Routing.Router> 連同任何想要的參數。</span><span class="sxs-lookup"><span data-stu-id="87b6d-120">Receives the <xref:Microsoft.AspNetCore.Components.RouteData> from the <xref:Microsoft.AspNetCore.Components.Routing.Router> along with any desired parameters.</span></span>
* <span data-ttu-id="87b6d-121">使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-121">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="87b6d-122">您可以選擇性地指定 <xref:Microsoft.AspNetCore.Components.RouteView.DefaultLayout> 具有版面配置類別的參數，以用於未指定版面配置的元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-122">You can optionally specify a <xref:Microsoft.AspNetCore.Components.RouteView.DefaultLayout> parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="87b6d-123">預設 Blazor 範本會指定 `MainLayout` 元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-123">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="87b6d-124">`MainLayout.razor`位於範本專案的資料夾中 `Shared` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-124">`MainLayout.razor` is in the template project's `Shared` folder.</span></span> <span data-ttu-id="87b6d-125">如需版面配置的詳細資訊，請參閱 <xref:blazor/layouts> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-125">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="87b6d-126">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-126">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="87b6d-127">下列元件會回應和的要求 `/BlazorRoute` `/DifferentBlazorRoute` ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-127">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> <span data-ttu-id="87b6d-128">為了讓 Url 正確解析，應用程式必須 `<base>` 在其檔案 `wwwroot/index.html` （）或檔案（）中包含標記，並在 Blazor WebAssembly `Pages/_Host.cshtml` Blazor Server 屬性中指定應用程式基底路徑 `href` （ `<base href="/">` ）。</span><span class="sxs-lookup"><span data-stu-id="87b6d-128">For URLs to resolve correctly, the app must include a `<base>` tag in its `wwwroot/index.html` file (Blazor WebAssembly) or `Pages/_Host.cshtml` file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="87b6d-129">如需詳細資訊，請參閱 <xref:blazor/host-and-deploy/index#app-base-path> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-129">For more information, see <xref:blazor/host-and-deploy/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="87b6d-130">在找不到內容時提供自訂內容</span><span class="sxs-lookup"><span data-stu-id="87b6d-130">Provide custom content when content isn't found</span></span>

<span data-ttu-id="87b6d-131"><xref:Microsoft.AspNetCore.Components.Routing.Router>如果找不到所要求路由的內容，此元件可讓應用程式指定自訂內容。</span><span class="sxs-lookup"><span data-stu-id="87b6d-131">The <xref:Microsoft.AspNetCore.Components.Routing.Router> component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="87b6d-132">在檔案中 `App.razor` ，于元件的範本參數中設定自訂內容 <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> <xref:Microsoft.AspNetCore.Components.Routing.Router> ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-132">In the `App.razor` file, set custom content in the <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> template parameter of the <xref:Microsoft.AspNetCore.Components.Routing.Router> component:</span></span>

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

<span data-ttu-id="87b6d-133">標記的內容 `<NotFound>` 可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-133">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="87b6d-134">若要將預設版面配置套用至 <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> 內容，請參閱 <xref:blazor/layouts> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-134">To apply a default layout to <xref:Microsoft.AspNetCore.Components.Routing.Router.NotFound> content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="87b6d-135">從多個元件路由至元件</span><span class="sxs-lookup"><span data-stu-id="87b6d-135">Route to components from multiple assemblies</span></span>

<span data-ttu-id="87b6d-136">使用 <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> 參數來指定 <xref:Microsoft.AspNetCore.Components.Routing.Router> 搜尋可路由的元件時，要考慮的元件的其他元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-136">Use the <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> parameter to specify additional assemblies for the <xref:Microsoft.AspNetCore.Components.Routing.Router> component to consider when searching for routable components.</span></span> <span data-ttu-id="87b6d-137">除了指定的元件以外，還會考慮指定的元件 `AppAssembly` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-137">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="87b6d-138">在下列範例中， `Component1` 是在參考的類別庫中定義的可路由元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-138">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="87b6d-139">下列 <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> 範例會產生的路由支援 `Component1` ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-139">The following <xref:Microsoft.AspNetCore.Components.Routing.Router.AdditionalAssemblies> example results in routing support for `Component1`:</span></span>

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="87b6d-140">路由參數</span><span class="sxs-lookup"><span data-stu-id="87b6d-140">Route parameters</span></span>

<span data-ttu-id="87b6d-141">路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：</span><span class="sxs-lookup"><span data-stu-id="87b6d-141">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

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

<span data-ttu-id="87b6d-142">不支援選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="87b6d-142">Optional parameters aren't supported.</span></span> <span data-ttu-id="87b6d-143">`@page`在上述範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="87b6d-143">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="87b6d-144">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-144">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="87b6d-145">第二個指示詞 `@page` 會採用 `{text}` route 參數，並將值指派給 `Text` 屬性。</span><span class="sxs-lookup"><span data-stu-id="87b6d-145">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="87b6d-146">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="87b6d-146">Route constraints</span></span>

<span data-ttu-id="87b6d-147">路由條件約束會將路由區段上的類型比對強制套用至元件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-147">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="87b6d-148">在下列範例中，對元件的路由 `Users` 只會符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="87b6d-148">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="87b6d-149">`Id`要求 URL 上有路由區段。</span><span class="sxs-lookup"><span data-stu-id="87b6d-149">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="87b6d-150">`Id`區段是一個整數（ `int` ）。</span><span class="sxs-lookup"><span data-stu-id="87b6d-150">The `Id` segment is an integer (`int`).</span></span>

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="87b6d-151">下表所示的路由條件約束可供使用。</span><span class="sxs-lookup"><span data-stu-id="87b6d-151">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="87b6d-152">若為符合不因文化特性而異的路由條件約束，請參閱表格下方的警告以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="87b6d-152">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="87b6d-153">條件約束</span><span class="sxs-lookup"><span data-stu-id="87b6d-153">Constraint</span></span> | <span data-ttu-id="87b6d-154">範例</span><span class="sxs-lookup"><span data-stu-id="87b6d-154">Example</span></span>           | <span data-ttu-id="87b6d-155">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="87b6d-155">Example Matches</span></span>                                                                  | <span data-ttu-id="87b6d-156">非變異值</span><span class="sxs-lookup"><span data-stu-id="87b6d-156">Invariant</span></span><br><span data-ttu-id="87b6d-157">culture</span><span class="sxs-lookup"><span data-stu-id="87b6d-157">culture</span></span><br><span data-ttu-id="87b6d-158">比對</span><span class="sxs-lookup"><span data-stu-id="87b6d-158">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="87b6d-159">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="87b6d-159">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="87b6d-160">No</span><span class="sxs-lookup"><span data-stu-id="87b6d-160">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="87b6d-161">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="87b6d-161">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="87b6d-162">Yes</span><span class="sxs-lookup"><span data-stu-id="87b6d-162">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="87b6d-163">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="87b6d-163">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="87b6d-164">Yes</span><span class="sxs-lookup"><span data-stu-id="87b6d-164">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="87b6d-165">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="87b6d-165">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="87b6d-166">Yes</span><span class="sxs-lookup"><span data-stu-id="87b6d-166">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="87b6d-167">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="87b6d-167">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="87b6d-168">Yes</span><span class="sxs-lookup"><span data-stu-id="87b6d-168">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="87b6d-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="87b6d-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="87b6d-170">No</span><span class="sxs-lookup"><span data-stu-id="87b6d-170">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="87b6d-171">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="87b6d-171">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="87b6d-172">Yes</span><span class="sxs-lookup"><span data-stu-id="87b6d-172">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="87b6d-173">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="87b6d-173">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="87b6d-174">Yes</span><span class="sxs-lookup"><span data-stu-id="87b6d-174">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="87b6d-175">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 <xref:System.DateTime>) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="87b6d-175">Route constraints that verify the URL and are converted to a CLR type (such as `int` or <xref:System.DateTime>) always use the invariant culture.</span></span> <span data-ttu-id="87b6d-176">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="87b6d-176">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="87b6d-177">包含點的 Url 路由</span><span class="sxs-lookup"><span data-stu-id="87b6d-177">Routing with URLs that contain dots</span></span>

<span data-ttu-id="87b6d-178">在 Blazor Server 應用程式中，中的預設路由 `_Host.cshtml` 是 `/` （ `@page "/"` ）。</span><span class="sxs-lookup"><span data-stu-id="87b6d-178">In Blazor Server apps, the default route in `_Host.cshtml` is `/` (`@page "/"`).</span></span> <span data-ttu-id="87b6d-179">包含點（）的要求 URL `.` 不符合預設路由，因為 URL 會顯示要求檔案。</span><span class="sxs-lookup"><span data-stu-id="87b6d-179">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="87b6d-180">如果 Blazor 靜態檔案不存在，應用程式會傳回*404-找不*到的回應。</span><span class="sxs-lookup"><span data-stu-id="87b6d-180">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="87b6d-181">若要使用包含點的路由，請 `_Host.cshtml` 使用下列路由範本進行設定：</span><span class="sxs-lookup"><span data-stu-id="87b6d-181">To use routes that contain a dot, configure `_Host.cshtml` with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="87b6d-182">此 `"/{**path}"` 範本包含：</span><span class="sxs-lookup"><span data-stu-id="87b6d-182">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="87b6d-183">雙星號*catch-all*語法（ `**` ）可跨多個資料夾界限捕捉路徑，而不需要編碼正斜線（ `/` ）。</span><span class="sxs-lookup"><span data-stu-id="87b6d-183">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="87b6d-184">`path`路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="87b6d-184">`path` route parameter name.</span></span>

> [!NOTE]
> <span data-ttu-id="87b6d-185">*Catch-all* `*` / `**` 元件（）中**不**支援 Catch Razor -all 參數語法（） `.razor` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-185">*Catch-all* parameter syntax (`*`/`**`) is **not** supported in Razor components (`.razor`).</span></span>

<span data-ttu-id="87b6d-186">如需詳細資訊，請參閱 <xref:fundamentals/routing> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-186">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="87b6d-187">NavLink 元件</span><span class="sxs-lookup"><span data-stu-id="87b6d-187">NavLink component</span></span>

<span data-ttu-id="87b6d-188"><xref:Microsoft.AspNetCore.Components.Routing.NavLink>建立導覽連結時，請使用元件來取代 HTML 超連結元素（ `<a>` ）。</span><span class="sxs-lookup"><span data-stu-id="87b6d-188">Use a <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="87b6d-189"><xref:Microsoft.AspNetCore.Components.Routing.NavLink>元件的行為類似 `<a>` 元素，但它會根據 `active` 其是否 `href` 符合目前的 URL 來切換 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="87b6d-189">A <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="87b6d-190">`active`類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。</span><span class="sxs-lookup"><span data-stu-id="87b6d-190">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span> <span data-ttu-id="87b6d-191">（選擇性）將 CSS 類別名稱指派給， <xref:Microsoft.AspNetCore.Components.Routing.NavLink.ActiveClass?displayProperty=nameWithType> 以在目前的路由符合時，將自訂 css 類別套用至轉譯的連結 `href` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-191">Optionally, assign a CSS class name to <xref:Microsoft.AspNetCore.Components.Routing.NavLink.ActiveClass?displayProperty=nameWithType> to apply a custom CSS class to the rendered link when the current route matches the `href`.</span></span>

<span data-ttu-id="87b6d-192">下列 `NavMenu` 元件 [`Bootstrap`](https://getbootstrap.com/docs/) 會建立導覽列，以示範如何使用 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件：</span><span class="sxs-lookup"><span data-stu-id="87b6d-192">The following `NavMenu` component creates a [`Bootstrap`](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use <xref:Microsoft.AspNetCore.Components.Routing.NavLink> components:</span></span>

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="87b6d-193">有兩個 <xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch> 選項可供您指派給 `Match` 元素的屬性 `<NavLink>` ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-193">There are two <xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch> options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="87b6d-194"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.All?displayProperty=nameWithType>： <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 當符合整個目前的 URL 時，就會作用中。</span><span class="sxs-lookup"><span data-stu-id="87b6d-194"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.All?displayProperty=nameWithType>: The <xref:Microsoft.AspNetCore.Components.Routing.NavLink> is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="87b6d-195"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.Prefix?displayProperty=nameWithType>（*預設值*）： <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 當它符合目前 URL 的任何前置詞時，就會作用中。</span><span class="sxs-lookup"><span data-stu-id="87b6d-195"><xref:Microsoft.AspNetCore.Components.Routing.NavLinkMatch.Prefix?displayProperty=nameWithType> (*default*): The <xref:Microsoft.AspNetCore.Components.Routing.NavLink> is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="87b6d-196">在上述範例中，Home 會 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> `href=""` 符合 home URL，而且只會 `active` 在應用程式的預設基底路徑 URL （例如，）接收 CSS 類別 `https://localhost:5001/` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-196">In the preceding example, the Home <xref:Microsoft.AspNetCore.Components.Routing.NavLink> `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="87b6d-197">第二個會在 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> `active` 使用者造訪具有前置詞的任何 URL 時收到類別 `MyComponent` （例如， `https://localhost:5001/MyComponent` 和 `https://localhost:5001/MyComponent/AnotherSegment` ）。</span><span class="sxs-lookup"><span data-stu-id="87b6d-197">The second <xref:Microsoft.AspNetCore.Components.Routing.NavLink> receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="87b6d-198">其他 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件屬性會傳遞至呈現的錨點標記。</span><span class="sxs-lookup"><span data-stu-id="87b6d-198">Additional <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="87b6d-199">在下列範例中， <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 元件包含 `target` 屬性：</span><span class="sxs-lookup"><span data-stu-id="87b6d-199">In the following example, the <xref:Microsoft.AspNetCore.Components.Routing.NavLink> component includes the `target` attribute:</span></span>

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="87b6d-200">會轉譯下列 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="87b6d-200">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank">My page</a>
```

> [!WARNING]
> <span data-ttu-id="87b6d-201">由於 Blazor 呈現子內容的方式， `NavLink` 如果在 `for` `NavLink` （子系）元件的內容中使用遞增迴圈變數，迴圈內的轉譯元件需要區域索引變數：</span><span class="sxs-lookup"><span data-stu-id="87b6d-201">Due to the way that Blazor renders child content, rendering `NavLink` components inside a `for` loop requires a local index variable if the incrementing loop variable is used in the `NavLink` (child) component's content:</span></span>
>
> ```razor
> @for (int c = 0; c < 10; c++)
> {
>     var current = c;
>     <li ...>
>         <NavLink ... href="@c">
>             <span ...></span> @current
>         </NavLink>
>     </li>
> }
> ```
>
> <span data-ttu-id="87b6d-202">在此案例中使用索引變數，是在其子[內容](xref:blazor/components/index#child-content)中使用迴圈變數的**任何**子元件（而不只是元件）的需求 `NavLink` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-202">Using an index variable in this scenario is a requirement for **any** child component that uses a loop variable in its [child content](xref:blazor/components/index#child-content), not just the `NavLink` component.</span></span>
>
> <span data-ttu-id="87b6d-203">或者，使用 `foreach` 迴圈搭配 <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType> ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-203">Alternatively, use a `foreach` loop with <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType>:</span></span>
>
> ```razor
> @foreach(var c in Enumerable.Range(0,10))
> {
>     <li ...>
>         <NavLink ... href="@c">
>             <span ...></span> @c
>         </NavLink>
>     </li>
> }
> ```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="87b6d-204">URI 和流覽狀態協助程式</span><span class="sxs-lookup"><span data-stu-id="87b6d-204">URI and navigation state helpers</span></span>

<span data-ttu-id="87b6d-205">用於 <xref:Microsoft.AspNetCore.Components.NavigationManager> 在 c # 程式碼中處理 uri 和導覽。</span><span class="sxs-lookup"><span data-stu-id="87b6d-205">Use <xref:Microsoft.AspNetCore.Components.NavigationManager> to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="87b6d-206"><xref:Microsoft.AspNetCore.Components.NavigationManager>提供下表所示的事件和方法。</span><span class="sxs-lookup"><span data-stu-id="87b6d-206"><xref:Microsoft.AspNetCore.Components.NavigationManager> provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="87b6d-207">成員</span><span class="sxs-lookup"><span data-stu-id="87b6d-207">Member</span></span> | <span data-ttu-id="87b6d-208">Description</span><span class="sxs-lookup"><span data-stu-id="87b6d-208">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.Components.NavigationManager.Uri> | <span data-ttu-id="87b6d-209">取得目前的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="87b6d-209">Gets the current absolute URI.</span></span> |
| <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> | <span data-ttu-id="87b6d-210">取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="87b6d-210">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="87b6d-211">通常會 <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> 對應至 `href` `<base>` `wwwroot/index.html` （ Blazor WebAssembly ）或 `Pages/_Host.cshtml` （）中檔元素上的屬性 Blazor Server 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-211">Typically, <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> corresponds to the `href` attribute on the document's `<base>` element in `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` (Blazor Server).</span></span> |
| <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A> | <span data-ttu-id="87b6d-212">導覽至指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="87b6d-212">Navigates to the specified URI.</span></span> <span data-ttu-id="87b6d-213">如果 `forceLoad` 為 `true` ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-213">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="87b6d-214">已略過用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="87b6d-214">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="87b6d-215">瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</span><span class="sxs-lookup"><span data-stu-id="87b6d-215">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| <xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged> | <span data-ttu-id="87b6d-216">導覽位置變更時引發的事件。</span><span class="sxs-lookup"><span data-stu-id="87b6d-216">An event that fires when the navigation location has changed.</span></span> |
| <xref:Microsoft.AspNetCore.Components.NavigationManager.ToAbsoluteUri%2A> | <span data-ttu-id="87b6d-217">將相對 URI 轉換為絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="87b6d-217">Converts a relative URI into an absolute URI.</span></span> |
| <span style="word-break:normal;word-wrap:normal"><xref:Microsoft.AspNetCore.Components.NavigationManager.ToBaseRelativePath%2A></span> | <span data-ttu-id="87b6d-218">假設基底 URI （例如先前傳回的 URI <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri> ），會將絕對 uri 轉換成相對於基底 uri 前置詞的 uri。</span><span class="sxs-lookup"><span data-stu-id="87b6d-218">Given a base URI (for example, a URI previously returned by <xref:Microsoft.AspNetCore.Components.NavigationManager.BaseUri>), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="87b6d-219">下列元件 `Counter` 會在選取按鈕時，流覽至應用程式的元件：</span><span class="sxs-lookup"><span data-stu-id="87b6d-219">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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

<span data-ttu-id="87b6d-220">下列元件會藉由訂閱來處理位置已變更事件 <xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-220">The following component handles a location changed event by subscribing to <xref:Microsoft.AspNetCore.Components.NavigationManager.LocationChanged?displayProperty=nameWithType>.</span></span> <span data-ttu-id="87b6d-221">`HandleLocationChanged`當架構呼叫時，方法會解除掛鉤 `Dispose` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-221">The `HandleLocationChanged` method is unhooked when `Dispose` is called by the framework.</span></span> <span data-ttu-id="87b6d-222">Unhooking 方法允許元件的垃圾收集。</span><span class="sxs-lookup"><span data-stu-id="87b6d-222">Unhooking the method permits garbage collection of the component.</span></span>

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

<span data-ttu-id="87b6d-223"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs>提供事件的下列相關資訊：</span><span class="sxs-lookup"><span data-stu-id="87b6d-223"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs> provides the following information about the event:</span></span>

* <span data-ttu-id="87b6d-224"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>：新位置的 URL。</span><span class="sxs-lookup"><span data-stu-id="87b6d-224"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>: The URL of the new location.</span></span>
* <span data-ttu-id="87b6d-225"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>：如果是，則會 `true` Blazor 攔截瀏覽器的導覽。</span><span class="sxs-lookup"><span data-stu-id="87b6d-225"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>: If `true`, Blazor intercepted the navigation from the browser.</span></span> <span data-ttu-id="87b6d-226">如果 `false` <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A?displayProperty=nameWithType> 為，則會造成導覽。</span><span class="sxs-lookup"><span data-stu-id="87b6d-226">If `false`, <xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A?displayProperty=nameWithType> caused the navigation to occur.</span></span>

<span data-ttu-id="87b6d-227">如需有關元件處置的詳細資訊，請參閱 <xref:blazor/components/lifecycle#component-disposal-with-idisposable> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-227">For more information on component disposal, see <xref:blazor/components/lifecycle#component-disposal-with-idisposable>.</span></span>

## <a name="query-string-and-parse-parameters"></a><span data-ttu-id="87b6d-228">查詢字串和剖析參數</span><span class="sxs-lookup"><span data-stu-id="87b6d-228">Query string and parse parameters</span></span>

<span data-ttu-id="87b6d-229">您可以從的屬性取得要求的查詢字串 <xref:Microsoft.AspNetCore.Components.NavigationManager> <xref:Microsoft.AspNetCore.Components.NavigationManager.Uri> ：</span><span class="sxs-lookup"><span data-stu-id="87b6d-229">The query string of a request can be obtained from the <xref:Microsoft.AspNetCore.Components.NavigationManager>'s <xref:Microsoft.AspNetCore.Components.NavigationManager.Uri> property:</span></span>

```razor
@inject NavigationManager Navigation

...

var query = new Uri(Navigation.Uri).Query;
```

<span data-ttu-id="87b6d-230">若要剖析查詢字串的參數：</span><span class="sxs-lookup"><span data-stu-id="87b6d-230">To parse a query string's parameters:</span></span>

* <span data-ttu-id="87b6d-231">新增[AspNetCore. WebUtilities](https://www.nuget.org/packages/Microsoft.AspNetCore.WebUtilities)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="87b6d-231">Add a package reference for [Microsoft.AspNetCore.WebUtilities](https://www.nuget.org/packages/Microsoft.AspNetCore.WebUtilities).</span></span>
* <span data-ttu-id="87b6d-232">使用剖析查詢字串之後，取得值 <xref:Microsoft.AspNetCore.WebUtilities.QueryHelpers.ParseQuery%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-232">Obtain the value after parsing the query string with <xref:Microsoft.AspNetCore.WebUtilities.QueryHelpers.ParseQuery%2A?displayProperty=nameWithType>.</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.WebUtilities
@inject NavigationManager NavigationManager

<h1>Query string parse example</h1>

<p>Value: @queryValue</p>

@code {
    private string queryValue = "Not set";

    protected override void OnInitialized()
    {
        var query = new Uri(NavigationManager.Uri).Query;

        if (QueryHelpers.ParseQuery(query).TryGetValue("{KEY}", out var value))
        {
            queryValue = value;
        }
    }
}
```

<span data-ttu-id="87b6d-233">`{KEY}`上述範例中的預留位置是查詢字串參數索引鍵。</span><span class="sxs-lookup"><span data-stu-id="87b6d-233">The placeholder `{KEY}` in the preceding example is the query string parameter key.</span></span> <span data-ttu-id="87b6d-234">例如，URL 索引鍵/值組會 `?ship=Tardis` 使用的索引鍵 `ship` 。</span><span class="sxs-lookup"><span data-stu-id="87b6d-234">For example, the URL key-value pair `?ship=Tardis` uses a key of `ship`.</span></span>
