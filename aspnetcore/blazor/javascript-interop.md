---
title: ASP.NET Core Blazor JavaScript interop
author: guardrex
description: 瞭解如何從 Blazor 應用程式中的 JavaScript，從 .NET 和 .NET 方法叫用 JavaScript 函式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: c4f2444b60fc2d3a8af893df379cf62636a7bdd5
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213359"
---
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a><span data-ttu-id="e129f-103">ASP.NET Core Blazor JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="e129f-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="e129f-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e129f-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e129f-105">Blazor 應用程式可以從 JavaScript 程式碼，叫用 .NET 和 .NET 方法的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="e129f-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="e129f-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e129f-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="e129f-107">從 .NET 方法叫用 JavaScript 函式</span><span class="sxs-lookup"><span data-stu-id="e129f-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="e129f-108">有時候需要 .NET 程式碼來呼叫 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="e129f-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="e129f-109">例如，JavaScript 呼叫可以將 JavaScript 程式庫中的瀏覽器功能或功能公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="e129f-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span> <span data-ttu-id="e129f-110">此案例稱為*JavaScript 互通性*（*JS interop*）。</span><span class="sxs-lookup"><span data-stu-id="e129f-110">This scenario is called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="e129f-111">若要從 .NET 呼叫 JavaScript，請使用 `IJSRuntime` 的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="e129f-111">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="e129f-112">若要發出 JS interop 呼叫，請在您的元件中插入 `IJSRuntime` 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="e129f-112">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="e129f-113">`InvokeAsync<T>` 方法會接受您想要叫用之 JavaScript 函數的識別碼，以及任何數目的 JSON 可序列化引數。</span><span class="sxs-lookup"><span data-stu-id="e129f-113">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="e129f-114">函數識別碼相對於全域範圍（`window`）。</span><span class="sxs-lookup"><span data-stu-id="e129f-114">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="e129f-115">如果您想要呼叫 `window.someScope.someFunction`，則會 `someScope.someFunction`識別碼。</span><span class="sxs-lookup"><span data-stu-id="e129f-115">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="e129f-116">在呼叫函式之前，不需要先註冊函式。</span><span class="sxs-lookup"><span data-stu-id="e129f-116">There's no need to register the function before it's called.</span></span> <span data-ttu-id="e129f-117">傳回類型 `T` 也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="e129f-117">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="e129f-118">`T` 應符合最佳對應至所傳回 JSON 類型的 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="e129f-118">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="e129f-119">若為已啟用可呈現的 Blazor 伺服器應用程式，在初始預入期間，不可能呼叫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="e129f-119">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="e129f-120">在建立與瀏覽器的連線之後，必須延後 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e129f-120">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="e129f-121">如需詳細資訊，請參閱偵測[何時將 Blazor 應用程式進行預呈現](#detect-when-a-blazor-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="e129f-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="e129f-122">下列範例是根據[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)，這是以實驗性 JavaScript 為基礎的解碼器。</span><span class="sxs-lookup"><span data-stu-id="e129f-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="e129f-123">此範例示範如何從C#方法叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e129f-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="e129f-124">JavaScript 函式會從C#方法接受位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="e129f-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="e129f-125">在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供使用 `TextDecoder` 來解碼傳遞陣列並傳回解碼值的 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="e129f-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="e129f-126">JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（ *.js*）載入，其中包含腳本檔案的參考：</span><span class="sxs-lookup"><span data-stu-id="e129f-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="e129f-127">下列元件：</span><span class="sxs-lookup"><span data-stu-id="e129f-127">The following component:</span></span>

* <span data-ttu-id="e129f-128">選取元件按鈕（**轉換陣列**）時，使用 `JSRuntime` 叫用 `convertArray` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e129f-128">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="e129f-129">呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="e129f-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="e129f-130">此字串會傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="e129f-130">The string is returned to the component for display.</span></span>

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a><span data-ttu-id="e129f-131">使用 IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="e129f-131">Use of IJSRuntime</span></span>

