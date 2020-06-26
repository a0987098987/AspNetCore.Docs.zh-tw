---
title: 從 ASP.NET Core 中的 JavaScript 函數呼叫 .NET 方法Blazor
author: guardrex
description: 瞭解如何從應用程式中的 JavaScript 函式叫用 .NET 方法 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: 91f2aa893c06728b4b71d010241a2cb5a307ae0b
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85400189"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-blazor"></a><span data-ttu-id="4dd05-103">從 ASP.NET Core 中的 JavaScript 函數呼叫 .NET 方法Blazor</span><span class="sxs-lookup"><span data-stu-id="4dd05-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="4dd05-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)、 [Shashikant](http://wisne.co)Rudrawadi 和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4dd05-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Shashikant Rudrawadi](http://wisne.co), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4dd05-105">Blazor應用程式可以從 javascript 函數的 .net 方法和 .net 方法叫用 javascript 函式。</span><span class="sxs-lookup"><span data-stu-id="4dd05-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="4dd05-106">這些案例稱為*JavaScript 互通性*（*JS interop*）。</span><span class="sxs-lookup"><span data-stu-id="4dd05-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="4dd05-107">本文涵蓋從 JavaScript 叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="4dd05-107">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="4dd05-108">如需如何從 .NET 呼叫 JavaScript 函式的詳細資訊，請參閱 <xref:blazor/call-javascript-from-dotnet> 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-108">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="4dd05-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="4dd05-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="4dd05-110">靜態 .NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="4dd05-110">Static .NET method call</span></span>

<span data-ttu-id="4dd05-111">若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。</span><span class="sxs-lookup"><span data-stu-id="4dd05-111">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="4dd05-112">傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。</span><span class="sxs-lookup"><span data-stu-id="4dd05-112">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="4dd05-113">非同步版本慣用於支援 Blazor Server 案例。</span><span class="sxs-lookup"><span data-stu-id="4dd05-113">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="4dd05-114">.NET 方法必須是公用的、靜態的，而且具有 [`[JSInvokable]`](xref:Microsoft.JSInterop.JSInvokableAttribute) 屬性。</span><span class="sxs-lookup"><span data-stu-id="4dd05-114">The .NET method must be public, static, and have the [`[JSInvokable]`](xref:Microsoft.JSInterop.JSInvokableAttribute) attribute.</span></span> <span data-ttu-id="4dd05-115">目前不支援呼叫開放式泛型方法。</span><span class="sxs-lookup"><span data-stu-id="4dd05-115">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="4dd05-116">範例應用程式包含可傳回陣列的 c # 方法 `int` 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-116">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="4dd05-117">[`[JSInvokable]`](xref:Microsoft.JSInterop.JSInvokableAttribute)屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="4dd05-117">The [`[JSInvokable]`](xref:Microsoft.JSInterop.JSInvokableAttribute) attribute is applied to the method.</span></span>

<span data-ttu-id="4dd05-118">`Pages/JsInterop.razor`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-118">`Pages/JsInterop.razor`:</span></span>

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

<span data-ttu-id="4dd05-119">提供給用戶端的 JavaScript 會叫用 c # .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="4dd05-119">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="4dd05-120">`wwwroot/exampleJsInterop.js`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-120">`wwwroot/exampleJsInterop.js`:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="4dd05-121">**`Trigger .NET static method ReturnArrayAsync`** 選取按鈕時，請檢查瀏覽器 網頁程式開發人員工具中的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="4dd05-121">When the **`Trigger .NET static method ReturnArrayAsync`** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="4dd05-122">主控台輸出如下：</span><span class="sxs-lookup"><span data-stu-id="4dd05-122">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="4dd05-123">第四個數組值會推送至所傳回的陣列（ `data.push(4);` ） `ReturnArrayAsync` 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-123">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="4dd05-124">根據預設，方法識別碼是方法名稱，但您可以使用屬性（attribute）函式來指定不同的識別碼 [`[JSInvokable]`](xref:Microsoft.JSInterop.JSInvokableAttribute) ：</span><span class="sxs-lookup"><span data-stu-id="4dd05-124">By default, the method identifier is the method name, but you can specify a different identifier using the [`[JSInvokable]`](xref:Microsoft.JSInterop.JSInvokableAttribute) attribute constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="4dd05-125">在用戶端 JavaScript 檔案中：</span><span class="sxs-lookup"><span data-stu-id="4dd05-125">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a><span data-ttu-id="4dd05-126">實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="4dd05-126">Instance method call</span></span>

<span data-ttu-id="4dd05-127">您也可以從 JavaScript 呼叫 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="4dd05-127">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="4dd05-128">若要從 JavaScript 叫用 .NET 實例方法：</span><span class="sxs-lookup"><span data-stu-id="4dd05-128">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="4dd05-129">以傳址方式將 .NET 實例傳遞至 JavaScript：</span><span class="sxs-lookup"><span data-stu-id="4dd05-129">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="4dd05-130">對進行靜態呼叫 <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-130">Make a static call to <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A?displayProperty=nameWithType>.</span></span>
  * <span data-ttu-id="4dd05-131">將實例包裝在 <xref:Microsoft.JSInterop.DotNetObjectReference> 實例中，並 <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A> 在實例上呼叫 <xref:Microsoft.JSInterop.DotNetObjectReference> 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-131">Wrap the instance in a <xref:Microsoft.JSInterop.DotNetObjectReference> instance and call <xref:Microsoft.JSInterop.DotNetObjectReference.Create%2A> on the <xref:Microsoft.JSInterop.DotNetObjectReference> instance.</span></span> <span data-ttu-id="4dd05-132">處置 <xref:Microsoft.JSInterop.DotNetObjectReference> 物件（本章節稍後會顯示一個範例）。</span><span class="sxs-lookup"><span data-stu-id="4dd05-132">Dispose of <xref:Microsoft.JSInterop.DotNetObjectReference> objects (an example appears later in this section).</span></span>
* <span data-ttu-id="4dd05-133">使用或函數，在實例上叫用 .NET 實例方法 `invokeMethod` `invokeMethodAsync` 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-133">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="4dd05-134">從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="4dd05-134">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="4dd05-135">範例應用程式會將訊息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="4dd05-135">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="4dd05-136">如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="4dd05-136">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="4dd05-137">**`Trigger .NET instance method HelloHelper.SayHello`** 選取按鈕時， `ExampleJsInterop.CallHelloHelperSayHello` 會呼叫，並將名稱傳遞 `Blazor` 至方法。</span><span class="sxs-lookup"><span data-stu-id="4dd05-137">When the **`Trigger .NET instance method HelloHelper.SayHello`** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="4dd05-138">`Pages/JsInterop.razor`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-138">`Pages/JsInterop.razor`:</span></span>

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

<span data-ttu-id="4dd05-139">`CallHelloHelperSayHello``sayHello`使用的新實例叫用 JavaScript 函數 `HelloHelper` 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-139">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="4dd05-140">`JsInteropClasses/ExampleJsInterop.cs`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-140">`JsInteropClasses/ExampleJsInterop.cs`:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

<span data-ttu-id="4dd05-141">`wwwroot/exampleJsInterop.js`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-141">`wwwroot/exampleJsInterop.js`:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="4dd05-142">名稱會傳遞至的函式 `HelloHelper` ，以設定 `HelloHelper.Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4dd05-142">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="4dd05-143">執行 JavaScript 函式時 `sayHello` ， `HelloHelper.SayHello` 會傳回 `Hello, {Name}!` 訊息，由 javascript 函數寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="4dd05-143">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="4dd05-144">`JsInteropClasses/HelloHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-144">`JsInteropClasses/HelloHelper.cs`:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="4dd05-145">瀏覽器 網頁程式開發人員工具中的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="4dd05-145">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="4dd05-146">若要避免記憶體流失，並允許在建立的元件上進行垃圾收集 <xref:Microsoft.JSInterop.DotNetObjectReference> ，請採用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="4dd05-146">To avoid a memory leak and allow garbage collection on a component that creates a <xref:Microsoft.JSInterop.DotNetObjectReference>, adopt one of the following approaches:</span></span>

* <span data-ttu-id="4dd05-147">處置建立實例之類別中的物件 <xref:Microsoft.JSInterop.DotNetObjectReference> ：</span><span class="sxs-lookup"><span data-stu-id="4dd05-147">Dispose of the object in the class that created the <xref:Microsoft.JSInterop.DotNetObjectReference> instance:</span></span>

  ```csharp
  public class ExampleJsInterop : IDisposable
  {
      private readonly IJSRuntime jsRuntime;
      private DotNetObjectReference<HelloHelper> objRef;

      public ExampleJsInterop(IJSRuntime jsRuntime)
      {
          this.jsRuntime = jsRuntime;
      }

      public ValueTask<string> CallHelloHelperSayHello(string name)
      {
          objRef = DotNetObjectReference.Create(new HelloHelper(name));

          return jsRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              objRef);
      }

      public void Dispose()
      {
          objRef?.Dispose();
      }
  }
  ```

  <span data-ttu-id="4dd05-148">類別中所顯示的上述模式 `ExampleJsInterop` 也可以在元件中執行：</span><span class="sxs-lookup"><span data-stu-id="4dd05-148">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>

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
      private DotNetObjectReference<HelloHelper> objRef;

      public async Task TriggerNetInstanceMethod()
      {
          objRef = DotNetObjectReference.Create(new HelloHelper("Blazor"));

          await JSRuntime.InvokeAsync<string>(
              "exampleJsFunctions.sayHello",
              objRef);
      }

      public void Dispose()
      {
          objRef?.Dispose();
      }
  }
  ```

