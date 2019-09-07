---
title: ASP.NET Core Blazor JavaScript interop
author: guardrex
description: 瞭解如何從 Blazor apps 中的 JavaScript，從 .NET 和 .NET 方法叫用 JavaScript 函式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/javascript-interop
ms.openlocfilehash: 4e2c979971f8f550af4aa9653880bfd1e5fae731
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800294"
---
# <a name="aspnet-core-blazor-javascript-interop"></a><span data-ttu-id="e35c8-103">ASP.NET Core Blazor JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="e35c8-103">ASP.NET Core Blazor JavaScript interop</span></span>

<span data-ttu-id="e35c8-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e35c8-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e35c8-105">Blazor 應用程式可以從 JavaScript 程式碼，叫用 .NET 和 .NET 方法的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="e35c8-105">A Blazor app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

<span data-ttu-id="e35c8-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e35c8-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="e35c8-107">從 .NET 方法叫用 JavaScript 函式</span><span class="sxs-lookup"><span data-stu-id="e35c8-107">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="e35c8-108">有時候需要 .NET 程式碼來呼叫 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="e35c8-108">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="e35c8-109">例如，JavaScript 呼叫可以將 JavaScript 程式庫中的瀏覽器功能或功能公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="e35c8-109">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="e35c8-110">若要從 .net 呼叫 JavaScript，請使用`IJSRuntime`抽象概念。</span><span class="sxs-lookup"><span data-stu-id="e35c8-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="e35c8-111">`InvokeAsync<T>`方法會接受您想要叫用之 JavaScript 函數的識別碼，以及任何數目的 JSON 可序列化引數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-111">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="e35c8-112">函數識別碼相對於全域範圍（`window`）。</span><span class="sxs-lookup"><span data-stu-id="e35c8-112">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="e35c8-113">如果您想要呼叫`window.someScope.someFunction`，識別碼會是`someScope.someFunction`。</span><span class="sxs-lookup"><span data-stu-id="e35c8-113">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="e35c8-114">在呼叫函式之前，不需要先註冊函式。</span><span class="sxs-lookup"><span data-stu-id="e35c8-114">There's no need to register the function before it's called.</span></span> <span data-ttu-id="e35c8-115">傳回型`T`別也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="e35c8-115">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="e35c8-116">針對伺服器端應用程式：</span><span class="sxs-lookup"><span data-stu-id="e35c8-116">For server-side apps:</span></span>

* <span data-ttu-id="e35c8-117">伺服器端應用程式會處理多個使用者要求。</span><span class="sxs-lookup"><span data-stu-id="e35c8-117">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="e35c8-118">不要在`JSRuntime.Current`元件中呼叫以叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-118">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="e35c8-119">`IJSRuntime`插入抽象概念，並使用插入的物件發出 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e35c8-119">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>
* <span data-ttu-id="e35c8-120">當 Blazor 應用程式預先呈現時，無法呼叫 JavaScript，因為尚未建立與瀏覽器的連接。</span><span class="sxs-lookup"><span data-stu-id="e35c8-120">While a Blazor app is prerendering, calling into JavaScript isn't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="e35c8-121">如需詳細資訊，請參閱偵測[Blazor 應用程式何時進行預呈現](#detect-when-a-blazor-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="e35c8-121">For more information, see the [Detect when a Blazor app is prerendering](#detect-when-a-blazor-app-is-prerendering) section.</span></span>

<span data-ttu-id="e35c8-122">下列範例是根據[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)，這是以實驗性 JavaScript 為基礎的解碼器。</span><span class="sxs-lookup"><span data-stu-id="e35c8-122">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="e35c8-123">此範例示範如何從C#方法叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-123">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="e35c8-124">JavaScript 函式會從C#方法接受位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="e35c8-124">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="e35c8-125">在 wwwroot/index.html `<head>` （Blazor用戶端）或*Pages/_Host*的元素中，提供用`TextDecoder`來解碼傳遞陣列的函式：</span><span class="sxs-lookup"><span data-stu-id="e35c8-125">Inside the `<head>` element of *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side), provide a function that uses `TextDecoder` to decode a passed array:</span></span>

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

