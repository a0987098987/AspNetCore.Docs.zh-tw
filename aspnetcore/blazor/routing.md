---
title: ASP.NET Core Blazor 路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求，以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: d4b76c00f79f333884fa7e30b27eadc6e36de287
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589929"
---
# <a name="aspnet-core-opno-locblazor-routing"></a><span data-ttu-id="43dfd-103">ASP.NET Core Blazor 路由</span><span class="sxs-lookup"><span data-stu-id="43dfd-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="43dfd-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="43dfd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="43dfd-105">瞭解如何路由傳送要求，以及如何使用 `NavLink` 元件，在 Blazor 應用程式中建立導覽連結。</span><span class="sxs-lookup"><span data-stu-id="43dfd-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="43dfd-106">ASP.NET Core 端點路由整合</span><span class="sxs-lookup"><span data-stu-id="43dfd-106">ASP.NET Core endpoint routing integration</span></span>

Blazor<span data-ttu-id="43dfd-107"> Server 已整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="43dfd-107"> Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="43dfd-108">ASP.NET Core 應用程式會設定為在 `Startup.Configure` 中接受具有 `MapBlazorHub` 之互動式元件的連入連線：</span><span class="sxs-lookup"><span data-stu-id="43dfd-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="43dfd-109">最常見的設定是將所有要求路由傳送至 Razor 頁面，做為 Blazor 伺服器應用程式的伺服器端部分的主機。</span><span class="sxs-lookup"><span data-stu-id="43dfd-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="43dfd-110">依照慣例，*主機*頁面通常會命名為 *_Host*。</span><span class="sxs-lookup"><span data-stu-id="43dfd-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="43dfd-111">主機檔案中指定的路由稱為「回溯*路由*」，因為它在路由比對中以低優先順序運作。</span><span class="sxs-lookup"><span data-stu-id="43dfd-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="43dfd-112">當其他路由不相符時，就會考慮回退路由。</span><span class="sxs-lookup"><span data-stu-id="43dfd-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="43dfd-113">這可讓應用程式使用其他控制器和頁面，而不會干擾 Blazor 伺服器應用程式。</span><span class="sxs-lookup"><span data-stu-id="43dfd-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="43dfd-114">路由範本</span><span class="sxs-lookup"><span data-stu-id="43dfd-114">Route templates</span></span>

