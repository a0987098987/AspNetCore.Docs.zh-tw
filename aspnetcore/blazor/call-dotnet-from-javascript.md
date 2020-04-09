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
# <a name="call-net-methods-from-javascript-functions-in-aspnet-core-opno-locblazor"></a><span data-ttu-id="11c1c-103">從 ASP.NET 核心中的 JavaScript 函數呼叫 .NET 方法Blazor</span><span class="sxs-lookup"><span data-stu-id="11c1c-103">Call .NET methods from JavaScript functions in ASP.NET Core Blazor</span></span>

<span data-ttu-id="11c1c-104">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)、[丹尼爾·羅斯](https://github.com/danroth27)、[沙希坎特·魯德拉瓦迪](http://wisne.co)和[盧克·萊瑟姆](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="11c1c-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), [Shashikant Rudrawadi](http://wisne.co), and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="11c1c-105">應用Blazor可以從 .NET 方法和 .NET 函數調用 JAvaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="11c1c-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="11c1c-106">這些方案稱為*JavaScript 互通性*(*JS 互通*)。</span><span class="sxs-lookup"><span data-stu-id="11c1c-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="11c1c-107">本文介紹從 JAVAScript 調用 .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-107">This article covers invoking .NET methods from JavaScript.</span></span> <span data-ttu-id="11c1c-108">有關如何從 .NET 調用 JavaScript<xref:blazor/call-javascript-from-dotnet>函數的資訊, 請參閱。</span><span class="sxs-lookup"><span data-stu-id="11c1c-108">For information on how to call JavaScript functions from .NET, see <xref:blazor/call-javascript-from-dotnet>.</span></span>

<span data-ttu-id="11c1c-109">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11c1c-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="static-net-method-call"></a><span data-ttu-id="11c1c-110">靜態 .NET 方法呼叫</span><span class="sxs-lookup"><span data-stu-id="11c1c-110">Static .NET method call</span></span>

<span data-ttu-id="11c1c-111">要從 JavaScript 呼叫靜態`DotNet.invokeMethod`.NET 方法,`DotNet.invokeMethodAsync`請使用或函數。</span><span class="sxs-lookup"><span data-stu-id="11c1c-111">To invoke a static .NET method from JavaScript, use the `DotNet.invokeMethod` or `DotNet.invokeMethodAsync` functions.</span></span> <span data-ttu-id="11c1c-112">傳遞要調用的靜態方法的標識符、包含函數的程式集的名稱以及任何參數。</span><span class="sxs-lookup"><span data-stu-id="11c1c-112">Pass in the identifier of the static method you wish to call, the name of the assembly containing the function, and any arguments.</span></span> <span data-ttu-id="11c1c-113">非同步版本優先支援Blazor伺服器方案。</span><span class="sxs-lookup"><span data-stu-id="11c1c-113">The asynchronous version is preferred to support Blazor Server scenarios.</span></span> <span data-ttu-id="11c1c-114">.NET 方法必須是公共的、靜態的,並且`[JSInvokable]`具有 該屬性。</span><span class="sxs-lookup"><span data-stu-id="11c1c-114">The .NET method must be public, static, and have the `[JSInvokable]` attribute.</span></span> <span data-ttu-id="11c1c-115">當前不支援調用打開的泛型方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-115">Calling open generic methods isn't currently supported.</span></span>

<span data-ttu-id="11c1c-116">範例應用包括一個用於返回陣列的`int`C# 方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-116">The sample app includes a C# method to return an `int` array.</span></span> <span data-ttu-id="11c1c-117">該`JSInvokable`屬性應用於方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-117">The `JSInvokable` attribute is applied to the method.</span></span>

<span data-ttu-id="11c1c-118">*頁面/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-118">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="11c1c-119">提供給用戶端的 JavaScript 呼叫 C# .NET 方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-119">JavaScript served to the client invokes the C# .NET method.</span></span>

<span data-ttu-id="11c1c-120">*wwwroot/範例JsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-120">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=8-14)]

<span data-ttu-id="11c1c-121">選擇**觸發器 .NET 靜態方法 ReturnArrayAsync**按鈕時,請檢查瀏覽器的 Web 開發人員工具中的控制台輸出。</span><span class="sxs-lookup"><span data-stu-id="11c1c-121">When the **Trigger .NET static method ReturnArrayAsync** button is selected, examine the console output in the browser's web developer tools.</span></span>

<span data-ttu-id="11c1c-122">主控台輸出為:</span><span class="sxs-lookup"><span data-stu-id="11c1c-122">The console output is:</span></span>

```console
Array(4) [ 1, 2, 3, 4 ]
```

<span data-ttu-id="11c1c-123">第四個陣列值被推送到傳`data.push(4);`回`ReturnArrayAsync`的陣列 ( )</span><span class="sxs-lookup"><span data-stu-id="11c1c-123">The fourth array value is pushed to the array (`data.push(4);`) returned by `ReturnArrayAsync`.</span></span>

<span data-ttu-id="11c1c-124">預設情況下,方法識別碼是方法名稱,但您可以使用`JSInvokableAttribute`建構函數指定不同的識別碼:</span><span class="sxs-lookup"><span data-stu-id="11c1c-124">By default, the method identifier is the method name, but you can specify a different identifier using the `JSInvokableAttribute` constructor:</span></span>

```csharp
@code {
    [JSInvokable("DifferentMethodName")]
    public static Task<int[]> ReturnArrayAsync()
    {
        return Task.FromResult(new int[] { 1, 2, 3 });
    }
}
```

<span data-ttu-id="11c1c-125">在用戶端 JavaScript 檔案中:</span><span class="sxs-lookup"><span data-stu-id="11c1c-125">In the client-side JavaScript file:</span></span>

```javascript
returnArrayAsyncJs: function () {
  DotNet.invokeMethodAsync('BlazorSample', 'DifferentMethodName')
    .then(data => {
      data.push(4);
      console.log(data);
    });
}
```

## <a name="instance-method-call"></a><span data-ttu-id="11c1c-126">實體方法呼叫</span><span class="sxs-lookup"><span data-stu-id="11c1c-126">Instance method call</span></span>

<span data-ttu-id="11c1c-127">您還可以從 JAVAScript 呼叫 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-127">You can also call .NET instance methods from JavaScript.</span></span> <span data-ttu-id="11c1c-128">要從 JavaScript 呼叫 .NET 實體方法,請執行:</span><span class="sxs-lookup"><span data-stu-id="11c1c-128">To invoke a .NET instance method from JavaScript:</span></span>

* <span data-ttu-id="11c1c-129">透過參考 JavaScript 傳遞 .NET 實體:</span><span class="sxs-lookup"><span data-stu-id="11c1c-129">Pass the .NET instance by reference to JavaScript:</span></span>
  * <span data-ttu-id="11c1c-130">對 靜態呼叫`DotNetObjectReference.Create`。</span><span class="sxs-lookup"><span data-stu-id="11c1c-130">Make a static call to `DotNetObjectReference.Create`.</span></span>
  * <span data-ttu-id="11c1c-131">在`DotNetObjectReference`實例中包裝實例並調`Create`用 實`DotNetObjectReference`例。</span><span class="sxs-lookup"><span data-stu-id="11c1c-131">Wrap the instance in a `DotNetObjectReference` instance and call `Create` on the `DotNetObjectReference` instance.</span></span> <span data-ttu-id="11c1c-132">處置`DotNetObjectReference`物件(本節稍後將出現一個示例)。</span><span class="sxs-lookup"><span data-stu-id="11c1c-132">Dispose of `DotNetObjectReference` objects (an example appears later in this section).</span></span>
* <span data-ttu-id="11c1c-133">使用`invokeMethod`呼`invokeMethodAsync`叫的實體上的 .NET 實例方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-133">Invoke .NET instance methods on the instance using the `invokeMethod` or `invokeMethodAsync` functions.</span></span> <span data-ttu-id="11c1c-134">從 JavaScript 調用其他 .NET 方法時,也可以作為參數傳遞 .NET 實例。</span><span class="sxs-lookup"><span data-stu-id="11c1c-134">The .NET instance can also be passed as an argument when invoking other .NET methods from JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="11c1c-135">示例應用將消息記錄到用戶端主控台。</span><span class="sxs-lookup"><span data-stu-id="11c1c-135">The sample app logs messages to the client-side console.</span></span> <span data-ttu-id="11c1c-136">有關範例應用示範的以下範例,請檢查瀏覽器的開發人員工具中的瀏覽器控制台輸出。</span><span class="sxs-lookup"><span data-stu-id="11c1c-136">For the following examples demonstrated by the sample app, examine the browser's console output in the browser's developer tools.</span></span>

<span data-ttu-id="11c1c-137">當**選擇觸發器 .NET 實例方法 HelloHelper.SayHello**`ExampleJsInterop.CallHelloHelperSayHello`按鈕時,`Blazor`將調用該按鈕 並將名稱傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="11c1c-137">When the **Trigger .NET instance method HelloHelper.SayHello** button is selected, `ExampleJsInterop.CallHelloHelperSayHello` is called and passes a name, `Blazor`, to the method.</span></span>

<span data-ttu-id="11c1c-138">*頁面/JsInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-138">*Pages/JsInterop.razor*:</span></span>

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

<span data-ttu-id="11c1c-139">`CallHelloHelperSayHello`使用 新的實體呼叫`sayHello`JavaScript`HelloHelper`函數 。</span><span class="sxs-lookup"><span data-stu-id="11c1c-139">`CallHelloHelperSayHello` invokes the JavaScript function `sayHello` with a new instance of `HelloHelper`.</span></span>

<span data-ttu-id="11c1c-140">*JsInterop 類別/範例JsInterop.cs*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-140">*JsInteropClasses/ExampleJsInterop.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/ExampleJsInterop.cs?name=snippet1&highlight=11-18)]

<span data-ttu-id="11c1c-141">*wwwroot/範例JsInterop.js*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-141">*wwwroot/exampleJsInterop.js*:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=15-18)]

<span data-ttu-id="11c1c-142">名稱傳遞給`HelloHelper`'的構造函數,該構造函數`HelloHelper.Name`設置 屬性。</span><span class="sxs-lookup"><span data-stu-id="11c1c-142">The name is passed to `HelloHelper`'s constructor, which sets the `HelloHelper.Name` property.</span></span> <span data-ttu-id="11c1c-143">執行 JavaScript`sayHello`函數`HelloHelper.SayHello`時`Hello, {Name}!`, 傳回 由 JavaScript 函數寫入主控台的消息。</span><span class="sxs-lookup"><span data-stu-id="11c1c-143">When the JavaScript function `sayHello` is executed, `HelloHelper.SayHello` returns the `Hello, {Name}!` message, which is written to the console by the JavaScript function.</span></span>

<span data-ttu-id="11c1c-144">*JsInterop類/HelloHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-144">*JsInteropClasses/HelloHelper.cs*:</span></span>

[!code-csharp[](./common/samples/3.x/BlazorWebAssemblySample/JsInteropClasses/HelloHelper.cs?name=snippet1&highlight=5,10-11)]

<span data-ttu-id="11c1c-145">瀏覽器的 Web 開發人員工具中的主控台輸出:</span><span class="sxs-lookup"><span data-stu-id="11c1c-145">Console output in the browser's web developer tools:</span></span>

```console
Hello, Blazor!
```

<span data-ttu-id="11c1c-146">為了避免記憶體洩漏並允許在創建 的元件上進行垃圾回收`DotNetObjectReference`,請採用以下方法之一:</span><span class="sxs-lookup"><span data-stu-id="11c1c-146">To avoid a memory leak and allow garbage collection on a component that creates a `DotNetObjectReference`, adopt one of the following approaches:</span></span>

* <span data-ttu-id="11c1c-147">釋放建立`DotNetObjectReference`實體的類別的物件:</span><span class="sxs-lookup"><span data-stu-id="11c1c-147">Dispose of the object in the class that created the `DotNetObjectReference` instance:</span></span>

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

  <span data-ttu-id="11c1c-148">`ExampleJsInterop`類別顯示的上述模式也可以在元件中實現:</span><span class="sxs-lookup"><span data-stu-id="11c1c-148">The preceding pattern shown in the `ExampleJsInterop` class can also be implemented in a component:</span></span>

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

* <span data-ttu-id="11c1c-149">當元件或類別不釋放 時`DotNetObjectReference`,透過呼`.dispose()`叫 : 釋放用戶端的物件:</span><span class="sxs-lookup"><span data-stu-id="11c1c-149">When the component or class doesn't dispose of the `DotNetObjectReference`, dispose of the object on the client by calling `.dispose()`:</span></span>

  ```javascript
  window.myFunction = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'MyMethod');
    dotnetHelper.dispose();
  }
  ```

## <a name="component-instance-method-call"></a><span data-ttu-id="11c1c-150">元件實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="11c1c-150">Component instance method call</span></span>

<span data-ttu-id="11c1c-151">要呼叫元件的 .NET 方法,請進行:</span><span class="sxs-lookup"><span data-stu-id="11c1c-151">To invoke a component's .NET methods:</span></span>

* <span data-ttu-id="11c1c-152">使用`invokeMethod``invokeMethodAsync`或函數對元件進行靜態方法調用。</span><span class="sxs-lookup"><span data-stu-id="11c1c-152">Use the `invokeMethod` or `invokeMethodAsync` function to make a static method call to the component.</span></span>
* <span data-ttu-id="11c1c-153">元件的靜態方法將呼叫包裝為其實例方法作為呼叫`Action`。</span><span class="sxs-lookup"><span data-stu-id="11c1c-153">The component's static method wraps the call to its instance method as an invoked `Action`.</span></span>

<span data-ttu-id="11c1c-154">在用戶端 JavaScript 中:</span><span class="sxs-lookup"><span data-stu-id="11c1c-154">In the client-side JavaScript:</span></span>

```javascript
function updateMessageCallerJS() {
  DotNet.invokeMethod('BlazorSample', 'UpdateMessageCaller');
}
```

<span data-ttu-id="11c1c-155">*頁面/JSInterop 元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-155">*Pages/JSInteropComponent.razor*:</span></span>

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

<span data-ttu-id="11c1c-156">當有多個元件(每個元件都具有要調用的實例方法)時,請使用幫助器類調用每個元件的實例`Action`方法(如 ss)。</span><span class="sxs-lookup"><span data-stu-id="11c1c-156">When there are several components, each with instance methods to call, use a helper class to invoke the instance methods (as `Action`s) of each component.</span></span>

<span data-ttu-id="11c1c-157">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="11c1c-157">In the following example:</span></span>

* <span data-ttu-id="11c1c-158">該`JSInterop`元件包含多個`ListItem`元件。</span><span class="sxs-lookup"><span data-stu-id="11c1c-158">The `JSInterop` component contains several `ListItem` components.</span></span>
* <span data-ttu-id="11c1c-159">每個`ListItem`元件由消息和按鈕組成。</span><span class="sxs-lookup"><span data-stu-id="11c1c-159">Each `ListItem` component is composed of a message and a button.</span></span>
* <span data-ttu-id="11c1c-160">選擇`ListItem`元件按鈕時,該方法`ListItem``UpdateMessage`將更改清單項文本並隱藏該按鈕。</span><span class="sxs-lookup"><span data-stu-id="11c1c-160">When a `ListItem` component button is selected, that `ListItem`'s `UpdateMessage` method changes the list item text and hides the button.</span></span>

<span data-ttu-id="11c1c-161">*MessageUpdateInvokeHelper.cs*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-161">*MessageUpdateInvokeHelper.cs*:</span></span>

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

<span data-ttu-id="11c1c-162">在用戶端 JavaScript 中:</span><span class="sxs-lookup"><span data-stu-id="11c1c-162">In the client-side JavaScript:</span></span>

```javascript
window.updateMessageCallerJS = (dotnetHelper) => {
    dotnetHelper.invokeMethod('BlazorSample', 'UpdateMessageCaller');
    dotnetHelper.dispose();
}
```

<span data-ttu-id="11c1c-163">*分享/清單項目.razor*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-163">*Shared/ListItem.razor*:</span></span>

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

<span data-ttu-id="11c1c-164">*頁面/JSInterop.razor*:</span><span class="sxs-lookup"><span data-stu-id="11c1c-164">*Pages/JSInterop.razor*:</span></span>

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

## <a name="avoid-circular-object-references"></a><span data-ttu-id="11c1c-165">避免循環物件參考</span><span class="sxs-lookup"><span data-stu-id="11c1c-165">Avoid circular object references</span></span>

<span data-ttu-id="11c1c-166">在用戶端上不能序列化包含迴圈引用的物件:</span><span class="sxs-lookup"><span data-stu-id="11c1c-166">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="11c1c-167">.NET 方法調用。</span><span class="sxs-lookup"><span data-stu-id="11c1c-167">.NET method calls.</span></span>
* <span data-ttu-id="11c1c-168">當返回類型具有迴圈引用時,JAVAScript 方法從 C# 調用。</span><span class="sxs-lookup"><span data-stu-id="11c1c-168">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="11c1c-169">有關詳細資訊,請參閱以下問題:</span><span class="sxs-lookup"><span data-stu-id="11c1c-169">For more information, see the following issues:</span></span>

* [<span data-ttu-id="11c1c-170">不支援迴圈引用,取兩個(點網/阿斯平內芯#20525)</span><span class="sxs-lookup"><span data-stu-id="11c1c-170">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="11c1c-171">建議:在序列化時添加處理迴圈引用的機制(點網/運行時#30820)</span><span class="sxs-lookup"><span data-stu-id="11c1c-171">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a><span data-ttu-id="11c1c-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="11c1c-172">Additional resources</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* [<span data-ttu-id="11c1c-173">Interop元件.剃鬚範例(dotnet/AspNetCore GitHub 儲存庫,3.1 發佈分支)</span><span class="sxs-lookup"><span data-stu-id="11c1c-173">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* <span data-ttu-id="11c1c-174">[在伺服器應用中Blazor執行大型資料傳輸](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span><span class="sxs-lookup"><span data-stu-id="11c1c-174">[Perform large data transfers in Blazor Server apps](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)</span></span>
