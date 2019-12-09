---
title: ASP.NET Core Blazor 生命週期
author: guardrex
description: 瞭解如何在 ASP.NET Core Blazor 應用程式中使用 Razor 元件生命週期方法。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: e600e7c7a6a8c646a655520bd5c127f2cd662753
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944027"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="e012f-103">ASP.NET Core Blazor 生命週期</span><span class="sxs-lookup"><span data-stu-id="e012f-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="e012f-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e012f-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="e012f-105">Blazor 架構包含同步和非同步生命週期方法。</span><span class="sxs-lookup"><span data-stu-id="e012f-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="e012f-106">覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。</span><span class="sxs-lookup"><span data-stu-id="e012f-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="e012f-107">生命週期方法</span><span class="sxs-lookup"><span data-stu-id="e012f-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="e012f-108">元件初始化方法</span><span class="sxs-lookup"><span data-stu-id="e012f-108">Component initialization methods</span></span>

<span data-ttu-id="e012f-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> 會執行初始化元件的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e012f-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> execute code that initializes a component.</span></span> <span data-ttu-id="e012f-110">只有在第一次具現化元件時，才會呼叫這些方法一次。</span><span class="sxs-lookup"><span data-stu-id="e012f-110">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="e012f-111">若要執行非同步作業，請在作業上使用 `OnInitializedAsync` 和 `await` 關鍵字：</span><span class="sxs-lookup"><span data-stu-id="e012f-111">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="e012f-112">在 `OnInitializedAsync` 生命週期事件期間，必須進行元件初始化期間的非同步工作。</span><span class="sxs-lookup"><span data-stu-id="e012f-112">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="e012f-113">如需同步作業，請使用 `OnInitialized`：</span><span class="sxs-lookup"><span data-stu-id="e012f-113">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="e012f-114">設定參數之前</span><span class="sxs-lookup"><span data-stu-id="e012f-114">Before parameters are set</span></span>

<span data-ttu-id="e012f-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> 會在轉譯樹狀結構中設定元件的父系所提供的參數：</span><span class="sxs-lookup"><span data-stu-id="e012f-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="e012f-116"><xref:Microsoft.AspNetCore.Components.ParameterView> 在每次呼叫 `SetParametersAsync` 時都包含整個參數值集。</span><span class="sxs-lookup"><span data-stu-id="e012f-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="e012f-117">`SetParametersAsync` 的預設執行會使用在 `ParameterView`中具有對應值的 `[Parameter]` 或 `[CascadingParameter]` 屬性，來設定每個屬性的值。</span><span class="sxs-lookup"><span data-stu-id="e012f-117">The default implementation of `SetParametersAsync` sets the value of each property with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="e012f-118">`ParameterView` 中沒有對應值的參數會保持不變。</span><span class="sxs-lookup"><span data-stu-id="e012f-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="e012f-119">如果未叫用 `base.SetParametersAync`，自訂程式碼就可以任何需要的方式解讀傳入的參數值。</span><span class="sxs-lookup"><span data-stu-id="e012f-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="e012f-120">例如，不需要將傳入的參數指派給類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="e012f-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="e012f-121">設定參數之後</span><span class="sxs-lookup"><span data-stu-id="e012f-121">After parameters are set</span></span>

<span data-ttu-id="e012f-122">當元件已從其父系接收參數，且已將值指派給屬性時，會呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>。</span><span class="sxs-lookup"><span data-stu-id="e012f-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="e012f-123">這些方法會在元件初始化之後執行，而且每次指定新的參數值時，都會執行此動作：</span><span class="sxs-lookup"><span data-stu-id="e012f-123">These methods are executed after component initialization and each time new parameter values are specified:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="e012f-124">當套用參數和屬性值時，必須在 `OnParametersSetAsync` 生命週期事件期間進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="e012f-124">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="e012f-125">元件呈現之後</span><span class="sxs-lookup"><span data-stu-id="e012f-125">After component render</span></span>

