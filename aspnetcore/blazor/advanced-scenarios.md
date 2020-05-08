---
title: ASP.NET Core Blazor advanced 案例
author: guardrex
description: 深入瞭解中的 advanced Blazor案例，包括如何將手動 RenderTreeBuilder 邏輯併入應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: b47e7b1d7ff148bb5a8d299d3d2089999f017863
ms.sourcegitcommit: 84b46594f57608f6ac4f0570172c7051df507520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82967333"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a>ASP.NET Core Blazor 的先進案例

By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)

## <a name="blazor-server-circuit-handler"></a>Blazor 伺服器線路處理常式

Blazor 伺服器可讓程式碼定義*電路處理常式*，以允許對使用者線路狀態的變更執行程式碼。 線路處理常式是透過衍生自`CircuitHandler`並在應用程式的服務容器中註冊類別來執行。 下列的線路處理常式範例會追蹤開啟的 SignalR 連接：

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => circuits.Count;
}
```

線路處理常式是使用 DI 註冊。 範圍實例會針對每個線路實例而建立。 使用上述`TrackingCircuitHandler`範例中的時，會建立單一服務，因為必須追蹤所有線路的狀態：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

如果自訂電路處理常式的方法擲回未處理的例外狀況，則例外狀況對 Blazor 伺服器線路而言是嚴重的。 若要容忍處理常式程式碼或呼叫方法中的例外狀況，請使用錯誤處理和記錄，將程式碼包裝在一個或多個[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。

當線路因使用者已中斷連線而結束，而架構正在清除線路狀態時，架構會處置線路的 DI 範圍。 處置範圍會處置任何執行<xref:System.IDisposable?displayProperty=fullName>的線路範圍 DI 服務。 如果任何 DI 服務在處置期間擲回未處理的例外狀況，則架構會記錄例外狀況。

## <a name="manual-rendertreebuilder-logic"></a>手動 RenderTreeBuilder 邏輯

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`提供操作元件和專案的方法，包括在 c # 程式碼中手動建立元件。

> [!NOTE]
> 使用來`RenderTreeBuilder`建立元件是一個先進的案例。 格式不正確的元件（例如，未封閉的標記標記）可能會導致未定義的行為。

請考慮下列`PetDetails`元件，它可以手動內建在另一個元件中：

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

在下列範例中， `CreateComponent`方法中的迴圈會產生三`PetDetails`個元件。 呼叫`RenderTreeBuilder`方法來建立元件（`OpenComponent`和`AddAttribute`）時，序號是源程式碼號。 Blazor 差異演算法依賴對應于不同程式程式碼的序號，而不是相異的呼叫調用。 使用`RenderTreeBuilder`方法建立元件時，硬式編碼序號的引數。 **使用計算或計數器來產生序號可能會導致效能不佳。** 如需詳細資訊，請參閱[序號與程式程式碼號，而不是執行順序](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)一節。

`BuiltContent`成分

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
> 中`Microsoft.AspNetCore.Components.RenderTree`的類型允許處理轉譯作業的*結果*。 這些是 Blazor framework 執行的內部詳細資料。 這些類型應該被視為不*穩定*，未來的版本可能會變更。

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>序號與程式程式碼號相關，而不是執行順序

Razor 元件檔案（*razor*）一律會進行編譯。 編譯是在解讀程式碼方面的潛在優勢，因為編譯步驟可以用來插入資訊，以在執行時間改善應用程式效能。

這些改良功能的重要範例包括*序號*。 序號會向運行時程表示輸出來自哪些不同和已排序的程式程式碼。 執行時間會使用這項資訊，以線性時間產生有效率的樹狀差異，這比一般樹狀結構的差異演算法通常還能快得多。

請考慮下列 Razor 元件（*razor*）檔案：

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

上述程式碼會編譯成如下所示的內容：

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

當程式碼第一次執行時，如果`someFlag`是`true`，則產生器會接收：

| 順序 | 類型      | 資料   |
| :------: | --------- | :----: |
| 0        | Text node | First  |
| 1        | Text node | Second |

想像一下`someFlag` ， `false`會變成，然後再次呈現標記。 這次，產生器會接收：

| 順序 | 類型       | 資料   |
| :------: | ---------- | :----: |
| 1        | Text node  | Second |

當執行時間執行 diff 時，會看到順序`0`中的專案已移除，因此它會產生下列簡單的*編輯腳本*：

* 移除第一個文位元組點。

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a>以程式設計方式產生序號的問題

想像一下，您會改為撰寫下列轉譯樹產生器邏輯：

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

現在，第一個輸出是：

| 順序 | 類型      | 資料   |
| :------: | --------- | :----: |
| 0        | Text node | First  |
| 1        | Text node | Second |

此結果與先前的案例相同，因此不會有負面問題存在。 `someFlag``false`在第二個轉譯上，輸出為：

| 順序 | 類型      | 資料   |
| :------: | --------- | ------ |
| 0        | Text node | Second |

這次，diff 演算法發現發生了*兩*項變更，而演算法會產生下列編輯腳本：

* 將第一個文位元組點的值變更為`Second`。
* 移除第二個文位元組點。

