<span data-ttu-id="0dbef-101">當 Blazor 伺服器應用程式進行預先處理時，某些動作（例如呼叫 JavaScript）並不可能，因為尚未建立與瀏覽器的連接。</span><span class="sxs-lookup"><span data-stu-id="0dbef-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="0dbef-102">元件可能需要在資源清單時以不同的方式呈現。</span><span class="sxs-lookup"><span data-stu-id="0dbef-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="0dbef-103">若要延遲 JavaScript interop 呼叫，直到建立與瀏覽器的連線之後，您可以使用 `OnAfterRenderAsync` 元件生命週期事件。</span><span class="sxs-lookup"><span data-stu-id="0dbef-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the `OnAfterRenderAsync` component lifecycle event.</span></span> <span data-ttu-id="0dbef-104">只有在完整呈現應用程式並建立用戶端連線之後，才會呼叫此事件。</span><span class="sxs-lookup"><span data-stu-id="0dbef-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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

<span data-ttu-id="0dbef-105">如需上述範例程式碼，請在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor 伺服器）的 `<head>` 元素內提供 `setElementText` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="0dbef-105">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="0dbef-106">函式會使用 `IJSRuntime.InvokeVoidAsync` 來呼叫，而且不會傳回值：</span><span class="sxs-lookup"><span data-stu-id="0dbef-106">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

```html
<!--  -->
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> <span data-ttu-id="0dbef-107">上述範例只會修改檔物件模型（DOM），僅供示範之用。</span><span class="sxs-lookup"><span data-stu-id="0dbef-107">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="0dbef-108">在大部分的情況下，不建議直接修改具有 JavaScript 的 DOM，因為 JavaScript 可能會干擾 Blazor 的變更追蹤。</span><span class="sxs-lookup"><span data-stu-id="0dbef-108">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>

<span data-ttu-id="0dbef-109">下列元件示範如何使用 JavaScript interop 做為元件初始化邏輯的一部分，使其與可處理性相容。</span><span class="sxs-lookup"><span data-stu-id="0dbef-109">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="0dbef-110">此元件顯示可以從 `OnAfterRenderAsync` 內觸發轉譯更新。</span><span class="sxs-lookup"><span data-stu-id="0dbef-110">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="0dbef-111">開發人員必須避免在此案例中建立無限迴圈。</span><span class="sxs-lookup"><span data-stu-id="0dbef-111">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="0dbef-112">呼叫 `JSRuntime.InvokeAsync` 的位置時，`ElementRef` 只會用於 `OnAfterRenderAsync` 中，而不會用於任何較早的生命週期方法，因為在轉譯元件之前，沒有 JavaScript 元素。</span><span class="sxs-lookup"><span data-stu-id="0dbef-112">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="0dbef-113">呼叫 `StateHasChanged`，以從 JavaScript interop 呼叫取得新狀態來 rerender 元件。</span><span class="sxs-lookup"><span data-stu-id="0dbef-113">`StateHasChanged` is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="0dbef-114">程式碼不會建立無限迴圈，因為只有在 `infoFromJs` `null` 時，才會呼叫 `StateHasChanged`。</span><span class="sxs-lookup"><span data-stu-id="0dbef-114">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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

<span data-ttu-id="0dbef-115">如需上述範例程式碼，請在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor 伺服器）的 `<head>` 元素內提供 `setElementText` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="0dbef-115">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="0dbef-116">呼叫函式時，會使用 `IJSRuntime.InvokeAsync` 並傳回值：</span><span class="sxs-lookup"><span data-stu-id="0dbef-116">The function is called with `IJSRuntime.InvokeAsync` and returns a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> <span data-ttu-id="0dbef-117">上述範例只會修改檔物件模型（DOM），僅供示範之用。</span><span class="sxs-lookup"><span data-stu-id="0dbef-117">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="0dbef-118">在大部分的情況下，不建議直接修改具有 JavaScript 的 DOM，因為 JavaScript 可能會干擾 Blazor 的變更追蹤。</span><span class="sxs-lookup"><span data-stu-id="0dbef-118">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>
