---
title: ASP.NET Core 效能最佳做法
author: mjrousos
description: 提升 ASP.NET Core 應用程式效能及避免常見效能問題的秘訣。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/06/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/performance-best-practices
ms.openlocfilehash: e83019a0f905fa9cd0f0c39960b787bc5b13b64f
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775384"
---
# <a name="aspnet-core-performance-best-practices"></a>ASP.NET Core 效能最佳做法

由[Mike Rousos](https://github.com/mjrousos)

本文提供 ASP.NET Core 的效能最佳作法指導方針。

## <a name="cache-aggressively"></a>主動快取

本檔的數個部分會討論快取。 如需詳細資訊，請參閱<xref:performance/caching/response>。

## <a name="understand-hot-code-paths"></a>瞭解熱程式碼路徑

在本檔中，*熱程式碼路徑*會定義為經常呼叫的程式碼路徑，而且會發生大部分的執行時間。 熱程式碼路徑通常會限制應用程式相應放大和效能，並在本檔的數個部分中討論。

## <a name="avoid-blocking-calls"></a>避免封鎖呼叫

ASP.NET Core 應用程式應設計為同時處理許多要求。 非同步 Api 允許小型的執行緒集區，藉由不等待封鎖呼叫來處理數以千計的並行要求。 執行緒可以處理另一個要求，而不是等待長時間執行的同步工作完成。

ASP.NET Core 應用程式中常見的效能問題是封鎖可能是非同步呼叫。 許多同步封鎖呼叫會導致[執行緒集](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/)區耗盡並降低回應時間。

**不要**：

* 藉由呼叫工作來封鎖非同步執行。 [Wait](/dotnet/api/system.threading.tasks.task.wait)或[task。 Result](/dotnet/api/system.threading.tasks.task-1.result)。
* 取得通用程式碼路徑中的鎖定。 在架構上平行執行程式碼時，ASP.NET Core apps 最具效能。
* 呼叫[Task。執行](/dotnet/api/system.threading.tasks.task.run)並立即等待它。 ASP.NET Core 已經在一般執行緒集區執行緒上執行應用程式程式碼，因此呼叫工作。只會產生額外不必要的執行緒集區排程。 即使已排程的程式碼會封鎖執行緒，工作也不會阻止這種情況。

**Do**：

* 將[熱程式碼路徑](#understand-hot-code-paths)設為非同步。
* 如果有非同步 API 可供使用，請以非同步方式呼叫資料存取、i/o 和長時間執行的作業 Api。 請勿使用工作[。](/dotnet/api/system.threading.tasks.task.run) **請執行，** 讓 synchronus API 成為非同步。
* 將控制器/Razor頁面動作設為非同步。 整個呼叫堆疊都是非同步，以便受益于[非同步/](/dotnet/csharp/programming-guide/concepts/async/)等候模式。

分析工具（例如[PerfView](https://github.com/Microsoft/perfview)）可以用來尋找經常加入[執行緒集](/windows/desktop/procthread/thread-pools)區的執行緒。 `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start`事件表示已加入執行緒集區的執行緒。 <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc)  -->

## <a name="minimize-large-object-allocations"></a>最小化大型物件配置

[.Net Core 垃圾收集](/dotnet/standard/garbage-collection/)行程會自動管理 ASP.NET Core 應用程式中的記憶體配置和釋放。 自動垃圾收集通常表示開發人員不需要擔心釋放記憶體的方式或時機。 不過，清除未參考的物件會耗用 CPU 時間，因此開發人員應該盡可能減少在[熱程式碼路徑](#understand-hot-code-paths)中設定物件的情況。 垃圾收集特別耗費大量物件（> 85 K 位元組）。 大型物件會儲存在[大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)上，並要求完整（層代2）垃圾收集來進行清除。 不同于層代0和第1代回收，第2代回收需要暫時暫停應用程式執行。 大型物件的頻繁配置和取消配置可能會造成不一致的效能。

建議：

* **請考慮快**取經常使用的大型物件。 快取大型物件可避免耗用昂貴的配置。
* 使用[ArrayPool\<T>](/dotnet/api/system.buffers.arraypool-1)來儲存大型陣列，以**執行**集區緩衝區。
* **請勿**在[熱程式碼路徑](#understand-hot-code-paths)上配置許多短期的大型物件。

您可以藉由檢查[PerfView](https://github.com/Microsoft/perfview)中的垃圾收集（GC）統計資料並檢查，來診斷上述的記憶體問題：

* 垃圾收集暫停時間。
* 花費在垃圾收集的處理器時間百分比。
* 層代0、1和2的垃圾收集數目。

如需詳細資訊，請參閱[垃圾收集和效能](/dotnet/standard/garbage-collection/performance)。

## <a name="optimize-data-access-and-io"></a>將資料存取和 i/o 優化

與資料存放區和其他遠端服務的互動通常是 ASP.NET Core 應用程式中最慢的部分。 有效率地讀取和寫入資料對於良好的效能非常重要。

建議：

* **請**以非同步方式呼叫所有資料存取 api。
* **請勿**抓取超過所需的資料。 撰寫查詢，只傳回目前 HTTP 要求所需的資料。
* 如果可以接受稍微過期的資料，**請考慮快**取從資料庫或遠端服務抓取的經常存取資料。 根據案例而定，請使用[MemoryCache](xref:performance/caching/memory)或[microsoft.web.distributedcache](xref:performance/caching/distributed)。 如需詳細資訊，請參閱<xref:performance/caching/response>。
* **儘量減少**網路來回行程。 其目標是要在單一呼叫中抓取所需的資料，而不是在數個呼叫中取得。
* 在存取資料進行唯讀時，**請不要**在 Entity Framework Core 中使用[無追蹤查詢](/ef/core/querying/tracking#no-tracking-queries)。 EF Core 可以更有效率地傳回無追蹤查詢的結果。
* **執行**篩選和匯總 LINQ 查詢（例如`.Where`， `.Select`使用、 `.Sum`或語句），以便篩選由資料庫執行。
* **請考慮 EF Core**在用戶端上解析一些查詢運算子，這可能會導致執行效率不佳的查詢。 如需詳細資訊，請參閱[用戶端評估效能問題](/ef/core/querying/client-eval#client-evaluation-performance-issues)。
* **請勿**在集合上使用投射查詢，這可能會導致執行 "N + 1" SQL 查詢。 如需詳細資訊，請參閱相互[關聯子查詢的優化](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries)。

如需可改善高擴充應用程式效能的方法，請參閱[EF 高效](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)能：

* [DbContext 共用](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [明確地編譯查詢](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

我們建議您先測量前述高效能方法的影響，再認可程式碼基底。 已編譯查詢的額外複雜度可能不會證明效能改進。

藉由[Application Insights](/azure/application-insights/app-insights-overview)或使用程式碼剖析工具來查看存取資料所花費的時間，可以偵測到查詢問題。 大部分的資料庫也會提供有關經常執行之查詢的統計資料。

## <a name="pool-http-connections-with-httpclientfactory"></a>使用 HttpClientFactory 集區 HTTP 連線

雖然[HttpClient](/dotnet/api/system.net.http.httpclient)會執行`IDisposable`介面，但它是為了重複使用而設計的。 封閉式`HttpClient`實例會讓通訊端在`TIME_WAIT`一小段時間內保持開啟狀態。 如果經常使用建立及處置`HttpClient`物件的程式碼路徑，應用程式可能會耗盡可用的通訊端。 [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)是在 ASP.NET Core 2.1 中引進，做為此問題的解決方案。 它會處理共用 HTTP 連線，以優化效能和可靠性。

建議：

* **請勿**直接建立和處置`HttpClient`實例。
* **請**務必使用[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)來`HttpClient`取出實例。 如需詳細資訊，請參閱[使用 HttpClientFactory 來執行可復原的 HTTP 要求](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)。

## <a name="keep-common-code-paths-fast"></a>快速保持通用程式碼路徑

您想要讓所有程式碼快速地進行。 經常呼叫的程式碼路徑是優化的最重要。 它們包括：

* 應用程式要求處理管線中的中介軟體元件，特別是在管線早期執行中介軟體。 這些元件對效能有很大的影響。
* 針對每個要求執行的程式碼，或每個要求多次。 例如，自訂記錄、授權處理常式或暫時性服務的初始化。

建議：

* **請勿**使用具有長時間執行之工作的自訂中介軟體元件。
* **請使用效能**分析工具（例如[Visual Studio 診斷工具](/visualstudio/profiling/profiling-feature-tour)或[PerfView](https://github.com/Microsoft/perfview)）來識別熱程式[代碼路徑](#understand-hot-code-paths)。

## <a name="complete-long-running-tasks-outside-of-http-requests"></a>完成 HTTP 要求以外的長時間執行工作

對 ASP.NET Core 應用程式的大部分要求可以由呼叫必要服務並傳回 HTTP 回應的控制器或頁面模型來處理。 對於涉及長時間執行之工作的某些要求，最好將整個要求-回應程式設為非同步。

建議：

* **請**不要等候長時間執行的工作在一般 HTTP 要求處理過程中完成。
* **請考慮使用** [Azure Function](/azure/azure-functions/)來處理具有[背景服務](xref:fundamentals/host/hosted-services)或跨進程的長時間執行要求。 跨進程完成工作對於需要大量 CPU 的工作特別有用。
* **請使用即時**通訊選項（例如[SignalR](xref:signalr/introduction)），以非同步方式與用戶端通訊。

## <a name="minify-client-assets"></a>縮小用戶端資產

具有複雜前端的 ASP.NET Core 應用程式經常會提供許多 JavaScript、CSS 或影像檔案。 初始載入要求的效能可以藉由下列方式改善：

* 將多個檔案結合成一個的組合。
* 縮小，藉由移除空白字元和批註來減少檔案大小。

建議：

* **請**使用 ASP.NET Core 的[內建支援](xref:client-side/bundling-and-minification)，來組合和縮小用戶端資產。
* **請考慮其他**協力廠商工具（例如[Webpack](https://webpack.js.org/)），以進行複雜的用戶端資產管理。

## <a name="compress-responses"></a>壓縮回應

 減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。 減少承載大小的其中一種方法是壓縮應用程式的回應。 如需詳細資訊，請參閱[回應壓縮](xref:performance/response-compression)。

## <a name="use-the-latest-aspnet-core-release"></a>使用最新的 ASP.NET Core 版本

ASP.NET Core 的每個新版本都包含效能改進。 .NET Core 和 ASP.NET Core 的優化意味著較新的版本通常會優於較舊的版本。 例如，.NET Core 2.1 已從[\<Span T>](https://msdn.microsoft.com/magazine/mt814808.aspx)新增編譯之正則運算式和受惠的支援。 ASP.NET Core 2.2 已新增對 HTTP/2 的支援。 [ASP.NET Core 3.0 新增了許多改善](xref:aspnetcore-3.0)，可減少記憶體使用量並改善輸送量。 如果效能是優先順序，請考慮升級至目前版本的 ASP.NET Core。

## <a name="minimize-exceptions"></a>最小化例外狀況

例外狀況應該很罕見。 相對於其他程式碼流程模式，擲回和攔截例外狀況的速度較慢。 因此，例外狀況不應該用來控制一般程式流程。

建議：

* **請勿**使用擲回或攔截例外狀況做為一般程式流程的方法，特別是在[熱程式碼路徑](#understand-hot-code-paths)中。
* **請**在應用程式中包含邏輯，以偵測並處理會造成例外狀況的條件。
* 針對異常或非預期的情況，**請**擲回或攔截例外狀況。

應用程式診斷工具（例如 Application Insights）有助於找出應用程式中可能會影響效能的常見例外狀況。

## <a name="performance-and-reliability"></a>效能與可靠性

下列各節提供效能秘訣和已知的可靠性問題和解決方案。

## <a name="avoid-synchronous-read-or-write-on-httprequesthttpresponse-body"></a>避免在 HttpRequest/HttpResponse 主體上進行同步讀取或寫入

ASP.NET Core 中的所有 i/o 都是非同步。 伺服器會執行`Stream`介面，其中同時具有同步和非同步多載。 最好的是非同步，以避免封鎖執行緒集區執行緒。 封鎖執行緒可能會導致執行緒集區耗盡。

不要**這麼做：** 下列範例會使用<xref:System.IO.StreamReader.ReadToEnd*>。 它會封鎖目前的執行緒來等候結果。 這是透過[async 同步](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
)的範例。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet1)]

在上述程式碼中`Get` ，會以同步方式將整個 HTTP 要求主體讀取到記憶體中。 如果用戶端緩慢上傳，應用程式會透過非同步進行同步處理。 應用程式會透過非同步進行同步處理，因為 Kestrel**不支援同步**讀取。

**請這樣做：** 下列範例會使用<xref:System.IO.StreamReader.ReadToEndAsync*> ，而不會在讀取時封鎖執行緒。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet2)]

上述程式碼會以非同步方式將整個 HTTP 要求主體讀取到記憶體中。

> [!WARNING]
> 如果要求很大，將整個 HTTP 要求主體讀取到記憶體中，可能會導致記憶體不足（OOM）狀況。 OOM 可能會導致拒絕服務。  如需詳細資訊，請參閱本檔中的[避免將大型要求內文或回應本文讀取到記憶體](#arlb)中。

**請這樣做：** 下列範例是使用非緩衝處理要求主體的完全非同步：

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MyFirstController.cs?name=snippet3)]

上述程式碼會以非同步方式將要求主體還原序列化成 c # 物件。

## <a name="prefer-readformasync-over-requestform"></a>偏好透過要求 ReadFormAsync。表單

使用 `HttpContext.Request.ReadFormAsync` 取代 `HttpContext.Request.Form`。
`HttpContext.Request.Form`只有在下列情況下，才能安全地讀取：

* 已由呼叫讀取表單`ReadFormAsync`，且
* 正在使用讀取快取的表單值`HttpContext.Request.Form`

不要**這麼做：** 下列範例會使用`HttpContext.Request.Form`。  `HttpContext.Request.Form`會使用[非同步同步](https://github.com/davidfowl/AspNetCoreDiagnosticScenarios/blob/master/AsyncGuidance.md#warning-sync-over-async
)處理，而且可能會導致執行緒集區耗盡。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet1)]

**請這樣做：** 下列範例會使用`HttpContext.Request.ReadFormAsync`來以非同步方式讀取表單主體。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/MySecondController.cs?name=snippet2)]

<a name="arlb"></a>

## <a name="avoid-reading-large-request-bodies-or-response-bodies-into-memory"></a>避免將大型要求內文或回應主體讀取到記憶體中

在 .NET 中，大於 85 KB 的每個物件配置最後都會出現在大型物件堆積（[LOH](https://blogs.msdn.microsoft.com/maoni/2006/04/19/large-object-heap/)）中。 大型物件的成本很高，方法有兩種：

* 配置成本很高，因為必須清除新配置之大型物件的記憶體。 CLR 保證會清除所有新設定物件的記憶體。
* LOH 會隨著堆積的其餘部分一起收集。 LOH 需要完整的[垃圾收集](/dotnet/standard/garbage-collection/fundamentals)或[Gen2 集合](/dotnet/standard/garbage-collection/fundamentals#generations)。

這[篇 blog 文章](https://adamsitnik.com/Array-Pool/#the-problem)會簡單說明此問題：

> 配置大型物件時，會將它標示為 Gen 2 物件。 不是針對小型物件的 Gen 0。 結果是，如果您在 LOH 中用盡記憶體，GC 就會清除整個受控堆積，而不只是 LOH。 因此，它會清除 Gen 0、Gen 1 和 Gen 2，包括 LOH。 這稱為「完整垃圾收集」，而且是最耗時的垃圾收集。 對於許多應用程式而言，這可能是可接受的。 但絕對不適用於高效能 web 伺服器，因為需要幾個海量儲存體緩衝區來處理平均 web 要求（從通訊端讀取、解壓縮、解碼 JSON & 更多）。

輕鬆自在管理將大型要求或回應本文儲存成單一`byte[]`或： `string`

* 可能會導致 LOH 中的空間快速耗盡。
* 可能會造成應用程式的效能問題，因為執行的是完整的 Gc。

## <a name="working-with-a-synchronous-data-processing-api"></a>使用同步資料處理 API

使用僅支援同步讀取和寫入的序列化程式/還原序列化程式時（例如， [JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)）：

* 將資料以非同步方式緩衝到記憶體中，然後再將它傳遞至序列化程式/還原序列化程式。

> [!WARNING]
> 如果要求很大，可能會導致記憶體不足（OOM）狀況。 OOM 可能會導致拒絕服務。  如需詳細資訊，請參閱本檔中的[避免將大型要求內文或回應本文讀取到記憶體](#arlb)中。

ASP.NET Core 3.0 預設<xref:System.Text.Json>會使用 JSON 序列化。 <xref:System.Text.Json>:

* 非同步讀取和寫入 JSON。
* 已針對 UTF-8 文字進行優化。
* 通常比`Newtonsoft.Json`更高的效能。

## <a name="do-not-store-ihttpcontextaccessorhttpcontext-in-a-field"></a>不要將 IHttpCoNtextAccessor 儲存在欄位中

[IHttpCoNtextAccessor](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext) `HttpContext`從要求執行緒存取時，會傳回使用中要求的。 不`IHttpContextAccessor.HttpContext`應**not**儲存在欄位或變數中。

不要**這麼做：** 下列範例會將儲存`HttpContext`在欄位中，然後在稍後嘗試使用它。

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet1)]

上述程式碼經常會在此函式`HttpContext`中捕獲 null 或不正確的。

**請這樣做：** 下列範例：

* 將儲存<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor>在欄位中。
* 在正確`HttpContext`的時間使用欄位，並檢查`null`。

[!code-csharp[](performance-best-practices/samples/3.0/MyType.cs?name=snippet2)]

## <a name="do-not-access-httpcontext-from-multiple-threads"></a>不要從多個執行緒存取 HttpCoNtext

`HttpContext`不*是安全*執行緒。 以`HttpContext`平行方式從多個執行緒存取可能會導致未定義的行為，例如停止回應、當機和資料損毀。

不要**這麼做：** 下列範例會建立三個平行要求，並記錄連出 HTTP 要求之前和之後的傳入要求路徑。 要求路徑可從多個執行緒存取，可能會平行處理。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet1&highlight=25,28)]

**請這樣做：** 下列範例會先複製傳入要求中的所有資料，再進行三個平行要求。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncFirstController.cs?name=snippet2&highlight=6,8,22,28)]

## <a name="do-not-use-the-httpcontext-after-the-request-is-complete"></a>要求完成後，請勿使用 HttpCoNtext

`HttpContext`只有在 ASP.NET Core 管線中有使用中的 HTTP 要求時，才有效。 整個 ASP.NET Core 管線是執行每個要求的非同步委派鏈。 當這個`Task`鏈傳回的完成時， `HttpContext`就會回收。

不要**這麼做：** 下列範例會使用`async void` ，讓 HTTP 要求在第一個`await`到達時完成：

* 這在 ASP.NET Core 應用程式中**一律**是不良的做法。
* 在 HTTP `HttpResponse`要求完成後存取。
* 導致進程當機。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncBadVoidController.cs?name=snippet1)]

**請這樣做：** 下列範例會將傳回`Task`至架構，因此在動作完成之前，HTTP 要求不會完成。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/AsyncSecondController.cs?name=snippet1)]