<span data-ttu-id="43dfd-115">`Router` 元件可讓您使用指定的路由來路由傳送至每個元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="43dfd-116">`Router` 元件會出現在*應用程式 razor*檔案中：</span><span class="sxs-lookup"><span data-stu-id="43dfd-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="43dfd-117">編譯具有 `@page` 指示詞的*razor*檔案時，會提供所產生的類別 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> 指定路由範本。</span><span class="sxs-lookup"><span data-stu-id="43dfd-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="43dfd-118">在執行時間，`RouteView` 元件：</span><span class="sxs-lookup"><span data-stu-id="43dfd-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="43dfd-119">接收來自 `Router` 的 `RouteData` 以及任何想要的參數。</span><span class="sxs-lookup"><span data-stu-id="43dfd-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="43dfd-120">使用指定的參數，以配置（或選擇性的預設版面配置）呈現指定的元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="43dfd-121">您可以選擇性地指定具有版面配置類別的 `DefaultLayout` 參數，以用於未指定版面配置的元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="43dfd-122">預設 Blazor 範本會指定 `MainLayout` 元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="43dfd-123">*MainLayout*是在範本專案的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="43dfd-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="43dfd-124">如需版面配置的詳細資訊，請參閱 <xref:blazor/layouts>。</span><span class="sxs-lookup"><span data-stu-id="43dfd-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="43dfd-125">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="43dfd-126">下列元件會回應 `/BlazorRoute` 和 `/DifferentBlazorRoute` 的要求：</span><span class="sxs-lookup"><span data-stu-id="43dfd-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="43dfd-127">為了讓 Url 正確解析，應用程式必須在其*wwwroot/index.html*檔案（Blazor WebAssembly）或*Pages/_Host*檔（Blazor 伺服器）中包含 `<base>` 標記，並在 `href` 屬性（`<base href="/">`）中指定應用程式基底路徑.</span><span class="sxs-lookup"><span data-stu-id="43dfd-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="43dfd-128">如需詳細資訊，請參閱<xref:host-and-deploy/blazor/index#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="43dfd-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="43dfd-129">在找不到內容時提供自訂內容</span><span class="sxs-lookup"><span data-stu-id="43dfd-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="43dfd-130">如果找不到所要求路由的內容，`Router` 元件可讓應用程式指定自訂內容。</span><span class="sxs-lookup"><span data-stu-id="43dfd-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="43dfd-131">在*應用程式 razor*檔案中，于 `Router` 元件的 `NotFound` 範本參數中設定自訂內容：</span><span class="sxs-lookup"><span data-stu-id="43dfd-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="43dfd-132">`<NotFound>` 標記的內容可以包含任意專案，例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="43dfd-133">若要將預設版面配置套用至 `NotFound` 內容，請參閱 <xref:blazor/layouts>。</span><span class="sxs-lookup"><span data-stu-id="43dfd-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="43dfd-134">從多個元件路由至元件</span><span class="sxs-lookup"><span data-stu-id="43dfd-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="43dfd-135">使用 `AdditionalAssemblies` 參數來指定搜尋可路由的元件時，要考慮的 `Router` 元件的其他元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="43dfd-136">除了 `AppAssembly`指定的元件以外，還會考慮指定的元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="43dfd-137">在下列範例中，`Component1` 是在參考的類別庫中定義的可路由元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="43dfd-138">下列 `AdditionalAssemblies` 範例會導致 `Component1` 的路由支援：</span><span class="sxs-lookup"><span data-stu-id="43dfd-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="43dfd-139">路由參數</span><span class="sxs-lookup"><span data-stu-id="43dfd-139">Route parameters</span></span>

<span data-ttu-id="43dfd-140">路由器會使用路由參數來填入具有相同名稱的對應元件參數（不區分大小寫）：</span><span class="sxs-lookup"><span data-stu-id="43dfd-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="43dfd-141">ASP.NET Core 3.0 中的 Blazor 應用程式不支援選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="43dfd-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="43dfd-142">上一個範例中會套用兩個 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="43dfd-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="43dfd-143">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="43dfd-144">第二個 `@page` 指示詞會採用 `{text}` 路由參數，並將值指派給 `Text` 屬性。</span><span class="sxs-lookup"><span data-stu-id="43dfd-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="43dfd-145">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="43dfd-145">Route constraints</span></span>

