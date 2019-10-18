---
title: ASP.NET Core Blazor JavaScript interop
author: guardrex
description: 瞭解如何從 Blazor apps 中的 JavaScript，從 .NET 和 .NET 方法叫用 JavaScript 函式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/javascript-interop
ms.openlocfilehash: a8c3a0951761faab1c11507834aeef2507388d71
ms.sourcegitcommit: ce2bfb01f2cc7dd83f8a97da0689d232c71bcdc4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72531136"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="35522-103">ASP.NET Core Blazor JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="35522-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="35522-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="35522-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="35522-105">Blazor 應用程式可以從 JavaScript 程式碼，叫用 .NET 和 .NET 方法的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="35522-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="35522-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35522-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="35522-107">從 .NET 方法叫用 JavaScript 函式</span><span class="sxs-lookup"><span data-stu-id="35522-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="35522-108">有時候需要 .NET 程式碼來呼叫 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="35522-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="35522-109">例如，JavaScript 呼叫可以將 JavaScript 程式庫中的瀏覽器功能或功能公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="35522-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="35522-110">若要從 .NET 呼叫 JavaScript，請使用 `IJSRuntime` 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="35522-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="35522-111">@No__t-0 方法會接受您想要叫用之 JavaScript 函數的識別碼，以及任何數目的 JSON 可序列化引數。</span><span class="sxs-lookup"><span data-stu-id="35522-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="35522-112">函數識別碼相對於全域範圍（`window`）。</span><span class="sxs-lookup"><span data-stu-id="35522-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="35522-113">如果您想要呼叫 `window.someScope.someFunction`，識別碼會 `someScope.someFunction`。</span><span class="sxs-lookup"><span data-stu-id="35522-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="35522-114">在呼叫函式之前，不需要先註冊函式。</span><span class="sxs-lookup"><span data-stu-id="35522-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="35522-115">傳回類型 `T` 也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="35522-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="35522-116">針對 Blazor 伺服器應用程式：</span><span class="sxs-lookup"><span data-stu-id="35522-116">For Blazor Server apps:</span></span>

