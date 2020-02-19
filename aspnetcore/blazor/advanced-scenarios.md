---
title: ASP.NET Core Blazor advanced 案例
author: guardrex
description: 深入瞭解 Blazor中的先進案例，包括如何將手動 RenderTreeBuilder 邏輯納入應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453258"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="cdd59-103">ASP.NET Core Blazor 的先進案例</span><span class="sxs-lookup"><span data-stu-id="cdd59-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="cdd59-104">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="cdd59-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="cdd59-105">手動 RenderTreeBuilder 邏輯</span><span class="sxs-lookup"><span data-stu-id="cdd59-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="cdd59-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` 提供操作元件和元素的方法，包括在程式碼中C#手動建立元件。</span><span class="sxs-lookup"><span data-stu-id="cdd59-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="cdd59-107">使用 `RenderTreeBuilder` 建立元件是一個先進的案例。</span><span class="sxs-lookup"><span data-stu-id="cdd59-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="cdd59-108">格式不正確的元件（例如，未封閉的標記標記）可能會導致未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="cdd59-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="cdd59-109">請考慮下列 `PetDetails` 元件，其可手動內建于另一個元件中：</span><span class="sxs-lookup"><span data-stu-id="cdd59-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="cdd59-110">在下列範例中，`CreateComponent` 方法中的迴圈會產生三個 `PetDetails` 元件。</span><span class="sxs-lookup"><span data-stu-id="cdd59-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="cdd59-111">呼叫 `RenderTreeBuilder` 方法來建立元件（`OpenComponent` 和 `AddAttribute`）時，序號是源程式碼號。</span><span class="sxs-lookup"><span data-stu-id="cdd59-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="cdd59-112">Blazor 差異演算法依賴對應于不同程式程式碼的序號，而不是相異的呼叫調用。</span><span class="sxs-lookup"><span data-stu-id="cdd59-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="cdd59-113">建立具有 `RenderTreeBuilder` 方法的元件時，請硬式編碼序號的引數。</span><span class="sxs-lookup"><span data-stu-id="cdd59-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="cdd59-114">**使用計算或計數器來產生序號可能會導致效能不佳。**</span><span class="sxs-lookup"><span data-stu-id="cdd59-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="cdd59-115">如需詳細資訊，請參閱[序號與程式程式碼號，而不是執行順序](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)一節。</span><span class="sxs-lookup"><span data-stu-id="cdd59-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="cdd59-116">`BuiltContent` 元件：</span><span class="sxs-lookup"><span data-stu-id="cdd59-116">`BuiltContent` component:</span></span>

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
> <span data-ttu-id="cdd59-117">`Microsoft.AspNetCore.Components.RenderTree` 中的類型允許處理轉譯作業的*結果*。</span><span class="sxs-lookup"><span data-stu-id="cdd59-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="cdd59-118">這些是 Blazor framework 執行的內部詳細資料。</span><span class="sxs-lookup"><span data-stu-id="cdd59-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="cdd59-119">這些類型應該被視為不*穩定*，未來的版本可能會變更。</span><span class="sxs-lookup"><span data-stu-id="cdd59-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="cdd59-120">序號與程式程式碼號相關，而不是執行順序</span><span class="sxs-lookup"><span data-stu-id="cdd59-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="cdd59-121">Razor 元件檔案（*razor*）一律會進行編譯。</span><span class="sxs-lookup"><span data-stu-id="cdd59-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="cdd59-122">編譯是在解讀程式碼方面的潛在優勢，因為編譯步驟可以用來插入資訊，以在執行時間改善應用程式效能。</span><span class="sxs-lookup"><span data-stu-id="cdd59-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="cdd59-123">這些改良功能的重要範例包括*序號*。</span><span class="sxs-lookup"><span data-stu-id="cdd59-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="cdd59-124">序號會向運行時程表示輸出來自哪些不同和已排序的程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="cdd59-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="cdd59-125">執行時間會使用這項資訊，以線性時間產生有效率的樹狀差異，這比一般樹狀結構的差異演算法通常還能快得多。</span><span class="sxs-lookup"><span data-stu-id="cdd59-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="cdd59-126">請考慮下列 Razor 元件（*razor*）檔案：</span><span class="sxs-lookup"><span data-stu-id="cdd59-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="cdd59-127">上述程式碼會編譯成如下所示的內容：</span><span class="sxs-lookup"><span data-stu-id="cdd59-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="cdd59-128">第一次執行程式碼時，如果 `true``someFlag`，則產生器會接收：</span><span class="sxs-lookup"><span data-stu-id="cdd59-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="cdd59-129">順序</span><span class="sxs-lookup"><span data-stu-id="cdd59-129">Sequence</span></span> | <span data-ttu-id="cdd59-130">類型</span><span class="sxs-lookup"><span data-stu-id="cdd59-130">Type</span></span>      | <span data-ttu-id="cdd59-131">資料</span><span class="sxs-lookup"><span data-stu-id="cdd59-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cdd59-132">0</span><span class="sxs-lookup"><span data-stu-id="cdd59-132">0</span></span>        | <span data-ttu-id="cdd59-133">Text node</span><span class="sxs-lookup"><span data-stu-id="cdd59-133">Text node</span></span> | <span data-ttu-id="cdd59-134">First</span><span class="sxs-lookup"><span data-stu-id="cdd59-134">First</span></span>  |
| <span data-ttu-id="cdd59-135">1</span><span class="sxs-lookup"><span data-stu-id="cdd59-135">1</span></span>        | <span data-ttu-id="cdd59-136">Text node</span><span class="sxs-lookup"><span data-stu-id="cdd59-136">Text node</span></span> | <span data-ttu-id="cdd59-137">Second</span><span class="sxs-lookup"><span data-stu-id="cdd59-137">Second</span></span> |

