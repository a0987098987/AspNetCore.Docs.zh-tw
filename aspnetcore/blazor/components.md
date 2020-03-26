---
title: 建立和使用 ASP.NET Core Razor 元件
author: guardrex
description: 瞭解如何建立和使用 Razor 元件，包括如何系結至資料、處理事件，以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: bc1d07aef9cd60b89343a034168daa6754f4421b
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306503"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="2850d-103">建立和使用 ASP.NET Core Razor 元件</span><span class="sxs-lookup"><span data-stu-id="2850d-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="2850d-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2850d-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="2850d-105">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2850d-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="2850d-106"> 應用程式是使用*元件*所建立。</span><span class="sxs-lookup"><span data-stu-id="2850d-106"> apps are built using *components*.</span></span> <span data-ttu-id="2850d-107">「元件」（component）是一種獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="2850d-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="2850d-108">元件包含 HTML 標籤，以及插入資料或回應 UI 事件所需的處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="2850d-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="2850d-109">元件具有彈性且輕量。</span><span class="sxs-lookup"><span data-stu-id="2850d-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="2850d-110">它們可以在專案之間進行嵌套、重複使用及共用。</span><span class="sxs-lookup"><span data-stu-id="2850d-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="2850d-111">元件類別</span><span class="sxs-lookup"><span data-stu-id="2850d-111">Component classes</span></span>

<span data-ttu-id="2850d-112">元件會使用C#和 HTML 標籤的組合，在[razor](xref:mvc/views/razor)元件檔案（*razor*）中執行。</span><span class="sxs-lookup"><span data-stu-id="2850d-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="2850d-113">Blazor 中的元件正式稱為*Razor 元件*。</span><span class="sxs-lookup"><span data-stu-id="2850d-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="2850d-114">元件的名稱必須以大寫字元開頭。</span><span class="sxs-lookup"><span data-stu-id="2850d-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="2850d-115">例如， *MyCoolComponent*有效，而*MyCoolComponent*則無效。</span><span class="sxs-lookup"><span data-stu-id="2850d-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="2850d-116">元件的 UI 是使用 HTML 定義的。</span><span class="sxs-lookup"><span data-stu-id="2850d-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="2850d-117">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="2850d-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="2850d-118">編譯應用程式時，會將 HTML 標籤和C#轉譯邏輯轉換成元件類別。</span><span class="sxs-lookup"><span data-stu-id="2850d-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="2850d-119">產生的類別名稱與檔案的名稱相符。</span><span class="sxs-lookup"><span data-stu-id="2850d-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="2850d-120">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="2850d-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="2850d-121">在 `@code` 區塊中，會使用事件處理或定義其他元件邏輯的方法來指定元件狀態（屬性、欄位）。</span><span class="sxs-lookup"><span data-stu-id="2850d-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="2850d-122">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="2850d-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="2850d-123">元件成員可以使用C#開頭為 `@`的運算式，做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="2850d-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="2850d-124">例如， C#欄位是藉由在功能變數名稱前面加上 `@` 來呈現。</span><span class="sxs-lookup"><span data-stu-id="2850d-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="2850d-125">下列範例會評估並呈現：</span><span class="sxs-lookup"><span data-stu-id="2850d-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="2850d-126">`_headingFontStyle` 至 `font-style`的 CSS 屬性值。</span><span class="sxs-lookup"><span data-stu-id="2850d-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="2850d-127">`_headingText` 至 `<h1>` 元素的內容。</span><span class="sxs-lookup"><span data-stu-id="2850d-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="2850d-128">一開始呈現元件之後，元件會重新產生其轉譯樹狀結構，以回應事件。</span><span class="sxs-lookup"><span data-stu-id="2850d-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> Blazor<span data-ttu-id="2850d-129"> 接著會比較新的轉譯樹狀結構與上一個，並將任何修改套用至瀏覽器的檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="2850d-129"> then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="2850d-130">元件是一般C#類別，可以放在專案內的任何位置。</span><span class="sxs-lookup"><span data-stu-id="2850d-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="2850d-131">產生網頁的元件通常會位於*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="2850d-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="2850d-132">非頁面元件通常會放在*共用*資料夾中，或加入至專案的自訂資料夾中。</span><span class="sxs-lookup"><span data-stu-id="2850d-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="2850d-133">一般而言，元件的命名空間是從應用程式的根命名空間和元件在應用程式內的位置（資料夾）衍生而來。</span><span class="sxs-lookup"><span data-stu-id="2850d-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="2850d-134">如果應用程式的根命名空間是 `BlazorApp`，且 `Counter` 元件位於*Pages*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="2850d-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="2850d-135">`Counter` 元件的命名空間為 `BlazorApp.Pages`。</span><span class="sxs-lookup"><span data-stu-id="2850d-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="2850d-136">元件的完整類型名稱為 `BlazorApp.Pages.Counter`。</span><span class="sxs-lookup"><span data-stu-id="2850d-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="2850d-137">如需詳細資訊，請參閱匯[入元件](#import-components)一節。</span><span class="sxs-lookup"><span data-stu-id="2850d-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="2850d-138">若要使用自訂資料夾，請將自訂資料夾的命名空間新增至父元件或應用程式的 *_Imports razor*檔案。</span><span class="sxs-lookup"><span data-stu-id="2850d-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="2850d-139">例如，下列命名空間會在應用程式的根命名空間為 `BlazorApp`時，讓*元件*資料夾中的元件可供使用：</span><span class="sxs-lookup"><span data-stu-id="2850d-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a><span data-ttu-id="2850d-140">靜態資產</span><span class="sxs-lookup"><span data-stu-id="2850d-140">Static assets</span></span>

