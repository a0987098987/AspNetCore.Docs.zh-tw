---
title: ASP.NET Core Blazor 事件處理
author: guardrex
description: 瞭解 Blazor的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: c144841805e07a136f153c25a78c7f9af7c5801b
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511362"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="c5792-103">ASP.NET Core Blazor 事件處理</span><span class="sxs-lookup"><span data-stu-id="c5792-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="c5792-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="c5792-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="c5792-105">Razor 元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="c5792-105">Razor components provide event handling features.</span></span> <span data-ttu-id="c5792-106">對於名為[`@on{EVENT}`](xref:mvc/views/razor#onevent)的 HTML 專案屬性（例如，`@onclick`）與委派類型的值，Razor 元件會將屬性的值視為事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="c5792-106">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="c5792-107">下列程式碼會在 UI 中選取按鈕時，呼叫 `UpdateHeading` 方法：</span><span class="sxs-lookup"><span data-stu-id="c5792-107">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="c5792-108">下列程式碼會在 UI 中變更核取方塊時呼叫 `CheckChanged` 方法：</span><span class="sxs-lookup"><span data-stu-id="c5792-108">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="c5792-109">事件處理常式也可以是非同步，並傳回 <xref:System.Threading.Tasks.Task>。</span><span class="sxs-lookup"><span data-stu-id="c5792-109">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="c5792-110">不需要手動呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)。</span><span class="sxs-lookup"><span data-stu-id="c5792-110">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="c5792-111">例外狀況會在發生時記錄。</span><span class="sxs-lookup"><span data-stu-id="c5792-111">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="c5792-112">在下列範例中，當選取按鈕時，會以非同步方式呼叫 `UpdateHeading`：</span><span class="sxs-lookup"><span data-stu-id="c5792-112">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

## <a name="event-argument-types"></a><span data-ttu-id="c5792-113">事件引數類型</span><span class="sxs-lookup"><span data-stu-id="c5792-113">Event argument types</span></span>

<span data-ttu-id="c5792-114">對於某些事件，則允許事件引數類型。</span><span class="sxs-lookup"><span data-stu-id="c5792-114">For some events, event argument types are permitted.</span></span> <span data-ttu-id="c5792-115">只有在方法中使用事件種類時，才需要在方法呼叫中指定事件種類。</span><span class="sxs-lookup"><span data-stu-id="c5792-115">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="c5792-116">下表顯示支援的 `EventArgs`。</span><span class="sxs-lookup"><span data-stu-id="c5792-116">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="c5792-117">事件</span><span class="sxs-lookup"><span data-stu-id="c5792-117">Event</span></span>            | <span data-ttu-id="c5792-118">類別</span><span class="sxs-lookup"><span data-stu-id="c5792-118">Class</span></span>                | <span data-ttu-id="c5792-119">DOM 事件和注意事項</span><span class="sxs-lookup"><span data-stu-id="c5792-119">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="c5792-120">剪貼簿</span><span class="sxs-lookup"><span data-stu-id="c5792-120">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="c5792-121">`oncut`、`oncopy`、`onpaste`</span><span class="sxs-lookup"><span data-stu-id="c5792-121">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="c5792-122">拖曳</span><span class="sxs-lookup"><span data-stu-id="c5792-122">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="c5792-123">`ondrag`、`ondragstart`、`ondragenter`、`ondragleave`、`ondragover`、`ondrop`、`ondragend`</span><span class="sxs-lookup"><span data-stu-id="c5792-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="c5792-124">`DataTransfer` 和 `DataTransferItem` 保存拖曳的專案資料。</span><span class="sxs-lookup"><span data-stu-id="c5792-124">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="c5792-125">錯誤</span><span class="sxs-lookup"><span data-stu-id="c5792-125">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="c5792-126">事件</span><span class="sxs-lookup"><span data-stu-id="c5792-126">Event</span></span>            | `EventArgs`          | <span data-ttu-id="c5792-127">*一般*</span><span class="sxs-lookup"><span data-stu-id="c5792-127">*General*</span></span><br><span data-ttu-id="c5792-128">`onactivate`、`onbeforeactivate`、`onbeforedeactivate`、`ondeactivate`、`onended`、`onfullscreenchange`、`onfullscreenerror`、`onloadeddata`、`onloadedmetadata`、`onpointerlockchange`、`onpointerlockerror`、`onreadystatechange`、`onscroll`</span><span class="sxs-lookup"><span data-stu-id="c5792-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="c5792-129">*剪貼簿*</span><span class="sxs-lookup"><span data-stu-id="c5792-129">*Clipboard*</span></span><br><span data-ttu-id="c5792-130">`onbeforecut`、`onbeforecopy`、`onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="c5792-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="c5792-131">*輸入*</span><span class="sxs-lookup"><span data-stu-id="c5792-131">*Input*</span></span><br><span data-ttu-id="c5792-132">`oninvalid`、`onreset`、`onselect`、`onselectionchange`、`onselectstart`、`onsubmit`</span><span class="sxs-lookup"><span data-stu-id="c5792-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="c5792-133">*媒體*</span><span class="sxs-lookup"><span data-stu-id="c5792-133">*Media*</span></span><br><span data-ttu-id="c5792-134">`oncanplay`、`oncanplaythrough`、`oncuechange`、`ondurationchange`、`onemptied`、`onpause`、`onplay`、`onplaying`、`onratechange`、`onseeked`、`onseeking`、`onstalled`、`onstop`、`onsuspend`、`ontimeupdate`、`onvolumechange`、`onwaiting`</span><span class="sxs-lookup"><span data-stu-id="c5792-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="c5792-135">Focus</span><span class="sxs-lookup"><span data-stu-id="c5792-135">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="c5792-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="c5792-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="c5792-137">不包含 `relatedTarget`的支援。</span><span class="sxs-lookup"><span data-stu-id="c5792-137">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="c5792-138">輸入</span><span class="sxs-lookup"><span data-stu-id="c5792-138">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="c5792-139">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="c5792-139">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="c5792-140">鍵盤</span><span class="sxs-lookup"><span data-stu-id="c5792-140">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="c5792-141">`onkeydown`、`onkeypress`、`onkeyup`</span><span class="sxs-lookup"><span data-stu-id="c5792-141">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="c5792-142">滑鼠</span><span class="sxs-lookup"><span data-stu-id="c5792-142">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="c5792-143">`onclick`、`oncontextmenu`、`ondblclick`、`onmousedown`、`onmouseup`、`onmouseover`、`onmousemove`、`onmouseout`</span><span class="sxs-lookup"><span data-stu-id="c5792-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="c5792-144">滑鼠指標</span><span class="sxs-lookup"><span data-stu-id="c5792-144">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="c5792-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="c5792-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="c5792-146">滑鼠滾輪</span><span class="sxs-lookup"><span data-stu-id="c5792-146">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="c5792-147">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="c5792-147">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="c5792-148">進度</span><span class="sxs-lookup"><span data-stu-id="c5792-148">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="c5792-149">`onabort`、`onload`、`onloadend`、`onloadstart`、`onprogress`、`ontimeout`</span><span class="sxs-lookup"><span data-stu-id="c5792-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="c5792-150">觸控</span><span class="sxs-lookup"><span data-stu-id="c5792-150">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="c5792-151">`ontouchstart`、`ontouchend`、`ontouchmove`、`ontouchenter`、`ontouchleave`、`ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="c5792-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="c5792-152">`TouchPoint` 代表觸控式裝置上的單一連絡人點。</span><span class="sxs-lookup"><span data-stu-id="c5792-152">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="c5792-153">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="c5792-153">For more information, see the following resources:</span></span>

* <span data-ttu-id="c5792-154">[ASP.NET Core 參考來源中的 EventArgs 類別（dotnet/aspnetcore release/3.1 分支）](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)。</span><span class="sxs-lookup"><span data-stu-id="c5792-154">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="c5792-155">[MDN web 檔： GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; 包含哪些 HTML 元素支援每個 DOM 事件的資訊。</span><span class="sxs-lookup"><span data-stu-id="c5792-155">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="c5792-156">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="c5792-156">Lambda expressions</span></span>

<span data-ttu-id="c5792-157">您也可以使用[Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)：</span><span class="sxs-lookup"><span data-stu-id="c5792-157">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="c5792-158">關閉其他值（例如反覆運算一組專案時）通常會很方便。</span><span class="sxs-lookup"><span data-stu-id="c5792-158">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="c5792-159">下列範例會建立三個按鈕，其中每個都會呼叫 `UpdateHeading` 在 UI 中選取時傳遞事件引數（`MouseEventArgs`）和其按鈕編號（`buttonNumber`）：</span><span class="sxs-lookup"><span data-stu-id="c5792-159">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="c5792-160">請勿直接在 lambda 運算式**中使用 `for`** 迴圈中的迴圈變數（`i`）。</span><span class="sxs-lookup"><span data-stu-id="c5792-160">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="c5792-161">否則，所有 lambda 運算式都會使用相同的變數，使 `i`值在所有 lambda 中都相同。</span><span class="sxs-lookup"><span data-stu-id="c5792-161">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="c5792-162">請一律在本機變數（上述範例中的`buttonNumber`）中捕獲其值，然後使用它。</span><span class="sxs-lookup"><span data-stu-id="c5792-162">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="c5792-163">App eventcallback</span><span class="sxs-lookup"><span data-stu-id="c5792-163">EventCallback</span></span>

<span data-ttu-id="c5792-164">有一個常見的嵌套元件案例，就是當子元件事件發生時，想要執行父元件的方法&mdash;例如，當子系中發生 `onclick` 事件時。</span><span class="sxs-lookup"><span data-stu-id="c5792-164">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="c5792-165">若要在元件之間公開事件，請使用 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="c5792-165">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="c5792-166">父元件可以將回呼方法指派給子元件的 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="c5792-166">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="c5792-167">範例應用程式中的 `ChildComponent` （*Components/ChildComponent*）示範如何設定按鈕的 `onclick` 處理常式，以接收來自範例 `ParentComponent`的 `EventCallback` 委派。</span><span class="sxs-lookup"><span data-stu-id="c5792-167">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="c5792-168">系統會使用 `MouseEventArgs`輸入 `EventCallback`，這適用于來自週邊裝置的 `onclick` 事件：</span><span class="sxs-lookup"><span data-stu-id="c5792-168">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="c5792-169">`ParentComponent` 會將子系的 `EventCallback<T>` （`OnClickCallback`）設定為其 `ShowMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="c5792-169">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="c5792-170">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="c5792-170">*Pages/ParentComponent.razor*:</span></span>

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

<span data-ttu-id="c5792-171">在 `ChildComponent`中選取按鈕時：</span><span class="sxs-lookup"><span data-stu-id="c5792-171">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="c5792-172">呼叫 `ParentComponent`的 `ShowMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="c5792-172">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="c5792-173">`_messageText` 會更新並顯示在 `ParentComponent`中。</span><span class="sxs-lookup"><span data-stu-id="c5792-173">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="c5792-174">回呼的方法（`ShowMessage`）中不需要呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes) 。</span><span class="sxs-lookup"><span data-stu-id="c5792-174">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="c5792-175">系統會自動呼叫 `StateHasChanged` 來 rerender `ParentComponent`，就像子事件會觸發在子系內執行之事件處理常式中的元件 rerendering 一樣。</span><span class="sxs-lookup"><span data-stu-id="c5792-175">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="c5792-176">`EventCallback` 和 `EventCallback<T>` 允許非同步委派。</span><span class="sxs-lookup"><span data-stu-id="c5792-176">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="c5792-177">`EventCallback<T>` 是強型別，而且需要特定的引數類型。</span><span class="sxs-lookup"><span data-stu-id="c5792-177">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="c5792-178">`EventCallback` 是弱式類型，而且允許任何引數類型。</span><span class="sxs-lookup"><span data-stu-id="c5792-178">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="c5792-179">使用 `InvokeAsync` 叫用 `EventCallback` 或 `EventCallback<T>`，並等待 <xref:System.Threading.Tasks.Task>：</span><span class="sxs-lookup"><span data-stu-id="c5792-179">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="c5792-180">使用 `EventCallback` 和 `EventCallback<T>` 進行事件處理和系結元件參數。</span><span class="sxs-lookup"><span data-stu-id="c5792-180">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="c5792-181">偏好 `EventCallback`的強型別 `EventCallback<T>`。</span><span class="sxs-lookup"><span data-stu-id="c5792-181">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="c5792-182">`EventCallback<T>` 為元件的使用者提供更好的錯誤意見反應。</span><span class="sxs-lookup"><span data-stu-id="c5792-182">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="c5792-183">與其他 UI 事件處理常式類似，指定事件參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="c5792-183">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="c5792-184">當沒有任何值傳遞至回呼時，請使用 `EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="c5792-184">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="c5792-185">防止預設動作</span><span class="sxs-lookup"><span data-stu-id="c5792-185">Prevent default actions</span></span>

<span data-ttu-id="c5792-186">使用[`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault)指示詞屬性來防止事件的預設動作。</span><span class="sxs-lookup"><span data-stu-id="c5792-186">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="c5792-187">在輸入裝置上選取索引鍵，且元素焦點位於文字方塊上時，瀏覽器通常會在文字方塊中顯示索引鍵的字元。</span><span class="sxs-lookup"><span data-stu-id="c5792-187">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="c5792-188">在下列範例中，會藉由指定 `@onkeypress:preventDefault` 指示詞屬性來防止預設行為。</span><span class="sxs-lookup"><span data-stu-id="c5792-188">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="c5792-189">計數器會遞增，而且 **+** 的索引鍵不會捕捉到 `<input>` 元素的值中：</span><span class="sxs-lookup"><span data-stu-id="c5792-189">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

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

