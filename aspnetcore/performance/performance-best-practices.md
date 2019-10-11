---
title: ASP.NET Core 效能最佳做法
author: mjrousos
description: 提升 ASP.NET Core 應用程式效能及避免常見效能問題的秘訣。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/26/2019
uid: performance/performance-best-practices
ms.openlocfilehash: a2952f5234cdef7f749a1af8dd4adcb887290629
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259769"
---
# <a name="aspnet-core-performance-best-practices"></a><span data-ttu-id="d5f55-103">ASP.NET Core 效能最佳做法</span><span class="sxs-lookup"><span data-stu-id="d5f55-103">ASP.NET Core Performance Best Practices</span></span>

<span data-ttu-id="d5f55-104">由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="d5f55-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="d5f55-105">本文提供 ASP.NET Core 的效能最佳作法指導方針。</span><span class="sxs-lookup"><span data-stu-id="d5f55-105">This article provides guidelines for performance best practices with ASP.NET Core.</span></span>

## <a name="cache-aggressively"></a><span data-ttu-id="d5f55-106">主動快取</span><span class="sxs-lookup"><span data-stu-id="d5f55-106">Cache aggressively</span></span>

<span data-ttu-id="d5f55-107">本檔的數個部分會討論快取。</span><span class="sxs-lookup"><span data-stu-id="d5f55-107">Caching is discussed in several parts of this document.</span></span> <span data-ttu-id="d5f55-108">如需詳細資訊，請參閱<xref:performance/caching/response>。</span><span class="sxs-lookup"><span data-stu-id="d5f55-108">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="understand-hot-code-paths"></a><span data-ttu-id="d5f55-109">瞭解熱程式碼路徑</span><span class="sxs-lookup"><span data-stu-id="d5f55-109">Understand hot code paths</span></span>

<span data-ttu-id="d5f55-110">在本檔中，*熱程式碼路徑*會定義為經常呼叫的程式碼路徑，而且會發生大部分的執行時間。</span><span class="sxs-lookup"><span data-stu-id="d5f55-110">In this document, a *hot code path* is defined as a code path that is frequently called and where much of the execution time occurs.</span></span> <span data-ttu-id="d5f55-111">熱程式碼路徑通常會限制應用程式相應放大和效能，並在本檔的數個部分中討論。</span><span class="sxs-lookup"><span data-stu-id="d5f55-111">Hot code paths typically limit app scale-out and performance and are discussed in several parts of this document.</span></span>

## <a name="avoid-blocking-calls"></a><span data-ttu-id="d5f55-112">避免封鎖呼叫</span><span class="sxs-lookup"><span data-stu-id="d5f55-112">Avoid blocking calls</span></span>

<span data-ttu-id="d5f55-113">ASP.NET Core 應用程式應設計為同時處理許多要求。</span><span class="sxs-lookup"><span data-stu-id="d5f55-113">ASP.NET Core apps should be designed to process many requests simultaneously.</span></span> <span data-ttu-id="d5f55-114">非同步 Api 允許小型的執行緒集區，藉由不等待封鎖呼叫來處理數以千計的並行要求。</span><span class="sxs-lookup"><span data-stu-id="d5f55-114">Asynchronous APIs allow a small pool of threads to handle thousands of concurrent requests by not waiting on blocking calls.</span></span> <span data-ttu-id="d5f55-115">執行緒可以處理另一個要求，而不是等待長時間執行的同步工作完成。</span><span class="sxs-lookup"><span data-stu-id="d5f55-115">Rather than waiting on a long-running synchronous task to complete, the thread can work on another request.</span></span>

<span data-ttu-id="d5f55-116">ASP.NET Core 應用程式中常見的效能問題是封鎖可能是非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="d5f55-116">A common performance problem in ASP.NET Core apps is blocking calls that could be asynchronous.</span></span> <span data-ttu-id="d5f55-117">許多同步封鎖呼叫會導致[執行緒集](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)區耗盡並降低回應時間。</span><span class="sxs-lookup"><span data-stu-id="d5f55-117">Many synchronous blocking calls lead to [Thread Pool starvation](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) and degraded response times.</span></span>

<span data-ttu-id="d5f55-118">**不建議**：</span><span class="sxs-lookup"><span data-stu-id="d5f55-118">**Do not**:</span></span>

* <span data-ttu-id="d5f55-119">藉由呼叫工作來封鎖非同步執行。 [Wait](/dotnet/api/system.threading.tasks.task.wait)或[task。 Result](/dotnet/api/system.threading.tasks.task-1.result)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-119">Block asynchronous execution by calling [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) or [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).</span></span>
* <span data-ttu-id="d5f55-120">取得通用程式碼路徑中的鎖定。</span><span class="sxs-lookup"><span data-stu-id="d5f55-120">Acquire locks in common code paths.</span></span> <span data-ttu-id="d5f55-121">在架構上平行執行程式碼時，ASP.NET Core apps 最具效能。</span><span class="sxs-lookup"><span data-stu-id="d5f55-121">ASP.NET Core apps are most performant when architected to run code in parallel.</span></span>

<span data-ttu-id="d5f55-122">**建議**：</span><span class="sxs-lookup"><span data-stu-id="d5f55-122">**Do**:</span></span>

