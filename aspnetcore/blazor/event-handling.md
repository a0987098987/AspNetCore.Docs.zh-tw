---
title: ASP.NET Core Blazor事件處理
author: guardrex
description: 瞭解Blazor的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: a9b0d0efd4afd4941bd4d93f33adecdf3288992f
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767066"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="7fdbb-103">ASP.NET Core Blazor 事件處理</span><span class="sxs-lookup"><span data-stu-id="7fdbb-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="7fdbb-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7fdbb-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="7fdbb-105">Razor 元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-105">Razor components provide event handling features.</span></span> <span data-ttu-id="7fdbb-106">對於名為[`@on{EVENT}`](xref:mvc/views/razor#onevent) （例如， `@onclick`）且具有委派類型值的 HTML 專案屬性，Razor 元件會將屬性的值視為事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-106">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="7fdbb-107">下列程式碼會在`UpdateHeading` UI 中選取按鈕時呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-107">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="7fdbb-108">下列程式碼會在`CheckChanged` UI 中變更核取方塊時呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-108">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="7fdbb-109">事件處理常式也可以是非同步<xref:System.Threading.Tasks.Task>，並且會傳回。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-109">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="7fdbb-110">不需要手動呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-110">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="7fdbb-111">例外狀況會在發生時記錄。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-111">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="7fdbb-112">在下列範例中， `UpdateHeading`當選取按鈕時，會以非同步方式呼叫：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-112">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

## <a name="event-argument-types"></a><span data-ttu-id="7fdbb-113">事件引數類型</span><span class="sxs-lookup"><span data-stu-id="7fdbb-113">Event argument types</span></span>

<span data-ttu-id="7fdbb-114">對於某些事件，則允許事件引數類型。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-114">For some events, event argument types are permitted.</span></span> <span data-ttu-id="7fdbb-115">只有在方法中使用事件種類時，才需要在方法呼叫中指定事件種類。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-115">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="7fdbb-116">下`EventArgs`表顯示支援的。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-116">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="7fdbb-117">事件</span><span class="sxs-lookup"><span data-stu-id="7fdbb-117">Event</span></span>            | <span data-ttu-id="7fdbb-118">類別</span><span class="sxs-lookup"><span data-stu-id="7fdbb-118">Class</span></span>                | <span data-ttu-id="7fdbb-119">DOM 事件和注意事項</span><span class="sxs-lookup"><span data-stu-id="7fdbb-119">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="7fdbb-120">剪貼簿</span><span class="sxs-lookup"><span data-stu-id="7fdbb-120">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="7fdbb-121">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-121">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="7fdbb-122">拖曳</span><span class="sxs-lookup"><span data-stu-id="7fdbb-122">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="7fdbb-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="7fdbb-124">`DataTransfer`並`DataTransferItem`按住拖曳的專案資料。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-124">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="7fdbb-125">錯誤</span><span class="sxs-lookup"><span data-stu-id="7fdbb-125">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="7fdbb-126">事件</span><span class="sxs-lookup"><span data-stu-id="7fdbb-126">Event</span></span>            | `EventArgs`          | <span data-ttu-id="7fdbb-127">*一般*</span><span class="sxs-lookup"><span data-stu-id="7fdbb-127">*General*</span></span><br><span data-ttu-id="7fdbb-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="7fdbb-129">*剪貼簿*</span><span class="sxs-lookup"><span data-stu-id="7fdbb-129">*Clipboard*</span></span><br><span data-ttu-id="7fdbb-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="7fdbb-131">*輸入*</span><span class="sxs-lookup"><span data-stu-id="7fdbb-131">*Input*</span></span><br><span data-ttu-id="7fdbb-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="7fdbb-133">*媒體*</span><span class="sxs-lookup"><span data-stu-id="7fdbb-133">*Media*</span></span><br><span data-ttu-id="7fdbb-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="7fdbb-135">Focus</span><span class="sxs-lookup"><span data-stu-id="7fdbb-135">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="7fdbb-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="7fdbb-137">不包含的`relatedTarget`支援。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-137">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="7fdbb-138">輸入</span><span class="sxs-lookup"><span data-stu-id="7fdbb-138">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="7fdbb-139">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-139">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="7fdbb-140">鍵盤</span><span class="sxs-lookup"><span data-stu-id="7fdbb-140">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="7fdbb-141">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-141">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="7fdbb-142">滑鼠</span><span class="sxs-lookup"><span data-stu-id="7fdbb-142">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="7fdbb-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="7fdbb-144">滑鼠指標</span><span class="sxs-lookup"><span data-stu-id="7fdbb-144">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="7fdbb-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="7fdbb-146">滑鼠滾輪</span><span class="sxs-lookup"><span data-stu-id="7fdbb-146">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="7fdbb-147">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-147">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="7fdbb-148">進度</span><span class="sxs-lookup"><span data-stu-id="7fdbb-148">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="7fdbb-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="7fdbb-150">觸控</span><span class="sxs-lookup"><span data-stu-id="7fdbb-150">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="7fdbb-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="7fdbb-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="7fdbb-152">`TouchPoint`代表觸控裝置上的單一連絡人點。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-152">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="7fdbb-153">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-153">For more information, see the following resources:</span></span>

* <span data-ttu-id="7fdbb-154">[ASP.NET Core 參考來源中的 EventArgs 類別（dotnet/aspnetcore release/3.1 分支）](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-154">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="7fdbb-155">[MDN web 檔： GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash;包含哪些 HTML 元素支援每個 DOM 事件的資訊。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-155">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="7fdbb-156">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="7fdbb-156">Lambda expressions</span></span>

<span data-ttu-id="7fdbb-157">您也可以使用[Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-157">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="7fdbb-158">關閉其他值（例如反覆運算一組專案時）通常會很方便。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-158">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="7fdbb-159">下列範例會建立三個按鈕，在 UI 中`UpdateHeading`選取時，每個`MouseEventArgs`都會呼叫傳遞事件引數`buttonNumber`（）和其按鈕編號（）：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-159">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="7fdbb-160">請**不要在**lambda 運算式中直接`i`使用`for`迴圈中的迴圈變數（）。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-160">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="7fdbb-161">否則，所有 lambda 運算式都會使用相同的變數， `i`使的值在所有 lambda 中都相同。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-161">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="7fdbb-162">請一律在本機變數中捕捉其值`buttonNumber` （在上述範例中為），然後使用它。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-162">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="7fdbb-163">App eventcallback</span><span class="sxs-lookup"><span data-stu-id="7fdbb-163">EventCallback</span></span>

<span data-ttu-id="7fdbb-164">有一個常見的嵌套元件案例，就是在子元件事件發生&mdash;時（例如，當子系中發生`onclick`事件時），想要執行父元件的方法。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-164">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="7fdbb-165">若要在元件之間公開事件， `EventCallback`請使用。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-165">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="7fdbb-166">父元件可以將回呼方法指派給子元件的`EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-166">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="7fdbb-167">範例`ChildComponent`應用程式（*Components/ChildComponent*）中的會示範如何設定按鈕的`onclick`處理常式，以接收來自範例的`EventCallback` `ParentComponent`委派。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-167">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="7fdbb-168">`EventCallback`會使用`MouseEventArgs`輸入，這適用于來自週邊裝置`onclick`的事件：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-168">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="7fdbb-169">會`ParentComponent`將子系的`EventCallback<T>` （`OnClickCallback`）設定為`ShowMessage`其方法。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-169">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="7fdbb-170">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-170">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="7fdbb-171">當您在中選取按鈕時`ChildComponent`：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-171">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="7fdbb-172">會`ParentComponent`呼叫`ShowMessage`的方法。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-172">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="7fdbb-173">`_messageText`會更新並顯示在中`ParentComponent`。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-173">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="7fdbb-174">回呼的方法[StateHasChanged](xref:blazor/lifecycle#state-changes) （`ShowMessage`）中不需要呼叫 StateHasChanged。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-174">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="7fdbb-175">`StateHasChanged`會自動呼叫來 rerender `ParentComponent`，就像子事件會觸發元件 rerendering 在子系內執行的事件處理常式一樣。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-175">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="7fdbb-176">`EventCallback`和`EventCallback<T>`允許非同步委派。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-176">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="7fdbb-177">`EventCallback<T>`是強型別，而且需要特定的引數類型。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-177">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="7fdbb-178">`EventCallback`是弱型別，並允許任何引數類型。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-178">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="7fdbb-179">使用叫`EventCallback`用`EventCallback<T>` `InvokeAsync`或，並等待<xref:System.Threading.Tasks.Task>：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-179">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="7fdbb-180">針對`EventCallback`事件`EventCallback<T>`處理和系結元件參數使用和。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-180">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="7fdbb-181">慣用強`EventCallback<T>`型別`EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-181">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="7fdbb-182">`EventCallback<T>`為元件的使用者提供更好的錯誤意見反應。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-182">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="7fdbb-183">與其他 UI 事件處理常式類似，指定事件參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-183">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="7fdbb-184">當`EventCallback`沒有任何值傳遞給回呼時，請使用。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-184">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="7fdbb-185">防止預設動作</span><span class="sxs-lookup"><span data-stu-id="7fdbb-185">Prevent default actions</span></span>

<span data-ttu-id="7fdbb-186">[`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault)使用指示詞屬性可防止事件的預設動作。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-186">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="7fdbb-187">在輸入裝置上選取索引鍵，且元素焦點位於文字方塊上時，瀏覽器通常會在文字方塊中顯示索引鍵的字元。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-187">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="7fdbb-188">在下列範例中，會藉由指定`@onkeypress:preventDefault`指示詞屬性來防止預設行為。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-188">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="7fdbb-189">計數器會遞增，而且索引**+** 鍵不會被捕捉`<input>`到元素的值中：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-189">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="7fdbb-190">指定不`@on{EVENT}:preventDefault`含值的屬性，相當於`@on{EVENT}:preventDefault="true"`。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-190">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="7fdbb-191">屬性的值也可以是運算式。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-191">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="7fdbb-192">在下列範例中， `_shouldPreventDefault`是設定`bool`為`true`或`false`的欄位：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-192">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="7fdbb-193">不需要事件處理常式來防止預設動作。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-193">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="7fdbb-194">事件處理常式和防止預設動作案例可以獨立使用。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-194">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="7fdbb-195">停止事件傳播</span><span class="sxs-lookup"><span data-stu-id="7fdbb-195">Stop event propagation</span></span>

<span data-ttu-id="7fdbb-196">[`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation)使用指示詞屬性來停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="7fdbb-196">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="7fdbb-197">在下列範例中，選取核取方塊可防止從第二個子`<div>`系的 click 事件傳播到父系`<div>`：</span><span class="sxs-lookup"><span data-stu-id="7fdbb-197">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
