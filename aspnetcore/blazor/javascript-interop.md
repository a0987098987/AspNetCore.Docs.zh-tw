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
# <a name="aspnet-core-opno-locblazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript interop

By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 應用程式可以從 JavaScript 程式碼，叫用 .NET 和 .NET 方法的 JavaScript 函式。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>從 .NET 方法叫用 JavaScript 函式

有時候需要 .NET 程式碼來呼叫 JavaScript 函式。 例如，JavaScript 呼叫可以將 JavaScript 程式庫中的瀏覽器功能或功能公開給應用程式。 此案例稱為*JavaScript 互通性*（*JS interop*）。

若要從 .NET 呼叫 JavaScript，請使用 `IJSRuntime` 的抽象概念。 若要發出 JS interop 呼叫，請在您的元件中插入 `IJSRuntime` 抽象概念。 `InvokeAsync<T>` 方法會接受您想要叫用之 JavaScript 函數的識別碼，以及任何數目的 JSON 可序列化引數。 函數識別碼相對於全域範圍（`window`）。 如果您想要呼叫 `window.someScope.someFunction`，則會 `someScope.someFunction`識別碼。 在呼叫函式之前，不需要先註冊函式。 傳回類型 `T` 也必須是 JSON 可序列化。 `T` 應符合最佳對應至所傳回 JSON 類型的 .NET 類型。

