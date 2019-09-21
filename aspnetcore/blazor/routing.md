---
title: ASP.NET Core Blazor 路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求，以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: 6d9d1614b6e0cc9f4711de0db4513ada4841809f
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168185"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="002e7-103">ASP.NET Core Blazor 路由</span><span class="sxs-lookup"><span data-stu-id="002e7-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="002e7-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="002e7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="002e7-105">瞭解如何路由傳送要求，以及如何使用`NavLink`元件在 Blazor apps 中建立導覽連結。</span><span class="sxs-lookup"><span data-stu-id="002e7-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="002e7-106">ASP.NET Core 端點路由整合</span><span class="sxs-lookup"><span data-stu-id="002e7-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="002e7-107">Blazor 伺服器已整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)中。</span><span class="sxs-lookup"><span data-stu-id="002e7-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="002e7-108">ASP.NET Core 應用程式已設定為接受`MapBlazorHub`中`Startup.Configure`的互動式元件連入連線：</span><span class="sxs-lookup"><span data-stu-id="002e7-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="002e7-109">路由範本</span><span class="sxs-lookup"><span data-stu-id="002e7-109">Route templates</span></span>

<span data-ttu-id="002e7-110">此`Router`元件可讓您使用指定的路由來路由傳送至每個元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-110">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="002e7-111">此`Router`元件會出現在*應用程式 razor*檔案中：</span><span class="sxs-lookup"><span data-stu-id="002e7-111">The `Router` component appears in the *App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="002e7-112">編譯含有 `@page`指示詞的 razor 檔案時，會提供<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本的所產生類別。</span><span class="sxs-lookup"><span data-stu-id="002e7-112">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="002e7-113">在執行時間， `RouteView`元件：</span><span class="sxs-lookup"><span data-stu-id="002e7-113">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="002e7-114">從接收，連同任何想要的參數。 `Router` `RouteData`</span><span class="sxs-lookup"><span data-stu-id="002e7-114">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="002e7-115">使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-115">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="002e7-116">您可以選擇性地指定`DefaultLayout`具有版面配置類別的參數，以用於未指定版面配置的元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-116">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="002e7-117">預設的 Blazor 範本會指定`MainLayout`元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-117">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="002e7-118">*MainLayout*是在範本專案的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="002e7-118">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="002e7-119">如需版面配置的詳細資訊<xref:blazor/layouts>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="002e7-119">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="002e7-120">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-120">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="002e7-121">下列元件會回應和`/BlazorRoute` `/DifferentBlazorRoute`的要求：</span><span class="sxs-lookup"><span data-stu-id="002e7-121">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="002e7-122">為了讓 url 正確解析，應用程式必須在其`<base>` *wwwroot/index.html*檔案（Blazor WebAssembly）或*Pages/_Host. cshtml*檔案（Blazor 伺服器）中包含一個標記，其中包含`href`屬性中指定的應用程式基底路徑（`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="002e7-122">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="002e7-123">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/index#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="002e7-123">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="002e7-124">在找不到內容時提供自訂內容</span><span class="sxs-lookup"><span data-stu-id="002e7-124">Provide custom content when content isn't found</span></span>

