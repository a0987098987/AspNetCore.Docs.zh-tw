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
ms.openlocfilehash: df5bb676df59b538179a69978040521c4ee78ed1
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146364"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="a367b-103">ASP.NET Core Blazor 生命週期</span><span class="sxs-lookup"><span data-stu-id="a367b-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="a367b-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a367b-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a367b-105">Blazor 架構包含同步和非同步生命週期方法。</span><span class="sxs-lookup"><span data-stu-id="a367b-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="a367b-106">覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。</span><span class="sxs-lookup"><span data-stu-id="a367b-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="a367b-107">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="a367b-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="a367b-108">元件初始化方法</span><span class="sxs-lookup"><span data-stu-id="a367b-108">Component initialization methods</span></span>

<span data-ttu-id="a367b-109">當元件從其父元件收到其初始參數之後初始化時，就會叫用 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*>。</span><span class="sxs-lookup"><span data-stu-id="a367b-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="a367b-110">當元件執行非同步作業時，請使用 `OnInitializedAsync`，而且應該在作業完成時重新整理。</span><span class="sxs-lookup"><span data-stu-id="a367b-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span> <span data-ttu-id="a367b-111">只有在第一次具現化元件時，才會呼叫這些方法一次。</span><span class="sxs-lookup"><span data-stu-id="a367b-111">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="a367b-112">若為同步作業，請覆寫 `OnInitialized`：</span><span class="sxs-lookup"><span data-stu-id="a367b-112">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="a367b-113">若要執行非同步作業，請覆寫 `OnInitializedAsync`，並在作業上使用 `await` 關鍵字：</span><span class="sxs-lookup"><span data-stu-id="a367b-113">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="a367b-114">設定參數之前</span><span class="sxs-lookup"><span data-stu-id="a367b-114">Before parameters are set</span></span>

<span data-ttu-id="a367b-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> 會在轉譯樹狀結構中設定元件的父系所提供的參數：</span><span class="sxs-lookup"><span data-stu-id="a367b-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="a367b-116"><xref:Microsoft.AspNetCore.Components.ParameterView> 在每次呼叫 `SetParametersAsync` 時都包含整個參數值集。</span><span class="sxs-lookup"><span data-stu-id="a367b-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="a367b-117">`SetParametersAsync` 的預設執行會使用在 `ParameterView`中具有對應值的 `[Parameter]` 或 `[CascadingParameter]` 屬性，來設定每個屬性的值。</span><span class="sxs-lookup"><span data-stu-id="a367b-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="a367b-118">`ParameterView` 中沒有對應值的參數會保持不變。</span><span class="sxs-lookup"><span data-stu-id="a367b-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="a367b-119">如果未叫用 `base.SetParametersAync`，自訂程式碼就可以任何需要的方式解讀傳入的參數值。</span><span class="sxs-lookup"><span data-stu-id="a367b-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="a367b-120">例如，不需要將傳入的參數指派給類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="a367b-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="a367b-121">設定參數之後</span><span class="sxs-lookup"><span data-stu-id="a367b-121">After parameters are set</span></span>

<span data-ttu-id="a367b-122">呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>：</span><span class="sxs-lookup"><span data-stu-id="a367b-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="a367b-123">當元件初始化並從其父元件收到第一組參數時。</span><span class="sxs-lookup"><span data-stu-id="a367b-123">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="a367b-124">當父元件重新呈現和提供時：</span><span class="sxs-lookup"><span data-stu-id="a367b-124">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="a367b-125">只有已知的基本不可變類型，其中至少有一個參數已變更。</span><span class="sxs-lookup"><span data-stu-id="a367b-125">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="a367b-126">任何複雜類型的參數。</span><span class="sxs-lookup"><span data-stu-id="a367b-126">Any complex-typed parameters.</span></span> <span data-ttu-id="a367b-127">架構無法得知複雜型別參數的值是否在內部變動，因此它會將參數集視為已變更。</span><span class="sxs-lookup"><span data-stu-id="a367b-127">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="a367b-128">當套用參數和屬性值時，必須在 `OnParametersSetAsync` 生命週期事件期間進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="a367b-128">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="a367b-129">元件呈現之後</span><span class="sxs-lookup"><span data-stu-id="a367b-129">After component render</span></span>