<span data-ttu-id="e129f-132">若要使用 `IJSRuntime` 抽象概念，請採用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="e129f-132">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="e129f-133">將 `IJSRuntime` 抽象層插入 Razor 元件（*razor*）：</span><span class="sxs-lookup"><span data-stu-id="e129f-133">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="e129f-134">在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供 `handleTickerChanged` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e129f-134">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="e129f-135">函式會使用 `IJSRuntime.InvokeVoidAsync` 來呼叫，而且不會傳回值：</span><span class="sxs-lookup"><span data-stu-id="e129f-135">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="e129f-136">將 `IJSRuntime` 抽象層插入至類別（ *.cs*）：</span><span class="sxs-lookup"><span data-stu-id="e129f-136">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="e129f-137">在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供 `handleTickerChanged` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e129f-137">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="e129f-138">呼叫函式時，會使用 `JSRuntime.InvokeAsync` 並傳回值：</span><span class="sxs-lookup"><span data-stu-id="e129f-138">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="e129f-139">針對使用[BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e129f-139">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="e129f-140">在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：</span><span class="sxs-lookup"><span data-stu-id="e129f-140">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="e129f-141">`showPrompt` &ndash; 會產生接受使用者輸入的提示（使用者的名稱），並將名稱傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="e129f-141">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="e129f-142">`displayWelcome` &ndash; 會將歡迎訊息從呼叫者指派給具有 `welcome``id` 的 DOM 物件。</span><span class="sxs-lookup"><span data-stu-id="e129f-142">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="e129f-143">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="e129f-143">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="e129f-144">將參考 JavaScript 檔案的 `<script>` 標記放在*wwwroot/index.html*檔案中（Blazor WebAssembly）或*Pages/_Host. cshtml*檔案（Blazor 伺服器）。</span><span class="sxs-lookup"><span data-stu-id="e129f-144">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="e129f-145">*wwwroot/index.html* （Blazor WebAssembly）：</span><span class="sxs-lookup"><span data-stu-id="e129f-145">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="e129f-146">*Pages/_Host. cshtml* （Blazor Server）：</span><span class="sxs-lookup"><span data-stu-id="e129f-146">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="e129f-147">請勿將 `<script>` 標記放在元件檔中，因為 `<script>` 標記無法動態更新。</span><span class="sxs-lookup"><span data-stu-id="e129f-147">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="e129f-148">.NET 方法會藉由呼叫 `IJSRuntime.InvokeAsync<T>`，與*exampleJsInterop*檔案中的 JavaScript 函式進行 interop。</span><span class="sxs-lookup"><span data-stu-id="e129f-148">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="e129f-149">`IJSRuntime` 抽象概念是非同步，可讓 Blazor 伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="e129f-149">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="e129f-150">如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請將轉換成 `IJSInProcessRuntime` 並改為呼叫 `Invoke<T>`。</span><span class="sxs-lookup"><span data-stu-id="e129f-150">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="e129f-151">我們建議大部分的 JS interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="e129f-151">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="e129f-152">範例應用程式包含一個可示範 JS interop 的元件。</span><span class="sxs-lookup"><span data-stu-id="e129f-152">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="e129f-153">元件：</span><span class="sxs-lookup"><span data-stu-id="e129f-153">The component:</span></span>

* <span data-ttu-id="e129f-154">透過 JavaScript 提示接收使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="e129f-154">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="e129f-155">將文字傳回到元件以進行處理。</span><span class="sxs-lookup"><span data-stu-id="e129f-155">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="e129f-156">呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="e129f-156">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="e129f-157">*Pages/JSInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="e129f-157">*Pages/JSInterop.razor*:</span></span>

```razor
@page "/JSInterop"
@using BlazorSample.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        // showPrompt is implemented in wwwroot/exampleJsInterop.js
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");
        // displayWelcome is implemented in wwwroot/exampleJsInterop.js
        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="e129f-158">當您選取元件的 [**觸發程式 JavaScript 提示**] 按鈕來執行 `TriggerJsPrompt` 時，會呼叫*wwwroot/exampleJsInterop*檔案中提供的 JavaScript `showPrompt` 函數。</span><span class="sxs-lookup"><span data-stu-id="e129f-158">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="e129f-159">`showPrompt` 函式會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。</span><span class="sxs-lookup"><span data-stu-id="e129f-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="e129f-160">元件會將使用者的名稱儲存在本機變數中，`name`。</span><span class="sxs-lookup"><span data-stu-id="e129f-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="e129f-161">儲存在 `name` 中的字串會合並到歡迎訊息中，該訊息會傳遞至 JavaScript 函式 `displayWelcome`，這會將歡迎訊息轉譯為標題標記。</span><span class="sxs-lookup"><span data-stu-id="e129f-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="e129f-162">呼叫 void JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="e129f-162">Call a void JavaScript function</span></span>

<span data-ttu-id="e129f-163">會使用 `IJSRuntime.InvokeVoidAsync`來呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="e129f-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a><span data-ttu-id="e129f-164">偵測 Blazor 應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="e129f-164">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="e129f-165">捕捉元素的參考</span><span class="sxs-lookup"><span data-stu-id="e129f-165">Capture references to elements</span></span>

<span data-ttu-id="e129f-166">某些 JS interop 案例需要 HTML 元素的參考。</span><span class="sxs-lookup"><span data-stu-id="e129f-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="e129f-167">例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在元素上呼叫類似命令的 Api，例如 `focus` 或 `play`。</span><span class="sxs-lookup"><span data-stu-id="e129f-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="e129f-168">使用下列方法來抓取元件中的 HTML 元素參考：</span><span class="sxs-lookup"><span data-stu-id="e129f-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="e129f-169">將 `@ref` 屬性加入至 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="e129f-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="e129f-170">定義 `ElementReference` 類型的欄位，其名稱符合 `@ref` 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="e129f-170">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="e129f-171">下列範例會示範如何捕捉 `username` `<input>` 元素的參考：</span><span class="sxs-lookup"><span data-stu-id="e129f-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="e129f-172">只使用專案參考來改變不會與 Blazor互動之空白專案的內容。</span><span class="sxs-lookup"><span data-stu-id="e129f-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="e129f-173">當協力廠商 API 提供內容給元素時，此案例很有用。</span><span class="sxs-lookup"><span data-stu-id="e129f-173">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="e129f-174">因為 Blazor 不會與專案互動，所以 Blazor的元素和 DOM 的標記法之間不可能發生衝突。</span><span class="sxs-lookup"><span data-stu-id="e129f-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="e129f-175">在下列範例中，改變未排序清單（`ul`）的內容很*危險*，因為 Blazor 與 DOM 互動以填入此元素的清單專案（`<li>`）：</span><span class="sxs-lookup"><span data-stu-id="e129f-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="e129f-176">如果 JS interop 變動此元素 `MyList` 的內容，而 Blazor 嘗試將差異套用至專案，則差異不會符合 DOM。</span><span class="sxs-lookup"><span data-stu-id="e129f-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="e129f-177">就 .NET 程式碼而言，`ElementReference` 是不透明的控制碼。</span><span class="sxs-lookup"><span data-stu-id="e129f-177">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="e129f-178">`ElementReference` 的*唯一*做法是透過 JS interop 將它傳遞至 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="e129f-178">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="e129f-179">當您這麼做時，JavaScript 端程式碼會接收 `HTMLElement` 實例，它可以搭配一般的 DOM Api 來使用。</span><span class="sxs-lookup"><span data-stu-id="e129f-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="e129f-180">例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：</span><span class="sxs-lookup"><span data-stu-id="e129f-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="e129f-181">*exampleJsInterop .js*：</span><span class="sxs-lookup"><span data-stu-id="e129f-181">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="e129f-182">若要呼叫不會傳回值的 JavaScript 函數，請使用 `IJSRuntime.InvokeVoidAsync`。</span><span class="sxs-lookup"><span data-stu-id="e129f-182">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="e129f-183">下列程式碼會使用已捕捉的 `ElementReference`來呼叫前面的 JavaScript 函式，以將焦點放在使用者名稱輸入上：</span><span class="sxs-lookup"><span data-stu-id="e129f-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="e129f-184">若要使用擴充方法，請建立會接收 `IJSRuntime` 實例的靜態擴充方法：</span><span class="sxs-lookup"><span data-stu-id="e129f-184">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="e129f-185">`Focus` 方法是直接在物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="e129f-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="e129f-186">下列範例假設 `Focus` 方法可從 `JsInteropClasses` 命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="e129f-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="e129f-187">只有在呈現元件之後，才會填入 `username` 變數。</span><span class="sxs-lookup"><span data-stu-id="e129f-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="e129f-188">如果擴展 `ElementReference` 傳遞至 JavaScript 程式碼，JavaScript 程式碼就會收到 `null`的值。</span><span class="sxs-lookup"><span data-stu-id="e129f-188">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="e129f-189">若要在元件完成轉譯之後操作元素參考（以設定元素的初始焦點），請使用[OnAfterRenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="e129f-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="e129f-190">當使用泛型型別並傳回值時，請使用[ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1)：</span><span class="sxs-lookup"><span data-stu-id="e129f-190">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="e129f-191">`GenericMethod` 是直接在具有類型的物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="e129f-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="e129f-192">下列範例假設 `GenericMethod` 可從 `JsInteropClasses` 命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="e129f-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="e129f-193">跨元件的參考元素</span><span class="sxs-lookup"><span data-stu-id="e129f-193">Reference elements across components</span></span>

<span data-ttu-id="e129f-194">`ElementReference` 只有在元件的 `OnAfterRender` 方法中才有效（而元素參考是 `struct`），因此無法在元件之間傳遞專案參考。</span><span class="sxs-lookup"><span data-stu-id="e129f-194">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="e129f-195">若要讓父系元件能夠將專案參考提供給其他元件，父系元件可以：</span><span class="sxs-lookup"><span data-stu-id="e129f-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="e129f-196">允許子元件註冊回呼。</span><span class="sxs-lookup"><span data-stu-id="e129f-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="e129f-197">使用傳遞的元素參考，在 `OnAfterRender` 事件期間叫用已註冊的回呼。</span><span class="sxs-lookup"><span data-stu-id="e129f-197">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="e129f-198">這個方法會間接讓子元件與父系的元素參考互動。</span><span class="sxs-lookup"><span data-stu-id="e129f-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="e129f-199">下列 Blazor WebAssembly 範例說明方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="e129f-200">在*wwwroot/index.html*的 `<head>` 中：</span><span class="sxs-lookup"><span data-stu-id="e129f-200">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="e129f-201">在*wwwroot/index.html*的 `<body>` 中：</span><span class="sxs-lookup"><span data-stu-id="e129f-201">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="e129f-202">*Pages/Index. razor* （父系元件）：</span><span class="sxs-lookup"><span data-stu-id="e129f-202">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="e129f-203">*Pages/Index. .cs*：</span><span class="sxs-lookup"><span data-stu-id="e129f-203">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool _disposing;
        private IList<IObserver<ElementReference>> _subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference _title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnNext(_title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            _disposing = true;

            foreach (var subscription in _subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            _subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (_disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            _subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self._subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="e129f-204">*Shared/SurveyPrompt. razor* （子元件）：</span><span class="sxs-lookup"><span data-stu-id="e129f-204">*Shared/SurveyPrompt.razor* (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="e129f-205">*Shared/SurveyPrompt. .cs*：</span><span class="sxs-lookup"><span data-stu-id="e129f-205">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable _subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (_subscription != null)
            {
                _subscription.Dispose();
            }

            _subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            _subscription = null;
        }

        public void OnError(Exception error)
        {
            _subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            _subscription?.Dispose();
        }
    }
}
```

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="e129f-206">從 JavaScript 函式呼叫 .NET 方法</span><span class="sxs-lookup"><span data-stu-id="e129f-206">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="e129f-207">靜態 .NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="e129f-207">Static .NET method call</span></span>

<span data-ttu-id="e129f-208">若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。</span><span class="sxs-lookup"><span data-stu-id="e129f-208">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="e129f-209">傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。</span><span class="sxs-lookup"><span data-stu-id="e129f-209">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="e129f-210">最好是非同步版本，以支援 Blazor 伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="e129f-210">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="e129f-211">.NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e129f-211">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="e129f-212">目前不支援呼叫開放式泛型方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-212">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="e129f-213">範例應用程式包含C#方法，以傳回 `int`的陣列。</span><span class="sxs-lookup"><span data-stu-id="e129f-213">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="e129f-214">`JSInvokable` 屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-214">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="e129f-215">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="e129f-215">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary"
        onclick="exampleJsFunctions.returnArrayAsyncJs()">
    Trigger .NET static method ReturnArrayAsync
</button>

@code {
    [JSInvokable]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="e129f-216">提供給用戶端的 JavaScript 會C#叫用 .net 方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-216">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="e129f-217">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="e129f-217">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="e129f-218">選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="e129f-218">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="e129f-219">主控台輸出如下：</span><span class="sxs-lookup"><span data-stu-id="e129f-219">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="e129f-220">第四個數組值會推送至 `ReturnArrayAsync`所傳回的陣列（`data.push(4);`）。</span><span class="sxs-lookup"><span data-stu-id="e129f-220">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="e129f-221">根據預設，方法識別碼是方法名稱，但您可以使用 `JSInvokableAttribute` 的函式來指定不同的識別碼：</span><span class="sxs-lookup"><span data-stu-id="e129f-221">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="e129f-222">在用戶端 JavaScript 檔案中：</span><span class="sxs-lookup"><span data-stu-id="e129f-222">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a><span data-ttu-id="e129f-223">實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="e129f-223">Instance method call</span></span>

<span data-ttu-id="e129f-224">您也可以從 JavaScript 呼叫 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-224">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="e129f-225">若要從 JavaScript 叫用 .NET 實例方法：</span><span class="sxs-lookup"><span data-stu-id="e129f-225">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="e129f-226">將 .NET 實例包裝在 `DotNetObjectReference` 實例中，以將其傳遞給 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="e129f-226">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="e129f-227">.NET 實例會以傳址方式傳遞至 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="e129f-227">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="e129f-228">使用 `invokeMethod` 或 `invokeMethodAsync` 函數，在實例上叫用 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-228">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="e129f-229">從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="e129f-229">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="e129f-230">範例應用程式會將訊息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="e129f-230">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="e129f-231">如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="e129f-231">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="e129f-232">選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時，會呼叫 `ExampleJsInterop.CallHelloHelperSayHello`，並將名稱 `Blazor`傳遞至方法。</span><span class="sxs-lookup"><span data-stu-id="e129f-232">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="e129f-233">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="e129f-233">*Pages/JsInterop.razor*:</span></span>

```razor
<button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
    Trigger .NET instance method HelloHelper.SayHello
</button>

@code {
    public async Task TriggerNetInstanceMethod()
    {
        var exampleJsInterop = new ExampleJsInterop(JSRuntime);
        await exampleJsInterop.CallHelloHelperSayHello("Blazor");
    }
}
```

<span data-ttu-id="e129f-234">`CallHelloHelperSayHello` 會使用 `HelloHelper`的新實例，叫用 JavaScript 函數 `sayHello`。</span><span class="sxs-lookup"><span data-stu-id="e129f-234">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="e129f-235">*JsInteropClasses/ExampleJsInterop .cs*：</span><span class="sxs-lookup"><span data-stu-id="e129f-235">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="e129f-236">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="e129f-236">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="e129f-237">名稱會傳遞至 `HelloHelper`的函式，以設定 `HelloHelper.Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e129f-237">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="e129f-238">執行 JavaScript 函式 `sayHello` 時，`HelloHelper.SayHello` 會傳回 `Hello, {Name}!` 訊息，JavaScript 函式會將它寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="e129f-238">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="e129f-239">*JsInteropClasses/HelloHelper .cs*：</span><span class="sxs-lookup"><span data-stu-id="e129f-239">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="e129f-240">瀏覽器 網頁程式開發人員工具中的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="e129f-240">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="e129f-241">共用類別庫中的 interop 程式碼</span><span class="sxs-lookup"><span data-stu-id="e129f-241">Share interop code in a class library</span></span>

<span data-ttu-id="e129f-242">JS interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。</span><span class="sxs-lookup"><span data-stu-id="e129f-242">JS interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="e129f-243">類別庫會處理在建立的元件中內嵌 JavaScript 資源。</span><span class="sxs-lookup"><span data-stu-id="e129f-243">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="e129f-244">JavaScript 檔案會放在*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e129f-244">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="e129f-245">工具會在建立程式庫時，負責內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="e129f-245">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="e129f-246">在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。</span><span class="sxs-lookup"><span data-stu-id="e129f-246">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="e129f-247">還原套件之後，應用程式程式碼可以呼叫 JavaScript，就像是C#一樣。</span><span class="sxs-lookup"><span data-stu-id="e129f-247">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="e129f-248">如需詳細資訊，請參閱 <xref:blazor/class-libraries>。</span><span class="sxs-lookup"><span data-stu-id="e129f-248">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="e129f-249">強化 JS interop 呼叫</span><span class="sxs-lookup"><span data-stu-id="e129f-249">Harden JS interop calls</span></span>

<span data-ttu-id="e129f-250">JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。</span><span class="sxs-lookup"><span data-stu-id="e129f-250">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="e129f-251">根據預設，Blazor Server 應用程式會在一分鐘後，在伺服器上執行的 JS interop 呼叫數次。</span><span class="sxs-lookup"><span data-stu-id="e129f-251">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="e129f-252">如果應用程式可以容忍較積極的超時（例如10秒），請使用下列其中一種方法來設定超時時間：</span><span class="sxs-lookup"><span data-stu-id="e129f-252">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="e129f-253">全域在 `Startup.ConfigureServices`中，指定 [超時]：</span><span class="sxs-lookup"><span data-stu-id="e129f-253">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="e129f-254">在元件程式碼中的每個調用，單一呼叫可以指定 timeout：</span><span class="sxs-lookup"><span data-stu-id="e129f-254">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="e129f-255">如需資源耗盡的詳細資訊，請參閱 <xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="e129f-255">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="e129f-256">在 Blazor 伺服器應用程式中執行大型資料傳輸</span><span class="sxs-lookup"><span data-stu-id="e129f-256">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="e129f-257">在某些情況下，必須在 JavaScript 和 Blazor之間傳輸大量資料。</span><span class="sxs-lookup"><span data-stu-id="e129f-257">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="e129f-258">通常會在下列情況進行大型資料傳輸：</span><span class="sxs-lookup"><span data-stu-id="e129f-258">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="e129f-259">瀏覽器檔案系統 Api 可用來上傳或下載檔案。</span><span class="sxs-lookup"><span data-stu-id="e129f-259">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="e129f-260">需要具有協力廠商程式庫的互通性。</span><span class="sxs-lookup"><span data-stu-id="e129f-260">Interop with a third party library is required.</span></span>

<span data-ttu-id="e129f-261">在 Blazor 伺服器中，有一項限制是為了避免傳遞可能會導致效能問題的單一大型訊息。</span><span class="sxs-lookup"><span data-stu-id="e129f-261">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="e129f-262">開發在 JavaScript 和 Blazor之間傳輸資料的程式碼時，請考慮下列指導方針：</span><span class="sxs-lookup"><span data-stu-id="e129f-262">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="e129f-263">將資料分割成較小的片段，並依序傳送資料區段，直到伺服器收到所有資料為止。</span><span class="sxs-lookup"><span data-stu-id="e129f-263">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="e129f-264">不要以 JavaScript 和C#程式碼配置大型物件。</span><span class="sxs-lookup"><span data-stu-id="e129f-264">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="e129f-265">傳送或接收資料時，不要長時間封鎖主要 UI 執行緒。</span><span class="sxs-lookup"><span data-stu-id="e129f-265">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="e129f-266">釋放處理常式完成或取消時所耗用的任何記憶體。</span><span class="sxs-lookup"><span data-stu-id="e129f-266">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="e129f-267">基於安全性目的，強制執行下列額外的需求：</span><span class="sxs-lookup"><span data-stu-id="e129f-267">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="e129f-268">宣告可傳遞的檔案或資料大小上限。</span><span class="sxs-lookup"><span data-stu-id="e129f-268">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="e129f-269">宣告從用戶端到伺服器的最小上傳速率。</span><span class="sxs-lookup"><span data-stu-id="e129f-269">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="e129f-270">伺服器收到資料之後，資料可以是：</span><span class="sxs-lookup"><span data-stu-id="e129f-270">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="e129f-271">暫時儲存在記憶體緩衝區中，直到收集所有區段為止。</span><span class="sxs-lookup"><span data-stu-id="e129f-271">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="e129f-272">立即使用。</span><span class="sxs-lookup"><span data-stu-id="e129f-272">Consumed immediately.</span></span> <span data-ttu-id="e129f-273">例如，資料可以立即儲存在資料庫中，或在每個區段收到時寫入磁片。</span><span class="sxs-lookup"><span data-stu-id="e129f-273">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="e129f-274">下列檔案上傳者類別會處理用戶端的 JS interop。</span><span class="sxs-lookup"><span data-stu-id="e129f-274">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="e129f-275">上載者類別會使用 JS interop 來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="e129f-275">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="e129f-276">輪詢用戶端以傳送資料區段。</span><span class="sxs-lookup"><span data-stu-id="e129f-276">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="e129f-277">如果輪詢超時，則中止交易。</span><span class="sxs-lookup"><span data-stu-id="e129f-277">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="e129f-278">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="e129f-278">In the preceding example:</span></span>

* <span data-ttu-id="e129f-279">`_maxBase64SegmentSize` 設定為 `8192`，這是從 `_maxBase64SegmentSize = _segmentSize * 4 / 3`計算而來。</span><span class="sxs-lookup"><span data-stu-id="e129f-279">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="e129f-280">低層級的 .NET Core 記憶體管理 Api 用來將伺服器上的記憶體區段儲存在 `_uploadedSegments`中。</span><span class="sxs-lookup"><span data-stu-id="e129f-280">Low level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="e129f-281">`ReceiveFile` 方法是用來處理透過 JS interop 的上傳：</span><span class="sxs-lookup"><span data-stu-id="e129f-281">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="e129f-282">檔案大小是以位元組為單位，透過 JS interop 與 `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`來決定。</span><span class="sxs-lookup"><span data-stu-id="e129f-282">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="e129f-283">要接收的區段數目會計算並儲存在 `numberOfSegments`中。</span><span class="sxs-lookup"><span data-stu-id="e129f-283">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="e129f-284">在 `for` 迴圈中，會透過具有 `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`的 JS interop 來要求區段。</span><span class="sxs-lookup"><span data-stu-id="e129f-284">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="e129f-285">在解碼之前，所有區段（但最後一個）必須是8192個位元組。</span><span class="sxs-lookup"><span data-stu-id="e129f-285">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="e129f-286">系統會強制用戶端以有效率的方式傳送資料。</span><span class="sxs-lookup"><span data-stu-id="e129f-286">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="e129f-287">針對每個收到的區段，會先執行檢查，再使用 <xref:System.Convert.TryFromBase64String*>進行解碼。</span><span class="sxs-lookup"><span data-stu-id="e129f-287">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="e129f-288">當上傳完成之後，會以新的 <xref:System.IO.Stream> （`SegmentedStream`）傳回資料的資料流程。</span><span class="sxs-lookup"><span data-stu-id="e129f-288">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="e129f-289">分段資料流程類別會將區段清單公開為 readonly 不可搜尋的 <xref:System.IO.Stream>：</span><span class="sxs-lookup"><span data-stu-id="e129f-289">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="e129f-290">下列程式碼會實行 JavaScript 函式來接收資料：</span><span class="sxs-lookup"><span data-stu-id="e129f-290">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```

## <a name="additional-resources"></a><span data-ttu-id="e129f-291">其他資源</span><span class="sxs-lookup"><span data-stu-id="e129f-291">Additional resources</span></span>

* [<span data-ttu-id="e129f-292">InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）</span><span class="sxs-lookup"><span data-stu-id="e129f-292">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
