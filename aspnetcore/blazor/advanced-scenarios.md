---
title: ASP.NET核心Blazor進階機制
author: guardrex
description: 瞭解中的Blazor高級方案,包括如何將手動 RenderTreeBuilder 邏輯合併到應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5edbbe36e8389bac0335594b1e4331aee1c02867
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78659450"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="b6ece-103">ASP.NET核心布拉佐爾高級方案</span><span class="sxs-lookup"><span data-stu-id="b6ece-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="b6ece-104">由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="b6ece-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="blazor-server-circuit-handler"></a><span data-ttu-id="b6ece-105">布拉佐伺服器電路處理程式</span><span class="sxs-lookup"><span data-stu-id="b6ece-105">Blazor Server circuit handler</span></span>

<span data-ttu-id="b6ece-106">Blazor Server 允許代碼定義*電路處理程式*,它允許在更改使用者電路狀態時運行代碼。</span><span class="sxs-lookup"><span data-stu-id="b6ece-106">Blazor Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="b6ece-107">電路處理程式是通過從`CircuitHandler`應用的服務容器中派生和註冊類實現的。</span><span class="sxs-lookup"><span data-stu-id="b6ece-107">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="b6ece-108">以下電路處理程式範例追蹤開啟的 SignalR 連線:</span><span class="sxs-lookup"><span data-stu-id="b6ece-108">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

