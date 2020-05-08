---
title: 在 ASP.NET Core 中從 .NET 方法呼叫 JavaScript 函式Blazor
author: guardrex
description: 瞭解如何從應用程式中的 .NET 方法叫Blazor用 JavaScript 函數。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-javascript-from-dotnet
ms.openlocfilehash: 064f504e94cd65862370d4551c6cb44210a8238f
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967294"
---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-blazor"></a><span data-ttu-id="a1456-103">在 ASP.NET Core 中從 .NET 方法呼叫 JavaScript 函式Blazor</span><span class="sxs-lookup"><span data-stu-id="a1456-103">Call JavaScript functions from .NET methods in ASP.NET Core Blazor</span></span>

<span data-ttu-id="a1456-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a1456-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="a1456-105">Blazor應用程式可以從 javascript 函數的 .net 方法和 .net 方法叫用 javascript 函式。</span><span class="sxs-lookup"><span data-stu-id="a1456-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="a1456-106">這些案例稱為*JavaScript 互通性*（*JS interop*）。</span><span class="sxs-lookup"><span data-stu-id="a1456-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="a1456-107">本文涵蓋從 .NET 叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="a1456-107">This article covers invoking JavaScript functions from .NET.</span></span> <span data-ttu-id="a1456-108">如需如何從 JavaScript 呼叫 .NET 方法的詳細資訊， <xref:blazor/call-dotnet-from-javascript>請參閱。</span><span class="sxs-lookup"><span data-stu-id="a1456-108">For information on how to call .NET methods from JavaScript, see <xref:blazor/call-dotnet-from-javascript>.</span></span>

<span data-ttu-id="a1456-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a1456-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a1456-110">若要從 .NET 呼叫 JavaScript，請使用`IJSRuntime`抽象概念。</span><span class="sxs-lookup"><span data-stu-id="a1456-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="a1456-111">若要發出 JS interop 呼叫，請`IJSRuntime`在您的元件中插入抽象概念。</span><span class="sxs-lookup"><span data-stu-id="a1456-111">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="a1456-112">`InvokeAsync<T>`方法會接受您想要叫用之 JavaScript 函數的識別碼，以及任何數目的 JSON 可序列化引數。</span><span class="sxs-lookup"><span data-stu-id="a1456-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="a1456-113">函數識別碼相對於全域範圍（`window`）。</span><span class="sxs-lookup"><span data-stu-id="a1456-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="a1456-114">如果您想要呼叫`window.someScope.someFunction`，識別碼會是`someScope.someFunction`。</span><span class="sxs-lookup"><span data-stu-id="a1456-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="a1456-115">在呼叫函式之前，不需要先註冊函式。</span><span class="sxs-lookup"><span data-stu-id="a1456-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="a1456-116">傳回型`T`別也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="a1456-116">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="a1456-117">`T`應符合最佳對應至所傳回 JSON 類型的 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="a1456-117">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="a1456-118">若Blazor為已啟用可呈現的伺服器應用程式，在初始的預入期間，不可能呼叫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="a1456-118">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="a1456-119">在建立與瀏覽器的連線之後，必須延後 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1456-119">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="a1456-120">如需詳細資訊，請參閱偵測[ Blazor伺服器應用程式何時進行預呈現](#detect-when-a-blazor-server-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="a1456-120">For more information, see the [Detect when a Blazor Server app is prerendering](#detect-when-a-blazor-server-app-is-prerendering) section.</span></span>

<span data-ttu-id="a1456-121">下列範例是以 JavaScript 為基礎的解碼器為基礎[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)。</span><span class="sxs-lookup"><span data-stu-id="a1456-121">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), a JavaScript-based decoder.</span></span> <span data-ttu-id="a1456-122">此範例示範如何從 c # 方法叫用 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="a1456-122">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="a1456-123">JavaScript 函式會接受 c # 方法的位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="a1456-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="a1456-124">`<head>`在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host* Blazor的元素中，提供 JavaScript 函式來使用`TextDecoder`來解碼傳遞的陣列，並傳回解碼的值：</span><span class="sxs-lookup"><span data-stu-id="a1456-124">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="a1456-125">JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（*.js*）載入，其中包含腳本檔案的參考：</span><span class="sxs-lookup"><span data-stu-id="a1456-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="a1456-126">下列元件：</span><span class="sxs-lookup"><span data-stu-id="a1456-126">The following component:</span></span>

