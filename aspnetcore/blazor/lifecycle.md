---
title: ASP.NET Core Blazor生命週期
author: guardrex
description: 瞭解如何在 ASP.NET Core Razor Blazor應用程式中使用元件生命週期方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: 571f14247efe08ac6abbd6d1e2720656f94c213c
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967450"
---
# <a name="aspnet-core-blazor-lifecycle"></a>ASP.NET Core Blazor生命週期

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

此Blazor架構包含同步和非同步生命週期方法。 覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。

## <a name="lifecycle-methods"></a>生命週期方法

### <a name="component-initialization-methods"></a>元件初始化方法

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>當<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A>元件從其父元件接收到其初始參數之後，就會叫用和。 當`OnInitializedAsync`元件執行非同步作業時使用，而且應該在作業完成時重新整理。

針對同步作業，覆寫`OnInitialized`：

```csharp
protected override void OnInitialized()
{
    ...
}
```

若要執行非同步作業，請`OnInitializedAsync`覆寫並`await`在作業上使用關鍵字：

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor將[其內容](xref:blazor/hosting-model-configuration#render-mode)呼叫`OnInitializedAsync`呈現**_兩次_** 的伺服器應用程式：

* 當元件一開始以靜態方式轉譯為頁面的一部分時。
* 第二次當瀏覽器建立與伺服器的連接時。

若要防止中`OnInitializedAsync`的開發人員程式碼執行兩次，請參閱在預做後重新設定[狀態](#stateful-reconnection-after-prerendering)一節。

在預先Blazor處理伺服器應用程式時，因為尚未建立與瀏覽器的連接，所以無法執行某些動作（例如呼叫 JavaScript）。 元件可能需要在資源清單時以不同的方式呈現。 如需詳細資訊，請參閱偵測[應用程式何時進行預呈現](#detect-when-the-app-is-prerendering)一節。

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="before-parameters-are-set"></a>設定參數之前

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A>在轉譯樹狀結構中設定元件的父系所提供的參數：

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView>每次`SetParametersAsync`呼叫時，包含一組完整的參數值。

的預設執行會`SetParametersAsync`使用在中具有對應值的`[Parameter]`或`[CascadingParameter]`屬性，來設定每個屬性的值`ParameterView`。 在中`ParameterView`沒有對應值的參數會保持不變。

如果`base.SetParametersAync`未叫用，自訂程式碼就可以任何需要的方式解讀傳入的參數值。 例如，不需要將傳入的參數指派給類別的屬性。

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="after-parameters-are-set"></a>設定參數之後

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A>系統<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet%2A>會呼叫和：

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
> 當套用參數和屬性值時， `OnParametersSetAsync`必須在生命週期事件期間進行非同步工作。

```csharp
protected override void OnParametersSet()
{
    ...
}
```

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="after-component-render"></a>元件呈現之後

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A>在<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A>元件完成呈現之後，會呼叫和。 此時會填入元素和元件參考。 使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。

`OnAfterRenderAsync`和`firstRender` `OnAfterRender`的參數：

* 會在第`true`一次呈現元件實例時設定為。
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
> 在生命週期事件期間， `OnAfterRenderAsync`必須立即執行非同步工作。
>
> 即使您<xref:System.Threading.Tasks.Task>從`OnAfterRenderAsync`傳回，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。 這是為了避免無限的呈現迴圈。 它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender`在`OnAfterRenderAsync` *伺服器上進行預呈現時，不會呼叫和。*

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="suppress-ui-refreshing"></a>隱藏 UI 重新整理

覆<xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>寫以隱藏 UI 重新整理。 如果執行傳回`true`，則會重新整理 UI：

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender`每次呈現元件時，都會呼叫。

即使`ShouldRender`已覆寫，元件一律會一開始呈現。

## <a name="state-changes"></a>狀態變更

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>通知元件其狀態已變更。 當適用時， `StateHasChanged`呼叫會導致元件重新顯示。

## <a name="handle-incomplete-async-actions-at-render"></a>處理轉譯時的未完成非同步動作

在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。 當生命週期`null`方法正在執行時，物件可能會或未完全填入資料。 提供轉譯邏輯，以確認物件已初始化。 當物件為`null`時，呈現預留位置 UI 專案（例如，載入訊息）。

在`FetchData` Blazor範本的元件中， `OnInitializedAsync`會覆寫為 asychronously 接收預測資料（`forecasts`）。 當`forecasts`為`null`時，會向使用者顯示載入訊息。 在所`Task`傳回的`OnInitializedAsync`完成之後，元件會以更新的狀態重新顯示。

伺服器範本中的Blazor *Pages/FetchData* ：

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>使用 IDisposable 的元件處置

如果元件會執行<xref:System.IDisposable>，則會在從 UI 中移除元件時呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。 下列元件會使用`@implements IDisposable`和`Dispose`方法：

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
> 不<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>支援`Dispose`在中呼叫。 `StateHasChanged`可能會在卸載轉譯器的過程中叫用，因此不支援在該時間點要求 UI 更新。

取消訂閱來自 .NET 事件的事件處理常式。 下列[ Blazor表單](xref:blazor/forms-validation)範例示範如何在`Dispose`方法中解除掛接事件處理常式：

* 私用欄位和 lambda 方法

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* 私用方法方法

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a>處理錯誤

如需在生命週期方法執行期間處理錯誤的<xref:blazor/handle-errors#lifecycle-methods>詳細資訊，請參閱。

## <a name="stateful-reconnection-after-prerendering"></a>預呈現後的具狀態重新連接

在Blazor伺服器應用程式中`RenderMode` ， `ServerPrerendered`當為時，元件一開始會以靜態方式呈現為頁面的一部分。 當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。 如果存在用於初始化元件的[OnInitialized {Async}](#component-initialization-methods)生命週期方法，則會執行*兩次*方法：

* 當元件以靜態方式資源清單時。
* 建立伺服器連接之後。

這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。

若要避免Blazor伺服器應用程式中的雙呈現案例：

* 傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。
* 在預入期間使用識別碼來儲存元件狀態。
* 在可呈現後使用識別碼，以取得快取的狀態。

下列程式碼示範在以`WeatherForecastService`範本為基礎Blazor的伺服器應用程式中已更新，可避免雙重呈現：

```csharp
public class WeatherForecastService
{
    private static readonly string[] summaries = new[]
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
                Summary = summaries[rng.Next(summaries.Length)]
            }).ToArray();
        });
    }
}
```

如需的`RenderMode`詳細資訊，請<xref:blazor/hosting-model-configuration#render-mode>參閱。

## <a name="detect-when-the-app-is-prerendering"></a>偵測應用程式何時已進行預呈現

[!INCLUDE[](~/includes/blazor-prerendering.md)]
