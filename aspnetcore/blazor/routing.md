---
title: ASP.NET Core Blazor 路由
author: guardrex
description: 瞭解如何在應用程式中路由傳送要求, 以及關於 NavLink 元件。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/routing
ms.openlocfilehash: 197b1a91b3540d21639c3ee775b2c490da7b23fe
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030395"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="e7aa8-103">ASP.NET Core Blazor 路由</span><span class="sxs-lookup"><span data-stu-id="e7aa8-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="e7aa8-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e7aa8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e7aa8-105">瞭解如何路由傳送要求, 以及如何使用`NavLink`元件在 Blazor apps 中建立導覽連結。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="e7aa8-106">ASP.NET Core 端點路由整合</span><span class="sxs-lookup"><span data-stu-id="e7aa8-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="e7aa8-107">Blazor 伺服器端會整合到[ASP.NET Core 端點路由](xref:fundamentals/routing)中。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="e7aa8-108">ASP.NET Core 應用程式已設定為接受`MapBlazorHub`中`Startup.Configure`的互動式元件連入連線:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="e7aa8-109">路由範本</span><span class="sxs-lookup"><span data-stu-id="e7aa8-109">Route templates</span></span>

<span data-ttu-id="e7aa8-110">此`Router`元件會啟用路由, 並將路由範本提供給每個可存取的元件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="e7aa8-111">此`Router`元件會出現在*應用程式 razor*檔案中:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="e7aa8-112">在 Blazor 伺服器端應用程式中:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="e7aa8-113">在 Blazor 用戶端應用程式中:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="e7aa8-114">編譯含有 `@page`指示詞的 razor 檔案時, 會提供<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本的所產生類別。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="e7aa8-115">在執行時間, 路由器會尋找具有`RouteAttribute`的元件類別, 並使用符合所要求 URL 的路由範本來呈現元件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="e7aa8-116">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="e7aa8-117">下列元件會回應和`/BlazorRoute` `/DifferentBlazorRoute`的要求:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="e7aa8-118">若要正確產生路由, 應用程式必須在`<base>`其*wwwroot/index.html*檔 (Blazor 用戶端) 或*Pages/_Host*檔 (Blazor 伺服器端) 中包含標記, 其具有`href`屬性中指定的應用程式基底路徑 (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="e7aa8-118">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="e7aa8-119">如需詳細資訊，請參閱 <xref:host-and-deploy/blazor/client-side#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-119">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="e7aa8-120">在找不到內容時提供自訂內容</span><span class="sxs-lookup"><span data-stu-id="e7aa8-120">Provide custom content when content isn't found</span></span>

<span data-ttu-id="e7aa8-121">如果`Router`找不到所要求路由的內容, 此元件可讓應用程式指定自訂內容。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-121">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="e7aa8-122">在*應用程式 razor*檔案中, 于`<NotFoundContent>` `Router`元件的元素中設定自訂內容:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-122">In the *App.razor* file, set custom content in the `<NotFoundContent>` element of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

<span data-ttu-id="e7aa8-123">的內容`<NotFoundContent>`可以包含任意專案, 例如其他互動式元件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-123">The content of `<NotFoundContent>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="e7aa8-124">路由參數</span><span class="sxs-lookup"><span data-stu-id="e7aa8-124">Route parameters</span></span>

<span data-ttu-id="e7aa8-125">路由器會使用路由參數來填入具有相同名稱的對應元件參數 (不區分大小寫):</span><span class="sxs-lookup"><span data-stu-id="e7aa8-125">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="e7aa8-126">ASP.NET Core 3.0 Preview 中的 Blazor 應用程式不支援選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-126">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="e7aa8-127">在`@page`上述範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-127">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="e7aa8-128">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-128">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="e7aa8-129">第二`@page`個指示詞`{text}`會採用 route 參數, 並將值`Text`指派給屬性。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-129">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="e7aa8-130">路由條件約束</span><span class="sxs-lookup"><span data-stu-id="e7aa8-130">Route constraints</span></span>

<span data-ttu-id="e7aa8-131">路由條件約束會將路由區段上的類型比對強制套用至元件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-131">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="e7aa8-132">在下列範例中, 對`Users`元件的路由只會符合下列條件:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-132">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="e7aa8-133">要求 URL 上有路由區段。`Id`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-133">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="e7aa8-134">區段是一個整數 (`int`)。 `Id`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-134">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="e7aa8-135">下表所示的路由條件約束可供使用。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-135">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="e7aa8-136">若為符合不因文化特性而異的路由條件約束, 請參閱表格下方的警告以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-136">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="e7aa8-137">條件約束</span><span class="sxs-lookup"><span data-stu-id="e7aa8-137">Constraint</span></span> | <span data-ttu-id="e7aa8-138">範例</span><span class="sxs-lookup"><span data-stu-id="e7aa8-138">Example</span></span>           | <span data-ttu-id="e7aa8-139">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="e7aa8-139">Example Matches</span></span>                                                                  | <span data-ttu-id="e7aa8-140">非變異值</span><span class="sxs-lookup"><span data-stu-id="e7aa8-140">Invariant</span></span><br><span data-ttu-id="e7aa8-141">文化特性</span><span class="sxs-lookup"><span data-stu-id="e7aa8-141">culture</span></span><br><span data-ttu-id="e7aa8-142">比對</span><span class="sxs-lookup"><span data-stu-id="e7aa8-142">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="e7aa8-143">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-143">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="e7aa8-144">否</span><span class="sxs-lookup"><span data-stu-id="e7aa8-144">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="e7aa8-145">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-145">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="e7aa8-146">是</span><span class="sxs-lookup"><span data-stu-id="e7aa8-146">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="e7aa8-147">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-147">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="e7aa8-148">是</span><span class="sxs-lookup"><span data-stu-id="e7aa8-148">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="e7aa8-149">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-149">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="e7aa8-150">是</span><span class="sxs-lookup"><span data-stu-id="e7aa8-150">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="e7aa8-151">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-151">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="e7aa8-152">是</span><span class="sxs-lookup"><span data-stu-id="e7aa8-152">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="e7aa8-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="e7aa8-154">否</span><span class="sxs-lookup"><span data-stu-id="e7aa8-154">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="e7aa8-155">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-155">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="e7aa8-156">是</span><span class="sxs-lookup"><span data-stu-id="e7aa8-156">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="e7aa8-157">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-157">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="e7aa8-158">是</span><span class="sxs-lookup"><span data-stu-id="e7aa8-158">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="e7aa8-159">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-159">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="e7aa8-160">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-160">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="e7aa8-161">NavLink 元件</span><span class="sxs-lookup"><span data-stu-id="e7aa8-161">NavLink component</span></span>

<span data-ttu-id="e7aa8-162">建立導覽連結時, 請使用`<a>`元件來取代HTML超連結元素()。`NavLink`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-162">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="e7aa8-163">元件`NavLink`的行為`<a>`類似元素`active` , 但它會根據其`href`是否符合目前的 URL 來切換 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-163">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="e7aa8-164">`active`類別可協助使用者瞭解在顯示的導覽連結中, 哪個頁面是使用中的頁面。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-164">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="e7aa8-165">下列`NavMenu`元件會建立[啟動](https://getbootstrap.com/docs/)程式導覽列, 示範如何使用`NavLink`元件:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-165">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="e7aa8-166">有兩個`NavLinkMatch`選項可`Match`供您指派給`<NavLink>`元素的屬性:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-166">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="e7aa8-167">`NavLinkMatch.All`當符合整個目前的 URL 時, 就會作用中`NavLink`。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="e7aa8-167">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="e7aa8-168">`NavLinkMatch.Prefix`(*預設值*)當符合目前 URL 的任何前置詞時, 就會作用中`NavLink`。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="e7aa8-168">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="e7aa8-169">在上述`NavLink`範例中, home `href=""`會符合 home `active` URL, 而且只會在應用程式的預設基底路徑 URL (例如, `https://localhost:5001/`) 接收 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-169">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="e7aa8-170">第二`NavLink`個會`active`在使用者`MyComponent`造訪具有前置詞的任何 URL 時收到類別 (例如`https://localhost:5001/MyComponent` , `https://localhost:5001/MyComponent/AnotherSegment`和)。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-170">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="e7aa8-171">其他`NavLink`元件屬性會傳遞至呈現的錨點標記。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-171">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="e7aa8-172">在下列範例中, `NavLink`元件`target`包含屬性:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-172">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="e7aa8-173">會轉譯下列 HTML 標籤:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-173">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="e7aa8-174">URI 和流覽狀態協助程式</span><span class="sxs-lookup"><span data-stu-id="e7aa8-174">URI and navigation state helpers</span></span>

<span data-ttu-id="e7aa8-175">用`Microsoft.AspNetCore.Components.IUriHelper`來處理常式代碼中C#的 uri 和導覽。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-175">Use `Microsoft.AspNetCore.Components.IUriHelper` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="e7aa8-176">`IUriHelper`提供下表所示的事件和方法。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-176">`IUriHelper` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="e7aa8-177">成員</span><span class="sxs-lookup"><span data-stu-id="e7aa8-177">Member</span></span> | <span data-ttu-id="e7aa8-178">說明</span><span class="sxs-lookup"><span data-stu-id="e7aa8-178">Description</span></span> |
| ------ | ----------- |
| `GetAbsoluteUri` | <span data-ttu-id="e7aa8-179">取得目前的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-179">Gets the current absolute URI.</span></span> |
| `GetBaseUri` | <span data-ttu-id="e7aa8-180">取得可在相對 URI 路徑前面加上的基底 URI (含尾端斜線), 以產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-180">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="e7aa8-181">`href` `<base>`通常會對應至wwwroot/index.html(Blazor用戶端)或Pages/_Host(Blazor伺服器端)中檔元素上的屬性。`GetBaseUri`</span><span class="sxs-lookup"><span data-stu-id="e7aa8-181">Typically, `GetBaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="e7aa8-182">導覽至指定的 URI。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-182">Navigates to the specified URI.</span></span> <span data-ttu-id="e7aa8-183">如果`forceLoad`為`true`:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-183">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="e7aa8-184">已略過用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-184">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="e7aa8-185">瀏覽器會強制從伺服器載入新頁面, 無論 URI 是否通常由用戶端路由器處理。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-185">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `OnLocationChanged` | <span data-ttu-id="e7aa8-186">導覽位置變更時引發的事件。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-186">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="e7aa8-187">將相對 URI 轉換為絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-187">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="e7aa8-188">假設基底 uri (例如先前傳回的`GetBaseUri`uri), 會將絕對 uri 轉換成相對於基底 uri 前置詞的 uri。</span><span class="sxs-lookup"><span data-stu-id="e7aa8-188">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="e7aa8-189">下列元件會在選取按鈕時, `Counter`流覽至應用程式的元件:</span><span class="sxs-lookup"><span data-stu-id="e7aa8-189">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```
