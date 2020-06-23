---
title: ASP.NET Core Blazor 事件處理
author: guardrex
description: 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/04/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/event-handling
ms.openlocfilehash: 4ac7b82d734f078cf50901d02e7d0c4eb8bb45bb
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242415"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="aa667-103">ASP.NET Core Blazor 事件處理</span><span class="sxs-lookup"><span data-stu-id="aa667-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="aa667-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="aa667-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

Razor<span data-ttu-id="aa667-105">元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="aa667-105"> components provide event handling features.</span></span> <span data-ttu-id="aa667-106">對於名為的 HTML 專案屬性 [`@on{EVENT}`](xref:mvc/views/razor#onevent) （例如， `@onclick` ）與委派類型的值，元件會將 Razor 屬性的值視為事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="aa667-106">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="aa667-107">下列程式碼會在 `UpdateHeading` UI 中選取按鈕時呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="aa667-107">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="aa667-108">下列程式碼會在 `CheckChanged` UI 中變更核取方塊時呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="aa667-108">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="aa667-109">事件處理常式也可以是非同步，並且會傳回 <xref:System.Threading.Tasks.Task> 。</span><span class="sxs-lookup"><span data-stu-id="aa667-109">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="aa667-110">不需要手動呼叫[StateHasChanged](xref:blazor/components/lifecycle#state-changes)。</span><span class="sxs-lookup"><span data-stu-id="aa667-110">There's no need to manually call [StateHasChanged](xref:blazor/components/lifecycle#state-changes).</span></span> <span data-ttu-id="aa667-111">例外狀況會在發生時記錄。</span><span class="sxs-lookup"><span data-stu-id="aa667-111">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="aa667-112">在下列範例中， `UpdateHeading` 當選取按鈕時，會以非同步方式呼叫：</span><span class="sxs-lookup"><span data-stu-id="aa667-112">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

## <a name="event-argument-types"></a><span data-ttu-id="aa667-113">事件引數類型</span><span class="sxs-lookup"><span data-stu-id="aa667-113">Event argument types</span></span>

<span data-ttu-id="aa667-114">對於某些事件，則允許事件引數類型。</span><span class="sxs-lookup"><span data-stu-id="aa667-114">For some events, event argument types are permitted.</span></span> <span data-ttu-id="aa667-115">在事件方法定義中指定事件參數是選擇性的，只有在方法中使用事件種類時才需要。</span><span class="sxs-lookup"><span data-stu-id="aa667-115">Specifying an event parameter in an event method definition is optional and only necessary if the event type is used in the method.</span></span> <span data-ttu-id="aa667-116">在下列範例中， `MouseEventArgs` 會在方法中使用事件引數 `ShowMessage` 來設定郵件內文：</span><span class="sxs-lookup"><span data-stu-id="aa667-116">In the following example, the `MouseEventArgs` event argument is used in the `ShowMessage` method to set message text:</span></span>

```csharp
private void ShowMessage(MouseEventArgs e)
{
    messageText = $"The mouse is at coordinates: {e.ScreenX}:{e.ScreenY}";
}
```

<span data-ttu-id="aa667-117"><xref:System.EventArgs>下表顯示支援的。</span><span class="sxs-lookup"><span data-stu-id="aa667-117">Supported <xref:System.EventArgs> are shown in the following table.</span></span>

| <span data-ttu-id="aa667-118">事件</span><span class="sxs-lookup"><span data-stu-id="aa667-118">Event</span></span>            | <span data-ttu-id="aa667-119">類別</span><span class="sxs-lookup"><span data-stu-id="aa667-119">Class</span></span>                | <span data-ttu-id="aa667-120">DOM 事件和注意事項</span><span class="sxs-lookup"><span data-stu-id="aa667-120">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="aa667-121">剪貼簿</span><span class="sxs-lookup"><span data-stu-id="aa667-121">Clipboard</span></span>        | <xref:Microsoft.AspNetCore.Components.Web.ClipboardEventArgs> | <span data-ttu-id="aa667-122">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="aa667-122">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="aa667-123">拖曳</span><span class="sxs-lookup"><span data-stu-id="aa667-123">Drag</span></span>             | <xref:Microsoft.AspNetCore.Components.Web.DragEventArgs> | <span data-ttu-id="aa667-124">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="aa667-124">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="aa667-125"><xref:Microsoft.AspNetCore.Components.Web.DataTransfer>並 <xref:Microsoft.AspNetCore.Components.Web.DataTransferItem> 按住拖曳的專案資料。</span><span class="sxs-lookup"><span data-stu-id="aa667-125"><xref:Microsoft.AspNetCore.Components.Web.DataTransfer> and <xref:Microsoft.AspNetCore.Components.Web.DataTransferItem> hold dragged item data.</span></span> |
| <span data-ttu-id="aa667-126">錯誤</span><span class="sxs-lookup"><span data-stu-id="aa667-126">Error</span></span>            | <xref:Microsoft.AspNetCore.Components.Web.ErrorEventArgs> | `onerror` |
| <span data-ttu-id="aa667-127">事件</span><span class="sxs-lookup"><span data-stu-id="aa667-127">Event</span></span>            | <xref:System.EventArgs> | <span data-ttu-id="aa667-128">*一般*</span><span class="sxs-lookup"><span data-stu-id="aa667-128">*General*</span></span><br><span data-ttu-id="aa667-129">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="aa667-129">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="aa667-130">*剪貼簿*</span><span class="sxs-lookup"><span data-stu-id="aa667-130">*Clipboard*</span></span><br><span data-ttu-id="aa667-131">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="aa667-131">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="aa667-132">*輸入*</span><span class="sxs-lookup"><span data-stu-id="aa667-132">*Input*</span></span><br><span data-ttu-id="aa667-133">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit></span><span class="sxs-lookup"><span data-stu-id="aa667-133">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit></span></span><br><br><span data-ttu-id="aa667-134">*媒體*</span><span class="sxs-lookup"><span data-stu-id="aa667-134">*Media*</span></span><br><span data-ttu-id="aa667-135">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onended`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="aa667-135">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onended`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span><br><br><span data-ttu-id="aa667-136"><xref:Microsoft.AspNetCore.Components.Web.EventHandlers>保存屬性，以設定事件名稱和事件引數類型之間的對應。</span><span class="sxs-lookup"><span data-stu-id="aa667-136"><xref:Microsoft.AspNetCore.Components.Web.EventHandlers> holds attributes to configure the mappings between event names and event argument types.</span></span> |
| <span data-ttu-id="aa667-137">Focus</span><span class="sxs-lookup"><span data-stu-id="aa667-137">Focus</span></span>            | <xref:Microsoft.AspNetCore.Components.Web.FocusEventArgs> | <span data-ttu-id="aa667-138">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="aa667-138">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="aa667-139">不包含的支援 `relatedTarget` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-139">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="aa667-140">輸入</span><span class="sxs-lookup"><span data-stu-id="aa667-140">Input</span></span>            | <xref:Microsoft.AspNetCore.Components.ChangeEventArgs> | <span data-ttu-id="aa667-141">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="aa667-141">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="aa667-142">鍵盤</span><span class="sxs-lookup"><span data-stu-id="aa667-142">Keyboard</span></span>         | <xref:Microsoft.AspNetCore.Components.Web.KeyboardEventArgs> | <span data-ttu-id="aa667-143">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="aa667-143">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="aa667-144">滑鼠</span><span class="sxs-lookup"><span data-stu-id="aa667-144">Mouse</span></span>            | <xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> | <span data-ttu-id="aa667-145">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="aa667-145">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="aa667-146">滑鼠指標</span><span class="sxs-lookup"><span data-stu-id="aa667-146">Mouse pointer</span></span>    | <xref:Microsoft.AspNetCore.Components.Web.PointerEventArgs> | <span data-ttu-id="aa667-147">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="aa667-147">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="aa667-148">滑鼠滾輪</span><span class="sxs-lookup"><span data-stu-id="aa667-148">Mouse wheel</span></span>      | <xref:Microsoft.AspNetCore.Components.Web.WheelEventArgs> | <span data-ttu-id="aa667-149">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="aa667-149">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="aa667-150">進度</span><span class="sxs-lookup"><span data-stu-id="aa667-150">Progress</span></span>         | <xref:Microsoft.AspNetCore.Components.Web.ProgressEventArgs> | <span data-ttu-id="aa667-151">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="aa667-151">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="aa667-152">觸控</span><span class="sxs-lookup"><span data-stu-id="aa667-152">Touch</span></span>            | <xref:Microsoft.AspNetCore.Components.Web.TouchEventArgs> | <span data-ttu-id="aa667-153">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="aa667-153">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="aa667-154"><xref:Microsoft.AspNetCore.Components.Web.TouchPoint>代表觸控裝置上的單一連絡人點。</span><span class="sxs-lookup"><span data-stu-id="aa667-154"><xref:Microsoft.AspNetCore.Components.Web.TouchPoint> represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="aa667-155">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="aa667-155">For more information, see the following resources:</span></span>

* <span data-ttu-id="aa667-156">[ `EventArgs` ASP.NET Core 參考來源中的類別（dotnet/aspnetcore release/3.1 分支）](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)。</span><span class="sxs-lookup"><span data-stu-id="aa667-156">[`EventArgs` classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="aa667-157">[MDN web 檔： GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers)：包含哪些 HTML 元素支援每個 DOM 事件的資訊。</span><span class="sxs-lookup"><span data-stu-id="aa667-157">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers): Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="aa667-158">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="aa667-158">Lambda expressions</span></span>

<span data-ttu-id="aa667-159">您也可以使用[Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)：</span><span class="sxs-lookup"><span data-stu-id="aa667-159">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="aa667-160">關閉其他值（例如反覆運算一組專案時）通常會很方便。</span><span class="sxs-lookup"><span data-stu-id="aa667-160">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="aa667-161">下列範例會建立三個按鈕， `UpdateHeading` 在 UI 中選取時，每個都會呼叫傳遞事件引數（ <xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> ）和其按鈕編號（ `buttonNumber` ）：</span><span class="sxs-lookup"><span data-stu-id="aa667-161">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (<xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs>) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="aa667-162">請勿直接在 lambda 運算式**中使用迴圈**變數，例如 `i` 在先前的 `for` 迴圈範例中，或迴圈中的參考變數 `foreach` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-162">Do **not** use a loop variable directly in a lambda expression, such as `i` in the preceding `for` loop example or a reference variable in a `foreach` loop.</span></span> <span data-ttu-id="aa667-163">否則，所有 lambda 運算式都會使用相同的變數，這會導致在所有 lambda 中使用相同的值。</span><span class="sxs-lookup"><span data-stu-id="aa667-163">Otherwise, the same variable is used by all lambda expressions, which results in use of the same value in all lambdas.</span></span> <span data-ttu-id="aa667-164">一律在本機變數中捕獲變數的值，然後使用它。</span><span class="sxs-lookup"><span data-stu-id="aa667-164">Always capture the variable's value in a local variable and then use it.</span></span> <span data-ttu-id="aa667-165">在上述範例中，迴圈變數 `i` 會指派給 `buttonNumber` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-165">In the preceding example, the loop variable `i` is assigned to `buttonNumber`.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="aa667-166">App eventcallback</span><span class="sxs-lookup"><span data-stu-id="aa667-166">EventCallback</span></span>

<span data-ttu-id="aa667-167">有一個常見的嵌套元件案例，就是當子元件事件發生時，想要執行父元件的方法。</span><span class="sxs-lookup"><span data-stu-id="aa667-167">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs.</span></span> <span data-ttu-id="aa667-168">`onclick`子元件中發生的事件是常見的使用案例。</span><span class="sxs-lookup"><span data-stu-id="aa667-168">An `onclick` event occurring in the child component is a common use case.</span></span> <span data-ttu-id="aa667-169">若要在元件之間公開事件，請使用 <xref:Microsoft.AspNetCore.Components.EventCallback> 。</span><span class="sxs-lookup"><span data-stu-id="aa667-169">To expose events across components, use an <xref:Microsoft.AspNetCore.Components.EventCallback>.</span></span> <span data-ttu-id="aa667-170">父元件可以將回呼方法指派給子元件的 <xref:Microsoft.AspNetCore.Components.EventCallback> 。</span><span class="sxs-lookup"><span data-stu-id="aa667-170">A parent component can assign a callback method to a child component's <xref:Microsoft.AspNetCore.Components.EventCallback>.</span></span>

<span data-ttu-id="aa667-171">`ChildComponent`範例應用程式（）中的 `Components/ChildComponent.razor` 會示範如何設定按鈕的 `onclick` 處理常式，以接收 <xref:Microsoft.AspNetCore.Components.EventCallback> 來自範例的委派 `ParentComponent` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-171">The `ChildComponent` in the sample app (`Components/ChildComponent.razor`) demonstrates how a button's `onclick` handler is set up to receive an <xref:Microsoft.AspNetCore.Components.EventCallback> delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="aa667-172"><xref:Microsoft.AspNetCore.Components.EventCallback>會使用輸入 `MouseEventArgs` ，這適用于 `onclick` 來自週邊裝置的事件：</span><span class="sxs-lookup"><span data-stu-id="aa667-172">The <xref:Microsoft.AspNetCore.Components.EventCallback> is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](../common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="aa667-173">會 `ParentComponent` 將子系的 <xref:Microsoft.AspNetCore.Components.EventCallback%601> （ `OnClickCallback` ）設定為其 `ShowMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="aa667-173">The `ParentComponent` sets the child's <xref:Microsoft.AspNetCore.Components.EventCallback%601> (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="aa667-174">`Pages/ParentComponent.razor`:</span><span class="sxs-lookup"><span data-stu-id="aa667-174">`Pages/ParentComponent.razor`:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@messageText</b></p>

@code {
    private string messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="aa667-175">當您在中選取按鈕時 `ChildComponent` ：</span><span class="sxs-lookup"><span data-stu-id="aa667-175">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="aa667-176">`ParentComponent` `ShowMessage` 會呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="aa667-176">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="aa667-177">`messageText`會更新並顯示在中 `ParentComponent` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-177">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="aa667-178">[`StateHasChanged`](xref:blazor/components/lifecycle#state-changes)回呼的方法（）中不需要呼叫 `ShowMessage` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-178">A call to [`StateHasChanged`](xref:blazor/components/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="aa667-179"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>會自動呼叫來 rerender `ParentComponent` ，就像子事件會觸發元件 rerendering 在子系內執行的事件處理常式一樣。</span><span class="sxs-lookup"><span data-stu-id="aa667-179"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="aa667-180"><xref:Microsoft.AspNetCore.Components.EventCallback>和 <xref:Microsoft.AspNetCore.Components.EventCallback%601> 允許非同步委派。</span><span class="sxs-lookup"><span data-stu-id="aa667-180"><xref:Microsoft.AspNetCore.Components.EventCallback> and <xref:Microsoft.AspNetCore.Components.EventCallback%601> permit asynchronous delegates.</span></span> <span data-ttu-id="aa667-181"><xref:Microsoft.AspNetCore.Components.EventCallback%601>是強型別，而且需要特定的引數類型。</span><span class="sxs-lookup"><span data-stu-id="aa667-181"><xref:Microsoft.AspNetCore.Components.EventCallback%601> is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="aa667-182"><xref:Microsoft.AspNetCore.Components.EventCallback>是弱型別，並允許任何引數類型。</span><span class="sxs-lookup"><span data-stu-id="aa667-182"><xref:Microsoft.AspNetCore.Components.EventCallback> is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />
```

<span data-ttu-id="aa667-183">使用叫 <xref:Microsoft.AspNetCore.Components.EventCallback> <xref:Microsoft.AspNetCore.Components.EventCallback%601> 用或 <xref:Microsoft.AspNetCore.Components.EventCallback.InvokeAsync%2A> ，並等待 <xref:System.Threading.Tasks.Task> ：</span><span class="sxs-lookup"><span data-stu-id="aa667-183">Invoke an <xref:Microsoft.AspNetCore.Components.EventCallback> or <xref:Microsoft.AspNetCore.Components.EventCallback%601> with <xref:Microsoft.AspNetCore.Components.EventCallback.InvokeAsync%2A> and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await OnClickCallback.InvokeAsync(arg);
```

<span data-ttu-id="aa667-184"><xref:Microsoft.AspNetCore.Components.EventCallback> <xref:Microsoft.AspNetCore.Components.EventCallback%601> 針對事件處理和系結元件參數使用和。</span><span class="sxs-lookup"><span data-stu-id="aa667-184">Use <xref:Microsoft.AspNetCore.Components.EventCallback> and <xref:Microsoft.AspNetCore.Components.EventCallback%601> for event handling and binding component parameters.</span></span>

<span data-ttu-id="aa667-185">慣用強型別 <xref:Microsoft.AspNetCore.Components.EventCallback%601> <xref:Microsoft.AspNetCore.Components.EventCallback> 。</span><span class="sxs-lookup"><span data-stu-id="aa667-185">Prefer the strongly typed <xref:Microsoft.AspNetCore.Components.EventCallback%601> over <xref:Microsoft.AspNetCore.Components.EventCallback>.</span></span> <span data-ttu-id="aa667-186"><xref:Microsoft.AspNetCore.Components.EventCallback%601>為元件的使用者提供更好的錯誤意見反應。</span><span class="sxs-lookup"><span data-stu-id="aa667-186"><xref:Microsoft.AspNetCore.Components.EventCallback%601> provides better error feedback to users of the component.</span></span> <span data-ttu-id="aa667-187">與其他 UI 事件處理常式類似，指定事件參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="aa667-187">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="aa667-188"><xref:Microsoft.AspNetCore.Components.EventCallback>當沒有任何值傳遞給回呼時，請使用。</span><span class="sxs-lookup"><span data-stu-id="aa667-188">Use <xref:Microsoft.AspNetCore.Components.EventCallback> when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="aa667-189">防止預設動作</span><span class="sxs-lookup"><span data-stu-id="aa667-189">Prevent default actions</span></span>

<span data-ttu-id="aa667-190">使用指示詞 [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) 屬性可防止事件的預設動作。</span><span class="sxs-lookup"><span data-stu-id="aa667-190">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="aa667-191">在輸入裝置上選取索引鍵，且元素焦點位於文字方塊上時，瀏覽器通常會在文字方塊中顯示索引鍵的字元。</span><span class="sxs-lookup"><span data-stu-id="aa667-191">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="aa667-192">在下列範例中，會藉由指定指示詞屬性來防止預設行為 `@onkeypress:preventDefault` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-192">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="aa667-193">計數器會遞增，而且索引 **+** 鍵不會被捕捉到 `<input>` 元素的值中：</span><span class="sxs-lookup"><span data-stu-id="aa667-193">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            count++;
        }
    }
}
```

<span data-ttu-id="aa667-194">指定 `@on{EVENT}:preventDefault` 不含值的屬性，相當於 `@on{EVENT}:preventDefault="true"` 。</span><span class="sxs-lookup"><span data-stu-id="aa667-194">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="aa667-195">屬性的值也可以是運算式。</span><span class="sxs-lookup"><span data-stu-id="aa667-195">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="aa667-196">在下列範例中， `shouldPreventDefault` 是 `bool` 設定為或的欄位 `true` `false` ：</span><span class="sxs-lookup"><span data-stu-id="aa667-196">In the following example, `shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="shouldPreventDefault" />
```

<span data-ttu-id="aa667-197">不需要事件處理常式來防止預設動作。</span><span class="sxs-lookup"><span data-stu-id="aa667-197">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="aa667-198">事件處理常式和防止預設動作案例可以獨立使用。</span><span class="sxs-lookup"><span data-stu-id="aa667-198">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="aa667-199">停止事件傳播</span><span class="sxs-lookup"><span data-stu-id="aa667-199">Stop event propagation</span></span>

<span data-ttu-id="aa667-200">使用指示詞 [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) 屬性來停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="aa667-200">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="aa667-201">在下列範例中，選取核取方塊可防止從第二個子系的 click 事件 `<div>` 傳播到父系 `<div>` ：</span><span class="sxs-lookup"><span data-stu-id="aa667-201">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
