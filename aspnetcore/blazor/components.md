---
title: 建立和使用 ASP.NET Core Razor 元件
author: guardrex
description: 瞭解如何建立和使用 Razor 元件, 包括如何系結至資料、處理事件, 以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/components
ms.openlocfilehash: 752f49f020acf26efcb304ed5e28e27c478dac83
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487599"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="cb82e-103">建立和使用 ASP.NET Core Razor 元件</span><span class="sxs-lookup"><span data-stu-id="cb82e-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="cb82e-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cb82e-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="cb82e-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cb82e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cb82e-106">Blazor 應用程式是使用*元件*所建立。</span><span class="sxs-lookup"><span data-stu-id="cb82e-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="cb82e-107">「元件」 (component) 是一種獨立的使用者介面 (UI) 區塊, 例如頁面、對話方塊或表單。</span><span class="sxs-lookup"><span data-stu-id="cb82e-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="cb82e-108">元件包含 HTML 標籤, 以及插入資料或回應 UI 事件所需的處理邏輯。</span><span class="sxs-lookup"><span data-stu-id="cb82e-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="cb82e-109">元件具有彈性且輕量。</span><span class="sxs-lookup"><span data-stu-id="cb82e-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="cb82e-110">它們可以在專案之間進行嵌套、重複使用及共用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="cb82e-111">元件類別</span><span class="sxs-lookup"><span data-stu-id="cb82e-111">Component classes</span></span>

<span data-ttu-id="cb82e-112">元件會使用C#和 HTML 標籤的組合, 在[razor](xref:mvc/views/razor)元件檔案 (*razor*) 中執行。</span><span class="sxs-lookup"><span data-stu-id="cb82e-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="cb82e-113">Blazor 中的元件正式稱為*Razor 元件*。</span><span class="sxs-lookup"><span data-stu-id="cb82e-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="cb82e-114">元件的名稱必須以大寫字元開頭。</span><span class="sxs-lookup"><span data-stu-id="cb82e-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="cb82e-115">例如, *MyCoolComponent*有效, 而*MyCoolComponent*則無效。</span><span class="sxs-lookup"><span data-stu-id="cb82e-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="cb82e-116">只要使用`_RazorComponentInclude` MSBuild 屬性將檔案識別為 Razor 元件檔案, 就可以使用 *. cshtml*副檔名來撰寫元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-116">Components can be authored using the *.cshtml* file extension as long as the files are identified as Razor component files using the `_RazorComponentInclude` MSBuild property.</span></span> <span data-ttu-id="cb82e-117">例如, 指定*Pages*資料夾下所有 *. cshtml*檔案的應用程式, 都應該視為 Razor 元件檔案:</span><span class="sxs-lookup"><span data-stu-id="cb82e-117">For example, an app that specifies that all *.cshtml* files under the *Pages* folder should be treated as Razor components files:</span></span>

```xml
<PropertyGroup>
  <_RazorComponentInclude>Pages\**\*.cshtml</_RazorComponentInclude>
</PropertyGroup>
```

<span data-ttu-id="cb82e-118">元件的 UI 是使用 HTML 定義的。</span><span class="sxs-lookup"><span data-stu-id="cb82e-118">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="cb82e-119">動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。</span><span class="sxs-lookup"><span data-stu-id="cb82e-119">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="cb82e-120">編譯應用程式時, 會將 HTML 標籤和C#轉譯邏輯轉換成元件類別。</span><span class="sxs-lookup"><span data-stu-id="cb82e-120">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="cb82e-121">產生的類別名稱與檔案的名稱相符。</span><span class="sxs-lookup"><span data-stu-id="cb82e-121">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="cb82e-122">元件類別的成員均定義於 `@code` 區塊中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-122">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="cb82e-123">`@code`在區塊中, 會使用事件處理或定義其他元件邏輯的方法來指定元件狀態 (屬性、欄位)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-123">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="cb82e-124">允許一個以上的 `@code` 區塊。</span><span class="sxs-lookup"><span data-stu-id="cb82e-124">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="cb82e-125">在 ASP.NET Core 3.0 的先前預覽中`@functions` , 區塊用於與 Razor 元件中的`@code`區塊相同的用途。</span><span class="sxs-lookup"><span data-stu-id="cb82e-125">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="cb82e-126">`@functions`區塊會繼續在 Razor 元件中運作, 但我們建議使用`@code` ASP.NET Core 3.0 Preview 6 或更新版本中的區塊。</span><span class="sxs-lookup"><span data-stu-id="cb82e-126">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="cb82e-127">元件成員可以使用C#開頭為`@`的運算式, 做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="cb82e-127">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="cb82e-128">例如, C#欄位的呈現方式是在功能變數名稱`@`前面加上。</span><span class="sxs-lookup"><span data-stu-id="cb82e-128">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="cb82e-129">下列範例會評估並呈現:</span><span class="sxs-lookup"><span data-stu-id="cb82e-129">The following example evaluates and renders:</span></span>

* <span data-ttu-id="cb82e-130">`_headingFontStyle`至的 CSS 屬性值`font-style`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-130">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="cb82e-131">`_headingText`至`<h1>`元素的內容。</span><span class="sxs-lookup"><span data-stu-id="cb82e-131">`_headingText` to the content of the `<h1>` element.</span></span>

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="cb82e-132">一開始呈現元件之後, 元件會重新產生其轉譯樹狀結構, 以回應事件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-132">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="cb82e-133">然後, Blazor 會比較新的轉譯樹狀結構與上一個, 並將任何修改套用至瀏覽器的檔物件模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-133">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="cb82e-134">元件是一般C#類別, 可以放在專案內的任何位置。</span><span class="sxs-lookup"><span data-stu-id="cb82e-134">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="cb82e-135">產生網頁的元件通常會位於*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-135">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="cb82e-136">非頁面元件通常會放在*共用*資料夾中, 或加入至專案的自訂資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-136">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="cb82e-137">若要使用自訂資料夾, 請將自訂資料夾的命名空間新增至父元件或應用程式的 *_Imports*檔案。</span><span class="sxs-lookup"><span data-stu-id="cb82e-137">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="cb82e-138">例如, 下列命名空間會在應用程式的根命名空間為`WebApplication`時, 讓元件資料夾中的元件可供使用:</span><span class="sxs-lookup"><span data-stu-id="cb82e-138">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```cshtml
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="cb82e-139">將元件整合到 Razor Pages 和 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="cb82e-139">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="cb82e-140">將元件與現有的 Razor Pages 和 MVC 應用程式搭配使用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-140">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="cb82e-141">不需要重新撰寫現有的頁面或 views 就能使用 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-141">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="cb82e-142">當頁面或視圖呈現時, 會同時資源清單元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-142">When the page or view is rendered, components are prerendered at the same time.</span></span>

<span data-ttu-id="cb82e-143">若要從頁面或視圖呈現元件, 請使用`RenderComponentAsync<TComponent>` HTML helper 方法:</span><span class="sxs-lookup"><span data-stu-id="cb82e-143">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

<span data-ttu-id="cb82e-144">雖然頁面和視圖可以使用元件, 但相反的情況並非如此。</span><span class="sxs-lookup"><span data-stu-id="cb82e-144">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="cb82e-145">元件不能使用視圖和頁面特定的案例, 例如部分視圖和區段。</span><span class="sxs-lookup"><span data-stu-id="cb82e-145">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="cb82e-146">若要在元件中使用部分視圖的邏輯, 請將部分視圖邏輯分解成元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-146">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="cb82e-147">如需有關如何呈現元件, 以及如何在 Blazor 伺服器端應用程式中管理元件狀態的詳細資訊<xref:blazor/hosting-models> , 請參閱一文。</span><span class="sxs-lookup"><span data-stu-id="cb82e-147">For more information on how components are rendered and component state is managed in Blazor server-side apps, see the <xref:blazor/hosting-models> article.</span></span>

## <a name="use-components"></a><span data-ttu-id="cb82e-148">使用元件</span><span class="sxs-lookup"><span data-stu-id="cb82e-148">Use components</span></span>

<span data-ttu-id="cb82e-149">元件可以包含其他元件, 方法是使用 HTML 專案語法來宣告它們。</span><span class="sxs-lookup"><span data-stu-id="cb82e-149">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="cb82e-150">使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。</span><span class="sxs-lookup"><span data-stu-id="cb82e-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="cb82e-151">屬性系結會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="cb82e-151">Attribute binding is case sensitive.</span></span> <span data-ttu-id="cb82e-152">例如, `@bind`是有效的, 而且`@Bind`無效。</span><span class="sxs-lookup"><span data-stu-id="cb82e-152">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="cb82e-153">在*Index*中的下列標記會呈現`HeadingComponent`實例:</span><span class="sxs-lookup"><span data-stu-id="cb82e-153">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.razor?name=snippet_HeadingComponent)]

<span data-ttu-id="cb82e-154">*Components/HeadingComponent. razor*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-154">*Components/HeadingComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/HeadingComponent.razor)]

<span data-ttu-id="cb82e-155">如果元件包含的 HTML 專案具有大寫的第一個字母, 但不符合元件名稱, 則會發出警告, 指出該元素有未預期的名稱。</span><span class="sxs-lookup"><span data-stu-id="cb82e-155">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="cb82e-156">為元件的命名空間加入語句,可讓元件可用,這會移除警告。`@using`</span><span class="sxs-lookup"><span data-stu-id="cb82e-156">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="cb82e-157">元件參數</span><span class="sxs-lookup"><span data-stu-id="cb82e-157">Component parameters</span></span>

<span data-ttu-id="cb82e-158">元件可以具有*元件參數*, 其定義方式是在元件類別上使用具有`[Parameter]`屬性的公用屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-158">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="cb82e-159">使用這些屬性來指定標記中元件的引數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-159">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="cb82e-160">*Components/ChildComponent. razor*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-160">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="cb82e-161">在下列範例中, `ParentComponent`會設定的`Title`屬性`ChildComponent`值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-161">In the following example, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="cb82e-162">*Pages/ParentComponent. razor*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-162">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=5-6)]

## <a name="child-content"></a><span data-ttu-id="cb82e-163">子內容</span><span class="sxs-lookup"><span data-stu-id="cb82e-163">Child content</span></span>