## <a name="do-not-capture-the-httpcontext-in-background-threads"></a>不要在背景執行緒中捕捉 HttpCoNtext

不要**這麼做：** 下列範例顯示關閉正在`HttpContext`從`Controller`屬性中捕捉。 這是不正確的作法，因為工作專案可能會：

* 在要求範圍外執行。
* 嘗試讀取錯誤`HttpContext`的。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet1)]

**請這樣做：** 下列範例：

* 在要求期間複製背景工作中所需的資料。
* 未參考控制器中的任何專案。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetFirstController.cs?name=snippet2)]

背景工作應實作為託管服務。 如需詳細資訊，請參閱[搭配託管服務的背景工作](xref:fundamentals/host/hosted-services)。

## <a name="do-not-capture-services-injected-into-the-controllers-on-background-threads"></a>不要在背景執行緒上捕捉插入至控制器的服務

不要**這麼做：** 下列範例顯示關閉是`DbContext`從`Controller` action 參數捕捉。 這是不正確的作法。  工作專案可以在要求範圍外執行。 的`ContosoDbContext`範圍是要求，因而產生`ObjectDisposedException`。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet1)]

**請這樣做：** 下列範例：

* <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory>插入，以便在背景工作專案中建立範圍。 `IServiceScopeFactory`是單一的。
* 在背景執行緒中建立新的相依性插入範圍。
* 未參考控制器中的任何專案。
* 不會`ContosoDbContext`從傳入要求中捕捉。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2)]

