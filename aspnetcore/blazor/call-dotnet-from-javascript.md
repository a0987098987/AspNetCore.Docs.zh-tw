---
title: 從 ASP.NET Core 中的 JavaScript 函數呼叫 .NET 方法Blazor
author: guardrex
description: 瞭解如何從應用程式中Blazor的 JavaScript 函式叫用 .net 方法。
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
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: 1bc75f0825b114a24def287bb7ccb11c27514f01
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767183"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-blazor"></a>從 ASP.NET Core 中的 JavaScript 函數呼叫 .NET 方法Blazor

By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)、 [Shashikant](http://wisne.co)Rudrawadi 和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor應用程式可以從 javascript 函數的 .net 方法和 .net 方法叫用 javascript 函式。 這些案例稱為*JavaScript 互通性*（*JS interop*）。

本文涵蓋從 JavaScript 叫用 .NET 方法。 如需如何從 .NET 呼叫 JavaScript 函式的詳細資訊<xref:blazor/call-javascript-from-dotnet>，請參閱。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="static-net-method-call"></a>靜態 .NET 方法呼叫

若要從 JavaScript 叫用靜態 .NET 方法，請`DotNet.invokeMethod`使用`DotNet.invokeMethodAsync`或函數。 傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。 若要支援Blazor伺服器案例，最好先進行非同步版本。 .NET 方法必須是公用的、靜態的，而且具有`[JSInvokable]`屬性。 目前不支援呼叫開放式泛型方法。

範例應用程式包含可傳回`int`陣列的 c # 方法。 `JSInvokable`屬性會套用至方法。

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

提供給用戶端的 JavaScript 會叫用 c # .NET 方法。

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。

主控台輸出如下：

```console
Array(4) [ 1, 2, 3, 4 ]
```

第四個數組值會推送至所傳回`data.push(4);`的陣列（ `ReturnArrayAsync`）。

根據預設，方法識別碼是方法名稱，但您可以使用此`JSInvokableAttribute`函數來指定不同的識別碼：

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

## <a name="instance-method-call"></a>實例方法呼叫

您也可以從 JavaScript 呼叫 .NET 實例方法。 若要從 JavaScript 叫用 .NET 實例方法：

* 以傳址方式將 .NET 實例傳遞至 JavaScript：
  * 對進行靜態呼叫`DotNetObjectReference.Create`。
  * 將實例包裝在`DotNetObjectReference`實例中，並`Create`在`DotNetObjectReference`實例上呼叫。 處置`DotNetObjectReference`物件（本章節稍後會顯示一個範例）。
* 使用`invokeMethod`或`invokeMethodAsync`函數，在實例上叫用 .net 實例方法。 從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。

> [!NOTE]
> 範例應用程式會將訊息記錄到用戶端主控台。 如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。

選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時`ExampleJsInterop.CallHelloHelperSayHello` ，會呼叫，並將名稱`Blazor`傳遞至方法。

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

`CallHelloHelperSayHello`使用的新實例`sayHello`叫用 JavaScript 函數`HelloHelper`。

*JsInteropClasses/ExampleJsInterop .cs*：

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

*wwwroot/exampleJsInterop*：

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

名稱會傳遞至的`HelloHelper`函式，以設定`HelloHelper.Name`屬性。 執行 JavaScript `sayHello`函式時， `HelloHelper.SayHello`會傳回`Hello, {Name}!`訊息，由 javascript 函數寫入主控台。

*JsInteropClasses/HelloHelper .cs*：

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

瀏覽器 網頁程式開發人員工具中的主控台輸出：

```console
Hello, Blazor!
```

若要避免記憶體流失，並允許在建立的`DotNetObjectReference`元件上進行垃圾收集，請採用下列其中一種方法：

* 處置建立`DotNetObjectReference`實例之類別中的物件：

  ```csharp
  public class ExampleJsInterop : IDisposable
  {
      private readonly IJSRuntime _jsRuntime;
      private DotNetObjectReference<HelloHelper> _objRef;

      public ExampleJsInterop(IJSRuntime jsRuntime)
      {
          _jsRuntime = jsRuntime;
      }

      public ValueTask<string> CallHelloHelperSayHello(string name)
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper(name));

          return _jsRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

  `ExampleJsInterop`類別中所顯示的上述模式也可以在元件中執行：

  ```razor
  @page "/JSInteropComponent"
  @using BlazorSample.JsInteropClasses
  @implements IDisposable
  @inject IJSRuntime JSRuntime

  <h1>JavaScript Interop</h1>

  <button type="button" class="btn btn-primary" @onclick="TriggerNetInstanceMethod">
      Trigger .NET instance method HelloHelper.SayHello
  </button>

  @code {
      private DotNetObjectReference<HelloHelper> _objRef;

      public async Task TriggerNetInstanceMethod()
      {
          _objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

          await JSRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              _objRef);
      }

      public void Dispose()
      {
          _objRef?.Dispose();
      }
  }
  ```

* 當元件或類別未處置時`DotNetObjectReference`，請呼叫`.dispose()`來處置用戶端上的物件：

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a>元件實例方法呼叫

若要叫用元件的 .NET 方法：

* 使用`invokeMethod`或`invokeMethodAsync`函式，對元件進行靜態方法呼叫。
* 元件的靜態方法會將其實例方法的呼叫包裝為叫用`Action`的。

在用戶端 JavaScript 中：

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

*Pages/JSInteropComponent. razor*：

```razor
@page "/JSInteropComponent"

