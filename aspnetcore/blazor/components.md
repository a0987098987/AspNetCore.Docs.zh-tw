---
title: 建立和使用 ASP.NET Core Razor 元件
author: guardrex
description: 瞭解如何建立和使用 Razor 元件，包括如何系結至資料、處理事件，以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: blazor/components
ms.openlocfilehash: a71bbf3921417cbd23aeb14d0d78ad8354d6e93a
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378694"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="31b3c-103">建立和使用 ASP.NET Core Razor 元件</span><span class="sxs-lookup"><span data-stu-id="31b3c-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="31b3c-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="31b3c-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="31b3c-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="31b3c-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="31b3c-106">Blazor 應用程式是使用*元件*所建立。</span><span class="sxs-lookup"><span data-stu-id="31b3c-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="31b3c-107">「元件」（component）是一種獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="31b3c-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="31b3c-108">元件包含 HTML 標籤，以及插入資料或回應 UI 事件所需的處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="31b3c-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="31b3c-109">元件具有彈性且輕量。</span><span class="sxs-lookup"><span data-stu-id="31b3c-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="31b3c-110">它們可以在專案之間進行嵌套、重複使用及共用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="31b3c-111">元件類別</span><span class="sxs-lookup"><span data-stu-id="31b3c-111">Component classes</span></span>

