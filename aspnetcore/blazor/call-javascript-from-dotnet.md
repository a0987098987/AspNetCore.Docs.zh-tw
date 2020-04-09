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
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-opno-locblazor"></a>從 ASP.NET 核心中的 .NET 方法呼叫 JavaScript 函數Blazor

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)、[丹尼爾·羅斯](https://github.com/danroth27)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

應用Blazor可以從 .NET 方法和 .NET 函數調用 JAvaScript 函數。 這些方案稱為*JavaScript 互通性*(*JS 互通*)。

本文介紹從 .NET 調用 JavaScript 函數。 有關如何從 JAVAScript 調用 .NET<xref:blazor/call-dotnet-from-javascript>方法的資訊, 請參閱。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))

要從 .NET 調用 JavaScript,`IJSRuntime`請使用抽象。 要發出 JS 互通呼`IJSRuntime`叫, 請將抽象注入到元件中。 該方法`InvokeAsync<T>`採用要調用的 JAVAScript 函數的標識符以及任意數量的 JSON 可序列化參數。 函數識別碼與全域作用域 ()`window`相關。 如果要呼叫`window.someScope.someFunction`, 識別碼`someScope.someFunction`為 。 無需在調用函數之前註冊該函數。 返回類型`T`也必須是 JSON 可序列化的類型。 `T`應符合最佳映射到返回的 JSON 類型的 .NET 類型。

