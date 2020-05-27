---
---
當 Blazor 伺服器應用程式進行預先處理時，某些動作（例如呼叫 JavaScript）並不可能，因為尚未建立與瀏覽器的連接。 元件可能需要在資源清單時以不同的方式呈現。

若要延遲 JavaScript interop 呼叫，直到建立與瀏覽器的連線之後，您可以使用[OnAfterRenderAsync 元件生命週期事件](xref:blazor/lifecycle#after-component-render)。 只有在完整呈現應用程式並建立用戶端連線之後，才會呼叫此事件。

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

針對上述範例程式碼，請在 `setElementText` `<head>` *Wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. cshtml* （Blazor Server）的元素內提供 JavaScript 函式。 呼叫函式時 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> ，不會傳回值：

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> 上述範例只會修改檔物件模型（DOM），僅供示範之用。 在大部分的情況下，不建議直接修改具有 JavaScript 的 DOM，因為 JavaScript 可能會干擾 Blazor 的變更追蹤。

下列元件示範如何使用 JavaScript interop 做為元件初始化邏輯的一部分，使其與可處理性相容。 此元件顯示可以從內部觸發轉譯更新 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> 。 開發人員必須避免在此案例中建立無限迴圈。

在呼叫的位置中 <xref:Microsoft.JSInterop.JSRuntime.InvokeAsync%2A?displayProperty=nameWithType> ， `ElementRef` 只會用於 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> ，而不是在任何較早的生命週期方法中使用，因為在呈現元件之前，沒有 JavaScript 元素。

呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)以 rerender 具有從 JavaScript interop 呼叫取得之新狀態的元件。 程式碼不會建立無限迴圈，因為 `StateHasChanged` 只有在是時才會呼叫 `infoFromJs` `null` 。

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

針對上述範例程式碼，請在 `setElementText` `<head>` *Wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. cshtml* （Blazor Server）的元素內提供 JavaScript 函式。 使用呼叫函式 <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> ，並傳回值：

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> 上述範例只會修改檔物件模型（DOM），僅供示範之用。 在大部分的情況下，不建議直接修改具有 JavaScript 的 DOM，因為 JavaScript 可能會干擾 Blazor 的變更追蹤。
