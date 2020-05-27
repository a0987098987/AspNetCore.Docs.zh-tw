---
<span data-ttu-id="81ba9-101">標題： ' 從 .NET 方法呼叫 JavaScript 函式 ASP.NET Core Blazor ' author：描述： ' 瞭解如何從應用程式中的 .net 方法叫用 javascript 函式 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-101">title: 'Call JavaScript functions from .NET methods in ASP.NET Core Blazor' author: description: 'Learn how to invoke JavaScript functions from .NET methods in Blazor apps.'</span></span>
<span data-ttu-id="81ba9-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="81ba9-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="81ba9-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="81ba9-103">'Blazor'</span></span>
- <span data-ttu-id="81ba9-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="81ba9-104">'Identity'</span></span>
- <span data-ttu-id="81ba9-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="81ba9-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="81ba9-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="81ba9-106">'Razor'</span></span>
- <span data-ttu-id="81ba9-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="81ba9-107">'SignalR' uid:</span></span> 

---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-blazor"></a><span data-ttu-id="81ba9-108">在 ASP.NET Core 中從 .NET 方法呼叫 JavaScript 函式Blazor</span><span class="sxs-lookup"><span data-stu-id="81ba9-108">Call JavaScript functions from .NET methods in ASP.NET Core Blazor</span></span>

<span data-ttu-id="81ba9-109">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="81ba9-109">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="81ba9-110">Blazor應用程式可以從 javascript 函數的 .net 方法和 .net 方法叫用 javascript 函式。</span><span class="sxs-lookup"><span data-stu-id="81ba9-110">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="81ba9-111">這些案例稱為*JavaScript 互通性*（*JS interop*）。</span><span class="sxs-lookup"><span data-stu-id="81ba9-111">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="81ba9-112">本文涵蓋從 .NET 叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-112">This article covers invoking JavaScript functions from .NET.</span></span> <span data-ttu-id="81ba9-113">如需如何從 JavaScript 呼叫 .NET 方法的詳細資訊，請參閱 <xref:blazor/call-dotnet-from-javascript> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-113">For information on how to call .NET methods from JavaScript, see <xref:blazor/call-dotnet-from-javascript>.</span></span>

