---
title: ASP.NET核心中的元件標記說明程式
author: guardrex
ms.author: riande
description: 瞭解如何使用ASP.NET核心元件標記説明器在頁面和檢視中呈現Razor元件。
ms.custom: mvc
ms.date: 04/15/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: aaa4b92a8912b4f52d861ed07432aa7cf3ca5240
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440957"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="90424-103">ASP.NET核心中的元件標記說明程式</span><span class="sxs-lookup"><span data-stu-id="90424-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="90424-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="90424-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="90424-105">要從頁面或檢視呈現元件,請使用[元件標記說明程式](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)。</span><span class="sxs-lookup"><span data-stu-id="90424-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90424-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="90424-106">Prerequisites</span></span>

<span data-ttu-id="90424-107">依「準備應用程式」 的指南<xref:blazor/integrate-components#prepare-the-app>在文章的*頁面與檢視部分使用元件*。</span><span class="sxs-lookup"><span data-stu-id="90424-107">Follow the guidance in the *Prepare the app to use components in pages and views* section of the <xref:blazor/integrate-components#prepare-the-app> article.</span></span>

## <a name="component-tag-helper"></a><span data-ttu-id="90424-108">元件標記說明程式</span><span class="sxs-lookup"><span data-stu-id="90424-108">Component Tag Helper</span></span>

<span data-ttu-id="90424-109">以下元件標記說明程式在頁面或檢視中`Counter`呈現元件:</span><span class="sxs-lookup"><span data-stu-id="90424-109">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Pages

...

<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="90424-110">前面的範例假定元件`Counter`位於應用的 *「主頁」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="90424-110">The preceding example assumes that the `Counter` component is in the app's *Pages* folder.</span></span>

<span data-ttu-id="90424-111">元件標記説明程式還可以將參數傳遞給元件。</span><span class="sxs-lookup"><span data-stu-id="90424-111">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="90424-112">請考慮設定複選框`ColorfulCheckbox`分頁顏色和大小的以下元件:</span><span class="sxs-lookup"><span data-stu-id="90424-112">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

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

<span data-ttu-id="90424-113">`Size` (`int``Color`)`string`與 ( )[元件參數](xref:blazor/components#component-parameters)可以由元件標記說明程式設定:</span><span class="sxs-lookup"><span data-stu-id="90424-113">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Shared

...

<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="90424-114">前面的範例假定元件`ColorfulCheckbox`位於應用的 *「共用」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="90424-114">The preceding example assumes that the `ColorfulCheckbox` component is in the app's *Shared* folder.</span></span>

<span data-ttu-id="90424-115">以下 HTML 呈現在頁面或檢視中:</span><span class="sxs-lookup"><span data-stu-id="90424-115">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="90424-116">傳遞引用的字串需要[顯式 Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions),`param-Color`如前面的 範例所示。</span><span class="sxs-lookup"><span data-stu-id="90424-116">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="90424-117">`string`類型值的 Razor 解析行為不適用於`param-*`屬性, 因為屬性`object`是一種 類型。</span><span class="sxs-lookup"><span data-stu-id="90424-117">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="90424-118">參數類型必須是 JSON 可序列化的,這通常意味著類型必須具有預設構造函數和可設置屬性。</span><span class="sxs-lookup"><span data-stu-id="90424-118">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="90424-119">例如,`Size`您可以為 和在前面的範`Color`例中 指定值`Size``Color`,因為和的類型是`int`基`string`元類型 ( 和 ),JSON 序列化器支援。</span><span class="sxs-lookup"><span data-stu-id="90424-119">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="90424-120">在下面的範例中,類別傳遞給元件:</span><span class="sxs-lookup"><span data-stu-id="90424-120">In the following example, a class object is passed to the component:</span></span>

<span data-ttu-id="90424-121">*MyClass.cs*:</span><span class="sxs-lookup"><span data-stu-id="90424-121">*MyClass.cs*:</span></span>

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

<span data-ttu-id="90424-122">**類必須具有公共無參數構造函數。**</span><span class="sxs-lookup"><span data-stu-id="90424-122">**The class must have a public parameterless constructor.**</span></span>

<span data-ttu-id="90424-123">*分享/我的元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="90424-123">*Shared/MyComponent.razor*:</span></span>

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

<span data-ttu-id="90424-124">*頁面/MyPage.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="90424-124">*Pages/MyPage.cshtml*:</span></span>

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

<span data-ttu-id="90424-125">前面的範例假定元件`MyComponent`位於應用的 *「共用」* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="90424-125">The preceding example assumes that the `MyComponent` component is in the app's *Shared* folder.</span></span> <span data-ttu-id="90424-126">`MyClass`在套用的命名空間中`{APP ASSEMBLY}`。</span><span class="sxs-lookup"><span data-stu-id="90424-126">`MyClass` is in the app's namespace (`{APP ASSEMBLY}`).</span></span>

<span data-ttu-id="90424-127"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定元件是否:</span><span class="sxs-lookup"><span data-stu-id="90424-127"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="90424-128">預置到頁面中。</span><span class="sxs-lookup"><span data-stu-id="90424-128">Is prerendered into the page.</span></span>
* <span data-ttu-id="90424-129">在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。</span><span class="sxs-lookup"><span data-stu-id="90424-129">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="90424-130">成像模式</span><span class="sxs-lookup"><span data-stu-id="90424-130">Render Mode</span></span> | <span data-ttu-id="90424-131">描述</span><span class="sxs-lookup"><span data-stu-id="90424-131">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="90424-132">將元件呈現為靜態 HTML,並包含伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="90424-132">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="90424-133">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="90424-133">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="90424-134">渲染伺服器應用的Blazor標記。</span><span class="sxs-lookup"><span data-stu-id="90424-134">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="90424-135">不包括元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="90424-135">Output from the component isn't included.</span></span> <span data-ttu-id="90424-136">當使用者代理啟動時,此標記用於引導Blazor應用。</span><span class="sxs-lookup"><span data-stu-id="90424-136">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="90424-137">將元件呈現為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="90424-137">Renders the component into static HTML.</span></span> |

<span data-ttu-id="90424-138">雖然頁面和視圖可以使用元件,但事實並非如此。</span><span class="sxs-lookup"><span data-stu-id="90424-138">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="90424-139">元件無法使用特定於檢視和頁面的功能,如部分檢視和節。</span><span class="sxs-lookup"><span data-stu-id="90424-139">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="90424-140">要使用元件中部分檢視的邏輯,將部分檢視邏輯分解到元件中。</span><span class="sxs-lookup"><span data-stu-id="90424-140">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="90424-141">不支援從靜態 HTML 頁呈現伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="90424-141">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90424-142">其他資源</span><span class="sxs-lookup"><span data-stu-id="90424-142">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
