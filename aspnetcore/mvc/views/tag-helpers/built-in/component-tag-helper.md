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
# <a name="component-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的元件標記協助程式

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

若要從頁面或視圖呈現元件，請使用[元件標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)協助程式。

下列元件標記協助程式會在頁面或視圖中轉譯 `Counter` 元件：

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

元件標記協助程式也可以將參數傳遞給元件。 請考慮下列 `ColorfulCheckbox` 元件，其會設定核取方塊標籤的色彩和大小：

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

元件標記協助程式可以設定 `Size` （`int`）和 `Color` （`string`）[元件參數](xref:blazor/components#component-parameters)：

```cshtml
<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

下列 HTML 會在頁面或視圖中呈現：

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

傳遞加上引號的字串需要[明確的 Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions)，如上述範例中的 `param-Color` 所示。 `string` 類型值的 Razor 剖析行為不適用於 `param-*` 屬性，因為該屬性是 `object` 類型。

參數類型必須是可序列化的 JSON，這通常表示該類型必須有預設的函式和可設定的屬性。 例如，您可以在上述範例中指定 `Size` 和 `Color` 的值，因為 `Size` 和 `Color` 的類型是 JSON 序列化程式所支援的基本類型（`int` 和 `string`）。

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> 設定元件是否：

* 會資源清單到頁面中。
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動 Blazor 應用程式所需的資訊。

| 轉譯模式 | 描述 |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將元件轉譯為靜態 HTML，並包含 Blazor 伺服器應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 呈現 Blazor 伺服器應用程式的標記。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將元件轉譯為靜態 HTML。 |

雖然頁面和視圖可以使用元件，但相反的情況並非如此。 元件不能使用 view 和 page 特有的功能，例如部分視圖和區段。 若要在元件中使用部分視圖的邏輯，請將部分視圖邏輯分解成元件。

不支援從靜態 HTML 網頁轉譯伺服器元件。

## <a name="additional-resources"></a>其他資源

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components>
