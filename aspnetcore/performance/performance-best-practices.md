---
title: ASP.NET Core 效能最佳做法
author: mjrousos
description: ASP.NET Core 應用程式中的效能和避免常見效能問題的秘訣。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 05/10/2019
uid: performance/performance-best-practices
ms.openlocfilehash: 7651dff18f98c60057660c8946c3daa66d272f6a
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536075"
---
# <a name="aspnet-core-performance-best-practices"></a><span data-ttu-id="6583c-103">ASP.NET Core 效能最佳做法</span><span class="sxs-lookup"><span data-stu-id="6583c-103">ASP.NET Core Performance Best Practices</span></span>

<span data-ttu-id="6583c-104">藉由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="6583c-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="6583c-105">本主題提供指導方針的效能與 ASP.NET Core 的最佳作法。</span><span class="sxs-lookup"><span data-stu-id="6583c-105">This topic provides guidelines for performance best practices with ASP.NET Core.</span></span>

<a name="hot"></a>

<span data-ttu-id="6583c-106">本文件中，*程式碼路徑*定義為程式碼路徑，常被稱為和大部分的執行時間發生的位置。</span><span class="sxs-lookup"><span data-stu-id="6583c-106">In this document, a *hot code path* is defined as a code path that is frequently called and where much of the execution time occurs.</span></span> <span data-ttu-id="6583c-107">熱門的程式碼路徑通常會限制應用程式向外延展與效能。</span><span class="sxs-lookup"><span data-stu-id="6583c-107">Hot code paths typically limit app scale-out and performance.</span></span>

## <a name="cache-aggressively"></a><span data-ttu-id="6583c-108">積極地快取</span><span class="sxs-lookup"><span data-stu-id="6583c-108">Cache aggressively</span></span>

<span data-ttu-id="6583c-109">快取會討論這份文件的幾個部分。</span><span class="sxs-lookup"><span data-stu-id="6583c-109">Caching is discussed in several parts of this document.</span></span> <span data-ttu-id="6583c-110">如需詳細資訊，請參閱 <xref:performance/caching/response>。</span><span class="sxs-lookup"><span data-stu-id="6583c-110">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="avoid-blocking-calls"></a><span data-ttu-id="6583c-111">避免封鎖呼叫</span><span class="sxs-lookup"><span data-stu-id="6583c-111">Avoid blocking calls</span></span>

<span data-ttu-id="6583c-112">ASP.NET Core 應用程式應該可同時處理許多要求。</span><span class="sxs-lookup"><span data-stu-id="6583c-112">ASP.NET Core apps should be designed to process many requests simultaneously.</span></span> <span data-ttu-id="6583c-113">非同步 Api 可讓小型以處理數千個並行要求，不等待封鎖呼叫的執行緒集區。</span><span class="sxs-lookup"><span data-stu-id="6583c-113">Asynchronous APIs allow a small pool of threads to handle thousands of concurrent requests by not waiting on blocking calls.</span></span> <span data-ttu-id="6583c-114">而不是等待長時間執行同步工作完成，執行緒可以使用另一個要求。</span><span class="sxs-lookup"><span data-stu-id="6583c-114">Rather than waiting on a long-running synchronous task to complete, the thread can work on another request.</span></span>

<span data-ttu-id="6583c-115">常見的效能問題，ASP.NET Core 應用程式中的封鎖可能是非同步的呼叫。</span><span class="sxs-lookup"><span data-stu-id="6583c-115">A common performance problem in ASP.NET Core apps is blocking calls that could be asynchronous.</span></span> <span data-ttu-id="6583c-116">許多同步封鎖呼叫會導致[執行緒集區耗盡](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)且降低回應時間。</span><span class="sxs-lookup"><span data-stu-id="6583c-116">Many synchronous blocking calls lead to [Thread Pool starvation](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) and degraded response times.</span></span>

<span data-ttu-id="6583c-117">**不建議**：</span><span class="sxs-lookup"><span data-stu-id="6583c-117">**Do not**:</span></span>