* <span data-ttu-id="4dd05-149">當元件或類別未處置時 <xref:Microsoft.JSInterop.DotNetObjectReference> ，請呼叫來處置用戶端上的物件 `.dispose()` ：</span><span class="sxs-lookup"><span data-stu-id="4dd05-149">When the component or class doesn't dispose of the <xref:Microsoft.JSInterop.DotNetObjectReference>, dispose of the object on the client by calling `.dispose()`:</span></span>

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a><span data-ttu-id="4dd05-150">元件實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="4dd05-150">Component instance method call</span></span>

<span data-ttu-id="4dd05-151">若要叫用元件的 .NET 方法：</span><span class="sxs-lookup"><span data-stu-id="4dd05-151">To invoke a component's .NET methods:</span></span>

* <span data-ttu-id="4dd05-152">使用或函式 `invokeMethod` `invokeMethodAsync` ，對元件進行靜態方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="4dd05-152">Use the `invokeMethod` or `invokeMethodAsync` function to make a static method call to the component.</span></span>
* <span data-ttu-id="4dd05-153">元件的靜態方法會將其實例方法的呼叫包裝為叫用的 <xref:System.Action> 。</span><span class="sxs-lookup"><span data-stu-id="4dd05-153">The component's static method wraps the call to its instance method as an invoked <xref:System.Action>.</span></span>