<span data-ttu-id="e35c8-126">JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（ *.js*）載入，其中包含腳本檔案的參考：</span><span class="sxs-lookup"><span data-stu-id="e35c8-126">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="e35c8-127">下列元件：</span><span class="sxs-lookup"><span data-stu-id="e35c8-127">The following component:</span></span>

* <span data-ttu-id="e35c8-128">選取元件`ConvertArray`按鈕（**轉換陣列**）時，使用來叫用`JsRuntime` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-128">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="e35c8-129">呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="e35c8-129">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="e35c8-130">此字串會傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="e35c8-130">The string is returned to the component for display.</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

<span data-ttu-id="e35c8-131">若要使用`IJSRuntime`抽象概念，請採用下列任何一種方法：</span><span class="sxs-lookup"><span data-stu-id="e35c8-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="e35c8-132">將抽象概念插入 razor 元件（razor）： `IJSRuntime`</span><span class="sxs-lookup"><span data-stu-id="e35c8-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* <span data-ttu-id="e35c8-133">將`IJSRuntime`抽象概念插入類別（ *.cs*）：</span><span class="sxs-lookup"><span data-stu-id="e35c8-133">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* <span data-ttu-id="e35c8-134">針對使用[BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)的動態內容產生，請`[Inject]`使用屬性：</span><span class="sxs-lookup"><span data-stu-id="e35c8-134">For dynamic content generation with [BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="e35c8-135">在本主題隨附的用戶端範例應用程式中，用戶端應用程式可使用兩個 JavaScript 函式來與 DOM 互動，以接收使用者輸入並顯示歡迎訊息：</span><span class="sxs-lookup"><span data-stu-id="e35c8-135">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="e35c8-136">`showPrompt`&ndash;產生提示以接受使用者輸入（使用者的名稱），並將名稱傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="e35c8-136">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="e35c8-137">`displayWelcome`將歡迎訊息從呼叫者指派給`id`具有之的`welcome`DOM 物件。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="e35c8-137">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="e35c8-138">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-138">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="e35c8-139">將參考 JavaScript 檔案的 標記放在wwwroot/index.html檔案（Blazor用戶端）或Pages/_Host檔（Blazor伺服器端）`<script>`中。</span><span class="sxs-lookup"><span data-stu-id="e35c8-139">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side).</span></span>

<span data-ttu-id="e35c8-140">*wwwroot/index.html*（Blazor 用戶端）：</span><span class="sxs-lookup"><span data-stu-id="e35c8-140">*wwwroot/index.html* (Blazor client-side):</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="e35c8-141">*Pages/_Host. cshtml*（Blazor 伺服器端）：</span><span class="sxs-lookup"><span data-stu-id="e35c8-141">*Pages/_Host.cshtml* (Blazor server-side):</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/_Host.cshtml?highlight=29)]

<span data-ttu-id="e35c8-142">請勿將`<script>`標記放在元件檔中， `<script>`因為無法動態更新標記。</span><span class="sxs-lookup"><span data-stu-id="e35c8-142">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="e35c8-143">.NET 方法會藉由呼叫`IJSRuntime.InvokeAsync<T>`，與*exampleJsInterop*中的 JavaScript 函式進行 interop。</span><span class="sxs-lookup"><span data-stu-id="e35c8-143">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="e35c8-144">`IJSRuntime`抽象概念是非同步，允許伺服器端案例。</span><span class="sxs-lookup"><span data-stu-id="e35c8-144">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="e35c8-145">如果應用程式執行用戶端，而您想要以同步方式叫用 JavaScript 函式`IJSInProcessRuntime` ，請`Invoke<T>`改為向下轉換並呼叫。</span><span class="sxs-lookup"><span data-stu-id="e35c8-145">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="e35c8-146">我們建議大部分的 JavaScript interop 程式庫都使用非同步 Api，以確保所有案例（用戶端或伺服器端）都有可用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="e35c8-146">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="e35c8-147">範例應用程式包含一個可示範 JavaScript interop 的元件。</span><span class="sxs-lookup"><span data-stu-id="e35c8-147">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="e35c8-148">元件：</span><span class="sxs-lookup"><span data-stu-id="e35c8-148">The component:</span></span>