<span data-ttu-id="81ba9-114">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="81ba9-114">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="81ba9-115">若要從 .NET 呼叫 JavaScript，請使用 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="81ba9-115">To call into JavaScript from .NET, use the <xref:Microsoft.JSInterop.IJSRuntime> abstraction.</span></span> <span data-ttu-id="81ba9-116">若要發出 JS interop 呼叫，請 <xref:Microsoft.JSInterop.IJSRuntime> 在您的元件中插入抽象概念。</span><span class="sxs-lookup"><span data-stu-id="81ba9-116">To issue JS interop calls, inject the <xref:Microsoft.JSInterop.IJSRuntime> abstraction in your component.</span></span> <span data-ttu-id="81ba9-117"><xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A>取得您想要叫用之 JavaScript 函式的識別碼，以及任何數目的 JSON 可序列化引數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-117"><xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="81ba9-118">函數識別碼相對於全域範圍（ `window` ）。</span><span class="sxs-lookup"><span data-stu-id="81ba9-118">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="81ba9-119">如果您想要呼叫 `window.someScope.someFunction` ，識別碼會是 `someScope.someFunction` 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-119">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="81ba9-120">在呼叫函式之前，不需要先註冊函式。</span><span class="sxs-lookup"><span data-stu-id="81ba9-120">There's no need to register the function before it's called.</span></span> <span data-ttu-id="81ba9-121">傳回型別 `T` 也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="81ba9-121">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="81ba9-122">`T`應符合最佳對應至所傳回 JSON 類型的 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="81ba9-122">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="81ba9-123">若為 Blazor 已啟用可呈現的伺服器應用程式，在初始的預入期間，不可能呼叫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="81ba9-123">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="81ba9-124">在建立與瀏覽器的連線之後，必須延後 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="81ba9-124">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="81ba9-125">如需詳細資訊，請參閱偵測[ Blazor 伺服器應用程式何時進行預呈現](#detect-when-a-blazor-server-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="81ba9-125">For more information, see the [Detect when a Blazor Server app is prerendering](#detect-when-a-blazor-server-app-is-prerendering) section.</span></span>

<span data-ttu-id="81ba9-126">下列範例是以 JavaScript 為基礎的解碼器為基礎[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)。</span><span class="sxs-lookup"><span data-stu-id="81ba9-126">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), a JavaScript-based decoder.</span></span> <span data-ttu-id="81ba9-127">此範例示範如何從 c # 方法叫用 JavaScript 函式，將開發人員程式碼的需求卸載至現有的 JavaScript API。</span><span class="sxs-lookup"><span data-stu-id="81ba9-127">The example demonstrates how to invoke a JavaScript function from a C# method that offloads a requirement from developer code to an existing JavaScript API.</span></span> <span data-ttu-id="81ba9-128">JavaScript 函式會接受 c # 方法的位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="81ba9-128">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="81ba9-129">在 `<head>` *Wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host*的元素中 Blazor ，提供 JavaScript 函式來使用 `TextDecoder` 來解碼傳遞的陣列，並傳回解碼的值：</span><span class="sxs-lookup"><span data-stu-id="81ba9-129">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="81ba9-130">JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（*.js*）載入，其中包含腳本檔案的參考：</span><span class="sxs-lookup"><span data-stu-id="81ba9-130">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="81ba9-131">下列元件：</span><span class="sxs-lookup"><span data-stu-id="81ba9-131">The following component:</span></span>

* <span data-ttu-id="81ba9-132">`convertArray` `JSRuntime` 選取元件按鈕（**轉換陣列**）時，使用來叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-132">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="81ba9-133">呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="81ba9-133">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="81ba9-134">此字串會傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="81ba9-134">The string is returned to the component for display.</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a><span data-ttu-id="81ba9-135">IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="81ba9-135">IJSRuntime</span></span>

<span data-ttu-id="81ba9-136">若要使用 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念，請採用下列任何一種方法：</span><span class="sxs-lookup"><span data-stu-id="81ba9-136">To use the <xref:Microsoft.JSInterop.IJSRuntime> abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="81ba9-137">將 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念插入 Razor 元件（*razor*）中：</span><span class="sxs-lookup"><span data-stu-id="81ba9-137">Inject the <xref:Microsoft.JSInterop.IJSRuntime> abstraction into the Razor component (*.razor*):</span></span>

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="81ba9-138">在 `<head>` *Wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host. cshtml* （Server）的元素內 Blazor ，提供 `handleTickerChanged` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-138">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="81ba9-139">呼叫函式時 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> ，不會傳回值：</span><span class="sxs-lookup"><span data-stu-id="81ba9-139">The function is called with <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> and doesn't return a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="81ba9-140">將 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念插入類別（*.cs*）：</span><span class="sxs-lookup"><span data-stu-id="81ba9-140">Inject the <xref:Microsoft.JSInterop.IJSRuntime> abstraction into a class (*.cs*):</span></span>

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="81ba9-141">在 `<head>` *Wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host. cshtml* （Server）的元素內 Blazor ，提供 `handleTickerChanged` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-141">Inside the `<head>` element of *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="81ba9-142">使用呼叫函式 `JSRuntime.InvokeAsync` ，並傳回值：</span><span class="sxs-lookup"><span data-stu-id="81ba9-142">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="81ba9-143">針對使用[BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="81ba9-143">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="81ba9-144">在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：</span><span class="sxs-lookup"><span data-stu-id="81ba9-144">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="81ba9-145">`showPrompt`&ndash;產生提示以接受使用者輸入（使用者的名稱），並將名稱傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="81ba9-145">`showPrompt` &ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="81ba9-146">`displayWelcome`將 &ndash; 歡迎訊息從呼叫者指派給具有之的 DOM 物件 `id` `welcome` 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-146">`displayWelcome` &ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="81ba9-147">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="81ba9-147">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="81ba9-148">將 `<script>` 參考 JavaScript 檔案的標記放在*wwwroot/index.html*檔案（ Blazor WebAssembly）或*Pages/_Host. cshtml*檔案（ Blazor 伺服器）中。</span><span class="sxs-lookup"><span data-stu-id="81ba9-148">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server).</span></span>

