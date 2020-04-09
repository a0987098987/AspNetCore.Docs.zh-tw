---
title: 從 ASP.NET 核心中的 JavaScript 函數呼叫 .NET 方法Blazor
author: guardrex
description: 瞭解如何從應用中的BlazorJAVAScript 函數呼叫 .NET 方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: e2344dd15efd243a405373b6cf0362f28b48173a
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976946"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a>從 ASP.NET 核心中的 JavaScript 函數呼叫 .NET 方法Blazor

哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)、[丹尼爾·羅斯](https://github.com/danroth27)、[沙希坎特·魯德拉瓦迪](http://wisne.co)和[盧克·萊瑟姆](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

應用Blazor可以從 .NET 方法和 .NET 函數調用 JAvaScript 函數。 這些方案稱為*JavaScript 互通性*(*JS 互通*)。

本文介紹從 JAVAScript 調用 .NET 方法。 有關如何從 .NET 調用 JavaScript<xref:blazor/call-javascript-from-dotnet>函數的資訊, 請參閱。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))

## <a name="static-net-method-call"></a>靜態 .NET 方法呼叫

要從 JavaScript 呼叫靜態`DotNet.invokeMethod`.NET 方法,`DotNet.invokeMethodAsync`請使用或函數。 傳遞要調用的靜態方法的標識符、包含函數的程式集的名稱以及任何參數。 非同步版本優先支援Blazor伺服器方案。 .NET 方法必須是公共的、靜態的,並且`[JSInvokable]`具有 該屬性。 當前不支援調用打開的泛型方法。

範例應用包括一個用於返回陣列的`int`C# 方法。 該`JSInvokable`屬性應用於方法。

*頁面/JsInterop.razor*:

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

提供給用戶端的 JavaScript 呼叫 C# .NET 方法。

*wwwroot/範例JsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

選擇**觸發器 .NET 靜態方法 ReturnArrayAsync**按鈕時,請檢查瀏覽器的 Web 開發人員工具中的控制台輸出。

主控台輸出為:

```console
Array(4) [ 1, 2, 3, 4 ]
```

第四個陣列值被推送到傳`data.push(4);`回`ReturnArrayAsync`的陣列 ( )

預設情況下,方法識別碼是方法名稱,但您可以使用`JSInvokableAttribute`建構函數指定不同的識別碼:

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

在用戶端 JavaScript 檔案中:

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a>實體方法呼叫

您還可以從 JAVAScript 呼叫 .NET 實例方法。 要從 JavaScript 呼叫 .NET 實體方法,請執行:

* 透過參考 JavaScript 傳遞 .NET 實體:
  * 對 靜態呼叫`DotNetObjectReference.Create`。
  * 在`DotNetObjectReference`實例中包裝實例並調`Create`用 實`DotNetObjectReference`例。 處置`DotNetObjectReference`物件(本節稍後將出現一個示例)。
* 使用`invokeMethod`呼`invokeMethodAsync`叫的實體上的 .NET 實例方法。 從 JavaScript 調用其他 .NET 方法時,也可以作為參數傳遞 .NET 實例。

> [!NOTE]
> 示例應用將消息記錄到用戶端主控台。 有關範例應用示範的以下範例,請檢查瀏覽器的開發人員工具中的瀏覽器控制台輸出。

當**選擇觸發器 .NET 實例方法 HelloHelper.SayHello**`ExampleJsInterop.CallHelloHelperSayHello`按鈕時,`Blazor`將調用該按鈕 並將名稱傳遞給方法。

*頁面/JsInterop.razor*:

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

`CallHelloHelperSayHello`使用 新的實體呼叫`sayHello`JavaScript`HelloHelper`函數 。

*JsInterop 類別/範例JsInterop.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

*wwwroot/範例JsInterop.js*:

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

名稱傳遞給`HelloHelper`'的構造函數,該構造函數`HelloHelper.Name`設置 屬性。 執行 JavaScript`sayHello`函數`HelloHelper.SayHello`時`Hello, {Name}!`, 傳回 由 JavaScript 函數寫入主控台的消息。

*JsInterop類/HelloHelper.cs*:

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

瀏覽器的 Web 開發人員工具中的主控台輸出:

```console
Hello, Blazor!
```

為了避免記憶體洩漏並允許在創建 的元件上進行垃圾回收`DotNetObjectReference`,請採用以下方法之一:

* 釋放建立`DotNetObjectReference`實體的類別的物件:

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

  `ExampleJsInterop`類別顯示的上述模式也可以在元件中實現:

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

* 當元件或類別不釋放 時`DotNetObjectReference`,透過呼`.dispose()`叫 : 釋放用戶端的物件:

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a>元件實例方法呼叫

要呼叫元件的 .NET 方法,請進行:

* 使用`invokeMethod``invokeMethodAsync`或函數對元件進行靜態方法調用。
* 元件的靜態方法將呼叫包裝為其實例方法作為呼叫`Action`。

在用戶端 JavaScript 中:

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

*頁面/JSInterop 元件.razor*:

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

當有多個元件(每個元件都具有要調用的實例方法)時,請使用幫助器類調用每個元件的實例`Action`方法(如 ss)。

在下例中︰

* 該`JSInterop`元件包含多個`ListItem`元件。
* 每個`ListItem`元件由消息和按鈕組成。
* 選擇`ListItem`元件按鈕時,該方法`ListItem``UpdateMessage`將更改清單項文本並隱藏該按鈕。

*MessageUpdateInvokeHelper.cs*:

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

在用戶端 JavaScript 中:

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

*分享/清單項目.razor*:

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

*頁面/JSInterop.razor*:

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

## <a name="avoid-circular-object-references"></a>避免循環物件參考

在用戶端上不能序列化包含迴圈引用的物件:

* .NET 方法調用。
* 當返回類型具有迴圈引用時,JAVAScript 方法從 C# 調用。

有關詳細資訊,請參閱以下問題:

* [不支援迴圈引用,取兩個(點網/阿斯平內芯#20525)](https://github.com/dotnet/aspnetcore/issues/20525)
* [建議:在序列化時添加處理迴圈引用的機制(點網/運行時#30820)](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a>其他資源

* <xref:blazor/call-javascript-from-dotnet>
* [Interop元件.剃鬚範例(dotnet/AspNetCore GitHub 儲存庫,3.1 發佈分支)](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [在伺服器應用中Blazor執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