* <span data-ttu-id="e35c8-149">透過 JavaScript 提示接收使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="e35c8-149">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="e35c8-150">將文字傳回到元件以進行處理。</span><span class="sxs-lookup"><span data-stu-id="e35c8-150">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="e35c8-151">呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="e35c8-151">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="e35c8-152">*Pages/JSInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-152">*Pages/JSInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="e35c8-153">當`TriggerJsPrompt`您選取元件的 [觸發程式**JavaScript 提示**] 按鈕來執行時`showPrompt` ，會呼叫*wwwroot/exampleJsInterop*檔案中提供的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-153">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="e35c8-154">`showPrompt`函式會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。</span><span class="sxs-lookup"><span data-stu-id="e35c8-154">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="e35c8-155">元件會將使用者的名稱儲存在本機變數`name`中。</span><span class="sxs-lookup"><span data-stu-id="e35c8-155">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="e35c8-156">儲存在中`name`的字串會併入歡迎使用的訊息中，其會傳遞至 JavaScript 函`displayWelcome`式，以將歡迎訊息轉譯為標題標記。</span><span class="sxs-lookup"><span data-stu-id="e35c8-156">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="e35c8-157">呼叫 void JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="e35c8-157">Call a void JavaScript function</span></span>

<span data-ttu-id="e35c8-158">傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函式會使用`IJSRuntime.InvokeAsync<object>`來呼叫， `null`該函式會傳回。</span><span class="sxs-lookup"><span data-stu-id="e35c8-158">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeAsync<object>`, which returns `null`.</span></span>

## <a name="detect-when-a-blazor-app-is-prerendering"></a><span data-ttu-id="e35c8-159">偵測 Blazor 應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="e35c8-159">Detect when a Blazor app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="e35c8-160">捕捉元素的參考</span><span class="sxs-lookup"><span data-stu-id="e35c8-160">Capture references to elements</span></span>

<span data-ttu-id="e35c8-161">某些[JavaScript interop](xref:blazor/javascript-interop)案例需要 HTML 元素的參考。</span><span class="sxs-lookup"><span data-stu-id="e35c8-161">Some [JavaScript interop](xref:blazor/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="e35c8-162">例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在專案上呼叫類似命令的 api，例如`focus`或。 `play`</span><span class="sxs-lookup"><span data-stu-id="e35c8-162">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="e35c8-163">使用下列方法來抓取元件中的 HTML 元素參考：</span><span class="sxs-lookup"><span data-stu-id="e35c8-163">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="e35c8-164">`@ref`將屬性加入至 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="e35c8-164">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="e35c8-165">定義類型`ElementReference`的欄位，其名稱符合`@ref`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="e35c8-165">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="e35c8-166">下列範例顯示如何`username` `<input>`捕捉專案的參考：</span><span class="sxs-lookup"><span data-stu-id="e35c8-166">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> <span data-ttu-id="e35c8-167">當 Blazor 與參考的專案互動時，**請勿使用已**捕捉的專案參考作為擴展或操作 DOM 的方式。</span><span class="sxs-lookup"><span data-stu-id="e35c8-167">Do **not** use captured element references as a way of populating or manipulating the DOM when Blazor interacts with the elements referenced.</span></span> <span data-ttu-id="e35c8-168">這麼做可能會干擾宣告式轉譯模型。</span><span class="sxs-lookup"><span data-stu-id="e35c8-168">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="e35c8-169">就 .net 程式`ElementReference`代碼而言，是一個不透明的控制碼。</span><span class="sxs-lookup"><span data-stu-id="e35c8-169">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="e35c8-170">您*唯一*可以做`ElementReference`的事，就是透過 javascript interop 將它傳遞至 javascript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="e35c8-170">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="e35c8-171">當您這麼做時，JavaScript 端程式碼會接收`HTMLElement`一個實例，它可以搭配一般的 DOM api 來使用。</span><span class="sxs-lookup"><span data-stu-id="e35c8-171">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="e35c8-172">例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：</span><span class="sxs-lookup"><span data-stu-id="e35c8-172">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="e35c8-173">*exampleJsInterop .js*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-173">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="e35c8-174">使用`IJSRuntime.InvokeAsync<T>`並呼叫`exampleJsFunctions.focusElement` ，以將焦點放在元素：`ElementReference`</span><span class="sxs-lookup"><span data-stu-id="e35c8-174">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementReference` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="e35c8-175">若要使用擴充方法將焦點放在元素，請建立可接收`IJSRuntime`實例的靜態擴充方法：</span><span class="sxs-lookup"><span data-stu-id="e35c8-175">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="e35c8-176">方法是直接在物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="e35c8-176">The method is called directly on the object.</span></span> <span data-ttu-id="e35c8-177">下列範例假設可以`Focus` `JsInteropClasses`從命名空間取得靜態方法：</span><span class="sxs-lookup"><span data-stu-id="e35c8-177">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> <span data-ttu-id="e35c8-178">只有`username`在呈現元件之後，才會填入變數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-178">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="e35c8-179">如果將擴展`ElementReference`傳遞至 javascript 程式碼，javascript 程式碼就會收到的`null`值。</span><span class="sxs-lookup"><span data-stu-id="e35c8-179">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="e35c8-180">若要在元件完成轉譯之後操作元素參考 (以設定專案的初始焦點), 請使用`OnAfterRenderAsync`或[元件生命週期方法](xref:blazor/components#lifecycle-methods) `OnAfterRender`。</span><span class="sxs-lookup"><span data-stu-id="e35c8-180">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:blazor/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="e35c8-181">從 JavaScript 函式呼叫 .NET 方法</span><span class="sxs-lookup"><span data-stu-id="e35c8-181">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="e35c8-182">靜態 .NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="e35c8-182">Static .NET method call</span></span>

