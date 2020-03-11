---
title: ASP.NET Core Blazor 生命週期
author: guardrex
description: 瞭解如何在 ASP.NET Core Blazor 應用程式中使用 Razor 元件生命週期方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: ecacd0a9728cbefd716e9dc7cd8a8c62f3df6e0d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659926"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET Core Blazor 生命週期

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Blazor 架構包含同步和非同步生命週期方法。 覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。

## <a name="lifecycle-methods"></a>生命週期方法

### <a name="component-initialization-methods"></a>元件初始化方法

當元件從其父元件收到其初始參數之後初始化時，就會叫用 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*>。 當元件執行非同步作業時，請使用 `OnInitializedAsync`，而且應該在作業完成時重新整理。

若為同步作業，請覆寫 `OnInitialized`：

```csharp
protected override void OnInitialized()
{
    ...
}
```

若要執行非同步作業，請覆寫 `OnInitializedAsync`，並在作業上使用 `await` 關鍵字：

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor 伺服器應用程式可將[其內容呼叫呈現](xref:blazor/hosting-model-configuration#render-mode)`OnInitializedAsync` **_兩次_** ：

* 當元件一開始以靜態方式轉譯為頁面的一部分時。
* 第二次當瀏覽器建立與伺服器的連接時。

若要防止 `OnInitializedAsync` 中的開發人員程式碼執行兩次，請參閱 [已自[呈現後](#stateful-reconnection-after-prerendering)重新連線] 區段。

當 Blazor 伺服器應用程式進行預先處理時，某些動作（例如呼叫 JavaScript）並不可能，因為尚未建立與瀏覽器的連接。 元件可能需要在資源清單時以不同的方式呈現。 如需詳細資訊，請參閱偵測[應用程式何時進行預呈現](#detect-when-the-app-is-prerendering)一節。

### <a name="before-parameters-are-set"></a>設定參數之前

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> 會在轉譯樹狀結構中設定元件的父系所提供的參數：

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView> 在每次呼叫 `SetParametersAsync` 時都包含整個參數值集。

`SetParametersAsync` 的預設執行會使用在 `ParameterView`中具有對應值的 `[Parameter]` 或 `[CascadingParameter]` 屬性，來設定每個屬性的值。 `ParameterView` 中沒有對應值的參數會保持不變。

如果未叫用 `base.SetParametersAync`，自訂程式碼就可以任何需要的方式解讀傳入的參數值。 例如，不需要將傳入的參數指派給類別的屬性。

### <a name="after-parameters-are-set"></a>設定參數之後

呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>：

* 當元件初始化並從其父元件收到第一組參數時。
* 當父元件重新呈現和提供時：
  * 只有已知的基本不可變類型，其中至少有一個參數已變更。
  * 任何複雜類型的參數。 架構無法得知複雜型別參數的值是否在內部變動，因此它會將參數集視為已變更。

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> 當套用參數和屬性值時，必須在 `OnParametersSetAsync` 生命週期事件期間進行非同步工作。

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a>元件呈現之後

在元件完成呈現之後，會呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*>。 此時會填入元素和元件參考。 使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。

`OnAfterRenderAsync` 和 `OnAfterRender`的 `firstRender` 參數：

* 會在第一次呈現元件實例時設定為 `true`。
* 可以用來確保初始化工作只會執行一次。

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> 在 `OnAfterRenderAsync` 生命週期事件期間，必須在轉譯之後立即進行非同步工作。
>
> 即使您從 `OnAfterRenderAsync`傳回 <xref:System.Threading.Tasks.Task>，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。 這是為了避免無限的呈現迴圈。 它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

在*伺服器上進行預呈現時，不會呼叫*`OnAfterRender` 和 `OnAfterRenderAsync`。

### <a name="suppress-ui-refreshing"></a>隱藏 UI 重新整理

覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> 以隱藏 UI 重新整理。 如果執行會傳回 `true`，則會重新整理 UI：

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

每次呈現元件時，都會呼叫 `ShouldRender`。

即使 `ShouldRender` 遭到覆寫，元件一律會一開始呈現。

## <a name="state-changes"></a>狀態變更

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> 通知元件其狀態已變更。 當適用時，呼叫 `StateHasChanged` 會導致元件重新顯示。

## <a name="handle-incomplete-async-actions-at-render"></a>處理轉譯時的未完成非同步動作

在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。 當生命週期方法正在執行時，物件可能 `null` 或未完全填入資料。 提供轉譯邏輯，以確認物件已初始化。 當物件 `null`時，轉譯預留位置 UI 元素（例如載入訊息）。

在 Blazor 範本的 `FetchData` 元件中，`OnInitializedAsync` 會覆寫為 asychronously 接收預測資料（`forecasts`）。 當 `forecasts` `null`時，就會向使用者顯示載入訊息。 `OnInitializedAsync` 所傳回的 `Task` 完成之後，元件會以更新的狀態重新顯示。

Blazor 伺服器範本中的*Pages/FetchData* ：

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>使用 IDisposable 的元件處置

如果元件會執行 <xref:System.IDisposable>，當元件從 UI 中移除時，就會呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。 下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> 不支援在 `Dispose` 中呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>。 在卸載轉譯器的過程中，可能會叫用 `StateHasChanged`，因此不支援在該時間點要求 UI 更新。

## <a name="handle-errors"></a>處理錯誤

如需在生命週期方法執行期間處理錯誤的詳細資訊，請參閱 <xref:blazor/handle-errors#lifecycle-methods>。

## <a name="stateful-reconnection-after-prerendering"></a>預呈現後的具狀態重新連接

在 Blazor 伺服器應用程式中，當 `RenderMode` `ServerPrerendered`時，元件一開始會以靜態方式轉譯為頁面的一部分。 當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。 如果存在用於初始化元件的[OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods)生命週期方法，則會執行*兩次*方法：

* 當元件以靜態方式資源清單時。
* 建立伺服器連接之後。

這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。

若要避免 Blazor 伺服器應用程式中的雙呈現案例：

* 傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。
* 在預入期間使用識別碼來儲存元件狀態。
* 在可呈現後使用識別碼，以取得快取的狀態。

下列程式碼示範以範本為基礎 Blazor 伺服器應用程式中，可避免雙重呈現的更新 `WeatherForecastService`：

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

如需 `RenderMode`的詳細資訊，請參閱 <xref:blazor/hosting-model-configuration#render-mode>。

## <a name="detect-when-the-app-is-prerendering"></a>偵測應用程式何時已進行預呈現

[!INCLUDE[](~/includes/blazor-prerendering.md)]
