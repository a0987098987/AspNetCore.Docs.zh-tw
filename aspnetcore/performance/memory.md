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
# <a name="memory-management-and-garbage-collection-gc-in-aspnet-core"></a>ASP.NET核心中的記憶體管理和垃圾回收 (GC)

由[塞巴斯蒂安·羅斯](https://github.com/sebastienros)和[里克·安德森](https://twitter.com/RickAndMSFT)

記憶體管理很複雜,即使在像 .NET 這樣的託管框架中也是如此。 分析和理解記憶體問題可能具有挑戰性。 本文：

* 受許多*記憶體洩漏*和*GC 不工作*問題引起的。 這些問題大多是由於不瞭解記憶體消耗在 .NET Core 中的工作方式,或者不瞭解如何測量記憶體消耗造成的。
* 演示有問題的記憶體使用,並建議替代方法。

## <a name="how-garbage-collection-gc-works-in-net-core"></a>垃圾回收 (GC) 在 .NET 核心中的工作方式

GC 分配堆段,其中每個段是連續的記憶體範圍。 放置在堆中的物件分為 3 代之一:0、1 或 2。 生成確定 GC 嘗試釋放應用程式不再引用的託管物件上的記憶體的頻率。 編號較低的代是 GC 更頻繁地使用。

對象根據其生存期從一代移動到另一代。 隨著物件壽命的延長,它們被移動到更高的一代。 如前所述,較高一代是 GC 的較少頻率。 短期生存對象始終保留在第 0 代中。 例如,在 Web 請求期間引用的對像是短暫的。 應用程式級[單例](xref:fundamentals/dependency-injection#service-lifetimes)通常遷移到第 2 代。

當 ASP.NET 核心應用啟動時,GC:

* 為初始堆段保留一些記憶體。
* 載入運行時時提交一小部分記憶體。

出於性能原因,上述記憶體分配完成。 性能優勢來自連續記憶體中的堆段。

### <a name="call-gccollect"></a>調用 GC。收集

調用[GC。明確收集](xref:System.GC.Collect*):

* **不應**通過生產ASP.NET核心應用來完成。
* 在調查記憶體洩漏時很有用。
* 調查時,驗證 GC 從記憶體中刪除了所有懸空物件,以便可以測量記憶體。

## <a name="analyzing-the-memory-usage-of-an-app"></a>分析應用的記憶體使用方式

專用工具可幫助分析記憶體使用方式:

- 計數物件參照
- 測量 GC 對 CPU 使用率的影響
- 測量每一代使用的記憶體空間

使用以下工具分析記憶體使用方式:

* [點網跟蹤](/dotnet/core/diagnostics/dotnet-trace):可用於生產機器。
* [無需視覺化工作室調試器即可分析記憶體使用方式](/visualstudio/profiling/memory-usage-without-debugging2)
* [分析 Visual Studio 中的記憶體使用量](/visualstudio/profiling/memory-usage)

### <a name="detecting-memory-issues"></a>偵測記憶體問題

任務管理器可用於瞭解應用使用的ASP.NET記憶體量。 工作管理員記憶體值:

* 表示ASP.NET進程使用的記憶體量。
* 包括應用的活物件和其他記憶體消費者,如本機記憶體使用方式。

如果任務管理器記憶體值無限增加且從不變展,則應用會洩漏記憶體。 以下各節演示並解釋幾種記憶體使用模式。

## <a name="sample-display-memory-usage-app"></a>範例顯示記憶體使用方式套用

[記憶體洩漏示例應用](https://github.com/sebastienros/memoryleak)在 GitHub 上可用。 記憶體洩漏應用:

* 包括一個診斷控制器,用於收集應用的即時記憶體和 GC 數據。
* 具有顯示記憶體和 GC 數據的索引頁。 每秒刷新一次索引頁。
* 包含 API 控制器,該控制器提供各種記憶體載入模式。
* 不是受支援的工具,但是,它可用於顯示ASP.NET核心應用的記憶體使用模式。

運行記憶體洩漏。 分配的記憶體會緩慢增加,直到發生 GC。 記憶體增加,因為該工具分配自定義物件以捕獲數據。 下圖顯示了發生第 0 代 GC 時記憶體洩漏索引頁。 該圖表顯示 0 RPS(每秒請求),因為未調用來自 API 控制器的 API 終結點。

![前一圖表](memory/_static/0RPS.png)

圖表顯示記憶體使用方式的兩個值:

- 已分配:託管物件佔用的記憶體量
- [工作集](/windows/win32/memory/working-set):進程虛擬位址空間中的頁面集,當前駐留在物理記憶體中。 顯示的工作集與任務管理器顯示的值相同。

### <a name="transient-objects"></a>瞬態物件

以下 API 創建一個 10 KB 字串實例並將其返回到用戶端。 在每個請求上,一個新對象在記憶體中分配並寫入回應。 字串在 .NET 中儲存為 UTF-16 字元,因此每個字元在記憶體中需要 2 個字節。

```csharp
[HttpGet("bigstring")]
public ActionResult<string> GetBigString()
{
    return new String('x', 10 * 1024);
}
```

以下圖形的負載相對較小,以顯示記憶體分配如何受到 GC 的影響。

![前一圖表](memory/_static/bigstring.png)

上圖顯示:

* 4K RPS(每秒請求)。
* 第 0 代 GC 集合大約每兩秒發生一次。
* 工作集保持不變,約為 500 MB。
* CPU 為 12%。
* 記憶體消耗和釋放(通過 GC)穩定。

下圖以機器可以處理的最大輸送量進行。

![前一圖表](memory/_static/bigstring2.png)

上圖顯示:

* 22K RPS
* 第 0 代 GC 集合每秒發生多次。
* 第 1 代集合被觸發,因為應用每秒分配的記憶體顯著增加。
* 工作集保持不變,約為 500 MB。
* CPU 為 33%。
* 記憶體消耗和釋放(通過 GC)穩定。
* CPU (33%)未過度使用,因此垃圾回收可以跟上大量分配。

### <a name="workstation-gc-vs-server-gc"></a>工作站 GC 與伺服器 GC

.NET 垃圾回收器有兩種不同的模式:

* **工作站 GC**:針對桌面進行了優化。
* **伺服器 GC**。 ASP.NET核心應用的預設 GC。 針對伺服器進行了優化。

GC 模式可以在專案檔或已發佈應用程式的*運行時 config.json*檔中顯式設置。 以下標記顯示項目檔中的`ServerGarbageCollection`設定:

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

在`ServerGarbageCollection`專案檔中更改需要重新生成應用。

**註:** 伺服器垃圾資源在具有單個核心的電腦上**無法使用**。 如需詳細資訊，請參閱 <xref:System.Runtime.GCSettings.IsServerGC>。

下圖顯示了使用工作站 GC 的 5K RPS 下的記憶體配置檔。

![前一圖表](memory/_static/workstation.png)

此圖表和伺服器版本之間的差異很大:

- 工作集從 500 MB 下降到 70 MB。
- GC 每秒生成數次集合,而不是每兩秒生成一次。
- GC 從 300 MB 下降到 10 MB。

在典型的 Web 伺服器環境中,CPU 使用率比記憶體更重要,因此伺服器 GC 更好。 如果記憶體利用率高且 CPU 使用率相對較低,則工作站 GC 可能更具有性能。 例如,高密度託管多個記憶體不足的 Web 應用。

<a name="sc"></a>

### <a name="gc-using-docker-and-small-containers"></a>使用 Docker 與小型容器的 GC

當在一台電腦上運行多個容器化應用時,工作站 GC 可能比伺服器 GC 更具預製功能。 有關詳細資訊,請參閱[在小型容器中使用伺服器 GC 執行](https://devblogs.microsoft.com/dotnet/running-with-server-gc-in-a-small-container-scenario-part-0/),並在[小型容器方案第 1 部分中使用伺服器 GC 執行 = GC 堆的硬限制](https://devblogs.microsoft.com/dotnet/running-with-server-gc-in-a-small-container-scenario-part-1-hard-limit-for-the-gc-heap/)。

### <a name="persistent-object-references"></a>持久物件參照

GC 無法釋放引用的物件。 引用但不再需要的物件會導致記憶體洩漏。 如果應用經常分配物件,並且在不再需要物件后無法釋放它們,則記憶體使用量將隨著時間的推移而增加。

以下 API 創建一個 10 KB 字串實例並將其返回到用戶端。 與上一個示例的區別是此實例由靜態成員引用,這意味著它永遠不會可用於收集。

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

上述程式碼：

* 是典型的記憶體洩漏的示例。
* 頻繁調用時,會導致應用記憶體增加,直到進程崩潰,`OutOfMemory`出現異常。

![前一圖表](memory/_static/eternal.png)

在前面的影像中:

* 負載測試`/api/staticstring`終結點會導致記憶體線性增加。
* GC 嘗試通過調用第 2 代集合來釋放記憶體,因為記憶體壓力增大。
* GC 無法釋放洩漏的記憶體。 分配和工作集會隨時間而增加。

某些方案(如緩存)要求保持物件引用,直到記憶體壓力強制釋放它們。 類<xref:System.WeakReference>可用於這種類型的緩存代碼。 對`WeakReference`像是在記憶體壓力下收集的。 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>使用的`WeakReference`默認實現。

### <a name="native-memory"></a>本機記憶體

某些 .NET Core 物件依賴於本機記憶體。 GC**無法**收集本機記憶體。 使用本機記憶體的 .NET物件必須使用本機代碼釋放它。

.NET<xref:System.IDisposable>提供允許開發人員釋放本機記憶體的介面。 即使<xref:System.IDisposable.Dispose*>未調用,在[終結器](/dotnet/csharp/programming-guide/classes-and-structs/destructors)運行時`Dispose`,也會正確實現類調用。

請考慮下列程式碼：

```csharp
[HttpGet("fileprovider")]
public void GetFileProvider()
{
    var fp = new PhysicalFileProvider(TempPath);
    fp.Watch("*.*");
}
```

[物理檔提供程式](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0)是託管類,因此將在請求結束時收集任何實例。

下圖顯示連續調用 API`fileprovider`時的 記憶體配置檔。

![前一圖表](memory/_static/fileprovider.png)

前面的圖表顯示了此類實現的一個明顯問題,因為它不斷增加記憶體使用量。 這是一個已知的問題,正在跟蹤[在此問題](https://github.com/dotnet/aspnetcore/issues/3110)。

使用者代碼中也可能發生相同的洩漏,其原因之一如下:

* 未正確釋放類。
* 忘記調用應釋放的`Dispose`從屬物件的方法。

### <a name="large-objects-heap"></a>大型物件堆

頻繁的記憶體分配/空閒週期可能會分散記憶體,尤其是在分配大塊記憶體時。 對象以連續的記憶體塊分配。 為了緩解碎片,當 GC 釋放記憶體時,它會嘗試對它進行碎片整理。 此過程為**壓縮**。 壓實涉及移動物件。 移動大型物件會造成性能損失。 因此,GC 會為_大型_物件(稱為[大型物件堆](/dotnet/standard/garbage-collection/large-object-heap)(LOH) 創建特殊內存區域。 大於 85,000 位元組(約 83 KB)的物件是:

* 放置在 LOH 上。
* 未壓縮。
* 在第 2 代 GC 期間收集。

當 LOH 已滿時,GC 將觸發第 2 代集合。 第二代集合:

* 本質上是緩慢的。
* 此外,還要承擔觸發所有其他代的集合的成本。

以下代碼可立即壓縮 LOH:

```csharp
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();
```

有關<xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode>壓縮 LOH 的資訊,請參閱。

在使用 .NET Core 3.0 及更高版本的容器中,LOH 會自動壓縮。

以下說明此行為的 API:

```csharp
[HttpGet("loh/{size=85000}")]
public int GetLOH1(int size)
{
   return new byte[size].Length;
}
```

下圖顯示了在最大負載下調用終結點的`/api/loh/84975`記憶體配置檔:

![前一圖表](memory/_static/loh1.png)

下圖顯示了呼叫終結點的`/api/loh/84976`記憶體設定檔,僅分配*了一個字節*:

![前一圖表](memory/_static/loh2.png)

注意:結構`byte[]`具有開銷位元組。 這就是為什麼 84,976 位元組觸發 85,000 限制的原因。

比較前面的兩個圖表:

* 這兩種情況的工作集都類似,大約 450 MB。
* LOH 下的請求(84,975 位元組)主要顯示第0代集合。
* 過 LOH 請求生成恆定的第 2 代集合。 第 2 代集合成本高昂。 需要更多的 CPU,輸送量下降近 50%。

臨時大型物件特別成問題,因為它們會導致第 2 代 GC。

為了達到最佳性能,應盡量減少大型物件的使用。 如果可能,拆分大型物件。 例如,ASP.NET Core 中的[回應緩存](xref:performance/caching/response)中間件將緩存條目拆分為小於 85,000 位元組的塊。

以下連結顯示了將物件保持在 LOH 限制下的 ASP.NET 核心方法:

* [回應快取/流/流實用程式。cs](https://github.com/dotnet/AspNetCore/blob/v3.0.0/src/Middleware/ResponseCaching/src/Streams/StreamUtilities.cs#L16)
* [回應快取/記憶體回應快取.cs](https://github.com/aspnet/ResponseCaching/blob/c1cb7576a0b86e32aec990c22df29c780af29ca5/src/Microsoft.AspNetCore.ResponseCaching/Internal/MemoryResponseCache.cs#L55)

如需詳細資訊，請參閱

* [未覆寫的大物件堆](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [大型物件堆](/dotnet/standard/garbage-collection/large-object-heap)

### <a name="httpclient"></a>HttpClient

使用<xref:System.Net.Http.HttpClient>不當可能會導致資源洩漏。 系統資源,如資料庫連接、套接字、檔句柄等:

* 比記憶更稀缺。
* 洩漏時比記憶體問題更大。

經驗豐富的 .NET 開發<xref:System.IDisposable.Dispose*>人員知道<xref:System.IDisposable>調用實現 的物件。 不釋放實現`IDisposable`的物件通常會導致記憶體洩漏或系統資源洩漏。

`HttpClient`實現`IDisposable`,但**不應**在每一次調用時都處置。 相反,`HttpClient`應該重複使用。

以下終結點在每個請求上建立並釋放一`HttpClient`個新實例:

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

在負載下,將記錄以下錯誤訊息:

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

即使`HttpClient`實例已釋放,操作系統也會釋放實際網路連接。 以不斷建立新連線,_連接埠耗盡_。 每個用戶端連接都需要其自己的用戶端埠。

防止連接埠耗盡的一種方法是重用同一`HttpClient`實例:

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

`HttpClient`當應用停止時,實例將被釋放。 此示例顯示,在每次使用后,不應釋放每個一次性資源。

有關處理`HttpClient`實例存留期的更好方法,請參閱以下內容:

* [HttpClient 和存留期管理](/aspnet/core/fundamentals/http-requests#httpclient-and-lifetime-management)
* [HTTPClient 工廠部落格](https://devblogs.microsoft.com/aspnet/asp-net-core-2-1-preview1-introducing-httpclient-factory/)
 
### <a name="object-pooling"></a>物件池

前面的示例演示如何使`HttpClient`實例成為靜態的,並被所有請求重用。 重用可防止資源耗盡。

物件池:

* 使用重用模式。
* 專為創建成本高昂的對象而設計。

池是預初始化物件的集合,可以在線程之間保留和釋放。 池可以定義分配規則,如限制、預定義大小或增長率。

NuGet 包[Microsoft.擴展.ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/)包含有助於管理此類池的類。

以下 API 終結點實例`byte`化了 在每個請求上填充隨機數的緩衝區:

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

下圖顯示了使用中等負載呼叫前面的 API 的圖表顯示:

![前一圖表](memory/_static/array.png)

在前面的圖表中,第 0 代集合大約每秒發生一次。

可以通過使用[ArrayPool\<T>](xref:System.Buffers.ArrayPool`1)`byte`池化緩衝區來優化前面的代碼。 靜態實例跨請求重用。

此方法的不同做法是從 API 返回池物件。 這意味著:

* 從 方法返回時,物件將失去控制。
* 無法釋放物件。

要設定物件的處置:

* 將池陣組封裝在一次性物件中。
* 使用[HttpContext.Response.註冊為 Dispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*)註冊池物件。

`RegisterForDispose`將負責對目標物件的`Dispose`調用,以便僅在 HTTP 請求完成時釋放它。

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

應用與非池式版本相同的負載會導致以下圖表:

![前一圖表](memory/_static/pooledarray.png)

主要區別是分配位元組,因此,第 0 代集合要少得多。

## <a name="additional-resources"></a>其他資源

* [記憶體回收](/dotnet/standard/garbage-collection/)
* [使用並發視覺化工具瞭解不同的 GC 模式](https://blogs.msdn.microsoft.com/seteplia/2017/01/05/understanding-different-gc-modes-with-concurrency-visualizer/)
* [未覆寫的大物件堆](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [大型物件堆](/dotnet/standard/garbage-collection/large-object-heap)