<span data-ttu-id="a367b-130">在元件完成呈現之後，會呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*>。</span><span class="sxs-lookup"><span data-stu-id="a367b-130"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="a367b-131">此時會填入元素和元件參考。</span><span class="sxs-lookup"><span data-stu-id="a367b-131">Element and component references are populated at this point.</span></span> <span data-ttu-id="a367b-132">使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="a367b-132">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="a367b-133">`OnAfterRenderAsync` 和 `OnAfterRender`的 `firstRender` 參數：</span><span class="sxs-lookup"><span data-stu-id="a367b-133">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="a367b-134">會在第一次呈現元件實例時設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="a367b-134">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="a367b-135">可以用來確保初始化工作只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="a367b-135">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="a367b-136">在 `OnAfterRenderAsync` 生命週期事件期間，必須在轉譯之後立即進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="a367b-136">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="a367b-137">即使您從 `OnAfterRenderAsync`傳回 <xref:System.Threading.Tasks.Task>，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="a367b-137">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="a367b-138">這是為了避免無限的呈現迴圈。</span><span class="sxs-lookup"><span data-stu-id="a367b-138">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="a367b-139">它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="a367b-139">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="a367b-140">在*伺服器上進行預呈現時，不會呼叫*`OnAfterRender` 和 `OnAfterRenderAsync`。</span><span class="sxs-lookup"><span data-stu-id="a367b-140">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="a367b-141">隱藏 UI 重新整理</span><span class="sxs-lookup"><span data-stu-id="a367b-141">Suppress UI refreshing</span></span>

<span data-ttu-id="a367b-142">覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> 以隱藏 UI 重新整理。</span><span class="sxs-lookup"><span data-stu-id="a367b-142">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="a367b-143">如果執行會傳回 `true`，則會重新整理 UI：</span><span class="sxs-lookup"><span data-stu-id="a367b-143">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="a367b-144">每次呈現元件時，都會呼叫 `ShouldRender`。</span><span class="sxs-lookup"><span data-stu-id="a367b-144">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="a367b-145">即使 `ShouldRender` 遭到覆寫，元件一律會一開始呈現。</span><span class="sxs-lookup"><span data-stu-id="a367b-145">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="a367b-146">狀態變更</span><span class="sxs-lookup"><span data-stu-id="a367b-146">State changes</span></span>

<span data-ttu-id="a367b-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> 通知元件其狀態已變更。</span><span class="sxs-lookup"><span data-stu-id="a367b-147"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="a367b-148">當適用時，呼叫 `StateHasChanged` 會導致元件重新顯示。</span><span class="sxs-lookup"><span data-stu-id="a367b-148">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="a367b-149">處理轉譯時的未完成非同步動作</span><span class="sxs-lookup"><span data-stu-id="a367b-149">Handle incomplete async actions at render</span></span>

<span data-ttu-id="a367b-150">在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="a367b-150">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="a367b-151">當生命週期方法正在執行時，物件可能 `null` 或未完全填入資料。</span><span class="sxs-lookup"><span data-stu-id="a367b-151">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="a367b-152">提供轉譯邏輯，以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="a367b-152">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="a367b-153">當物件 `null`時，轉譯預留位置 UI 元素（例如載入訊息）。</span><span class="sxs-lookup"><span data-stu-id="a367b-153">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="a367b-154">在 Blazor 範本的 `FetchData` 元件中，`OnInitializedAsync` 會覆寫為 asychronously 接收預測資料（`forecasts`）。</span><span class="sxs-lookup"><span data-stu-id="a367b-154">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="a367b-155">當 `forecasts` `null`時，就會向使用者顯示載入訊息。</span><span class="sxs-lookup"><span data-stu-id="a367b-155">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="a367b-156">`OnInitializedAsync` 所傳回的 `Task` 完成之後，元件會以更新的狀態重新顯示。</span><span class="sxs-lookup"><span data-stu-id="a367b-156">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="a367b-157">Blazor 伺服器範本中的*Pages/FetchData* ：</span><span class="sxs-lookup"><span data-stu-id="a367b-157">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="a367b-158">使用 IDisposable 的元件處置</span><span class="sxs-lookup"><span data-stu-id="a367b-158">Component disposal with IDisposable</span></span>

<span data-ttu-id="a367b-159">如果元件會執行 <xref:System.IDisposable>，當元件從 UI 中移除時，就會呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="a367b-159">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="a367b-160">下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：</span><span class="sxs-lookup"><span data-stu-id="a367b-160">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="a367b-161">不支援在 `Dispose` 中呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>。</span><span class="sxs-lookup"><span data-stu-id="a367b-161">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="a367b-162">在卸載轉譯器的過程中，可能會叫用 `StateHasChanged`，因此不支援在該時間點要求 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="a367b-162">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="a367b-163">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="a367b-163">Handle errors</span></span>

<span data-ttu-id="a367b-164">如需在生命週期方法執行期間處理錯誤的詳細資訊，請參閱 <xref:blazor/handle-errors#lifecycle-methods>。</span><span class="sxs-lookup"><span data-stu-id="a367b-164">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