<span data-ttu-id="4dd05-154">在用戶端 JavaScript 中：</span><span class="sxs-lookup"><span data-stu-id="4dd05-154">In the client-side JavaScript:</span></span>

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

<span data-ttu-id="4dd05-155">`Pages/JSInteropComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-155">`Pages/JSInteropComponent.razor`:</span></span>

```razor
@page "/JSInteropComponent"

<p>
    Message: @message
</p>

<p>
    <button onclick="updateMessageCallerJS()">Call JS Method</button>
</p>

@code {
    private static Action action;
    private string message = "Select the button.";

    protected override void OnInitialized()
    {
        action = UpdateMessage;
    }

    private void UpdateMessage()
    {
        message = "UpdateMessage Called!";
        StateHasChanged();
    }

    [JSInvokable]
    public static void UpdateMessageCaller()
    {
        action.Invoke();
    }
}
```

<span data-ttu-id="4dd05-156">有數個元件時，每個都有實例方法來呼叫，請使用 helper 類別來叫用每個元件的實例方法（as <xref:System.Action> ）。</span><span class="sxs-lookup"><span data-stu-id="4dd05-156">When there are several components, each with instance methods to call, use a helper class to invoke the instance methods (as <xref:System.Action>s) of each component.</span></span>

<span data-ttu-id="4dd05-157">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="4dd05-157">In the following example:</span></span>

* <span data-ttu-id="4dd05-158">此 `JSInteropExample` 元件包含多個 `ListItem` 元件。</span><span class="sxs-lookup"><span data-stu-id="4dd05-158">The `JSInteropExample` component contains several `ListItem` components.</span></span>
* <span data-ttu-id="4dd05-159">每個 `ListItem` 元件都是由一個訊息和一個按鈕所組成。</span><span class="sxs-lookup"><span data-stu-id="4dd05-159">Each `ListItem` component is composed of a message and a button.</span></span>
* <span data-ttu-id="4dd05-160">選取 [ `ListItem` 元件] 按鈕時，該 `ListItem` `UpdateMessage` 方法會變更清單專案文字，並隱藏按鈕。</span><span class="sxs-lookup"><span data-stu-id="4dd05-160">When a `ListItem` component button is selected, that `ListItem`'s `UpdateMessage` method changes the list item text and hides the button.</span></span>

<span data-ttu-id="4dd05-161">`MessageUpdateInvokeHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-161">`MessageUpdateInvokeHelper.cs`:</span></span>

