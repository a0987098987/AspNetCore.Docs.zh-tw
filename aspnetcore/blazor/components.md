---
title: 建立和使用 ASP.NET Core Razor 元件
author: guardrex
description: 瞭解如何建立和使用 Razor 元件，包括如何系結至資料、處理事件，以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/25/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: bc1d07aef9cd60b89343a034168daa6754f4421b
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306503"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>建立和使用 ASP.NET Core Razor 元件

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

Blazor 應用程式是使用*元件*所建立。 「元件」（component）是一種獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。 元件包含 HTML 標籤，以及插入資料或回應 UI 事件所需的處理邏輯。 元件具有彈性且輕量。 它們可以在專案之間進行嵌套、重複使用及共用。

## <a name="component-classes"></a>元件類別

元件會使用C#和 HTML 標籤的組合，在[razor](xref:mvc/views/razor)元件檔案（*razor*）中執行。 Blazor 中的元件正式稱為*Razor 元件*。

元件的名稱必須以大寫字元開頭。 例如， *MyCoolComponent*有效，而*MyCoolComponent*則無效。

元件的 UI 是使用 HTML 定義的。 動態轉譯邏輯 (例如迴圈、條件、運算式) 是使用內嵌的 C# 語法 (稱為 [Razor](xref:mvc/views/razor)) 來新增的。 編譯應用程式時，會將 HTML 標籤和C#轉譯邏輯轉換成元件類別。 產生的類別名稱與檔案的名稱相符。

元件類別的成員均定義於 `@code` 區塊中。 在 `@code` 區塊中，會使用事件處理或定義其他元件邏輯的方法來指定元件狀態（屬性、欄位）。 允許一個以上的 `@code` 區塊。

元件成員可以使用C#開頭為 `@`的運算式，做為元件轉譯邏輯的一部分。 例如， C#欄位是藉由在功能變數名稱前面加上 `@` 來呈現。 下列範例會評估並呈現：

* `_headingFontStyle` 至 `font-style`的 CSS 屬性值。
* `_headingText` 至 `<h1>` 元素的內容。

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

一開始呈現元件之後，元件會重新產生其轉譯樹狀結構，以回應事件。 Blazor 接著會比較新的轉譯樹狀結構與上一個，並將任何修改套用至瀏覽器的檔物件模型（DOM）。

元件是一般C#類別，可以放在專案內的任何位置。 產生網頁的元件通常會位於*Pages*資料夾中。 非頁面元件通常會放在*共用*資料夾中，或加入至專案的自訂資料夾中。

一般而言，元件的命名空間是從應用程式的根命名空間和元件在應用程式內的位置（資料夾）衍生而來。 如果應用程式的根命名空間是 `BlazorApp`，且 `Counter` 元件位於*Pages*資料夾中：

* `Counter` 元件的命名空間為 `BlazorApp.Pages`。
* 元件的完整類型名稱為 `BlazorApp.Pages.Counter`。

