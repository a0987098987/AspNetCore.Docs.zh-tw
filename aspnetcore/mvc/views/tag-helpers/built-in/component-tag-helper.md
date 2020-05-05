---
title: ASP.NET Core 中的元件標記協助程式
author: guardrex
ms.author: riande
description: 瞭解如何使用 ASP.NET Core 元件標記協助程式來轉譯頁面Razor和視圖中的元件。
ms.custom: mvc
ms.date: 04/15/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 4e003e5ed5e7863d8a218c0f02bb37e214e31910
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82773925"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="058aa-103">ASP.NET Core 中的元件標記協助程式</span><span class="sxs-lookup"><span data-stu-id="058aa-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="058aa-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="058aa-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="058aa-105">若要從頁面或視圖呈現元件，請使用[元件標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="058aa-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="058aa-106">必要條件</span><span class="sxs-lookup"><span data-stu-id="058aa-106">Prerequisites</span></span>

<span data-ttu-id="058aa-107">請遵循<xref:blazor/integrate-components#prepare-the-app>文章的*準備應用程式以使用頁面和 views 中的元件*一節中的指導方針。</span><span class="sxs-lookup"><span data-stu-id="058aa-107">Follow the guidance in the *Prepare the app to use components in pages and views* section of the <xref:blazor/integrate-components#prepare-the-app> article.</span></span>

## <a name="component-tag-helper"></a><span data-ttu-id="058aa-108">元件標記協助程式</span><span class="sxs-lookup"><span data-stu-id="058aa-108">Component Tag Helper</span></span>

<span data-ttu-id="058aa-109">下列元件標記協助程式會在`Counter`頁面或視圖中呈現元件：</span><span class="sxs-lookup"><span data-stu-id="058aa-109">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Pages

...

<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="058aa-110">上述範例假設`Counter`元件位於應用程式的*Pages*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="058aa-110">The preceding example assumes that the `Counter` component is in the app's *Pages* folder.</span></span>

<span data-ttu-id="058aa-111">元件標記協助程式也可以將參數傳遞給元件。</span><span class="sxs-lookup"><span data-stu-id="058aa-111">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="058aa-112">請考慮下列`ColorfulCheckbox`設定核取方塊標籤色彩和大小的元件：</span><span class="sxs-lookup"><span data-stu-id="058aa-112">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

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

<span data-ttu-id="058aa-113">元件`Size`標記`int`協助程式`Color`可以`string`設定（）和（）[元件參數](xref:blazor/components#component-parameters)：</span><span class="sxs-lookup"><span data-stu-id="058aa-113">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Shared

...

<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="058aa-114">上述範例假設`ColorfulCheckbox`元件位於應用程式的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="058aa-114">The preceding example assumes that the `ColorfulCheckbox` component is in the app's *Shared* folder.</span></span>

<span data-ttu-id="058aa-115">下列 HTML 會在頁面或視圖中呈現：</span><span class="sxs-lookup"><span data-stu-id="058aa-115">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="058aa-116">傳遞加上引號的字串需要[明確的 Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions)，如`param-Color`上述範例中所示。</span><span class="sxs-lookup"><span data-stu-id="058aa-116">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="058aa-117">`string`類型值的 Razor 剖析行為不適用於`param-*`屬性，因為屬性是`object`類型。</span><span class="sxs-lookup"><span data-stu-id="058aa-117">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="058aa-118">參數類型必須是可序列化的 JSON，這通常表示該類型必須有預設的函式和可設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="058aa-118">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="058aa-119">例如，您可以在上述範例中指定`Size`和`Color`的值， `Size`因為和`Color`的類型是基本類型（`int`和`string`），這是 JSON 序列化程式所支援的型別。</span><span class="sxs-lookup"><span data-stu-id="058aa-119">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="058aa-120">在下列範例中，類別物件會傳遞至元件：</span><span class="sxs-lookup"><span data-stu-id="058aa-120">In the following example, a class object is passed to the component:</span></span>

<span data-ttu-id="058aa-121">*MyClass.cs*：</span><span class="sxs-lookup"><span data-stu-id="058aa-121">*MyClass.cs*:</span></span>

```csharp
public class MyClass
{
    public MyClass()
    {
    }

    public int MyInt { get; set; } = 999;
    public string MyString { get; set; } = "Initial value";
}
```

<span data-ttu-id="058aa-122">**類別必須有公用無參數的函式。**</span><span class="sxs-lookup"><span data-stu-id="058aa-122">**The class must have a public parameterless constructor.**</span></span>

<span data-ttu-id="058aa-123">*Shared/mycomponent 之下. razor*：</span><span class="sxs-lookup"><span data-stu-id="058aa-123">*Shared/MyComponent.razor*:</span></span>

```razor
<h2>MyComponent</h2>

<p>Int: @MyObject.MyInt</p>
<p>String: @MyObject.MyString</p>

@code
{
    [Parameter]
    public MyClass MyObject { get; set; }
}
```

<span data-ttu-id="058aa-124">*Pages/mypage.aspx. cshtml*：</span><span class="sxs-lookup"><span data-stu-id="058aa-124">*Pages/MyPage.cshtml*:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}
@using {APP ASSEMBLY}.Shared

...

@{
    var myObject = new MyClass();
    myObject.MyInt = 7;
    myObject.MyString = "Set by MyPage";
}

<component type="typeof(MyComponent)" render-mode="ServerPrerendered" 
    param-MyObject="@myObject" />
```

<span data-ttu-id="058aa-125">上述範例假設`MyComponent`元件位於應用程式的*共用*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="058aa-125">The preceding example assumes that the `MyComponent` component is in the app's *Shared* folder.</span></span> <span data-ttu-id="058aa-126">`MyClass`位於應用程式的命名空間（`{APP ASSEMBLY}`）。</span><span class="sxs-lookup"><span data-stu-id="058aa-126">`MyClass` is in the app's namespace (`{APP ASSEMBLY}`).</span></span>

<span data-ttu-id="058aa-127"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定元件是否：</span><span class="sxs-lookup"><span data-stu-id="058aa-127"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="058aa-128">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="058aa-128">Is prerendered into the page.</span></span>
* <span data-ttu-id="058aa-129">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="058aa-129">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="058aa-130">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="058aa-130">Render Mode</span></span> | <span data-ttu-id="058aa-131">說明</span><span class="sxs-lookup"><span data-stu-id="058aa-131">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="058aa-132">將元件轉譯為靜態 HTML，並包含Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="058aa-132">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="058aa-133">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="058aa-133">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="058aa-134">呈現Blazor伺服器應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="058aa-134">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="058aa-135">不包含來自元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="058aa-135">Output from the component isn't included.</span></span> <span data-ttu-id="058aa-136">當使用者代理程式啟動時，會使用此標記來啟動Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="058aa-136">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="058aa-137">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="058aa-137">Renders the component into static HTML.</span></span> |

<span data-ttu-id="058aa-138">雖然頁面和視圖可以使用元件，但相反的情況並非如此。</span><span class="sxs-lookup"><span data-stu-id="058aa-138">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="058aa-139">元件不能使用 view 和 page 特有的功能，例如部分視圖和區段。</span><span class="sxs-lookup"><span data-stu-id="058aa-139">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="058aa-140">若要在元件中使用部分視圖的邏輯，請將部分視圖邏輯分解成元件。</span><span class="sxs-lookup"><span data-stu-id="058aa-140">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="058aa-141">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="058aa-141">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="058aa-142">其他資源</span><span class="sxs-lookup"><span data-stu-id="058aa-142">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