* <span data-ttu-id="6583c-118">封鎖呼叫的非同步執行[Task.Wait](/dotnet/api/system.threading.tasks.task.wait)或是[Task.Result](/dotnet/api/system.threading.tasks.task-1.result)。</span><span class="sxs-lookup"><span data-stu-id="6583c-118">Block asynchronous execution by calling [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) or [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).</span></span>
* <span data-ttu-id="6583c-119">取得常見的程式碼路徑中的鎖定。</span><span class="sxs-lookup"><span data-stu-id="6583c-119">Acquire locks in common code paths.</span></span> <span data-ttu-id="6583c-120">ASP.NET Core 應用程式是架構以平行方式執行程式碼時的最有效。</span><span class="sxs-lookup"><span data-stu-id="6583c-120">ASP.NET Core apps are most performant when architected to run code in parallel.</span></span>

<span data-ttu-id="6583c-121">**建議**：</span><span class="sxs-lookup"><span data-stu-id="6583c-121">**Do**:</span></span>

* <span data-ttu-id="6583c-122">製作[經常性存取程式碼路徑](#hot)非同步。</span><span class="sxs-lookup"><span data-stu-id="6583c-122">Make [hot code paths](#hot) asynchronous.</span></span>
* <span data-ttu-id="6583c-123">以非同步方式呼叫資料存取和長時間執行作業的 Api。</span><span class="sxs-lookup"><span data-stu-id="6583c-123">Call data access and long-running operations APIs asynchronously.</span></span>
* <span data-ttu-id="6583c-124">讓控制站/Razor 頁面動作非同步。</span><span class="sxs-lookup"><span data-stu-id="6583c-124">Make controller/Razor Page actions asynchronous.</span></span> <span data-ttu-id="6583c-125">整個呼叫堆疊為了受益於[非同步/等候](/dotnet/csharp/programming-guide/concepts/async/)模式，因此是非同步的。</span><span class="sxs-lookup"><span data-stu-id="6583c-125">The entire call stack is asynchronous in order to benefit from [async/await](/dotnet/csharp/programming-guide/concepts/async/) patterns.</span></span>

<span data-ttu-id="6583c-126">程式碼剖析工具，例如[PerfView](https://github.com/Microsoft/perfview)，可用來尋找執行緒不斷新增到[執行緒集區](/windows/desktop/procthread/thread-pools)。</span><span class="sxs-lookup"><span data-stu-id="6583c-126">A profiler, such as [PerfView](https://github.com/Microsoft/perfview), can be used to find threads frequently added to the [Thread Pool](/windows/desktop/procthread/thread-pools).</span></span> <span data-ttu-id="6583c-127">`Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start`事件表示加入至執行緒集區的執行緒。</span><span class="sxs-lookup"><span data-stu-id="6583c-127">The `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` event indicates a thread added to the thread pool.</span></span> <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a><span data-ttu-id="6583c-128">最小化大型物件配置</span><span class="sxs-lookup"><span data-stu-id="6583c-128">Minimize large object allocations</span></span>

<!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core -->
<span data-ttu-id="6583c-129">[.NET Core 記憶體回收行程](/dotnet/standard/garbage-collection/)會自動管理 ASP.NET Core 應用程式中的記憶體配置與釋放。</span><span class="sxs-lookup"><span data-stu-id="6583c-129">The [.NET Core garbage collector](/dotnet/standard/garbage-collection/) manages allocation and release of memory automatically in ASP.NET Core apps.</span></span> <span data-ttu-id="6583c-130">自動記憶體回收通常表示開發人員不需要擔心如何或何時釋放記憶體。</span><span class="sxs-lookup"><span data-stu-id="6583c-130">Automatic garbage collection generally means that developers don't need to worry about how or when memory is freed.</span></span> <span data-ttu-id="6583c-131">不過，清除未參考的物件會占用 CPU 時間，因此開發人員應該盡可能減少在[經常性存取程式碼路徑](#hot)中配置物件的情況。</span><span class="sxs-lookup"><span data-stu-id="6583c-131">However, cleaning up unreferenced objects takes CPU time, so developers should minimize allocating objects in [hot code paths](#hot).</span></span> <span data-ttu-id="6583c-132">在大型物件 (> 85,000 個位元組) 上執行記憶體回收特別耗費成本。</span><span class="sxs-lookup"><span data-stu-id="6583c-132">Garbage collection is especially expensive on large objects (> 85 K bytes).</span></span> <span data-ttu-id="6583c-133">大型物件儲存在[大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)中，而且需要完整 (第 2 代) 記憶體回收來清除。</span><span class="sxs-lookup"><span data-stu-id="6583c-133">Large objects are stored on the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) and require a full (generation 2) garbage collection to clean up.</span></span> <span data-ttu-id="6583c-134">不同於第 0 代與第 1 代回收，第 2 代回收需要暫時停止應用程式執行。</span><span class="sxs-lookup"><span data-stu-id="6583c-134">Unlike generation 0 and generation 1 collections, a generation 2 collection requires a temporary suspension of app execution.</span></span> <span data-ttu-id="6583c-135">常見的大型物件配置與取消配置可能會導致不一致的效能。</span><span class="sxs-lookup"><span data-stu-id="6583c-135">Frequent allocation and de-allocation of large objects can cause inconsistent performance.</span></span>

<span data-ttu-id="6583c-136">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-136">Recommendations:</span></span>

* <span data-ttu-id="6583c-137">**請**考慮快取經常使用的大型物件。</span><span class="sxs-lookup"><span data-stu-id="6583c-137">**Do** consider caching large objects that are frequently used.</span></span> <span data-ttu-id="6583c-138">快取大型物件，可避免耗費資源的配置。</span><span class="sxs-lookup"><span data-stu-id="6583c-138">Caching large objects prevents expensive allocations.</span></span>
* <span data-ttu-id="6583c-139">**請**使用 [`ArrayPool<T>`](/dotnet/api/system.buffers.arraypool-1) 將緩衝區放入集區以儲存大型陣列。</span><span class="sxs-lookup"><span data-stu-id="6583c-139">**Do** pool buffers by using an [`ArrayPool<T>`](/dotnet/api/system.buffers.arraypool-1) to store large arrays.</span></span>
* <span data-ttu-id="6583c-140">**請勿**在[經常性存取程式碼路徑](#hot)上配置許多存留期短的大型物件。</span><span class="sxs-lookup"><span data-stu-id="6583c-140">**Do not** allocate many, short-lived large objects on [hot code paths](#hot).</span></span>

<span data-ttu-id="6583c-141">藉由檢閱中的記憶體回收 (GC) 統計資料，您可以診斷記憶體問題，例如先前所述， [PerfView](https://github.com/Microsoft/perfview)並檢查：</span><span class="sxs-lookup"><span data-stu-id="6583c-141">Memory issues, such as the preceding, can be diagnosed by reviewing garbage collection (GC) stats in [PerfView](https://github.com/Microsoft/perfview) and examining:</span></span>

* <span data-ttu-id="6583c-142">記憶體回收暫停時間。</span><span class="sxs-lookup"><span data-stu-id="6583c-142">Garbage collection pause time.</span></span>
* <span data-ttu-id="6583c-143">在記憶體回收花費的處理器時間百分比。</span><span class="sxs-lookup"><span data-stu-id="6583c-143">What percentage of the processor time is spent in garbage collection.</span></span>
* <span data-ttu-id="6583c-144">多少記憶體回收是層代 0、 1 和 2。</span><span class="sxs-lookup"><span data-stu-id="6583c-144">How many garbage collections are generation 0, 1, and 2.</span></span>

<span data-ttu-id="6583c-145">如需詳細資訊，請參閱 <c0> [ 回收和效能](/dotnet/standard/garbage-collection/performance)。</span><span class="sxs-lookup"><span data-stu-id="6583c-145">For more information, see [Garbage Collection and Performance](/dotnet/standard/garbage-collection/performance).</span></span>

## <a name="optimize-data-access"></a><span data-ttu-id="6583c-146">最佳化資料存取</span><span class="sxs-lookup"><span data-stu-id="6583c-146">Optimize Data Access</span></span>

<span data-ttu-id="6583c-147">與資料存放區和其他的遠端服務互動通常是 ASP.NET Core 應用程式最慢的一部分。</span><span class="sxs-lookup"><span data-stu-id="6583c-147">Interactions with a data store and other remote services are often the slowest parts of an ASP.NET Core app.</span></span> <span data-ttu-id="6583c-148">讀取和寫入有效的資料是很重要的良好的效能。</span><span class="sxs-lookup"><span data-stu-id="6583c-148">Reading and writing data efficiently is critical for good performance.</span></span>

<span data-ttu-id="6583c-149">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-149">Recommendations:</span></span>

* <span data-ttu-id="6583c-150">**請**以非同步方式呼叫所有資料存取 API。</span><span class="sxs-lookup"><span data-stu-id="6583c-150">**Do** call all data access APIs asynchronously.</span></span>
* <span data-ttu-id="6583c-151">**請勿**擷取比所需資料多的資料。</span><span class="sxs-lookup"><span data-stu-id="6583c-151">**Do not** retrieve more data than is necessary.</span></span> <span data-ttu-id="6583c-152">撰寫查詢來只傳回目前的 HTTP 要求所需的資料。</span><span class="sxs-lookup"><span data-stu-id="6583c-152">Write queries to return just the data that's necessary for the current HTTP request.</span></span>
* <span data-ttu-id="6583c-153">**請**考將擷取自資料庫或遠端服務的頻繁存取資料放入快取 (若可接受稍微過時的資料)。</span><span class="sxs-lookup"><span data-stu-id="6583c-153">**Do** consider caching frequently accessed data retrieved from a database or remote service if slightly out-of-date data is acceptable.</span></span> <span data-ttu-id="6583c-154">視案例而定，使用 [MemoryCache](xref:performance/caching/memory) 或 [DistributedCache](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="6583c-154">Depending on the scenario, use a [MemoryCache](xref:performance/caching/memory) or a [DistributedCache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="6583c-155">如需詳細資訊，請參閱 <xref:performance/caching/response>。</span><span class="sxs-lookup"><span data-stu-id="6583c-155">For more information, see <xref:performance/caching/response>.</span></span>
* <span data-ttu-id="6583c-156">**請**將網路來回行程降到最低。</span><span class="sxs-lookup"><span data-stu-id="6583c-156">**Do** minimize network round trips.</span></span> <span data-ttu-id="6583c-157">目標是在單一呼叫中而非數個呼叫擷取所需的資料。</span><span class="sxs-lookup"><span data-stu-id="6583c-157">The goal is to retrieve the required data in a single call rather than several calls.</span></span>
* <span data-ttu-id="6583c-158">**請**使用 Entity Framework Core 中的[無追蹤查詢](/ef/core/querying/tracking#no-tracking-queries) (針對唯讀目的存取資料時)。</span><span class="sxs-lookup"><span data-stu-id="6583c-158">**Do** use [no-tracking queries](/ef/core/querying/tracking#no-tracking-queries) in Entity Framework Core when accessing data for read-only purposes.</span></span> <span data-ttu-id="6583c-159">EF Core 能以更有效率的方式傳回無追蹤查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="6583c-159">EF Core can return the results of no-tracking queries more efficiently.</span></span>
* <span data-ttu-id="6583c-160">**請**篩選及彙總 LINQ 查詢 (例如，使用 `.Where`、`.Select` 或 `.Sum` 陳述式)，讓篩選由資料庫執行。</span><span class="sxs-lookup"><span data-stu-id="6583c-160">**Do** filter and aggregate LINQ queries (with `.Where`, `.Select`, or `.Sum` statements, for example) so that the filtering is performed by the database.</span></span>
* <span data-ttu-id="6583c-161">**請**考慮 EF Core 會解析用戶端上的一些查詢運算子，這可能會導致沒有效率的查詢執行。</span><span class="sxs-lookup"><span data-stu-id="6583c-161">**Do** consider that EF Core resolves some query operators on the client, which may lead to inefficient query execution.</span></span> <span data-ttu-id="6583c-162">如需詳細資訊，請參閱[用戶端評估效能問題](/ef/core/querying/client-eval#client-evaluation-performance-issues)。</span><span class="sxs-lookup"><span data-stu-id="6583c-162">For more information, see [Client evaluation performance issues](/ef/core/querying/client-eval#client-evaluation-performance-issues).</span></span>
* <span data-ttu-id="6583c-163">**請勿**在集合上使用投影查詢，這可能會導致執行 "N+1" 個 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="6583c-163">**Do not** use projection queries on collections, which can result in executing "N + 1" SQL queries.</span></span> <span data-ttu-id="6583c-164">如需詳細資訊，請參閱[相互關聯子查詢的最佳化](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)。</span><span class="sxs-lookup"><span data-stu-id="6583c-164">For more information, see [Optimization of correlated subqueries](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).</span></span>

<span data-ttu-id="6583c-165">請參閱[EF 高效能](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)如可改善高級別應用程式的效能的解決方法：</span><span class="sxs-lookup"><span data-stu-id="6583c-165">See [EF High Performance](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) for approaches that may improve performance in high-scale apps:</span></span>

* [<span data-ttu-id="6583c-166">DbContext 共用</span><span class="sxs-lookup"><span data-stu-id="6583c-166">DbContext pooling</span></span>](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [<span data-ttu-id="6583c-167">明確地編譯的查詢</span><span class="sxs-lookup"><span data-stu-id="6583c-167">Explicitly compiled queries</span></span>](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

<span data-ttu-id="6583c-168">我們建議您衡量前面的高效能方法，再認可程式碼基底的影響。</span><span class="sxs-lookup"><span data-stu-id="6583c-168">We recommend measuring the impact of the preceding high-performance approaches before committing the code base.</span></span> <span data-ttu-id="6583c-169">已編譯查詢的額外複雜性也不是合理的效能改進。</span><span class="sxs-lookup"><span data-stu-id="6583c-169">The additional complexity of compiled queries may not justify the performance improvement.</span></span>

<span data-ttu-id="6583c-170">藉由檢閱時間偵測到問題的查詢所花費的與的存取資料[Application Insights](/azure/application-insights/app-insights-overview)或程式碼剖析工具。</span><span class="sxs-lookup"><span data-stu-id="6583c-170">Query issues can be detected by reviewing the time spent accessing data with [Application Insights](/azure/application-insights/app-insights-overview) or with profiling tools.</span></span> <span data-ttu-id="6583c-171">大部分的資料庫也提供統計資料相關常執行的查詢。</span><span class="sxs-lookup"><span data-stu-id="6583c-171">Most databases also make statistics available concerning frequently executed queries.</span></span>

## <a name="pool-http-connections-with-httpclientfactory"></a><span data-ttu-id="6583c-172">與 HttpClientFactory 的集區 HTTP 連線</span><span class="sxs-lookup"><span data-stu-id="6583c-172">Pool HTTP connections with HttpClientFactory</span></span>

<span data-ttu-id="6583c-173">雖然[HttpClient](/dotnet/api/system.net.http.httpclient)實作`IDisposable`介面，就適合重複使用。</span><span class="sxs-lookup"><span data-stu-id="6583c-173">Although [HttpClient](/dotnet/api/system.net.http.httpclient) implements the `IDisposable` interface, it's designed for reuse.</span></span> <span data-ttu-id="6583c-174">關閉`HttpClient`執行個體保留通訊端在中開啟`TIME_WAIT`狀態一段時間。</span><span class="sxs-lookup"><span data-stu-id="6583c-174">Closed `HttpClient` instances leave sockets open in the `TIME_WAIT` state for a short period of time.</span></span> <span data-ttu-id="6583c-175">如果程式碼路徑，會建立並處置`HttpClient`物件經常會，應用程式可能會耗盡可用的通訊端。</span><span class="sxs-lookup"><span data-stu-id="6583c-175">If a code path that creates and disposes of `HttpClient` objects is frequently used, the app may exhaust available sockets.</span></span> <span data-ttu-id="6583c-176">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)引進了 ASP.NET Core 2.1 中做為方案這個問題。</span><span class="sxs-lookup"><span data-stu-id="6583c-176">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) was introduced in ASP.NET Core 2.1 as a solution to this problem.</span></span> <span data-ttu-id="6583c-177">它會處理共用的 HTTP 連線，以最佳化效能和可靠性。</span><span class="sxs-lookup"><span data-stu-id="6583c-177">It handles pooling HTTP connections to optimize performance and reliability.</span></span>

<span data-ttu-id="6583c-178">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-178">Recommendations:</span></span>

* <span data-ttu-id="6583c-179">**請勿**直接建立及處置 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6583c-179">**Do not** create and dispose of `HttpClient` instances directly.</span></span>
* <span data-ttu-id="6583c-180">**請**使用 [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) 來擷取 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="6583c-180">**Do** use [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) to retrieve `HttpClient` instances.</span></span> <span data-ttu-id="6583c-181">如需詳細資訊，請參閱[使用 HttpClientFactory 實作具有恢復功能的 HTTP 要求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)。</span><span class="sxs-lookup"><span data-stu-id="6583c-181">For more information, see [Use HttpClientFactory to implement resilient HTTP requests](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).</span></span>

## <a name="keep-common-code-paths-fast"></a><span data-ttu-id="6583c-182">快速的常見程式碼路徑</span><span class="sxs-lookup"><span data-stu-id="6583c-182">Keep common code paths fast</span></span>

<span data-ttu-id="6583c-183">您想要您的程式碼，快速、 經常呼叫的程式碼路徑都以最佳化最重要：</span><span class="sxs-lookup"><span data-stu-id="6583c-183">You want all of your code to be fast, frequently called code paths are the most critical to optimize:</span></span>

* <span data-ttu-id="6583c-184">在應用程式的要求處理管線的中介軟體元件，特別是中介軟體提早在管線中執行。</span><span class="sxs-lookup"><span data-stu-id="6583c-184">Middleware components in the app's request processing pipeline, especially middleware run early in the pipeline.</span></span> <span data-ttu-id="6583c-185">這些元件會對效能很大的影響。</span><span class="sxs-lookup"><span data-stu-id="6583c-185">These components have a large impact on performance.</span></span>
* <span data-ttu-id="6583c-186">每個要求或多個時間每個要求所執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6583c-186">Code that's executed for every request or multiple times per request.</span></span> <span data-ttu-id="6583c-187">比方說，自訂的記錄、 授權的處理常式或暫時性服務初始化。</span><span class="sxs-lookup"><span data-stu-id="6583c-187">For example, custom logging, authorization handlers, or initialization of transient services.</span></span>

<span data-ttu-id="6583c-188">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-188">Recommendations:</span></span>

* <span data-ttu-id="6583c-189">**請勿**搭配長時間執行的工作使用自訂中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="6583c-189">**Do not** use custom middleware components with long-running tasks.</span></span>
* <span data-ttu-id="6583c-190">**請**使用效能分析工具 (例如 [Visual Studio 診斷工具](/visualstudio/profiling/profiling-feature-tour)或 [PerfView](https://github.com/Microsoft/perfview)) 來識別[經常性存取程式碼路徑](#hot)。</span><span class="sxs-lookup"><span data-stu-id="6583c-190">**Do** use performance profiling tools, such as [Visual Studio Diagnostic Tools](/visualstudio/profiling/profiling-feature-tour) or [PerfView](https://github.com/Microsoft/perfview)), to identify [hot code paths](#hot).</span></span>

## <a name="complete-long-running-tasks-outside-of-http-requests"></a><span data-ttu-id="6583c-191">完成長時間執行的工作以外的 HTTP 要求</span><span class="sxs-lookup"><span data-stu-id="6583c-191">Complete long-running Tasks outside of HTTP requests</span></span>

<span data-ttu-id="6583c-192">控制站或呼叫必要的服務，並傳回 HTTP 回應的頁面模型可以處理大部分的要求，以將 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="6583c-192">Most requests to an ASP.NET Core app can be handled by a controller or page model calling necessary services and returning an HTTP response.</span></span> <span data-ttu-id="6583c-193">某些涉及長時間執行工作的要求，最好讓整個要求-回應程序的非同步。</span><span class="sxs-lookup"><span data-stu-id="6583c-193">For some requests that involve long-running tasks, it's better to make the entire request-response process asynchronous.</span></span>

<span data-ttu-id="6583c-194">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-194">Recommendations:</span></span>

* <span data-ttu-id="6583c-195">**請勿**等候長時間執行的工作在一般 HTTP 要求處理期間完成。</span><span class="sxs-lookup"><span data-stu-id="6583c-195">**Do not** wait for long-running tasks to complete as part of ordinary HTTP request processing.</span></span>
* <span data-ttu-id="6583c-196">**請**考慮透過[背景服務](xref:fundamentals/host/hosted-services)或透過 [Azure Function](/azure/azure-functions/) 在處理序外處理長時間執行的要求。</span><span class="sxs-lookup"><span data-stu-id="6583c-196">**Do** consider handling long-running requests with [background services](xref:fundamentals/host/hosted-services) or out of process with an [Azure Function](/azure/azure-functions/).</span></span> <span data-ttu-id="6583c-197">對於需要大量 CPU 的工作，在處理序外完成工作特別有幫助。</span><span class="sxs-lookup"><span data-stu-id="6583c-197">Completing work out-of-process is especially beneficial for CPU-intensive tasks.</span></span>
* <span data-ttu-id="6583c-198">**請**使用即時通訊選項 (例如 [SignalR](xref:signalr/introduction))，以非同步方式與用戶端通訊。</span><span class="sxs-lookup"><span data-stu-id="6583c-198">**Do** use real-time communication options, such as [SignalR](xref:signalr/introduction), to communicate with clients asynchronously.</span></span>

## <a name="minify-client-assets"></a><span data-ttu-id="6583c-199">縮短用戶端資產</span><span class="sxs-lookup"><span data-stu-id="6583c-199">Minify client assets</span></span>

<span data-ttu-id="6583c-200">使用複雜的前端的 ASP.NET Core 應用程式經常會提供許多 JavaScript、 CSS 或影像檔。</span><span class="sxs-lookup"><span data-stu-id="6583c-200">ASP.NET Core apps with complex front-ends frequently serve many JavaScript, CSS, or image files.</span></span> <span data-ttu-id="6583c-201">初始載入要求的效能可改善：</span><span class="sxs-lookup"><span data-stu-id="6583c-201">Performance of initial load requests can be improved by:</span></span>

* <span data-ttu-id="6583c-202">合併，將多個檔案合併成一個。</span><span class="sxs-lookup"><span data-stu-id="6583c-202">Bundling, which combines multiple files into one.</span></span>
* <span data-ttu-id="6583c-203">壓縮，藉由移除空白字元和註解減少檔案大小。</span><span class="sxs-lookup"><span data-stu-id="6583c-203">Minifying, which reduces the size of files by removing whitespace and comments.</span></span>

<span data-ttu-id="6583c-204">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-204">Recommendations:</span></span>

* <span data-ttu-id="6583c-205">**請**使用 ASP.NET Core 的[內建支援](xref:client-side/bundling-and-minification)來包裝及縮小用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="6583c-205">**Do** use ASP.NET Core's [built-in support](xref:client-side/bundling-and-minification) for bundling and minifying client assets.</span></span>
* <span data-ttu-id="6583c-206">**請**考慮其他協力廠商工具 (例如 [Webpack](https://webpack.js.org/)) 來進行複雜的用戶端資產管理。</span><span class="sxs-lookup"><span data-stu-id="6583c-206">**Do** consider other third-party tools, such as [Webpack](https://webpack.js.org/), for complex client asset management.</span></span>

## <a name="compress-responses"></a><span data-ttu-id="6583c-207">壓縮回應</span><span class="sxs-lookup"><span data-stu-id="6583c-207">Compress responses</span></span>

 <span data-ttu-id="6583c-208">減少回應的大小通常應用程式的回應速度通常會大幅增加。</span><span class="sxs-lookup"><span data-stu-id="6583c-208">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="6583c-209">減少承載大小的方法之一是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="6583c-209">One way to reduce payload sizes is to compress an app's responses.</span></span> <span data-ttu-id="6583c-210">如需詳細資訊，請參閱 <c0> [ 回應壓縮](xref:performance/response-compression)。</span><span class="sxs-lookup"><span data-stu-id="6583c-210">For more information, see [Response compression](xref:performance/response-compression).</span></span>

## <a name="use-the-latest-aspnet-core-release"></a><span data-ttu-id="6583c-211">使用最新的 ASP.NET Core 版本</span><span class="sxs-lookup"><span data-stu-id="6583c-211">Use the latest ASP.NET Core release</span></span>

<span data-ttu-id="6583c-212">ASP.NET Core 的每個新版本包含效能改進。</span><span class="sxs-lookup"><span data-stu-id="6583c-212">Each new release of ASP.NET Core includes performance improvements.</span></span> <span data-ttu-id="6583c-213">.NET Core 與 ASP.NET Core 中的最佳化表示較新版本通常勝過較舊版本。</span><span class="sxs-lookup"><span data-stu-id="6583c-213">Optimizations in .NET Core and ASP.NET Core mean that newer versions generally outperform older versions.</span></span> <span data-ttu-id="6583c-214">例如，.NET Core 2.1 新增支援編譯的規則運算式，並受惠[ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6583c-214">For example, .NET Core 2.1 added support for compiled regular expressions and benefitted from [`Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx).</span></span> <span data-ttu-id="6583c-215">新增 ASP.NET Core 2.2 支援 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="6583c-215">ASP.NET Core 2.2 added support for HTTP/2.</span></span> <span data-ttu-id="6583c-216">如果效能是優先考量，請考慮升級至目前版本的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="6583c-216">If performance is a priority, consider upgrading to the current version of ASP.NET Core.</span></span>

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a><span data-ttu-id="6583c-217">最小化例外狀況</span><span class="sxs-lookup"><span data-stu-id="6583c-217">Minimize exceptions</span></span>

<span data-ttu-id="6583c-218">例外狀況應該很少見。</span><span class="sxs-lookup"><span data-stu-id="6583c-218">Exceptions should be rare.</span></span> <span data-ttu-id="6583c-219">擲回和攔截例外狀況是相對於其他程式碼流程模式變慢。</span><span class="sxs-lookup"><span data-stu-id="6583c-219">Throwing and catching exceptions is slow relative to other code flow patterns.</span></span> <span data-ttu-id="6583c-220">因為這個緣故，例外狀況，不應該用來控制一般程式流程中。</span><span class="sxs-lookup"><span data-stu-id="6583c-220">Because of this, exceptions shouldn't be used to control normal program flow.</span></span>

<span data-ttu-id="6583c-221">建議：</span><span class="sxs-lookup"><span data-stu-id="6583c-221">Recommendations:</span></span>

* <span data-ttu-id="6583c-222">**請勿**使用擲回或攔截例外狀況做為一般程式流程，尤其是在[經常性存取程式碼路徑](#hot)中。</span><span class="sxs-lookup"><span data-stu-id="6583c-222">**Do not** use throwing or catching exceptions as a means of normal program flow, especially in [hot code paths](#hot).</span></span>
* <span data-ttu-id="6583c-223">**請**在應用程式中納入邏輯，以偵測並處理會造成例外狀況的情況。</span><span class="sxs-lookup"><span data-stu-id="6583c-223">**Do** include logic in the app to detect and handle conditions that would cause an exception.</span></span>
* <span data-ttu-id="6583c-224">**請**擲回或攔截異常或非預期狀況的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6583c-224">**Do** throw or catch exceptions for unusual or unexpected conditions.</span></span>

<span data-ttu-id="6583c-225">應用程式診斷工具，例如 Application Insights 可協助識別可能會影響效能的應用程式中的常見例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6583c-225">App diagnostic tools, such as Application Insights, can help to identify common exceptions in an app that may affect performance.</span></span>