<span data-ttu-id="b6ece-109">電路處理程式使用 DI 註冊。</span><span class="sxs-lookup"><span data-stu-id="b6ece-109">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="b6ece-110">根據電路的實例創建作用域實例。</span><span class="sxs-lookup"><span data-stu-id="b6ece-110">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="b6ece-111">`TrackingCircuitHandler`使用前面的範例中,將建立單例服務,因為必須追蹤所有電路的狀態:</span><span class="sxs-lookup"><span data-stu-id="b6ece-111">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="b6ece-112">如果自定義電路處理程式的方法引發未處理的異常,則異常對 Blazor Server 電路是致命的。</span><span class="sxs-lookup"><span data-stu-id="b6ece-112">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="b6ece-113">要容忍處理程式代碼或調用方法中的異常,請用錯誤處理和日誌記錄將代碼包裝在一個或多個[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。</span><span class="sxs-lookup"><span data-stu-id="b6ece-113">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

<span data-ttu-id="b6ece-114">當電路因用戶斷開連接而結束,並且框架正在清理電路狀態時,框架將釋放電路的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="b6ece-114">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="b6ece-115">處置範圍將釋放實現<xref:System.IDisposable?displayProperty=fullName>的任何電路範圍的 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="b6ece-115">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="b6ece-116">如果任何 DI 服務在處置期間引發未處理的異常,則框架將記錄異常。</span><span class="sxs-lookup"><span data-stu-id="b6ece-116">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="b6ece-117">手動成成樹建構器邏輯</span><span class="sxs-lookup"><span data-stu-id="b6ece-117">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="b6ece-118">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`提供了操作元件和元素的方法,包括在 C# 代碼中手動構建元件。</span><span class="sxs-lookup"><span data-stu-id="b6ece-118">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="b6ece-119">使用`RenderTreeBuilder`創建元件是進階方案。</span><span class="sxs-lookup"><span data-stu-id="b6ece-119">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="b6ece-120">格式錯誤的元件(例如,未關閉的標記標記)可能會導致未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="b6ece-120">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="b6ece-121">請考慮以下`PetDetails`元件,這些元件可以手動建構到另一個元件中:</span><span class="sxs-lookup"><span data-stu-id="b6ece-121">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="b6ece-122">在下面的範例中,`CreateComponent`方法中的循環生成三`PetDetails`個 元件。</span><span class="sxs-lookup"><span data-stu-id="b6ece-122">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="b6ece-123">呼叫`RenderTreeBuilder`建立元件(`OpenComponent``AddAttribute`與 ) 的方法來建立 序列號時是原始程式碼行號。</span><span class="sxs-lookup"><span data-stu-id="b6ece-123">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="b6ece-124">Blazor 差異演演演算法依賴於對應於不同代碼行的序列號,而不是不同的調用調用。</span><span class="sxs-lookup"><span data-stu-id="b6ece-124">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="b6ece-125">使用`RenderTreeBuilder`方法建立元件時,對序列號的參數進行硬編碼。</span><span class="sxs-lookup"><span data-stu-id="b6ece-125">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="b6ece-126">**使用計算或計數器生成序列號可能會導致性能不佳。**</span><span class="sxs-lookup"><span data-stu-id="b6ece-126">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="b6ece-127">有關詳細資訊,請參閱[序列號與代碼行號相關,而不是執行訂單](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)部分。</span><span class="sxs-lookup"><span data-stu-id="b6ece-127">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="b6ece-128">`BuiltContent`元件:</span><span class="sxs-lookup"><span data-stu-id="b6ece-128">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="b6ece-129">中`Microsoft.AspNetCore.Components.RenderTree`的類型允許處理呈現操作*的結果*。</span><span class="sxs-lookup"><span data-stu-id="b6ece-129">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="b6ece-130">這些是布拉佐爾框架實施的內部細節。</span><span class="sxs-lookup"><span data-stu-id="b6ece-130">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="b6ece-131">這些類型應視為*不穩定*,並可能在未來版本中更改。</span><span class="sxs-lookup"><span data-stu-id="b6ece-131">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="b6ece-132">序列號與代碼行號相關,而不是執行順序</span><span class="sxs-lookup"><span data-stu-id="b6ece-132">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="b6ece-133">剃刀元件檔 (*.razor*) 總是被編譯.</span><span class="sxs-lookup"><span data-stu-id="b6ece-133">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="b6ece-134">與解釋代碼時,編譯是一個潛在的優勢,因為編譯步驟可用於注入提高運行時應用性能的資訊。</span><span class="sxs-lookup"><span data-stu-id="b6ece-134">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="b6ece-135">這些改進的一個關鍵範例的序列*號*。</span><span class="sxs-lookup"><span data-stu-id="b6ece-135">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="b6ece-136">序列號指示運行時輸出來自哪些代碼行不同且順序明確。</span><span class="sxs-lookup"><span data-stu-id="b6ece-136">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="b6ece-137">運行時使用此資訊在線性時間生成有效的樹差異,這比常規樹差異演演演算法通常可能的速度要快得多。</span><span class="sxs-lookup"><span data-stu-id="b6ece-137">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="b6ece-138">請考慮以下 Razor 元件 (*.razor*) 檔案:</span><span class="sxs-lookup"><span data-stu-id="b6ece-138">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="b6ece-139">前面的代碼編譯為類似以下內容:</span><span class="sxs-lookup"><span data-stu-id="b6ece-139">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="b6ece-140">當代碼首次執行時,如果`someFlag`是`true`,生成器將接收:</span><span class="sxs-lookup"><span data-stu-id="b6ece-140">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="b6ece-141">順序</span><span class="sxs-lookup"><span data-stu-id="b6ece-141">Sequence</span></span> | <span data-ttu-id="b6ece-142">類型</span><span class="sxs-lookup"><span data-stu-id="b6ece-142">Type</span></span>      | <span data-ttu-id="b6ece-143">資料</span><span class="sxs-lookup"><span data-stu-id="b6ece-143">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="b6ece-144">0</span><span class="sxs-lookup"><span data-stu-id="b6ece-144">0</span></span>        | <span data-ttu-id="b6ece-145">Text node</span><span class="sxs-lookup"><span data-stu-id="b6ece-145">Text node</span></span> | <span data-ttu-id="b6ece-146">First</span><span class="sxs-lookup"><span data-stu-id="b6ece-146">First</span></span>  |
| <span data-ttu-id="b6ece-147">1</span><span class="sxs-lookup"><span data-stu-id="b6ece-147">1</span></span>        | <span data-ttu-id="b6ece-148">Text node</span><span class="sxs-lookup"><span data-stu-id="b6ece-148">Text node</span></span> | <span data-ttu-id="b6ece-149">Second</span><span class="sxs-lookup"><span data-stu-id="b6ece-149">Second</span></span> |

<span data-ttu-id="b6ece-150">想像一`someFlag``false`下 ,這將成為 ,標記將再次呈現。</span><span class="sxs-lookup"><span data-stu-id="b6ece-150">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="b6ece-151">這一次,產生器收到:</span><span class="sxs-lookup"><span data-stu-id="b6ece-151">This time, the builder receives:</span></span>

| <span data-ttu-id="b6ece-152">順序</span><span class="sxs-lookup"><span data-stu-id="b6ece-152">Sequence</span></span> | <span data-ttu-id="b6ece-153">類型</span><span class="sxs-lookup"><span data-stu-id="b6ece-153">Type</span></span>       | <span data-ttu-id="b6ece-154">資料</span><span class="sxs-lookup"><span data-stu-id="b6ece-154">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="b6ece-155">1</span><span class="sxs-lookup"><span data-stu-id="b6ece-155">1</span></span>        | <span data-ttu-id="b6ece-156">Text node</span><span class="sxs-lookup"><span data-stu-id="b6ece-156">Text node</span></span>  | <span data-ttu-id="b6ece-157">Second</span><span class="sxs-lookup"><span data-stu-id="b6ece-157">Second</span></span> |

<span data-ttu-id="b6ece-158">當執行時執行差異時,它會看到序列`0`中的項目被刪除,因此它產生以下瑣碎*的編輯文稿*:</span><span class="sxs-lookup"><span data-stu-id="b6ece-158">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="b6ece-159">刪除第一個文字節點。</span><span class="sxs-lookup"><span data-stu-id="b6ece-159">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="b6ece-160">以程式設計程式產生序列號的問題</span><span class="sxs-lookup"><span data-stu-id="b6ece-160">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="b6ece-161">相反,假設您編寫了以下渲染樹產生器邏輯:</span><span class="sxs-lookup"><span data-stu-id="b6ece-161">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="b6ece-162">現在,第一個輸出是:</span><span class="sxs-lookup"><span data-stu-id="b6ece-162">Now, the first output is:</span></span>

| <span data-ttu-id="b6ece-163">順序</span><span class="sxs-lookup"><span data-stu-id="b6ece-163">Sequence</span></span> | <span data-ttu-id="b6ece-164">類型</span><span class="sxs-lookup"><span data-stu-id="b6ece-164">Type</span></span>      | <span data-ttu-id="b6ece-165">資料</span><span class="sxs-lookup"><span data-stu-id="b6ece-165">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="b6ece-166">0</span><span class="sxs-lookup"><span data-stu-id="b6ece-166">0</span></span>        | <span data-ttu-id="b6ece-167">Text node</span><span class="sxs-lookup"><span data-stu-id="b6ece-167">Text node</span></span> | <span data-ttu-id="b6ece-168">First</span><span class="sxs-lookup"><span data-stu-id="b6ece-168">First</span></span>  |
| <span data-ttu-id="b6ece-169">1</span><span class="sxs-lookup"><span data-stu-id="b6ece-169">1</span></span>        | <span data-ttu-id="b6ece-170">Text node</span><span class="sxs-lookup"><span data-stu-id="b6ece-170">Text node</span></span> | <span data-ttu-id="b6ece-171">Second</span><span class="sxs-lookup"><span data-stu-id="b6ece-171">Second</span></span> |

<span data-ttu-id="b6ece-172">此結果與之前的情況相同,因此不存在負面問題。</span><span class="sxs-lookup"><span data-stu-id="b6ece-172">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="b6ece-173">`someFlag`是`false`在第二個渲染上,輸出是:</span><span class="sxs-lookup"><span data-stu-id="b6ece-173">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="b6ece-174">順序</span><span class="sxs-lookup"><span data-stu-id="b6ece-174">Sequence</span></span> | <span data-ttu-id="b6ece-175">類型</span><span class="sxs-lookup"><span data-stu-id="b6ece-175">Type</span></span>      | <span data-ttu-id="b6ece-176">資料</span><span class="sxs-lookup"><span data-stu-id="b6ece-176">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="b6ece-177">0</span><span class="sxs-lookup"><span data-stu-id="b6ece-177">0</span></span>        | <span data-ttu-id="b6ece-178">Text node</span><span class="sxs-lookup"><span data-stu-id="b6ece-178">Text node</span></span> | <span data-ttu-id="b6ece-179">Second</span><span class="sxs-lookup"><span data-stu-id="b6ece-179">Second</span></span> |

<span data-ttu-id="b6ece-180">這一次,diff 演演算法看到*發生了兩*個更改,並且該演演演算法生成以下編輯腳本:</span><span class="sxs-lookup"><span data-stu-id="b6ece-180">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="b6ece-181">將第一個文字節點的值變更為`Second`。</span><span class="sxs-lookup"><span data-stu-id="b6ece-181">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="b6ece-182">刪除第二個文字節點。</span><span class="sxs-lookup"><span data-stu-id="b6ece-182">Remove the second text node.</span></span>

<span data-ttu-id="b6ece-183">生成序列號已丟失`if/else`有關 原始代碼中分支和迴圈存在位置的所有有用資訊。</span><span class="sxs-lookup"><span data-stu-id="b6ece-183">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="b6ece-184">這會導致差異的時間比以前**長一倍**。</span><span class="sxs-lookup"><span data-stu-id="b6ece-184">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="b6ece-185">這是一個微不足道的例子。</span><span class="sxs-lookup"><span data-stu-id="b6ece-185">This is a trivial example.</span></span> <span data-ttu-id="b6ece-186">在結構複雜且深度嵌套的更現實的情況下,尤其是迴圈中,性能成本通常較高。</span><span class="sxs-lookup"><span data-stu-id="b6ece-186">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="b6ece-187">diff 演演演算法不必立即識別已插入或刪除的循環塊或分支,而是必須深入地重新詛咒到渲染樹中。</span><span class="sxs-lookup"><span data-stu-id="b6ece-187">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="b6ece-188">這通常會導致必須構建較長的編輯腳本,因為 diff 演演演算法對新舊結構之間的關係有誤。</span><span class="sxs-lookup"><span data-stu-id="b6ece-188">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="b6ece-189">指導和結論</span><span class="sxs-lookup"><span data-stu-id="b6ece-189">Guidance and conclusions</span></span>

* <span data-ttu-id="b6ece-190">如果動態生成序列號,則應用性能會受到影響。</span><span class="sxs-lookup"><span data-stu-id="b6ece-190">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="b6ece-191">框架無法在運行時自動創建自己的序列號,因為除非在編譯時捕獲必要的信息,否則不存在必要的資訊。</span><span class="sxs-lookup"><span data-stu-id="b6ece-191">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="b6ece-192">不要編寫手動實現`RenderTreeBuilder`的邏輯的長塊。</span><span class="sxs-lookup"><span data-stu-id="b6ece-192">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="b6ece-193">首選 *.razor*檔,並允許編譯器處理序列號。</span><span class="sxs-lookup"><span data-stu-id="b6ece-193">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="b6ece-194">如果`RenderTreeBuilder`無法避免手動邏輯,請將長代碼塊拆分為在調用`OpenRegion`/`CloseRegion`中 包裝的較小部分。</span><span class="sxs-lookup"><span data-stu-id="b6ece-194">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="b6ece-195">每個區域都有其各自的序列號空間,因此您可以在每個區域內從零(或任何其他任意數位)重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b6ece-195">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="b6ece-196">如果對序列號進行硬編碼,差異演演演算法只需要該序列號的值增加。</span><span class="sxs-lookup"><span data-stu-id="b6ece-196">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="b6ece-197">初始值和間隙無關緊要。</span><span class="sxs-lookup"><span data-stu-id="b6ece-197">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="b6ece-198">一個合法的選項是使用代碼行號作為序列號,或從零開始,並增加 1 或數百(或任何首選間隔)。</span><span class="sxs-lookup"><span data-stu-id="b6ece-198">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="b6ece-199">使用序列號,而其他樹差異 UI 框架不使用它們。</span><span class="sxs-lookup"><span data-stu-id="b6ece-199"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="b6ece-200">使用序列號時,差異速度要快得多,並且Blazor具有編譯步驟的優點,該步驟可自動處理序列號,以便開發人員創作 *.razor*檔。</span><span class="sxs-lookup"><span data-stu-id="b6ece-200">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a><span data-ttu-id="b6ece-201">在伺服器應用中Blazor執行大型資料傳輸</span><span class="sxs-lookup"><span data-stu-id="b6ece-201">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="b6ece-202">在某些情況下,必須在 JAVAScriptBlazor和 之間傳輸大量數據。</span><span class="sxs-lookup"><span data-stu-id="b6ece-202">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="b6ece-203">通常,在以下時間進行大型數據傳輸:</span><span class="sxs-lookup"><span data-stu-id="b6ece-203">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="b6ece-204">瀏覽器檔案系統 API 用於上載或下載檔案。</span><span class="sxs-lookup"><span data-stu-id="b6ece-204">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="b6ece-205">需要與第三方庫進行互操作。</span><span class="sxs-lookup"><span data-stu-id="b6ece-205">Interop with a third party library is required.</span></span>

<span data-ttu-id="b6ece-206">在BlazorServer 中,有限制,以防止傳遞可能導致性能問題的單個大型消息。</span><span class="sxs-lookup"><span data-stu-id="b6ece-206">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="b6ece-207">在開發在 JavaScriptBlazor和 之間傳輸資料的代碼時,請考慮以下指南:</span><span class="sxs-lookup"><span data-stu-id="b6ece-207">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="b6ece-208">將數據切成更小的部分,並按順序發送數據段,直到伺服器接收所有數據。</span><span class="sxs-lookup"><span data-stu-id="b6ece-208">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="b6ece-209">不要在 JavaScript 和 C# 代碼中分配大型物件。</span><span class="sxs-lookup"><span data-stu-id="b6ece-209">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="b6ece-210">在發送或接收數據時,不要長時間阻止主 UI 線程。</span><span class="sxs-lookup"><span data-stu-id="b6ece-210">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="b6ece-211">釋放進程完成或取消時消耗的任何記憶體。</span><span class="sxs-lookup"><span data-stu-id="b6ece-211">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="b6ece-212">出於安全目的,強制實施以下附加要求:</span><span class="sxs-lookup"><span data-stu-id="b6ece-212">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="b6ece-213">聲明可以傳遞的最大檔或數據大小。</span><span class="sxs-lookup"><span data-stu-id="b6ece-213">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="b6ece-214">聲明從用戶端到伺服器的最低上載速率。</span><span class="sxs-lookup"><span data-stu-id="b6ece-214">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="b6ece-215">伺服器接收資料後,資料可以是:</span><span class="sxs-lookup"><span data-stu-id="b6ece-215">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="b6ece-216">暫時存儲在記憶體緩衝區中,直到收集所有段。</span><span class="sxs-lookup"><span data-stu-id="b6ece-216">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="b6ece-217">立即使用。</span><span class="sxs-lookup"><span data-stu-id="b6ece-217">Consumed immediately.</span></span> <span data-ttu-id="b6ece-218">例如,數據可以立即存儲在資料庫中,或在接收每個段時寫入磁碟。</span><span class="sxs-lookup"><span data-stu-id="b6ece-218">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="b6ece-219">以下檔上載器類處理與用戶端的 JS 互通。</span><span class="sxs-lookup"><span data-stu-id="b6ece-219">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="b6ece-220">上傳器類別使用 JS 互通來:</span><span class="sxs-lookup"><span data-stu-id="b6ece-220">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="b6ece-221">輪詢客戶端以發送數據段。</span><span class="sxs-lookup"><span data-stu-id="b6ece-221">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="b6ece-222">如果輪詢超時,則中止事務。</span><span class="sxs-lookup"><span data-stu-id="b6ece-222">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="b6ece-223">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="b6ece-223">In the preceding example:</span></span>

* <span data-ttu-id="b6ece-224">`_maxBase64SegmentSize`設定為`8192`,`_maxBase64SegmentSize = _segmentSize * 4 / 3`從計算。</span><span class="sxs-lookup"><span data-stu-id="b6ece-224">The `_maxBase64SegmentSize` is set to `8192`, which is calculated from `_maxBase64SegmentSize = _segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="b6ece-225">低級 .NET 核心記憶體管理 API`_uploadedSegments`用於在中儲存伺服器上的記憶體段。</span><span class="sxs-lookup"><span data-stu-id="b6ece-225">Low-level .NET Core memory management APIs are used to store the memory segments on the server in `_uploadedSegments`.</span></span>
* <span data-ttu-id="b6ece-226">方法`ReceiveFile`用於透過 JS 互通處理上載:</span><span class="sxs-lookup"><span data-stu-id="b6ece-226">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="b6ece-227">檔大小透過 JS`_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`與的聯名字節確定。</span><span class="sxs-lookup"><span data-stu-id="b6ece-227">The file size is determined in bytes through JS interop with `_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="b6ece-228">要接收的段數計算並儲存在中`numberOfSegments`。</span><span class="sxs-lookup"><span data-stu-id="b6ece-228">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="b6ece-229">段在`for`使用 JS 的循環`_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`中要求使用 。</span><span class="sxs-lookup"><span data-stu-id="b6ece-229">The segments are requested in a `for` loop through JS interop with `_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="b6ece-230">解碼前,除最後一個段外的所有段必須為 8,192 位元組。</span><span class="sxs-lookup"><span data-stu-id="b6ece-230">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="b6ece-231">用戶端被迫以有效的方式發送數據。</span><span class="sxs-lookup"><span data-stu-id="b6ece-231">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="b6ece-232">對於接收的每個段,在使用<xref:System.Convert.TryFromBase64String*>解碼之前執行檢查。</span><span class="sxs-lookup"><span data-stu-id="b6ece-232">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String*>.</span></span>
  * <span data-ttu-id="b6ece-233">上載完成後,具有數據的流將作為新的<xref:System.IO.Stream>`SegmentedStream`( ) 傳回。</span><span class="sxs-lookup"><span data-stu-id="b6ece-233">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="b6ece-234">分段流類將段清單公開為唯讀不可查找: <xref:System.IO.Stream></span><span class="sxs-lookup"><span data-stu-id="b6ece-234">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="b6ece-235">以下代碼實現 JavaScript 函數來接收資料:</span><span class="sxs-lookup"><span data-stu-id="b6ece-235">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```