對於Blazor啟用了預算的伺服器應用,在初始預渲染期間無法調用 JavaScript。 JavaScript 互通調用必須推遲到與瀏覽器建立連接之後。 有關詳細資訊,請參閱[Blazor"檢測伺服器應用何時是預渲染"](#detect-when-a-blazor-server-app-is-prerendering)部分。

下面的範例基於[TextDecoder,](https://developer.mozilla.org/docs/Web/API/TextDecoder)一個基於 JavaScript 的解碼器。 該範例展示如何從 C# 方法呼叫 JavaScript 函數。 JavaScript 函數接受來自 C# 方法的位元組,解碼陣列,並將文字返回到元件進行顯示。

在`<head>`wwwroot/index.html(WebAssembly)Blazor或*頁面/_Host.cshtml(*Blazor伺服器)的元素中,提供了 JavaScript`TextDecoder`函數,用於解碼傳遞的陣列並傳回*wwwroot/index.html*解碼的值:

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

JavaScript 代碼(如上例中所示的代碼)也可以從 JavaScript 檔案 *(.js*) 載入,並參考文稿檔:

```html
<script src="exampleJsInterop.js"></script>
```

以下元件:

* 在選擇元件`convertArray`按鈕`JSRuntime`(**轉換陣列**) 時呼叫 JavaScript 函數。
* 呼叫 JavaScript 函數後,傳遞的陣列將轉換為字串。 字串將返回到元件進行顯示。

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a>IJS 執行時間

要使用抽象`IJSRuntime`,請使用以下任一方法:

* 將`IJSRuntime`抽象注入 Razor 元件 *(.razor):*

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  在`<head>`wwwroot/index.html(WebAssembly)Blazor或*頁面/_Host.cshtml(*Blazor伺服器)的`handleTickerChanged`元素中,提供 JavaScript 功能。 *wwwroot/index.html* 函數呼叫`IJSRuntime.InvokeVoidAsync`,不返回值:

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* 將`IJSRuntime`抽象注入類別 *(.cs*):

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  在`<head>`wwwroot/index.html(WebAssembly)Blazor或*頁面/_Host.cshtml(*Blazor伺服器)的`handleTickerChanged`元素中,提供 JavaScript 功能。 *wwwroot/index.html* 函式呼叫`JSRuntime.InvokeAsync`並傳回值:

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* 對於使用[BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)產生動態`[Inject]`內容, 請使用以下屬性:

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

在本主題附帶的用戶端範例應用程式中,與 DOM 互動的應用可以使用兩個 JavaScript 函數來接收使用者輸入並顯示歡迎訊息:

* `showPrompt`&ndash;生成接受使用者輸入(使用者名)並將名稱返回給調用方的提示。
* `displayWelcome`&ndash;將歡迎消息從調用方分配給具有`id`的`welcome`DOM 物件。

*wwwroot/範例JsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

將`<script>`引用 JavaScript 檔案的標記放在*wwwroot/index.html*檔案(WebAssembly)Blazor或*頁面/ _Host.cshtml*檔案(Blazor伺服器)中。

*wwwroot/index.html(*Blazor網路彙編):

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

*頁面/_Host.cshtml* Blazor (伺服器):

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

不要在元件檔中放置`<script>`標記,`<script>`因為無法動態更新標記。

.NET 方法通過調`IJSRuntime.InvokeAsync<T>`用 與 示例*JsInterop.js*檔中的 JavaScript 函數互通。

抽象`IJSRuntime`是異步的,Blazor允許 伺服器方案。 如果應用是BlazorWebAssembly 應用,並且您希望同步調用 JavaScript 函數,`IJSInProcessRuntime`請`Invoke<T>`向下轉換到 並呼叫。 我們建議大多數 JS 互通庫使用非同步 API 來確保庫在所有方案中都可用。

範例應用包括一個元件,用於演示 JS 互通。 元件:

* 通過 JavaScript 提示接收用戶輸入。
* 將文字返回到元件進行處理。
* 呼叫第二個 JavaScript 函數,該函數與 DOM 交互以顯示歡迎消息。

*頁面/JSInterop.razor*:

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

1. 當`TriggerJsPrompt`通過選擇元件的**觸發 JavaScript 提示按鈕**執行時`showPrompt`,將調用*wwwroot/exampleJsInterop.js*檔中提供的 JavaScript 函數。
1. 該`showPrompt`函數接受使用者輸入(使用者名),該輸入由 HTML 編碼並返回到元件。 元件將使用者名儲存在本地變數中`name`。
1. 儲存在中的`name`字串被合併到歡迎訊息中,該訊息傳遞給 JavaScript 函數`displayWelcome`, 該函數將歡迎消息呈現為標題標記。

## <a name="call-a-void-javascript-function"></a>呼叫不合法的 JavaScript 函數

返回[void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[未定義的](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)`IJSRuntime.InvokeVoidAsync`JavaScript 函數使用 呼叫。

## <a name="detect-when-a-opno-locblazor-server-app-is-prerendering"></a>偵Blazor測 伺服器應用何時預像
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a>捕捉對元素的引照

某些 JS 互通方案需要引用 HTML 元素。 例如,UI 庫可能需要元素引用進行初始化,或者您可能需要在元素(`focus`如`play`或) 上調用類似於命令的 API。

使用以下方法擷取對元件中 HTML 元素的參考:

* 向`@ref`HTML 元素添加屬性。
* 定義名稱與`@ref`屬性`ElementReference`值 匹配的類型欄位。

下面的範例顯示捕捉對元素的`username``<input>`引照:

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> 僅使用元素引用來更改不與Blazor交互的空元素的內容。 當第三方 API 向元素提供內容時,此方案非常有用。 由於Blazor不與元素交互,因此元素的表示形式與 DOM 之間Blazor不存在衝突。
>
> 在下面的範例中,改變無序列表的內容*dangerous*`ul`( ) 是危險Blazor的,因為與 DOM 互動以填充此`<li>`元素的清單項目 ( ):
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
> 如果 JS 互斥`MyList`對元素 的內容進行Blazor變異, 並嘗試將差異應用於元素,則差異將不匹配 DOM。

就 .NET 代碼而言`ElementReference`,是不透明句柄。 唯一`ElementReference`能*用的就是通過*JS互通將其傳遞給JAVaScript代碼。 執行此操作時,JAVAScript 端代碼會接收一`HTMLElement`個實例,它可以與正常的 DOM API 一起使用。

例如,以下代碼定義了一個 .NET 擴展方法,該方法允許將焦點設置在元素上:

*範例 JsInterop.js*:

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

要呼叫不傳回值的 JavaScript 函數`IJSRuntime.InvokeVoidAsync`,請使用 。 以下代碼透過使用捕捉`ElementReference`的呼叫前面的 JavaScript 函數來設定對使用者名輸入的關注:

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

要使用擴充方法,請建立接收實例`IJSRuntime`的 靜態延伸方法:

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

該方法`Focus`直接在對象上調用。 以下範例假定`Focus`該方法可`JsInteropClasses`從 命名空間獲得:

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> 僅當`username`呈現元件后,才會填充該變數。 如果將未填充`ElementReference`的代碼傳送給 JavaScript 代碼,JAvaScript 代碼`null`會收到的值 。 您可以在元件完成呈現後操作元素引用(將初始焦點設定在元素上),請使用 On[後 RenderAsync 或 OnAfterRender 元件生命週期方法](xref:blazor/lifecycle#after-component-render)。

使用泛型型態並傳回值時,請使用[ValueTask\<T>](xref:System.Threading.Tasks.ValueTask`1):

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

`GenericMethod`直接在具有類型的物件上調用。 以下範例假定 可`GenericMethod``JsInteropClasses`從 命名空間獲得:

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a>跨元件的參考元素

`ElementReference`僅在元件`OnAfterRender`的方法中保證有效(元素引用為`struct`),因此元素引用不能在元件之間傳遞。

要使父元件使元素引用可用於其他元件,父元件可以:

* 允許子組件註冊回調。
* 使用傳遞的元素引用在`OnAfterRender`事件期間調用已註冊的回調。 此方法間接允許子元件與父元素引用進行交互。

下面的BlazorWebAssembly 範例說明了該方法。

wwwroot/index.html`<head>`*wwwroot/index.html*:

```html
<style>
    .red { color: red }
</style>
```

wwwroot/index.html`<body>`*wwwroot/index.html*:

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

*頁面/索引.razor(* 父元件):

```razor
@page "/"

<h1 @ref="_title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

*頁面/索引.razor.cs*:

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

*共用/調查提示.剃鬚刀*(子元件):

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

*共用/調查提示.剃鬚刀.cs*:

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

## <a name="harden-js-interop-calls"></a>強化 JS 互通呼叫

JS 互操作可能會由於網路錯誤而失敗,應視為不可靠。 默認情況下,伺服器應用Blazor在一分鐘后超時伺服器上的 JS 互通調用。 如果應用可以容忍更具攻擊性的超時(如 10 秒),請使用以下方法之一設置超時:

* 全域中的`Startup.ConfigureServices`指定逾時:

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* 在元件代碼中按每次呼叫,單一呼叫可以指定逾時:

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

有關資源耗盡的詳細資訊,請參閱<xref:security/blazor/server>。

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a>避免循環物件參考

在用戶端上不能序列化包含迴圈引用的物件:

* .NET 方法調用。
* 當返回類型具有迴圈引用時,JAVAScript 方法從 C# 調用。

有關詳細資訊,請參閱以下問題:

* [不支援迴圈引用,取兩個(點網/阿斯平內芯#20525)](https://github.com/dotnet/aspnetcore/issues/20525)
* [建議:在序列化時添加處理迴圈引用的機制(點網/運行時#30820)](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a>其他資源

* <xref:blazor/call-dotnet-from-javascript>
* [Interop元件.剃鬚範例(dotnet/AspNetCore GitHub 儲存庫,3.1 發佈分支)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [在伺服器應用中Blazor執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