<span data-ttu-id="002e7-125">如果`Router`找不到所要求路由的內容，此元件可讓應用程式指定自訂內容。</span><span class="sxs-lookup"><span data-stu-id="002e7-125">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="002e7-126">在*應用程式的 razor*檔案中，于`NotFound` `Router`元件的範本參數中設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="002e7-126">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```cshtml
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

<span data-ttu-id="002e7-127">`<NotFound>`標記的內容可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-127">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="002e7-128">若要將預設版面配置`NotFound`套用至內容<xref:blazor/layouts>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="002e7-128">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="002e7-129">從多個元件路由至元件</span><span class="sxs-lookup"><span data-stu-id="002e7-129">Route to components from multiple assemblies</span></span>

<span data-ttu-id="002e7-130">使用參數來指定搜尋可路由的元件`Router`時，要考慮的元件的其他元件。 `AdditionalAssemblies`</span><span class="sxs-lookup"><span data-stu-id="002e7-130">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="002e7-131">除了指定的`AppAssembly`元件以外，還會考慮指定的元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-131">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="002e7-132">在下列範例中， `Component1`是在參考的類別庫中定義的可路由元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-132">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="002e7-133">下列`AdditionalAssemblies`範例會產生的`Component1`路由支援：</span><span class="sxs-lookup"><span data-stu-id="002e7-133">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="002e7-134">< 路由器 AppAssembly = "typeof （程式）。元件 "AdditionalAssemblies =" new [] {typeof （Component1）。Assembly} > .。。</Router></span><span class="sxs-lookup"><span data-stu-id="002e7-134"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="002e7-135">路由參數</span><span class="sxs-lookup"><span data-stu-id="002e7-135">Route parameters</span></span>

<span data-ttu-id="002e7-136">路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：</span><span class="sxs-lookup"><span data-stu-id="002e7-136">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="002e7-137">ASP.NET Core 3.0 Preview 中的 Blazor 應用程式不支援選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="002e7-137">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="002e7-138">在`@page`上述範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="002e7-138">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="002e7-139">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-139">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="002e7-140">第二`@page`個指示詞`{text}`會採用 route 參數，並將值`Text`指派給屬性。</span><span class="sxs-lookup"><span data-stu-id="002e7-140">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="002e7-141">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="002e7-141">Route constraints</span></span>

<span data-ttu-id="002e7-142">路由條件約束會將路由區段上的類型比對強制套用至元件。</span><span class="sxs-lookup"><span data-stu-id="002e7-142">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="002e7-143">在下列範例中，對`Users`元件的路由只會符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="002e7-143">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="002e7-144">要求 URL 上有路由區段。`Id`</span><span class="sxs-lookup"><span data-stu-id="002e7-144">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="002e7-145">區段是一個整數（`int`）。 `Id`</span><span class="sxs-lookup"><span data-stu-id="002e7-145">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="002e7-146">下表所示的路由條件約束可供使用。</span><span class="sxs-lookup"><span data-stu-id="002e7-146">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="002e7-147">若為符合不因文化特性而異的路由條件約束，請參閱表格下方的警告以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="002e7-147">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="002e7-148">條件約束</span><span class="sxs-lookup"><span data-stu-id="002e7-148">Constraint</span></span> | <span data-ttu-id="002e7-149">範例</span><span class="sxs-lookup"><span data-stu-id="002e7-149">Example</span></span>           | <span data-ttu-id="002e7-150">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="002e7-150">Example Matches</span></span>                                                                  | <span data-ttu-id="002e7-151">非變異值</span><span class="sxs-lookup"><span data-stu-id="002e7-151">Invariant</span></span><br><span data-ttu-id="002e7-152">文化特性</span><span class="sxs-lookup"><span data-stu-id="002e7-152">culture</span></span><br><span data-ttu-id="002e7-153">比對</span><span class="sxs-lookup"><span data-stu-id="002e7-153">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="002e7-154">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="002e7-154">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="002e7-155">否</span><span class="sxs-lookup"><span data-stu-id="002e7-155">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="002e7-156">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="002e7-156">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="002e7-157">是</span><span class="sxs-lookup"><span data-stu-id="002e7-157">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="002e7-158">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="002e7-158">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="002e7-159">是</span><span class="sxs-lookup"><span data-stu-id="002e7-159">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="002e7-160">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="002e7-160">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="002e7-161">是</span><span class="sxs-lookup"><span data-stu-id="002e7-161">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="002e7-162">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="002e7-162">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="002e7-163">是</span><span class="sxs-lookup"><span data-stu-id="002e7-163">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="002e7-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="002e7-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="002e7-165">否</span><span class="sxs-lookup"><span data-stu-id="002e7-165">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="002e7-166">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="002e7-166">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="002e7-167">是</span><span class="sxs-lookup"><span data-stu-id="002e7-167">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="002e7-168">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="002e7-168">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="002e7-169">是</span><span class="sxs-lookup"><span data-stu-id="002e7-169">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="002e7-170">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="002e7-170">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="002e7-171">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="002e7-171">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="002e7-172">包含點的 Url 路由</span><span class="sxs-lookup"><span data-stu-id="002e7-172">Routing with URLs that contain dots</span></span>

<span data-ttu-id="002e7-173">在 Blazor 伺服器應用程式中， *_Host*中的預設路由是`/` （`@page "/"`）。</span><span class="sxs-lookup"><span data-stu-id="002e7-173">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="002e7-174">包含點（`.`）的要求 URL 不符合預設路由，因為 URL 會顯示要求檔案。</span><span class="sxs-lookup"><span data-stu-id="002e7-174">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="002e7-175">如果靜態檔案不存在，Blazor 應用程式會傳回*404-找不*到的回應。</span><span class="sxs-lookup"><span data-stu-id="002e7-175">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="002e7-176">若要使用包含點的路由，請使用下列路由範本來設定 *_Host* ：</span><span class="sxs-lookup"><span data-stu-id="002e7-176">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="002e7-177">此`"/{**path}"`範本包含：</span><span class="sxs-lookup"><span data-stu-id="002e7-177">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="002e7-178">雙星號*catch-all*語法（`**`）可跨多個資料夾界限捕捉路徑，而不需要編碼正`/`斜線（）。</span><span class="sxs-lookup"><span data-stu-id="002e7-178">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="002e7-179">`path`路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="002e7-179">A `path` route parameter name.</span></span>

<span data-ttu-id="002e7-180">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="002e7-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="002e7-181">NavLink 元件</span><span class="sxs-lookup"><span data-stu-id="002e7-181">NavLink component</span></span>

<span data-ttu-id="002e7-182">建立導覽連結時，請使用`<a>`元件來取代HTML超連結元素（）。`NavLink`</span><span class="sxs-lookup"><span data-stu-id="002e7-182">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="002e7-183">元件`NavLink`的行為`<a>`類似元素`active` ，但它會根據其`href`是否符合目前的 URL 來切換 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="002e7-183">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="002e7-184">`active`類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。</span><span class="sxs-lookup"><span data-stu-id="002e7-184">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="002e7-185">下列`NavMenu`元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列，示範如何使用`NavLink`元件：</span><span class="sxs-lookup"><span data-stu-id="002e7-185">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="002e7-186">有兩個`NavLinkMatch`選項可`Match`供您指派給`<NavLink>`元素的屬性：</span><span class="sxs-lookup"><span data-stu-id="002e7-186">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="002e7-187">`NavLinkMatch.All`當符合整個目前的 URL 時，就會作用中`NavLink`。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="002e7-187">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="002e7-188">`NavLinkMatch.Prefix`（*預設值*）當符合目前 URL 的任何前置詞時，就會作用中`NavLink`。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="002e7-188">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="002e7-189">在上述`NavLink`範例中，home `href=""`會符合 home `active` URL，而且只會在應用程式的預設基底路徑 URL （例如， `https://localhost:5001/`）接收 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="002e7-189">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="002e7-190">第二`NavLink`個會`active`在使用者`MyComponent`造訪具有前置詞的任何 URL 時收到類別（例如`https://localhost:5001/MyComponent` ， `https://localhost:5001/MyComponent/AnotherSegment`和）。</span><span class="sxs-lookup"><span data-stu-id="002e7-190">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="002e7-191">其他`NavLink`元件屬性會傳遞至呈現的錨點標記。</span><span class="sxs-lookup"><span data-stu-id="002e7-191">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="002e7-192">在下列範例中， `NavLink`元件`target`包含屬性：</span><span class="sxs-lookup"><span data-stu-id="002e7-192">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="002e7-193">會轉譯下列 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="002e7-193">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="002e7-194">URI 和流覽狀態協助程式</span><span class="sxs-lookup"><span data-stu-id="002e7-194">URI and navigation state helpers</span></span>

