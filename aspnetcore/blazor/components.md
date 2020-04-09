---
title: 建立與使用ASP.NET核心剃鬚刀元件
author: guardrex
description: 瞭解如何創建和使用 Razor 元件,包括如何綁定到數據、處理事件和管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: bc1d07aef9cd60b89343a034168daa6754f4421b
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80306503"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>建立與使用ASP.NET核心剃鬚刀元件

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)([如何下載](xref:index#how-to-download-a-sample))

Blazor應用程式是使用*元件*構建的。 元件是用戶介面 (UI) 的自包含塊,如頁面、對話框或窗體。 元件包括 HTML 標記和注入資料或回應 UI 事件所需的處理邏輯。 元件靈活輕巧。 它們可以嵌套、重用和在項目之間共用。

## <a name="component-classes"></a>元件類

元件在[Razor](xref:mvc/views/razor)元件檔 *(.razor*) 中使用 C# 和 HTML 標記的組合實現。 中Blazor一個元件被稱為 Razor*元件*。

元件的名稱必須以大寫字元開頭。 例如 *,MyCool 元件.razor*是有效的,*而 myCool元件.razor*無效。

元件的 UI 使用 HTML 定義。 動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。 編譯應用時,HTML 標記和 C# 呈現邏輯將轉換為元件類。 生成的類的名稱與檔的名稱匹配。

元件類別的成員均定義於 `@code` 區塊中。 在塊`@code`中,元件狀態(屬性、欄位)使用事件處理或定義其他元件邏輯的方法指定。 允許一個以上的 `@code` 區塊。

元件成員可以使用 以`@`開頭的 C# 運算式用作元件呈現邏輯的一部分。 例如,C# 字段通過`@`前置字位名稱來呈現。 以下範例計算和渲染:

* `_headingFontStyle`到`font-style`的 CSS 屬性值。
* `_headingText`元素的內容`<h1>`。

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

最初呈現元件后,元件將重新生成其呈現樹以回應事件。 Blazor然後,將新的呈現樹與上一個呈現樹進行比較,並將任何修改應用於瀏覽器的文檔物件模型 (DOM)。

元件是普通的 C# 類,可以放置在專案中的任意位置。 生成網頁的元件通常駐留在 *「頁面」* 資料夾中。 非頁面元件經常放置在 *「共用」* 資料夾或添加到專案的自訂資料夾中。

通常,元件的命名空間派生自應用的根命名空間和應用中元件的位置(資料夾)。 若套用的根命名空間`BlazorApp`是`Counter`, 並且元件駐留在 *「 頁面」* 資料夾中:

* 元件`Counter`的命名空間為`BlazorApp.Pages`。
* 元件的完全限定類型名稱稱為`BlazorApp.Pages.Counter`。

有關詳細資訊,請參閱[導入元件](#import-components)部分。

要使用自訂資料夾,請將自訂資料夾的命名空間添加到父元件或應用的 *_Imports.razor*檔中。 例如, 當套用的根命名`BlazorApp`空間為 : 時,以下命名空間使*元件*資料夾中的元件可用:

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a>靜態資產

Blazor遵循ASP.NET核心應用將靜態資產放在專案的[Web 根 (wwwroot) 資料夾下的](xref:fundamentals/index#web-root)約定。

使用基礎相對路徑`/`( ) 引用靜態資產的 Web 根。 在下面的範例中 *,logo.png*實際位於 *[PROJECT ROOT]/wwwroot/影像*資料夾中:

```razor
<img alt="Company logo" src="/images/logo.png" />
```

剃刀元件**不支援**波浪斜槓表示法`~/`( 。

有關設定應用基本路徑的資訊,請參<xref:host-and-deploy/blazor/index#app-base-path>閱 。

## <a name="tag-helpers-arent-supported-in-components"></a>元件中不支援標記說明器

Razor 元件 *(.razor*檔案)不支援[標籤說明器](xref:mvc/views/tag-helpers/intro)。 要在Blazor中 提供類似標記説明程式的功能,請創建與標記説明程式具有相同功能的元件,然後改用該元件。

## <a name="use-components"></a>使用元件

元件可以透過使用 HTML 元素語法聲明它們來包括其他元件。 使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。

*Index.razor*中列標記呈現實體`HeadingComponent`:

```razor
<HeadingComponent />
```

*元件/標題元件.razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

如果元件包含具有大寫第一個字母與元件名稱不匹配的 HTML 元素,則會發出警告,指示該元素具有意外的名稱。 為`@using`元件的命名空間添加指令使元件可用,從而解決警告。

## <a name="routing"></a>路由

路由Blazor是通過向應用中的每個可訪問元件提供路由範本來實現的。

編譯具有`@page`指令的 Razor 檔時,將為生成<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>的類指定 路由範本。 在運行時,路由器查找具有的`RouteAttribute`元件類,並呈現具有與請求 URL 匹配的路由範本的元件。

```razor
@page "/ParentComponent"

...
```

如需詳細資訊，請參閱 <xref:blazor/routing>。

## <a name="parameters"></a>參數

### <a name="route-parameters"></a>路由參數

元件可以從`@page`指令中提供的路由範本接收路由參數。 路由器使用路由參數填充相應的元件參數。

*頁面/路由參數.剃鬚刀*:

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

不支援可選參數,因此在前面的示例中應用了`@page`兩個指令。 第一個允許在沒有參數的情況下導航到元件。 第二`@page`個指令接收`{text}`路由參數並將值分配給`Text`屬性 。

Razor 元件 *(.razor*)**不支援**擷取跨多個資料夾邊界的路徑的 *「全部」* 參數語法 (`*`/`**`)

### <a name="component-parameters"></a>元件參數

元件可以具有*元件參數*,這些參數使用具有`[Parameter]`屬性的元件類上的公共屬性進行定義。 使用這些屬性來指定標記中元件的引數。

*元件/子元件.razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

在範例應用程式中的以下範例中,`ParentComponent`將設定`Title``ChildComponent`屬性的值。

*頁面/父元件.razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a>子內容

元件可以設置另一個元件的內容。 分配元件提供指定接收元件的標記之間的內容。

在下面的範例中,`ChildComponent`具有`ChildContent``RenderFragment`表示 屬性, 表示要呈現的 UI 段。 的值`ChildContent`位於元件的標記中,其中應呈現內容。 的值`ChildContent`從父元件接收,並在 Bootstrap`panel-body`面板的 中呈現。

*元件/子元件.razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> 接收`RenderFragment`內容的屬性必須按約定`ChildContent`命名 。

`ParentComponent`範例應用中可以通過將內容放置`<ChildComponent>`在 標記中來`ChildComponent`提供用於呈現 的內容。

*頁面/父元件.razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>屬性計算與任意參數

除了元件聲明的參數之外,元件還可以捕獲和呈現其他屬性。 可以在字典中捕獲其他屬性,然後在使用[`@attributes`](xref:mvc/views/razor#attributes)Razor 指令呈現元件時*將其壓縮*到元素上。 在定義生成支援各種自定義的標記元素的元件時,此方案非常有用。 例如,單獨定義支援許多參數的的屬性`<input>`可能很乏味。

在下面的`<input>`範例中,第一個`id="useIndividualParams"`元素 ( ) 使用單個元件`<input>`參數,`id="useAttributesDict"`而第二個元素 ( ) 使用屬性 splatt:

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

參數的類型必須使用字串鍵 。`IEnumerable<KeyValuePair<string, object>>` 在這種情況下`IReadOnlyDictionary<string, object>`,使用也是一個選項。

使用這兩`<input>`種方法的呈現元素是相同的:

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

要接受任何屬性,請使用`[Parameter]``CaptureUnmatchedValues`屬性將 元件參數定義為`true`:

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

上`CaptureUnmatchedValues``[Parameter]`的屬性允許參數匹配與任何其他參數不匹配的所有屬性。 元件只能使用`CaptureUnmatchedValues`定義單個參數。 使用的屬性類型`CaptureUnmatchedValues``Dictionary<string, object>`必須與字串鍵一起分配。 `IEnumerable<KeyValuePair<string, object>>`或`IReadOnlyDictionary<string, object>`此方案中的選項。

`@attributes`相對於元素屬性位置的位置非常重要。 在`@attributes`元素上進行壓縮時,從右向左處理屬性(最後處理到第一個)。 請考慮使用`Child`元件的以下元件範例:

*父元件.razor*:

```razor
<ChildComponent extra="10" />
```

*子元件.razor*:

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

元件`Child`屬性`extra`設定為 右`@attributes`方 。 元件`Parent`的渲染`<div>`包含在透過額外屬性`extra="5"`時,因為屬性從右向左處理(最後處理到第一):

```html
<div extra="5" />
```

`extra`在下面的範例中,與`@attributes`的順序在`Child`元件的`<div>`中顛倒了 :

*父元件.razor*:

```razor
<ChildComponent extra="10" />
```

*子元件.razor*:

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

透過額外屬性`<div>``extra="10"`時,`Parent`元件中呈現的包含:

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a>捕捉對元件的參考

元件參照提供參考元件的實體, 以便您可以向該實體的指令,例如`Show`或`Reset`。 要捕獲元件引用,

* 向子[`@ref`](xref:mvc/views/razor#ref)元件添加屬性。
* 定義與子元件類型相同的欄位。

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

呈現元件時,`_loginDialog`該欄位將填充`MyLoginDialog`子元件實例。 然後,您可以在元件實例上調用 .NET 方法。

> [!IMPORTANT]
> 僅當`_loginDialog`呈現元件及其輸出包括`MyLoginDialog`元素后,才會填充該變數。 在那之前,沒有什麼可參考的。 您可以在元件完成呈現後操作元件參考,請使用[On 後 RenderAsync 或 On 後渲染方法](xref:blazor/lifecycle#after-component-render)。

要參考循環中的元件,請參考[擷取擷取對多個類似子元件(dotnet/aspnetcore #13358) 的參考](https://github.com/dotnet/aspnetcore/issues/13358)。

雖然捕獲元件引用使用類似的語法來[捕獲元素引用](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements),但它不是 JAVAScript 互通功能。 元件引用不會傳遞給僅在 .NET&mdash;代碼 中使用的 JavaScript 代碼。

> [!NOTE]
> **請勿**使用元件引用來更改子元件的狀態。 相反,使用普通聲明性參數將數據傳遞給子元件。 使用正常聲明性參數會導致子元件在正確時間自動重新呈現。

## <a name="invoke-component-methods-externally-to-update-state"></a>外部呼叫元件方法以更新狀態

Blazor使用`SynchronizationContext`強制執行的單個邏輯線程。 元件的[生命週期方法和](xref:blazor/lifecycle)由Blazor引發的任何事件回調都`SynchronizationContext`在此 執行。 如果必須根據外部事件(如計時器或其他通知)更新元件,請使用方法`InvokeAsync`,該方法將調度到Blazor的`SynchronizationContext`。

例如,考慮通知*器服務*,該服務可以通知任何已更新狀態的偵聽元件:

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

註冊為`NotifierService`單項:

* 在BlazorWebAssembly 中`Program.Main`,在 中註冊服務:

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* 在Blazor「伺服器」中,`Startup.ConfigureServices`在中註冊服務:

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

使用`NotifierService`更新元件:

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

在前面的`NotifierService`範例中,呼叫元件`OnNotify`的方法以外的Blazor `SynchronizationContext` ' `InvokeAsync`用於切換到正確的上下文並排隊渲染。

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>使用\@鍵控制元素與元件的保留

在呈現元素或元件的清單以及隨後更改的元素或元件時,Blazor其擴散演演演算法必須決定保留以前的元素或元件中的哪些元素或元件,以及模型物件應該如何映射到它們。 通常,此過程是自動的,可以忽略,但在某些情況下,您可能需要控制該過程。

請考慮下列範例：

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

`People`集合的內容可能會隨著插入、刪除或重新排序的條目而更改。 當元件重新成成成時`<DetailsEditor>`, 元件可能會更改為接收`Details`不同的 參數值。 這可能導致比預期更複雜的重新渲染。 在某些情況下,重新渲染可能會導致明顯的行為差異,例如元素焦點丟失。

可以使用`@key`指令屬性控制映射過程。 `@key`使擴散演算法保證根據鍵的值保留元素或元件:

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="@person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

當`People`集合變更時,擴散演演演算法將`<DetailsEditor>``person`保留實例和 實體之間的關聯:

* 如果從`Person``People`清單中刪除了 , 則`<DetailsEditor>`僅從 UI 中刪除相應的實例。 其他實例保持不變。
* 如果將`Person`插入到清單中的某個位置,則在相應的`<DetailsEditor>`位置 插入一個新實例。 其他實例保持不變。
* 如果`Person`重新排序了條目,則`<DetailsEditor>`相應的 實例將保留並在 UI 中重新排序。

在某些情況下,使用`@key`可最大程度地降低重新渲染的複雜性,並避免 DOM 的有狀態部分(如焦點位置)發生潛在問題。

> [!IMPORTANT]
> 鍵是每個容器元素或元件的本地。 不會跨文檔全域比較密鑰。

### <a name="when-to-use-key"></a>何時使用\@金鑰

通常,每當呈現清單(例如`@key`,在`@foreach`塊中)並且存在合適的值來定義`@key`時 ,使用 都有意義。

您還可以使用`@key`Blazor防止 在物件變更時保留元素或元件子樹:

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

如果`@currentPerson`發生變更`@key`, 屬性Blazor指令將`<div>`強制放棄整個 及其後代,並在 UI 中使用新元素和元件重建子樹。 如果需要保證在更改時`@currentPerson`不保留任何 UI 狀態,則此功能非常有用。

### <a name="when-not-to-use-key"></a>何時不使用\@鍵

與`@key`時發生梭差會有性能成本。 性能成本並不大,但僅指定`@key`控制元素或元件保留規則是否有利於應用。

即使`@key`不使用Blazor, 也會盡可能保留子元素和元件實例。 使用`@key`的唯一優點是控制模型實例*如何*映射到保留的元件實例,而不是選擇映射的擴散演演演算法。

### <a name="what-values-to-use-for-key"></a>鍵使用\@的值

通常,為`@key`:

* 對物件實例建模(例如,如前面的`Person`示例中所示的實例)。 這可確保基於物件引用相等性保存。
* 唯一識別碼(例如,類型的主要`int`鍵值`string``Guid`。

確保用於的值`@key`不衝突。 如果在同一父元素中檢測到衝突值,則Blazor引發異常,因為它無法確定將舊元素或元件映射到新元素或元件。 僅使用不同的值,如物件實例或主鍵值。

## <a name="partial-class-support"></a>部分類別支援

剃刀元件作為部分類生成。 剃刀元件使用以下任一方法創作:

* C# 程式碼[`@code`](xref:mvc/views/razor#code)在塊中定義,在單個檔中包含 HTML 標記和 Razor 程式碼。 Blazor樣本使用此方法定義其 Razor 元件。
* C# 程式碼放置在定義為部分類的代碼後面檔中。

下面的範例顯示從範本生成的`Counter`應用中`@code`具有塊Blazor的 預設元件。 HTML 標籤、Razor 代碼與 C# 程式碼位於同一檔中:

*計數器.razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

也可以`Counter`使用帶有部分類別的代碼後面檔案建立元件:

*計數器.razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.razor.cs*:

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

根據需要向部分類檔添加任何必需的命名空間。 Razor 元件使用的典型命名空間包括:

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>指定基類別

該[`@inherits`](xref:mvc/views/razor#inherits)指令可用於為元件指定基類。 下面的範例展示如何繼承基類,`BlazorRocksBase`以提供元件的屬性和方法。 基類應派生自`ComponentBase`。

*頁面/布拉佐羅克斯.剃鬚刀*:

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.cs*:

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

## <a name="specify-an-attribute"></a>指定屬性

屬性可以在 Razor 元件中使用[`@attribute`](xref:mvc/views/razor#attribute)指令 指定。 以下範例將`[Authorize]`該屬性套用於元件類別:

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>匯入元件

使用 Razor 創作的元件的命名空間基於(按優先順序順序):

* [`@namespace`](xref:mvc/views/razor#namespace)Razor 檔案(*.razor*`@namespace BlazorSample.MyNamespace`) 標記( )中指定 。
* 專案檔`RootNamespace`中 (`<RootNamespace>BlazorSample</RootNamespace>`。
* 專案名稱,取自專案檔的檔名 *(.csproj*) 以及從專案根到元件的路徑。 例如,框架解析 *[PROJECT ROOT]/頁面/索引.razor* *(BlazorSample.csproj*) 到命名`BlazorSample.Pages`空間 。 元件遵循 C# 名稱綁定規則。 對於此範例`Index`中的元件,作用網域中的元件都是所有元件:
  * 在同一個資料夾中,*頁面*。
  * 專案根中的元件不顯式指定其他命名空間。

使用 Razor[`@using`](xref:mvc/views/razor#using)的 指令將不同命名空間中定義的元件引入範圍。

如果另一個`NavMenu.razor`元件 (中)存在於*BlazorSample/共用/* 資料夾中,則`Index.razor`該元件`@using`可用於以下 語句:

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

也可以使用完全限定的名稱引用元件,這不需要指令[`@using`](xref:mvc/views/razor#using):

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> 不支持`global::`資格認證。
>
> 不支援使用別名`using`語句導入元件(例如,)。 `@using Foo = Bar`
>
> 不支援部分限定名稱。 例如,不支援新增`@using BlazorSample`和引用`NavMenu.razor` `<Shared.NavMenu></Shared.NavMenu>` 。

## <a name="conditional-html-element-attributes"></a>檔案 HTML 元素屬性

HTML 元素屬性根據 .NET 值有條件地呈現。 如果值為`false``null`或 ,則不呈現屬性。 如果值為`true`,則屬性將最小化。

在下面的範例中,`IsCompleted`確定`checked`是否呈現在元素的標籤中:

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

如果是`IsCompleted``true`,則複選框將呈現為:

```html
<input type="checkbox" checked />
```

如果是`IsCompleted``false`,則複選框將呈現為:

```html
<input type="checkbox" />
```

如需詳細資訊，請參閱 <xref:mvc/views/razor>。

> [!WARNING]
> 當 .NET[aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)`bool`類型為 時,某些 HTML 屬性(如 aria 按下)無法正常工作。 在這些情況下,請使用`string`型態而不是`bool`。

## <a name="raw-html"></a>原始 HTML

字串通常使用 DOM 文字節點呈現,這意味著它們可能包含的任何標記都將被忽略並視為文本文本。 要呈現原始 HTML,`MarkupString`在值中包裝 HTML 內容。 該值被解析為 HTML 或 SVG 並插入到 DOM 中。

> [!WARNING]
> 渲染從任何不受信任的源構造的原始 HTML 存在**安全風險**,應避免使用!

下面的範例顯示使用`MarkupString`類型將靜態 HTML 內容區塊加入元件的呈現輸出中:

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a>階層與參數

在某些情況下,使用[元件參數](#component-parameters)將數據從祖先元件流向後代元件不方便,尤其是在有多個元件層時。 級聯值和參數為祖先元件提供了一種為其所有後代元件提供值的便捷方法,從而解決了此問題。 級聯值和參數也為元件提供了一種協調方法。

### <a name="theme-example"></a>主題範例

在範例應用程式中的以下範例中,`ThemeInfo`類別指定向到元件層次結構的主題資訊,以便應用給定部分中的所有按鈕共用相同的樣式。

*UIThemeclasses/主題資訊.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

祖先元件可以使用級聯值元件提供級聯值。 元件`CascadingValue`包裝元件層次結構的子樹,並向該子樹中的所有元件提供單個值。

例如,範例應用將應用佈局之一的主題資訊`ThemeInfo`( )`@Body`指定為構成 屬性佈局正文的所有元件的級聯參數。 `ButtonClass`在佈局元件中分配`btn-success`的值。 任何後代元件都可以通過`ThemeInfo`級聯物件使用此屬性。

`CascadingValuesParametersLayout`元件:

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

要使用級聯值,元件使用 屬性`[CascadingParameter]`聲明級聯參數。 級聯值按類型綁定到級聯參數。

在範例應用中,`CascadingValuesParametersTheme`元件將`ThemeInfo`級聯值綁定到級聯參數。 該參數用於為元件顯示的按鈕之一設置 CSS 類。

`CascadingValuesParametersTheme`元件:

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

要在同一子樹中級聯同一類型的多個值,請為每個`Name``CascadingValue`元件及其相應的`CascadingParameter`提供唯一字串。 在下面的範例中,兩個`CascadingValue`元件`MyCascadingType`依名稱級聯不同的實例:

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

在子體元件中,級聯參數從祖先元件中的相應級聯值接收其值:

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>選項卡集範例

級聯參數還使元件能夠跨元件層次結構進行協作。 例如,在示例應用中考慮以下*TabSet*示例。

範例應用程式具有介面`ITab`,用於選項卡實現:

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

元件`CascadingValuesParametersTabSet`使用包含多個`TabSet``Tab`元件的元件:

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

子`Tab`元件不會顯示參數為參數傳遞給`TabSet`。 相反,子`Tab`元件是子`TabSet`內容的一部分。 但是,仍`TabSet`需要瞭解每個`Tab`元件,以便它可以呈現標頭和活動選項卡。為了啟用這種協調而無需其他代碼,`TabSet`元件*可以作為級聯值提供自身*,然後由`Tab`後代 元件選取。

`TabSet`元件:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

後代`Tab`元件將包含`TabSet`捕獲為級聯參數,因此`Tab`元件將自己添加到 和座標上`TabSet`哪個選項卡處於活動狀態。

`Tab`元件:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>剃刀範本

可以使用 Razor 樣本語法定義渲染片段。 Razor 樣本是定義 UI 代碼段並採用以下格式的一種方式:

```razor
@<{HTML tag}>...</{HTML tag}>
```

下面的範例說明了如何在元件中直接`RenderFragment`指定`RenderFragment<T>`和值和呈現範本。 成像片段也可以為參數傳遞為[樣本化的元件](xref:blazor/templated-components)。

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

上述代碼的呈現輸出:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a>可延伸向量圖形 (SVG) 影像

由於Blazor成像 HTML,瀏覽器支援的影像,包括可延伸向量圖形 (SVG) 影像 *(.svg*) 透過`<img>`標記支援:

```html
<img alt="Example image" src="some-image.svg" />
```

同樣,樣式表檔的 CSS 規則 *(.css)* 中支援 SVG 圖像。

```css
.my-element {
    background-image: url("some-image.svg");
}
```

但是,並非所有方案都支援內聯 SVG 標記。 如果將`<svg>`標記直接放入元件檔 *(.razor),* 則支援基本圖像呈現,但不支援許多高級方案。 例如,`<use>`標記目前不受尊重`@bind`, 並且不能與某些 SVG 標記一起使用。 我們期望在未來的版本中解決這些限制。

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/server>&ndash;包括有關構建Blazor必須應對資源耗盡的伺服器應用的指導。
