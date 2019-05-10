---
title: 使用資料流在 ASP.NET Core SignalR
author: bradygaster
description: 了解如何在用戶端與伺服器之間的資料串流。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/12/2019
uid: signalr/streaming
ms.openlocfilehash: 8f39fdfa45766b5bbec572970f009abefefdc419
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897195"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>使用資料流在 ASP.NET Core SignalR

由[brennan Conroy](https://github.com/BrennanConroy)提供

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR 支援串流用戶端至伺服器，並從伺服器到用戶端。 這是適用於案例經過一段時間抵達的資料片段的位置。 串流處理時，每個片段會傳送至用戶端或伺服器，它會變成可用，而不是等待所有的資料變成可用。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR 支援資料流的伺服器方法的傳回值。 這是適用於案例經過一段時間抵達的資料片段的位置。 時傳回的值串流處理到用戶端，每個片段會傳送至用戶端，它會變成可用，而不是等待變成可用的所有資料。

::: moniker-end

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>設定串流處理的中樞

::: moniker range=">= aspnetcore-3.0"

中樞的方法會自動變成資料流處理的中樞方法傳回時<xref:System.Threading.Channels.ChannelReader%601>， `IAsyncEnumerable<T>`， `Task<ChannelReader<T>>`，或`Task<IAsyncEnumerable<T>>`。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

中樞的方法會自動變成資料流處理的中樞方法傳回時<xref:System.Threading.Channels.ChannelReader%601>或`Task<ChannelReader<T>>`。

::: moniker-end

### <a name="server-to-client-streaming"></a>伺服器到用戶端串流

::: moniker range=">= aspnetcore-3.0"

串流處理中樞方法可以傳回`IAsyncEnumerable<T>`除了`ChannelReader<T>`。 最簡單的方式傳回`IAsyncEnumerable<T>`是透過讓中樞方法的非同步迭代器方法，如下列範例所示。 中樞非同步迭代器方法可以接受`CancellationToken`時從資料流的用戶端取消訂閱所觸發的參數。 非同步迭代器方法避免常見的問題，通道，例如未傳回`ChannelReader`及早或結束方法，而不會完成<xref:System.Threading.Channels.ChannelWriter`1>。

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

下列範例顯示資料串流到用戶端會使用通道的基本的概念。 每當物件寫入<xref:System.Threading.Channels.ChannelWriter%601>，該物件會立即傳送至用戶端。 在結束時，`ChannelWriter`完成告知用戶端的資料流已關閉。

> [!NOTE]
> 寫入`ChannelWriter<T>`在背景執行緒，然後傳回`ChannelReader`儘速。 其他中樞叫用會封鎖直到`ChannelReader`會傳回。
>
> 中的邏輯包裝`try ... catch`。 完成`Channel`中`catch`和外部`catch`確定中樞方法叫用會正確地完成。

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

伺服器到用戶端資料流的中樞方法可以接受`CancellationToken`時從資料流的用戶端取消訂閱所觸發的參數。 若要停止伺服器作業，並釋放任何資源，如果用戶端中斷連線的資料流結束前使用此權杖。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端-伺服器串流處理

中樞的方法會自動變成用戶端-伺服器串流處理中樞方法，當它接受一或多個<xref:System.Threading.Channels.ChannelReader`1>s。 下列範例示範讀取從用戶端傳送的資料流處理資料的基本概念。 每當用戶端會寫入<xref:System.Threading.Channels.ChannelWriter`1>，將資料寫入到`ChannelReader`中樞方法讀取伺服器上。

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a>.NET 用戶端

### <a name="server-to-client-streaming"></a>伺服器到用戶端串流

`StreamAsChannelAsync`方法`HubConnection`用來叫用伺服器到用戶端的串流處理方式。 將中樞方法的名稱和定義中的中樞方法的引數傳遞`StreamAsChannelAsync`。 泛型參數上的`StreamAsChannelAsync<T>`指定的資料流的方法所傳回的物件類型。 A`ChannelReader<T>`傳回的資料流的引動過程，和代表用戶端上的資料流。

::: moniker range=">= aspnetcore-2.2"

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

### <a name="client-to-server-streaming"></a>用戶端-伺服器串流處理

若要叫用用戶端-伺服器串流處理中樞方法從.NET 用戶端，建立`Channel`，並傳遞`ChannelReader`做為引數`SendAsync`， `InvokeAsync`，或`StreamAsChannelAsync`，取決於叫用中樞方法。

每當資料寫入`ChannelWriter`，在伺服器上的中樞方法從用戶端接收資料的新項目。

若要結束資料流，完成與通道`channel.Writer.Complete()`。

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>JavaScript 用戶端

### <a name="server-to-client-streaming"></a>伺服器到用戶端串流

JavaScript 用戶端呼叫伺服器到用戶端串流上的方法與中樞`connection.stream`。 `stream` 方法接受兩個引數：

* 中樞方法的名稱。 下列範例中，在中樞方法名稱是`Counter`。
* 中樞的方法中定義的引數。 下列範例中，引數都要接收的資料流項目和資料流項目之間的延遲數目的計數。

`connection.stream` 會傳回`IStreamResult`，其中包含`subscribe`方法。 傳遞`IStreamSubscriber`要`subscribe`並設定`next`， `error`，並`complete`回呼以接收來自通知`stream`引動過程。

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。 呼叫這個方法會導致取消`CancellationToken`中樞方法參數，如果您提供的其中一個。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>用戶端-伺服器串流處理

JavaScript 用戶端中樞上呼叫用戶端-伺服器串流處理方法，藉由傳入`Subject`做為引數`send`， `invoke`，或`stream`，取決於叫用中樞方法。 `Subject`是一個類別，看起來像`Subject`。 比方說在 RxJS，您可以使用[主旨](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)從該程式庫的類別。

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

呼叫`subject.next(item)`與項目會寫入資料流中的項目和中樞方法接收伺服器上的項目。

若要結束資料流，呼叫`subject.complete()`。

## <a name="java-client"></a>Java 用戶端

### <a name="server-to-client-streaming"></a>伺服器到用戶端串流

SignalR Java 用戶端會使用`stream`方法來叫用資料流的方法。 `stream` 接受三個或多個引數：

* 資料流項目的預期的類型。
* 中樞方法的名稱。
* 中樞的方法中定義的引數。

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

`stream`方法`HubConnection`傳回資料流項目類型的可預見值。 可觀察的型別`subscribe`方法正是`onNext`，`onError`和`onCompleted`定義處理常式。

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
