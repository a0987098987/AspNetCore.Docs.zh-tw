---
title: ASP.NETBlazor核心狀態管理
author: guardrex
description: 瞭解如何在伺服器應用中Blazor保持狀態。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- SignalR
uid: blazor/state-management
ms.openlocfilehash: e8a1959a8fc05ea59362bb5824181a9d2e418811
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218865"
---
# <a name="aspnet-core-opno-locblazor-state-management"></a>ASP.NETBlazor核心狀態管理

作者：[Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor伺服器是一個有狀態的應用框架。 大多數時候,應用程式保持與伺服器的持續連接。 用戶的狀態在*電路*中保持在伺服器的記憶體中。 

使用者電路的狀態範例包括:

* 呈現的&mdash;UI 是元件實例的層次結構及其最新的呈現輸出。
* 元件實例中任何欄位和屬性的值。
* 在[依賴項注入 (DI)](xref:fundamentals/dependency-injection)服務實例中持有的數據,這些實例範圍可限定在電路中。

> [!NOTE]
> 本文介紹伺服器應用中Blazor的狀態持久性。 BlazorWebAssembly 應用可以利用[瀏覽器中的用戶端狀態持久性,](#client-side-in-the-browser)但需要超出本文範圍的自定義解決方案或第三方包。

## <a name="opno-locblazor-circuits"></a>Blazor電路

如果使用者遇到臨時網路連接丟失Blazor, 嘗試將使用者重新連接到其原始電路,以便他們可以繼續使用該應用程式。 但是,在伺服器記憶體中將使用者重新連接到其原始電路並不總是可能的:

* 伺服器無法永久保留斷開連接的電路。 伺服器必須在超時後或伺服器處於記憶體壓力下釋放斷開連接的電路。
* 在多伺服器負載均衡部署環境中,任何伺服器處理請求在任意給定時間都可能不可用。 當不再需要處理請求的總量時,單個伺服器可能會失敗或自動刪除。 當用戶嘗試重新連接時,原始伺服器可能不可用。
* 使用者可能會關閉並重新打開瀏覽器或重新載入頁面,從而刪除瀏覽器記憶體中保持的任何狀態。 例如,透過 JAVAScript 互通調用設置的值將丟失。

當使用者無法重新連接到其原始電路時,使用者將收到一個狀態為空的新電路。 這相當於關閉和重新打開桌面應用。

## <a name="preserve-state-across-circuits"></a>跨電路保留狀態

在某些情況下,需要保留跨電路的狀態。 如果:

* Web 伺服器將不可用。
* 用戶的瀏覽器被迫使用新的 Web 伺服器啟動新電路。

通常,跨電路維護狀態適用於使用者積極創建數據的情況,而不僅僅是讀取已經存在的數據。

為了在單一電路之外保留狀態,*不要只將資料儲存在伺服器的記憶體中*。 應用必須將數據保存到其他存儲位置。 狀態持久性不是自動&mdash;的,在開發應用以實現有狀態的數據持久性時,您必須採取措施。

數據持久性通常只針對用戶花費努力創建的高價值狀態。 在以下範例中,持續狀態可以節省時間或幫助商務工作:

* 多步驟 Webform&ndash;如果使用者的狀態丟失,則使用者重新輸入多步驟過程幾個已完成步驟的數據非常耗時。 如果使用者從多步驟窗體導航,並在以後返回到窗體,則使用者將在此方案中失去狀態。
* 購物車&ndash;可以維護代表潛在收入的應用的任何商業重要元件。 失去狀態的使用者,因此他們的購物車,在稍後返回網站時可能會購買較少的產品或服務。

通常不需要保留易於重新建立的狀態,例如輸入到尚未提交的登錄對話方塊中的使用者名。

> [!IMPORTANT]
> 套用只能保留*套用狀態*。 無法持久化 U,例如元件實例及其呈現樹。 元件和呈現樹通常不可序列化。 要保留類似 UI 狀態的內容(如 TreeView 的擴展節點),應用必須具有自定義代碼,才能將行為建模為可序列化應用狀態。

## <a name="where-to-persist-state"></a>在何處保持狀態

存在三個Blazor常見位置,用於在伺服器應用中保持狀態。 每種方法最適合不同的方案,並有不同的注意事項:

* [資料庫中的伺服器端](#server-side-in-a-database)
* [URL](#url)
* [瀏覽器中的用戶端](#client-side-in-the-browser)

### <a name="server-side-in-a-database"></a>資料庫中的伺服器端

對於永久數據持久性或任何必須跨越多個使用者或設備的數據,獨立的伺服器端資料庫幾乎肯定是最佳選擇。 選項包括：

* 關聯 SQL 資料庫
* 索引鍵-值存放區
* Blob 儲存
* 表儲存

將數據保存到資料庫中后,用戶可以隨時啟動新電路。 用戶的數據將保留並在任何新電路中可用。

關於 Azure 資料儲存選項的詳細資訊,請參考[Azure 儲存文件](/azure/storage/)與[Azure 資料庫](https://azure.microsoft.com/product-categories/databases/)。

### <a name="url"></a>URL

對於表示導航狀態的瞬態數據,將資料建模為網址的一部分。 在網址中建模的狀態範例包括:

* 已查看實體的 ID。
* 分頁網格中的當前頁碼。

保存瀏覽器位址列的內容:

* 如果用戶手動重新載入頁面。
* 如果 Web 伺服器&mdash;不可用 ,則使用者被迫重新載入頁面以連接到其他伺服器。

有關使用`@page`指令定義網址的資訊,請<xref:blazor/routing>參閱 。

### <a name="client-side-in-the-browser"></a>瀏覽器中的用戶端

對於使用者正在積極創建的瞬態數據,常見的備份存儲是瀏覽器`localStorage`和`sessionStorage`集合。 如果電路被放棄,則不需要應用來管理或清除存儲狀態,這與伺服器端存儲具有優勢。

> [!NOTE]
> 本節中的「用戶端」是指瀏覽器中的用戶端方案,而不是[BlazorWebAssembly 託管模型](xref:blazor/hosting-models#blazor-webassembly)。 `localStorage``sessionStorage`和可以在BlazorWebAssembly 應用中使用,但只能透過編寫自訂代碼或使用第三方包。

`localStorage`和`sessionStorage`不同的如下:

* `localStorage`範圍限定為用戶的瀏覽器。 如果使用者重新載入頁面或關閉並重新打開瀏覽器,則狀態將保持不變。 如果用戶打開多個瀏覽器選項卡,則狀態將在選項卡之間共用。 資料會保留,`localStorage`直到顯式清除。
* `sessionStorage`限定到使用者的瀏覽器選項卡。如果使用者重新載入選項卡,則狀態將保持不變。 如果使用者關閉選項卡或瀏覽器,狀態將丟失。 如果使用者打開多個瀏覽器選項卡,則每個選項卡都有其自己的獨立版本的數據。

通常,`sessionStorage`使用更安全。 `sessionStorage`避免了使用者打開多個選項卡並遇到以下情況的風險:

* 跨選項卡的狀態存儲中的 Bug。
* 當選項卡覆蓋其他選項卡的狀態時,混淆行為。

`localStorage`如果應用必須在關閉和重新打開瀏覽器時保持狀態,則是更好的選擇。

使用瀏覽器儲存的注意事項:

* 與使用伺服器端資料庫類似,載入和保存數據是異步的。
* 與伺服器端資料庫不同,在預渲染期間存儲不可用,因為在預渲染階段瀏覽器中不存在請求的頁面。
* 存儲幾千位元組的數據Blazor對於伺服器應用是合理的。 除了幾千位元組之外,您還必須考慮性能影響,因為數據透過網路載入和保存。
* 用戶可以查看或篡改數據。 ASP.NET核心[數據保護](xref:security/data-protection/introduction)可以降低風險。

## <a name="third-party-browser-storage-solutions"></a>第三方瀏覽器儲存解決方案

第三方 NuGet 套件提供`localStorage`用於`sessionStorage`使用和的 API。

值得考慮選擇透明地使用 ASP.NET 核心[資料保護的套件](xref:security/data-protection/introduction)。 ASP.NET核心數據保護對存儲的數據進行加密,並降低篡改存儲數據的潛在風險。 如果 JSON 序列化數據儲存在純文本中,則使用者可以使用瀏覽器開發人員工具查看數據,還可以修改儲存的資料。 保護數據並不總是問題,因為數據在本質上可能微不足道。 例如,讀取或修改 UI 元素的儲存顏色對使用者或組織來說並不構成重大安全風險。 避免允許使用者檢查或篡改*敏感資料*。

## <a name="protected-browser-storage-experimental-package"></a>受保護的瀏覽器儲存實驗套件

為 微軟提供[資料保護](xref:security/data-protection/introduction)`localStorage`的 NuGet`sessionStorage`套件範例 是[.AspNetCore.受保護的瀏覽器儲存](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)。

> [!WARNING]
> `Microsoft.AspNetCore.ProtectedBrowserStorage`是一個不支持的實驗包,不適合生產在這個時候使用。

### <a name="installation"></a>安裝

要安裝`Microsoft.AspNetCore.ProtectedBrowserStorage`套件:

1. 在Blazor「伺服器」應用專案中,添加對[Microsoft 的](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage)包引用。
1. 在頂級 HTML 中(例如,在預設項目樣本中的*Pages/_Host.cshtml*檔案中),新增以下`<script>`標籤:

   ```html
   <script src="_content/Microsoft.AspNetCore.ProtectedBrowserStorage/protectedBrowserStorage.js"></script>
   ```

1. 在`Startup.ConfigureServices`方法中,`AddProtectedBrowserStorage`呼`localStorage`叫`sessionStorage`以 新增和服務到服務集合:

   ```csharp
   services.AddProtectedBrowserStorage();
   ```

### <a name="save-and-load-data-within-a-component"></a>儲存在元件中儲存及載入資料

在需要將資料載入或儲存到瀏覽器儲存的任何元件中,使用[`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component)注入以下任一的實體:

* `ProtectedLocalStorage`
* `ProtectedSessionStorage`

選擇取決於您希望使用哪個後備存儲。 在下面的範例中,`sessionStorage`使用:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore
```

語句`@using`可以放置在 *_Imports.razor*檔中,而不是元件中。 使用 *_Imports.razor*檔案使命名空間可用於應用或整個應用的較大部分。

在`_currentCount`專案樣本的`Counter`元件中保留該值,`IncrementCount`請修改要`ProtectedSessionStore.SetAsync`使用的方法:

```csharp
private async Task IncrementCount()
{
    _currentCount++;
    await ProtectedSessionStore.SetAsync("count", _currentCount);
}
```

在更大、更現實的應用中,單個字段的存儲不太可能發生。 應用更有可能存儲包含複雜狀態的整個模型物件。 `ProtectedSessionStore`自動序列化和脫序列化 JSON 數據。

在前面的代碼範例中,`_currentCount`資料儲存`sessionStorage['count']`在用戶的瀏覽器中。 資料不是以純文字儲存的,而是使用ASP.NET核心的[資料保護進行保護](xref:security/data-protection/introduction)。 如果在`sessionStorage['count']`瀏覽器的開發人員控制台中評估加密數據,則可以看到加密數據。

要復`_currentCount`原資料,如果使用者稍後傳`Counter`回到 元件(包括如果他們在一個全新的電路上),請`ProtectedSessionStore.GetAsync`使用 :

```csharp
protected override async Task OnInitializedAsync()
{
    _currentCount = await ProtectedSessionStore.GetAsync<int>("count");
}
```

如果元件的參數包括導覽狀態,請在`ProtectedSessionStore.GetAsync``OnParametersSetAsync`中呼叫並分配結果,`OnInitializedAsync`而不是 。 `OnInitializedAsync`僅在首次實例化元件時調用一次。 `OnInitializedAsync`如果使用者在同一頁上導航到其他 URL,則以後不會再次調用。 如需詳細資訊，請參閱 <xref:blazor/lifecycle>。

> [!WARNING]
> 僅當伺服器未啟用預渲染時,本節中的示例才起作用。 開啟預成像後,將產生類似於以下錯誤:
>
> > 此時無法發出 JavaScript 互通呼叫。 這是因為元件正在預呈現。
>
> 禁用預渲染或添加其他代碼以處理預渲染。 要瞭解有關編寫適用於預算的代碼的更多詳細資訊,請參閱[「處理預渲染」](#handle-prerendering)部分。

### <a name="handle-the-loading-state"></a>處理載入狀態

由於瀏覽器存儲是異步的(通過網路連接訪問),因此在載入數據並可供元件使用之前,始終有一段時間。 為獲得最佳結果,在載入正在進行時呈現載入狀態消息,而不是顯示空白或默認資料。

一種方法是跟蹤數據是否正在`null`載入(仍在載入)。 在預設`Counter`元件中,計數在中`int`保持。 通過將`_currentCount`問號 (`?`)`int`添加到 類型 ( ) 使空。

```csharp
private int? _currentCount;
```

選擇僅在載入資料時顯示這些元素,而不是無條件顯示計數和**增量**按鈕:

```razor
@if (_currentCount.HasValue)
{
    <p>Current count: <strong>@_currentCount</strong></p>

    <button @onclick="IncrementCount">Increment</button>
}
else
{
    <p>Loading...</p>
}
```

### <a name="handle-prerendering"></a>處理預像

在預成成的紀錄期間:

* 與用戶瀏覽器的互動式連接不存在。
* 瀏覽器還沒有一個頁面,它可以運行JAVAScript代碼。

`localStorage`或`sessionStorage`在預渲染期間不可用。 如果元件嘗試與儲存互動,則產生錯誤類似於:

> 此時無法發出 JavaScript 互通呼叫。 這是因為元件正在預呈現。

解決錯誤的一種方法是禁用預渲染。 如果應用大量使用基於瀏覽器的存儲,這通常是最佳選擇。 預渲染會增加複雜性,並且對應用沒有好處,因為應用在可用或`localStorage``sessionStorage`可用之前無法預算任何有用的內容。

要關閉預先成,請開啟*頁面/_Host.cshtml*檔`render-mode`並將[元件標記說明器](xref:mvc/views/tag-helpers/builtin-th/component-tag-helper)<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server>變更為 。

預渲染對於不使用`localStorage``sessionStorage`或的其他頁面可能很有用。 要保持預渲染啟用,請延遲載入操作,直到瀏覽器連接到電路。 以下是儲存計數器值的範例:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedLocalStorage ProtectedLocalStore

... rendering code goes here ...

@code {
    private int? _currentCount;
    private bool _isConnected = false;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // When execution reaches this point, the first *interactive* render
            // is complete. The component has an active connection to the browser.
            _isConnected = true;
            await LoadStateAsync();
            StateHasChanged();
        }
    }

    private async Task LoadStateAsync()
    {
        _currentCount = await ProtectedLocalStore.GetAsync<int>("prerenderedCount");
    }

    private async Task IncrementCount()
    {
        _currentCount++;
        await ProtectedSessionStore.SetAsync("count", _currentCount);
    }
}
```

### <a name="factor-out-the-state-preservation-to-a-common-location"></a>將狀態儲存分解到公共位置

如果許多元件依賴於基於瀏覽器的存儲,則多次重新實現狀態提供程式代碼會創建代碼重複。 避免程式碼重複的一個選項是建立封裝狀態*提供程式邏輯的狀態提供程式父元件*。 子元件可以使用持久數據,而不考慮狀態持久性機制。

在以下`CounterStateProvider`元件範例中,將保留計數器資料:

```razor
@using Microsoft.AspNetCore.ProtectedBrowserStorage
@inject ProtectedSessionStorage ProtectedSessionStore

@if (_hasLoaded)
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
    private bool _hasLoaded;

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    public int CurrentCount { get; set; }

    protected override async Task OnInitializedAsync()
    {
        CurrentCount = await ProtectedSessionStore.GetAsync<int>("count");
        _hasLoaded = true;
    }

    public async Task SaveChangesAsync()
    {
        await ProtectedSessionStore.SetAsync("count", CurrentCount);
    }
}
```

元件`CounterStateProvider`在載入完成之前不呈現其子內容來處理載入階段。

要使用該`CounterStateProvider`元件,將元件的實例環繞到需要存取計數器狀態的任何其他元件周圍。 要使應用中的所有元件都能存取`CounterStateProvider`狀態,將元件環繞`Router``App`在 元件中 *(App.razor*):

```razor
<CounterStateProvider>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CounterStateProvider>
```

包裝元件接收並可以修改持久化計數器狀態。 以下`Counter`元件實現模式:

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

不需要上述元件與`ProtectedBrowserStorage`交互,也不處理"載入"階段。

要處理前面所述的預算,`CounterStateProvider`可以修改,以便使用計數器數據的所有元件自動使用預算。 有關詳細資訊,請參閱["處理預渲染"](#handle-prerendering)部分。

通常,建議*狀態提供程式父元件*模式:

* 在許多其他元件中使用狀態。
* 如果只有一個頂級狀態物件要持久化。

要在不同位置保留許多不同的狀態物件並使用不同的物件子集,最好避免全域處理狀態的載入和保存。
