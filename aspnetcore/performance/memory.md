---
title: ASP.NET Core 中的記憶體管理和模式
author: rick-anderson
description: 瞭解記憶體在 ASP.NET Core 中的管理方式，以及垃圾收集行程（GC）的運作方式。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: performance/memory
ms.openlocfilehash: dfc789d080beec09a4f0eb34c3809b9f2df0d4b5
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75357274"
---
# <a name="memory-management-and-garbage-collection-gc-in-aspnet-core"></a>ASP.NET Core 中的記憶體管理和垃圾收集（GC）

By [Sébastien Ros](https://github.com/sebastienros)和[Rick Anderson](https://twitter.com/RickAndMSFT)

記憶體管理是很複雜的，即使是在 .NET 之類的 managed 架構中也一樣。 分析和瞭解記憶體問題可能是一項挑戰。 這篇文章：

* 有許多*記憶體*流失和*GC 無法運作*的問題。 大部分的問題都是因為不了解記憶體耗用量在 .NET Core 中的運作方式，或不了解其測量方式所造成。
* 示範有問題的記憶體使用，並建議替代的方法。

## <a name="how-garbage-collection-gc-works-in-net-core"></a>垃圾收集（GC）在 .NET Core 中的運作方式

GC 會配置堆積區段，其中每個區段都是連續的記憶體範圍。 放在堆積中的物件會分類成3個層代中的其中一個：0、1或2。 產生會決定 GC 嘗試釋放應用程式不再參考之受管理物件記憶體的頻率。 較低編號的層代會更頻繁地進行 GC。

物件會根據其存留期從一個世代移至另一個層代。 隨著物件的存留時間較久，它們會移至較高的層代。 如先前所述，較高的層代是較不常的 GC。 短期存留期的物件一律會保留在層代0中。 例如，在 web 要求存留期間所參考的物件會短暫存留。 應用層級[單次個體](xref:fundamentals/dependency-injection#service-lifetimes)通常會遷移至層代2。

當 ASP.NET Core 應用程式啟動時，GC：

* 會針對初始堆積區段保留一些記憶體。
* 載入執行時間時，認可記憶體的一小部分。

先前的記憶體配置是基於效能考慮而完成。 效能優勢來自連續記憶體中的堆積區段。

### <a name="call-gccollect"></a>呼叫 GC。收集

呼叫[GC。](xref:System.GC.Collect*)明確地收集：

* **不**應該由生產 ASP.NET Core 應用程式來完成。
* 在調查記憶體流失時很有用。
* 進行調查時，會確認 GC 已從記憶體中移除所有無關聯物件，以便測量記憶體。

## <a name="analyzing-the-memory-usage-of-an-app"></a>分析應用程式的記憶體使用量

專用工具可以協助分析記憶體使用量：

- 計算物件參考
- 測量 GC 對 CPU 使用量有多少影響
- 測量每個層代所使用的記憶體空間

使用下列工具來分析記憶體使用量：

* [dotnet-trace](/dotnet/core/diagnostics/dotnet-trace)：可用於生產機器。
* [不使用 Visual Studio 偵錯工具分析記憶體使用量](/visualstudio/profiling/memory-usage-without-debugging2)
* [分析 Visual Studio 中的記憶體使用狀況](/visualstudio/profiling/memory-usage)

### <a name="detecting-memory-issues"></a>偵測記憶體問題

工作管理員可用來瞭解 ASP.NET 應用程式所使用的記憶體數量。 [工作管理員記憶體] 值：

* 代表 ASP.NET 進程所使用的記憶體數量。
* 包含應用程式的生活物件以及其他記憶體取用者，例如原生記憶體使用量。

如果工作管理員記憶體值無限期地增加，而且永遠不會壓平合併，應用程式就會發生記憶體流失的情況。 下列各節將示範和說明數個記憶體使用模式。

## <a name="sample-display-memory-usage-app"></a>範例顯示記憶體使用量應用程式

[MemoryLeak 範例應用程式](https://github.com/sebastienros/memoryleak)可在 GitHub 上取得。 MemoryLeak 應用程式：

* 包含診斷控制器，可收集應用程式的即時記憶體和 GC 資料。
* 具有顯示記憶體和 GC 資料的 [索引] 頁面。 [索引] 頁面會每秒重新整理一次。
* 包含可提供各種記憶體負載模式的 API 控制器。
* 不是支援的工具，不過，它可以用來顯示 ASP.NET Core 應用程式的記憶體使用模式。

執行 MemoryLeak。 配置的記憶體會慢慢增加，直到發生 GC 為止。 記憶體會增加，因為此工具會配置自訂物件來捕獲資料。 下圖顯示發生 Gen 0 GC 時的 MemoryLeak 索引頁面。 此圖表顯示0個 RPS （每秒的要求數），因為尚未呼叫來自 API 控制器的 API 端點。

![先前的圖表](memory/_static/0RPS.png)

此圖表會顯示記憶體使用量的兩個值：

- 已配置：受管理物件所佔用的記憶體數量
- [工作集](/windows/win32/memory/working-set)：目前位於實體記憶體中的進程虛擬位址空間內的一組頁面。 所顯示的工作集與 [工作管理員] 會顯示相同的值。

### <a name="transient-objects"></a>暫時性物件

下列 API 會建立一個 10 KB 的字串實例，並將它傳回給用戶端。 在每個要求上，會在記憶體中配置新的物件，並將其寫入至回應。 字串會在 .NET 中儲存為 UTF-16 字元，因此每個字元會在記憶體中使用2個位元組。

```csharp
[HttpGet("bigstring")]
public ActionResult<string> GetBigString()
{
    return new String('x', 10 * 1024);
}
```

下圖會以相對較小的負載產生，以顯示 GC 如何影響記憶體配置。

![先前的圖表](memory/_static/bigstring.png)

上圖顯示：

* 4K RPS （每秒的要求數）。
* 層代 0 GC 回收大約每兩秒發生一次。
* 工作集在大約 500 MB 是常數。
* CPU 為12%。
* 記憶體耗用量和釋放（透過 GC）是穩定的。

下圖是以機器可以處理的最大輸送量為依據。

![先前的圖表](memory/_static/bigstring2.png)

上圖顯示：

* 22K RPS
* 層代 0 GC 回收每秒發生數次。
* 系統會觸發第1代集合，因為應用程式每秒配置的記憶體會大幅增加。
* 工作集在大約 500 MB 是常數。
* CPU 為33%。
* 記憶體耗用量和釋放（透過 GC）是穩定的。
* CPU （33%）未過度使用，因此垃圾收集可能會跟上大量的配置。

### <a name="workstation-gc-vs-server-gc"></a>工作站 GC 與伺服器 GC 的比較

.NET 垃圾收集行程有兩種不同的模式：

* **工作站 GC**：針對桌面優化。
* **伺服器 GC**。 ASP.NET Core 應用程式的預設 GC。 已針對伺服器進行優化。

GC 模式可以在專案檔或已發佈應用程式的 *.runtimeconfig.json*中明確設定。 下列標記會顯示在專案檔中設定 `ServerGarbageCollection`：

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

變更專案檔中的 `ServerGarbageCollection` 需要重建應用程式。

**注意：** 在具有單一核心的機器上**無法**使用伺服器垃圾收集。 如需詳細資訊，請參閱<xref:System.Runtime.GCSettings.IsServerGC>。

下圖顯示使用工作站 GC 之已測的 RPS 下的記憶體設定檔。

![先前的圖表](memory/_static/workstation.png)

此圖表與伺服器版本之間的差異很重要：

- 工作集從 500 MB 降到 70 MB。
- GC 每秒會執行層代0回收多次，而不是每兩秒。
- GC 從 300 MB 降到 10 MB。

在典型的 web 伺服器環境中，CPU 使用量比記憶體更重要，因此伺服器 GC 會比較好。 如果記憶體使用率很高，且 CPU 使用率相對較低，則工作站 GC 可能更具效能。 例如，在記憶體不足的情況下，裝載數個 web 應用程式的高密度。

### <a name="persistent-object-references"></a>持續性物件參考

GC 無法釋放所參考的物件。 已參考但不再需要的物件會導致記憶體流失。 如果應用程式經常設定物件，而且不再需要時無法加以釋放，記憶體使用量會隨著時間而增加。

下列 API 會建立一個 10 KB 的字串實例，並將它傳回給用戶端。 上一個範例的差異在於，此實例是由靜態成員所參考，這表示它永遠無法用於集合。

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

* 是一般記憶體流失的範例。
* 使用頻繁的呼叫，會導致應用程式記憶體增加，直到進程損毀並出現 `OutOfMemory` 例外狀況。

![先前的圖表](memory/_static/eternal.png)

在上圖中：

* 負載測試 `/api/staticstring` 端點會導致記憶體中的線性增加。
* GC 會藉由呼叫第2代回收，在記憶體壓力增加時，嘗試釋放記憶體。
* GC 無法釋放洩漏的記憶體。 已配置和工作集增加了一段時間。

某些案例（例如快取）需要保留物件參考，直到記憶體壓力強制釋放它們為止。 <xref:System.WeakReference> 類別可以用於這種類型的快取程式碼。 記憶體壓力下會收集 `WeakReference` 物件。 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 的預設執行會使用 `WeakReference`。

### <a name="native-memory"></a>原生記憶體

有些 .NET Core 物件依賴原生記憶體。 GC**無法**收集原生記憶體。 使用原生記憶體的 .NET 物件必須使用機器碼釋放它。

.NET 提供 <xref:System.IDisposable> 介面，讓開發人員釋放原生記憶體。 即使未呼叫 <xref:System.IDisposable.Dispose*>，在完成項執行時，正確實作為[的類別](/dotnet/csharp/programming-guide/classes-and-structs/destructors)也會呼叫 `Dispose`。

請考慮下列程式碼：

```csharp
[HttpGet("fileprovider")]
public void GetFileProvider()
{
    var fp = new PhysicalFileProvider(TempPath);
    fp.Watch("*.*");
}
```

[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0)是 managed 類別，因此會在要求結束時收集任何實例。

下圖顯示連續叫用 `fileprovider` API 時的記憶體設定檔。

![先前的圖表](memory/_static/fileprovider.png)

上圖顯示此類別的執行明顯問題，因為它會持續增加記憶體使用量。 這是在[此問題](https://github.com/aspnet/Home/issues/3110)中追蹤的已知問題。

在使用者程式碼中，可能會發生相同的流失，如下所示：

* 未正確釋放類別。
* 忘記叫用應處置之相依物件的 `Dispose`方法。

### <a name="large-objects-heap"></a>大型物件堆積

頻繁的記憶體配置/免費週期可以分割記憶體，特別是在配置大型記憶體區塊時。 物件會配置在連續的記憶體區塊中。 為了減輕片段，當 GC 釋放記憶體時，它嘗試重組。 此進程稱為「**壓縮**」。 壓縮牽涉到移動物件。 移動大型物件會對效能造成負面影響。 基於這個理由，GC 會為_大型_物件（稱為[大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)（LOH））建立一個特殊的記憶體區域。 大於85000位元組（大約 83 KB）的物件為：

* 放在 LOH 上。
* 未壓縮。
* 在層代 2 Gc 期間收集。

當 LOH 已滿時，GC 將會觸發層代2回收。 第2代集合：

* 本質上很慢。
* 此外，也會產生在所有其他層代上觸發集合的成本。

下列程式碼會立即壓縮 LOH：

```csharp
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();
```

如需有關壓縮 LOH 的詳細資訊，請參閱 <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode>。

在使用 .NET Core 3.0 和更新版本的容器中，會自動壓縮 LOH。

下列 API 會說明這個行為：

```csharp
[HttpGet("loh/{size=85000}")]
public int GetLOH1(int size)
{
   return new byte[size].Length;
}
```

下圖顯示在 [最大負載] 底下呼叫 `/api/loh/84975` 端點的記憶體設定檔：

![先前的圖表](memory/_static/loh1.png)

下圖顯示呼叫 `/api/loh/84976` 端點的記憶體設定檔，*只配置一個位元組*：

![先前的圖表](memory/_static/loh2.png)

注意： `byte[]` 結構有額外的位元組。 這就是為什麼84976個位元組會觸發85000限制的原因。

比較上述兩個圖表：

* 這兩種案例的工作集都很類似，大約是 450 MB。
* [LOH 要求（84975位元組）] 底下顯示大部分的層代0回收。
* Over LOH 要求會產生常數層代2回收。 第2代回收的成本很高。 需要更多的 CPU，輸送量幾乎下降了50%。

暫存大型物件特別有問題，因為它們會造成 gen2 Gc。

為了達到最大效能，應該將大型物件使用降至最低。 可能的話，請分割大型物件。 例如，ASP.NET Core 中的[回應](xref:performance/caching/response)快取中介軟體會將快取專案分割成小於85000個位元組的區塊。

下列連結顯示將物件保留在 LOH 限制之下的 ASP.NET Core 方法：
- [ResponseCaching/資料流程/StreamUtilities .cs](https://github.com/aspnet/AspNetCore/blob/v3.0.0/src/Middleware/ResponseCaching/src/Streams/StreamUtilities.cs#L16)
- [ResponseCaching/MemoryResponseCache .cs](https://github.com/aspnet/ResponseCaching/blob/c1cb7576a0b86e32aec990c22df29c780af29ca5/src/Microsoft.AspNetCore.ResponseCaching/Internal/MemoryResponseCache.cs#L55)

如需詳細資訊，請參閱＜＞。

* [發現大型物件堆積](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)

### <a name="httpclient"></a>HttpClient

不正確地使用 <xref:System.Net.Http.HttpClient> 會導致資源洩漏。 系統資源，例如資料庫連接、通訊端、檔案控制代碼等：

* 比記憶體更少。
* 當洩漏記憶體時，會有更多的問題。

有經驗的 .NET 開發人員知道要在執行 <xref:System.IDisposable>的物件上呼叫 <xref:System.IDisposable.Dispose*>。 不處置執行 `IDisposable` 的物件，通常會導致記憶體流失或系統資源洩漏。

`HttpClient` 會執行 `IDisposable`，但**不**應該在每次叫用時加以處置。 相反地，應該重複使用 `HttpClient`。

下列端點會在每個要求上建立和處置新的 `HttpClient` 實例：

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

在負載之下，會記錄下列錯誤訊息：

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

雖然會處置 `HttpClient` 實例，但實際的網路連線需要一些時間才能由作業系統釋放。 藉由持續建立新的連接，就會發生_埠耗盡_。 每個用戶端連接都需要自己的用戶端埠。

防止埠耗盡的方法之一，就是重複使用相同的 `HttpClient` 實例：

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

當應用程式停止時，就會釋放 `HttpClient` 實例。 這個範例顯示，每次使用之後，不應處置每個可處置的資源。

請參閱下列內容，以取得更好的方法來處理 `HttpClient` 實例的存留期：

* [HttpClient 和存留期管理](/aspnet/core/fundamentals/http-requests#httpclient-and-lifetime-management)
* [HTTPClient factory 的 blog](https://devblogs.microsoft.com/aspnet/asp-net-core-2-1-preview1-introducing-httpclient-factory/)
 
### <a name="object-pooling"></a>物件共用

先前的範例示範如何將 `HttpClient` 實例設為靜態，並由所有要求重複使用。 重複使用會導致資源不足。

物件共用：

* 會使用重複使用模式。
* 是針對建立成本昂貴的物件所設計。

集區是預先初始化的物件集合，可以線上程之間保留和釋放。 集區可以定義配置規則，例如限制、預先定義的大小或成長率。

[ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/)的 NuGet 套件包含可協助管理這類集區的類別。

下列 API 端點會具現化在每個要求上填入亂數字的 `byte` 緩衝區：

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

下列圖表顯示使用中等負載呼叫先前的 API：

![先前的圖表](memory/_static/array.png)

在上圖中，層代0回收大約每秒發生一次。

上述程式碼可以藉由使用[ArrayPool\<t >](xref:System.Buffers.ArrayPool`1)來將 `byte` 緩衝區進行優化。 靜態實例會在要求之間重複使用。

這種方法的不同之處在于，會從 API 傳回集區物件。 這表示：

* 當您從方法傳回時，物件就會從您的控制項移出。
* 您無法釋放物件。

若要設定物件的處置：

* 將集區陣列封裝在可處置的物件中。
* 使用[HttpCoNtext. RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*)註冊集區物件。

`RegisterForDispose` 會負責呼叫目標物件上的 `Dispose`，使其只有在 HTTP 要求完成時才會釋放。

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

套用與非集區版本相同的負載，會導致下列圖表：

![先前的圖表](memory/_static/pooledarray.png)

主要差異是配置的位元組，因此產生的層代0回收量會較少。

## <a name="additional-resources"></a>其他資源

* [記憶體回收](/dotnet/standard/garbage-collection/)
* [瞭解使用並行視覺化的不同 GC 模式](https://blogs.msdn.microsoft.com/seteplia/2017/01/05/understanding-different-gc-modes-with-concurrency-visualizer/)
* [發現大型物件堆積](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [大型物件堆積](/dotnet/standard/garbage-collection/large-object-heap)