<span data-ttu-id="cdd59-138">假設 `someFlag` 會變成 `false`，然後再次轉譯標記。</span><span class="sxs-lookup"><span data-stu-id="cdd59-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="cdd59-139">這次，產生器會接收：</span><span class="sxs-lookup"><span data-stu-id="cdd59-139">This time, the builder receives:</span></span>

| <span data-ttu-id="cdd59-140">順序</span><span class="sxs-lookup"><span data-stu-id="cdd59-140">Sequence</span></span> | <span data-ttu-id="cdd59-141">類型</span><span class="sxs-lookup"><span data-stu-id="cdd59-141">Type</span></span>       | <span data-ttu-id="cdd59-142">資料</span><span class="sxs-lookup"><span data-stu-id="cdd59-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="cdd59-143">1</span><span class="sxs-lookup"><span data-stu-id="cdd59-143">1</span></span>        | <span data-ttu-id="cdd59-144">Text node</span><span class="sxs-lookup"><span data-stu-id="cdd59-144">Text node</span></span>  | <span data-ttu-id="cdd59-145">Second</span><span class="sxs-lookup"><span data-stu-id="cdd59-145">Second</span></span> |

<span data-ttu-id="cdd59-146">當執行時間執行差異時，它會看到順序 `0` 的專案已移除，因此它會產生下列簡單的*編輯腳本*：</span><span class="sxs-lookup"><span data-stu-id="cdd59-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="cdd59-147">移除第一個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="cdd59-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="cdd59-148">以程式設計方式產生序號的問題</span><span class="sxs-lookup"><span data-stu-id="cdd59-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="cdd59-149">想像一下，您會改為撰寫下列轉譯樹產生器邏輯：</span><span class="sxs-lookup"><span data-stu-id="cdd59-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="cdd59-150">現在，第一個輸出是：</span><span class="sxs-lookup"><span data-stu-id="cdd59-150">Now, the first output is:</span></span>

| <span data-ttu-id="cdd59-151">順序</span><span class="sxs-lookup"><span data-stu-id="cdd59-151">Sequence</span></span> | <span data-ttu-id="cdd59-152">類型</span><span class="sxs-lookup"><span data-stu-id="cdd59-152">Type</span></span>      | <span data-ttu-id="cdd59-153">資料</span><span class="sxs-lookup"><span data-stu-id="cdd59-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="cdd59-154">0</span><span class="sxs-lookup"><span data-stu-id="cdd59-154">0</span></span>        | <span data-ttu-id="cdd59-155">Text node</span><span class="sxs-lookup"><span data-stu-id="cdd59-155">Text node</span></span> | <span data-ttu-id="cdd59-156">First</span><span class="sxs-lookup"><span data-stu-id="cdd59-156">First</span></span>  |
| <span data-ttu-id="cdd59-157">1</span><span class="sxs-lookup"><span data-stu-id="cdd59-157">1</span></span>        | <span data-ttu-id="cdd59-158">Text node</span><span class="sxs-lookup"><span data-stu-id="cdd59-158">Text node</span></span> | <span data-ttu-id="cdd59-159">Second</span><span class="sxs-lookup"><span data-stu-id="cdd59-159">Second</span></span> |

