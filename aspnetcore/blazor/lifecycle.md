---
title: ASP.NET Core Blazor 生命週期
author: guardrex
description: 瞭解如何在 ASP.NET Core Blazor 應用程式中使用 Razor 元件生命週期方法。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: 1482f6b2147c74b11836e8029401bb8bcb3cdb2d
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681410"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a>ASP.NET Core Blazor 生命週期

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

Blazor 架構包含同步和非同步生命週期方法。 覆寫生命週期方法，在元件初始化和轉譯期間，對元件執行其他作業。

## <a name="lifecycle-methods"></a>生命週期方法

### <a name="component-initialization-methods"></a>元件初始化方法

<xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> 會執行初始化元件的程式碼。 只有在第一次具現化元件時，才會呼叫這些方法一次。

若要執行非同步作業，請在作業上使用 `OnInitializedAsync` 和 `await` 關鍵字：

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> 在 `OnInitializedAsync` 生命週期事件期間，必須進行元件初始化期間的非同步工作。

如需同步作業，請使用 `OnInitialized`：

```csharp
protected override void OnInitialized()
{
    ...
}
```

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

`SetParametersAsync` 的預設執行會設定每個屬性的值，其以 `[Parameter]` 或 `[CascadingParameter]` 屬性裝飾，在 `ParameterView`中具有對應的值。 `ParameterView` 中沒有對應值的參數會保持不變。

如果未叫用 `base.SetParametersAync`，自訂程式碼就可以任何需要的方式解讀傳入的參數值。 例如，不需要將傳入的參數指派給類別的屬性。

### <a name="after-parameters-are-set"></a>設定參數之後

當元件已從其父系接收參數，且已將值指派給屬性時，會呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> 和 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*>。 這些方法會在元件初始化之後執行，而且每次指定新的參數值時，都會執行此動作：

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

[!code-cshtml[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a>使用 IDisposable 的元件處置

如果元件會執行 <xref:System.IDisposable>，當元件從 UI 中移除時，就會呼叫[Dispose 方法](/dotnet/standard/garbage-collection/implementing-dispose)。 下列元件會使用 `@implements IDisposable` 和 `Dispose` 方法：

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
> 不支援在 `Dispose` 中呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*>。 在卸載轉譯器的過程中，可能會叫用 `StateHasChanged`，因此不支援在該時間點要求 UI 更新。

## <a name="handle-errors"></a>處理錯誤

如需在生命週期方法執行期間處理錯誤的詳細資訊，請參閱 <xref:blazor/handle-errors#lifecycle-methods>。
