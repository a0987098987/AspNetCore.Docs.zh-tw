---
title: ASP.NET Core Blazor 級聯的值和參數
author: guardrex
description: 瞭解如何將資料從上階元件傳送到子元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/cascading-values-and-parameters
ms.openlocfilehash: c426be21b520520c6745ada95be35816f7365c21
ms.sourcegitcommit: fa89d6553378529ae86b388689ac2c6f38281bb9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86059925"
---
# <a name="aspnet-core-blazor-cascading-values-and-parameters"></a><span data-ttu-id="c8327-103">ASP.NET Core Blazor 級聯的值和參數</span><span class="sxs-lookup"><span data-stu-id="c8327-103">ASP.NET Core Blazor cascading values and parameters</span></span>

<span data-ttu-id="c8327-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c8327-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="c8327-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c8327-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c8327-106">在某些情況下，使用[元件參數](xref:blazor/components/index#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。</span><span class="sxs-lookup"><span data-stu-id="c8327-106">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](xref:blazor/components/index#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="c8327-107">串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="c8327-107">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="c8327-108">級聯的值和參數也會提供一種方法來協調元件。</span><span class="sxs-lookup"><span data-stu-id="c8327-108">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="c8327-109">主題範例</span><span class="sxs-lookup"><span data-stu-id="c8327-109">Theme example</span></span>

<span data-ttu-id="c8327-110">在範例應用程式的下列範例中， `ThemeInfo` 類別會指定主題資訊以向下流動元件階層，讓應用程式中指定部分內的所有按鈕共用相同的樣式。</span><span class="sxs-lookup"><span data-stu-id="c8327-110">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="c8327-111">`UIThemeClasses/ThemeInfo.cs`:</span><span class="sxs-lookup"><span data-stu-id="c8327-111">`UIThemeClasses/ThemeInfo.cs`:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="c8327-112">祖系元件可以使用串聯值元件來提供串聯值。</span><span class="sxs-lookup"><span data-stu-id="c8327-112">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="c8327-113">此 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。</span><span class="sxs-lookup"><span data-stu-id="c8327-113">The <xref:Microsoft.AspNetCore.Components.CascadingValue%601> component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="c8327-114">例如，範例應用程式 `ThemeInfo` 會在其中一個應用程式的配置中，將主題資訊（）指定為構成屬性版面配置主體之所有元件的串聯參數 `@Body` 。</span><span class="sxs-lookup"><span data-stu-id="c8327-114">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="c8327-115">`ButtonClass`在版面配置元件中，會指派的值 `btn-success` 。</span><span class="sxs-lookup"><span data-stu-id="c8327-115">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="c8327-116">任何子代元件都可以透過串聯物件使用此屬性 `ThemeInfo` 。</span><span class="sxs-lookup"><span data-stu-id="c8327-116">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="c8327-117">`CascadingValuesParametersLayout`成分</span><span class="sxs-lookup"><span data-stu-id="c8327-117">`CascadingValuesParametersLayout` component:</span></span>

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

<span data-ttu-id="c8327-118">為了利用串聯值，元件會使用屬性宣告串聯式參數 [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="c8327-118">To make use of cascading values, components declare cascading parameters using the [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) attribute.</span></span> <span data-ttu-id="c8327-119">串聯式值會依類型系結至串聯式參數。</span><span class="sxs-lookup"><span data-stu-id="c8327-119">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="c8327-120">在範例應用程式中，元件會將串聯值系結至串聯式 `CascadingValuesParametersTheme` `ThemeInfo` 參數。</span><span class="sxs-lookup"><span data-stu-id="c8327-120">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="c8327-121">參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="c8327-121">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="c8327-122">`CascadingValuesParametersTheme`成分</span><span class="sxs-lookup"><span data-stu-id="c8327-122">`CascadingValuesParametersTheme` component:</span></span>

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

<span data-ttu-id="c8327-123">若要在相同的子樹中串聯多個相同類型的值，請為 <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> 每個 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件和其對應的屬性提供唯一的字串 [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="c8327-123">To cascade multiple values of the same type within the same subtree, provide a unique <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> string to each <xref:Microsoft.AspNetCore.Components.CascadingValue%601> component and its corresponding [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) attribute.</span></span> <span data-ttu-id="c8327-124">在下列範例中，兩個 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件依名稱串聯不同的實例 `MyCascadingType` ：</span><span class="sxs-lookup"><span data-stu-id="c8327-124">In the following example, two <xref:Microsoft.AspNetCore.Components.CascadingValue%601> components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value="@parentCascadeParameter1" Name="CascadeParam1">
    <CascadingValue Value="@ParentCascadeParameter2" Name="CascadeParam2">
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

<span data-ttu-id="c8327-125">在子系元件中，串聯的參數會以名稱從上階元件中對應的串聯值接收其值：</span><span class="sxs-lookup"><span data-stu-id="c8327-125">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="c8327-126">TabSet 範例</span><span class="sxs-lookup"><span data-stu-id="c8327-126">TabSet example</span></span>

<span data-ttu-id="c8327-127">串聯式參數也可以讓元件在元件階層之間共同作業。</span><span class="sxs-lookup"><span data-stu-id="c8327-127">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="c8327-128">例如，請考慮 `TabSet` 範例應用程式中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="c8327-128">For example, consider the following `TabSet` example in the sample app.</span></span>

<span data-ttu-id="c8327-129">範例應用程式具有可 `ITab` 執行 tab 鍵的介面：</span><span class="sxs-lookup"><span data-stu-id="c8327-129">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](../common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="c8327-130">`CascadingValuesParametersTabSet`元件會使用 `TabSet` 元件，其中包含數個 `Tab` 元件：</span><span class="sxs-lookup"><span data-stu-id="c8327-130">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
@page "/CascadingValuesParametersTabSet"

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

@code {
    private bool showThirdTab;
}
```

<span data-ttu-id="c8327-131">子 `Tab` 元件不會明確地當做參數傳遞至 `TabSet` 。</span><span class="sxs-lookup"><span data-stu-id="c8327-131">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="c8327-132">相反地，子 `Tab` 元件是的子內容的一部分 `TabSet` 。</span><span class="sxs-lookup"><span data-stu-id="c8327-132">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="c8327-133">不過， `TabSet` 仍然需要知道每個元件， `Tab` 使其可以呈現標頭和使用中的索引標籤。若要啟用這項協調而不需要額外的程式碼， `TabSet` 元件*可以提供本身作為*串聯的值，然後由子代 `Tab` 元件挑選。</span><span class="sxs-lookup"><span data-stu-id="c8327-133">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="c8327-134">`TabSet`成分</span><span class="sxs-lookup"><span data-stu-id="c8327-134">`TabSet` component:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="c8327-135">子系 `Tab` 元件會以串聯式參數的形式捕捉包含的 `TabSet` ，因此 `Tab` 元件會將自己加入至索引標籤作用 `TabSet` 中的和座標。</span><span class="sxs-lookup"><span data-stu-id="c8327-135">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="c8327-136">`Tab`成分</span><span class="sxs-lookup"><span data-stu-id="c8327-136">`Tab` component:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]
