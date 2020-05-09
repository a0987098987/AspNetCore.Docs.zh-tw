---
title: ASP.NET Core Blazor生命週期
author: guardrex
description: 瞭解如何在 ASP.NET Core Razor Blazor應用程式中使用元件生命週期方法。
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
ms.openlocfilehash: 81699158a161d0e9c9621235840979ebcd634a7e
ms.sourcegitcommit: 363e3a2a035f4082cb92e7b75ed150ba304258b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82976697"
---
# <a name="aspnet-core-blazor-lifecycle"></a><span data-ttu-id="10a23-103">ASP.NET Core Blazor生命週期</span><span class="sxs-lookup"><span data-stu-id="10a23-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="10a23-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="10a23-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="10a23-105">此Blazor架構包含同步和非同步生命週期方法。</span><span class="sxs-lookup"><span data-stu-id="10a23-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="10a23-106">覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。</span><span class="sxs-lookup"><span data-stu-id="10a23-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="10a23-107">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="10a23-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="10a23-108">元件初始化方法</span><span class="sxs-lookup"><span data-stu-id="10a23-108">Component initialization methods</span></span>

<span data-ttu-id="10a23-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>當<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A>元件從其父元件接收到其初始參數之後，就會叫用和。</span><span class="sxs-lookup"><span data-stu-id="10a23-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A> are invoked when the component is initialized after having received its initial parameters from its parent component.</span></span> <span data-ttu-id="10a23-110">當`OnInitializedAsync`元件執行非同步作業時使用，而且應該在作業完成時重新整理。</span><span class="sxs-lookup"><span data-stu-id="10a23-110">Use `OnInitializedAsync` when the component performs an asynchronous operation and should refresh when the operation is completed.</span></span>

