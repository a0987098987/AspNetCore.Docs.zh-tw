---
title: 建立和使用 ASP.NET Core Razor 元件
author: guardrex
description: 瞭解如何建立和使用 Razor 元件，包括如何系結至資料、處理事件，以及管理元件生命週期。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components
ms.openlocfilehash: 2a6de1a39737f98cb151a0556f36c223d86f9752
ms.sourcegitcommit: d243fadeda20ad4f142ea60301ae5f5e0d41ed60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723947"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>建立和使用 ASP.NET Core Razor 元件

By [Luke Latham](https://github.com/guardrex)、 [Daniel Roth](https://github.com/danroth27)和[Tobias Bartsch](https://www.aveo-solutions.com/)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

Blazor應用程式是使用*元件*所建立。 「元件」（component）是一種獨立的使用者介面（UI）區塊，例如頁面、對話方塊或表單。 元件包含 HTML 標籤，以及插入資料或回應 UI 事件所需的處理邏輯。 元件具有彈性且輕量。 它們可以在專案之間進行嵌套、重複使用及共用。

## <a name="component-classes"></a>元件類別

元件會 [Razor](xref:mvc/views/razor) 使用 c # 和 HTML 標籤的組合，在元件檔（*razor*）中執行。 中的元件 Blazor 正式地稱為「元件」（ * Razor component*）。

元件的名稱必須以大寫字元開頭。 例如， *MyCoolComponent*有效，而*MyCoolComponent*則無效。

元件的 UI 是使用 HTML 定義的。 動態轉譯邏輯（例如，迴圈、條件、運算式）是使用名為的內嵌 c # 語法加入 *Razor* 。 編譯應用程式時，會將 HTML 標籤和 c # 轉譯邏輯轉換成元件類別。 產生的類別名稱與檔案的名稱相符。

元件類別的成員定義于 [`@code`][1] 區塊中。 在 [`@code`][1] 區塊中，會使用事件處理或定義其他元件邏輯的方法來指定元件狀態（屬性、欄位）。 允許一個以上的 [`@code`][1] 區塊。

元件成員可以使用開頭為的 c # 運算式，做為元件轉譯邏輯的一部分 `@` 。 例如，c # 欄位是藉由在功能變數名稱前面加上的方式 `@` 來呈現。 下列範例會評估並呈現：

* `headingFontStyle`至的 CSS 屬性值 `font-style` 。
* `headingText`至元素的內容 `<h1>` 。

```razor
<h1 style="font-style:@headingFontStyle">@headingText</h1>

@code {
    private string headingFontStyle = "italic";
    private string headingText = "Put on your new Blazor!";
}
```

一開始呈現元件之後，元件會重新產生其轉譯樹狀結構，以回應事件。 Blazor然後比較新的轉譯樹狀結構與上一個，並將任何修改套用至瀏覽器的檔物件模型（DOM）。

元件是一般的 c # 類別，可以放在專案內的任何位置。 產生網頁的元件通常會位於*Pages*資料夾中。 非頁面元件通常會放在*共用*資料夾中，或加入至專案的自訂資料夾中。

一般而言，元件的命名空間是從應用程式的根命名空間和元件在應用程式內的位置（資料夾）衍生而來。 如果應用程式的根命名空間為 `BlazorApp` ，且 `Counter` 元件位於*Pages*資料夾中：

* `Counter`元件的命名空間是 `BlazorApp.Pages` 。
* 元件的完整類型名稱為 `BlazorApp.Pages.Counter` 。

若為保存元件的自訂資料夾，請將 `using` 語句新增至父元件或應用程式的 *_Imports razor*檔案。 下列範例會讓 [*元件*] 資料夾中的元件可供使用：

```razor
@using BlazorApp.Components
```

或者，您也可以直接參考元件：

```razor
<BlazorApp.Components.MyCoolComponent />
```

如需詳細資訊，請參閱匯[入元件](#import-components)一節。

## <a name="razor-syntax"></a>Razor 語法

Razor應用程式中的元件會 Blazor 廣泛使用 Razor 語法。 如果您不熟悉 Razor 標記語言，建議您先閱讀， <xref:mvc/views/razor> 再繼續進行。

存取語法上的內容時 Razor ，請特別注意下列各節：

* [Directives](xref:mvc/views/razor#directives)指示 `@` 詞：-前面加上的保留關鍵字，通常會變更元件標記的剖析或運作方式。
* 指示詞[屬性](xref:mvc/views/razor#directive-attributes)： `@` -前面加上的保留關鍵字，通常會變更元件元素的剖析或運作方式。

## <a name="static-assets"></a>靜態資產

Blazor遵循 ASP.NET Core 應用程式在專案的[web 根目錄（wwwroot）資料夾](xref:fundamentals/index#web-root)下放置靜態資產的慣例。

使用基底相對路徑（ `/` ）來參考靜態資產的 web 根目錄。 在下列範例中， *logo.png*實際上位於 *{PROJECT ROOT}/wwwroot/images*資料夾中：

```razor
<img alt="Company logo" src="/images/logo.png" />
```

Razor元件**不**支援波形符-斜線標記法（ `~/` ）。

如需設定應用程式基底路徑的詳細資訊，請參閱 <xref:host-and-deploy/blazor/index#app-base-path> 。

## <a name="tag-helpers-arent-supported-in-components"></a>元件中不支援標記協助程式

[Tag Helpers](xref:mvc/views/tag-helpers/intro) Razor 元件（*razor*檔案）中不支援標記協助程式。 若要在中提供標籤協助程式的功能 Blazor ，請建立元件，其功能與標記協助程式相同，並改用元件。

## <a name="use-components"></a>使用元件

元件可以包含其他元件，方法是使用 HTML 專案語法來宣告它們。 使用元件的標記看起來像是 HTML 標籤，其中標籤名稱是元件類型。

在*Index*中的下列標記會呈現 `HeadingComponent` 實例：

```razor
<HeadingComponent />
```

*Components/HeadingComponent. razor*：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

如果元件包含的 HTML 專案具有大寫的第一個字母，但不符合元件名稱，則會發出警告，指出該元素有未預期的名稱。 [`@using`][2]為元件的命名空間加入指示詞可讓元件可用，這會解析警告。

## <a name="routing"></a>路由

中的路由 Blazor 會藉由提供路由範本給應用程式中每個可存取的元件來達成。

當編譯具有指示詞的檔案時 Razor [`@page`][9] ，系統會指定路由範本給產生的類別 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> 。 在執行時間，路由器會尋找具有的元件類別 <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> ，並轉譯哪個元件具有符合所要求 URL 的路由範本。

```razor
@page "/ParentComponent"

...
```

如需詳細資訊，請參閱 <xref:blazor/routing> 。

## <a name="parameters"></a>參數

### <a name="route-parameters"></a>路由參數

元件可以從指示詞中提供的路由範本接收路由參數 [`@page`][9] 。 路由器會使用路由參數來填入對應的元件參數。

*Pages/RouteParameter. razor*：

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

不支援選擇性參數，因此 [`@page`][9] 在上述範例中會套用兩個指示詞。 第一個則允許不使用參數導覽至元件。 第二個指示詞會 [`@page`][9] 接收 `{text}` 路由參數，並將值指派給 `Text` 屬性。

*Catch-all*參數語法（ `*` / `**` ），它會跨多個資料夾界限來捕捉路徑**not** ，但 Razor 元件（*razor*）並不支援。

### <a name="component-parameters"></a>元件參數

元件可以具有*元件參數*，其定義方式是在元件類別上使用具有屬性的公用屬性 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 。 使用這些屬性來指定標記中元件的引數。

*Components/ChildComponent. razor*：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

在範例應用程式的下列範例中，會 `ParentComponent` 設定的 `Title` 屬性值 `ChildComponent` 。

*Pages/ParentComponent. razor*：

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

> [!WARNING]
> 請勿建立可寫入其本身*元件參數*的元件，請改用私用欄位。 如需詳細資訊，請參閱[不要建立可寫入自己的參數屬性的元件](#dont-create-components-that-write-to-their-own-parameter-properties)一節。

## <a name="child-content"></a>子內容

元件可以設定另一個元件的內容。 指派元件會在指定接收元件的標記之間提供內容。

在下列範例中， `ChildComponent` 有一個 `ChildContent` 代表的屬性 <xref:Microsoft.AspNetCore.Components.RenderFragment> ，代表要呈現的 UI 區段。 的值位於 `ChildContent` 元件的標記中，應在其中呈現內容。 的值 `ChildContent` 會從父元件接收，並在啟動載入面板的內轉譯 `panel-body` 。

*Components/ChildComponent. razor*：

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> 接收內容的屬性 <xref:Microsoft.AspNetCore.Components.RenderFragment> 必須依照慣例命名 `ChildContent` 。

`ParentComponent`範例應用程式中的會將內容放在標籤內，藉以提供呈現的內容 `ChildComponent` `<ChildComponent>` 。

*Pages/ParentComponent. razor*：

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>屬性展開和任意參數

除了元件的宣告參數之外，元件還可以捕捉和轉譯其他屬性。 您可以在字典中捕捉其他屬性，然後在使用指示詞轉譯元件時， *splatted*至元素 [`@attributes`][3] Razor 。 當定義的元件會產生支援各種自訂的標記專案時，這個案例就很有用。 例如，針對支援許多參數的，分別定義屬性可能會很繁瑣 `<input>` 。

在下列範例中，第一個 `<input>` 元素（ `id="useIndividualParams"` ）會使用個別的元件參數，而第二個 `<input>` 元素（ `id="useAttributesDict"` ）則使用屬性展開：

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

參數的類型必須 `IEnumerable<KeyValuePair<string, object>>` 使用字串索引鍵來執行。 `IReadOnlyDictionary<string, object>`在此案例中，使用也是一個選項。

`<input>`使用這兩種方法的轉譯元素都相同：

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

若要接受任意屬性，請使用屬性設定為的屬性來定義元件參數 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> `true` ：

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues>上的屬性 [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) 允許參數比對與任何其他參數不相符的所有屬性。 元件只能定義具有的單一參數 <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> 。 搭配使用的屬性類型 <xref:Microsoft.AspNetCore.Components.ParameterAttribute.CaptureUnmatchedValues> 必須可從 `Dictionary<string, object>` 使用字串索引鍵來指派。 `IEnumerable<KeyValuePair<string, object>>`或 `IReadOnlyDictionary<string, object>` 也是此案例中的選項。

[`@attributes`][3]相對於元素屬性位置的位置很重要。 當在專案 [`@attributes`][3] 上 splatted 時，會從右至左（最後一個）處理屬性。 請考慮使用元件的下列元件範例 `Child` ：

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

`Child`元件的 `extra` 屬性會設定為的右邊 [`@attributes`][3] 。 `Parent` `<div>` `extra="5"` 當您透過其他屬性傳遞時，元件的會包含，因為屬性是由右至左（最後一個）來處理：

```html
<div extra="5" />
```

在下列範例中，和的順序 `extra` [`@attributes`][3] 會在 `Child` 元件的中反轉 `<div>` ：

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

在元件中轉譯的會在 `<div>` `Parent` `extra="10"` 透過其他屬性傳遞時包含：

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a>捕獲元件的參考

元件參考提供參考元件實例的方法，讓您可以對該實例發出命令，例如 `Show` 或 `Reset` 。 若要捕捉元件參考：

* 將 [`@ref`][4] 屬性加入至子元件。
* 定義與子元件類型相同的欄位。

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

當元件呈現時， `loginDialog` 欄位會填入 `MyLoginDialog` 子元件實例。 接著，您可以在元件實例上叫用 .NET 方法。

> [!IMPORTANT]
> 只有在轉譯 `loginDialog` 元件之後才會填入變數，而且其輸出會包含 `MyLoginDialog` 元素。 直到該點為止，沒有任何可參考的內容。 若要在元件完成呈現之後操作元件參考，請使用[OnAfterRenderAsync 或 OnAfterRender 方法](xref:blazor/lifecycle#after-component-render)。

若要參考迴圈中的元件，請參閱[捕捉多個類似子元件的參考（dotnet/aspnetcore #13358）](https://github.com/dotnet/aspnetcore/issues/13358)。

雖然捕捉元件參考使用類似的語法來[捕捉元素參考](xref:blazor/call-javascript-from-dotnet#capture-references-to-elements)，但它並不是 JavaScript interop 功能。 元件參考不會傳遞至 JavaScript 程式碼。 元件參考只在 .NET 程式碼中使用。

> [!NOTE]
> 請勿**使用元件**參考來改變子元件的狀態。 請改用一般宣告式參數，將資料傳遞至子元件。 使用一般宣告式參數會導致子元件自動 rerender 正確的時間。

## <a name="invoke-component-methods-externally-to-update-state"></a>在外部叫用元件方法來更新狀態

Blazor會使用同步處理內容（ <xref:System.Threading.SynchronizationContext> ）來強制執行單一邏輯執行緒。 元件的[生命週期方法](xref:blazor/lifecycle)和所引發的任何事件回呼 Blazor 都會在同步處理內容上執行。

Blazor伺服器的同步處理內容會嘗試模擬單一執行緒環境，讓它與瀏覽器中的 WebAssembly 模型（單一執行緒）緊密相符。 在任何指定的時間點，只會在一個執行緒上執行工作，以提供單一邏輯執行緒的印象。 不會同時執行兩個作業。

在事件中，必須根據外來事件（例如計時器或其他通知）來更新元件，請使用 `InvokeAsync` 方法，這會分派至 Blazor 的同步處理內容。 例如，假設有一個通知程式*服務*可通知任何處于已更新狀態的「接聽」元件：

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

將註冊 `NotifierService` 為 singletion：

* 在 Blazor WebAssembly 中，于中註冊服務 `Program.Main` ：

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* 在 [伺服器] 中 Blazor ，于註冊服務 `Startup.ConfigureServices` ：

  ```csharp
  services.AddScoped<NotifierService>();
  ```

使用 `NotifierService` 來更新元件：

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

在上述範例中，會在 `NotifierService` 的同步處理內容之外叫用元件的 `OnNotify` 方法 Blazor 。 `InvokeAsync`用來切換至正確的內容，並將轉譯排入佇列。

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>使用 \@ 金鑰來控制元素和元件的保留

當轉譯專案或元件清單，以及後續變更的專案或元件時， Blazor 的比較演算法必須決定哪些先前的專案或元件可以保留，以及模型物件應如何對應至這些專案。 一般來說，此程式是自動的，可以忽略，但在某些情況下，您可能會想要控制進程。

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

集合的內容 `People` 可能會隨著插入、刪除或重新排序的專案而變更。 當元件 rerenders 時， `<DetailsEditor>` 元件可能會變更以接收不同的 `Details` 參數值。 這可能會導致比預期更複雜的 rerendering。 在某些情況下，rerendering 可能會導致可見的行為差異，例如失去元素的焦點。

您可以使用指示詞屬性來控制對應進程 [`@key`][5] 。 [`@key`][5]導致比較演算法根據索引鍵的值，保證保留元素或元件：

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

當 `People` 集合變更時，比較演算法會保留 `<DetailsEditor>` 實例和實例之間的關聯 `person` ：

* 如果 `Person` 從清單中刪除 `People` ，則只 `<DetailsEditor>` 會從 UI 移除對應的實例。 其他實例則保持不變。
* 如果在 `Person` 清單中的某個位置插入，則會在 `<DetailsEditor>` 對應的位置插入一個新的實例。 其他實例則保持不變。
* 如果 `Person` 重新排序專案，則 `<DetailsEditor>` 會保留對應的實例，並在 UI 中重新排序。

在某些情況下，使用可將 [`@key`][5] rerendering 的複雜性降到最低，並避免 DOM 的具狀態部分可能發生的問題，例如焦點位置。

> [!IMPORTANT]
> 索引鍵在每個容器元素或元件的本機。 金鑰不會在檔之間進行全域比較。

### <a name="when-to-use-key"></a>使用金鑰的時機 \@

一般而言， [`@key`][5] 只要轉譯清單（例如，在[foreach](/dotnet/csharp/language-reference/keywords/foreach-in)區塊中），而且有適合的值來定義時，就有合理的使用方式 [`@key`][5] 。

當物件變更時，您也可以使用 [`@key`][5] 來防止 Blazor 保留元素或元件子樹：

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

如果 `@currentPerson` 變更，attribute 指示詞會 [`@key`][5] 強制 Blazor 捨棄整個及其下階， `<div>` 並使用新的元素和元件重建 UI 內的子樹。 如果您需要保證變更時不會保留任何 UI 狀態，這會很有用 `@currentPerson` 。

### <a name="when-not-to-use-key"></a>不使用金鑰的時機 \@

與比較時，會產生效能成本 [`@key`][5] 。 效能成本並不大，但只會指定 [`@key`][5] 控制元素或元件保留規則是否能讓應用程式受益。

即使 [`@key`][5] 未使用，也會盡可能 Blazor 保留子項目和元件實例。 使用的唯一優點 [`@key`][5] 是控制模型實例*如何*對應至保留的元件實例，而不是用來選取對應的比較演算法。

### <a name="what-values-to-use-for-key"></a>要用於金鑰的值 \@

一般來說，提供下列其中一種類型的值是合理的 [`@key`][5] ：

* 模型物件實例（例如， `Person` 如先前範例所示的實例）。 這可確保根據物件參考的相等性進行保留。
* 唯一識別碼（例如，、或類型的主要索引 `int` 鍵值 `string` `Guid` ）。

請確定用於的值 [`@key`][5] 不會造成衝突。 如果在相同的父元素中偵測到衝突值，則會擲回例外狀況， Blazor 因為它無法以決定性的方式將舊專案或元件對應到新的專案或元件。 只使用不同的值，例如物件實例或主鍵值。

## <a name="dont-create-components-that-write-to-their-own-parameter-properties"></a>不要建立會寫入自己的參數屬性的元件

在下列情況下，會覆寫參數：

* 子元件的內容會以呈現 <xref:Microsoft.AspNetCore.Components.RenderFragment> 。
* <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>在父元件中呼叫。

參數會重設，因為呼叫時會 rerenders 父元件 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> ，並將新的參數值提供給子元件。

請考慮下列 `Expander` 元件：

* 呈現子內容。
* 使用元件參數來顯示子內容的切換。

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @Expanded)

    @if (Expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void Toggle()
    {
        Expanded = !Expanded;
    }
}
```

`Expander`元件會新增至可能呼叫的父元件 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> ：

```razor
<Expander Expanded="true">
    <h1>Hello, world!</h1>
</Expander>

<Expander Expanded="true" />

<button @onclick="@(() => StateHasChanged())">
    Call StateHasChanged
</button>
```

一開始， `Expander` 元件會在其屬性切換時獨立行為 `Expanded` 。 子元件會如預期般維護其狀態。 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>在父系中呼叫時， `Expanded` 第一個子元件的參數會重設回其初始值（ `true` ）。 第二個 `Expander` 元件的 `Expanded` 值不會重設，因為第二個元件中不會轉譯任何子內容。

若要維護上述案例中的狀態，請使用元件中的*私用欄位* `Expander` 來維護其切換狀態。

下列 `Expander` 元件：

* 接受 `Expanded` 來自父系的元件參數值。
* 將元件參數值指派給 OnInitialized 事件中的私用*欄位*（ `expanded` ）。 [OnInitialized event](xref:blazor/lifecycle#component-initialization-methods)
* 會使用私用欄位來維護其內部切換狀態。

```razor
<div @onclick="@Toggle">
    Toggle (Expanded = @expanded)

    @if (expanded)
    {
        @ChildContent
    }
</div>

@code {
    [Parameter]
    public bool Expanded { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private bool expanded;

    protected override void OnInitialized()
    {
        expanded = Expanded;
    }

    private void Toggle()
    {
        expanded = !expanded;
    }
}
```

## <a name="partial-class-support"></a>部分類別支援

Razor元件是以部分類別的形式產生。 Razor元件是使用下列其中一種方法來撰寫的：

* C # 程式碼定義于 [`@code`][1] 區塊中，並在單一檔案中使用 HTML 標籤和程式 Razor 代碼。 Blazor範本會 Razor 使用這種方法來定義其元件。
* C # 程式碼會放在定義為部分類別的程式碼後置檔案中。

下列範例顯示在 `Counter` [`@code`][1] 從範本產生的應用程式中具有區塊的預設元件 Blazor 。 HTML 標籤、程式 Razor 代碼和 c # 程式碼位於相同的檔案中：

*Counter. razor*：

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

您 `Counter` 也可以使用具有部分類別的程式碼後置檔案來建立元件：

*Counter. razor*：

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.razor.cs*：

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

視需要將任何必要的命名空間新增至部分類別檔案。 元件所使用的一般命名空間 Razor 包括：

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>指定基類

指示詞 [`@inherits`][6] 可以用來指定元件的基類。 下列範例會顯示元件如何繼承基類， `BlazorRocksBase` 以提供元件的屬性和方法。 基類應該衍生自 <xref:Microsoft.AspNetCore.Components.ComponentBase> 。

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

屬性可以在具有指示詞的元件中指定 Razor [`@attribute`][7] 。 下列範例會將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至元件類別：

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>匯入元件

以撰寫之元件的命名空間 Razor 是根據（依優先順序排列）：

* [`@namespace`][8]檔案 Razor （*razor*）標記中的指定（ `@namespace BlazorSample.MyNamespace` ）。
* 專案 `RootNamespace` 在專案檔中的（ `<RootNamespace>BlazorSample</RootNamespace>` ）。
* 從專案檔的檔案名（*.csproj*）取得的專案名稱，以及從專案根目錄到元件的路徑。 例如，架構會將 *{PROJECT ROOT}/Pages/Index.razor* （*BlazorSample*）解析為命名空間 `BlazorSample.Pages` 。 元件遵循 c # 名稱系結規則。 針對 `Index` 此範例中的元件，範圍內的元件都是元件：
  * 在相同的資料夾中，*頁面*。
  * 專案根目錄中未明確指定不同命名空間的元件。

使用的指示詞，在不同的命名空間中定義的元件會帶入範圍中 Razor [`@using`][2] 。

如果 `NavMenu.razor` *BlazorSample/Shared/* 資料夾中有另一個元件，則可以在中使用此元件， `Index.razor` 並搭配下列 [`@using`][2] 語句：

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

元件也可以使用其完整名稱來參考，而不需要指示詞 [`@using`][2] ：

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> `global::`不支援該限定性。
>
> 不支援匯入具有別名[using](/dotnet/csharp/language-reference/keywords/using-statement)語句的元件（例如 `@using Foo = Bar` ）。
>
> 不支援部分限定的名稱。 例如， `@using BlazorSample` 不支援新增和參考 `NavMenu` 元件（ `NavMenu.razor` ） `<Shared.NavMenu></Shared.NavMenu>` 。

## <a name="conditional-html-element-attributes"></a>條件式 HTML 元素屬性

HTML 專案屬性會根據 .NET 值有條件地呈現。 如果值為 `false` 或 `null` ，則不會呈現屬性。 如果值為 `true` ，則會以最小化的方式呈現屬性。

在下列範例中， `IsCompleted` 會判斷 `checked` 是否呈現在專案的標記中：

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

如果 `IsCompleted` 為 `true` ，則會將核取方塊轉譯為：

```html
<input type="checkbox" checked />
```

如果 `IsCompleted` 為 `false` ，則會將核取方塊轉譯為：

```html
<input type="checkbox" />
```

如需詳細資訊，請參閱 <xref:mvc/views/razor> 。

> [!WARNING]
> 當 .NET 類型為時，某些 HTML 屬性（例如，[按下的 aria](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons)）無法正常運作 `bool` 。 在這些情況下，請使用型別， `string` 而不是 `bool` 。

## <a name="raw-html"></a>原始 HTML

字串通常會使用 DOM 文位元組點來呈現，這表示它們可能包含的任何標記都會被忽略，並視為常值。 若要轉譯原始 HTML，請將 HTML 內容包裝在 `MarkupString` 值中。 此值會剖析為 HTML 或 SVG，並插入 DOM 中。

> [!WARNING]
> 轉譯從任何未受信任來源所建立的原始 HTML 會有**安全性風險**，應予以避免！

下列範例顯示如何使用 `MarkupString` 類型，將靜態 HTML 內容的區塊新增至元件的轉譯輸出：

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a>級聯的值和參數

在某些情況下，使用[元件參數](#component-parameters)將資料從上階元件傳送到子元件是不方便的，特別是在有數個元件層時。 串聯的值和參數可讓上階元件提供一個值給其所有子系元件，藉此解決這個問題。 級聯的值和參數也會提供一種方法來協調元件。

### <a name="theme-example"></a>主題範例

在範例應用程式的下列範例中， `ThemeInfo` 類別會指定主題資訊以向下流動元件階層，讓應用程式中指定部分內的所有按鈕共用相同的樣式。

*UIThemeClasses/ThemeInfo .cs*：

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

祖系元件可以使用串聯值元件來提供串聯值。 此 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件會包裝元件階層的子樹，並提供單一值給該子樹內的所有元件。

例如，範例應用程式 `ThemeInfo` 會在其中一個應用程式的配置中，將主題資訊（）指定為構成屬性版面配置主體之所有元件的串聯參數 `@Body` 。 `ButtonClass`在版面配置元件中，會指派的值 `btn-success` 。 任何子代元件都可以透過串聯物件使用此屬性 `ThemeInfo` 。

`CascadingValuesParametersLayout`成分

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

為了利用串聯值，元件會使用屬性宣告串聯式參數 [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 。 串聯式值會依類型系結至串聯式參數。

在範例應用程式中，元件會將串聯值系結至串聯式 `CascadingValuesParametersTheme` `ThemeInfo` 參數。 參數是用來為元件所顯示的其中一個按鈕設定 CSS 類別。

`CascadingValuesParametersTheme`成分

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

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
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

若要在相同的子樹中串聯多個相同類型的值，請為 <xref:Microsoft.AspNetCore.Components.CascadingValue%601.Name%2A> 每個 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件和其對應的屬性提供唯一的字串 [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 。 在下列範例中，兩個 <xref:Microsoft.AspNetCore.Components.CascadingValue%601> 元件依名稱串聯不同的實例 `MyCascadingType` ：

```razor
<CascadingValue Value=@parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType parentCascadeParameter1;

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

範例應用程式具有可 `ITab` 執行 tab 鍵的介面：

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

`CascadingValuesParametersTabSet`元件會使用 `TabSet` 元件，其中包含數個 `Tab` 元件：

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

子 `Tab` 元件不會明確地當做參數傳遞至 `TabSet` 。 相反地，子 `Tab` 元件是的子內容的一部分 `TabSet` 。 不過， `TabSet` 仍然需要知道每個元件， `Tab` 使其可以呈現標頭和使用中的索引標籤。若要啟用這項協調而不需要額外的程式碼， `TabSet` 元件*可以提供本身作為*串聯的值，然後由子代 `Tab` 元件挑選。

`TabSet`成分

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

子系 `Tab` 元件會以串聯式參數的形式捕捉包含的 `TabSet` ，因此 `Tab` 元件會將自己加入至索引標籤作用 `TabSet` 中的和座標。

`Tab`成分

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Razor範本

您可以使用範本語法來定義轉譯片段 Razor 。 Razor範本是定義 UI 程式碼片段並採用下列格式的方式：

```razor
@<{HTML tag}>...</{HTML tag}>
```

下列範例說明如何指定 <xref:Microsoft.AspNetCore.Components.RenderFragment> 和 <xref:Microsoft.AspNetCore.Components.RenderFragment%601> 值，並直接在元件中呈現範本。 轉譯片段也可以當做引數傳遞至樣板[化元件](xref:blazor/templated-components)。

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

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

由於 Blazor 會轉譯 HTML，瀏覽器支援的影像（包括可擴充的向量圖形（svg）影像（*svg*））可透過 `<img>` 標記來支援：

```html
<img alt="Example image" src="some-image.svg" />
```

同樣地，樣式表單檔案（*.css*）的 CSS 規則也支援 SVG 影像：

```css
.my-element {
    background-image: url("some-image.svg");
}
```

不過，在所有案例中不支援內嵌 SVG 標記。 如果您將 `<svg>` 標記直接放入元件檔案（*razor*），則會支援基本映射轉譯，但尚不支援許多先進的案例。 例如， `<use>` 目前未遵守標記，而且 [`@bind`][10] 無法與某些 SVG 標記搭配使用。 如需詳細資訊，請參閱[中的 SVG 支援 Blazor （dotnet/aspnetcore #18271）](https://github.com/dotnet/aspnetcore/issues/18271)。

## <a name="additional-resources"></a>其他資源

* <xref:security/blazor/server/threat-mitigation>：包含有關建立 Blazor 伺服器應用程式的指導方針，這些服務必須與資源耗盡有關。

<!--Reference links in article-->
[1]: <xref:mvc/views/razor#code>
[2]: <xref:mvc/views/razor#using>
[3]: <xref:mvc/views/razor#attributes>
[4]: <xref:mvc/views/razor#ref>
[5]: <xref:mvc/views/razor#key>
[6]: <xref:mvc/views/razor#inherits>
[7]: <xref:mvc/views/razor#attribute>
[8]: <xref:mvc/views/razor#namespace>
[9]: <xref:mvc/views/razor#page>
[10]: <xref:mvc/views/razor#bind>
