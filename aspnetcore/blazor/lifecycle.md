---
title: ASP.NET核心Blazor生命週期
author: guardrex
description: 瞭解如何在ASP.NET核心Blazor應用中使用Razor元件生命週期方法。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: e7450ad57acc87500bb977aa8349c6ee009e3bf4
ms.sourcegitcommit: c9d1208e86160615b2d914cce74a839ae41297a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "81791458"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET核心Blazor生命週期

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

該Blazor框架包括同步和異步生命週期方法。 重寫生命週期方法,在元件初始化和呈現期間對元件執行其他操作。

## <a name="lifecycle-methods"></a>生命週期方法

### <a name="component-initialization-methods"></a>元件初始化方法

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*>並在<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*>從其父元件收到其初始參數后初始化元件時調用。 當`OnInitializedAsync`元件執行非同步操作並在操作完成後刷新時使用。

對同步操作,重寫`OnInitialized`:

```csharp
protected override void OnInitialized()
{
    ...
}
```

要執行非同步操作,在操作`OnInitializedAsync`上重寫並使用`await`關鍵字:

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor[已預先呈現內容](xref:blazor/hosting-model-configuration#render-mode)的伺服器套用`OnInitializedAsync`**_兩次_**:

* 當元件最初作為頁面的一部分以靜態方式呈現時。
* 第二次瀏覽器建立與伺服器的連接。

要防止開發人員代碼在`OnInitializedAsync`中運行兩次,請參閱[預渲染后的狀態重新連接](#stateful-reconnection-after-prerendering)部分。

當Blazor伺服器應用處於預渲染狀態時,某些操作(如調用 JAvaScript)是不可能的,因為尚未與瀏覽器建立連接。 元件在預渲染時可能需要以不同的方式呈現。 有關詳細資訊,請參閱[「檢測應用何時是預渲染」](#detect-when-the-app-is-prerendering)部分。

如果設置了任何事件處理程式,則取消處理它們。 有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。

### <a name="before-parameters-are-set"></a>在設定參數之前

<xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*>設定元件的父級在呈現樹中提供的參數:

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<xref:Microsoft.AspNetCore.Components.ParameterView>每次`SetParametersAsync`調用都包含整個參數值集。

的`SetParametersAsync`預設設定每個屬性的值,`[Parameter]`其`[CascadingParameter]`或 屬性在中具有`ParameterView`相應的值。 中沒有相應值`ParameterView`的參數保持不變。

如果未`base.SetParametersAync`調用,自定義代碼可以以所需的任何方式解釋傳入參數值。 例如,不需要將傳入參數分配給類上的屬性。

如果設置了任何事件處理程式,則取消處理它們。 有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。

### <a name="after-parameters-are-set"></a>設定參數後

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*>並<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>稱為:

* 當元件初始化並已收到其第一組參數時,
* 當父元件重新呈現與提供時:
  * 只有已知的原始不可變類型,其中至少有一個參數已更改。
  * 任何複雜類型的參數。 框架無法知道複雜類型參數的值是否在內部發生突變,因此將參數集視為已更改。

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> 在`OnParametersSetAsync`生命週期事件期間,應用參數和屬性值時必須發生非同步工作。

```csharp
protected override void OnParametersSet()
{
    ...
}
```

如果設置了任何事件處理程式,則取消處理它們。 有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。

### <a name="after-component-render"></a>元件成像後

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*>並在<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*>元件完成呈現後調用。 此時將填充元素和元件引用。 使用此階段可以使用呈現的內容執行其他初始化步驟,例如啟動對呈現 DOM 元素運行的第三方 JavaScript 庫。

與`firstRender``OnAfterRender`的`OnAfterRenderAsync`參數 。

* 設置為`true`首次呈現元件實例。
* 可用於確保初始化工作僅執行一次。

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
> 在`OnAfterRenderAsync`生命週期事件期間,渲染后立即進行非同步工作。
>
> 即使返回<xref:System.Threading.Tasks.Task>`OnAfterRenderAsync`from ,框架也不會在任務完成後為元件安排進一步的呈現週期。 這是為了避免無限渲染迴圈。 它不同於其他生命週期方法,後者計劃返回的任務完成後再進行一個渲染週期。

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

`OnAfterRender`在`OnAfterRenderAsync`*伺服器上預渲染時不調用。*

如果設置了任何事件處理程式,則取消處理它們。 有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。

### <a name="suppress-ui-refreshing"></a>禁止 UI 刷新

覆蓋<xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>以禁止 UI 刷新。 如果實現返回`true`,則刷新 UI:

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

`ShouldRender`每次呈現元件時都會調用。

即使`ShouldRender`被重寫,元件也始終被呈現。

## <a name="state-changes"></a>狀態變更

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>通知元件其狀態已更改。 如果適用,調用`StateHasChanged`會導致重新呈現元件。

## <a name="handle-incomplete-async-actions-at-render"></a>在渲染時處理不完整的非同步操作

在呈現元件之前,在生命週期事件中執行的異步操作可能尚未完成。 在執行生命週期方法`null`時,物件可能已或不完全填充了數據。 提供呈現邏輯以確認物件已初始化。 成像佔位元 UI 元素(例如,載入訊息`null`),而物件為 。

在`FetchData`樣本的元件中Blazor,`OnInitializedAsync`被重寫為以動態接收預測資料 ()。`forecasts` 當`forecasts``null`為 時,將向用戶顯示載入消息。 完成`Task`返回`OnInitializedAsync`後,元件將重新呈現為更新狀態。

Blazor伺服器樣本中的*頁面/FetchData.razor:*

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>使用 I 一次性工具處理元件

如果元件實現<xref:System.IDisposable>,則當從 UI 中移除元件時,將呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。 以下元件使用`@implements IDisposable`與方法`Dispose`:

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
> 不支援<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>呼`Dispose`叫 。 `StateHasChanged`可能會作為拆解呈現器的一部分調用,因此不支援此時請求 UI 更新。

從 .NET 事件取消訂閱事件處理程式。 以下[Blazor表單](xref:blazor/forms-validation)範例展示`Dispose`如何在方法中取消掛鉤事件處理程式:

* 私人領域和 lambda 方法

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* 私有方法方法

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a>處理錯誤

有關在生命週期方法執行期間處理錯誤的資訊,請參<xref:blazor/handle-errors#lifecycle-methods>閱 。

## <a name="stateful-reconnection-after-prerendering"></a>預先成圖狀態重新連線

在BlazorServer`RenderMode`應用`ServerPrerendered`中, 當 是 時,元件最初作為頁面的一部分以靜態方式呈現。 一旦瀏覽器建立回伺服器的連接,元件*將再次*呈現,並且元件現在具有交互性。 如果存在用於初始化元件的[On 初始化 {Async}](#component-initialization-methods)生命週期方法,則執行該方法*兩次*:

* 靜態預呈現元件時。
* 建立伺服器連接後。

當最終呈現元件時,這可能導致UI中顯示的數據發生顯著變化。

要避免Blazor伺服器應用中的雙重呈現方案,請進行以下檢查:

* 傳遞可用於在預渲染期間緩存狀態並在應用重新啟動后檢索狀態的標識符。
* 在預渲染期間使用標識符保存元件狀態。
* 在預渲染后使用標識符來檢索緩存的狀態。

以下代碼演示了基於`WeatherForecastService`Blazor樣本的 Server 應用中的更新,該應用避免了雙重呈現:

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

有關的詳細資訊,`RenderMode`請參閱<xref:blazor/hosting-model-configuration#render-mode>。

## <a name="detect-when-the-app-is-prerendering"></a>偵測應用何時預像

[!INCLUDE[](~/includes/blazor-prerendering.md)]
