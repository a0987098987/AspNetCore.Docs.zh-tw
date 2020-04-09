---
title: ASP.NET核心Blazor進階機制
author: guardrex
description: 瞭解中的Blazor高級方案,包括如何將手動 RenderTreeBuilder 邏輯合併到應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5edbbe36e8389bac0335594b1e4331aee1c02867
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78659450"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a>ASP.NET核心布拉佐爾高級方案

由[盧克·萊瑟姆](https://github.com/guardrex)和[丹尼爾·羅斯](https://github.com/danroth27)

## <a name="blazor-server-circuit-handler"></a>布拉佐伺服器電路處理程式

Blazor Server 允許代碼定義*電路處理程式*,它允許在更改使用者電路狀態時運行代碼。 電路處理程式是通過從`CircuitHandler`應用的服務容器中派生和註冊類實現的。 以下電路處理程式範例追蹤開啟的 SignalR 連線:

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

電路處理程式使用 DI 註冊。 根據電路的實例創建作用域實例。 `TrackingCircuitHandler`使用前面的範例中,將建立單例服務,因為必須追蹤所有電路的狀態:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

如果自定義電路處理程式的方法引發未處理的異常,則異常對 Blazor Server 電路是致命的。 要容忍處理程式代碼或調用方法中的異常,請用錯誤處理和日誌記錄將代碼包裝在一個或多個[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中。

當電路因用戶斷開連接而結束,並且框架正在清理電路狀態時,框架將釋放電路的 DI 範圍。 處置範圍將釋放實現<xref:System.IDisposable?displayProperty=fullName>的任何電路範圍的 DI 服務。 如果任何 DI 服務在處置期間引發未處理的異常,則框架將記錄異常。

## <a name="manual-rendertreebuilder-logic"></a>手動成成樹建構器邏輯

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder`提供了操作元件和元素的方法,包括在 C# 代碼中手動構建元件。

> [!NOTE]
> 使用`RenderTreeBuilder`創建元件是進階方案。 格式錯誤的元件(例如,未關閉的標記標記)可能會導致未定義的行為。

請考慮以下`PetDetails`元件,這些元件可以手動建構到另一個元件中:

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

在下面的範例中,`CreateComponent`方法中的循環生成三`PetDetails`個 元件。 呼叫`RenderTreeBuilder`建立元件(`OpenComponent``AddAttribute`與 ) 的方法來建立 序列號時是原始程式碼行號。 Blazor 差異演演演算法依賴於對應於不同代碼行的序列號,而不是不同的調用調用。 使用`RenderTreeBuilder`方法建立元件時,對序列號的參數進行硬編碼。 **使用計算或計數器生成序列號可能會導致性能不佳。** 有關詳細資訊,請參閱[序列號與代碼行號相關,而不是執行訂單](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order)部分。

`BuiltContent`元件:

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
> 中`Microsoft.AspNetCore.Components.RenderTree`的類型允許處理呈現操作*的結果*。 這些是布拉佐爾框架實施的內部細節。 這些類型應視為*不穩定*,並可能在未來版本中更改。

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>序列號與代碼行號相關,而不是執行順序

剃刀元件檔 (*.razor*) 總是被編譯. 與解釋代碼時,編譯是一個潛在的優勢,因為編譯步驟可用於注入提高運行時應用性能的資訊。

這些改進的一個關鍵範例的序列*號*。 序列號指示運行時輸出來自哪些代碼行不同且順序明確。 運行時使用此資訊在線性時間生成有效的樹差異,這比常規樹差異演演演算法通常可能的速度要快得多。

請考慮以下 Razor 元件 (*.razor*) 檔案:

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

前面的代碼編譯為類似以下內容:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

當代碼首次執行時,如果`someFlag`是`true`,生成器將接收:

| 順序 | 類型      | 資料   |
| :------: | --------- | :----: |
| 0        | Text node | First  |
| 1        | Text node | Second |

想像一`someFlag``false`下 ,這將成為 ,標記將再次呈現。 這一次,產生器收到:

| 順序 | 類型       | 資料   |
| :------: | ---------- | :----: |
| 1        | Text node  | Second |

當執行時執行差異時,它會看到序列`0`中的項目被刪除,因此它產生以下瑣碎*的編輯文稿*:

* 刪除第一個文字節點。

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a>以程式設計程式產生序列號的問題

相反,假設您編寫了以下渲染樹產生器邏輯:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

現在,第一個輸出是:

| 順序 | 類型      | 資料   |
| :------: | --------- | :----: |
| 0        | Text node | First  |
| 1        | Text node | Second |

此結果與之前的情況相同,因此不存在負面問題。 `someFlag`是`false`在第二個渲染上,輸出是:

| 順序 | 類型      | 資料   |
| :------: | --------- | ------ |
| 0        | Text node | Second |

這一次,diff 演演算法看到*發生了兩*個更改,並且該演演演算法生成以下編輯腳本:

* 將第一個文字節點的值變更為`Second`。
* 刪除第二個文字節點。

生成序列號已丟失`if/else`有關 原始代碼中分支和迴圈存在位置的所有有用資訊。 這會導致差異的時間比以前**長一倍**。

這是一個微不足道的例子。 在結構複雜且深度嵌套的更現實的情況下,尤其是迴圈中,性能成本通常較高。 diff 演演演算法不必立即識別已插入或刪除的循環塊或分支,而是必須深入地重新詛咒到渲染樹中。 這通常會導致必須構建較長的編輯腳本,因為 diff 演演演算法對新舊結構之間的關係有誤。

### <a name="guidance-and-conclusions"></a>指導和結論

* 如果動態生成序列號,則應用性能會受到影響。
* 框架無法在運行時自動創建自己的序列號,因為除非在編譯時捕獲必要的信息,否則不存在必要的資訊。
* 不要編寫手動實現`RenderTreeBuilder`的邏輯的長塊。 首選 *.razor*檔,並允許編譯器處理序列號。 如果`RenderTreeBuilder`無法避免手動邏輯,請將長代碼塊拆分為在調用`OpenRegion`/`CloseRegion`中 包裝的較小部分。 每個區域都有其各自的序列號空間,因此您可以在每個區域內從零(或任何其他任意數位)重新啟動。
* 如果對序列號進行硬編碼,差異演演演算法只需要該序列號的值增加。 初始值和間隙無關緊要。 一個合法的選項是使用代碼行號作為序列號,或從零開始,並增加 1 或數百(或任何首選間隔)。 
* Blazor使用序列號,而其他樹差異 UI 框架不使用它們。 使用序列號時,差異速度要快得多,並且Blazor具有編譯步驟的優點,該步驟可自動處理序列號,以便開發人員創作 *.razor*檔。

## <a name="perform-large-data-transfers-in-opno-locblazor-server-apps"></a>在伺服器應用中Blazor執行大型資料傳輸

在某些情況下,必須在 JAVAScriptBlazor和 之間傳輸大量數據。 通常,在以下時間進行大型數據傳輸:

* 瀏覽器檔案系統 API 用於上載或下載檔案。
* 需要與第三方庫進行互操作。

在BlazorServer 中,有限制,以防止傳遞可能導致性能問題的單個大型消息。

在開發在 JavaScriptBlazor和 之間傳輸資料的代碼時,請考慮以下指南:

* 將數據切成更小的部分,並按順序發送數據段,直到伺服器接收所有數據。
* 不要在 JavaScript 和 C# 代碼中分配大型物件。
* 在發送或接收數據時,不要長時間阻止主 UI 線程。
* 釋放進程完成或取消時消耗的任何記憶體。
* 出於安全目的,強制實施以下附加要求:
  * 聲明可以傳遞的最大檔或數據大小。
  * 聲明從用戶端到伺服器的最低上載速率。
* 伺服器接收資料後,資料可以是:
  * 暫時存儲在記憶體緩衝區中,直到收集所有段。
  * 立即使用。 例如,數據可以立即存儲在資料庫中,或在接收每個段時寫入磁碟。

以下檔上載器類處理與用戶端的 JS 互通。 上傳器類別使用 JS 互通來:

* 輪詢客戶端以發送數據段。
* 如果輪詢超時,則中止事務。

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime _jsRuntime;
    private readonly int _segmentSize = 6144;
    private readonly int _maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> _thisReference;
    private List<IMemoryOwner<byte>> _uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        _jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await _jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)_segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await _jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < _maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(_segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, _segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          _uploadedSegments.Add(current);
        }

        var segments = _uploadedSegments;
        _uploadedSegments = null;

        return new SegmentedStream(segments, _segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (_uploadedSegments != null)
        {
            foreach (var segment in _uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

在上述範例中：

* `_maxBase64SegmentSize`設定為`8192`,`_maxBase64SegmentSize = _segmentSize * 4 / 3`從計算。
* 低級 .NET 核心記憶體管理 API`_uploadedSegments`用於在中儲存伺服器上的記憶體段。
* 方法`ReceiveFile`用於透過 JS 互通處理上載:
  * 檔大小透過 JS`_jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`與的聯名字節確定。
  * 要接收的段數計算並儲存在中`numberOfSegments`。
  * 段在`for`使用 JS 的循環`_jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`中要求使用 。 解碼前,除最後一個段外的所有段必須為 8,192 位元組。 用戶端被迫以有效的方式發送數據。
  * 對於接收的每個段,在使用<xref:System.Convert.TryFromBase64String*>解碼之前執行檢查。
  * 上載完成後,具有數據的流將作為新的<xref:System.IO.Stream>`SegmentedStream`( ) 傳回。

分段流類將段清單公開為唯讀不可查找: <xref:System.IO.Stream>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> _sequence;
    private long _currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            _sequence = new ReadOnlySequence<byte>(
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

        _sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(_currentPosition + count < _sequence.Length ? 
            count : _sequence.Length - _currentPosition);
        var data = _sequence.Slice(_currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        _currentPosition += bytesToWrite;

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

以下代碼實現 JavaScript 函數來接收資料:

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
