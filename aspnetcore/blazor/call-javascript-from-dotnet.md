---
標題： ' 從 .NET 方法呼叫 JavaScript 函式 ASP.NET Core Blazor ' author：描述： ' 瞭解如何從應用程式中的 .net 方法叫用 javascript 函式 Blazor 。
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-blazor"></a>在 ASP.NET Core 中從 .NET 方法呼叫 JavaScript 函式Blazor

By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)

Blazor應用程式可以從 javascript 函數的 .net 方法和 .net 方法叫用 javascript 函式。 這些案例稱為*JavaScript 互通性*（*JS interop*）。

本文涵蓋從 .NET 叫用 JavaScript 函數。 如需如何從 JavaScript 呼叫 .NET 方法的詳細資訊，請參閱 <xref:blazor/call-dotnet-from-javascript> 。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

若要從 .NET 呼叫 JavaScript，請使用 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念。 若要發出 JS interop 呼叫，請 <xref:Microsoft.JSInterop.IJSRuntime> 在您的元件中插入抽象概念。 <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A>取得您想要叫用之 JavaScript 函式的識別碼，以及任何數目的 JSON 可序列化引數。 函數識別碼相對於全域範圍（ `window` ）。 如果您想要呼叫 `window.someScope.someFunction` ，識別碼會是 `someScope.someFunction` 。 在呼叫函式之前，不需要先註冊函式。 傳回型別 `T` 也必須是 JSON 可序列化。 `T`應符合最佳對應至所傳回 JSON 類型的 .NET 類型。

