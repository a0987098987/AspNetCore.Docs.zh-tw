---
title: ASP.NET Core 中的記憶體管理和模式
author: rick-anderson
description: 瞭解記憶體在 ASP.NET Core 中的管理方式，以及垃圾收集行程（GC）的運作方式。
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: performance/memory
ms.openlocfilehash: 48397e9fe7da912c1930f17fb86b686f0a20c60e
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73638238"
---
# <a name="memory-management-and-garbage-collection-gc-in-aspnet-core"></a><span data-ttu-id="75a20-103">ASP.NET Core 中的記憶體管理和垃圾收集（GC）</span><span class="sxs-lookup"><span data-stu-id="75a20-103">Memory management and garbage collection (GC) in ASP.NET Core</span></span>

<span data-ttu-id="75a20-104">By [Sébastien Ros](https://github.com/sebastienros)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="75a20-104">By [Sébastien Ros](https://github.com/sebastienros) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="75a20-105">記憶體管理是很複雜的，即使是在 .NET 之類的 managed 架構中也一樣。</span><span class="sxs-lookup"><span data-stu-id="75a20-105">Memory management is complex, even in a managed framework like .NET.</span></span> <span data-ttu-id="75a20-106">分析和瞭解記憶體問題可能是一項挑戰。</span><span class="sxs-lookup"><span data-stu-id="75a20-106">Analyzing and understanding memory issues can be challenging.</span></span> <span data-ttu-id="75a20-107">這篇文章：</span><span class="sxs-lookup"><span data-stu-id="75a20-107">This article:</span></span>

* <span data-ttu-id="75a20-108">有許多*記憶體*流失和*GC 無法運作*的問題。</span><span class="sxs-lookup"><span data-stu-id="75a20-108">Was motivated by many *memory leak* and *GC not working* issues.</span></span> <span data-ttu-id="75a20-109">大部分的問題都是因為不了解記憶體耗用量在 .NET Core 中的運作方式，或不了解其測量方式所造成。</span><span class="sxs-lookup"><span data-stu-id="75a20-109">Most of these issues were caused by not understanding how memory consumption works in .NET Core, or not understanding how it's measured.</span></span>
* <span data-ttu-id="75a20-110">示範有問題的記憶體使用，並建議替代的方法。</span><span class="sxs-lookup"><span data-stu-id="75a20-110">Demonstrates problematic memory use, and suggests alternative approaches.</span></span>

## <a name="how-garbage-collection-gc-works-in-net-core"></a><span data-ttu-id="75a20-111">垃圾收集（GC）在 .NET Core 中的運作方式</span><span class="sxs-lookup"><span data-stu-id="75a20-111">How garbage collection (GC) works in .NET Core</span></span>

<span data-ttu-id="75a20-112">GC 會配置堆積區段，其中每個區段都是連續的記憶體範圍。</span><span class="sxs-lookup"><span data-stu-id="75a20-112">The GC allocates heap segments where each segment is a contiguous range of memory.</span></span> <span data-ttu-id="75a20-113">放在堆積中的物件會分類成3個層代中的其中一個：0、1或2。</span><span class="sxs-lookup"><span data-stu-id="75a20-113">Objects placed in the heap are categorized into one of 3 generations: 0, 1, or 2.</span></span> <span data-ttu-id="75a20-114">產生會決定 GC 嘗試釋放應用程式不再參考之受管理物件記憶體的頻率。</span><span class="sxs-lookup"><span data-stu-id="75a20-114">The generation determines the frequency the GC attempts to release memory on managed objects that are no longer referenced by the app.</span></span> <span data-ttu-id="75a20-115">較低編號的層代會更頻繁地進行 GC。</span><span class="sxs-lookup"><span data-stu-id="75a20-115">Lower numbered generations are GC'd more frequently.</span></span>

<span data-ttu-id="75a20-116">物件會根據其存留期從一個世代移至另一個層代。</span><span class="sxs-lookup"><span data-stu-id="75a20-116">Objects are moved from one generation to another based on their lifetime.</span></span> <span data-ttu-id="75a20-117">隨著物件的存留時間較久，它們會移至較高的層代。</span><span class="sxs-lookup"><span data-stu-id="75a20-117">As objects live longer, they are moved into a higher generation.</span></span> <span data-ttu-id="75a20-118">如先前所述，較高的層代是較不常的 GC。</span><span class="sxs-lookup"><span data-stu-id="75a20-118">As mentioned previously, higher generations are GC'd less often.</span></span> <span data-ttu-id="75a20-119">短期存留期的物件一律會保留在層代0中。</span><span class="sxs-lookup"><span data-stu-id="75a20-119">Short term lived objects always remain in generation 0.</span></span> <span data-ttu-id="75a20-120">例如，在 web 要求存留期間所參考的物件會短暫存留。</span><span class="sxs-lookup"><span data-stu-id="75a20-120">For example, objects that are referenced during the life of a web request are short lived.</span></span> <span data-ttu-id="75a20-121">應用層級[單次個體](xref:fundamentals/dependency-injection#service-lifetimes)通常會遷移至層代2。</span><span class="sxs-lookup"><span data-stu-id="75a20-121">Application level [singletons](xref:fundamentals/dependency-injection#service-lifetimes) generally migrate to generation 2.</span></span>

<span data-ttu-id="75a20-122">當 ASP.NET Core 應用程式啟動時，GC：</span><span class="sxs-lookup"><span data-stu-id="75a20-122">When an ASP.NET Core app starts, the GC:</span></span>

* <span data-ttu-id="75a20-123">會針對初始堆積區段保留一些記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-123">Reserves some memory for the initial heap segments.</span></span>
* <span data-ttu-id="75a20-124">載入執行時間時，認可記憶體的一小部分。</span><span class="sxs-lookup"><span data-stu-id="75a20-124">Commits a small portion of memory when the runtime is loaded.</span></span>

<span data-ttu-id="75a20-125">先前的記憶體配置是基於效能考慮而完成。</span><span class="sxs-lookup"><span data-stu-id="75a20-125">The preceding memory allocations are done for performance reasons.</span></span> <span data-ttu-id="75a20-126">效能優勢來自連續記憶體中的堆積區段。</span><span class="sxs-lookup"><span data-stu-id="75a20-126">The performance benefit comes from heap segments in contiguous memory.</span></span>

### <a name="call-gccollect"></a><span data-ttu-id="75a20-127">呼叫 GC。收集</span><span class="sxs-lookup"><span data-stu-id="75a20-127">Call GC.Collect</span></span>

<span data-ttu-id="75a20-128">呼叫[GC。](xref:System.GC.Collect*)明確地收集：</span><span class="sxs-lookup"><span data-stu-id="75a20-128">Calling [GC.Collect](xref:System.GC.Collect*) explicitly:</span></span>

* <span data-ttu-id="75a20-129">**不**應該由生產 ASP.NET Core 應用程式來完成。</span><span class="sxs-lookup"><span data-stu-id="75a20-129">Should **not** be done by production ASP.NET Core apps.</span></span>
* <span data-ttu-id="75a20-130">在調查記憶體流失時很有用。</span><span class="sxs-lookup"><span data-stu-id="75a20-130">Is useful when investigating memory leaks.</span></span>
* <span data-ttu-id="75a20-131">進行調查時，會確認 GC 已從記憶體中移除所有無關聯物件，以便測量記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-131">When investigating, verifies the GC has removed all dangling objects from memory so memory can be measured.</span></span>

## <a name="analyzing-the-memory-usage-of-an-app"></a><span data-ttu-id="75a20-132">分析應用程式的記憶體使用量</span><span class="sxs-lookup"><span data-stu-id="75a20-132">Analyzing the memory usage of an app</span></span>

<span data-ttu-id="75a20-133">專用工具可以協助分析記憶體使用量：</span><span class="sxs-lookup"><span data-stu-id="75a20-133">Dedicated tools can help analyzing memory usage:</span></span>

- <span data-ttu-id="75a20-134">計算物件參考</span><span class="sxs-lookup"><span data-stu-id="75a20-134">Counting object references</span></span>
- <span data-ttu-id="75a20-135">測量 GC 對 CPU 使用量有多少影響</span><span class="sxs-lookup"><span data-stu-id="75a20-135">Measuring how much impact the GC has on CPU usage</span></span>
- <span data-ttu-id="75a20-136">測量每個層代所使用的記憶體空間</span><span class="sxs-lookup"><span data-stu-id="75a20-136">Measuring memory space used for each generation</span></span>

<span data-ttu-id="75a20-137">使用下列工具來分析記憶體使用量：</span><span class="sxs-lookup"><span data-stu-id="75a20-137">Use the following tools to analyze memory usage:</span></span>

* <span data-ttu-id="75a20-138">[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace)：可用於生產機器。</span><span class="sxs-lookup"><span data-stu-id="75a20-138">[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace): Can be  used on production machines.</span></span>
* [<span data-ttu-id="75a20-139">不使用 Visual Studio 偵錯工具分析記憶體使用量</span><span class="sxs-lookup"><span data-stu-id="75a20-139">Analyze memory usage without the Visual Studio debugger</span></span>](/visualstudio/profiling/memory-usage-without-debugging2)
* [<span data-ttu-id="75a20-140">分析 Visual Studio 中的記憶體使用狀況</span><span class="sxs-lookup"><span data-stu-id="75a20-140">Profile memory usage in Visual Studio</span></span>](/visualstudio/profiling/memory-usage)

### <a name="detecting-memory-issues"></a><span data-ttu-id="75a20-141">偵測記憶體問題</span><span class="sxs-lookup"><span data-stu-id="75a20-141">Detecting memory issues</span></span>

<span data-ttu-id="75a20-142">工作管理員可用來瞭解 ASP.NET 應用程式所使用的記憶體數量。</span><span class="sxs-lookup"><span data-stu-id="75a20-142">Task Manager can be used to get an idea of how much memory an ASP.NET app is using.</span></span> <span data-ttu-id="75a20-143">[工作管理員記憶體] 值：</span><span class="sxs-lookup"><span data-stu-id="75a20-143">The Task Manager memory value:</span></span>

* <span data-ttu-id="75a20-144">代表 ASP.NET 進程所使用的記憶體數量。</span><span class="sxs-lookup"><span data-stu-id="75a20-144">Represents the amount of memory that is used by the ASP.NET process.</span></span>
* <span data-ttu-id="75a20-145">包含應用程式的生活物件以及其他記憶體取用者，例如原生記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="75a20-145">Includes the app's living objects and other memory consumers such as native memory usage.</span></span>

<span data-ttu-id="75a20-146">如果工作管理員記憶體值無限期地增加，而且永遠不會壓平合併，應用程式就會發生記憶體流失的情況。</span><span class="sxs-lookup"><span data-stu-id="75a20-146">If the Task Manager memory value increases indefinitely and never flattens out, the app has a memory leak.</span></span> <span data-ttu-id="75a20-147">下列各節將示範和說明數個記憶體使用模式。</span><span class="sxs-lookup"><span data-stu-id="75a20-147">The following sections demonstrate and explain several memory usage patterns.</span></span>

## <a name="sample-display-memory-usage-app"></a><span data-ttu-id="75a20-148">範例顯示記憶體使用量應用程式</span><span class="sxs-lookup"><span data-stu-id="75a20-148">Sample display memory usage app</span></span>

<span data-ttu-id="75a20-149">[MemoryLeak 範例應用程式](https://github.com/sebastienros/memoryleak)可在 GitHub 上取得。</span><span class="sxs-lookup"><span data-stu-id="75a20-149">The [MemoryLeak sample app](https://github.com/sebastienros/memoryleak) is available on GitHub.</span></span> <span data-ttu-id="75a20-150">MemoryLeak 應用程式：</span><span class="sxs-lookup"><span data-stu-id="75a20-150">The MemoryLeak app:</span></span>

* <span data-ttu-id="75a20-151">包含診斷控制器，它會收集應用程式的實際 tine 記憶體和 GC 資料。</span><span class="sxs-lookup"><span data-stu-id="75a20-151">Includes a diagnostic controller that gathers real-tine memory and GC data for the app.</span></span>
* <span data-ttu-id="75a20-152">具有顯示記憶體和 GC 資料的 [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="75a20-152">Has an Index page that displays the memory and GC data.</span></span> <span data-ttu-id="75a20-153">[索引] 頁面會每秒重新整理一次。</span><span class="sxs-lookup"><span data-stu-id="75a20-153">The Index page is refreshed every second.</span></span>
* <span data-ttu-id="75a20-154">包含可提供各種記憶體負載模式的 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="75a20-154">Contains an API controller that provides various memory load patterns.</span></span>
* <span data-ttu-id="75a20-155">不是支援的工具，不過，它可以用來顯示 ASP.NET Core 應用程式的記憶體使用模式。</span><span class="sxs-lookup"><span data-stu-id="75a20-155">Is not a supported tool, however, it can be used to display memory usage patterns of ASP.NET Core apps.</span></span>

<span data-ttu-id="75a20-156">執行 MemoryLeak。</span><span class="sxs-lookup"><span data-stu-id="75a20-156">Run MemoryLeak.</span></span> <span data-ttu-id="75a20-157">配置的記憶體會慢慢增加，直到發生 GC 為止。</span><span class="sxs-lookup"><span data-stu-id="75a20-157">Allocated memory slowly increases until a GC occurs.</span></span> <span data-ttu-id="75a20-158">記憶體會增加，因為此工具會配置自訂物件來捕獲資料。</span><span class="sxs-lookup"><span data-stu-id="75a20-158">Memory increases because the tool allocates custom object to capture data.</span></span> <span data-ttu-id="75a20-159">下圖顯示發生 Gen 0 GC 時的 MemoryLeak 索引頁面。</span><span class="sxs-lookup"><span data-stu-id="75a20-159">The following image shows the MemoryLeak Index page when a Gen 0 GC occurs.</span></span> <span data-ttu-id="75a20-160">此圖表顯示0個 RPS （每秒的要求數），因為尚未呼叫來自 API 控制器的 API 端點。</span><span class="sxs-lookup"><span data-stu-id="75a20-160">The chart shows 0 RPS (Requests per second) because no API endpoints from the API controller have been called.</span></span>

![先前的圖表](memory/_static/0RPS.png)

<span data-ttu-id="75a20-162">此圖表會顯示記憶體使用量的兩個值：</span><span class="sxs-lookup"><span data-stu-id="75a20-162">The chart displays two values for the memory usage:</span></span>

- <span data-ttu-id="75a20-163">已配置：受管理物件所佔用的記憶體數量</span><span class="sxs-lookup"><span data-stu-id="75a20-163">Allocated: the amount of memory occupied by managed objects</span></span>
- <span data-ttu-id="75a20-164">工作集：進程所使用的實體記憶體（RAM）總計。</span><span class="sxs-lookup"><span data-stu-id="75a20-164">Working set: the total physical memory (RAM) used by the process.</span></span> <span data-ttu-id="75a20-165">所顯示的工作集與 [工作管理員] 可以顯示的值相同。</span><span class="sxs-lookup"><span data-stu-id="75a20-165">The working set shown is the same value Task Manager can display.</span></span>

### <a name="transient-objects"></a><span data-ttu-id="75a20-166">暫時性物件</span><span class="sxs-lookup"><span data-stu-id="75a20-166">Transient objects</span></span>

<span data-ttu-id="75a20-167">下列 API 會建立一個 10 KB 的字串實例，並將它傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="75a20-167">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="75a20-168">在每個要求上，會在記憶體中配置新的物件，並將其寫入至回應。</span><span class="sxs-lookup"><span data-stu-id="75a20-168">On each request, a new object is allocated in memory and written to the response.</span></span> <span data-ttu-id="75a20-169">字串會在 .NET 中儲存為 UTF-16 字元，因此每個字元會在記憶體中使用2個位元組。</span><span class="sxs-lookup"><span data-stu-id="75a20-169">Strings are stored as UTF-16 characters in .NET so each character takes 2 bytes in memory.</span></span>

```csharp
[HttpGet("bigstring")]
public ActionResult<string> GetBigString()
{
    return new String('x', 10 * 1024);
}
```

<span data-ttu-id="75a20-170">下圖會以相對較小的負載產生，以顯示 GC 如何影響記憶體配置。</span><span class="sxs-lookup"><span data-stu-id="75a20-170">The following graph is generated with a relatively small load in to show how memory allocations are impacted by the GC.</span></span>

![先前的圖表](memory/_static/bigstring.png)

<span data-ttu-id="75a20-172">上圖顯示：</span><span class="sxs-lookup"><span data-stu-id="75a20-172">The preceding chart shows:</span></span>

* <span data-ttu-id="75a20-173">4K RPS （每秒的要求數）。</span><span class="sxs-lookup"><span data-stu-id="75a20-173">4K RPS (Requests per second).</span></span>
* <span data-ttu-id="75a20-174">層代 0 GC 回收大約每兩秒發生一次。</span><span class="sxs-lookup"><span data-stu-id="75a20-174">Generation 0 GC collections occur about every two seconds.</span></span>
* <span data-ttu-id="75a20-175">工作集在大約 500 MB 是常數。</span><span class="sxs-lookup"><span data-stu-id="75a20-175">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="75a20-176">CPU 為12%。</span><span class="sxs-lookup"><span data-stu-id="75a20-176">CPU is 12%.</span></span>
* <span data-ttu-id="75a20-177">記憶體耗用量和釋放（透過 GC）是穩定的。</span><span class="sxs-lookup"><span data-stu-id="75a20-177">The memory consumption and release (through GC) is stable.</span></span>

<span data-ttu-id="75a20-178">下圖是以機器可以處理的最大輸送量為依據。</span><span class="sxs-lookup"><span data-stu-id="75a20-178">The following chart is taken at the max throughput that can be handled by the machine.</span></span>

![先前的圖表](memory/_static/bigstring2.png)

<span data-ttu-id="75a20-180">上圖顯示：</span><span class="sxs-lookup"><span data-stu-id="75a20-180">The preceding chart shows:</span></span>

* <span data-ttu-id="75a20-181">22 RPS</span><span class="sxs-lookup"><span data-stu-id="75a20-181">22 RPS</span></span>
* <span data-ttu-id="75a20-182">層代 0 GC 回收每秒發生數次。</span><span class="sxs-lookup"><span data-stu-id="75a20-182">Generation 0 GC collections occur several times per second.</span></span>
* <span data-ttu-id="75a20-183">系統會觸發第1代集合，因為應用程式每秒配置的記憶體會大幅增加。</span><span class="sxs-lookup"><span data-stu-id="75a20-183">Generation 1 collections are triggered because the app allocated significantly more memory per second.</span></span>
* <span data-ttu-id="75a20-184">工作集在大約 500 MB 是常數。</span><span class="sxs-lookup"><span data-stu-id="75a20-184">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="75a20-185">CPU 為33%。</span><span class="sxs-lookup"><span data-stu-id="75a20-185">CPU is 33%.</span></span>
* <span data-ttu-id="75a20-186">記憶體耗用量和釋放（透過 GC）是穩定的。</span><span class="sxs-lookup"><span data-stu-id="75a20-186">The memory consumption and release (through GC) is stable.</span></span>
* <span data-ttu-id="75a20-187">CPU （33%）未過度使用，因此垃圾收集可能會跟上大量的配置。</span><span class="sxs-lookup"><span data-stu-id="75a20-187">The CPU (33%) is not over-utilized, therefore the garbage collection can keep up with a high number of allocations.</span></span>

### <a name="workstation-gc-vs-server-gc"></a><span data-ttu-id="75a20-188">工作站 GC 與伺服器 GC 的比較</span><span class="sxs-lookup"><span data-stu-id="75a20-188">Workstation GC vs. Server GC</span></span>

<span data-ttu-id="75a20-189">.NET 垃圾收集行程有兩種不同的模式：</span><span class="sxs-lookup"><span data-stu-id="75a20-189">The .NET Garbage Collector has two different modes:</span></span>

* <span data-ttu-id="75a20-190">**工作站 GC**：針對桌面優化。</span><span class="sxs-lookup"><span data-stu-id="75a20-190">**Workstation GC**: Optimized for the desktop.</span></span>
* <span data-ttu-id="75a20-191">**伺服器 GC**。</span><span class="sxs-lookup"><span data-stu-id="75a20-191">**Server GC**.</span></span> <span data-ttu-id="75a20-192">ASP.NET Core 應用程式的預設 GC。</span><span class="sxs-lookup"><span data-stu-id="75a20-192">The default GC for ASP.NET Core apps.</span></span> <span data-ttu-id="75a20-193">已針對伺服器進行優化。</span><span class="sxs-lookup"><span data-stu-id="75a20-193">Optimized for the server.</span></span>

<span data-ttu-id="75a20-194">GC 模式可以在專案檔或已發佈應用程式的 *.runtimeconfig.json*中明確設定。</span><span class="sxs-lookup"><span data-stu-id="75a20-194">The GC mode can be set explicitly in the project file or in the *runtimeconfig.json* file of the published app.</span></span> <span data-ttu-id="75a20-195">下列標記會顯示在專案檔中設定 `ServerGarbageCollection`：</span><span class="sxs-lookup"><span data-stu-id="75a20-195">The following markup shows setting `ServerGarbageCollection` in the project file:</span></span>

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

<span data-ttu-id="75a20-196">變更專案檔中的 `ServerGarbageCollection` 需要重建應用程式。</span><span class="sxs-lookup"><span data-stu-id="75a20-196">Changing `ServerGarbageCollection` in the project file requires the app to be rebuilt.</span></span>

<span data-ttu-id="75a20-197">**注意：** 在具有單一核心的機器上**無法**使用伺服器垃圾收集。</span><span class="sxs-lookup"><span data-stu-id="75a20-197">**Note:** Server garbage collection is **not** available on machines with a single core.</span></span> <span data-ttu-id="75a20-198">如需詳細資訊，請參閱<xref:System.Runtime.GCSettings.IsServerGC>。</span><span class="sxs-lookup"><span data-stu-id="75a20-198">For more information, see <xref:System.Runtime.GCSettings.IsServerGC>.</span></span>

<span data-ttu-id="75a20-199">下圖顯示使用工作站 GC 之已測的 RPS 下的記憶體設定檔。</span><span class="sxs-lookup"><span data-stu-id="75a20-199">The following image shows the memory profile under a 5K RPS using the Workstation GC.</span></span>

![先前的圖表](memory/_static/workstation.png)

<span data-ttu-id="75a20-201">此圖表與伺服器版本之間的差異很重要：</span><span class="sxs-lookup"><span data-stu-id="75a20-201">The differences between this chart and the server version are significant:</span></span>

- <span data-ttu-id="75a20-202">工作集從 500 MB 降到 70 MB。</span><span class="sxs-lookup"><span data-stu-id="75a20-202">The working set drops from 500 MB to 70 MB.</span></span>
- <span data-ttu-id="75a20-203">GC 每秒會執行層代0回收多次，而不是每兩秒。</span><span class="sxs-lookup"><span data-stu-id="75a20-203">The GC does generation 0 collections multiple times per second instead of every two seconds.</span></span>
- <span data-ttu-id="75a20-204">GC 從 300 MB 降到 10 MB。</span><span class="sxs-lookup"><span data-stu-id="75a20-204">GC drops from 300 MB to 10 MB.</span></span>

<span data-ttu-id="75a20-205">在典型的 web 伺服器環境中，CPU 使用量比記憶體更重要，因此伺服器 GC 會比較好。</span><span class="sxs-lookup"><span data-stu-id="75a20-205">On a typical web server environment, CPU usage is more important than memory, therefore the Server GC is better.</span></span> <span data-ttu-id="75a20-206">如果記憶體使用率很高，且 CPU 使用率相對較低，則工作站 GC 可能更具效能。</span><span class="sxs-lookup"><span data-stu-id="75a20-206">If memory utilization is high and CPU usage is relatively low, the Workstation GC might be more performant.</span></span> <span data-ttu-id="75a20-207">例如，在記憶體不足的情況下，裝載數個 web 應用程式的高密度。</span><span class="sxs-lookup"><span data-stu-id="75a20-207">For example, high density hosting several web apps where memory is scarce.</span></span>

### <a name="persistent-object-references"></a><span data-ttu-id="75a20-208">持續性物件參考</span><span class="sxs-lookup"><span data-stu-id="75a20-208">Persistent object references</span></span>

<span data-ttu-id="75a20-209">GC 無法釋放所參考的物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-209">The GC cannot free objects that are referenced.</span></span> <span data-ttu-id="75a20-210">已參考但不再需要的物件會導致記憶體流失。</span><span class="sxs-lookup"><span data-stu-id="75a20-210">Objects that are referenced but no longer needed result in a memory leak.</span></span> <span data-ttu-id="75a20-211">如果應用程式經常設定物件，而且不再需要時無法加以釋放，記憶體使用量會隨著時間而增加。</span><span class="sxs-lookup"><span data-stu-id="75a20-211">If the app frequently allocates objects and fails to free them after they are no longer needed, memory usage will increase over time.</span></span>

<span data-ttu-id="75a20-212">下列 API 會建立一個 10 KB 的字串實例，並將它傳回給用戶端。</span><span class="sxs-lookup"><span data-stu-id="75a20-212">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="75a20-213">上一個範例的差異在於，此實例是由靜態成員所參考，這表示它永遠無法用於集合。</span><span class="sxs-lookup"><span data-stu-id="75a20-213">The difference with the previous example is that this instance is referenced by a static member, which means it's never available for collection.</span></span>

```csharp
private static ConcurrentBag<string> _staticStrings = new ConcurrentBag<string>();

[HttpGet("staticstring")]
public ActionResult<string> GetStaticString()
{
    var bigString = new String('x', 10 * 1024);
    _staticStrings.Add(bigString);
    return bigString;
}
```

<span data-ttu-id="75a20-214">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="75a20-214">The preceding code:</span></span>

* <span data-ttu-id="75a20-215">是一般記憶體流失的範例。</span><span class="sxs-lookup"><span data-stu-id="75a20-215">Is an example of a typical memory leak.</span></span>
* <span data-ttu-id="75a20-216">使用頻繁的呼叫，會導致應用程式記憶體增加，直到進程損毀並出現 `OutOfMemory` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="75a20-216">With frequent calls, causes app memory to increases until the process crashes with an `OutOfMemory` exception.</span></span>

![先前的圖表](memory/_static/eternal.png)

<span data-ttu-id="75a20-218">在上圖中：</span><span class="sxs-lookup"><span data-stu-id="75a20-218">In the preceding image:</span></span>

* <span data-ttu-id="75a20-219">負載測試 `/api/staticstring` 端點會導致記憶體中的線性增加。</span><span class="sxs-lookup"><span data-stu-id="75a20-219">Load testing the `/api/staticstring` endpoint causes a linear increase in memory.</span></span>
* <span data-ttu-id="75a20-220">GC 會藉由呼叫第2代回收，在記憶體壓力增加時，嘗試釋放記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-220">The GC tries to free memory as the memory pressure grows, by calling a generation 2 collection.</span></span>
* <span data-ttu-id="75a20-221">GC 無法釋放洩漏的記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-221">The GC cannot free the leaked memory.</span></span> <span data-ttu-id="75a20-222">已配置和工作集增加了一段時間。</span><span class="sxs-lookup"><span data-stu-id="75a20-222">Allocated and working set increase with time.</span></span>

<span data-ttu-id="75a20-223">某些案例（例如快取）需要保留物件參考，直到記憶體壓力強制釋放它們為止。</span><span class="sxs-lookup"><span data-stu-id="75a20-223">Some scenarios, such as caching, require object references to be held until memory pressure forces them to be released.</span></span> <span data-ttu-id="75a20-224"><xref:System.WeakReference> 類別可以用於這種類型的快取程式碼。</span><span class="sxs-lookup"><span data-stu-id="75a20-224">The <xref:System.WeakReference> class can be used for this type of caching code.</span></span> <span data-ttu-id="75a20-225">記憶體壓力下會收集 `WeakReference` 物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-225">A `WeakReference` object is collected under memory pressures.</span></span> <span data-ttu-id="75a20-226"><xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 的預設執行會使用 `WeakReference`。</span><span class="sxs-lookup"><span data-stu-id="75a20-226">The default implementation of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> uses `WeakReference`.</span></span>

### <a name="native-memory"></a><span data-ttu-id="75a20-227">原生記憶體</span><span class="sxs-lookup"><span data-stu-id="75a20-227">Native memory</span></span>

<span data-ttu-id="75a20-228">有些 .NET Core 物件依賴原生記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-228">Some .NET Core objects rely on native memory.</span></span> <span data-ttu-id="75a20-229">GC**無法**收集原生記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-229">Native memory can **not** be collected by the GC.</span></span> <span data-ttu-id="75a20-230">使用原生記憶體的 .NET 物件必須使用機器碼釋放它。</span><span class="sxs-lookup"><span data-stu-id="75a20-230">The .NET object using native memory must free it using native code.</span></span>

<span data-ttu-id="75a20-231">.NET 提供 <xref:System.IDisposable> 介面，讓開發人員釋放原生記憶體。</span><span class="sxs-lookup"><span data-stu-id="75a20-231">.NET provides the <xref:System.IDisposable> interface to let developers release native memory.</span></span> <span data-ttu-id="75a20-232">即使未呼叫 <xref:System.IDisposable.Dispose*>，在完成項執行時，正確實作為[的類別](/dotnet/csharp/programming-guide/classes-and-structs/destructors)也會呼叫 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="75a20-232">Even if <xref:System.IDisposable.Dispose*> is not called, correctly implemented classes call `Dispose` when the [finalizer](/dotnet/csharp/programming-guide/classes-and-structs/destructors) runs.</span></span>

<span data-ttu-id="75a20-233">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="75a20-233">Consider the following code:</span></span>

```csharp
[HttpGet("fileprovider")]
public void GetFileProvider()
{
    var fp = new PhysicalFileProvider(TempPath);
    fp.Watch("*.*");
}
```

<span data-ttu-id="75a20-234">[PhysicaFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0)是 managed 類別，因此會在要求結束時收集任何實例。</span><span class="sxs-lookup"><span data-stu-id="75a20-234">[PhysicaFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0) is a managed class, so any instance will be collected at the end of the request.</span></span>

<span data-ttu-id="75a20-235">下圖顯示連續叫用 `fileprovider` API 時的記憶體設定檔。</span><span class="sxs-lookup"><span data-stu-id="75a20-235">The following image shows the memory profile while invoking the `fileprovider` API continuously.</span></span>

![先前的圖表](memory/_static/fileprovider.png)

<span data-ttu-id="75a20-237">上圖顯示此類別的執行明顯問題，因為它會持續增加記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="75a20-237">The preceding chart shows an obvious issue with the implementation of this class, as it keeps increasing memory usage.</span></span> <span data-ttu-id="75a20-238">這是在[此問題](https://github.com/aspnet/Home/issues/3110)中追蹤的已知問題。</span><span class="sxs-lookup"><span data-stu-id="75a20-238">This is a known problem that is being tracked in [this issue](https://github.com/aspnet/Home/issues/3110).</span></span>

<span data-ttu-id="75a20-239">在使用者程式碼中，可能會發生相同的流失，如下所示：</span><span class="sxs-lookup"><span data-stu-id="75a20-239">The same leak could be happen in user code, by one of the following:</span></span>

* <span data-ttu-id="75a20-240">未正確釋放類別。</span><span class="sxs-lookup"><span data-stu-id="75a20-240">Not releasing the class correctly.</span></span>
* <span data-ttu-id="75a20-241">忘記叫用應處置之相依物件的 `Dispose`方法。</span><span class="sxs-lookup"><span data-stu-id="75a20-241">Forgetting to invoke the `Dispose`method of the dependent objects that should be disposed.</span></span>

### <a name="large-objects-heap"></a><span data-ttu-id="75a20-242">大型物件堆積</span><span class="sxs-lookup"><span data-stu-id="75a20-242">Large objects heap</span></span>

<span data-ttu-id="75a20-243">頻繁的記憶體配置/免費週期可以分割記憶體，特別是在配置大型記憶體區塊時。</span><span class="sxs-lookup"><span data-stu-id="75a20-243">Frequent memory allocation/free cycles can fragment memory, especially when allocating large chunks of memory.</span></span> <span data-ttu-id="75a20-244">物件會配置在連續的記憶體區塊中。</span><span class="sxs-lookup"><span data-stu-id="75a20-244">Objects are allocated in contiguous blocks of memory.</span></span> <span data-ttu-id="75a20-245">為了減輕片段，當 GC 釋放記憶體時，它嘗試重組。</span><span class="sxs-lookup"><span data-stu-id="75a20-245">To mitigate fragmentation, when the GC frees memory, it trys to defragment it.</span></span> <span data-ttu-id="75a20-246">此進程稱為「**壓縮**」。</span><span class="sxs-lookup"><span data-stu-id="75a20-246">This process is called **compaction**.</span></span> <span data-ttu-id="75a20-247">壓縮牽涉到移動物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-247">Compaction involves moving objects.</span></span> <span data-ttu-id="75a20-248">移動大型物件會對效能造成負面影響。</span><span class="sxs-lookup"><span data-stu-id="75a20-248">Moving large objects imposes a performance penalty.</span></span> <span data-ttu-id="75a20-249">基於這個理由，GC 會為_大型_物件（稱為[大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)（LOH））建立一個特殊的記憶體區域。</span><span class="sxs-lookup"><span data-stu-id="75a20-249">For this reason the GC creates a special memory zone for _large_ objects, called the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) (LOH).</span></span> <span data-ttu-id="75a20-250">大於85000位元組（大約 83 KB）的物件為：</span><span class="sxs-lookup"><span data-stu-id="75a20-250">Objects that are greater than 85,000 bytes (approximately 83 KB) are:</span></span>

* <span data-ttu-id="75a20-251">放在 LOH 上。</span><span class="sxs-lookup"><span data-stu-id="75a20-251">Placed on the LOH.</span></span>
* <span data-ttu-id="75a20-252">未壓縮。</span><span class="sxs-lookup"><span data-stu-id="75a20-252">Not compacted.</span></span>
* <span data-ttu-id="75a20-253">在層代 2 Gc 期間收集。</span><span class="sxs-lookup"><span data-stu-id="75a20-253">Collected during generation 2 GCs.</span></span>

<span data-ttu-id="75a20-254">當 LOH 已滿時，GC 將會觸發層代2回收。</span><span class="sxs-lookup"><span data-stu-id="75a20-254">When the LOH is full, the GC will trigger a generation 2 collection.</span></span> <span data-ttu-id="75a20-255">第2代集合：</span><span class="sxs-lookup"><span data-stu-id="75a20-255">Generation 2 collections:</span></span>

* <span data-ttu-id="75a20-256">本質上很慢。</span><span class="sxs-lookup"><span data-stu-id="75a20-256">Are inherently slow.</span></span>
* <span data-ttu-id="75a20-257">此外，也會產生在所有其他層代上觸發集合的成本。</span><span class="sxs-lookup"><span data-stu-id="75a20-257">Additionally incur the cost of triggering a collection on all other generations.</span></span>

<span data-ttu-id="75a20-258">下列程式碼會立即壓縮 LOH：</span><span class="sxs-lookup"><span data-stu-id="75a20-258">The following code compacts the LOH immediately:</span></span>

```csharp
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();
```

<span data-ttu-id="75a20-259">如需有關壓縮 LOH 的詳細資訊，請參閱 <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode>。</span><span class="sxs-lookup"><span data-stu-id="75a20-259">See <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode> for information on compacting the LOH.</span></span>

<span data-ttu-id="75a20-260">在使用 .NET Core 3.0 和更新版本的容器中，會自動壓縮 LOH。</span><span class="sxs-lookup"><span data-stu-id="75a20-260">In containers using .NET Core 3.0 and later, the LOH is automatically compacted.</span></span>

<span data-ttu-id="75a20-261">下列 API 會說明這個行為：</span><span class="sxs-lookup"><span data-stu-id="75a20-261">The following API that illustrates this behavior:</span></span>

```csharp
[HttpGet("loh/{size=85000}")]
public int GetLOH1(int size)
{
   return new byte[size].Length;
}
```

<span data-ttu-id="75a20-262">下圖顯示在 [最大負載] 底下呼叫 `/api/loh/84975` 端點的記憶體設定檔：</span><span class="sxs-lookup"><span data-stu-id="75a20-262">The following chart shows the memory profile of calling the `/api/loh/84975` endpoint, under maximum load:</span></span>

![先前的圖表](memory/_static/loh1.png)

<span data-ttu-id="75a20-264">下圖顯示呼叫 `/api/loh/84976` 端點的記憶體設定檔，*只配置一個位元組*：</span><span class="sxs-lookup"><span data-stu-id="75a20-264">The following chart shows the memory profile of calling the `/api/loh/84976` endpoint, allocating *just one more byte*:</span></span>

![先前的圖表](memory/_static/loh2.png)

<span data-ttu-id="75a20-266">注意： `byte[]` 結構有額外的位元組。</span><span class="sxs-lookup"><span data-stu-id="75a20-266">Note: The `byte[]` structure has overhead bytes.</span></span> <span data-ttu-id="75a20-267">這就是為什麼84976個位元組會觸發85000限制的原因。</span><span class="sxs-lookup"><span data-stu-id="75a20-267">That's why 84,976 bytes triggers the 85,000 limit.</span></span>

<span data-ttu-id="75a20-268">比較上述兩個圖表：</span><span class="sxs-lookup"><span data-stu-id="75a20-268">Comparing the two preceding charts:</span></span>

* <span data-ttu-id="75a20-269">這兩種案例的工作集都很類似，大約是 450 MB。</span><span class="sxs-lookup"><span data-stu-id="75a20-269">The working set is similar for both scenarios, about 450 MB.</span></span>
* <span data-ttu-id="75a20-270">[LOH 要求（84975位元組）] 底下顯示大部分的層代0回收。</span><span class="sxs-lookup"><span data-stu-id="75a20-270">The under LOH requests (84,975 bytes) shows mostly generation 0 collections.</span></span>
* <span data-ttu-id="75a20-271">Over LOH 要求會產生常數層代2回收。</span><span class="sxs-lookup"><span data-stu-id="75a20-271">The over LOH requests generate constant generation 2 collections.</span></span> <span data-ttu-id="75a20-272">第2代回收的成本很高。</span><span class="sxs-lookup"><span data-stu-id="75a20-272">Generation 2 collections are expensive.</span></span> <span data-ttu-id="75a20-273">需要更多的 CPU，輸送量幾乎下降了50%。</span><span class="sxs-lookup"><span data-stu-id="75a20-273">More CPU is required and throughput drops almost 50%.</span></span>

<span data-ttu-id="75a20-274">暫存大型物件特別有問題，因為它們會造成 gen2 Gc。</span><span class="sxs-lookup"><span data-stu-id="75a20-274">Temporary large objects are particularly problematic because they cause gen2 GCs.</span></span>

<span data-ttu-id="75a20-275">為了達到最大效能，應該將大型物件使用降至最低。</span><span class="sxs-lookup"><span data-stu-id="75a20-275">For maximum performance, large object use should be minimized.</span></span> <span data-ttu-id="75a20-276">可能的話，請分割大型物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-276">If possible, split up large objects.</span></span> <span data-ttu-id="75a20-277">例如，ASP.NET Core 中的[回應](xref:performance/caching/response)快取中介軟體會將快取專案分割成小於85000個位元組的區塊。</span><span class="sxs-lookup"><span data-stu-id="75a20-277">For example, [Response Caching](xref:performance/caching/response) middleware in ASP.NET Core split the cache entries into blocks less than 85,000 bytes.</span></span>

<span data-ttu-id="75a20-278">下列連結顯示將物件保留在 LOH 限制之下的 ASP.NET Core 方法：</span><span class="sxs-lookup"><span data-stu-id="75a20-278">The following links show the ASP.NET Core approach to keeping objects under the LOH limit:</span></span>
- [<span data-ttu-id="75a20-279">ResponseCaching/資料流程/StreamUtilities .cs</span><span class="sxs-lookup"><span data-stu-id="75a20-279">ResponseCaching/Streams/StreamUtilities.cs</span></span>](https://github.com/aspnet/AspNetCore/blob/v3.0.0/src/Middleware/ResponseCaching/src/Streams/StreamUtilities.cs#L16)
- [<span data-ttu-id="75a20-280">ResponseCaching/MemoryResponseCache .cs</span><span class="sxs-lookup"><span data-stu-id="75a20-280">ResponseCaching/MemoryResponseCache.cs</span></span>](https://github.com/aspnet/ResponseCaching/blob/c1cb7576a0b86e32aec990c22df29c780af29ca5/src/Microsoft.AspNetCore.ResponseCaching/Internal/MemoryResponseCache.cs#L55)

<span data-ttu-id="75a20-281">如需詳細資訊，請參閱:</span><span class="sxs-lookup"><span data-stu-id="75a20-281">For more information, see:</span></span>

* [<span data-ttu-id="75a20-282">發現大型物件堆積</span><span class="sxs-lookup"><span data-stu-id="75a20-282">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="75a20-283">大型物件堆積</span><span class="sxs-lookup"><span data-stu-id="75a20-283">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)

### <a name="httpclient"></a><span data-ttu-id="75a20-284">HttpClient</span><span class="sxs-lookup"><span data-stu-id="75a20-284">HttpClient</span></span>

<span data-ttu-id="75a20-285">不正確地使用 <xref:System.Net.Http.HttpClient> 會導致資源洩漏。</span><span class="sxs-lookup"><span data-stu-id="75a20-285">Incorrectly using <xref:System.Net.Http.HttpClient> can result in a resource leak.</span></span> <span data-ttu-id="75a20-286">系統資源，例如資料庫連接、通訊端、檔案控制代碼等：</span><span class="sxs-lookup"><span data-stu-id="75a20-286">System resources, such as database connections, sockets, file handles, etc.:</span></span>

* <span data-ttu-id="75a20-287">比記憶體更少。</span><span class="sxs-lookup"><span data-stu-id="75a20-287">Are more scarce than memory.</span></span>
* <span data-ttu-id="75a20-288">當洩漏記憶體時，會有更多的問題。</span><span class="sxs-lookup"><span data-stu-id="75a20-288">Are more problematic when leaked than memory.</span></span>

<span data-ttu-id="75a20-289">有經驗的 .NET 開發人員知道要在執行 <xref:System.IDisposable>的物件上呼叫 <xref:System.IDisposable.Dispose*>。</span><span class="sxs-lookup"><span data-stu-id="75a20-289">Experienced .NET developers know to call <xref:System.IDisposable.Dispose*> on objects that implement <xref:System.IDisposable>.</span></span> <span data-ttu-id="75a20-290">不處置執行 `IDisposable` 的物件，通常會導致記憶體流失或系統資源洩漏。</span><span class="sxs-lookup"><span data-stu-id="75a20-290">Not disposing objects that implement `IDisposable` typically results in leaked memory or leaked system resources.</span></span>

<span data-ttu-id="75a20-291">`HttpClient` 會執行 `IDisposable`，但**不**應該在每次叫用時加以處置。</span><span class="sxs-lookup"><span data-stu-id="75a20-291">`HttpClient` implements `IDisposable`, but should **not** be disposed on every invocation.</span></span> <span data-ttu-id="75a20-292">相反地，應該重複使用 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="75a20-292">Rather, `HttpClient` should be reused.</span></span>

<span data-ttu-id="75a20-293">下列端點會在每個要求上建立和處置新的 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="75a20-293">The following endpoint creates and disposes a new  `HttpClient` instance on every request:</span></span>

```csharp
[HttpGet("httpclient1")]
public async Task<int> GetHttpClient1(string url)
{
    using (var httpClient = new HttpClient())
    {
        var result = await httpClient.GetAsync(url);
        return (int)result.StatusCode;
    }
}
```

<span data-ttu-id="75a20-294">在負載之下，會記錄下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="75a20-294">Under load, the following error messages are logged:</span></span>

```
fail: Microsoft.AspNetCore.Server.Kestrel[13]
      Connection id "0HLG70PBE1CR1", Request id "0HLG70PBE1CR1:00000031":
      An unhandled exception was thrown by the application.
System.Net.Http.HttpRequestException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted --->
    System.Net.Sockets.SocketException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted
   at System.Net.Http.ConnectHelper.ConnectAsync(String host, Int32 port,
    CancellationToken cancellationToken)
```

<span data-ttu-id="75a20-295">雖然會處置 `HttpClient` 實例，但實際的網路連線需要一些時間才能由作業系統釋放。</span><span class="sxs-lookup"><span data-stu-id="75a20-295">Even though the `HttpClient` instances are disposed, the actual network connection takes some time to be released by the operating system.</span></span> <span data-ttu-id="75a20-296">藉由持續建立新的連接，就會發生_埠耗盡_。</span><span class="sxs-lookup"><span data-stu-id="75a20-296">By continuously creating new connections, _ports exhaustion_ occurs.</span></span> <span data-ttu-id="75a20-297">每個用戶端連接都需要自己的用戶端埠。</span><span class="sxs-lookup"><span data-stu-id="75a20-297">Each client connection requires its own client port.</span></span>

<span data-ttu-id="75a20-298">防止埠耗盡的方法之一，就是重複使用相同的 `HttpClient` 實例：</span><span class="sxs-lookup"><span data-stu-id="75a20-298">One way to prevent port exhaustion is to reuse the same `HttpClient` instance:</span></span>

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

<span data-ttu-id="75a20-299">當應用程式停止時，就會釋放 `HttpClient` 實例。</span><span class="sxs-lookup"><span data-stu-id="75a20-299">The `HttpClient` instance is released when the app stops.</span></span> <span data-ttu-id="75a20-300">這個範例顯示，每次使用之後，不應處置每個可處置的資源。</span><span class="sxs-lookup"><span data-stu-id="75a20-300">This example shows that not every disposable resource should be disposed after each use.</span></span>

<span data-ttu-id="75a20-301">請參閱下列內容，以取得更好的方法來處理 `HttpClient` 實例的存留期：</span><span class="sxs-lookup"><span data-stu-id="75a20-301">See the following for a better way to handle the lifetime of an `HttpClient` instance:</span></span>

* [<span data-ttu-id="75a20-302">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="75a20-302">HttpClient and lifetime management</span></span>](/aspnet/core/fundamentals/http-requests#httpclient-and-lifetime-management)
* [<span data-ttu-id="75a20-303">HTTPClient factory 的 blog</span><span class="sxs-lookup"><span data-stu-id="75a20-303">HTTPClient factory blog</span></span>](https://devblogs.microsoft.com/aspnet/asp-net-core-2-1-preview1-introducing-httpclient-factory/)
 
### <a name="object-pooling"></a><span data-ttu-id="75a20-304">物件共用</span><span class="sxs-lookup"><span data-stu-id="75a20-304">Object pooling</span></span>

<span data-ttu-id="75a20-305">先前的範例示範如何將 `HttpClient` 實例設為靜態，並由所有要求重複使用。</span><span class="sxs-lookup"><span data-stu-id="75a20-305">The previous example showed how the `HttpClient` instance can be made static and reused by all requests.</span></span> <span data-ttu-id="75a20-306">重複使用會導致資源不足。</span><span class="sxs-lookup"><span data-stu-id="75a20-306">Reuse prevents running out of resources.</span></span>

<span data-ttu-id="75a20-307">物件共用：</span><span class="sxs-lookup"><span data-stu-id="75a20-307">Object pooling:</span></span>

* <span data-ttu-id="75a20-308">會使用重複使用模式。</span><span class="sxs-lookup"><span data-stu-id="75a20-308">Uses the reuse pattern.</span></span>
* <span data-ttu-id="75a20-309">是針對建立成本昂貴的物件所設計。</span><span class="sxs-lookup"><span data-stu-id="75a20-309">Is designed for objects that are expensive to create.</span></span>

<span data-ttu-id="75a20-310">集區是預先初始化的物件集合，可以線上程之間保留和釋放。</span><span class="sxs-lookup"><span data-stu-id="75a20-310">A pool is a collection of pre-initialized objects that can be reserved and released across threads.</span></span> <span data-ttu-id="75a20-311">集區可以定義配置規則，例如限制、預先定義的大小或成長率。</span><span class="sxs-lookup"><span data-stu-id="75a20-311">Pools can define allocation rules such as limits, predefined sizes, or growth rate.</span></span>

<span data-ttu-id="75a20-312">[ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/)的 NuGet 套件包含可協助管理這類集區的類別。</span><span class="sxs-lookup"><span data-stu-id="75a20-312">The NuGet package [Microsoft.Extensions.ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) contains classes that help to manage such pools.</span></span>

<span data-ttu-id="75a20-313">下列 API 端點會具現化在每個要求上填入亂數字的 `byte` 緩衝區：</span><span class="sxs-lookup"><span data-stu-id="75a20-313">The following API endpoint instantiates a `byte` buffer that is filled with random numbers on each request:</span></span>

```csharp
        [HttpGet("array/{size}")]
        public byte[] GetArray(int size)
        {
            var random = new Random();
            var array = new byte[size];
            random.NextBytes(array);

            return array;
        }
```

<span data-ttu-id="75a20-314">下列圖表顯示使用中等負載呼叫先前的 API：</span><span class="sxs-lookup"><span data-stu-id="75a20-314">The following chart display calling the preceding API with moderate load:</span></span>

![先前的圖表](memory/_static/array.png)

<span data-ttu-id="75a20-316">在上圖中，層代0回收大約每秒發生一次。</span><span class="sxs-lookup"><span data-stu-id="75a20-316">In the preceding chart, generation 0 collections happen approximately once per second.</span></span>

<span data-ttu-id="75a20-317">上述程式碼可以藉由使用[`ArrayPool<T>`](xref:System.Buffers.ArrayPool`1)來共用 `byte` 緩衝區來進行優化。</span><span class="sxs-lookup"><span data-stu-id="75a20-317">The preceding code can be optimized by pooling the `byte` buffer by using [`ArrayPool<T>`](xref:System.Buffers.ArrayPool`1).</span></span> <span data-ttu-id="75a20-318">靜態實例會在要求之間重複使用。</span><span class="sxs-lookup"><span data-stu-id="75a20-318">A static instance is reused across requests.</span></span>

<span data-ttu-id="75a20-319">這種方法的不同之處在于，會從 API 傳回集區物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-319">What's different with this approach is that a pooled object is returned from the API.</span></span> <span data-ttu-id="75a20-320">這表示：</span><span class="sxs-lookup"><span data-stu-id="75a20-320">That means:</span></span>

* <span data-ttu-id="75a20-321">當您從方法傳回時，物件就會從您的控制項移出。</span><span class="sxs-lookup"><span data-stu-id="75a20-321">The object is out of your control as soon as you return from the method.</span></span>
* <span data-ttu-id="75a20-322">您無法釋放物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-322">You can't release the object.</span></span>

<span data-ttu-id="75a20-323">若要設定物件的處置：</span><span class="sxs-lookup"><span data-stu-id="75a20-323">To set up disposal of the object:</span></span>

* <span data-ttu-id="75a20-324">將集區陣列封裝在可處置的物件中。</span><span class="sxs-lookup"><span data-stu-id="75a20-324">Encapsulate the pooled array in a disposable object.</span></span>
* <span data-ttu-id="75a20-325">使用[HttpCoNtext. RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*)註冊集區物件。</span><span class="sxs-lookup"><span data-stu-id="75a20-325">Register the pooled object with [HttpContext.Response.RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*).</span></span>

<span data-ttu-id="75a20-326">`RegisterForDispose` 會負責呼叫目標物件上的 `Dispose`，使其只有在 HTTP 要求完成時才會釋放。</span><span class="sxs-lookup"><span data-stu-id="75a20-326">`RegisterForDispose` will take care of calling `Dispose`on the target object so that it's only released when the HTTP request is complete.</span></span>

```csharp
private static ArrayPool<byte> _arrayPool = ArrayPool<byte>.Create();

private class PooledArray : IDisposable
{
    public byte[] Array { get; private set; }

    public PooledArray(int size)
    {
        Array = _arrayPool.Rent(size);
    }

    public void Dispose()
    {
        _arrayPool.Return(Array);
    }
}

[HttpGet("pooledarray/{size}")]
public byte[] GetPooledArray(int size)
{
    var pooledArray = new PooledArray(size);

    var random = new Random();
    random.NextBytes(pooledArray.Array);

    HttpContext.Response.RegisterForDispose(pooledArray);

    return pooledArray.Array;
}
```

<span data-ttu-id="75a20-327">套用與非集區版本相同的負載，會導致下列圖表：</span><span class="sxs-lookup"><span data-stu-id="75a20-327">Applying the same load as the non-pooled version results in the following chart:</span></span>

![先前的圖表](memory/_static/pooledarray.png)

<span data-ttu-id="75a20-329">主要差異是配置的位元組，因此產生的層代0回收量會較少。</span><span class="sxs-lookup"><span data-stu-id="75a20-329">The main difference is allocated bytes, and as a consequence much fewer generation 0 collections.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75a20-330">其他資源</span><span class="sxs-lookup"><span data-stu-id="75a20-330">Additional resources</span></span>

* [<span data-ttu-id="75a20-331">記憶體回收</span><span class="sxs-lookup"><span data-stu-id="75a20-331">Garbage Collection</span></span>](/dotnet/standard/garbage-collection/)
* [<span data-ttu-id="75a20-332">瞭解使用並行視覺化的不同 GC 模式</span><span class="sxs-lookup"><span data-stu-id="75a20-332">Understanding different GC modes with Concurrency Visualizer</span></span>](https://blogs.msdn.microsoft.com/seteplia/2017/01/05/understanding-different-gc-modes-with-concurrency-visualizer/)
* [<span data-ttu-id="75a20-333">發現大型物件堆積</span><span class="sxs-lookup"><span data-stu-id="75a20-333">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="75a20-334">大型物件堆積</span><span class="sxs-lookup"><span data-stu-id="75a20-334">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)