<span data-ttu-id="cdd59-160">此結果與先前的案例相同，因此不會有負面問題存在。</span><span class="sxs-lookup"><span data-stu-id="cdd59-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="cdd59-161">`someFlag` 是在第二個轉譯 `false`，而輸出則是：</span><span class="sxs-lookup"><span data-stu-id="cdd59-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="cdd59-162">順序</span><span class="sxs-lookup"><span data-stu-id="cdd59-162">Sequence</span></span> | <span data-ttu-id="cdd59-163">類型</span><span class="sxs-lookup"><span data-stu-id="cdd59-163">Type</span></span>      | <span data-ttu-id="cdd59-164">資料</span><span class="sxs-lookup"><span data-stu-id="cdd59-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="cdd59-165">0</span><span class="sxs-lookup"><span data-stu-id="cdd59-165">0</span></span>        | <span data-ttu-id="cdd59-166">Text node</span><span class="sxs-lookup"><span data-stu-id="cdd59-166">Text node</span></span> | <span data-ttu-id="cdd59-167">Second</span><span class="sxs-lookup"><span data-stu-id="cdd59-167">Second</span></span> |

<span data-ttu-id="cdd59-168">這次，diff 演算法發現發生了*兩*項變更，而演算法會產生下列編輯腳本：</span><span class="sxs-lookup"><span data-stu-id="cdd59-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="cdd59-169">將第一個文位元組點的值變更為 `Second`。</span><span class="sxs-lookup"><span data-stu-id="cdd59-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="cdd59-170">移除第二個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="cdd59-170">Remove the second text node.</span></span>

<span data-ttu-id="cdd59-171">產生序號已遺失有關 `if/else` 分支和迴圈在原始程式碼中出現位置的所有實用資訊。</span><span class="sxs-lookup"><span data-stu-id="cdd59-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="cdd59-172">這會導致差異**兩倍，但前提**是之前。</span><span class="sxs-lookup"><span data-stu-id="cdd59-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="cdd59-173">這是一個簡單的範例。</span><span class="sxs-lookup"><span data-stu-id="cdd59-173">This is a trivial example.</span></span> <span data-ttu-id="cdd59-174">在具有複雜和深層嵌套結構的更真實案例中，尤其是使用迴圈時，效能成本通常較高。</span><span class="sxs-lookup"><span data-stu-id="cdd59-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="cdd59-175">Diff 演算法不會立即識別已插入或移除的迴圈區塊或分支，而是必須將深度遞迴到轉譯樹狀結構中。</span><span class="sxs-lookup"><span data-stu-id="cdd59-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="cdd59-176">這通常需要建立更長的編輯腳本，因為差異演算法會 misinformed 舊的和新結構如何彼此相關。</span><span class="sxs-lookup"><span data-stu-id="cdd59-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="cdd59-177">指引和結論</span><span class="sxs-lookup"><span data-stu-id="cdd59-177">Guidance and conclusions</span></span>

* <span data-ttu-id="cdd59-178">如果序號是動態產生的，應用程式效能會受到影響。</span><span class="sxs-lookup"><span data-stu-id="cdd59-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="cdd59-179">架構無法在執行時間自動建立自己的序號，因為必要的資訊不存在，除非是在編譯時期加以捕捉。</span><span class="sxs-lookup"><span data-stu-id="cdd59-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="cdd59-180">請勿撰寫以手動方式執行之 `RenderTreeBuilder` 邏輯的長區塊。</span><span class="sxs-lookup"><span data-stu-id="cdd59-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="cdd59-181">偏好*razor*檔案，並允許編譯器處理序號。</span><span class="sxs-lookup"><span data-stu-id="cdd59-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="cdd59-182">如果您無法避免手動 `RenderTreeBuilder` 邏輯，請將長塊的程式碼分割成較小的片段，並以 `OpenRegion`/`CloseRegion` 呼叫來包裝。</span><span class="sxs-lookup"><span data-stu-id="cdd59-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="cdd59-183">每個區域都有自己的序號個別空間，因此您可以在每個區域內從零（或任何其他任一數字）重新開機。</span><span class="sxs-lookup"><span data-stu-id="cdd59-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="cdd59-184">如果序號已硬式編碼，則 diff 演算法只會要求序號增加值。</span><span class="sxs-lookup"><span data-stu-id="cdd59-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="cdd59-185">起始值和間距無關。</span><span class="sxs-lookup"><span data-stu-id="cdd59-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="cdd59-186">一個合法的選項是使用程式程式碼號做為序號，或從零開始，並以一個或數百個（或任何慣用的間隔）來增加。</span><span class="sxs-lookup"><span data-stu-id="cdd59-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="cdd59-187"> 會使用序號，而其他樹狀結構比較的 UI 架構則不會使用它們。</span><span class="sxs-lookup"><span data-stu-id="cdd59-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="cdd59-188">使用序號時，比較速度會更快，而且 Blazor 有一個編譯步驟，可針對撰寫*razor*檔案的開發人員自動處理序號。</span><span class="sxs-lookup"><span data-stu-id="cdd59-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
