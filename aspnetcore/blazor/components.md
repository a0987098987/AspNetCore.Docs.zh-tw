---
title: 建立與使用ASP.NET核心剃鬚刀元件
author: guardrex
description: 瞭解如何創建和使用 Razor 元件,包括如何綁定到數據、處理事件和管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/21/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: 4434636992cb2506ef6525996690946f97c43764
ms.sourcegitcommit: c9d1208e86160615b2d914cce74a839ae41297a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "81791481"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="b550c-103">建立與使用ASP.NET核心剃鬚刀元件</span><span class="sxs-lookup"><span data-stu-id="b550c-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="b550c-104">由[盧克·萊瑟姆](https://github.com/guardrex),[丹尼爾·羅斯](https://github.com/danroth27)和[托比亞斯·巴特施](https://www.aveo-solutions.com/)</span><span class="sxs-lookup"><span data-stu-id="b550c-104">By [Luke Latham](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), and [Tobias Bartsch](https://www.aveo-solutions.com/)</span></span>

<span data-ttu-id="b550c-105">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b550c-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="b550c-106">應用程式是使用*元件*構建的。</span><span class="sxs-lookup"><span data-stu-id="b550c-106"> apps are built using *components*.</span></span> <span data-ttu-id="b550c-107">元件是用戶介面 (UI) 的自包含塊,如頁面、對話框或窗體。</span><span class="sxs-lookup"><span data-stu-id="b550c-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="b550c-108">元件包括 HTML 標記和注入資料或回應 UI 事件所需的處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="b550c-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="b550c-109">元件靈活輕巧。</span><span class="sxs-lookup"><span data-stu-id="b550c-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="b550c-110">它們可以嵌套、重用和在項目之間共用。</span><span class="sxs-lookup"><span data-stu-id="b550c-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="b550c-111">元件類</span><span class="sxs-lookup"><span data-stu-id="b550c-111">Component classes</span></span>

<span data-ttu-id="b550c-112">元件在[Razor](xref:mvc/views/razor)元件檔 *(.razor*) 中使用 C# 和 HTML 標記的組合實現。</span><span class="sxs-lookup"><span data-stu-id="b550c-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="b550c-113">中Blazor一個元件被稱為 Razor*元件*。</span><span class="sxs-lookup"><span data-stu-id="b550c-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="b550c-114">元件的名稱必須以大寫字元開頭。</span><span class="sxs-lookup"><span data-stu-id="b550c-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="b550c-115">例如 *,MyCool 元件.razor*是有效的,*而 myCool元件.razor*無效。</span><span class="sxs-lookup"><span data-stu-id="b550c-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="b550c-116">元件的 UI 使用 HTML 定義。</span><span class="sxs-lookup"><span data-stu-id="b550c-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="b550c-117">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="b550c-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="b550c-118">編譯應用時,HTML 標記和 C# 呈現邏輯將轉換為元件類。</span><span class="sxs-lookup"><span data-stu-id="b550c-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="b550c-119">生成的類的名稱與檔的名稱匹配。</span><span class="sxs-lookup"><span data-stu-id="b550c-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="b550c-120">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="b550c-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="b550c-121">在塊`@code`中,元件狀態(屬性、欄位)使用事件處理或定義其他元件邏輯的方法指定。</span><span class="sxs-lookup"><span data-stu-id="b550c-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="b550c-122">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="b550c-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="b550c-123">元件成員可以使用 以`@`開頭的 C# 運算式用作元件呈現邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="b550c-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="b550c-124">例如,C# 字段通過`@`前置字位名稱來呈現。</span><span class="sxs-lookup"><span data-stu-id="b550c-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="b550c-125">以下範例計算和渲染:</span><span class="sxs-lookup"><span data-stu-id="b550c-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="b550c-126">`_headingFontStyle`到`font-style`的 CSS 屬性值。</span><span class="sxs-lookup"><span data-stu-id="b550c-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="b550c-127">`_headingText`元素的內容`<h1>`。</span><span class="sxs-lookup"><span data-stu-id="b550c-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="b550c-128">最初呈現元件后,元件將重新生成其呈現樹以回應事件。</span><span class="sxs-lookup"><span data-stu-id="b550c-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="b550c-129">然後,將新的呈現樹與上一個呈現樹進行比較,並將任何修改應用於瀏覽器的文檔物件模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="b550c-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="b550c-130">元件是普通的 C# 類,可以放置在專案中的任意位置。</span><span class="sxs-lookup"><span data-stu-id="b550c-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="b550c-131">生成網頁的元件通常駐留在 *「頁面」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b550c-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="b550c-132">非頁面元件經常放置在 *「共用」* 資料夾或添加到專案的自訂資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b550c-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="b550c-133">通常,元件的命名空間派生自應用的根命名空間和應用中元件的位置(資料夾)。</span><span class="sxs-lookup"><span data-stu-id="b550c-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="b550c-134">若套用的根命名空間`BlazorApp`是`Counter`, 並且元件駐留在 *「 頁面」* 資料夾中:</span><span class="sxs-lookup"><span data-stu-id="b550c-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="b550c-135">元件`Counter`的命名空間為`BlazorApp.Pages`。</span><span class="sxs-lookup"><span data-stu-id="b550c-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="b550c-136">元件的完全限定類型名稱稱為`BlazorApp.Pages.Counter`。</span><span class="sxs-lookup"><span data-stu-id="b550c-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="b550c-137">有關詳細資訊,請參閱[導入元件](#import-components)部分。</span><span class="sxs-lookup"><span data-stu-id="b550c-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="b550c-138">要使用自訂資料夾,請將自訂資料夾的命名空間添加到父元件或應用的 *_Imports.razor*檔中。</span><span class="sxs-lookup"><span data-stu-id="b550c-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="b550c-139">例如, 當套用的根命名`BlazorApp`空間為 : 時,以下命名空間使*元件*資料夾中的元件可用:</span><span class="sxs-lookup"><span data-stu-id="b550c-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="b550c-140">靜態資產</span><span class="sxs-lookup"><span data-stu-id="b550c-140">Static assets</span></span>