<span data-ttu-id="43dfd-146">路由條件約束會將路由區段上的類型比對強制套用至元件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="43dfd-147">在下列範例中，`Users` 元件的路由只會符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="43dfd-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="43dfd-148">要求 URL 上有 `Id` 的路由區段。</span><span class="sxs-lookup"><span data-stu-id="43dfd-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="43dfd-149">`Id` 區段是一個整數（`int`）。</span><span class="sxs-lookup"><span data-stu-id="43dfd-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="43dfd-150">下表所示的路由條件約束可供使用。</span><span class="sxs-lookup"><span data-stu-id="43dfd-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="43dfd-151">若為符合不因文化特性而異的路由條件約束，請參閱表格下方的警告以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="43dfd-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="43dfd-152">條件約束</span><span class="sxs-lookup"><span data-stu-id="43dfd-152">Constraint</span></span> | <span data-ttu-id="43dfd-153">範例</span><span class="sxs-lookup"><span data-stu-id="43dfd-153">Example</span></span>           | <span data-ttu-id="43dfd-154">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="43dfd-154">Example Matches</span></span>                                                                  | <span data-ttu-id="43dfd-155">非變異值</span><span class="sxs-lookup"><span data-stu-id="43dfd-155">Invariant</span></span><br><span data-ttu-id="43dfd-156">文化特性</span><span class="sxs-lookup"><span data-stu-id="43dfd-156">culture</span></span><br><span data-ttu-id="43dfd-157">比對</span><span class="sxs-lookup"><span data-stu-id="43dfd-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="43dfd-158">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="43dfd-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="43dfd-159">否</span><span class="sxs-lookup"><span data-stu-id="43dfd-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="43dfd-160">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="43dfd-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="43dfd-161">是</span><span class="sxs-lookup"><span data-stu-id="43dfd-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="43dfd-162">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="43dfd-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="43dfd-163">是</span><span class="sxs-lookup"><span data-stu-id="43dfd-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="43dfd-164">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="43dfd-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="43dfd-165">是</span><span class="sxs-lookup"><span data-stu-id="43dfd-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="43dfd-166">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="43dfd-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="43dfd-167">是</span><span class="sxs-lookup"><span data-stu-id="43dfd-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="43dfd-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="43dfd-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="43dfd-169">否</span><span class="sxs-lookup"><span data-stu-id="43dfd-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="43dfd-170">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="43dfd-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="43dfd-171">是</span><span class="sxs-lookup"><span data-stu-id="43dfd-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="43dfd-172">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="43dfd-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="43dfd-173">是</span><span class="sxs-lookup"><span data-stu-id="43dfd-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="43dfd-174">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="43dfd-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="43dfd-175">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="43dfd-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="43dfd-176">包含點的 Url 路由</span><span class="sxs-lookup"><span data-stu-id="43dfd-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="43dfd-177">在 Blazor 伺服器應用程式中， *_Host*中的預設路由是 `/` （`@page "/"`）。</span><span class="sxs-lookup"><span data-stu-id="43dfd-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="43dfd-178">包含點（`.`）的要求 URL 不符合預設路由，因為 URL 會顯示要求檔案。</span><span class="sxs-lookup"><span data-stu-id="43dfd-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="43dfd-179">Blazor 應用程式針對不存在的靜態檔案傳回*404-找不到*回應。</span><span class="sxs-lookup"><span data-stu-id="43dfd-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="43dfd-180">若要使用包含點的路由，請使用下列路由範本來設定 *_Host* ：</span><span class="sxs-lookup"><span data-stu-id="43dfd-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="43dfd-181">`"/{**path}"` 範本包含：</span><span class="sxs-lookup"><span data-stu-id="43dfd-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="43dfd-182">雙星號的*catch-all*語法（`**`），可跨多個資料夾界限捕捉路徑，而不需要編碼正斜線（`/`）。</span><span class="sxs-lookup"><span data-stu-id="43dfd-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="43dfd-183">`path` 路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="43dfd-183">A `path` route parameter name.</span></span>

<span data-ttu-id="43dfd-184">如需詳細資訊，請參閱<xref:fundamentals/routing>。</span><span class="sxs-lookup"><span data-stu-id="43dfd-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="43dfd-185">NavLink 元件</span><span class="sxs-lookup"><span data-stu-id="43dfd-185">NavLink component</span></span>

