<span data-ttu-id="6cf63-101">當 Blazor 伺服器應用程式進行預先處理時，某些動作（例如呼叫 JavaScript）並不可能，因為尚未建立與瀏覽器的連接。</span><span class="sxs-lookup"><span data-stu-id="6cf63-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="6cf63-102">元件可能需要在資源清單時以不同的方式呈現。</span><span class="sxs-lookup"><span data-stu-id="6cf63-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="6cf63-103">若要延遲 JavaScript interop 呼叫，直到建立與瀏覽器的連線之後，您可以使用`OnAfterRenderAsync`元件生命週期事件。</span><span class="sxs-lookup"><span data-stu-id="6cf63-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="6cf63-104">只有在完整呈現應用程式並建立用戶端連線之後，才會呼叫此事件。</span><span class="sxs-lookup"><span data-stu-id="6cf63-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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

<span data-ttu-id="6cf63-105">下列元件示範如何使用 JavaScript interop 做為元件初始化邏輯的一部分，使其與可處理性相容。</span><span class="sxs-lookup"><span data-stu-id="6cf63-105">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="6cf63-106">此元件顯示可以從內部`OnAfterRenderAsync`觸發轉譯更新。</span><span class="sxs-lookup"><span data-stu-id="6cf63-106">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="6cf63-107">開發人員必須避免在此案例中建立無限迴圈。</span><span class="sxs-lookup"><span data-stu-id="6cf63-107">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="6cf63-108">在`JSRuntime.InvokeAsync`呼叫的位置`ElementRef`中，只會`OnAfterRenderAsync`用於，而不是在任何較早的生命週期方法中使用，因為在呈現元件之前，沒有 JavaScript 元素。</span><span class="sxs-lookup"><span data-stu-id="6cf63-108">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="6cf63-109">`StateHasChanged`呼叫以 rerender 具有從 JavaScript interop 呼叫取得之新狀態的元件。</span><span class="sxs-lookup"><span data-stu-id="6cf63-109">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="6cf63-110">程式碼不會建立無限迴圈， `StateHasChanged`因為只有在是`infoFromJs` `null`時才會呼叫。</span><span class="sxs-lookup"><span data-stu-id="6cf63-110">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

```cshtml
@page "/prerendered-interop"
@using Microsoft.AspNetCore.Components
@using Microsoft.JSInterop
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