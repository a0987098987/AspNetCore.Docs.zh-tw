---
title: ASP.NET Core 中的元件標記協助程式
author: guardrex
ms.author: riande
description: 瞭解如何使用 ASP.NET Core 元件標記協助程式，在頁面和視圖中轉譯 Razor 元件。
ms.custom: mvc
ms.date: 03/18/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 801ceb73de5bb4ef7500624e1fbddbf96d1ab89c
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226393"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="11dd6-103">ASP.NET Core 中的元件標記協助程式</span><span class="sxs-lookup"><span data-stu-id="11dd6-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="11dd6-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="11dd6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="11dd6-105">若要從頁面或視圖呈現元件，請使用[元件標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="11dd6-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

<span data-ttu-id="11dd6-106">下列元件標記協助程式會在頁面或視圖中轉譯 `Counter` 元件：</span><span class="sxs-lookup"><span data-stu-id="11dd6-106">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="11dd6-107">元件標記協助程式也可以將參數傳遞給元件。</span><span class="sxs-lookup"><span data-stu-id="11dd6-107">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="11dd6-108">請考慮下列 `ColorfulCheckbox` 元件，其會設定核取方塊標籤的色彩和大小：</span><span class="sxs-lookup"><span data-stu-id="11dd6-108">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

<span data-ttu-id="11dd6-109">元件標記協助程式可以設定 `Size` （`int`）和 `Color` （`string`）[元件參數](xref:blazor/components#component-parameters)：</span><span class="sxs-lookup"><span data-stu-id="11dd6-109">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="11dd6-110">下列 HTML 會在頁面或視圖中呈現：</span><span class="sxs-lookup"><span data-stu-id="11dd6-110">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="11dd6-111">傳遞加上引號的字串需要[明確的 Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions)，如上述範例中的 `param-Color` 所示。</span><span class="sxs-lookup"><span data-stu-id="11dd6-111">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="11dd6-112">`string` 類型值的 Razor 剖析行為不適用於 `param-*` 屬性，因為該屬性是 `object` 類型。</span><span class="sxs-lookup"><span data-stu-id="11dd6-112">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="11dd6-113">參數類型必須是可序列化的 JSON，這通常表示該類型必須有預設的函式和可設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="11dd6-113">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="11dd6-114">例如，您可以在上述範例中指定 `Size` 和 `Color` 的值，因為 `Size` 和 `Color` 的類型是 JSON 序列化程式所支援的基本類型（`int` 和 `string`）。</span><span class="sxs-lookup"><span data-stu-id="11dd6-114">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="11dd6-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> 設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="11dd6-115"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="11dd6-116">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="11dd6-116">Is prerendered into the page.</span></span>
* <span data-ttu-id="11dd6-117">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="11dd6-117">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="11dd6-118">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="11dd6-118">Render Mode</span></span> | <span data-ttu-id="11dd6-119">描述</span><span class="sxs-lookup"><span data-stu-id="11dd6-119">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="11dd6-120">將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="11dd6-120">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="11dd6-121">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11dd6-121">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="11dd6-122">呈現 Blazor 伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="11dd6-122">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="11dd6-123">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="11dd6-123">Output from the component isn't included.</span></span> <span data-ttu-id="11dd6-124">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="11dd6-124">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="11dd6-125">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="11dd6-125">Renders the component into static HTML.</span></span> |

<span data-ttu-id="11dd6-126">雖然頁面和視圖可以使用元件，但相反的情況並非如此。</span><span class="sxs-lookup"><span data-stu-id="11dd6-126">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="11dd6-127">元件不能使用 view 和 page 特有的功能，例如部分視圖和區段。</span><span class="sxs-lookup"><span data-stu-id="11dd6-127">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="11dd6-128">若要在元件中使用部分視圖的邏輯，請將部分視圖邏輯分解成元件。</span><span class="sxs-lookup"><span data-stu-id="11dd6-128">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="11dd6-129">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="11dd6-129">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11dd6-130">其他資源</span><span class="sxs-lookup"><span data-stu-id="11dd6-130">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
