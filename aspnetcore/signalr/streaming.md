---
title: 在 ASP.NET Core 中使用串流 SignalR
author: bradygaster
description: 瞭解如何在用戶端與伺服器之間串流資料。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/streaming
ms.openlocfilehash: 21dd8180fe168f81ed68b01f02b81a6264d6e5a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667724"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a>在 ASP.NET Core 中使用串流 SignalR

依[Brennan Conroy](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR 支援從用戶端到伺服器，以及從伺服器到用戶端的串流處理。 這適用于資料片段在一段時間內抵達的案例。 進行串流處理時，每個片段會在用戶端或伺服器可供使用時立即傳送到該伺服器，而不是等待所有資料都可供使用。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR 支援伺服器方法的資料流程傳回值。 這適用于資料片段在一段時間內抵達的案例。 當傳回值串流處理至用戶端時，每個片段會在其可用時立即傳送至用戶端，而不是等待所有資料都可供使用。

::: moniker-end

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>設定用於串流的中樞

::: moniker range=">= aspnetcore-3.0"

中樞方法會在傳回 <xref:System.Collections.Generic.IAsyncEnumerable`1>、<xref:System.Threading.Channels.ChannelReader%601>、`Task<IAsyncEnumerable<T>>`或 `Task<ChannelReader<T>>`時，自動變成串流中樞方法。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

中樞方法會在傳回 <xref:System.Threading.Channels.ChannelReader%601> 或 `Task<ChannelReader<T>>`時，自動變成串流中樞方法。

::: moniker-end

### <a name="server-to-client-streaming"></a>伺服器對用戶端串流

::: moniker range=">= aspnetcore-3.0"

除了 `ChannelReader<T>`之外，串流中樞方法還可以傳回 `IAsyncEnumerable<T>`。 傳回 `IAsyncEnumerable<T>` 最簡單的方式，就是將中樞方法設為非同步反覆運算器方法，如下列範例所示。 中樞非同步反覆運算器方法可以接受用戶端從串流取消訂閱時所觸發的 `CancellationToken` 參數。 非同步反覆運算器方法會避免通道常見的問題，例如，不會提早傳回 `ChannelReader`，也不會結束方法，而不會完成 <xref:System.Threading.Channels.ChannelWriter`1>。

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

下列範例顯示使用通道將資料串流處理至用戶端的基本概念。 每當物件寫入 <xref:System.Threading.Channels.ChannelWriter%601>時，物件就會立即傳送至用戶端。 最後，`ChannelWriter` 完成，告知用戶端資料流程已關閉。

> [!NOTE]
> 在背景執行緒上寫入 `ChannelWriter<T>`，並儘快傳回 `ChannelReader`。 在傳回 `ChannelReader` 之前，會封鎖其他中樞調用。
>
> 將邏輯包裝在 `try ... catch`中。 完成 `catch` 和 `catch` 外部的 `Channel`，以確定中樞方法叫用已正確完成。

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

伺服器對用戶端串流中樞方法可以接受用戶端從串流取消訂閱時所觸發的 `CancellationToken` 參數。 若用戶端在資料流程結尾之前中斷連線，請使用此權杖來停止伺服器作業並釋放任何資源。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端對伺服器串流

中樞方法會在接受 <xref:System.Threading.Channels.ChannelReader%601> 或 <xref:System.Collections.Generic.IAsyncEnumerable%601>類型的一或多個物件時，自動成為用戶端對伺服器的串流中樞方法。 下列範例顯示讀取從用戶端傳送之串流資料的基本概念。 每當用戶端寫入 <xref:System.Threading.Channels.ChannelWriter%601>時，資料就會寫入中樞方法所讀取之伺服器上的 `ChannelReader` 中。

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

以下是方法的 <xref:System.Collections.Generic.IAsyncEnumerable%601> 版本。

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

`HubConnection` 上的 `StreamAsync` 和 `StreamAsChannelAsync` 方法是用來叫用伺服器對用戶端串流方法。 將中樞方法中定義的中樞方法名稱和引數傳遞給 `StreamAsync` 或 `StreamAsChannelAsync`。 `StreamAsync<T>` 和 `StreamAsChannelAsync<T>` 上的泛型參數會指定串流方法所傳回的物件類型。 `IAsyncEnumerable<T>` 或 `ChannelReader<T>` 類型的物件會從資料流程調用傳回，並代表用戶端上的資料流程。

傳回 `IAsyncEnumerable<int>`的 `StreamAsync` 範例：

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

傳回 `ChannelReader<int>`的對應 `StreamAsChannelAsync` 範例：

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

`HubConnection` 上的 `StreamAsChannelAsync` 方法是用來叫用伺服器對用戶端串流方法。 將中樞方法中定義的中樞方法名稱和引數傳遞給 `StreamAsChannelAsync`。 `StreamAsChannelAsync<T>` 上的泛型參數會指定串流方法所傳回的物件類型。 會從資料流程調用傳回 `ChannelReader<T>`，並代表用戶端上的資料流程。

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

`HubConnection` 上的 `StreamAsChannelAsync` 方法是用來叫用伺服器對用戶端串流方法。 將中樞方法中定義的中樞方法名稱和引數傳遞給 `StreamAsChannelAsync`。 `StreamAsChannelAsync<T>` 上的泛型參數會指定串流方法所傳回的物件類型。 會從資料流程調用傳回 `ChannelReader<T>`，並代表用戶端上的資料流程。

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

有兩種方式可從 .NET 用戶端叫用用戶端到伺服器的串流中樞方法。 視所叫用的中樞方法而定，您可以將 `IAsyncEnumerable<T>` 或 `ChannelReader` 當做引數傳入 `SendAsync`、`InvokeAsync`或 `StreamAsChannelAsync`。

每當資料寫入 `IAsyncEnumerable` 或 `ChannelWriter` 物件時，伺服器上的中樞方法就會使用來自用戶端的資料接收新的專案。

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

或者，如果您要使用 `ChannelWriter`，您可以使用 `channel.Writer.Complete()`完成通道：

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

JavaScript 用戶端會使用 `connection.stream`，在中樞上呼叫伺服器對用戶端串流方法。 `stream` 方法接受兩個引數：

* 中樞方法的名稱。 在下列範例中，中樞方法名稱是 `Counter`。
* 中樞方法中定義的引數。 在下列範例中，引數是要接收的資料流程專案數，以及資料流程專案之間的延遲計數。

`connection.stream` 會傳回 `IStreamResult`，其中包含 `subscribe` 方法。 將 `IStreamSubscriber` 傳遞至 `subscribe`，並設定 `next`、`error`和 `complete` 回呼，以接收來自 `stream` 調用的通知。

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

若要從用戶端結束資料流程，請在從 `subscribe` 方法傳回的 `ISubscription` 上呼叫 `dispose` 方法。 如果您提供中樞方法的 `CancellationToken` 參數，則呼叫這個方法會導致取消。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

若要從用戶端結束資料流程，請在從 `subscribe` 方法傳回的 `ISubscription` 上呼叫 `dispose` 方法。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端對伺服器串流

JavaScript 用戶端會根據所叫用的中樞方法，將 `Subject` 當做引數傳入 `send`、`invoke`或 `stream`，以呼叫中樞上的用戶端對伺服器串流方法。 `Subject` 是一個看起來像 `Subject`的類別。 例如，在 RxJS 中，您可以使用該程式庫中的[Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)類別。

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

使用專案呼叫 `subject.next(item)` 會將專案寫入資料流程，而中樞方法會在伺服器上接收專案。

若要結束資料流程，請呼叫 `subject.complete()`。

## <a name="java-client"></a>Java 用戶端

### <a name="server-to-client-streaming"></a>伺服器對用戶端串流

SignalR JAVA 用戶端會使用 `stream` 方法來叫用串流方法。 `stream` 接受三個或多個引數：

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

`HubConnection` 上的 `stream` 方法會傳回資料流程專案類型的可觀察。 可觀察型別的 `subscribe` 方法是定義 `onNext`、`onError` 和 `onCompleted` 處理常式的位置。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
