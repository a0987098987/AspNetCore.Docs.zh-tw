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
# <a name="aspnet-core-blazor-javascript-interop"></a>ASP.NET Core Blazor JavaScript interop

By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 應用程式可以從 JavaScript 程式碼，叫用 .NET 和 .NET 方法的 JavaScript 函式。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="invoke-javascript-functions-from-net-methods"></a>從 .NET 方法叫用 JavaScript 函式

有時候需要 .NET 程式碼來呼叫 JavaScript 函式。 例如，JavaScript 呼叫可以將 JavaScript 程式庫中的瀏覽器功能或功能公開給應用程式。

若要從 .NET 呼叫 JavaScript，請使用 `IJSRuntime` 抽象概念。 @No__t-0 方法會接受您想要叫用之 JavaScript 函數的識別碼，以及任何數目的 JSON 可序列化引數。 函數識別碼相對於全域範圍（`window`）。 如果您想要呼叫 `window.someScope.someFunction`，識別碼會 `someScope.someFunction`。 在呼叫函式之前，不需要先註冊函式。 傳回類型 `T` 也必須是 JSON 可序列化。

針對 Blazor 伺服器應用程式：

* Blazor 伺服器應用程式會處理多個使用者要求。 請勿在元件中呼叫 `JSRuntime.Current`，以叫用 JavaScript 函數。
* 插入 `IJSRuntime` 抽象概念，並使用插入的物件發出 JavaScript interop 呼叫。
* 當 Blazor 應用程式預先呈現時，無法呼叫 JavaScript，因為尚未建立與瀏覽器的連接。 如需詳細資訊，請參閱偵測[Blazor 應用程式何時進行預呈現](#detect-when-a-blazor-app-is-prerendering)一節。

下列範例是根據[TextDecoder](https://developer.mozilla.org/docs/Web/API/TextDecoder)，這是以實驗性 JavaScript 為基礎的解碼器。 此範例示範如何從C#方法叫用 JavaScript 函數。 JavaScript 函式會從C#方法接受位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。

在*wwwroot/index.html* （Blazor WebAssembly）或*Pages/_Host. Cshtml* （Blazor Server）的 `<head>` 元素內，提供使用 `TextDecoder` 來解碼傳遞陣列的函式：

[!code-html[](javascript-interop/samples_snapshot/index-script.html)]

JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案（ *.js*）載入，其中包含腳本檔案的參考：

```html
<script src="exampleJsInterop.js"></script>
```

下列元件：

* 選取元件按鈕（**轉換陣列**）時，使用 `JsRuntime` 叫用 @no__t 0 JavaScript 函數。
* 呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。 此字串會傳回給元件以供顯示。

[!code-cshtml[](javascript-interop/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

若要使用 `IJSRuntime` 抽象概念，請採用下列任一方法：

* 將 `IJSRuntime` 抽象概念插入 Razor 元件（*razor*）：

  [!code-cshtml[](javascript-interop/samples_snapshot/inject-abstraction.razor?highlight=1)]

* 將 `IJSRuntime` 抽象概念插入類別（ *.cs*）：

  [!code-csharp[](javascript-interop/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

* 針對使用[BuildRenderTree](xref:blazor/components#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：

  ```csharp
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：

* `showPrompt` &ndash; 會產生接受使用者輸入的提示（使用者的名稱），並將名稱傳回給呼叫者。
* `displayWelcome` &ndash; 會將歡迎訊息從呼叫者指派給具有 `id` of `welcome` 的 DOM 物件。

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

將參考 JavaScript 檔案的 `<script>` 標記放在*wwwroot/index.html*檔案（Blazor WebAssembly）或*Pages/_Host. Cshtml*檔案（Blazor 伺服器）中。

*wwwroot/index.html* （Blazor WebAssembly）：

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=15)]

*Pages/_Host. cshtml* （Blazor Server）：

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=21)]

請勿將 `<script>` 標記放在元件檔中，因為 @no__t 1 標記無法動態更新。

.NET 方法會藉由呼叫 `IJSRuntime.InvokeAsync<T>`，與*exampleJsInterop*檔案中的 JavaScript 函數互通。

@No__t-0 抽象概念是非同步，允許 Blazor 伺服器案例。 如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請將轉換為 `IJSInProcessRuntime`，並改為呼叫 `Invoke<T>`。 我們建議大部分的 JavaScript interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。

範例應用程式包含一個可示範 JavaScript interop 的元件。 元件：

* 透過 JavaScript 提示接收使用者輸入。
* 將文字傳回到元件以進行處理。
* 呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。

*Pages/JSInterop. razor*：

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop1&highlight=3,19-21,23-25)]

1. 當 `TriggerJsPrompt` 是藉由選取元件的 [**觸發程式 JavaScript 提示**] 按鈕來執行時，會呼叫*wwwroot/exampleJsInterop*檔案中提供的 JavaScript `showPrompt` 函數。
1. @No__t-0 函式會接受使用者輸入（使用者的名稱），這是 HTML 編碼並傳回給元件。 元件會將使用者的名稱儲存在本機變數中，`name`。
1. 儲存在 `name` 中的字串會合並到歡迎訊息中，該訊息會傳遞至 JavaScript 函式 `displayWelcome`，這會將歡迎訊息轉譯為標題標記。

## <a name="call-a-void-javascript-function"></a>呼叫 void JavaScript 函數

會使用 `IJSRuntime.InvokeVoidAsync` 呼叫傳回[void （0）/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函數。

## <a name="detect-when-a-blazor-app-is-prerendering"></a>偵測 Blazor 應用程式何時已進行預呈現
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>捕捉元素的參考

某些[JavaScript interop](xref:blazor/javascript-interop)案例需要 HTML 元素的參考。 例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在元素上呼叫類似命令的 Api，例如 `focus` 或 `play`。

使用下列方法來抓取元件中的 HTML 元素參考：

* 將 `@ref` 屬性加入至 HTML 元素。
* 定義 `ElementReference` 類型的欄位，其名稱符合 @no__t 1 屬性的值。

下列範例顯示如何捕捉 `username` `<input>` 元素的參考：

```cshtml
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!NOTE]
> 請勿**使用已**捕捉的元素參考做為擴展 DOM 的方式。 這麼做可能會干擾宣告式轉譯模型。

就 .NET 程式碼而言，`ElementReference` 是不透明的控制碼。 @No__t-1 的*唯一*做法是透過 javascript interop 將它傳遞至 javascript 程式碼。 當您這麼做時，JavaScript 端程式碼會收到 `HTMLElement` 實例，它可以搭配一般的 DOM Api 來使用。

例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：

*exampleJsInterop .js*：

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

使用 `IJSRuntime.InvokeAsync<T>`，並以 `ElementReference` 呼叫 `exampleJsFunctions.focusElement`，以將焦點放在元素：

[!code-cshtml[](javascript-interop/samples_snapshot/component1.razor?highlight=1,3,11-12)]

若要使用擴充方法將焦點放在元素，請建立會接收 `IJSRuntime` 實例的靜態擴充方法：

```csharp
public static Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<object>(
        "exampleJsFunctions.focusElement", elementRef);
}
```

方法是直接在物件上呼叫。 下列範例假設 `JsInteropClasses` 命名空間中提供了靜態 `Focus` 方法：

[!code-cshtml[](javascript-interop/samples_snapshot/component2.razor?highlight=1,4,12)]

> [!IMPORTANT]
> 只有在呈現元件之後，才會填入 `username` 變數。 如果擴展 `ElementReference` 傳遞至 JavaScript 程式碼，JavaScript 程式碼就會收到 `null` 的值。 若要在元件完成轉譯之後操作元素參考（以設定專案的初始焦點），請使用 `OnAfterRenderAsync` 或 @no__t 1[元件生命週期方法](xref:blazor/components#lifecycle-methods)。

## <a name="invoke-net-methods-from-javascript-functions"></a>從 JavaScript 函式呼叫 .NET 方法

### <a name="static-net-method-call"></a>靜態 .NET 方法呼叫

若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。 傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。 最好是非同步版本來支援 Blazor 伺服器案例。 若要從 JavaScript 叫用 .NET 方法，.NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 的屬性。 根據預設，方法識別碼是方法名稱，但您可以使用 `JSInvokableAttribute` 的函式來指定不同的識別碼。 目前不支援呼叫開放式泛型方法。

範例應用程式包含傳回C# `int` 陣列的方法。 @No__t-0 屬性會套用至方法。

*Pages/JsInterop. razor*：

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop2&highlight=7-11)]

提供給用戶端的 JavaScript 會C#叫用 .net 方法。

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。

主控台輸出如下：

```console
Array(4) [ 1, 2, 3, 4 ]
```

第四個數組值會推送至 `ReturnArrayAsync` 所傳回的陣列（`data.push(4);`）。

### <a name="instance-method-call"></a>實例方法呼叫

您也可以從 JavaScript 呼叫 .NET 實例方法。 若要從 JavaScript 叫用 .NET 實例方法：

* 將 .NET 實例包裝在 `DotNetObjectReference` 實例中，以將其傳遞給 JavaScript。 .NET 實例會以傳址方式傳遞至 JavaScript。
* 使用 `invokeMethod` 或 `invokeMethodAsync` 函數，在實例上叫用 .NET 實例方法。 從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。

> [!NOTE]
> 範例應用程式會將訊息記錄到用戶端主控台。 如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。

選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時，會呼叫 `ExampleJsInterop.CallHelloHelperSayHello`，並將名稱（`Blazor`）傳遞給方法。

*Pages/JsInterop. razor*：

[!code-cshtml[](./common/samples/3.x/BlazorWebAssemblySample/Pages/JsInterop.razor?name=snippet_JSInterop3&highlight=8-9)]

`CallHelloHelperSayHello` 會以 `HelloHelper` 的新實例叫用 `sayHello` 的 JavaScript 函數。

*JsInteropClasses/ExampleJsInterop .cs*：

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=10-16)]

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

名稱會傳遞至 `HelloHelper` 的「函式」，以設定 `HelloHelper.Name` 屬性。 當執行 JavaScript 函式 `sayHello` 時，`HelloHelper.SayHello` 會傳回 `Hello, {Name}!` 訊息，由 JavaScript 函式寫入主控台。

*JsInteropClasses/HelloHelper .cs*：

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

瀏覽器 網頁程式開發人員工具中的主控台輸出：

```console
Hello, Blazor!
```

## <a name="share-interop-code-in-a-class-library"></a>共用類別庫中的 interop 程式碼

JavaScript interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。

類別庫會處理在建立的元件中內嵌 JavaScript 資源。 JavaScript 檔案會放在*wwwroot*資料夾中。 工具會在建立程式庫時，負責內嵌資源。

在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。 還原套件之後，應用程式程式碼可以呼叫 JavaScript，就像是C#一樣。

如需詳細資訊，請參閱<xref:blazor/class-libraries>。

## <a name="harden-js-interop-calls"></a>強化 JS interop 呼叫

JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。 根據預設，Blazor 伺服器應用程式會在一分鐘之後，將伺服器上的 JS interop 呼叫數倍。 如果應用程式可以容忍較積極的超時（例如10秒），請使用下列其中一種方法來設定超時時間：

* 全域在 `Startup.ConfigureServices` 中，指定 timeout：

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
