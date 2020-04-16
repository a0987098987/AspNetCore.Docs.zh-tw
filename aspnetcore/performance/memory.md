---
title: ASP.NET核心中的記憶體管理和模式
author: rick-anderson
description: 瞭解如何在ASP.NET核心中管理記憶體,以及垃圾回收器 (GC) 的工作原理。
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: performance/memory
ms.openlocfilehash: b2af9cb567cdb1d7b2d0942601fcc3ebd999a5d9
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440944"
---
# <a name="memory-management-and-garbage-collection-gc-in-aspnet-core"></a><span data-ttu-id="cf3aa-103">ASP.NET核心中的記憶體管理和垃圾回收 (GC)</span><span class="sxs-lookup"><span data-stu-id="cf3aa-103">Memory management and garbage collection (GC) in ASP.NET Core</span></span>

<span data-ttu-id="cf3aa-104">由[塞巴斯蒂安·羅斯](https://github.com/sebastienros)和[里克·安德森](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf3aa-104">By [Sébastien Ros](https://github.com/sebastienros) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf3aa-105">記憶體管理很複雜,即使在像 .NET 這樣的託管框架中也是如此。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-105">Memory management is complex, even in a managed framework like .NET.</span></span> <span data-ttu-id="cf3aa-106">分析和理解記憶體問題可能具有挑戰性。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-106">Analyzing and understanding memory issues can be challenging.</span></span> <span data-ttu-id="cf3aa-107">本文：</span><span class="sxs-lookup"><span data-stu-id="cf3aa-107">This article:</span></span>

* <span data-ttu-id="cf3aa-108">受許多*記憶體洩漏*和*GC 不工作*問題引起的。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-108">Was motivated by many *memory leak* and *GC not working* issues.</span></span> <span data-ttu-id="cf3aa-109">這些問題大多是由於不瞭解記憶體消耗在 .NET Core 中的工作方式,或者不瞭解如何測量記憶體消耗造成的。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-109">Most of these issues were caused by not understanding how memory consumption works in .NET Core, or not understanding how it's measured.</span></span>
* <span data-ttu-id="cf3aa-110">演示有問題的記憶體使用,並建議替代方法。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-110">Demonstrates problematic memory use, and suggests alternative approaches.</span></span>

## <a name="how-garbage-collection-gc-works-in-net-core"></a><span data-ttu-id="cf3aa-111">垃圾回收 (GC) 在 .NET 核心中的工作方式</span><span class="sxs-lookup"><span data-stu-id="cf3aa-111">How garbage collection (GC) works in .NET Core</span></span>

<span data-ttu-id="cf3aa-112">GC 分配堆段,其中每個段是連續的記憶體範圍。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-112">The GC allocates heap segments where each segment is a contiguous range of memory.</span></span> <span data-ttu-id="cf3aa-113">放置在堆中的物件分為 3 代之一:0、1 或 2。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-113">Objects placed in the heap are categorized into one of 3 generations: 0, 1, or 2.</span></span> <span data-ttu-id="cf3aa-114">生成確定 GC 嘗試釋放應用程式不再引用的託管物件上的記憶體的頻率。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-114">The generation determines the frequency the GC attempts to release memory on managed objects that are no longer referenced by the app.</span></span> <span data-ttu-id="cf3aa-115">編號較低的代是 GC 更頻繁地使用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-115">Lower numbered generations are GC'd more frequently.</span></span>

<span data-ttu-id="cf3aa-116">對象根據其生存期從一代移動到另一代。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-116">Objects are moved from one generation to another based on their lifetime.</span></span> <span data-ttu-id="cf3aa-117">隨著物件壽命的延長,它們被移動到更高的一代。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-117">As objects live longer, they are moved into a higher generation.</span></span> <span data-ttu-id="cf3aa-118">如前所述,較高一代是 GC 的較少頻率。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-118">As mentioned previously, higher generations are GC'd less often.</span></span> <span data-ttu-id="cf3aa-119">短期生存對象始終保留在第 0 代中。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-119">Short term lived objects always remain in generation 0.</span></span> <span data-ttu-id="cf3aa-120">例如,在 Web 請求期間引用的對像是短暫的。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-120">For example, objects that are referenced during the life of a web request are short lived.</span></span> <span data-ttu-id="cf3aa-121">應用程式級[單例](xref:fundamentals/dependency-injection#service-lifetimes)通常遷移到第 2 代。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-121">Application level [singletons](xref:fundamentals/dependency-injection#service-lifetimes) generally migrate to generation 2.</span></span>

<span data-ttu-id="cf3aa-122">當 ASP.NET 核心應用啟動時,GC:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-122">When an ASP.NET Core app starts, the GC:</span></span>

* <span data-ttu-id="cf3aa-123">為初始堆段保留一些記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-123">Reserves some memory for the initial heap segments.</span></span>
* <span data-ttu-id="cf3aa-124">載入運行時時提交一小部分記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-124">Commits a small portion of memory when the runtime is loaded.</span></span>

<span data-ttu-id="cf3aa-125">出於性能原因,上述記憶體分配完成。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-125">The preceding memory allocations are done for performance reasons.</span></span> <span data-ttu-id="cf3aa-126">性能優勢來自連續記憶體中的堆段。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-126">The performance benefit comes from heap segments in contiguous memory.</span></span>

### <a name="call-gccollect"></a><span data-ttu-id="cf3aa-127">調用 GC。收集</span><span class="sxs-lookup"><span data-stu-id="cf3aa-127">Call GC.Collect</span></span>

<span data-ttu-id="cf3aa-128">調用[GC。明確收集](xref:System.GC.Collect*):</span><span class="sxs-lookup"><span data-stu-id="cf3aa-128">Calling [GC.Collect](xref:System.GC.Collect*) explicitly:</span></span>

* <span data-ttu-id="cf3aa-129">**不應**通過生產ASP.NET核心應用來完成。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-129">Should **not** be done by production ASP.NET Core apps.</span></span>
* <span data-ttu-id="cf3aa-130">在調查記憶體洩漏時很有用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-130">Is useful when investigating memory leaks.</span></span>
* <span data-ttu-id="cf3aa-131">調查時,驗證 GC 從記憶體中刪除了所有懸空物件,以便可以測量記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-131">When investigating, verifies the GC has removed all dangling objects from memory so memory can be measured.</span></span>

## <a name="analyzing-the-memory-usage-of-an-app"></a><span data-ttu-id="cf3aa-132">分析應用的記憶體使用方式</span><span class="sxs-lookup"><span data-stu-id="cf3aa-132">Analyzing the memory usage of an app</span></span>

<span data-ttu-id="cf3aa-133">專用工具可幫助分析記憶體使用方式:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-133">Dedicated tools can help analyzing memory usage:</span></span>

- <span data-ttu-id="cf3aa-134">計數物件參照</span><span class="sxs-lookup"><span data-stu-id="cf3aa-134">Counting object references</span></span>
- <span data-ttu-id="cf3aa-135">測量 GC 對 CPU 使用率的影響</span><span class="sxs-lookup"><span data-stu-id="cf3aa-135">Measuring how much impact the GC has on CPU usage</span></span>
- <span data-ttu-id="cf3aa-136">測量每一代使用的記憶體空間</span><span class="sxs-lookup"><span data-stu-id="cf3aa-136">Measuring memory space used for each generation</span></span>

<span data-ttu-id="cf3aa-137">使用以下工具分析記憶體使用方式:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-137">Use the following tools to analyze memory usage:</span></span>

* <span data-ttu-id="cf3aa-138">[點網跟蹤](/dotnet/core/diagnostics/dotnet-trace):可用於生產機器。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-138">[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace): Can be  used on production machines.</span></span>
* [<span data-ttu-id="cf3aa-139">無需視覺化工作室調試器即可分析記憶體使用方式</span><span class="sxs-lookup"><span data-stu-id="cf3aa-139">Analyze memory usage without the Visual Studio debugger</span></span>](/visualstudio/profiling/memory-usage-without-debugging2)
* [<span data-ttu-id="cf3aa-140">分析 Visual Studio 中的記憶體使用量</span><span class="sxs-lookup"><span data-stu-id="cf3aa-140">Profile memory usage in Visual Studio</span></span>](/visualstudio/profiling/memory-usage)

### <a name="detecting-memory-issues"></a><span data-ttu-id="cf3aa-141">偵測記憶體問題</span><span class="sxs-lookup"><span data-stu-id="cf3aa-141">Detecting memory issues</span></span>

<span data-ttu-id="cf3aa-142">任務管理器可用於瞭解應用使用的ASP.NET記憶體量。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-142">Task Manager can be used to get an idea of how much memory an ASP.NET app is using.</span></span> <span data-ttu-id="cf3aa-143">工作管理員記憶體值:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-143">The Task Manager memory value:</span></span>

* <span data-ttu-id="cf3aa-144">表示ASP.NET進程使用的記憶體量。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-144">Represents the amount of memory that is used by the ASP.NET process.</span></span>
* <span data-ttu-id="cf3aa-145">包括應用的活物件和其他記憶體消費者,如本機記憶體使用方式。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-145">Includes the app's living objects and other memory consumers such as native memory usage.</span></span>

<span data-ttu-id="cf3aa-146">如果任務管理器記憶體值無限增加且從不變展,則應用會洩漏記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-146">If the Task Manager memory value increases indefinitely and never flattens out, the app has a memory leak.</span></span> <span data-ttu-id="cf3aa-147">以下各節演示並解釋幾種記憶體使用模式。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-147">The following sections demonstrate and explain several memory usage patterns.</span></span>

## <a name="sample-display-memory-usage-app"></a><span data-ttu-id="cf3aa-148">範例顯示記憶體使用方式套用</span><span class="sxs-lookup"><span data-stu-id="cf3aa-148">Sample display memory usage app</span></span>

<span data-ttu-id="cf3aa-149">[記憶體洩漏示例應用](https://github.com/sebastienros/memoryleak)在 GitHub 上可用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-149">The [MemoryLeak sample app](https://github.com/sebastienros/memoryleak) is available on GitHub.</span></span> <span data-ttu-id="cf3aa-150">記憶體洩漏應用:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-150">The MemoryLeak app:</span></span>

* <span data-ttu-id="cf3aa-151">包括一個診斷控制器,用於收集應用的即時記憶體和 GC 數據。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-151">Includes a diagnostic controller that gathers real-time memory and GC data for the app.</span></span>
* <span data-ttu-id="cf3aa-152">具有顯示記憶體和 GC 數據的索引頁。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-152">Has an Index page that displays the memory and GC data.</span></span> <span data-ttu-id="cf3aa-153">每秒刷新一次索引頁。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-153">The Index page is refreshed every second.</span></span>
* <span data-ttu-id="cf3aa-154">包含 API 控制器,該控制器提供各種記憶體載入模式。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-154">Contains an API controller that provides various memory load patterns.</span></span>
* <span data-ttu-id="cf3aa-155">不是受支援的工具,但是,它可用於顯示ASP.NET核心應用的記憶體使用模式。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-155">Is not a supported tool, however, it can be used to display memory usage patterns of ASP.NET Core apps.</span></span>

<span data-ttu-id="cf3aa-156">運行記憶體洩漏。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-156">Run MemoryLeak.</span></span> <span data-ttu-id="cf3aa-157">分配的記憶體會緩慢增加,直到發生 GC。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-157">Allocated memory slowly increases until a GC occurs.</span></span> <span data-ttu-id="cf3aa-158">記憶體增加,因為該工具分配自定義物件以捕獲數據。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-158">Memory increases because the tool allocates custom object to capture data.</span></span> <span data-ttu-id="cf3aa-159">下圖顯示了發生第 0 代 GC 時記憶體洩漏索引頁。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-159">The following image shows the MemoryLeak Index page when a Gen 0 GC occurs.</span></span> <span data-ttu-id="cf3aa-160">該圖表顯示 0 RPS(每秒請求),因為未調用來自 API 控制器的 API 終結點。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-160">The chart shows 0 RPS (Requests per second) because no API endpoints from the API controller have been called.</span></span>

![前一圖表](memory/_static/0RPS.png)

<span data-ttu-id="cf3aa-162">圖表顯示記憶體使用方式的兩個值:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-162">The chart displays two values for the memory usage:</span></span>

- <span data-ttu-id="cf3aa-163">已分配:託管物件佔用的記憶體量</span><span class="sxs-lookup"><span data-stu-id="cf3aa-163">Allocated: the amount of memory occupied by managed objects</span></span>
- <span data-ttu-id="cf3aa-164">[工作集](/windows/win32/memory/working-set):進程虛擬位址空間中的頁面集,當前駐留在物理記憶體中。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-164">[Working set](/windows/win32/memory/working-set): The set of pages in the virtual address space of the process that are currently resident in physical memory.</span></span> <span data-ttu-id="cf3aa-165">顯示的工作集與任務管理器顯示的值相同。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-165">The working set shown is the same value Task Manager displays.</span></span>

### <a name="transient-objects"></a><span data-ttu-id="cf3aa-166">瞬態物件</span><span class="sxs-lookup"><span data-stu-id="cf3aa-166">Transient objects</span></span>

<span data-ttu-id="cf3aa-167">以下 API 創建一個 10 KB 字串實例並將其返回到用戶端。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-167">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="cf3aa-168">在每個請求上,一個新對象在記憶體中分配並寫入回應。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-168">On each request, a new object is allocated in memory and written to the response.</span></span> <span data-ttu-id="cf3aa-169">字串在 .NET 中儲存為 UTF-16 字元,因此每個字元在記憶體中需要 2 個字節。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-169">Strings are stored as UTF-16 characters in .NET so each character takes 2 bytes in memory.</span></span>

```csharp
[HttpGet("bigstring")]
public ActionResult<string> GetBigString()
{
    return new String('x', 10 * 1024);
}
```

<span data-ttu-id="cf3aa-170">以下圖形的負載相對較小,以顯示記憶體分配如何受到 GC 的影響。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-170">The following graph is generated with a relatively small load in to show how memory allocations are impacted by the GC.</span></span>

![前一圖表](memory/_static/bigstring.png)

<span data-ttu-id="cf3aa-172">上圖顯示:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-172">The preceding chart shows:</span></span>

* <span data-ttu-id="cf3aa-173">4K RPS(每秒請求)。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-173">4K RPS (Requests per second).</span></span>
* <span data-ttu-id="cf3aa-174">第 0 代 GC 集合大約每兩秒發生一次。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-174">Generation 0 GC collections occur about every two seconds.</span></span>
* <span data-ttu-id="cf3aa-175">工作集保持不變,約為 500 MB。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-175">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="cf3aa-176">CPU 為 12%。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-176">CPU is 12%.</span></span>
* <span data-ttu-id="cf3aa-177">記憶體消耗和釋放(通過 GC)穩定。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-177">The memory consumption and release (through GC) is stable.</span></span>

<span data-ttu-id="cf3aa-178">下圖以機器可以處理的最大輸送量進行。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-178">The following chart is taken at the max throughput that can be handled by the machine.</span></span>

![前一圖表](memory/_static/bigstring2.png)

<span data-ttu-id="cf3aa-180">上圖顯示:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-180">The preceding chart shows:</span></span>

* <span data-ttu-id="cf3aa-181">22K RPS</span><span class="sxs-lookup"><span data-stu-id="cf3aa-181">22K RPS</span></span>
* <span data-ttu-id="cf3aa-182">第 0 代 GC 集合每秒發生多次。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-182">Generation 0 GC collections occur several times per second.</span></span>
* <span data-ttu-id="cf3aa-183">第 1 代集合被觸發,因為應用每秒分配的記憶體顯著增加。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-183">Generation 1 collections are triggered because the app allocated significantly more memory per second.</span></span>
* <span data-ttu-id="cf3aa-184">工作集保持不變,約為 500 MB。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-184">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="cf3aa-185">CPU 為 33%。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-185">CPU is 33%.</span></span>
* <span data-ttu-id="cf3aa-186">記憶體消耗和釋放(通過 GC)穩定。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-186">The memory consumption and release (through GC) is stable.</span></span>
* <span data-ttu-id="cf3aa-187">CPU (33%)未過度使用,因此垃圾回收可以跟上大量分配。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-187">The CPU (33%) is not over-utilized, therefore the garbage collection can keep up with a high number of allocations.</span></span>

### <a name="workstation-gc-vs-server-gc"></a><span data-ttu-id="cf3aa-188">工作站 GC 與伺服器 GC</span><span class="sxs-lookup"><span data-stu-id="cf3aa-188">Workstation GC vs. Server GC</span></span>

<span data-ttu-id="cf3aa-189">.NET 垃圾回收器有兩種不同的模式:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-189">The .NET Garbage Collector has two different modes:</span></span>

* <span data-ttu-id="cf3aa-190">**工作站 GC**:針對桌面進行了優化。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-190">**Workstation GC**: Optimized for the desktop.</span></span>
* <span data-ttu-id="cf3aa-191">**伺服器 GC**。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-191">**Server GC**.</span></span> <span data-ttu-id="cf3aa-192">ASP.NET核心應用的預設 GC。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-192">The default GC for ASP.NET Core apps.</span></span> <span data-ttu-id="cf3aa-193">針對伺服器進行了優化。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-193">Optimized for the server.</span></span>

<span data-ttu-id="cf3aa-194">GC 模式可以在專案檔或已發佈應用程式的*運行時 config.json*檔中顯式設置。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-194">The GC mode can be set explicitly in the project file or in the *runtimeconfig.json* file of the published app.</span></span> <span data-ttu-id="cf3aa-195">以下標記顯示項目檔中的`ServerGarbageCollection`設定:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-195">The following markup shows setting `ServerGarbageCollection` in the project file:</span></span>

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

<span data-ttu-id="cf3aa-196">在`ServerGarbageCollection`專案檔中更改需要重新生成應用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-196">Changing `ServerGarbageCollection` in the project file requires the app to be rebuilt.</span></span>

<span data-ttu-id="cf3aa-197">**註:** 伺服器垃圾資源在具有單個核心的電腦上**無法使用**。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-197">**Note:** Server garbage collection is **not** available on machines with a single core.</span></span> <span data-ttu-id="cf3aa-198">如需詳細資訊，請參閱 <xref:System.Runtime.GCSettings.IsServerGC>。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-198">For more information, see <xref:System.Runtime.GCSettings.IsServerGC>.</span></span>

<span data-ttu-id="cf3aa-199">下圖顯示了使用工作站 GC 的 5K RPS 下的記憶體配置檔。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-199">The following image shows the memory profile under a 5K RPS using the Workstation GC.</span></span>

![前一圖表](memory/_static/workstation.png)

<span data-ttu-id="cf3aa-201">此圖表和伺服器版本之間的差異很大:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-201">The differences between this chart and the server version are significant:</span></span>

- <span data-ttu-id="cf3aa-202">工作集從 500 MB 下降到 70 MB。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-202">The working set drops from 500 MB to 70 MB.</span></span>
- <span data-ttu-id="cf3aa-203">GC 每秒生成數次集合,而不是每兩秒生成一次。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-203">The GC does generation 0 collections multiple times per second instead of every two seconds.</span></span>
- <span data-ttu-id="cf3aa-204">GC 從 300 MB 下降到 10 MB。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-204">GC drops from 300 MB to 10 MB.</span></span>

<span data-ttu-id="cf3aa-205">在典型的 Web 伺服器環境中,CPU 使用率比記憶體更重要,因此伺服器 GC 更好。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-205">On a typical web server environment, CPU usage is more important than memory, therefore the Server GC is better.</span></span> <span data-ttu-id="cf3aa-206">如果記憶體利用率高且 CPU 使用率相對較低,則工作站 GC 可能更具有性能。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-206">If memory utilization is high and CPU usage is relatively low, the Workstation GC might be more performant.</span></span> <span data-ttu-id="cf3aa-207">例如,高密度託管多個記憶體不足的 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-207">For example, high density hosting several web apps where memory is scarce.</span></span>

<a name="sc"></a>

### <a name="gc-using-docker-and-small-containers"></a><span data-ttu-id="cf3aa-208">使用 Docker 與小型容器的 GC</span><span class="sxs-lookup"><span data-stu-id="cf3aa-208">GC using Docker and small containers</span></span>

<span data-ttu-id="cf3aa-209">當在一台電腦上運行多個容器化應用時,工作站 GC 可能比伺服器 GC 更具預製功能。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-209">When multiple containerized apps are running on one machine, Workstation GC might be more preformant than Server GC.</span></span> <span data-ttu-id="cf3aa-210">有關詳細資訊,請參閱[在小型容器中使用伺服器 GC 執行](https://devblogs.microsoft.com/dotnet/running-with-server-gc-in-a-small-container-scenario-part-0/),並在[小型容器方案第 1 部分中使用伺服器 GC 執行 = GC 堆的硬限制](https://devblogs.microsoft.com/dotnet/running-with-server-gc-in-a-small-container-scenario-part-1-hard-limit-for-the-gc-heap/)。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-210">For more information, see [Running with Server GC in a Small Container](https://devblogs.microsoft.com/dotnet/running-with-server-gc-in-a-small-container-scenario-part-0/) and [Running with Server GC in a Small Container Scenario Part 1 – Hard Limit for the GC Heap](https://devblogs.microsoft.com/dotnet/running-with-server-gc-in-a-small-container-scenario-part-1-hard-limit-for-the-gc-heap/).</span></span>

### <a name="persistent-object-references"></a><span data-ttu-id="cf3aa-211">持久物件參照</span><span class="sxs-lookup"><span data-stu-id="cf3aa-211">Persistent object references</span></span>

<span data-ttu-id="cf3aa-212">GC 無法釋放引用的物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-212">The GC cannot free objects that are referenced.</span></span> <span data-ttu-id="cf3aa-213">引用但不再需要的物件會導致記憶體洩漏。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-213">Objects that are referenced but no longer needed result in a memory leak.</span></span> <span data-ttu-id="cf3aa-214">如果應用經常分配物件,並且在不再需要物件后無法釋放它們,則記憶體使用量將隨著時間的推移而增加。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-214">If the app frequently allocates objects and fails to free them after they are no longer needed, memory usage will increase over time.</span></span>

<span data-ttu-id="cf3aa-215">以下 API 創建一個 10 KB 字串實例並將其返回到用戶端。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-215">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="cf3aa-216">與上一個示例的區別是此實例由靜態成員引用,這意味著它永遠不會可用於收集。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-216">The difference with the previous example is that this instance is referenced by a static member, which means it's never available for collection.</span></span>

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

<span data-ttu-id="cf3aa-217">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="cf3aa-217">The preceding code:</span></span>

* <span data-ttu-id="cf3aa-218">是典型的記憶體洩漏的示例。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-218">Is an example of a typical memory leak.</span></span>
* <span data-ttu-id="cf3aa-219">頻繁調用時,會導致應用記憶體增加,直到進程崩潰,`OutOfMemory`出現異常。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-219">With frequent calls, causes app memory to increases until the process crashes with an `OutOfMemory` exception.</span></span>

![前一圖表](memory/_static/eternal.png)

<span data-ttu-id="cf3aa-221">在前面的影像中:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-221">In the preceding image:</span></span>

* <span data-ttu-id="cf3aa-222">負載測試`/api/staticstring`終結點會導致記憶體線性增加。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-222">Load testing the `/api/staticstring` endpoint causes a linear increase in memory.</span></span>
* <span data-ttu-id="cf3aa-223">GC 嘗試通過調用第 2 代集合來釋放記憶體,因為記憶體壓力增大。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-223">The GC tries to free memory as the memory pressure grows, by calling a generation 2 collection.</span></span>
* <span data-ttu-id="cf3aa-224">GC 無法釋放洩漏的記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-224">The GC cannot free the leaked memory.</span></span> <span data-ttu-id="cf3aa-225">分配和工作集會隨時間而增加。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-225">Allocated and working set increase with time.</span></span>

<span data-ttu-id="cf3aa-226">某些方案(如緩存)要求保持物件引用,直到記憶體壓力強制釋放它們。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-226">Some scenarios, such as caching, require object references to be held until memory pressure forces them to be released.</span></span> <span data-ttu-id="cf3aa-227">類<xref:System.WeakReference>可用於這種類型的緩存代碼。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-227">The <xref:System.WeakReference> class can be used for this type of caching code.</span></span> <span data-ttu-id="cf3aa-228">對`WeakReference`像是在記憶體壓力下收集的。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-228">A `WeakReference` object is collected under memory pressures.</span></span> <span data-ttu-id="cf3aa-229"><xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>使用的`WeakReference`默認實現。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-229">The default implementation of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> uses `WeakReference`.</span></span>

### <a name="native-memory"></a><span data-ttu-id="cf3aa-230">本機記憶體</span><span class="sxs-lookup"><span data-stu-id="cf3aa-230">Native memory</span></span>

<span data-ttu-id="cf3aa-231">某些 .NET Core 物件依賴於本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-231">Some .NET Core objects rely on native memory.</span></span> <span data-ttu-id="cf3aa-232">GC**無法**收集本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-232">Native memory can **not** be collected by the GC.</span></span> <span data-ttu-id="cf3aa-233">使用本機記憶體的 .NET物件必須使用本機代碼釋放它。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-233">The .NET object using native memory must free it using native code.</span></span>

<span data-ttu-id="cf3aa-234">.NET<xref:System.IDisposable>提供允許開發人員釋放本機記憶體的介面。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-234">.NET provides the <xref:System.IDisposable> interface to let developers release native memory.</span></span> <span data-ttu-id="cf3aa-235">即使<xref:System.IDisposable.Dispose*>未調用,在[終結器](/dotnet/csharp/programming-guide/classes-and-structs/destructors)運行時`Dispose`,也會正確實現類調用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-235">Even if <xref:System.IDisposable.Dispose*> is not called, correctly implemented classes call `Dispose` when the [finalizer](/dotnet/csharp/programming-guide/classes-and-structs/destructors) runs.</span></span>

<span data-ttu-id="cf3aa-236">請考慮下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="cf3aa-236">Consider the following code:</span></span>

```csharp
[HttpGet("fileprovider")]
public void GetFileProvider()
{
    var fp = new PhysicalFileProvider(TempPath);
    fp.Watch("*.*");
}
```

<span data-ttu-id="cf3aa-237">[物理檔提供程式](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0)是託管類,因此將在請求結束時收集任何實例。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-237">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0) is a managed class, so any instance will be collected at the end of the request.</span></span>

<span data-ttu-id="cf3aa-238">下圖顯示連續調用 API`fileprovider`時的 記憶體配置檔。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-238">The following image shows the memory profile while invoking the `fileprovider` API continuously.</span></span>

![前一圖表](memory/_static/fileprovider.png)

<span data-ttu-id="cf3aa-240">前面的圖表顯示了此類實現的一個明顯問題,因為它不斷增加記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-240">The preceding chart shows an obvious issue with the implementation of this class, as it keeps increasing memory usage.</span></span> <span data-ttu-id="cf3aa-241">這是一個已知的問題,正在跟蹤[在此問題](https://github.com/dotnet/aspnetcore/issues/3110)。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-241">This is a known problem that is being tracked in [this issue](https://github.com/dotnet/aspnetcore/issues/3110).</span></span>

<span data-ttu-id="cf3aa-242">使用者代碼中也可能發生相同的洩漏,其原因之一如下:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-242">The same leak could be happen in user code, by one of the following:</span></span>

* <span data-ttu-id="cf3aa-243">未正確釋放類。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-243">Not releasing the class correctly.</span></span>
* <span data-ttu-id="cf3aa-244">忘記調用應釋放的`Dispose`從屬物件的方法。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-244">Forgetting to invoke the `Dispose`method of the dependent objects that should be disposed.</span></span>

### <a name="large-objects-heap"></a><span data-ttu-id="cf3aa-245">大型物件堆</span><span class="sxs-lookup"><span data-stu-id="cf3aa-245">Large objects heap</span></span>

<span data-ttu-id="cf3aa-246">頻繁的記憶體分配/空閒週期可能會分散記憶體,尤其是在分配大塊記憶體時。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-246">Frequent memory allocation/free cycles can fragment memory, especially when allocating large chunks of memory.</span></span> <span data-ttu-id="cf3aa-247">對象以連續的記憶體塊分配。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-247">Objects are allocated in contiguous blocks of memory.</span></span> <span data-ttu-id="cf3aa-248">為了緩解碎片,當 GC 釋放記憶體時,它會嘗試對它進行碎片整理。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-248">To mitigate fragmentation, when the GC frees memory, it trys to defragment it.</span></span> <span data-ttu-id="cf3aa-249">此過程為**壓縮**。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-249">This process is called **compaction**.</span></span> <span data-ttu-id="cf3aa-250">壓實涉及移動物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-250">Compaction involves moving objects.</span></span> <span data-ttu-id="cf3aa-251">移動大型物件會造成性能損失。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-251">Moving large objects imposes a performance penalty.</span></span> <span data-ttu-id="cf3aa-252">因此,GC 會為_大型_物件(稱為[大型物件堆](/dotnet/standard/garbage-collection/large-object-heap)(LOH) 創建特殊內存區域。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-252">For this reason the GC creates a special memory zone for _large_ objects, called the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) (LOH).</span></span> <span data-ttu-id="cf3aa-253">大於 85,000 位元組(約 83 KB)的物件是:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-253">Objects that are greater than 85,000 bytes (approximately 83 KB) are:</span></span>

* <span data-ttu-id="cf3aa-254">放置在 LOH 上。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-254">Placed on the LOH.</span></span>
* <span data-ttu-id="cf3aa-255">未壓縮。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-255">Not compacted.</span></span>
* <span data-ttu-id="cf3aa-256">在第 2 代 GC 期間收集。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-256">Collected during generation 2 GCs.</span></span>

<span data-ttu-id="cf3aa-257">當 LOH 已滿時,GC 將觸發第 2 代集合。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-257">When the LOH is full, the GC will trigger a generation 2 collection.</span></span> <span data-ttu-id="cf3aa-258">第二代集合:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-258">Generation 2 collections:</span></span>

* <span data-ttu-id="cf3aa-259">本質上是緩慢的。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-259">Are inherently slow.</span></span>
* <span data-ttu-id="cf3aa-260">此外,還要承擔觸發所有其他代的集合的成本。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-260">Additionally incur the cost of triggering a collection on all other generations.</span></span>

<span data-ttu-id="cf3aa-261">以下代碼可立即壓縮 LOH:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-261">The following code compacts the LOH immediately:</span></span>

```csharp
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();
```

<span data-ttu-id="cf3aa-262">有關<xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode>壓縮 LOH 的資訊,請參閱。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-262">See <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode> for information on compacting the LOH.</span></span>

<span data-ttu-id="cf3aa-263">在使用 .NET Core 3.0 及更高版本的容器中,LOH 會自動壓縮。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-263">In containers using .NET Core 3.0 and later, the LOH is automatically compacted.</span></span>

<span data-ttu-id="cf3aa-264">以下說明此行為的 API:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-264">The following API that illustrates this behavior:</span></span>

```csharp
[HttpGet("loh/{size=85000}")]
public int GetLOH1(int size)
{
   return new byte[size].Length;
}
```

<span data-ttu-id="cf3aa-265">下圖顯示了在最大負載下調用終結點的`/api/loh/84975`記憶體配置檔:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-265">The following chart shows the memory profile of calling the `/api/loh/84975` endpoint, under maximum load:</span></span>

![前一圖表](memory/_static/loh1.png)

<span data-ttu-id="cf3aa-267">下圖顯示了呼叫終結點的`/api/loh/84976`記憶體設定檔,僅分配*了一個字節*:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-267">The following chart shows the memory profile of calling the `/api/loh/84976` endpoint, allocating *just one more byte*:</span></span>

![前一圖表](memory/_static/loh2.png)

<span data-ttu-id="cf3aa-269">注意:結構`byte[]`具有開銷位元組。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-269">Note: The `byte[]` structure has overhead bytes.</span></span> <span data-ttu-id="cf3aa-270">這就是為什麼 84,976 位元組觸發 85,000 限制的原因。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-270">That's why 84,976 bytes triggers the 85,000 limit.</span></span>

<span data-ttu-id="cf3aa-271">比較前面的兩個圖表:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-271">Comparing the two preceding charts:</span></span>

* <span data-ttu-id="cf3aa-272">這兩種情況的工作集都類似,大約 450 MB。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-272">The working set is similar for both scenarios, about 450 MB.</span></span>
* <span data-ttu-id="cf3aa-273">LOH 下的請求(84,975 位元組)主要顯示第0代集合。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-273">The under LOH requests (84,975 bytes) shows mostly generation 0 collections.</span></span>
* <span data-ttu-id="cf3aa-274">過 LOH 請求生成恆定的第 2 代集合。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-274">The over LOH requests generate constant generation 2 collections.</span></span> <span data-ttu-id="cf3aa-275">第 2 代集合成本高昂。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-275">Generation 2 collections are expensive.</span></span> <span data-ttu-id="cf3aa-276">需要更多的 CPU,輸送量下降近 50%。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-276">More CPU is required and throughput drops almost 50%.</span></span>

<span data-ttu-id="cf3aa-277">臨時大型物件特別成問題,因為它們會導致第 2 代 GC。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-277">Temporary large objects are particularly problematic because they cause gen2 GCs.</span></span>

<span data-ttu-id="cf3aa-278">為了達到最佳性能,應盡量減少大型物件的使用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-278">For maximum performance, large object use should be minimized.</span></span> <span data-ttu-id="cf3aa-279">如果可能,拆分大型物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-279">If possible, split up large objects.</span></span> <span data-ttu-id="cf3aa-280">例如,ASP.NET Core 中的[回應緩存](xref:performance/caching/response)中間件將緩存條目拆分為小於 85,000 位元組的塊。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-280">For example, [Response Caching](xref:performance/caching/response) middleware in ASP.NET Core split the cache entries into blocks less than 85,000 bytes.</span></span>

<span data-ttu-id="cf3aa-281">以下連結顯示了將物件保持在 LOH 限制下的 ASP.NET 核心方法:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-281">The following links show the ASP.NET Core approach to keeping objects under the LOH limit:</span></span>

* [<span data-ttu-id="cf3aa-282">回應快取/流/流實用程式。cs</span><span class="sxs-lookup"><span data-stu-id="cf3aa-282">ResponseCaching/Streams/StreamUtilities.cs</span></span>](https://github.com/dotnet/AspNetCore/blob/v3.0.0/src/Middleware/ResponseCaching/src/Streams/StreamUtilities.cs#L16)
* [<span data-ttu-id="cf3aa-283">回應快取/記憶體回應快取.cs</span><span class="sxs-lookup"><span data-stu-id="cf3aa-283">ResponseCaching/MemoryResponseCache.cs</span></span>](https://github.com/aspnet/ResponseCaching/blob/c1cb7576a0b86e32aec990c22df29c780af29ca5/src/Microsoft.AspNetCore.ResponseCaching/Internal/MemoryResponseCache.cs#L55)

<span data-ttu-id="cf3aa-284">如需詳細資訊，請參閱</span><span class="sxs-lookup"><span data-stu-id="cf3aa-284">For more information, see:</span></span>

* [<span data-ttu-id="cf3aa-285">未覆寫的大物件堆</span><span class="sxs-lookup"><span data-stu-id="cf3aa-285">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="cf3aa-286">大型物件堆</span><span class="sxs-lookup"><span data-stu-id="cf3aa-286">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)

### <a name="httpclient"></a><span data-ttu-id="cf3aa-287">HttpClient</span><span class="sxs-lookup"><span data-stu-id="cf3aa-287">HttpClient</span></span>

<span data-ttu-id="cf3aa-288">使用<xref:System.Net.Http.HttpClient>不當可能會導致資源洩漏。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-288">Incorrectly using <xref:System.Net.Http.HttpClient> can result in a resource leak.</span></span> <span data-ttu-id="cf3aa-289">系統資源,如資料庫連接、套接字、檔句柄等:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-289">System resources, such as database connections, sockets, file handles, etc.:</span></span>

* <span data-ttu-id="cf3aa-290">比記憶更稀缺。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-290">Are more scarce than memory.</span></span>
* <span data-ttu-id="cf3aa-291">洩漏時比記憶體問題更大。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-291">Are more problematic when leaked than memory.</span></span>

<span data-ttu-id="cf3aa-292">經驗豐富的 .NET 開發<xref:System.IDisposable.Dispose*>人員知道<xref:System.IDisposable>調用實現 的物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-292">Experienced .NET developers know to call <xref:System.IDisposable.Dispose*> on objects that implement <xref:System.IDisposable>.</span></span> <span data-ttu-id="cf3aa-293">不釋放實現`IDisposable`的物件通常會導致記憶體洩漏或系統資源洩漏。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-293">Not disposing objects that implement `IDisposable` typically results in leaked memory or leaked system resources.</span></span>

<span data-ttu-id="cf3aa-294">`HttpClient`實現`IDisposable`,但**不應**在每一次調用時都處置。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-294">`HttpClient` implements `IDisposable`, but should **not** be disposed on every invocation.</span></span> <span data-ttu-id="cf3aa-295">相反,`HttpClient`應該重複使用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-295">Rather, `HttpClient` should be reused.</span></span>

<span data-ttu-id="cf3aa-296">以下終結點在每個請求上建立並釋放一`HttpClient`個新實例:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-296">The following endpoint creates and disposes a new  `HttpClient` instance on every request:</span></span>

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

<span data-ttu-id="cf3aa-297">在負載下,將記錄以下錯誤訊息:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-297">Under load, the following error messages are logged:</span></span>

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

<span data-ttu-id="cf3aa-298">即使`HttpClient`實例已釋放,操作系統也會釋放實際網路連接。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-298">Even though the `HttpClient` instances are disposed, the actual network connection takes some time to be released by the operating system.</span></span> <span data-ttu-id="cf3aa-299">以不斷建立新連線,_連接埠耗盡_。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-299">By continuously creating new connections, _ports exhaustion_ occurs.</span></span> <span data-ttu-id="cf3aa-300">每個用戶端連接都需要其自己的用戶端埠。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-300">Each client connection requires its own client port.</span></span>

<span data-ttu-id="cf3aa-301">防止連接埠耗盡的一種方法是重用同一`HttpClient`實例:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-301">One way to prevent port exhaustion is to reuse the same `HttpClient` instance:</span></span>

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

<span data-ttu-id="cf3aa-302">`HttpClient`當應用停止時,實例將被釋放。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-302">The `HttpClient` instance is released when the app stops.</span></span> <span data-ttu-id="cf3aa-303">此示例顯示,在每次使用后,不應釋放每個一次性資源。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-303">This example shows that not every disposable resource should be disposed after each use.</span></span>

<span data-ttu-id="cf3aa-304">有關處理`HttpClient`實例存留期的更好方法,請參閱以下內容:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-304">See the following for a better way to handle the lifetime of an `HttpClient` instance:</span></span>

* [<span data-ttu-id="cf3aa-305">HttpClient 和存留期管理</span><span class="sxs-lookup"><span data-stu-id="cf3aa-305">HttpClient and lifetime management</span></span>](/aspnet/core/fundamentals/http-requests#httpclient-and-lifetime-management)
* [<span data-ttu-id="cf3aa-306">HTTPClient 工廠部落格</span><span class="sxs-lookup"><span data-stu-id="cf3aa-306">HTTPClient factory blog</span></span>](https://devblogs.microsoft.com/aspnet/asp-net-core-2-1-preview1-introducing-httpclient-factory/)
 
### <a name="object-pooling"></a><span data-ttu-id="cf3aa-307">物件池</span><span class="sxs-lookup"><span data-stu-id="cf3aa-307">Object pooling</span></span>

<span data-ttu-id="cf3aa-308">前面的示例演示如何使`HttpClient`實例成為靜態的,並被所有請求重用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-308">The previous example showed how the `HttpClient` instance can be made static and reused by all requests.</span></span> <span data-ttu-id="cf3aa-309">重用可防止資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-309">Reuse prevents running out of resources.</span></span>

<span data-ttu-id="cf3aa-310">物件池:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-310">Object pooling:</span></span>

* <span data-ttu-id="cf3aa-311">使用重用模式。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-311">Uses the reuse pattern.</span></span>
* <span data-ttu-id="cf3aa-312">專為創建成本高昂的對象而設計。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-312">Is designed for objects that are expensive to create.</span></span>

<span data-ttu-id="cf3aa-313">池是預初始化物件的集合,可以在線程之間保留和釋放。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-313">A pool is a collection of pre-initialized objects that can be reserved and released across threads.</span></span> <span data-ttu-id="cf3aa-314">池可以定義分配規則,如限制、預定義大小或增長率。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-314">Pools can define allocation rules such as limits, predefined sizes, or growth rate.</span></span>

<span data-ttu-id="cf3aa-315">NuGet 包[Microsoft.擴展.ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/)包含有助於管理此類池的類。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-315">The NuGet package [Microsoft.Extensions.ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) contains classes that help to manage such pools.</span></span>

<span data-ttu-id="cf3aa-316">以下 API 終結點實例`byte`化了 在每個請求上填充隨機數的緩衝區:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-316">The following API endpoint instantiates a `byte` buffer that is filled with random numbers on each request:</span></span>

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

<span data-ttu-id="cf3aa-317">下圖顯示了使用中等負載呼叫前面的 API 的圖表顯示:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-317">The following chart display calling the preceding API with moderate load:</span></span>

![前一圖表](memory/_static/array.png)

<span data-ttu-id="cf3aa-319">在前面的圖表中,第 0 代集合大約每秒發生一次。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-319">In the preceding chart, generation 0 collections happen approximately once per second.</span></span>

<span data-ttu-id="cf3aa-320">可以通過使用[ArrayPool\<T>](xref:System.Buffers.ArrayPool`1)`byte`池化緩衝區來優化前面的代碼。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-320">The preceding code can be optimized by pooling the `byte` buffer by using [ArrayPool\<T>](xref:System.Buffers.ArrayPool`1).</span></span> <span data-ttu-id="cf3aa-321">靜態實例跨請求重用。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-321">A static instance is reused across requests.</span></span>

<span data-ttu-id="cf3aa-322">此方法的不同做法是從 API 返回池物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-322">What's different with this approach is that a pooled object is returned from the API.</span></span> <span data-ttu-id="cf3aa-323">這意味著:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-323">That means:</span></span>

* <span data-ttu-id="cf3aa-324">從 方法返回時,物件將失去控制。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-324">The object is out of your control as soon as you return from the method.</span></span>
* <span data-ttu-id="cf3aa-325">無法釋放物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-325">You can't release the object.</span></span>

<span data-ttu-id="cf3aa-326">要設定物件的處置:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-326">To set up disposal of the object:</span></span>

* <span data-ttu-id="cf3aa-327">將池陣組封裝在一次性物件中。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-327">Encapsulate the pooled array in a disposable object.</span></span>
* <span data-ttu-id="cf3aa-328">使用[HttpContext.Response.註冊為 Dispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*)註冊池物件。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-328">Register the pooled object with [HttpContext.Response.RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*).</span></span>

<span data-ttu-id="cf3aa-329">`RegisterForDispose`將負責對目標物件的`Dispose`調用,以便僅在 HTTP 請求完成時釋放它。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-329">`RegisterForDispose` will take care of calling `Dispose`on the target object so that it's only released when the HTTP request is complete.</span></span>

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

<span data-ttu-id="cf3aa-330">應用與非池式版本相同的負載會導致以下圖表:</span><span class="sxs-lookup"><span data-stu-id="cf3aa-330">Applying the same load as the non-pooled version results in the following chart:</span></span>

![前一圖表](memory/_static/pooledarray.png)

<span data-ttu-id="cf3aa-332">主要區別是分配位元組,因此,第 0 代集合要少得多。</span><span class="sxs-lookup"><span data-stu-id="cf3aa-332">The main difference is allocated bytes, and as a consequence much fewer generation 0 collections.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf3aa-333">其他資源</span><span class="sxs-lookup"><span data-stu-id="cf3aa-333">Additional resources</span></span>

* [<span data-ttu-id="cf3aa-334">記憶體回收</span><span class="sxs-lookup"><span data-stu-id="cf3aa-334">Garbage Collection</span></span>](/dotnet/standard/garbage-collection/)
* [<span data-ttu-id="cf3aa-335">使用並發視覺化工具瞭解不同的 GC 模式</span><span class="sxs-lookup"><span data-stu-id="cf3aa-335">Understanding different GC modes with Concurrency Visualizer</span></span>](https://blogs.msdn.microsoft.com/seteplia/2017/01/05/understanding-different-gc-modes-with-concurrency-visualizer/)
* [<span data-ttu-id="cf3aa-336">未覆寫的大物件堆</span><span class="sxs-lookup"><span data-stu-id="cf3aa-336">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="cf3aa-337">大型物件堆</span><span class="sxs-lookup"><span data-stu-id="cf3aa-337">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)