* <span data-ttu-id="d5f55-123">將[熱程式碼路徑](#understand-hot-code-paths)設為非同步。</span><span class="sxs-lookup"><span data-stu-id="d5f55-123">Make [hot code paths](#understand-hot-code-paths) asynchronous.</span></span>
* <span data-ttu-id="d5f55-124">以非同步方式呼叫資料存取和長時間執行的作業 Api。</span><span class="sxs-lookup"><span data-stu-id="d5f55-124">Call data access and long-running operations APIs asynchronously.</span></span>
* <span data-ttu-id="d5f55-125">讓控制站/Razor 頁面動作非同步。</span><span class="sxs-lookup"><span data-stu-id="d5f55-125">Make controller/Razor Page actions asynchronous.</span></span> <span data-ttu-id="d5f55-126">整個呼叫堆疊為了受益於[非同步/等候](/dotnet/csharp/programming-guide/concepts/async/)模式，因此是非同步的。</span><span class="sxs-lookup"><span data-stu-id="d5f55-126">The entire call stack is asynchronous in order to benefit from [async/await](/dotnet/csharp/programming-guide/concepts/async/) patterns.</span></span>

<span data-ttu-id="d5f55-127">分析工具（例如[PerfView](https://github.com/Microsoft/perfview)）可以用來尋找經常加入[執行緒集](/windows/desktop/procthread/thread-pools)區的執行緒。</span><span class="sxs-lookup"><span data-stu-id="d5f55-127">A profiler, such as [PerfView](https://github.com/Microsoft/perfview), can be used to find threads frequently added to the [Thread Pool](/windows/desktop/procthread/thread-pools).</span></span> <span data-ttu-id="d5f55-128">@No__t-0 事件表示已加入執行緒集區的執行緒。</span><span class="sxs-lookup"><span data-stu-id="d5f55-128">The `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` event indicates a thread added to the thread pool.</span></span> <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a><span data-ttu-id="d5f55-129">最小化大型物件配置</span><span class="sxs-lookup"><span data-stu-id="d5f55-129">Minimize large object allocations</span></span>

<span data-ttu-id="d5f55-130">[.NET Core 記憶體回收行程](/dotnet/standard/garbage-collection/)會自動管理 ASP.NET Core 應用程式中的記憶體配置與釋放。</span><span class="sxs-lookup"><span data-stu-id="d5f55-130">The [.NET Core garbage collector](/dotnet/standard/garbage-collection/) manages allocation and release of memory automatically in ASP.NET Core apps.</span></span> <span data-ttu-id="d5f55-131">自動記憶體回收通常表示開發人員不需要擔心如何或何時釋放記憶體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-131">Automatic garbage collection generally means that developers don't need to worry about how or when memory is freed.</span></span> <span data-ttu-id="d5f55-132">不過，清除未參考的物件會占用 CPU 時間，因此開發人員應該盡可能減少在[經常性存取程式碼路徑](#understand-hot-code-paths)中配置物件的情況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-132">However, cleaning up unreferenced objects takes CPU time, so developers should minimize allocating objects in [hot code paths](#understand-hot-code-paths).</span></span> <span data-ttu-id="d5f55-133">在大型物件 (> 85,000 個位元組) 上執行記憶體回收特別耗費成本。</span><span class="sxs-lookup"><span data-stu-id="d5f55-133">Garbage collection is especially expensive on large objects (> 85 K bytes).</span></span> <span data-ttu-id="d5f55-134">大型物件儲存在[大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)中，而且需要完整 (第 2 代) 記憶體回收來清除。</span><span class="sxs-lookup"><span data-stu-id="d5f55-134">Large objects are stored on the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) and require a full (generation 2) garbage collection to clean up.</span></span> <span data-ttu-id="d5f55-135">不同於第 0 代與第 1 代回收，第 2 代回收需要暫時停止應用程式執行。</span><span class="sxs-lookup"><span data-stu-id="d5f55-135">Unlike generation 0 and generation 1 collections, a generation 2 collection requires a temporary suspension of app execution.</span></span> <span data-ttu-id="d5f55-136">常見的大型物件配置與取消配置可能會導致不一致的效能。</span><span class="sxs-lookup"><span data-stu-id="d5f55-136">Frequent allocation and de-allocation of large objects can cause inconsistent performance.</span></span>

<span data-ttu-id="d5f55-137">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-137">Recommendations:</span></span>

* <span data-ttu-id="d5f55-138">**請**考慮快取經常使用的大型物件。</span><span class="sxs-lookup"><span data-stu-id="d5f55-138">**Do** consider caching large objects that are frequently used.</span></span> <span data-ttu-id="d5f55-139">快取大型物件可避免耗用昂貴的配置。</span><span class="sxs-lookup"><span data-stu-id="d5f55-139">Caching large objects prevents expensive allocations.</span></span>
* <span data-ttu-id="d5f55-140">**請**使用 [`ArrayPool<T>`](/dotnet/api/system.buffers.arraypool-1) 將緩衝區放入集區以儲存大型陣列。</span><span class="sxs-lookup"><span data-stu-id="d5f55-140">**Do** pool buffers by using an [`ArrayPool<T>`](/dotnet/api/system.buffers.arraypool-1) to store large arrays.</span></span>
* <span data-ttu-id="d5f55-141">**請勿**在[經常性存取程式碼路徑](#understand-hot-code-paths)上配置許多存留期短的大型物件。</span><span class="sxs-lookup"><span data-stu-id="d5f55-141">**Do not** allocate many, short-lived large objects on [hot code paths](#understand-hot-code-paths).</span></span>

<span data-ttu-id="d5f55-142">藉由檢閱中的記憶體回收 (GC) 統計資料，您可以診斷記憶體問題，例如先前所述， [PerfView](https://github.com/Microsoft/perfview)並檢查：</span><span class="sxs-lookup"><span data-stu-id="d5f55-142">Memory issues, such as the preceding, can be diagnosed by reviewing garbage collection (GC) stats in [PerfView](https://github.com/Microsoft/perfview) and examining:</span></span>

* <span data-ttu-id="d5f55-143">垃圾收集暫停時間。</span><span class="sxs-lookup"><span data-stu-id="d5f55-143">Garbage collection pause time.</span></span>
* <span data-ttu-id="d5f55-144">花費在垃圾收集的處理器時間百分比。</span><span class="sxs-lookup"><span data-stu-id="d5f55-144">What percentage of the processor time is spent in garbage collection.</span></span>
* <span data-ttu-id="d5f55-145">層代0、1和2的垃圾收集數目。</span><span class="sxs-lookup"><span data-stu-id="d5f55-145">How many garbage collections are generation 0, 1, and 2.</span></span>

<span data-ttu-id="d5f55-146">如需詳細資訊，請參閱 <c0> [ 回收和效能](/dotnet/standard/garbage-collection/performance)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-146">For more information, see [Garbage Collection and Performance](/dotnet/standard/garbage-collection/performance).</span></span>

## <a name="optimize-data-access"></a><span data-ttu-id="d5f55-147">優化資料存取</span><span class="sxs-lookup"><span data-stu-id="d5f55-147">Optimize Data Access</span></span>

<span data-ttu-id="d5f55-148">與資料存放區和其他遠端服務的互動通常是 ASP.NET Core 應用程式中最慢的部分。</span><span class="sxs-lookup"><span data-stu-id="d5f55-148">Interactions with a data store and other remote services are often the slowest parts of an ASP.NET Core app.</span></span> <span data-ttu-id="d5f55-149">有效率地讀取和寫入資料對於良好的效能非常重要。</span><span class="sxs-lookup"><span data-stu-id="d5f55-149">Reading and writing data efficiently is critical for good performance.</span></span>

<span data-ttu-id="d5f55-150">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-150">Recommendations:</span></span>

* <span data-ttu-id="d5f55-151">**請**以非同步方式呼叫所有資料存取 API。</span><span class="sxs-lookup"><span data-stu-id="d5f55-151">**Do** call all data access APIs asynchronously.</span></span>
* <span data-ttu-id="d5f55-152">**請勿**擷取比所需資料多的資料。</span><span class="sxs-lookup"><span data-stu-id="d5f55-152">**Do not** retrieve more data than is necessary.</span></span> <span data-ttu-id="d5f55-153">撰寫查詢來只傳回目前的 HTTP 要求所需的資料。</span><span class="sxs-lookup"><span data-stu-id="d5f55-153">Write queries to return just the data that's necessary for the current HTTP request.</span></span>
* <span data-ttu-id="d5f55-154">**請**考將擷取自資料庫或遠端服務的頻繁存取資料放入快取 (若可接受稍微過時的資料)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-154">**Do** consider caching frequently accessed data retrieved from a database or remote service if slightly out-of-date data is acceptable.</span></span> <span data-ttu-id="d5f55-155">視案例而定，使用 [MemoryCache](xref:performance/caching/memory) 或 [DistributedCache](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-155">Depending on the scenario, use a [MemoryCache](xref:performance/caching/memory) or a [DistributedCache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="d5f55-156">如需詳細資訊，請參閱 <xref:performance/caching/response>。</span><span class="sxs-lookup"><span data-stu-id="d5f55-156">For more information, see <xref:performance/caching/response>.</span></span>
* <span data-ttu-id="d5f55-157">**請**將網路來回行程降到最低。</span><span class="sxs-lookup"><span data-stu-id="d5f55-157">**Do** minimize network round trips.</span></span> <span data-ttu-id="d5f55-158">目標是在單一呼叫中而非數個呼叫擷取所需的資料。</span><span class="sxs-lookup"><span data-stu-id="d5f55-158">The goal is to retrieve the required data in a single call rather than several calls.</span></span>
* <span data-ttu-id="d5f55-159">**請**使用 Entity Framework Core 中的[無追蹤查詢](/ef/core/querying/tracking#no-tracking-queries) (針對唯讀目的存取資料時)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-159">**Do** use [no-tracking queries](/ef/core/querying/tracking#no-tracking-queries) in Entity Framework Core when accessing data for read-only purposes.</span></span> <span data-ttu-id="d5f55-160">EF Core 能以更有效率的方式傳回無追蹤查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="d5f55-160">EF Core can return the results of no-tracking queries more efficiently.</span></span>
* <span data-ttu-id="d5f55-161">**請**篩選及彙總 LINQ 查詢 (例如，使用 `.Where`、`.Select` 或 `.Sum` 陳述式)，讓篩選由資料庫執行。</span><span class="sxs-lookup"><span data-stu-id="d5f55-161">**Do** filter and aggregate LINQ queries (with `.Where`, `.Select`, or `.Sum` statements, for example) so that the filtering is performed by the database.</span></span>
* <span data-ttu-id="d5f55-162">**請**考慮 EF Core 會解析用戶端上的一些查詢運算子，這可能會導致沒有效率的查詢執行。</span><span class="sxs-lookup"><span data-stu-id="d5f55-162">**Do** consider that EF Core resolves some query operators on the client, which may lead to inefficient query execution.</span></span> <span data-ttu-id="d5f55-163">如需詳細資訊，請參閱[用戶端評估效能問題](/ef/core/querying/client-eval#client-evaluation-performance-issues)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-163">For more information, see [Client evaluation performance issues](/ef/core/querying/client-eval#client-evaluation-performance-issues).</span></span>
* <span data-ttu-id="d5f55-164">**不這麼做**使用投影查詢集合，可能會導致執行 "N+1"上 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="d5f55-164">**Do not** use projection queries on collections, which can result in executing "N + 1" SQL queries.</span></span> <span data-ttu-id="d5f55-165">如需詳細資訊，請參閱 [相互關聯子查詢的最佳化](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-165">For more information, see [Optimization of correlated subqueries](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).</span></span>

<span data-ttu-id="d5f55-166">如需可改善高擴充應用程式效能的方法，請參閱[EF 高效](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)能：</span><span class="sxs-lookup"><span data-stu-id="d5f55-166">See [EF High Performance](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) for approaches that may improve performance in high-scale apps:</span></span>

* [<span data-ttu-id="d5f55-167">DbCoNtext 共用</span><span class="sxs-lookup"><span data-stu-id="d5f55-167">DbContext pooling</span></span>](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [<span data-ttu-id="d5f55-168">明確編譯的查詢</span><span class="sxs-lookup"><span data-stu-id="d5f55-168">Explicitly compiled queries</span></span>](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

<span data-ttu-id="d5f55-169">我們建議您先測量前述高效能方法的影響，再認可程式碼基底。</span><span class="sxs-lookup"><span data-stu-id="d5f55-169">We recommend measuring the impact of the preceding high-performance approaches before committing the code base.</span></span> <span data-ttu-id="d5f55-170">已編譯查詢的額外複雜度可能不會證明效能改進。</span><span class="sxs-lookup"><span data-stu-id="d5f55-170">The additional complexity of compiled queries may not justify the performance improvement.</span></span>

<span data-ttu-id="d5f55-171">藉由[Application Insights](/azure/application-insights/app-insights-overview)或使用程式碼剖析工具來查看存取資料所花費的時間，可以偵測到查詢問題。</span><span class="sxs-lookup"><span data-stu-id="d5f55-171">Query issues can be detected by reviewing the time spent accessing data with [Application Insights](/azure/application-insights/app-insights-overview) or with profiling tools.</span></span> <span data-ttu-id="d5f55-172">大部分的資料庫也會提供有關經常執行之查詢的統計資料。</span><span class="sxs-lookup"><span data-stu-id="d5f55-172">Most databases also make statistics available concerning frequently executed queries.</span></span>

## <a name="pool-http-connections-with-httpclientfactory"></a><span data-ttu-id="d5f55-173">使用 HttpClientFactory 集區 HTTP 連線</span><span class="sxs-lookup"><span data-stu-id="d5f55-173">Pool HTTP connections with HttpClientFactory</span></span>

<span data-ttu-id="d5f55-174">雖然[HttpClient](/dotnet/api/system.net.http.httpclient)會執行 @no__t 1 介面，但它是為了重複使用而設計的。</span><span class="sxs-lookup"><span data-stu-id="d5f55-174">Although [HttpClient](/dotnet/api/system.net.http.httpclient) implements the `IDisposable` interface, it's designed for reuse.</span></span> <span data-ttu-id="d5f55-175">封閉式 `HttpClient` 實例會讓通訊端在 `TIME_WAIT` 狀態中開啟一小段時間。</span><span class="sxs-lookup"><span data-stu-id="d5f55-175">Closed `HttpClient` instances leave sockets open in the `TIME_WAIT` state for a short period of time.</span></span> <span data-ttu-id="d5f55-176">如果經常使用建立和處置 `HttpClient` 物件的程式碼路徑，應用程式可能會耗盡可用的通訊端。</span><span class="sxs-lookup"><span data-stu-id="d5f55-176">If a code path that creates and disposes of `HttpClient` objects is frequently used, the app may exhaust available sockets.</span></span> <span data-ttu-id="d5f55-177">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)是在 ASP.NET Core 2.1 中引進，做為此問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="d5f55-177">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) was introduced in ASP.NET Core 2.1 as a solution to this problem.</span></span> <span data-ttu-id="d5f55-178">它會處理共用 HTTP 連線，以優化效能和可靠性。</span><span class="sxs-lookup"><span data-stu-id="d5f55-178">It handles pooling HTTP connections to optimize performance and reliability.</span></span>

<span data-ttu-id="d5f55-179">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-179">Recommendations:</span></span>

* <span data-ttu-id="d5f55-180">**請勿**直接建立及處置 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-180">**Do not** create and dispose of `HttpClient` instances directly.</span></span>
* <span data-ttu-id="d5f55-181">**請**使用 [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) 來擷取 `HttpClient` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-181">**Do** use [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) to retrieve `HttpClient` instances.</span></span> <span data-ttu-id="d5f55-182">如需詳細資訊，請參閱[使用 HttpClientFactory 實作具有恢復功能的 HTTP 要求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-182">For more information, see [Use HttpClientFactory to implement resilient HTTP requests](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).</span></span>

## <a name="keep-common-code-paths-fast"></a><span data-ttu-id="d5f55-183">快速保持通用程式碼路徑</span><span class="sxs-lookup"><span data-stu-id="d5f55-183">Keep common code paths fast</span></span>

<span data-ttu-id="d5f55-184">您想要讓所有程式碼都快速、經常被呼叫的程式碼路徑，最重要的是優化：</span><span class="sxs-lookup"><span data-stu-id="d5f55-184">You want all of your code to be fast, frequently called code paths are the most critical to optimize:</span></span>

* <span data-ttu-id="d5f55-185">應用程式要求處理管線中的中介軟體元件，特別是在管線早期執行中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-185">Middleware components in the app's request processing pipeline, especially middleware run early in the pipeline.</span></span> <span data-ttu-id="d5f55-186">這些元件對效能有很大的影響。</span><span class="sxs-lookup"><span data-stu-id="d5f55-186">These components have a large impact on performance.</span></span>
* <span data-ttu-id="d5f55-187">針對每個要求執行的程式碼，或每個要求多次。</span><span class="sxs-lookup"><span data-stu-id="d5f55-187">Code that's executed for every request or multiple times per request.</span></span> <span data-ttu-id="d5f55-188">例如，自訂記錄、授權處理常式或暫時性服務的初始化。</span><span class="sxs-lookup"><span data-stu-id="d5f55-188">For example, custom logging, authorization handlers, or initialization of transient services.</span></span>

<span data-ttu-id="d5f55-189">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-189">Recommendations:</span></span>

* <span data-ttu-id="d5f55-190">**請勿**搭配長時間執行的工作使用自訂中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="d5f55-190">**Do not** use custom middleware components with long-running tasks.</span></span>
* <span data-ttu-id="d5f55-191">**請**使用效能分析工具 (例如 [Visual Studio 診斷工具](/visualstudio/profiling/profiling-feature-tour)或 [PerfView](https://github.com/Microsoft/perfview)) 來識別[經常性存取程式碼路徑](#understand-hot-code-paths)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-191">**Do** use performance profiling tools, such as [Visual Studio Diagnostic Tools](/visualstudio/profiling/profiling-feature-tour) or [PerfView](https://github.com/Microsoft/perfview)), to identify [hot code paths](#understand-hot-code-paths).</span></span>

## <a name="complete-long-running-tasks-outside-of-http-requests"></a><span data-ttu-id="d5f55-192">完成 HTTP 要求以外的長時間執行工作</span><span class="sxs-lookup"><span data-stu-id="d5f55-192">Complete long-running Tasks outside of HTTP requests</span></span>

<span data-ttu-id="d5f55-193">對 ASP.NET Core 應用程式的大部分要求可以由呼叫必要服務並傳回 HTTP 回應的控制器或頁面模型來處理。</span><span class="sxs-lookup"><span data-stu-id="d5f55-193">Most requests to an ASP.NET Core app can be handled by a controller or page model calling necessary services and returning an HTTP response.</span></span> <span data-ttu-id="d5f55-194">對於涉及長時間執行之工作的某些要求，最好將整個要求-回應程式設為非同步。</span><span class="sxs-lookup"><span data-stu-id="d5f55-194">For some requests that involve long-running tasks, it's better to make the entire request-response process asynchronous.</span></span>

<span data-ttu-id="d5f55-195">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-195">Recommendations:</span></span>

* <span data-ttu-id="d5f55-196">**請勿**等候長時間執行的工作在一般 HTTP 要求處理期間完成。</span><span class="sxs-lookup"><span data-stu-id="d5f55-196">**Do not** wait for long-running tasks to complete as part of ordinary HTTP request processing.</span></span>
* <span data-ttu-id="d5f55-197">**請**考慮透過[背景服務](xref:fundamentals/host/hosted-services)或透過 [Azure Function](/azure/azure-functions/) 在處理序外處理長時間執行的要求。</span><span class="sxs-lookup"><span data-stu-id="d5f55-197">**Do** consider handling long-running requests with [background services](xref:fundamentals/host/hosted-services) or out of process with an [Azure Function](/azure/azure-functions/).</span></span> <span data-ttu-id="d5f55-198">對於需要大量 CPU 的工作，在處理序外完成工作特別有幫助。</span><span class="sxs-lookup"><span data-stu-id="d5f55-198">Completing work out-of-process is especially beneficial for CPU-intensive tasks.</span></span>
* <span data-ttu-id="d5f55-199">**請**使用即時通訊選項 (例如 [SignalR](xref:signalr/introduction))，以非同步方式與用戶端通訊。</span><span class="sxs-lookup"><span data-stu-id="d5f55-199">**Do** use real-time communication options, such as [SignalR](xref:signalr/introduction), to communicate with clients asynchronously.</span></span>

## <a name="minify-client-assets"></a><span data-ttu-id="d5f55-200">縮小用戶端資產</span><span class="sxs-lookup"><span data-stu-id="d5f55-200">Minify client assets</span></span>

<span data-ttu-id="d5f55-201">具有複雜前端的 ASP.NET Core 應用程式經常會提供許多 JavaScript、CSS 或影像檔案。</span><span class="sxs-lookup"><span data-stu-id="d5f55-201">ASP.NET Core apps with complex front-ends frequently serve many JavaScript, CSS, or image files.</span></span> <span data-ttu-id="d5f55-202">初始載入要求的效能可以藉由下列方式改善：</span><span class="sxs-lookup"><span data-stu-id="d5f55-202">Performance of initial load requests can be improved by:</span></span>

* <span data-ttu-id="d5f55-203">合併，將多個檔案合併成一個。</span><span class="sxs-lookup"><span data-stu-id="d5f55-203">Bundling, which combines multiple files into one.</span></span>
* <span data-ttu-id="d5f55-204">壓縮，藉由移除空白字元和註解減少檔案大小。</span><span class="sxs-lookup"><span data-stu-id="d5f55-204">Minifying, which reduces the size of files by removing whitespace and comments.</span></span>

<span data-ttu-id="d5f55-205">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-205">Recommendations:</span></span>

* <span data-ttu-id="d5f55-206">**請**使用 ASP.NET Core 的[內建支援](xref:client-side/bundling-and-minification)來包裝及縮小用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="d5f55-206">**Do** use ASP.NET Core's [built-in support](xref:client-side/bundling-and-minification) for bundling and minifying client assets.</span></span>
* <span data-ttu-id="d5f55-207">**請**考慮其他協力廠商工具 (例如 [Webpack](https://webpack.js.org/)) 來進行複雜的用戶端資產管理。</span><span class="sxs-lookup"><span data-stu-id="d5f55-207">**Do** consider other third-party tools, such as [Webpack](https://webpack.js.org/), for complex client asset management.</span></span>

## <a name="compress-responses"></a><span data-ttu-id="d5f55-208">壓縮回應</span><span class="sxs-lookup"><span data-stu-id="d5f55-208">Compress responses</span></span>

 <span data-ttu-id="d5f55-209">減少回應的大小通常可大幅提升應用程式的回應速度。</span><span class="sxs-lookup"><span data-stu-id="d5f55-209">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="d5f55-210">減少承載大小的其中一個方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="d5f55-210">One way to reduce payload sizes is to compress an app's responses.</span></span> <span data-ttu-id="d5f55-211">如需詳細資訊，請參閱 <c0> [ 回應壓縮](xref:performance/response-compression)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-211">For more information, see [Response compression](xref:performance/response-compression).</span></span>

## <a name="use-the-latest-aspnet-core-release"></a><span data-ttu-id="d5f55-212">使用最新的 ASP.NET Core 版本</span><span class="sxs-lookup"><span data-stu-id="d5f55-212">Use the latest ASP.NET Core release</span></span>

<span data-ttu-id="d5f55-213">ASP.NET Core 的每個新版本都包含效能改進。</span><span class="sxs-lookup"><span data-stu-id="d5f55-213">Each new release of ASP.NET Core includes performance improvements.</span></span> <span data-ttu-id="d5f55-214">.NET Core 和 ASP.NET Core 的優化意味著較新的版本通常會優於較舊的版本。</span><span class="sxs-lookup"><span data-stu-id="d5f55-214">Optimizations in .NET Core and ASP.NET Core mean that newer versions generally outperform older versions.</span></span> <span data-ttu-id="d5f55-215">例如，.NET Core 2.1 已從[`Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx)新增編譯之正則運算式和受惠的支援。</span><span class="sxs-lookup"><span data-stu-id="d5f55-215">For example, .NET Core 2.1 added support for compiled regular expressions and benefitted from [`Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx).</span></span> <span data-ttu-id="d5f55-216">ASP.NET Core 2.2 已新增對 HTTP/2 的支援。</span><span class="sxs-lookup"><span data-stu-id="d5f55-216">ASP.NET Core 2.2 added support for HTTP/2.</span></span> <span data-ttu-id="d5f55-217">[ASP.NET Core 3.0 新增了許多改善](xref:aspnetcore-3.0)，可減少記憶體使用量並改善輸送量。</span><span class="sxs-lookup"><span data-stu-id="d5f55-217">[ASP.NET Core 3.0 adds many improvements](xref:aspnetcore-3.0) that reduce memory usage and improve throughput.</span></span> <span data-ttu-id="d5f55-218">如果效能是優先順序，請考慮升級至目前版本的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="d5f55-218">If performance is a priority, consider upgrading to the current version of ASP.NET Core.</span></span>

## <a name="minimize-exceptions"></a><span data-ttu-id="d5f55-219">最小化例外狀況</span><span class="sxs-lookup"><span data-stu-id="d5f55-219">Minimize exceptions</span></span>

<span data-ttu-id="d5f55-220">例外狀況應該很罕見。</span><span class="sxs-lookup"><span data-stu-id="d5f55-220">Exceptions should be rare.</span></span> <span data-ttu-id="d5f55-221">相對於其他程式碼流程模式，擲回和攔截例外狀況的速度較慢。</span><span class="sxs-lookup"><span data-stu-id="d5f55-221">Throwing and catching exceptions is slow relative to other code flow patterns.</span></span> <span data-ttu-id="d5f55-222">因此，例外狀況不應該用來控制一般程式流程。</span><span class="sxs-lookup"><span data-stu-id="d5f55-222">Because of this, exceptions shouldn't be used to control normal program flow.</span></span>

<span data-ttu-id="d5f55-223">相關</span><span class="sxs-lookup"><span data-stu-id="d5f55-223">Recommendations:</span></span>

* <span data-ttu-id="d5f55-224">**請勿**使用擲回或攔截例外狀況做為一般程式流程，尤其是在[經常性存取程式碼路徑](#understand-hot-code-paths)中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-224">**Do not** use throwing or catching exceptions as a means of normal program flow, especially in [hot code paths](#understand-hot-code-paths).</span></span>
* <span data-ttu-id="d5f55-225">**請**在應用程式中納入邏輯，以偵測並處理會造成例外狀況的情況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-225">**Do** include logic in the app to detect and handle conditions that would cause an exception.</span></span>
* <span data-ttu-id="d5f55-226">**請**擲回或攔截異常或非預期狀況的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-226">**Do** throw or catch exceptions for unusual or unexpected conditions.</span></span>

<span data-ttu-id="d5f55-227">應用程式診斷工具（例如 Application Insights）有助於找出應用程式中可能會影響效能的常見例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-227">App diagnostic tools, such as Application Insights, can help to identify common exceptions in an app that may affect performance.</span></span>

## <a name="performance-and-reliability"></a><span data-ttu-id="d5f55-228">效能與可靠性</span><span class="sxs-lookup"><span data-stu-id="d5f55-228">Performance and reliability</span></span>

<span data-ttu-id="d5f55-229">下列各節提供效能秘訣和已知的可靠性問題和解決方案。</span><span class="sxs-lookup"><span data-stu-id="d5f55-229">The following sections provide performance tips and known reliability problems and solutions.</span></span>

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a><span data-ttu-id="d5f55-230">避免在 HttpRequest/HttpResponse 主體上進行同步讀取或寫入</span><span class="sxs-lookup"><span data-stu-id="d5f55-230">Avoid synchronous read or write on HttpRequest/HttpResponse body</span></span>

<span data-ttu-id="d5f55-231">ASP.NET Core 中的所有 IO 都是非同步。</span><span class="sxs-lookup"><span data-stu-id="d5f55-231">All IO in ASP.NET Core is asynchronous.</span></span> <span data-ttu-id="d5f55-232">伺服器會執行具有同步和非同步多載的 @no__t 0 介面。</span><span class="sxs-lookup"><span data-stu-id="d5f55-232">Servers implement the `Stream` interface, which has both synchronous and asynchronous overloads.</span></span> <span data-ttu-id="d5f55-233">最好的是非同步，以避免封鎖執行緒集區執行緒。</span><span class="sxs-lookup"><span data-stu-id="d5f55-233">The asynchronous ones should be preferred to avoid blocking thread pool threads.</span></span> <span data-ttu-id="d5f55-234">封鎖執行緒可能會導致執行緒集區耗盡。</span><span class="sxs-lookup"><span data-stu-id="d5f55-234">Blocking threads can lead to thread pool starvation.</span></span>

<span data-ttu-id="d5f55-235">**不要這麼做：** 下列範例使用 <xref:System.IO.StreamReader.ReadToEnd*>。</span><span class="sxs-lookup"><span data-stu-id="d5f55-235">**Do not do this:** The following example uses the <xref:System.IO.StreamReader.ReadToEnd*>.</span></span> <span data-ttu-id="d5f55-236">它會封鎖目前的執行緒來等候結果。</span><span class="sxs-lookup"><span data-stu-id="d5f55-236">It blocks the current thread to wait for the result.</span></span> <span data-ttu-id="d5f55-237">這是透過 async @ no__t-1 @no__t 0sync 的範例。</span><span class="sxs-lookup"><span data-stu-id="d5f55-237">This is an example of [sync over async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
).</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

<span data-ttu-id="d5f55-238">在上述程式碼中，`Get` 會同步地將整個 HTTP 要求主體讀取到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-238">In the preceding code, `Get` synchronously reads the entire HTTP request body into memory.</span></span> <span data-ttu-id="d5f55-239">如果用戶端緩慢上傳，應用程式會透過非同步進行同步處理。</span><span class="sxs-lookup"><span data-stu-id="d5f55-239">If the client is slowly uploading, the app is doing sync over async.</span></span> <span data-ttu-id="d5f55-240">應用程式會透過非同步進行同步處理，因為 Kestrel**不支援同步**讀取。</span><span class="sxs-lookup"><span data-stu-id="d5f55-240">The app does sync over async because Kestrel does **NOT** support synchronous reads.</span></span>

<span data-ttu-id="d5f55-241">**請這樣做：** 下列範例使用 <xref:System.IO.StreamReader.ReadToEndAsync*>，而且不會在讀取時封鎖執行緒。</span><span class="sxs-lookup"><span data-stu-id="d5f55-241">**Do this:** The following example uses <xref:System.IO.StreamReader.ReadToEndAsync*> and does not block the thread while reading.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

<span data-ttu-id="d5f55-242">上述程式碼會以非同步方式將整個 HTTP 要求主體讀取到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-242">The preceding code asynchronously reads the entire HTTP request body into memory.</span></span>

> [!WARNING]
> <span data-ttu-id="d5f55-243">如果要求很大，將整個 HTTP 要求主體讀取到記憶體中，可能會導致記憶體不足（OOM）狀況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-243">If the request is large, reading the entire HTTP request body into memory could lead to an out of memory (OOM) condition.</span></span> <span data-ttu-id="d5f55-244">OOM 可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="d5f55-244">OOM can result in a Denial Of Service.</span></span>  <span data-ttu-id="d5f55-245">如需詳細資訊，請參閱本檔中的[避免將大型要求內文或回應本文讀取到記憶體](#arlb)中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-245">For more information, see [Avoid reading large request bodies or response bodies into memory](#arlb) in this document.</span></span>

<span data-ttu-id="d5f55-246">**請這樣做：** 下列範例是使用非緩衝處理要求主體的完全非同步：</span><span class="sxs-lookup"><span data-stu-id="d5f55-246">**Do this:** The following example is fully asynchronous using a non buffered request body:</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

<span data-ttu-id="d5f55-247">上述程式碼會以非同步方式將整個 HTTP 要求主體讀取到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-247">The preceding code asynchronously reads the entire HTTP request body into memory.</span></span>

> [!WARNING]
> <span data-ttu-id="d5f55-248">如果要求很大，將整個 HTTP 要求主體讀取到記憶體中，可能會導致記憶體不足（OOM）狀況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-248">If the request is large, reading the entire HTTP request body into memory could lead to an out of memory (OOM) condition.</span></span> <span data-ttu-id="d5f55-249">OOM 可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="d5f55-249">OOM can result in a Denial Of Service.</span></span>  <span data-ttu-id="d5f55-250">如需詳細資訊，請參閱本檔中的[避免將大型要求內文或回應本文讀取到記憶體](#arlb)中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-250">For more information, see [Avoid reading large request bodies or response bodies into memory](#arlb) in this document.</span></span>

## <a name="prefer-readformasync-over-requestform"></a><span data-ttu-id="d5f55-251">偏好透過要求 ReadFormAsync。表單</span><span class="sxs-lookup"><span data-stu-id="d5f55-251">Prefer ReadFormAsync over Request.Form</span></span>

<span data-ttu-id="d5f55-252">使用 `HttpContext.Request.ReadFormAsync` 取代 `HttpContext.Request.Form`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-252">Use `HttpContext.Request.ReadFormAsync` instead of `HttpContext.Request.Form`.</span></span>
<span data-ttu-id="d5f55-253">只有在下列情況下，才可以安全地讀取 `HttpContext.Request.Form`：</span><span class="sxs-lookup"><span data-stu-id="d5f55-253">`HttpContext.Request.Form` can be safely read only with the following conditions:</span></span>

* <span data-ttu-id="d5f55-254">@No__t-0 的呼叫已讀取表單，且</span><span class="sxs-lookup"><span data-stu-id="d5f55-254">The form has been read by a call to `ReadFormAsync`, and</span></span>
* <span data-ttu-id="d5f55-255">正在使用 `HttpContext.Request.Form` 讀取快取的表單值</span><span class="sxs-lookup"><span data-stu-id="d5f55-255">The cached form value is being read using `HttpContext.Request.Form`</span></span>

<span data-ttu-id="d5f55-256">**不要這麼做：** 下列範例使用 `HttpContext.Request.Form`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-256">**Do not do this:** The following example uses `HttpContext.Request.Form`.</span></span>  <span data-ttu-id="d5f55-257">`HttpContext.Request.Form` 會使用非同步 @ no__t-2 的 @no__t 1sync，而且可能會導致執行緒集區耗盡。</span><span class="sxs-lookup"><span data-stu-id="d5f55-257">`HttpContext.Request.Form` uses [sync over async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
) and can lead to thread pool starvation.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

<span data-ttu-id="d5f55-258">**請這樣做：** 下列範例會使用 `HttpContext.Request.ReadFormAsync`，以非同步方式讀取表單主體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-258">**Do this:** The following example uses `HttpContext.Request.ReadFormAsync` to read the form body asynchronously.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a><span data-ttu-id="d5f55-259">避免將大型要求內文或回應主體讀取到記憶體中</span><span class="sxs-lookup"><span data-stu-id="d5f55-259">Avoid reading large request bodies or response bodies into memory</span></span>

<span data-ttu-id="d5f55-260">在 .NET 中，大於 85 KB 的每個物件配置最後都會出現在大型物件堆積（[LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)）中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-260">In .NET, every object allocation greater than 85 KB ends up in the large object heap ([LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)).</span></span> <span data-ttu-id="d5f55-261">大型物件的成本很高，方法有兩種：</span><span class="sxs-lookup"><span data-stu-id="d5f55-261">Large objects are expensive in two ways:</span></span>

* <span data-ttu-id="d5f55-262">配置成本很高，因為必須清除新配置之大型物件的記憶體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-262">The allocation cost is high because the memory for a newly allocated large object has to be cleared.</span></span> <span data-ttu-id="d5f55-263">CLR 保證會清除所有新設定物件的記憶體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-263">The CLR guarantees that memory for all newly allocated objects is cleared.</span></span>
* <span data-ttu-id="d5f55-264">LOH 會隨著堆積的其餘部分一起收集。</span><span class="sxs-lookup"><span data-stu-id="d5f55-264">LOH is collected with the rest of the heap.</span></span> <span data-ttu-id="d5f55-265">LOH 需要完整的[垃圾收集](/dotnet/standard/garbage-collection/fundamentals)或[Gen2 集合](/dotnet/standard/garbage-collection/fundamentals#generations)。</span><span class="sxs-lookup"><span data-stu-id="d5f55-265">LOH requires a full [garbage collection](/dotnet/standard/garbage-collection/fundamentals) or [Gen2 collection](/dotnet/standard/garbage-collection/fundamentals#generations).</span></span>

<span data-ttu-id="d5f55-266">這[篇 blog 文章](https://adamsitnik.com/Array-Pool/#the-problem)會簡單說明此問題：</span><span class="sxs-lookup"><span data-stu-id="d5f55-266">This [blog post](https://adamsitnik.com/Array-Pool/#the-problem) describes the problem succinctly:</span></span>

> <span data-ttu-id="d5f55-267">配置大型物件時，會將它標示為 Gen 2 物件。</span><span class="sxs-lookup"><span data-stu-id="d5f55-267">When a large object is allocated, it’s marked as Gen 2 object.</span></span> <span data-ttu-id="d5f55-268">不是針對小型物件的 Gen 0。</span><span class="sxs-lookup"><span data-stu-id="d5f55-268">Not Gen 0 as for small objects.</span></span> <span data-ttu-id="d5f55-269">結果是，如果您在 LOH 中用盡記憶體，GC 就會清除整個受控堆積，而不只是 LOH。</span><span class="sxs-lookup"><span data-stu-id="d5f55-269">The consequences are that if you run out of memory in LOH, GC cleans up the whole managed heap, not only LOH.</span></span> <span data-ttu-id="d5f55-270">因此，它會清除 Gen 0、Gen 1 和 Gen 2，包括 LOH。</span><span class="sxs-lookup"><span data-stu-id="d5f55-270">So it cleans up Gen 0, Gen 1 and Gen 2 including LOH.</span></span> <span data-ttu-id="d5f55-271">這稱為「完整垃圾收集」，而且是最耗時的垃圾收集。</span><span class="sxs-lookup"><span data-stu-id="d5f55-271">This is called full garbage collection and is the most time-consuming garbage collection.</span></span> <span data-ttu-id="d5f55-272">對於許多應用程式而言，這可能是可接受的。</span><span class="sxs-lookup"><span data-stu-id="d5f55-272">For many applications, it can be acceptable.</span></span> <span data-ttu-id="d5f55-273">但絕對不適用於高效能 web 伺服器，因為需要幾個海量儲存體緩衝區來處理平均 web 要求（從通訊端讀取、解壓縮、解碼 JSON & 更多）。</span><span class="sxs-lookup"><span data-stu-id="d5f55-273">But definitely not for high-performance web servers, where few big memory buffers are needed to handle an average web request (read from a socket, decompress, decode JSON & more).</span></span>

<span data-ttu-id="d5f55-274">輕鬆自在管理將大型要求或回應本文儲存成單一 `byte[]` 或 `string`：</span><span class="sxs-lookup"><span data-stu-id="d5f55-274">Naively storing a large request or response body into a single `byte[]` or `string`:</span></span>

* <span data-ttu-id="d5f55-275">可能會導致 LOH 中的空間快速耗盡。</span><span class="sxs-lookup"><span data-stu-id="d5f55-275">May result in quickly running out of space in the LOH.</span></span>
* <span data-ttu-id="d5f55-276">可能會造成應用程式的效能問題，因為執行的是完整的 Gc。</span><span class="sxs-lookup"><span data-stu-id="d5f55-276">May cause performance issues for the app because of full GCs running.</span></span>

## <a name="working-with-a-synchronous-data-processing-api"></a><span data-ttu-id="d5f55-277">使用同步資料處理 API</span><span class="sxs-lookup"><span data-stu-id="d5f55-277">Working with a synchronous data processing API</span></span>

<span data-ttu-id="d5f55-278">使用僅支援同步讀取和寫入的序列化程式/還原序列化程式時（例如， [JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)）：</span><span class="sxs-lookup"><span data-stu-id="d5f55-278">When using a serializer/de-serializer that only supports synchronous reads and writes (for example,  [JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)):</span></span>

* <span data-ttu-id="d5f55-279">將資料以非同步方式緩衝到記憶體中，然後再將它傳遞至序列化程式/還原序列化程式。</span><span class="sxs-lookup"><span data-stu-id="d5f55-279">Buffer the data into memory asynchronously before passing it into the serializer/de-serializer.</span></span>

> [!WARNING]
> <span data-ttu-id="d5f55-280">如果要求很大，可能會導致記憶體不足（OOM）狀況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-280">If the request is large, it could lead to an out of memory (OOM) condition.</span></span> <span data-ttu-id="d5f55-281">OOM 可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="d5f55-281">OOM can result in a Denial Of Service.</span></span>  <span data-ttu-id="d5f55-282">如需詳細資訊，請參閱本檔中的[避免將大型要求內文或回應本文讀取到記憶體](#arlb)中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-282">For more information, see [Avoid reading large request bodies or response bodies into memory](#arlb) in this document.</span></span>

<span data-ttu-id="d5f55-283">ASP.NET Core 3.0 預設會使用 <xref:System.Text.Json> 來進行 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="d5f55-283">ASP.NET Core 3.0 uses <xref:System.Text.Json> by default for JSON serialization.</span></span> <span data-ttu-id="d5f55-284"><xref:System.Text.Json>:</span><span class="sxs-lookup"><span data-stu-id="d5f55-284"><xref:System.Text.Json>:</span></span>

* <span data-ttu-id="d5f55-285">非同步讀取和寫入 JSON。</span><span class="sxs-lookup"><span data-stu-id="d5f55-285">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="d5f55-286">已針對 UTF-8 文字進行優化。</span><span class="sxs-lookup"><span data-stu-id="d5f55-286">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="d5f55-287">通常高於 `Newtonsoft.Json` 的效能。</span><span class="sxs-lookup"><span data-stu-id="d5f55-287">Typically higher performance than `Newtonsoft.Json`.</span></span>

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a><span data-ttu-id="d5f55-288">不要將 IHttpCoNtextAccessor 儲存在欄位中</span><span class="sxs-lookup"><span data-stu-id="d5f55-288">Do not store IHttpContextAccessor.HttpContext in a field</span></span>

<span data-ttu-id="d5f55-289">[IHttpCoNtextAccessor](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)從要求執行緒存取時，會傳回使用中要求的 @no__t 1。</span><span class="sxs-lookup"><span data-stu-id="d5f55-289">The [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext) returns the `HttpContext` of the active request when accessed from the request thread.</span></span> <span data-ttu-id="d5f55-290">@No__t-0**不**應儲存在欄位或變數中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-290">The `IHttpContextAccessor.HttpContext` should **not** be stored in a field or variable.</span></span>

<span data-ttu-id="d5f55-291">**不要這麼做：** 下列範例會將 `HttpContext` 儲存在欄位中，然後稍後嘗試使用它。</span><span class="sxs-lookup"><span data-stu-id="d5f55-291">**Do not do this:** The following example stores the `HttpContext` in a field, and then attempts to use it later.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

<span data-ttu-id="d5f55-292">上述程式碼經常會在此函式中捕捉 null 或不正確的 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-292">The preceding code frequently captures a null or incorrect `HttpContext` in the constructor.</span></span>

<span data-ttu-id="d5f55-293">**請這樣做：** 下列範例：</span><span class="sxs-lookup"><span data-stu-id="d5f55-293">**Do this:** The following example:</span></span>

* <span data-ttu-id="d5f55-294">將 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 儲存在欄位中。</span><span class="sxs-lookup"><span data-stu-id="d5f55-294">Stores the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> in a field.</span></span>
* <span data-ttu-id="d5f55-295">在正確的時間使用 `HttpContext` 欄位，並檢查是否有 `null`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-295">Uses the `HttpContext` field at the correct time and checks for `null`.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a><span data-ttu-id="d5f55-296">不要從多個執行緒存取 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="d5f55-296">Do not access HttpContext from multiple threads</span></span>

<span data-ttu-id="d5f55-297">`HttpContext`*不*是安全線程。</span><span class="sxs-lookup"><span data-stu-id="d5f55-297">`HttpContext` is *NOT* thread-safe.</span></span> <span data-ttu-id="d5f55-298">以平行方式從多個執行緒存取 `HttpContext`，可能會導致未定義的行為，例如停止回應、當機和資料損毀。</span><span class="sxs-lookup"><span data-stu-id="d5f55-298">Accessing `HttpContext` from multiple threads in parallel can result in undefined behavior such as hangs, crashes, and data corruption.</span></span>

<span data-ttu-id="d5f55-299">**不要這麼做：** 下列範例會建立三個平行要求，並記錄連出 HTTP 要求之前和之後的傳入要求路徑。</span><span class="sxs-lookup"><span data-stu-id="d5f55-299">**Do not do this:** The following example makes three parallel requests and logs the incoming request path before and after the outgoing HTTP request.</span></span> <span data-ttu-id="d5f55-300">要求路徑可從多個執行緒存取，可能會平行處理。</span><span class="sxs-lookup"><span data-stu-id="d5f55-300">The request path is accessed from multiple threads, potentially in parallel.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

<span data-ttu-id="d5f55-301">**請這樣做：** 下列範例會先複製傳入要求中的所有資料，再進行三個平行要求。</span><span class="sxs-lookup"><span data-stu-id="d5f55-301">**Do this:** The following example copies all data from the incoming request before making the three parallel requests.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a><span data-ttu-id="d5f55-302">要求完成後，請勿使用 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="d5f55-302">Do not use the HttpContext after the request is complete</span></span>

<span data-ttu-id="d5f55-303">只有在 ASP.NET Core 管線中有使用中的 HTTP 要求時，`HttpContext` 才有效。</span><span class="sxs-lookup"><span data-stu-id="d5f55-303">`HttpContext` is only valid as long as there is an active HTTP request in the ASP.NET Core pipeline.</span></span> <span data-ttu-id="d5f55-304">整個 ASP.NET Core 管線是執行每個要求的非同步委派鏈。</span><span class="sxs-lookup"><span data-stu-id="d5f55-304">The entire ASP.NET Core pipeline is an asynchronous chain of delegates that executes every request.</span></span> <span data-ttu-id="d5f55-305">當這個鏈傳回的 `Task` 完成時，就會回收 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-305">When the `Task` returned from this chain completes, the `HttpContext` is recycled.</span></span>

<span data-ttu-id="d5f55-306">**不要這麼做：** 下列範例使用 `async void`：</span><span class="sxs-lookup"><span data-stu-id="d5f55-306">**Do not do this:** The following example uses `async void`:</span></span>

* <span data-ttu-id="d5f55-307">這在 ASP.NET Core 應用程式中**一律**是不良的做法。</span><span class="sxs-lookup"><span data-stu-id="d5f55-307">Which is **ALWAYS** a bad practice in ASP.NET Core apps.</span></span>
* <span data-ttu-id="d5f55-308">在 HTTP 要求完成後，存取 `HttpResponse`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-308">Accesses the `HttpResponse` after the HTTP request is complete.</span></span>
* <span data-ttu-id="d5f55-309">導致進程當機。</span><span class="sxs-lookup"><span data-stu-id="d5f55-309">Crashes the process.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

<span data-ttu-id="d5f55-310">**請這樣做：** 下列範例會將 `Task` 傳回至架構，因此在動作完成之前，HTTP 要求不會完成。</span><span class="sxs-lookup"><span data-stu-id="d5f55-310">**Do this:** The following example returns a `Task` to the framework so the HTTP request doesn't complete until the action completes.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a><span data-ttu-id="d5f55-311">不要在背景執行緒中捕捉 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="d5f55-311">Do not capture the HttpContext in background threads</span></span>

<span data-ttu-id="d5f55-312">**不要這麼做：** 下列範例顯示關閉會從 `Controller` 屬性中捕捉 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-312">**Do not do this:** The following example shows a closure is capturing the `HttpContext` from the `Controller` property.</span></span> <span data-ttu-id="d5f55-313">這是不正確的作法，因為工作專案可能會：</span><span class="sxs-lookup"><span data-stu-id="d5f55-313">This is a bad practice because the work item could:</span></span>

* <span data-ttu-id="d5f55-314">在要求範圍外執行。</span><span class="sxs-lookup"><span data-stu-id="d5f55-314">Run outside of the request scope.</span></span>
* <span data-ttu-id="d5f55-315">嘗試讀取錯誤的 `HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-315">Attempt to read the wrong `HttpContext`.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

<span data-ttu-id="d5f55-316">**請這樣做：** 下列範例：</span><span class="sxs-lookup"><span data-stu-id="d5f55-316">**Do this:** The following example:</span></span>

* <span data-ttu-id="d5f55-317">在要求期間複製背景工作中所需的資料。</span><span class="sxs-lookup"><span data-stu-id="d5f55-317">Copies the data required in the background task during the request.</span></span>
* <span data-ttu-id="d5f55-318">未參考控制器中的任何專案。</span><span class="sxs-lookup"><span data-stu-id="d5f55-318">Doesn't reference anything from the controller.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a><span data-ttu-id="d5f55-319">不要在背景執行緒上捕捉插入至控制器的服務</span><span class="sxs-lookup"><span data-stu-id="d5f55-319">Do not capture services injected into the controllers on background threads</span></span>

<span data-ttu-id="d5f55-320">**不要這麼做：** 下列範例顯示關閉正在從 `Controller` 動作參數中捕捉 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-320">**Do not do this:** The following example shows a closure is capturing the `DbContext` from the `Controller` action parameter.</span></span> <span data-ttu-id="d5f55-321">這是不正確的作法。</span><span class="sxs-lookup"><span data-stu-id="d5f55-321">This is a bad practice.</span></span>  <span data-ttu-id="d5f55-322">工作專案可以在要求範圍外執行。</span><span class="sxs-lookup"><span data-stu-id="d5f55-322">The work item could run outside of the request scope.</span></span> <span data-ttu-id="d5f55-323">@No__t-0 的範圍是要求，導致 `ObjectDisposedException`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-323">The `ContosoDbContext` is scoped to the request, resulting in an `ObjectDisposedException`.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

<span data-ttu-id="d5f55-324">**請這樣做：** 下列範例：</span><span class="sxs-lookup"><span data-stu-id="d5f55-324">**Do this:** The following example:</span></span>

* <span data-ttu-id="d5f55-325">插入 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory>，以便在背景工作專案中建立範圍。</span><span class="sxs-lookup"><span data-stu-id="d5f55-325">Injects an <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> in order to create a scope in the background work item.</span></span> <span data-ttu-id="d5f55-326">`IServiceScopeFactory` 是單一的。</span><span class="sxs-lookup"><span data-stu-id="d5f55-326">`IServiceScopeFactory` is a singleton.</span></span>
* <span data-ttu-id="d5f55-327">在背景執行緒中建立新的相依性插入範圍。</span><span class="sxs-lookup"><span data-stu-id="d5f55-327">Creates a new dependency injection scope in the background thread.</span></span>
* <span data-ttu-id="d5f55-328">未參考控制器中的任何專案。</span><span class="sxs-lookup"><span data-stu-id="d5f55-328">Doesn't reference anything from the controller.</span></span>
* <span data-ttu-id="d5f55-329">不會從傳入要求中捕捉 `ContosoDbContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-329">Doesn't capture the `ContosoDbContext` from the incoming request.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

<span data-ttu-id="d5f55-330">下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d5f55-330">The following highlighted code:</span></span>

* <span data-ttu-id="d5f55-331">建立背景作業存留期的範圍，並從中解析服務。</span><span class="sxs-lookup"><span data-stu-id="d5f55-331">Creates a scope for the lifetime of the background operation and resolves services from it.</span></span>
* <span data-ttu-id="d5f55-332">使用來自正確範圍的 `ContosoDbContext`。</span><span class="sxs-lookup"><span data-stu-id="d5f55-332">Uses `ContosoDbContext` from the correct scope.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a><span data-ttu-id="d5f55-333">啟動回應主體之後，請勿修改狀態碼或標頭</span><span class="sxs-lookup"><span data-stu-id="d5f55-333">Do not modify the status code or headers after the response body has started</span></span>

<span data-ttu-id="d5f55-334">ASP.NET Core 不會緩衝 HTTP 回應主體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-334">ASP.NET Core does not buffer the HTTP response body.</span></span> <span data-ttu-id="d5f55-335">第一次寫入回應時：</span><span class="sxs-lookup"><span data-stu-id="d5f55-335">The first time the response is written:</span></span>

* <span data-ttu-id="d5f55-336">標頭會連同主體的該區塊一起傳送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="d5f55-336">The headers are sent along with that chunk of the body to the client.</span></span>
* <span data-ttu-id="d5f55-337">您不能再變更回應標頭。</span><span class="sxs-lookup"><span data-stu-id="d5f55-337">It's no longer possible to change response headers.</span></span>

<span data-ttu-id="d5f55-338">**不要這麼做：** 下列程式碼會在回應已經開始之後，嘗試新增回應標頭：</span><span class="sxs-lookup"><span data-stu-id="d5f55-338">**Do not do this:** The following code tries to add response headers after the response has already started:</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

<span data-ttu-id="d5f55-339">在上述程式碼中，如果 `next()` 已寫入回應，`context.Response.Headers["test"] = "test value";` 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d5f55-339">In the preceding code, `context.Response.Headers["test"] = "test value";` will throw an exception if `next()` has written to the response.</span></span>

<span data-ttu-id="d5f55-340">**請這樣做：** 下列範例會先檢查 HTTP 回應是否已啟動，然後再修改標頭。</span><span class="sxs-lookup"><span data-stu-id="d5f55-340">**Do this:** The following example checks if the HTTP response has started before modifying the headers.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

<span data-ttu-id="d5f55-341">**請這樣做：** 下列範例會在回應標頭排清至用戶端之前，使用 `HttpResponse.OnStarting` 來設定標頭。</span><span class="sxs-lookup"><span data-stu-id="d5f55-341">**Do this:** The following example uses `HttpResponse.OnStarting` to set the headers before the response headers are flushed to the client.</span></span>

<span data-ttu-id="d5f55-342">檢查回應是否未啟動，可讓您在寫入回應標頭之前，註冊將會叫用的回呼。</span><span class="sxs-lookup"><span data-stu-id="d5f55-342">Checking if the response has not started allows registering a callback that will be invoked just before response headers are written.</span></span> <span data-ttu-id="d5f55-343">檢查回應是否尚未啟動：</span><span class="sxs-lookup"><span data-stu-id="d5f55-343">Checking if the response has not started:</span></span>

* <span data-ttu-id="d5f55-344">提供可即時附加或覆寫標頭的功能。</span><span class="sxs-lookup"><span data-stu-id="d5f55-344">Provides the ability to append or override headers just in time.</span></span>
* <span data-ttu-id="d5f55-345">不需要知道管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d5f55-345">Doesn't require knowledge of the next middleware in the pipeline.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a><span data-ttu-id="d5f55-346">如果您已經開始寫入回應主體，請勿呼叫 next （）</span><span class="sxs-lookup"><span data-stu-id="d5f55-346">Do not call next() if you have already started writing to the response body</span></span>

<span data-ttu-id="d5f55-347">元件只有在可以處理和操作回應時才會被呼叫。</span><span class="sxs-lookup"><span data-stu-id="d5f55-347">Components only expect to be called if it's possible for them to handle and manipulate the response.</span></span>
