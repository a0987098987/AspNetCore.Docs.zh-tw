---
title: Razor 元件 JavaScript interop
author: guardrex
description: 了解如何叫用 JavaScript 函式，從.NET 和.NET 從 JavaScript Blazor 和 Razor 元件的應用程式中的方法。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/javascript-interop
ms.openlocfilehash: f2588f4ed1ec2f01218283625fae4632d0a8ae58
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515430"
---
# <a name="razor-components-javascript-interop"></a><span data-ttu-id="1c72a-103">Razor 元件 JavaScript interop</span><span class="sxs-lookup"><span data-stu-id="1c72a-103">Razor Components JavaScript interop</span></span>

<span data-ttu-id="1c72a-104">藉由[Javier Calvarro Nelson](https://github.com/javiercn)， [Daniel Roth](https://github.com/danroth27)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c72a-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c72a-105">Razor 元件應用程式可以叫用 JavaScript 函式，從.NET 和.NET 中的 JavaScript 程式碼的方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-105">A Razor Components app can invoke JavaScript functions from .NET and .NET methods from JavaScript code.</span></span>

## <a name="invoke-javascript-functions-from-net-methods"></a><span data-ttu-id="1c72a-106">叫用.NET 方法的 JavaScript 函式</span><span class="sxs-lookup"><span data-stu-id="1c72a-106">Invoke JavaScript functions from .NET methods</span></span>

<span data-ttu-id="1c72a-107">有些.NET 程式碼時要呼叫的 JavaScript 函式需要的時候。</span><span class="sxs-lookup"><span data-stu-id="1c72a-107">There are times when .NET code is required to call a JavaScript function.</span></span> <span data-ttu-id="1c72a-108">例如，瀏覽器功能或從 JavaScript 程式庫應用程式的功能，可以公開 JavaScript 呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c72a-108">For example, a JavaScript call can expose browser capabilities or functionality from a JavaScript library to the app.</span></span>

<span data-ttu-id="1c72a-109">若要從.NET 呼叫 JavaScript，使用`IJSRuntime`抽象概念。</span><span class="sxs-lookup"><span data-stu-id="1c72a-109">To call into JavaScript from .NET, use the `IJSRuntime` abstraction.</span></span> <span data-ttu-id="1c72a-110">`InvokeAsync<T>`方法會採用您想要搭配任意數目的 JSON 可序列化的引數叫用的 JavaScript 函式的識別項。</span><span class="sxs-lookup"><span data-stu-id="1c72a-110">The `InvokeAsync<T>` method takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="1c72a-111">函式識別項是相對於全域範圍 (`window`)。</span><span class="sxs-lookup"><span data-stu-id="1c72a-111">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="1c72a-112">如果您想要呼叫`window.someScope.someFunction`，此識別項是`someScope.someFunction`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-112">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="1c72a-113">就不需要註冊函式，會在呼叫之前。</span><span class="sxs-lookup"><span data-stu-id="1c72a-113">There's no need to register the function before it's called.</span></span> <span data-ttu-id="1c72a-114">傳回的型別`T`也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="1c72a-114">The return type `T` must also be JSON serializable.</span></span>

<span data-ttu-id="1c72a-115">針對伺服器端 ASP.NET Core Razor 元件應用程式：</span><span class="sxs-lookup"><span data-stu-id="1c72a-115">For server-side ASP.NET Core Razor Components apps:</span></span>

* <span data-ttu-id="1c72a-116">伺服器端應用程式會處理多個使用者要求。</span><span class="sxs-lookup"><span data-stu-id="1c72a-116">Multiple user requests are processed by the server-side app.</span></span> <span data-ttu-id="1c72a-117">請不要呼叫`JSRuntime.Current`元件叫用 JavaScript 函式中。</span><span class="sxs-lookup"><span data-stu-id="1c72a-117">Don't call `JSRuntime.Current` in a component to invoke JavaScript functions.</span></span>
* <span data-ttu-id="1c72a-118">插入`IJSRuntime`抽象並用來發出 interop 的 JavaScript 呼叫插入的物件。</span><span class="sxs-lookup"><span data-stu-id="1c72a-118">Inject the `IJSRuntime` abstraction and use the injected object to issue JavaScript interop calls.</span></span>

<span data-ttu-id="1c72a-119">下列範例根據[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)，實驗性的 JavaScript 基礎解碼器。</span><span class="sxs-lookup"><span data-stu-id="1c72a-119">The following example is based on [TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder), an experimental JavaScript-based decoder.</span></span> <span data-ttu-id="1c72a-120">此範例示範如何叫用的 JavaScript 函式，從C#方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-120">The example demonstrates how to invoke a JavaScript function from a C# method.</span></span> <span data-ttu-id="1c72a-121">JavaScript 函式會接受位元組陣列，從C#方法，將陣列，並傳回元件用於顯示的文字。</span><span class="sxs-lookup"><span data-stu-id="1c72a-121">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="1c72a-122">內部`<head>`項目*wwwroot/index.html*，提供使用的函式`TextDecoder`解碼傳入的陣列：</span><span class="sxs-lookup"><span data-stu-id="1c72a-122">Inside the `<head>` element of *wwwroot/index.html*, provide a function that uses `TextDecoder` to decode a passed array:</span></span>

```html
<script>
  window.ConvertArray = (win1251Array) => {
    var win1251decoder = new TextDecoder('windows-1251');
    var bytes = new Uint8Array(win1251Array);
    var decodedArray = win1251decoder.decode(bytes);
    console.log(decodedArray);
    return decodedArray;
  };
</script>
```

<span data-ttu-id="1c72a-123">JavaScript 程式碼，例如上述範例中，所示的程式碼也可以載入 JavaScript 檔案 (*.js*) 中的指令碼檔案的參考*wwwroot/index.html*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c72a-123">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (*.js*) with a reference to the script file in the *wwwroot/index.html* file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="1c72a-124">下列元件：</span><span class="sxs-lookup"><span data-stu-id="1c72a-124">The following component:</span></span>

* <span data-ttu-id="1c72a-125">叫用`ConvertArray`JavaScript 函式使用`JsRuntime`當元件 按鈕 (**轉換陣列**) 已選取。</span><span class="sxs-lookup"><span data-stu-id="1c72a-125">Invokes the `ConvertArray` JavaScript function using `JsRuntime` when a component button (**Convert Array**) is selected.</span></span>
* <span data-ttu-id="1c72a-126">JavaScript 函式呼叫之後，會傳遞的陣列轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="1c72a-126">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="1c72a-127">若要顯示的元件，會傳回字串。</span><span class="sxs-lookup"><span data-stu-id="1c72a-127">The string is returned to the component for display.</span></span>

```cshtml
@page "/"
@inject IJSRuntime JsRuntime;

<h1>Call JavaScript Function Example</h1>

<button type="button" class="btn btn-primary" onclick="@ConvertArray">
    Convert Array
</button>

<p class="mt-2" style="font-size:1.6em">
    <span class="badge badge-success">
        @ConvertedText
    </span>
</p>

@functions {
    // Quote (c)2005 Universal Pictures: Serenity
    // https://www.uphe.com/movies/serenity
    // David Krumholtz on IMDB: https://www.imdb.com/name/nm0472710/

    private MarkupString ConvertedText =
        new MarkupString("Select the <b>Convert Array</b> button.");

    private uint[] QuoteArray = new uint[]
        {
            60, 101, 109, 62, 67, 97, 110, 39, 116, 32, 115, 116, 111, 112, 32,
            116, 104, 101, 32, 115, 105, 103, 110, 97, 108, 44, 32, 77, 97,
            108, 46, 60, 47, 101, 109, 62, 32, 45, 32, 77, 114, 46, 32, 85, 110,
            105, 118, 101, 114, 115, 101, 10, 10,
        };

    private async void ConvertArray()
    {
        var text =
            await JsRuntime.InvokeAsync<string>("ConvertArray", QuoteArray);

        ConvertedText = new MarkupString(text);

        StateHasChanged();
    }
}
```

<span data-ttu-id="1c72a-128">若要使用`IJSRuntime`抽象概念，採用任何下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="1c72a-128">To use the `IJSRuntime` abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="1c72a-129">插入`IJSRuntime`Razor 檔案的抽象概念 (*.razor*， *.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="1c72a-129">Inject the `IJSRuntime` abstraction into the Razor file (*.razor*, *.cshtml*):</span></span>

  ```cshtml
  @inject IJSRuntime JSRuntime

  @functions {
      public override void OnInit()
      {
          StocksService.OnStockTickerUpdated += stockUpdate =>
          {
              JSRuntime.InvokeAsync<object>(
                  "handleTickerChanged",
                  stockUpdate.symbol,
                  stockUpdate.price);
          };
      }
  }
  ```

* <span data-ttu-id="1c72a-130">插入`IJSRuntime`抽象類別 (*.cs*):</span><span class="sxs-lookup"><span data-stu-id="1c72a-130">Inject the `IJSRuntime` abstraction into a class (*.cs*):</span></span>

  ```csharp
  public class JsInteropClasses
  {
      private readonly IJSRuntime _jsRuntime;

      public JsInteropClasses(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public Task<string> TickerChanged(string data)
      {
          // The handleTickerChanged JavaScript method is implemented
          // in a JavaScript file, such as 'wwwroot/tickerJsInterop.js'.
          return _jsRuntime.InvokeAsync<object>(
              "handleTickerChanged",
              stockUpdate.symbol,
              stockUpdate.price);
      }
  }
  ```

* <span data-ttu-id="1c72a-131">使用動態內容產生`BuildRenderTree`，使用`[Inject]`屬性：</span><span class="sxs-lookup"><span data-stu-id="1c72a-131">For dynamic content generation with `BuildRenderTree`, use the `[Inject]` attribute:</span></span>

  ```csharp
  [Inject] IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="1c72a-132">本主題相關的用戶端範例應用程式，在兩個 JavaScript 函式可與接收使用者輸入並顯示歡迎訊息 DOM 互動的用戶端應用程式：</span><span class="sxs-lookup"><span data-stu-id="1c72a-132">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the client-side app that interact with the DOM to receive user input and display a welcome message:</span></span>

* `showPrompt` <span data-ttu-id="1c72a-133">&ndash; 會產生接受使用者輸入 （使用者名稱） 的提示，並傳回給呼叫者的名稱。</span><span class="sxs-lookup"><span data-stu-id="1c72a-133">&ndash; Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* `displayWelcome` <span data-ttu-id="1c72a-134">&ndash; 會從呼叫端的歡迎訊息指派至 DOM 物件，但`id`的`welcome`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-134">&ndash; Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="1c72a-135">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-135">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="1c72a-136">地方`<script>`參考中的 JavaScript 檔案的標記*wwwroot/index.html*檔案：</span><span class="sxs-lookup"><span data-stu-id="1c72a-136">Place the `<script>` tag that references the JavaScript file in the *wwwroot/index.html* file:</span></span>

[!code-html[](./common/samples/3.x/BlazorSample/wwwroot/index.html?highlight=15)]

<span data-ttu-id="1c72a-137">請不要將`<script>`標記中的元件檔案，因為`<script>`標記無法動態更新。</span><span class="sxs-lookup"><span data-stu-id="1c72a-137">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="1c72a-138">中的函式使用 JavaScript 的.NET 方法 interop *exampleJsInterop.js*檔案，藉由呼叫`IJSRuntime.InvokeAsync<T>`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-138">.NET methods interop with the JavaScript functions in the *exampleJsInterop.js* file by calling `IJSRuntime.InvokeAsync<T>`.</span></span>

<span data-ttu-id="1c72a-139">`IJSRuntime`抽象層是以非同步方式來提供伺服器端案例。</span><span class="sxs-lookup"><span data-stu-id="1c72a-139">The `IJSRuntime` abstraction is asynchronous to allow for server-side scenarios.</span></span> <span data-ttu-id="1c72a-140">如果應用程式執行用戶端，而且您想要以同步方式叫用的 JavaScript 函式來向下轉型`IJSInProcessRuntime`並呼叫`Invoke<T>`改。</span><span class="sxs-lookup"><span data-stu-id="1c72a-140">If the app runs client-side and you want to invoke a JavaScript function synchronously, downcast to `IJSInProcessRuntime` and call `Invoke<T>` instead.</span></span> <span data-ttu-id="1c72a-141">我們建議大部分的 JavaScript interop 程式庫使用非同步 Api，以確保程式庫都適用於所有案例中，用戶端或伺服器端。</span><span class="sxs-lookup"><span data-stu-id="1c72a-141">We recommend that most JavaScript interop libraries use the async APIs to ensure that the libraries are available in all scenarios, client-side or server-side.</span></span>

<span data-ttu-id="1c72a-142">範例應用程式包含元件，可示範 JavaScript interop。</span><span class="sxs-lookup"><span data-stu-id="1c72a-142">The sample app includes a component to demonstrate JavaScript interop.</span></span> <span data-ttu-id="1c72a-143">元件：</span><span class="sxs-lookup"><span data-stu-id="1c72a-143">The component:</span></span>

* <span data-ttu-id="1c72a-144">接收透過 JavaScript 提示使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="1c72a-144">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="1c72a-145">要處理的元件傳回的文字。</span><span class="sxs-lookup"><span data-stu-id="1c72a-145">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="1c72a-146">呼叫第二個的 JavaScript 函式與顯示歡迎訊息 DOM 互動。</span><span class="sxs-lookup"><span data-stu-id="1c72a-146">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="1c72a-147">*Pages/JSInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-147">*Pages/JSInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. <span data-ttu-id="1c72a-148">當`TriggerJsPrompt`執行選取的元件**觸發程序 JavaScript 提示**按鈕，JavaScript`showPrompt`中提供的函式*wwwroot/exampleJsInterop.js*檔案呼叫。</span><span class="sxs-lookup"><span data-stu-id="1c72a-148">When `TriggerJsPrompt` is executed by selecting the component's **Trigger JavaScript Prompt** button, the JavaScript `showPrompt` function provided in the *wwwroot/exampleJsInterop.js* file is called.</span></span>
1. <span data-ttu-id="1c72a-149">`showPrompt`函式會接受使用者輸入 （使用者的名稱），也就是 HTML 編碼，並傳回至元件。</span><span class="sxs-lookup"><span data-stu-id="1c72a-149">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="1c72a-150">該元件會將使用者的名稱儲存在本機變數中， `name`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-150">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="1c72a-151">將字串儲存在`name`歡迎訊息，傳遞至 JavaScript 函式時，會併入`displayWelcome`，會呈現標題標記的歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="1c72a-151">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="capture-references-to-elements"></a><span data-ttu-id="1c72a-152">擷取的項目參考</span><span class="sxs-lookup"><span data-stu-id="1c72a-152">Capture references to elements</span></span>

<span data-ttu-id="1c72a-153">有些[JavaScript interop](xref:razor-components/javascript-interop)案例需要 HTML 項目的參考。</span><span class="sxs-lookup"><span data-stu-id="1c72a-153">Some [JavaScript interop](xref:razor-components/javascript-interop) scenarios require references to HTML elements.</span></span> <span data-ttu-id="1c72a-154">比方說，UI 程式庫可能需要的項目參考進行初始化，或者您可能需要類似命令的 Api 呼叫項目，例如`focus`或`play`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-154">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="1c72a-155">您可以擷取在元件中的 HTML 項目的參考，加上`ref`屬性的 HTML 項目，然後定義類型的欄位`ElementRef`名稱的值相符`ref`屬性。</span><span class="sxs-lookup"><span data-stu-id="1c72a-155">You can capture references to HTML elements in a component by adding a `ref` attribute to the HTML element and then defining a field of type `ElementRef` whose name matches the value of the `ref` attribute.</span></span>

<span data-ttu-id="1c72a-156">下列範例會示範擷取使用者名稱輸入項目的參考：</span><span class="sxs-lookup"><span data-stu-id="1c72a-156">The following example shows capturing a reference to the username input element:</span></span>

```cshtml
<input ref="username" ...>

@functions {
    ElementRef username;
}
```

> [!NOTE]
> <span data-ttu-id="1c72a-157">請勿**不**這種方式填入 DOM 中使用擷取的項目參考</span><span class="sxs-lookup"><span data-stu-id="1c72a-157">Do **not** use captured element references as a way of populating the DOM.</span></span> <span data-ttu-id="1c72a-158">如此一來，可能會干擾宣告式轉譯模型。</span><span class="sxs-lookup"><span data-stu-id="1c72a-158">Doing so may interfere with the declarative rendering model.</span></span>

<span data-ttu-id="1c72a-159">.NET 程式碼是而言，`ElementRef`是不透明的控制代碼。</span><span class="sxs-lookup"><span data-stu-id="1c72a-159">As far as .NET code is concerned, an `ElementRef` is an opaque handle.</span></span> <span data-ttu-id="1c72a-160">*僅*件事您可以使用執行`ElementRef`是將其傳遞至 JavaScript 程式碼，透過 JavaScript interop。</span><span class="sxs-lookup"><span data-stu-id="1c72a-160">The *only* thing you can do with `ElementRef` is pass it through to JavaScript code via JavaScript interop.</span></span> <span data-ttu-id="1c72a-161">當您這樣做時，JavaScript 後端程式碼會接收`HTMLElement`執行個體，它可以使用一般的 DOM Api。</span><span class="sxs-lookup"><span data-stu-id="1c72a-161">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="1c72a-162">比方說，下列程式碼會定義.NET 延伸模組方法，以允許將焦點設定在項目：</span><span class="sxs-lookup"><span data-stu-id="1c72a-162">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="1c72a-163">*exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-163">*exampleJsInterop.js*:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="1c72a-164">使用`IJSRuntime.InvokeAsync<T>`並呼叫`exampleJsFunctions.focusElement`使用`ElementRef`專注的項目：</span><span class="sxs-lookup"><span data-stu-id="1c72a-164">Use `IJSRuntime.InvokeAsync<T>` and call `exampleJsFunctions.focusElement` with an `ElementRef` to focus an element:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component1.cshtml?highlight=1,3,7,11-12)]

<span data-ttu-id="1c72a-165">若要使用擴充方法，將項目，建立一個靜態擴充方法會接收`IJSRuntime`執行個體：</span><span class="sxs-lookup"><span data-stu-id="1c72a-165">To use an extension method to focus an element, create a static extension method that receives the `IJSRuntime` instance:</span></span>

```csharp
public static Task Focus(this ElementRef elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="1c72a-166">直接在物件上呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-166">The method is called directly on the object.</span></span> <span data-ttu-id="1c72a-167">下列範例假設靜態`Focus`方法可從`JsInteropClasses`命名空間：</span><span class="sxs-lookup"><span data-stu-id="1c72a-167">The following example assumes that the static `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-cshtml[](javascript-interop/samples_snapshot/component2.cshtml?highlight=1,4,8,12)]

> [!IMPORTANT]
> <span data-ttu-id="1c72a-168">`username`元件呈現，並包含其輸出之後，才會填入變數`>`項目。</span><span class="sxs-lookup"><span data-stu-id="1c72a-168">The `username` variable is only populated after the component renders and its output includes the `>` element.</span></span> <span data-ttu-id="1c72a-169">如果您嘗試將傳遞擴展`ElementRef`JavaScript 程式碼中，JavaScript 程式碼會接收`null`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-169">If you try to pass an unpopulated `ElementRef` to JavaScript code, the JavaScript code receives `null`.</span></span> <span data-ttu-id="1c72a-170">若要操作項目參考，該元件已經完成呈現 （若要設定初始焦點的項目上） 的使用之後`OnAfterRenderAsync`或是`OnAfterRender`[元件的生命週期方法](xref:razor-components/components#lifecycle-methods)。</span><span class="sxs-lookup"><span data-stu-id="1c72a-170">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the `OnAfterRenderAsync` or `OnAfterRender` [component lifecycle methods](xref:razor-components/components#lifecycle-methods).</span></span>

## <a name="invoke-net-methods-from-javascript-functions"></a><span data-ttu-id="1c72a-171">叫用.NET 方法，從 JavaScript 函式</span><span class="sxs-lookup"><span data-stu-id="1c72a-171">Invoke .NET methods from JavaScript functions</span></span>

### <a name="static-net-method-call"></a><span data-ttu-id="1c72a-172">靜態的.NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="1c72a-172">Static .NET method call</span></span>

<span data-ttu-id="1c72a-173">若要叫用靜態的.NET 方法，從 JavaScript，請使用`DotNet.invokeMethod`或`DotNet.invokeMethodAsync`函式。</span><span class="sxs-lookup"><span data-stu-id="1c72a-173">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="1c72a-174">在您想要呼叫時，包含函式，以及任何引數的組件名稱的靜態方法的識別碼中傳遞。</span><span class="sxs-lookup"><span data-stu-id="1c72a-174">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="1c72a-175">支援伺服器端案例最好的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="1c72a-175">The asynchronous version is preferred to support server-side scenarios.</span></span> <span data-ttu-id="1c72a-176">若要可叫用 javascript，.NET 方法必須是公用、 靜態的以及與裝飾`[JSInvokable]`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-176">To be invokable from JavaScript, the .NET method must be public, static, and decorated with `[JSInvokable]`.</span></span> <span data-ttu-id="1c72a-177">根據預設，方法識別項是方法名稱，但您可以指定不同的識別項使用`JSInvokableAttribute`建構函式。</span><span class="sxs-lookup"><span data-stu-id="1c72a-177">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor.</span></span> <span data-ttu-id="1c72a-178">呼叫開放式泛型方法目前不支援。</span><span class="sxs-lookup"><span data-stu-id="1c72a-178">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="1c72a-179">範例應用程式包含C#方法傳回的陣列`int`s。</span><span class="sxs-lookup"><span data-stu-id="1c72a-179">The sample app includes a C# method to return an array of `int`s.</span></span> <span data-ttu-id="1c72a-180">方法以裝飾`JSInvokable`屬性。</span><span class="sxs-lookup"><span data-stu-id="1c72a-180">The method is decorated with the `JSInvokable` attribute.</span></span>

<span data-ttu-id="1c72a-181">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-181">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop2&highlight=7-11)]

<span data-ttu-id="1c72a-182">提供給用戶端 JavaScript 叫用C#.NET 方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-182">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="1c72a-183">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-183">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="1c72a-184">當**觸發程序.NET 靜態方法 ReturnArrayAsync**選取按鈕時，檢查在瀏覽器的 web 開發人員工具主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="1c72a-184">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="1c72a-185">主控台輸出為：</span><span class="sxs-lookup"><span data-stu-id="1c72a-185">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="1c72a-186">第四個陣列值推入至的陣列 (`data.push(4);`) 所傳回`ReturnArrayAsync`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-186">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

### <a name="instance-method-call"></a><span data-ttu-id="1c72a-187">執行個體方法呼叫</span><span class="sxs-lookup"><span data-stu-id="1c72a-187">Instance method call</span></span>

<span data-ttu-id="1c72a-188">您也可以從 JavaScript 呼叫.NET 執行個體方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-188">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="1c72a-189">要叫用的.NET 執行個體方法，從 JavaScript，第一次.NET 執行個體傳遞給 JavaScript 藉由包裝在`DotNetObjectRef`執行個體。</span><span class="sxs-lookup"><span data-stu-id="1c72a-189">To invoke a .NET instance method from JavaScript, first pass the .NET instance to JavaScript by wrapping it in a `DotNetObjectRef` instance.</span></span> <span data-ttu-id="1c72a-190">.NET 執行個體由參考傳遞至 JavaScript，以及您可以叫用執行個體使用的.NET 執行個體方法`invokeMethod`或`invokeMethodAsync`函式。</span><span class="sxs-lookup"><span data-stu-id="1c72a-190">The .NET instance is passed by reference to JavaScript, and you can invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="1c72a-191">您也可以將.NET 執行個體傳遞做為引數，當叫用 JavaScript 從其他.NET 方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-191">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="1c72a-192">範例應用程式會將訊息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="1c72a-192">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="1c72a-193">下列範例中所示範的範例應用程式，請檢查瀏覽器的開發人員工具中的瀏覽器的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="1c72a-193">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="1c72a-194">當**觸發程序的.NET 執行個體方法 HelloHelper.SayHello**選取按鈕時，`ExampleJsInterop.CallHelloHelperSayHello`呼叫，並將名稱中，傳遞`Blazor`，方法。</span><span class="sxs-lookup"><span data-stu-id="1c72a-194">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="1c72a-195">*Pages/JsInterop.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-195">*Pages/JsInterop.cshtml*:</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorSample/Pages/JsInterop.cshtml?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` <span data-ttu-id="1c72a-196">JavaScript 函式會叫用`sayHello`的新執行個體與`HelloHelper`。</span><span class="sxs-lookup"><span data-stu-id="1c72a-196">invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="1c72a-197">*JsInteropClasses/ExampleJsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-197">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

<span data-ttu-id="1c72a-198">*wwwroot/exampleJsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-198">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorSample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="1c72a-199">將名稱傳遞給`HelloHelper`的建構函式，它會設定`HelloHelper.Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="1c72a-199">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="1c72a-200">當 JavaScript 函式`sayHello`執行時，`HelloHelper.SayHello`傳回`Hello, {Name}!`訊息，由 JavaScript 函式寫入至主控台。</span><span class="sxs-lookup"><span data-stu-id="1c72a-200">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="1c72a-201">*JsInteropClasses/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c72a-201">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorSample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="1c72a-202">在瀏覽器的 web 開發人員工具中輸出的主控台：</span><span class="sxs-lookup"><span data-stu-id="1c72a-202">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-razor-component-class-library"></a><span data-ttu-id="1c72a-203">共用元件 Razor 類別庫中的 interop 程式碼</span><span class="sxs-lookup"><span data-stu-id="1c72a-203">Share interop code in a Razor Component class library</span></span>

<span data-ttu-id="1c72a-204">JavaScript interop 程式碼可以包含在 Razor 元件類別庫 (`dotnet new razorclasslib`)，這可讓您共用的 NuGet 套件中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="1c72a-204">JavaScript interop code can be included in a Razor Component class library (`dotnet new razorclasslib`), which allows you to share the code in a NuGet package.</span></span>

<span data-ttu-id="1c72a-205">Razor 元件類別程式庫會處理已建置的組件中內嵌的 JavaScript 資源。</span><span class="sxs-lookup"><span data-stu-id="1c72a-205">The Razor Component class library handles embedding JavaScript resources in the built assembly.</span></span> <span data-ttu-id="1c72a-206">JavaScript 檔案會放置於*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="1c72a-206">The JavaScript files are placed in the *wwwroot* folder.</span></span> <span data-ttu-id="1c72a-207">這項工具會負責建置程式庫時，內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="1c72a-207">The tooling takes care of embedding the resources when the library is built.</span></span>

<span data-ttu-id="1c72a-208">應用程式的專案檔中參考的內建的 NuGet 套件，就像任何標準的 NuGet 套件參考。</span><span class="sxs-lookup"><span data-stu-id="1c72a-208">The built NuGet package is referenced in the project file of the app just as any normal NuGet package is referenced.</span></span> <span data-ttu-id="1c72a-209">還原應用程式之後，應用程式程式碼可以呼叫 JavaScript，就好像C#。</span><span class="sxs-lookup"><span data-stu-id="1c72a-209">After the app is restored, app code can call into JavaScript as if it were C#.</span></span>

<span data-ttu-id="1c72a-210">如需詳細資訊，請參閱<xref:razor-components/class-libraries>。</span><span class="sxs-lookup"><span data-stu-id="1c72a-210">For more information, see <xref:razor-components/class-libraries>.</span></span>
