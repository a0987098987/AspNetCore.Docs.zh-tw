---
title: ASP.NET核心Blazor事件處理
author: guardrex
description: 瞭解Blazor事件處理功能,包括事件參數類型、事件回調和管理默認瀏覽器事件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: c144841805e07a136f153c25a78c7f9af7c5801b
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79511362"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="4a6fa-103">ASP.NET核心布拉佐爾事件處理</span><span class="sxs-lookup"><span data-stu-id="4a6fa-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="4a6fa-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="4a6fa-105">剃刀元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-105">Razor components provide event handling features.</span></span> <span data-ttu-id="4a6fa-106">對於具有委託類型值的[`@on{EVENT}`](xref:mvc/views/razor#onevent)名為`@onclick`(例如 ) 的 HTML 元素屬性,Razor 元件將該屬性的值視為事件處理程式。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-106">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="4a6fa-107">在 UI`UpdateHeading`中選擇 按鍵時,以下代碼將呼叫方法:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-107">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="4a6fa-108">當 UI`CheckChanged`中 改變選擇框時,以下代碼將呼叫方法:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-108">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="4a6fa-109">事件處理程式也可以非同步並傳<xref:System.Threading.Tasks.Task>回 。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-109">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="4a6fa-110">不須手動呼叫[State 已變更](xref:blazor/lifecycle#state-changes)。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-110">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="4a6fa-111">異常發生時將記錄這些異常。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-111">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="4a6fa-112">在下面的範例中,`UpdateHeading`在選擇按鈕時會調整:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-112">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

## <a name="event-argument-types"></a><span data-ttu-id="4a6fa-113">事件參數類型</span><span class="sxs-lookup"><span data-stu-id="4a6fa-113">Event argument types</span></span>

<span data-ttu-id="4a6fa-114">對於某些事件,允許事件參數類型。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-114">For some events, event argument types are permitted.</span></span> <span data-ttu-id="4a6fa-115">僅當方法中使用事件類型時,才需要在方法調用中指定事件類型。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-115">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="4a6fa-116">下表`EventArgs`中顯示了支援的"</span><span class="sxs-lookup"><span data-stu-id="4a6fa-116">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="4a6fa-117">事件</span><span class="sxs-lookup"><span data-stu-id="4a6fa-117">Event</span></span>            | <span data-ttu-id="4a6fa-118">類別</span><span class="sxs-lookup"><span data-stu-id="4a6fa-118">Class</span></span>                | <span data-ttu-id="4a6fa-119">DOM 事件與註解</span><span class="sxs-lookup"><span data-stu-id="4a6fa-119">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="4a6fa-120">剪貼簿</span><span class="sxs-lookup"><span data-stu-id="4a6fa-120">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="4a6fa-121">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-121">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="4a6fa-122">拖曳</span><span class="sxs-lookup"><span data-stu-id="4a6fa-122">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="4a6fa-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="4a6fa-124">`DataTransfer`並`DataTransferItem`保存拖動的項目數據。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-124">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="4a6fa-125">錯誤</span><span class="sxs-lookup"><span data-stu-id="4a6fa-125">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="4a6fa-126">事件</span><span class="sxs-lookup"><span data-stu-id="4a6fa-126">Event</span></span>            | `EventArgs`          | <span data-ttu-id="4a6fa-127">*一般*</span><span class="sxs-lookup"><span data-stu-id="4a6fa-127">*General*</span></span><br><span data-ttu-id="4a6fa-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="4a6fa-129">*剪貼簿*</span><span class="sxs-lookup"><span data-stu-id="4a6fa-129">*Clipboard*</span></span><br><span data-ttu-id="4a6fa-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="4a6fa-131">*輸入*</span><span class="sxs-lookup"><span data-stu-id="4a6fa-131">*Input*</span></span><br><span data-ttu-id="4a6fa-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="4a6fa-133">*媒體*</span><span class="sxs-lookup"><span data-stu-id="4a6fa-133">*Media*</span></span><br><span data-ttu-id="4a6fa-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="4a6fa-135">Focus</span><span class="sxs-lookup"><span data-stu-id="4a6fa-135">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="4a6fa-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="4a6fa-137">不包括對`relatedTarget`的支援。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-137">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="4a6fa-138">輸入</span><span class="sxs-lookup"><span data-stu-id="4a6fa-138">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="4a6fa-139">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-139">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="4a6fa-140">鍵盤</span><span class="sxs-lookup"><span data-stu-id="4a6fa-140">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="4a6fa-141">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-141">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="4a6fa-142">滑鼠</span><span class="sxs-lookup"><span data-stu-id="4a6fa-142">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="4a6fa-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="4a6fa-144">滑鼠指標</span><span class="sxs-lookup"><span data-stu-id="4a6fa-144">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="4a6fa-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="4a6fa-146">滑鼠滾輪</span><span class="sxs-lookup"><span data-stu-id="4a6fa-146">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="4a6fa-147">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-147">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="4a6fa-148">進度</span><span class="sxs-lookup"><span data-stu-id="4a6fa-148">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="4a6fa-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="4a6fa-150">觸控</span><span class="sxs-lookup"><span data-stu-id="4a6fa-150">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="4a6fa-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="4a6fa-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="4a6fa-152">`TouchPoint`表示觸摸敏感設備上的單個接觸點。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-152">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="4a6fa-153">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="4a6fa-153">For more information, see the following resources:</span></span>

* <span data-ttu-id="4a6fa-154">[ASP.NET核心參考源中的 EventArgs 類(dotnet/aspnetcore 版本/3.1 分支)。](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-154">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="4a6fa-155">[MDN Web 文件:全域事件處理程式](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers)&ndash;包括 HTML 元素支援每個 DOM 事件的資訊。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-155">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="4a6fa-156">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="4a6fa-156">Lambda expressions</span></span>

<span data-ttu-id="4a6fa-157">[蘭姆達運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)也可以使用:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-157">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="4a6fa-158">關閉其他值通常很方便,例如,在一組元素上反覆運算時。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-158">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="4a6fa-159">下面的範例建立三個按鈕,每個按鈕在`UpdateHeading`UI 中選擇時呼`MouseEventArgs`叫傳遞 事件`buttonNumber`參數 ( ) 及其按鈕編號 ( ):</span><span class="sxs-lookup"><span data-stu-id="4a6fa-159">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

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
> <span data-ttu-id="4a6fa-160">**不要**直接在 lambda`i`表示`for`式中的 循環中使用循環變數 ( ) 。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-160">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="4a6fa-161">否則,所有 lambda 表達式都使用相同的變數,`i`導致所有 lambda 的值相同。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-161">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="4a6fa-162">始終在局部變數中捕獲其值(`buttonNumber`在前面的範例中),然後使用它。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-162">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="4a6fa-163">事件回檔</span><span class="sxs-lookup"><span data-stu-id="4a6fa-163">EventCallback</span></span>

<span data-ttu-id="4a6fa-164">具有嵌套元件的常見方案是希望在子元件事件發生&mdash;時運行父元件的方法,例如,當子元件中發生事件`onclick`時。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-164">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="4a6fa-165">要跨元件公開事件,請使用`EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-165">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="4a6fa-166">父元件可以將回檔方法分配給子元件的`EventCallback`。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-166">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="4a6fa-167">範例`ChildComponent`應用程式 (*元件/子元件.razor)* 中展示如何設定按鈕`onclick`的處理程式以接收來自範例的`EventCallback``ParentComponent`委託。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-167">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="4a6fa-168">`EventCallback`鍵入 的`MouseEventArgs`與 鍵入,適用於`onclick`來自外圍 裝置的事件:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-168">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="4a6fa-169">將`ParentComponent`孩子的`EventCallback<T>``OnClickCallback`( )`ShowMessage`設置為其 方法。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-169">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="4a6fa-170">*頁面/父元件.razor*:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-170">*Pages/ParentComponent.razor*:</span></span>

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

<span data-ttu-id="4a6fa-171">選擇此按鈕時`ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-171">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="4a6fa-172">呼叫`ParentComponent`''`ShowMessage`的方法 。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-172">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="4a6fa-173">`_messageText`更新並顯示在中`ParentComponent`。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-173">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="4a6fa-174">回檔方法`ShowMessage`() 中不需要呼叫[StateHasChanged。](xref:blazor/lifecycle#state-changes)</span><span class="sxs-lookup"><span data-stu-id="4a6fa-174">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="4a6fa-175">`StateHasChanged`自動調用 以重新`ParentComponent`呈現 ,就像子事件觸發在子級內執行的事件處理程式中的元件重新呈現一樣。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-175">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="4a6fa-176">`EventCallback`並允許`EventCallback<T>`非同步委託。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-176">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="4a6fa-177">`EventCallback<T>`類型為強,需要特定的參數類型。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-177">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="4a6fa-178">`EventCallback`類型較弱,允許任何參數類型。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-178">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="4a6fa-179">呼叫`EventCallback`或`EventCallback<T>``InvokeAsync`並<xref:System.Threading.Tasks.Task>等待 :</span><span class="sxs-lookup"><span data-stu-id="4a6fa-179">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="4a6fa-180">用於`EventCallback``EventCallback<T>`事件 處理和綁定元件參數。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-180">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="4a6fa-181">更喜歡強鍵入`EventCallback<T>``EventCallback`超過 。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-181">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="4a6fa-182">`EventCallback<T>`向元件的使用者提供更好的錯誤反饋。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-182">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="4a6fa-183">與其他 UI 事件處理程式類似,指定事件參數是可選的。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-183">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="4a6fa-184">當`EventCallback`沒有傳遞給回調的值時,請使用。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-184">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="4a6fa-185">防止預設作業</span><span class="sxs-lookup"><span data-stu-id="4a6fa-185">Prevent default actions</span></span>

<span data-ttu-id="4a6fa-186">使用[`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault)指令屬性防止事件的預設操作。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-186">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="4a6fa-187">在輸入裝置上選擇鍵且元素焦點位於文本框中時,瀏覽器通常會在文本框中顯示該鍵的字元。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-187">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="4a6fa-188">在下面的示例中,通過指定`@onkeypress:preventDefault`指令屬性來防止預設行為。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-188">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="4a6fa-189">計數器遞增,**+** 鍵不會捕捉到`<input>`元素的值中:</span><span class="sxs-lookup"><span data-stu-id="4a6fa-189">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

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

<span data-ttu-id="4a6fa-190">指定沒有`@on{EVENT}:preventDefault`值的屬性等效於`@on{EVENT}:preventDefault="true"`。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-190">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="4a6fa-191">屬性的值也可以是表達式。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-191">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="4a6fa-192">在下面的`_shouldPreventDefault`範例中,是設定`bool`為`true``false`或的欄位設定為 :</span><span class="sxs-lookup"><span data-stu-id="4a6fa-192">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="4a6fa-193">不需要事件處理程式來防止預設操作。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-193">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="4a6fa-194">事件處理程式和阻止預設操作方案可以獨立使用。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-194">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="4a6fa-195">停止事件傳播</span><span class="sxs-lookup"><span data-stu-id="4a6fa-195">Stop event propagation</span></span>

<span data-ttu-id="4a6fa-196">使用[`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation)指令屬性停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="4a6fa-196">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="4a6fa-197">在下面的範例中,選擇選擇選擇選擇此方塊, 並可`<div>`防止第二個子級 的`<div>`單擊事件傳播到父級 :</span><span class="sxs-lookup"><span data-stu-id="4a6fa-197">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

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