Blazor<span data-ttu-id="b550c-141">遵循ASP.NET核心應用將靜態資產放在專案的[Web 根 (wwwroot) 資料夾下的](xref:fundamentals/index#web-root)約定。</span><span class="sxs-lookup"><span data-stu-id="b550c-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="b550c-142">使用基礎相對路徑`/`( ) 引用靜態資產的 Web 根。</span><span class="sxs-lookup"><span data-stu-id="b550c-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="b550c-143">在下面的範例中 *,logo.png*實際位於 *[PROJECT ROOT]/wwwroot/影像*資料夾中:</span><span class="sxs-lookup"><span data-stu-id="b550c-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="b550c-144">剃刀元件**不支援**波浪斜槓表示法`~/`( 。</span><span class="sxs-lookup"><span data-stu-id="b550c-144">Razor components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="b550c-145">有關設定應用基本路徑的資訊,請參<xref:host-and-deploy/blazor/index#app-base-path>閱 。</span><span class="sxs-lookup"><span data-stu-id="b550c-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="b550c-146">元件中不支援標記說明器</span><span class="sxs-lookup"><span data-stu-id="b550c-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="b550c-147">Razor 元件 *(.razor*檔案)不支援[標籤說明器](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="b550c-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="b550c-148">要在Blazor中 提供類似標記説明程式的功能,請創建與標記説明程式具有相同功能的元件,然後改用該元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="b550c-149">使用元件</span><span class="sxs-lookup"><span data-stu-id="b550c-149">Use components</span></span>

<span data-ttu-id="b550c-150">元件可以透過使用 HTML 元素語法聲明它們來包括其他元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="b550c-151">使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。</span><span class="sxs-lookup"><span data-stu-id="b550c-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="b550c-152">*Index.razor*中列標記呈現實體`HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="b550c-152">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="b550c-153">*元件/標題元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-153">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="b550c-154">如果元件包含具有大寫第一個字母與元件名稱不匹配的 HTML 元素,則會發出警告,指示該元素具有意外的名稱。</span><span class="sxs-lookup"><span data-stu-id="b550c-154">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="b550c-155">為`@using`元件的命名空間添加指令使元件可用,從而解決警告。</span><span class="sxs-lookup"><span data-stu-id="b550c-155">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="b550c-156">路由</span><span class="sxs-lookup"><span data-stu-id="b550c-156">Routing</span></span>

<span data-ttu-id="b550c-157">路由Blazor是通過向應用中的每個可訪問元件提供路由範本來實現的。</span><span class="sxs-lookup"><span data-stu-id="b550c-157">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="b550c-158">編譯具有`@page`指令的 Razor 檔時,將為生成<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>的類指定 路由範本。</span><span class="sxs-lookup"><span data-stu-id="b550c-158">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="b550c-159">在運行時,路由器查找具有的`RouteAttribute`元件類,並呈現具有與請求 URL 匹配的路由範本的元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-159">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="b550c-160">如需詳細資訊，請參閱 <xref:blazor/routing>。</span><span class="sxs-lookup"><span data-stu-id="b550c-160">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="b550c-161">參數</span><span class="sxs-lookup"><span data-stu-id="b550c-161">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="b550c-162">路由參數</span><span class="sxs-lookup"><span data-stu-id="b550c-162">Route parameters</span></span>

<span data-ttu-id="b550c-163">元件可以從`@page`指令中提供的路由範本接收路由參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-163">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="b550c-164">路由器使用路由參數填充相應的元件參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-164">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="b550c-165">*頁面/路由參數.剃鬚刀*:</span><span class="sxs-lookup"><span data-stu-id="b550c-165">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="b550c-166">不支援可選參數,因此在前面的示例中應用了`@page`兩個指令。</span><span class="sxs-lookup"><span data-stu-id="b550c-166">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="b550c-167">第一個允許在沒有參數的情況下導航到元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-167">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="b550c-168">第二`@page`個指令接收`{text}`路由參數並將值分配給`Text`屬性 。</span><span class="sxs-lookup"><span data-stu-id="b550c-168">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="b550c-169">Razor 元件 *(.razor*)**不支援**擷取跨多個資料夾邊界的路徑的 *「全部」* 參數語法 (`*`/`**`)</span><span class="sxs-lookup"><span data-stu-id="b550c-169">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="b550c-170">元件參數</span><span class="sxs-lookup"><span data-stu-id="b550c-170">Component parameters</span></span>

<span data-ttu-id="b550c-171">元件可以具有*元件參數*,這些參數使用具有`[Parameter]`屬性的元件類上的公共屬性進行定義。</span><span class="sxs-lookup"><span data-stu-id="b550c-171">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="b550c-172">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="b550c-172">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="b550c-173">*元件/子元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-173">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="b550c-174">在範例應用程式中的以下範例中,`ParentComponent`將設定`Title``ChildComponent`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="b550c-174">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="b550c-175">*頁面/父元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-175">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

> [!WARNING]
> <span data-ttu-id="b550c-176">不要建立寫入其自己的*元件參數的元件*,而是使用私有欄位。</span><span class="sxs-lookup"><span data-stu-id="b550c-176">Don't create components that write to their own *component parameters*, use a private field instead.</span></span> <span data-ttu-id="b550c-177">有關詳細資訊,請參閱[「不創建寫入其自己的參數屬性的元件](#dont-create-components-that-write-to-their-own-parameter-properties)」部分。</span><span class="sxs-lookup"><span data-stu-id="b550c-177">For more information, see the [Don't create components that write to their own parameter properties](#dont-create-components-that-write-to-their-own-parameter-properties) section.</span></span>

## <a name="child-content"></a><span data-ttu-id="b550c-178">子內容</span><span class="sxs-lookup"><span data-stu-id="b550c-178">Child content</span></span>

<span data-ttu-id="b550c-179">元件可以設置另一個元件的內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-179">Components can set the content of another component.</span></span> <span data-ttu-id="b550c-180">分配元件提供指定接收元件的標記之間的內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-180">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="b550c-181">在下面的範例中,`ChildComponent`具有`ChildContent``RenderFragment`表示 屬性, 表示要呈現的 UI 段。</span><span class="sxs-lookup"><span data-stu-id="b550c-181">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="b550c-182">的值`ChildContent`位於元件的標記中,其中應呈現內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-182">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="b550c-183">的值`ChildContent`從父元件接收,並在 Bootstrap`panel-body`面板的 中呈現。</span><span class="sxs-lookup"><span data-stu-id="b550c-183">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="b550c-184">*元件/子元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-184">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="b550c-185">接收`RenderFragment`內容的屬性必須按約定`ChildContent`命名 。</span><span class="sxs-lookup"><span data-stu-id="b550c-185">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="b550c-186">`ParentComponent`範例應用中可以通過將內容放置`<ChildComponent>`在 標記中來`ChildComponent`提供用於呈現 的內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-186">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="b550c-187">*頁面/父元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-187">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="b550c-188">屬性計算與任意參數</span><span class="sxs-lookup"><span data-stu-id="b550c-188">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="b550c-189">除了元件聲明的參數之外,元件還可以捕獲和呈現其他屬性。</span><span class="sxs-lookup"><span data-stu-id="b550c-189">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="b550c-190">可以在字典中捕獲其他屬性,然後在使用[`@attributes`](xref:mvc/views/razor#attributes)Razor 指令呈現元件時*將其壓縮*到元素上。</span><span class="sxs-lookup"><span data-stu-id="b550c-190">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="b550c-191">在定義生成支援各種自定義的標記元素的元件時,此方案非常有用。</span><span class="sxs-lookup"><span data-stu-id="b550c-191">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="b550c-192">例如,單獨定義支援許多參數的的屬性`<input>`可能很乏味。</span><span class="sxs-lookup"><span data-stu-id="b550c-192">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="b550c-193">在下面的`<input>`範例中,第一個`id="useIndividualParams"`元素 ( ) 使用單個元件`<input>`參數,`id="useAttributesDict"`而第二個元素 ( ) 使用屬性 splatt:</span><span class="sxs-lookup"><span data-stu-id="b550c-193">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="b550c-194">參數的類型必須使用字串鍵 。`IEnumerable<KeyValuePair<string, object>>`</span><span class="sxs-lookup"><span data-stu-id="b550c-194">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="b550c-195">在這種情況下`IReadOnlyDictionary<string, object>`,使用也是一個選項。</span><span class="sxs-lookup"><span data-stu-id="b550c-195">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="b550c-196">使用這兩`<input>`種方法的呈現元素是相同的:</span><span class="sxs-lookup"><span data-stu-id="b550c-196">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="b550c-197">要接受任何屬性,請使用`[Parameter]``CaptureUnmatchedValues`屬性將 元件參數定義為`true`:</span><span class="sxs-lookup"><span data-stu-id="b550c-197">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="b550c-198">上`CaptureUnmatchedValues``[Parameter]`的屬性允許參數匹配與任何其他參數不匹配的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="b550c-198">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="b550c-199">元件只能使用`CaptureUnmatchedValues`定義單個參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-199">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="b550c-200">使用的屬性類型`CaptureUnmatchedValues``Dictionary<string, object>`必須與字串鍵一起分配。</span><span class="sxs-lookup"><span data-stu-id="b550c-200">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="b550c-201">`IEnumerable<KeyValuePair<string, object>>`或`IReadOnlyDictionary<string, object>`此方案中的選項。</span><span class="sxs-lookup"><span data-stu-id="b550c-201">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="b550c-202">`@attributes`相對於元素屬性位置的位置非常重要。</span><span class="sxs-lookup"><span data-stu-id="b550c-202">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="b550c-203">在`@attributes`元素上進行壓縮時,從右向左處理屬性(最後處理到第一個)。</span><span class="sxs-lookup"><span data-stu-id="b550c-203">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="b550c-204">請考慮使用`Child`元件的以下元件範例:</span><span class="sxs-lookup"><span data-stu-id="b550c-204">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="b550c-205">*父元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-205">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="b550c-206">*子元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-206">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="b550c-207">元件`Child`屬性`extra`設定為 右`@attributes`方 。</span><span class="sxs-lookup"><span data-stu-id="b550c-207">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="b550c-208">元件`Parent`的渲染`<div>`包含在透過額外屬性`extra="5"`時,因為屬性從右向左處理(最後處理到第一):</span><span class="sxs-lookup"><span data-stu-id="b550c-208">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="b550c-209">`extra`在下面的範例中,與`@attributes`的順序在`Child`元件的`<div>`中顛倒了 :</span><span class="sxs-lookup"><span data-stu-id="b550c-209">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="b550c-210">*父元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-210">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="b550c-211">*子元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-211">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="b550c-212">透過額外屬性`<div>``extra="10"`時,`Parent`元件中呈現的包含:</span><span class="sxs-lookup"><span data-stu-id="b550c-212">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="b550c-213">捕捉對元件的參考</span><span class="sxs-lookup"><span data-stu-id="b550c-213">Capture references to components</span></span>

<span data-ttu-id="b550c-214">元件參照提供參考元件的實體, 以便您可以向該實體的指令,例如`Show`或`Reset`。</span><span class="sxs-lookup"><span data-stu-id="b550c-214">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="b550c-215">要捕獲元件引用,</span><span class="sxs-lookup"><span data-stu-id="b550c-215">To capture a component reference:</span></span>

* <span data-ttu-id="b550c-216">向子[`@ref`](xref:mvc/views/razor#ref)元件添加屬性。</span><span class="sxs-lookup"><span data-stu-id="b550c-216">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="b550c-217">定義與子元件類型相同的欄位。</span><span class="sxs-lookup"><span data-stu-id="b550c-217">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="b550c-218">呈現元件時,`_loginDialog`該欄位將填充`MyLoginDialog`子元件實例。</span><span class="sxs-lookup"><span data-stu-id="b550c-218">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="b550c-219">然後,您可以在元件實例上調用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="b550c-219">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b550c-220">僅當`_loginDialog`呈現元件及其輸出包括`MyLoginDialog`元素后,才會填充該變數。</span><span class="sxs-lookup"><span data-stu-id="b550c-220">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="b550c-221">在那之前,沒有什麼可參考的。</span><span class="sxs-lookup"><span data-stu-id="b550c-221">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="b550c-222">您可以在元件完成呈現後操作元件參考,請使用[On 後 RenderAsync 或 On 後渲染方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="b550c-222">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="b550c-223">要參考循環中的元件,請參考[擷取擷取對多個類似子元件(dotnet/aspnetcore #13358) 的參考](https://github.com/dotnet/aspnetcore/issues/13358)。</span><span class="sxs-lookup"><span data-stu-id="b550c-223">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="b550c-224">雖然捕獲元件引用使用類似的語法來[捕獲元素引用](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements),但它不是 JAVAScript 互通功能。</span><span class="sxs-lookup"><span data-stu-id="b550c-224">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="b550c-225">元件引用不會傳遞給僅在 .NET&mdash;代碼 中使用的 JavaScript 代碼。</span><span class="sxs-lookup"><span data-stu-id="b550c-225">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="b550c-226">**請勿**使用元件引用來更改子元件的狀態。</span><span class="sxs-lookup"><span data-stu-id="b550c-226">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="b550c-227">相反,使用普通聲明性參數將數據傳遞給子元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-227">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="b550c-228">使用正常聲明性參數會導致子元件在正確時間自動重新呈現。</span><span class="sxs-lookup"><span data-stu-id="b550c-228">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="b550c-229">外部呼叫元件方法以更新狀態</span><span class="sxs-lookup"><span data-stu-id="b550c-229">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="b550c-230">使用`SynchronizationContext`強制執行的單個邏輯線程。</span><span class="sxs-lookup"><span data-stu-id="b550c-230"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="b550c-231">元件的[生命週期方法和](xref:blazor/lifecycle)由Blazor引發的任何事件回調都`SynchronizationContext`在此 執行。</span><span class="sxs-lookup"><span data-stu-id="b550c-231">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="b550c-232">如果必須根據外部事件(如計時器或其他通知)更新元件,請使用方法`InvokeAsync`,該方法將調度到Blazor的`SynchronizationContext`。</span><span class="sxs-lookup"><span data-stu-id="b550c-232">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="b550c-233">例如,考慮通知*器服務*,該服務可以通知任何已更新狀態的偵聽元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-233">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="b550c-234">註冊為`NotifierService`單項:</span><span class="sxs-lookup"><span data-stu-id="b550c-234">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="b550c-235">在BlazorWebAssembly 中`Program.Main`,在 中註冊服務:</span><span class="sxs-lookup"><span data-stu-id="b550c-235">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="b550c-236">在Blazor「伺服器」中,`Startup.ConfigureServices`在中註冊服務:</span><span class="sxs-lookup"><span data-stu-id="b550c-236">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="b550c-237">使用`NotifierService`更新元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-237">Use the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="b550c-238">在前面的`NotifierService`範例中,呼叫元件`OnNotify`的方法以外的Blazor `SynchronizationContext` '</span><span class="sxs-lookup"><span data-stu-id="b550c-238">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="b550c-239">`InvokeAsync`用於切換到正確的上下文並排隊渲染。</span><span class="sxs-lookup"><span data-stu-id="b550c-239">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="b550c-240">使用\@鍵控制元素與元件的保留</span><span class="sxs-lookup"><span data-stu-id="b550c-240">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="b550c-241">在呈現元素或元件的清單以及隨後更改的元素或元件時,Blazor其擴散演演演算法必須決定保留以前的元素或元件中的哪些元素或元件,以及模型物件應該如何映射到它們。</span><span class="sxs-lookup"><span data-stu-id="b550c-241">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="b550c-242">通常,此過程是自動的,可以忽略,但在某些情況下,您可能需要控制該過程。</span><span class="sxs-lookup"><span data-stu-id="b550c-242">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="b550c-243">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="b550c-243">Consider the following example:</span></span>

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

<span data-ttu-id="b550c-244">`People`集合的內容可能會隨著插入、刪除或重新排序的條目而更改。</span><span class="sxs-lookup"><span data-stu-id="b550c-244">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="b550c-245">當元件重新成成成時`<DetailsEditor>`, 元件可能會更改為接收`Details`不同的 參數值。</span><span class="sxs-lookup"><span data-stu-id="b550c-245">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="b550c-246">這可能導致比預期更複雜的重新渲染。</span><span class="sxs-lookup"><span data-stu-id="b550c-246">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="b550c-247">在某些情況下,重新渲染可能會導致明顯的行為差異,例如元素焦點丟失。</span><span class="sxs-lookup"><span data-stu-id="b550c-247">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="b550c-248">可以使用[`@key`](xref:mvc/views/razor#key)指令屬性控制映射過程。</span><span class="sxs-lookup"><span data-stu-id="b550c-248">The mapping process can be controlled with the [`@key`](xref:mvc/views/razor#key) directive attribute.</span></span> <span data-ttu-id="b550c-249">`@key`使擴散演算法保證根據鍵的值保留元素或元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-249">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="b550c-250">當`People`集合變更時,擴散演演演算法將`<DetailsEditor>``person`保留實例和 實體之間的關聯:</span><span class="sxs-lookup"><span data-stu-id="b550c-250">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="b550c-251">如果從`Person``People`清單中刪除了 , 則`<DetailsEditor>`僅從 UI 中刪除相應的實例。</span><span class="sxs-lookup"><span data-stu-id="b550c-251">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="b550c-252">其他實例保持不變。</span><span class="sxs-lookup"><span data-stu-id="b550c-252">Other instances are left unchanged.</span></span>
* <span data-ttu-id="b550c-253">如果將`Person`插入到清單中的某個位置,則在相應的`<DetailsEditor>`位置 插入一個新實例。</span><span class="sxs-lookup"><span data-stu-id="b550c-253">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="b550c-254">其他實例保持不變。</span><span class="sxs-lookup"><span data-stu-id="b550c-254">Other instances are left unchanged.</span></span>
* <span data-ttu-id="b550c-255">如果`Person`重新排序了條目,則`<DetailsEditor>`相應的 實例將保留並在 UI 中重新排序。</span><span class="sxs-lookup"><span data-stu-id="b550c-255">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="b550c-256">在某些情況下,使用`@key`可最大程度地降低重新渲染的複雜性,並避免 DOM 的有狀態部分(如焦點位置)發生潛在問題。</span><span class="sxs-lookup"><span data-stu-id="b550c-256">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b550c-257">鍵是每個容器元素或元件的本地。</span><span class="sxs-lookup"><span data-stu-id="b550c-257">Keys are local to each container element or component.</span></span> <span data-ttu-id="b550c-258">不會跨文檔全域比較密鑰。</span><span class="sxs-lookup"><span data-stu-id="b550c-258">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="b550c-259">何時使用\@金鑰</span><span class="sxs-lookup"><span data-stu-id="b550c-259">When to use \@key</span></span>

<span data-ttu-id="b550c-260">通常,每當呈現清單(例如`@key`,在`@foreach`塊中)並且存在合適的值來定義`@key`時 ,使用 都有意義。</span><span class="sxs-lookup"><span data-stu-id="b550c-260">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="b550c-261">您還可以使用`@key`Blazor防止 在物件變更時保留元素或元件子樹:</span><span class="sxs-lookup"><span data-stu-id="b550c-261">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="b550c-262">如果`@currentPerson`發生變更`@key`, 屬性Blazor指令將`<div>`強制放棄整個 及其後代,並在 UI 中使用新元素和元件重建子樹。</span><span class="sxs-lookup"><span data-stu-id="b550c-262">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="b550c-263">如果需要保證在更改時`@currentPerson`不保留任何 UI 狀態,則此功能非常有用。</span><span class="sxs-lookup"><span data-stu-id="b550c-263">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="b550c-264">何時不使用\@鍵</span><span class="sxs-lookup"><span data-stu-id="b550c-264">When not to use \@key</span></span>

<span data-ttu-id="b550c-265">與`@key`時發生梭差會有性能成本。</span><span class="sxs-lookup"><span data-stu-id="b550c-265">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="b550c-266">性能成本並不大,但僅指定`@key`控制元素或元件保留規則是否有利於應用。</span><span class="sxs-lookup"><span data-stu-id="b550c-266">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="b550c-267">即使`@key`不使用Blazor, 也會盡可能保留子元素和元件實例。</span><span class="sxs-lookup"><span data-stu-id="b550c-267">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="b550c-268">使用`@key`的唯一優點是控制模型實例*如何*映射到保留的元件實例,而不是選擇映射的擴散演演演算法。</span><span class="sxs-lookup"><span data-stu-id="b550c-268">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="b550c-269">鍵使用\@的值</span><span class="sxs-lookup"><span data-stu-id="b550c-269">What values to use for \@key</span></span>

<span data-ttu-id="b550c-270">通常,為`@key`:</span><span class="sxs-lookup"><span data-stu-id="b550c-270">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="b550c-271">對物件實例建模(例如,如前面的`Person`示例中所示的實例)。</span><span class="sxs-lookup"><span data-stu-id="b550c-271">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="b550c-272">這可確保基於物件引用相等性保存。</span><span class="sxs-lookup"><span data-stu-id="b550c-272">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="b550c-273">唯一識別碼(例如,類型的主要`int`鍵值`string``Guid`。</span><span class="sxs-lookup"><span data-stu-id="b550c-273">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="b550c-274">確保用於的值`@key`不衝突。</span><span class="sxs-lookup"><span data-stu-id="b550c-274">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="b550c-275">如果在同一父元素中檢測到衝突值,則Blazor引發異常,因為它無法確定將舊元素或元件映射到新元素或元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-275">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="b550c-276">僅使用不同的值,如物件實例或主鍵值。</span><span class="sxs-lookup"><span data-stu-id="b550c-276">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="dont-create-components-that-write-to-their-own-parameter-properties"></a><span data-ttu-id="b550c-277">不要建立寫入其自身參數屬性的元件</span><span class="sxs-lookup"><span data-stu-id="b550c-277">Don't create components that write to their own parameter properties</span></span>

<span data-ttu-id="b550c-278">參數在以下條件下被覆寫:</span><span class="sxs-lookup"><span data-stu-id="b550c-278">Parameters are overwritten under the following conditions:</span></span>

* <span data-ttu-id="b550c-279">子元件的內容使用 呈現。 `RenderFragment`</span><span class="sxs-lookup"><span data-stu-id="b550c-279">A child component's content is rendered with a `RenderFragment`.</span></span>
* <span data-ttu-id="b550c-280"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>在父元件中調用。</span><span class="sxs-lookup"><span data-stu-id="b550c-280"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called in the parent component.</span></span>

<span data-ttu-id="b550c-281">參數被重置,因為調用時<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>父元件重新呈現,並且向子元件提供了新的參數值。</span><span class="sxs-lookup"><span data-stu-id="b550c-281">Parameters are reset because the parent component rerenders when <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called and new parameter values are supplied to the child component.</span></span>

<span data-ttu-id="b550c-282">請考慮以下`Expander`元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-282">Consider the following `Expander` component that:</span></span>

* <span data-ttu-id="b550c-283">渲染子內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-283">Renders child content.</span></span>
* <span data-ttu-id="b550c-284">切換顯示具有元件參數的子內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-284">Toggles showing child content with a component parameter.</span></span>

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

<span data-ttu-id="b550c-285">此`Expander`元件將新增到可能呼叫`StateHasChanged`的父元件中:</span><span class="sxs-lookup"><span data-stu-id="b550c-285">The `Expander` component is added to a parent component that may call `StateHasChanged`:</span></span>

```razor
<Expander Expanded="true">
    <h1>Hello, world!</h1>
</Expander>

<Expander Expanded="true" />

<button @onclick="@(() => StateHasChanged())">
    Call StateHasChanged
</button>
```

<span data-ttu-id="b550c-286">最初,當`Expander`元件的屬性切換時,它們`Expanded`的行為是獨立的。</span><span class="sxs-lookup"><span data-stu-id="b550c-286">Initially, the `Expander` components behave independently when their `Expanded` properties are toggled.</span></span> <span data-ttu-id="b550c-287">子元件按預期保持其狀態。</span><span class="sxs-lookup"><span data-stu-id="b550c-287">The child components maintain their states as expected.</span></span> <span data-ttu-id="b550c-288">在`StateHasChanged`父級中調用時,`Expanded`第一個子元件的參數將重置回其初始值 ()。`true`</span><span class="sxs-lookup"><span data-stu-id="b550c-288">When `StateHasChanged` is called in the parent, the `Expanded` parameter of the first child component is reset back to its initial value (`true`).</span></span> <span data-ttu-id="b550c-289">第二`Expander`個元件`Expanded`的值不會重置,因為第二個元件中沒有呈現子內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-289">The second `Expander` component's `Expanded` value isn't reset because no child content is rendered in the second component.</span></span>

<span data-ttu-id="b550c-290">要在上述方案中保持狀態,請使用元件`Expander`中的*私有欄位*來保持其切換狀態。</span><span class="sxs-lookup"><span data-stu-id="b550c-290">To maintain state in the preceding scenario, use a *private field* in the `Expander` component to maintain its toggled state.</span></span>

<span data-ttu-id="b550c-291">以下`Expander`元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-291">The following `Expander` component:</span></span>

* <span data-ttu-id="b550c-292">接受父`Expanded`級的元件參數值。</span><span class="sxs-lookup"><span data-stu-id="b550c-292">Accepts the `Expanded` component parameter value from the parent.</span></span>
* <span data-ttu-id="b550c-293">將元件參數值分配到[On 初始化事件中](xref:blazor/lifecycle#component-initialization-methods)的*專用欄位*`_expanded`() 。</span><span class="sxs-lookup"><span data-stu-id="b550c-293">Assigns the component parameter value to a *private field* (`_expanded`) in the [OnInitialized event](xref:blazor/lifecycle#component-initialization-methods).</span></span>
* <span data-ttu-id="b550c-294">使用專用欄位保持其內部切換狀態。</span><span class="sxs-lookup"><span data-stu-id="b550c-294">Uses the private field to maintain its internal toggle state.</span></span>

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @_expanded)

    @if (_expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private bool _expanded;

    protected override void OnInitialized()
    {
        _expanded = Expanded;
    }

    private void Toggle()
    {
        _expanded = !_expanded;
    }
}
```

## <a name="partial-class-support"></a><span data-ttu-id="b550c-295">部分類別支援</span><span class="sxs-lookup"><span data-stu-id="b550c-295">Partial class support</span></span>

<span data-ttu-id="b550c-296">剃刀元件作為部分類生成。</span><span class="sxs-lookup"><span data-stu-id="b550c-296">Razor components are generated as partial classes.</span></span> <span data-ttu-id="b550c-297">剃刀元件使用以下任一方法創作:</span><span class="sxs-lookup"><span data-stu-id="b550c-297">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="b550c-298">C# 程式碼[`@code`](xref:mvc/views/razor#code)在塊中定義,在單個檔中包含 HTML 標記和 Razor 程式碼。</span><span class="sxs-lookup"><span data-stu-id="b550c-298">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="b550c-299">樣本使用此方法定義其 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-299"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="b550c-300">C# 程式碼放置在定義為部分類的代碼後面檔中。</span><span class="sxs-lookup"><span data-stu-id="b550c-300">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="b550c-301">下面的範例顯示從範本生成的`Counter`應用中`@code`具有塊Blazor的 預設元件。</span><span class="sxs-lookup"><span data-stu-id="b550c-301">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="b550c-302">HTML 標籤、Razor 代碼與 C# 程式碼位於同一檔中:</span><span class="sxs-lookup"><span data-stu-id="b550c-302">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="b550c-303">*計數器.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-303">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="b550c-304">也可以`Counter`使用帶有部分類別的代碼後面檔案建立元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-304">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="b550c-305">*計數器.razor*:</span><span class="sxs-lookup"><span data-stu-id="b550c-305">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="b550c-306">*Counter.razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="b550c-306">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="b550c-307">根據需要向部分類檔添加任何必需的命名空間。</span><span class="sxs-lookup"><span data-stu-id="b550c-307">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="b550c-308">Razor 元件使用的典型命名空間包括:</span><span class="sxs-lookup"><span data-stu-id="b550c-308">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="b550c-309">指定基類別</span><span class="sxs-lookup"><span data-stu-id="b550c-309">Specify a base class</span></span>

<span data-ttu-id="b550c-310">該[`@inherits`](xref:mvc/views/razor#inherits)指令可用於為元件指定基類。</span><span class="sxs-lookup"><span data-stu-id="b550c-310">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="b550c-311">下面的範例展示如何繼承基類,`BlazorRocksBase`以提供元件的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="b550c-311">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="b550c-312">基類應派生自`ComponentBase`。</span><span class="sxs-lookup"><span data-stu-id="b550c-312">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="b550c-313">*頁面/布拉佐羅克斯.剃鬚刀*:</span><span class="sxs-lookup"><span data-stu-id="b550c-313">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="b550c-314">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="b550c-314">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="b550c-315">指定屬性</span><span class="sxs-lookup"><span data-stu-id="b550c-315">Specify an attribute</span></span>

<span data-ttu-id="b550c-316">屬性可以在 Razor 元件中使用[`@attribute`](xref:mvc/views/razor#attribute)指令 指定。</span><span class="sxs-lookup"><span data-stu-id="b550c-316">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="b550c-317">以下範例將`[Authorize]`該屬性套用於元件類別:</span><span class="sxs-lookup"><span data-stu-id="b550c-317">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="b550c-318">匯入元件</span><span class="sxs-lookup"><span data-stu-id="b550c-318">Import components</span></span>

<span data-ttu-id="b550c-319">使用 Razor 創作的元件的命名空間基於(按優先順序順序):</span><span class="sxs-lookup"><span data-stu-id="b550c-319">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="b550c-320">[`@namespace`](xref:mvc/views/razor#namespace)Razor 檔案(*.razor*`@namespace BlazorSample.MyNamespace`) 標記( )中指定 。</span><span class="sxs-lookup"><span data-stu-id="b550c-320">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="b550c-321">專案檔`RootNamespace`中 (`<RootNamespace>BlazorSample</RootNamespace>`。</span><span class="sxs-lookup"><span data-stu-id="b550c-321">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="b550c-322">專案名稱,取自專案檔的檔名 *(.csproj*) 以及從專案根到元件的路徑。</span><span class="sxs-lookup"><span data-stu-id="b550c-322">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="b550c-323">例如,框架解析 *[PROJECT ROOT]/頁面/索引.razor* *(BlazorSample.csproj*) 到命名`BlazorSample.Pages`空間 。</span><span class="sxs-lookup"><span data-stu-id="b550c-323">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="b550c-324">元件遵循 C# 名稱綁定規則。</span><span class="sxs-lookup"><span data-stu-id="b550c-324">Components follow C# name binding rules.</span></span> <span data-ttu-id="b550c-325">對於此範例`Index`中的元件,作用網域中的元件都是所有元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-325">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="b550c-326">在同一個資料夾中,*頁面*。</span><span class="sxs-lookup"><span data-stu-id="b550c-326">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="b550c-327">專案根中的元件不顯式指定其他命名空間。</span><span class="sxs-lookup"><span data-stu-id="b550c-327">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="b550c-328">使用 Razor[`@using`](xref:mvc/views/razor#using)的 指令將不同命名空間中定義的元件引入範圍。</span><span class="sxs-lookup"><span data-stu-id="b550c-328">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="b550c-329">如果另一個`NavMenu.razor`元件 (中)存在於*BlazorSample/共用/* 資料夾中,則`Index.razor`該元件`@using`可用於以下 語句:</span><span class="sxs-lookup"><span data-stu-id="b550c-329">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="b550c-330">也可以使用完全限定的名稱引用元件,這不需要指令[`@using`](xref:mvc/views/razor#using):</span><span class="sxs-lookup"><span data-stu-id="b550c-330">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="b550c-331">不支持`global::`資格認證。</span><span class="sxs-lookup"><span data-stu-id="b550c-331">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="b550c-332">不支援使用別名`using`語句導入元件(例如,)。 `@using Foo = Bar`</span><span class="sxs-lookup"><span data-stu-id="b550c-332">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="b550c-333">不支援部分限定名稱。</span><span class="sxs-lookup"><span data-stu-id="b550c-333">Partially qualified names aren't supported.</span></span> <span data-ttu-id="b550c-334">例如,不支援新增`@using BlazorSample`和引用`NavMenu.razor` `<Shared.NavMenu></Shared.NavMenu>` 。</span><span class="sxs-lookup"><span data-stu-id="b550c-334">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="b550c-335">檔案 HTML 元素屬性</span><span class="sxs-lookup"><span data-stu-id="b550c-335">Conditional HTML element attributes</span></span>

<span data-ttu-id="b550c-336">HTML 元素屬性根據 .NET 值有條件地呈現。</span><span class="sxs-lookup"><span data-stu-id="b550c-336">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="b550c-337">如果值為`false``null`或 ,則不呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="b550c-337">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="b550c-338">如果值為`true`,則屬性將最小化。</span><span class="sxs-lookup"><span data-stu-id="b550c-338">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="b550c-339">在下面的範例中,`IsCompleted`確定`checked`是否呈現在元素的標籤中:</span><span class="sxs-lookup"><span data-stu-id="b550c-339">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="b550c-340">如果是`IsCompleted``true`,則複選框將呈現為:</span><span class="sxs-lookup"><span data-stu-id="b550c-340">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="b550c-341">如果是`IsCompleted``false`,則複選框將呈現為:</span><span class="sxs-lookup"><span data-stu-id="b550c-341">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="b550c-342">如需詳細資訊，請參閱 <xref:mvc/views/razor>。</span><span class="sxs-lookup"><span data-stu-id="b550c-342">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="b550c-343">當 .NET[aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)`bool`類型為 時,某些 HTML 屬性(如 aria 按下)無法正常工作。</span><span class="sxs-lookup"><span data-stu-id="b550c-343">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="b550c-344">在這些情況下,請使用`string`型態而不是`bool`。</span><span class="sxs-lookup"><span data-stu-id="b550c-344">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="b550c-345">原始 HTML</span><span class="sxs-lookup"><span data-stu-id="b550c-345">Raw HTML</span></span>

<span data-ttu-id="b550c-346">字串通常使用 DOM 文字節點呈現,這意味著它們可能包含的任何標記都將被忽略並視為文本文本。</span><span class="sxs-lookup"><span data-stu-id="b550c-346">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="b550c-347">要呈現原始 HTML,`MarkupString`在值中包裝 HTML 內容。</span><span class="sxs-lookup"><span data-stu-id="b550c-347">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="b550c-348">該值被解析為 HTML 或 SVG 並插入到 DOM 中。</span><span class="sxs-lookup"><span data-stu-id="b550c-348">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="b550c-349">渲染從任何不受信任的源構造的原始 HTML 存在**安全風險**,應避免使用!</span><span class="sxs-lookup"><span data-stu-id="b550c-349">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="b550c-350">下面的範例顯示使用`MarkupString`類型將靜態 HTML 內容區塊加入元件的呈現輸出中:</span><span class="sxs-lookup"><span data-stu-id="b550c-350">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="b550c-351">階層與參數</span><span class="sxs-lookup"><span data-stu-id="b550c-351">Cascading values and parameters</span></span>

<span data-ttu-id="b550c-352">在某些情況下,使用[元件參數](#component-parameters)將數據從祖先元件流向後代元件不方便,尤其是在有多個元件層時。</span><span class="sxs-lookup"><span data-stu-id="b550c-352">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="b550c-353">級聯值和參數為祖先元件提供了一種為其所有後代元件提供值的便捷方法,從而解決了此問題。</span><span class="sxs-lookup"><span data-stu-id="b550c-353">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="b550c-354">級聯值和參數也為元件提供了一種協調方法。</span><span class="sxs-lookup"><span data-stu-id="b550c-354">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="b550c-355">主題範例</span><span class="sxs-lookup"><span data-stu-id="b550c-355">Theme example</span></span>

<span data-ttu-id="b550c-356">在範例應用程式中的以下範例中,`ThemeInfo`類別指定向到元件層次結構的主題資訊,以便應用給定部分中的所有按鈕共用相同的樣式。</span><span class="sxs-lookup"><span data-stu-id="b550c-356">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="b550c-357">*UIThemeclasses/主題資訊.cs*:</span><span class="sxs-lookup"><span data-stu-id="b550c-357">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="b550c-358">祖先元件可以使用級聯值元件提供級聯值。</span><span class="sxs-lookup"><span data-stu-id="b550c-358">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="b550c-359">元件`CascadingValue`包裝元件層次結構的子樹,並向該子樹中的所有元件提供單個值。</span><span class="sxs-lookup"><span data-stu-id="b550c-359">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="b550c-360">例如,範例應用將應用佈局之一的主題資訊`ThemeInfo`( )`@Body`指定為構成 屬性佈局正文的所有元件的級聯參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-360">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="b550c-361">`ButtonClass`在佈局元件中分配`btn-success`的值。</span><span class="sxs-lookup"><span data-stu-id="b550c-361">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="b550c-362">任何後代元件都可以通過`ThemeInfo`級聯物件使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="b550c-362">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="b550c-363">`CascadingValuesParametersLayout`元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-363">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="b550c-364">要使用級聯值,元件使用 屬性`[CascadingParameter]`聲明級聯參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-364">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="b550c-365">級聯值按類型綁定到級聯參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-365">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="b550c-366">在範例應用中,`CascadingValuesParametersTheme`元件將`ThemeInfo`級聯值綁定到級聯參數。</span><span class="sxs-lookup"><span data-stu-id="b550c-366">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="b550c-367">該參數用於為元件顯示的按鈕之一設置 CSS 類。</span><span class="sxs-lookup"><span data-stu-id="b550c-367">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="b550c-368">`CascadingValuesParametersTheme`元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-368">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="b550c-369">要在同一子樹中級聯同一類型的多個值,請為每個`Name``CascadingValue`元件及其相應的`CascadingParameter`提供唯一字串。</span><span class="sxs-lookup"><span data-stu-id="b550c-369">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="b550c-370">在下面的範例中,兩個`CascadingValue`元件`MyCascadingType`依名稱級聯不同的實例:</span><span class="sxs-lookup"><span data-stu-id="b550c-370">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="b550c-371">在子體元件中,級聯參數從祖先元件中的相應級聯值接收其值:</span><span class="sxs-lookup"><span data-stu-id="b550c-371">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="b550c-372">選項卡集範例</span><span class="sxs-lookup"><span data-stu-id="b550c-372">TabSet example</span></span>

<span data-ttu-id="b550c-373">級聯參數還使元件能夠跨元件層次結構進行協作。</span><span class="sxs-lookup"><span data-stu-id="b550c-373">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="b550c-374">例如,在示例應用中考慮以下*TabSet*示例。</span><span class="sxs-lookup"><span data-stu-id="b550c-374">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="b550c-375">範例應用程式具有介面`ITab`,用於選項卡實現:</span><span class="sxs-lookup"><span data-stu-id="b550c-375">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="b550c-376">元件`CascadingValuesParametersTabSet`使用包含多個`TabSet``Tab`元件的元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-376">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="b550c-377">子`Tab`元件不會顯示參數為參數傳遞給`TabSet`。</span><span class="sxs-lookup"><span data-stu-id="b550c-377">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="b550c-378">相反,子`Tab`元件是子`TabSet`內容的一部分。</span><span class="sxs-lookup"><span data-stu-id="b550c-378">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="b550c-379">但是,仍`TabSet`需要瞭解每個`Tab`元件,以便它可以呈現標頭和活動選項卡。為了啟用這種協調而無需其他代碼,`TabSet`元件*可以作為級聯值提供自身*,然後由`Tab`後代 元件選取。</span><span class="sxs-lookup"><span data-stu-id="b550c-379">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="b550c-380">`TabSet`元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-380">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="b550c-381">後代`Tab`元件將包含`TabSet`捕獲為級聯參數,因此`Tab`元件將自己添加到 和座標上`TabSet`哪個選項卡處於活動狀態。</span><span class="sxs-lookup"><span data-stu-id="b550c-381">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="b550c-382">`Tab`元件:</span><span class="sxs-lookup"><span data-stu-id="b550c-382">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="b550c-383">剃刀範本</span><span class="sxs-lookup"><span data-stu-id="b550c-383">Razor templates</span></span>

<span data-ttu-id="b550c-384">可以使用 Razor 樣本語法定義渲染片段。</span><span class="sxs-lookup"><span data-stu-id="b550c-384">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="b550c-385">Razor 樣本是定義 UI 代碼段並採用以下格式的一種方式:</span><span class="sxs-lookup"><span data-stu-id="b550c-385">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="b550c-386">下面的範例說明了如何在元件中直接`RenderFragment`指定`RenderFragment<T>`和值和呈現範本。</span><span class="sxs-lookup"><span data-stu-id="b550c-386">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="b550c-387">成像片段也可以為參數傳遞為[樣本化的元件](xref:blazor/templated-components)。</span><span class="sxs-lookup"><span data-stu-id="b550c-387">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="b550c-388">上述代碼的呈現輸出:</span><span class="sxs-lookup"><span data-stu-id="b550c-388">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="b550c-389">可延伸向量圖形 (SVG) 影像</span><span class="sxs-lookup"><span data-stu-id="b550c-389">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="b550c-390">由於Blazor成像 HTML,瀏覽器支援的影像,包括可延伸向量圖形 (SVG) 影像 *(.svg*) 透過`<img>`標記支援:</span><span class="sxs-lookup"><span data-stu-id="b550c-390">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="b550c-391">同樣,樣式表檔的 CSS 規則 *(.css)* 中支援 SVG 圖像。</span><span class="sxs-lookup"><span data-stu-id="b550c-391">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="b550c-392">但是,並非所有方案都支援內聯 SVG 標記。</span><span class="sxs-lookup"><span data-stu-id="b550c-392">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="b550c-393">如果將`<svg>`標記直接放入元件檔 *(.razor),* 則支援基本圖像呈現,但不支援許多高級方案。</span><span class="sxs-lookup"><span data-stu-id="b550c-393">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="b550c-394">例如,`<use>`標記目前不受尊重`@bind`, 並且不能與某些 SVG 標記一起使用。</span><span class="sxs-lookup"><span data-stu-id="b550c-394">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="b550c-395">我們期望在未來的版本中解決這些限制。</span><span class="sxs-lookup"><span data-stu-id="b550c-395">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b550c-396">其他資源</span><span class="sxs-lookup"><span data-stu-id="b550c-396">Additional resources</span></span>

* <span data-ttu-id="b550c-397"><xref:security/blazor/server>&ndash;包括有關構建Blazor必須應對資源耗盡的伺服器應用的指導。</span><span class="sxs-lookup"><span data-stu-id="b550c-397"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