* <span data-ttu-id="a1456-127">選取元件`convertArray`按鈕（ `JSRuntime` **轉換陣列**）時，使用來叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="a1456-127">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="a1456-128">呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="a1456-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="a1456-129">此字串會傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="a1456-129">The string is returned to the component for display.</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a><span data-ttu-id="a1456-130">IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="a1456-130">IJSRuntime</span></span>

<span data-ttu-id="a1456-131">若要使用`IJSRuntime`抽象概念，請採用下列任何一種方法：</span><span class="sxs-lookup"><span data-stu-id="a1456-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="a1456-132">將`IJSRuntime`抽象概念插入Razor元件（*razor*）中：</span><span class="sxs-lookup"><span data-stu-id="a1456-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="a1456-133">在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. cshtml* （Blazor Server）的`handleTickerChanged` `<head>`元素內，提供 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="a1456-133">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="a1456-134">呼叫函式時`IJSRuntime.InvokeVoidAsync` ，不會傳回值：</span><span class="sxs-lookup"><span data-stu-id="a1456-134">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="a1456-135">將`IJSRuntime`抽象概念插入類別（*.cs*）：</span><span class="sxs-lookup"><span data-stu-id="a1456-135">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="a1456-136">在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. cshtml* （Blazor Server）的`handleTickerChanged` `<head>`元素內，提供 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="a1456-136">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="a1456-137">使用`JSRuntime.InvokeAsync`呼叫函式，並傳回值：</span><span class="sxs-lookup"><span data-stu-id="a1456-137">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="a1456-138">針對使用[BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)的動態內容產生，請`[Inject]`使用屬性：</span><span class="sxs-lookup"><span data-stu-id="a1456-138">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="a1456-139">在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：</span><span class="sxs-lookup"><span data-stu-id="a1456-139">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="a1456-140">`showPrompt`&ndash;產生提示以接受使用者輸入（使用者的名稱），並將名稱傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="a1456-140">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="a1456-141">`displayWelcome`&ndash;將歡迎訊息從呼叫者指派給具有`id`之的`welcome`DOM 物件。</span><span class="sxs-lookup"><span data-stu-id="a1456-141">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="a1456-142">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="a1456-142">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="a1456-143">將參考`<script>` JavaScript 檔案的標記放在*Wwwroot/index.html*檔案（Blazor WebAssembly）或*Pages/_Host. cshtml*檔案（Blazor伺服器）中。</span><span class="sxs-lookup"><span data-stu-id="a1456-143">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="a1456-144">*wwwroot/index.html* （Blazor WebAssembly）：</span><span class="sxs-lookup"><span data-stu-id="a1456-144">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="a1456-145">*Pages/_Host. cshtml* （Blazor伺服器）：</span><span class="sxs-lookup"><span data-stu-id="a1456-145">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="a1456-146">請勿將`<script>`標記放在元件檔中， `<script>`因為無法動態更新標記。</span><span class="sxs-lookup"><span data-stu-id="a1456-146">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="a1456-147">.NET 方法會藉由呼叫`IJSRuntime.InvokeAsync<T>`，與*ExampleJsInterop*中的 JavaScript 函式進行 interop。</span><span class="sxs-lookup"><span data-stu-id="a1456-147">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="a1456-148">`IJSRuntime`抽象概念是非同步，允許Blazor伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="a1456-148">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="a1456-149">如果應用程式是Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函`IJSInProcessRuntime`式， `Invoke<T>`請改為向下轉換並呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1456-149">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="a1456-150">我們建議大部分的 JS interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="a1456-150">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="a1456-151">範例應用程式包含一個可示範 JS interop 的元件。</span><span class="sxs-lookup"><span data-stu-id="a1456-151">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="a1456-152">元件：</span><span class="sxs-lookup"><span data-stu-id="a1456-152">The component:</span></span>