如需詳細資訊，請參閱匯[入元件](#import-components)一節。

若要使用自訂資料夾，請將自訂資料夾的命名空間新增至父元件或應用程式的 *_Imports razor*檔案。 例如，下列命名空間會在應用程式的根命名空間為 `BlazorApp`時，讓*元件*資料夾中的元件可供使用：

```razor
@using BlazorApp.Components
```

## <a name="static-assets"></a>靜態資產

Blazor 遵循 ASP.NET Core 應用程式在專案的[web 根目錄（wwwroot）資料夾](xref:fundamentals/index#web-root)下放置靜態資產的慣例。

使用基底相對路徑（`/`）來參考靜態資產的 web 根目錄。 在下列範例中，*標誌 .png*實際上位於 *{PROJECT ROOT}/wwwroot/images*資料夾中：

```razor
<img alt="Company logo" src="/images/logo.png" />
```

Razor 元件**不**支援波形符-斜線標記法（`~/`）。

如需設定應用程式基底路徑的詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#app-base-path>。

## <a name="tag-helpers-arent-supported-in-components"></a>元件中不支援標記協助程式

Razor 元件（*razor*檔案）中不支援[標記](xref:mvc/views/tag-helpers/intro)協助程式。 若要在 Blazor中提供標籤協助程式的功能，請建立元件，其功能與標記協助程式相同，並改用元件。

## <a name="use-components"></a>使用元件

元件可以包含其他元件，方法是使用 HTML 專案語法來宣告它們。 使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。

在*Index*中的下列標記會呈現 `HeadingComponent` 實例：

```razor
<HeadingComponent />
```

*Components/HeadingComponent. razor*：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

如果元件包含的 HTML 專案具有大寫的第一個字母，但不符合元件名稱，則會發出警告，指出該元素有未預期的名稱。 為元件的命名空間加入 `@using` 指示詞可讓元件可用，這會解析警告。

## <a name="routing"></a>路由

Blazor 中的路由會藉由提供路由範本給應用程式中的每個可存取元件來達到目的。

當您編譯具有 `@page` 指示詞的 Razor 檔案時，會提供指定路由範本 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> 給產生的類別。 在執行時間，路由器會尋找具有 `RouteAttribute` 的元件類別，並轉譯哪一個元件的路由範本符合要求的 URL。

```razor
@page "/ParentComponent"

...
```

如需詳細資訊，請參閱 <xref:blazor/routing>。

## <a name="parameters"></a>參數

### <a name="route-parameters"></a>路由參數

元件可以從 `@page` 指示詞所提供的路由範本接收路由參數。 路由器會使用路由參數來填入對應的元件參數。

*Pages/RouteParameter. razor*：

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

不支援選擇性參數，因此在上述範例中會套用兩個 `@page` 的指示詞。 第一個則允許不使用參數導覽至元件。 第二個 `@page` 指示詞會接收 `{text}` 路由參數，並將值指派給 `Text` 屬性。

Razor 元件（*razor*）**不**支援*Catch-all*參數語法（`*`/`**`），它會跨多個資料夾界限捕捉路徑。

### <a name="component-parameters"></a>元件參數

元件可以具有*元件參數*，其使用元件類別上的公用屬性（具有 `[Parameter]` 屬性）來定義。 使用這些屬性來指定標記中元件的引數。

*Components/ChildComponent. razor*：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

在範例應用程式的下列範例中，`ParentComponent` 設定 `ChildComponent`的 `Title` 屬性值。

*Pages/ParentComponent. razor*：

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a>子內容

元件可以設定另一個元件的內容。 指派元件會在指定接收元件的標記之間提供內容。

在下列範例中，`ChildComponent` 具有代表 `RenderFragment`的 `ChildContent` 屬性，代表要呈現的 UI 區段。 `ChildContent` 的值位於元件的標記中，應在其中呈現內容。 `ChildContent` 的值會從父元件接收，並在啟動程式面板的 `panel-body`內轉譯。

*Components/ChildComponent. razor*：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> 接收 `RenderFragment` 內容的屬性必須依照慣例命名 `ChildContent`。

範例應用程式中的 `ParentComponent` 可以藉由將內容放入 `<ChildComponent>` 標記中，來提供呈現 `ChildComponent` 的內容。

*Pages/ParentComponent. razor*：

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>屬性展開和任意參數

除了元件的宣告參數之外，元件還可以捕捉和轉譯其他屬性。 您可以在字典中捕捉其他屬性，然後在使用[`@attributes`](xref:mvc/views/razor#attributes) Razor 指示詞轉譯元件時，將其*splatted*到元素上。 當定義的元件會產生支援各種自訂的標記專案時，這個案例就很有用。 例如，針對支援許多參數的 `<input>` 分別定義屬性，可能會很繁瑣。

在下列範例中，第一個 `<input>` 元素（`id="useIndividualParams"`）使用個別的元件參數，而第二個 `<input>` 元素（`id="useAttributesDict"`）使用屬性展開：

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

參數的類型必須使用字串索引鍵來執行 `IEnumerable<KeyValuePair<string, object>>`。 在此案例中，使用 `IReadOnlyDictionary<string, object>` 也是一個選項。

使用這兩種方法轉譯的 `<input>` 元素都相同：

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

若要接受任意屬性，請使用 `[Parameter]` 屬性定義元件參數，並將 `CaptureUnmatchedValues` 屬性設定為 `true`：

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

`[Parameter]` 上的 `CaptureUnmatchedValues` 屬性可讓參數符合所有不符合任何其他參數的屬性。 元件只能定義具有 `CaptureUnmatchedValues`的單一參數。 搭配 `CaptureUnmatchedValues` 使用的屬性類型必須可從具有字串索引鍵的 `Dictionary<string, object>` 指派。 `IEnumerable<KeyValuePair<string, object>>` 或 `IReadOnlyDictionary<string, object>` 也是此案例中的選項。

相對於元素屬性位置的 `@attributes` 位置很重要。 當 `@attributes` 在專案上 splatted 時，會從右至左（最後一個）處理屬性。 請考慮使用 `Child` 元件的下列元件範例：

*ParentComponent razor*：

```razor
<ChildComponent extra="10" />
```

*ChildComponent razor*：

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

`Child` 元件的 `extra` 屬性會設定為 `@attributes`的右邊。 `Parent` 元件的轉譯 `<div>` 會在透過額外屬性傳遞時包含 `extra="5"`，因為屬性是由右至左處理（最後一個）：

```html
<div extra="5" />
```

在下列範例中，`extra` 和 `@attributes` 的順序會在 `Child` 元件的 `<div>`中反轉：

*ParentComponent razor*：

```razor
<ChildComponent extra="10" />
```

*ChildComponent razor*：

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

當透過其他屬性傳遞時，`Parent` 元件中呈現的 `<div>` 會包含 `extra="10"`：

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a>捕獲元件的參考

元件參考提供參考元件實例的方法，讓您可以發出命令給該實例，例如 `Show` 或 `Reset`。 若要捕捉元件參考：

* 將[`@ref`](xref:mvc/views/razor#ref)屬性加入至子元件。
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

當元件呈現時，`_loginDialog` 欄位會填入 `MyLoginDialog` 子元件實例。 接著，您可以在元件實例上叫用 .NET 方法。

> [!IMPORTANT]
> 只有在轉譯元件之後才會填入 `_loginDialog` 變數，且其輸出會包含 `MyLoginDialog` 元素。 直到該點為止，沒有任何可參考的內容。 若要在元件完成呈現之後操作元件參考，請使用[OnAfterRenderAsync 或 OnAfterRender 方法](xref:blazor/lifecycle#after-component-render)。

若要參考迴圈中的元件，請參閱[捕捉多個類似子元件的參考（dotnet/aspnetcore #13358）](https://github.com/dotnet/aspnetcore/issues/13358)。

雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)，但它並不是 JavaScript interop 功能。 元件參考不會傳遞至 JavaScript 程式碼，&mdash;它們只會在 .NET 程式碼中使用。

> [!NOTE]
> 請勿**使用元件**參考來改變子元件的狀態。 請改用一般宣告式參數，將資料傳遞至子元件。 使用一般宣告式參數會導致子元件自動 rerender 正確的時間。

## <a name="invoke-component-methods-externally-to-update-state"></a>在外部叫用元件方法來更新狀態

Blazor 會使用 `SynchronizationContext` 來強制執行單一邏輯執行緒。 元件的[生命週期方法](xref:blazor/lifecycle)和 Blazor 所引發的任何事件回呼都會在此 `SynchronizationContext`上執行。 在事件中，必須根據外來事件（例如計時器或其他通知）更新元件，請使用 `InvokeAsync` 方法，這會分派給 Blazor的 `SynchronizationContext`。

例如，假設有一個通知程式*服務*可通知任何處于已更新狀態的「接聽」元件：

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

將 `NotifierService` 註冊為 singletion：

* 在 Blazor WebAssembly 中，于 `Program.Main`註冊服務：

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* 在 Blazor 伺服器中，于 `Startup.ConfigureServices`註冊服務：

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

使用 `NotifierService` 來更新元件：

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

在上述範例中，`NotifierService` 會在 Blazor的 `SynchronizationContext`之外叫用元件的 `OnNotify` 方法。 `InvokeAsync` 可用來切換至正確的內容，並將轉譯排入佇列。

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>使用 \@鍵來控制元素和元件的保留

當轉譯專案或元件的清單，以及後續變更的專案或元件時，Blazor的比較演算法必須決定哪些先前的專案或元件可以保留，以及模型物件應如何對應到它們。 一般來說，此程式是自動的，可以忽略，但在某些情況下，您可能會想要控制進程。

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

`People` 集合的內容可能會隨著插入、刪除或重新排序的專案而變更。 當元件 rerenders 時，`<DetailsEditor>` 元件可能會變更，以接收不同的 `Details` 參數值。 這可能會導致比預期更複雜的 rerendering。 在某些情況下，rerendering 可能會導致可見的行為差異，例如失去元素的焦點。

您可以使用 `@key` 指示詞屬性來控制對應進程。 `@key` 會使比較演算法保證根據索引鍵的值來保留元素或元件：

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

當 `People` 集合變更時，比較演算法會保留 `<DetailsEditor>` 實例與 `person` 實例之間的關聯：

* 如果 `Person` 從 `People` 清單中刪除，則只會從 UI 移除對應的 `<DetailsEditor>` 實例。 其他實例則保持不變。
* 如果在清單中的某個位置插入 `Person`，就會在該對應的位置插入一個新的 `<DetailsEditor>` 實例。 其他實例則保持不變。
* 如果重新排序 `Person` 專案，則會保留對應的 `<DetailsEditor>` 實例，並在 UI 中重新排序。

在某些情況下，使用 `@key` 會將 rerendering 的複雜性降到最低，並避免 DOM 的具狀態部分可能發生的問題，例如焦點位置。

> [!IMPORTANT]
> 索引鍵在每個容器元素或元件的本機。 金鑰不會在檔之間進行全域比較。

### <a name="when-to-use-key"></a>使用 \@金鑰的時機

一般而言，只要轉譯清單（例如，在 `@foreach` 區塊中），而且有適當的值來定義 `@key`，就有合理的使用 `@key`。

當物件變更時，您也可以使用 `@key` 來防止 Blazor 保留元素或元件子樹：

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

如果 `@currentPerson` 變更，`@key` attribute 指示詞會強制 Blazor 捨棄整個 `<div>` 及其下階，並使用新的元素和元件重建 UI 內的子樹。 如果您需要保證 `@currentPerson` 變更時不會保留任何 UI 狀態，這會很有用。

### <a name="when-not-to-use-key"></a>不使用 \@金鑰的時機

與 `@key`進行比較時，會產生效能成本。 效能成本並不大，但只有在控制元素或元件保留規則以受益于應用程式時，才指定 `@key`。

即使未使用 `@key`，Blazor 也會盡可能保留子項目和元件實例。 使用 `@key` 的唯一優點是控制模型實例*如何*對應至保留的元件實例，而不是用來選取對應的比較演算法。

### <a name="what-values-to-use-for-key"></a>要用於 \@金鑰的值

一般來說，提供下列其中一種類型的 `@key`值是合理的：

* 模型物件實例（例如，如先前範例所示的 `Person` 實例）。 這可確保根據物件參考的相等性進行保留。
* 唯一識別碼（例如，`int`、`string`或 `Guid`類型的主要金鑰值）。

請確定用於 `@key` 的值不會造成衝突。 如果在相同的父元素中偵測到衝突值，Blazor 會擲回例外狀況，因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。 只使用不同的值，例如物件實例或主鍵值。

## <a name="partial-class-support"></a>部分類別支援

Razor 元件是以部分類別的形式產生。 Razor 元件是使用下列其中一種方法來撰寫的：

* C#程式碼是以 HTML 標籤和 Razor 程式碼的[`@code`](xref:mvc/views/razor#code)區塊定義在單一檔案中。 Blazor 範本會使用這種方法來定義其 Razor 元件。
* C#程式碼會放在定義為部分類別的程式碼後置檔案中。

下列範例顯示在從 Blazor 範本產生的應用程式中，具有 `@code` 區塊的預設 `Counter` 元件。 HTML 標籤、Razor 程式碼和C#程式碼位於相同的檔案中：

*Counter. razor*：

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

您也可以使用具有部分類別的程式碼後置檔案來建立 `Counter` 元件：

*Counter. razor*：

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.razor.cs*：

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

視需要將任何必要的命名空間新增至部分類別檔案。 Razor 元件所使用的一般命名空間包括：

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>指定基類

[`@inherits`](xref:mvc/views/razor#inherits)指示詞可以用來指定元件的基類。 下列範例會顯示元件如何繼承基類（`BlazorRocksBase`），以提供元件的屬性和方法。 基類應該衍生自 `ComponentBase`。

*Pages/BlazorRocks. razor*：

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.cs*：

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

您可以使用[`@attribute`](xref:mvc/views/razor#attribute)指示詞，在 Razor 元件中指定屬性。 下列範例會將 `[Authorize]` 屬性套用至元件類別：

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>匯入元件

以 Razor 撰寫之元件的命名空間是根據（依優先順序排列）：

* Razor 檔案（*razor*）標記中的[`@namespace`](xref:mvc/views/razor#namespace)指定（`@namespace BlazorSample.MyNamespace`）。
* 專案檔的 `RootNamespace` （`<RootNamespace>BlazorSample</RootNamespace>`）。
* 從專案檔的檔案名（ *.csproj*）取得的專案名稱，以及從專案根目錄到元件的路徑。 例如，架構會將 *{PROJECT ROOT}/Pages/Index.razor* （*BlazorSample*）解析為 `BlazorSample.Pages`的命名空間。 元件會C#遵循名稱系結規則。 針對此範例中的 `Index` 元件，範圍內的元件都是元件：
  * 在相同的資料夾中，*頁面*。
  * 專案根目錄中未明確指定不同命名空間的元件。

使用 Razor 的[`@using`](xref:mvc/views/razor#using)指示詞，在不同的命名空間中定義的元件會帶入範圍中。

如果*BlazorSample/Shared/* 資料夾中有另一個元件（`NavMenu.razor`）存在，則元件可用於 `Index.razor`，並具有下列 `@using` 語句：

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

元件也可以使用其完整名稱來參考，這不需要[`@using`](xref:mvc/views/razor#using)指示詞：

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> 不支援 `global::` 限定性。
>
> 不支援匯入具有別名 `using` 語句的元件（例如，`@using Foo = Bar`）。
>
> 不支援部分限定的名稱。 例如，不支援使用 `<Shared.NavMenu></Shared.NavMenu>` 來新增 `@using BlazorSample` 和參考 `NavMenu.razor`。

## <a name="conditional-html-element-attributes"></a>條件式 HTML 元素屬性

HTML 專案屬性會根據 .NET 值有條件地呈現。 如果 `false` 或 `null`值，則不會呈現屬性。 如果 `true`值，則會將屬性轉譯為最小化。

在下列範例中，`IsCompleted` 會決定是否要在元素的標記中轉譯 `checked`：

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

如果 `true``IsCompleted`，則會將核取方塊轉譯為：

```html
<input type="checkbox" checked />
```

如果 `false``IsCompleted`，則會將核取方塊轉譯為：

```html
<input type="checkbox" />
```

如需詳細資訊，請參閱 <xref:mvc/views/razor>。

> [!WARNING]
> 當 .NET 類型為 `bool`時，某些 HTML 屬性（例如，由[aria 按下](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)）無法正常運作。 在這些情況下，請使用 `string` 類型，而不是 `bool`。

## <a name="raw-html"></a>原始 HTML

字串通常會使用 DOM 文位元組點來呈現，這表示它們可能包含的任何標記都會被忽略，並視為常值。 若要呈現原始 HTML，請將 HTML 內容包裝 `MarkupString` 值。 此值會剖析為 HTML 或 SVG，並插入 DOM 中。

> [!WARNING]
> 轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**，應予以避免！

下列範例顯示如何使用 `MarkupString` 類型，將靜態 HTML 內容的區塊新增至元件的轉譯輸出：

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a>級聯的值和參數

在某些情況下，使用[元件參數](#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。 串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。 級聯的值和參數也會提供一種方法來協調元件。

### <a name="theme-example"></a>主題範例

在範例應用程式的下列範例中，`ThemeInfo` 類別會指定要向下流動元件階層的主題資訊，讓應用程式中指定部分內的所有按鈕共用相同的樣式。

*UIThemeClasses/ThemeInfo .cs*：

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

祖系元件可以使用串聯值元件來提供串聯值。 `CascadingValue` 元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。

例如，範例應用程式會在其中一個應用程式的配置中，將主題資訊（`ThemeInfo`）指定為構成 `@Body` 屬性之配置主體的所有元件的串聯參數。 `ButtonClass` 會在版面配置元件中指派 `btn-success` 的值。 任何子代元件都可以透過 `ThemeInfo` 級聯的物件來取用這個屬性。

`CascadingValuesParametersLayout` 元件：

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

為了利用串聯值，元件會使用 `[CascadingParameter]` 屬性宣告串聯式參數。 串聯式值會依類型系結至串聯式參數。

在範例應用程式中，`CascadingValuesParametersTheme` 元件會將 `ThemeInfo` 級聯的值系結至串聯式參數。 參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。

`CascadingValuesParametersTheme` 元件：

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

若要在相同的子樹中串聯多個相同類型的值，請為每個 `CascadingValue` 元件和其對應的 `CascadingParameter`提供唯一的 `Name` 字串。 在下列範例中，兩個 `CascadingValue` 元件會依名稱將不同的實例 `MyCascadingType`：

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

在子系元件中，串聯的參數會以名稱從上階元件中對應的串聯值接收其值：

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>TabSet 範例

串聯式參數也可以讓元件在元件階層之間共同作業。 例如，請考慮範例應用程式中的下列*TabSet*範例。

範例應用程式有一個 `ITab` 的介面，可執行索引標籤：

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

`CascadingValuesParametersTabSet` 元件會使用 `TabSet` 元件，其中包含數個 `Tab` 元件：

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

子 `Tab` 元件不會明確地當做參數傳遞至 `TabSet`。 相反地，子 `Tab` 元件是 `TabSet`之子內容的一部分。 不過，`TabSet` 還是必須知道每個 `Tab` 元件，才能轉譯標頭和使用中的索引標籤。若要在不需要額外程式碼的情況下啟用這項協調，`TabSet` 元件*可以將其本身提供為*串聯的值，然後由子 `Tab` 元件挑選。

`TabSet` 元件：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

子系 `Tab` 元件會將包含的 `TabSet` 作為串聯參數來捕捉，因此 `Tab` 元件會將自己加入至 `TabSet`，並對作用中的索引標籤進行協調。

`Tab` 元件：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Razor 範本

轉譯片段可以使用 Razor 範本語法來定義。 Razor 範本是定義 UI 程式碼片段並採用下列格式的方式：

```razor
@<{HTML tag}>...</{HTML tag}>
```

下列範例說明如何指定 `RenderFragment` 和 `RenderFragment<T>` 值，並直接在元件中呈現範本。 轉譯片段也可以當做引數傳遞至樣板[化元件](xref:blazor/templated-components)。

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

上述程式碼的轉譯輸出：

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a>可擴充向量圖形（SVG）影像

由於 Blazor 會轉譯 HTML，因此瀏覽器支援的影像（包括可擴充的向量圖形（SVG）影像（*svg*））可透過 `<img>` 標記來支援：

```html
<img alt="Example image" src="some-image.svg" />
```

同樣地，樣式表單檔案（ *.css*）的 CSS 規則也支援 SVG 影像：

```css
.my-element {
    background-image: url("some-image.svg");
}
```

不過，在所有案例中不支援內嵌 SVG 標記。 如果您將 `<svg>` 標籤直接放入元件檔（*razor*）中，則會支援基本映射轉譯，但尚不支援許多先進的案例。 例如，目前未遵守 `<use>` 標籤，且 `@bind` 無法與某些 SVG 標記搭配使用。 我們希望在未來的版本中解決這些限制。

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/server> &ndash; 包含有關建立 Blazor 伺服器應用程式的指導方針，必須與資源耗盡爭用。
