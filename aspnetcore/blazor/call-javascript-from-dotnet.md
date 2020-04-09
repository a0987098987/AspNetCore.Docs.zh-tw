---
title: 從 ASP.NET 核心中的 .NET 方法呼叫 JavaScript 函數Blazor
author: guardrex
description: 瞭解如何從Blazor應用中的 .NET 方法調用 JAvaScript 函數。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-javascript-from-dotnet
ms.openlocfilehash: 0c6b6a0a8f88fa912523e7772fcd84ef4ce3b4ff
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977011"
---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="55edd-103">從 ASP.NET 核心中的 .NET 方法呼叫 JavaScript 函數Blazor</span><span class="sxs-lookup"><span data-stu-id="55edd-103">Call JavaScript functions from .NET methods in ASP.NET Core Blazor</span></span>

<span data-ttu-id="55edd-104">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)、[丹尼爾·羅斯](https://github.com/danroth27)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="55edd-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="55edd-105">應用Blazor可以從 .NET 方法和 .NET 函數調用 JAvaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="55edd-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="55edd-106">這些方案稱為*JavaScript 互通性*(*JS 互通*)。</span><span class="sxs-lookup"><span data-stu-id="55edd-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="55edd-107">本文介紹從 .NET 調用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="55edd-107">This article covers invoking JavaScript functions from .NET.</span></span> <span data-ttu-id="55edd-108">有關如何從 JAVAScript 調用 .NET<xref:blazor/call-dotnet-from-javascript>方法的資訊, 請參閱。</span><span class="sxs-lookup"><span data-stu-id="55edd-108">For information on how to call .NET methods from JavaScript, see <xref:blazor/call-dotnet-from-javascript>.</span></span>

<span data-ttu-id="55edd-109">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="55edd-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="55edd-110">要從 .NET 調用 JavaScript,`IJSRuntime`請使用抽象。</span><span class="sxs-lookup"><span data-stu-id="55edd-110">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="55edd-111">要發出 JS 互通呼`IJSRuntime`叫, 請將抽象注入到元件中。</span><span class="sxs-lookup"><span data-stu-id="55edd-111">To issue JS interop calls, inject the `IJSRuntime` abstraction in your component.</span></span> <span data-ttu-id="55edd-112">該方法`InvokeAsync<T>`採用要調用的 JAVAScript 函數的標識符以及任意數量的 JSON 可序列化參數。</span><span class="sxs-lookup"><span data-stu-id="55edd-112">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="55edd-113">函數識別碼與全域作用域 ()`window`相關。</span><span class="sxs-lookup"><span data-stu-id="55edd-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="55edd-114">如果要呼叫`window.someScope.someFunction`, 識別碼`someScope.someFunction`為 。</span><span class="sxs-lookup"><span data-stu-id="55edd-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="55edd-115">無需在調用函數之前註冊該函數。</span><span class="sxs-lookup"><span data-stu-id="55edd-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="55edd-116">返回類型`T`也必須是 JSON 可序列化的類型。</span><span class="sxs-lookup"><span data-stu-id="55edd-116">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="55edd-117">`T`應符合最佳映射到返回的 JSON 類型的 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="55edd-117">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="55edd-118">對於Blazor啟用了預算的伺服器應用,在初始預渲染期間無法調用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="55edd-118">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="55edd-119">JavaScript 互通調用必須推遲到與瀏覽器建立連接之後。</span><span class="sxs-lookup"><span data-stu-id="55edd-119">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="55edd-120">有關詳細資訊,請參閱[Blazor"檢測伺服器應用何時是預渲染"](#detect-when-a-blazor-server-app-is-prerendering)部分。</span><span class="sxs-lookup"><span data-stu-id="55edd-120">For more information, see the [Detect when a Blazor Server app is prerendering](#detect-when-a-blazor-server-app-is-prerendering) section.</span></span>

<span data-ttu-id="55edd-121">下面的範例基於[TextDecoder,](https://developer.mozilla.org/docs/Web/API/TextDecoder)一個基於 JavaScript 的解碼器。</span><span class="sxs-lookup"><span data-stu-id="55edd-121">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), a JavaScript-based decoder.</span></span> <span data-ttu-id="55edd-122">該範例展示如何從 C# 方法呼叫 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="55edd-122">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="55edd-123">JavaScript 函數接受來自 C# 方法的位元組,解碼陣列,並將文字返回到元件進行顯示。</span><span class="sxs-lookup"><span data-stu-id="55edd-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="55edd-124">在`<head>`wwwroot/index.html(WebAssembly)Blazor或*頁面/_Host.cshtml(*Blazor伺服器)的元素中,提供了 JavaScript`TextDecoder`函數,用於解碼傳遞的陣列並傳回*wwwroot/index.html*解碼的值:</span><span class="sxs-lookup"><span data-stu-id="55edd-124">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="55edd-125">JavaScript 代碼(如上例中所示的代碼)也可以從 JavaScript 檔案 *(.js*) 載入,並參考文稿檔:</span><span class="sxs-lookup"><span data-stu-id="55edd-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="55edd-126">以下元件:</span><span class="sxs-lookup"><span data-stu-id="55edd-126">The following component:</span></span>

* <span data-ttu-id="55edd-127">在選擇元件`convertArray`按鈕`JSRuntime`(**轉換陣列**) 時呼叫 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="55edd-127">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="55edd-128">呼叫 JavaScript 函數後,傳遞的陣列將轉換為字串。</span><span class="sxs-lookup"><span data-stu-id="55edd-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="55edd-129">字串將返回到元件進行顯示。</span><span class="sxs-lookup"><span data-stu-id="55edd-129">The string is returned to the component for display.</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a><span data-ttu-id="55edd-130">IJS 執行時間</span><span class="sxs-lookup"><span data-stu-id="55edd-130">IJSRuntime</span></span>

<span data-ttu-id="55edd-131">要使用抽象`IJSRuntime`,請使用以下任一方法:</span><span class="sxs-lookup"><span data-stu-id="55edd-131">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="55edd-132">將`IJSRuntime`抽象注入 Razor 元件 *(.razor):*</span><span class="sxs-lookup"><span data-stu-id="55edd-132">Inject the `IJSRuntime` abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="55edd-133">在`<head>`wwwroot/index.html(WebAssembly)Blazor或*頁面/_Host.cshtml(*Blazor伺服器)的`handleTickerChanged`元素中,提供 JavaScript 功能。 *wwwroot/index.html*</span><span class="sxs-lookup"><span data-stu-id="55edd-133">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="55edd-134">函數呼叫`IJSRuntime.InvokeVoidAsync`,不返回值:</span><span class="sxs-lookup"><span data-stu-id="55edd-134">The function is called with `IJSRuntime.InvokeVoidAsync` and doesn't return a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="55edd-135">將`IJSRuntime`抽象注入類別 *(.cs*):</span><span class="sxs-lookup"><span data-stu-id="55edd-135">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="55edd-136">在`<head>`wwwroot/index.html(WebAssembly)Blazor或*頁面/_Host.cshtml(*Blazor伺服器)的`handleTickerChanged`元素中,提供 JavaScript 功能。 *wwwroot/index.html*</span><span class="sxs-lookup"><span data-stu-id="55edd-136">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="55edd-137">函式呼叫`JSRuntime.InvokeAsync`並傳回值:</span><span class="sxs-lookup"><span data-stu-id="55edd-137">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="55edd-138">對於使用[BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)產生動態`[Inject]`內容, 請使用以下屬性:</span><span class="sxs-lookup"><span data-stu-id="55edd-138">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="55edd-139">在本主題附帶的用戶端範例應用程式中,與 DOM 互動的應用可以使用兩個 JavaScript 函數來接收使用者輸入並顯示歡迎訊息:</span><span class="sxs-lookup"><span data-stu-id="55edd-139">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="55edd-140">`showPrompt`&ndash;生成接受使用者輸入(使用者名)並將名稱返回給調用方的提示。</span><span class="sxs-lookup"><span data-stu-id="55edd-140">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="55edd-141">`displayWelcome`&ndash;將歡迎消息從調用方分配給具有`id`的`welcome`DOM 物件。</span><span class="sxs-lookup"><span data-stu-id="55edd-141">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="55edd-142">*wwwroot/範例JsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="55edd-142">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="55edd-143">將`<script>`引用 JavaScript 檔案的標記放在*wwwroot/index.html*檔案(WebAssembly)Blazor或*頁面/ _Host.cshtml*檔案(Blazor伺服器)中。</span><span class="sxs-lookup"><span data-stu-id="55edd-143">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="55edd-144">*wwwroot/index.html(*Blazor網路彙編):</span><span class="sxs-lookup"><span data-stu-id="55edd-144">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="55edd-145">*頁面/_Host.cshtml* Blazor (伺服器):</span><span class="sxs-lookup"><span data-stu-id="55edd-145">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="55edd-146">不要在元件檔中放置`<script>`標記,`<script>`因為無法動態更新標記。</span><span class="sxs-lookup"><span data-stu-id="55edd-146">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="55edd-147">.NET 方法通過調`IJSRuntime.InvokeAsync<T>`用 與 示例*JsInterop.js*檔中的 JavaScript 函數互通。</span><span class="sxs-lookup"><span data-stu-id="55edd-147">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="55edd-148">抽象`IJSRuntime`是異步的,Blazor允許 伺服器方案。</span><span class="sxs-lookup"><span data-stu-id="55edd-148">The `IJSRuntime` abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="55edd-149">如果應用是BlazorWebAssembly 應用,並且您希望同步調用 JavaScript 函數,`IJSInProcessRuntime`請`Invoke<T>`向下轉換到 並呼叫。</span><span class="sxs-lookup"><span data-stu-id="55edd-149">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="55edd-150">我們建議大多數 JS 互通庫使用非同步 API 來確保庫在所有方案中都可用。</span><span class="sxs-lookup"><span data-stu-id="55edd-150">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="55edd-151">範例應用包括一個元件,用於演示 JS 互通。</span><span class="sxs-lookup"><span data-stu-id="55edd-151">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="55edd-152">元件:</span><span class="sxs-lookup"><span data-stu-id="55edd-152">The component:</span></span>

* <span data-ttu-id="55edd-153">通過 JavaScript 提示接收用戶輸入。</span><span class="sxs-lookup"><span data-stu-id="55edd-153">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="55edd-154">將文字返回到元件進行處理。</span><span class="sxs-lookup"><span data-stu-id="55edd-154">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="55edd-155">呼叫第二個 JavaScript 函數,該函數與 DOM 交互以顯示歡迎消息。</span><span class="sxs-lookup"><span data-stu-id="55edd-155">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="55edd-156">*頁面/JSInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="55edd-156">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="55edd-157">當`TriggerJsPrompt`通過選擇元件的**觸發 JavaScript 提示按鈕**執行時`showPrompt`,將調用*wwwroot/exampleJsInterop.js*檔中提供的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="55edd-157">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="55edd-158">該`showPrompt`函數接受使用者輸入(使用者名),該輸入由 HTML 編碼並返回到元件。</span><span class="sxs-lookup"><span data-stu-id="55edd-158">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="55edd-159">元件將使用者名儲存在本地變數中`name`。</span><span class="sxs-lookup"><span data-stu-id="55edd-159">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="55edd-160">儲存在中的`name`字串被合併到歡迎訊息中,該訊息傳遞給 JavaScript 函數`displayWelcome`, 該函數將歡迎消息呈現為標題標記。</span><span class="sxs-lookup"><span data-stu-id="55edd-160">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="55edd-161">呼叫不合法的 JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="55edd-161">Call a void JavaScript function</span></span>

<span data-ttu-id="55edd-162">返回[void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[未定義的](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)`IJSRuntime.InvokeVoidAsync`JavaScript 函數使用 呼叫。</span><span class="sxs-lookup"><span data-stu-id="55edd-162">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with `IJSRuntime.InvokeVoidAsync`.</span></span>

## <a name="detect-when-a-opno-locblazor-server-app-is-prerendering"></a><span data-ttu-id="55edd-163">偵Blazor測 伺服器應用何時預像</span><span class="sxs-lookup"><span data-stu-id="55edd-163">Detect when a Blazor Server app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="55edd-164">捕捉對元素的引照</span><span class="sxs-lookup"><span data-stu-id="55edd-164">Capture references to elements</span></span>

<span data-ttu-id="55edd-165">某些 JS 互通方案需要引用 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="55edd-165">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="55edd-166">例如,UI 庫可能需要元素引用進行初始化,或者您可能需要在元素(`focus`如`play`或) 上調用類似於命令的 API。</span><span class="sxs-lookup"><span data-stu-id="55edd-166">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="55edd-167">使用以下方法擷取對元件中 HTML 元素的參考:</span><span class="sxs-lookup"><span data-stu-id="55edd-167">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="55edd-168">向`@ref`HTML 元素添加屬性。</span><span class="sxs-lookup"><span data-stu-id="55edd-168">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="55edd-169">定義名稱與`@ref`屬性`ElementReference`值 匹配的類型欄位。</span><span class="sxs-lookup"><span data-stu-id="55edd-169">Define a field of type `ElementReference` whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="55edd-170">下面的範例顯示捕捉對元素的`username``<input>`引照:</span><span class="sxs-lookup"><span data-stu-id="55edd-170">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="55edd-171">僅使用元素引用來更改不與Blazor交互的空元素的內容。</span><span class="sxs-lookup"><span data-stu-id="55edd-171">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="55edd-172">當第三方 API 向元素提供內容時,此方案非常有用。</span><span class="sxs-lookup"><span data-stu-id="55edd-172">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="55edd-173">由於Blazor不與元素交互,因此元素的表示形式與 DOM 之間Blazor不存在衝突。</span><span class="sxs-lookup"><span data-stu-id="55edd-173">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="55edd-174">在下面的範例中,改變無序列表的內容*dangerous*`ul`( ) 是危險Blazor的,因為與 DOM 互動以填充此`<li>`元素的清單項目 ( ):</span><span class="sxs-lookup"><span data-stu-id="55edd-174">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="55edd-175">如果 JS 互斥`MyList`對元素 的內容進行Blazor變異, 並嘗試將差異應用於元素,則差異將不匹配 DOM。</span><span class="sxs-lookup"><span data-stu-id="55edd-175">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="55edd-176">就 .NET 代碼而言`ElementReference`,是不透明句柄。</span><span class="sxs-lookup"><span data-stu-id="55edd-176">As far as .NET code is concerned, an `ElementReference` is an opaque handle.</span></span> <span data-ttu-id="55edd-177">唯一`ElementReference`能*用的就是通過*JS互通將其傳遞給JAVaScript代碼。</span><span class="sxs-lookup"><span data-stu-id="55edd-177">The *only* thing you can do with `ElementReference` is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="55edd-178">執行此操作時,JAVAScript 端代碼會接收一`HTMLElement`個實例,它可以與正常的 DOM API 一起使用。</span><span class="sxs-lookup"><span data-stu-id="55edd-178">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="55edd-179">例如,以下代碼定義了一個 .NET 擴展方法,該方法允許將焦點設置在元素上:</span><span class="sxs-lookup"><span data-stu-id="55edd-179">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="55edd-180">*範例 JsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="55edd-180">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="55edd-181">要呼叫不傳回值的 JavaScript 函數`IJSRuntime.InvokeVoidAsync`,請使用 。</span><span class="sxs-lookup"><span data-stu-id="55edd-181">To call a JavaScript function that doesn't return a value, use `IJSRuntime.InvokeVoidAsync`.</span></span> <span data-ttu-id="55edd-182">以下代碼透過使用捕捉`ElementReference`的呼叫前面的 JavaScript 函數來設定對使用者名輸入的關注:</span><span class="sxs-lookup"><span data-stu-id="55edd-182">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured `ElementReference`:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="55edd-183">要使用擴充方法,請建立接收實例`IJSRuntime`的 靜態延伸方法:</span><span class="sxs-lookup"><span data-stu-id="55edd-183">To use an extension method, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="55edd-184">該方法`Focus`直接在對象上調用。</span><span class="sxs-lookup"><span data-stu-id="55edd-184">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="55edd-185">以下範例假定`Focus`該方法可`JsInteropClasses`從 命名空間獲得:</span><span class="sxs-lookup"><span data-stu-id="55edd-185">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="55edd-186">僅當`username`呈現元件后,才會填充該變數。</span><span class="sxs-lookup"><span data-stu-id="55edd-186">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="55edd-187">如果將未填充`ElementReference`的代碼傳送給 JavaScript 代碼,JAvaScript 代碼`null`會收到的值 。</span><span class="sxs-lookup"><span data-stu-id="55edd-187">If an unpopulated `ElementReference` is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="55edd-188">您可以在元件完成呈現後操作元素引用(將初始焦點設定在元素上),請使用 On[後 RenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="55edd-188">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="55edd-189">使用泛型型態並傳回值時,請使用[ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span><span class="sxs-lookup"><span data-stu-id="55edd-189">When working with generic types and returning a value, use [ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="55edd-190">`GenericMethod`直接在具有類型的物件上調用。</span><span class="sxs-lookup"><span data-stu-id="55edd-190">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="55edd-191">以下範例假定 可`GenericMethod``JsInteropClasses`從 命名空間獲得:</span><span class="sxs-lookup"><span data-stu-id="55edd-191">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="55edd-192">跨元件的參考元素</span><span class="sxs-lookup"><span data-stu-id="55edd-192">Reference elements across components</span></span>

<span data-ttu-id="55edd-193">`ElementReference`僅在元件`OnAfterRender`的方法中保證有效(元素引用為`struct`),因此元素引用不能在元件之間傳遞。</span><span class="sxs-lookup"><span data-stu-id="55edd-193">An `ElementReference` is only guaranteed valid in a component's `OnAfterRender` method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="55edd-194">要使父元件使元素引用可用於其他元件,父元件可以:</span><span class="sxs-lookup"><span data-stu-id="55edd-194">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="55edd-195">允許子組件註冊回調。</span><span class="sxs-lookup"><span data-stu-id="55edd-195">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="55edd-196">使用傳遞的元素引用在`OnAfterRender`事件期間調用已註冊的回調。</span><span class="sxs-lookup"><span data-stu-id="55edd-196">Invoke the registered callbacks during the `OnAfterRender` event with the passed element reference.</span></span> <span data-ttu-id="55edd-197">此方法間接允許子元件與父元素引用進行交互。</span><span class="sxs-lookup"><span data-stu-id="55edd-197">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="55edd-198">下面的BlazorWebAssembly 範例說明了該方法。</span><span class="sxs-lookup"><span data-stu-id="55edd-198">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="55edd-199">wwwroot/index.html`<head>`*wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="55edd-199">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="55edd-200">wwwroot/index.html`<body>`*wwwroot/index.html*:</span><span class="sxs-lookup"><span data-stu-id="55edd-200">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="55edd-201">*頁面/索引.razor(* 父元件):</span><span class="sxs-lookup"><span data-stu-id="55edd-201">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="55edd-202">*頁面/索引.razor.cs*:</span><span class="sxs-lookup"><span data-stu-id="55edd-202">*Pages/Index.razor.cs*:</span></span>

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

<span data-ttu-id="55edd-203">*共用/調查提示.剃鬚刀*(子元件):</span><span class="sxs-lookup"><span data-stu-id="55edd-203">*Shared/SurveyPrompt.razor* (child component):</span></span>

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

<span data-ttu-id="55edd-204">*共用/調查提示.剃鬚刀.cs*:</span><span class="sxs-lookup"><span data-stu-id="55edd-204">*Shared/SurveyPrompt.razor.cs*:</span></span>

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

## <a name="harden-js-interop-calls"></a><span data-ttu-id="55edd-205">強化 JS 互通呼叫</span><span class="sxs-lookup"><span data-stu-id="55edd-205">Harden JS interop calls</span></span>

<span data-ttu-id="55edd-206">JS 互操作可能會由於網路錯誤而失敗,應視為不可靠。</span><span class="sxs-lookup"><span data-stu-id="55edd-206">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="55edd-207">默認情況下,伺服器應用Blazor在一分鐘后超時伺服器上的 JS 互通調用。</span><span class="sxs-lookup"><span data-stu-id="55edd-207">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="55edd-208">如果應用可以容忍更具攻擊性的超時(如 10 秒),請使用以下方法之一設置超時:</span><span class="sxs-lookup"><span data-stu-id="55edd-208">If an app can tolerate a more aggressive timeout, such as 10 seconds, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="55edd-209">全域中的`Startup.ConfigureServices`指定逾時:</span><span class="sxs-lookup"><span data-stu-id="55edd-209">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="55edd-210">在元件代碼中按每次呼叫,單一呼叫可以指定逾時:</span><span class="sxs-lookup"><span data-stu-id="55edd-210">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="55edd-211">有關資源耗盡的詳細資訊,請參閱<xref:security/blazor/server>。</span><span class="sxs-lookup"><span data-stu-id="55edd-211">For more information on resource exhaustion, see <xref:security/blazor/server>.</span></span>

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a><span data-ttu-id="55edd-212">避免循環物件參考</span><span class="sxs-lookup"><span data-stu-id="55edd-212">Avoid circular object references</span></span>

<span data-ttu-id="55edd-213">在用戶端上不能序列化包含迴圈引用的物件:</span><span class="sxs-lookup"><span data-stu-id="55edd-213">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="55edd-214">.NET 方法調用。</span><span class="sxs-lookup"><span data-stu-id="55edd-214">.NET method calls.</span></span>
* <span data-ttu-id="55edd-215">當返回類型具有迴圈引用時,JAVAScript 方法從 C# 調用。</span><span class="sxs-lookup"><span data-stu-id="55edd-215">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="55edd-216">有關詳細資訊,請參閱以下問題:</span><span class="sxs-lookup"><span data-stu-id="55edd-216">For more information, see the following issues:</span></span>

* [<span data-ttu-id="55edd-217">不支援迴圈引用,取兩個(點網/阿斯平內芯#20525)</span><span class="sxs-lookup"><span data-stu-id="55edd-217">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="55edd-218">建議:在序列化時添加處理迴圈引用的機制(點網/運行時#30820)</span><span class="sxs-lookup"><span data-stu-id="55edd-218">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a><span data-ttu-id="55edd-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="55edd-219">Additional resources</span></span>

* <xref:blazor/call-dotnet-from-javascript>
* [<span data-ttu-id="55edd-220">Interop元件.剃鬚範例(dotnet/AspNetCore GitHub 儲存庫,3.1 發佈分支)</span><span class="sxs-lookup"><span data-stu-id="55edd-220">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="55edd-221">[在伺服器應用中Blazor執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="55edd-221">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
