---
no-loc:
- Blazor
- SignalR
ms.openlocfilehash: 5f3e22e04fe18149ec5a8acb42f42a8ef83a7664
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78659716"
---
當Blazor伺服器應用處於預渲染狀態時,某些操作(如調用 JAvaScript)是不可能的,因為尚未與瀏覽器建立連接。 元件在預渲染時可能需要以不同的方式呈現。

要延遲 JavaScript 互通呼叫,直到建立與瀏覽器的連線後,可以使用 On[後 RenderAsync 元件生命週期事件](xref:blazor/lifecycle#after-component-render)。 僅當應用程式完全呈現並建立用戶端連接後,才會調用此事件。

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<div @ref="divElement">Text during render</div>

@code {
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await JSRuntime.InvokeVoidAsync(
                "setElementText", divElement, "Text after render");
        }
    }
}
```

對於前面的範例代碼,在*wwwroot/index.html(WebAssembly)* 或Blazor*頁面/_Host.cshtml(*Blazor伺服器)`setElementText``<head>`的元素內提供 JavaScript 函數。 函數呼叫`IJSRuntime.InvokeVoidAsync`,不返回值:

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> 前面的範例直接修改文件物件模型 (DOM) 僅用於演示目的。 在大多數情況下,不建議使用 JAvaScript 直接修改 DOM,因為 JavaScript 可能會干擾Blazor更改跟蹤。

以下元件演示如何以與預算相容的方式將 JAVAScript 互操作用作元件初始化邏輯的一部分。 該元件顯示可以從內部`OnAfterRenderAsync`觸發呈現更新。 開發人員必須避免在此方案中創建無限迴圈。

在`JSRuntime.InvokeAsync`調用的位置`ElementRef`, 僅在任何早期`OnAfterRenderAsync`生命週期 方法中使用,而不是在任何早期生命週期方法中使用,因為在呈現元件之前沒有 JAVAScript 元素。

呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)以使用從 JavaScript 互通呼叫取得的新狀態重新呈現元件。 程式碼不會建立無限循環,`StateHasChanged`因為只有`infoFromJs`時`null`呼叫 。

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

Set value via JS interop call:
<div id="val-set-by-interop" @ref="divElement"></div>

@code {
    private string infoFromJs;
    private ElementReference divElement;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementText", divElement, "Hello from interop call!");

            StateHasChanged();
        }
    }
}
```

對於前面的範例代碼,在*wwwroot/index.html(WebAssembly)* 或Blazor*頁面/_Host.cshtml(*Blazor伺服器)`setElementText``<head>`的元素內提供 JavaScript 函數。 函式呼叫`IJSRuntime.InvokeAsync`並傳回值:

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> 前面的範例直接修改文件物件模型 (DOM) 僅用於演示目的。 在大多數情況下,不建議使用 JAvaScript 直接修改 DOM,因為 JavaScript 可能會干擾Blazor更改跟蹤。