產生序號已遺失關於`if/else`分支和迴圈在原始程式碼中出現位置的所有實用資訊。 這會導致差異**兩倍，但前提**是之前。

這是一個簡單的範例。 在具有複雜和深層嵌套結構的更真實案例中，尤其是使用迴圈時，效能成本通常較高。 Diff 演算法不會立即識別已插入或移除的迴圈區塊或分支，而是必須將深度遞迴到轉譯樹狀結構中。 這通常需要建立更長的編輯腳本，因為差異演算法會 misinformed 舊的和新結構如何彼此相關。

### <a name="guidance-and-conclusions"></a>指引和結論

* 如果序號是動態產生的，應用程式效能會受到影響。
* 架構無法在執行時間自動建立自己的序號，因為必要的資訊不存在，除非是在編譯時期加以捕捉。
* 請勿撰寫長時間區塊的手動執行`RenderTreeBuilder`邏輯。 偏好*razor*檔案，並允許編譯器處理序號。 如果您無法`RenderTreeBuilder`避免手動邏輯，請將長塊的程式碼分割成較小的`OpenRegion` / `CloseRegion`片段，以呼叫。 每個區域都有自己的序號個別空間，因此您可以在每個區域內從零（或任何其他任一數字）重新開機。
* 如果序號已硬式編碼，則 diff 演算法只會要求序號增加值。 起始值和間距無關。 一個合法的選項是使用程式程式碼號做為序號，或從零開始，並以一個或數百個（或任何慣用的間隔）來增加。 
* Blazor會使用序號，而其他樹狀結構比較的 UI 架構則不會使用它們。 當使用序號時，比較速度會更快， Blazor而且具有可自動處理序號的編譯步驟，讓開發人員撰寫*razor*檔案。

## <a name="perform-large-data-transfers-in-blazor-server-apps"></a>在伺服器應用程式中Blazor執行大型資料傳輸

在某些情況下，必須在 JavaScript 和Blazor之間傳輸大量資料。 通常會在下列情況進行大型資料傳輸：

* 瀏覽器檔案系統 Api 可用來上傳或下載檔案。
* 需要具有協力廠商程式庫的互通性。

在Blazor伺服器中，有一項限制是為了避免傳遞可能會導致效能問題的單一大型訊息。

開發在 JavaScript 和Blazor之間傳輸資料的程式碼時，請考慮下列指導方針：

* 將資料分割成較小的片段，並依序傳送資料區段，直到伺服器收到所有資料為止。
* 不要以 JavaScript 和 c # 程式碼配置大型物件。
* 傳送或接收資料時，不要長時間封鎖主要 UI 執行緒。
* 釋放處理常式完成或取消時所耗用的任何記憶體。
* 基於安全性目的，強制執行下列額外的需求：
  * 宣告可傳遞的檔案或資料大小上限。
  * 宣告從用戶端到伺服器的最小上傳速率。
* 伺服器收到資料之後，資料可以是：
  * 暫時儲存在記憶體緩衝區中，直到收集所有區段為止。
  * 立即使用。 例如，資料可以立即儲存在資料庫中，或在每個區段收到時寫入磁片。

下列檔案上傳者類別會處理用戶端的 JS interop。 上載者類別會使用 JS interop 來執行下列動作：

* 輪詢用戶端以傳送資料區段。
* 如果輪詢超時，則中止交易。

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime jsRuntime;
    private readonly int segmentSize = 6144;
    private readonly int maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> thisReference;
    private List<IMemoryOwner<byte>> uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        this.jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          uploadedSegments.Add(current);
        }

        var segments = uploadedSegments;
        uploadedSegments = null;

        return new SegmentedStream(segments, segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (uploadedSegments != null)
        {
            foreach (var segment in uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

在上述範例中：

* `maxBase64SegmentSize`會設定為`8192`，這是從`maxBase64SegmentSize = segmentSize * 4 / 3`計算。
* 低層級的 .NET Core 記憶體管理 Api 是用來將伺服器上的記憶體區段儲存`uploadedSegments`在中。
* `ReceiveFile`方法是用來處理透過 JS interop 的上傳：
  * 檔案大小是以位元組為單位，透過 JS interop `jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`與來決定。
  * 要接收的區段數目會計算並儲存在中`numberOfSegments`。
  * 這些區段會在透過 JS `for` interop 與`jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`的迴圈中要求。 在解碼之前，所有區段（但最後一個）必須是8192個位元組。 系統會強制用戶端以有效率的方式傳送資料。
  * 針對每個收到的區段，會先執行檢查<xref:System.Convert.TryFromBase64String%2A>，然後再使用進行解碼。
  * 當上傳完成之後，會以新<xref:System.IO.Stream>的（`SegmentedStream`）傳回資料的資料流程。

分段資料流程類別會將區段清單公開為 readonly 不可搜尋<xref:System.IO.Stream>：

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> sequence;
    private long currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(currentPosition + count < sequence.Length ? 
            count : sequence.Length - currentPosition);
        var data = sequence.Slice(currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

下列程式碼會實行 JavaScript 函式來接收資料：

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```