```csharp
using System;
using Microsoft.JSInterop;

public class MessageUpdateInvokeHelper
{
    private Action action;

    public MessageUpdateInvokeHelper(Action action)
    {
        action = action;
    }

    [JSInvokable("BlazorSample")]
    public void UpdateMessageCaller()
    {
        action.Invoke();
    }
}
```

<span data-ttu-id="4dd05-162">在用戶端 JavaScript 中：</span><span class="sxs-lookup"><span data-stu-id="4dd05-162">In the client-side JavaScript:</span></span>

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

<span data-ttu-id="4dd05-163">`Shared/ListItem.razor`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-163">`Shared/ListItem.razor`:</span></span>

```razor
@inject IJSRuntime JsRuntime

<li>
    @message
    <button @onclick="InteropCall" style="display:@display">InteropCall</button>
</li>

@code {
    private string message = "Select one of these list item buttons.";
    private string display = "inline-block";
    private MessageUpdateInvokeHelper messageUpdateInvokeHelper;

    protected override void OnInitialized()
    {
        messageUpdateInvokeHelper = new MessageUpdateInvokeHelper(UpdateMessage);
    }

    protected async Task InteropCall()
    {
        await JsRuntime.InvokeVoidAsync("updateMessageCallerJS",
            DotNetObjectReference.Create(messageUpdateInvokeHelper));
    }

    private void UpdateMessage()
    {
        message = "UpdateMessage Called!";
        display = "none";
        StateHasChanged();
    }
}
```

<span data-ttu-id="4dd05-164">`Pages/JSInteropExample.razor`:</span><span class="sxs-lookup"><span data-stu-id="4dd05-164">`Pages/JSInteropExample.razor`:</span></span>

```razor
@page "/JSInteropExample"

<h1>List of components</h1>

<ul>
    <ListItem />
    <ListItem />
    <ListItem />
    <ListItem />
</ul>
```

[!INCLUDE[Share interop code in a class library](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a><span data-ttu-id="4dd05-165">避免迴圈物件參考</span><span class="sxs-lookup"><span data-stu-id="4dd05-165">Avoid circular object references</span></span>

<span data-ttu-id="4dd05-166">在用戶端上，包含迴圈參考的物件無法序列化為下列任一項：</span><span class="sxs-lookup"><span data-stu-id="4dd05-166">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="4dd05-167">.NET 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="4dd05-167">.NET method calls.</span></span>
* <span data-ttu-id="4dd05-168">當傳回類型具有迴圈參考時，來自 c # 的 JavaScript 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="4dd05-168">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="4dd05-169">如需詳細資訊，請參閱下列問題：</span><span class="sxs-lookup"><span data-stu-id="4dd05-169">For more information, see the following issues:</span></span>

* [<span data-ttu-id="4dd05-170">不支援迴圈參考，接受兩個（dotnet/aspnetcore #20525）</span><span class="sxs-lookup"><span data-stu-id="4dd05-170">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="4dd05-171">提案：在序列化時加入用來處理迴圈參考的機制（dotnet/runtime #30820）</span><span class="sxs-lookup"><span data-stu-id="4dd05-171">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a><span data-ttu-id="4dd05-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="4dd05-172">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="4dd05-173">`InteropComponent.razor`範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）</span><span class="sxs-lookup"><span data-stu-id="4dd05-173">`InteropComponent.razor` example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="4dd05-174">[在應用程式中執行大型資料傳輸 Blazor Server](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="4dd05-174">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
