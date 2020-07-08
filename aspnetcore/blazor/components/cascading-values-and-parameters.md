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
# <a name="aspnet-core-blazor-cascading-values-and-parameters"></a>ASP.NET Core Blazor 級聯的值和參數

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

在某些情況下，使用[元件參數](xref:blazor/components/index#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。 串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。 級聯的值和參數也會提供一種方法來協調元件。

### <a name="theme-example"></a>主題範例

在範例應用程式的下列範例中， `ThemeInfo` 類別會指定主題資訊以向下流動元件階層，讓應用程式中指定部分內的所有按鈕共用相同的樣式。

`UIThemeClasses/ThemeInfo.cs`:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

祖系元件可以使用串聯值元件來提供串聯值。 此 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。

例如，範例應用程式 `ThemeInfo` 會在其中一個應用程式的配置中，將主題資訊（）指定為構成屬性版面配置主體之所有元件的串聯參數 `@Body` 。 `ButtonClass`在版面配置元件中，會指派的值 `btn-success` 。 任何子代元件都可以透過串聯物件使用此屬性 `ThemeInfo` 。

`CascadingValuesParametersLayout`成分

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

為了利用串聯值，元件會使用屬性宣告串聯式參數 [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 。 串聯式值會依類型系結至串聯式參數。

在範例應用程式中，元件會將串聯值系結至串聯式 `CascadingValuesParametersTheme` `ThemeInfo` 參數。 參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。

`CascadingValuesParametersTheme`成分

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

若要在相同的子樹中串聯多個相同類型的值，請為 <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> 每個 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件和其對應的屬性提供唯一的字串 [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 。 在下列範例中，兩個 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件依名稱串聯不同的實例 `MyCascadingType` ：

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

在子系元件中，串聯的參數會以名稱從上階元件中對應的串聯值接收其值：

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>TabSet 範例

串聯式參數也可以讓元件在元件階層之間共同作業。 例如，請考慮 `TabSet` 範例應用程式中的下列範例。

範例應用程式具有可 `ITab` 執行 tab 鍵的介面：

[!code-csharp[](../common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

`CascadingValuesParametersTabSet`元件會使用 `TabSet` 元件，其中包含數個 `Tab` 元件：

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

子 `Tab` 元件不會明確地當做參數傳遞至 `TabSet` 。 相反地，子 `Tab` 元件是的子內容的一部分 `TabSet` 。 不過， `TabSet` 仍然需要知道每個元件， `Tab` 使其可以呈現標頭和使用中的索引標籤。若要啟用這項協調而不需要額外的程式碼， `TabSet` 元件*可以提供本身作為*串聯的值，然後由子代 `Tab` 元件挑選。

`TabSet`成分

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

子系 `Tab` 元件會以串聯式參數的形式捕捉包含的 `TabSet` ，因此 `Tab` 元件會將自己加入至索引標籤作用 `TabSet` 中的和座標。

`Tab`成分

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]