* <span data-ttu-id="a1456-153">透過 JavaScript 提示接收使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a1456-153">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="a1456-154">將文字傳回到元件以進行處理。</span><span class="sxs-lookup"><span data-stu-id="a1456-154">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="a1456-155">呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="a1456-155">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="a1456-156">*Pages/JSInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="a1456-156">*Pages/JSInterop.razor*:</span></span>

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
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");

        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. <span data-ttu-id="a1456-157">當`TriggerJsPrompt`您選取元件的 [觸發程式**JavaScript 提示**] 按鈕來執行時`showPrompt` ，會呼叫*Wwwroot/exampleJsInterop*檔案中提供的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="a1456-157">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="a1456-158">`showPrompt`函式會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。</span><span class="sxs-lookup"><span data-stu-id="a1456-158">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="a1456-159">元件會將使用者的名稱儲存在本機變數中`name`。</span><span class="sxs-lookup"><span data-stu-id="a1456-159">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="a1456-160">儲存在中`name`的字串會併入歡迎使用的訊息中，其會傳遞至 JavaScript 函`displayWelcome`式，以將歡迎訊息轉譯為標題標記。</span><span class="sxs-lookup"><span data-stu-id="a1456-160">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="a1456-161">呼叫 void JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="a1456-161">Call a void JavaScript function</span></span>

<span data-ttu-id="a1456-162">會使用`IJSRuntime.InvokeVoidAsync`呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="a1456-162">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-blazor-server-app-is-prerendering"></a><span data-ttu-id="a1456-163">偵測Blazor伺服器應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="a1456-163">Detect when a Blazor Server app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="a1456-164">捕捉元素的參考</span><span class="sxs-lookup"><span data-stu-id="a1456-164">Capture references to elements</span></span>

