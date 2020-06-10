---
title: ASP.NET Core Blazor 生命週期
author: guardrex
description: 瞭解如何 Razor 在 ASP.NET Core 應用程式中使用元件生命週期方法 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/lifecycle
ms.openlocfilehash: 9dcbb2ca21cc689063198e1ccc90583db4229183
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "83864578"
---
# <a name="aspnet-core-blazor-lifecycle"></a>ASP.NET Core Blazor 生命週期

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

此 Blazor 架構包含同步和非同步生命週期方法。 覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。

## <a name="lifecycle-methods"></a>生命週期方法

### <a name="component-initialization-methods"></a>元件初始化方法

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A>當元件從其父元件接收到其初始參數之後，就會叫用和。 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>當元件執行非同步作業時使用，而且應該在作業完成時重新整理。

針對同步作業，覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A> ：

```csharp
protected override void OnInitialized()
{
    ...
}
```

若要執行非同步作業，請覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 並在作業上使用[await](/dotnet/csharp/language-reference/operators/await)運算子：

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor將[其內容呼叫呈現](xref:blazor/hosting-model-configuration#render-mode) <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> **_兩次_** 的伺服器應用程式：

* 當元件一開始以靜態方式轉譯為頁面的一部分時。
* 第二次當瀏覽器建立與伺服器的連接時。

若要防止中的開發人員程式碼執行 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 兩次，請參閱在預做[後](#stateful-reconnection-after-prerendering)重新設定狀態一節。

在預先 Blazor 處理伺服器應用程式時，因為尚未建立與瀏覽器的連接，所以無法執行某些動作（例如呼叫 JavaScript）。 元件可能需要在資源清單時以不同的方式呈現。 如需詳細資訊，請參閱偵測[應用程式何時進行預呈現](#detect-when-the-app-is-prerendering)一節。

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

<xref:Microsoft.AspNetCore.Components.ParameterView>每次呼叫時，包含一組完整的參數值 <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> 。

的預設執行 <xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> [`[Parameter]`](xref:Microsoft.AspNetCore.Components.ParameterAttribute) [`[CascadingParameter]`](xref:Microsoft.AspNetCore.Components.CascadingParameterAttribute) 會使用在中具有對應值的或屬性，來設定每個屬性的值 <xref:Microsoft.AspNetCore.Components.ParameterView> 。 在中沒有對應值的參數 <xref:Microsoft.AspNetCore.Components.ParameterView> 會保持不變。

如果[基底。](xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A)不會叫用 SetParametersAync，自訂程式碼可以用任何需要的方式解讀傳入的參數值。 例如，不需要將傳入的參數指派給類別的屬性。

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="after-parameters-are-set"></a>設定參數之後

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet%2A>系統會呼叫和：

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
> 當套用參數和屬性值時，必須在生命週期事件期間進行非同步工作 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> 。

```csharp
protected override void OnParametersSet()
{
    ...
}
```

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="after-component-render"></a>元件呈現之後

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A>在 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 元件完成呈現之後，會呼叫和。 此時會填入元素和元件參考。 使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。

`firstRender`和的參數 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> ：

* 會 `true` 在第一次呈現元件實例時設定為。
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
> 在生命週期事件期間，必須立即執行非同步工作 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> 。
>
> 即使您 <xref:System.Threading.Tasks.Task> 從傳回 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> ，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。 這是為了避免無限的呈現迴圈。 它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A>在 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> *伺服器上進行預呈現時，不會呼叫和。*

如果已設定任何事件處理常式，請將它們解除鎖定以供處置。 如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。

### <a name="suppress-ui-refreshing"></a>隱藏 UI 重新整理

覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 以隱藏 UI 重新整理。 如果執行傳回 `true` ，則會重新整理 UI：

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>每次呈現元件時，都會呼叫。

即使 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 已覆寫，元件一律會一開始呈現。

如需詳細資訊，請參閱 <xref:performance/blazor/webassembly-best-practices#avoid-unnecessary-component-renders> 。

## <a name="state-changes"></a>狀態變更

<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>通知元件其狀態已變更。 當適用時，呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> 會導致元件重新顯示。

## <a name="handle-incomplete-async-actions-at-render"></a>處理轉譯時的未完成非同步動作

在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。 `null`當生命週期方法正在執行時，物件可能會或未完全填入資料。 提供轉譯邏輯，以確認物件已初始化。 當物件為時，呈現預留位置 UI 專案（例如，載入訊息） `null` 。

在 `FetchData` 範本的元件中 Blazor ， <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 會覆寫為 asychronously 接收預測資料（ `forecasts` ）。 當 `forecasts` 為時 `null` ，會向使用者顯示載入訊息。 在所 `Task` 傳回的 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 完成之後，元件會以更新的狀態重新顯示。

伺服器範本中的*Pages/FetchData* Blazor ：

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>使用 IDisposable 的元件處置

如果元件會執行 <xref:System.IDisposable> ，則會在從 UI 中移除元件時呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。 下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：

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
> <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>不支援在中呼叫 `Dispose` 。 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>可能會在卸載轉譯器的過程中叫用，因此不支援在該時間點要求 UI 更新。

取消訂閱來自 .NET 事件的事件處理常式。 下列[ Blazor 表單](xref:blazor/forms-validation)範例示範如何在方法中解除掛接事件處理常式 `Dispose` ：

* 私用欄位和 lambda 方法

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* 私用方法方法

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a>處理錯誤

如需在生命週期方法執行期間處理錯誤的詳細資訊，請參閱 <xref:blazor/handle-errors#lifecycle-methods> 。

## <a name="stateful-reconnection-after-prerendering"></a>預呈現後的具狀態重新連接

在 Blazor 伺服器應用程式中 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> ，當為時 <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> ，元件一開始會以靜態方式呈現為頁面的一部分。 當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。 如果存在用於初始化元件的[OnInitialized {Async}](#component-initialization-methods)生命週期方法，則會執行*兩次*方法：

* 當元件以靜態方式資源清單時。
* 建立伺服器連接之後。

這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。

若要避免伺服器應用程式中的雙呈現案例 Blazor ：

* 傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。
* 在預入期間使用識別碼來儲存元件狀態。
* 在可呈現後使用識別碼，以取得快取的狀態。

下列程式碼示範 `WeatherForecastService` 在以範本為基礎的 Blazor 伺服器應用程式中已更新，可避免雙重呈現：

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

如需的詳細資訊 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper.RenderMode> ，請參閱 <xref:blazor/hosting-model-configuration#render-mode> 。

## <a name="detect-when-the-app-is-prerendering"></a>偵測應用程式何時已進行預呈現

[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="cancelable-background-work"></a>可取消的背景工作

元件通常會執行長時間執行的背景工作，例如進行網路呼叫（ <xref:System.Net.Http.HttpClient> ）並與資料庫互動。 在幾種情況下，最好停止背景工作以節省系統資源。 例如，當使用者離開元件時，背景非同步作業不會自動停止。

背景工作專案可能需要取消的其他原因包括：

* 執行中的背景工作使用了錯誤的輸入資料或處理參數來啟動。
* 目前執行中的背景工作專案集必須以一組新的工作專案取代。
* 必須變更目前正在執行之工作的優先順序。
* 必須關閉應用程式，才能將它重新部署到伺服器。
* 伺服器資源會受到限制，請強制背景工作專案的重新排定。

若要在元件中執行可取消的背景工作模式：

* 使用 <xref:System.Threading.CancellationTokenSource> 和 <xref:System.Threading.CancellationToken> 。
* 在[元件的處置](#component-disposal-with-idisposable)和任何點上，手動解除標記時，請呼叫[CancellationTokenSource](xref:System.Threading.CancellationTokenSource.Cancel%2A) ，以表示應該取消背景工作。
* 在非同步呼叫傳回之後，呼叫 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> 權杖上的。

在下例中︰

* `await Task.Delay(5000, cts.Token);`代表長時間執行的非同步背景工作。
* `BackgroundResourceMethod`代表長時間執行的背景方法，如果在 `Resource` 呼叫方法之前處置，則不應該啟動。

```razor
@implements IDisposable
@using System.Threading

<button @onclick="LongRunningWork">Trigger long running work</button>

@code {
    private Resource resource = new Resource();
    private CancellationTokenSource cts = new CancellationTokenSource();

    protected async Task LongRunningWork()
    {
        await Task.Delay(5000, cts.Token);

        cts.Token.ThrowIfCancellationRequested();
        resource.BackgroundResourceMethod();
    }

    public void Dispose()
    {
        cts.Cancel();
        cts.Dispose();
        resource.Dispose();
    }

    private class Resource : IDisposable
    {
        private bool disposed;

        public void BackgroundResourceMethod()
        {
            if (disposed)
            {
                throw new ObjectDisposedException(nameof(Resource));
            }
            
            ...
        }
        
        public void Dispose()
        {
            disposed = true;
        }
    }
}
```
