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
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET核心性能最佳實務

由[邁克·盧梭斯](https://github.com/mjrousos)

本文提供了ASP.NET核心的性能最佳實務指南。

## <a name="cache-aggressively"></a>主動快取

本文檔的幾個部分討論了緩存。 如需詳細資訊，請參閱 <xref:performance/caching/response>。

## <a name="understand-hot-code-paths"></a>瞭解熱程式路徑

在本文中,*熱代碼路徑*定義為經常呼叫的代碼路徑以及大部分執行時間發生的位置。 熱代碼路徑通常限制應用橫向擴展和性能,本文檔的幾個部分對此進行討論。

## <a name="avoid-blocking-calls"></a>避免阻塞呼叫

ASP.NET核心應用應設計為同時處理許多請求。 非同步 API 允許小線程池通過不等待阻塞調用來處理數千個併發請求。 線程可以處理另一個請求,而不是等待長時間運行的同步任務來完成。

ASP.NET Core 應用中常見的性能問題是阻止可能是非同步的調用。 許多同步阻塞調用會導致[線程池不足](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)和回應時間降低。

**不要**:

* 通過調用[Task.Wait](/dotnet/api/system.threading.tasks.task.wait)或[Task.結果](/dotnet/api/system.threading.tasks.task-1.result)來阻止非同步執行。
* 獲取公共代碼路徑中的鎖。 ASP.NET核心應用在建構為並行運行代碼時性能最出色。
* 調用[任務.運行](/dotnet/api/system.threading.tasks.task.run)並立即等待它。 ASP.NET Core 已經在正常的線程池線程上運行應用代碼,因此調用 Task.Run 只會導致不必要的線程池計劃。 即使計劃的代碼會阻止線程,Task.Run 也不會阻止這一點。

**做**:

* 使[熱代碼路徑](#understand-hot-code-paths)非同步。
* 如果非同步 API 可用,則非同步 API 會非同步調用資料存取、I/O 和長期運行的操作 API。 **不要**使用[Task.Run](/dotnet/api/system.threading.tasks.task.run)使同步 API 同步。
* 使控制器/Razor 頁面操作非同步。 整個調用堆疊是非同步的,以便從[非同步/等待](/dotnet/csharp/programming-guide/concepts/async/)模式中獲益。

探查器(如[PerfView)](https://github.com/Microsoft/perfview)可用於尋找經常加入[執行到 執行緒池的線程](/windows/desktop/procthread/thread-pools)。 該`Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start`事件指示添加到線程池的線程。 <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a>最小化大型物件配置

[.NET 核心垃圾回收器](/dotnet/standard/garbage-collection/)在 ASP.NET 核心應用中自動管理記憶體的分配和釋放。 自動垃圾回收通常意味著開發人員無需擔心如何或何時釋放記憶體。 但是,清理未引用的物件需要 CPU 時間,因此開發人員應盡量減少[在熱代碼路徑](#understand-hot-code-paths)中分配物件。 垃圾回收對於大型物件(> 85 K 位元組)特別昂貴。 大型物件存儲在[大型物件堆](/dotnet/standard/garbage-collection/large-object-heap)上,需要完整(第2代)垃圾回收來清理。 與第 0 代和第 1 代集合不同,第 2 代集合需要暫時暫停應用執行。 頻繁分配和取消分配大型物件可能會導致性能不一致。

建議：

* **請考慮**緩存經常使用的大型物件。 緩存大型物件可防止昂貴的分配。
* 使用[ArrayPool\<T>](/dotnet/api/system.buffers.arraypool-1)儲存大型陣列來**執行**池緩衝區。
* **不要**在[熱代碼路徑](#understand-hot-code-paths)上分配許多短壽命的大物件。

可以通過查看[PerfView](https://github.com/Microsoft/perfview)中的垃圾回收 (GC) 統計資訊並檢查記憶體問題(如上述問題)來診斷:

* 垃圾回收暫停時間。
* 在垃圾回收中花費的處理器時間的百分比。
* 有多少垃圾回收是生成 0、1 和 2。

有關詳細資訊,請參閱[垃圾回收和性能](/dotnet/standard/garbage-collection/performance)。

## <a name="optimize-data-access-and-io"></a>優化資料存取和 I/O

與資料存儲和其他遠端服務的交互通常是ASP.NET核心應用中最慢的部分。 高效讀取和寫入數據對於良好的性能至關重要。

建議：

* **請**非所有資料存取 API。
* **不要**檢索超過所需數據的數據。 編寫查詢以僅返回當前 HTTP 請求所需的數據。
* **如果**可以接受稍微過時的數據,請考慮緩存從資料庫或遠端服務檢索的頻繁訪問的數據。 依專案,使用[記憶體快取](xref:performance/caching/memory)或[分散式快取](xref:performance/caching/distributed)。 如需詳細資訊，請參閱 <xref:performance/caching/response>。
* **盡量減少**網路往返。 目標是在單個調用中檢索所需的數據,而不是多個調用。
* 將唯讀取資料時,**請**使用實體框架核心中的[不追蹤查詢](/ef/core/querying/tracking#no-tracking-queries)。 EF Core 可以更有效地返回無跟蹤查詢的結果。
* **執行**篩選與聚合 LINQ`.Where``.Select`查詢(`.Sum`例如 ,使用 或語句),以便由資料庫執行篩選。
* **一應考慮**EF Core 解析用戶端上的一些查詢運算符,這可能導致查詢執行效率低下。 有關詳細資訊,請參閱[客戶端評估性能問題](/ef/core/querying/client-eval#client-evaluation-performance-issues)。
* **不要**對集合使用投影查詢,這可能導致執行"N + 1"SQL 查詢。 有關詳細資訊,請參閱[優化相關子查詢](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)。

有關可能提高大規模應用中性能的方法,請參閱[EF 高性能](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries):

* [DbContext 共用](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [明確地編譯查詢](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

我們建議在提交代碼庫之前測量上述高性能方法的影響。 編譯查詢的額外複雜性可能無法證明性能改進的合理性。

可以通過查看使用[應用程式見解](/azure/application-insights/app-insights-overview)或分析工具訪問數據所花費的時間來檢測查詢問題。 大多數資料庫還提供有關頻繁執行的查詢的統計資訊。

## <a name="pool-http-connections-with-httpclientfactory"></a>使用 HTTPClientFactory 池 HTTP 連線

儘管[HTTPClient](/dotnet/api/system.net.http.httpclient)`IDisposable`實現了該介面,但它旨在重用。 關閉`HttpClient`的實例使套接字處於`TIME_WAIT`狀態短時間打開。 如果經常使用創建和釋放`HttpClient`對象的代碼路徑,則應用可能會耗盡可用的套接字。 [HTTPClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)在 ASP.NET 核心 2.1 中引入,作為此問題的解決方案。 它處理池 HTTP 連接以優化性能和可靠性。

建議：

* **不要**直接創建和處置`HttpClient`實例。
* **請使用** [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)`HttpClient`檢索 實例。 有關詳細資訊,請參閱使用[HTTPClientFactory 實現彈性 HTTP 請求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)。

## <a name="keep-common-code-paths-fast"></a>保持常見程式碼路徑快速

您希望所有代碼都快速。 經常調用的代碼路徑是優化的最重要路徑。 其中包括：

* 應用請求處理管道中的中間件元件,尤其是中間件在管道中早期運行。 這些元件對性能有很大影響。
* 為每個請求執行的代碼或每個請求多次執行的代碼。 例如,自定義日誌記錄、授權處理程式或暫態服務的初始化。

建議：

* **請勿**將自定義中間件元件用於長時間運行的任務。
* **請使用**效能分析工具(如[視覺化工作室診斷工具](/visualstudio/profiling/profiling-feature-tour)或[PerfView)](https://github.com/Microsoft/perfview)來識別[熱程式路徑](#understand-hot-code-paths)。

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>在 HTTP 要求之外完成長時間執行的工作

對ASP.NET Core 應用的大多數請求都可以由調用必要服務的控制器或頁面模型處理並返回 HTTP 回應。 對於某些涉及長時間運行的任務的請求,最好使整個請求-回應過程異步。

建議：

* **不要**等待長時間運行的任務作為普通 HTTP 請求處理的一部分完成。
* **請考慮**使用[後台服務](xref:fundamentals/host/hosted-services)處理長時間運行的請求,或者使用[Azure 函數](/azure/azure-functions/)處理進程外的請求。 在進程外完成工作對 CPU 密集型任務特別有利。
* **請使用**即時通訊選項([SignalR](xref:signalr/introduction)如 )以非同步方式與用戶端通訊。

## <a name="minify-client-assets"></a>將客戶資產進行最小化

ASP.NET具有複雜前端的核心應用經常提供許多 JAVAScript、CSS 或圖像檔。 初始載入要求的效能可以通過:

* 捆綁,它將多個文件合併為一個檔。
* 明量化,通過刪除空格和註釋來減小檔的大小。

建議：

* **使用**ASP.NET Core 的[內置支援](xref:client-side/bundling-and-minification)來捆綁和美化客戶資產。
* **請考慮**其他第三方工具,如[Webpack,](https://webpack.js.org/)用於複雜的用戶端資產管理。

## <a name="compress-responses"></a>壓縮回應

 減小回應大小通常會增加應用程式的回應能力,通常顯著。 減少有效負載大小的一種方法是壓縮應用的回應。 有關詳細資訊,請參閱[回應壓縮](xref:performance/response-compression)。

## <a name="use-the-latest-aspnet-core-release"></a>使用最新的ASP.NET核心版本

每個新版本ASP.NET核心包括性能改進。 .NET Core 和 ASP.NET 核心中的優化意味著較新版本的性能通常優於舊版本。 例如,.NET Core 2.1 添加了對編譯正則表達式的支援,並從[span\<T>](https://msdn.microsoft.com/magazine/mt814808.aspx)中獲益。 ASP.NET核心 2.2 添加了對 HTTP/2 的支援。 [ASP.NET Core 3.0 添加了許多改進](xref:aspnetcore-3.0),以減少記憶體使用並提高輸送量。 如果性能是重中之重,請考慮升級到當前版本的 ASP.NET 酷睿。

## <a name="minimize-exceptions"></a>最小化異常

異常應該很少見。 與其他代碼流模式相比,引發和捕獲異常的速度很慢。 因此,不應使用異常來控制正常程式流。

建議：

* **請勿**使用引發或捕獲異常作為正常程式流的手段,尤其是在[熱代碼路徑](#understand-hot-code-paths)中。
* **在**應用中是否包括邏輯,以檢測和處理會導致異常的條件。
* 對於異常或意外的情況,**一應**引發或捕獲異常。

應用診斷工具(如應用程式見解)可幫助識別應用中可能影響性能的常見異常。

## <a name="performance-and-reliability"></a>效能和可靠性

以下各節提供性能提示和已知可靠性問題和解決方案。

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a>避免在 HTTPRequest/ HttpResponse 正文上同步讀取或寫入

ASP.NET核心中的所有I/O都是非同步的。 伺服器實現`Stream`介面,該介面具有同步和非同步重載。 應首選異步線程以避免阻塞線程池線程。 阻塞線程可能導致線程池不足。

**不要這樣做:** 下面的範例使用<xref:System.IO.StreamReader.ReadToEnd*>。 它阻止當前線程等待結果。 這是[一個通過異步同步](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
)的範例。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

在前面的代碼中,`Get`同步將整個 HTTP 請求正文讀取到記憶體中。 如果用戶端正在緩慢上載,則應用正在通過非同步進行同步。 應用程式會同步,因為 Kestrel**不支援**同步讀取。

**執行此操作:** 下面的示例使用<xref:System.IO.StreamReader.ReadToEndAsync*>並且不會在讀取時阻止線程。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

前面的代碼非同步地將整個 HTTP 請求正文讀取到記憶體中。

> [!WARNING]
> 如果請求很大,將整個 HTTP 請求正文讀取到記憶體中可能會導致記憶體不足 (OOM) 條件。 OOM可能會導致拒絕服務。  有關詳細資訊,請參閱本文檔中[避免將大型請求正文或回應正文讀取到記憶體](#arlb)中。

**執行此操作:** 以下範例使用非緩衝請求正文完全非同步:

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

前面的代碼非同步將請求正文序列化為 C# 物件。

## <a name="prefer-readformasync-over-requestform"></a>預設閱讀形式同步而不是要求.表單

使用 `HttpContext.Request.ReadFormAsync` 取代 `HttpContext.Request.Form`。
`HttpContext.Request.Form`只能在以下條件下安全讀取:

* 表單已透過呼叫與`ReadFormAsync`
* 使用`HttpContext.Request.Form`

**不要這樣做:** 下面的範例使用`HttpContext.Request.Form`。  `HttpContext.Request.Form`[使用非同步](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
)同步,並可能導致線程池不足。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

**執行此操作:** 下面的範例用於`HttpContext.Request.ReadFormAsync`非同步讀取窗體正文。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a>避免將大型要求正文或回應正文讀取到記憶體中

在 .NET 中,每個大於 85 KB 的物件分配最終都位於大型物件堆[(LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)) 中。 大型物件在兩個方面非常昂貴:

* 分配成本很高,因為必須清除新分配的大型物件的記憶體。 CLR 保證清除所有新分配物件的記憶體。
* LOH 與堆的其餘部分一起收集。 LOH 需要完整的[垃圾回收](/dotnet/standard/garbage-collection/fundamentals)或[第 2 代收集](/dotnet/standard/garbage-collection/fundamentals#generations)。

這個[博客文章](https://adamsitnik.com/Array-Pool/#the-problem)簡明扼要地描述了這個問題:

> 分配大型物件時,將將其標記為第 2 代物件。 對於小物件,不是第 0 代。 其後果是,如果內存在 LOH 中耗盡,GC 將清理整個託管堆,而不僅僅是 LOH。 因此,它清理第 0 代、第 1 代和第 2 代(包括 LOH)。 這稱為完全垃圾回收,是最耗時的垃圾回收。 對於許多應用程式,這是可以接受的。 但絕對不是高性能 Web 伺服器,因為處理普通 Web 請求只需要很少大記憶體緩衝區(從套接字讀取、解壓縮、解碼 JSON &更多)。

天真地將大型要求或回應正文儲存到單個`byte[]``string`或 :

* 可能導致 LOH 中空間迅速耗盡。
* 由於完全運行了 DC,可能會導致應用的性能問題。

## <a name="working-with-a-synchronous-data-processing-api"></a>使用同步資料處理 API

使用僅支援同步讀取與寫入的序列化器/去序列化器時(例如[,JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)):

* 將數據異步緩衝到記憶體中,然後再將其傳遞到序列化器/去序列化器中。

> [!WARNING]
> 如果請求很大,則可能導致記憶體不足 (OOM) 條件。 OOM可能會導致拒絕服務。  有關詳細資訊,請參閱本文檔中[避免將大型請求正文或回應正文讀取到記憶體](#arlb)中。

默認情況下,ASP.NET核心 3.0<xref:System.Text.Json>用於 JSON 序列化。 <xref:System.Text.Json>:

* 非同步讀取和寫入 JSON。
* 針對 UTF-8 文本進行了優化。
* 效能通常高於`Newtonsoft.Json`。

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a>不要將 IHTTPContextAccessor.HTTPContext 儲存在欄位中

從請求線程存取時[,IHTTPContextAccessor.HTTPContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)`HttpContext`返回活動請求。 `IHttpContextAccessor.HttpContext`**不應**存儲在欄位或變數中。

**不要這樣做:** 下面的示例將 存儲`HttpContext`在欄位中,然後嘗試稍後使用它。

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

前面的代碼經常在建構函數擷取空或不正確`HttpContext`。

**執行此操作:** 以下範例:

* 將<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>存儲在欄位中。
* 在正確的`HttpContext`時間使用該欄位並檢查`null`。

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a>不要從多個執行緒來存取 HTTPContext

`HttpContext`*不是*線程安全的。 並行`HttpContext`從多個線程訪問可能會導致未定義的行為,如掛起、崩潰和數據損壞。

**不要這樣做:** 下面的示例發出三個並行請求,並在傳出 HTTP 請求之前和之後記錄傳入的請求路徑。 請求路徑從多個線程訪問,可能並行訪問。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

**執行此操作:** 以下示例在發出三個並行請求之前複製來自傳入請求的所有數據。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a>要求完成後不要使用 HTTPContext

`HttpContext`僅當ASP.NET核心管道中有活動 HTTP 請求時,才有效。 整個ASP.NET核心管道是執行每個請求的非同步委託鏈。 當`Task`從此連結完成時,將回收`HttpContext`。

**不要這樣做:** 以下範例使用`async void`讓 HTTP 要求在到達`await`第一個 要求時完成:

* 這在ASP.NET核心應用程序中**總是**一個壞的做法。
* 存`HttpResponse`取 HTTP 要求完成後的 。
* 崩潰進程。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

**執行此操作:** 下面的示例返回`Task`到框架,因此 HTTP 請求在操作完成之前不會完成。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a>不要在背景緒中擷取 HTTPContext

**不要這樣做:** 下面的範例顯示閉包正在從 屬性擷`HttpContext`取`Controller`。 這是一個糟糕的做法,因為工作項可以:

* 在請求範圍之外運行。
* 試圖讀錯`HttpContext`。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

**執行此操作:** 以下範例:

* 複製請求期間後台任務中所需的數據。
* 不引用控制器的任何內容。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

後台任務應作為託管服務實現。 如需詳細資訊，請參閱[搭配託管服務的背景工作](xref:fundamentals/host/hosted-services)。

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a>不要捕捉後台線程式控制器的服務

**不要這樣做:** 下面的範例顯示閉包正在從`DbContext``Controller`操作參數擷取 。 這是一個壞的做法。  工作項可以運行在請求範圍之外。 `ContosoDbContext`的範圍限定到請求,從而導致`ObjectDisposedException`。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

**執行此操作:** 以下範例:

* 注入<xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory>,以便在後台工作項中創建作用域。 `IServiceScopeFactory`是一個單一的。
* 在後台線程中創建新的依賴項注入作用域。
* 不引用控制器的任何內容。
* 不要從傳入要求捕捉`ContosoDbContext`。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

以下突顯的代碼:

* 為後台操作的存留期創建一個作用域,並從中解析服務。
* 從`ContosoDbContext`正確的作用域使用。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a>回應正文啟動後,不要修改狀態代碼或標頭

ASP.NET核心不會緩衝 HTTP 回應正文。 初編寫回應:

* 標頭與正文的塊一起發送到用戶端。
* 不再可以更改回應標頭。

**不要這樣做:** 以下代碼嘗試在回應啟動後新增回應標頭:

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

在前面的代碼中,`context.Response.Headers["test"] = "test value";``next()`如果已寫入回應,將引發異常。

**執行此操作:** 以下範例檢查 HTTP 回應在修改標頭之前是否已啟動。

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

**執行此操作:** 以下範例用於`HttpResponse.OnStarting`在回應標頭刷新到用戶端之前設置標頭。

檢查回應是否尚未啟動,可以註冊將在編寫回應標頭之前調用的回調。 檢查回應是否尚未啟動:

* 提供及時追加或覆蓋標頭的能力。
* 不需要瞭解管道中的下一個中間件。

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a>如果您已經開始寫入回應正文,請不要呼叫下一個()

僅當元件可以處理和操作回應時,才期望調用元件。
