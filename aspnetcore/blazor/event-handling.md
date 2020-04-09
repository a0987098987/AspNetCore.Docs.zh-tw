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
# <a name="aspnet-core-blazor-event-handling"></a>ASP.NET核心布拉佐爾事件處理

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

剃刀元件提供事件處理功能。 對於具有委託類型值的[`@on{EVENT}`](xref:mvc/views/razor#onevent)名為`@onclick`(例如 ) 的 HTML 元素屬性,Razor 元件將該屬性的值視為事件處理程式。

在 UI`UpdateHeading`中選擇 按鍵時,以下代碼將呼叫方法:

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

當 UI`CheckChanged`中 改變選擇框時,以下代碼將呼叫方法:

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

事件處理程式也可以非同步並傳<xref:System.Threading.Tasks.Task>回 。 不須手動呼叫[State 已變更](xref:blazor/lifecycle#state-changes)。 異常發生時將記錄這些異常。

在下面的範例中,`UpdateHeading`在選擇按鈕時會調整:

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

## <a name="event-argument-types"></a>事件參數類型

對於某些事件,允許事件參數類型。 僅當方法中使用事件類型時,才需要在方法調用中指定事件類型。

下表`EventArgs`中顯示了支援的"

| 事件            | 類別                | DOM 事件與註解 |
| ---------------- | -------------------- | -------------------- |
| 剪貼簿        | `ClipboardEventArgs` | `oncut`, `oncopy`, `onpaste` |
| 拖曳             | `DragEventArgs`      | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`<br><br>`DataTransfer`並`DataTransferItem`保存拖動的項目數據。 |
| 錯誤            | `ErrorEventArgs`     | `onerror` |
| 事件            | `EventArgs`          | *一般*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*剪貼簿*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*輸入*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`<br><br>*媒體*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting` |
| Focus            | `FocusEventArgs`     | `onfocus`, `onblur`, `onfocusin`, `onfocusout`<br><br>不包括對`relatedTarget`的支援。 |
| 輸入            | `ChangeEventArgs`    | `onchange`, `oninput` |
| 鍵盤         | `KeyboardEventArgs`  | `onkeydown`, `onkeypress`, `onkeyup` |
| 滑鼠            | `MouseEventArgs`     | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` |
| 滑鼠指標    | `PointerEventArgs`   | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` |
| 滑鼠滾輪      | `WheelEventArgs`     | `onwheel`, `onmousewheel` |
| 進度         | `ProgressEventArgs`  | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` |
| 觸控            | `TouchEventArgs`     | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`<br><br>`TouchPoint`表示觸摸敏感設備上的單個接觸點。 |

如需詳細資訊，請參閱下列資源：

* [ASP.NET核心參考源中的 EventArgs 類(dotnet/aspnetcore 版本/3.1 分支)。](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)
* [MDN Web 文件:全域事件處理程式](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers)&ndash;包括 HTML 元素支援每個 DOM 事件的資訊。

## <a name="lambda-expressions"></a>Lambda 運算式

[蘭姆達運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)也可以使用:

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

關閉其他值通常很方便,例如,在一組元素上反覆運算時。 下面的範例建立三個按鈕,每個按鈕在`UpdateHeading`UI 中選擇時呼`MouseEventArgs`叫傳遞 事件`buttonNumber`參數 ( ) 及其按鈕編號 ( ):

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
> **不要**直接在 lambda`i`表示`for`式中的 循環中使用循環變數 ( ) 。 否則,所有 lambda 表達式都使用相同的變數,`i`導致所有 lambda 的值相同。 始終在局部變數中捕獲其值(`buttonNumber`在前面的範例中),然後使用它。

## <a name="eventcallback"></a>事件回檔

具有嵌套元件的常見方案是希望在子元件事件發生&mdash;時運行父元件的方法,例如,當子元件中發生事件`onclick`時。 要跨元件公開事件,請使用`EventCallback`。 父元件可以將回檔方法分配給子元件的`EventCallback`。

範例`ChildComponent`應用程式 (*元件/子元件.razor)* 中展示如何設定按鈕`onclick`的處理程式以接收來自範例的`EventCallback``ParentComponent`委託。 `EventCallback`鍵入 的`MouseEventArgs`與 鍵入,適用於`onclick`來自外圍 裝置的事件:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

將`ParentComponent`孩子的`EventCallback<T>``OnClickCallback`( )`ShowMessage`設置為其 方法。

*頁面/父元件.razor*:

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

選擇此按鈕時`ChildComponent`:

* 呼叫`ParentComponent`''`ShowMessage`的方法 。 `_messageText`更新並顯示在中`ParentComponent`。
* 回檔方法`ShowMessage`() 中不需要呼叫[StateHasChanged。](xref:blazor/lifecycle#state-changes) `StateHasChanged`自動調用 以重新`ParentComponent`呈現 ,就像子事件觸發在子級內執行的事件處理程式中的元件重新呈現一樣。

`EventCallback`並允許`EventCallback<T>`非同步委託。 `EventCallback<T>`類型為強,需要特定的參數類型。 `EventCallback`類型較弱,允許任何參數類型。

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

呼叫`EventCallback`或`EventCallback<T>``InvokeAsync`並<xref:System.Threading.Tasks.Task>等待 :

```csharp
await callback.InvokeAsync(arg);
```

用於`EventCallback``EventCallback<T>`事件 處理和綁定元件參數。

更喜歡強鍵入`EventCallback<T>``EventCallback`超過 。 `EventCallback<T>`向元件的使用者提供更好的錯誤反饋。 與其他 UI 事件處理程式類似,指定事件參數是可選的。 當`EventCallback`沒有傳遞給回調的值時,請使用。

## <a name="prevent-default-actions"></a>防止預設作業

使用[`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault)指令屬性防止事件的預設操作。

在輸入裝置上選擇鍵且元素焦點位於文本框中時,瀏覽器通常會在文本框中顯示該鍵的字元。 在下面的示例中,通過指定`@onkeypress:preventDefault`指令屬性來防止預設行為。 計數器遞增,**+** 鍵不會捕捉到`<input>`元素的值中:

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

指定沒有`@on{EVENT}:preventDefault`值的屬性等效於`@on{EVENT}:preventDefault="true"`。

屬性的值也可以是表達式。 在下面的`_shouldPreventDefault`範例中,是設定`bool`為`true``false`或的欄位設定為 :

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

不需要事件處理程式來防止預設操作。 事件處理程式和阻止預設操作方案可以獨立使用。

## <a name="stop-event-propagation"></a>停止事件傳播

使用[`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation)指令屬性停止事件傳播。

在下面的範例中,選擇選擇選擇選擇此方塊, 並可`<div>`防止第二個子級 的`<div>`單擊事件傳播到父級 :

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
