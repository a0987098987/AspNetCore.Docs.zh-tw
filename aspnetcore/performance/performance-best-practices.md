---
title: ASP.NET核心性能最佳實務
author: mjrousos
description: 提高 ASP.NET 核心應用的性能並避免常見的性能問題的提示。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/06/2020
no-loc:
- SignalR
uid: performance/performance-best-practices
ms.openlocfilehash: 068a35fbe410dad24030fe68c0dfd062b402212c
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977180"
---
# <a name="aspnet-core-performance-best-practices"></a><span data-ttu-id="6c397-103">ASP.NET核心性能最佳實務</span><span class="sxs-lookup"><span data-stu-id="6c397-103">ASP.NET Core Performance Best Practices</span></span>

<span data-ttu-id="6c397-104">由[邁克·盧梭斯](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="6c397-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="6c397-105">本文提供了ASP.NET核心的性能最佳實務指南。</span><span class="sxs-lookup"><span data-stu-id="6c397-105">This article provides guidelines for performance best practices with ASP.NET Core.</span></span>

## <a name="cache-aggressively"></a><span data-ttu-id="6c397-106">主動快取</span><span class="sxs-lookup"><span data-stu-id="6c397-106">Cache aggressively</span></span>

<span data-ttu-id="6c397-107">本文檔的幾個部分討論了緩存。</span><span class="sxs-lookup"><span data-stu-id="6c397-107">Caching is discussed in several parts of this document.</span></span> <span data-ttu-id="6c397-108">如需詳細資訊，請參閱 <xref:performance/caching/response>。</span><span class="sxs-lookup"><span data-stu-id="6c397-108">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="understand-hot-code-paths"></a><span data-ttu-id="6c397-109">瞭解熱程式路徑</span><span class="sxs-lookup"><span data-stu-id="6c397-109">Understand hot code paths</span></span>

<span data-ttu-id="6c397-110">在本文中,*熱代碼路徑*定義為經常呼叫的代碼路徑以及大部分執行時間發生的位置。</span><span class="sxs-lookup"><span data-stu-id="6c397-110">In this document, a *hot code path* is defined as a code path that is frequently called and where much of the execution time occurs.</span></span> <span data-ttu-id="6c397-111">熱代碼路徑通常限制應用橫向擴展和性能,本文檔的幾個部分對此進行討論。</span><span class="sxs-lookup"><span data-stu-id="6c397-111">Hot code paths typically limit app scale-out and performance and are discussed in several parts of this document.</span></span>

## <a name="avoid-blocking-calls"></a><span data-ttu-id="6c397-112">避免阻塞呼叫</span><span class="sxs-lookup"><span data-stu-id="6c397-112">Avoid blocking calls</span></span>

<span data-ttu-id="6c397-113">ASP.NET核心應用應設計為同時處理許多請求。</span><span class="sxs-lookup"><span data-stu-id="6c397-113">ASP.NET Core apps should be designed to process many requests simultaneously.</span></span> <span data-ttu-id="6c397-114">非同步 API 允許小線程池通過不等待阻塞調用來處理數千個併發請求。</span><span class="sxs-lookup"><span data-stu-id="6c397-114">Asynchronous APIs allow a small pool of threads to handle thousands of concurrent requests by not waiting on blocking calls.</span></span> <span data-ttu-id="6c397-115">線程可以處理另一個請求,而不是等待長時間運行的同步任務來完成。</span><span class="sxs-lookup"><span data-stu-id="6c397-115">Rather than waiting on a long-running synchronous task to complete, the thread can work on another request.</span></span>

<span data-ttu-id="6c397-116">ASP.NET Core 應用中常見的性能問題是阻止可能是非同步的調用。</span><span class="sxs-lookup"><span data-stu-id="6c397-116">A common performance problem in ASP.NET Core apps is blocking calls that could be asynchronous.</span></span> <span data-ttu-id="6c397-117">許多同步阻塞調用會導致[線程池不足](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)和回應時間降低。</span><span class="sxs-lookup"><span data-stu-id="6c397-117">Many synchronous blocking calls lead to [Thread Pool starvation](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) and degraded response times.</span></span>

<span data-ttu-id="6c397-118">**不要**:</span><span class="sxs-lookup"><span data-stu-id="6c397-118">**Do not**:</span></span>

* <span data-ttu-id="6c397-119">通過調用[Task.Wait](/dotnet/api/system.threading.tasks.task.wait)或[Task.結果](/dotnet/api/system.threading.tasks.task-1.result)來阻止非同步執行。</span><span class="sxs-lookup"><span data-stu-id="6c397-119">Block asynchronous execution by calling [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) or [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).</span></span>
* <span data-ttu-id="6c397-120">獲取公共代碼路徑中的鎖。</span><span class="sxs-lookup"><span data-stu-id="6c397-120">Acquire locks in common code paths.</span></span> <span data-ttu-id="6c397-121">ASP.NET核心應用在建構為並行運行代碼時性能最出色。</span><span class="sxs-lookup"><span data-stu-id="6c397-121">ASP.NET Core apps are most performant when architected to run code in parallel.</span></span>
* <span data-ttu-id="6c397-122">調用[任務.運行](/dotnet/api/system.threading.tasks.task.run)並立即等待它。</span><span class="sxs-lookup"><span data-stu-id="6c397-122">Call [Task.Run](/dotnet/api/system.threading.tasks.task.run) and immediately await it.</span></span> <span data-ttu-id="6c397-123">ASP.NET Core 已經在正常的線程池線程上運行應用代碼,因此調用 Task.Run 只會導致不必要的線程池計劃。</span><span class="sxs-lookup"><span data-stu-id="6c397-123">ASP.NET Core already runs app code on normal Thread Pool threads, so calling Task.Run only results in extra unnecessary Thread Pool scheduling.</span></span> <span data-ttu-id="6c397-124">即使計劃的代碼會阻止線程,Task.Run 也不會阻止這一點。</span><span class="sxs-lookup"><span data-stu-id="6c397-124">Even if the scheduled code would block a thread, Task.Run does not prevent that.</span></span>

<span data-ttu-id="6c397-125">**做**:</span><span class="sxs-lookup"><span data-stu-id="6c397-125">**Do**:</span></span>

* <span data-ttu-id="6c397-126">使[熱代碼路徑](#understand-hot-code-paths)非同步。</span><span class="sxs-lookup"><span data-stu-id="6c397-126">Make [hot code paths](#understand-hot-code-paths) asynchronous.</span></span>
* <span data-ttu-id="6c397-127">如果非同步 API 可用,則非同步 API 會非同步調用資料存取、I/O 和長期運行的操作 API。</span><span class="sxs-lookup"><span data-stu-id="6c397-127">Call data access, I/O, and long-running operations APIs asynchronously if an asynchronous API is available.</span></span> <span data-ttu-id="6c397-128">**不要**使用[Task.Run](/dotnet/api/system.threading.tasks.task.run)使同步 API 同步。</span><span class="sxs-lookup"><span data-stu-id="6c397-128">Do **not** use [Task.Run](/dotnet/api/system.threading.tasks.task.run) to make a synchronus API asynchronous.</span></span>
* <span data-ttu-id="6c397-129">使控制器/Razor 頁面操作非同步。</span><span class="sxs-lookup"><span data-stu-id="6c397-129">Make controller/Razor Page actions asynchronous.</span></span> <span data-ttu-id="6c397-130">整個調用堆疊是非同步的,以便從[非同步/等待](/dotnet/csharp/programming-guide/concepts/async/)模式中獲益。</span><span class="sxs-lookup"><span data-stu-id="6c397-130">The entire call stack is asynchronous in order to benefit from [async/await](/dotnet/csharp/programming-guide/concepts/async/) patterns.</span></span>

<span data-ttu-id="6c397-131">探查器(如[PerfView)](https://github.com/Microsoft/perfview)可用於尋找經常加入[執行到 執行緒池的線程](/windows/desktop/procthread/thread-pools)。</span><span class="sxs-lookup"><span data-stu-id="6c397-131">A profiler, such as [PerfView](https://github.com/Microsoft/perfview), can be used to find threads frequently added to the [Thread Pool](/windows/desktop/procthread/thread-pools).</span></span> <span data-ttu-id="6c397-132">該`Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start`事件指示添加到線程池的線程。</span><span class="sxs-lookup"><span data-stu-id="6c397-132">The `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` event indicates a thread added to the thread pool.</span></span> <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a><span data-ttu-id="6c397-133">最小化大型物件配置</span><span class="sxs-lookup"><span data-stu-id="6c397-133">Minimize large object allocations</span></span>

<span data-ttu-id="6c397-134">[.NET 核心垃圾回收器](/dotnet/standard/garbage-collection/)在 ASP.NET 核心應用中自動管理記憶體的分配和釋放。</span><span class="sxs-lookup"><span data-stu-id="6c397-134">The [.NET Core garbage collector](/dotnet/standard/garbage-collection/) manages allocation and release of memory automatically in ASP.NET Core apps.</span></span> <span data-ttu-id="6c397-135">自動垃圾回收通常意味著開發人員無需擔心如何或何時釋放記憶體。</span><span class="sxs-lookup"><span data-stu-id="6c397-135">Automatic garbage collection generally means that developers don't need to worry about how or when memory is freed.</span></span> <span data-ttu-id="6c397-136">但是,清理未引用的物件需要 CPU 時間,因此開發人員應盡量減少[在熱代碼路徑](#understand-hot-code-paths)中分配物件。</span><span class="sxs-lookup"><span data-stu-id="6c397-136">However, cleaning up unreferenced objects takes CPU time, so developers should minimize allocating objects in [hot code paths](#understand-hot-code-paths).</span></span> <span data-ttu-id="6c397-137">垃圾回收對於大型物件(> 85 K 位元組)特別昂貴。</span><span class="sxs-lookup"><span data-stu-id="6c397-137">Garbage collection is especially expensive on large objects (> 85 K bytes).</span></span> <span data-ttu-id="6c397-138">大型物件存儲在[大型物件堆](/dotnet/standard/garbage-collection/large-object-heap)上,需要完整(第2代)垃圾回收來清理。</span><span class="sxs-lookup"><span data-stu-id="6c397-138">Large objects are stored on the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) and require a full (generation 2) garbage collection to clean up.</span></span> <span data-ttu-id="6c397-139">與第 0 代和第 1 代集合不同,第 2 代集合需要暫時暫停應用執行。</span><span class="sxs-lookup"><span data-stu-id="6c397-139">Unlike generation 0 and generation 1 collections, a generation 2 collection requires a temporary suspension of app execution.</span></span> <span data-ttu-id="6c397-140">頻繁分配和取消分配大型物件可能會導致性能不一致。</span><span class="sxs-lookup"><span data-stu-id="6c397-140">Frequent allocation and de-allocation of large objects can cause inconsistent performance.</span></span>

<span data-ttu-id="6c397-141">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-141">Recommendations:</span></span>

* <span data-ttu-id="6c397-142">**請考慮**緩存經常使用的大型物件。</span><span class="sxs-lookup"><span data-stu-id="6c397-142">**Do** consider caching large objects that are frequently used.</span></span> <span data-ttu-id="6c397-143">緩存大型物件可防止昂貴的分配。</span><span class="sxs-lookup"><span data-stu-id="6c397-143">Caching large objects prevents expensive allocations.</span></span>
* <span data-ttu-id="6c397-144">使用[ArrayPool\<T>](/dotnet/api/system.buffers.arraypool-1)儲存大型陣列來**執行**池緩衝區。</span><span class="sxs-lookup"><span data-stu-id="6c397-144">**Do** pool buffers by using an [ArrayPool\<T>](/dotnet/api/system.buffers.arraypool-1) to store large arrays.</span></span>
* <span data-ttu-id="6c397-145">**不要**在[熱代碼路徑](#understand-hot-code-paths)上分配許多短壽命的大物件。</span><span class="sxs-lookup"><span data-stu-id="6c397-145">**Do not** allocate many, short-lived large objects on [hot code paths](#understand-hot-code-paths).</span></span>

<span data-ttu-id="6c397-146">可以通過查看[PerfView](https://github.com/Microsoft/perfview)中的垃圾回收 (GC) 統計資訊並檢查記憶體問題(如上述問題)來診斷:</span><span class="sxs-lookup"><span data-stu-id="6c397-146">Memory issues, such as the preceding, can be diagnosed by reviewing garbage collection (GC) stats in [PerfView](https://github.com/Microsoft/perfview) and examining:</span></span>

* <span data-ttu-id="6c397-147">垃圾回收暫停時間。</span><span class="sxs-lookup"><span data-stu-id="6c397-147">Garbage collection pause time.</span></span>
* <span data-ttu-id="6c397-148">在垃圾回收中花費的處理器時間的百分比。</span><span class="sxs-lookup"><span data-stu-id="6c397-148">What percentage of the processor time is spent in garbage collection.</span></span>
* <span data-ttu-id="6c397-149">有多少垃圾回收是生成 0、1 和 2。</span><span class="sxs-lookup"><span data-stu-id="6c397-149">How many garbage collections are generation 0, 1, and 2.</span></span>

<span data-ttu-id="6c397-150">有關詳細資訊,請參閱[垃圾回收和性能](/dotnet/standard/garbage-collection/performance)。</span><span class="sxs-lookup"><span data-stu-id="6c397-150">For more information, see [Garbage Collection and Performance](/dotnet/standard/garbage-collection/performance).</span></span>

## <a name="optimize-data-access-and-io"></a><span data-ttu-id="6c397-151">優化資料存取和 I/O</span><span class="sxs-lookup"><span data-stu-id="6c397-151">Optimize data access and I/O</span></span>

<span data-ttu-id="6c397-152">與資料存儲和其他遠端服務的交互通常是ASP.NET核心應用中最慢的部分。</span><span class="sxs-lookup"><span data-stu-id="6c397-152">Interactions with a data store and other remote services are often the slowest parts of an ASP.NET Core app.</span></span> <span data-ttu-id="6c397-153">高效讀取和寫入數據對於良好的性能至關重要。</span><span class="sxs-lookup"><span data-stu-id="6c397-153">Reading and writing data efficiently is critical for good performance.</span></span>

<span data-ttu-id="6c397-154">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-154">Recommendations:</span></span>

* <span data-ttu-id="6c397-155">**請**非所有資料存取 API。</span><span class="sxs-lookup"><span data-stu-id="6c397-155">**Do** call all data access APIs asynchronously.</span></span>
* <span data-ttu-id="6c397-156">**不要**檢索超過所需數據的數據。</span><span class="sxs-lookup"><span data-stu-id="6c397-156">**Do not** retrieve more data than is necessary.</span></span> <span data-ttu-id="6c397-157">編寫查詢以僅返回當前 HTTP 請求所需的數據。</span><span class="sxs-lookup"><span data-stu-id="6c397-157">Write queries to return just the data that's necessary for the current HTTP request.</span></span>
* <span data-ttu-id="6c397-158">**如果**可以接受稍微過時的數據,請考慮緩存從資料庫或遠端服務檢索的頻繁訪問的數據。</span><span class="sxs-lookup"><span data-stu-id="6c397-158">**Do** consider caching frequently accessed data retrieved from a database or remote service if slightly out-of-date data is acceptable.</span></span> <span data-ttu-id="6c397-159">依專案,使用[記憶體快取](xref:performance/caching/memory)或[分散式快取](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="6c397-159">Depending on the scenario, use a [MemoryCache](xref:performance/caching/memory) or a [DistributedCache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="6c397-160">如需詳細資訊，請參閱 <xref:performance/caching/response>。</span><span class="sxs-lookup"><span data-stu-id="6c397-160">For more information, see <xref:performance/caching/response>.</span></span>
* <span data-ttu-id="6c397-161">**盡量減少**網路往返。</span><span class="sxs-lookup"><span data-stu-id="6c397-161">**Do** minimize network round trips.</span></span> <span data-ttu-id="6c397-162">目標是在單個調用中檢索所需的數據,而不是多個調用。</span><span class="sxs-lookup"><span data-stu-id="6c397-162">The goal is to retrieve the required data in a single call rather than several calls.</span></span>
* <span data-ttu-id="6c397-163">將唯讀取資料時,**請**使用實體框架核心中的[不追蹤查詢](/ef/core/querying/tracking#no-tracking-queries)。</span><span class="sxs-lookup"><span data-stu-id="6c397-163">**Do** use [no-tracking queries](/ef/core/querying/tracking#no-tracking-queries) in Entity Framework Core when accessing data for read-only purposes.</span></span> <span data-ttu-id="6c397-164">EF Core 可以更有效地返回無跟蹤查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="6c397-164">EF Core can return the results of no-tracking queries more efficiently.</span></span>
* <span data-ttu-id="6c397-165">**執行**篩選與聚合 LINQ`.Where``.Select`查詢(`.Sum`例如 ,使用 或語句),以便由資料庫執行篩選。</span><span class="sxs-lookup"><span data-stu-id="6c397-165">**Do** filter and aggregate LINQ queries (with `.Where`, `.Select`, or `.Sum` statements, for example) so that the filtering is performed by the database.</span></span>
* <span data-ttu-id="6c397-166">**一應考慮**EF Core 解析用戶端上的一些查詢運算符,這可能導致查詢執行效率低下。</span><span class="sxs-lookup"><span data-stu-id="6c397-166">**Do** consider that EF Core resolves some query operators on the client, which may lead to inefficient query execution.</span></span> <span data-ttu-id="6c397-167">有關詳細資訊,請參閱[客戶端評估性能問題](/ef/core/querying/client-eval#client-evaluation-performance-issues)。</span><span class="sxs-lookup"><span data-stu-id="6c397-167">For more information, see [Client evaluation performance issues](/ef/core/querying/client-eval#client-evaluation-performance-issues).</span></span>
* <span data-ttu-id="6c397-168">**不要**對集合使用投影查詢,這可能導致執行"N + 1"SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="6c397-168">**Do not** use projection queries on collections, which can result in executing "N + 1" SQL queries.</span></span> <span data-ttu-id="6c397-169">有關詳細資訊,請參閱[優化相關子查詢](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)。</span><span class="sxs-lookup"><span data-stu-id="6c397-169">For more information, see [Optimization of correlated subqueries](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).</span></span>

<span data-ttu-id="6c397-170">有關可能提高大規模應用中性能的方法,請參閱[EF 高性能](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries):</span><span class="sxs-lookup"><span data-stu-id="6c397-170">See [EF High Performance](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) for approaches that may improve performance in high-scale apps:</span></span>

* [<span data-ttu-id="6c397-171">DbContext 共用</span><span class="sxs-lookup"><span data-stu-id="6c397-171">DbContext pooling</span></span>](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [<span data-ttu-id="6c397-172">明確地編譯查詢</span><span class="sxs-lookup"><span data-stu-id="6c397-172">Explicitly compiled queries</span></span>](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

<span data-ttu-id="6c397-173">我們建議在提交代碼庫之前測量上述高性能方法的影響。</span><span class="sxs-lookup"><span data-stu-id="6c397-173">We recommend measuring the impact of the preceding high-performance approaches before committing the code base.</span></span> <span data-ttu-id="6c397-174">編譯查詢的額外複雜性可能無法證明性能改進的合理性。</span><span class="sxs-lookup"><span data-stu-id="6c397-174">The additional complexity of compiled queries may not justify the performance improvement.</span></span>

<span data-ttu-id="6c397-175">可以通過查看使用[應用程式見解](/azure/application-insights/app-insights-overview)或分析工具訪問數據所花費的時間來檢測查詢問題。</span><span class="sxs-lookup"><span data-stu-id="6c397-175">Query issues can be detected by reviewing the time spent accessing data with [Application Insights](/azure/application-insights/app-insights-overview) or with profiling tools.</span></span> <span data-ttu-id="6c397-176">大多數資料庫還提供有關頻繁執行的查詢的統計資訊。</span><span class="sxs-lookup"><span data-stu-id="6c397-176">Most databases also make statistics available concerning frequently executed queries.</span></span>

## <a name="pool-http-connections-with-httpclientfactory"></a><span data-ttu-id="6c397-177">使用 HTTPClientFactory 池 HTTP 連線</span><span class="sxs-lookup"><span data-stu-id="6c397-177">Pool HTTP connections with HttpClientFactory</span></span>

<span data-ttu-id="6c397-178">儘管[HTTPClient](/dotnet/api/system.net.http.httpclient)`IDisposable`實現了該介面,但它旨在重用。</span><span class="sxs-lookup"><span data-stu-id="6c397-178">Although [HttpClient](/dotnet/api/system.net.http.httpclient) implements the `IDisposable` interface, it's designed for reuse.</span></span> <span data-ttu-id="6c397-179">關閉`HttpClient`的實例使套接字處於`TIME_WAIT`狀態短時間打開。</span><span class="sxs-lookup"><span data-stu-id="6c397-179">Closed `HttpClient` instances leave sockets open in the `TIME_WAIT` state for a short period of time.</span></span> <span data-ttu-id="6c397-180">如果經常使用創建和釋放`HttpClient`對象的代碼路徑,則應用可能會耗盡可用的套接字。</span><span class="sxs-lookup"><span data-stu-id="6c397-180">If a code path that creates and disposes of `HttpClient` objects is frequently used, the app may exhaust available sockets.</span></span> <span data-ttu-id="6c397-181">[HTTPClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)在 ASP.NET 核心 2.1 中引入,作為此問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="6c397-181">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) was introduced in ASP.NET Core 2.1 as a solution to this problem.</span></span> <span data-ttu-id="6c397-182">它處理池 HTTP 連接以優化性能和可靠性。</span><span class="sxs-lookup"><span data-stu-id="6c397-182">It handles pooling HTTP connections to optimize performance and reliability.</span></span>

<span data-ttu-id="6c397-183">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-183">Recommendations:</span></span>

* <span data-ttu-id="6c397-184">**不要**直接創建和處置`HttpClient`實例。</span><span class="sxs-lookup"><span data-stu-id="6c397-184">**Do not** create and dispose of `HttpClient` instances directly.</span></span>
* <span data-ttu-id="6c397-185">**請使用** [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)`HttpClient`檢索 實例。</span><span class="sxs-lookup"><span data-stu-id="6c397-185">**Do** use [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) to retrieve `HttpClient` instances.</span></span> <span data-ttu-id="6c397-186">有關詳細資訊,請參閱使用[HTTPClientFactory 實現彈性 HTTP 請求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)。</span><span class="sxs-lookup"><span data-stu-id="6c397-186">For more information, see [Use HttpClientFactory to implement resilient HTTP requests](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).</span></span>

## <a name="keep-common-code-paths-fast"></a><span data-ttu-id="6c397-187">保持常見程式碼路徑快速</span><span class="sxs-lookup"><span data-stu-id="6c397-187">Keep common code paths fast</span></span>

<span data-ttu-id="6c397-188">您希望所有代碼都快速。</span><span class="sxs-lookup"><span data-stu-id="6c397-188">You want all of your code to be fast.</span></span> <span data-ttu-id="6c397-189">經常調用的代碼路徑是優化的最重要路徑。</span><span class="sxs-lookup"><span data-stu-id="6c397-189">Frequently-called code paths are the most critical to optimize.</span></span> <span data-ttu-id="6c397-190">其中包括：</span><span class="sxs-lookup"><span data-stu-id="6c397-190">These include:</span></span>

* <span data-ttu-id="6c397-191">應用請求處理管道中的中間件元件,尤其是中間件在管道中早期運行。</span><span class="sxs-lookup"><span data-stu-id="6c397-191">Middleware components in the app's request processing pipeline, especially middleware run early in the pipeline.</span></span> <span data-ttu-id="6c397-192">這些元件對性能有很大影響。</span><span class="sxs-lookup"><span data-stu-id="6c397-192">These components have a large impact on performance.</span></span>
* <span data-ttu-id="6c397-193">為每個請求執行的代碼或每個請求多次執行的代碼。</span><span class="sxs-lookup"><span data-stu-id="6c397-193">Code that's executed for every request or multiple times per request.</span></span> <span data-ttu-id="6c397-194">例如,自定義日誌記錄、授權處理程式或暫態服務的初始化。</span><span class="sxs-lookup"><span data-stu-id="6c397-194">For example, custom logging, authorization handlers, or initialization of transient services.</span></span>

<span data-ttu-id="6c397-195">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-195">Recommendations:</span></span>

* <span data-ttu-id="6c397-196">**請勿**將自定義中間件元件用於長時間運行的任務。</span><span class="sxs-lookup"><span data-stu-id="6c397-196">**Do not** use custom middleware components with long-running tasks.</span></span>
* <span data-ttu-id="6c397-197">**請使用**效能分析工具(如[視覺化工作室診斷工具](/visualstudio/profiling/profiling-feature-tour)或[PerfView)](https://github.com/Microsoft/perfview)來識別[熱程式路徑](#understand-hot-code-paths)。</span><span class="sxs-lookup"><span data-stu-id="6c397-197">**Do** use performance profiling tools, such as [Visual Studio Diagnostic Tools](/visualstudio/profiling/profiling-feature-tour) or [PerfView](https://github.com/Microsoft/perfview)), to identify [hot code paths](#understand-hot-code-paths).</span></span>

## <a name="complete-long-running-tasks-outside-of-http-requests"></a><span data-ttu-id="6c397-198">在 HTTP 要求之外完成長時間執行的工作</span><span class="sxs-lookup"><span data-stu-id="6c397-198">Complete long-running Tasks outside of HTTP requests</span></span>

<span data-ttu-id="6c397-199">對ASP.NET Core 應用的大多數請求都可以由調用必要服務的控制器或頁面模型處理並返回 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="6c397-199">Most requests to an ASP.NET Core app can be handled by a controller or page model calling necessary services and returning an HTTP response.</span></span> <span data-ttu-id="6c397-200">對於某些涉及長時間運行的任務的請求,最好使整個請求-回應過程異步。</span><span class="sxs-lookup"><span data-stu-id="6c397-200">For some requests that involve long-running tasks, it's better to make the entire request-response process asynchronous.</span></span>

<span data-ttu-id="6c397-201">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-201">Recommendations:</span></span>

* <span data-ttu-id="6c397-202">**不要**等待長時間運行的任務作為普通 HTTP 請求處理的一部分完成。</span><span class="sxs-lookup"><span data-stu-id="6c397-202">**Do not** wait for long-running tasks to complete as part of ordinary HTTP request processing.</span></span>
* <span data-ttu-id="6c397-203">**請考慮**使用[後台服務](xref:fundamentals/host/hosted-services)處理長時間運行的請求,或者使用[Azure 函數](/azure/azure-functions/)處理進程外的請求。</span><span class="sxs-lookup"><span data-stu-id="6c397-203">**Do** consider handling long-running requests with [background services](xref:fundamentals/host/hosted-services) or out of process with an [Azure Function](/azure/azure-functions/).</span></span> <span data-ttu-id="6c397-204">在進程外完成工作對 CPU 密集型任務特別有利。</span><span class="sxs-lookup"><span data-stu-id="6c397-204">Completing work out-of-process is especially beneficial for CPU-intensive tasks.</span></span>
* <span data-ttu-id="6c397-205">**請使用**即時通訊選項([SignalR](xref:signalr/introduction)如 )以非同步方式與用戶端通訊。</span><span class="sxs-lookup"><span data-stu-id="6c397-205">**Do** use real-time communication options, such as [SignalR](xref:signalr/introduction), to communicate with clients asynchronously.</span></span>

## <a name="minify-client-assets"></a><span data-ttu-id="6c397-206">將客戶資產進行最小化</span><span class="sxs-lookup"><span data-stu-id="6c397-206">Minify client assets</span></span>

<span data-ttu-id="6c397-207">ASP.NET具有複雜前端的核心應用經常提供許多 JAVAScript、CSS 或圖像檔。</span><span class="sxs-lookup"><span data-stu-id="6c397-207">ASP.NET Core apps with complex front-ends frequently serve many JavaScript, CSS, or image files.</span></span> <span data-ttu-id="6c397-208">初始載入要求的效能可以通過:</span><span class="sxs-lookup"><span data-stu-id="6c397-208">Performance of initial load requests can be improved by:</span></span>

* <span data-ttu-id="6c397-209">捆綁,它將多個文件合併為一個檔。</span><span class="sxs-lookup"><span data-stu-id="6c397-209">Bundling, which combines multiple files into one.</span></span>
* <span data-ttu-id="6c397-210">明量化,通過刪除空格和註釋來減小檔的大小。</span><span class="sxs-lookup"><span data-stu-id="6c397-210">Minifying, which reduces the size of files by removing whitespace and comments.</span></span>

<span data-ttu-id="6c397-211">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-211">Recommendations:</span></span>

* <span data-ttu-id="6c397-212">**使用**ASP.NET Core 的[內置支援](xref:client-side/bundling-and-minification)來捆綁和美化客戶資產。</span><span class="sxs-lookup"><span data-stu-id="6c397-212">**Do** use ASP.NET Core's [built-in support](xref:client-side/bundling-and-minification) for bundling and minifying client assets.</span></span>
* <span data-ttu-id="6c397-213">**請考慮**其他第三方工具,如[Webpack,](https://webpack.js.org/)用於複雜的用戶端資產管理。</span><span class="sxs-lookup"><span data-stu-id="6c397-213">**Do** consider other third-party tools, such as [Webpack](https://webpack.js.org/), for complex client asset management.</span></span>

## <a name="compress-responses"></a><span data-ttu-id="6c397-214">壓縮回應</span><span class="sxs-lookup"><span data-stu-id="6c397-214">Compress responses</span></span>

 <span data-ttu-id="6c397-215">減小回應大小通常會增加應用程式的回應能力,通常顯著。</span><span class="sxs-lookup"><span data-stu-id="6c397-215">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="6c397-216">減少有效負載大小的一種方法是壓縮應用的回應。</span><span class="sxs-lookup"><span data-stu-id="6c397-216">One way to reduce payload sizes is to compress an app's responses.</span></span> <span data-ttu-id="6c397-217">有關詳細資訊,請參閱[回應壓縮](xref:performance/response-compression)。</span><span class="sxs-lookup"><span data-stu-id="6c397-217">For more information, see [Response compression](xref:performance/response-compression).</span></span>

## <a name="use-the-latest-aspnet-core-release"></a><span data-ttu-id="6c397-218">使用最新的ASP.NET核心版本</span><span class="sxs-lookup"><span data-stu-id="6c397-218">Use the latest ASP.NET Core release</span></span>

<span data-ttu-id="6c397-219">每個新版本ASP.NET核心包括性能改進。</span><span class="sxs-lookup"><span data-stu-id="6c397-219">Each new release of ASP.NET Core includes performance improvements.</span></span> <span data-ttu-id="6c397-220">.NET Core 和 ASP.NET 核心中的優化意味著較新版本的性能通常優於舊版本。</span><span class="sxs-lookup"><span data-stu-id="6c397-220">Optimizations in .NET Core and ASP.NET Core mean that newer versions generally outperform older versions.</span></span> <span data-ttu-id="6c397-221">例如,.NET Core 2.1 添加了對編譯正則表達式的支援,並從[span\<T>](https://msdn.microsoft.com/magazine/mt814808.aspx)中獲益。</span><span class="sxs-lookup"><span data-stu-id="6c397-221">For example, .NET Core 2.1 added support for compiled regular expressions and benefitted from [Span\<T>](https://msdn.microsoft.com/magazine/mt814808.aspx).</span></span> <span data-ttu-id="6c397-222">ASP.NET核心 2.2 添加了對 HTTP/2 的支援。</span><span class="sxs-lookup"><span data-stu-id="6c397-222">ASP.NET Core 2.2 added support for HTTP/2.</span></span> <span data-ttu-id="6c397-223">[ASP.NET Core 3.0 添加了許多改進](xref:aspnetcore-3.0),以減少記憶體使用並提高輸送量。</span><span class="sxs-lookup"><span data-stu-id="6c397-223">[ASP.NET Core 3.0 adds many improvements](xref:aspnetcore-3.0) that reduce memory usage and improve throughput.</span></span> <span data-ttu-id="6c397-224">如果性能是重中之重,請考慮升級到當前版本的 ASP.NET 酷睿。</span><span class="sxs-lookup"><span data-stu-id="6c397-224">If performance is a priority, consider upgrading to the current version of ASP.NET Core.</span></span>

## <a name="minimize-exceptions"></a><span data-ttu-id="6c397-225">最小化異常</span><span class="sxs-lookup"><span data-stu-id="6c397-225">Minimize exceptions</span></span>

<span data-ttu-id="6c397-226">異常應該很少見。</span><span class="sxs-lookup"><span data-stu-id="6c397-226">Exceptions should be rare.</span></span> <span data-ttu-id="6c397-227">與其他代碼流模式相比,引發和捕獲異常的速度很慢。</span><span class="sxs-lookup"><span data-stu-id="6c397-227">Throwing and catching exceptions is slow relative to other code flow patterns.</span></span> <span data-ttu-id="6c397-228">因此,不應使用異常來控制正常程式流。</span><span class="sxs-lookup"><span data-stu-id="6c397-228">Because of this, exceptions shouldn't be used to control normal program flow.</span></span>

<span data-ttu-id="6c397-229">建議：</span><span class="sxs-lookup"><span data-stu-id="6c397-229">Recommendations:</span></span>

* <span data-ttu-id="6c397-230">**請勿**使用引發或捕獲異常作為正常程式流的手段,尤其是在[熱代碼路徑](#understand-hot-code-paths)中。</span><span class="sxs-lookup"><span data-stu-id="6c397-230">**Do not** use throwing or catching exceptions as a means of normal program flow, especially in [hot code paths](#understand-hot-code-paths).</span></span>
* <span data-ttu-id="6c397-231">**在**應用中是否包括邏輯,以檢測和處理會導致異常的條件。</span><span class="sxs-lookup"><span data-stu-id="6c397-231">**Do** include logic in the app to detect and handle conditions that would cause an exception.</span></span>
* <span data-ttu-id="6c397-232">對於異常或意外的情況,**一應**引發或捕獲異常。</span><span class="sxs-lookup"><span data-stu-id="6c397-232">**Do** throw or catch exceptions for unusual or unexpected conditions.</span></span>

<span data-ttu-id="6c397-233">應用診斷工具(如應用程式見解)可幫助識別應用中可能影響性能的常見異常。</span><span class="sxs-lookup"><span data-stu-id="6c397-233">App diagnostic tools, such as Application Insights, can help to identify common exceptions in an app that may affect performance.</span></span>

## <a name="performance-and-reliability"></a><span data-ttu-id="6c397-234">效能和可靠性</span><span class="sxs-lookup"><span data-stu-id="6c397-234">Performance and reliability</span></span>

<span data-ttu-id="6c397-235">以下各節提供性能提示和已知可靠性問題和解決方案。</span><span class="sxs-lookup"><span data-stu-id="6c397-235">The following sections provide performance tips and known reliability problems and solutions.</span></span>

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a><span data-ttu-id="6c397-236">避免在 HTTPRequest/ HttpResponse 正文上同步讀取或寫入</span><span class="sxs-lookup"><span data-stu-id="6c397-236">Avoid synchronous read or write on HttpRequest/HttpResponse body</span></span>

<span data-ttu-id="6c397-237">ASP.NET核心中的所有I/O都是非同步的。</span><span class="sxs-lookup"><span data-stu-id="6c397-237">All I/O in ASP.NET Core is asynchronous.</span></span> <span data-ttu-id="6c397-238">伺服器實現`Stream`介面,該介面具有同步和非同步重載。</span><span class="sxs-lookup"><span data-stu-id="6c397-238">Servers implement the `Stream` interface, which has both synchronous and asynchronous overloads.</span></span> <span data-ttu-id="6c397-239">應首選異步線程以避免阻塞線程池線程。</span><span class="sxs-lookup"><span data-stu-id="6c397-239">The asynchronous ones should be preferred to avoid blocking thread pool threads.</span></span> <span data-ttu-id="6c397-240">阻塞線程可能導致線程池不足。</span><span class="sxs-lookup"><span data-stu-id="6c397-240">Blocking threads can lead to thread pool starvation.</span></span>

<span data-ttu-id="6c397-241">**不要這樣做:** 下面的範例使用<xref:System.IO.StreamReader.ReadToEnd*>。</span><span class="sxs-lookup"><span data-stu-id="6c397-241">**Do not do this:** The following example uses the <xref:System.IO.StreamReader.ReadToEnd*>.</span></span> <span data-ttu-id="6c397-242">它阻止當前線程等待結果。</span><span class="sxs-lookup"><span data-stu-id="6c397-242">It blocks the current thread to wait for the result.</span></span> <span data-ttu-id="6c397-243">這是[一個通過異步同步](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
)的範例。</span><span class="sxs-lookup"><span data-stu-id="6c397-243">This is an example of [sync over async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
).</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

<span data-ttu-id="6c397-244">在前面的代碼中,`Get`同步將整個 HTTP 請求正文讀取到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="6c397-244">In the preceding code, `Get` synchronously reads the entire HTTP request body into memory.</span></span> <span data-ttu-id="6c397-245">如果用戶端正在緩慢上載,則應用正在通過非同步進行同步。</span><span class="sxs-lookup"><span data-stu-id="6c397-245">If the client is slowly uploading, the app is doing sync over async.</span></span> <span data-ttu-id="6c397-246">應用程式會同步,因為 Kestrel**不支援**同步讀取。</span><span class="sxs-lookup"><span data-stu-id="6c397-246">The app does sync over async because Kestrel does **NOT** support synchronous reads.</span></span>

<span data-ttu-id="6c397-247">**執行此操作:** 下面的示例使用<xref:System.IO.StreamReader.ReadToEndAsync*>並且不會在讀取時阻止線程。</span><span class="sxs-lookup"><span data-stu-id="6c397-247">**Do this:** The following example uses <xref:System.IO.StreamReader.ReadToEndAsync*> and does not block the thread while reading.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

<span data-ttu-id="6c397-248">前面的代碼非同步地將整個 HTTP 請求正文讀取到記憶體中。</span><span class="sxs-lookup"><span data-stu-id="6c397-248">The preceding code asynchronously reads the entire HTTP request body into memory.</span></span>

> [!WARNING]
> <span data-ttu-id="6c397-249">如果請求很大,將整個 HTTP 請求正文讀取到記憶體中可能會導致記憶體不足 (OOM) 條件。</span><span class="sxs-lookup"><span data-stu-id="6c397-249">If the request is large, reading the entire HTTP request body into memory could lead to an out of memory (OOM) condition.</span></span> <span data-ttu-id="6c397-250">OOM可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="6c397-250">OOM can result in a Denial Of Service.</span></span>  <span data-ttu-id="6c397-251">有關詳細資訊,請參閱本文檔中[避免將大型請求正文或回應正文讀取到記憶體](#arlb)中。</span><span class="sxs-lookup"><span data-stu-id="6c397-251">For more information, see [Avoid reading large request bodies or response bodies into memory](#arlb) in this document.</span></span>

<span data-ttu-id="6c397-252">**執行此操作:** 以下範例使用非緩衝請求正文完全非同步:</span><span class="sxs-lookup"><span data-stu-id="6c397-252">**Do this:** The following example is fully asynchronous using a non buffered request body:</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

<span data-ttu-id="6c397-253">前面的代碼非同步將請求正文序列化為 C# 物件。</span><span class="sxs-lookup"><span data-stu-id="6c397-253">The preceding code asynchronously de-serializes the request body into a C# object.</span></span>

## <a name="prefer-readformasync-over-requestform"></a><span data-ttu-id="6c397-254">預設閱讀形式同步而不是要求.表單</span><span class="sxs-lookup"><span data-stu-id="6c397-254">Prefer ReadFormAsync over Request.Form</span></span>

<span data-ttu-id="6c397-255">使用 `HttpContext.Request.ReadFormAsync` 取代 `HttpContext.Request.Form`。</span><span class="sxs-lookup"><span data-stu-id="6c397-255">Use `HttpContext.Request.ReadFormAsync` instead of `HttpContext.Request.Form`.</span></span>
<span data-ttu-id="6c397-256">`HttpContext.Request.Form`只能在以下條件下安全讀取:</span><span class="sxs-lookup"><span data-stu-id="6c397-256">`HttpContext.Request.Form` can be safely read only with the following conditions:</span></span>

* <span data-ttu-id="6c397-257">表單已透過呼叫與`ReadFormAsync`</span><span class="sxs-lookup"><span data-stu-id="6c397-257">The form has been read by a call to `ReadFormAsync`, and</span></span>
* <span data-ttu-id="6c397-258">使用`HttpContext.Request.Form`</span><span class="sxs-lookup"><span data-stu-id="6c397-258">The cached form value is being read using `HttpContext.Request.Form`</span></span>

<span data-ttu-id="6c397-259">**不要這樣做:** 下面的範例使用`HttpContext.Request.Form`。</span><span class="sxs-lookup"><span data-stu-id="6c397-259">**Do not do this:** The following example uses `HttpContext.Request.Form`.</span></span>  <span data-ttu-id="6c397-260">`HttpContext.Request.Form`[使用非同步](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
)同步,並可能導致線程池不足。</span><span class="sxs-lookup"><span data-stu-id="6c397-260">`HttpContext.Request.Form` uses [sync over async](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
) and can lead to thread pool starvation.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

<span data-ttu-id="6c397-261">**執行此操作:** 下面的範例用於`HttpContext.Request.ReadFormAsync`非同步讀取窗體正文。</span><span class="sxs-lookup"><span data-stu-id="6c397-261">**Do this:** The following example uses `HttpContext.Request.ReadFormAsync` to read the form body asynchronously.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a><span data-ttu-id="6c397-262">避免將大型要求正文或回應正文讀取到記憶體中</span><span class="sxs-lookup"><span data-stu-id="6c397-262">Avoid reading large request bodies or response bodies into memory</span></span>

<span data-ttu-id="6c397-263">在 .NET 中,每個大於 85 KB 的物件分配最終都位於大型物件堆[(LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)) 中。</span><span class="sxs-lookup"><span data-stu-id="6c397-263">In .NET, every object allocation greater than 85 KB ends up in the large object heap ([LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)).</span></span> <span data-ttu-id="6c397-264">大型物件在兩個方面非常昂貴:</span><span class="sxs-lookup"><span data-stu-id="6c397-264">Large objects are expensive in two ways:</span></span>

* <span data-ttu-id="6c397-265">分配成本很高,因為必須清除新分配的大型物件的記憶體。</span><span class="sxs-lookup"><span data-stu-id="6c397-265">The allocation cost is high because the memory for a newly allocated large object has to be cleared.</span></span> <span data-ttu-id="6c397-266">CLR 保證清除所有新分配物件的記憶體。</span><span class="sxs-lookup"><span data-stu-id="6c397-266">The CLR guarantees that memory for all newly allocated objects is cleared.</span></span>
* <span data-ttu-id="6c397-267">LOH 與堆的其餘部分一起收集。</span><span class="sxs-lookup"><span data-stu-id="6c397-267">LOH is collected with the rest of the heap.</span></span> <span data-ttu-id="6c397-268">LOH 需要完整的[垃圾回收](/dotnet/standard/garbage-collection/fundamentals)或[第 2 代收集](/dotnet/standard/garbage-collection/fundamentals#generations)。</span><span class="sxs-lookup"><span data-stu-id="6c397-268">LOH requires a full [garbage collection](/dotnet/standard/garbage-collection/fundamentals) or [Gen2 collection](/dotnet/standard/garbage-collection/fundamentals#generations).</span></span>

<span data-ttu-id="6c397-269">這個[博客文章](https://adamsitnik.com/Array-Pool/#the-problem)簡明扼要地描述了這個問題:</span><span class="sxs-lookup"><span data-stu-id="6c397-269">This [blog post](https://adamsitnik.com/Array-Pool/#the-problem) describes the problem succinctly:</span></span>

> <span data-ttu-id="6c397-270">分配大型物件時,將將其標記為第 2 代物件。</span><span class="sxs-lookup"><span data-stu-id="6c397-270">When a large object is allocated, it's marked as Gen 2 object.</span></span> <span data-ttu-id="6c397-271">對於小物件,不是第 0 代。</span><span class="sxs-lookup"><span data-stu-id="6c397-271">Not Gen 0 as for small objects.</span></span> <span data-ttu-id="6c397-272">其後果是,如果內存在 LOH 中耗盡,GC 將清理整個託管堆,而不僅僅是 LOH。</span><span class="sxs-lookup"><span data-stu-id="6c397-272">The consequences are that if you run out of memory in LOH, GC cleans up the whole managed heap, not only LOH.</span></span> <span data-ttu-id="6c397-273">因此,它清理第 0 代、第 1 代和第 2 代(包括 LOH)。</span><span class="sxs-lookup"><span data-stu-id="6c397-273">So it cleans up Gen 0, Gen 1 and Gen 2 including LOH.</span></span> <span data-ttu-id="6c397-274">這稱為完全垃圾回收,是最耗時的垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="6c397-274">This is called full garbage collection and is the most time-consuming garbage collection.</span></span> <span data-ttu-id="6c397-275">對於許多應用程式,這是可以接受的。</span><span class="sxs-lookup"><span data-stu-id="6c397-275">For many applications, it can be acceptable.</span></span> <span data-ttu-id="6c397-276">但絕對不是高性能 Web 伺服器,因為處理普通 Web 請求只需要很少大記憶體緩衝區(從套接字讀取、解壓縮、解碼 JSON &更多)。</span><span class="sxs-lookup"><span data-stu-id="6c397-276">But definitely not for high-performance web servers, where few big memory buffers are needed to handle an average web request (read from a socket, decompress, decode JSON & more).</span></span>

<span data-ttu-id="6c397-277">天真地將大型要求或回應正文儲存到單個`byte[]``string`或 :</span><span class="sxs-lookup"><span data-stu-id="6c397-277">Naively storing a large request or response body into a single `byte[]` or `string`:</span></span>

* <span data-ttu-id="6c397-278">可能導致 LOH 中空間迅速耗盡。</span><span class="sxs-lookup"><span data-stu-id="6c397-278">May result in quickly running out of space in the LOH.</span></span>
* <span data-ttu-id="6c397-279">由於完全運行了 DC,可能會導致應用的性能問題。</span><span class="sxs-lookup"><span data-stu-id="6c397-279">May cause performance issues for the app because of full GCs running.</span></span>

## <a name="working-with-a-synchronous-data-processing-api"></a><span data-ttu-id="6c397-280">使用同步資料處理 API</span><span class="sxs-lookup"><span data-stu-id="6c397-280">Working with a synchronous data processing API</span></span>

<span data-ttu-id="6c397-281">使用僅支援同步讀取與寫入的序列化器/去序列化器時(例如[,JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)):</span><span class="sxs-lookup"><span data-stu-id="6c397-281">When using a serializer/de-serializer that only supports synchronous reads and writes (for example,  [JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)):</span></span>

* <span data-ttu-id="6c397-282">將數據異步緩衝到記憶體中,然後再將其傳遞到序列化器/去序列化器中。</span><span class="sxs-lookup"><span data-stu-id="6c397-282">Buffer the data into memory asynchronously before passing it into the serializer/de-serializer.</span></span>

> [!WARNING]
> <span data-ttu-id="6c397-283">如果請求很大,則可能導致記憶體不足 (OOM) 條件。</span><span class="sxs-lookup"><span data-stu-id="6c397-283">If the request is large, it could lead to an out of memory (OOM) condition.</span></span> <span data-ttu-id="6c397-284">OOM可能會導致拒絕服務。</span><span class="sxs-lookup"><span data-stu-id="6c397-284">OOM can result in a Denial Of Service.</span></span>  <span data-ttu-id="6c397-285">有關詳細資訊,請參閱本文檔中[避免將大型請求正文或回應正文讀取到記憶體](#arlb)中。</span><span class="sxs-lookup"><span data-stu-id="6c397-285">For more information, see [Avoid reading large request bodies or response bodies into memory](#arlb) in this document.</span></span>

<span data-ttu-id="6c397-286">默認情況下,ASP.NET核心 3.0<xref:System.Text.Json>用於 JSON 序列化。</span><span class="sxs-lookup"><span data-stu-id="6c397-286">ASP.NET Core 3.0 uses <xref:System.Text.Json> by default for JSON serialization.</span></span> <span data-ttu-id="6c397-287"><xref:System.Text.Json>:</span><span class="sxs-lookup"><span data-stu-id="6c397-287"><xref:System.Text.Json>:</span></span>

* <span data-ttu-id="6c397-288">非同步讀取和寫入 JSON。</span><span class="sxs-lookup"><span data-stu-id="6c397-288">Reads and writes JSON asynchronously.</span></span>
* <span data-ttu-id="6c397-289">針對 UTF-8 文本進行了優化。</span><span class="sxs-lookup"><span data-stu-id="6c397-289">Is optimized for UTF-8 text.</span></span>
* <span data-ttu-id="6c397-290">效能通常高於`Newtonsoft.Json`。</span><span class="sxs-lookup"><span data-stu-id="6c397-290">Typically higher performance than `Newtonsoft.Json`.</span></span>

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a><span data-ttu-id="6c397-291">不要將 IHTTPContextAccessor.HTTPContext 儲存在欄位中</span><span class="sxs-lookup"><span data-stu-id="6c397-291">Do not store IHttpContextAccessor.HttpContext in a field</span></span>

<span data-ttu-id="6c397-292">從請求線程存取時[,IHTTPContextAccessor.HTTPContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)`HttpContext`返回活動請求。</span><span class="sxs-lookup"><span data-stu-id="6c397-292">The [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext) returns the `HttpContext` of the active request when accessed from the request thread.</span></span> <span data-ttu-id="6c397-293">`IHttpContextAccessor.HttpContext`**不應**存儲在欄位或變數中。</span><span class="sxs-lookup"><span data-stu-id="6c397-293">The `IHttpContextAccessor.HttpContext` should **not** be stored in a field or variable.</span></span>

<span data-ttu-id="6c397-294">**不要這樣做:** 下面的示例將 存儲`HttpContext`在欄位中,然後嘗試稍後使用它。</span><span class="sxs-lookup"><span data-stu-id="6c397-294">**Do not do this:** The following example stores the `HttpContext` in a field and then attempts to use it later.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

<span data-ttu-id="6c397-295">前面的代碼經常在建構函數擷取空或不正確`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="6c397-295">The preceding code frequently captures a null or incorrect `HttpContext` in the constructor.</span></span>

<span data-ttu-id="6c397-296">**執行此操作:** 以下範例:</span><span class="sxs-lookup"><span data-stu-id="6c397-296">**Do this:** The following example:</span></span>

* <span data-ttu-id="6c397-297">將<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>存儲在欄位中。</span><span class="sxs-lookup"><span data-stu-id="6c397-297">Stores the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> in a field.</span></span>
* <span data-ttu-id="6c397-298">在正確的`HttpContext`時間使用該欄位並檢查`null`。</span><span class="sxs-lookup"><span data-stu-id="6c397-298">Uses the `HttpContext` field at the correct time and checks for `null`.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a><span data-ttu-id="6c397-299">不要從多個執行緒來存取 HTTPContext</span><span class="sxs-lookup"><span data-stu-id="6c397-299">Do not access HttpContext from multiple threads</span></span>

<span data-ttu-id="6c397-300">`HttpContext`*不是*線程安全的。</span><span class="sxs-lookup"><span data-stu-id="6c397-300">`HttpContext` is *NOT* thread-safe.</span></span> <span data-ttu-id="6c397-301">並行`HttpContext`從多個線程訪問可能會導致未定義的行為,如掛起、崩潰和數據損壞。</span><span class="sxs-lookup"><span data-stu-id="6c397-301">Accessing `HttpContext` from multiple threads in parallel can result in undefined behavior such as hangs, crashes, and data corruption.</span></span>

<span data-ttu-id="6c397-302">**不要這樣做:** 下面的示例發出三個並行請求,並在傳出 HTTP 請求之前和之後記錄傳入的請求路徑。</span><span class="sxs-lookup"><span data-stu-id="6c397-302">**Do not do this:** The following example makes three parallel requests and logs the incoming request path before and after the outgoing HTTP request.</span></span> <span data-ttu-id="6c397-303">請求路徑從多個線程訪問,可能並行訪問。</span><span class="sxs-lookup"><span data-stu-id="6c397-303">The request path is accessed from multiple threads, potentially in parallel.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

<span data-ttu-id="6c397-304">**執行此操作:** 以下示例在發出三個並行請求之前複製來自傳入請求的所有數據。</span><span class="sxs-lookup"><span data-stu-id="6c397-304">**Do this:** The following example copies all data from the incoming request before making the three parallel requests.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a><span data-ttu-id="6c397-305">要求完成後不要使用 HTTPContext</span><span class="sxs-lookup"><span data-stu-id="6c397-305">Do not use the HttpContext after the request is complete</span></span>

<span data-ttu-id="6c397-306">`HttpContext`僅當ASP.NET核心管道中有活動 HTTP 請求時,才有效。</span><span class="sxs-lookup"><span data-stu-id="6c397-306">`HttpContext` is only valid as long as there is an active HTTP request in the ASP.NET Core pipeline.</span></span> <span data-ttu-id="6c397-307">整個ASP.NET核心管道是執行每個請求的非同步委託鏈。</span><span class="sxs-lookup"><span data-stu-id="6c397-307">The entire ASP.NET Core pipeline is an asynchronous chain of delegates that executes every request.</span></span> <span data-ttu-id="6c397-308">當`Task`從此連結完成時,將回收`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="6c397-308">When the `Task` returned from this chain completes, the `HttpContext` is recycled.</span></span>

<span data-ttu-id="6c397-309">**不要這樣做:** 以下範例使用`async void`讓 HTTP 要求在到達`await`第一個 要求時完成:</span><span class="sxs-lookup"><span data-stu-id="6c397-309">**Do not do this:** The following example uses `async void` which makes the HTTP request complete when the first `await` is reached:</span></span>

* <span data-ttu-id="6c397-310">這在ASP.NET核心應用程序中**總是**一個壞的做法。</span><span class="sxs-lookup"><span data-stu-id="6c397-310">Which is **ALWAYS** a bad practice in ASP.NET Core apps.</span></span>
* <span data-ttu-id="6c397-311">存`HttpResponse`取 HTTP 要求完成後的 。</span><span class="sxs-lookup"><span data-stu-id="6c397-311">Accesses the `HttpResponse` after the HTTP request is complete.</span></span>
* <span data-ttu-id="6c397-312">崩潰進程。</span><span class="sxs-lookup"><span data-stu-id="6c397-312">Crashes the process.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

<span data-ttu-id="6c397-313">**執行此操作:** 下面的示例返回`Task`到框架,因此 HTTP 請求在操作完成之前不會完成。</span><span class="sxs-lookup"><span data-stu-id="6c397-313">**Do this:** The following example returns a `Task` to the framework, so the HTTP request doesn't complete until the action completes.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a><span data-ttu-id="6c397-314">不要在背景緒中擷取 HTTPContext</span><span class="sxs-lookup"><span data-stu-id="6c397-314">Do not capture the HttpContext in background threads</span></span>

<span data-ttu-id="6c397-315">**不要這樣做:** 下面的範例顯示閉包正在從 屬性擷`HttpContext`取`Controller`。</span><span class="sxs-lookup"><span data-stu-id="6c397-315">**Do not do this:** The following example shows a closure is capturing the `HttpContext` from the `Controller` property.</span></span> <span data-ttu-id="6c397-316">這是一個糟糕的做法,因為工作項可以:</span><span class="sxs-lookup"><span data-stu-id="6c397-316">This is a bad practice because the work item could:</span></span>

* <span data-ttu-id="6c397-317">在請求範圍之外運行。</span><span class="sxs-lookup"><span data-stu-id="6c397-317">Run outside of the request scope.</span></span>
* <span data-ttu-id="6c397-318">試圖讀錯`HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="6c397-318">Attempt to read the wrong `HttpContext`.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

<span data-ttu-id="6c397-319">**執行此操作:** 以下範例:</span><span class="sxs-lookup"><span data-stu-id="6c397-319">**Do this:** The following example:</span></span>

* <span data-ttu-id="6c397-320">複製請求期間後台任務中所需的數據。</span><span class="sxs-lookup"><span data-stu-id="6c397-320">Copies the data required in the background task during the request.</span></span>
* <span data-ttu-id="6c397-321">不引用控制器的任何內容。</span><span class="sxs-lookup"><span data-stu-id="6c397-321">Doesn't reference anything from the controller.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

<span data-ttu-id="6c397-322">後台任務應作為託管服務實現。</span><span class="sxs-lookup"><span data-stu-id="6c397-322">Background tasks should be implemented as hosted services.</span></span> <span data-ttu-id="6c397-323">如需詳細資訊，請參閱[搭配託管服務的背景工作](xref:fundamentals/host/hosted-services)。</span><span class="sxs-lookup"><span data-stu-id="6c397-323">For more information, see [Background tasks with hosted services](xref:fundamentals/host/hosted-services).</span></span>

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a><span data-ttu-id="6c397-324">不要捕捉後台線程式控制器的服務</span><span class="sxs-lookup"><span data-stu-id="6c397-324">Do not capture services injected into the controllers on background threads</span></span>

<span data-ttu-id="6c397-325">**不要這樣做:** 下面的範例顯示閉包正在從`DbContext``Controller`操作參數擷取 。</span><span class="sxs-lookup"><span data-stu-id="6c397-325">**Do not do this:** The following example shows a closure is capturing the `DbContext` from the `Controller` action parameter.</span></span> <span data-ttu-id="6c397-326">這是一個壞的做法。</span><span class="sxs-lookup"><span data-stu-id="6c397-326">This is a bad practice.</span></span>  <span data-ttu-id="6c397-327">工作項可以運行在請求範圍之外。</span><span class="sxs-lookup"><span data-stu-id="6c397-327">The work item could run outside of the request scope.</span></span> <span data-ttu-id="6c397-328">`ContosoDbContext`的範圍限定到請求,從而導致`ObjectDisposedException`。</span><span class="sxs-lookup"><span data-stu-id="6c397-328">The `ContosoDbContext` is scoped to the request, resulting in an `ObjectDisposedException`.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

<span data-ttu-id="6c397-329">**執行此操作:** 以下範例:</span><span class="sxs-lookup"><span data-stu-id="6c397-329">**Do this:** The following example:</span></span>

* <span data-ttu-id="6c397-330">注入<xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory>,以便在後台工作項中創建作用域。</span><span class="sxs-lookup"><span data-stu-id="6c397-330">Injects an <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> in order to create a scope in the background work item.</span></span> <span data-ttu-id="6c397-331">`IServiceScopeFactory`是一個單一的。</span><span class="sxs-lookup"><span data-stu-id="6c397-331">`IServiceScopeFactory` is a singleton.</span></span>
* <span data-ttu-id="6c397-332">在後台線程中創建新的依賴項注入作用域。</span><span class="sxs-lookup"><span data-stu-id="6c397-332">Creates a new dependency injection scope in the background thread.</span></span>
* <span data-ttu-id="6c397-333">不引用控制器的任何內容。</span><span class="sxs-lookup"><span data-stu-id="6c397-333">Doesn't reference anything from the controller.</span></span>
* <span data-ttu-id="6c397-334">不要從傳入要求捕捉`ContosoDbContext`。</span><span class="sxs-lookup"><span data-stu-id="6c397-334">Doesn't capture the `ContosoDbContext` from the incoming request.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

<span data-ttu-id="6c397-335">以下突顯的代碼:</span><span class="sxs-lookup"><span data-stu-id="6c397-335">The following highlighted code:</span></span>

* <span data-ttu-id="6c397-336">為後台操作的存留期創建一個作用域,並從中解析服務。</span><span class="sxs-lookup"><span data-stu-id="6c397-336">Creates a scope for the lifetime of the background operation and resolves services from it.</span></span>
* <span data-ttu-id="6c397-337">從`ContosoDbContext`正確的作用域使用。</span><span class="sxs-lookup"><span data-stu-id="6c397-337">Uses `ContosoDbContext` from the correct scope.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a><span data-ttu-id="6c397-338">回應正文啟動後,不要修改狀態代碼或標頭</span><span class="sxs-lookup"><span data-stu-id="6c397-338">Do not modify the status code or headers after the response body has started</span></span>

<span data-ttu-id="6c397-339">ASP.NET核心不會緩衝 HTTP 回應正文。</span><span class="sxs-lookup"><span data-stu-id="6c397-339">ASP.NET Core does not buffer the HTTP response body.</span></span> <span data-ttu-id="6c397-340">初編寫回應:</span><span class="sxs-lookup"><span data-stu-id="6c397-340">The first time the response is written:</span></span>

* <span data-ttu-id="6c397-341">標頭與正文的塊一起發送到用戶端。</span><span class="sxs-lookup"><span data-stu-id="6c397-341">The headers are sent along with that chunk of the body to the client.</span></span>
* <span data-ttu-id="6c397-342">不再可以更改回應標頭。</span><span class="sxs-lookup"><span data-stu-id="6c397-342">It's no longer possible to change response headers.</span></span>

<span data-ttu-id="6c397-343">**不要這樣做:** 以下代碼嘗試在回應啟動後新增回應標頭:</span><span class="sxs-lookup"><span data-stu-id="6c397-343">**Do not do this:** The following code tries to add response headers after the response has already started:</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

<span data-ttu-id="6c397-344">在前面的代碼中,`context.Response.Headers["test"] = "test value";``next()`如果已寫入回應,將引發異常。</span><span class="sxs-lookup"><span data-stu-id="6c397-344">In the preceding code, `context.Response.Headers["test"] = "test value";` will throw an exception if `next()` has written to the response.</span></span>

<span data-ttu-id="6c397-345">**執行此操作:** 以下範例檢查 HTTP 回應在修改標頭之前是否已啟動。</span><span class="sxs-lookup"><span data-stu-id="6c397-345">**Do this:** The following example checks if the HTTP response has started before modifying the headers.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

<span data-ttu-id="6c397-346">**執行此操作:** 以下範例用於`HttpResponse.OnStarting`在回應標頭刷新到用戶端之前設置標頭。</span><span class="sxs-lookup"><span data-stu-id="6c397-346">**Do this:** The following example uses `HttpResponse.OnStarting` to set the headers before the response headers are flushed to the client.</span></span>

<span data-ttu-id="6c397-347">檢查回應是否尚未啟動,可以註冊將在編寫回應標頭之前調用的回調。</span><span class="sxs-lookup"><span data-stu-id="6c397-347">Checking if the response has not started allows registering a callback that will be invoked just before response headers are written.</span></span> <span data-ttu-id="6c397-348">檢查回應是否尚未啟動:</span><span class="sxs-lookup"><span data-stu-id="6c397-348">Checking if the response has not started:</span></span>

* <span data-ttu-id="6c397-349">提供及時追加或覆蓋標頭的能力。</span><span class="sxs-lookup"><span data-stu-id="6c397-349">Provides the ability to append or override headers just in time.</span></span>
* <span data-ttu-id="6c397-350">不需要瞭解管道中的下一個中間件。</span><span class="sxs-lookup"><span data-stu-id="6c397-350">Doesn't require knowledge of the next middleware in the pipeline.</span></span>

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a><span data-ttu-id="6c397-351">如果您已經開始寫入回應正文,請不要呼叫下一個()</span><span class="sxs-lookup"><span data-stu-id="6c397-351">Do not call next() if you have already started writing to the response body</span></span>

<span data-ttu-id="6c397-352">僅當元件可以處理和操作回應時,才期望調用元件。</span><span class="sxs-lookup"><span data-stu-id="6c397-352">Components only expect to be called if it's possible for them to handle and manipulate the response.</span></span>
