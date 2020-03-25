---
title: 從 ASP.NET Core Blazor 中的 JavaScript 函數呼叫 .NET 方法
author: guardrex
description: 瞭解如何從 Blazor 應用程式中的 JavaScript 函式叫用 .NET 方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/call-dotnet-from-javascript
ms.openlocfilehash: dbf44fe7923998c65119e42d97c304890fa95523
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218787"
---
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="1069c-103">從 ASP.NET Core Blazor 中的 JavaScript 函數呼叫 .NET 方法</span><span class="sxs-lookup"><span data-stu-id="1069c-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="1069c-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)、 [Shashikant](http://wisne.co)Rudrawadi 和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1069c-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Shashikant Rudrawadi](http://wisne.co), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="1069c-105">Blazor 應用程式可以從 JavaScript 函數的 .NET 方法和 .NET 方法叫用 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="1069c-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="1069c-106">這些案例稱為*JavaScript 互通性*（*JS interop*）。</span><span class="sxs-lookup"><span data-stu-id="1069c-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="1069c-107">本文涵蓋從 JavaScript 叫用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-107">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="1069c-108">如需如何從 .NET 呼叫 JavaScript 函式的詳細資訊，請參閱 <xref:blazor/call-javascript-from-dotnet>。</span><span class="sxs-lookup"><span data-stu-id="1069c-108">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="1069c-109">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1069c-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="1069c-110">靜態 .NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="1069c-110">Static .NET method call</span></span>

<span data-ttu-id="1069c-111">若要從 JavaScript 叫用靜態 .NET 方法，請使用 `DotNet.invokeMethod` 或 `DotNet.invokeMethodAsync` 函數。</span><span class="sxs-lookup"><span data-stu-id="1069c-111">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="1069c-112">傳入您想要呼叫之靜態方法的識別碼、包含函數的元件名稱，以及任何引數。</span><span class="sxs-lookup"><span data-stu-id="1069c-112">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="1069c-113">最好是非同步版本，以支援 Blazor 伺服器案例。</span><span class="sxs-lookup"><span data-stu-id="1069c-113">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="1069c-114">.NET 方法必須是公用的、靜態的，而且具有 `[JSInvokable]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="1069c-114">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="1069c-115">目前不支援呼叫開放式泛型方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-115">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="1069c-116">範例應用程式包含傳回C# `int` 陣列的方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-116">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="1069c-117">`JSInvokable` 屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-117">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="1069c-118">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="1069c-118">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="1069c-119">提供給用戶端的 JavaScript 會C#叫用 .net 方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-119">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="1069c-120">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="1069c-120">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="1069c-121">選取 [**觸發程式 .net 靜態方法 ReturnArrayAsync** ] 按鈕時，請檢查瀏覽器的 網頁程式開發人員工具中的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="1069c-121">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="1069c-122">主控台輸出如下：</span><span class="sxs-lookup"><span data-stu-id="1069c-122">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="1069c-123">第四個數組值會推送至 `ReturnArrayAsync`所傳回的陣列（`data.push(4);`）。</span><span class="sxs-lookup"><span data-stu-id="1069c-123">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="1069c-124">根據預設，方法識別碼是方法名稱，但您可以使用 `JSInvokableAttribute` 的函式來指定不同的識別碼：</span><span class="sxs-lookup"><span data-stu-id="1069c-124">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="1069c-125">在用戶端 JavaScript 檔案中：</span><span class="sxs-lookup"><span data-stu-id="1069c-125">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a><span data-ttu-id="1069c-126">實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="1069c-126">Instance method call</span></span>

<span data-ttu-id="1069c-127">您也可以從 JavaScript 呼叫 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-127">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="1069c-128">若要從 JavaScript 叫用 .NET 實例方法：</span><span class="sxs-lookup"><span data-stu-id="1069c-128">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="1069c-129">以傳址方式將 .NET 實例傳遞至 JavaScript：</span><span class="sxs-lookup"><span data-stu-id="1069c-129">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="1069c-130">對 `DotNetObjectReference.Create`進行靜態呼叫。</span><span class="sxs-lookup"><span data-stu-id="1069c-130">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="1069c-131">將實例包裝在 `DotNetObjectReference` 實例中，並在 `DotNetObjectReference` 實例上呼叫 `Create`。</span><span class="sxs-lookup"><span data-stu-id="1069c-131">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="1069c-132">處置 `DotNetObjectReference` 物件（本章節稍後會顯示一個範例）。</span><span class="sxs-lookup"><span data-stu-id="1069c-132">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="1069c-133">使用 `invokeMethod` 或 `invokeMethodAsync` 函數，在實例上叫用 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-133">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="1069c-134">從 JavaScript 叫用其他 .NET 方法時，也可以將 .NET 實例當做引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="1069c-134">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="1069c-135">範例應用程式會將訊息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="1069c-135">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="1069c-136">如需範例應用程式所示範的下列範例，請在瀏覽器的開發人員工具中檢查瀏覽器的主控台輸出。</span><span class="sxs-lookup"><span data-stu-id="1069c-136">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="1069c-137">選取 [**觸發程式 .net 實例方法 HelloHelper. SayHello** ] 按鈕時，會呼叫 `ExampleJsInterop.CallHelloHelperSayHello`，並將名稱 `Blazor`傳遞至方法。</span><span class="sxs-lookup"><span data-stu-id="1069c-137">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="1069c-138">*Pages/JsInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="1069c-138">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="1069c-139">`CallHelloHelperSayHello` 會使用 `HelloHelper`的新實例，叫用 JavaScript 函數 `sayHello`。</span><span class="sxs-lookup"><span data-stu-id="1069c-139">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="1069c-140">*JsInteropClasses/ExampleJsInterop .cs*：</span><span class="sxs-lookup"><span data-stu-id="1069c-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

<span data-ttu-id="1069c-141">*wwwroot/exampleJsInterop*：</span><span class="sxs-lookup"><span data-stu-id="1069c-141">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="1069c-142">名稱會傳遞至 `HelloHelper`的函式，以設定 `HelloHelper.Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="1069c-142">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="1069c-143">執行 JavaScript 函式 `sayHello` 時，`HelloHelper.SayHello` 會傳回 `Hello, {Name}!` 訊息，JavaScript 函式會將它寫入主控台。</span><span class="sxs-lookup"><span data-stu-id="1069c-143">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="1069c-144">*JsInteropClasses/HelloHelper .cs*：</span><span class="sxs-lookup"><span data-stu-id="1069c-144">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="1069c-145">瀏覽器 網頁程式開發人員工具中的主控台輸出：</span><span class="sxs-lookup"><span data-stu-id="1069c-145">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="1069c-146">若要避免記憶體流失，並允許在建立 `DotNetObjectReference`的元件上進行垃圾收集，請採用下列其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="1069c-146">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, adopt one of the following approaches:</span></span>

* <span data-ttu-id="1069c-147">在建立 `DotNetObjectReference` 實例的類別中處置物件：</span><span class="sxs-lookup"><span data-stu-id="1069c-147">Dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

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

  <span data-ttu-id="1069c-148">`ExampleJsInterop` 類別中所顯示的先前模式也可以在元件中執行：</span><span class="sxs-lookup"><span data-stu-id="1069c-148">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>

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

* <span data-ttu-id="1069c-149">當元件或類別未處置 `DotNetObjectReference`時，請藉由呼叫 `.dispose()`來處置用戶端上的物件：</span><span class="sxs-lookup"><span data-stu-id="1069c-149">When the component or class doesn't dispose of the `DotNetObjectReference`, dispose of the object on the client by calling `.dispose()`:</span></span>

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a><span data-ttu-id="1069c-150">元件實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="1069c-150">Component instance method call</span></span>

<span data-ttu-id="1069c-151">若要叫用元件的 .NET 方法：</span><span class="sxs-lookup"><span data-stu-id="1069c-151">To invoke a component's .NET methods:</span></span>

* <span data-ttu-id="1069c-152">使用 `invokeMethod` 或 `invokeMethodAsync` 函數，對元件進行靜態方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="1069c-152">Use the `invokeMethod` or `invokeMethodAsync` function to make a static method call to the component.</span></span>
* <span data-ttu-id="1069c-153">元件的靜態方法會將其實例方法的呼叫包裝為叫用的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="1069c-153">The component's static method wraps the call to its instance method as an invoked `Action`.</span></span>

<span data-ttu-id="1069c-154">在用戶端 JavaScript 中：</span><span class="sxs-lookup"><span data-stu-id="1069c-154">In the client-side JavaScript:</span></span>

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

<span data-ttu-id="1069c-155">*Pages/JSInteropComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="1069c-155">*Pages/JSInteropComponent.razor*:</span></span>

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

<span data-ttu-id="1069c-156">有數個元件時，每個都有實例方法來呼叫，請使用 helper 類別來叫用每個元件的實例方法（如 `Action`s）。</span><span class="sxs-lookup"><span data-stu-id="1069c-156">When there are several components, each with instance methods to call, use a helper class to invoke the instance methods (as `Action`s) of each component.</span></span>

<span data-ttu-id="1069c-157">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="1069c-157">In the following example:</span></span>

* <span data-ttu-id="1069c-158">`JSInterop` 元件包含數個 `ListItem` 元件。</span><span class="sxs-lookup"><span data-stu-id="1069c-158">The `JSInterop` component contains several `ListItem` components.</span></span>
* <span data-ttu-id="1069c-159">每個 `ListItem` 元件都是由一個訊息和一個按鈕所組成。</span><span class="sxs-lookup"><span data-stu-id="1069c-159">Each `ListItem` component is composed of a message and a button.</span></span>
* <span data-ttu-id="1069c-160">選取 [`ListItem` 元件] 按鈕時，`ListItem`的 `UpdateMessage` 方法會變更清單專案文字，並隱藏按鈕。</span><span class="sxs-lookup"><span data-stu-id="1069c-160">When a `ListItem` component button is selected, that `ListItem`'s `UpdateMessage` method changes the list item text and hides the button.</span></span>

<span data-ttu-id="1069c-161">*MessageUpdateInvokeHelper.cs*：</span><span class="sxs-lookup"><span data-stu-id="1069c-161">*MessageUpdateInvokeHelper.cs*:</span></span>

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

<span data-ttu-id="1069c-162">在用戶端 JavaScript 中：</span><span class="sxs-lookup"><span data-stu-id="1069c-162">In the client-side JavaScript:</span></span>

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

<span data-ttu-id="1069c-163">*共用/全部的 razor*：</span><span class="sxs-lookup"><span data-stu-id="1069c-163">*Shared/ListItem.razor*:</span></span>

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

<span data-ttu-id="1069c-164">*Pages/JSInterop. razor*：</span><span class="sxs-lookup"><span data-stu-id="1069c-164">*Pages/JSInterop.razor*:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="1069c-165">其他資源</span><span class="sxs-lookup"><span data-stu-id="1069c-165">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="1069c-166">InteropComponent razor 範例（dotnet/AspNetCore GitHub 存放庫，3.1 發行分支）</span><span class="sxs-lookup"><span data-stu-id="1069c-166">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="1069c-167">[在 Blazor 伺服器應用程式中執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="1069c-167">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