若為已啟用可呈現的 Blazor 伺服器應用程式，在初始預入期間，不可能呼叫 JavaScript。 在建立與瀏覽器的連線之後，必須延後 JavaScript interop 呼叫。 如需詳細資訊，請參閱偵測[何時將 Blazor 應用程式進行預呈現](#detect-when-a-blazor-app-is-prerendering)一節。

下列範例是根據[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)，這是以實驗性 JavaScript 為基礎的解碼器。 此範例示範如何從C#方法叫用 JavaScript 函數。 JavaScript 函式會從C#方法接受位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。

在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供使用 `TextDecoder` 來解碼傳遞陣列並傳回解碼值的 JavaScript 函式：

[!code-html[](javascript-interop/samples_snapshot/index-script-convertarray.html)]

JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（ *.js*）載入，其中包含腳本檔案的參考：

```html
<script src="exampleJsInterop.js"></script>
```

下列元件：

* 選取元件按鈕（**轉換陣列**）時，使用 `JSRuntime` 叫用 `convertArray` JavaScript 函數。
* 呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。 此字串會傳回給元件以供顯示。

[!code-razor[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="use-of-ijsruntime"></a>使用 IJSRuntime

若要使用 `IJSRuntime` 抽象概念，請採用下列任一方法：

* 將 `IJSRuntime` 抽象層插入 Razor 元件（*razor*）：

  [!code-razor[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

  在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供 `handleTickerChanged` JavaScript 函數。 函式會使用 `IJSRuntime.InvokeVoidAsync` 來呼叫，而且不會傳回值：

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged1.html)]

* 將 `IJSRuntime` 抽象層插入至類別（ *.cs*）：

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供 `handleTickerChanged` JavaScript 函數。 呼叫函式時，會使用 `JSRuntime.InvokeAsync` 並傳回值：

  [!code-html[](javascript-interop/samples_snapshot/index-script-handleTickerChanged2.html)]

* 針對使用[BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：

* `showPrompt` &ndash; 會產生接受使用者輸入的提示（使用者的名稱），並將名稱傳回給呼叫者。
* `displayWelcome` &ndash; 會將歡迎訊息從呼叫者指派給具有 `welcome``id` 的 DOM 物件。

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

將參考 JavaScript 檔案的 `<script>` 標記放在*wwwroot/index.html*檔案中（Blazor WebAssembly）或*Pages/_Host. cshtml*檔案（Blazor 伺服器）。

*wwwroot/index.html* （Blazor WebAssembly）：

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*Pages/_Host. cshtml* （Blazor Server）：

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

請勿將 `<script>` 標記放在元件檔中，因為 `<script>` 標記無法動態更新。

.NET 方法會藉由呼叫 `IJSRuntime.InvokeAsync<T>`，與*exampleJsInterop*檔案中的 JavaScript 函式進行 interop。

`IJSRuntime` 抽象概念是非同步，可讓 Blazor 伺服器案例。 如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請將轉換成 `IJSInProcessRuntime` 並改為呼叫 `Invoke<T>`。 我們建議大部分的 JS interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。

範例應用程式包含一個可示範 JS interop 的元件。 元件：

* 透過 JavaScript 提示接收使用者輸入。
* 將文字傳回到元件以進行處理。
* 呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。

*Pages/JSInterop. razor*：

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

1. 當您選取元件的 [**觸發程式 JavaScript 提示**] 按鈕來執行 `TriggerJsPrompt` 時，會呼叫*wwwroot/exampleJsInterop*檔案中提供的 JavaScript `showPrompt` 函數。
1. `showPrompt` 函式會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。 元件會將使用者的名稱儲存在本機變數中，`name`。
1. 儲存在 `name` 中的字串會合並到歡迎訊息中，該訊息會傳遞至 JavaScript 函式 `displayWelcome`，這會將歡迎訊息轉譯為標題標記。

## <a name="call-a-void-javascript-function"></a>呼叫 void JavaScript 函數

會使用 `IJSRuntime.InvokeVoidAsync`來呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函式。

## <a name="detect-when-a-opno-locblazor-app-is-prerendering"></a>偵測 Blazor 應用程式何時已進行預呈現
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>捕捉元素的參考

某些 JS interop 案例需要 HTML 元素的參考。 例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在元素上呼叫類似命令的 Api，例如 `focus` 或 `play`。

使用下列方法來抓取元件中的 HTML 元素參考：

* 將 `@ref` 屬性加入至 HTML 元素。
* 定義 `ElementReference` 類型的欄位，其名稱符合 `@ref` 屬性的值。

下列範例會示範如何捕捉 `username` `<input>` 元素的參考：

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> 只使用專案參考來改變不會與 Blazor互動之空白專案的內容。 當協力廠商 API 提供內容給元素時，此案例很有用。 因為 Blazor 不會與專案互動，所以 Blazor的元素和 DOM 的標記法之間不可能發生衝突。
>
> 在下列範例中，改變未排序清單（`ul`）的內容很*危險*，因為 Blazor 與 DOM 互動以填入此元素的清單專案（`<li>`）：
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
> 如果 JS interop 變動此元素 `MyList` 的內容，而 Blazor 嘗試將差異套用至專案，則差異不會符合 DOM。

就 .NET 程式碼而言，`ElementReference` 是不透明的控制碼。 `ElementReference` 的*唯一*做法是透過 JS interop 將它傳遞至 JavaScript 程式碼。 當您這麼做時，JavaScript 端程式碼會接收 `HTMLElement` 實例，它可以搭配一般的 DOM Api 來使用。

例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：

*exampleJsInterop .js*：

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

若要呼叫不會傳回值的 JavaScript 函數，請使用 `IJSRuntime.InvokeVoidAsync`。 下列程式碼會使用已捕捉的 `ElementReference`來呼叫前面的 JavaScript 函式，以將焦點放在使用者名稱輸入上：

[!code-razor[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

若要使用擴充方法，請建立會接收 `IJSRuntime` 實例的靜態擴充方法：

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

`Focus` 方法是直接在物件上呼叫。 下列範例假設 `Focus` 方法可從 `JsInteropClasses` 命名空間取得：

[!code-razor[](javascript-interop/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> 只有在呈現元件之後，才會填入 `username` 變數。 如果擴展 `ElementReference` 傳遞至 JavaScript 程式碼，JavaScript 程式碼就會收到 `null`的值。 若要在元件完成轉譯之後操作元素參考（以設定元素的初始焦點），請使用[OnAfterRenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。

當使用泛型型別並傳回值時，請使用[ValueTask\<t >](xref:System.Threading.Tasks.ValueTask`1)：

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod` 是直接在具有類型的物件上呼叫。 下列範例假設 `GenericMethod` 可從 `JsInteropClasses` 命名空間取得：

[!code-razor[](javascript-interop/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a>跨元件的參考元素

`ElementReference` 只有在元件的 `OnAfterRender` 方法中才有效（而元素參考是 `struct`），因此無法在元件之間傳遞專案參考。

若要讓父系元件能夠將專案參考提供給其他元件，父系元件可以：

* 允許子元件註冊回呼。
* 使用傳遞的元素參考，在 `OnAfterRender` 事件期間叫用已註冊的回呼。 這個方法會間接讓子元件與父系的元素參考互動。

下列 Blazor WebAssembly 範例說明方法。

在*wwwroot/index.html*的 `<head>` 中：

```html
<style>
    .red { color: red }
</style>
```

在*wwwroot/index.html*的 `<body>` 中：

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

*Pages/Index. razor* （父系元件）：

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

*Pages/Index. .cs*：

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

*Shared/SurveyPrompt. razor* （子元件）：

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

*Shared/SurveyPrompt. .cs*：

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

## <a name="invoke-net-methods-from-javascript-functions"></a>從 JavaScript 函式呼叫 .NET 方法

### <a name="static-net-method-call"></a>靜態 .NET 方法呼叫

若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。 傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。 最好是非同步版本，以支援 Blazor 伺服器案例。 .NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 屬性。 目前不支援呼叫開放式泛型方法。

範例應用程式包含C#方法，以傳回 `int`的陣列。 `JSInvokable` 屬性會套用至方法。

*Pages/JsInterop. razor*：

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

提供給用戶端的 JavaScript 會C#叫用 .net 方法。

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。

主控台輸出如下：

```console
Array(4) [ 1, 2, 3, 4 ]
```

第四個數組值會推送至 `ReturnArrayAsync`所傳回的陣列（`data.push(4);`）。

根據預設，方法識別碼是方法名稱，但您可以使用 `JSInvokableAttribute` 的函式來指定不同的識別碼：

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

在用戶端 JavaScript 檔案中：

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

### <a name="instance-method-call"></a>實例方法呼叫

您也可以從 JavaScript 呼叫 .NET 實例方法。 若要從 JavaScript 叫用 .NET 實例方法：

* 將 .NET 實例包裝在 `DotNetObjectReference` 實例中，以將其傳遞給 JavaScript。 .NET 實例會以傳址方式傳遞至 JavaScript。
* 使用 `invokeMethod` 或 `invokeMethodAsync` 函數，在實例上叫用 .NET 實例方法。 從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。

> [!NOTE]
> 範例應用程式會將訊息記錄到用戶端主控台。 如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。

選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時，會呼叫 `ExampleJsInterop.CallHelloHelperSayHello`，並將名稱 `Blazor`傳遞至方法。

*Pages/JsInterop. razor*：

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

`CallHelloHelperSayHello` 會使用 `HelloHelper`的新實例，叫用 JavaScript 函數 `sayHello`。

*JsInteropClasses/ExampleJsInterop .cs*：

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

名稱會傳遞至 `HelloHelper`的函式，以設定 `HelloHelper.Name` 屬性。 執行 JavaScript 函式 `sayHello` 時，`HelloHelper.SayHello` 會傳回 `Hello, {Name}!` 訊息，JavaScript 函式會將它寫入主控台。

*JsInteropClasses/HelloHelper .cs*：

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

瀏覽器 網頁程式開發人員工具中的主控台輸出：

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>共用類別庫中的 interop 程式碼

JS interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。

類別庫會處理在建立的元件中內嵌 JavaScript 資源。 JavaScript 檔案會放在*wwwroot*資料夾中。 工具會在建立程式庫時，負責內嵌資源。

在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。 還原套件之後，應用程式程式碼可以呼叫 JavaScript，就像是C#一樣。

如需詳細資訊，請參閱 <xref:blazor/class-libraries>。

## <a name="harden-js-interop-calls"></a>強化 JS interop 呼叫

JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。 根據預設，Blazor Server 應用程式會在一分鐘後，在伺服器上執行的 JS interop 呼叫數次。 如果應用程式可以容忍較積極的超時（例如10秒），請使用下列其中一種方法來設定超時時間：

* 全域在 `Startup.ConfigureServices`中，指定 [超時]：

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* 在元件程式碼中的每個調用，單一呼叫可以指定 timeout：

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

如需資源耗盡的詳細資訊，請參閱 <xref:security/blazor/server>。

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>在 Blazor 伺服器應用程式中執行大型資料傳輸

在某些情況下，必須在 JavaScript 和 Blazor之間傳輸大量資料。 通常會在下列情況進行大型資料傳輸：

* 瀏覽器檔案系統 Api 可用來上傳或下載檔案。
* 需要具有協力廠商程式庫的互通性。

在 Blazor 伺服器中，有一項限制是為了避免傳遞可能會導致效能問題的單一大型訊息。

開發在 JavaScript 和 Blazor之間傳輸資料的程式碼時，請考慮下列指導方針：

* 將資料分割成較小的片段，並依序傳送資料區段，直到伺服器收到所有資料為止。
* 不要以 JavaScript 和C#程式碼配置大型物件。
* 傳送或接收資料時，不要長時間封鎖主要 UI 執行緒。
* 釋放處理常式完成或取消時所耗用的任何記憶體。
* 基於安全性目的，強制執行下列額外的需求：
  * 宣告可傳遞的檔案或資料大小上限。
  * 宣告從用戶端到伺服器的最小上傳速率。
* 伺服器收到資料之後，資料可以是：
  * 暫時儲存在記憶體緩衝區中，直到收集所有區段為止。
  * 立即使用。 例如，資料可以立即儲存在資料庫中，或在每個區段收到時寫入磁片。

下列檔案上傳者類別會處理用戶端的 JS interop。 上載者類別會使用 JS interop 來執行下列動作：

* 輪詢用戶端以傳送資料區段。
* 如果輪詢超時，則中止交易。

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

在上述範例中：

* `_maxBase64SegmentSize` 設定為 `8192`，這是從 `_maxBase64SegmentSize = _segmentSize * 4 / 3`計算而來。
* 低層級的 .NET Core 記憶體管理 Api 用來將伺服器上的記憶體區段儲存在 `_uploadedSegments`中。
* `ReceiveFile` 方法是用來處理透過 JS interop 的上傳：
  * 檔案大小是以位元組為單位，透過 JS interop 與 `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`來決定。
  * 要接收的區段數目會計算並儲存在 `numberOfSegments`中。
  * 在 `for` 迴圈中，會透過具有 `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`的 JS interop 來要求區段。 在解碼之前，所有區段（但最後一個）必須是8192個位元組。 系統會強制用戶端以有效率的方式傳送資料。
  * 針對每個收到的區段，會先執行檢查，再使用 <xref:System.Convert.TryFromBase64String*>進行解碼。
  * 當上傳完成之後，會以新的 <xref:System.IO.Stream> （`SegmentedStream`）傳回資料的資料流程。

分段資料流程類別會將區段清單公開為 readonly 不可搜尋的 <xref:System.IO.Stream>：

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

下列程式碼會實行 JavaScript 函式來接收資料：

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

## <a name="additional-resources"></a>其他資源

* [InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
