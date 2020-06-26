---
title: 在 ASP.NET Core 中使用串流SignalR
author: bradygaster
description: 瞭解如何在用戶端與伺服器之間串流資料。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/streaming
ms.openlocfilehash: c7a3c7bb88230d84025bdf02deb98b51a2d1f92a
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85406169"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>在 ASP.NET Core 中使用串流SignalR

依[Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR 支援從用戶端到伺服器，以及從伺服器到用戶端的串流處理。 這適用于資料片段在一段時間內抵達的案例。 進行串流處理時，每個片段會在用戶端或伺服器可供使用時立即傳送到該伺服器，而不是等待所有資料都可供使用。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR 支援伺服器方法的資料流程傳回值。 這適用于資料片段在一段時間內抵達的案例。 當傳回值串流處理至用戶端時，每個片段會在其可用時立即傳送至用戶端，而不是等待所有資料都可供使用。

::: moniker-end

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="set-up-a-hub-for-streaming"></a>設定用於串流的中樞

::: moniker range=">= aspnetcore-3.0"

當中樞方法傳回 <xref:System.Collections.Generic.IAsyncEnumerable`1> 、、或時，它會自動變成串流中樞方法 <xref:System.Threading.Channels.ChannelReader%601> `Task<IAsyncEnumerable<T>>` `Task<ChannelReader<T>>` 。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

中樞方法會在傳回或時自動變成串流中樞方法 <xref:System.Threading.Channels.ChannelReader%601> `Task<ChannelReader<T>>` 。

::: moniker-end

### <a name="server-to-client-streaming"></a>伺服器對用戶端串流

::: moniker range=">= aspnetcore-3.0"

除了之外，串流中樞方法還可以傳回 `IAsyncEnumerable<T>` `ChannelReader<T>` 。 最簡單的方法 `IAsyncEnumerable<T>` 是將中樞方法設為非同步反覆運算器方法，如下列範例所示。 中樞非同步反覆運算器方法可以接受 `CancellationToken` 用戶端從串流取消訂閱時所觸發的參數。 非同步反覆運算器方法會避免通道常見的問題，例如不 `ChannelReader` 會提早傳回，也不會結束方法，而不會完成 <xref:System.Threading.Channels.ChannelWriter`1> 。

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

下列範例顯示使用通道將資料串流處理至用戶端的基本概念。 每當物件寫入時 <xref:System.Threading.Channels.ChannelWriter%601> ，物件就會立即傳送至用戶端。 結束 `ChannelWriter` 時，會完成以告知用戶端資料流程已關閉。

> [!NOTE]
> 將寫入 `ChannelWriter<T>` 背景執行緒，並 `ChannelReader` 儘快傳回。 在傳回之前，會封鎖其他中樞調用 `ChannelReader` 。
>
> 將邏輯包裝在中 `try ... catch` 。 完成 `Channel` 中 `catch` 和外部的， `catch` 以確定中樞方法叫用已正確完成。

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

伺服器對用戶端串流中樞方法可以接受 `CancellationToken` 用戶端從串流取消訂閱時所觸發的參數。 若用戶端在資料流程結尾之前中斷連線，請使用此權杖來停止伺服器作業並釋放任何資源。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端對伺服器串流

中樞方法會在接受一或多個類型或的物件時，自動成為用戶端對伺服器的串流中樞方法 <xref:System.Threading.Channels.ChannelReader%601> <xref:System.Collections.Generic.IAsyncEnumerable%601> 。 下列範例顯示讀取從用戶端傳送之串流資料的基本概念。 每當用戶端寫入時 <xref:System.Threading.Channels.ChannelWriter%601> ，就會將資料寫入 `ChannelReader` 中樞方法所讀取之伺服器上的。

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<xref:System.Collections.Generic.IAsyncEnumerable%601>以下是方法的版本。

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a>.NET 用戶端

### <a name="server-to-client-streaming"></a>伺服器對用戶端串流


::: moniker range=">= aspnetcore-3.0"

`StreamAsync`上的和 `StreamAsChannelAsync` 方法 `HubConnection` 是用來叫用伺服器對用戶端串流方法。 將中樞方法中定義的中樞方法名稱和引數傳遞至 `StreamAsync` 或 `StreamAsChannelAsync` 。 和上的泛型參數會 `StreamAsync<T>` `StreamAsChannelAsync<T>` 指定串流方法所傳回的物件類型。 型別 `IAsyncEnumerable<T>` 為或的物件 `ChannelReader<T>` 會從資料流程調用傳回，並代表用戶端上的資料流程。

傳回的 `StreamAsync` 範例 `IAsyncEnumerable<int>` ：

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

傳回的對應 `StreamAsChannelAsync` 範例 `ChannelReader<int>` ：

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

`StreamAsChannelAsync`上的方法 `HubConnection` 是用來叫用伺服器對用戶端串流方法。 將中樞方法中定義的中樞方法名稱和引數傳遞至 `StreamAsChannelAsync` 。 上的泛型參數會 `StreamAsChannelAsync<T>` 指定串流方法所傳回的物件類型。 `ChannelReader<T>`會從資料流程調用傳回，並代表用戶端上的資料流程。

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`StreamAsChannelAsync`上的方法 `HubConnection` 是用來叫用伺服器對用戶端串流方法。 將中樞方法中定義的中樞方法名稱和引數傳遞至 `StreamAsChannelAsync` 。 上的泛型參數會 `StreamAsChannelAsync<T>` 指定串流方法所傳回的物件類型。 `ChannelReader<T>`會從資料流程調用傳回，並代表用戶端上的資料流程。

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端對伺服器串流

有兩種方式可從 .NET 用戶端叫用用戶端到伺服器的串流中樞方法。 `IAsyncEnumerable<T>` `ChannelReader` `SendAsync` 視所叫用 `InvokeAsync` `StreamAsChannelAsync` 的中樞方法而定，您可以將或當做引數傳入、或。

每次將資料寫入 `IAsyncEnumerable` 或 `ChannelWriter` 物件時，伺服器上的中樞方法都會收到新的專案，其中包含來自用戶端的資料。

如果使用 `IAsyncEnumerable` 物件，則資料流程會在傳回資料流程專案的方法結束後結束。

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

或者，如果您正在使用 `ChannelWriter` ，您可以使用來完成通道 `channel.Writer.Complete()` ：

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>JavaScript 用戶端

### <a name="server-to-client-streaming"></a>伺服器對用戶端串流

JavaScript 用戶端會使用在中樞上呼叫伺服器對用戶端串流方法 `connection.stream` 。 `stream`方法接受兩個引數：

* 中樞方法的名稱。 在下列範例中，中樞方法名稱是 `Counter` 。
* 中樞方法中定義的引數。 在下列範例中，引數是要接收的資料流程專案數，以及資料流程專案之間的延遲計數。

`connection.stream`傳回 `IStreamResult` ，其中包含 `subscribe` 方法。 將傳遞 `IStreamSubscriber` 至， `subscribe` 並設定 `next` 、 `error` 和 `complete` 回呼，以接收來自調用的通知 `stream` 。

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

若要從用戶端結束資料流程，請 `dispose` 在 `ISubscription` 從方法傳回的上呼叫方法 `subscribe` 。 `CancellationToken`如果您提供中樞方法的參數，呼叫這個方法會導致取消。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

若要從用戶端結束資料流程，請 `dispose` 在 `ISubscription` 從方法傳回的上呼叫方法 `subscribe` 。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端對伺服器串流

JavaScript 用戶端會呼叫中樞上的用戶端對伺服器串流方法，方法是將當做 `Subject` 引數傳遞至 `send` 、 `invoke` 或 `stream` ，視所叫用的中樞方法而定。 `Subject`是看起來像的類別 `Subject` 。 例如，在 RxJS 中，您可以使用該程式庫中的[Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)類別。

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

使用專案呼叫會將 `subject.next(item)` 專案寫入資料流程，而中樞方法會在伺服器上接收專案。

若要結束資料流程，請呼叫 `subject.complete()` 。

## <a name="java-client"></a>Java 用戶端

### <a name="server-to-client-streaming"></a>伺服器對用戶端串流

SignalRJAVA 用戶端會使用 `stream` 方法來叫用串流方法。 `stream`接受三個或多個引數：

* 資料流程專案的預期型別。
* 中樞方法的名稱。
* 中樞方法中定義的引數。

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

上的方法會傳回 `stream` `HubConnection` 資料流程專案類型的可觀察。 可觀察類型的 `subscribe` 方法為 `onNext` ， `onError` 並 `onCompleted` 定義處理常式。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