<span data-ttu-id="43dfd-186">建立導覽連結時，請使用 `NavLink` 元件來取代 HTML 超連結元素（`<a>`）。</span><span class="sxs-lookup"><span data-stu-id="43dfd-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="43dfd-187">`NavLink` 元件的行為就像 `<a>` 元素，不同之處在于它會根據其 `href` 是否符合目前的 URL 來切換 `active` 的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="43dfd-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="43dfd-188">`active` 類別可協助使用者瞭解在顯示的導覽連結中，哪個頁面是使用中的頁面。</span><span class="sxs-lookup"><span data-stu-id="43dfd-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="43dfd-189">下列 `NavMenu` 元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列，示範如何使用 `NavLink` 元件：</span><span class="sxs-lookup"><span data-stu-id="43dfd-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="43dfd-190">有兩個 `NavLinkMatch` 選項可供您指派給 `<NavLink>` 元素的 `Match` 屬性：</span><span class="sxs-lookup"><span data-stu-id="43dfd-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="43dfd-191">`NavLinkMatch.All` &ndash; 當它符合整個目前的 URL 時，`NavLink` 為作用中。</span><span class="sxs-lookup"><span data-stu-id="43dfd-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="43dfd-192">`NavLinkMatch.Prefix` （*預設值*） &ndash; 當它符合目前 URL 的任何前置詞時，`NavLink` 為作用中。</span><span class="sxs-lookup"><span data-stu-id="43dfd-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="43dfd-193">在上述範例中，Home `NavLink` `href=""` 符合 home URL，而且只會在應用程式的預設基底路徑 URL （例如 `https://localhost:5001/`）收到 `active` 的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="43dfd-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="43dfd-194">第二個 `NavLink` 會在使用者造訪具有 `MyComponent` 前置詞的任何 URL 時收到 `active` 類別（例如 `https://localhost:5001/MyComponent` 和 `https://localhost:5001/MyComponent/AnotherSegment`）。</span><span class="sxs-lookup"><span data-stu-id="43dfd-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="43dfd-195">額外的 `NavLink` 元件屬性會傳遞至呈現的錨點標記。</span><span class="sxs-lookup"><span data-stu-id="43dfd-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="43dfd-196">在下列範例中，`NavLink` 元件包含 `target` 屬性：</span><span class="sxs-lookup"><span data-stu-id="43dfd-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="43dfd-197">會轉譯下列 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="43dfd-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="43dfd-198">URI 和流覽狀態協助程式</span><span class="sxs-lookup"><span data-stu-id="43dfd-198">URI and navigation state helpers</span></span>

<span data-ttu-id="43dfd-199">使用 `Microsoft.AspNetCore.Components.NavigationManager` 來處理常式代碼中C#的 uri 和導覽。</span><span class="sxs-lookup"><span data-stu-id="43dfd-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="43dfd-200">`NavigationManager` 提供下表所示的事件和方法。</span><span class="sxs-lookup"><span data-stu-id="43dfd-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="43dfd-201">成員</span><span class="sxs-lookup"><span data-stu-id="43dfd-201">Member</span></span> | <span data-ttu-id="43dfd-202">描述</span><span class="sxs-lookup"><span data-stu-id="43dfd-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="43dfd-203">取得目前的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="43dfd-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="43dfd-204">取得可在相對 URI 路徑前面加上的基底 URI （含尾端斜線），以產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="43dfd-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="43dfd-205">一般來說，`BaseUri` 會對應至*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host*中檔的 `<base>` 元素上的 `href` 屬性（Blazor Server）。</span><span class="sxs-lookup"><span data-stu-id="43dfd-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="43dfd-206">導覽至指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="43dfd-206">Navigates to the specified URI.</span></span> <span data-ttu-id="43dfd-207">如果 `forceLoad` 是 `true`：</span><span class="sxs-lookup"><span data-stu-id="43dfd-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="43dfd-208">已略過用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="43dfd-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="43dfd-209">瀏覽器會強制從伺服器載入新頁面，無論 URI 是否通常由用戶端路由器處理。</span><span class="sxs-lookup"><span data-stu-id="43dfd-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="43dfd-210">導覽位置變更時引發的事件。</span><span class="sxs-lookup"><span data-stu-id="43dfd-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="43dfd-211">將相對 URI 轉換為絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="43dfd-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="43dfd-212">假設基底 URI （例如先前由 `GetBaseUri` 所傳回的 URI），會將絕對 URI 轉換成相對於基底 URI 前置詞的 URI。</span><span class="sxs-lookup"><span data-stu-id="43dfd-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="43dfd-213">下列元件會在選取按鈕時，流覽至應用程式的 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="43dfd-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