<span data-ttu-id="e012f-126">在元件完成呈現之後，會呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*>。</span><span class="sxs-lookup"><span data-stu-id="e012f-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="e012f-127">此時會填入元素和元件參考。</span><span class="sxs-lookup"><span data-stu-id="e012f-127">Element and component references are populated at this point.</span></span> <span data-ttu-id="e012f-128">使用此階段來執行使用轉譯內容的其他初始化步驟，例如啟用在轉譯的 DOM 元素上操作的協力廠商 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="e012f-128">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="e012f-129">`OnAfterRenderAsync` 和 `OnAfterRender`的 `firstRender` 參數：</span><span class="sxs-lookup"><span data-stu-id="e012f-129">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="e012f-130">會在第一次呈現元件實例時設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="e012f-130">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="e012f-131">可以用來確保初始化工作只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="e012f-131">Can be used to ensure that initialization work is only performed once.</span></span>

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
> <span data-ttu-id="e012f-132">在 `OnAfterRenderAsync` 生命週期事件期間，必須在轉譯之後立即進行非同步工作。</span><span class="sxs-lookup"><span data-stu-id="e012f-132">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="e012f-133">即使您從 `OnAfterRenderAsync`傳回 <xref:System.Threading.Tasks.Task>，架構也不會在該工作完成後，為您的元件排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="e012f-133">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="e012f-134">這是為了避免無限的呈現迴圈。</span><span class="sxs-lookup"><span data-stu-id="e012f-134">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="e012f-135">它與其他生命週期方法不同，後者會在傳回的工作完成後，排程進一步的轉譯週期。</span><span class="sxs-lookup"><span data-stu-id="e012f-135">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="e012f-136">在*伺服器上進行預呈現時，不會呼叫*`OnAfterRender` 和 `OnAfterRenderAsync`。</span><span class="sxs-lookup"><span data-stu-id="e012f-136">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="e012f-137">隱藏 UI 重新整理</span><span class="sxs-lookup"><span data-stu-id="e012f-137">Suppress UI refreshing</span></span>

<span data-ttu-id="e012f-138">覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> 以隱藏 UI 重新整理。</span><span class="sxs-lookup"><span data-stu-id="e012f-138">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="e012f-139">如果執行會傳回 `true`，則會重新整理 UI：</span><span class="sxs-lookup"><span data-stu-id="e012f-139">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="e012f-140">每次呈現元件時，都會呼叫 `ShouldRender`。</span><span class="sxs-lookup"><span data-stu-id="e012f-140">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="e012f-141">即使 `ShouldRender` 遭到覆寫，元件一律會一開始呈現。</span><span class="sxs-lookup"><span data-stu-id="e012f-141">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="e012f-142">狀態變更</span><span class="sxs-lookup"><span data-stu-id="e012f-142">State changes</span></span>

<span data-ttu-id="e012f-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> 通知元件其狀態已變更。</span><span class="sxs-lookup"><span data-stu-id="e012f-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="e012f-144">當適用時，呼叫 `StateHasChanged` 會導致元件重新顯示。</span><span class="sxs-lookup"><span data-stu-id="e012f-144">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="e012f-145">處理轉譯時的未完成非同步動作</span><span class="sxs-lookup"><span data-stu-id="e012f-145">Handle incomplete async actions at render</span></span>

<span data-ttu-id="e012f-146">在呈現元件之前，在生命週期事件中執行的非同步動作可能尚未完成。</span><span class="sxs-lookup"><span data-stu-id="e012f-146">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="e012f-147">當生命週期方法正在執行時，物件可能 `null` 或未完全填入資料。</span><span class="sxs-lookup"><span data-stu-id="e012f-147">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="e012f-148">提供轉譯邏輯，以確認物件已初始化。</span><span class="sxs-lookup"><span data-stu-id="e012f-148">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="e012f-149">當物件 `null`時，轉譯預留位置 UI 元素（例如載入訊息）。</span><span class="sxs-lookup"><span data-stu-id="e012f-149">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="e012f-150">在 Blazor 範本的 `FetchData` 元件中，`OnInitializedAsync` 會覆寫為 asychronously 接收預測資料（`forecasts`）。</span><span class="sxs-lookup"><span data-stu-id="e012f-150">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="e012f-151">當 `forecasts` `null`時，就會向使用者顯示載入訊息。</span><span class="sxs-lookup"><span data-stu-id="e012f-151">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="e012f-152">`OnInitializedAsync` 所傳回的 `Task` 完成之後，元件會以更新的狀態重新顯示。</span><span class="sxs-lookup"><span data-stu-id="e012f-152">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="e012f-153">Blazor 伺服器範本中的*Pages/FetchData* ：</span><span class="sxs-lookup"><span data-stu-id="e012f-153">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-razor[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="e012f-154">使用 IDisposable 的元件處置</span><span class="sxs-lookup"><span data-stu-id="e012f-154">Component disposal with IDisposable</span></span>

<span data-ttu-id="e012f-155">如果元件會執行 <xref:System.IDisposable>，當元件從 UI 中移除時，就會呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。</span><span class="sxs-lookup"><span data-stu-id="e012f-155">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="e012f-156">下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：</span><span class="sxs-lookup"><span data-stu-id="e012f-156">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
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
> <span data-ttu-id="e012f-157">不支援在 `Dispose` 中呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>。</span><span class="sxs-lookup"><span data-stu-id="e012f-157">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="e012f-158">在卸載轉譯器的過程中，可能會叫用 `StateHasChanged`，因此不支援在該時間點要求 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="e012f-158">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="e012f-159">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="e012f-159">Handle errors</span></span>

<span data-ttu-id="e012f-160">如需在生命週期方法執行期間處理錯誤的詳細資訊，請參閱 <xref:blazor/handle-errors#lifecycle-methods>。</span><span class="sxs-lookup"><span data-stu-id="e012f-160">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
