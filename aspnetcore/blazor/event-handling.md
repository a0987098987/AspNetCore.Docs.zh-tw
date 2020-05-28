---
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---
# <a name="aspnet-core-blazor-event-handling"></a>ASP.NET Core Blazor 事件處理

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Razor元件提供事件處理功能。 對於名為的 HTML 專案屬性 [`@on{EVENT}`](xref:mvc/views/razor#onevent) （例如， `@onclick` ）與委派類型的值，元件會將 Razor 屬性的值視為事件處理常式。

下列程式碼會在 `UpdateHeading` UI 中選取按鈕時呼叫方法：

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

下列程式碼會在 `CheckChanged` UI 中變更核取方塊時呼叫方法：

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

事件處理常式也可以是非同步，並且會傳回 <xref:System.Threading.Tasks.Task> 。 不需要手動呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)。 例外狀況會在發生時記錄。

在下列範例中， `UpdateHeading` 當選取按鈕時，會以非同步方式呼叫：

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

## <a name="event-argument-types"></a>事件引數類型

對於某些事件，則允許事件引數類型。 只有在方法中使用事件種類時，才需要在方法呼叫中指定事件種類。

<xref:System.EventArgs>下表顯示支援的。

| 事件            | 類別                | DOM 事件和注意事項 |
| ---
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-------- |---標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---------- |---標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---------- | |剪貼簿 |<xref:Microsoft.AspNetCore.Components.Web.ClipboardEventArgs> | `oncut`, `oncopy`, `onpaste` | |拖曳 |<xref:Microsoft.AspNetCore.Components.Web.DragEventArgs> | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`,`ondragend`<br><br><xref:Microsoft.AspNetCore.Components.Web.DataTransfer>並 <xref:Microsoft.AspNetCore.Components.Web.DataTransferItem> 按住拖曳的專案資料。 | |錯誤 |<xref:Microsoft.AspNetCore.Components.Web.ErrorEventArgs> | `onerror`| |事件 |<xref:System.EventArgs>  |  *一般*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*剪貼簿*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*輸入*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit><br><br>*媒體*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`<br><br><xref:Microsoft.AspNetCore.Components.Web.EventHandlers>保存屬性，以設定事件名稱和事件引數類型之間的對應。 | |焦點 |<xref:Microsoft.AspNetCore.Components.Web.FocusEventArgs> | `onfocus`, `onblur`, `onfocusin`,`onfocusout`<br><br>不包含的支援 `relatedTarget` 。 | |輸入 |<xref:Microsoft.AspNetCore.Components.ChangeEventArgs> | `onchange`, `oninput` | |鍵盤 |<xref:Microsoft.AspNetCore.Components.Web.KeyboardEventArgs> | `onkeydown`, `onkeypress`, `onkeyup` | |滑鼠 |<xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` | |滑鼠指標 |<xref:Microsoft.AspNetCore.Components.Web.PointerEventArgs> | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` | |滑鼠滾輪 |<xref:Microsoft.AspNetCore.Components.Web.WheelEventArgs> | `onwheel`, `onmousewheel` | |進度 |<xref:Microsoft.AspNetCore.Components.Web.ProgressEventArgs> | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` | |觸控 |<xref:Microsoft.AspNetCore.Components.Web.TouchEventArgs> | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`,`ontouchcancel`<br><br><xref:Microsoft.AspNetCore.Components.Web.TouchPoint>代表觸控裝置上的單一連絡人點。 |

如需詳細資訊，請參閱下列資源：

* [ASP.NET Core 參考來源中的 EventArgs 類別（dotnet/aspnetcore release/3.1 分支）](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)。
* [MDN web 檔： GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers)：包含哪些 HTML 元素支援每個 DOM 事件的資訊。

## <a name="lambda-expressions"></a>Lambda 運算式

您也可以使用[Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)：

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

關閉其他值（例如反覆運算一組專案時）通常會很方便。 下列範例會建立三個按鈕， `UpdateHeading` 在 UI 中選取時，每個都會呼叫傳遞事件引數（ <xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> ）和其按鈕編號（ `buttonNumber` ）：

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
> 請**不要在** `i` `for` lambda 運算式中直接使用迴圈中的迴圈變數（）。 否則，所有 lambda 運算式都會使用相同的變數，使 `i` 的值在所有 lambda 中都相同。 請一律在本機變數中捕捉其值（ `buttonNumber` 在上述範例中為），然後使用它。