<span data-ttu-id="10a23-111">針對同步作業，覆寫`OnInitialized`：</span><span class="sxs-lookup"><span data-stu-id="10a23-111">For a synchronous operation, override `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

<span data-ttu-id="10a23-112">若要執行非同步作業，請`OnInitializedAsync`覆寫並`await`在作業上使用關鍵字：</span><span class="sxs-lookup"><span data-stu-id="10a23-112">To perform an asynchronous operation, override `OnInitializedAsync` and use the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

Blazor<span data-ttu-id="10a23-113">將[其內容](xref:blazor/hosting-model-configuration#render-mode)呼叫`OnInitializedAsync`呈現**_兩次_** 的伺服器應用程式：</span><span class="sxs-lookup"><span data-stu-id="10a23-113"> Server apps that [prerender their content](xref:blazor/hosting-model-configuration#render-mode) call `OnInitializedAsync` **_twice_**:</span></span>

* <span data-ttu-id="10a23-114">當元件一開始以靜態方式轉譯為頁面的一部分時。</span><span class="sxs-lookup"><span data-stu-id="10a23-114">Once when the component is initially rendered statically as part of the page.</span></span>
* <span data-ttu-id="10a23-115">第二次當瀏覽器建立與伺服器的連接時。</span><span class="sxs-lookup"><span data-stu-id="10a23-115">A second time when the browser establishes a connection back to the server.</span></span>

<span data-ttu-id="10a23-116">若要防止中`OnInitializedAsync`的開發人員程式碼執行兩次，請參閱在預做後重新設定[狀態](#stateful-reconnection-after-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="10a23-116">To prevent developer code in `OnInitializedAsync` from running twice, see the [Stateful reconnection after prerendering](#stateful-reconnection-after-prerendering) section.</span></span>

<span data-ttu-id="10a23-117">在預先Blazor處理伺服器應用程式時，因為尚未建立與瀏覽器的連接，所以無法執行某些動作（例如呼叫 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="10a23-117">While a Blazor Server app is prerendering, certain actions, such as calling into JavaScript, aren't possible because a connection with the browser hasn't been established.</span></span> <span data-ttu-id="10a23-118">元件可能需要在資源清單時以不同的方式呈現。</span><span class="sxs-lookup"><span data-stu-id="10a23-118">Components may need to render differently when prerendered.</span></span> <span data-ttu-id="10a23-119">如需詳細資訊，請參閱偵測[應用程式何時進行預呈現](#detect-when-the-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="10a23-119">For more information, see the [Detect when the app is prerendering](#detect-when-the-app-is-prerendering) section.</span></span>

<span data-ttu-id="10a23-120">如果已設定任何事件處理常式，請將它們解除鎖定以供處置。</span><span class="sxs-lookup"><span data-stu-id="10a23-120">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="10a23-121">如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。</span><span class="sxs-lookup"><span data-stu-id="10a23-121">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="before-parameters-are-set"></a><span data-ttu-id="10a23-122">設定參數之前</span><span class="sxs-lookup"><span data-stu-id="10a23-122">Before parameters are set</span></span>

<span data-ttu-id="10a23-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A>在轉譯樹狀結構中設定元件的父系所提供的參數：</span><span class="sxs-lookup"><span data-stu-id="10a23-123"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync%2A> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="10a23-124"><xref:Microsoft.AspNetCore.Components.ParameterView>每次`SetParametersAsync`呼叫時，包含一組完整的參數值。</span><span class="sxs-lookup"><span data-stu-id="10a23-124"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="10a23-125">的預設執行會`SetParametersAsync`使用在中具有對應值的`[Parameter]`或`[CascadingParameter]`屬性，來設定每個屬性的值`ParameterView`。</span><span class="sxs-lookup"><span data-stu-id="10a23-125">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="10a23-126">在中`ParameterView`沒有對應值的參數會保持不變。</span><span class="sxs-lookup"><span data-stu-id="10a23-126">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="10a23-127">如果`base.SetParametersAync`未叫用，自訂程式碼就可以任何需要的方式解讀傳入的參數值。</span><span class="sxs-lookup"><span data-stu-id="10a23-127">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="10a23-128">例如，不需要將傳入的參數指派給類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="10a23-128">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

<span data-ttu-id="10a23-129">如果已設定任何事件處理常式，請將它們解除鎖定以供處置。</span><span class="sxs-lookup"><span data-stu-id="10a23-129">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="10a23-130">如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。</span><span class="sxs-lookup"><span data-stu-id="10a23-130">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="10a23-131">設定參數之後</span><span class="sxs-lookup"><span data-stu-id="10a23-131">After parameters are set</span></span>

<span data-ttu-id="10a23-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A>系統<xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet%2A>會呼叫和：</span><span class="sxs-lookup"><span data-stu-id="10a23-132"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync%2A> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet%2A> are called:</span></span>

* <span data-ttu-id="10a23-133">當元件初始化並從其父元件收到第一組參數時。</span><span class="sxs-lookup"><span data-stu-id="10a23-133">When the component is initialized and has received its first set of parameters from its parent component.</span></span>
* <span data-ttu-id="10a23-134">當父元件重新呈現和提供時：</span><span class="sxs-lookup"><span data-stu-id="10a23-134">When the parent component re-renders and supplies:</span></span>
  * <span data-ttu-id="10a23-135">只有已知的基本不可變類型，其中至少有一個參數已變更。</span><span class="sxs-lookup"><span data-stu-id="10a23-135">Only known primitive immutable types of which at least one parameter has changed.</span></span>
  * <span data-ttu-id="10a23-136">任何複雜類型的參數。</span><span class="sxs-lookup"><span data-stu-id="10a23-136">Any complex-typed parameters.</span></span> <span data-ttu-id="10a23-137">架構無法得知複雜型別參數的值是否在內部變動，因此它會將參數集視為已變更。</span><span class="sxs-lookup"><span data-stu-id="10a23-137">The framework can't know whether the values of a complex-typed parameter have mutated internally, so it treats the parameter set as changed.</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="10a23-138">當套用參數和屬性值時， `OnParametersSetAsync`必須在生命週期事件期間進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="10a23-138">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

<span data-ttu-id="10a23-139">如果已設定任何事件處理常式，請將它們解除鎖定以供處置。</span><span class="sxs-lookup"><span data-stu-id="10a23-139">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="10a23-140">如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。</span><span class="sxs-lookup"><span data-stu-id="10a23-140">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="after-component-render"></a><span data-ttu-id="10a23-141">元件呈現之後</span><span class="sxs-lookup"><span data-stu-id="10a23-141">After component render</span></span>

<span data-ttu-id="10a23-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A>在<xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A>元件完成呈現之後，會呼叫和。</span><span class="sxs-lookup"><span data-stu-id="10a23-142"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync%2A> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> are called after a component has finished rendering.</span></span> <span data-ttu-id="10a23-143">此時會填入元素和元件參考。</span><span class="sxs-lookup"><span data-stu-id="10a23-143">Element and component references are populated at this point.</span></span> <span data-ttu-id="10a23-144">使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="10a23-144">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="10a23-145">`OnAfterRenderAsync`和`firstRender` `OnAfterRender`的參數：</span><span class="sxs-lookup"><span data-stu-id="10a23-145">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="10a23-146">會在第`true`一次呈現元件實例時設定為。</span><span class="sxs-lookup"><span data-stu-id="10a23-146">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="10a23-147">可以用來確保初始化工作只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="10a23-147">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="10a23-148">在生命週期事件期間， `OnAfterRenderAsync`必須立即執行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="10a23-148">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="10a23-149">即使您<xref:System.Threading.Tasks.Task>從`OnAfterRenderAsync`傳回，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="10a23-149">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="10a23-150">這是為了避免無限的呈現迴圈。</span><span class="sxs-lookup"><span data-stu-id="10a23-150">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="10a23-151">它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="10a23-151">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="10a23-152">`OnAfterRender`在`OnAfterRenderAsync` *伺服器上進行預呈現時，不會呼叫和。*</span><span class="sxs-lookup"><span data-stu-id="10a23-152">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

<span data-ttu-id="10a23-153">如果已設定任何事件處理常式，請將它們解除鎖定以供處置。</span><span class="sxs-lookup"><span data-stu-id="10a23-153">If any event handlers are set up, unhook them on disposal.</span></span> <span data-ttu-id="10a23-154">如需詳細資訊，請參閱[使用 IDisposable 的元件處置](#component-disposal-with-idisposable)一節。</span><span class="sxs-lookup"><span data-stu-id="10a23-154">For more information, see the [Component disposal with IDisposable](#component-disposal-with-idisposable) section.</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="10a23-155">隱藏 UI 重新整理</span><span class="sxs-lookup"><span data-stu-id="10a23-155">Suppress UI refreshing</span></span>

<span data-ttu-id="10a23-156">覆<xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>寫以隱藏 UI 重新整理。</span><span class="sxs-lookup"><span data-stu-id="10a23-156">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> to suppress UI refreshing.</span></span> <span data-ttu-id="10a23-157">如果執行傳回`true`，則會重新整理 UI：</span><span class="sxs-lookup"><span data-stu-id="10a23-157">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="10a23-158">`ShouldRender`每次呈現元件時，都會呼叫。</span><span class="sxs-lookup"><span data-stu-id="10a23-158">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="10a23-159">即使`ShouldRender`已覆寫，元件一律會一開始呈現。</span><span class="sxs-lookup"><span data-stu-id="10a23-159">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="10a23-160">狀態變更</span><span class="sxs-lookup"><span data-stu-id="10a23-160">State changes</span></span>

<span data-ttu-id="10a23-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>通知元件其狀態已變更。</span><span class="sxs-lookup"><span data-stu-id="10a23-161"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> notifies the component that its state has changed.</span></span> <span data-ttu-id="10a23-162">當適用時， `StateHasChanged`呼叫會導致元件重新顯示。</span><span class="sxs-lookup"><span data-stu-id="10a23-162">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="10a23-163">處理轉譯時的未完成非同步動作</span><span class="sxs-lookup"><span data-stu-id="10a23-163">Handle incomplete async actions at render</span></span>

<span data-ttu-id="10a23-164">在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="10a23-164">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="10a23-165">當生命週期`null`方法正在執行時，物件可能會或未完全填入資料。</span><span class="sxs-lookup"><span data-stu-id="10a23-165">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="10a23-166">提供轉譯邏輯，以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="10a23-166">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="10a23-167">當物件為`null`時，呈現預留位置 UI 專案（例如，載入訊息）。</span><span class="sxs-lookup"><span data-stu-id="10a23-167">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="10a23-168">在`FetchData` Blazor範本的元件中， `OnInitializedAsync`會覆寫為 asychronously 接收預測資料（`forecasts`）。</span><span class="sxs-lookup"><span data-stu-id="10a23-168">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="10a23-169">當`forecasts`為`null`時，會向使用者顯示載入訊息。</span><span class="sxs-lookup"><span data-stu-id="10a23-169">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="10a23-170">在所`Task`傳回的`OnInitializedAsync`完成之後，元件會以更新的狀態重新顯示。</span><span class="sxs-lookup"><span data-stu-id="10a23-170">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="10a23-171">伺服器範本中的Blazor *Pages/FetchData* ：</span><span class="sxs-lookup"><span data-stu-id="10a23-171">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="10a23-172">使用 IDisposable 的元件處置</span><span class="sxs-lookup"><span data-stu-id="10a23-172">Component disposal with IDisposable</span></span>

<span data-ttu-id="10a23-173">如果元件會執行<xref:System.IDisposable>，則會在從 UI 中移除元件時呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="10a23-173">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="10a23-174">下列元件會使用`@implements IDisposable`和`Dispose`方法：</span><span class="sxs-lookup"><span data-stu-id="10a23-174">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

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
> <span data-ttu-id="10a23-175">不<xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>支援`Dispose`在中呼叫。</span><span class="sxs-lookup"><span data-stu-id="10a23-175">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> in `Dispose` isn't supported.</span></span> <span data-ttu-id="10a23-176">`StateHasChanged`可能會在卸載轉譯器的過程中叫用，因此不支援在該時間點要求 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="10a23-176">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

<span data-ttu-id="10a23-177">取消訂閱來自 .NET 事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="10a23-177">Unsubscribe event handlers from .NET events.</span></span> <span data-ttu-id="10a23-178">下列[ Blazor表單](xref:blazor/forms-validation)範例示範如何在`Dispose`方法中解除掛接事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="10a23-178">The following [Blazor form](xref:blazor/forms-validation) examples show how to unhook an event handler in the `Dispose` method:</span></span>

* <span data-ttu-id="10a23-179">私用欄位和 lambda 方法</span><span class="sxs-lookup"><span data-stu-id="10a23-179">Private field and lambda approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-1.razor?highlight=23,28)]

* <span data-ttu-id="10a23-180">私用方法方法</span><span class="sxs-lookup"><span data-stu-id="10a23-180">Private method approach</span></span>

  [!code-razor[](lifecycle/samples_snapshot/3.x/event-handler-disposal-2.razor?highlight=16,26)]

## <a name="handle-errors"></a><span data-ttu-id="10a23-181">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="10a23-181">Handle errors</span></span>

<span data-ttu-id="10a23-182">如需在生命週期方法執行期間處理錯誤的<xref:blazor/handle-errors#lifecycle-methods>詳細資訊，請參閱。</span><span class="sxs-lookup"><span data-stu-id="10a23-182">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>

## <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="10a23-183">預呈現後的具狀態重新連接</span><span class="sxs-lookup"><span data-stu-id="10a23-183">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="10a23-184">在Blazor伺服器應用程式中`RenderMode` ， `ServerPrerendered`當為時，元件一開始會以靜態方式呈現為頁面的一部分。</span><span class="sxs-lookup"><span data-stu-id="10a23-184">In a Blazor Server app when `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="10a23-185">當瀏覽器建立回到伺服器的連接後，就會*再次*轉譯該元件，而且該元件現在是互動式的。</span><span class="sxs-lookup"><span data-stu-id="10a23-185">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="10a23-186">如果存在用於初始化元件的[OnInitialized {Async}](#component-initialization-methods)生命週期方法，則會執行*兩次*方法：</span><span class="sxs-lookup"><span data-stu-id="10a23-186">If the [OnInitialized{Async}](#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="10a23-187">當元件以靜態方式資源清單時。</span><span class="sxs-lookup"><span data-stu-id="10a23-187">When the component is prerendered statically.</span></span>
* <span data-ttu-id="10a23-188">建立伺服器連接之後。</span><span class="sxs-lookup"><span data-stu-id="10a23-188">After the server connection has been established.</span></span>

<span data-ttu-id="10a23-189">這可能會導致在最後呈現元件時，UI 中顯示的資料有明顯的變更。</span><span class="sxs-lookup"><span data-stu-id="10a23-189">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="10a23-190">若要避免Blazor伺服器應用程式中的雙呈現案例：</span><span class="sxs-lookup"><span data-stu-id="10a23-190">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="10a23-191">傳入識別碼，可在自動處理期間用來快取狀態，並在應用程式重新開機之後，取得狀態。</span><span class="sxs-lookup"><span data-stu-id="10a23-191">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="10a23-192">在預入期間使用識別碼來儲存元件狀態。</span><span class="sxs-lookup"><span data-stu-id="10a23-192">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="10a23-193">在可呈現後使用識別碼，以取得快取的狀態。</span><span class="sxs-lookup"><span data-stu-id="10a23-193">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="10a23-194">下列程式碼示範在以`WeatherForecastService`範本為基礎Blazor的伺服器應用程式中已更新，可避免雙重呈現：</span><span class="sxs-lookup"><span data-stu-id="10a23-194">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

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

<span data-ttu-id="10a23-195">如需的`RenderMode`詳細資訊，請<xref:blazor/hosting-model-configuration#render-mode>參閱。</span><span class="sxs-lookup"><span data-stu-id="10a23-195">For more information on the `RenderMode`, see <xref:blazor/hosting-model-configuration#render-mode>.</span></span>

## <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="10a23-196">偵測應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="10a23-196">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="cancelable-background-work"></a><span data-ttu-id="10a23-197">可取消的背景工作</span><span class="sxs-lookup"><span data-stu-id="10a23-197">Cancelable background work</span></span>

<span data-ttu-id="10a23-198">元件通常會執行長時間執行的背景工作，例如進行網路呼叫<xref:System.Net.Http.HttpClient>（）並與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="10a23-198">Components often perform long-running background work, such as making network calls (<xref:System.Net.Http.HttpClient>) and interacting with databases.</span></span> <span data-ttu-id="10a23-199">在幾種情況下，最好停止背景工作以節省系統資源。</span><span class="sxs-lookup"><span data-stu-id="10a23-199">It's desirable to stop the background work to conserve system resources in several situations.</span></span> <span data-ttu-id="10a23-200">例如，當使用者離開元件時，背景非同步作業不會自動停止。</span><span class="sxs-lookup"><span data-stu-id="10a23-200">For example, background asynchronous operations don't automatically stop when a user navigates away from a component.</span></span>

<span data-ttu-id="10a23-201">背景工作專案可能需要取消的其他原因包括：</span><span class="sxs-lookup"><span data-stu-id="10a23-201">Other reasons why background work items might require cancellation include:</span></span>

* <span data-ttu-id="10a23-202">執行中的背景工作使用了錯誤的輸入資料或處理參數來啟動。</span><span class="sxs-lookup"><span data-stu-id="10a23-202">An executing background task was started with faulty input data or processing parameters.</span></span>
* <span data-ttu-id="10a23-203">目前執行中的背景工作專案集必須以一組新的工作專案取代。</span><span class="sxs-lookup"><span data-stu-id="10a23-203">The current set of executing background work items must be replaced with a new set of work items.</span></span>
* <span data-ttu-id="10a23-204">必須變更目前正在執行之工作的優先順序。</span><span class="sxs-lookup"><span data-stu-id="10a23-204">The priority of currently executing tasks must be changed.</span></span>
* <span data-ttu-id="10a23-205">必須關閉應用程式，才能將它重新部署到伺服器。</span><span class="sxs-lookup"><span data-stu-id="10a23-205">The app has to be shut down in order to redeploy it to the server.</span></span>
* <span data-ttu-id="10a23-206">伺服器資源會受到限制，請強制 backgound 工作專案的重新排定。</span><span class="sxs-lookup"><span data-stu-id="10a23-206">Server resources become limited, necessitating the rescheduling of backgound work items.</span></span>

<span data-ttu-id="10a23-207">若要在元件中執行可取消的背景工作模式：</span><span class="sxs-lookup"><span data-stu-id="10a23-207">To implement a cancelable background work pattern in a component:</span></span>

* <span data-ttu-id="10a23-208">使用<xref:System.Threading.CancellationTokenSource>和<xref:System.Threading.CancellationToken>。</span><span class="sxs-lookup"><span data-stu-id="10a23-208">Use a <xref:System.Threading.CancellationTokenSource> and <xref:System.Threading.CancellationToken>.</span></span>
* <span data-ttu-id="10a23-209">在[元件的處置](#component-disposal-with-idisposable)和任何點上，手動解除標記時，請呼叫[CancellationTokenSource](xref:System.Threading.CancellationTokenSource.Cancel%2A) ，以表示應該取消背景工作。</span><span class="sxs-lookup"><span data-stu-id="10a23-209">On [disposal of the component](#component-disposal-with-idisposable) and at any point cancellation is desired by manually cancelling the token, call [CancellationTokenSource.Cancel](xref:System.Threading.CancellationTokenSource.Cancel%2A) to signal that the background work should be cancelled.</span></span>
* <span data-ttu-id="10a23-210">在非同步呼叫傳回之後，呼叫<xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A>權杖上的。</span><span class="sxs-lookup"><span data-stu-id="10a23-210">After the asynchronous call returns, call <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> on the token.</span></span>

<span data-ttu-id="10a23-211">在下例中︰</span><span class="sxs-lookup"><span data-stu-id="10a23-211">In the following example:</span></span>

* <span data-ttu-id="10a23-212">`await Task.Delay(5000, cts.Token);`代表長時間執行的非同步背景工作。</span><span class="sxs-lookup"><span data-stu-id="10a23-212">`await Task.Delay(5000, cts.Token);` represents long-running asynchronous background work.</span></span>
* <span data-ttu-id="10a23-213">`BackgroundResourceMethod`代表長時間執行的背景方法，如果在`Resource`呼叫方法之前處置，則不應該啟動。</span><span class="sxs-lookup"><span data-stu-id="10a23-213">`BackgroundResourceMethod` represents a long-running background method that shouldn't start if the `Resource` is disposed before the method is called.</span></span>

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