<span data-ttu-id="002e7-195">用`Microsoft.AspNetCore.Components.NavigationManager`來處理常式代碼中C#的 uri 和導覽。</span><span class="sxs-lookup"><span data-stu-id="002e7-195">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="002e7-196">`NavigationManager`提供下表所示的事件和方法。</span><span class="sxs-lookup"><span data-stu-id="002e7-196">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="002e7-197">成員</span><span class="sxs-lookup"><span data-stu-id="002e7-197">Member</span></span> | <span data-ttu-id="002e7-198">描述</span><span class="sxs-lookup"><span data-stu-id="002e7-198">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="002e7-199">取得目前的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="002e7-199">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="002e7-200">取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="002e7-200">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="002e7-201">`href` `<base>`通常會對應至wwwroot/index.html（BlazorWebAssembly）或Pages/_Host.cshtml（BlazorServer）中檔元素上的屬性。`BaseUri`</span><span class="sxs-lookup"><span data-stu-id="002e7-201">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="002e7-202">導覽至指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="002e7-202">Navigates to the specified URI.</span></span> <span data-ttu-id="002e7-203">如果`forceLoad`為`true`：</span><span class="sxs-lookup"><span data-stu-id="002e7-203">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="002e7-204">已略過用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="002e7-204">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="002e7-205">瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</span><span class="sxs-lookup"><span data-stu-id="002e7-205">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="002e7-206">導覽位置變更時引發的事件。</span><span class="sxs-lookup"><span data-stu-id="002e7-206">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="002e7-207">將相對 URI 轉換為絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="002e7-207">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="002e7-208">假設基底 uri （例如先前傳回的`GetBaseUri`uri），會將絕對 uri 轉換成相對於基底 uri 前置詞的 uri。</span><span class="sxs-lookup"><span data-stu-id="002e7-208">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="002e7-209">下列元件會在選取按鈕時， `Counter`流覽至應用程式的元件：</span><span class="sxs-lookup"><span data-stu-id="002e7-209">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
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
