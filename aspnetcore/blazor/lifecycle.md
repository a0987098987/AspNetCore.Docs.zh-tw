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
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="f84e8-103">ASP.NET核心Blazor生命週期</span><span class="sxs-lookup"><span data-stu-id="f84e8-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="f84e8-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f84e8-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="f84e8-105">該Blazor框架包括同步和異步生命週期方法。</span><span class="sxs-lookup"><span data-stu-id="f84e8-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="f84e8-106">重寫生命週期方法,在元件初始化和呈現期間對元件執行其他操作。</span><span class="sxs-lookup"><span data-stu-id="f84e8-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="f84e8-107">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="f84e8-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="f84e8-108">元件初始化方法</span><span class="sxs-lookup"><span data-stu-id="f84e8-108">Component initialization methods</span></span>

<span data-ttu-id="f84e8-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*>並在<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*>從其父元件收到其初始參數后初始化元件時調用。</span><span class="sxs-lookup"><span data-stu-id="f84e8-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="f84e8-110">當`OnInitializedAsync`元件執行非同步操作並在操作完成後刷新時使用。</span><span class="sxs-lookup"><span data-stu-id="f84e8-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="f84e8-111">對同步操作,重寫`OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="f84e8-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="f84e8-112">要執行非同步操作,在操作`OnInitializedAsync`上重寫並使用`await`關鍵字:</span><span class="sxs-lookup"><span data-stu-id="f84e8-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="f84e8-113">[已預先呈現內容](xref:blazor/hosting-model-configuration#render-mode)的伺服器套用`OnInitializedAsync`**_兩次_**:</span><span class="sxs-lookup"><span data-stu-id="f84e8-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="f84e8-114">當元件最初作為頁面的一部分以靜態方式呈現時。</span><span class="sxs-lookup"><span data-stu-id="f84e8-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="f84e8-115">第二次瀏覽器建立與伺服器的連接。</span><span class="sxs-lookup"><span data-stu-id="f84e8-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="f84e8-116">要防止開發人員代碼在`OnInitializedAsync`中運行兩次,請參閱[預渲染后的狀態重新連接](#stateful-reconnection-after-prerendering)部分。</span><span class="sxs-lookup"><span data-stu-id="f84e8-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="f84e8-117">當Blazor伺服器應用處於預渲染狀態時,某些操作(如調用 JAvaScript)是不可能的,因為尚未與瀏覽器建立連接。</span><span class="sxs-lookup"><span data-stu-id="f84e8-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="f84e8-118">元件在預渲染時可能需要以不同的方式呈現。</span><span class="sxs-lookup"><span data-stu-id="f84e8-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="f84e8-119">有關詳細資訊,請參閱[「檢測應用何時是預渲染」](#detect-when-the-app-is-prerendering)部分。</span><span class="sxs-lookup"><span data-stu-id="f84e8-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

<span data-ttu-id="f84e8-120">如果設置了任何事件處理程式,則取消處理它們。</span><span class="sxs-lookup"><span data-stu-id="f84e8-120">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f84e8-121">有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。</span><span class="sxs-lookup"><span data-stu-id="f84e8-121">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="f84e8-122">在設定參數之前</span><span class="sxs-lookup"><span data-stu-id="f84e8-122">Before parameters are set</span></span>

<span data-ttu-id="f84e8-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*>設定元件的父級在呈現樹中提供的參數:</span><span class="sxs-lookup"><span data-stu-id="f84e8-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="f84e8-124"><xref:Microsoft.AspNetCore.Components.ParameterView>每次`SetParametersAsync`調用都包含整個參數值集。</span><span class="sxs-lookup"><span data-stu-id="f84e8-124"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="f84e8-125">的`SetParametersAsync`預設設定每個屬性的值,`[Parameter]`其`[CascadingParameter]`或 屬性在中具有`ParameterView`相應的值。</span><span class="sxs-lookup"><span data-stu-id="f84e8-125">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="f84e8-126">中沒有相應值`ParameterView`的參數保持不變。</span><span class="sxs-lookup"><span data-stu-id="f84e8-126">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="f84e8-127">如果未`base.SetParametersAync`調用,自定義代碼可以以所需的任何方式解釋傳入參數值。</span><span class="sxs-lookup"><span data-stu-id="f84e8-127">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="f84e8-128">例如,不需要將傳入參數分配給類上的屬性。</span><span class="sxs-lookup"><span data-stu-id="f84e8-128">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

<span data-ttu-id="f84e8-129">如果設置了任何事件處理程式,則取消處理它們。</span><span class="sxs-lookup"><span data-stu-id="f84e8-129">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f84e8-130">有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。</span><span class="sxs-lookup"><span data-stu-id="f84e8-130">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="f84e8-131">設定參數後</span><span class="sxs-lookup"><span data-stu-id="f84e8-131">After parameters are set</span></span>

<span data-ttu-id="f84e8-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*>並<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>稱為:</span><span class="sxs-lookup"><span data-stu-id="f84e8-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called:</span></span>

* <span data-ttu-id="f84e8-133">當元件初始化並已收到其第一組參數時,</span><span class="sxs-lookup"><span data-stu-id="f84e8-133">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="f84e8-134">當父元件重新呈現與提供時:</span><span class="sxs-lookup"><span data-stu-id="f84e8-134">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="f84e8-135">只有已知的原始不可變類型,其中至少有一個參數已更改。</span><span class="sxs-lookup"><span data-stu-id="f84e8-135">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="f84e8-136">任何複雜類型的參數。</span><span class="sxs-lookup"><span data-stu-id="f84e8-136">Any complex-typed parameters.</span></span> <span data-ttu-id="f84e8-137">框架無法知道複雜類型參數的值是否在內部發生突變,因此將參數集視為已更改。</span><span class="sxs-lookup"><span data-stu-id="f84e8-137">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="f84e8-138">在`OnParametersSetAsync`生命週期事件期間,應用參數和屬性值時必須發生非同步工作。</span><span class="sxs-lookup"><span data-stu-id="f84e8-138">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="f84e8-139">如果設置了任何事件處理程式,則取消處理它們。</span><span class="sxs-lookup"><span data-stu-id="f84e8-139">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f84e8-140">有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。</span><span class="sxs-lookup"><span data-stu-id="f84e8-140">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-component-render"></a><span data-ttu-id="f84e8-141">元件成像後</span><span class="sxs-lookup"><span data-stu-id="f84e8-141">After component render</span></span>

<span data-ttu-id="f84e8-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*>並在<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*>元件完成呈現後調用。</span><span class="sxs-lookup"><span data-stu-id="f84e8-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="f84e8-143">此時將填充元素和元件引用。</span><span class="sxs-lookup"><span data-stu-id="f84e8-143">Element and component references are populated at this point.</span></span> <span data-ttu-id="f84e8-144">使用此階段可以使用呈現的內容執行其他初始化步驟,例如啟動對呈現 DOM 元素運行的第三方 JavaScript 庫。</span><span class="sxs-lookup"><span data-stu-id="f84e8-144">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="f84e8-145">與`firstRender``OnAfterRender`的`OnAfterRenderAsync`參數 。</span><span class="sxs-lookup"><span data-stu-id="f84e8-145">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="f84e8-146">設置為`true`首次呈現元件實例。</span><span class="sxs-lookup"><span data-stu-id="f84e8-146">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="f84e8-147">可用於確保初始化工作僅執行一次。</span><span class="sxs-lookup"><span data-stu-id="f84e8-147">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="f84e8-148">在`OnAfterRenderAsync`生命週期事件期間,渲染后立即進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="f84e8-148">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="f84e8-149">即使返回<xref:System.Threading.Tasks.Task>`OnAfterRenderAsync`from ,框架也不會在任務完成後為元件安排進一步的呈現週期。</span><span class="sxs-lookup"><span data-stu-id="f84e8-149">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="f84e8-150">這是為了避免無限渲染迴圈。</span><span class="sxs-lookup"><span data-stu-id="f84e8-150">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="f84e8-151">它不同於其他生命週期方法,後者計劃返回的任務完成後再進行一個渲染週期。</span><span class="sxs-lookup"><span data-stu-id="f84e8-151">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="f84e8-152">`OnAfterRender`在`OnAfterRenderAsync`*伺服器上預渲染時不調用。*</span><span class="sxs-lookup"><span data-stu-id="f84e8-152">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

<span data-ttu-id="f84e8-153">如果設置了任何事件處理程式,則取消處理它們。</span><span class="sxs-lookup"><span data-stu-id="f84e8-153">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="f84e8-154">有關詳細資訊,請參閱[IAA 的元件處置](#component-disposal-with-idisposable)部分。</span><span class="sxs-lookup"><span data-stu-id="f84e8-154">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="f84e8-155">禁止 UI 刷新</span><span class="sxs-lookup"><span data-stu-id="f84e8-155">Suppress UI refreshing</span></span>

<span data-ttu-id="f84e8-156">覆蓋<xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*>以禁止 UI 刷新。</span><span class="sxs-lookup"><span data-stu-id="f84e8-156">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="f84e8-157">如果實現返回`true`,則刷新 UI:</span><span class="sxs-lookup"><span data-stu-id="f84e8-157">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="f84e8-158">`ShouldRender`每次呈現元件時都會調用。</span><span class="sxs-lookup"><span data-stu-id="f84e8-158">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="f84e8-159">即使`ShouldRender`被重寫,元件也始終被呈現。</span><span class="sxs-lookup"><span data-stu-id="f84e8-159">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="f84e8-160">狀態變更</span><span class="sxs-lookup"><span data-stu-id="f84e8-160">State changes</span></span>

<span data-ttu-id="f84e8-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>通知元件其狀態已更改。</span><span class="sxs-lookup"><span data-stu-id="f84e8-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="f84e8-162">如果適用,調用`StateHasChanged`會導致重新呈現元件。</span><span class="sxs-lookup"><span data-stu-id="f84e8-162">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="f84e8-163">在渲染時處理不完整的非同步操作</span><span class="sxs-lookup"><span data-stu-id="f84e8-163">Handle incomplete async actions at render</span></span>

<span data-ttu-id="f84e8-164">在呈現元件之前,在生命週期事件中執行的異步操作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="f84e8-164">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="f84e8-165">在執行生命週期方法`null`時,物件可能已或不完全填充了數據。</span><span class="sxs-lookup"><span data-stu-id="f84e8-165">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="f84e8-166">提供呈現邏輯以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="f84e8-166">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="f84e8-167">成像佔位元 UI 元素(例如,載入訊息`null`),而物件為 。</span><span class="sxs-lookup"><span data-stu-id="f84e8-167">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="f84e8-168">在`FetchData`樣本的元件中Blazor,`OnInitializedAsync`被重寫為以動態接收預測資料 ()。`forecasts`</span><span class="sxs-lookup"><span data-stu-id="f84e8-168">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="f84e8-169">當`forecasts``null`為 時,將向用戶顯示載入消息。</span><span class="sxs-lookup"><span data-stu-id="f84e8-169">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="f84e8-170">完成`Task`返回`OnInitializedAsync`後,元件將重新呈現為更新狀態。</span><span class="sxs-lookup"><span data-stu-id="f84e8-170">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="f84e8-171">Blazor伺服器樣本中的*頁面/FetchData.razor:*</span><span class="sxs-lookup"><span data-stu-id="f84e8-171">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="f84e8-172">使用 I 一次性工具處理元件</span><span class="sxs-lookup"><span data-stu-id="f84e8-172">Component disposal with IDisposable</span></span>

<span data-ttu-id="f84e8-173">如果元件實現<xref:System.IDisposable>,則當從 UI 中移除元件時,將呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="f84e8-173">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="f84e8-174">以下元件使用`@implements IDisposable`與方法`Dispose`:</span><span class="sxs-lookup"><span data-stu-id="f84e8-174">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="f84e8-175">不支援<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>呼`Dispose`叫 。</span><span class="sxs-lookup"><span data-stu-id="f84e8-175">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="f84e8-176">`StateHasChanged`可能會作為拆解呈現器的一部分調用,因此不支援此時請求 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="f84e8-176">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

<span data-ttu-id="f84e8-177">從 .NET 事件取消訂閱事件處理程式。</span><span class="sxs-lookup"><span data-stu-id="f84e8-177">Unsubscribe event handlers from .NET events.</span></span> <span data-ttu-id="f84e8-178">以下[Blazor表單](xref:blazor/forms-validation)範例展示`Dispose`如何在方法中取消掛鉤事件處理程式:</span><span class="sxs-lookup"><span data-stu-id="f84e8-178">The following [Blazor form](xref:blazor/forms-validation) examples show how to unhook an event handler in the `Dispose` method:</span></span>

* <span data-ttu-id="f84e8-179">私人領域和 lambda 方法</span><span class="sxs-lookup"><span data-stu-id="f84e8-179">Private field and lambda approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* <span data-ttu-id="f84e8-180">私有方法方法</span><span class="sxs-lookup"><span data-stu-id="f84e8-180">Private method approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a><span data-ttu-id="f84e8-181">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="f84e8-181">Handle errors</span></span>

<span data-ttu-id="f84e8-182">有關在生命週期方法執行期間處理錯誤的資訊,請參<xref:blazor/handle-errors#lifecycle-methods>閱 。</span><span class="sxs-lookup"><span data-stu-id="f84e8-182">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="f84e8-183">預先成圖狀態重新連線</span><span class="sxs-lookup"><span data-stu-id="f84e8-183">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="f84e8-184">在BlazorServer`RenderMode`應用`ServerPrerendered`中, 當 是 時,元件最初作為頁面的一部分以靜態方式呈現。</span><span class="sxs-lookup"><span data-stu-id="f84e8-184">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="f84e8-185">一旦瀏覽器建立回伺服器的連接,元件*將再次*呈現,並且元件現在具有交互性。</span><span class="sxs-lookup"><span data-stu-id="f84e8-185">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="f84e8-186">如果存在用於初始化元件的[On 初始化 {Async}](#component-initialization-methods)生命週期方法,則執行該方法*兩次*:</span><span class="sxs-lookup"><span data-stu-id="f84e8-186">If the [OnInitialized{Async}](#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="f84e8-187">靜態預呈現元件時。</span><span class="sxs-lookup"><span data-stu-id="f84e8-187">When the component is prerendered statically.</span></span>
* <span data-ttu-id="f84e8-188">建立伺服器連接後。</span><span class="sxs-lookup"><span data-stu-id="f84e8-188">After the server connection has been established.</span></span>

<span data-ttu-id="f84e8-189">當最終呈現元件時,這可能導致UI中顯示的數據發生顯著變化。</span><span class="sxs-lookup"><span data-stu-id="f84e8-189">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="f84e8-190">要避免Blazor伺服器應用中的雙重呈現方案,請進行以下檢查:</span><span class="sxs-lookup"><span data-stu-id="f84e8-190">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="f84e8-191">傳遞可用於在預渲染期間緩存狀態並在應用重新啟動后檢索狀態的標識符。</span><span class="sxs-lookup"><span data-stu-id="f84e8-191">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="f84e8-192">在預渲染期間使用標識符保存元件狀態。</span><span class="sxs-lookup"><span data-stu-id="f84e8-192">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="f84e8-193">在預渲染后使用標識符來檢索緩存的狀態。</span><span class="sxs-lookup"><span data-stu-id="f84e8-193">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="f84e8-194">以下代碼演示了基於`WeatherForecastService`Blazor樣本的 Server 應用中的更新,該應用避免了雙重呈現:</span><span class="sxs-lookup"><span data-stu-id="f84e8-194">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

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

<span data-ttu-id="f84e8-195">有關的詳細資訊,`RenderMode`請參閱<xref:blazor/hosting-model-configuration#render-mode>。</span><span class="sxs-lookup"><span data-stu-id="f84e8-195">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="f84e8-196">偵測應用何時預像</span><span class="sxs-lookup"><span data-stu-id="f84e8-196">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]