<span data-ttu-id="81ba9-149">*wwwroot/index.html* （ Blazor WebAssembly）：</span><span class="sxs-lookup"><span data-stu-id="81ba9-149">*wwwroot/index.html* (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="81ba9-150">*Pages/_Host. cshtml* （ Blazor 伺服器）：</span><span class="sxs-lookup"><span data-stu-id="81ba9-150">*Pages/_Host.cshtml* (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="81ba9-151">請勿將 `<script>` 標記放在元件檔中，因為 `<script>` 無法動態更新標記。</span><span class="sxs-lookup"><span data-stu-id="81ba9-151">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="81ba9-152">.NET 方法會藉由呼叫，與*exampleJsInterop*中的 JavaScript 函式進行 interop <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-152">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType>.</span></span>

<span data-ttu-id="81ba9-153"><xref:Microsoft.JSInterop.IJSRuntime>抽象概念是非同步，允許 Blazor 伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="81ba9-153">The <xref:Microsoft.JSInterop.IJSRuntime> abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="81ba9-154">如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請改為向下轉換 <xref:Microsoft.JSInterop.IJSInProcessRuntime> 並呼叫 <xref:Microsoft.JSInterop.IJSInProcessRuntime.Invoke%2A> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-154">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to <xref:Microsoft.JSInterop.IJSInProcessRuntime> and call <xref:Microsoft.JSInterop.IJSInProcessRuntime.Invoke%2A> instead.</span></span> <span data-ttu-id="81ba9-155">我們建議大部分的 JS interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="81ba9-155">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="81ba9-156">範例應用程式包含一個可示範 JS interop 的元件。</span><span class="sxs-lookup"><span data-stu-id="81ba9-156">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="81ba9-157">元件：</span><span class="sxs-lookup"><span data-stu-id="81ba9-157">The component:</span></span>

* <span data-ttu-id="81ba9-158">透過 JavaScript 提示接收使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="81ba9-158">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="81ba9-159">將文字傳回到元件以進行處理。</span><span class="sxs-lookup"><span data-stu-id="81ba9-159">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="81ba9-160">呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="81ba9-160">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="81ba9-161">*Pages/JSInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="81ba9-161">*Pages/JSInterop.razor*:</span></span>

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

1. <span data-ttu-id="81ba9-162">當您 `TriggerJsPrompt` 選取元件的 [觸發程式**JavaScript 提示**] 按鈕來執行時， `showPrompt` 會呼叫*wwwroot/ExampleJsInterop*檔案中提供的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-162">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="81ba9-163">函式 `showPrompt` 會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。</span><span class="sxs-lookup"><span data-stu-id="81ba9-163">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="81ba9-164">元件會將使用者的名稱儲存在本機變數中 `name` 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-164">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="81ba9-165">儲存在中的字串 `name` 會併入歡迎使用的訊息中，其會傳遞至 JavaScript 函式，以將 `displayWelcome` 歡迎訊息轉譯為標題標記。</span><span class="sxs-lookup"><span data-stu-id="81ba9-165">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="81ba9-166">呼叫 void JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="81ba9-166">Call a void JavaScript function</span></span>

<span data-ttu-id="81ba9-167">會使用呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函數 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-167">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType>.</span></span>

## <a name="detect-when-a-blazor-server-app-is-prerendering"></a><span data-ttu-id="81ba9-168">偵測 Blazor 伺服器應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="81ba9-168">Detect when a Blazor Server app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="81ba9-169">捕捉元素的參考</span><span class="sxs-lookup"><span data-stu-id="81ba9-169">Capture references to elements</span></span>

<span data-ttu-id="81ba9-170">某些 JS interop 案例需要 HTML 元素的參考。</span><span class="sxs-lookup"><span data-stu-id="81ba9-170">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="81ba9-171">例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在專案上呼叫類似命令的 Api，例如 `focus` 或 `play` 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-171">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="81ba9-172">使用下列方法來抓取元件中的 HTML 元素參考：</span><span class="sxs-lookup"><span data-stu-id="81ba9-172">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="81ba9-173">將 `@ref` 屬性加入至 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="81ba9-173">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="81ba9-174">定義類型的欄位， <xref:Microsoft.AspNetCore.Components.ElementReference> 其名稱符合屬性的值 `@ref` 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-174">Define a field of type <xref:Microsoft.AspNetCore.Components.ElementReference> whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="81ba9-175">下列範例顯示如何捕捉專案的參考 `username` `<input>` ：</span><span class="sxs-lookup"><span data-stu-id="81ba9-175">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="81ba9-176">只使用專案參考來改變不會與互動之空白元素的內容 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-176">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="81ba9-177">當協力廠商 API 提供內容給元素時，此案例很有用。</span><span class="sxs-lookup"><span data-stu-id="81ba9-177">This scenario is useful when a 3rd party API supplies content to the element.</span></span> <span data-ttu-id="81ba9-178">因為 Blazor 不會與專案互動，所以專案和 DOM 的標記法之間不可能發生衝突 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-178">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="81ba9-179">在下列範例中，因為會*dangerous* `ul` Blazor 與 DOM 互動以填入此元素的清單專案（），所以改變未排序清單的內容會很危險（） `<li>` ：</span><span class="sxs-lookup"><span data-stu-id="81ba9-179">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
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
> <span data-ttu-id="81ba9-180">如果 JS interop 變動此元素的內容， `MyList` 並 Blazor 嘗試將差異套用至專案，則差異不會符合 DOM。</span><span class="sxs-lookup"><span data-stu-id="81ba9-180">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="81ba9-181">就 .NET 程式碼而言， <xref:Microsoft.AspNetCore.Components.ElementReference> 是一個不透明的控制碼。</span><span class="sxs-lookup"><span data-stu-id="81ba9-181">As far as .NET code is concerned, an <xref:Microsoft.AspNetCore.Components.ElementReference> is an opaque handle.</span></span> <span data-ttu-id="81ba9-182">您*唯一*可以做的事 <xref:Microsoft.AspNetCore.Components.ElementReference> ，就是透過 JS interop 將它傳遞至 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="81ba9-182">The *only* thing you can do with <xref:Microsoft.AspNetCore.Components.ElementReference> is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="81ba9-183">當您這麼做時，JavaScript 端程式碼會接收一個 `HTMLElement` 實例，它可以搭配一般的 DOM api 來使用。</span><span class="sxs-lookup"><span data-stu-id="81ba9-183">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="81ba9-184">例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：</span><span class="sxs-lookup"><span data-stu-id="81ba9-184">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="81ba9-185">*exampleJsInterop .js*：</span><span class="sxs-lookup"><span data-stu-id="81ba9-185">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="81ba9-186">若要呼叫不會傳回值的 JavaScript 函數，請使用 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-186">To call a JavaScript function that doesn't return a value, use <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="81ba9-187">下列程式碼會藉由使用已捕捉的來呼叫前面的 JavaScript 函式，將焦點放在使用者名稱輸入上 <xref:Microsoft.AspNetCore.Components.ElementReference> ：</span><span class="sxs-lookup"><span data-stu-id="81ba9-187">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured <xref:Microsoft.AspNetCore.Components.ElementReference>:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="81ba9-188">若要使用擴充方法，請建立會接收實例的靜態擴充方法 <xref:Microsoft.JSInterop.IJSRuntime> ：</span><span class="sxs-lookup"><span data-stu-id="81ba9-188">To use an extension method, create a static extension method that receives the <xref:Microsoft.JSInterop.IJSRuntime> instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="81ba9-189">`Focus`方法是直接在物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="81ba9-189">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="81ba9-190">下列範例假設 `Focus` 方法可從 `JsInteropClasses` 命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="81ba9-190">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="81ba9-191">`username`只有在呈現元件之後，才會填入變數。</span><span class="sxs-lookup"><span data-stu-id="81ba9-191">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="81ba9-192">如果將擴展 <xref:Microsoft.AspNetCore.Components.ElementReference> 傳遞至 javascript 程式碼，javascript 程式碼就會收到的值 `null` 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-192">If an unpopulated <xref:Microsoft.AspNetCore.Components.ElementReference> is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="81ba9-193">若要在元件完成轉譯之後操作元素參考（以設定元素的初始焦點），請使用[OnAfterRenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="81ba9-193">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [OnAfterRenderAsync or OnAfterRender component lifecycle methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="81ba9-194">當使用泛型型別並傳回值時，請使用 <xref:System.Threading.Tasks.ValueTask%601> ：</span><span class="sxs-lookup"><span data-stu-id="81ba9-194">When working with generic types and returning a value, use <xref:System.Threading.Tasks.ValueTask%601>:</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="81ba9-195">`GenericMethod`會在具有類型的物件上直接呼叫。</span><span class="sxs-lookup"><span data-stu-id="81ba9-195">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="81ba9-196">下列範例假設 `GenericMethod` 可以從 `JsInteropClasses` 命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="81ba9-196">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="81ba9-197">跨元件的參考元素</span><span class="sxs-lookup"><span data-stu-id="81ba9-197">Reference elements across components</span></span>

<span data-ttu-id="81ba9-198"><xref:Microsoft.AspNetCore.Components.ElementReference>只有在元件的 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 方法（和元素參考是）中才保證有效 `struct` ，因此無法在元件之間傳遞專案參考。</span><span class="sxs-lookup"><span data-stu-id="81ba9-198">An <xref:Microsoft.AspNetCore.Components.ElementReference> is only guaranteed valid in a component's <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="81ba9-199">若要讓父系元件能夠將專案參考提供給其他元件，父系元件可以：</span><span class="sxs-lookup"><span data-stu-id="81ba9-199">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="81ba9-200">允許子元件註冊回呼。</span><span class="sxs-lookup"><span data-stu-id="81ba9-200">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="81ba9-201">使用傳遞的元素參考，在事件期間叫用已註冊的回呼 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-201">Invoke the registered callbacks during the <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> event with the passed element reference.</span></span> <span data-ttu-id="81ba9-202">這個方法會間接讓子元件與父系的元素參考互動。</span><span class="sxs-lookup"><span data-stu-id="81ba9-202">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="81ba9-203">下列 Blazor WebAssembly 範例說明方法。</span><span class="sxs-lookup"><span data-stu-id="81ba9-203">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="81ba9-204">在 `<head>` *wwwroot/index.html*的中：</span><span class="sxs-lookup"><span data-stu-id="81ba9-204">In the `<head>` of *wwwroot/index.html*:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="81ba9-205">在 `<body>` *wwwroot/index.html*的中：</span><span class="sxs-lookup"><span data-stu-id="81ba9-205">In the `<body>` of *wwwroot/index.html*:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="81ba9-206">*Pages/Index. razor* （父系元件）：</span><span class="sxs-lookup"><span data-stu-id="81ba9-206">*Pages/Index.razor* (parent component):</span></span>

```razor
@page "/"

<h1 @ref="title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="81ba9-207">*Pages/Index. .cs*：</span><span class="sxs-lookup"><span data-stu-id="81ba9-207">*Pages/Index.razor.cs*:</span></span>

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

<span data-ttu-id="81ba9-208">*Shared/SurveyPrompt. razor* （子元件）：</span><span class="sxs-lookup"><span data-stu-id="81ba9-208">*Shared/SurveyPrompt.razor* (child component):</span></span>

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

<span data-ttu-id="81ba9-209">*Shared/SurveyPrompt. .cs*：</span><span class="sxs-lookup"><span data-stu-id="81ba9-209">*Shared/SurveyPrompt.razor.cs*:</span></span>

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

## <a name="harden-js-interop-calls"></a><span data-ttu-id="81ba9-210">強化 JS interop 呼叫</span><span class="sxs-lookup"><span data-stu-id="81ba9-210">Harden JS interop calls</span></span>

<span data-ttu-id="81ba9-211">JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。</span><span class="sxs-lookup"><span data-stu-id="81ba9-211">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="81ba9-212">根據預設， Blazor 伺服器應用程式會在一分鐘之後，將伺服器上的 JS interop 呼叫數倍。</span><span class="sxs-lookup"><span data-stu-id="81ba9-212">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="81ba9-213">如果應用程式可以容忍更積極的超時時間，請使用下列其中一種方法來設定超時時間：</span><span class="sxs-lookup"><span data-stu-id="81ba9-213">If an app can tolerate a more aggressive timeout, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="81ba9-214">全域在中 `Startup.ConfigureServices` ，指定 [超時]：</span><span class="sxs-lookup"><span data-stu-id="81ba9-214">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="81ba9-215">在元件程式碼中的每個調用，單一呼叫可以指定 timeout：</span><span class="sxs-lookup"><span data-stu-id="81ba9-215">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="81ba9-216">如需資源耗盡的詳細資訊，請參閱 <xref:security/blazor/server/threat-mitigation> 。</span><span class="sxs-lookup"><span data-stu-id="81ba9-216">For more information on resource exhaustion, see <xref:security/blazor/server/threat-mitigation>.</span></span>

[!INCLUDE[](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a><span data-ttu-id="81ba9-217">避免迴圈物件參考</span><span class="sxs-lookup"><span data-stu-id="81ba9-217">Avoid circular object references</span></span>

<span data-ttu-id="81ba9-218">在用戶端上，包含迴圈參考的物件無法序列化為下列任一項：</span><span class="sxs-lookup"><span data-stu-id="81ba9-218">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="81ba9-219">.NET 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="81ba9-219">.NET method calls.</span></span>
* <span data-ttu-id="81ba9-220">當傳回類型具有迴圈參考時，來自 c # 的 JavaScript 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="81ba9-220">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="81ba9-221">如需詳細資訊，請參閱下列問題：</span><span class="sxs-lookup"><span data-stu-id="81ba9-221">For more information, see the following issues:</span></span>

* [<span data-ttu-id="81ba9-222">不支援迴圈參考，接受兩個（dotnet/aspnetcore #20525）</span><span class="sxs-lookup"><span data-stu-id="81ba9-222">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="81ba9-223">提案：在序列化時加入用來處理迴圈參考的機制（dotnet/runtime #30820）</span><span class="sxs-lookup"><span data-stu-id="81ba9-223">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a><span data-ttu-id="81ba9-224">其他資源</span><span class="sxs-lookup"><span data-stu-id="81ba9-224">Additional resources</span></span>

* <xref:blazor/call-dotnet-from-javascript>
* [<span data-ttu-id="81ba9-225">InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）</span><span class="sxs-lookup"><span data-stu-id="81ba9-225">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="81ba9-226">[在伺服器應用程式中執行大型資料傳輸 Blazor](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="81ba9-226">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