<span data-ttu-id="cb82e-164">元件可以設定另一個元件的內容。</span><span class="sxs-lookup"><span data-stu-id="cb82e-164">Components can set the content of another component.</span></span> <span data-ttu-id="cb82e-165">指派元件會在指定接收元件的標記之間提供內容。</span><span class="sxs-lookup"><span data-stu-id="cb82e-165">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="cb82e-166">在下列範例中, `ChildComponent`有一個`ChildContent`代表`RenderFragment`的屬性, 代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="cb82e-166">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="cb82e-167">的值`ChildContent`位於元件的標記中, 應在其中呈現內容。</span><span class="sxs-lookup"><span data-stu-id="cb82e-167">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="cb82e-168">的值`ChildContent`會從父元件接收, 並在啟動載入面板的`panel-body`內轉譯。</span><span class="sxs-lookup"><span data-stu-id="cb82e-168">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="cb82e-169">*Components/ChildComponent. razor*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-169">*Components/ChildComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="cb82e-170">接收`RenderFragment`內容的屬性必須依照慣例命名`ChildContent` 。</span><span class="sxs-lookup"><span data-stu-id="cb82e-170">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="cb82e-171">下列`ParentComponent`可以提供內容, 將內容`<ChildComponent>`放`ChildComponent`在標籤內來呈現。</span><span class="sxs-lookup"><span data-stu-id="cb82e-171">The following `ParentComponent` can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="cb82e-172">*Pages/ParentComponent. razor*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-172">*Pages/ParentComponent.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="cb82e-173">屬性展開和任意參數</span><span class="sxs-lookup"><span data-stu-id="cb82e-173">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="cb82e-174">除了元件的宣告參數之外, 元件還可以捕捉和轉譯其他屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-174">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="cb82e-175">您可以在字典中捕捉其他屬性, 然後在使用[@attributes](xref:mvc/views/razor#attributes) Razor 指示詞轉譯元件時, splatted 至元素。</span><span class="sxs-lookup"><span data-stu-id="cb82e-175">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [@attributes](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="cb82e-176">當定義的元件會產生支援各種自訂的標記專案時, 這個案例就很有用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-176">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="cb82e-177">例如, 針對`<input>`支援許多參數的, 分別定義屬性可能會很繁瑣。</span><span class="sxs-lookup"><span data-stu-id="cb82e-177">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="cb82e-178">在下列範例中, 第一個`<input>`元素 (`id="useIndividualParams"`) 會使用個別的元件參數, 而`<input>`第二`id="useAttributesDict"`個元素 () 則使用屬性展開:</span><span class="sxs-lookup"><span data-stu-id="cb82e-178">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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
            { "required", "true" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="cb82e-179">參數的類型必須使用字串索引`IEnumerable<KeyValuePair<string, object>>`鍵來執行。</span><span class="sxs-lookup"><span data-stu-id="cb82e-179">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="cb82e-180">在`IReadOnlyDictionary<string, object>`此案例中, 使用也是一個選項。</span><span class="sxs-lookup"><span data-stu-id="cb82e-180">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="cb82e-181">使用這兩種方法的轉譯元素都相同:`<input>`</span><span class="sxs-lookup"><span data-stu-id="cb82e-181">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="true"
       size="50">
```

<span data-ttu-id="cb82e-182">若要接受任意屬性, 請使用`[Parameter]` `CaptureUnmatchedValues`屬性設定為的屬性來`true`定義元件參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-182">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```cshtml
@code {
    [Parameter(CaptureUnmatchedAttributes = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="cb82e-183">`CaptureUnmatchedValues` 上`[Parameter]`的屬性允許參數比對與任何其他參數不相符的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-183">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="cb82e-184">元件只能定義具有`CaptureUnmatchedValues`的單一參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-184">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="cb82e-185">搭配使用`CaptureUnmatchedValues`的屬性類型必須可從`Dictionary<string, object>`使用字串索引鍵來指派。</span><span class="sxs-lookup"><span data-stu-id="cb82e-185">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="cb82e-186">`IEnumerable<KeyValuePair<string, object>>`或`IReadOnlyDictionary<string, object>`也是此案例中的選項。</span><span class="sxs-lookup"><span data-stu-id="cb82e-186">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

## <a name="data-binding"></a><span data-ttu-id="cb82e-187">資料繫結</span><span class="sxs-lookup"><span data-stu-id="cb82e-187">Data binding</span></span>

<span data-ttu-id="cb82e-188">元件和 DOM 元素的資料系結都是使用[@bind](xref:mvc/views/razor#bind)屬性來完成。</span><span class="sxs-lookup"><span data-stu-id="cb82e-188">Data binding to both components and DOM elements is accomplished with the [@bind](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="cb82e-189">下列範例`_italicsCheck`會將欄位系結至核取方塊的已核取狀態:</span><span class="sxs-lookup"><span data-stu-id="cb82e-189">The following example binds the `_italicsCheck` field to the check box's checked state:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    @bind="_italicsCheck" />
```

<span data-ttu-id="cb82e-190">選取並清除核取方塊時, 屬性的值會分別更新為`true`和。 `false`</span><span class="sxs-lookup"><span data-stu-id="cb82e-190">When the check box is selected and cleared, the property's value is updated to `true` and `false`, respectively.</span></span>

<span data-ttu-id="cb82e-191">只有在呈現元件時, 才會在 UI 中更新此核取方塊, 而不是回應變更屬性的值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-191">The check box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="cb82e-192">由於元件會在事件處理常式程式碼執行之後自行呈現, 因此屬性更新通常會立即反映在 UI 中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-192">Since components render themselves after event handler code executes, property updates are usually reflected in the UI immediately.</span></span>

<span data-ttu-id="cb82e-193">使用`@bind`搭配屬性(`<input @bind="CurrentValue" />`) 基本上等同于下列內容: `CurrentValue`</span><span class="sxs-lookup"><span data-stu-id="cb82e-193">Using `@bind` with a `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```cshtml
<input value="@CurrentValue"
    @onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)" />
```

<span data-ttu-id="cb82e-194">當元件呈現時, `value`輸入專案的會`CurrentValue`來自屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-194">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="cb82e-195">當使用者在文字方塊中輸入時, `onchange`就會引發事件, `CurrentValue`並將屬性設定為已變更的值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-195">When the user types in the text box, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="cb82e-196">事實上, 程式碼產生會稍微複雜一點, 因為`@bind`會處理一些執行型別轉換的情況。</span><span class="sxs-lookup"><span data-stu-id="cb82e-196">In reality, the code generation is a little more complex because `@bind` handles a few cases where type conversions are performed.</span></span> <span data-ttu-id="cb82e-197">在原則上`@bind` , 會將運算式的目前值`value`與屬性產生關聯, 並使用已註冊的處理常式來處理變更。</span><span class="sxs-lookup"><span data-stu-id="cb82e-197">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="cb82e-198">`onchange`除了使用[@bind-value](xref:mvc/views/razor#bind) `event` [@bind-value:event](xref:mvc/views/razor#bind)語法來處理事件之外, 也可以使用其他事件來系結屬性或欄位, 方法是指定具有參數 () 的屬性。 `@bind`</span><span class="sxs-lookup"><span data-stu-id="cb82e-198">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [@bind-value](xref:mvc/views/razor#bind) attribute with an `event` parameter ([@bind-value:event](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="cb82e-199">下列範例`CurrentValue`會系結`oninput`事件的屬性:</span><span class="sxs-lookup"><span data-stu-id="cb82e-199">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```cshtml
<input @bind-value="CurrentValue" @bind-value:event="oninput" />
```

<span data-ttu-id="cb82e-200">不同`onchange`于當元素失去`oninput`焦點時引發的, 會在文字方塊的值變更時引發。</span><span class="sxs-lookup"><span data-stu-id="cb82e-200">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="cb82e-201">**全球化**</span><span class="sxs-lookup"><span data-stu-id="cb82e-201">**Globalization**</span></span>

<span data-ttu-id="cb82e-202">`@bind`系統會使用目前文化特性的規則, 將值格式化以顯示和剖析。</span><span class="sxs-lookup"><span data-stu-id="cb82e-202">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="cb82e-203">您可以從<xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>屬性存取目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-203">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="cb82e-204">[InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture)用於下欄欄位類型 (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="cb82e-204">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="cb82e-205">前面的欄位類型:</span><span class="sxs-lookup"><span data-stu-id="cb82e-205">The preceding field types:</span></span>

* <span data-ttu-id="cb82e-206">會使用其適當的瀏覽器格式規則來顯示。</span><span class="sxs-lookup"><span data-stu-id="cb82e-206">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="cb82e-207">不能包含自由格式的文字。</span><span class="sxs-lookup"><span data-stu-id="cb82e-207">Can't contain free-form text.</span></span>
* <span data-ttu-id="cb82e-208">根據瀏覽器的執行方式提供使用者互動特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-208">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="cb82e-209">下欄欄位類型具有特定的格式需求, 而且目前不受 Blazor 支援, 因為並非所有主要瀏覽器都支援:</span><span class="sxs-lookup"><span data-stu-id="cb82e-209">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="cb82e-210">`@bind`支援參數, 以提供用於剖析和格式化值的。 <xref:System.Globalization.CultureInfo?displayProperty=fullName> `@bind:culture`</span><span class="sxs-lookup"><span data-stu-id="cb82e-210">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="cb82e-211">使用`date`和欄位類型時, `number`不建議指定文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-211">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="cb82e-212">`date`和`number`具有提供必要文化特性的內建 Blazor 支援。</span><span class="sxs-lookup"><span data-stu-id="cb82e-212">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="cb82e-213">如需如何設定使用者文化特性的詳細資訊, 請參閱[當地語系化](#localization)一節。</span><span class="sxs-lookup"><span data-stu-id="cb82e-213">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="cb82e-214">**格式字串**</span><span class="sxs-lookup"><span data-stu-id="cb82e-214">**Format strings**</span></span>

<span data-ttu-id="cb82e-215">資料系結會使用來[@bind:format](xref:mvc/views/razor#bind)處理格式字串。<xref:System.DateTime></span><span class="sxs-lookup"><span data-stu-id="cb82e-215">Data binding works with <xref:System.DateTime> format strings using [@bind:format](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="cb82e-216">目前無法使用其他格式運算式, 例如貨幣或數位格式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-216">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```cshtml
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="cb82e-217">在上述程式碼中, `<input>`元素的欄位類型 (`type`) 預設為`text`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-217">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="cb82e-218">`@bind:format`支援系結下列 .NET 類型:</span><span class="sxs-lookup"><span data-stu-id="cb82e-218">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="cb82e-219"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="cb82e-219"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="cb82e-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="cb82e-220"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="cb82e-221">屬性會指定要套用`value`至`<input>`元素之的日期格式。 `@bind:format`</span><span class="sxs-lookup"><span data-stu-id="cb82e-221">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="cb82e-222">當發生`onchange`事件時, 也會使用此格式來剖析值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-222">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="cb82e-223">不建議指定`date`欄位類型的格式, 因為 Blazor 具有格式日期的內建支援。</span><span class="sxs-lookup"><span data-stu-id="cb82e-223">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span>

<span data-ttu-id="cb82e-224">**元件參數**</span><span class="sxs-lookup"><span data-stu-id="cb82e-224">**Component parameters**</span></span>

<span data-ttu-id="cb82e-225">Binding 可辨識元件參數, `@bind-{property}`其中可以跨元件系結屬性值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-225">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="cb82e-226">下列子元件 (`ChildComponent`) `Year`具有元件參數和`YearChanged`回呼:</span><span class="sxs-lookup"><span data-stu-id="cb82e-226">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="cb82e-227">`EventCallback<T>`[app eventcallback](#eventcallback)一節中會說明。</span><span class="sxs-lookup"><span data-stu-id="cb82e-227">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="cb82e-228">下列父元件會使用`ChildComponent` , 並`ParentYear`將參數從父系系結至`Year`子元件上的參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-228">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="cb82e-229">載入會`ParentComponent`產生下列標記:</span><span class="sxs-lookup"><span data-stu-id="cb82e-229">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="cb82e-230">`ParentYear`如果藉由選取`ParentComponent`中的按鈕來變更屬性的值`ChildComponent` , `Year`則會更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-230">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="cb82e-231">當為重新顯示時`Year` , 的新值會在 UI 中呈現: `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="cb82e-231">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="cb82e-232">參數`Year`是可系結的, 因為它`YearChanged`有`Year`符合參數類型的伴隨事件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-232">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="cb82e-233">依照慣例, `<ChildComponent @bind-Year="ParentYear" />`基本上等同于撰寫:</span><span class="sxs-lookup"><span data-stu-id="cb82e-233">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```cshtml
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="cb82e-234">一般來說, 屬性可以使用`@bind-property:event`屬性系結至對應的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-234">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="cb82e-235">例如, 您可以使用`MyProp`下列兩個屬性`MyEventHandler` , 將屬性系結至:</span><span class="sxs-lookup"><span data-stu-id="cb82e-235">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```cshtml
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="cb82e-236">事件處理</span><span class="sxs-lookup"><span data-stu-id="cb82e-236">Event handling</span></span>

<span data-ttu-id="cb82e-237">Razor 元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="cb82e-237">Razor components provide event handling features.</span></span> <span data-ttu-id="cb82e-238">針對名為`on{event}`的 HTML 專案屬性 (例如, `onclick`和`onsubmit`) 與委派類型的值, Razor 元件會將屬性的值視為事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-238">For an HTML element attribute named `on{event}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="cb82e-239">屬性的名稱一律會格式化[ @on{event}](xref:mvc/views/razor#onevent)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-239">The attribute's name is always formatted [@on{event}](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="cb82e-240">下列程式碼會在`UpdateHeading` UI 中選取按鈕時呼叫方法:</span><span class="sxs-lookup"><span data-stu-id="cb82e-240">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="cb82e-241">下列程式碼會在`CheckChanged` UI 中變更核取方塊時呼叫方法:</span><span class="sxs-lookup"><span data-stu-id="cb82e-241">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```cshtml
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="cb82e-242">事件處理常式也可以是非同步<xref:System.Threading.Tasks.Task>, 並且會傳回。</span><span class="sxs-lookup"><span data-stu-id="cb82e-242">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="cb82e-243">不需要手動呼叫`StateHasChanged()`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-243">There's no need to manually call `StateHasChanged()`.</span></span> <span data-ttu-id="cb82e-244">例外狀況會在發生時記錄。</span><span class="sxs-lookup"><span data-stu-id="cb82e-244">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="cb82e-245">在下列範例中, `UpdateHeading`當選取按鈕時, 會以非同步方式呼叫:</span><span class="sxs-lookup"><span data-stu-id="cb82e-245">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```cshtml
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="cb82e-246">事件引數類型</span><span class="sxs-lookup"><span data-stu-id="cb82e-246">Event argument types</span></span>

<span data-ttu-id="cb82e-247">對於某些事件, 則允許事件引數類型。</span><span class="sxs-lookup"><span data-stu-id="cb82e-247">For some events, event argument types are permitted.</span></span> <span data-ttu-id="cb82e-248">如果不需要存取這些事件種類的其中一個, 則在方法呼叫中不需要。</span><span class="sxs-lookup"><span data-stu-id="cb82e-248">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="cb82e-249">下表顯示支援的[UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) 。</span><span class="sxs-lookup"><span data-stu-id="cb82e-249">Supported [UIEventArgs](https://github.com/aspnet/AspNetCore/blob/release/3.0-preview8/src/Components/Components/src/UIEventArgs.cs) are shown in the following table.</span></span>

| <span data-ttu-id="cb82e-250">Event - 事件</span><span class="sxs-lookup"><span data-stu-id="cb82e-250">Event</span></span> | <span data-ttu-id="cb82e-251">類別</span><span class="sxs-lookup"><span data-stu-id="cb82e-251">Class</span></span> |
| ----- | ----- |
| <span data-ttu-id="cb82e-252">剪貼簿</span><span class="sxs-lookup"><span data-stu-id="cb82e-252">Clipboard</span></span> | `UIClipboardEventArgs` |
| <span data-ttu-id="cb82e-253">拖放式</span><span class="sxs-lookup"><span data-stu-id="cb82e-253">Drag</span></span>  | <span data-ttu-id="cb82e-254">`UIDragEventArgs`會在拖放作業期間用來保存拖曳的資料, 而且可能會包含一或`UIDataTransferItem`多個。 &ndash; `DataTransfer`</span><span class="sxs-lookup"><span data-stu-id="cb82e-254">`UIDragEventArgs` &ndash; `DataTransfer` is used to hold the dragged data during a drag and drop operation and may hold one or more `UIDataTransferItem`.</span></span> <span data-ttu-id="cb82e-255">`UIDataTransferItem`表示一個拖曳資料項目。</span><span class="sxs-lookup"><span data-stu-id="cb82e-255">`UIDataTransferItem` represents one drag data item.</span></span> |
| <span data-ttu-id="cb82e-256">Error</span><span class="sxs-lookup"><span data-stu-id="cb82e-256">Error</span></span> | `UIErrorEventArgs` |
| <span data-ttu-id="cb82e-257">焦點</span><span class="sxs-lookup"><span data-stu-id="cb82e-257">Focus</span></span> | <span data-ttu-id="cb82e-258">`UIFocusEventArgs`不包含的`relatedTarget`支援。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="cb82e-258">`UIFocusEventArgs` &ndash; Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="cb82e-259">`<input>` 變更</span><span class="sxs-lookup"><span data-stu-id="cb82e-259">`<input>` change</span></span> | `UIChangeEventArgs` |
| <span data-ttu-id="cb82e-260">鍵盤</span><span class="sxs-lookup"><span data-stu-id="cb82e-260">Keyboard</span></span> | `UIKeyboardEventArgs` |
| <span data-ttu-id="cb82e-261">滑鼠</span><span class="sxs-lookup"><span data-stu-id="cb82e-261">Mouse</span></span> | `UIMouseEventArgs` |
| <span data-ttu-id="cb82e-262">滑鼠指標</span><span class="sxs-lookup"><span data-stu-id="cb82e-262">Mouse pointer</span></span> | `UIPointerEventArgs` |
| <span data-ttu-id="cb82e-263">滑鼠滾輪</span><span class="sxs-lookup"><span data-stu-id="cb82e-263">Mouse wheel</span></span> | `UIWheelEventArgs` |
| <span data-ttu-id="cb82e-264">進度</span><span class="sxs-lookup"><span data-stu-id="cb82e-264">Progress</span></span> | `UIProgressEventArgs` |
| <span data-ttu-id="cb82e-265">觸控</span><span class="sxs-lookup"><span data-stu-id="cb82e-265">Touch</span></span> | <span data-ttu-id="cb82e-266">`UITouchEventArgs`&ndash; 代表觸控裝置上`UITouchPoint`的單一連絡人點。</span><span class="sxs-lookup"><span data-stu-id="cb82e-266">`UITouchEventArgs` &ndash; `UITouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="cb82e-267">如需上表中事件的屬性和事件處理行為的詳細資訊, 請參閱[參考來源中的 EventArgs 類別](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-267">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source](https://github.com/aspnet/AspNetCore/tree/release/3.0-preview8/src/Components/Web/src).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="cb82e-268">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="cb82e-268">Lambda expressions</span></span>

<span data-ttu-id="cb82e-269">您也可以使用 Lambda 運算式:</span><span class="sxs-lookup"><span data-stu-id="cb82e-269">Lambda expressions can also be used:</span></span>

```cshtml
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="cb82e-270">關閉其他值 (例如反覆運算一組專案時) 通常會很方便。</span><span class="sxs-lookup"><span data-stu-id="cb82e-270">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="cb82e-271">下列範例會建立三個按鈕, 在 UI 中`UpdateHeading`選取時, 每個`UIMouseEventArgs`都會呼叫傳遞事件引數`buttonNumber`() 和其按鈕編號 ():</span><span class="sxs-lookup"><span data-stu-id="cb82e-271">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`UIMouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="cb82e-272">請不要在 lambda 運算式中直接`i`使用`for`迴圈中的迴圈變數 ()。</span><span class="sxs-lookup"><span data-stu-id="cb82e-272">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="cb82e-273">否則, 所有 lambda 運算式都會使用相同的變數, `i`使的值在所有 lambda 中都相同。</span><span class="sxs-lookup"><span data-stu-id="cb82e-273">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="cb82e-274">請一律在本機變數中捕捉其值`buttonNumber` (在上述範例中為), 然後使用它。</span><span class="sxs-lookup"><span data-stu-id="cb82e-274">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="cb82e-275">App eventcallback</span><span class="sxs-lookup"><span data-stu-id="cb82e-275">EventCallback</span></span>

<span data-ttu-id="cb82e-276">有一個常見的嵌套元件案例, 就是在子元件事件發生&mdash;時 (例如, `onclick`當子系中發生事件時), 想要執行父元件的方法。</span><span class="sxs-lookup"><span data-stu-id="cb82e-276">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="cb82e-277">若要在元件之間公開事件, `EventCallback`請使用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-277">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="cb82e-278">父元件可以將回呼方法指派給子元件的`EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-278">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="cb82e-279">範例`ChildComponent`應用程式中的會示範如何`EventCallback`設定按鈕`onclick`的處理常式, 以接收來自範例的`ParentComponent`委派。</span><span class="sxs-lookup"><span data-stu-id="cb82e-279">The `ChildComponent` in the sample app demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="cb82e-280">會使用`UIMouseEventArgs`輸入,`onclick`這適用于來自週邊裝置的事件: `EventCallback`</span><span class="sxs-lookup"><span data-stu-id="cb82e-280">The `EventCallback` is typed with `UIMouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="cb82e-281">會將子系`ShowMessage`設定為其方法:`EventCallback<T>` `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="cb82e-281">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.razor?name=snippet_ParentComponent&highlight=6,16-19)]

<span data-ttu-id="cb82e-282">當您在中`ChildComponent`選取按鈕時:</span><span class="sxs-lookup"><span data-stu-id="cb82e-282">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="cb82e-283">會呼叫`ShowMessage`的方法。 `ParentComponent`</span><span class="sxs-lookup"><span data-stu-id="cb82e-283">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="cb82e-284">`messageText`會更新並顯示在中`ParentComponent`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-284">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="cb82e-285">回呼的方法`StateHasChanged` (`ShowMessage`) 中不需要呼叫。</span><span class="sxs-lookup"><span data-stu-id="cb82e-285">A call to `StateHasChanged` isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="cb82e-286">`StateHasChanged`會自動呼叫來 rerender `ParentComponent`, 就像子事件會觸發元件 rerendering 在子系內執行的事件處理常式一樣。</span><span class="sxs-lookup"><span data-stu-id="cb82e-286">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="cb82e-287">`EventCallback`和`EventCallback<T>`允許非同步委派。</span><span class="sxs-lookup"><span data-stu-id="cb82e-287">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="cb82e-288">`EventCallback<T>`是強型別, 而且需要特定的引數類型。</span><span class="sxs-lookup"><span data-stu-id="cb82e-288">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="cb82e-289">`EventCallback`是弱型別, 並允許任何引數類型。</span><span class="sxs-lookup"><span data-stu-id="cb82e-289">`EventCallback` is weakly typed and allows any argument type.</span></span>

```cshtml
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="cb82e-290">使用叫用`EventCallback<T>` <xref:System.Threading.Tasks.Task>或,並等待: `EventCallback` `InvokeAsync`</span><span class="sxs-lookup"><span data-stu-id="cb82e-290">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="cb82e-291">針對`EventCallback`事件`EventCallback<T>`處理和系結元件參數使用和。</span><span class="sxs-lookup"><span data-stu-id="cb82e-291">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="cb82e-292">慣用強`EventCallback<T>` `EventCallback`型別。</span><span class="sxs-lookup"><span data-stu-id="cb82e-292">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="cb82e-293">`EventCallback<T>`為元件的使用者提供更好的錯誤意見反應。</span><span class="sxs-lookup"><span data-stu-id="cb82e-293">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="cb82e-294">與其他 UI 事件處理常式類似, 指定事件參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="cb82e-294">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="cb82e-295">當`EventCallback`沒有任何值傳遞給回呼時, 請使用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-295">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="capture-references-to-components"></a><span data-ttu-id="cb82e-296">捕獲元件的參考</span><span class="sxs-lookup"><span data-stu-id="cb82e-296">Capture references to components</span></span>

<span data-ttu-id="cb82e-297">元件參考提供參考元件實例的方法, 讓您可以對該實例發出命令, 例如`Show`或。 `Reset`</span><span class="sxs-lookup"><span data-stu-id="cb82e-297">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="cb82e-298">若要捕捉元件參考:</span><span class="sxs-lookup"><span data-stu-id="cb82e-298">To capture a component reference:</span></span>

* <span data-ttu-id="cb82e-299">[@ref](xref:mvc/views/razor#ref)將屬性加入至子元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-299">Add an [@ref](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="cb82e-300">定義與子元件類型相同的欄位。</span><span class="sxs-lookup"><span data-stu-id="cb82e-300">Define a field with the same type as the child component.</span></span>
* <span data-ttu-id="cb82e-301">`@ref:suppressField`提供參數, 以隱藏支援欄位產生。</span><span class="sxs-lookup"><span data-stu-id="cb82e-301">Provide the `@ref:suppressField` parameter, which suppresses backing field generation.</span></span> <span data-ttu-id="cb82e-302">如需詳細資訊, 請參閱[3.0.0-preview9 中@ref的移除自動支援欄位支援](https://github.com/aspnet/Announcements/issues/381)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-302">For more information, see [Removing automatic backing field support for @ref in 3.0.0-preview9](https://github.com/aspnet/Announcements/issues/381).</span></span>

```cshtml
<MyLoginDialog @ref="loginDialog" @ref:suppressField ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="cb82e-303">當元件呈現時, `loginDialog`欄位會填入`MyLoginDialog`子元件實例。</span><span class="sxs-lookup"><span data-stu-id="cb82e-303">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="cb82e-304">接著, 您可以在元件實例上叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="cb82e-304">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb82e-305">只有在轉譯元件之後才會填入`MyLoginDialog` 變數,而且其輸出會包含元素。`loginDialog`</span><span class="sxs-lookup"><span data-stu-id="cb82e-305">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="cb82e-306">直到該點為止, 沒有任何可參考的內容。</span><span class="sxs-lookup"><span data-stu-id="cb82e-306">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="cb82e-307">若要在元件完成呈現之後操作元件參考, 請使用`OnAfterRenderAsync`或`OnAfterRender`方法。</span><span class="sxs-lookup"><span data-stu-id="cb82e-307">To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.</span></span>

<!-- HOLD https://github.com/aspnet/AspNetCore.Docs/pull/13818
Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.

The Razor compiler automatically generates a backing field for element and component references when using [@ref](xref:mvc/views/razor#ref). In the following example, there's no need to create a `myLoginDialog` field for the `LoginDialog` component:

```cshtml
<LoginDialog @ref="myLoginDialog" ... />

@code {
    private void OnSomething()
    {
        myLoginDialog.Show();
    }
}
```

When the component is rendered, the generated `myLoginDialog` field is populated with the `LoginDialog` component instance. You can then invoke .NET methods on the component instance.

In some cases, a backing field is required. For example, declare a backing field when referencing generic components. To suppress backing field generation, specify the `@ref:suppressField` parameter.

> [!IMPORTANT]
> The generated `myLoginDialog` variable is only populated after the component is rendered and its output includes the `LoginDialog` element. Until that point, there's nothing to reference. To manipulate components references after the component has finished rendering, use the `OnAfterRenderAsync` or `OnAfterRender` methods.
-->

<span data-ttu-id="cb82e-308">雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/javascript-interop#capture-references-to-elements), 但它並不是[JavaScript interop](xref:blazor/javascript-interop)功能。</span><span class="sxs-lookup"><span data-stu-id="cb82e-308">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="cb82e-309">元件參考不會傳遞至 JavaScript&mdash;程式碼, 而只會在 .net 程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-309">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="cb82e-310">請勿使用元件參考來改變子元件的狀態。</span><span class="sxs-lookup"><span data-stu-id="cb82e-310">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="cb82e-311">請改用一般宣告式參數, 將資料傳遞至子元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-311">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="cb82e-312">使用一般宣告式參數會導致子元件自動 rerender 正確的時間。</span><span class="sxs-lookup"><span data-stu-id="cb82e-312">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="cb82e-313">使用\@金鑰來控制元素和元件的保留</span><span class="sxs-lookup"><span data-stu-id="cb82e-313">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="cb82e-314">當轉譯專案或元件的清單, 以及後續變更的專案或元件時, Blazor 的比較演算法必須決定哪些先前的專案或元件可以保留, 以及模型物件應如何對應至這些專案。</span><span class="sxs-lookup"><span data-stu-id="cb82e-314">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="cb82e-315">一般來說, 此程式是自動的, 可以忽略, 但在某些情況下, 您可能會想要控制進程。</span><span class="sxs-lookup"><span data-stu-id="cb82e-315">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="cb82e-316">參考下列範例：</span><span class="sxs-lookup"><span data-stu-id="cb82e-316">Consider the following example:</span></span>

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

<span data-ttu-id="cb82e-317">`People`集合的內容可能會隨著插入、刪除或重新排序的專案而變更。</span><span class="sxs-lookup"><span data-stu-id="cb82e-317">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="cb82e-318">當元件 rerenders 時, `<DetailsEditor>`元件可能會變更以接收不同`Details`的參數值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-318">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="cb82e-319">這可能會導致比預期更複雜的 rerendering。</span><span class="sxs-lookup"><span data-stu-id="cb82e-319">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="cb82e-320">在某些情況下, rerendering 可能會導致可見的行為差異, 例如失去元素的焦點。</span><span class="sxs-lookup"><span data-stu-id="cb82e-320">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="cb82e-321">您可以使用`@key`指示詞屬性來控制對應進程。</span><span class="sxs-lookup"><span data-stu-id="cb82e-321">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="cb82e-322">`@key`導致比較演算法根據索引鍵的值, 保證保留元素或元件:</span><span class="sxs-lookup"><span data-stu-id="cb82e-322">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="@person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="cb82e-323">當集合變更時, 比較演算法會保留實例和`person`實例`<DetailsEditor>`之間的關聯: `People`</span><span class="sxs-lookup"><span data-stu-id="cb82e-323">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="cb82e-324">如果從清單中刪除, 則只會從 UI `<DetailsEditor>`移除對應的實例。 `People` `Person`</span><span class="sxs-lookup"><span data-stu-id="cb82e-324">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="cb82e-325">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="cb82e-325">Other instances are left unchanged.</span></span>
* <span data-ttu-id="cb82e-326">如果在清單中的某個位置插入, 則會在對應`<DetailsEditor>`的位置插入一個新的實例。 `Person`</span><span class="sxs-lookup"><span data-stu-id="cb82e-326">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="cb82e-327">其他實例則保持不變。</span><span class="sxs-lookup"><span data-stu-id="cb82e-327">Other instances are left unchanged.</span></span>
* <span data-ttu-id="cb82e-328">如果`Person`重新排序專案, 則會保留對應`<DetailsEditor>`的實例, 並在 UI 中重新排序。</span><span class="sxs-lookup"><span data-stu-id="cb82e-328">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="cb82e-329">在某些情況下, 使用`@key`可將 rerendering 的複雜性降到最低, 並避免 DOM 的具狀態部分可能發生的問題, 例如焦點位置。</span><span class="sxs-lookup"><span data-stu-id="cb82e-329">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cb82e-330">索引鍵在每個容器元素或元件的本機。</span><span class="sxs-lookup"><span data-stu-id="cb82e-330">Keys are local to each container element or component.</span></span> <span data-ttu-id="cb82e-331">金鑰不會在檔之間進行全域比較。</span><span class="sxs-lookup"><span data-stu-id="cb82e-331">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="cb82e-332">使用\@金鑰的時機</span><span class="sxs-lookup"><span data-stu-id="cb82e-332">When to use \@key</span></span>

<span data-ttu-id="cb82e-333">一般來說, 每當轉譯清單 (例如`@key` , `@foreach`在區塊中), 而且有適合的值來定義時, 就有合理的`@key`使用方式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-333">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="cb82e-334">當物件變更時`@key` , 您也可以使用來防止 Blazor 保留元素或元件子樹:</span><span class="sxs-lookup"><span data-stu-id="cb82e-334">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```cshtml
<div @key="@currentPerson">
    ... content that depends on @currentPerson ...
</div>
```

<span data-ttu-id="cb82e-335">如果`@currentPerson`變更`<div>` , attribute 指示詞會強制 Blazor 捨棄整個及其下階, 並使用新的元素和元件重建 UI 中的子樹。 `@key`</span><span class="sxs-lookup"><span data-stu-id="cb82e-335">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="cb82e-336">如果您需要保證變更時`@currentPerson`不會保留任何 UI 狀態, 這會很有用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-336">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="cb82e-337">不使用\@金鑰的時機</span><span class="sxs-lookup"><span data-stu-id="cb82e-337">When not to use \@key</span></span>

<span data-ttu-id="cb82e-338">與`@key`比較時, 會產生效能成本。</span><span class="sxs-lookup"><span data-stu-id="cb82e-338">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="cb82e-339">效能成本並不大, 但只會`@key`指定控制元素或元件保留規則是否能讓應用程式受益。</span><span class="sxs-lookup"><span data-stu-id="cb82e-339">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="cb82e-340">`@key`即使未使用, Blazor 也會盡可能保留子項目和元件實例。</span><span class="sxs-lookup"><span data-stu-id="cb82e-340">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="cb82e-341">使用的唯一優點是`@key`控制模型實例*如何*對應至保留的元件實例, 而不是用來選取對應的比較演算法。</span><span class="sxs-lookup"><span data-stu-id="cb82e-341">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="cb82e-342">要用於金鑰的\@值</span><span class="sxs-lookup"><span data-stu-id="cb82e-342">What values to use for \@key</span></span>

<span data-ttu-id="cb82e-343">一般來說, 提供下列其中一種類型的值是合理的`@key`:</span><span class="sxs-lookup"><span data-stu-id="cb82e-343">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="cb82e-344">模型物件實例 (例如, 如先前`Person`範例所示的實例)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-344">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="cb82e-345">這可確保根據物件參考的相等性進行保留。</span><span class="sxs-lookup"><span data-stu-id="cb82e-345">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="cb82e-346">唯一識別碼 (例如,、或`int` `Guid`類型`string`的主要索引鍵值)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-346">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="cb82e-347">請確定用於`@key`的值不會造成衝突。</span><span class="sxs-lookup"><span data-stu-id="cb82e-347">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="cb82e-348">如果在相同的父元素中偵測到衝突值, Blazor 會擲回例外狀況, 因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-348">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="cb82e-349">只使用不同的值, 例如物件實例或主鍵值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-349">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="cb82e-350">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="cb82e-350">Lifecycle methods</span></span>

<span data-ttu-id="cb82e-351">`OnInitializedAsync`並`OnInitialized`執行程式碼, 以初始化元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-351">`OnInitializedAsync` and `OnInitialized` execute code to initialize the component.</span></span> <span data-ttu-id="cb82e-352">若要執行非同步作業, 請`OnInitializedAsync`在作業`await`上使用和關鍵字:</span><span class="sxs-lookup"><span data-stu-id="cb82e-352">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

<span data-ttu-id="cb82e-353">如需同步操作, 請`OnInitialized`使用:</span><span class="sxs-lookup"><span data-stu-id="cb82e-353">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="cb82e-354">`OnParametersSetAsync`當`OnParametersSet`元件已從其父系接收參數, 並將值指派給屬性時, 會呼叫和。</span><span class="sxs-lookup"><span data-stu-id="cb82e-354">`OnParametersSetAsync` and `OnParametersSet` are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="cb82e-355">這些方法會在元件初始化之後和每次呈現元件時執行:</span><span class="sxs-lookup"><span data-stu-id="cb82e-355">These methods are executed after component initialization and each time the component is rendered:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="cb82e-356">`OnAfterRenderAsync`在`OnAfterRender`元件完成呈現之後, 會呼叫和。</span><span class="sxs-lookup"><span data-stu-id="cb82e-356">`OnAfterRenderAsync` and `OnAfterRender` are called after a component has finished rendering.</span></span> <span data-ttu-id="cb82e-357">此時會填入元素和元件參考。</span><span class="sxs-lookup"><span data-stu-id="cb82e-357">Element and component references are populated at this point.</span></span> <span data-ttu-id="cb82e-358">使用此階段來執行使用轉譯內容的其他初始化步驟, 例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="cb82e-358">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

### <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="cb82e-359">處理轉譯時的未完成非同步動作</span><span class="sxs-lookup"><span data-stu-id="cb82e-359">Handle incomplete async actions at render</span></span>

<span data-ttu-id="cb82e-360">在呈現元件之前, 在生命週期事件中執行的非同步動作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="cb82e-360">Asynchronous actions performed in lifecycle events may not have completed before the component is rendered.</span></span> <span data-ttu-id="cb82e-361">當生命週期`null`方法正在執行時, 物件可能會或未完全填入資料。</span><span class="sxs-lookup"><span data-stu-id="cb82e-361">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="cb82e-362">提供轉譯邏輯, 以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="cb82e-362">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="cb82e-363">當物件為`null`時, 呈現預留位置 UI 專案 (例如, 載入訊息)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-363">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="cb82e-364">在 Blazor 範本的`OnInitializedAsync` `forecasts`元件中, 會覆寫為 asychronously 接收預測資料 ()。 `FetchData`</span><span class="sxs-lookup"><span data-stu-id="cb82e-364">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="cb82e-365">當`forecasts` 為`null`時, 會向使用者顯示載入訊息。</span><span class="sxs-lookup"><span data-stu-id="cb82e-365">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="cb82e-366">在所`Task` `OnInitializedAsync`傳回的完成之後, 元件會以更新的狀態重新顯示。</span><span class="sxs-lookup"><span data-stu-id="cb82e-366">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="cb82e-367">*Pages/FetchData.razor*：</span><span class="sxs-lookup"><span data-stu-id="cb82e-367">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](components/samples_snapshot/3.x/FetchData.razor?highlight=9)]

### <a name="execute-code-before-parameters-are-set"></a><span data-ttu-id="cb82e-368">在設定參數之前執行程式碼</span><span class="sxs-lookup"><span data-stu-id="cb82e-368">Execute code before parameters are set</span></span>

<span data-ttu-id="cb82e-369">`SetParameters`在設定參數之前, 可以覆寫以執行程式碼:</span><span class="sxs-lookup"><span data-stu-id="cb82e-369">`SetParameters` can be overridden to execute code before parameters are set:</span></span>

```csharp
public override void SetParameters(ParameterView parameters)
{
    ...

    base.SetParameters(parameters);
}
```

<span data-ttu-id="cb82e-370">如果`base.SetParameters`未叫用, 自訂程式碼就可以任何需要的方式解讀傳入的參數值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-370">If `base.SetParameters` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="cb82e-371">例如, 傳入的參數不需要指派給類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-371">For example, the incoming parameters aren't required to be assigned to the properties on the class.</span></span>

### <a name="suppress-refreshing-of-the-ui"></a><span data-ttu-id="cb82e-372">隱藏 UI 的重新整理</span><span class="sxs-lookup"><span data-stu-id="cb82e-372">Suppress refreshing of the UI</span></span>

<span data-ttu-id="cb82e-373">`ShouldRender`可以覆寫以隱藏 UI 的重新整理。</span><span class="sxs-lookup"><span data-stu-id="cb82e-373">`ShouldRender` can be overridden to suppress refreshing of the UI.</span></span> <span data-ttu-id="cb82e-374">如果執行`true`傳回, 則會重新整理 UI。</span><span class="sxs-lookup"><span data-stu-id="cb82e-374">If the implementation returns `true`, the UI is refreshed.</span></span> <span data-ttu-id="cb82e-375">`ShouldRender`即使已覆寫, 元件一律會一開始呈現。</span><span class="sxs-lookup"><span data-stu-id="cb82e-375">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="cb82e-376">使用 IDisposable 的元件處置</span><span class="sxs-lookup"><span data-stu-id="cb82e-376">Component disposal with IDisposable</span></span>

<span data-ttu-id="cb82e-377">如果元件<xref:System.IDisposable>會執行, 則會在從 UI 中移除元件時呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-377">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="cb82e-378">下列元件會使用`@implements IDisposable` `Dispose`和方法:</span><span class="sxs-lookup"><span data-stu-id="cb82e-378">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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

## <a name="routing"></a><span data-ttu-id="cb82e-379">路由</span><span class="sxs-lookup"><span data-stu-id="cb82e-379">Routing</span></span>

<span data-ttu-id="cb82e-380">Blazor 中的路由是藉由將路由範本提供給應用程式中每個可存取的元件來達成。</span><span class="sxs-lookup"><span data-stu-id="cb82e-380">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="cb82e-381">編譯含有`@page`指示詞的 Razor 檔案時, 系統會<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本給產生的類別。</span><span class="sxs-lookup"><span data-stu-id="cb82e-381">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="cb82e-382">在執行時間, 路由器會尋找具有的`RouteAttribute`元件類別, 並轉譯哪個元件具有符合所要求 URL 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="cb82e-382">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="cb82e-383">多個路由範本可以套用至元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-383">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="cb82e-384">下列元件會回應和`/BlazorRoute` `/DifferentBlazorRoute`的要求:</span><span class="sxs-lookup"><span data-stu-id="cb82e-384">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a><span data-ttu-id="cb82e-385">路由參數</span><span class="sxs-lookup"><span data-stu-id="cb82e-385">Route parameters</span></span>

<span data-ttu-id="cb82e-386">元件可以從指示詞中`@page`提供的路由範本接收路由參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-386">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="cb82e-387">路由器會使用路由參數來填入對應的元件參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-387">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="cb82e-388">*路由參數元件*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-388">*Route Parameter component*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter)]

<span data-ttu-id="cb82e-389">不支援選擇性參數, 因此上述`@page`範例中會套用兩個指示詞。</span><span class="sxs-lookup"><span data-stu-id="cb82e-389">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="cb82e-390">第一個則允許不使用參數導覽至元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-390">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="cb82e-391">第二`@page`個指示詞`{text}`會採用 route 參數, 並將值`Text`指派給屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-391">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="base-class-inheritance-for-a-code-behind-experience"></a><span data-ttu-id="cb82e-392">「程式碼後置」體驗的基類繼承</span><span class="sxs-lookup"><span data-stu-id="cb82e-392">Base class inheritance for a "code-behind" experience</span></span>

<span data-ttu-id="cb82e-393">元件檔案會將 HTML 標籤C#和處理常式代碼混合在同一個檔案中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-393">Component files mix HTML markup and C# processing code in the same file.</span></span> <span data-ttu-id="cb82e-394">`@inherits`指示詞可用於提供具有「程式碼後置」體驗的 Blazor apps, 以分隔元件標記與處理常式代碼。</span><span class="sxs-lookup"><span data-stu-id="cb82e-394">The `@inherits` directive can be used to provide Blazor apps with a "code-behind" experience that separates component markup from processing code.</span></span>

<span data-ttu-id="cb82e-395">[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)會顯示元件如何繼承基類, `BlazorRocksBase`以提供元件的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="cb82e-395">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="cb82e-396">*Pages/BlazorRocks. razor*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-396">*Pages/BlazorRocks.razor*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.razor?name=snippet_BlazorRocks)]

<span data-ttu-id="cb82e-397">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-397">*BlazorRocksBase.cs*:</span></span>

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

<span data-ttu-id="cb82e-398">基類應該衍生自`ComponentBase`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-398">The base class should derive from `ComponentBase`.</span></span>

## <a name="import-components"></a><span data-ttu-id="cb82e-399">匯入元件</span><span class="sxs-lookup"><span data-stu-id="cb82e-399">Import components</span></span>

<span data-ttu-id="cb82e-400">以 Razor 撰寫之元件的命名空間是以為基礎:</span><span class="sxs-lookup"><span data-stu-id="cb82e-400">The namespace of a component authored with Razor is based on:</span></span>

* <span data-ttu-id="cb82e-401">專案的`RootNamespace`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-401">The project's `RootNamespace`.</span></span>
* <span data-ttu-id="cb82e-402">從專案根到元件的路徑。</span><span class="sxs-lookup"><span data-stu-id="cb82e-402">The path from the project root to the component.</span></span> <span data-ttu-id="cb82e-403">例如, `ComponentsSample/Pages/Index.razor`位於命名空間`ComponentsSample.Pages`中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-403">For example, `ComponentsSample/Pages/Index.razor` is in the namespace `ComponentsSample.Pages`.</span></span> <span data-ttu-id="cb82e-404">元件會C#遵循名稱系結規則。</span><span class="sxs-lookup"><span data-stu-id="cb82e-404">Components follow C# name binding rules.</span></span> <span data-ttu-id="cb82e-405">在*ComponentsSample*的情況下, 相同資料夾、*分頁*和父資料夾中的所有元件都在範圍內。</span><span class="sxs-lookup"><span data-stu-id="cb82e-405">In the case of *Index.razor*, all components in the same folder, *Pages*, and the parent folder, *ComponentsSample*, are in scope.</span></span>

<span data-ttu-id="cb82e-406">您可以使用 Razor 的[ \@using](xref:mvc/views/razor#using)指示詞, 將不同命名空間中定義的元件帶入範圍中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-406">Components defined in a different namespace can be brought into scope using Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="cb82e-407">`NavMenu.razor`如果資料夾`ComponentsSample/Shared/`中有另一個元件, 則可以在中`Index.razor`使用此元件, 並搭配下列`@using`語句:</span><span class="sxs-lookup"><span data-stu-id="cb82e-407">If another component, `NavMenu.razor`, exists in the folder `ComponentsSample/Shared/`, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```cshtml
@using ComponentsSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="cb82e-408">元件也可以使用其完整名稱來參考, 這樣就不再需要[ \@using](xref:mvc/views/razor#using)指示詞:</span><span class="sxs-lookup"><span data-stu-id="cb82e-408">Components can also be referenced using their fully qualified names, which removes the need for the [\@using](xref:mvc/views/razor#using) directive:</span></span>

```cshtml
This is the Index page.

<ComponentsSample.Shared.NavMenu></ComponentsSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="cb82e-409">不`global::`支援該限定性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-409">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="cb82e-410">不支援使用具有`using`別名的語句來`@using Foo = Bar`匯入元件 (例如)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-410">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="cb82e-411">不支援部分限定的名稱。</span><span class="sxs-lookup"><span data-stu-id="cb82e-411">Partially qualified names aren't supported.</span></span> <span data-ttu-id="cb82e-412">例如, `<Shared.NavMenu></Shared.NavMenu>`不支援`@using ComponentsSample`新增和`NavMenu.razor`參考。</span><span class="sxs-lookup"><span data-stu-id="cb82e-412">For example, adding `@using ComponentsSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="cb82e-413">條件式 HTML 元素屬性</span><span class="sxs-lookup"><span data-stu-id="cb82e-413">Conditional HTML element attributes</span></span>

<span data-ttu-id="cb82e-414">HTML 專案屬性會根據 .NET 值有條件地呈現。</span><span class="sxs-lookup"><span data-stu-id="cb82e-414">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="cb82e-415">如果值為`false`或`null`, 則不會呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-415">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="cb82e-416">如果值為`true`, 則會以最小化的方式呈現屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-416">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="cb82e-417">在下列範例中, `IsCompleted` `checked`會判斷是否呈現在專案的標記中:</span><span class="sxs-lookup"><span data-stu-id="cb82e-417">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```cshtml
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="cb82e-418">如果`IsCompleted` 為`true`, 則會將核取方塊轉譯為:</span><span class="sxs-lookup"><span data-stu-id="cb82e-418">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="cb82e-419">如果`IsCompleted` 為`false`, 則會將核取方塊轉譯為:</span><span class="sxs-lookup"><span data-stu-id="cb82e-419">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="cb82e-420">如需詳細資訊，請參閱 <xref:mvc/views/razor>。</span><span class="sxs-lookup"><span data-stu-id="cb82e-420">For more information, see <xref:mvc/views/razor>.</span></span>

## <a name="raw-html"></a><span data-ttu-id="cb82e-421">原始 HTML</span><span class="sxs-lookup"><span data-stu-id="cb82e-421">Raw HTML</span></span>

<span data-ttu-id="cb82e-422">字串通常會使用 DOM 文位元組點來呈現, 這表示它們可能包含的任何標記都會被忽略, 並視為常值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-422">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="cb82e-423">若要轉譯原始 HTML, 請將 HTML 內容包裝`MarkupString`在值中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-423">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="cb82e-424">此值會剖析為 HTML 或 SVG, 並插入 DOM 中。</span><span class="sxs-lookup"><span data-stu-id="cb82e-424">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="cb82e-425">轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**, 應予以避免!</span><span class="sxs-lookup"><span data-stu-id="cb82e-425">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="cb82e-426">下列範例顯示如何使用`MarkupString`類型, 將靜態 HTML 內容的區塊新增至元件的轉譯輸出:</span><span class="sxs-lookup"><span data-stu-id="cb82e-426">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="cb82e-427">樣板化元件</span><span class="sxs-lookup"><span data-stu-id="cb82e-427">Templated components</span></span>

<span data-ttu-id="cb82e-428">樣板化元件是接受一或多個 UI 範本做為參數的元件, 然後可以用來做為元件轉譯邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="cb82e-428">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="cb82e-429">樣板化元件可讓您撰寫比一般元件更容易重複使用的較高層級元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-429">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="cb82e-430">其中有幾個範例包括:</span><span class="sxs-lookup"><span data-stu-id="cb82e-430">A couple of examples include:</span></span>

* <span data-ttu-id="cb82e-431">資料表元件, 可讓使用者指定資料表標頭、資料列和頁尾的範本。</span><span class="sxs-lookup"><span data-stu-id="cb82e-431">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="cb82e-432">清單元件, 可讓使用者指定範本來轉譯清單中的專案。</span><span class="sxs-lookup"><span data-stu-id="cb82e-432">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="cb82e-433">範本參數</span><span class="sxs-lookup"><span data-stu-id="cb82e-433">Template parameters</span></span>

<span data-ttu-id="cb82e-434">樣板化元件是藉由指定一或多個類型`RenderFragment`為或`RenderFragment<T>`的元件參數所定義。</span><span class="sxs-lookup"><span data-stu-id="cb82e-434">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="cb82e-435">呈現片段代表要呈現的 UI 區段。</span><span class="sxs-lookup"><span data-stu-id="cb82e-435">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="cb82e-436">`RenderFragment<T>`採用可在叫用轉譯片段時指定的類型參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-436">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="cb82e-437">`TableTemplate`成分</span><span class="sxs-lookup"><span data-stu-id="cb82e-437">`TableTemplate` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.razor)]

<span data-ttu-id="cb82e-438">使用樣板化元件時, 可以使用符合參數名稱的子項目來指定範本參數 (`TableHeader` `RowTemplate`在下列範例中為):</span><span class="sxs-lookup"><span data-stu-id="cb82e-438">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a><span data-ttu-id="cb82e-439">範本內容參數</span><span class="sxs-lookup"><span data-stu-id="cb82e-439">Template context parameters</span></span>

<span data-ttu-id="cb82e-440">當做元素傳遞之`RenderFragment<T>`類型的元件引數具有名為`context`的隱含參數 (例如, `@context.PetId`從上述程式碼範例中), 但您可以使用子系`Context`上的屬性來變更參數名稱。元素.</span><span class="sxs-lookup"><span data-stu-id="cb82e-440">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="cb82e-441">在下列範例中, `RowTemplate`元素的`Context`屬性會指定`pet`參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-441">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```cshtml
<TableTemplate Items="@pets">
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

<span data-ttu-id="cb82e-442">或者, 您也可以在`Context` component 元素上指定屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-442">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="cb82e-443">指定`Context`的屬性會套用至所有指定的範本參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-443">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="cb82e-444">當您想要指定隱含子內容的內容參數名稱時 (不含任何換行的子項目), 這會很有用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-444">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="cb82e-445">在下列範例中, `Context`屬性會出現`TableTemplate`在元素上, 並套用至所有範本參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-445">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

### <a name="generic-typed-components"></a><span data-ttu-id="cb82e-446">泛型型別元件</span><span class="sxs-lookup"><span data-stu-id="cb82e-446">Generic-typed components</span></span>

<span data-ttu-id="cb82e-447">樣板化元件通常會以一般方式輸入。</span><span class="sxs-lookup"><span data-stu-id="cb82e-447">Templated components are often generically typed.</span></span> <span data-ttu-id="cb82e-448">例如, 泛型`ListViewTemplate`元件可以用來呈現`IEnumerable<T>`值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-448">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="cb82e-449">若要定義泛型元件, 請使用`@typeparam`指示詞來指定類型參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-449">To define a generic component, use the `@typeparam` directive to specify type parameters:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.razor)]

<span data-ttu-id="cb82e-450">使用泛型型別元件時, 會在可能的情況下推斷型別參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-450">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="cb82e-451">否則, 必須使用符合型別參數名稱的屬性來明確指定型別參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-451">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="cb82e-452">在下列範例中, `TItem="Pet"`會指定類型:</span><span class="sxs-lookup"><span data-stu-id="cb82e-452">In the following example, `TItem="Pet"` specifies the type:</span></span>

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="cb82e-453">級聯的值和參數</span><span class="sxs-lookup"><span data-stu-id="cb82e-453">Cascading values and parameters</span></span>

<span data-ttu-id="cb82e-454">在某些情況下, 使用[元件參數](#component-parameters)將資料從上階元件傳送到子元件是不方便的, 特別是在有數個元件層時。</span><span class="sxs-lookup"><span data-stu-id="cb82e-454">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="cb82e-455">串聯的值和參數可讓上階元件提供一個值給其所有子系元件, 藉此解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="cb82e-455">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="cb82e-456">級聯的值和參數也會提供一種方法來協調元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-456">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="cb82e-457">主題範例</span><span class="sxs-lookup"><span data-stu-id="cb82e-457">Theme example</span></span>

<span data-ttu-id="cb82e-458">在範例應用程式的下列範例中, `ThemeInfo`類別會指定主題資訊以向下流動元件階層, 讓應用程式中指定部分內的所有按鈕共用相同的樣式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-458">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="cb82e-459">*UIThemeClasses/ThemeInfo .cs*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-459">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="cb82e-460">祖系元件可以使用串聯值元件來提供串聯值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-460">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="cb82e-461">此`CascadingValue`元件會包裝元件階層的子樹, 並提供單一值給該子樹內的所有元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-461">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="cb82e-462">例如, 範例應用程式會在其中一個應用`ThemeInfo`程式的配置中, 將主題資訊 () 指定為構成`@Body`屬性版面配置主體之所有元件的串聯參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-462">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="cb82e-463">`ButtonClass`在版面配置元件中`btn-success` , 會指派的值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-463">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="cb82e-464">任何子代元件都可以透過`ThemeInfo`串聯物件使用此屬性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-464">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="cb82e-465">`CascadingValuesParametersLayout`成分</span><span class="sxs-lookup"><span data-stu-id="cb82e-465">`CascadingValuesParametersLayout` component:</span></span>

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
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

<span data-ttu-id="cb82e-466">若要利用串聯值, 元件會使用`[CascadingParameter]`屬性或根據字串名稱值來宣告串聯參數:</span><span class="sxs-lookup"><span data-stu-id="cb82e-466">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute or based on a string name value:</span></span>

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

<span data-ttu-id="cb82e-467">如果您有多個相同類型的串聯值, 而且需要在相同的子樹中區別它們, 則與字串名稱值的系結是相關的。</span><span class="sxs-lookup"><span data-stu-id="cb82e-467">Binding with a string name value is relevant if you have multiple cascading values of the same type and need to differentiate them within the same subtree.</span></span>

<span data-ttu-id="cb82e-468">串聯式值會依類型系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-468">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="cb82e-469">在範例應用程式中, `CascadingValuesParametersTheme`元件`ThemeInfo`會將串聯值系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-469">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="cb82e-470">參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="cb82e-470">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="cb82e-471">`CascadingValuesParametersTheme`成分</span><span class="sxs-lookup"><span data-stu-id="cb82e-471">`CascadingValuesParametersTheme` component:</span></span>

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

### <a name="tabset-example"></a><span data-ttu-id="cb82e-472">TabSet 範例</span><span class="sxs-lookup"><span data-stu-id="cb82e-472">TabSet example</span></span>

<span data-ttu-id="cb82e-473">串聯式參數也可以讓元件在元件階層之間共同作業。</span><span class="sxs-lookup"><span data-stu-id="cb82e-473">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="cb82e-474">例如, 請考慮範例應用程式中的下列*TabSet*範例。</span><span class="sxs-lookup"><span data-stu-id="cb82e-474">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="cb82e-475">範例應用程式具有`ITab`可執行 tab 鍵的介面:</span><span class="sxs-lookup"><span data-stu-id="cb82e-475">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

<span data-ttu-id="cb82e-476">元件會使用元件, 其中包含數個`Tab`元件: `TabSet` `CascadingValuesParametersTabSet`</span><span class="sxs-lookup"><span data-stu-id="cb82e-476">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.razor?name=snippet_TabSet)]

<span data-ttu-id="cb82e-477">子`Tab`元件不會明確地當做參數傳遞`TabSet`至。</span><span class="sxs-lookup"><span data-stu-id="cb82e-477">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="cb82e-478">相反地, 子`Tab`元件是的子內容`TabSet`的一部分。</span><span class="sxs-lookup"><span data-stu-id="cb82e-478">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="cb82e-479">不過, `TabSet`仍然需要知道每個`Tab`元件, 使其可以呈現標頭和使用中的索引標籤。若要啟用這項協調而不需要額外`TabSet`的程式碼, 元件*可以提供本身作為*串聯的值, 然後由子代`Tab`元件挑選。</span><span class="sxs-lookup"><span data-stu-id="cb82e-479">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="cb82e-480">`TabSet`成分</span><span class="sxs-lookup"><span data-stu-id="cb82e-480">`TabSet` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.razor)]

<span data-ttu-id="cb82e-481">子系`TabSet` `Tab` `TabSet`元件會以串聯式參數的形式捕捉包含的, 因此元件會將自己加入至索引標籤作用中的和座標。 `Tab`</span><span class="sxs-lookup"><span data-stu-id="cb82e-481">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="cb82e-482">`Tab`成分</span><span class="sxs-lookup"><span data-stu-id="cb82e-482">`Tab` component:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="cb82e-483">Razor 範本</span><span class="sxs-lookup"><span data-stu-id="cb82e-483">Razor templates</span></span>

<span data-ttu-id="cb82e-484">轉譯片段可以使用 Razor 範本語法來定義。</span><span class="sxs-lookup"><span data-stu-id="cb82e-484">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="cb82e-485">Razor 範本是定義 UI 程式碼片段並採用下列格式的方式:</span><span class="sxs-lookup"><span data-stu-id="cb82e-485">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```cshtml
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="cb82e-486">下列範例說明如何指定`RenderFragment`和`RenderFragment<T>`值, 並直接在元件中呈現範本。</span><span class="sxs-lookup"><span data-stu-id="cb82e-486">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="cb82e-487">轉譯片段也可以當做引數傳遞至樣板[化元件](#templated-components)。</span><span class="sxs-lookup"><span data-stu-id="cb82e-487">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

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

<span data-ttu-id="cb82e-488">上述程式碼的轉譯輸出:</span><span class="sxs-lookup"><span data-stu-id="cb82e-488">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="cb82e-489">手動 RenderTreeBuilder 邏輯</span><span class="sxs-lookup"><span data-stu-id="cb82e-489">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="cb82e-490">`Microsoft.AspNetCore.Components.RenderTree`提供操作元件和元素的方法, 包括在程式碼中C#手動建立元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-490">`Microsoft.AspNetCore.Components.RenderTree` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="cb82e-491">使用來`RenderTreeBuilder`建立元件是一個先進的案例。</span><span class="sxs-lookup"><span data-stu-id="cb82e-491">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="cb82e-492">格式不正確的元件 (例如, 未封閉的標記標記) 可能會導致未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="cb82e-492">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="cb82e-493">請考慮下列`PetDetails`元件, 它可以手動內建在另一個元件中:</span><span class="sxs-lookup"><span data-stu-id="cb82e-493">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="cb82e-494">在下列範例中, `CreateComponent`方法中的迴圈會產生三個`PetDetails`元件。</span><span class="sxs-lookup"><span data-stu-id="cb82e-494">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="cb82e-495">呼叫`RenderTreeBuilder`方法來建立元件 (`OpenComponent`和`AddAttribute`) 時, 序號是源程式碼號。</span><span class="sxs-lookup"><span data-stu-id="cb82e-495">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="cb82e-496">Blazor 差異演算法依賴對應于不同程式程式碼的序號, 而不是相異的呼叫調用。</span><span class="sxs-lookup"><span data-stu-id="cb82e-496">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="cb82e-497">使用`RenderTreeBuilder`方法建立元件時, 硬式編碼序號的引數。</span><span class="sxs-lookup"><span data-stu-id="cb82e-497">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="cb82e-498">**使用計算或計數器來產生序號可能會導致效能不佳。**</span><span class="sxs-lookup"><span data-stu-id="cb82e-498">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="cb82e-499">如需詳細資訊, 請參閱[序號與程式程式碼號, 而不是執行順序](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)一節。</span><span class="sxs-lookup"><span data-stu-id="cb82e-499">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="cb82e-500">`BuiltContent`成分</span><span class="sxs-lookup"><span data-stu-id="cb82e-500">`BuiltContent` component:</span></span>

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

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="cb82e-501">序號與程式程式碼號相關, 而不是執行順序</span><span class="sxs-lookup"><span data-stu-id="cb82e-501">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="cb82e-502">一律`.razor`會編譯 Blazor 檔案。</span><span class="sxs-lookup"><span data-stu-id="cb82e-502">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="cb82e-503">這可能是的絕佳優點`.razor` , 因為編譯步驟可以用來插入可在執行時間改善應用程式效能的資訊。</span><span class="sxs-lookup"><span data-stu-id="cb82e-503">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="cb82e-504">這些改良功能的重要範例包括*序號*。</span><span class="sxs-lookup"><span data-stu-id="cb82e-504">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="cb82e-505">序號會向運行時程表示輸出來自哪些不同和已排序的程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="cb82e-505">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="cb82e-506">執行時間會使用這項資訊, 以線性時間產生有效率的樹狀差異, 這比一般樹狀結構的差異演算法通常還能快得多。</span><span class="sxs-lookup"><span data-stu-id="cb82e-506">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="cb82e-507">請考慮下列簡單`.razor`的檔案:</span><span class="sxs-lookup"><span data-stu-id="cb82e-507">Consider the following simple `.razor` file:</span></span>

```cshtml
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="cb82e-508">上述程式碼會編譯成如下所示的內容:</span><span class="sxs-lookup"><span data-stu-id="cb82e-508">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="cb82e-509">當程式碼第一次執行時, 如果`someFlag`是`true`, 則產生器會接收:</span><span class="sxs-lookup"><span data-stu-id="cb82e-509">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="cb82e-510">序列</span><span class="sxs-lookup"><span data-stu-id="cb82e-510">Sequence</span></span> | <span data-ttu-id="cb82e-511">類型</span><span class="sxs-lookup"><span data-stu-id="cb82e-511">Type</span></span>      | <span data-ttu-id="cb82e-512">資料</span><span class="sxs-lookup"><span data-stu-id="cb82e-512">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cb82e-513">0</span><span class="sxs-lookup"><span data-stu-id="cb82e-513">0</span></span>        | <span data-ttu-id="cb82e-514">Text node</span><span class="sxs-lookup"><span data-stu-id="cb82e-514">Text node</span></span> | <span data-ttu-id="cb82e-515">第一個</span><span class="sxs-lookup"><span data-stu-id="cb82e-515">First</span></span>  |
| <span data-ttu-id="cb82e-516">1</span><span class="sxs-lookup"><span data-stu-id="cb82e-516">1</span></span>        | <span data-ttu-id="cb82e-517">Text node</span><span class="sxs-lookup"><span data-stu-id="cb82e-517">Text node</span></span> | <span data-ttu-id="cb82e-518">第二個</span><span class="sxs-lookup"><span data-stu-id="cb82e-518">Second</span></span> |

<span data-ttu-id="cb82e-519">想像一下, `false`會變成, 然後再次呈現標記。 `someFlag`</span><span class="sxs-lookup"><span data-stu-id="cb82e-519">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="cb82e-520">這次, 產生器會接收:</span><span class="sxs-lookup"><span data-stu-id="cb82e-520">This time, the builder receives:</span></span>

| <span data-ttu-id="cb82e-521">序列</span><span class="sxs-lookup"><span data-stu-id="cb82e-521">Sequence</span></span> | <span data-ttu-id="cb82e-522">類型</span><span class="sxs-lookup"><span data-stu-id="cb82e-522">Type</span></span>       | <span data-ttu-id="cb82e-523">資料</span><span class="sxs-lookup"><span data-stu-id="cb82e-523">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="cb82e-524">1</span><span class="sxs-lookup"><span data-stu-id="cb82e-524">1</span></span>        | <span data-ttu-id="cb82e-525">Text node</span><span class="sxs-lookup"><span data-stu-id="cb82e-525">Text node</span></span>  | <span data-ttu-id="cb82e-526">第二個</span><span class="sxs-lookup"><span data-stu-id="cb82e-526">Second</span></span> |

<span data-ttu-id="cb82e-527">當執行時間執行 diff 時, 會看到順序`0`中的專案已移除, 因此它會產生下列簡單的*編輯腳本*:</span><span class="sxs-lookup"><span data-stu-id="cb82e-527">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="cb82e-528">移除第一個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="cb82e-528">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="cb82e-529">當您以程式設計方式產生序號時, 會發生什麼錯誤</span><span class="sxs-lookup"><span data-stu-id="cb82e-529">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="cb82e-530">想像一下, 您會改為撰寫下列轉譯樹產生器邏輯:</span><span class="sxs-lookup"><span data-stu-id="cb82e-530">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="cb82e-531">現在, 第一個輸出是:</span><span class="sxs-lookup"><span data-stu-id="cb82e-531">Now, the first output is:</span></span>

| <span data-ttu-id="cb82e-532">序列</span><span class="sxs-lookup"><span data-stu-id="cb82e-532">Sequence</span></span> | <span data-ttu-id="cb82e-533">類型</span><span class="sxs-lookup"><span data-stu-id="cb82e-533">Type</span></span>      | <span data-ttu-id="cb82e-534">資料</span><span class="sxs-lookup"><span data-stu-id="cb82e-534">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cb82e-535">0</span><span class="sxs-lookup"><span data-stu-id="cb82e-535">0</span></span>        | <span data-ttu-id="cb82e-536">Text node</span><span class="sxs-lookup"><span data-stu-id="cb82e-536">Text node</span></span> | <span data-ttu-id="cb82e-537">第一個</span><span class="sxs-lookup"><span data-stu-id="cb82e-537">First</span></span>  |
| <span data-ttu-id="cb82e-538">1</span><span class="sxs-lookup"><span data-stu-id="cb82e-538">1</span></span>        | <span data-ttu-id="cb82e-539">Text node</span><span class="sxs-lookup"><span data-stu-id="cb82e-539">Text node</span></span> | <span data-ttu-id="cb82e-540">第二個</span><span class="sxs-lookup"><span data-stu-id="cb82e-540">Second</span></span> |

<span data-ttu-id="cb82e-541">此結果與先前的案例相同, 因此不會有負面問題存在。</span><span class="sxs-lookup"><span data-stu-id="cb82e-541">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="cb82e-542">`someFlag``false`在第二個轉譯上, 輸出為:</span><span class="sxs-lookup"><span data-stu-id="cb82e-542">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="cb82e-543">序列</span><span class="sxs-lookup"><span data-stu-id="cb82e-543">Sequence</span></span> | <span data-ttu-id="cb82e-544">類型</span><span class="sxs-lookup"><span data-stu-id="cb82e-544">Type</span></span>      | <span data-ttu-id="cb82e-545">資料</span><span class="sxs-lookup"><span data-stu-id="cb82e-545">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="cb82e-546">0</span><span class="sxs-lookup"><span data-stu-id="cb82e-546">0</span></span>        | <span data-ttu-id="cb82e-547">Text node</span><span class="sxs-lookup"><span data-stu-id="cb82e-547">Text node</span></span> | <span data-ttu-id="cb82e-548">第二個</span><span class="sxs-lookup"><span data-stu-id="cb82e-548">Second</span></span> |

<span data-ttu-id="cb82e-549">這次, diff 演算法發現發生了*兩*項變更, 而演算法會產生下列編輯腳本:</span><span class="sxs-lookup"><span data-stu-id="cb82e-549">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="cb82e-550">將第一個文位元組點的值變更為`Second`。</span><span class="sxs-lookup"><span data-stu-id="cb82e-550">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="cb82e-551">移除第二個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="cb82e-551">Remove the second text node.</span></span>

<span data-ttu-id="cb82e-552">產生序號已遺失關於`if/else`分支和迴圈在原始程式碼中出現位置的所有實用資訊。</span><span class="sxs-lookup"><span data-stu-id="cb82e-552">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="cb82e-553">這會導致差異**兩倍, 但前提**是之前。</span><span class="sxs-lookup"><span data-stu-id="cb82e-553">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="cb82e-554">這是一個簡單的範例。</span><span class="sxs-lookup"><span data-stu-id="cb82e-554">This is a trivial example.</span></span> <span data-ttu-id="cb82e-555">在具有複雜和深層嵌套結構的更真實案例中, 尤其是使用迴圈時, 效能成本會更嚴重。</span><span class="sxs-lookup"><span data-stu-id="cb82e-555">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="cb82e-556">Diff 演算法不會立即識別已插入或移除的迴圈區塊或分支, 而是必須在轉譯樹狀結構中進行深入的遞迴, 而且通常會建立更長的編輯腳本, 因為它 misinformed 了新舊結構的方式。相互關聯。</span><span class="sxs-lookup"><span data-stu-id="cb82e-556">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="cb82e-557">指引和結論</span><span class="sxs-lookup"><span data-stu-id="cb82e-557">Guidance and conclusions</span></span>

* <span data-ttu-id="cb82e-558">如果序號是動態產生的, 應用程式效能會受到影響。</span><span class="sxs-lookup"><span data-stu-id="cb82e-558">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="cb82e-559">架構無法在執行時間自動建立自己的序號, 因為必要的資訊不存在, 除非是在編譯時期加以捕捉。</span><span class="sxs-lookup"><span data-stu-id="cb82e-559">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="cb82e-560">請勿撰寫長時間區塊的手動執行`RenderTreeBuilder`邏輯。</span><span class="sxs-lookup"><span data-stu-id="cb82e-560">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="cb82e-561">偏好`.razor`使用檔案, 並允許編譯器處理序號。</span><span class="sxs-lookup"><span data-stu-id="cb82e-561">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span>
* <span data-ttu-id="cb82e-562">如果序號已硬式編碼, 則 diff 演算法只會要求序號增加值。</span><span class="sxs-lookup"><span data-stu-id="cb82e-562">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="cb82e-563">起始值和間距無關。</span><span class="sxs-lookup"><span data-stu-id="cb82e-563">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="cb82e-564">一個合法的選項是使用程式程式碼號做為序號, 或從零開始, 並以一個或數百個 (或任何慣用的間隔) 來增加。</span><span class="sxs-lookup"><span data-stu-id="cb82e-564">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="cb82e-565">Blazor 會使用序號, 而其他樹狀結構比較的 UI 架構則不會使用它們。</span><span class="sxs-lookup"><span data-stu-id="cb82e-565">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="cb82e-566">使用序號時, 比較速度會更快, 而且 Blazor 具有可自動處理序號的編譯步驟, 讓開發人員撰寫`.razor`檔案。</span><span class="sxs-lookup"><span data-stu-id="cb82e-566">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="localization"></a><span data-ttu-id="cb82e-567">當地語系化</span><span class="sxs-lookup"><span data-stu-id="cb82e-567">Localization</span></span>

<span data-ttu-id="cb82e-568">Blazor 伺服器端應用程式是使用[當地語系化中介軟體](xref:fundamentals/localization#localization-middleware)來當地語系化。</span><span class="sxs-lookup"><span data-stu-id="cb82e-568">Blazor server-side apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="cb82e-569">中介軟體會為從應用程式要求資源的使用者選取適當的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-569">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="cb82e-570">您可以使用下列其中一種方法來設定文化特性:</span><span class="sxs-lookup"><span data-stu-id="cb82e-570">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="cb82e-571">Cookie</span><span class="sxs-lookup"><span data-stu-id="cb82e-571">Cookies</span></span>](#cookies)
* [<span data-ttu-id="cb82e-572">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="cb82e-572">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="cb82e-573">如需詳細資訊與範例，請參閱<xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="cb82e-573">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="cb82e-574">Cookie</span><span class="sxs-lookup"><span data-stu-id="cb82e-574">Cookies</span></span>

<span data-ttu-id="cb82e-575">當地語系化文化特性 cookie 可以保存使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-575">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="cb82e-576">Cookie 是由`OnGet`應用程式的主機頁面 (*Pages/主機. cshtml .cs*) 的方法所建立。</span><span class="sxs-lookup"><span data-stu-id="cb82e-576">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="cb82e-577">當地語系化中介軟體會在後續要求中讀取 cookie, 以設定使用者的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-577">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="cb82e-578">使用 cookie 可確保 WebSocket 連接可以正確地傳播文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-578">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="cb82e-579">如果當地語系化架構是以 URL 路徑或查詢字串為基礎, 配置可能無法使用 Websocket, 因而無法保存文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-579">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="cb82e-580">因此, 建議的方法是使用當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="cb82e-580">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="cb82e-581">如果文化特性保存在當地語系化 cookie 中, 任何技術都可以用來指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-581">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="cb82e-582">如果應用程式已建立伺服器端 ASP.NET Core 的當地語系化配置, 請繼續使用應用程式的現有當地語系化基礎結構, 並在應用程式的配置內設定當地語系化文化特性 cookie。</span><span class="sxs-lookup"><span data-stu-id="cb82e-582">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="cb82e-583">下列範例顯示如何在可由當地語系化中介軟體讀取的 cookie 中設定目前的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-583">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="cb82e-584">在 Blazor 伺服器端應用程式中, 使用下列內容建立*Pages/主機. cshtml*檔案:</span><span class="sxs-lookup"><span data-stu-id="cb82e-584">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor server-side app:</span></span>

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

<span data-ttu-id="cb82e-585">當地語系化會在應用程式中處理:</span><span class="sxs-lookup"><span data-stu-id="cb82e-585">Localization is handled in the app:</span></span>

1. <span data-ttu-id="cb82e-586">瀏覽器會將初始 HTTP 要求傳送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-586">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="cb82e-587">文化特性是由當地語系化中介軟體所指派。</span><span class="sxs-lookup"><span data-stu-id="cb82e-587">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="cb82e-588">_Host `OnGet`中的方法會在 cookie 中保存文化特性, 做為回應的一部分。</span><span class="sxs-lookup"><span data-stu-id="cb82e-588">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="cb82e-589">瀏覽器會開啟 WebSocket 連線, 以建立互動式 Blazor 伺服器端會話。</span><span class="sxs-lookup"><span data-stu-id="cb82e-589">The browser opens a WebSocket connection to create an interactive Blazor server-side session.</span></span>
1. <span data-ttu-id="cb82e-590">當地語系化中介軟體會讀取 cookie 並指派文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-590">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="cb82e-591">Blazor 伺服器端會話的開頭是正確的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-591">The Blazor server-side session begins with the correct culture.</span></span>

## <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="cb82e-592">提供 UI 以選擇文化特性</span><span class="sxs-lookup"><span data-stu-id="cb82e-592">Provide UI to choose the culture</span></span>

<span data-ttu-id="cb82e-593">為了提供允許使用者選取文化特性的 UI, 建議使用以重新*導向為基礎的方法*。</span><span class="sxs-lookup"><span data-stu-id="cb82e-593">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="cb82e-594">此程式類似于在使用者嘗試存取安全資源&mdash;時, 會將使用者重新導向至登入頁面, 然後重新導向至原始資源的情況下, 在 web 應用程式中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="cb82e-594">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="cb82e-595">應用程式會透過重新導向至控制器的方式, 來保存使用者選取的文化特性。</span><span class="sxs-lookup"><span data-stu-id="cb82e-595">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="cb82e-596">控制器會將使用者選取的文化特性設定為 cookie, 並將使用者重新導向回到原始 URI。</span><span class="sxs-lookup"><span data-stu-id="cb82e-596">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="cb82e-597">在伺服器上建立 HTTP 端點, 以在 cookie 中設定使用者選取的文化特性, 並執行重新導向回到原始 URI:</span><span class="sxs-lookup"><span data-stu-id="cb82e-597">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="cb82e-598">`LocalRedirect`使用動作結果來防止開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="cb82e-598">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="cb82e-599">如需詳細資訊，請參閱 <xref:security/preventing-open-redirects>。</span><span class="sxs-lookup"><span data-stu-id="cb82e-599">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="cb82e-600">下列元件顯示當使用者選取文化特性時, 如何執行初始重新導向的範例:</span><span class="sxs-lookup"><span data-stu-id="cb82e-600">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```cshtml
@inject IUriHelper UriHelper

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(UIChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(UriHelper.GetAbsoluteUri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        UriHelper.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-blazor-apps"></a><span data-ttu-id="cb82e-601">在 Blazor apps 中使用 .NET 當地語系化案例</span><span class="sxs-lookup"><span data-stu-id="cb82e-601">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="cb82e-602">在 Blazor apps 中, 可以使用下列 .NET 當地語系化和全球化案例:</span><span class="sxs-lookup"><span data-stu-id="cb82e-602">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="cb82e-603">.NET 的資源系統</span><span class="sxs-lookup"><span data-stu-id="cb82e-603">.NET's resources system</span></span>
* <span data-ttu-id="cb82e-604">特定文化特性的數位和日期格式</span><span class="sxs-lookup"><span data-stu-id="cb82e-604">Culture-specific number and date formatting</span></span>

<span data-ttu-id="cb82e-605">Blazor 的`@bind`功能會根據使用者目前的文化特性來執行全球化。</span><span class="sxs-lookup"><span data-stu-id="cb82e-605">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="cb82e-606">如需詳細資訊, 請參閱[資料](#data-binding)系結一節。</span><span class="sxs-lookup"><span data-stu-id="cb82e-606">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="cb82e-607">目前支援一組有限的 ASP.NET Core 當地語系化案例:</span><span class="sxs-lookup"><span data-stu-id="cb82e-607">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="cb82e-608">`IStringLocalizer<>`Blazor 應用程式*支援*。</span><span class="sxs-lookup"><span data-stu-id="cb82e-608">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="cb82e-609">`IHtmlLocalizer<>`、 `IViewLocalizer<>`和資料批註當地語系化 ASP.NET Core MVC 案例中, 而且**不支援**Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cb82e-609">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="cb82e-610">如需詳細資訊，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="cb82e-610">For more information, see <xref:fundamentals/localization>.</span></span>