若為 Blazor 已啟用可呈現的伺服器應用程式，在初始的預入期間，不可能呼叫 JavaScript。 在建立與瀏覽器的連線之後，必須延後 JavaScript interop 呼叫。 如需詳細資訊，請參閱偵測[ Blazor 伺服器應用程式何時進行預呈現](#detect-when-a-blazor-server-app-is-prerendering)一節。

下列範例是以 JavaScript 為基礎的解碼器為基礎[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)。 此範例示範如何從 c # 方法叫用 JavaScript 函式，將開發人員程式碼的需求卸載至現有的 JavaScript API。 JavaScript 函式會接受 c # 方法的位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。

在 `<head>` *Wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host*的元素中 Blazor ，提供 JavaScript 函式來使用 `TextDecoder` 來解碼傳遞的陣列，並傳回解碼的值：

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（*.js*）載入，其中包含腳本檔案的參考：

```html
<script src="exampleJsInterop.js"></script>
```

下列元件：

* `convertArray` `JSRuntime` 選取元件按鈕（**轉換陣列**）時，使用來叫用 JavaScript 函數。
* 呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。 此字串會傳回給元件以供顯示。

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a>IJSRuntime

若要使用 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念，請採用下列任何一種方法：

* 將 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念插入 Razor 元件（*razor*）中：

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  在 `<head>` *Wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host. cshtml* （Server）的元素內 Blazor ，提供 `handleTickerChanged` JavaScript 函數。 呼叫函式時 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> ，不會傳回值：

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* 將 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念插入類別（*.cs*）：

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  在 `<head>` *Wwwroot/index.html* （ Blazor WebAssembly）或*Pages/_Host. cshtml* （Server）的元素內 Blazor ，提供 `handleTickerChanged` JavaScript 函數。 使用呼叫函式 `JSRuntime.InvokeAsync` ，並傳回值：

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* 針對使用[BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：

* `showPrompt`：產生可接受使用者輸入的提示（使用者的名稱），並將名稱傳回給呼叫者。
* `displayWelcome`：將歡迎訊息從呼叫者指派給具有之的 DOM 物件 `id` `welcome` 。

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

將 `<script>` 參考 JavaScript 檔案的標記放在*wwwroot/index.html*檔案（ Blazor WebAssembly）或*Pages/_Host. cshtml*檔案（ Blazor 伺服器）中。

*wwwroot/index.html* （ Blazor WebAssembly）：

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*Pages/_Host. cshtml* （ Blazor 伺服器）：

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

請勿將 `<script>` 標記放在元件檔中，因為 `<script>` 無法動態更新標記。

.NET 方法會藉由呼叫，與*exampleJsInterop*中的 JavaScript 函式進行 interop <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> 。

<xref:Microsoft.JSInterop.IJSRuntime>抽象概念是非同步，允許 Blazor 伺服器案例。 如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請改為向下轉換 <xref:Microsoft.JSInterop.IJSInProcessRuntime> 並呼叫 <xref:Microsoft.JSInterop.IJSInProcessRuntime.Invoke%2A> 。 我們建議大部分的 JS interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。

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
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");

        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

1. 當您 `TriggerJsPrompt` 選取元件的 [觸發程式**JavaScript 提示**] 按鈕來執行時， `showPrompt` 會呼叫*wwwroot/ExampleJsInterop*檔案中提供的 JavaScript 函數。
1. 函式 `showPrompt` 會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。 元件會將使用者的名稱儲存在本機變數中 `name` 。
1. 儲存在中的字串 `name` 會併入歡迎使用的訊息中，其會傳遞至 JavaScript 函式，以將 `displayWelcome` 歡迎訊息轉譯為標題標記。

## <a name="call-a-void-javascript-function"></a>呼叫 void JavaScript 函數

會使用呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函數 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> 。

## <a name="detect-when-a-blazor-server-app-is-prerendering"></a>偵測 Blazor 伺服器應用程式何時已進行預呈現
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>捕捉元素的參考

某些 JS interop 案例需要 HTML 元素的參考。 例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在專案上呼叫類似命令的 Api，例如 `focus` 或 `play` 。

使用下列方法來抓取元件中的 HTML 元素參考：

* 將 `@ref` 屬性加入至 HTML 元素。
* 定義類型的欄位， <xref:Microsoft.AspNetCore.Components.ElementReference> 其名稱符合屬性的值 `@ref` 。

下列範例顯示如何捕捉專案的參考 `username` `<input>` ：

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> 只使用專案參考來改變不會與互動之空白元素的內容 Blazor 。 當協力廠商 API 提供內容給元素時，此案例很有用。 因為 Blazor 不會與專案互動，所以專案和 DOM 的標記法之間不可能發生衝突 Blazor 。
>
> 在下列範例中，因為會*dangerous* `ul` Blazor 與 DOM 互動以填入此元素的清單專案（），所以改變未排序清單的內容會很危險（） `<li>` ：
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
> 如果 JS interop 變動此元素的內容， `MyList` 並 Blazor 嘗試將差異套用至專案，則差異不會符合 DOM。

就 .NET 程式碼而言， <xref:Microsoft.AspNetCore.Components.ElementReference> 是一個不透明的控制碼。 您*唯一*可以做的事 <xref:Microsoft.AspNetCore.Components.ElementReference> ，就是透過 JS interop 將它傳遞至 JavaScript 程式碼。 當您這麼做時，JavaScript 端程式碼會接收一個 `HTMLElement` 實例，它可以搭配一般的 DOM api 來使用。

例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：

*exampleJsInterop .js*：

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

若要呼叫不會傳回值的 JavaScript 函數，請使用 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> 。 下列程式碼會藉由使用已捕捉的來呼叫前面的 JavaScript 函式，將焦點放在使用者名稱輸入上 <xref:Microsoft.AspNetCore.Components.ElementReference> ：

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

若要使用擴充方法，請建立會接收實例的靜態擴充方法 <xref:Microsoft.JSInterop.IJSRuntime> ：

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

`Focus`方法是直接在物件上呼叫。 下列範例假設 `Focus` 方法可從 `JsInteropClasses` 命名空間取得：

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> `username`只有在呈現元件之後，才會填入變數。 如果將擴展 <xref:Microsoft.AspNetCore.Components.ElementReference> 傳遞至 javascript 程式碼，javascript 程式碼就會收到的值 `null` 。 若要在元件完成轉譯之後操作元素參考（以設定元素的初始焦點），請使用[OnAfterRenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。

當使用泛型型別並傳回值時，請使用 <xref:System.Threading.Tasks.ValueTask%601> ：

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod`會在具有類型的物件上直接呼叫。 下列範例假設 `GenericMethod` 可以從 `JsInteropClasses` 命名空間取得：

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a>跨元件的參考元素

<xref:Microsoft.AspNetCore.Components.ElementReference>只有在元件的 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 方法（和元素參考是）中才保證有效 `struct` ，因此無法在元件之間傳遞專案參考。

若要讓父系元件能夠將專案參考提供給其他元件，父系元件可以：

* 允許子元件註冊回呼。
* 使用傳遞的元素參考，在事件期間叫用已註冊的回呼 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 。 這個方法會間接讓子元件與父系的元素參考互動。

下列 Blazor WebAssembly 範例說明方法。

在 `<head>` *wwwroot/index.html*的中：

```html
<style>
    .red { color: red }
</style>
```

在 `<body>` *wwwroot/index.html*的中：

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

<h1 @ref="title">Hello, world!</h1>

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

## <a name="harden-js-interop-calls"></a>強化 JS interop 呼叫

JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。 根據預設， Blazor 伺服器應用程式會在一分鐘之後，將伺服器上的 JS interop 呼叫數倍。 如果應用程式可以容忍更積極的超時時間，請使用下列其中一種方法來設定超時時間：

* 全域在中 `Startup.ConfigureServices` ，指定 [超時]：

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* 在元件程式碼中的每個調用，單一呼叫可以指定 timeout：

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

如需資源耗盡的詳細資訊，請參閱 <xref:security/blazor/server/threat-mitigation> 。

[!INCLUDE[](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a>避免迴圈物件參考

在用戶端上，包含迴圈參考的物件無法序列化為下列任一項：

* .NET 方法呼叫。
* 當傳回類型具有迴圈參考時，來自 c # 的 JavaScript 方法呼叫。

如需詳細資訊，請參閱下列問題：

* [不支援迴圈參考，接受兩個（dotnet/aspnetcore #20525）](https://github.com/dotnet/aspnetcore/issues/20525)
* [提案：在序列化時加入用來處理迴圈參考的機制（dotnet/runtime #30820）](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a>其他資源

* <xref:blazor/call-dotnet-from-javascript>
* [InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [在伺服器應用程式中執行大型資料傳輸 Blazor](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