## <a name="eventcallback"></a>App eventcallback

有一個常見的嵌套元件案例，就是當子元件事件發生時，想要執行父元件的方法。 `onclick`子元件中發生的事件是常見的使用案例。 若要在元件之間公開事件，請使用 <xref:Microsoft.AspNetCore.Components.EventCallback> 。 父元件可以將回呼方法指派給子元件的 <xref:Microsoft.AspNetCore.Components.EventCallback> 。

`ChildComponent`範例應用程式（*Components/ChildComponent*）中的會示範如何設定按鈕的 `onclick` 處理常式，以接收 <xref:Microsoft.AspNetCore.Components.EventCallback> 來自範例的委派 `ParentComponent` 。 <xref:Microsoft.AspNetCore.Components.EventCallback>會使用輸入 `MouseEventArgs` ，這適用于 `onclick` 來自週邊裝置的事件：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

會 `ParentComponent` 將子系的 <xref:Microsoft.AspNetCore.Components.EventCallback%601> （ `OnClickCallback` ）設定為其 `ShowMessage` 方法。

*Pages/ParentComponent. razor*：

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

當您在中選取按鈕時 `ChildComponent` ：

* `ParentComponent` `ShowMessage` 會呼叫的方法。 `messageText`會更新並顯示在中 `ParentComponent` 。
* 回呼的方法（）中不需要呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes) `ShowMessage` 。 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>會自動呼叫來 rerender `ParentComponent` ，就像子事件會觸發元件 rerendering 在子系內執行的事件處理常式一樣。

<xref:Microsoft.AspNetCore.Components.EventCallback>和 <xref:Microsoft.AspNetCore.Components.EventCallback%601> 允許非同步委派。 <xref:Microsoft.AspNetCore.Components.EventCallback%601>是強型別，而且需要特定的引數類型。 <xref:Microsoft.AspNetCore.Components.EventCallback>是弱型別，並允許任何引數類型。

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />
```

使用叫 <xref:Microsoft.AspNetCore.Components.EventCallback> <xref:Microsoft.AspNetCore.Components.EventCallback%601> 用或 <xref:Microsoft.AspNetCore.Components.EventCallback.InvokeAsync%2A> ，並等待 <xref:System.Threading.Tasks.Task> ：

```csharp
await callback.InvokeAsync(arg);
```

<xref:Microsoft.AspNetCore.Components.EventCallback> <xref:Microsoft.AspNetCore.Components.EventCallback%601> 針對事件處理和系結元件參數使用和。

慣用強型別 <xref:Microsoft.AspNetCore.Components.EventCallback%601> <xref:Microsoft.AspNetCore.Components.EventCallback> 。 <xref:Microsoft.AspNetCore.Components.EventCallback%601>為元件的使用者提供更好的錯誤意見反應。 與其他 UI 事件處理常式類似，指定事件參數是選擇性的。 <xref:Microsoft.AspNetCore.Components.EventCallback>當沒有任何值傳遞給回呼時，請使用。

## <a name="prevent-default-actions"></a>防止預設動作

使用指示詞 [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) 屬性可防止事件的預設動作。

在輸入裝置上選取索引鍵，且元素焦點位於文字方塊上時，瀏覽器通常會在文字方塊中顯示索引鍵的字元。 在下列範例中，會藉由指定指示詞屬性來防止預設行為 `@onkeypress:preventDefault` 。 計數器會遞增，而且索引 **+** 鍵不會被捕捉到 `<input>` 元素的值中：

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

指定 `@on{EVENT}:preventDefault` 不含值的屬性，相當於 `@on{EVENT}:preventDefault="true"` 。

屬性的值也可以是運算式。 在下列範例中， `shouldPreventDefault` 是 `bool` 設定為或的欄位 `true` `false` ：

```razor
<input @onkeypress:preventDefault="shouldPreventDefault" />
```

不需要事件處理常式來防止預設動作。 事件處理常式和防止預設動作案例可以獨立使用。

## <a name="stop-event-propagation"></a>停止事件傳播

使用指示詞 [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) 屬性來停止事件傳播。

在下列範例中，選取核取方塊可防止從第二個子系的 click 事件 `<div>` 傳播到父系 `<div>` ：

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