<span data-ttu-id="c5792-190">指定不含值的 `@on{EVENT}:preventDefault` 屬性相當於 `@on{EVENT}:preventDefault="true"`。</span><span class="sxs-lookup"><span data-stu-id="c5792-190">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="c5792-191">屬性的值也可以是運算式。</span><span class="sxs-lookup"><span data-stu-id="c5792-191">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="c5792-192">在下列範例中，`_shouldPreventDefault` 是設定為 `true` 或 `false`的 `bool` 欄位：</span><span class="sxs-lookup"><span data-stu-id="c5792-192">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="c5792-193">不需要事件處理常式來防止預設動作。</span><span class="sxs-lookup"><span data-stu-id="c5792-193">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="c5792-194">事件處理常式和防止預設動作案例可以獨立使用。</span><span class="sxs-lookup"><span data-stu-id="c5792-194">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="c5792-195">停止事件傳播</span><span class="sxs-lookup"><span data-stu-id="c5792-195">Stop event propagation</span></span>

<span data-ttu-id="c5792-196">使用[`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation)指示詞屬性來停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="c5792-196">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="c5792-197">在下列範例中，選取核取方塊可防止從第二個子系 `<div>` 的 click 事件傳播到父 `<div>`：</span><span class="sxs-lookup"><span data-stu-id="c5792-197">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

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
