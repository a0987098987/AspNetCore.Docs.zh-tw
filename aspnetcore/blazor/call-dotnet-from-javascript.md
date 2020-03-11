---
title: 從 ASP.NET Core Blazor 中的 JavaScript 函數呼叫 .NET 方法
author: guardrex
description: 瞭解如何從 Blazor 應用程式中的 JavaScript 函式叫用 .NET 方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/19/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: f4964341e261c65269eedafbbd6e676c1054f427
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659443"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a>從 ASP.NET Core Blazor 中的 JavaScript 函數呼叫 .NET 方法

By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor 應用程式可以從 JavaScript 函數的 .NET 方法和 .NET 方法叫用 JavaScript 函式。 這些案例稱為*JavaScript 互通性*（*JS interop*）。

本文涵蓋從 JavaScript 叫用 .NET 方法。 如需如何從 .NET 呼叫 JavaScript 函式的詳細資訊，請參閱 <xref:blazor/call-javascript-from-dotnet>。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="static-net-method-call"></a>靜態 .NET 方法呼叫

若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。 傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。 最好是非同步版本，以支援 Blazor 伺服器案例。 .NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 屬性。 目前不支援呼叫開放式泛型方法。

範例應用程式包含傳回C# `int` 陣列的方法。 `JSInvokable` 屬性會套用至方法。

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

## <a name="instance-method-call"></a>實例方法呼叫

您也可以從 JavaScript 呼叫 .NET 實例方法。 若要從 JavaScript 叫用 .NET 實例方法：

* 以傳址方式將 .NET 實例傳遞至 JavaScript：
  * 對 `DotNetObjectReference.Create`進行靜態呼叫。
  * 將實例包裝在 `DotNetObjectReference` 實例中，並在 `DotNetObjectReference` 實例上呼叫 `Create`。 處置 `DotNetObjectReference` 物件（本章節稍後會顯示一個範例）。
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

若要避免記憶體流失，並允許在建立 `DotNetObjectReference`的元件上進行垃圾收集，請處置建立 `DotNetObjectReference` 實例之類別中的物件：

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
  
`ExampleJsInterop` 類別中所顯示的先前模式也可以在元件中執行：
  
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

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="additional-resources"></a>其他資源

* <xref:blazor/call-javascript-from-dotnet>
* [InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [在 Blazor 伺服器應用程式中執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
