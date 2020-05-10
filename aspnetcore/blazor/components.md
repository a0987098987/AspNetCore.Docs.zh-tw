---
title: 建立和使用 ASP.NET Core Razor元件
author: guardrex
description: 瞭解如何建立和使用Razor元件，包括如何系結至資料、處理事件，以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/21/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components
ms.openlocfilehash: de8696488292dfc409c5ed83ea0b88a54a001696
ms.sourcegitcommit: 6c7a149168d2c4d747c36de210bfab3abd60809a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "83003063"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="a6a2d-103">建立和使用 ASP.NET Core Razor元件</span><span class="sxs-lookup"><span data-stu-id="a6a2d-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="a6a2d-104">By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Tobias Bartsch](https://www.aveo-solutions.com/)</span><span class="sxs-lookup"><span data-stu-id="a6a2d-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Tobias Bartsch](https://www.aveo-solutions.com/)</span></span>

<span data-ttu-id="a6a2d-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a6a2d-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="a6a2d-106">應用程式是使用*元件*所建立。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-106"> apps are built using *components*.</span></span> <span data-ttu-id="a6a2d-107">「元件」（component）是一種獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="a6a2d-108">元件包含 HTML 標籤，以及插入資料或回應 UI 事件所需的處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="a6a2d-109">元件具有彈性且輕量。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="a6a2d-110">它們可以在專案之間進行嵌套、重複使用及共用。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="a6a2d-111">元件類別</span><span class="sxs-lookup"><span data-stu-id="a6a2d-111">Component classes</span></span>

<span data-ttu-id="a6a2d-112">元件會使用 c [Razor](xref:mvc/views/razor) # 和 HTML 標籤的組合，在元件檔（*razor*）中執行。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="a6a2d-113">中Blazor的元件正式地稱為「 \* Razor元件\*」（component）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="a6a2d-114">元件的名稱必須以大寫字元開頭。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="a6a2d-115">例如， *MyCoolComponent*有效，而*MyCoolComponent*則無效。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="a6a2d-116">元件的 UI 是使用 HTML 定義的。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="a6a2d-117">動態轉譯邏輯（例如，迴圈、條件、運算式）是使用名[Razor](xref:mvc/views/razor)為的內嵌 c # 語法加入。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="a6a2d-118">編譯應用程式時，會將 HTML 標籤和 c # 轉譯邏輯轉換成元件類別。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="a6a2d-119">產生的類別名稱與檔案的名稱相符。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="a6a2d-120">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="a6a2d-121">在`@code`區塊中，會使用事件處理或定義其他元件邏輯的方法來指定元件狀態（屬性、欄位）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="a6a2d-122">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="a6a2d-123">元件成員可以使用開頭為的 c # 運算式，做為元件轉譯邏輯的一部分`@`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="a6a2d-124">例如，c # 欄位是藉由在功能變數名稱`@`前面加上的方式來呈現。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="a6a2d-125">下列範例會評估並呈現：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="a6a2d-126">`headingFontStyle`至的 CSS 屬性值`font-style`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-126">`headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="a6a2d-127">`headingText`至`<h1>`元素的內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-127">`headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@headingFontStyle">@headingText</h1>

@code {
    private string headingFontStyle = "italic";
    private string headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="a6a2d-128">一開始呈現元件之後，元件會重新產生其轉譯樹狀結構，以回應事件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="a6a2d-129">然後比較新的轉譯樹狀結構與上一個，並將任何修改套用至瀏覽器的檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="a6a2d-130">元件是一般的 c # 類別，可以放在專案內的任何位置。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="a6a2d-131">產生網頁的元件通常會位於*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="a6a2d-132">非頁面元件通常會放在*共用*資料夾中，或加入至專案的自訂資料夾中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="a6a2d-133">一般而言，元件的命名空間是從應用程式的根命名空間和元件在應用程式內的位置（資料夾）衍生而來。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="a6a2d-134">如果應用程式的根命名空間`BlazorApp`為， `Counter`且元件位於*Pages*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="a6a2d-135">`Counter`元件的命名空間是`BlazorApp.Pages`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="a6a2d-136">元件的完整類型名稱為`BlazorApp.Pages.Counter`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="a6a2d-137">如需詳細資訊，請參閱匯[入元件](#import-components)一節。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="a6a2d-138">若要使用自訂資料夾，請將自訂資料夾的命名空間新增至父元件或應用程式的 *_Imports razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="a6a2d-139">例如，下列命名空間會在應用程式的*Components*根命名空間為`BlazorApp`時，讓元件資料夾中的元件可供使用：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="a6a2d-140">靜態資產</span><span class="sxs-lookup"><span data-stu-id="a6a2d-140">Static assets</span></span>

Blazor<span data-ttu-id="a6a2d-141">遵循 ASP.NET Core 應用程式在專案的[web 根目錄（wwwroot）資料夾](xref:fundamentals/index#web-root)下放置靜態資產的慣例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="a6a2d-142">使用基底相對路徑（`/`）來參考靜態資產的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="a6a2d-143">在下列範例中，*標誌 .png*實際上位於 *{PROJECT ROOT}/wwwroot/images*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

Razor<span data-ttu-id="a6a2d-144">元件**不**支援波形符-斜線標記法`~/`（）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-144"> components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="a6a2d-145">如需設定應用程式基底路徑的詳細資訊<xref:host-and-deploy/blazor/index#app-base-path>，請參閱。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="a6a2d-146">元件中不支援標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a6a2d-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="a6a2d-147">元件（*razor*檔案） Razor中不支援[標記](xref:mvc/views/tag-helpers/intro)協助程式。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="a6a2d-148">若要在中Blazor提供標籤協助程式的功能，請建立元件，其功能與標記協助程式相同，並改用元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="a6a2d-149">使用元件</span><span class="sxs-lookup"><span data-stu-id="a6a2d-149">Use components</span></span>

<span data-ttu-id="a6a2d-150">元件可以包含其他元件，方法是使用 HTML 專案語法來宣告它們。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="a6a2d-151">使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="a6a2d-152">在*Index*中的下列標記會呈現`HeadingComponent`實例：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-152">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="a6a2d-153">*Components/HeadingComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-153">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="a6a2d-154">如果元件包含的 HTML 專案具有大寫的第一個字母，但不符合元件名稱，則會發出警告，指出該元素有未預期的名稱。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-154">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="a6a2d-155">`@using`為元件的命名空間加入指示詞可讓元件可用，這會解析警告。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-155">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="a6a2d-156">路由</span><span class="sxs-lookup"><span data-stu-id="a6a2d-156">Routing</span></span>

<span data-ttu-id="a6a2d-157">中Blazor的路由會藉由提供路由範本給應用程式中每個可存取的元件來達成。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-157">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="a6a2d-158">Razor當編譯具有`@page`指示詞的檔案時，系統會<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本給產生的類別。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-158">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="a6a2d-159">在執行時間，路由器會尋找具有的`RouteAttribute`元件類別，並轉譯哪個元件具有符合所要求 URL 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-159">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="a6a2d-160">如需詳細資訊，請參閱<xref:blazor/routing>。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-160">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="a6a2d-161">參數</span><span class="sxs-lookup"><span data-stu-id="a6a2d-161">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="a6a2d-162">路由參數</span><span class="sxs-lookup"><span data-stu-id="a6a2d-162">Route parameters</span></span>

<span data-ttu-id="a6a2d-163">元件可以從指示詞中`@page`提供的路由範本接收路由參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-163">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="a6a2d-164">路由器會使用路由參數來填入對應的元件參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-164">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="a6a2d-165">*Pages/RouteParameter. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-165">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="a6a2d-166">不支援選擇性參數，因此在`@page`上述範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-166">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="a6a2d-167">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-167">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a6a2d-168">第二`@page`個指示詞`{text}`會接收路由參數，並將值`Text`指派給屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-168">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="a6a2d-169">*Catch-all*參數語法（`*`/`**`），它會跨多個資料夾界限來捕捉路徑**not** ，但Razor元件（*razor*）並不支援。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-169">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="a6a2d-170">元件參數</span><span class="sxs-lookup"><span data-stu-id="a6a2d-170">Component parameters</span></span>

<span data-ttu-id="a6a2d-171">元件可以具有*元件參數*，其定義方式是在元件類別上使用具有`[Parameter]`屬性的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-171">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="a6a2d-172">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-172">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="a6a2d-173">*Components/ChildComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-173">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="a6a2d-174">在範例應用程式的下列範例中，會`ParentComponent`設定的`Title`屬性值`ChildComponent`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-174">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="a6a2d-175">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-175">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

> [!WARNING]
> <span data-ttu-id="a6a2d-176">請勿建立可寫入其本身*元件參數*的元件，請改用私用欄位。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-176">Don't create components that write to their own *component parameters*, use a private field instead.</span></span> <span data-ttu-id="a6a2d-177">如需詳細資訊，請參閱[不要建立可寫入自己的參數屬性的元件](#dont-create-components-that-write-to-their-own-parameter-properties)一節。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-177">For more information, see the [Don't create components that write to their own parameter properties](#dont-create-components-that-write-to-their-own-parameter-properties) section.</span></span>

## <a name="child-content"></a><span data-ttu-id="a6a2d-178">子內容</span><span class="sxs-lookup"><span data-stu-id="a6a2d-178">Child content</span></span>

<span data-ttu-id="a6a2d-179">元件可以設定另一個元件的內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-179">Components can set the content of another component.</span></span> <span data-ttu-id="a6a2d-180">指派元件會在指定接收元件的標記之間提供內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-180">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="a6a2d-181">在下列範例中， `ChildComponent`有一個`ChildContent`代表的屬性`RenderFragment`，代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-181">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="a6a2d-182">的值`ChildContent`位於元件的標記中，應在其中呈現內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-182">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="a6a2d-183">的值`ChildContent`會從父元件接收，並在啟動載入面板的`panel-body`內轉譯。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-183">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="a6a2d-184">*Components/ChildComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-184">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="a6a2d-185">接收`RenderFragment`內容的屬性必須依照慣例命名`ChildContent` 。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-185">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="a6a2d-186">範例`ParentComponent`應用程式中的會將內容放在卷`ChildComponent`標內`<ChildComponent>` ，藉以提供呈現的內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-186">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="a6a2d-187">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-187">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="a6a2d-188">屬性展開和任意參數</span><span class="sxs-lookup"><span data-stu-id="a6a2d-188">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="a6a2d-189">除了元件的宣告參數之外，元件還可以捕捉和轉譯其他屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-189">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="a6a2d-190">您可以在字典中捕捉其他屬性，然後*splatted*在使用[`@attributes`](xref:mvc/views/razor#attributes) Razor指示詞轉譯元件時，splatted 至元素。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-190">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="a6a2d-191">當定義的元件會產生支援各種自訂的標記專案時，這個案例就很有用。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-191">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="a6a2d-192">例如，針對`<input>`支援許多參數的，分別定義屬性可能會很繁瑣。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-192">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="a6a2d-193">在下列範例中，第一個`<input>`元素（`id="useIndividualParams"`）會使用個別的元件參數，而`<input>`第二`id="useAttributesDict"`個元素（）則使用屬性展開：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-193">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="a6a2d-194">參數的類型必須使用字串索引`IEnumerable<KeyValuePair<string, object>>`鍵來執行。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-194">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="a6a2d-195">在`IReadOnlyDictionary<string, object>`此案例中，使用也是一個選項。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-195">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="a6a2d-196">使用這`<input>`兩種方法的轉譯元素都相同：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-196">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="a6a2d-197">若要接受任意屬性，請使用`[Parameter]` `CaptureUnmatchedValues`屬性設定為的屬性來`true`定義元件參數：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-197">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="a6a2d-198">上`CaptureUnmatchedValues` `[Parameter]`的屬性允許參數比對與任何其他參數不相符的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-198">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="a6a2d-199">元件只能定義具有`CaptureUnmatchedValues`的單一參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-199">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="a6a2d-200">搭配使用的屬性類型`CaptureUnmatchedValues`必須可從`Dictionary<string, object>`使用字串索引鍵來指派。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-200">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="a6a2d-201">`IEnumerable<KeyValuePair<string, object>>`或`IReadOnlyDictionary<string, object>`也是此案例中的選項。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-201">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="a6a2d-202">`@attributes`相對於元素屬性位置的位置很重要。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-202">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="a6a2d-203">當`@attributes`在專案上 splatted 時，會從右至左（最後一個）處理屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-203">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="a6a2d-204">請考慮使用`Child`元件的下列元件範例：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-204">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="a6a2d-205">*ParentComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-205">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="a6a2d-206">*ChildComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-206">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="a6a2d-207">`Child`元件的`extra`屬性會設定為的右邊`@attributes`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-207">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="a6a2d-208">當`Parent`您透過其他`<div>`屬性`extra="5"`傳遞時，元件的會包含，因為屬性是由右至左（最後一個）來處理：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-208">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="a6a2d-209">在下列範例中`extra` ，和`@attributes`的順序會在`Child`元件的`<div>`中反轉：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-209">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="a6a2d-210">*ParentComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-210">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="a6a2d-211">*ChildComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-211">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="a6a2d-212">在元件中轉譯的會在透過其他屬性傳遞時包含`extra="10"` `<div>` `Parent`</span><span class="sxs-lookup"><span data-stu-id="a6a2d-212">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="a6a2d-213">捕獲元件的參考</span><span class="sxs-lookup"><span data-stu-id="a6a2d-213">Capture references to components</span></span>

<span data-ttu-id="a6a2d-214">元件參考提供參考元件實例的方法，讓您可以對該實例發出命令，例如`Show`或。 `Reset`</span><span class="sxs-lookup"><span data-stu-id="a6a2d-214">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="a6a2d-215">若要捕捉元件參考：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-215">To capture a component reference:</span></span>

* <span data-ttu-id="a6a2d-216">將[`@ref`](xref:mvc/views/razor#ref)屬性加入至子元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-216">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="a6a2d-217">定義與子元件類型相同的欄位。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-217">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="a6a2d-218">當元件呈現時， `loginDialog`欄位會填入`MyLoginDialog`子元件實例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-218">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="a6a2d-219">接著，您可以在元件實例上叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-219">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6a2d-220">只有`loginDialog`在轉譯元件之後才會填入變數，而且其輸出會包含`MyLoginDialog`元素。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-220">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="a6a2d-221">直到該點為止，沒有任何可參考的內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-221">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="a6a2d-222">若要在元件完成呈現之後操作元件參考，請使用[OnAfterRenderAsync 或 OnAfterRender 方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-222">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="a6a2d-223">若要參考迴圈中的元件，請參閱[捕捉多個類似子元件的參考（dotnet/aspnetcore #13358）](https://github.com/dotnet/aspnetcore/issues/13358)。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-223">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="a6a2d-224">雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)，但它並不是 JavaScript interop 功能。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-224">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="a6a2d-225">元件參考不會傳遞至 JavaScript&mdash;程式碼，而只會在 .net 程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-225">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="a6a2d-226">請勿**使用元件**參考來改變子元件的狀態。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-226">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="a6a2d-227">請改用一般宣告式參數，將資料傳遞至子元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-227">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="a6a2d-228">使用一般宣告式參數會導致子元件自動 rerender 正確的時間。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-228">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="a6a2d-229">在外部叫用元件方法來更新狀態</span><span class="sxs-lookup"><span data-stu-id="a6a2d-229">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="a6a2d-230">會使用`SynchronizationContext`來強制執行單一邏輯執行緒。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-230"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="a6a2d-231">Blazor會在這個`SynchronizationContext`上執行元件的[生命週期方法](xref:blazor/lifecycle)和所引發的任何事件回呼。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-231">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="a6a2d-232">在事件中，必須根據外來事件（例如計時器或其他通知）更新元件，請使用會分派至`InvokeAsync` Blazor的`SynchronizationContext`方法。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-232">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="a6a2d-233">例如，假設有一個通知程式*服務*可通知任何處于已更新狀態的「接聽」元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-233">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="a6a2d-234">將註冊`NotifierService`為 singletion：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-234">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="a6a2d-235">在Blazor WebAssembly 中，于中`Program.Main`註冊服務：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-235">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="a6a2d-236">在Blazor [伺服器] 中，于`Startup.ConfigureServices`註冊服務：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-236">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="a6a2d-237">使用`NotifierService`來更新元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-237">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="a6a2d-238">在上述範例中， `NotifierService`會叫用位於`OnNotify`的之外Blazor的元件`SynchronizationContext`方法。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-238">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="a6a2d-239">`InvokeAsync`用來切換至正確的內容，並將轉譯排入佇列。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-239">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="a6a2d-240">使用\@金鑰來控制元素和元件的保留</span><span class="sxs-lookup"><span data-stu-id="a6a2d-240">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="a6a2d-241">當轉譯專案或元件清單，以及後續變更的專案或元件時， Blazor的比較演算法必須決定哪些先前的專案或元件可以保留，以及模型物件應如何對應至這些專案。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-241">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="a6a2d-242">一般來說，此程式是自動的，可以忽略，但在某些情況下，您可能會想要控制進程。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-242">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="a6a2d-243">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-243">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="a6a2d-244">`People`集合的內容可能會隨著插入、刪除或重新排序的專案而變更。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-244">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="a6a2d-245">當元件 rerenders 時， `<DetailsEditor>`元件可能會變更以接收不同`Details`的參數值。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-245">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="a6a2d-246">這可能會導致比預期更複雜的 rerendering。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-246">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="a6a2d-247">在某些情況下，rerendering 可能會導致可見的行為差異，例如失去元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-247">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="a6a2d-248">您可以使用[`@key`](xref:mvc/views/razor#key)指示詞屬性來控制對應進程。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-248">The mapping process can be controlled with the [`@key`](xref:mvc/views/razor#key) directive attribute.</span></span> <span data-ttu-id="a6a2d-249">`@key`導致比較演算法根據索引鍵的值，保證保留元素或元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-249">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="a6a2d-250">當`People`集合變更時，比較演算法會保留實例和`<DetailsEditor>` `person`實例之間的關聯：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-250">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="a6a2d-251">`Person`如果從`People`清單中刪除，則只會從 UI `<DetailsEditor>`移除對應的實例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-251">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="a6a2d-252">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-252">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a6a2d-253">`Person`如果在清單中的某個位置插入，則會在對應`<DetailsEditor>`的位置插入一個新的實例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-253">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="a6a2d-254">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-254">Other instances are left unchanged.</span></span>
* <span data-ttu-id="a6a2d-255">如果`Person`重新排序專案，則會保留對應`<DetailsEditor>`的實例，並在 UI 中重新排序。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-255">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="a6a2d-256">在某些情況下，使用`@key`可將 rerendering 的複雜性降到最低，並避免 DOM 的具狀態部分可能發生的問題，例如焦點位置。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-256">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a6a2d-257">索引鍵在每個容器元素或元件的本機。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-257">Keys are local to each container element or component.</span></span> <span data-ttu-id="a6a2d-258">金鑰不會在檔之間進行全域比較。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-258">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="a6a2d-259">使用\@金鑰的時機</span><span class="sxs-lookup"><span data-stu-id="a6a2d-259">When to use \@key</span></span>

<span data-ttu-id="a6a2d-260">一般來說，每當轉譯清單（例如`@key` ，在`@foreach`區塊中），而且有適合的值來定義時，就有合理的`@key`使用方式。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-260">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="a6a2d-261">當物件變更時`@key` ，您Blazor也可以使用來防止保留元素或元件子樹：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-261">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="a6a2d-262">如果`@currentPerson`變更， `@key` attribute 指示詞會Blazor強制捨棄整個`<div>`及其下階，並使用新的元素和元件重建 UI 內的子樹。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-262">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="a6a2d-263">如果您需要保證變更時`@currentPerson`不會保留任何 UI 狀態，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-263">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="a6a2d-264">不使用\@金鑰的時機</span><span class="sxs-lookup"><span data-stu-id="a6a2d-264">When not to use \@key</span></span>

<span data-ttu-id="a6a2d-265">與`@key`比較時，會產生效能成本。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-265">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="a6a2d-266">效能成本並不大，但只會`@key`指定控制元素或元件保留規則是否能讓應用程式受益。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-266">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="a6a2d-267">即使`@key`未使用， Blazor也會盡可能保留子項目和元件實例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-267">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="a6a2d-268">使用的唯一優點是`@key`控制模型實例*如何*對應至保留的元件實例，而不是用來選取對應的比較演算法。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-268">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="a6a2d-269">要用於金鑰的\@值</span><span class="sxs-lookup"><span data-stu-id="a6a2d-269">What values to use for \@key</span></span>

<span data-ttu-id="a6a2d-270">一般來說，提供下列其中一種類型的值是合理的`@key`：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-270">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="a6a2d-271">模型物件實例（例如，如先前`Person`範例所示的實例）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-271">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="a6a2d-272">這可確保根據物件參考的相等性進行保留。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-272">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="a6a2d-273">唯一識別碼（例如，、或`int` `string` `Guid`類型的主要索引鍵值）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-273">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="a6a2d-274">請確定用於的值`@key`不會造成衝突。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-274">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="a6a2d-275">如果在相同的父元素中偵測到衝突值Blazor ，則會擲回例外狀況，因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-275">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="a6a2d-276">只使用不同的值，例如物件實例或主鍵值。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-276">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="dont-create-components-that-write-to-their-own-parameter-properties"></a><span data-ttu-id="a6a2d-277">不要建立會寫入自己的參數屬性的元件</span><span class="sxs-lookup"><span data-stu-id="a6a2d-277">Don't create components that write to their own parameter properties</span></span>

<span data-ttu-id="a6a2d-278">在下列情況下，會覆寫參數：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-278">Parameters are overwritten under the following conditions:</span></span>

* <span data-ttu-id="a6a2d-279">子元件的內容會以呈現`RenderFragment`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-279">A child component's content is rendered with a `RenderFragment`.</span></span>
* <span data-ttu-id="a6a2d-280"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>在父元件中呼叫。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-280"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent component.</span></span>

<span data-ttu-id="a6a2d-281">參數會重設，因為呼叫時會<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> rerenders 父元件，並將新的參數值提供給子元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-281">Parameters are reset because the parent component rerenders when <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called and new parameter values are supplied to the child component.</span></span>

<span data-ttu-id="a6a2d-282">請考慮下列`Expander`元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-282">Consider the following `Expander` component that:</span></span>

* <span data-ttu-id="a6a2d-283">呈現子內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-283">Renders child content.</span></span>
* <span data-ttu-id="a6a2d-284">使用元件參數來顯示子內容的切換。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-284">Toggles showing child content with a component parameter.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @Expanded)

    @if (Expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void Toggle()
    {
        Expanded = !Expanded;
    }
}
```

<span data-ttu-id="a6a2d-285">`Expander`元件會新增至可能呼叫`StateHasChanged`的父元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-285">The `Expander` component is added to a parent component that may call `StateHasChanged`:</span></span>

```razor
<Expander Expanded="true">
    <h1>Hello, world!</h1>
</Expander>

<Expander Expanded="true" />

<button @onclick="@(() => StateHasChanged())">
    Call StateHasChanged
</button>
```

<span data-ttu-id="a6a2d-286">一開始， `Expander`元件會在其`Expanded`屬性切換時獨立行為。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-286">Initially, the `Expander` components behave independently when their `Expanded` properties are toggled.</span></span> <span data-ttu-id="a6a2d-287">子元件會如預期般維護其狀態。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-287">The child components maintain their states as expected.</span></span> <span data-ttu-id="a6a2d-288">在`StateHasChanged`父系中呼叫時，第一個`Expanded`子元件的參數會重設回其初始值（`true`）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-288">When `StateHasChanged` is called in the parent, the `Expanded` parameter of the first child component is reset back to its initial value (`true`).</span></span> <span data-ttu-id="a6a2d-289">第二`Expander`個元件`Expanded`的值不會重設，因為第二個元件中不會轉譯任何子內容。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-289">The second `Expander` component's `Expanded` value isn't reset because no child content is rendered in the second component.</span></span>

<span data-ttu-id="a6a2d-290">若要維護上述案例中的狀態，請*private field*使用`Expander`元件中的私用欄位來維護其切換狀態。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-290">To maintain state in the preceding scenario, use a *private field* in the `Expander` component to maintain its toggled state.</span></span>

<span data-ttu-id="a6a2d-291">下列`Expander`元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-291">The following `Expander` component:</span></span>

* <span data-ttu-id="a6a2d-292">接受來自`Expanded`父系的元件參數值。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-292">Accepts the `Expanded` component parameter value from the parent.</span></span>
* <span data-ttu-id="a6a2d-293">將元件參數值指派給[OnInitialized 事件](xref:blazor/lifecycle#component-initialization-methods)中的`expanded`私用*欄位*（）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-293">Assigns the component parameter value to a *private field* (`expanded`) in the [OnInitialized event](xref:blazor/lifecycle#component-initialization-methods).</span></span>
* <span data-ttu-id="a6a2d-294">會使用私用欄位來維護其內部切換狀態。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-294">Uses the private field to maintain its internal toggle state.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @expanded)

    @if (expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private bool expanded;

    protected override void OnInitialized()
    {
        expanded = Expanded;
    }

    private void Toggle()
    {
        expanded = !expanded;
    }
}
```

## <a name="partial-class-support"></a><span data-ttu-id="a6a2d-295">部分類別支援</span><span class="sxs-lookup"><span data-stu-id="a6a2d-295">Partial class support</span></span>

Razor<span data-ttu-id="a6a2d-296">元件是以部分類別的形式產生。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-296"> components are generated as partial classes.</span></span> Razor<span data-ttu-id="a6a2d-297">元件是使用下列其中一種方法來撰寫的：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-297"> components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="a6a2d-298">C # 程式碼定義于[`@code`](xref:mvc/views/razor#code)區塊中，並在Razor單一檔案中使用 HTML 標籤和程式碼。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-298">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="a6a2d-299">範本會使用Razor這種方法來定義其元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-299"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="a6a2d-300">C # 程式碼會放在定義為部分類別的程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-300">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="a6a2d-301">下列範例顯示在從`Counter` `@code` Blazor範本產生的應用程式中具有區塊的預設元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-301">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="a6a2d-302">HTML 標籤、 Razor程式碼和 c # 程式碼位於相同的檔案中：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-302">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="a6a2d-303">*Counter. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-303">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="a6a2d-304">您`Counter`也可以使用具有部分類別的程式碼後置檔案來建立元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-304">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="a6a2d-305">*Counter. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-305">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="a6a2d-306">*Counter.razor.cs*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-306">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

<span data-ttu-id="a6a2d-307">視需要將任何必要的命名空間新增至部分類別檔案。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-307">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="a6a2d-308">元件所使用的Razor一般命名空間包括：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-308">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="a6a2d-309">指定基類</span><span class="sxs-lookup"><span data-stu-id="a6a2d-309">Specify a base class</span></span>

<span data-ttu-id="a6a2d-310">[`@inherits`](xref:mvc/views/razor#inherits)指示詞可以用來指定元件的基類。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-310">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="a6a2d-311">下列範例會顯示元件如何繼承基類， `BlazorRocksBase`以提供元件的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-311">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="a6a2d-312">基類應該衍生自`ComponentBase`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-312">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="a6a2d-313">*Pages/BlazorRocks. razor*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-313">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="a6a2d-314">*BlazorRocksBase.cs*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-314">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a><span data-ttu-id="a6a2d-315">指定屬性</span><span class="sxs-lookup"><span data-stu-id="a6a2d-315">Specify an attribute</span></span>

<span data-ttu-id="a6a2d-316">屬性可以在具有Razor [`@attribute`](xref:mvc/views/razor#attribute)指示詞的元件中指定。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-316">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="a6a2d-317">下列範例會將`[Authorize]`屬性套用至元件類別：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-317">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="a6a2d-318">匯入元件</span><span class="sxs-lookup"><span data-stu-id="a6a2d-318">Import components</span></span>

<span data-ttu-id="a6a2d-319">以Razor撰寫之元件的命名空間是根據（依優先順序排列）：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-319">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="a6a2d-320">[`@namespace`](xref:mvc/views/razor#namespace)檔案（ Razor *razor*）標記中的指定（`@namespace BlazorSample.MyNamespace`）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-320">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="a6a2d-321">專案`RootNamespace`在專案檔中的（`<RootNamespace>BlazorSample</RootNamespace>`）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-321">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="a6a2d-322">從專案檔的檔案名（*.csproj*）取得的專案名稱，以及從專案根目錄到元件的路徑。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-322">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="a6a2d-323">例如，架構會將 *{PROJECT ROOT}/Pages/Index.razor* （*BlazorSample*）解析為命名空間`BlazorSample.Pages`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-323">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="a6a2d-324">元件遵循 c # 名稱系結規則。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-324">Components follow C# name binding rules.</span></span> <span data-ttu-id="a6a2d-325">針對此`Index`範例中的元件，範圍內的元件都是元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-325">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="a6a2d-326">在相同的資料夾中，*頁面*。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-326">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="a6a2d-327">專案根目錄中未明確指定不同命名空間的元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-327">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="a6a2d-328">使用Razor的[`@using`](xref:mvc/views/razor#using)指示詞，在不同的命名空間中定義的元件會帶入範圍中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-328">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="a6a2d-329">如果*BlazorSample/Shared/* 資料夾中有另一個元件`Index.razor` `@using` `NavMenu.razor`，則可以在中使用此元件，並搭配下列語句：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-329">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="a6a2d-330">元件也可以使用其完整名稱來參考，而不需要[`@using`](xref:mvc/views/razor#using)指示詞：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-330">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="a6a2d-331">不`global::`支援該限定性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-331">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="a6a2d-332">不支援使用具有`using`別名的語句來匯`@using Foo = Bar`入元件（例如）。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-332">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="a6a2d-333">不支援部分限定的名稱。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-333">Partially qualified names aren't supported.</span></span> <span data-ttu-id="a6a2d-334">例如， `<Shared.NavMenu></Shared.NavMenu>`不支援`@using BlazorSample`新增和`NavMenu.razor`參考。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-334">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="a6a2d-335">條件式 HTML 元素屬性</span><span class="sxs-lookup"><span data-stu-id="a6a2d-335">Conditional HTML element attributes</span></span>

<span data-ttu-id="a6a2d-336">HTML 專案屬性會根據 .NET 值有條件地呈現。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-336">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="a6a2d-337">如果值為`false`或`null`，則不會呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-337">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="a6a2d-338">如果值為`true`，則會以最小化的方式呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-338">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="a6a2d-339">在下列範例中， `IsCompleted` `checked`會判斷是否呈現在專案的標記中：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-339">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="a6a2d-340">如果`IsCompleted`為`true`，則會將核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-340">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="a6a2d-341">如果`IsCompleted`為`false`，則會將核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-341">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="a6a2d-342">如需詳細資訊，請參閱<xref:mvc/views/razor>。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-342">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="a6a2d-343">當 .NET 類型為時，某些 HTML 屬性（例如，[按下的 aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)）無法正常`bool`運作。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-343">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="a6a2d-344">在這些情況下，請`string`使用型別， `bool`而不是。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-344">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="a6a2d-345">原始 HTML</span><span class="sxs-lookup"><span data-stu-id="a6a2d-345">Raw HTML</span></span>

<span data-ttu-id="a6a2d-346">字串通常會使用 DOM 文位元組點來呈現，這表示它們可能包含的任何標記都會被忽略，並視為常值。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-346">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="a6a2d-347">若要轉譯原始 HTML，請將 HTML 內容包裝`MarkupString`在值中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-347">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="a6a2d-348">此值會剖析為 HTML 或 SVG，並插入 DOM 中。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-348">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="a6a2d-349">轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**，應予以避免！</span><span class="sxs-lookup"><span data-stu-id="a6a2d-349">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="a6a2d-350">下列範例顯示如何使用`MarkupString`類型，將靜態 HTML 內容的區塊新增至元件的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-350">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="a6a2d-351">級聯的值和參數</span><span class="sxs-lookup"><span data-stu-id="a6a2d-351">Cascading values and parameters</span></span>

<span data-ttu-id="a6a2d-352">在某些情況下，使用[元件參數](#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-352">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="a6a2d-353">串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-353">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="a6a2d-354">級聯的值和參數也會提供一種方法來協調元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-354">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="a6a2d-355">主題範例</span><span class="sxs-lookup"><span data-stu-id="a6a2d-355">Theme example</span></span>

<span data-ttu-id="a6a2d-356">在範例應用程式的下列範例中， `ThemeInfo`類別會指定主題資訊以向下流動元件階層，讓應用程式中指定部分內的所有按鈕共用相同的樣式。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-356">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="a6a2d-357">*UIThemeClasses/ThemeInfo .cs*：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-357">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="a6a2d-358">祖系元件可以使用串聯值元件來提供串聯值。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-358">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="a6a2d-359">此`CascadingValue`元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-359">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="a6a2d-360">例如，範例應用程式會在其中一個應用`ThemeInfo`程式的配置中，將主題資訊（）指定為構成`@Body`屬性版面配置主體之所有元件的串聯參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-360">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="a6a2d-361">`ButtonClass`在版面配置元件中`btn-success` ，會指派的值。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-361">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="a6a2d-362">任何子代元件都可以透過`ThemeInfo`串聯物件使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-362">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="a6a2d-363">`CascadingValuesParametersLayout`成分</span><span class="sxs-lookup"><span data-stu-id="a6a2d-363">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="a6a2d-364">為了利用串聯值，元件會使用`[CascadingParameter]`屬性宣告串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-364">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="a6a2d-365">串聯式值會依類型系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-365">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="a6a2d-366">在範例應用程式中， `CascadingValuesParametersTheme`元件`ThemeInfo`會將串聯值系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-366">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="a6a2d-367">參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-367">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="a6a2d-368">`CascadingValuesParametersTheme`成分</span><span class="sxs-lookup"><span data-stu-id="a6a2d-368">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="a6a2d-369">若要在相同的子樹中串聯多個相同類型的值， `Name`請提供唯一`CascadingValue`的字串給每`CascadingParameter`個元件及其對應的。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-369">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="a6a2d-370">在下列範例中，兩`CascadingValue`個元件依名稱串聯`MyCascadingType`不同的實例：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-370">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="a6a2d-371">在子系元件中，串聯的參數會以名稱從上階元件中對應的串聯值接收其值：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-371">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="a6a2d-372">TabSet 範例</span><span class="sxs-lookup"><span data-stu-id="a6a2d-372">TabSet example</span></span>

<span data-ttu-id="a6a2d-373">串聯式參數也可以讓元件在元件階層之間共同作業。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-373">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="a6a2d-374">例如，請考慮範例應用程式中的下列*TabSet*範例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-374">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="a6a2d-375">範例應用程式具有可`ITab`執行 tab 鍵的介面：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-375">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="a6a2d-376">`CascadingValuesParametersTabSet`元件會使用`TabSet`元件，其中包含數個`Tab`元件：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-376">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="a6a2d-377">子`Tab`元件不會明確地當做參數傳遞至`TabSet`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-377">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="a6a2d-378">相反地，子`Tab`元件是的子內容的一部分`TabSet`。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-378">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="a6a2d-379">不過， `TabSet`仍然需要知道每個`Tab`元件，使其可以呈現標頭和使用中的索引標籤。若要啟用這項協調而不需要額外`TabSet`的程式碼，元件*可以提供本身作為*串聯的值，然後由子代`Tab`元件挑選。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-379">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="a6a2d-380">`TabSet`成分</span><span class="sxs-lookup"><span data-stu-id="a6a2d-380">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="a6a2d-381">子系`Tab`元件會以串聯`TabSet`式參數的形式捕捉包含的`Tab` ，因此元件會將`TabSet`自己加入至索引標籤作用中的和座標。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-381">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="a6a2d-382">`Tab`成分</span><span class="sxs-lookup"><span data-stu-id="a6a2d-382">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Razor<span data-ttu-id="a6a2d-383">範本</span><span class="sxs-lookup"><span data-stu-id="a6a2d-383"> templates</span></span>

<span data-ttu-id="a6a2d-384">您可以使用Razor範本語法來定義轉譯片段。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-384">Render fragments can be defined using Razor template syntax.</span></span> Razor<span data-ttu-id="a6a2d-385">範本是定義 UI 程式碼片段並採用下列格式的方式：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-385"> templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="a6a2d-386">下列範例說明如何指定`RenderFragment`和`RenderFragment<T>`值，並直接在元件中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-386">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="a6a2d-387">轉譯片段也可以當做引數傳遞至樣板[化元件](xref:blazor/templated-components)。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-387">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="a6a2d-388">上述程式碼的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-388">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="a6a2d-389">可擴充向量圖形（SVG）影像</span><span class="sxs-lookup"><span data-stu-id="a6a2d-389">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="a6a2d-390">由於Blazor會轉譯 HTML，瀏覽器支援的影像（包括可擴充的向量圖形（svg）影像（*svg*））可透過`<img>`標記來支援：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-390">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="a6a2d-391">同樣地，樣式表單檔案（*.css*）的 CSS 規則也支援 SVG 影像：</span><span class="sxs-lookup"><span data-stu-id="a6a2d-391">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="a6a2d-392">不過，在所有案例中不支援內嵌 SVG 標記。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-392">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="a6a2d-393">如果您將`<svg>`標記直接放入元件檔案（*razor*），則會支援基本映射轉譯，但尚不支援許多先進的案例。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-393">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="a6a2d-394">例如， `<use>`目前未遵守標記，而且`@bind`無法與某些 SVG 標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-394">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="a6a2d-395">我們希望在未來的版本中解決這些限制。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-395">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6a2d-396">其他資源</span><span class="sxs-lookup"><span data-stu-id="a6a2d-396">Additional resources</span></span>

* <span data-ttu-id="a6a2d-397"><xref:security/blazor/server/threat-mitigation>&ndash;包含有關建立Blazor伺服器應用程式的指導方針，必須對抗資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="a6a2d-397"><xref:security/blazor/server/threat-mitigation> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