<span data-ttu-id="31b3c-112">元件會使用C#和 HTML 標籤的組合，在[razor](xref:mvc/views/razor)元件檔案（*razor*）中執行。</span><span class="sxs-lookup"><span data-stu-id="31b3c-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="31b3c-113">Blazor 中的元件正式稱為*Razor 元件*。</span><span class="sxs-lookup"><span data-stu-id="31b3c-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="31b3c-114">元件的名稱必須以大寫字元開頭。</span><span class="sxs-lookup"><span data-stu-id="31b3c-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="31b3c-115">例如， *MyCoolComponent*有效，而*MyCoolComponent*則無效。</span><span class="sxs-lookup"><span data-stu-id="31b3c-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="31b3c-116">元件的 UI 是使用 HTML 定義的。</span><span class="sxs-lookup"><span data-stu-id="31b3c-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="31b3c-117">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="31b3c-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="31b3c-118">編譯應用程式時，會將 HTML 標籤和C#轉譯邏輯轉換成元件類別。</span><span class="sxs-lookup"><span data-stu-id="31b3c-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="31b3c-119">產生的類別名稱與檔案的名稱相符。</span><span class="sxs-lookup"><span data-stu-id="31b3c-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="31b3c-120">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="31b3c-121">在 `@code` 區塊中，元件狀態（屬性、欄位）是以事件處理或定義其他元件邏輯的方法來指定。</span><span class="sxs-lookup"><span data-stu-id="31b3c-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="31b3c-122">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="31b3c-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="31b3c-123">在 ASP.NET Core 3.0 的先前預覽中，`@functions` 區塊用於與 Razor 元件中 @no__t 1 區塊相同的用途。</span><span class="sxs-lookup"><span data-stu-id="31b3c-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="31b3c-124">`@functions` 區塊會繼續在 Razor 元件中運作，但我們建議您在 ASP.NET Core 3.0 Preview 6 或更新版本中使用 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="31b3c-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="31b3c-125">元件成員可以使用C#開頭為 `@` 的運算式，做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="31b3c-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="31b3c-126">例如， C#欄位的呈現方式是在功能變數名稱前面加上 `@`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="31b3c-127">下列範例會評估並呈現：</span><span class="sxs-lookup"><span data-stu-id="31b3c-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="31b3c-128">`_headingFontStyle` 到 `font-style` 的 CSS 屬性值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="31b3c-129">`_headingText` 到 `<h1>` 元素的內容。</span><span class="sxs-lookup"><span data-stu-id="31b3c-129">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="31b3c-130">一開始呈現元件之後，元件會重新產生其轉譯樹狀結構，以回應事件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="31b3c-131">然後，Blazor 會比較新的轉譯樹狀結構與上一個，並將任何修改套用至瀏覽器的檔物件模型（DOM）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="31b3c-132">元件是一般C#類別，可以放在專案內的任何位置。</span><span class="sxs-lookup"><span data-stu-id="31b3c-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="31b3c-133">產生網頁的元件通常會位於*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="31b3c-134">非頁面元件通常會放在*共用*資料夾中，或加入至專案的自訂資料夾中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="31b3c-135">若要使用自訂資料夾，請將自訂資料夾的命名空間新增至父元件或應用程式的 *_Imports*檔案。</span><span class="sxs-lookup"><span data-stu-id="31b3c-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="31b3c-136">例如，下列命名空間會在應用程式的根命名空間為 `WebApplication` 時，讓*元件*資料夾中的元件可供使用：</span><span class="sxs-lookup"><span data-stu-id="31b3c-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="31b3c-137">將元件整合到 Razor Pages 和 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="31b3c-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="31b3c-138">將元件與現有的 Razor Pages 和 MVC 應用程式搭配使用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="31b3c-139">不需要重新撰寫現有的頁面或 views 就能使用 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="31b3c-140">當頁面或視圖呈現時，會同時資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="31b3c-141">若要從頁面或視圖呈現元件，請使用 `RenderComponentAsync<TComponent>` HTML helper 方法：</span><span class="sxs-lookup"><span data-stu-id="31b3c-141">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="MyComponent">
    @(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
</div>
```

<span data-ttu-id="31b3c-142">雖然頁面和視圖可以使用元件，但相反的情況並非如此。</span><span class="sxs-lookup"><span data-stu-id="31b3c-142">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="31b3c-143">元件不能使用視圖和頁面特定的案例，例如部分視圖和區段。</span><span class="sxs-lookup"><span data-stu-id="31b3c-143">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="31b3c-144">若要在元件中使用部分視圖的邏輯，請將部分視圖邏輯分解成元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-144">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="31b3c-145">如需有關如何呈現元件，以及如何在 Blazor Server 應用程式中管理元件狀態的詳細資訊，請參閱 <xref:blazor/hosting-models> 文章。</span><span class="sxs-lookup"><span data-stu-id="31b3c-145">For more information on how components are rendered and component state is managed in Blazor Server apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="31b3c-146">使用元件</span><span class="sxs-lookup"><span data-stu-id="31b3c-146">Use components</span></span>

<span data-ttu-id="31b3c-147">元件可以包含其他元件，方法是使用 HTML 專案語法來宣告它們。</span><span class="sxs-lookup"><span data-stu-id="31b3c-147">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="31b3c-148">使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。</span><span class="sxs-lookup"><span data-stu-id="31b3c-148">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="31b3c-149">屬性系結會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="31b3c-149">Attribute binding is case sensitive.</span></span> <span data-ttu-id="31b3c-150">例如，`@bind` 有效，而 `@Bind` 則無效。</span><span class="sxs-lookup"><span data-stu-id="31b3c-150">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="31b3c-151">在*Index*中的下列標記會轉譯 `HeadingComponent` 實例：</span><span class="sxs-lookup"><span data-stu-id="31b3c-151">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="31b3c-152">*Components/HeadingComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-152">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="31b3c-153">如果元件包含的 HTML 專案具有大寫的第一個字母，但不符合元件名稱，則會發出警告，指出該元素有未預期的名稱。</span><span class="sxs-lookup"><span data-stu-id="31b3c-153">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="31b3c-154">為元件的命名空間加入 @no__t 0 的語句，可讓元件可用，這會移除警告。</span><span class="sxs-lookup"><span data-stu-id="31b3c-154">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="31b3c-155">元件參數</span><span class="sxs-lookup"><span data-stu-id="31b3c-155">Component parameters</span></span>

<span data-ttu-id="31b3c-156">元件可以具有*元件參數*，其使用元件類別上的公用屬性來定義，並具有 `[Parameter]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-156">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="31b3c-157">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-157">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="31b3c-158">*Components/ChildComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-158">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="31b3c-159">在下列範例中，`ParentComponent` 會設定 `ChildComponent` 之 `Title` 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-159">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="31b3c-160">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-160">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="31b3c-161">子內容</span><span class="sxs-lookup"><span data-stu-id="31b3c-161">Child content</span></span>

<span data-ttu-id="31b3c-162">元件可以設定另一個元件的內容。</span><span class="sxs-lookup"><span data-stu-id="31b3c-162">Components can set the content of another component.</span></span> <span data-ttu-id="31b3c-163">指派元件會在指定接收元件的標記之間提供內容。</span><span class="sxs-lookup"><span data-stu-id="31b3c-163">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="31b3c-164">在下列範例中，`ChildComponent` 具有代表 `RenderFragment` 的 @no__t 1 屬性，代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="31b3c-164">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="31b3c-165">@No__t-0 的值位於應呈現內容之元件的標記中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-165">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="31b3c-166">從父元件接收 `ChildContent` 的值，並在啟動載入面板的 `panel-body` 中轉譯。</span><span class="sxs-lookup"><span data-stu-id="31b3c-166">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="31b3c-167">*Components/ChildComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-167">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="31b3c-168">接收 `RenderFragment` 內容的屬性必須依照慣例命名為 `ChildContent`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-168">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="31b3c-169">下列 `ParentComponent` 可以提供內容來呈現 `ChildComponent`，方法是將內容放在 `<ChildComponent>` 標記內。</span><span class="sxs-lookup"><span data-stu-id="31b3c-169">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="31b3c-170">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-170">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="31b3c-171">屬性展開和任意參數</span><span class="sxs-lookup"><span data-stu-id="31b3c-171">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="31b3c-172">除了元件的宣告參數之外，元件還可以捕捉和轉譯其他屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-172">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="31b3c-173">您可以在字典中捕捉其他屬性，然後在使用[@attributes](xref:mvc/views/razor#attributes) Razor 指示詞轉譯元件時， *splatted*至元素。</span><span class="sxs-lookup"><span data-stu-id="31b3c-173">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="31b3c-174">當定義的元件會產生支援各種自訂的標記專案時，這個案例就很有用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-174">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="31b3c-175">例如，針對支援許多參數的 @no__t 0 分別定義屬性，可能會很繁瑣。</span><span class="sxs-lookup"><span data-stu-id="31b3c-175">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="31b3c-176">在下列範例中，第一個 `<input>` 元素（`id="useIndividualParams"`）會使用個別的元件參數，而第二個 `<input>` 元素（`id="useAttributesDict"`）則使用屬性展開：</span><span class="sxs-lookup"><span data-stu-id="31b3c-176">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```cshtml
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

<span data-ttu-id="31b3c-177">參數的類型必須使用字串索引鍵來執行 `IEnumerable<KeyValuePair<string, object>>`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-177">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="31b3c-178">在此案例中，使用 `IReadOnlyDictionary<string, object>` 也是一個選項。</span><span class="sxs-lookup"><span data-stu-id="31b3c-178">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="31b3c-179">使用這兩種方法轉譯的 `<input>` 元素相同：</span><span class="sxs-lookup"><span data-stu-id="31b3c-179">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="31b3c-180">若要接受任意屬性，請使用 `[Parameter]` 屬性定義元件參數，並將 `CaptureUnmatchedValues` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-180">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="31b3c-181">@No__t-1 上的 `CaptureUnmatchedValues` 屬性允許參數比對與任何其他參數不相符的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-181">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="31b3c-182">元件只能定義具有 `CaptureUnmatchedValues` 的單一參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-182">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="31b3c-183">搭配 `CaptureUnmatchedValues` 使用的屬性類型必須可從具有字串索引鍵的 `Dictionary<string, object>` 指派。</span><span class="sxs-lookup"><span data-stu-id="31b3c-183">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="31b3c-184">`IEnumerable<KeyValuePair<string, object>>` 或 `IReadOnlyDictionary<string, object>` 也是此案例中的選項。</span><span class="sxs-lookup"><span data-stu-id="31b3c-184">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="31b3c-185">資料繫結</span><span class="sxs-lookup"><span data-stu-id="31b3c-185">Data binding</span></span>

<span data-ttu-id="31b3c-186">元件和 DOM 元素的資料系結都是使用[@bind](xref:mvc/views/razor#bind)屬性來完成。</span><span class="sxs-lookup"><span data-stu-id="31b3c-186">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="31b3c-187">下列範例會將 `CurrentValue` 屬性系結至文字方塊的值：</span><span class="sxs-lookup"><span data-stu-id="31b3c-187">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```cshtml
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="31b3c-188">當文字方塊失去焦點時，就會更新屬性的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-188">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="31b3c-189">只有在呈現元件時，才會更新 UI 中的文字方塊，而不會回應變更屬性的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-189">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="31b3c-190">由於元件會在事件處理常式程式碼執行之後自行呈現，因此在觸發事件處理常式之後，屬性更新*通常*會立即反映在 UI 中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-190">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="31b3c-191">使用 `@bind` 搭配 `CurrentValue` 屬性（`<input @bind="CurrentValue" />`）基本上等同于下列內容：</span><span class="sxs-lookup"><span data-stu-id="31b3c-191">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="31b3c-192">當元件呈現時，輸入專案的 `value` 會來自 `CurrentValue` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-192">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="31b3c-193">當使用者在文字方塊中輸入並變更元素焦點時，`onchange` 事件會引發，而 `CurrentValue` 屬性會設定為變更的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-193">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="31b3c-194">實際上，程式碼產生更為複雜，因為 `@bind` 會處理類型轉換執行的情況。</span><span class="sxs-lookup"><span data-stu-id="31b3c-194">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="31b3c-195">就原則而言，`@bind` 會將運算式的目前值與 @no__t 1 屬性產生關聯，並使用已註冊的處理常式來處理變更。</span><span class="sxs-lookup"><span data-stu-id="31b3c-195">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="31b3c-196">除了使用 `@bind` 語法處理 `onchange` 事件之外，也可以使用其他事件來系結屬性或欄位，方法是指定具有 `event` 參數（[@bind-value:event](xref:mvc/views/razor#bind)）的[@bind-value](xref:mvc/views/razor#bind)屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-196">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="31b3c-197">下列範例會系結 `oninput` 事件的 `CurrentValue` 屬性：</span><span class="sxs-lookup"><span data-stu-id="31b3c-197">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="31b3c-198">不同于當元素失去焦點時引發的 `onchange`，`oninput` 會在文字方塊的值變更時引發。</span><span class="sxs-lookup"><span data-stu-id="31b3c-198">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="31b3c-199">**無法剖析的值**</span><span class="sxs-lookup"><span data-stu-id="31b3c-199">**Unparsable values**</span></span>

<span data-ttu-id="31b3c-200">當使用者將無法剖析的值提供給資料系結元素時，會在觸發系結事件時，自動將無法剖析的值還原成先前的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-200">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="31b3c-201">請考慮下列案例：</span><span class="sxs-lookup"><span data-stu-id="31b3c-201">Consider the following scenario:</span></span>

* <span data-ttu-id="31b3c-202">@No__t-0 元素系結至 `int` 類型，其初始值為 `123`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-202">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```cshtml
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="31b3c-203">使用者會將專案的值更新為頁面中的 `123.45`，並變更元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="31b3c-203">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="31b3c-204">在上述案例中，元素的值會還原為 `123`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-204">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="31b3c-205">當 `123.45` 的值拒絕 `123` 的原始值時，使用者瞭解其值不被接受。</span><span class="sxs-lookup"><span data-stu-id="31b3c-205">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="31b3c-206">根據預設，系結會套用至元素的 `onchange` 事件（`@bind="{PROPERTY OR FIELD}"`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-206">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="31b3c-207">使用 `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` 來設定不同的事件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-207">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="31b3c-208">若為 `oninput` 事件（`@bind-value:event="oninput"`），回復會在引進無法剖析值的任何擊鍵之後發生。</span><span class="sxs-lookup"><span data-stu-id="31b3c-208">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="31b3c-209">以 `int` 系結型別為目標的 `oninput` 事件時，使用者無法輸入 @no__t 2 字元。</span><span class="sxs-lookup"><span data-stu-id="31b3c-209">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="31b3c-210">會立即移除 `.` 字元，因此使用者會立即收到僅允許整數的意見反應。</span><span class="sxs-lookup"><span data-stu-id="31b3c-210">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="31b3c-211">在某些情況下，@no__t 0 事件上的值不是理想的情況，例如，當使用者應允許清除無法剖析的 `<input>` 值時。</span><span class="sxs-lookup"><span data-stu-id="31b3c-211">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="31b3c-212">替代方案包括：</span><span class="sxs-lookup"><span data-stu-id="31b3c-212">Alternatives include:</span></span>

* <span data-ttu-id="31b3c-213">請勿使用 `oninput` 事件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-213">Don't use the `oninput` event.</span></span> <span data-ttu-id="31b3c-214">使用預設的 `onchange` 事件（`@bind="{PROPERTY OR FIELD}"`），在此情況下，除非元素失去焦點，否則不正確值不會還原。</span><span class="sxs-lookup"><span data-stu-id="31b3c-214">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="31b3c-215">系結至可為 null 的型別（例如 `int?` 或 `string`），並提供自訂邏輯來處理不正確專案。</span><span class="sxs-lookup"><span data-stu-id="31b3c-215">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="31b3c-216">使用[表單驗證元件](xref:blazor/forms-validation)，例如 `InputNumber` 或 `InputDate`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-216">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="31b3c-217">表單驗證元件有內建支援，可管理不正確輸入。</span><span class="sxs-lookup"><span data-stu-id="31b3c-217">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="31b3c-218">表單驗證元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-218">Form validation components:</span></span>
  * <span data-ttu-id="31b3c-219">允許使用者在相關聯的 `EditContext` 上提供不正確輸入和接收驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="31b3c-219">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="31b3c-220">在 UI 中顯示驗證錯誤，而不幹擾使用者輸入其他 webform 資料。</span><span class="sxs-lookup"><span data-stu-id="31b3c-220">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="31b3c-221">**全球化**</span><span class="sxs-lookup"><span data-stu-id="31b3c-221">**Globalization**</span></span>

<span data-ttu-id="31b3c-222">`@bind` 值會格式化為使用目前文化特性的規則來顯示和剖析。</span><span class="sxs-lookup"><span data-stu-id="31b3c-222">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="31b3c-223">您可以從 <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> 屬性存取目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-223">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="31b3c-224">[InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture)用於下欄欄位類型（`<input type="{TYPE}" />`）：</span><span class="sxs-lookup"><span data-stu-id="31b3c-224">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="31b3c-225">前面的欄位類型：</span><span class="sxs-lookup"><span data-stu-id="31b3c-225">The preceding field types:</span></span>

* <span data-ttu-id="31b3c-226">會使用其適當的瀏覽器格式規則來顯示。</span><span class="sxs-lookup"><span data-stu-id="31b3c-226">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="31b3c-227">不能包含自由格式的文字。</span><span class="sxs-lookup"><span data-stu-id="31b3c-227">Can't contain free-form text.</span></span>
* <span data-ttu-id="31b3c-228">根據瀏覽器的執行方式提供使用者互動特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-228">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="31b3c-229">下欄欄位類型具有特定的格式需求，而且目前不受 Blazor 支援，因為並非所有主要瀏覽器都支援：</span><span class="sxs-lookup"><span data-stu-id="31b3c-229">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="31b3c-230">`@bind` 支援 `@bind:culture` 參數，以提供用來剖析和格式化值的 <xref:System.Globalization.CultureInfo?displayProperty=fullName>。</span><span class="sxs-lookup"><span data-stu-id="31b3c-230">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="31b3c-231">使用 `date` 和 @no__t 1 欄位類型時，不建議指定文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-231">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="31b3c-232">`date` 和 `number` 具有內建的 Blazor 支援，可提供所需的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-232">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="31b3c-233">如需如何設定使用者文化特性的詳細資訊，請參閱[當地語系化](#localization)一節。</span><span class="sxs-lookup"><span data-stu-id="31b3c-233">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="31b3c-234">**格式字串**</span><span class="sxs-lookup"><span data-stu-id="31b3c-234">**Format strings**</span></span>

<span data-ttu-id="31b3c-235">資料系結搭配使用[@bind:format](xref:mvc/views/razor#bind)的 <xref:System.DateTime> 格式字串。</span><span class="sxs-lookup"><span data-stu-id="31b3c-235">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="31b3c-236">目前無法使用其他格式運算式，例如貨幣或數位格式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-236">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="31b3c-237">在上述程式碼中，@no__t 0 元素的欄位類型（`type`）預設為 `text`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-237">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="31b3c-238">支援下列 .NET 類型的 `@bind:format`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-238">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="31b3c-239"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="31b3c-239"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="31b3c-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="31b3c-240"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="31b3c-241">@No__t-0 屬性會指定要套用至 `<input>` 元素之 `value` 的日期格式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-241">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="31b3c-242">當發生 `onchange` 事件時，也會使用格式來剖析值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-242">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="31b3c-243">不建議指定 `date` 欄位類型的格式，因為 Blazor 具有格式日期的內建支援。</span><span class="sxs-lookup"><span data-stu-id="31b3c-243">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="31b3c-244">**元件參數**</span><span class="sxs-lookup"><span data-stu-id="31b3c-244">**Component parameters**</span></span>

<span data-ttu-id="31b3c-245">Binding 可辨識元件參數，其中 `@bind-{property}` 可以跨元件系結屬性值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-245">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="31b3c-246">下列子元件（`ChildComponent`）具有 `Year` 元件參數和 @no__t 2 回呼：</span><span class="sxs-lookup"><span data-stu-id="31b3c-246">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="31b3c-247">`EventCallback<T>` 會在[app eventcallback](#eventcallback)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="31b3c-247">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="31b3c-248">下列父元件會使用 `ChildComponent`，並將 `ParentYear` 參數從父系系結至子元件上的 `Year` 參數：</span><span class="sxs-lookup"><span data-stu-id="31b3c-248">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="31b3c-249">載入 `ParentComponent` 會產生下列標記：</span><span class="sxs-lookup"><span data-stu-id="31b3c-249">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="31b3c-250">如果 `ParentYear` 屬性的值是藉由選取 `ParentComponent` 中的按鈕來變更，則會更新 `ChildComponent` 的 `Year` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-250">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="31b3c-251">重新顯示 `ParentComponent` 時，會在 UI 中轉譯 `Year` 的新值：</span><span class="sxs-lookup"><span data-stu-id="31b3c-251">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="31b3c-252">@No__t-0 參數是可系結的，因為它具有符合 `Year` 參數類型的隨附 `YearChanged` 事件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-252">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="31b3c-253">依照慣例，`<ChildComponent @bind-Year="ParentYear" />` 基本上等同于撰寫：</span><span class="sxs-lookup"><span data-stu-id="31b3c-253">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="31b3c-254">一般來說，您可以使用 `@bind-property:event` 屬性，將屬性系結至對應的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-254">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="31b3c-255">例如，您可以使用下列兩個屬性，將 `MyProp` 的屬性系結至 `MyEventHandler`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-255">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="31b3c-256">事件處理</span><span class="sxs-lookup"><span data-stu-id="31b3c-256">Event handling</span></span>

<span data-ttu-id="31b3c-257">Razor 元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="31b3c-257">Razor components provide event handling features.</span></span> <span data-ttu-id="31b3c-258">針對名為 `on{event}` （例如，`onclick` 和 `onsubmit`）與委派類型值的 HTML 專案屬性，Razor 元件會將屬性的值視為事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-258">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="31b3c-259">屬性的名稱一律會格式化[@on {event}](xref:mvc/views/razor#onevent)。</span><span class="sxs-lookup"><span data-stu-id="31b3c-259">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="31b3c-260">下列程式碼會在 UI 中選取按鈕時，呼叫 `UpdateHeading` 方法：</span><span class="sxs-lookup"><span data-stu-id="31b3c-260">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="31b3c-261">下列程式碼會在 UI 中變更核取方塊時，呼叫 `CheckChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="31b3c-261">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="31b3c-262">事件處理常式也可以是非同步，並傳回 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="31b3c-262">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="31b3c-263">不需要手動呼叫 `StateHasChanged()`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-263">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="31b3c-264">例外狀況會在發生時記錄。</span><span class="sxs-lookup"><span data-stu-id="31b3c-264">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="31b3c-265">在下列範例中，當選取按鈕時，會以非同步方式呼叫 `UpdateHeading`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-265">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="31b3c-266">事件引數類型</span><span class="sxs-lookup"><span data-stu-id="31b3c-266">Event argument types</span></span>

<span data-ttu-id="31b3c-267">對於某些事件，則允許事件引數類型。</span><span class="sxs-lookup"><span data-stu-id="31b3c-267">For some events, event argument types are permitted.</span></span> <span data-ttu-id="31b3c-268">如果不需要存取這些事件種類的其中一個，則在方法呼叫中不需要。</span><span class="sxs-lookup"><span data-stu-id="31b3c-268">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="31b3c-269">支援的 `EventArgs` 會顯示在下表中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-269">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="31b3c-270">Event - 事件</span><span class="sxs-lookup"><span data-stu-id="31b3c-270">Event</span></span> | <span data-ttu-id="31b3c-271">執行個體</span><span class="sxs-lookup"><span data-stu-id="31b3c-271">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="31b3c-272">剪貼簿</span><span class="sxs-lookup"><span data-stu-id="31b3c-272">Clipboard</span></span>        | `ClipboardEventArgs` |
| <span data-ttu-id="31b3c-273">拖放式</span><span class="sxs-lookup"><span data-stu-id="31b3c-273">Drag</span></span>             | <span data-ttu-id="31b3c-274">`DragEventArgs` &ndash; `DataTransfer` 和 `DataTransferItem` 會保存拖曳的專案資料。</span><span class="sxs-lookup"><span data-stu-id="31b3c-274">`DragEventArgs` &ndash; `DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="31b3c-275">錯誤</span><span class="sxs-lookup"><span data-stu-id="31b3c-275">Error</span></span>            | `ErrorEventArgs` |
| <span data-ttu-id="31b3c-276">焦點</span><span class="sxs-lookup"><span data-stu-id="31b3c-276">Focus</span></span>            | <span data-ttu-id="31b3c-277">`FocusEventArgs` &ndash; 不包含對 `relatedTarget` 的支援。</span><span class="sxs-lookup"><span data-stu-id="31b3c-277">`FocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="31b3c-278">`<input>` 變更</span><span class="sxs-lookup"><span data-stu-id="31b3c-278">`<input>` change</span></span> | `ChangeEventArgs` |
| <span data-ttu-id="31b3c-279">鍵盤</span><span class="sxs-lookup"><span data-stu-id="31b3c-279">Keyboard</span></span>         | `KeyboardEventArgs` |
| <span data-ttu-id="31b3c-280">滑鼠</span><span class="sxs-lookup"><span data-stu-id="31b3c-280">Mouse</span></span>            | `MouseEventArgs` |
| <span data-ttu-id="31b3c-281">滑鼠指標</span><span class="sxs-lookup"><span data-stu-id="31b3c-281">Mouse pointer</span></span>    | `PointerEventArgs` |
| <span data-ttu-id="31b3c-282">滑鼠滾輪</span><span class="sxs-lookup"><span data-stu-id="31b3c-282">Mouse wheel</span></span>      | `WheelEventArgs` |
| <span data-ttu-id="31b3c-283">進度</span><span class="sxs-lookup"><span data-stu-id="31b3c-283">Progress</span></span>         | `ProgressEventArgs` |
| <span data-ttu-id="31b3c-284">觸控</span><span class="sxs-lookup"><span data-stu-id="31b3c-284">Touch</span></span>            | <span data-ttu-id="31b3c-285">`TouchEventArgs` &ndash; `TouchPoint` 代表觸控裝置上的單一連絡人點。</span><span class="sxs-lookup"><span data-stu-id="31b3c-285">`TouchEventArgs` &ndash; `TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="31b3c-286">如需上表中事件的屬性和事件處理行為的詳細資訊，請參閱[參考來源中的 EventArgs 類別（aspnet/AspNetCore release/3.0 分支）](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web)。</span><span class="sxs-lookup"><span data-stu-id="31b3c-286">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="31b3c-287">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="31b3c-287">Lambda expressions</span></span>

<span data-ttu-id="31b3c-288">您也可以使用 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="31b3c-288">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="31b3c-289">關閉其他值（例如反覆運算一組專案時）通常會很方便。</span><span class="sxs-lookup"><span data-stu-id="31b3c-289">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="31b3c-290">下列範例會建立三個按鈕，其中每個都會呼叫 `UpdateHeading` 會在 UI 中選取時傳遞事件引數（`MouseEventArgs`）和其按鈕編號（`buttonNumber`）：</span><span class="sxs-lookup"><span data-stu-id="31b3c-290">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="31b3c-291">請勿直接在 lambda 運算式**中使用 @no__t** 2 迴圈中的迴圈變數（`i`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-291">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="31b3c-292">否則，所有 lambda 運算式都會使用相同的變數，使 @no__t 0 的值在所有 lambda 中都相同。</span><span class="sxs-lookup"><span data-stu-id="31b3c-292">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="31b3c-293">一律在本機變數中捕捉其值（在上述範例中為 `buttonNumber`），然後使用它。</span><span class="sxs-lookup"><span data-stu-id="31b3c-293">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="31b3c-294">App eventcallback</span><span class="sxs-lookup"><span data-stu-id="31b3c-294">EventCallback</span></span>

<span data-ttu-id="31b3c-295">有一個常見的嵌套元件案例，就是當子元件事件發生時，需要執行父元件的方法 @ no__t-0for 範例（當子系中發生 @no__t 1 事件時）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-295">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="31b3c-296">若要在元件之間公開事件，請使用 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-296">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="31b3c-297">父元件可以將回呼方法指派給子元件的 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-297">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="31b3c-298">範例應用程式中的 @no__t 0 會示範如何設定按鈕的 @no__t 1 處理常式，以從範例的 `ParentComponent` 接收 @no__t 2 委派。</span><span class="sxs-lookup"><span data-stu-id="31b3c-298">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="31b3c-299">@No__t-0 是以 `MouseEventArgs` 輸入，這適用于來自週邊裝置的 @no__t 2 事件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-299">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="31b3c-300">@No__t-0 會將子系的 `EventCallback<T>` 設定為其 @no__t 2 方法：</span><span class="sxs-lookup"><span data-stu-id="31b3c-300">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="31b3c-301">在 `ChildComponent` 中選取按鈕時：</span><span class="sxs-lookup"><span data-stu-id="31b3c-301">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="31b3c-302">呼叫 `ParentComponent` 的 `ShowMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="31b3c-302">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="31b3c-303">`messageText` 會更新，並顯示在 `ParentComponent` 中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-303">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="31b3c-304">回呼的方法（`ShowMessage`）不需要呼叫 `StateHasChanged`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-304">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="31b3c-305">`StateHasChanged` 會自動呼叫以 rerender `ParentComponent`，就像子事件會觸發元件 rerendering 在子系內執行的事件處理常式一樣。</span><span class="sxs-lookup"><span data-stu-id="31b3c-305">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="31b3c-306">`EventCallback` 和 `EventCallback<T>` 允許非同步委派。</span><span class="sxs-lookup"><span data-stu-id="31b3c-306">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="31b3c-307">`EventCallback<T>` 是強型別，而且需要特定的引數型別。</span><span class="sxs-lookup"><span data-stu-id="31b3c-307">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="31b3c-308">`EventCallback` 是弱式類型，且允許任何引數類型。</span><span class="sxs-lookup"><span data-stu-id="31b3c-308">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="31b3c-309">使用 `InvokeAsync` 叫用 `EventCallback` 或 `EventCallback<T>`，並等待 <xref:System.Threading.Tasks.Task>：</span><span class="sxs-lookup"><span data-stu-id="31b3c-309">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="31b3c-310">使用 `EventCallback`，並 `EventCallback<T>` 用於事件處理和系結元件參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-310">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="31b3c-311">偏好強型別 `EventCallback<T>`，而不是 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-311">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="31b3c-312">`EventCallback<T>` 會對元件的使用者提供更好的錯誤意見反應。</span><span class="sxs-lookup"><span data-stu-id="31b3c-312">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="31b3c-313">與其他 UI 事件處理常式類似，指定事件參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="31b3c-313">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="31b3c-314">當沒有任何值傳遞至回呼時，請使用 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-314">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="chained-bind"></a><span data-ttu-id="31b3c-315">連鎖系結</span><span class="sxs-lookup"><span data-stu-id="31b3c-315">Chained bind</span></span>

<span data-ttu-id="31b3c-316">常見的案例是將資料系結參數連結至元件輸出中的 page 元素。</span><span class="sxs-lookup"><span data-stu-id="31b3c-316">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="31b3c-317">此案例稱為*連鎖*系結，因為會同時發生多個層級的系結。</span><span class="sxs-lookup"><span data-stu-id="31b3c-317">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="31b3c-318">無法使用頁面的元素中的 `@bind` 語法來執行連鎖系結。</span><span class="sxs-lookup"><span data-stu-id="31b3c-318">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="31b3c-319">事件處理常式和值必須分別指定。</span><span class="sxs-lookup"><span data-stu-id="31b3c-319">The event handler and value must be specified separately.</span></span> <span data-ttu-id="31b3c-320">不過，父元件可以使用 `@bind` 語法搭配元件的參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-320">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="31b3c-321">下列 `PasswordField` 元件（*self.passwordfield.text*）：</span><span class="sxs-lookup"><span data-stu-id="31b3c-321">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="31b3c-322">將 `<input>` 元素的值設定為 @no__t 1 屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-322">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="31b3c-323">將 `Password` 屬性的變更公開至具有[app eventcallback](#eventcallback)的父元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-323">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="31b3c-324">@No__t-0 元件會在另一個元件中使用：</span><span class="sxs-lookup"><span data-stu-id="31b3c-324">The `PasswordField` component is used in another component:</span></span>

```cshtml
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="31b3c-325">若要對上述範例中的密碼執行檢查或陷印錯誤：</span><span class="sxs-lookup"><span data-stu-id="31b3c-325">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="31b3c-326">在下列範例程式碼中建立 `Password` 的支援欄位（`password`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-326">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="31b3c-327">執行 `Password` setter 中的檢查或陷阱錯誤。</span><span class="sxs-lookup"><span data-stu-id="31b3c-327">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="31b3c-328">如果密碼的值中使用了空格，下列範例會向使用者提供立即的意見反應：</span><span class="sxs-lookup"><span data-stu-id="31b3c-328">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```cshtml
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="31b3c-329">捕獲元件的參考</span><span class="sxs-lookup"><span data-stu-id="31b3c-329">Capture references to components</span></span>

<span data-ttu-id="31b3c-330">元件參考提供參考元件實例的方法，讓您可以發出命令給該實例，例如 `Show` 或 `Reset`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-330">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="31b3c-331">若要捕捉元件參考：</span><span class="sxs-lookup"><span data-stu-id="31b3c-331">To capture a component reference:</span></span>

* <span data-ttu-id="31b3c-332">將[@ref](xref:mvc/views/razor#ref)屬性加入至子元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-332">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="31b3c-333">定義與子元件類型相同的欄位。</span><span class="sxs-lookup"><span data-stu-id="31b3c-333">Define a field with the same type as the child component.</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="31b3c-334">當元件轉譯時，[`loginDialog`] 欄位會填入 `MyLoginDialog` 子元件實例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-334">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="31b3c-335">接著，您可以在元件實例上叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="31b3c-335">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31b3c-336">只有在轉譯元件且其輸出包含 `MyLoginDialog` 元素之後，才會填入 `loginDialog` 變數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-336">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="31b3c-337">直到該點為止，沒有任何可參考的內容。</span><span class="sxs-lookup"><span data-stu-id="31b3c-337">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="31b3c-338">若要在元件完成呈現之後操作元件參考，請使用[OnAfterRenderAsync 或 OnAfterRender 方法](#lifecycle-methods)。</span><span class="sxs-lookup"><span data-stu-id="31b3c-338">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](#lifecycle-methods).</span></span>

<span data-ttu-id="31b3c-339">雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/javascript-interop#capture-references-to-elements)，但它並不是[JavaScript interop](xref:blazor/javascript-interop)功能。</span><span class="sxs-lookup"><span data-stu-id="31b3c-339">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="31b3c-340">元件參考不會傳遞至 JavaScript 程式碼 @ no__t-0they're 僅適用于 .NET 程式碼。</span><span class="sxs-lookup"><span data-stu-id="31b3c-340">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="31b3c-341">請勿**使用元件**參考來改變子元件的狀態。</span><span class="sxs-lookup"><span data-stu-id="31b3c-341">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="31b3c-342">請改用一般宣告式參數，將資料傳遞至子元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-342">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="31b3c-343">使用一般宣告式參數會導致子元件自動 rerender 正確的時間。</span><span class="sxs-lookup"><span data-stu-id="31b3c-343">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="31b3c-344">在外部叫用元件方法來更新狀態</span><span class="sxs-lookup"><span data-stu-id="31b3c-344">Invoke component methods externally to update state</span></span>

<span data-ttu-id="31b3c-345">Blazor 會使用 `SynchronizationContext` 來強制執行單一邏輯執行緒。</span><span class="sxs-lookup"><span data-stu-id="31b3c-345">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="31b3c-346">元件的生命週期方法以及 Blazor 所引發的任何事件回呼都會在此 `SynchronizationContext` 上執行。</span><span class="sxs-lookup"><span data-stu-id="31b3c-346">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="31b3c-347">在事件中，必須根據外來事件（例如計時器或其他通知）更新元件，請使用 `InvokeAsync` 方法，這會分派給 Blazor 的 `SynchronizationContext`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-347">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="31b3c-348">例如，假設有一個通知程式*服務*可通知任何處于已更新狀態的「接聽」元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-348">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="31b3c-349">使用 `NotifierService` 來更新元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-349">Usage of the `NotifierService` to update a component:</span></span>

```cshtml
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

<span data-ttu-id="31b3c-350">在上述範例中，`NotifierService` 會叫用元件在 Blazor 的 `SynchronizationContext` 以外的 `OnNotify` 方法。</span><span class="sxs-lookup"><span data-stu-id="31b3c-350">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="31b3c-351">`InvokeAsync` 是用來切換至正確的內容，並將轉譯排入佇列。</span><span class="sxs-lookup"><span data-stu-id="31b3c-351">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="31b3c-352">使用 @no__t 0key 來控制元素和元件的保留</span><span class="sxs-lookup"><span data-stu-id="31b3c-352">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="31b3c-353">當轉譯專案或元件的清單，以及後續變更的專案或元件時，Blazor 的比較演算法必須決定哪些先前的專案或元件可以保留，以及模型物件應如何對應至這些專案。</span><span class="sxs-lookup"><span data-stu-id="31b3c-353">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="31b3c-354">一般來說，此程式是自動的，可以忽略，但在某些情況下，您可能會想要控制進程。</span><span class="sxs-lookup"><span data-stu-id="31b3c-354">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="31b3c-355">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="31b3c-355">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="31b3c-356">@No__t-0 集合的內容可能會隨著插入、刪除或重新排序的專案而變更。</span><span class="sxs-lookup"><span data-stu-id="31b3c-356">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="31b3c-357">當元件 rerenders 時，@no__t 0 元件可能會變更，以接收不同的 @no__t 1 參數值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-357">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="31b3c-358">這可能會導致比預期更複雜的 rerendering。</span><span class="sxs-lookup"><span data-stu-id="31b3c-358">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="31b3c-359">在某些情況下，rerendering 可能會導致可見的行為差異，例如失去元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="31b3c-359">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="31b3c-360">您可以使用 `@key` 指示詞屬性來控制對應進程。</span><span class="sxs-lookup"><span data-stu-id="31b3c-360">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="31b3c-361">`@key` 會使比較演算法根據索引鍵的值，保證保留元素或元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-361">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="31b3c-362">當 @no__t 0 集合變更時，比較演算法會保留 @no__t 1 實例與 @no__t 2 實例之間的關聯：</span><span class="sxs-lookup"><span data-stu-id="31b3c-362">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="31b3c-363">如果從 `People` 清單中刪除 `Person`，則只會從 UI 移除對應的 `<DetailsEditor>` 實例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-363">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="31b3c-364">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="31b3c-364">Other instances are left unchanged.</span></span>
* <span data-ttu-id="31b3c-365">如果在清單中的某個位置插入 `Person`，則會在對應的位置插入一個新的 @no__t 1 實例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-365">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="31b3c-366">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="31b3c-366">Other instances are left unchanged.</span></span>
* <span data-ttu-id="31b3c-367">如果 `Person` 的專案重新排序，則會保留對應的 @no__t 1 實例，並在 UI 中重新排序。</span><span class="sxs-lookup"><span data-stu-id="31b3c-367">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="31b3c-368">在某些情況下，使用 `@key` 會將 rerendering 的複雜性降到最低，並避免 DOM 的具狀態部分可能發生的問題，例如焦點位置。</span><span class="sxs-lookup"><span data-stu-id="31b3c-368">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="31b3c-369">索引鍵在每個容器元素或元件的本機。</span><span class="sxs-lookup"><span data-stu-id="31b3c-369">Keys are local to each container element or component.</span></span> <span data-ttu-id="31b3c-370">金鑰不會在檔之間進行全域比較。</span><span class="sxs-lookup"><span data-stu-id="31b3c-370">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="31b3c-371">使用 @no__t 的時機-0key</span><span class="sxs-lookup"><span data-stu-id="31b3c-371">When to use \@key</span></span>

<span data-ttu-id="31b3c-372">一般來說，每當轉譯清單時（例如，在 @no__t 1 區塊中），使用 `@key` 就有意義，而且有適當的值可定義 `@key`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-372">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="31b3c-373">當物件變更時，您也可以使用 `@key` 來防止 Blazor 保留元素或元件子樹：</span><span class="sxs-lookup"><span data-stu-id="31b3c-373">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="31b3c-374">如果 `@currentPerson` 變更，則 @no__t 1 屬性指示詞會強制 Blazor 捨棄整個 `<div>` 及其下階，並使用新的專案和元件重建 UI 中的子樹。</span><span class="sxs-lookup"><span data-stu-id="31b3c-374">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="31b3c-375">如果您需要保證 `@currentPerson` 變更時不會保留任何 UI 狀態，這會很有用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-375">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="31b3c-376">當不使用時 \@key</span><span class="sxs-lookup"><span data-stu-id="31b3c-376">When not to use \@key</span></span>

<span data-ttu-id="31b3c-377">與 `@key` 進行比較時，會產生效能成本。</span><span class="sxs-lookup"><span data-stu-id="31b3c-377">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="31b3c-378">效能成本並不大，但只有在控制元素或元件保留規則以受益于應用程式時，才指定 `@key`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-378">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="31b3c-379">即使未使用 `@key`，Blazor 還是會盡可能保留子項目和元件實例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-379">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="31b3c-380">使用 `@key` 的唯一優點是控制模型實例*如何*對應至保留的元件實例，而不是用來選取對應的比較演算法。</span><span class="sxs-lookup"><span data-stu-id="31b3c-380">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="31b3c-381">要用於 @no__t 的值-0key</span><span class="sxs-lookup"><span data-stu-id="31b3c-381">What values to use for \@key</span></span>

<span data-ttu-id="31b3c-382">一般來說，提供下列其中一種類型的值給 `@key` 是合理的：</span><span class="sxs-lookup"><span data-stu-id="31b3c-382">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="31b3c-383">模型物件實例（例如，如先前範例所示的 `Person` 實例）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-383">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="31b3c-384">這可確保根據物件參考的相等性進行保留。</span><span class="sxs-lookup"><span data-stu-id="31b3c-384">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="31b3c-385">唯一識別碼（例如，類型 `int`、`string` 或 `Guid`）的主要金鑰值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-385">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="31b3c-386">請確定用於 `@key` 的值不會造成衝突。</span><span class="sxs-lookup"><span data-stu-id="31b3c-386">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="31b3c-387">如果在相同的父元素中偵測到衝突值，Blazor 會擲回例外狀況，因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-387">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="31b3c-388">只使用不同的值，例如物件實例或主鍵值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-388">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="31b3c-389">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="31b3c-389">Lifecycle methods</span></span>

<span data-ttu-id="31b3c-390">`OnInitializedAsync` 和 `OnInitialized` 會執行程式碼以初始化元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-390">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="31b3c-391">若要執行非同步作業，請在作業上使用 `OnInitializedAsync` 和 `await` 關鍵字：</span><span class="sxs-lookup"><span data-stu-id="31b3c-391">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="31b3c-392">在 `OnInitializedAsync` 週期事件期間，必須在元件初始化期間進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="31b3c-392">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="31b3c-393">如需同步操作，請使用 `OnInitialized`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-393">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="31b3c-394">當元件已從其父系接收參數，且已將值指派給屬性時，會呼叫 `OnParametersSetAsync` 和 `OnParametersSet`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-394">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="31b3c-395">這些方法會在元件初始化之後和每次呈現元件時執行：</span><span class="sxs-lookup"><span data-stu-id="31b3c-395">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="31b3c-396">當套用參數和屬性值時，非同步工作必須在 `OnParametersSetAsync` 週期事件期間發生。</span><span class="sxs-lookup"><span data-stu-id="31b3c-396">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="31b3c-397">在元件完成呈現之後，會呼叫 `OnAfterRenderAsync` 和 `OnAfterRender`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-397">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="31b3c-398">此時會填入元素和元件參考。</span><span class="sxs-lookup"><span data-stu-id="31b3c-398">Element and component references are populated at this point.</span></span> <span data-ttu-id="31b3c-399">使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="31b3c-399">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="31b3c-400">在*伺服器上預先呈現時，不會呼叫*`OnAfterRender`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-400">`OnAfterRender` *isn't called when prerendering on the server.*</span></span>

<span data-ttu-id="31b3c-401">@No__t-1 和 `OnAfterRender` 的 `firstRender` 參數為：</span><span class="sxs-lookup"><span data-stu-id="31b3c-401">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender` is:</span></span>

* <span data-ttu-id="31b3c-402">當第一次叫用元件實例時，設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-402">Set to `true` the first time that the component instance is invoked.</span></span>
* <span data-ttu-id="31b3c-403">確保初始化工作只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="31b3c-403">Ensures that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="31b3c-404">在 `OnAfterRenderAsync` 週期事件期間，必須立即執行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="31b3c-404">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="31b3c-405">處理轉譯時的未完成非同步動作</span><span class="sxs-lookup"><span data-stu-id="31b3c-405">Handle incomplete async actions at render</span></span>

<span data-ttu-id="31b3c-406">在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="31b3c-406">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="31b3c-407">當生命週期方法正在執行時，物件可能 @no__t 0 或未完全填入資料。</span><span class="sxs-lookup"><span data-stu-id="31b3c-407">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="31b3c-408">提供轉譯邏輯，以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="31b3c-408">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="31b3c-409">當物件 `null` 時，轉譯預留位置 UI 元素（例如，載入訊息）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-409">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="31b3c-410">在 Blazor 範本的 `FetchData` 元件中，`OnInitializedAsync` 會覆寫為 asychronously 接收預測資料（`forecasts`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-410">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="31b3c-411">當 `forecasts` `null` 時，會向使用者顯示載入訊息。</span><span class="sxs-lookup"><span data-stu-id="31b3c-411">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="31b3c-412">@No__t-1 傳回的 `Task` 完成後，元件會重新顯示並具有更新的狀態。</span><span class="sxs-lookup"><span data-stu-id="31b3c-412">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="31b3c-413">*Pages/FetchData.razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-413">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="31b3c-414">在設定參數之前執行程式碼</span><span class="sxs-lookup"><span data-stu-id="31b3c-414">Execute code before parameters are set</span></span>

<span data-ttu-id="31b3c-415">在設定參數之前，可以覆寫 `SetParameters` 來執行程式碼：</span><span class="sxs-lookup"><span data-stu-id="31b3c-415">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="31b3c-416">如果未叫用 `base.SetParameters`，自訂程式碼就可以任何需要的方式解讀傳入的參數值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-416">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="31b3c-417">例如，傳入的參數不需要指派給類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-417">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="31b3c-418">隱藏 UI 的重新整理</span><span class="sxs-lookup"><span data-stu-id="31b3c-418">Suppress refreshing of the UI</span></span>

<span data-ttu-id="31b3c-419">可以覆寫 `ShouldRender`，以隱藏 UI 的重新整理。</span><span class="sxs-lookup"><span data-stu-id="31b3c-419">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="31b3c-420">如果執行傳回 `true`，則會重新整理 UI。</span><span class="sxs-lookup"><span data-stu-id="31b3c-420">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="31b3c-421">即使 `ShouldRender` 遭到覆寫，元件一律會一開始呈現。</span><span class="sxs-lookup"><span data-stu-id="31b3c-421">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="31b3c-422">使用 IDisposable 的元件處置</span><span class="sxs-lookup"><span data-stu-id="31b3c-422">Component disposal with IDisposable</span></span>

<span data-ttu-id="31b3c-423">如果元件執行 <xref:System.IDisposable>，則會在從 UI 移除元件時呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="31b3c-423">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="31b3c-424">下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：</span><span class="sxs-lookup"><span data-stu-id="31b3c-424">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a><span data-ttu-id="31b3c-425">路由</span><span class="sxs-lookup"><span data-stu-id="31b3c-425">Routing</span></span>

<span data-ttu-id="31b3c-426">Blazor 中的路由是藉由將路由範本提供給應用程式中每個可存取的元件來達成。</span><span class="sxs-lookup"><span data-stu-id="31b3c-426">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="31b3c-427">編譯具有 `@page` 指示詞的 Razor 檔案時，產生的類別會指定路由範本 @no__t 1。</span><span class="sxs-lookup"><span data-stu-id="31b3c-427">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="31b3c-428">在執行時間，路由器會尋找具有 @no__t 0 的元件類別，並轉譯任何元件具有符合所要求 URL 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="31b3c-428">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="31b3c-429">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-429">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="31b3c-430">下列元件會回應 `/BlazorRoute` 和 `/DifferentBlazorRoute` 的要求：</span><span class="sxs-lookup"><span data-stu-id="31b3c-430">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="31b3c-431">路由參數</span><span class="sxs-lookup"><span data-stu-id="31b3c-431">Route parameters</span></span>

<span data-ttu-id="31b3c-432">元件可以從 `@page` 指示詞中提供的路由範本接收路由參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-432">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="31b3c-433">路由器會使用路由參數來填入對應的元件參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-433">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="31b3c-434">*路由參數元件*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-434">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="31b3c-435">不支援選擇性參數，因此上述範例中會套用兩個 @no__t 0 的指示詞。</span><span class="sxs-lookup"><span data-stu-id="31b3c-435">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="31b3c-436">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-436">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="31b3c-437">第二個 `@page` 指示詞會採用 `{text}` 路由參數，並將值指派給 `Text` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-437">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="31b3c-438">「程式碼後置」體驗的基類繼承</span><span class="sxs-lookup"><span data-stu-id="31b3c-438">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="31b3c-439">元件檔案會將 HTML 標籤C#和處理常式代碼混合在同一個檔案中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-439">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="31b3c-440">@No__t-0 指示詞可用來提供具有「程式碼後置」體驗的 Blazor apps，以分隔元件標記與處理常式代碼。</span><span class="sxs-lookup"><span data-stu-id="31b3c-440">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="31b3c-441">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)會顯示元件如何繼承基類（`BlazorRocksBase`），以提供元件的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="31b3c-441">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="31b3c-442">*Pages/BlazorRocks. razor*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-442">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="31b3c-443">*BlazorRocksBase.cs*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-443">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="31b3c-444">基類應該衍生自 `ComponentBase`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-444">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="31b3c-445">匯入元件</span><span class="sxs-lookup"><span data-stu-id="31b3c-445">Import components</span></span>

<span data-ttu-id="31b3c-446">以 Razor 撰寫之元件的命名空間是根據（依優先順序排列）：</span><span class="sxs-lookup"><span data-stu-id="31b3c-446">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="31b3c-447">Razor file （*razor*）標記中[的 @namespace](xref:mvc/views/razor#namespace)指定（`@namespace BlazorSample.MyNamespace`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-447">[@namespace](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="31b3c-448">專案檔中的專案 `RootNamespace` （`<RootNamespace>BlazorSample</RootNamespace>`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-448">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="31b3c-449">從專案檔的檔案名（ *.csproj*）取得的專案名稱，以及從專案根目錄到元件的路徑。</span><span class="sxs-lookup"><span data-stu-id="31b3c-449">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="31b3c-450">例如，架構會將 *{PROJECT ROOT}/Pages/Index.razor* （*BlazorSample*）解析為命名空間 `BlazorSample.Pages`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-450">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="31b3c-451">元件會C#遵循名稱系結規則。</span><span class="sxs-lookup"><span data-stu-id="31b3c-451">Components follow C# name binding rules.</span></span> <span data-ttu-id="31b3c-452">針對此範例中的 `Index` 元件，範圍內的元件是所有元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-452">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="31b3c-453">在相同的資料夾中，*頁面*。</span><span class="sxs-lookup"><span data-stu-id="31b3c-453">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="31b3c-454">專案根目錄中未明確指定不同命名空間的元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-454">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="31b3c-455">使用 Razor 的[@using](xref:mvc/views/razor#using)指示詞，在不同的命名空間中定義的元件會帶入範圍中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-455">Components defined in a different namespace are brought into scope using Razor's [@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="31b3c-456">如果*BlazorSample/Shared/* 資料夾中有另一個元件（`NavMenu.razor`）存在，則元件可用於 `Index.razor`，並具有下列 `@using` 個語句：</span><span class="sxs-lookup"><span data-stu-id="31b3c-456">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="31b3c-457">元件也可以使用其完整名稱來參考，這不需要[@using](xref:mvc/views/razor#using)指示詞：</span><span class="sxs-lookup"><span data-stu-id="31b3c-457">Components can also be referenced using their fully qualified names, which doesn't require the [@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="31b3c-458">不支援 `global::` 限定。</span><span class="sxs-lookup"><span data-stu-id="31b3c-458">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="31b3c-459">不支援以別名 `using` 的語句匯入元件（例如，`@using Foo = Bar`）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-459">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="31b3c-460">不支援部分限定的名稱。</span><span class="sxs-lookup"><span data-stu-id="31b3c-460">Partially qualified names aren't supported.</span></span> <span data-ttu-id="31b3c-461">例如，不支援在 `<Shared.NavMenu></Shared.NavMenu>` 中加入 `@using BlazorSample` 和參考 `NavMenu.razor`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-461">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="31b3c-462">條件式 HTML 元素屬性</span><span class="sxs-lookup"><span data-stu-id="31b3c-462">Conditional HTML element attributes</span></span>

<span data-ttu-id="31b3c-463">HTML 專案屬性會根據 .NET 值有條件地呈現。</span><span class="sxs-lookup"><span data-stu-id="31b3c-463">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="31b3c-464">如果值為 `false` 或 `null`，則不會呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-464">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="31b3c-465">如果值是 `true`，則會將屬性轉譯為最小化。</span><span class="sxs-lookup"><span data-stu-id="31b3c-465">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="31b3c-466">在下列範例中，`IsCompleted` 會決定是否要在專案的標記中呈現 `checked`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-466">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="31b3c-467">如果 `IsCompleted` `true`，則會將此核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="31b3c-467">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="31b3c-468">如果 `IsCompleted` `false`，則會將此核取方塊轉譯為：</span><span class="sxs-lookup"><span data-stu-id="31b3c-468">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="31b3c-469">如需詳細資訊，請參閱<xref:mvc/views/razor>。</span><span class="sxs-lookup"><span data-stu-id="31b3c-469">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="31b3c-470">當 .NET 類型為 `bool` 時，某些 HTML 屬性（例如，由[aria 按下](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)）無法正常運作。</span><span class="sxs-lookup"><span data-stu-id="31b3c-470">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="31b3c-471">在這些情況下，請使用 `string` 類型，而不是 `bool`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-471">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="31b3c-472">原始 HTML</span><span class="sxs-lookup"><span data-stu-id="31b3c-472">Raw HTML</span></span>

<span data-ttu-id="31b3c-473">字串通常會使用 DOM 文位元組點來呈現，這表示它們可能包含的任何標記都會被忽略，並視為常值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-473">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="31b3c-474">若要轉譯原始 HTML，請將 HTML 內容包裝為 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-474">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="31b3c-475">此值會剖析為 HTML 或 SVG，並插入 DOM 中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-475">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="31b3c-476">轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**，應予以避免！</span><span class="sxs-lookup"><span data-stu-id="31b3c-476">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="31b3c-477">下列範例顯示如何使用 `MarkupString` 型別，將靜態 HTML 內容的區塊新增至元件的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="31b3c-477">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="31b3c-478">樣板化元件</span><span class="sxs-lookup"><span data-stu-id="31b3c-478">Templated components</span></span>

<span data-ttu-id="31b3c-479">樣板化元件是接受一或多個 UI 範本做為參數的元件，然後可以用來做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="31b3c-479">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="31b3c-480">樣板化元件可讓您撰寫比一般元件更容易重複使用的較高層級元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-480">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="31b3c-481">其中有幾個範例包括：</span><span class="sxs-lookup"><span data-stu-id="31b3c-481">A couple of examples include:</span></span>

* <span data-ttu-id="31b3c-482">資料表元件，可讓使用者指定資料表標頭、資料列和頁尾的範本。</span><span class="sxs-lookup"><span data-stu-id="31b3c-482">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="31b3c-483">清單元件，可讓使用者指定範本來轉譯清單中的專案。</span><span class="sxs-lookup"><span data-stu-id="31b3c-483">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="31b3c-484">範本參數</span><span class="sxs-lookup"><span data-stu-id="31b3c-484">Template parameters</span></span>

<span data-ttu-id="31b3c-485">樣板化元件是藉由指定一或多個類型的元件參數 `RenderFragment` 或 `RenderFragment<T>` 來定義。</span><span class="sxs-lookup"><span data-stu-id="31b3c-485">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="31b3c-486">呈現片段代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="31b3c-486">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="31b3c-487">`RenderFragment<T>` 會採用可在叫用轉譯片段時指定的類型參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-487">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="31b3c-488">`TableTemplate` 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-488">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="31b3c-489">使用樣板化元件時，可以使用符合參數名稱的子專案來指定範本參數（`TableHeader`，並在下列範例中 `RowTemplate`）：</span><span class="sxs-lookup"><span data-stu-id="31b3c-489">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="31b3c-490">範本內容參數</span><span class="sxs-lookup"><span data-stu-id="31b3c-490">Template context parameters</span></span>

<span data-ttu-id="31b3c-491">當做元素傳遞之類型 `RenderFragment<T>` 的元件引數具有名為 `context` 的隱含參數（例如，從前面的程式碼範例，`@context.PetId`），但您可以使用子專案上的 `Context` 屬性來變更參數名稱。</span><span class="sxs-lookup"><span data-stu-id="31b3c-491">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="31b3c-492">在下列範例中，`RowTemplate` 元素的 `Context` 屬性會指定 `pet` 參數：</span><span class="sxs-lookup"><span data-stu-id="31b3c-492">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="31b3c-493">或者，您也可以在 component 元素上指定 `Context` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-493">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="31b3c-494">指定的 `Context` 屬性會套用至所有指定的範本參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-494">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="31b3c-495">當您想要指定隱含子內容的內容參數名稱時（不含任何換行的子項目），這會很有用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-495">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="31b3c-496">在下列範例中，`Context` 屬性會出現在 `TableTemplate` 元素上，並套用至所有範本參數：</span><span class="sxs-lookup"><span data-stu-id="31b3c-496">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="31b3c-497">泛型型別元件</span><span class="sxs-lookup"><span data-stu-id="31b3c-497">Generic-typed components</span></span>

<span data-ttu-id="31b3c-498">樣板化元件通常會以一般方式輸入。</span><span class="sxs-lookup"><span data-stu-id="31b3c-498">Templated components are often generically typed.</span></span> <span data-ttu-id="31b3c-499">例如，泛型 `ListViewTemplate` 元件可用來呈現 @no__t 1 的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-499">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="31b3c-500">若要定義泛型元件，請使用[@typeparam](xref:mvc/views/razor#typeparam)指示詞來指定類型參數：</span><span class="sxs-lookup"><span data-stu-id="31b3c-500">To define a generic component, use the [@typeparam](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="31b3c-501">使用泛型型別元件時，會在可能的情況下推斷型別參數：</span><span class="sxs-lookup"><span data-stu-id="31b3c-501">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="31b3c-502">否則，必須使用符合型別參數名稱的屬性來明確指定型別參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-502">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="31b3c-503">在下列範例中，`TItem="Pet"` 會指定類型：</span><span class="sxs-lookup"><span data-stu-id="31b3c-503">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="31b3c-504">級聯的值和參數</span><span class="sxs-lookup"><span data-stu-id="31b3c-504">Cascading values and parameters</span></span>

<span data-ttu-id="31b3c-505">在某些情況下，使用[元件參數](#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。</span><span class="sxs-lookup"><span data-stu-id="31b3c-505">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="31b3c-506">串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="31b3c-506">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="31b3c-507">級聯的值和參數也會提供一種方法來協調元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-507">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="31b3c-508">主題範例</span><span class="sxs-lookup"><span data-stu-id="31b3c-508">Theme example</span></span>

<span data-ttu-id="31b3c-509">在範例應用程式的下列範例中，@no__t 0 類別會指定主題資訊，以向下流動元件階層，讓應用程式中指定部分內的所有按鈕共用相同的樣式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-509">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="31b3c-510">*UIThemeClasses/ThemeInfo .cs*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-510">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="31b3c-511">祖系元件可以使用串聯值元件來提供串聯值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-511">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="31b3c-512">@No__t-0 元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-512">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="31b3c-513">例如，範例應用程式會在其中一個應用程式的配置中，將主題資訊（`ThemeInfo`）指定為構成 `@Body` 屬性之配置主體的所有元件的串聯參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-513">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="31b3c-514">`ButtonClass` 會在版面配置元件中指派 `btn-success` 的值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-514">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="31b3c-515">任何子代元件都可以透過 @no__t 0 串聯物件來取用這個屬性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-515">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="31b3c-516">`CascadingValuesParametersLayout` 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-516">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
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

<span data-ttu-id="31b3c-517">為了利用串聯值，元件會使用 `[CascadingParameter]` 屬性來宣告串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-517">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="31b3c-518">串聯式值會依類型系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-518">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="31b3c-519">在範例應用程式中，`CascadingValuesParametersTheme` 元件會將 @no__t 級串聯值系結至階層式參數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-519">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="31b3c-520">參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="31b3c-520">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="31b3c-521">`CascadingValuesParametersTheme` 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-521">`CascadingValuesParametersTheme` component:</span></span>

```cshtml
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

<span data-ttu-id="31b3c-522">若要在相同的子樹中串聯多個相同類型的值，請為每個 `CascadingValue` 元件和其對應的 `CascadingParameter` 提供唯一的 @no__t 0 字串。</span><span class="sxs-lookup"><span data-stu-id="31b3c-522">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="31b3c-523">在下列範例中，兩個 @no__t 0 元件會依名稱將不同的實例 `MyCascadingType`：</span><span class="sxs-lookup"><span data-stu-id="31b3c-523">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```cshtml
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="31b3c-524">在子系元件中，串聯的參數會以名稱從上階元件中對應的串聯值接收其值：</span><span class="sxs-lookup"><span data-stu-id="31b3c-524">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```cshtml
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="31b3c-525">TabSet 範例</span><span class="sxs-lookup"><span data-stu-id="31b3c-525">TabSet example</span></span>

<span data-ttu-id="31b3c-526">串聯式參數也可以讓元件在元件階層之間共同作業。</span><span class="sxs-lookup"><span data-stu-id="31b3c-526">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="31b3c-527">例如，請考慮範例應用程式中的下列*TabSet*範例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-527">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="31b3c-528">範例應用程式有一個 @no__t 0 的介面，可執行索引標籤：</span><span class="sxs-lookup"><span data-stu-id="31b3c-528">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="31b3c-529">@No__t 0 元件使用 `TabSet` 元件，其中包含數個 @no__t 2 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-529">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="31b3c-530">子 `Tab` 元件並未明確地當做參數傳遞至 `TabSet`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-530">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="31b3c-531">相反地，子 `Tab` 元件是 `TabSet` 之子內容的一部分。</span><span class="sxs-lookup"><span data-stu-id="31b3c-531">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="31b3c-532">不過，`TabSet` 仍然需要知道每個 @no__t 1 元件，使其可以呈現標頭和使用中的索引標籤。若要在不需要額外程式碼的情況下啟用這項協調，@no__t 2 元件*可以將其本身提供為*串聯的值，然後由子 @no__t 4 元件挑選。</span><span class="sxs-lookup"><span data-stu-id="31b3c-532">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="31b3c-533">`TabSet` 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-533">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="31b3c-534">子系 `Tab` 元件會將包含的 `TabSet` 捕捉為串聯參數，因此 @no__t 2 元件會將自己加入至 `TabSet`，以及在其上使用的座標。</span><span class="sxs-lookup"><span data-stu-id="31b3c-534">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="31b3c-535">`Tab` 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-535">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="31b3c-536">Razor 範本</span><span class="sxs-lookup"><span data-stu-id="31b3c-536">Razor templates</span></span>

<span data-ttu-id="31b3c-537">轉譯片段可以使用 Razor 範本語法來定義。</span><span class="sxs-lookup"><span data-stu-id="31b3c-537">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="31b3c-538">Razor 範本是定義 UI 程式碼片段並採用下列格式的方式：</span><span class="sxs-lookup"><span data-stu-id="31b3c-538">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="31b3c-539">下列範例說明如何指定 `RenderFragment` 和 @no__t 1 值，並直接在元件中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="31b3c-539">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="31b3c-540">轉譯片段也可以當做引數傳遞至樣板[化元件](#templated-components)。</span><span class="sxs-lookup"><span data-stu-id="31b3c-540">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```cshtml
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="31b3c-541">上述程式碼的轉譯輸出：</span><span class="sxs-lookup"><span data-stu-id="31b3c-541">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="31b3c-542">手動 RenderTreeBuilder 邏輯</span><span class="sxs-lookup"><span data-stu-id="31b3c-542">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="31b3c-543">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` 提供操作元件和元素的方法，包括在程式碼中C#手動建立元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-543">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="31b3c-544">使用 `RenderTreeBuilder` 來建立元件是一個先進的案例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-544">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="31b3c-545">格式不正確的元件（例如，未封閉的標記標記）可能會導致未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="31b3c-545">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="31b3c-546">請考慮下列 @no__t 0 元件，它可以手動內建在另一個元件中：</span><span class="sxs-lookup"><span data-stu-id="31b3c-546">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="31b3c-547">在下列範例中，`CreateComponent` 方法中的迴圈會產生三個 @no__t 1 元件。</span><span class="sxs-lookup"><span data-stu-id="31b3c-547">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="31b3c-548">呼叫 `RenderTreeBuilder` 方法來建立元件（`OpenComponent` 和 `AddAttribute`）時，序號是源程式碼號。</span><span class="sxs-lookup"><span data-stu-id="31b3c-548">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="31b3c-549">Blazor 差異演算法依賴對應于不同程式程式碼的序號，而不是相異的呼叫調用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-549">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="31b3c-550">建立具有 `RenderTreeBuilder` 方法的元件時，請硬式編碼序號的引數。</span><span class="sxs-lookup"><span data-stu-id="31b3c-550">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="31b3c-551">**使用計算或計數器來產生序號可能會導致效能不佳。**</span><span class="sxs-lookup"><span data-stu-id="31b3c-551">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="31b3c-552">如需詳細資訊，請參閱[序號與程式程式碼號，而不是執行順序](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)一節。</span><span class="sxs-lookup"><span data-stu-id="31b3c-552">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="31b3c-553">`BuiltContent` 元件：</span><span class="sxs-lookup"><span data-stu-id="31b3c-553">`BuiltContent` component:</span></span>

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> <span data-ttu-id="31b3c-554">!WARNING@No__t-0 中的類型允許處理轉譯作業的*結果*。</span><span class="sxs-lookup"><span data-stu-id="31b3c-554">![WARNING] The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="31b3c-555">這些是 Blazor framework 執行的內部詳細資料。</span><span class="sxs-lookup"><span data-stu-id="31b3c-555">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="31b3c-556">這些類型應該被視為不*穩定*，未來的版本可能會變更。</span><span class="sxs-lookup"><span data-stu-id="31b3c-556">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="31b3c-557">序號與程式程式碼號相關，而不是執行順序</span><span class="sxs-lookup"><span data-stu-id="31b3c-557">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="31b3c-558">Blazor `.razor`：一律會編譯檔案。</span><span class="sxs-lookup"><span data-stu-id="31b3c-558">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="31b3c-559">這可能是 `.razor` 的絕佳優點，因為您可以使用編譯步驟來插入資訊，以在執行時間改善應用程式效能。</span><span class="sxs-lookup"><span data-stu-id="31b3c-559">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="31b3c-560">這些改良功能的重要範例包括*序號*。</span><span class="sxs-lookup"><span data-stu-id="31b3c-560">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="31b3c-561">序號會向運行時程表示輸出來自哪些不同和已排序的程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="31b3c-561">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="31b3c-562">執行時間會使用這項資訊，以線性時間產生有效率的樹狀差異，這比一般樹狀結構的差異演算法通常還能快得多。</span><span class="sxs-lookup"><span data-stu-id="31b3c-562">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="31b3c-563">請考慮下列簡單的 `.razor` 檔案：</span><span class="sxs-lookup"><span data-stu-id="31b3c-563">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="31b3c-564">上述程式碼會編譯成如下所示的內容：</span><span class="sxs-lookup"><span data-stu-id="31b3c-564">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="31b3c-565">當程式碼第一次執行時，如果 `someFlag` 是 `true`，產生器會接收：</span><span class="sxs-lookup"><span data-stu-id="31b3c-565">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="31b3c-566">序列</span><span class="sxs-lookup"><span data-stu-id="31b3c-566">Sequence</span></span> | <span data-ttu-id="31b3c-567">輸入</span><span class="sxs-lookup"><span data-stu-id="31b3c-567">Type</span></span>      | <span data-ttu-id="31b3c-568">資料</span><span class="sxs-lookup"><span data-stu-id="31b3c-568">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="31b3c-569">0</span><span class="sxs-lookup"><span data-stu-id="31b3c-569">0</span></span>        | <span data-ttu-id="31b3c-570">Text node</span><span class="sxs-lookup"><span data-stu-id="31b3c-570">Text node</span></span> | <span data-ttu-id="31b3c-571">First</span><span class="sxs-lookup"><span data-stu-id="31b3c-571">First</span></span>  |
| <span data-ttu-id="31b3c-572">1</span><span class="sxs-lookup"><span data-stu-id="31b3c-572">1</span></span>        | <span data-ttu-id="31b3c-573">Text node</span><span class="sxs-lookup"><span data-stu-id="31b3c-573">Text node</span></span> | <span data-ttu-id="31b3c-574">Second</span><span class="sxs-lookup"><span data-stu-id="31b3c-574">Second</span></span> |

<span data-ttu-id="31b3c-575">假設 `someFlag` 會變成 `false`，並再次轉譯標記。</span><span class="sxs-lookup"><span data-stu-id="31b3c-575">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="31b3c-576">這次，產生器會接收：</span><span class="sxs-lookup"><span data-stu-id="31b3c-576">This time, the builder receives:</span></span>

| <span data-ttu-id="31b3c-577">序列</span><span class="sxs-lookup"><span data-stu-id="31b3c-577">Sequence</span></span> | <span data-ttu-id="31b3c-578">輸入</span><span class="sxs-lookup"><span data-stu-id="31b3c-578">Type</span></span>       | <span data-ttu-id="31b3c-579">資料</span><span class="sxs-lookup"><span data-stu-id="31b3c-579">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="31b3c-580">1</span><span class="sxs-lookup"><span data-stu-id="31b3c-580">1</span></span>        | <span data-ttu-id="31b3c-581">Text node</span><span class="sxs-lookup"><span data-stu-id="31b3c-581">Text node</span></span>  | <span data-ttu-id="31b3c-582">Second</span><span class="sxs-lookup"><span data-stu-id="31b3c-582">Second</span></span> |

<span data-ttu-id="31b3c-583">當執行時間執行差異時，會看到順序 `0` 的專案已移除，因此它會產生下列簡單的*編輯腳本*：</span><span class="sxs-lookup"><span data-stu-id="31b3c-583">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="31b3c-584">移除第一個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="31b3c-584">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="31b3c-585">當您以程式設計方式產生序號時，會發生什麼錯誤</span><span class="sxs-lookup"><span data-stu-id="31b3c-585">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="31b3c-586">想像一下，您會改為撰寫下列轉譯樹產生器邏輯：</span><span class="sxs-lookup"><span data-stu-id="31b3c-586">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="31b3c-587">現在，第一個輸出是：</span><span class="sxs-lookup"><span data-stu-id="31b3c-587">Now, the first output is:</span></span>

| <span data-ttu-id="31b3c-588">序列</span><span class="sxs-lookup"><span data-stu-id="31b3c-588">Sequence</span></span> | <span data-ttu-id="31b3c-589">輸入</span><span class="sxs-lookup"><span data-stu-id="31b3c-589">Type</span></span>      | <span data-ttu-id="31b3c-590">資料</span><span class="sxs-lookup"><span data-stu-id="31b3c-590">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="31b3c-591">0</span><span class="sxs-lookup"><span data-stu-id="31b3c-591">0</span></span>        | <span data-ttu-id="31b3c-592">Text node</span><span class="sxs-lookup"><span data-stu-id="31b3c-592">Text node</span></span> | <span data-ttu-id="31b3c-593">First</span><span class="sxs-lookup"><span data-stu-id="31b3c-593">First</span></span>  |
| <span data-ttu-id="31b3c-594">1</span><span class="sxs-lookup"><span data-stu-id="31b3c-594">1</span></span>        | <span data-ttu-id="31b3c-595">Text node</span><span class="sxs-lookup"><span data-stu-id="31b3c-595">Text node</span></span> | <span data-ttu-id="31b3c-596">Second</span><span class="sxs-lookup"><span data-stu-id="31b3c-596">Second</span></span> |

<span data-ttu-id="31b3c-597">此結果與先前的案例相同，因此不會有負面問題存在。</span><span class="sxs-lookup"><span data-stu-id="31b3c-597">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="31b3c-598">`someFlag` 在第二個轉譯中是 `false`，輸出則是：</span><span class="sxs-lookup"><span data-stu-id="31b3c-598">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="31b3c-599">序列</span><span class="sxs-lookup"><span data-stu-id="31b3c-599">Sequence</span></span> | <span data-ttu-id="31b3c-600">輸入</span><span class="sxs-lookup"><span data-stu-id="31b3c-600">Type</span></span>      | <span data-ttu-id="31b3c-601">資料</span><span class="sxs-lookup"><span data-stu-id="31b3c-601">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="31b3c-602">0</span><span class="sxs-lookup"><span data-stu-id="31b3c-602">0</span></span>        | <span data-ttu-id="31b3c-603">Text node</span><span class="sxs-lookup"><span data-stu-id="31b3c-603">Text node</span></span> | <span data-ttu-id="31b3c-604">Second</span><span class="sxs-lookup"><span data-stu-id="31b3c-604">Second</span></span> |

<span data-ttu-id="31b3c-605">這次，diff 演算法發現發生了*兩*項變更，而演算法會產生下列編輯腳本：</span><span class="sxs-lookup"><span data-stu-id="31b3c-605">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="31b3c-606">將第一個文位元組點的值變更為 `Second`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-606">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="31b3c-607">移除第二個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="31b3c-607">Remove the second text node.</span></span>

<span data-ttu-id="31b3c-608">產生序號已遺失有關 `if/else` 分支和迴圈出現在原始程式碼中的所有實用資訊。</span><span class="sxs-lookup"><span data-stu-id="31b3c-608">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="31b3c-609">這會導致差異**兩倍，但前提**是之前。</span><span class="sxs-lookup"><span data-stu-id="31b3c-609">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="31b3c-610">這是一個簡單的範例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-610">This is a trivial example.</span></span> <span data-ttu-id="31b3c-611">在具有複雜和深層嵌套結構的更真實案例中，尤其是使用迴圈時，效能成本會更嚴重。</span><span class="sxs-lookup"><span data-stu-id="31b3c-611">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="31b3c-612">Diff 演算法不會立即識別已插入或移除的迴圈區塊或分支，而是必須在轉譯樹狀結構中進行深入的遞迴，而且通常會建立更長的編輯腳本，因為它 misinformed 了新舊結構的方式。相互關聯。</span><span class="sxs-lookup"><span data-stu-id="31b3c-612">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="31b3c-613">指引和結論</span><span class="sxs-lookup"><span data-stu-id="31b3c-613">Guidance and conclusions</span></span>

* <span data-ttu-id="31b3c-614">如果序號是動態產生的，應用程式效能會受到影響。</span><span class="sxs-lookup"><span data-stu-id="31b3c-614">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="31b3c-615">架構無法在執行時間自動建立自己的序號，因為必要的資訊不存在，除非是在編譯時期加以捕捉。</span><span class="sxs-lookup"><span data-stu-id="31b3c-615">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="31b3c-616">請勿撰寫以手動方式執行的長區塊，`RenderTreeBuilder` 邏輯。</span><span class="sxs-lookup"><span data-stu-id="31b3c-616">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="31b3c-617">偏好 @no__t 0 的檔案，並允許編譯器處理序號。</span><span class="sxs-lookup"><span data-stu-id="31b3c-617">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="31b3c-618">如果您無法避免手動 `RenderTreeBuilder` 邏輯，請將較長的程式碼區塊分割成較小的片段，包裝在 `OpenRegion` @ no__t-2 @ no__t-3 呼叫中。</span><span class="sxs-lookup"><span data-stu-id="31b3c-618">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="31b3c-619">每個區域都有自己的序號個別空間，因此您可以在每個區域內從零（或任何其他任一數字）重新開機。</span><span class="sxs-lookup"><span data-stu-id="31b3c-619">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="31b3c-620">如果序號已硬式編碼，則 diff 演算法只會要求序號增加值。</span><span class="sxs-lookup"><span data-stu-id="31b3c-620">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="31b3c-621">起始值和間距無關。</span><span class="sxs-lookup"><span data-stu-id="31b3c-621">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="31b3c-622">一個合法的選項是使用程式程式碼號做為序號，或從零開始，並以一個或數百個（或任何慣用的間隔）來增加。</span><span class="sxs-lookup"><span data-stu-id="31b3c-622">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="31b3c-623">Blazor 會使用序號，而其他樹狀結構比較的 UI 架構則不會使用它們。</span><span class="sxs-lookup"><span data-stu-id="31b3c-623">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="31b3c-624">當使用序號時，比較速度會更快，而且 Blazor 具有可自動處理序號的編譯步驟，以便開發人員撰寫 @no__t 0 檔案。</span><span class="sxs-lookup"><span data-stu-id="31b3c-624">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="31b3c-625">當地語系化</span><span class="sxs-lookup"><span data-stu-id="31b3c-625">Localization</span></span>

<span data-ttu-id="31b3c-626">Blazor 伺服器應用程式是使用[當地語系化中介軟體](xref:fundamentals/localization#localization-middleware)來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="31b3c-626">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="31b3c-627">中介軟體會為從應用程式要求資源的使用者選取適當的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-627">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="31b3c-628">您可以使用下列其中一種方法來設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="31b3c-628">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="31b3c-629">Cookie</span><span class="sxs-lookup"><span data-stu-id="31b3c-629">Cookies</span></span>](#cookies)
* [<span data-ttu-id="31b3c-630">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="31b3c-630">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="31b3c-631">如需詳細資訊與範例，請參閱<xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="31b3c-631">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="31b3c-632">Cookie</span><span class="sxs-lookup"><span data-stu-id="31b3c-632">Cookies</span></span>

<span data-ttu-id="31b3c-633">當地語系化文化特性 cookie 可以保存使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-633">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="31b3c-634">Cookie 是由應用程式主機頁面的 `OnGet` 方法所建立（*Pages/主機. cshtml .cs*）。</span><span class="sxs-lookup"><span data-stu-id="31b3c-634">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="31b3c-635">當地語系化中介軟體會在後續要求中讀取 cookie，以設定使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-635">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="31b3c-636">使用 cookie 可確保 WebSocket 連接可以正確地傳播文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-636">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="31b3c-637">如果當地語系化架構是以 URL 路徑或查詢字串為基礎，配置可能無法使用 Websocket，因而無法保存文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-637">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="31b3c-638">因此，建議的方法是使用當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="31b3c-638">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="31b3c-639">如果文化特性保存在當地語系化 cookie 中，任何技術都可以用來指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-639">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="31b3c-640">如果應用程式已建立伺服器端 ASP.NET Core 的當地語系化配置，請繼續使用應用程式的現有當地語系化基礎結構，並在應用程式的配置內設定當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="31b3c-640">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="31b3c-641">下列範例顯示如何在可由當地語系化中介軟體讀取的 cookie 中設定目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-641">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="31b3c-642">在 Blazor 伺服器應用程式中，使用下列內容建立*Pages/主機. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="31b3c-642">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="31b3c-643">當地語系化會在應用程式中處理：</span><span class="sxs-lookup"><span data-stu-id="31b3c-643">Localization is handled in the app:</span></span>

1. <span data-ttu-id="31b3c-644">瀏覽器會將初始 HTTP 要求傳送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-644">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="31b3c-645">文化特性是由當地語系化中介軟體所指派。</span><span class="sxs-lookup"><span data-stu-id="31b3c-645">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="31b3c-646">*_Host*中的 `OnGet` 方法會在 cookie 中保存文化特性，做為回應的一部分。</span><span class="sxs-lookup"><span data-stu-id="31b3c-646">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="31b3c-647">瀏覽器會開啟 WebSocket 連線，以建立互動式 Blazor 伺服器會話。</span><span class="sxs-lookup"><span data-stu-id="31b3c-647">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="31b3c-648">當地語系化中介軟體會讀取 cookie 並指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-648">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="31b3c-649">Blazor 伺服器會話的開頭是正確的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-649">The Blazor Server session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="31b3c-650">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="31b3c-650">Provide UI to choose the culture</span></span>

<span data-ttu-id="31b3c-651">為了提供允許使用者選取文化特性的 UI，建議使用以重新*導向為基礎的方法*。</span><span class="sxs-lookup"><span data-stu-id="31b3c-651">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="31b3c-652">此程式類似于在使用者嘗試存取安全資源時，在 web 應用程式中發生的狀況。 @ no__t-0the 使用者會重新導向至登入頁面，然後重新導向至原始資源。</span><span class="sxs-lookup"><span data-stu-id="31b3c-652">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="31b3c-653">應用程式會透過重新導向至控制器的方式，來保存使用者選取的文化特性。</span><span class="sxs-lookup"><span data-stu-id="31b3c-653">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="31b3c-654">控制器會將使用者選取的文化特性設定為 cookie，並將使用者重新導向回到原始 URI。</span><span class="sxs-lookup"><span data-stu-id="31b3c-654">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="31b3c-655">在伺服器上建立 HTTP 端點，以在 cookie 中設定使用者選取的文化特性，並執行重新導向回到原始 URI：</span><span class="sxs-lookup"><span data-stu-id="31b3c-655">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="31b3c-656">使用 [`LocalRedirect`] 動作結果來防止開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="31b3c-656">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="31b3c-657">如需詳細資訊，請參閱<xref:security/preventing-open-redirects>。</span><span class="sxs-lookup"><span data-stu-id="31b3c-657">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="31b3c-658">下列元件顯示當使用者選取文化特性時，如何執行初始重新導向的範例：</span><span class="sxs-lookup"><span data-stu-id="31b3c-658">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="31b3c-659">在 Blazor apps 中使用 .NET 當地語系化案例</span><span class="sxs-lookup"><span data-stu-id="31b3c-659">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="31b3c-660">在 Blazor apps 中，可以使用下列 .NET 當地語系化和全球化案例：</span><span class="sxs-lookup"><span data-stu-id="31b3c-660">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="31b3c-661">.NET 的資源系統</span><span class="sxs-lookup"><span data-stu-id="31b3c-661">.NET's resources system</span></span>
* <span data-ttu-id="31b3c-662">特定文化特性的數位和日期格式</span><span class="sxs-lookup"><span data-stu-id="31b3c-662">Culture-specific number and date formatting</span></span>

<span data-ttu-id="31b3c-663">Blazor 的 `@bind` 功能會根據使用者目前的文化特性來執行全球化。</span><span class="sxs-lookup"><span data-stu-id="31b3c-663">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="31b3c-664">如需詳細資訊，請參閱[資料](#data-binding)系結一節。</span><span class="sxs-lookup"><span data-stu-id="31b3c-664">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="31b3c-665">目前支援一組有限的 ASP.NET Core 當地語系化案例：</span><span class="sxs-lookup"><span data-stu-id="31b3c-665">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="31b3c-666">Blazor apps*支援*`IStringLocalizer<>`。</span><span class="sxs-lookup"><span data-stu-id="31b3c-666">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="31b3c-667">`IHtmlLocalizer<>`、`IViewLocalizer<>` 和資料批註當地語系化會 ASP.NET Core MVC 案例中，而且**不支援**Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="31b3c-667">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="31b3c-668">如需詳細資訊，請參閱<xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="31b3c-668">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="31b3c-669">可擴充向量圖形（SVG）影像</span><span class="sxs-lookup"><span data-stu-id="31b3c-669">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="31b3c-670">由於 Blazor 會轉譯 HTML，瀏覽器支援的影像（包括可擴充的向量圖形（SVG）影像（*svg*））可透過 `<img>` 標記來支援：</span><span class="sxs-lookup"><span data-stu-id="31b3c-670">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="31b3c-671">同樣地，樣式表單檔案（ *.css*）的 CSS 規則也支援 SVG 影像：</span><span class="sxs-lookup"><span data-stu-id="31b3c-671">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="31b3c-672">不過，在所有案例中不支援內嵌 SVG 標記。</span><span class="sxs-lookup"><span data-stu-id="31b3c-672">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="31b3c-673">如果您將 `<svg>` 標記直接放入元件檔案（*razor*），則會支援基本映射轉譯，但尚不支援許多先進的案例。</span><span class="sxs-lookup"><span data-stu-id="31b3c-673">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="31b3c-674">例如，目前未遵守 `<use>` 標籤，且 `@bind` 無法與某些 SVG 標記搭配使用。</span><span class="sxs-lookup"><span data-stu-id="31b3c-674">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="31b3c-675">我們希望在未來的版本中解決這些限制。</span><span class="sxs-lookup"><span data-stu-id="31b3c-675">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31b3c-676">其他資源</span><span class="sxs-lookup"><span data-stu-id="31b3c-676">Additional resources</span></span>

* <span data-ttu-id="31b3c-677"><xref:security/blazor/server> &ndash; 包含有關建立 Blazor 伺服器應用程式的指導方針，必須對抗資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="31b3c-677"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
