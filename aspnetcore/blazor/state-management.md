---
title: ASP.NET Core Blazor 狀態管理
author: guardrex
description: 瞭解如何在 Blazor 伺服器應用程式中保存狀態。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/state-management
ms.openlocfilehash: ffb32a4f274a30f2a5ceed9cbf193285e85bab4c
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160141"
---
# <a name="aspnet-core-opno-locblazor-state-management"></a>ASP.NET Core Blazor 狀態管理

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor Server 是具狀態的應用程式架構。 在大部分的情況下，應用程式會維護與伺服器之間的持續連接。 使用者的狀態會保留在伺服器的記憶體中。 

保留給使用者線路的狀態範例包括：

* 呈現的 UI&mdash;元件實例的階層及其最新的轉譯輸出。
* 元件實例中任何欄位和屬性的值。
* 保留在範圍為線路之相依性[插入（DI）](xref:fundamentals/dependency-injection)服務實例中的資料。

> [!NOTE]
> 本文解決 Blazor Server 應用程式中的狀態持續性。 Blazor WebAssembly apps 可以利用[瀏覽器中的用戶端狀態持續](#client-side-in-the-browser)性，但需要自訂解決方案或協力廠商套件，超出本文的範圍。

## <a name="opno-locblazor-circuits"></a>Blazor 線路

如果使用者遇到暫時性的網路連線遺失，Blazor 會嘗試將使用者重新連接到其原始線路，讓他們可以繼續使用應用程式。 不過，不一定能夠將使用者重新連接到伺服器記憶體中的原始線路：

* 伺服器無法永久保留中斷連線的線路。 伺服器必須在超時之後或伺服器處於記憶體不足的壓力時，釋放中斷連線的線路。
* 在多伺服器、負載平衡的部署環境中，任何指定的時間都可能無法使用任何伺服器處理要求。 當不再需要處理要求的整體量時，個別伺服器可能會失敗或自動移除。 當使用者嘗試重新連線時，源伺服器可能無法使用。
* 使用者可能會關閉並重新開啟其瀏覽器或重載頁面，這會移除瀏覽器記憶體中保留的任何狀態。 例如，透過 JavaScript interop 呼叫所設定的值會遺失。

當使用者無法重新連線到其原始線路時，使用者會收到空白狀態的新線路。 這相當於關閉和重新開啟桌面應用程式。

## <a name="preserve-state-across-circuits"></a>跨線路保留狀態

在某些情況下，您需要保留各線路的狀態。 應用程式可以保留使用者的重要資料，如果：

* 網頁伺服器變得無法使用。
* 使用者的瀏覽器會強制使用新的 web 伺服器來啟動新的線路。

一般而言，跨線路維護狀態適用于使用者主動建立資料的情況，而不只是讀取已經存在的資料。

若要保留超過單一線路的狀態，*請勿只將資料儲存在伺服器的記憶體中*。 應用程式必須將資料保存到其他儲存位置。 狀態持續性不會自動&mdash;您在開發應用程式以執行具狀態的資料持續性時，必須採取步驟。

通常只有在高價值的狀態下，使用者才需要建立資料持續性。 在下列範例中，保存狀態可以節省商務工作的時間或輔助：

* 多步驟 webform &ndash; 當使用者的狀態遺失時，針對多步驟程式的數個已完成步驟重新輸入資料是非常耗時的。 如果使用者離開多步驟表單，並在稍後返回表單，則會在此案例中失去狀態。
* 購物車 &ndash; 應用程式的任何商業重要元件，其代表可能的收益。 失去其狀態的使用者，因此其購物車，可能會在日後返回網站時購買較少的產品或服務。

通常不需要保留容易重新建立的狀態，例如輸入登入對話方塊中未提交的使用者名稱。

> [!IMPORTANT]
> 應用程式只能保存*應用程式狀態*。 Ui 無法保存，例如元件實例和其呈現樹狀結構。 元件和轉譯樹狀結構通常不是可序列化的。 若要保存類似于 UI 狀態的內容（例如 TreeView 的展開節點），應用程式必須有自訂程式碼，以將行為模型化為可序列化的應用程式狀態。

## <a name="where-to-persist-state"></a>保存狀態的位置

有三個常見的位置存在於 Blazor 伺服器應用程式中的保存狀態。 每個方法最適合不同的案例，而且有不同的注意事項：

* [資料庫中的伺服器端](#server-side-in-a-database)
* [URL](#url)
* [瀏覽器中的用戶端](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a>資料庫中的伺服器端

針對永久資料保存或任何必須跨越多個使用者或裝置的資料，獨立的伺服器端資料庫幾乎都是最好的選擇。 這些選項包括：

* 關係 SQL 資料庫
* 索引鍵-值存放區
* Blob 存放區
* 資料表存放區

將資料儲存在資料庫之後，使用者可以隨時啟動新的電路。 使用者的資料會保留在任何新的線路中並可供使用。

如需 Azure 資料儲存體選項的詳細資訊，請參閱[Azure 儲存體檔](/azure/storage/)和[azure 資料庫](https://azure.microsoft.com/product-categories/databases/)。

### <a name="url"></a>{2&gt; URL&lt;2}

對於代表導覽狀態的暫時性資料，請將資料模型為 URL 的一部分。 在 URL 中模型化的狀態範例包括：

* 已查看之實體的識別碼。
* 分頁方格中的目前頁碼。

瀏覽器的網址列內容會保留：

* 如果使用者手動重載頁面。
* 如果 web 伺服器變得無法使用&mdash;則會強制使用者重載頁面，以便連接到不同的伺服器。

如需使用 `@page` 指示詞定義 URL 模式的詳細資訊，請參閱 <xref:blazor/routing>。

### <a name="client-side-in-the-browser"></a>瀏覽器中的用戶端

對於使用者主動建立的暫時性資料，常見的備份存放區是瀏覽器的 `localStorage` 和 `sessionStorage` 集合。 若已放棄迴圈，則不需要應用程式來管理或清除已儲存的狀態，這是優於伺服器端儲存體的優點。

> [!NOTE]
> 本節中的「用戶端」指的是瀏覽器中的用戶端案例，而不是[Blazor WebAssembly 裝載模型](xref:blazor/hosting-models#blazor-webassembly)。 `localStorage` 和 `sessionStorage` 可以在 Blazor WebAssembly 應用程式中使用，但只能透過撰寫自訂程式碼或使用協力廠商套件。

`localStorage` 和 `sessionStorage` 的差異如下：

* `localStorage` 的範圍設定為使用者的瀏覽器。 如果使用者重載頁面，或關閉並重新開啟瀏覽器，則狀態會保持不變。 如果使用者開啟多個瀏覽器索引標籤，則狀態會在索引標籤之間共用。 資料會保存在 `localStorage` 中，直到明確清除為止。
* `sessionStorage` 的範圍設定為使用者的瀏覽器索引標籤。如果使用者重載索引標籤，則狀態會保持不變。 如果使用者關閉索引標籤或瀏覽器，則狀態會遺失。 如果使用者開啟多個瀏覽器索引標籤，每個索引標籤都有自己獨立的資料版本。

一般來說，`sessionStorage` 較安全，使用。 `sessionStorage` 可避免使用者開啟多個索引標籤並遇到下列問題的風險：

* 索引標籤上狀態儲存體中的 bug。
* 當索引標籤覆寫其他索引標籤的狀態時，會造成混淆的行為。

如果應用程式必須在關閉並重新開啟瀏覽器時保存狀態，`localStorage` 是較好的選擇。

使用瀏覽器儲存的注意事項：

* 類似于使用伺服器端資料庫，載入和儲存資料是非同步。
* 與伺服器端資料庫不同的是，預先呈現期間無法使用儲存體，因為在預先呈現階段期間，瀏覽器中不存在要求的頁面。
* 儲存幾 kb 的資料可合理保存 Blazor 伺服器應用程式。 除了幾 kb 以外，您還必須考慮效能上的影響，因為資料是透過網路載入和儲存。
* 使用者可能會看到或篡改資料。 ASP.NET Core[資料保護](xref:security/data-protection/introduction)可以降低風險。

## <a name="third-party-browser-storage-solutions"></a>協力廠商瀏覽器儲存解決方案

協力廠商 NuGet 套件提供使用 `localStorage` 和 `sessionStorage`的 Api。

值得考慮選擇以透明方式使用 ASP.NET Core[資料保護](xref:security/data-protection/introduction)的套件。 ASP.NET Core 資料保護會將儲存的資料加密，並降低篡改已儲存資料的潛在風險。 如果 JSON 序列化資料是以純文字儲存，則使用者可以使用瀏覽器開發人員工具來查看資料，同時也會修改儲存的資料。 保護資料不一定會有問題，因為資料在本質上可能是很簡單的。 例如，讀取或修改 UI 專案的預存色彩對使用者或組織而言並不會有嚴重的安全性風險。 避免允許使用者檢查或篡改*敏感性資料*。

## <a name="protected-browser-storage-experimental-package"></a>受保護的瀏覽器儲存體實驗性封裝

提供`localStorage`和[資料保護](xref:security/data-protection/introduction)的 NuGet 套件範例是 AspNetCore.`sessionStorage` [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)。

> [!WARNING]
> `Microsoft.AspNetCore.ProtectedBrowserStorage` 是不受支援的實驗性封裝，目前不適用於生產環境使用。

### <a name="installation"></a>安裝

若要安裝 `Microsoft.AspNetCore.ProtectedBrowserStorage` 套件：

1. 在 Blazor Server 應用程式專案中，將套件參考新增至[AspNetCore. ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)。
1. 在最上層 HTML 中（例如，在預設專案範本的*Pages/_Host. cshtml*檔案中），加入下列 `<script>` 標記：

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. 在 `Startup.ConfigureServices` 方法中，呼叫 `AddProtectedBrowserStorage`，將 `localStorage` 和 `sessionStorage` 服務新增至服務集合：

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a>儲存和載入元件中的資料

在需要將資料載入或儲存至瀏覽器儲存體的任何元件中，使用[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component)來插入下列任一項的實例：

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

選擇取決於您想要使用的備份存放區。 在下列範例中，會使用 `sessionStorage`：

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

`@using` 語句可以放入 *_Imports 的 razor*檔案中，而不是在元件中。 使用 *_Imports razor*檔案可讓應用程式或整個應用程式的較大區段使用命名空間。

若要將 `currentCount` 值保存在專案範本的 `Counter` 元件中，請將 `IncrementCount` 方法修改為使用 `ProtectedSessionStore.SetAsync`：

```csharp
private async Task IncrementCount()
{
    currentCount++;
    await ProtectedSessionStore.SetAsync("count", currentCount);
}
```

在更大、更實際的應用程式中，個別欄位的儲存是不太可能發生的情況。 應用程式較可能儲存包含複雜狀態的整個模型物件。 `ProtectedSessionStore` 會自動序列化和還原序列化 JSON 資料。

在上述程式碼範例中，`currentCount` 資料會以 `sessionStorage['count']` 方式儲存在使用者的瀏覽器中。 資料不會以純文字儲存，而是使用 ASP.NET Core 的[資料保護](xref:security/data-protection/introduction)來保護。 如果在瀏覽器的開發人員主控台中評估 `sessionStorage['count']`，就可以看到加密的資料。

若要復原 `currentCount` 資料（如果使用者稍後回到 `Counter` 元件（包括它們是在全新的線路上），請使用 `ProtectedSessionStore.GetAsync`：

```csharp
protected override async Task OnInitializedAsync()
{
    currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

如果元件的參數包含導覽狀態，請呼叫 `ProtectedSessionStore.GetAsync` 並在 `OnParametersSetAsync`中指派結果，而不是 `OnInitializedAsync`。 只有第一次具現化元件時，才會呼叫 `OnInitializedAsync`。 如果使用者在相同頁面上繼續流覽至不同的 URL，則稍後不會再次呼叫 `OnInitializedAsync`。 如需詳細資訊，請參閱<xref:blazor/lifecycle>。

> [!WARNING]
> 此章節中的範例僅適用于伺服器未啟用預先安裝的情況。 啟用預入功能時，會產生類似下列的錯誤：
>
> > JavaScript interop calls cannot be issued at this time. This is because the component is being prerendered.
>
> 請停用已選擇的，或加入額外的程式碼以使用可處理的。 若要深入瞭解如何撰寫可搭配已預呈現運作的程式碼，請參閱[處理預呈現](#handle-prerendering)一節。

### <a name="handle-the-loading-state"></a>處理載入狀態

由於瀏覽器儲存體是非同步（透過網路連線存取），因此在載入資料並可供元件使用之前，一律會有一段時間。 若要獲得最佳結果，請在載入進行時轉譯載入狀態訊息，而不是顯示空白或預設的資料。

其中一種方法是追蹤資料是否 `null` （仍在載入中）。 In the default `Counter` component, the count is held in an `int`. Make `currentCount` nullable by adding a question mark (`?`) to the type (`int`):

```csharp
private int? currentCount;
```

Instead of unconditionally displaying the count and **Increment** button, choose to display these elements only if the data is loaded:

```razor
@if (currentCount.HasValue)
{
    <p>Current count: <strong>@currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a>Handle prerendering

During prerendering:

* An interactive connection to the user's browser doesn't exist.
* The browser doesn't yet have a page in which it can run JavaScript code.

`localStorage` or `sessionStorage` aren't available during prerendering. If the component attempts to interact with storage, an error is generated similar to:

> JavaScript interop calls cannot be issued at this time. This is because the component is being prerendered.

One way to resolve the error is to disable prerendering. This is usually the best choice if the app makes heavy use of browser-based storage. Prerendering adds complexity and doesn't benefit the app because the app can't prerender any useful content until `localStorage` or `sessionStorage` are available.

To disable prerendering, open the *Pages/_Host.cshtml* file and change the call to `render-mode` of the `Component` Tag Helper to `Server`.

Prerendering might be useful for other pages that don't use `localStorage` or `sessionStorage`. To keep prerendering enabled, defer the loading operation until the browser is connected to the circuit. The following is an example for storing a counter value:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore

... rendering code goes here ...

@code {
    private int? currentCount;
    private bool isConnected = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // When execution reaches this point, the first *interactive* render
            // is complete. The component has an active connection to the browser.
            isConnected = true;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        currentCount++;
        await ProtectedSessionStore.SetAsync("count", currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a>Factor out the state preservation to a common location

If many components rely on browser-based storage, re-implementing state provider code many times creates code duplication. One option for avoiding code duplication is to create a *state provider parent component* that encapsulates the state provider logic. Child components can work with persisted data without regard to the state persistence mechanism.

In the following example of a `CounterStateProvider` component, counter data is persisted:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (hasLoaded)
{
    <CascadingValue Value="@this">
        @ChildContent
    </CascadingValue>
}
else
{
    <p>Loading...</p>
}

@code {
    private bool hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

The `CounterStateProvider` component handles the loading phase by not rendering its child content until loading is complete.

To use the `CounterStateProvider` component, wrap an instance of the component around any other component that requires access to the counter state. To make the state accessible to all components in an app, wrap the `CounterStateProvider` component around the `Router` in the `App` component (*App.razor*):

```razor
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

Wrapped components receive and can modify the persisted counter state. The following `Counter` component implements the pattern:

```razor
@page "/counter"

<p>Current count: <strong>@CounterStateProvider.CurrentCount</strong></p>

<button @onclick="IncrementCount">Increment</button>

@code {
    [CascadingParameter]
    private CounterStateProvider CounterStateProvider { get; set; }

    private async Task IncrementCount()
    {
        CounterStateProvider.CurrentCount++;
        await CounterStateProvider.SaveChangesAsync();
    }
}
```

The preceding component isn't required to interact with `ProtectedBrowserStorage`, nor does it deal with a "loading" phase.

To deal with prerendering as described earlier, `CounterStateProvider` can be amended so that all of the components that consume the counter data automatically work with prerendering. See the [Handle prerendering](#handle-prerendering) section for details.

In general, *state provider parent component* pattern is recommended:

* To consume state in many other components.
* If there's just one top-level state object to persist.

To persist many different state objects and consume different subsets of objects in different places, it's better to avoid handling the loading and saving of state globally.
