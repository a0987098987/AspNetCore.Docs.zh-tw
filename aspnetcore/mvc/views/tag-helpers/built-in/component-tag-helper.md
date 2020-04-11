---
title: ASP.NET核心中的元件標記說明程式
author: guardrex
ms.author: riande
description: 瞭解如何使用ASP.NET核心元件標記説明器在頁面和檢視中呈現Razor元件。
ms.custom: mvc
ms.date: 04/01/2020
no-loc:
- Blazor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 4a6b21229ce086099fcddfeb51c3a959ef639f24
ms.sourcegitcommit: e8dc30453af8bbefcb61857987090d79230a461d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123434"
---
# <a name="component-tag-helper-in-aspnet-core"></a>ASP.NET核心中的元件標記說明程式

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

要從頁面或檢視呈現元件,請使用[元件標記說明程式](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)。

## <a name="prerequisites"></a>Prerequisites

依「準備應用程式」 的指南<xref:blazor/integrate-components#prepare-the-app-to-use-components-in-pages-and-views>在文章的*頁面與檢視部分使用元件*。

## <a name="component-tag-helper"></a>元件標記說明程式

以下元件標記說明程式在頁面或檢視中`Counter`呈現元件:

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Pages

...

<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

前面的範例假定元件`Counter`位於應用的 *「主頁」* 資料夾中。

元件標記説明程式還可以將參數傳遞給元件。 請考慮設定複選框`ColorfulCheckbox`分頁顏色和大小的以下元件:

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

`Size` (`int``Color`)`string`與 ( )[元件參數](xref:blazor/components#component-parameters)可以由元件標記說明程式設定:

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Shared

...

<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

前面的範例假定元件`ColorfulCheckbox`位於應用的 *「共用」* 資料夾中。

以下 HTML 呈現在頁面或檢視中:

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

傳遞引用的字串需要[顯式 Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions),`param-Color`如前面的 範例所示。 `string`類型值的 Razor 解析行為不適用於`param-*`屬性, 因為屬性`object`是一種 類型。

參數類型必須是 JSON 可序列化的,這通常意味著類型必須具有預設構造函數和可設置屬性。 例如,`Size`您可以為 和在前面的範`Color`例中 指定值`Size``Color`,因為和的類型是`int`基`string`元類型 ( 和 ),JSON 序列化器支援。

在下面的範例中,類別傳遞給元件:

*MyClass.cs*:

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

**類必須具有公共無參數構造函數。**

*分享/我的元件.razor*:

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

*頁面/MyPage.cshtml*:

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

前面的範例假定元件`MyComponent`位於應用的 *「共用」* 資料夾中。 `MyClass`在套用的命名空間中`{APP ASSEMBLY}`。

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定元件是否:

* 預置到頁面中。
* 在頁面上呈現為靜態 HTML,或者如果它包含從使用者代理引導 Blazor 應用的必要資訊。

| 成像模式 | 描述 |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將元件呈現為靜態 HTML,並包含伺服器應用的Blazor標記。 當使用者代理啟動時,此標記用於引導Blazor應用。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 渲染伺服器應用的Blazor標記。 不包括元件的輸出。 當使用者代理啟動時,此標記用於引導Blazor應用。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將元件呈現為靜態 HTML。 |

雖然頁面和視圖可以使用元件,但事實並非如此。 元件無法使用特定於檢視和頁面的功能,如部分檢視和節。 要使用元件中部分檢視的邏輯,將部分檢視邏輯分解到元件中。

不支援從靜態 HTML 頁呈現伺服器元件。

## <a name="additional-resources"></a>其他資源

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