<span data-ttu-id="a1456-165">某些 JS interop 案例需要 HTML 元素的參考。</span><span class="sxs-lookup"><span data-stu-id="a1456-165">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="a1456-166">例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在專案上呼叫類似命令的 Api，例如`focus`或。 `play`</span><span class="sxs-lookup"><span data-stu-id="a1456-166">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="a1456-167">使用下列方法來抓取元件中的 HTML 元素參考：</span><span class="sxs-lookup"><span data-stu-id="a1456-167">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="a1456-168">將`@ref`屬性加入至 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="a1456-168">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="a1456-169">定義類型`ElementReference`的欄位，其名稱符合`@ref`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="a1456-169">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="a1456-170">下列範例顯示如何`username` `<input>`捕捉專案的參考：</span><span class="sxs-lookup"><span data-stu-id="a1456-170">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="a1456-171">只使用專案參考來改變不會與Blazor互動之空白元素的內容。</span><span class="sxs-lookup"><span data-stu-id="a1456-171">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="a1456-172">當協力廠商 API 提供內容給元素時，此案例很有用。</span><span class="sxs-lookup"><span data-stu-id="a1456-172">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="a1456-173">因為Blazor不會與專案互動，所以專案和 DOM 的標記法之間Blazor不可能發生衝突。</span><span class="sxs-lookup"><span data-stu-id="a1456-173">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="a1456-174">在下列範例中，因為Blazor會與 DOM 互動以填入此元素的清單專案（`<li>`），所以改變未排序清單的內容會很*危險*（`ul`）：</span><span class="sxs-lookup"><span data-stu-id="a1456-174">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="a1456-175">如果 JS interop 變動此元素`MyList`的內容，並Blazor嘗試將差異套用至專案，則差異不會符合 DOM。</span><span class="sxs-lookup"><span data-stu-id="a1456-175">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="a1456-176">就 .NET 程式`ElementReference`代碼而言，是一個不透明的控制碼。</span><span class="sxs-lookup"><span data-stu-id="a1456-176">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="a1456-177">您*唯一*可以做的事`ElementReference` ，就是透過 JS interop 將它傳遞至 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="a1456-177">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="a1456-178">當您這麼做時，JavaScript 端程式碼會接收`HTMLElement`一個實例，它可以搭配一般的 DOM api 來使用。</span><span class="sxs-lookup"><span data-stu-id="a1456-178">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="a1456-179">例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：</span><span class="sxs-lookup"><span data-stu-id="a1456-179">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="a1456-180">*exampleJsInterop .js*：</span><span class="sxs-lookup"><span data-stu-id="a1456-180">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="a1456-181">若要呼叫不會傳回值的 JavaScript 函數，請`IJSRuntime.InvokeVoidAsync`使用。</span><span class="sxs-lookup"><span data-stu-id="a1456-181">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="a1456-182">下列程式碼會藉由使用已捕捉`ElementReference`的來呼叫前面的 JavaScript 函式，將焦點放在使用者名稱輸入上：</span><span class="sxs-lookup"><span data-stu-id="a1456-182">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="a1456-183">若要使用擴充方法，請建立會接收`IJSRuntime`實例的靜態擴充方法：</span><span class="sxs-lookup"><span data-stu-id="a1456-183">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="a1456-184">`Focus`方法是直接在物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1456-184">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="a1456-185">下列範例假設`Focus`方法可從`JsInteropClasses`命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="a1456-185">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="a1456-186">只有`username`在呈現元件之後，才會填入變數。</span><span class="sxs-lookup"><span data-stu-id="a1456-186">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="a1456-187">如果將擴展`ElementReference`傳遞至 javascript 程式碼，javascript 程式碼就會收到的`null`值。</span><span class="sxs-lookup"><span data-stu-id="a1456-187">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="a1456-188">若要在元件完成轉譯之後操作元素參考（以設定元素的初始焦點），請使用[OnAfterRenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="a1456-188">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="a1456-189">當使用泛型型別並傳回值時，請使用[ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1)：</span><span class="sxs-lookup"><span data-stu-id="a1456-189">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="a1456-190">`GenericMethod`會在具有類型的物件上直接呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1456-190">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="a1456-191">下列範例假設`GenericMethod`可以從`JsInteropClasses`命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="a1456-191">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="a1456-192">跨元件的參考元素</span><span class="sxs-lookup"><span data-stu-id="a1456-192">Reference elements across components</span></span>

