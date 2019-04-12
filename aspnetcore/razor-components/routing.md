---
title: 路由的 razor 元件
author: guardrex
description: 了解如何在應用程式及有關 NavLink 元件，將要求路由傳送。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515427"
---
# <a name="razor-components-routing"></a><span data-ttu-id="6c773-103">路由的 razor 元件</span><span class="sxs-lookup"><span data-stu-id="6c773-103">Razor Components routing</span></span>

<span data-ttu-id="6c773-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6c773-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6c773-105">了解如何在應用程式及有關 NavLink 元件，將要求路由傳送。</span><span class="sxs-lookup"><span data-stu-id="6c773-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="6c773-106">ASP.NET Core 端點路由整合</span><span class="sxs-lookup"><span data-stu-id="6c773-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="6c773-107">Razor 元件都已整合至[ASP.NET Core 路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="6c773-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="6c773-108">將 ASP.NET Core 應用程式設定為接受連入連線使用的互動式 Razor 元件`MapComponentHub<TComponent>`在`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="6c773-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> `MapComponentHub` <span data-ttu-id="6c773-109">指定的根元件`App`應該比對的選取器的 DOM 項目中呈現`app`:</span><span class="sxs-lookup"><span data-stu-id="6c773-109">specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="6c773-110">路由範本</span><span class="sxs-lookup"><span data-stu-id="6c773-110">Route templates</span></span>

<span data-ttu-id="6c773-111">`<Router>`元件可讓路由和路由範本會提供給每個可存取的元件。</span><span class="sxs-lookup"><span data-stu-id="6c773-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="6c773-112">`<Router>`元件會出現在*Components/App.razor*檔案：</span><span class="sxs-lookup"><span data-stu-id="6c773-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="6c773-113">當 *.razor*或 *.cshtml*檔案`@page`編譯指示詞，指定產生的類別<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本。</span><span class="sxs-lookup"><span data-stu-id="6c773-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="6c773-114">在執行階段，路由器會尋找使用的元件類別`RouteAttribute`並呈現任何元件有符合所要求的 URL 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="6c773-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="6c773-115">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="6c773-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="6c773-116">下列元件會回應要求`/BlazorRoute`和`/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="6c773-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` <span data-ttu-id="6c773-117">支援的設定，轉譯所要求的路由時的後援元件仍未解析的。</span><span class="sxs-lookup"><span data-stu-id="6c773-117">supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="6c773-118">藉由設定啟用此加入案例`FallbackComponent`後援的元件類別的型別參數。</span><span class="sxs-lookup"><span data-stu-id="6c773-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="6c773-119">下列範例會設定中所定義的元件*Pages/MyFallbackRazorComponent.cshtml*做為後援的元件，如`<Router>`:</span><span class="sxs-lookup"><span data-stu-id="6c773-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="6c773-120">若要正確地產生路由，應用程式必須包含`<base>`標記中的其*wwwroot/index.html*檔案中指定的應用程式基底路徑`href`屬性 (`<base href="/">`)。</span><span class="sxs-lookup"><span data-stu-id="6c773-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="6c773-121">如需詳細資訊，請參閱<xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="6c773-121">For more information, see <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="6c773-122">路由參數</span><span class="sxs-lookup"><span data-stu-id="6c773-122">Route parameters</span></span>

<span data-ttu-id="6c773-123">路由器會使用路由參數，來填入對應的元件參數具有相同的名稱 （不區分大小寫）：</span><span class="sxs-lookup"><span data-stu-id="6c773-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="6c773-124">選擇性參數不支援，因此兩個`@page`指示詞會套用在上述範例中。</span><span class="sxs-lookup"><span data-stu-id="6c773-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="6c773-125">第一個允許瀏覽至不含參數的元件。</span><span class="sxs-lookup"><span data-stu-id="6c773-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="6c773-126">第二個`@page`指示詞會採用`{text}`路由參數，並將值指派給`Text`屬性。</span><span class="sxs-lookup"><span data-stu-id="6c773-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="6c773-127">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="6c773-127">Route constraints</span></span>

<span data-ttu-id="6c773-128">路由條件約束會強制執行比對到元件的路由區段的型別。</span><span class="sxs-lookup"><span data-stu-id="6c773-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="6c773-129">在下列範例中，使用者元件的路由才會相符：</span><span class="sxs-lookup"><span data-stu-id="6c773-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="6c773-130">`Id`路由區段會出現在要求 URL。</span><span class="sxs-lookup"><span data-stu-id="6c773-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="6c773-131">`Id`區段是一個整數 (`int`)。</span><span class="sxs-lookup"><span data-stu-id="6c773-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="6c773-132">使用下表所示的路由條件約束。</span><span class="sxs-lookup"><span data-stu-id="6c773-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="6c773-133">符合文化特性而異的路由條件約束，請參閱下方的資料表，如需詳細資訊的警告。</span><span class="sxs-lookup"><span data-stu-id="6c773-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="6c773-134">條件約束</span><span class="sxs-lookup"><span data-stu-id="6c773-134">Constraint</span></span> | <span data-ttu-id="6c773-135">範例</span><span class="sxs-lookup"><span data-stu-id="6c773-135">Example</span></span>           | <span data-ttu-id="6c773-136">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="6c773-136">Example Matches</span></span>                                                                  | <span data-ttu-id="6c773-137">非變異值</span><span class="sxs-lookup"><span data-stu-id="6c773-137">Invariant</span></span><br><span data-ttu-id="6c773-138">文化特性</span><span class="sxs-lookup"><span data-stu-id="6c773-138">culture</span></span><br><span data-ttu-id="6c773-139">比對</span><span class="sxs-lookup"><span data-stu-id="6c773-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`<span data-ttu-id="6c773-140">,</span><span class="sxs-lookup"><span data-stu-id="6c773-140">,</span></span> `FALSE`                                                                  | <span data-ttu-id="6c773-141">否</span><span class="sxs-lookup"><span data-stu-id="6c773-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`<span data-ttu-id="6c773-142">,</span><span class="sxs-lookup"><span data-stu-id="6c773-142">,</span></span> `2016-12-31 7:32pm`                                                | <span data-ttu-id="6c773-143">是</span><span class="sxs-lookup"><span data-stu-id="6c773-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | `49.99`<span data-ttu-id="6c773-144">,</span><span class="sxs-lookup"><span data-stu-id="6c773-144">,</span></span> `-1,000.01`                                                             | <span data-ttu-id="6c773-145">是</span><span class="sxs-lookup"><span data-stu-id="6c773-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | `1.234`<span data-ttu-id="6c773-146">,</span><span class="sxs-lookup"><span data-stu-id="6c773-146">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="6c773-147">是</span><span class="sxs-lookup"><span data-stu-id="6c773-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | `1.234`<span data-ttu-id="6c773-148">,</span><span class="sxs-lookup"><span data-stu-id="6c773-148">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="6c773-149">是</span><span class="sxs-lookup"><span data-stu-id="6c773-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`<span data-ttu-id="6c773-150">,</span><span class="sxs-lookup"><span data-stu-id="6c773-150">,</span></span> `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | <span data-ttu-id="6c773-151">否</span><span class="sxs-lookup"><span data-stu-id="6c773-151">No</span></span>                               |
| `int`      | `{id:int}`        | `123456789`<span data-ttu-id="6c773-152">,</span><span class="sxs-lookup"><span data-stu-id="6c773-152">,</span></span> `-123456789`                                                        | <span data-ttu-id="6c773-153">是</span><span class="sxs-lookup"><span data-stu-id="6c773-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | `123456789`<span data-ttu-id="6c773-154">,</span><span class="sxs-lookup"><span data-stu-id="6c773-154">,</span></span> `-123456789`                                                        | <span data-ttu-id="6c773-155">是</span><span class="sxs-lookup"><span data-stu-id="6c773-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="6c773-156">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="6c773-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="6c773-157">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="6c773-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="6c773-158">NavLink 元件</span><span class="sxs-lookup"><span data-stu-id="6c773-158">NavLink component</span></span>

<span data-ttu-id="6c773-159">使用 NavLink 元件取代 HTML`<a>`時建立導覽連結的項目。</span><span class="sxs-lookup"><span data-stu-id="6c773-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="6c773-160">NavLink 元件的行為類似`<a>`項目，但它會切換`active`CSS 類別根據其`href`符合目前的 URL。</span><span class="sxs-lookup"><span data-stu-id="6c773-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="6c773-161">`active`類別可協助使用者了解哪一頁是使用中的頁面之間導覽連結顯示。</span><span class="sxs-lookup"><span data-stu-id="6c773-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="6c773-162">下列 NavMenu 元件會建立[Bootstrap](https://getbootstrap.com/docs/)導覽列中，示範如何使用 NavLink 元件：</span><span class="sxs-lookup"><span data-stu-id="6c773-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="6c773-163">有兩個`NavLinkMatch`選項：</span><span class="sxs-lookup"><span data-stu-id="6c773-163">There are two `NavLinkMatch` options:</span></span>

* `NavLinkMatch.All` <span data-ttu-id="6c773-164">&ndash; 指定 NavLink 應該是作用中時，它會比對整個目前的 URL。</span><span class="sxs-lookup"><span data-stu-id="6c773-164">&ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* `NavLinkMatch.Prefix` <span data-ttu-id="6c773-165">&ndash; 指定 NavLink 應該作用中，當它符合目前 URL 之任何前置詞。</span><span class="sxs-lookup"><span data-stu-id="6c773-165">&ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="6c773-166">在上述範例中，首頁 NavLink (`href=""`) 比對所有的 Url，並一律會收到`active`CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="6c773-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="6c773-167">只會收到第二個 NavLink`active`當使用者造訪 Blazor 路由元件類別 (`href="BlazorRoute"`)。</span><span class="sxs-lookup"><span data-stu-id="6c773-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