* <span data-ttu-id="35522-117">Blazor 伺服器應用程式會處理多個使用者要求。</span><span class="sxs-lookup"><span data-stu-id="35522-117">Multiple user requests are processed by the Blazor Server app.</span></span> <span data-ttu-id="35522-118">請勿在元件中呼叫 `JSRuntime.Current`，以叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="35522-119">插入 `IJSRuntime` 抽象概念，並使用插入的物件發出 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="35522-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="35522-120">當 Blazor 應用程式預先呈現時，無法呼叫 JavaScript，因為尚未建立與瀏覽器的連接。</span><span class="sxs-lookup"><span data-stu-id="35522-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="35522-121">如需詳細資訊，請參閱偵測[Blazor 應用程式何時進行預呈現](#detect-when-a-blazor-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="35522-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="35522-122">下列範例是根據[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)，這是以實驗性 JavaScript 為基礎的解碼器。</span><span class="sxs-lookup"><span data-stu-id="35522-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="35522-123">此範例示範如何從C#方法叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="35522-124">JavaScript 函式會從C#方法接受位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="35522-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="35522-125">在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供使用 `TextDecoder` 來解碼傳遞陣列的函式：</span><span class="sxs-lookup"><span data-stu-id="35522-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="35522-126">JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（ *.js*）載入，其中包含腳本檔案的參考：</span><span class="sxs-lookup"><span data-stu-id="35522-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="35522-127">下列元件：</span><span class="sxs-lookup"><span data-stu-id="35522-127">The following component:</span></span>

* <span data-ttu-id="35522-128">選取元件按鈕（**轉換陣列**）時，使用 `JsRuntime` 叫用 @no__t 0 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="35522-129">呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="35522-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="35522-130">此字串會傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="35522-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="35522-131">若要使用 `IJSRuntime` 抽象概念，請採用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="35522-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="35522-132">將 `IJSRuntime` 抽象概念插入 Razor 元件（*razor*）：</span><span class="sxs-lookup"><span data-stu-id="35522-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="35522-133">將 `IJSRuntime` 抽象概念插入類別（ *.cs*）：</span><span class="sxs-lookup"><span data-stu-id="35522-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="35522-134">針對使用[BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="35522-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="35522-135">在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：</span><span class="sxs-lookup"><span data-stu-id="35522-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="35522-136">`showPrompt` &ndash; 會產生接受使用者輸入的提示（使用者的名稱），並將名稱傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="35522-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="35522-137">`displayWelcome` &ndash; 會將歡迎訊息從呼叫者指派給具有 `id` of `welcome` 的 DOM 物件。</span><span class="sxs-lookup"><span data-stu-id="35522-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="35522-138">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="35522-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="35522-139">將參考 JavaScript 檔案的 `<script>` 標記放在*wwwroot/index.html*檔案（Blazor WebAssembly）或*Pages/_Host. Cshtml*檔案（Blazor 伺服器）中。</span><span class="sxs-lookup"><span data-stu-id="35522-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="35522-140">*wwwroot/index.html* （Blazor WebAssembly）：</span><span class="sxs-lookup"><span data-stu-id="35522-140">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="35522-141">*Pages/_Host. cshtml* （Blazor Server）：</span><span class="sxs-lookup"><span data-stu-id="35522-141">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

<span data-ttu-id="35522-142">請勿將 `<script>` 標記放在元件檔中，因為 @no__t 1 標記無法動態更新。</span><span class="sxs-lookup"><span data-stu-id="35522-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="35522-143">.NET 方法會藉由呼叫 `IJSRuntime.InvokeAsync<T>`，與*exampleJsInterop*檔案中的 JavaScript 函數互通。</span><span class="sxs-lookup"><span data-stu-id="35522-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="35522-144">@No__t-0 抽象概念是非同步，允許 Blazor 伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="35522-144">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="35522-145">如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請將轉換為 `IJSInProcessRuntime`，並改為呼叫 `Invoke<T>`。</span><span class="sxs-lookup"><span data-stu-id="35522-145">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="35522-146">我們建議大部分的 JavaScript interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="35522-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="35522-147">範例應用程式包含一個可示範 JavaScript interop 的元件。</span><span class="sxs-lookup"><span data-stu-id="35522-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="35522-148">元件：</span><span class="sxs-lookup"><span data-stu-id="35522-148">The component:</span></span>

* <span data-ttu-id="35522-149">透過 JavaScript 提示接收使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="35522-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="35522-150">將文字傳回到元件以進行處理。</span><span class="sxs-lookup"><span data-stu-id="35522-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="35522-151">呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="35522-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="35522-152">*Pages/JSInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="35522-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="35522-153">當 `TriggerJsPrompt` 是藉由選取元件的 [**觸發程式 JavaScript 提示**] 按鈕來執行時，會呼叫*wwwroot/exampleJsInterop*檔案中提供的 JavaScript `showPrompt` 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="35522-154">@No__t-0 函式會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。</span><span class="sxs-lookup"><span data-stu-id="35522-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="35522-155">元件會將使用者的名稱儲存在本機變數中，`name`。</span><span class="sxs-lookup"><span data-stu-id="35522-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="35522-156">儲存在 `name` 中的字串會合並到歡迎訊息中，該訊息會傳遞至 JavaScript 函式 `displayWelcome`，這會將歡迎訊息轉譯為標題標記。</span><span class="sxs-lookup"><span data-stu-id="35522-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="35522-157">呼叫 void JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="35522-157">Call a void JavaScript function</span></span>

<span data-ttu-id="35522-158">會使用 `IJSRuntime.InvokeVoidAsync` 呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="35522-159">偵測 Blazor 應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="35522-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="35522-160">捕捉元素的參考</span><span class="sxs-lookup"><span data-stu-id="35522-160">Capture references to elements</span></span>

<span data-ttu-id="35522-161">某些[JavaScript interop](xref:blazor/javascript-interop)案例需要 HTML 元素的參考。</span><span class="sxs-lookup"><span data-stu-id="35522-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="35522-162">例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在元素上呼叫類似命令的 Api，例如 `focus` 或 `play`。</span><span class="sxs-lookup"><span data-stu-id="35522-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="35522-163">使用下列方法來抓取元件中的 HTML 元素參考：</span><span class="sxs-lookup"><span data-stu-id="35522-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="35522-164">將 `@ref` 屬性加入至 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="35522-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="35522-165">定義 `ElementReference` 類型的欄位，其名稱符合 @no__t 1 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="35522-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="35522-166">下列範例顯示如何捕捉 `username` `<input>` 元素的參考：</span><span class="sxs-lookup"><span data-stu-id="35522-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="35522-167">請勿**使用已**捕捉的元素參考做為擴展 DOM 的方式。</span><span class="sxs-lookup"><span data-stu-id="35522-167">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="35522-168">這麼做可能會干擾宣告式轉譯模型。</span><span class="sxs-lookup"><span data-stu-id="35522-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="35522-169">就 .NET 程式碼而言，`ElementReference` 是不透明的控制碼。</span><span class="sxs-lookup"><span data-stu-id="35522-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="35522-170">@No__t-1 的*唯一*做法是透過 javascript interop 將它傳遞至 javascript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="35522-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="35522-171">當您這麼做時，JavaScript 端程式碼會收到 `HTMLElement` 實例，它可以搭配一般的 DOM Api 來使用。</span><span class="sxs-lookup"><span data-stu-id="35522-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="35522-172">例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：</span><span class="sxs-lookup"><span data-stu-id="35522-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="35522-173">*exampleJsInterop .js*：</span><span class="sxs-lookup"><span data-stu-id="35522-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="35522-174">使用 `IJSRuntime.InvokeAsync<T>`，並以 `ElementReference` 呼叫 `exampleJsFunctions.focusElement`，以將焦點放在元素：</span><span class="sxs-lookup"><span data-stu-id="35522-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="35522-175">若要使用擴充方法將焦點放在元素，請建立會接收 `IJSRuntime` 實例的靜態擴充方法：</span><span class="sxs-lookup"><span data-stu-id="35522-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="35522-176">方法是直接在物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="35522-176">The method is called directly on the object.</span></span> <span data-ttu-id="35522-177">下列範例假設 `JsInteropClasses` 命名空間中提供了靜態 `Focus` 方法：</span><span class="sxs-lookup"><span data-stu-id="35522-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="35522-178">只有在呈現元件之後，才會填入 `username` 變數。</span><span class="sxs-lookup"><span data-stu-id="35522-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="35522-179">如果擴展 `ElementReference` 傳遞至 JavaScript 程式碼，JavaScript 程式碼就會收到 `null` 的值。</span><span class="sxs-lookup"><span data-stu-id="35522-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="35522-180">若要在元件完成轉譯之後操作元素參考（以設定專案的初始焦點），請使用 `OnAfterRenderAsync` 或 @no__t 1[元件生命週期方法](xref:blazor/components#lifecycle-methods)。</span><span class="sxs-lookup"><span data-stu-id="35522-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="35522-181">從 JavaScript 函式呼叫 .NET 方法</span><span class="sxs-lookup"><span data-stu-id="35522-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="35522-182">靜態 .NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="35522-182">Static .NET method call</span></span>

<span data-ttu-id="35522-183">若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="35522-184">傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。</span><span class="sxs-lookup"><span data-stu-id="35522-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="35522-185">最好是非同步版本來支援 Blazor 伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="35522-185">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="35522-186">若要從 JavaScript 叫用 .NET 方法，.NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="35522-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="35522-187">根據預設，方法識別碼是方法名稱，但您可以使用 `JSInvokableAttribute` 的函式來指定不同的識別碼。</span><span class="sxs-lookup"><span data-stu-id="35522-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="35522-188">目前不支援呼叫開放式泛型方法。</span><span class="sxs-lookup"><span data-stu-id="35522-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="35522-189">範例應用程式包含傳回C# `int` 陣列的方法。</span><span class="sxs-lookup"><span data-stu-id="35522-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="35522-190">@No__t-0 屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="35522-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="35522-191">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="35522-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="35522-192">提供給用戶端的 JavaScript 會C#叫用 .net 方法。</span><span class="sxs-lookup"><span data-stu-id="35522-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="35522-193">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="35522-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="35522-194">選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="35522-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="35522-195">主控台輸出如下：</span><span class="sxs-lookup"><span data-stu-id="35522-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="35522-196">第四個數組值會推送至 `ReturnArrayAsync` 所傳回的陣列（`data.push(4);`）。</span><span class="sxs-lookup"><span data-stu-id="35522-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="35522-197">實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="35522-197">Instance method call</span></span>

<span data-ttu-id="35522-198">您也可以從 JavaScript 呼叫 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="35522-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="35522-199">若要從 JavaScript 叫用 .NET 實例方法：</span><span class="sxs-lookup"><span data-stu-id="35522-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="35522-200">將 .NET 實例包裝在 `DotNetObjectReference` 實例中，以將其傳遞給 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="35522-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="35522-201">.NET 實例會以傳址方式傳遞至 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="35522-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="35522-202">使用 `invokeMethod` 或 `invokeMethodAsync` 函數，在實例上叫用 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="35522-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="35522-203">從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="35522-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="35522-204">範例應用程式會將訊息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="35522-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="35522-205">如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="35522-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="35522-206">選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時，會呼叫 `ExampleJsInterop.CallHelloHelperSayHello`，並將名稱（`Blazor`）傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="35522-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="35522-207">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="35522-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="35522-208">`CallHelloHelperSayHello` 會以 `HelloHelper` 的新實例叫用 `sayHello` 的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="35522-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="35522-209">*JsInteropClasses/ExampleJsInterop .cs*：</span><span class="sxs-lookup"><span data-stu-id="35522-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="35522-210">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="35522-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="35522-211">名稱會傳遞至 `HelloHelper` 的「函式」，以設定 `HelloHelper.Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="35522-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="35522-212">當執行 JavaScript 函式 `sayHello` 時，`HelloHelper.SayHello` 會傳回 `Hello, {Name}!` 訊息，由 JavaScript 函式寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="35522-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="35522-213">*JsInteropClasses/HelloHelper .cs*：</span><span class="sxs-lookup"><span data-stu-id="35522-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="35522-214">瀏覽器 網頁程式開發人員工具中的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="35522-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="35522-215">共用類別庫中的 interop 程式碼</span><span class="sxs-lookup"><span data-stu-id="35522-215">Share interop code in a class library</span></span>

<span data-ttu-id="35522-216">JavaScript interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。</span><span class="sxs-lookup"><span data-stu-id="35522-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="35522-217">類別庫會處理在建立的元件中內嵌 JavaScript 資源。</span><span class="sxs-lookup"><span data-stu-id="35522-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="35522-218">JavaScript 檔案會放在*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="35522-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="35522-219">工具會在建立程式庫時，負責內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="35522-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="35522-220">在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。</span><span class="sxs-lookup"><span data-stu-id="35522-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="35522-221">還原套件之後，應用程式程式碼可以呼叫 JavaScript，就像是C#一樣。</span><span class="sxs-lookup"><span data-stu-id="35522-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="35522-222">如需詳細資訊，請參閱<xref:blazor/class-libraries>。</span><span class="sxs-lookup"><span data-stu-id="35522-222">For more information, see <xref:blazor/class-libraries>.</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="35522-223">強化 JS interop 呼叫</span><span class="sxs-lookup"><span data-stu-id="35522-223">Harden JS interop calls</span></span>

<span data-ttu-id="35522-224">JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。</span><span class="sxs-lookup"><span data-stu-id="35522-224">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="35522-225">根據預設，Blazor 伺服器應用程式會在一分鐘之後，將伺服器上的 JS interop 呼叫數倍。</span><span class="sxs-lookup"><span data-stu-id="35522-225">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="35522-226">如果應用程式可以容忍較積極的超時（例如10秒），請使用下列其中一種方法來設定超時時間：</span><span class="sxs-lookup"><span data-stu-id="35522-226">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="35522-227">全域在 `Startup.ConfigureServices` 中，指定 timeout：</span><span class="sxs-lookup"><span data-stu-id="35522-227">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="35522-228">在元件程式碼中的每個調用，單一呼叫可以指定 timeout：</span><span class="sxs-lookup"><span data-stu-id="35522-228">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="35522-229">如需資源耗盡的詳細資訊，請參閱 <xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="35522-229">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>
