當 Blazor 伺服器應用程式進行預先處理時，某些動作（例如呼叫 JavaScript）並不可能，因為尚未建立與瀏覽器的連接。 元件可能需要在資源清單時以不同的方式呈現。

若要延遲 JavaScript interop 呼叫，直到建立與瀏覽器的連線之後，您可以使用`OnAfterRenderAsync`元件生命週期事件。 只有在完整呈現應用程式並建立用戶端連線之後，才會呼叫此事件。

```cshtml
@using Microsoft.JSInterop
@inject IJSRuntime JSRuntime

<input @ref="myInput" value="Value set during render" />

@code {
    private ElementReference myInput;

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            JSRuntime.InvokeAsync<object>(
                "setElementValue", myInput, "Value set after render");
        }
    }
}
```

下列元件示範如何使用 JavaScript interop 做為元件初始化邏輯的一部分，使其與可處理性相容。 此元件顯示可以從內部`OnAfterRenderAsync`觸發轉譯更新。 開發人員必須避免在此案例中建立無限迴圈。

在`JSRuntime.InvokeAsync`呼叫的位置`ElementRef`中，只會`OnAfterRenderAsync`用於，而不是在任何較早的生命週期方法中使用，因為在呈現元件之前，沒有 JavaScript 元素。

`StateHasChanged`呼叫以 rerender 具有從 JavaScript interop 呼叫取得之新狀態的元件。 程式碼不會建立無限迴圈， `StateHasChanged`因為只有在是`infoFromJs` `null`時才會呼叫。

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
@inject IComponentContext ComponentContext
@inject IJSRuntime JSRuntime

<p>
    Get value via JS interop call:
    <strong id="val-get-by-interop">@(infoFromJs ?? "No value yet")</strong>
</p>

<p>
    Set value via JS interop call:
    <input id="val-set-by-interop" @ref="myElem" />
</p>

@code {
    private string infoFromJs;
    private ElementReference myElem;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender && infoFromJs == null)
        {
            infoFromJs = await JSRuntime.InvokeAsync<string>(
                "setElementValue", myElem, "Hello from interop call");

            StateHasChanged();
        }
    }
}
```

若要根據應用程式目前是否為已轉譯的內容，有條件地呈現`IsConnected`不同的內容`IComponentContext` ，請在服務上使用屬性。 對於 Blazor 伺服器應用程式`IsConnected` ， `true`只有在有作用中的用戶端連接時，才會傳回。 它一律`true`會在 Blazor WebAssembly apps 中傳回。

```cshtml
@page "/isconnected-example"
@using Microsoft.AspNetCore.Components.Services
@inject IComponentContext ComponentContext

<h1>IsConnected Example</h1>

<p>
    Current state:
    <strong id="connected-state">
        @(ComponentContext.IsConnected ? "connected" : "not connected")
    </strong>
</p>

<p>
    Clicks:
    <strong id="count">@count</strong>
    <button id="increment-count" @onclick="@(() => count++)">Click me</button>
</p>

@code {
    private int count;
}
```
