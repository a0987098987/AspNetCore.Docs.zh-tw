---
title: ASP.NET核心Blazor路由
author: guardrex
description: 瞭解如何在應用中路由請求以及 NavLink 元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/routing
ms.openlocfilehash: 87579c88a37e0258921e199db2b5d8c7627f5499
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218891"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="e1db7-103">ASP.NET核心布拉佐爾路由</span><span class="sxs-lookup"><span data-stu-id="e1db7-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="e1db7-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e1db7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e1db7-105">瞭解如何路由請求以及如何使用`NavLink`元件在 Blazor 應用中創建導航連結。</span><span class="sxs-lookup"><span data-stu-id="e1db7-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="e1db7-106">ASP.NET核心終結點路由整合</span><span class="sxs-lookup"><span data-stu-id="e1db7-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="e1db7-107">布拉佐伺服器整合[到 ASP.NET 核心端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="e1db7-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="e1db7-108">ASP.NET核心應用設定為接受`MapBlazorHub`具有`Startup.Configure`的 互動式元件的傳入連線:</span><span class="sxs-lookup"><span data-stu-id="e1db7-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="e1db7-109">最典型的配置是將所有請求路由到 Razor 頁面,該頁面充當 Blazor Server 應用伺服器端部分的主機。</span><span class="sxs-lookup"><span data-stu-id="e1db7-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="e1db7-110">依慣例,*主機*頁通常命名為 *_Host.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e1db7-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="e1db7-111">在主機檔中指定的路由稱為*回退路由*,因為它在路由匹配中優先順序較低。</span><span class="sxs-lookup"><span data-stu-id="e1db7-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="e1db7-112">當其他路由不匹配時,將考慮回退路由。</span><span class="sxs-lookup"><span data-stu-id="e1db7-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="e1db7-113">這允許應用程式使用其他控制器和頁面,而不會干擾 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="e1db7-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="e1db7-114">路由範本</span><span class="sxs-lookup"><span data-stu-id="e1db7-114">Route templates</span></span>

<span data-ttu-id="e1db7-115">該`Router`元件允許使用指定路由路由路由到每個元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="e1db7-116">元件`Router`將顯示在*App.razor*檔中:</span><span class="sxs-lookup"><span data-stu-id="e1db7-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="e1db7-117">編譯帶有`@page`指令的 *.razor*檔時,<xref:Microsoft.AspNetCore.Components.RouteAttribute>將提供指定 路由範本的生成類。</span><span class="sxs-lookup"><span data-stu-id="e1db7-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Components.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="e1db7-118">在執行時,`RouteView`元件:</span><span class="sxs-lookup"><span data-stu-id="e1db7-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="e1db7-119">從`RouteData`任何任何參數`Router`的參數接收 。</span><span class="sxs-lookup"><span data-stu-id="e1db7-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="e1db7-120">使用指定的參數渲染指定元件的佈局(或可選的預設佈局)。</span><span class="sxs-lookup"><span data-stu-id="e1db7-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="e1db7-121">可以選擇使用佈局類指定參數`DefaultLayout`,用於不指定佈局的元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="e1db7-122">預設的 Blazor 樣本`MainLayout`指定 元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="e1db7-123">*MainLayout.razor*位於範本專案的 *「共用」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e1db7-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="e1db7-124">有關佈局的詳細資訊,請參閱<xref:blazor/layouts>。</span><span class="sxs-lookup"><span data-stu-id="e1db7-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="e1db7-125">多個工藝路線範本可以應用於元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="e1db7-126">以下元件回應和`/BlazorRoute``/DifferentBlazorRoute`的請求。</span><span class="sxs-lookup"><span data-stu-id="e1db7-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

> [!IMPORTANT]
> <span data-ttu-id="e1db7-127">要正確解析 URL,應用必須在其*wwwroot/index.html*檔(Blazor WebAssembly)或*頁面/_Host.cshtml*檔(Blazor Server)中包含一個`href``<base href="/">``<base>`標記,並在屬性 () 中指定應用基本路徑。</span><span class="sxs-lookup"><span data-stu-id="e1db7-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="e1db7-128">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="e1db7-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="e1db7-129">在找不到內容時提供自訂內容</span><span class="sxs-lookup"><span data-stu-id="e1db7-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="e1db7-130">如果`Router`未找到請求的路由的內容,則元件允許應用指定自定義內容。</span><span class="sxs-lookup"><span data-stu-id="e1db7-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="e1db7-131">在*App.razor*檔中`NotFound``Router`,在 元件的樣本參數中設定自訂內容:</span><span class="sxs-lookup"><span data-stu-id="e1db7-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="e1db7-132">`<NotFound>`標記的內容可以包括任意專案,如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="e1db7-133">要將默認佈局應用於`NotFound`內容,請參閱<xref:blazor/layouts>。</span><span class="sxs-lookup"><span data-stu-id="e1db7-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="e1db7-134">從多個程式集路由到元件</span><span class="sxs-lookup"><span data-stu-id="e1db7-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="e1db7-135">使用`AdditionalAssemblies`參數`Router`為 元件指定在搜尋可路由元件時要考慮的其他程式集。</span><span class="sxs-lookup"><span data-stu-id="e1db7-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="e1db7-136">除了`AppAssembly`指定的程式集之外,還考慮指定程式集。</span><span class="sxs-lookup"><span data-stu-id="e1db7-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="e1db7-137">在下面的範例中,`Component1`是在引用的類庫中定義的可路由元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="e1db7-138">以下`AdditionalAssemblies`範例導致路`Component1`由 支援 :</span><span class="sxs-lookup"><span data-stu-id="e1db7-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```razor
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="e1db7-139">路由參數</span><span class="sxs-lookup"><span data-stu-id="e1db7-139">Route parameters</span></span>

<span data-ttu-id="e1db7-140">路由器使用路由參數使用同名(不區分大小寫)填充相應的元件參數:</span><span class="sxs-lookup"><span data-stu-id="e1db7-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

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

<span data-ttu-id="e1db7-141">不支援可選參數。</span><span class="sxs-lookup"><span data-stu-id="e1db7-141">Optional parameters aren't supported.</span></span> <span data-ttu-id="e1db7-142">在前面`@page`的示例中應用了兩個指令。</span><span class="sxs-lookup"><span data-stu-id="e1db7-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="e1db7-143">第一個允許在沒有參數的情況下導航到元件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="e1db7-144">第二`@page`個指令以`{text}`由參數並將值分配給屬性`Text`。</span><span class="sxs-lookup"><span data-stu-id="e1db7-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="e1db7-145">路由約束</span><span class="sxs-lookup"><span data-stu-id="e1db7-145">Route constraints</span></span>

<span data-ttu-id="e1db7-146">路由約束強制路由段上與元件的類型匹配。</span><span class="sxs-lookup"><span data-stu-id="e1db7-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="e1db7-147">在以下範例中,到元件的`Users`路由僅在以下情況匹配時匹配:</span><span class="sxs-lookup"><span data-stu-id="e1db7-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="e1db7-148">`Id`請求 URL 上存在路由段。</span><span class="sxs-lookup"><span data-stu-id="e1db7-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="e1db7-149">段`Id`是整數`int`( 。</span><span class="sxs-lookup"><span data-stu-id="e1db7-149">The `Id` segment is an integer (`int`).</span></span>

[!code-razor[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="e1db7-150">下表中顯示的路由約束可用。</span><span class="sxs-lookup"><span data-stu-id="e1db7-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="e1db7-151">有關與不變區域性匹配的路由約束,請參閱下表下面的警告以瞭解更多資訊。</span><span class="sxs-lookup"><span data-stu-id="e1db7-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="e1db7-152">條件約束</span><span class="sxs-lookup"><span data-stu-id="e1db7-152">Constraint</span></span> | <span data-ttu-id="e1db7-153">範例</span><span class="sxs-lookup"><span data-stu-id="e1db7-153">Example</span></span>           | <span data-ttu-id="e1db7-154">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="e1db7-154">Example Matches</span></span>                                                                  | <span data-ttu-id="e1db7-155">非變異值</span><span class="sxs-lookup"><span data-stu-id="e1db7-155">Invariant</span></span><br><span data-ttu-id="e1db7-156">culture</span><span class="sxs-lookup"><span data-stu-id="e1db7-156">culture</span></span><br><span data-ttu-id="e1db7-157">比對</span><span class="sxs-lookup"><span data-stu-id="e1db7-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="e1db7-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="e1db7-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="e1db7-159">否</span><span class="sxs-lookup"><span data-stu-id="e1db7-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="e1db7-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="e1db7-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="e1db7-161">是</span><span class="sxs-lookup"><span data-stu-id="e1db7-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="e1db7-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="e1db7-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="e1db7-163">是</span><span class="sxs-lookup"><span data-stu-id="e1db7-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="e1db7-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="e1db7-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="e1db7-165">是</span><span class="sxs-lookup"><span data-stu-id="e1db7-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="e1db7-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="e1db7-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="e1db7-167">是</span><span class="sxs-lookup"><span data-stu-id="e1db7-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="e1db7-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="e1db7-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="e1db7-169">否</span><span class="sxs-lookup"><span data-stu-id="e1db7-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="e1db7-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="e1db7-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="e1db7-171">是</span><span class="sxs-lookup"><span data-stu-id="e1db7-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="e1db7-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="e1db7-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="e1db7-173">是</span><span class="sxs-lookup"><span data-stu-id="e1db7-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="e1db7-174">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="e1db7-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="e1db7-175">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e1db7-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="e1db7-176">包含點的網址路由</span><span class="sxs-lookup"><span data-stu-id="e1db7-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="e1db7-177">在 Blazor Server 應用中 *,_Host.cshtml*中的`/`預設`@page "/"`路由為 ( 。</span><span class="sxs-lookup"><span data-stu-id="e1db7-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="e1db7-178">包含點`.`( ) 的請求網址 與預設路由不匹配,因為 URL 似乎請求檔。</span><span class="sxs-lookup"><span data-stu-id="e1db7-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="e1db7-179">Blazor 應用返回*404 - 未找到*不存在的靜態檔的回應。</span><span class="sxs-lookup"><span data-stu-id="e1db7-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="e1db7-180">要使用包含點的路由,請使用以下路由範本設定 *_Host.cshtml:*</span><span class="sxs-lookup"><span data-stu-id="e1db7-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="e1db7-181">該`"/{**path}"`樣本包括:</span><span class="sxs-lookup"><span data-stu-id="e1db7-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="e1db7-182">雙星號*捕獲所有*語`**`法 ( ) 用於跨多個資料夾邊界捕獲路徑,而無需編碼`/`正向斜杠 ( )。</span><span class="sxs-lookup"><span data-stu-id="e1db7-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="e1db7-183">`path`路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="e1db7-183">`path` route parameter name.</span></span>

> [!NOTE]
> <span data-ttu-id="e1db7-184">Razor 元件`*`/`**`(*.razor*)\**不支援\*\*\*「 全部捕捉*」參數語法 ( )</span><span class="sxs-lookup"><span data-stu-id="e1db7-184">*Catch-all* parameter syntax (`*`/`**`) is **not** supported in Razor components (*.razor*).</span></span>

<span data-ttu-id="e1db7-185">如需詳細資訊，請參閱 <xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="e1db7-185">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="e1db7-186">導覽連結元件</span><span class="sxs-lookup"><span data-stu-id="e1db7-186">NavLink component</span></span>

<span data-ttu-id="e1db7-187">建立瀏`NavLink`覽 連結時,使用元件代替`<a>`HTML 超連結元素 ( ) 。</span><span class="sxs-lookup"><span data-stu-id="e1db7-187">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="e1db7-188">元件`NavLink`行為類似於`<a>`元素,只不過它根據是否`active``href`與當前 URL 匹配來切換 CSS 類。</span><span class="sxs-lookup"><span data-stu-id="e1db7-188">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="e1db7-189">該`active`類可幫助使用者了解顯示的導航連結中哪個頁面是活動頁面。</span><span class="sxs-lookup"><span data-stu-id="e1db7-189">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="e1db7-190">以下`NavMenu`元件建立[Bootstrap](https://getbootstrap.com/docs/)導航列,示範如何使用`NavLink`元件:</span><span class="sxs-lookup"><span data-stu-id="e1db7-190">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-razor[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="e1db7-191">有兩`NavLinkMatch`個選項可以分配給`Match``<NavLink>`元素的屬性:</span><span class="sxs-lookup"><span data-stu-id="e1db7-191">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="e1db7-192">`NavLinkMatch.All`&ndash;與`NavLink`整個當前網址匹配時處於活動狀態。</span><span class="sxs-lookup"><span data-stu-id="e1db7-192">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="e1db7-193">`NavLinkMatch.Prefix`(*預設*)&ndash;與`NavLink`當前URL的任何首元匹配時處於活動狀態。</span><span class="sxs-lookup"><span data-stu-id="e1db7-193">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="e1db7-194">在前面的示例中,Home`NavLink``href=""`與主 URL 匹配,並且`active`僅在應用的預設基本路徑`https://localhost:5001/`URL(例如, ) 接收 CSS 類。</span><span class="sxs-lookup"><span data-stu-id="e1db7-194">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="e1db7-195">當使用者存`NavLink`取 具有`active`前置碼的任何網址時,`MyComponent`第二個 接收`https://localhost:5001/MyComponent`類別`https://localhost:5001/MyComponent/AnotherSegment`(例如, 與 。</span><span class="sxs-lookup"><span data-stu-id="e1db7-195">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="e1db7-196">其他`NavLink`元件屬性將傳遞到呈現的錨點標記。</span><span class="sxs-lookup"><span data-stu-id="e1db7-196">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="e1db7-197">在下面的範例中,`NavLink`元件包括以下`target`屬性:</span><span class="sxs-lookup"><span data-stu-id="e1db7-197">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```razor
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="e1db7-198">顯示以下 HTML 標籤:</span><span class="sxs-lookup"><span data-stu-id="e1db7-198">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="e1db7-199">URI 與導覽狀態說明器</span><span class="sxs-lookup"><span data-stu-id="e1db7-199">URI and navigation state helpers</span></span>

<span data-ttu-id="e1db7-200">用於<xref:Microsoft.AspNetCore.Components.NavigationManager>在 C# 代碼中處理 URI 和導航。</span><span class="sxs-lookup"><span data-stu-id="e1db7-200">Use <xref:Microsoft.AspNetCore.Components.NavigationManager> to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="e1db7-201">`NavigationManager`提供了下表中顯示的事件和方法。</span><span class="sxs-lookup"><span data-stu-id="e1db7-201">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="e1db7-202">member</span><span class="sxs-lookup"><span data-stu-id="e1db7-202">Member</span></span> | <span data-ttu-id="e1db7-203">描述</span><span class="sxs-lookup"><span data-stu-id="e1db7-203">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="e1db7-204">Uri</span><span class="sxs-lookup"><span data-stu-id="e1db7-204">Uri</span></span> | <span data-ttu-id="e1db7-205">獲取當前絕對URI。</span><span class="sxs-lookup"><span data-stu-id="e1db7-205">Gets the current absolute URI.</span></span> |
| <span data-ttu-id="e1db7-206">BaseUri</span><span class="sxs-lookup"><span data-stu-id="e1db7-206">BaseUri</span></span> | <span data-ttu-id="e1db7-207">獲取基礎 URI(帶有尾隨斜杠),該URI可以預置到相對 URI 路徑以生成絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="e1db7-207">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="e1db7-208">通常,`BaseUri`對應於`href``<base>`文檔 元素上的屬性(wwwroot/index.html(WebAssembly)*wwwroot/index.html*Blazor或*頁面/_Host.cshtml(*Blazor伺服器)。</span><span class="sxs-lookup"><span data-stu-id="e1db7-208">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| <span data-ttu-id="e1db7-209">導覽到</span><span class="sxs-lookup"><span data-stu-id="e1db7-209">NavigateTo</span></span> | <span data-ttu-id="e1db7-210">導航到指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="e1db7-210">Navigates to the specified URI.</span></span> <span data-ttu-id="e1db7-211">如果是`forceLoad``true`:</span><span class="sxs-lookup"><span data-stu-id="e1db7-211">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="e1db7-212">繞過用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="e1db7-212">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="e1db7-213">無論URI通常由用戶端路由器處理,瀏覽器都被迫從伺服器載入新頁面。</span><span class="sxs-lookup"><span data-stu-id="e1db7-213">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| <span data-ttu-id="e1db7-214">位置已變更</span><span class="sxs-lookup"><span data-stu-id="e1db7-214">LocationChanged</span></span> | <span data-ttu-id="e1db7-215">當導航位置更改時觸發的事件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-215">An event that fires when the navigation location has changed.</span></span> |
| <span data-ttu-id="e1db7-216">到絕對</span><span class="sxs-lookup"><span data-stu-id="e1db7-216">ToAbsoluteUri</span></span> | <span data-ttu-id="e1db7-217">將相對 URI 轉換為絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="e1db7-217">Converts a relative URI into an absolute URI.</span></span> |
| <span data-ttu-id="e1db7-218"><span style="word-break:normal;word-wrap:normal">到基礎相對路徑</span></span><span class="sxs-lookup"><span data-stu-id="e1db7-218"><span style="word-break:normal;word-wrap:normal">ToBaseRelativePath</span></span></span> | <span data-ttu-id="e1db7-219">給定基本 URI(例如,以前`GetBaseUri`由返回的 URI)將絕對 URI 轉換為相對於基本 URI 前綴的 URI。</span><span class="sxs-lookup"><span data-stu-id="e1db7-219">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="e1db7-220">選擇按鈕時,以下元件將導航到應用`Counter`的元件:</span><span class="sxs-lookup"><span data-stu-id="e1db7-220">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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

<span data-ttu-id="e1db7-221">以下元件處理位置更改的事件。</span><span class="sxs-lookup"><span data-stu-id="e1db7-221">The following component handles a location changed event.</span></span> <span data-ttu-id="e1db7-222">當`HandleLocationChanged`框架調用該方法`Dispose`時 ,該方法將取消掛號。</span><span class="sxs-lookup"><span data-stu-id="e1db7-222">The `HandleLocationChanged` method is unhooked when `Dispose` is called by the framework.</span></span> <span data-ttu-id="e1db7-223">取消掛接該方法允許對元件進行垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="e1db7-223">Unhooking the method permits garbage collection of the component.</span></span>

```razor
@implement IDisposable
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

<span data-ttu-id="e1db7-224"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs>提供有關該事件的以下資訊:</span><span class="sxs-lookup"><span data-stu-id="e1db7-224"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs> provides the following information about the event:</span></span>

* <span data-ttu-id="e1db7-225"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location>&ndash;新位置的 URL。</span><span class="sxs-lookup"><span data-stu-id="e1db7-225"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.Location> &ndash; The URL of the new location.</span></span>
* <span data-ttu-id="e1db7-226"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted>&ndash;`true`如果Blazor,從瀏覽器截獲了導航。</span><span class="sxs-lookup"><span data-stu-id="e1db7-226"><xref:Microsoft.AspNetCore.Components.Routing.LocationChangedEventArgs.IsNavigationIntercepted> &ndash; If `true`, Blazor intercepted the navigation from the browser.</span></span> <span data-ttu-id="e1db7-227">如果`false`,[導航管理器.導航到](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A)導致導航發生。</span><span class="sxs-lookup"><span data-stu-id="e1db7-227">If `false`, [NavigationManager.NavigateTo](xref:Microsoft.AspNetCore.Components.NavigationManager.NavigateTo%2A) caused the navigation to occur.</span></span>

<span data-ttu-id="e1db7-228">有關元件處理的詳細資訊,請參閱<xref:blazor/lifecycle#component-disposal-with-idisposable>。</span><span class="sxs-lookup"><span data-stu-id="e1db7-228">For more information on component disposal, see <xref:blazor/lifecycle#component-disposal-with-idisposable>.</span></span>