下列反白顯示的程式碼：

* 建立背景作業存留期的範圍，並從中解析服務。
* 從`ContosoDbContext`正確的範圍使用。

[!code-csharp[](performance-best-practices/samples/3.0/Controllers/FireAndForgetSecondController.cs?name=snippet2&highlight=9-16)]

## <a name="do-not-modify-the-status-code-or-headers-after-the-response-body-has-started"></a>啟動回應主體之後，請勿修改狀態碼或標頭

ASP.NET Core 不會緩衝 HTTP 回應主體。 第一次寫入回應時：

* 標頭會連同主體的該區塊一起傳送到用戶端。
* 您不能再變更回應標頭。

不要**這麼做：** 下列程式碼會在回應已經開始之後，嘗試新增回應標頭：

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet1)]

在上述程式碼中`context.Response.Headers["test"] = "test value";` ，如果`next()`已寫入回應，將會擲回例外狀況。

**請這樣做：** 下列範例會先檢查 HTTP 回應是否已啟動，然後再修改標頭。

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet2)]

**請這樣做：** 下列範例會先`HttpResponse.OnStarting`使用來設定標頭，再將回應標頭排清至用戶端。

檢查回應是否未啟動，可讓您在寫入回應標頭之前，註冊將會叫用的回呼。 檢查回應是否尚未啟動：

* 提供可即時附加或覆寫標頭的功能。
* 不需要知道管線中的下一個中介軟體。

[!code-csharp[](performance-best-practices/samples/3.0/Startup22.cs?name=snippet3)]

## <a name="do-not-call-next-if-you-have-already-started-writing-to-the-response-body"></a>如果您已經開始寫入回應主體，請勿呼叫 next （）

元件只有在可以處理和操作回應時才會被呼叫。