<p>
    Message: @_message
</p>

<p>
    <button onclick="updateMessageCallerJS()">Call JS Method</button>
</p>

@code {
    private static Action _action;
    private string _message = "Select the button.";

    protected override void OnInitialized()
    {
        _action = UpdateMessage;
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        StateHasChanged();
    }

    [JSInvokable]
    public static void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

有數個元件時，每個都有實例方法來呼叫，請使用 helper 類別來叫用每個`Action`元件的實例方法（as）。

在下例中︰

* 此`JSInterop`元件包含多`ListItem`個元件。
* 每`ListItem`個元件都是由一個訊息和一個按鈕所組成。
* 選取 [ `ListItem`元件] 按鈕時，該`ListItem` `UpdateMessage`方法會變更清單專案文字，並隱藏按鈕。

*MessageUpdateInvokeHelper.cs*：

```csharp
using System;
using Microsoft.JSInterop;

public class MessageUpdateInvokeHelper
{
    private Action _action;

    public MessageUpdateInvokeHelper(Action action)
    {
        _action = action;
    }

    [JSInvokable("BlazorSample")]
    public void UpdateMessageCaller()
    {
        _action.Invoke();
    }
}
```

在用戶端 JavaScript 中：

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

*共用/全部的 razor*：

```razor
@inject IJSRuntime JsRuntime

<li>
    @_message
    <button @onclick="InteropCall" style="display:@_display">InteropCall</button>
</li>

@code {
    private string _message = "Select one of these list item buttons.";
    private string _display = "inline-block";
    private MessageUpdateInvokeHelper _messageUpdateInvokeHelper;

    protected override void OnInitialized()
    {
        _messageUpdateInvokeHelper = new MessageUpdateInvokeHelper(UpdateMessage);
    }

    protected async Task InteropCall()
    {
        await JsRuntime.InvokeVoidAsync("updateMessageCallerJS",
            DotNetObjectReference.Create(_messageUpdateInvokeHelper));
    }

    private void UpdateMessage()
    {
        _message = "UpdateMessage Called!";
        _display = "none";
        StateHasChanged();
    }
}
```

*Pages/JSInterop. razor*：

```razor
@page "/JSInterop"

<h1>List of components</h1>

<ul>
    <ListItem />
    <ListItem />
    <ListItem />
    <ListItem />
</ul>
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a>避免迴圈物件參考

在用戶端上，包含迴圈參考的物件無法序列化為下列任一項：

* .NET 方法呼叫。
* 當傳回類型具有迴圈參考時，來自 c # 的 JavaScript 方法呼叫。

如需詳細資訊，請參閱下列問題：

* [不支援迴圈參考，接受兩個（dotnet/aspnetcore #20525）](https://github.com/dotnet/aspnetcore/issues/20525)
* [提案：在序列化時加入用來處理迴圈參考的機制（dotnet/runtime #30820）](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a>其他資源

* <xref:blazor/call-javascript-from-dotnet>
* [InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [在伺服器應用程式中Blazor執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
