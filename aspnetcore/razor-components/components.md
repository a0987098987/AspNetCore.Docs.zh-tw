---
title: 建立及使用 Razor 元件
author: guardrex
description: 了解如何建立和使用 Razor 元件，包括如何繫結至資料、 處理事件，以及管理元件的生命週期。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515438"
---
# <a name="create-and-use-razor-components"></a>建立及使用 Razor 元件

藉由[Luke Latham](https://github.com/guardrex)， [Daniel Roth](https://github.com/danroth27)，和[Morné Zaayman](https://github.com/MorneZaayman)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([如何下載](xref:index#how-to-download-a-sample))。 請參閱[入門](xref:razor-components/get-started)主題，以取得必要條件。

Razor 元件應用程式使用來建置*元件*。 元件是自封式的區塊的使用者介面 (UI)，例如頁面、 對話方塊中或表單。 元件包含 HTML 標記和處理所需的邏輯插入資料，或是在 UI 事件回應。 元件是富彈性且輕量級。 可以巢狀結構、 重複使用，並在專案之間共用。

## <a name="component-classes"></a>元件類別

元件的實作方式通常 Razor 元件檔案 (*.razor*) 搭配使用C#和 HTML 標記 (*.cshtml* Blazor 應用程式中使用的檔案)。

使用 Razor 元件應用程式都能編寫元件 *.cshtml*只要檔案可視為使用 Razor 元件檔案副檔名`_RazorComponentInclude`MSBuild 屬性。 比方說，建立使用 Razor 元件範本的應用程式指定所有 *.cshtml*下方的檔案*元件*資料夾應該視為 Razor 元件檔案：

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

元件 UI 是使用 HTML 來定義。 動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。 編譯 Razor 元件應用程式時，HTML 標記和C#轉譯邏輯會轉換成元件類別。 產生之類別的名稱比對的檔案名稱。

元件類別的成員中定義`@functions`區塊 (多部`@functions`區塊是允許)。 在 `@functions`區塊中，元件狀態 （屬性、 欄位） 會指定方法來處理事件或定義其他元件的邏輯。

元件成員再使用，因為部分元件的呈現邏輯使用C#開始使用的運算式`@`。 例如，C#前面加上呈現欄位`@`的欄位名稱。 下列範例會評估，並呈現：

* `_headingFontStyle` CSS 屬性值，如`font-style`。
* `_headingText` 內容`<h1>`項目。

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

一開始呈現元件之後，元件會重新產生其呈現樹狀結構，以回應事件。 Razor 元件再比較新的轉譯樹狀目錄中，針對上一個，並套用至瀏覽器的文件物件模型 (DOM) 的任何修改。

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>將元件整合至 Razor Pages 和 MVC 應用程式

使用現有的 Razor Pages 和 MVC 應用程式中的元件。 就不需要重新撰寫現有的頁面或檢視，以使用 Razor 元件。 呈現的網頁或檢視時，元件會 prerendered&dagger;在相同的時間。 

> [!NOTE]
> &dagger;伺服器端預呈現預設會啟用 Razor 元件應用程式。 用戶端 Blazor 應用程式會在即將推出的 Preview 4 版本中支援預呈現。 如需詳細資訊，請參閱 <<c0> [ 更新範本/中介軟體使用 MapFallbackToPage/檔案](https://github.com/aspnet/AspNetCore/issues/8852)。

若要轉譯的頁面或檢視的元件，請使用`RenderComponentAsync<TComponent>`HTML 協助程式方法：

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

從頁面和檢視轉譯的元件尚無法在 Preview 3 版本中的互動式的。 例如，選取按鈕不會觸發的方法呼叫。 未來的預覽版將會解決這項限制，並加入使用一般的項目和屬性語法的轉譯元件的支援。

雖然頁面和檢視表可以使用元件，反向並不正確。 元件無法使用檢視和頁面特定案例，例如部分檢視和區段。 若要使用邏輯從元件中的部分檢視、 部分檢視邏輯分解成元件。

## <a name="using-components"></a>使用元件

元件可以包含宣告的其他元件使用 HTML 項目語法。 使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。

下列標記呈現`HeadingComponent`(*HeadingComponent.cshtml*) 執行個體：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a>元件參數

元件可以有*元件的參數*，這使用來定義*非公用*元件類別的屬性裝飾`[Parameter]`。 使用這些屬性來指定標記中元件的引數。

在下列範例中，`ParentComponent`設定的值`Title`屬性`ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>子內容

另一個元件的內容時，可以設定元件。 指派的元件提供標記，用於指定接收元件之間的內容。 例如，`ParentComponent`可以提供內容的轉譯子元件放在內容`<ChildComponent>`標記。

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

子元件`ChildContent`屬性表示`RenderFragment`。 值`ChildContent`位於子元件的標記應該用來呈現內容的位置。 在下列範例中，值`ChildContent`會收到來自父元件，並在啟動程序的面板內呈現`panel-body`。

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> 屬性接收`RenderFragment`內容必須命名為`ChildContent`依照慣例。

## <a name="data-binding"></a>資料繫結

資料繫結元件和 DOM 項目以完成`bind`屬性。 下列範例會繫結`ItalicsCheck`屬性為核取方塊的核取狀態：

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

當核取方塊已選取，並清除時，要將屬性的值更新為`true`和`false`分別。

轉譯元件時，不是為了回應，若要變更屬性的值時，才會更新 UI 核取方塊。 事件處理常式程式碼執行之後，元件會轉譯本身，因為屬性更新是通常在 UI 中立即反映。

使用`bind`具有`CurrentValue`屬性 (`<input bind="@CurrentValue">`) 基本上等同於下列：

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

轉譯元件時，`value`的輸入項目來自`CurrentValue`屬性。 當使用者在文字方塊中，輸入`onchange`引發事件和`CurrentValue`屬性設定為已變更的值。 事實上，程式碼產生是稍微複雜因為`bind`處理少數的情況下會執行類型轉換。 基本上，`bind`產生關聯的運算式的目前值`value`使用已註冊的處理常式的屬性和處理變更。

除了`onchange`，屬性可以繫結使用其他事件，像是`oninput`因更明確了要繫結至的項目：

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

不同於`onchange`，`oninput`引發輸入到文字方塊中的每個字元。

**格式字串**

資料繫結搭配<xref:System.DateTime>格式字串。 此時，無法使用其他格式的運算式，例如貨幣或數字的格式。

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

`format-value`屬性會指定要套用至日期格式`value`的`input`項目。 的格式也用來剖析的值時`onchange`就會發生事件。

**元件參數**

繫結也會辨識元件參數，其中`bind-{property}`跨元件可以繫結的屬性值。

下列元件會使用`ChildComponent`並繫結`ParentYear`參數的父代`Year`子元件上的參數：

父元件：

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

子元件：

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

正在載入`ParentComponent`會產生下列標記：

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

如果的值`ParentYear`屬性會變更選取的按鈕，即可`ParentComponent`，則`Year`屬性`ChildComponent`會更新。 新值`Year`UI 中轉譯時`ParentComponent`會重新顯示：

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

`Year`參數是可繫結，因為它有隨附`YearChanged`的型別對應的事件`Year`參數。

依照慣例，`<ChildComponent bind-Year="@ParentYear" />`基本上等同於撰寫、

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

一般情況下，屬性可以繫結至對應的事件處理常式使用`bind-property-event`屬性。

## <a name="event-handling"></a>事件處理

Razor 元件提供事件處理功能。 HTML 元素屬性的具名`on<event>`(例如`onclick`， `onsubmit`) 以委派型別值、 Razor 元件，請將視為事件處理常式的屬性的值。 屬性的名稱一律以開頭`on`。

下列程式碼會呼叫`UpdateHeading`在 UI 中選取按鈕時的方法：

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

下列程式碼會呼叫`CheckboxChanged`方法在 UI 中變更該核取方塊時：

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

事件處理常式也可以非同步處理和傳回<xref:System.Threading.Tasks.Task>。 不需要手動呼叫`StateHasChanged()`。 發生時，會記錄例外狀況。

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

對於某些事件，允許特定事件的事件引數型別。 如果存取其中一個事件類型不是必要的它不需要在方法呼叫中。

支援的事件引數清單是：

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

也可以使用 lambda 運算式：

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

通常很方便透過額外的值，例如關閉時逐一查看一組項目。 下列範例會建立三個按鈕，每一個`UpdateHeading`傳遞的事件引數 (`UIMouseEventArgs`) 和按鈕的編號 (`buttonNumber`) 時在 UI 中選取：

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> 請勿**未**使用迴圈變數 (`i`) 中`for`直接在 lambda 運算式中的迴圈。 否則會造成所有的 lambda 運算式所使用相同的變數`i`所有 lambda 中相同的值。 一定會擷取它在本機變數的值 (`buttonNumber`在上述範例中)，然後使用它。

## <a name="capture-references-to-components"></a>擷取元件的參考

元件參考提供方式 get 元件執行個體的參考，讓您發出命令至該執行個體，例如`Show`或`Reset`。 若要擷取元件的參考，請新增`ref`屬性的子元件，然後定義具有相同名稱和相同類型的欄位，做為子元件。

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

轉譯元件時，`loginDialog`欄位會填入`MyLoginDialog`子元件執行個體。 然後，您可以叫用的元件執行個體上的.NET 方法。

> [!IMPORTANT]
> `loginDialog`元件會轉譯並包含其輸出之後，才會填入變數`MyLoginDialog`項目。 直到那時，沒有任何參考。 若要操作元件的參考，該元件已經完成轉譯之後，使用`OnAfterRenderAsync`或`OnAfterRender`方法。

雖然擷取元件的參考會使用類似的語法，來[擷取項目參考](xref:razor-components/javascript-interop#capture-references-to-elements)，它不[JavaScript interop](xref:razor-components/javascript-interop)功能。 元件參考都不會傳遞至 JavaScript 程式碼;只有.NET 程式碼中使用。

> [!NOTE]
> 請勿**不**使用元件參考来變動子元件的狀態。 請改用標準的宣告式參數將資料傳遞至子元件。 這會導致子元件，以自動 rerender 在正確時間。

## <a name="lifecycle-methods"></a>生命週期方法

`OnInitAsync` 和`OnInit`執行程式碼以初始化元件。 若要執行非同步作業，請使用`OnInitAsync`而`await`作業上的關鍵字：

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

同步作業中，使用`OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` 和`OnParametersSet`元件已收到來自其父代的參數和值會指派給屬性時，會呼叫。 在元件初始化之後，會執行這些方法，然後每當元件呈現：

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` 和`OnAfterRender`元件完成轉譯之後呼叫。 會在此時填入項目和元件的參考。 您可以使用這個階段，執行額外的初始化步驟中使用的呈現的內容，例如啟用轉譯的 DOM 項目運作的第三方 JavaScript 程式庫。

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` 您可以覆寫參數設定之前執行程式碼：

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

如果`base.SetParameters`不叫用的自訂程式碼可以解譯方式所需的任何輸入的參數值。 例如，傳入的參數不一定要指派給類別上的屬性。

`ShouldRender` 若要隱藏 ui 的 重新整理，可以被覆寫。 如果實作會傳回`true`，UI 會重新整理。 即使`ShouldRender`會覆寫時，該元件一開始一律呈現。

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>使用 IDisposable 的元件可供使用

如果元件會實作<xref:System.IDisposable>，則[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)從 UI 移除元件時呼叫。 下列元件會使用`@implements IDisposable`而`Dispose`方法：

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>路由

Razor 元件中的路由之後，即可提供應用程式中每個可存取元件的路由範本。

當 *.cshtml*檔案並`@page`編譯指示詞，指定產生的類別<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>指定路由範本。 在執行階段，路由器會尋找使用的元件類別`RouteAttribute`並呈現任何元件有符合所要求的 URL 的路由範本。

多個路由範本可以套用至元件。 下列元件會回應要求`/BlazorRoute`和`/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>路由參數

元件可以接收路由範本中提供的路由參數`@page`指示詞。 路由器會使用路由參數，來填入對應的元件參數。

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

不支援選擇性參數，因此兩個`@page`指示詞會套用在上述範例中。 第一個允許瀏覽至不含參數的元件。 第二個`@page`指示詞會採用`{text}`路由參數，並將值指派給`Text`屬性。

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>「 程式碼後置 」 體驗的基底類別繼承

元件檔 (*.cshtml*) 混合 HTML 標記和C#處理相同檔案中的程式碼。 `@inherits`指示詞可用來提供元件標記分開處理程式碼的 「 程式碼後置 」 體驗中的 Razor 元件應用程式。

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/)示範如何元件可以繼承的基底類別， `BlazorRocksBase`，以提供元件的屬性和方法。

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

基底類別應該衍生自`ComponentBase`。

## <a name="razor-support"></a>Razor 支援

**Razor 指示詞**

下表顯示 razor 指示詞。

| 指示詞 | 描述 |
| --------- | ----------- |
| [\@函式](xref:mvc/views/razor#section-5) | 將C#元件的程式碼區塊。 |
| `@implements` | 實作的介面，產生的元件類別。 |
| [\@繼承](xref:mvc/views/razor#section-3) | 提供完整的控制權，該元件繼承的類別。 |
| [\@inject](xref:mvc/views/razor#section-4) | 可讓服務從注入[服務容器](xref:fundamentals/dependency-injection)。 如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。 |
| `@layout` | 指定的配置元件。 配置元件用來避免程式碼重複和不一致。 |
| [\@page](xref:razor-pages/index#razor-pages) | 指定此元件應該直接處理要求。 `@page`指示詞可以使用路由和選擇性的參數來指定。 Razor 頁面與`@page`指示詞不需要在檔案頂端的第一個指示詞。 如需詳細資訊，請參閱[路由](xref:razor-components/routing)。 |
| [\@使用](xref:mvc/views/razor#using) | 將C#`using`指示詞加入產生的元件類別。 |
| [\@addTagHelper](xref:mvc/views/razor#tag-helpers) | 使用`@addTagHelper`使用比應用程式的組件的不同組件中的元件。 |

**條件式屬性**

屬性會根據.NET 值，有條件地呈現。 如果值為`false`或`null`，不會呈現的屬性。 如果值為`true`，會呈現該屬性最小化。

在下列範例中，`IsCompleted`判斷`checked`呈現控制項的標記中：

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

如果`IsCompleted`是`true`，核取方塊會呈現為：

```html
<input type="checkbox" checked>
```

如果`IsCompleted`是`false`，核取方塊會呈現為：

```html
<input type="checkbox">
```

**在 Razor 的其他資訊**

如需有關 Razor 的詳細資訊，請參閱[Razor 語法參考](xref:mvc/views/razor)。

## <a name="raw-html"></a>原始 HTML

字串通常呈現使用 DOM 文字節點，這表示它們可能包含任何標記為忽略，並視為常值文字。 若要轉譯原始 HTML，換行中的 HTML 內容`MarkupString`值。 此值會以 HTML 或 SVG 剖析並插入至 dom。

> [!WARNING]
> 從任何建構的原始 HTML 呈現未受信任的來源**安全性風險**，應該予以避免 ！

下列範例顯示如何使用`MarkupString`要轉譯的輸出的元件加入靜態 HTML 內容的區塊類型：

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>樣板化的元件

樣板化的元件是接受一或多個 UI 範本做為參數，然後為元件的轉譯邏輯的一部分使用的元件。 樣板化元件可讓您撰寫更一般的元件可重複使用的較高層級元件。 一些範例包括：

* 資料表元件，可讓使用者指定資料表的標頭、 資料列和頁尾範本。
* 可讓使用者從清單中指定用於轉譯項目範本清單元件。

### <a name="template-parameters"></a>範本參數

藉由指定一或多個元件的參數型別定義的樣板化的元件`RenderFragment`或`RenderFragment<T>`。 轉譯片段代表一段所呈現之元件的 UI。 轉譯片段 （選擇性） 使用參數來叫用轉譯片段時，可以指定。

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

當使用樣板化的元件，可以使用參數的名稱相符的子項目來指定範本參數 (`TableHeader`和`RowTemplate`在下列範例中):

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a>範本內容參數

元件的引數的型別`RenderFragment<T>`傳遞項目具有名為的隱含參數`context`(例如從上述的程式碼範例中， `@context.PetId`)，但您可以變更參數名稱使用`Context`子上的屬性項目。 在下列範例中，`RowTemplate`項目的`Context`屬性會指定`pet`參數：

```cshtml
<TableTemplate Items="@pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

或者，您可以指定`Context`元件項目上的屬性。 指定`Context`屬性會套用至所有指定的範本參數。 這適合用於當您想要指定隱含子內容的內容參數名稱 (而不需要任何包裝子項目)。 在下列範例中，`Context`屬性會出現在`TableTemplate`項目，並套用至所有的範本參數：

```cshtml
<TableTemplate Items="@pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a>泛型類型的元件

樣板化的元件通常以一般方式是型別。 例如，一般的清單檢視範本元件可以用來呈現`IEnumerable<T>`值。 若要定義一般元件，請使用`@typeparam`指示詞來指定型別參數。

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

當使用泛型型別元件，盡可能推斷型別參數：

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

否則，型別參數必須明確指定使用的屬性，以符合類型參數的名稱。 在下列範例中，`TItem="Pet"`指定的型別：

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>階層式值和參數

在某些情況下，不方便流程資料從子系的元件使用的上階元件[元件參數](#component-parameters)，特別是在有數個元件層級。 階層式值和參數解決這個問題，並提供便利的方式提供的值及其下階的元件的所有上階元件。 階層式值和參數也會提供元件，以協調的方式。

### <a name="theme-example"></a>佈景主題的範例

在下列*佈景主題*範例應用程式，從範例`ThemeInfo`類別會指定元件階層往下流動，以便所有指定的組件的應用程式內的按鈕都共用相同的樣式的佈景主題資訊。

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

上階元件可以提供階層式值，使用串聯值元件。 階層式值元件包裝子樹狀結構的元件階層，並提供該樹狀子目錄內的所有元件的單一值。

例如，範例應用程式指定佈景主題的資訊 (`ThemeInfo`) 中的應用程式的版面配置組成的版面配置主體的所有元件的串聯式參數為其中一個`@Body`屬性。 `ButtonClass` 被指派的值`btn-success`配置元件中。 任何子系的元件可以使用這個屬性，透過`ThemeInfo`階層式物件。

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

若要讓使用串聯值，元件會宣告使用串聯參數`[CascadingParameter]`屬性，或者根據字串名稱值：

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

如果您有多個相同類型的階層式值，而且需要在相同的樹狀子目錄中加以區隔，以字串名稱值的繫結是相關。

串聯值繫結至串聯參數的型別。

範例應用程式中的階層式值參數佈景主題元件繫結至`ThemeInfo`串聯值的串聯式參數。 參數用來設定其中一個元件所顯示的按鈕之 CSS 類別。

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>TabSet 範例

串聯參數也可讓元件階層中共同作業的元件。 例如，請考慮下列*TabSet*範例應用程式中的範例。

範例應用程式有`ITab`索引標籤實作的介面：

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

階層式值參數 TabSet 元件使用的索引標籤上設定的元件，其中包含數個索引標籤上的元件：

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

子索引標籤上的元件不明確當做參數傳遞至索引標籤設定。 相反地，子索引標籤上的元件是索引標籤上設定的子內容的一部分。 不過，索引標籤設定仍然需要知道每個索引標籤上的元件，以便它可以轉譯標頭和作用中的索引標籤。若要啟用這項協調，而不需要額外的程式碼，此索引標籤上設定的元件*做為階層式值可以提供本身*，然後挑選的子系的索引標籤元件。

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

子系的索引標籤上的元件擷取包含索引標籤組作為階層式的參數，因此 索引標籤上的元件將本身新增至索引標籤上設定，而座標上哪一個索引標籤為作用中。

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Razor 範本

呈現使用 Razor 範本語法就可以定義片段。 Razor 範本是用來定義 UI 程式碼片段，並假設以下列格式：

```cshtml
@<tag>...</tag>
```

下列範例說明如何指定`RenderFragment`和`RenderFragment<T>`值。

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

呈現使用的 Razor 範本可以做為引數傳遞到樣板化的元件，或直接呈現所定義的片段。 比方說前, 一個範本會直接轉譯下列 Razor 標記：

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

轉譯輸出：

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>手動 RenderTreeBuilder 邏輯

`Microsoft.AspNetCore.Components.RenderTree` 提供方法來管理元件和項目，包括建置元件，以手動方式在C#程式碼。

> [!NOTE]
> 使用`RenderTreeBuilder`建立元件是進階的案例。 格式不正確的元件 （例如，未封閉的標記） 會導致未定義的行為。

請考慮下列的寵物詳細資料元件 (*PetDetails.razor* Razor 元件;*PetDetails.cshtml* Blazor 中)，這可以手動建立另一個元件：

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

在下列範例中，在迴圈`CreateComponent`方法會產生三個寵物詳細資料元件。 當呼叫`RenderTreeBuilder`方法來建立的元件 (`OpenComponent`和`AddAttribute`)，序號會將原始程式碼行號。 差異演算法依賴對應至不同的行，程式碼，不是不同的序號 Razor 元件呼叫的引動過程。 建立具有元件時`RenderTreeBuilder`方法 」、 「 硬式編碼序號的引數。 **使用來產生序號的計算或計數器可能會導致效能不佳。**

建置元件 (*BuiltContent.razor* Razor 元件;*BuiltContent.cshtml* Blazor 中):

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```
