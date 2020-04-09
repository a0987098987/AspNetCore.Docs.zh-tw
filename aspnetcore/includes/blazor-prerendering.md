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
<span data-ttu-id="6883b-101">當Blazor伺服器應用處於預渲染狀態時,某些操作(如調用 JAvaScript)是不可能的,因為尚未與瀏覽器建立連接。</span><span class="sxs-lookup"><span data-stu-id="6883b-101">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="6883b-102">元件在預渲染時可能需要以不同的方式呈現。</span><span class="sxs-lookup"><span data-stu-id="6883b-102">Components may need to render differently when prerendered.</span></span>

<span data-ttu-id="6883b-103">要延遲 JavaScript 互通呼叫,直到建立與瀏覽器的連線後,可以使用 On[後 RenderAsync 元件生命週期事件](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="6883b-103">To delay JavaScript interop calls until after the connection with the browser is established, you can use the [OnAfterRenderAsync component lifecycle event](xref:blazor/lifecycle#after-component-render).</span></span> <span data-ttu-id="6883b-104">僅當應用程式完全呈現並建立用戶端連接後,才會調用此事件。</span><span class="sxs-lookup"><span data-stu-id="6883b-104">This event is only called after the app is fully rendered and the client connection is established.</span></span>

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

<span data-ttu-id="6883b-105">對於前面的範例代碼,在*wwwroot/index.html(WebAssembly)* 或Blazor*頁面/_Host.cshtml(*Blazor伺服器)`setElementText``<head>`的元素內提供 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="6883b-105">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="6883b-106">函數呼叫`IJSRuntime.InvokeVoidAsync`,不返回值:</span><span class="sxs-lookup"><span data-stu-id="6883b-106">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => element.innerText = text;
</script>
```

> [!WARNING]
> <span data-ttu-id="6883b-107">前面的範例直接修改文件物件模型 (DOM) 僅用於演示目的。</span><span class="sxs-lookup"><span data-stu-id="6883b-107">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="6883b-108">在大多數情況下,不建議使用 JAvaScript 直接修改 DOM,因為 JavaScript 可能會干擾Blazor更改跟蹤。</span><span class="sxs-lookup"><span data-stu-id="6883b-108">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>

<span data-ttu-id="6883b-109">以下元件演示如何以與預算相容的方式將 JAVAScript 互操作用作元件初始化邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="6883b-109">The following component demonstrates how to use JavaScript interop as part of a component's initialization logic in a way that's compatible with prerendering.</span></span> <span data-ttu-id="6883b-110">該元件顯示可以從內部`OnAfterRenderAsync`觸發呈現更新。</span><span class="sxs-lookup"><span data-stu-id="6883b-110">The component shows that it's possible to trigger a rendering update from inside `OnAfterRenderAsync`.</span></span> <span data-ttu-id="6883b-111">開發人員必須避免在此方案中創建無限迴圈。</span><span class="sxs-lookup"><span data-stu-id="6883b-111">The developer must avoid creating an infinite loop in this scenario.</span></span>

<span data-ttu-id="6883b-112">在`JSRuntime.InvokeAsync`調用的位置`ElementRef`, 僅在任何早期`OnAfterRenderAsync`生命週期 方法中使用,而不是在任何早期生命週期方法中使用,因為在呈現元件之前沒有 JAVAScript 元素。</span><span class="sxs-lookup"><span data-stu-id="6883b-112">Where `JSRuntime.InvokeAsync` is called, `ElementRef` is only used in `OnAfterRenderAsync` and not in any earlier lifecycle method because there's no JavaScript element until after the component is rendered.</span></span>

<span data-ttu-id="6883b-113">呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)以使用從 JavaScript 互通呼叫取得的新狀態重新呈現元件。</span><span class="sxs-lookup"><span data-stu-id="6883b-113">[StateHasChanged](xref:blazor/lifecycle#state-changes) is called to rerender the component with the new state obtained from the JavaScript interop call.</span></span> <span data-ttu-id="6883b-114">程式碼不會建立無限循環,`StateHasChanged`因為只有`infoFromJs`時`null`呼叫 。</span><span class="sxs-lookup"><span data-stu-id="6883b-114">The code doesn't create an infinite loop because `StateHasChanged` is only called when `infoFromJs` is `null`.</span></span>

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

<span data-ttu-id="6883b-115">對於前面的範例代碼,在*wwwroot/index.html(WebAssembly)* 或Blazor*頁面/_Host.cshtml(*Blazor伺服器)`setElementText``<head>`的元素內提供 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="6883b-115">For the preceding example code, provide a `setElementText` JavaScript function inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> <span data-ttu-id="6883b-116">函式呼叫`IJSRuntime.InvokeAsync`並傳回值:</span><span class="sxs-lookup"><span data-stu-id="6883b-116">The function is called with `IJSRuntime.InvokeAsync` and returns a value:</span></span>

```html
<script>
  window.setElementText = (element, text) => {
    element.innerText = text;
    return text;
  };
</script>
```

> [!WARNING]
> <span data-ttu-id="6883b-117">前面的範例直接修改文件物件模型 (DOM) 僅用於演示目的。</span><span class="sxs-lookup"><span data-stu-id="6883b-117">The preceding example modifies the Document Object Model (DOM) directly for demonstration purposes only.</span></span> <span data-ttu-id="6883b-118">在大多數情況下,不建議使用 JAvaScript 直接修改 DOM,因為 JavaScript 可能會干擾Blazor更改跟蹤。</span><span class="sxs-lookup"><span data-stu-id="6883b-118">Directly modifying the DOM with JavaScript isn't recommended in most scenarios because JavaScript can interfere with Blazor's change tracking.</span></span>