<span data-ttu-id="e35c8-183">若要從 JavaScript 叫用靜態 .net 方法，請`DotNet.invokeMethod`使用`DotNet.invokeMethodAsync`或函數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-183">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="e35c8-184">傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-184">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="e35c8-185">建議的非同步版本是為了支援伺服器端案例。</span><span class="sxs-lookup"><span data-stu-id="e35c8-185">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="e35c8-186">若要從 JavaScript 叫用 .net 方法，.net 方法必須是公用的、靜態的，而且`[JSInvokable]`具有屬性。</span><span class="sxs-lookup"><span data-stu-id="e35c8-186">To invoke a .NET method from JavaScript, the .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="e35c8-187">根據預設，方法識別碼是方法名稱，但您可以使用此`JSInvokableAttribute`函數來指定不同的識別碼。</span><span class="sxs-lookup"><span data-stu-id="e35c8-187">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="e35c8-188">目前不支援呼叫開放式泛型方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-188">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="e35c8-189">範例應用程式包含傳回C#之陣列的`int`方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-189">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="e35c8-190">`JSInvokable`屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-190">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="e35c8-191">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-191">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="e35c8-192">提供給用戶端的 JavaScript 會C#叫用 .net 方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-192">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="e35c8-193">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-193">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="e35c8-194">選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="e35c8-194">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="e35c8-195">主控台輸出如下：</span><span class="sxs-lookup"><span data-stu-id="e35c8-195">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="e35c8-196">第四個數組值會推送至所`data.push(4);` `ReturnArrayAsync`傳回的陣列（）。</span><span class="sxs-lookup"><span data-stu-id="e35c8-196">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="e35c8-197">實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="e35c8-197">Instance method call</span></span>