<span data-ttu-id="a1456-193">`ElementReference`只有在元件的`OnAfterRender`方法（和元素參考是`struct`）中才保證有效，因此無法在元件之間傳遞專案參考。</span><span class="sxs-lookup"><span data-stu-id="a1456-193">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="a1456-194">若要讓父系元件能夠將專案參考提供給其他元件，父系元件可以：</span><span class="sxs-lookup"><span data-stu-id="a1456-194">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="a1456-195">允許子元件註冊回呼。</span><span class="sxs-lookup"><span data-stu-id="a1456-195">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="a1456-196">使用傳遞的元素參考， `OnAfterRender`在事件期間叫用已註冊的回呼。</span><span class="sxs-lookup"><span data-stu-id="a1456-196">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="a1456-197">這個方法會間接讓子元件與父系的元素參考互動。</span><span class="sxs-lookup"><span data-stu-id="a1456-197">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="a1456-198">下列Blazor WebAssembly 範例說明方法。</span><span class="sxs-lookup"><span data-stu-id="a1456-198">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="a1456-199">`<head>`在*wwwroot/index.html*的中：</span><span class="sxs-lookup"><span data-stu-id="a1456-199">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="a1456-200">`<body>`在*wwwroot/index.html*的中：</span><span class="sxs-lookup"><span data-stu-id="a1456-200">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="a1456-201">*Pages/Index. razor* （父系元件）：</span><span class="sxs-lookup"><span data-stu-id="a1456-201">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="a1456-202">*Pages/Index. .cs*：</span><span class="sxs-lookup"><span data-stu-id="a1456-202">*Pages/Index.razor.cs*:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool disposing;
        private IList<IObserver<ElementReference>> subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in subscriptions)
            {
                try
                {
                    subscription.OnNext(title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            disposing = true;

            foreach (var subscription in subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            subscriptions.Add(observer);

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
                Self.subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="a1456-203">*Shared/SurveyPrompt. razor* （子元件）：</span><span class="sxs-lookup"><span data-stu-id="a1456-203">*Shared/SurveyPrompt.razor* (child component):</span></span>

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

<span data-ttu-id="a1456-204">*Shared/SurveyPrompt. .cs*：</span><span class="sxs-lookup"><span data-stu-id="a1456-204">*Shared/SurveyPrompt.razor.cs*:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace BlazorSample.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (subscription != null)
            {
                subscription.Dispose();
            }

            subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            subscription = null;
        }

        public void OnError(Exception error)
        {
            subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            subscription?.Dispose();
        }
    }
}
```

## <a name="harden-js-interop-calls"></a><span data-ttu-id="a1456-205">強化 JS interop 呼叫</span><span class="sxs-lookup"><span data-stu-id="a1456-205">Harden JS interop calls</span></span>

<span data-ttu-id="a1456-206">JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。</span><span class="sxs-lookup"><span data-stu-id="a1456-206">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="a1456-207">根據預設， Blazor伺服器應用程式會在一分鐘之後，將伺服器上的 JS interop 呼叫數倍。</span><span class="sxs-lookup"><span data-stu-id="a1456-207">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="a1456-208">如果應用程式可以容忍較積極的超時（例如10秒），請使用下列其中一種方法來設定超時時間：</span><span class="sxs-lookup"><span data-stu-id="a1456-208">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="a1456-209">全域在`Startup.ConfigureServices`中，指定 [超時]：</span><span class="sxs-lookup"><span data-stu-id="a1456-209">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="a1456-210">在元件程式碼中的每個調用，單一呼叫可以指定 timeout：</span><span class="sxs-lookup"><span data-stu-id="a1456-210">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="a1456-211">如需資源耗盡的詳細資訊， <xref:security/blazor/server/threat-mitigation>請參閱。</span><span class="sxs-lookup"><span data-stu-id="a1456-211">For more information on resource exhaustion, see <xref:security/blazor/server/threat-mitigation>.</span></span>

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a><span data-ttu-id="a1456-212">避免迴圈物件參考</span><span class="sxs-lookup"><span data-stu-id="a1456-212">Avoid circular object references</span></span>

<span data-ttu-id="a1456-213">在用戶端上，包含迴圈參考的物件無法序列化為下列任一項：</span><span class="sxs-lookup"><span data-stu-id="a1456-213">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="a1456-214">.NET 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1456-214">.NET method calls.</span></span>
* <span data-ttu-id="a1456-215">當傳回類型具有迴圈參考時，來自 c # 的 JavaScript 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1456-215">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="a1456-216">如需詳細資訊，請參閱下列問題：</span><span class="sxs-lookup"><span data-stu-id="a1456-216">For more information, see the following issues:</span></span>

* [<span data-ttu-id="a1456-217">不支援迴圈參考，接受兩個（dotnet/aspnetcore #20525）</span><span class="sxs-lookup"><span data-stu-id="a1456-217">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="a1456-218">提案：在序列化時加入用來處理迴圈參考的機制（dotnet/runtime #30820）</span><span class="sxs-lookup"><span data-stu-id="a1456-218">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a><span data-ttu-id="a1456-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="a1456-219">Additional resources</span></span>

* <xref:blazor/call-dotnet-from-javascript>
* [<span data-ttu-id="a1456-220">InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）</span><span class="sxs-lookup"><span data-stu-id="a1456-220">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="a1456-221">[在伺服器應用程式中Blazor執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="a1456-221">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
