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
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="eb0d9-103">ASP.NET Core Blazor 生命週期</span><span class="sxs-lookup"><span data-stu-id="eb0d9-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="eb0d9-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="eb0d9-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="eb0d9-105">Blazor 架構包含同步和非同步生命週期方法。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="eb0d9-106">覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="eb0d9-107">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="eb0d9-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="eb0d9-108">元件初始化方法</span><span class="sxs-lookup"><span data-stu-id="eb0d9-108">Component initialization methods</span></span>

<span data-ttu-id="eb0d9-109">當元件從其父元件收到其初始參數之後初始化時，就會叫用 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*>。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="eb0d9-110">當元件執行非同步作業時，請使用 `OnInitializedAsync`，而且應該在作業完成時重新整理。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="eb0d9-111">若為同步作業，請覆寫 `OnInitialized`：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="eb0d9-112">若要執行非同步作業，請覆寫 `OnInitializedAsync`，並在作業上使用 `await` 關鍵字：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="eb0d9-113"> 伺服器應用程式可將[其內容呼叫呈現](xref:blazor/hosting-model-configuration#render-mode)`OnInitializedAsync` **_兩次_** ：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="eb0d9-114">當元件一開始以靜態方式轉譯為頁面的一部分時。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="eb0d9-115">第二次當瀏覽器建立與伺服器的連接時。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="eb0d9-116">若要防止 `OnInitializedAsync` 中的開發人員程式碼執行兩次，請參閱 [已自[呈現後](#stateful-reconnection-after-prerendering)重新連線] 區段。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="eb0d9-117">當 Blazor 伺服器應用程式進行預先處理時，某些動作（例如呼叫 JavaScript）並不可能，因為尚未建立與瀏覽器的連接。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="eb0d9-118">元件可能需要在資源清單時以不同的方式呈現。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="eb0d9-119">如需詳細資訊，請參閱偵測[應用程式何時進行預呈現](#detect-when-the-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="eb0d9-120">設定參數之前</span><span class="sxs-lookup"><span data-stu-id="eb0d9-120">Before parameters are set</span></span>

<span data-ttu-id="eb0d9-121"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> 會在轉譯樹狀結構中設定元件的父系所提供的參數：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-121"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="eb0d9-122"><xref:Microsoft.AspNetCore.Components.ParameterView> 在每次呼叫 `SetParametersAsync` 時都包含整個參數值集。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-122"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="eb0d9-123">`SetParametersAsync` 的預設執行會使用在 `ParameterView`中具有對應值的 `[Parameter]` 或 `[CascadingParameter]` 屬性，來設定每個屬性的值。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-123">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="eb0d9-124">`ParameterView` 中沒有對應值的參數會保持不變。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-124">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="eb0d9-125">如果未叫用 `base.SetParametersAync`，自訂程式碼就可以任何需要的方式解讀傳入的參數值。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-125">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="eb0d9-126">例如，不需要將傳入的參數指派給類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-126">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="eb0d9-127">設定參數之後</span><span class="sxs-lookup"><span data-stu-id="eb0d9-127">After parameters are set</span></span>

<span data-ttu-id="eb0d9-128">呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-128"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="eb0d9-129">當元件初始化並從其父元件收到第一組參數時。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-129">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="eb0d9-130">當父元件重新呈現和提供時：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-130">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="eb0d9-131">只有已知的基本不可變類型，其中至少有一個參數已變更。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-131">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="eb0d9-132">任何複雜類型的參數。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-132">Any complex-typed parameters.</span></span> <span data-ttu-id="eb0d9-133">架構無法得知複雜型別參數的值是否在內部變動，因此它會將參數集視為已變更。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-133">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="eb0d9-134">當套用參數和屬性值時，必須在 `OnParametersSetAsync` 生命週期事件期間進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-134">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="eb0d9-135">元件呈現之後</span><span class="sxs-lookup"><span data-stu-id="eb0d9-135">After component render</span></span>

<span data-ttu-id="eb0d9-136">在元件完成呈現之後，會呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*>。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-136"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="eb0d9-137">此時會填入元素和元件參考。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-137">Element and component references are populated at this point.</span></span> <span data-ttu-id="eb0d9-138">使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-138">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="eb0d9-139">`OnAfterRenderAsync` 和 `OnAfterRender`的 `firstRender` 參數：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-139">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="eb0d9-140">會在第一次呈現元件實例時設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-140">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="eb0d9-141">可以用來確保初始化工作只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-141">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="eb0d9-142">在 `OnAfterRenderAsync` 生命週期事件期間，必須在轉譯之後立即進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-142">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="eb0d9-143">即使您從 `OnAfterRenderAsync`傳回 <xref:System.Threading.Tasks.Task>，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-143">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="eb0d9-144">這是為了避免無限的呈現迴圈。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-144">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="eb0d9-145">它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-145">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="eb0d9-146">在*伺服器上進行預呈現時，不會呼叫*`OnAfterRender` 和 `OnAfterRenderAsync`。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-146">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="eb0d9-147">隱藏 UI 重新整理</span><span class="sxs-lookup"><span data-stu-id="eb0d9-147">Suppress UI refreshing</span></span>

<span data-ttu-id="eb0d9-148">覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> 以隱藏 UI 重新整理。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-148">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="eb0d9-149">如果執行會傳回 `true`，則會重新整理 UI：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-149">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="eb0d9-150">每次呈現元件時，都會呼叫 `ShouldRender`。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-150">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="eb0d9-151">即使 `ShouldRender` 遭到覆寫，元件一律會一開始呈現。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-151">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="eb0d9-152">狀態變更</span><span class="sxs-lookup"><span data-stu-id="eb0d9-152">State changes</span></span>

<span data-ttu-id="eb0d9-153"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> 通知元件其狀態已變更。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-153"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="eb0d9-154">當適用時，呼叫 `StateHasChanged` 會導致元件重新顯示。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-154">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="eb0d9-155">處理轉譯時的未完成非同步動作</span><span class="sxs-lookup"><span data-stu-id="eb0d9-155">Handle incomplete async actions at render</span></span>

<span data-ttu-id="eb0d9-156">在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-156">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="eb0d9-157">當生命週期方法正在執行時，物件可能 `null` 或未完全填入資料。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-157">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="eb0d9-158">提供轉譯邏輯，以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-158">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="eb0d9-159">當物件 `null`時，轉譯預留位置 UI 元素（例如載入訊息）。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-159">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="eb0d9-160">在 Blazor 範本的 `FetchData` 元件中，`OnInitializedAsync` 會覆寫為 asychronously 接收預測資料（`forecasts`）。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-160">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="eb0d9-161">當 `forecasts` `null`時，就會向使用者顯示載入訊息。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-161">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="eb0d9-162">`OnInitializedAsync` 所傳回的 `Task` 完成之後，元件會以更新的狀態重新顯示。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-162">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="eb0d9-163">Blazor 伺服器範本中的*Pages/FetchData* ：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-163">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="eb0d9-164">使用 IDisposable 的元件處置</span><span class="sxs-lookup"><span data-stu-id="eb0d9-164">Component disposal with IDisposable</span></span>

<span data-ttu-id="eb0d9-165">如果元件會執行 <xref:System.IDisposable>，當元件從 UI 中移除時，就會呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-165">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="eb0d9-166">下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-166">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="eb0d9-167">不支援在 `Dispose` 中呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-167">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="eb0d9-168">在卸載轉譯器的過程中，可能會叫用 `StateHasChanged`，因此不支援在該時間點要求 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-168">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="eb0d9-169">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="eb0d9-169">Handle errors</span></span>

<span data-ttu-id="eb0d9-170">如需在生命週期方法執行期間處理錯誤的詳細資訊，請參閱 <xref:blazor/handle-errors#lifecycle-methods>。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-170">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="eb0d9-171">預呈現後的具狀態重新連接</span><span class="sxs-lookup"><span data-stu-id="eb0d9-171">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="eb0d9-172">在 Blazor 伺服器應用程式中，當 `RenderMode` `ServerPrerendered`時，元件一開始會以靜態方式轉譯為頁面的一部分。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-172">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="eb0d9-173">當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-173">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="eb0d9-174">如果存在用於初始化元件的[OnInitialized {Async}](xref:blazor/lifecycle#component-initialization-methods)生命週期方法，則會執行*兩次*方法：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-174">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="eb0d9-175">當元件以靜態方式資源清單時。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-175">When the component is prerendered statically.</span></span>
* <span data-ttu-id="eb0d9-176">建立伺服器連接之後。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-176">After the server connection has been established.</span></span>

<span data-ttu-id="eb0d9-177">這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-177">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="eb0d9-178">若要避免 Blazor 伺服器應用程式中的雙呈現案例：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-178">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="eb0d9-179">傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-179">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="eb0d9-180">在預入期間使用識別碼來儲存元件狀態。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-180">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="eb0d9-181">在可呈現後使用識別碼，以取得快取的狀態。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-181">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="eb0d9-182">下列程式碼示範以範本為基礎 Blazor 伺服器應用程式中，可避免雙重呈現的更新 `WeatherForecastService`：</span><span class="sxs-lookup"><span data-stu-id="eb0d9-182">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

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

<span data-ttu-id="eb0d9-183">如需 `RenderMode`的詳細資訊，請參閱 <xref:blazor/hosting-model-configuration#render-mode>。</span><span class="sxs-lookup"><span data-stu-id="eb0d9-183">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="eb0d9-184">偵測應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="eb0d9-184">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]