<span data-ttu-id="e35c8-198">您也可以從 JavaScript 呼叫 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-198">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="e35c8-199">若要從 JavaScript 叫用 .NET 實例方法：</span><span class="sxs-lookup"><span data-stu-id="e35c8-199">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="e35c8-200">將 .net 實例包裝在`DotNetObjectReference`實例中，以將其傳遞給 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="e35c8-200">Pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectReference` instance.</span></span> <span data-ttu-id="e35c8-201">.NET 實例會以傳址方式傳遞至 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="e35c8-201">The .NET instance is passed by reference to JavaScript.</span></span>
* <span data-ttu-id="e35c8-202">使用`invokeMethod` 或`invokeMethodAsync`函數，在實例上叫用 .net 實例方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-202">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="e35c8-203">從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="e35c8-203">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="e35c8-204">範例應用程式會將訊息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="e35c8-204">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="e35c8-205">如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="e35c8-205">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="e35c8-206">選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時`ExampleJsInterop.CallHelloHelperSayHello` ，會呼叫，並將名稱`Blazor`傳遞至方法。</span><span class="sxs-lookup"><span data-stu-id="e35c8-206">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="e35c8-207">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-207">*Pages/JsInterop.razor*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

<span data-ttu-id="e35c8-208">`CallHelloHelperSayHello`使用的新`HelloHelper`實例`sayHello`叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="e35c8-208">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="e35c8-209">*JsInteropClasses/ExampleJsInterop .cs*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-209">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="e35c8-210">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-210">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="e35c8-211">名稱會傳遞至的`HelloHelper`函式，以`HelloHelper.Name`設定屬性。</span><span class="sxs-lookup"><span data-stu-id="e35c8-211">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="e35c8-212">執行 javascript `sayHello`函式時， `HelloHelper.SayHello` `Hello, {Name}!`會傳回訊息，由 javascript 函數寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="e35c8-212">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="e35c8-213">*JsInteropClasses/HelloHelper .cs*：</span><span class="sxs-lookup"><span data-stu-id="e35c8-213">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="e35c8-214">瀏覽器 網頁程式開發人員工具中的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="e35c8-214">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a><span data-ttu-id="e35c8-215">共用類別庫中的 interop 程式碼</span><span class="sxs-lookup"><span data-stu-id="e35c8-215">Share interop code in a class library</span></span>

<span data-ttu-id="e35c8-216">JavaScript interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。</span><span class="sxs-lookup"><span data-stu-id="e35c8-216">JavaScript interop code can be included in a class library, which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="e35c8-217">類別庫會處理在建立的元件中內嵌 JavaScript 資源。</span><span class="sxs-lookup"><span data-stu-id="e35c8-217">The class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="e35c8-218">JavaScript 檔案會放在*wwwroot*資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e35c8-218">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="e35c8-219">工具會在建立程式庫時，負責內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="e35c8-219">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="e35c8-220">在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。</span><span class="sxs-lookup"><span data-stu-id="e35c8-220">The built NuGet package is referenced in the app's project file the same way that any NuGet package is referenced.</span></span> <span data-ttu-id="e35c8-221">還原套件之後，應用程式程式碼可以呼叫 JavaScript，就像是C#一樣。</span><span class="sxs-lookup"><span data-stu-id="e35c8-221">After the package is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="e35c8-222">如需詳細資訊，請參閱 <xref:blazor/class-libraries>。</span><span class="sxs-lookup"><span data-stu-id="e35c8-222">For more information, see <xref:blazor/class-libraries>.</span></span>