Blazor<span data-ttu-id="2850d-141"> 遵循 ASP.NET Core 應用程式在專案的[web 根目錄（wwwroot）資料夾](xref:fundamentals/index#web-root)下放置靜態資產的慣例。</span><span class="sxs-lookup"><span data-stu-id="2850d-141"> follows the convention of ASP.NET Core apps placing static assets under the project's [web root (wwwroot) folder](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="2850d-142">使用基底相對路徑（`/`）來參考靜態資產的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="2850d-142">Use a base-relative path (`/`) to refer to the web root for a static asset.</span></span> <span data-ttu-id="2850d-143">在下列範例中，*標誌 .png*實際上位於 *{PROJECT ROOT}/wwwroot/images*資料夾中：</span><span class="sxs-lookup"><span data-stu-id="2850d-143">In the following example, *logo.png* is physically located in the *{PROJECT ROOT}/wwwroot/images* folder:</span></span>

```razor
<img alt="Company logo" src="/images/logo.png" />
```

<span data-ttu-id="2850d-144">Razor 元件**不**支援波形符-斜線標記法（`~/`）。</span><span class="sxs-lookup"><span data-stu-id="2850d-144">Razor components do **not** support tilde-slash notation (`~/`).</span></span>

<span data-ttu-id="2850d-145">如需設定應用程式基底路徑的詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#app-base-path>。</span><span class="sxs-lookup"><span data-stu-id="2850d-145">For information on setting an app's base path, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="tag-helpers-arent-supported-in-components"></a><span data-ttu-id="2850d-146">元件中不支援標記協助程式</span><span class="sxs-lookup"><span data-stu-id="2850d-146">Tag Helpers aren't supported in components</span></span>

<span data-ttu-id="2850d-147">Razor 元件（*razor*檔案）中不支援[標記](xref:mvc/views/tag-helpers/intro)協助程式。</span><span class="sxs-lookup"><span data-stu-id="2850d-147">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="2850d-148">若要在 Blazor中提供標籤協助程式的功能，請建立元件，其功能與標記協助程式相同，並改用元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-148">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="2850d-149">使用元件</span><span class="sxs-lookup"><span data-stu-id="2850d-149">Use components</span></span>

<span data-ttu-id="2850d-150">元件可以包含其他元件，方法是使用 HTML 專案語法來宣告它們。</span><span class="sxs-lookup"><span data-stu-id="2850d-150">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="2850d-151">使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。</span><span class="sxs-lookup"><span data-stu-id="2850d-151">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="2850d-152">在*Index*中的下列標記會呈現 `HeadingComponent` 實例：</span><span class="sxs-lookup"><span data-stu-id="2850d-152">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="2850d-153">*Components/HeadingComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-153">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="2850d-154">如果元件包含的 HTML 專案具有大寫的第一個字母，但不符合元件名稱，則會發出警告，指出該元素有未預期的名稱。</span><span class="sxs-lookup"><span data-stu-id="2850d-154">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="2850d-155">為元件的命名空間加入 `@using` 指示詞可讓元件可用，這會解析警告。</span><span class="sxs-lookup"><span data-stu-id="2850d-155">Adding an `@using` directive for the component's namespace makes the component available, which resolves the warning.</span></span>

## <a name="routing"></a><span data-ttu-id="2850d-156">路由</span><span class="sxs-lookup"><span data-stu-id="2850d-156">Routing</span></span>

<span data-ttu-id="2850d-157">Blazor 中的路由會藉由提供路由範本給應用程式中的每個可存取元件來達到目的。</span><span class="sxs-lookup"><span data-stu-id="2850d-157">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="2850d-158">當您編譯具有 `@page` 指示詞的 Razor 檔案時，會提供指定路由範本 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> 給產生的類別。</span><span class="sxs-lookup"><span data-stu-id="2850d-158">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="2850d-159">在執行時間，路由器會尋找具有 `RouteAttribute` 的元件類別，並轉譯哪一個元件的路由範本符合要求的 URL。</span><span class="sxs-lookup"><span data-stu-id="2850d-159">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

```razor
@page "/ParentComponent"

...
```

<span data-ttu-id="2850d-160">如需詳細資訊，請參閱 <xref:blazor/routing>。</span><span class="sxs-lookup"><span data-stu-id="2850d-160">For more information, see <xref:blazor/routing>.</span></span>

## <a name="parameters"></a><span data-ttu-id="2850d-161">參數</span><span class="sxs-lookup"><span data-stu-id="2850d-161">Parameters</span></span>

### <a name="route-parameters"></a><span data-ttu-id="2850d-162">路由參數</span><span class="sxs-lookup"><span data-stu-id="2850d-162">Route parameters</span></span>

<span data-ttu-id="2850d-163">元件可以從 `@page` 指示詞所提供的路由範本接收路由參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-163">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="2850d-164">路由器會使用路由參數來填入對應的元件參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-164">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="2850d-165">*Pages/RouteParameter. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-165">*Pages/RouteParameter.razor*:</span></span>

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

<span data-ttu-id="2850d-166">不支援選擇性參數，因此在上述範例中會套用兩個 `@page` 的指示詞。</span><span class="sxs-lookup"><span data-stu-id="2850d-166">Optional parameters aren't supported, so two `@page` directives are applied in the preceding example.</span></span> <span data-ttu-id="2850d-167">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-167">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="2850d-168">第二個 `@page` 指示詞會接收 `{text}` 路由參數，並將值指派給 `Text` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-168">The second `@page` directive receives the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="2850d-169">Razor 元件（*razor*）**不**支援*Catch-all*參數語法（`*`/`**`），它會跨多個資料夾界限捕捉路徑。</span><span class="sxs-lookup"><span data-stu-id="2850d-169">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

### <a name="component-parameters"></a><span data-ttu-id="2850d-170">元件參數</span><span class="sxs-lookup"><span data-stu-id="2850d-170">Component parameters</span></span>

<span data-ttu-id="2850d-171">元件可以具有*元件參數*，其使用元件類別上的公用屬性（具有 `[Parameter]` 屬性）來定義。</span><span class="sxs-lookup"><span data-stu-id="2850d-171">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="2850d-172">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="2850d-172">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="2850d-173">*Components/ChildComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-173">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

<span data-ttu-id="2850d-174">在範例應用程式的下列範例中，`ParentComponent` 設定 `ChildComponent`的 `Title` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="2850d-174">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="2850d-175">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-175">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="2850d-176">子內容</span><span class="sxs-lookup"><span data-stu-id="2850d-176">Child content</span></span>

<span data-ttu-id="2850d-177">元件可以設定另一個元件的內容。</span><span class="sxs-lookup"><span data-stu-id="2850d-177">Components can set the content of another component.</span></span> <span data-ttu-id="2850d-178">指派元件會在指定接收元件的標記之間提供內容。</span><span class="sxs-lookup"><span data-stu-id="2850d-178">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="2850d-179">在下列範例中，`ChildComponent` 具有代表 `RenderFragment`的 `ChildContent` 屬性，代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="2850d-179">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="2850d-180">`ChildContent` 的值位於元件的標記中，應在其中呈現內容。</span><span class="sxs-lookup"><span data-stu-id="2850d-180">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="2850d-181">`ChildContent` 的值會從父元件接收，並在啟動程式面板的 `panel-body`內轉譯。</span><span class="sxs-lookup"><span data-stu-id="2850d-181">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="2850d-182">*Components/ChildComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="2850d-183">接收 `RenderFragment` 內容的屬性必須依照慣例命名 `ChildContent`。</span><span class="sxs-lookup"><span data-stu-id="2850d-183">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="2850d-184">範例應用程式中的 `ParentComponent` 可以藉由將內容放入 `<ChildComponent>` 標記中，來提供呈現 `ChildComponent` 的內容。</span><span class="sxs-lookup"><span data-stu-id="2850d-184">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="2850d-185">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-185">*Pages/ParentComponent.razor*:</span></span>

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="2850d-186">屬性展開和任意參數</span><span class="sxs-lookup"><span data-stu-id="2850d-186">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="2850d-187">除了元件的宣告參數之外，元件還可以捕捉和轉譯其他屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-187">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="2850d-188">您可以在字典中捕捉其他屬性，然後在使用[`@attributes`](xref:mvc/views/razor#attributes) Razor 指示詞轉譯元件時，將其*splatted*到元素上。</span><span class="sxs-lookup"><span data-stu-id="2850d-188">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="2850d-189">當定義的元件會產生支援各種自訂的標記專案時，這個案例就很有用。</span><span class="sxs-lookup"><span data-stu-id="2850d-189">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="2850d-190">例如，針對支援許多參數的 `<input>` 分別定義屬性，可能會很繁瑣。</span><span class="sxs-lookup"><span data-stu-id="2850d-190">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="2850d-191">在下列範例中，第一個 `<input>` 元素（`id="useIndividualParams"`）使用個別的元件參數，而第二個 `<input>` 元素（`id="useAttributesDict"`）使用屬性展開：</span><span class="sxs-lookup"><span data-stu-id="2850d-191">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="2850d-192">參數的類型必須使用字串索引鍵來執行 `IEnumerable<KeyValuePair<string, object>>`。</span><span class="sxs-lookup"><span data-stu-id="2850d-192">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="2850d-193">在此案例中，使用 `IReadOnlyDictionary<string, object>` 也是一個選項。</span><span class="sxs-lookup"><span data-stu-id="2850d-193">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="2850d-194">使用這兩種方法轉譯的 `<input>` 元素都相同：</span><span class="sxs-lookup"><span data-stu-id="2850d-194">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="2850d-195">若要接受任意屬性，請使用 `[Parameter]` 屬性定義元件參數，並將 `CaptureUnmatchedValues` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="2850d-195">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="2850d-196">`[Parameter]` 上的 `CaptureUnmatchedValues` 屬性可讓參數符合所有不符合任何其他參數的屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-196">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="2850d-197">元件只能定義具有 `CaptureUnmatchedValues`的單一參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-197">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="2850d-198">搭配 `CaptureUnmatchedValues` 使用的屬性類型必須可從具有字串索引鍵的 `Dictionary<string, object>` 指派。</span><span class="sxs-lookup"><span data-stu-id="2850d-198">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="2850d-199">`IEnumerable<KeyValuePair<string, object>>` 或 `IReadOnlyDictionary<string, object>` 也是此案例中的選項。</span><span class="sxs-lookup"><span data-stu-id="2850d-199">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="2850d-200">相對於元素屬性位置的 `@attributes` 位置很重要。</span><span class="sxs-lookup"><span data-stu-id="2850d-200">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="2850d-201">當 `@attributes` 在專案上 splatted 時，會從右至左（最後一個）處理屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-201">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="2850d-202">請考慮使用 `Child` 元件的下列元件範例：</span><span class="sxs-lookup"><span data-stu-id="2850d-202">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="2850d-203">*ParentComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-203">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="2850d-204">*ChildComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-204">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="2850d-205">`Child` 元件的 `extra` 屬性會設定為 `@attributes`的右邊。</span><span class="sxs-lookup"><span data-stu-id="2850d-205">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="2850d-206">`Parent` 元件的轉譯 `<div>` 會在透過額外屬性傳遞時包含 `extra="5"`，因為屬性是由右至左處理（最後一個）：</span><span class="sxs-lookup"><span data-stu-id="2850d-206">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="2850d-207">在下列範例中，`extra` 和 `@attributes` 的順序會在 `Child` 元件的 `<div>`中反轉：</span><span class="sxs-lookup"><span data-stu-id="2850d-207">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="2850d-208">*ParentComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-208">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="2850d-209">*ChildComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-209">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="2850d-210">當透過其他屬性傳遞時，`Parent` 元件中呈現的 `<div>` 會包含 `extra="10"`：</span><span class="sxs-lookup"><span data-stu-id="2850d-210">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a><span data-ttu-id="2850d-211">捕獲元件的參考</span><span class="sxs-lookup"><span data-stu-id="2850d-211">Capture references to components</span></span>

<span data-ttu-id="2850d-212">元件參考提供參考元件實例的方法，讓您可以發出命令給該實例，例如 `Show` 或 `Reset`。</span><span class="sxs-lookup"><span data-stu-id="2850d-212">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="2850d-213">若要捕捉元件參考：</span><span class="sxs-lookup"><span data-stu-id="2850d-213">To capture a component reference:</span></span>

* <span data-ttu-id="2850d-214">將[`@ref`](xref:mvc/views/razor#ref)屬性加入至子元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-214">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="2850d-215">定義與子元件類型相同的欄位。</span><span class="sxs-lookup"><span data-stu-id="2850d-215">Define a field with the same type as the child component.</span></span>

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

<span data-ttu-id="2850d-216">當元件呈現時，`_loginDialog` 欄位會填入 `MyLoginDialog` 子元件實例。</span><span class="sxs-lookup"><span data-stu-id="2850d-216">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="2850d-217">接著，您可以在元件實例上叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="2850d-217">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2850d-218">只有在轉譯元件之後才會填入 `_loginDialog` 變數，且其輸出會包含 `MyLoginDialog` 元素。</span><span class="sxs-lookup"><span data-stu-id="2850d-218">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="2850d-219">直到該點為止，沒有任何可參考的內容。</span><span class="sxs-lookup"><span data-stu-id="2850d-219">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="2850d-220">若要在元件完成呈現之後操作元件參考，請使用[OnAfterRenderAsync 或 OnAfterRender 方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="2850d-220">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="2850d-221">若要參考迴圈中的元件，請參閱[捕捉多個類似子元件的參考（dotnet/aspnetcore #13358）](https://github.com/dotnet/aspnetcore/issues/13358)。</span><span class="sxs-lookup"><span data-stu-id="2850d-221">To reference components in a loop, see [Capture references to multiple similar child-components (dotnet/aspnetcore #13358)](https://github.com/dotnet/aspnetcore/issues/13358).</span></span>

<span data-ttu-id="2850d-222">雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)，但它並不是 JavaScript interop 功能。</span><span class="sxs-lookup"><span data-stu-id="2850d-222">While capturing component references use a similar syntax to [capturing element references](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements), it isn't a JavaScript interop feature.</span></span> <span data-ttu-id="2850d-223">元件參考不會傳遞至 JavaScript 程式碼，&mdash;它們只會在 .NET 程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="2850d-223">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="2850d-224">請勿**使用元件**參考來改變子元件的狀態。</span><span class="sxs-lookup"><span data-stu-id="2850d-224">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="2850d-225">請改用一般宣告式參數，將資料傳遞至子元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-225">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="2850d-226">使用一般宣告式參數會導致子元件自動 rerender 正確的時間。</span><span class="sxs-lookup"><span data-stu-id="2850d-226">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="2850d-227">在外部叫用元件方法來更新狀態</span><span class="sxs-lookup"><span data-stu-id="2850d-227">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="2850d-228"> 會使用 `SynchronizationContext` 來強制執行單一邏輯執行緒。</span><span class="sxs-lookup"><span data-stu-id="2850d-228"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="2850d-229">元件的[生命週期方法](xref:blazor/lifecycle)和 Blazor 所引發的任何事件回呼都會在此 `SynchronizationContext`上執行。</span><span class="sxs-lookup"><span data-stu-id="2850d-229">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="2850d-230">在事件中，必須根據外來事件（例如計時器或其他通知）更新元件，請使用 `InvokeAsync` 方法，這會分派給 Blazor的 `SynchronizationContext`。</span><span class="sxs-lookup"><span data-stu-id="2850d-230">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="2850d-231">例如，假設有一個通知程式*服務*可通知任何處于已更新狀態的「接聽」元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-231">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="2850d-232">將 `NotifierService` 註冊為 singletion：</span><span class="sxs-lookup"><span data-stu-id="2850d-232">Register the `NotifierService` as a singletion:</span></span>

* <span data-ttu-id="2850d-233">在 Blazor WebAssembly 中，于 `Program.Main`註冊服務：</span><span class="sxs-lookup"><span data-stu-id="2850d-233">In Blazor WebAssembly, register the service in `Program.Main`:</span></span>

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* <span data-ttu-id="2850d-234">在 Blazor 伺服器中，于 `Startup.ConfigureServices`註冊服務：</span><span class="sxs-lookup"><span data-stu-id="2850d-234">In Blazor Server, register the service in `Startup.ConfigureServices`:</span></span>

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

<span data-ttu-id="2850d-235">使用 `NotifierService` 來更新元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-235">Use the `NotifierService` to update a component:</span></span>

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

<span data-ttu-id="2850d-236">在上述範例中，`NotifierService` 會在 Blazor的 `SynchronizationContext`之外叫用元件的 `OnNotify` 方法。</span><span class="sxs-lookup"><span data-stu-id="2850d-236">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="2850d-237">`InvokeAsync` 可用來切換至正確的內容，並將轉譯排入佇列。</span><span class="sxs-lookup"><span data-stu-id="2850d-237">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="2850d-238">使用 \@鍵來控制元素和元件的保留</span><span class="sxs-lookup"><span data-stu-id="2850d-238">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="2850d-239">當轉譯專案或元件的清單，以及後續變更的專案或元件時，Blazor的比較演算法必須決定哪些先前的專案或元件可以保留，以及模型物件應如何對應到它們。</span><span class="sxs-lookup"><span data-stu-id="2850d-239">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="2850d-240">一般來說，此程式是自動的，可以忽略，但在某些情況下，您可能會想要控制進程。</span><span class="sxs-lookup"><span data-stu-id="2850d-240">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="2850d-241">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="2850d-241">Consider the following example:</span></span>

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

<span data-ttu-id="2850d-242">`People` 集合的內容可能會隨著插入、刪除或重新排序的專案而變更。</span><span class="sxs-lookup"><span data-stu-id="2850d-242">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="2850d-243">當元件 rerenders 時，`<DetailsEditor>` 元件可能會變更，以接收不同的 `Details` 參數值。</span><span class="sxs-lookup"><span data-stu-id="2850d-243">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="2850d-244">這可能會導致比預期更複雜的 rerendering。</span><span class="sxs-lookup"><span data-stu-id="2850d-244">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="2850d-245">在某些情況下，rerendering 可能會導致可見的行為差異，例如失去元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="2850d-245">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="2850d-246">您可以使用 `@key` 指示詞屬性來控制對應進程。</span><span class="sxs-lookup"><span data-stu-id="2850d-246">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="2850d-247">`@key` 會使比較演算法保證根據索引鍵的值來保留元素或元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-247">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="2850d-248">當 `People` 集合變更時，比較演算法會保留 `<DetailsEditor>` 實例與 `person` 實例之間的關聯：</span><span class="sxs-lookup"><span data-stu-id="2850d-248">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="2850d-249">如果 `Person` 從 `People` 清單中刪除，則只會從 UI 移除對應的 `<DetailsEditor>` 實例。</span><span class="sxs-lookup"><span data-stu-id="2850d-249">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="2850d-250">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="2850d-250">Other instances are left unchanged.</span></span>
* <span data-ttu-id="2850d-251">如果在清單中的某個位置插入 `Person`，就會在該對應的位置插入一個新的 `<DetailsEditor>` 實例。</span><span class="sxs-lookup"><span data-stu-id="2850d-251">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="2850d-252">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="2850d-252">Other instances are left unchanged.</span></span>
* <span data-ttu-id="2850d-253">如果重新排序 `Person` 專案，則會保留對應的 `<DetailsEditor>` 實例，並在 UI 中重新排序。</span><span class="sxs-lookup"><span data-stu-id="2850d-253">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="2850d-254">在某些情況下，使用 `@key` 會將 rerendering 的複雜性降到最低，並避免 DOM 的具狀態部分可能發生的問題，例如焦點位置。</span><span class="sxs-lookup"><span data-stu-id="2850d-254">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2850d-255">索引鍵在每個容器元素或元件的本機。</span><span class="sxs-lookup"><span data-stu-id="2850d-255">Keys are local to each container element or component.</span></span> <span data-ttu-id="2850d-256">金鑰不會在檔之間進行全域比較。</span><span class="sxs-lookup"><span data-stu-id="2850d-256">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="2850d-257">使用 \@金鑰的時機</span><span class="sxs-lookup"><span data-stu-id="2850d-257">When to use \@key</span></span>

<span data-ttu-id="2850d-258">一般而言，只要轉譯清單（例如，在 `@foreach` 區塊中），而且有適當的值來定義 `@key`，就有合理的使用 `@key`。</span><span class="sxs-lookup"><span data-stu-id="2850d-258">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="2850d-259">當物件變更時，您也可以使用 `@key` 來防止 Blazor 保留元素或元件子樹：</span><span class="sxs-lookup"><span data-stu-id="2850d-259">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="2850d-260">如果 `@currentPerson` 變更，`@key` attribute 指示詞會強制 Blazor 捨棄整個 `<div>` 及其下階，並使用新的元素和元件重建 UI 內的子樹。</span><span class="sxs-lookup"><span data-stu-id="2850d-260">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="2850d-261">如果您需要保證 `@currentPerson` 變更時不會保留任何 UI 狀態，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="2850d-261">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="2850d-262">不使用 \@金鑰的時機</span><span class="sxs-lookup"><span data-stu-id="2850d-262">When not to use \@key</span></span>

<span data-ttu-id="2850d-263">與 `@key`進行比較時，會產生效能成本。</span><span class="sxs-lookup"><span data-stu-id="2850d-263">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="2850d-264">效能成本並不大，但只有在控制元素或元件保留規則以受益于應用程式時，才指定 `@key`。</span><span class="sxs-lookup"><span data-stu-id="2850d-264">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="2850d-265">即使未使用 `@key`，Blazor 也會盡可能保留子項目和元件實例。</span><span class="sxs-lookup"><span data-stu-id="2850d-265">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="2850d-266">使用 `@key` 的唯一優點是控制模型實例*如何*對應至保留的元件實例，而不是用來選取對應的比較演算法。</span><span class="sxs-lookup"><span data-stu-id="2850d-266">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="2850d-267">要用於 \@金鑰的值</span><span class="sxs-lookup"><span data-stu-id="2850d-267">What values to use for \@key</span></span>

<span data-ttu-id="2850d-268">一般來說，提供下列其中一種類型的 `@key`值是合理的：</span><span class="sxs-lookup"><span data-stu-id="2850d-268">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="2850d-269">模型物件實例（例如，如先前範例所示的 `Person` 實例）。</span><span class="sxs-lookup"><span data-stu-id="2850d-269">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="2850d-270">這可確保根據物件參考的相等性進行保留。</span><span class="sxs-lookup"><span data-stu-id="2850d-270">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="2850d-271">唯一識別碼（例如，`int`、`string`或 `Guid`類型的主要金鑰值）。</span><span class="sxs-lookup"><span data-stu-id="2850d-271">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="2850d-272">請確定用於 `@key` 的值不會造成衝突。</span><span class="sxs-lookup"><span data-stu-id="2850d-272">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="2850d-273">如果在相同的父元素中偵測到衝突值，Blazor 會擲回例外狀況，因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-273">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="2850d-274">只使用不同的值，例如物件實例或主鍵值。</span><span class="sxs-lookup"><span data-stu-id="2850d-274">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="2850d-275">部分類別支援</span><span class="sxs-lookup"><span data-stu-id="2850d-275">Partial class support</span></span>

<span data-ttu-id="2850d-276">Razor 元件是以部分類別的形式產生。</span><span class="sxs-lookup"><span data-stu-id="2850d-276">Razor components are generated as partial classes.</span></span> <span data-ttu-id="2850d-277">Razor 元件是使用下列其中一種方法來撰寫的：</span><span class="sxs-lookup"><span data-stu-id="2850d-277">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="2850d-278">C#程式碼是以 HTML 標籤和 Razor 程式碼的[`@code`](xref:mvc/views/razor#code)區塊定義在單一檔案中。</span><span class="sxs-lookup"><span data-stu-id="2850d-278">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="2850d-279"> 範本會使用這種方法來定義其 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-279"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="2850d-280">C#程式碼會放在定義為部分類別的程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="2850d-280">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="2850d-281">下列範例顯示在從 Blazor 範本產生的應用程式中，具有 `@code` 區塊的預設 `Counter` 元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-281">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="2850d-282">HTML 標籤、Razor 程式碼和C#程式碼位於相同的檔案中：</span><span class="sxs-lookup"><span data-stu-id="2850d-282">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="2850d-283">*Counter. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-283">*Counter.razor*:</span></span>

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

<span data-ttu-id="2850d-284">您也可以使用具有部分類別的程式碼後置檔案來建立 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-284">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="2850d-285">*Counter. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-285">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="2850d-286">*Counter.razor.cs*：</span><span class="sxs-lookup"><span data-stu-id="2850d-286">*Counter.razor.cs*:</span></span>

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

<span data-ttu-id="2850d-287">視需要將任何必要的命名空間新增至部分類別檔案。</span><span class="sxs-lookup"><span data-stu-id="2850d-287">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="2850d-288">Razor 元件所使用的一般命名空間包括：</span><span class="sxs-lookup"><span data-stu-id="2850d-288">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="2850d-289">指定基類</span><span class="sxs-lookup"><span data-stu-id="2850d-289">Specify a base class</span></span>

<span data-ttu-id="2850d-290">[`@inherits`](xref:mvc/views/razor#inherits)指示詞可以用來指定元件的基類。</span><span class="sxs-lookup"><span data-stu-id="2850d-290">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="2850d-291">下列範例會顯示元件如何繼承基類（`BlazorRocksBase`），以提供元件的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="2850d-291">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="2850d-292">基類應該衍生自 `ComponentBase`。</span><span class="sxs-lookup"><span data-stu-id="2850d-292">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="2850d-293">*Pages/BlazorRocks. razor*：</span><span class="sxs-lookup"><span data-stu-id="2850d-293">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="2850d-294">*BlazorRocksBase.cs*：</span><span class="sxs-lookup"><span data-stu-id="2850d-294">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="2850d-295">指定屬性</span><span class="sxs-lookup"><span data-stu-id="2850d-295">Specify an attribute</span></span>

<span data-ttu-id="2850d-296">您可以使用[`@attribute`](xref:mvc/views/razor#attribute)指示詞，在 Razor 元件中指定屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-296">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="2850d-297">下列範例會將 `[Authorize]` 屬性套用至元件類別：</span><span class="sxs-lookup"><span data-stu-id="2850d-297">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="2850d-298">匯入元件</span><span class="sxs-lookup"><span data-stu-id="2850d-298">Import components</span></span>

<span data-ttu-id="2850d-299">以 Razor 撰寫之元件的命名空間是根據（依優先順序排列）：</span><span class="sxs-lookup"><span data-stu-id="2850d-299">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="2850d-300">Razor 檔案（*razor*）標記中的[`@namespace`](xref:mvc/views/razor#namespace)指定（`@namespace BlazorSample.MyNamespace`）。</span><span class="sxs-lookup"><span data-stu-id="2850d-300">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="2850d-301">專案檔的 `RootNamespace` （`<RootNamespace>BlazorSample</RootNamespace>`）。</span><span class="sxs-lookup"><span data-stu-id="2850d-301">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="2850d-302">從專案檔的檔案名（ *.csproj*）取得的專案名稱，以及從專案根目錄到元件的路徑。</span><span class="sxs-lookup"><span data-stu-id="2850d-302">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="2850d-303">例如，架構會將 *{PROJECT ROOT}/Pages/Index.razor* （*BlazorSample*）解析為 `BlazorSample.Pages`的命名空間。</span><span class="sxs-lookup"><span data-stu-id="2850d-303">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="2850d-304">元件會C#遵循名稱系結規則。</span><span class="sxs-lookup"><span data-stu-id="2850d-304">Components follow C# name binding rules.</span></span> <span data-ttu-id="2850d-305">針對此範例中的 `Index` 元件，範圍內的元件都是元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-305">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="2850d-306">在相同的資料夾中，*頁面*。</span><span class="sxs-lookup"><span data-stu-id="2850d-306">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="2850d-307">專案根目錄中未明確指定不同命名空間的元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-307">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="2850d-308">使用 Razor 的[`@using`](xref:mvc/views/razor#using)指示詞，在不同的命名空間中定義的元件會帶入範圍中。</span><span class="sxs-lookup"><span data-stu-id="2850d-308">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="2850d-309">如果*BlazorSample/Shared/* 資料夾中有另一個元件（`NavMenu.razor`）存在，則元件可用於 `Index.razor`，並具有下列 `@using` 語句：</span><span class="sxs-lookup"><span data-stu-id="2850d-309">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="2850d-310">元件也可以使用其完整名稱來參考，這不需要[`@using`](xref:mvc/views/razor#using)指示詞：</span><span class="sxs-lookup"><span data-stu-id="2850d-310">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="2850d-311">不支援 `global::` 限定性。</span><span class="sxs-lookup"><span data-stu-id="2850d-311">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="2850d-312">不支援匯入具有別名 `using` 語句的元件（例如，`@using Foo = Bar`）。</span><span class="sxs-lookup"><span data-stu-id="2850d-312">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="2850d-313">不支援部分限定的名稱。</span><span class="sxs-lookup"><span data-stu-id="2850d-313">Partially qualified names aren't supported.</span></span> <span data-ttu-id="2850d-314">例如，不支援使用 `<Shared.NavMenu></Shared.NavMenu>` 來新增 `@using BlazorSample` 和參考 `NavMenu.razor`。</span><span class="sxs-lookup"><span data-stu-id="2850d-314">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="2850d-315">條件式 HTML 元素屬性</span><span class="sxs-lookup"><span data-stu-id="2850d-315">Conditional HTML element attributes</span></span>

<span data-ttu-id="2850d-316">HTML 專案屬性會根據 .NET 值有條件地呈現。</span><span class="sxs-lookup"><span data-stu-id="2850d-316">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="2850d-317">如果 `false` 或 `null`值，則不會呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-317">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="2850d-318">如果 `true`值，則會將屬性轉譯為最小化。</span><span class="sxs-lookup"><span data-stu-id="2850d-318">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="2850d-319">在下列範例中，`IsCompleted` 會決定是否要在元素的標記中轉譯 `checked`：</span><span class="sxs-lookup"><span data-stu-id="2850d-319">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="2850d-320">如果 `true``IsCompleted`，則會將核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="2850d-320">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="2850d-321">如果 `false``IsCompleted`，則會將核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="2850d-321">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="2850d-322">如需詳細資訊，請參閱 <xref:mvc/views/razor>。</span><span class="sxs-lookup"><span data-stu-id="2850d-322">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="2850d-323">當 .NET 類型為 `bool`時，某些 HTML 屬性（例如，由[aria 按下](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)）無法正常運作。</span><span class="sxs-lookup"><span data-stu-id="2850d-323">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="2850d-324">在這些情況下，請使用 `string` 類型，而不是 `bool`。</span><span class="sxs-lookup"><span data-stu-id="2850d-324">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="2850d-325">原始 HTML</span><span class="sxs-lookup"><span data-stu-id="2850d-325">Raw HTML</span></span>

<span data-ttu-id="2850d-326">字串通常會使用 DOM 文位元組點來呈現，這表示它們可能包含的任何標記都會被忽略，並視為常值。</span><span class="sxs-lookup"><span data-stu-id="2850d-326">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="2850d-327">若要呈現原始 HTML，請將 HTML 內容包裝 `MarkupString` 值。</span><span class="sxs-lookup"><span data-stu-id="2850d-327">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="2850d-328">此值會剖析為 HTML 或 SVG，並插入 DOM 中。</span><span class="sxs-lookup"><span data-stu-id="2850d-328">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="2850d-329">轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**，應予以避免！</span><span class="sxs-lookup"><span data-stu-id="2850d-329">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="2850d-330">下列範例顯示如何使用 `MarkupString` 類型，將靜態 HTML 內容的區塊新增至元件的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="2850d-330">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="2850d-331">級聯的值和參數</span><span class="sxs-lookup"><span data-stu-id="2850d-331">Cascading values and parameters</span></span>

<span data-ttu-id="2850d-332">在某些情況下，使用[元件參數](#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。</span><span class="sxs-lookup"><span data-stu-id="2850d-332">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="2850d-333">串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="2850d-333">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="2850d-334">級聯的值和參數也會提供一種方法來協調元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-334">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="2850d-335">主題範例</span><span class="sxs-lookup"><span data-stu-id="2850d-335">Theme example</span></span>

<span data-ttu-id="2850d-336">在範例應用程式的下列範例中，`ThemeInfo` 類別會指定要向下流動元件階層的主題資訊，讓應用程式中指定部分內的所有按鈕共用相同的樣式。</span><span class="sxs-lookup"><span data-stu-id="2850d-336">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="2850d-337">*UIThemeClasses/ThemeInfo .cs*：</span><span class="sxs-lookup"><span data-stu-id="2850d-337">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="2850d-338">祖系元件可以使用串聯值元件來提供串聯值。</span><span class="sxs-lookup"><span data-stu-id="2850d-338">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="2850d-339">`CascadingValue` 元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。</span><span class="sxs-lookup"><span data-stu-id="2850d-339">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="2850d-340">例如，範例應用程式會在其中一個應用程式的配置中，將主題資訊（`ThemeInfo`）指定為構成 `@Body` 屬性之配置主體的所有元件的串聯參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-340">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="2850d-341">`ButtonClass` 會在版面配置元件中指派 `btn-success` 的值。</span><span class="sxs-lookup"><span data-stu-id="2850d-341">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="2850d-342">任何子代元件都可以透過 `ThemeInfo` 級聯的物件來取用這個屬性。</span><span class="sxs-lookup"><span data-stu-id="2850d-342">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="2850d-343">`CascadingValuesParametersLayout` 元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-343">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="2850d-344">為了利用串聯值，元件會使用 `[CascadingParameter]` 屬性宣告串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-344">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="2850d-345">串聯式值會依類型系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-345">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="2850d-346">在範例應用程式中，`CascadingValuesParametersTheme` 元件會將 `ThemeInfo` 級聯的值系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="2850d-346">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="2850d-347">參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="2850d-347">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="2850d-348">`CascadingValuesParametersTheme` 元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-348">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="2850d-349">若要在相同的子樹中串聯多個相同類型的值，請為每個 `CascadingValue` 元件和其對應的 `CascadingParameter`提供唯一的 `Name` 字串。</span><span class="sxs-lookup"><span data-stu-id="2850d-349">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="2850d-350">在下列範例中，兩個 `CascadingValue` 元件會依名稱將不同的實例 `MyCascadingType`：</span><span class="sxs-lookup"><span data-stu-id="2850d-350">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

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

<span data-ttu-id="2850d-351">在子系元件中，串聯的參數會以名稱從上階元件中對應的串聯值接收其值：</span><span class="sxs-lookup"><span data-stu-id="2850d-351">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="2850d-352">TabSet 範例</span><span class="sxs-lookup"><span data-stu-id="2850d-352">TabSet example</span></span>

<span data-ttu-id="2850d-353">串聯式參數也可以讓元件在元件階層之間共同作業。</span><span class="sxs-lookup"><span data-stu-id="2850d-353">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="2850d-354">例如，請考慮範例應用程式中的下列*TabSet*範例。</span><span class="sxs-lookup"><span data-stu-id="2850d-354">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="2850d-355">範例應用程式有一個 `ITab` 的介面，可執行索引標籤：</span><span class="sxs-lookup"><span data-stu-id="2850d-355">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="2850d-356">`CascadingValuesParametersTabSet` 元件會使用 `TabSet` 元件，其中包含數個 `Tab` 元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-356">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="2850d-357">子 `Tab` 元件不會明確地當做參數傳遞至 `TabSet`。</span><span class="sxs-lookup"><span data-stu-id="2850d-357">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="2850d-358">相反地，子 `Tab` 元件是 `TabSet`之子內容的一部分。</span><span class="sxs-lookup"><span data-stu-id="2850d-358">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="2850d-359">不過，`TabSet` 還是必須知道每個 `Tab` 元件，才能轉譯標頭和使用中的索引標籤。若要在不需要額外程式碼的情況下啟用這項協調，`TabSet` 元件*可以將其本身提供為*串聯的值，然後由子 `Tab` 元件挑選。</span><span class="sxs-lookup"><span data-stu-id="2850d-359">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="2850d-360">`TabSet` 元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-360">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="2850d-361">子系 `Tab` 元件會將包含的 `TabSet` 作為串聯參數來捕捉，因此 `Tab` 元件會將自己加入至 `TabSet`，並對作用中的索引標籤進行協調。</span><span class="sxs-lookup"><span data-stu-id="2850d-361">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="2850d-362">`Tab` 元件：</span><span class="sxs-lookup"><span data-stu-id="2850d-362">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="2850d-363">Razor 範本</span><span class="sxs-lookup"><span data-stu-id="2850d-363">Razor templates</span></span>

<span data-ttu-id="2850d-364">轉譯片段可以使用 Razor 範本語法來定義。</span><span class="sxs-lookup"><span data-stu-id="2850d-364">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="2850d-365">Razor 範本是定義 UI 程式碼片段並採用下列格式的方式：</span><span class="sxs-lookup"><span data-stu-id="2850d-365">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="2850d-366">下列範例說明如何指定 `RenderFragment` 和 `RenderFragment<T>` 值，並直接在元件中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="2850d-366">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="2850d-367">轉譯片段也可以當做引數傳遞至樣板[化元件](xref:blazor/templated-components)。</span><span class="sxs-lookup"><span data-stu-id="2850d-367">Render fragments can also be passed as arguments to [templated components](xref:blazor/templated-components).</span></span>

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

<span data-ttu-id="2850d-368">上述程式碼的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="2850d-368">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="2850d-369">可擴充向量圖形（SVG）影像</span><span class="sxs-lookup"><span data-stu-id="2850d-369">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="2850d-370">由於 Blazor 會轉譯 HTML，因此瀏覽器支援的影像（包括可擴充的向量圖形（SVG）影像（*svg*））可透過 `<img>` 標記來支援：</span><span class="sxs-lookup"><span data-stu-id="2850d-370">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="2850d-371">同樣地，樣式表單檔案（ *.css*）的 CSS 規則也支援 SVG 影像：</span><span class="sxs-lookup"><span data-stu-id="2850d-371">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="2850d-372">不過，在所有案例中不支援內嵌 SVG 標記。</span><span class="sxs-lookup"><span data-stu-id="2850d-372">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="2850d-373">如果您將 `<svg>` 標籤直接放入元件檔（*razor*）中，則會支援基本映射轉譯，但尚不支援許多先進的案例。</span><span class="sxs-lookup"><span data-stu-id="2850d-373">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="2850d-374">例如，目前未遵守 `<use>` 標籤，且 `@bind` 無法與某些 SVG 標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="2850d-374">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="2850d-375">我們希望在未來的版本中解決這些限制。</span><span class="sxs-lookup"><span data-stu-id="2850d-375">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2850d-376">其他資源</span><span class="sxs-lookup"><span data-stu-id="2850d-376">Additional resources</span></span>

* <span data-ttu-id="2850d-377"><xref:security/blazor/server> &ndash; 包含有關建立 Blazor 伺服器應用程式的指導方針，必須與資源耗盡爭用。</span><span class="sxs-lookup"><span data-stu-id="2850d-377"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
