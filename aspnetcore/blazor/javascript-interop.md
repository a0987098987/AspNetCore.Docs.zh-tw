---
title: ASP.NET Core Blazor JavaScript interop
author: guardrex
description: 瞭解如何從 Blazor 應用程式中的 JavaScript，從 .NET 和 .NET 方法叫用 JavaScript 函式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/javascript-interop
ms.openlocfilehash: 4edef123bc1fe41845b8060b9c3b8e77ffd2969d
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885468"
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

會 使用`IJSRuntime.InvokeVoidAsync`呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或 [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) 的 JavaScript 函數。

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

## <a name="invoke-net-methods-from-javascript-functions"></a>從 JavaScript 函式呼叫 .NET 方法

### <a name="static-net-method-call"></a>靜態 .NET 方法呼叫

若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。 傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。 最好是非同步版本，以支援 Blazor 伺服器案例。 若要從 JavaScript 叫用 .NET 方法，.NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 的屬性。 根據預設，方法識別碼是方法名稱，但您可以使用 `JSInvokableAttribute` 的函式來指定不同的識別碼。 目前不支援呼叫開放式泛型方法。

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

如需詳細資訊，請參閱<xref:blazor/class-libraries>。

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

## <a name="additional-resources"></a>其他資源

* [InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
