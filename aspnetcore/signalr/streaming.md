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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="e695d-103">使用資料流在 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e695d-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="e695d-104">由[brennan Conroy](https://github.com/BrennanConroy)提供</span><span class="sxs-lookup"><span data-stu-id="e695d-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e695d-105">ASP.NET Core SignalR 支援串流用戶端至伺服器，並從伺服器到用戶端。</span><span class="sxs-lookup"><span data-stu-id="e695d-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="e695d-106">這是適用於案例經過一段時間抵達的資料片段的位置。</span><span class="sxs-lookup"><span data-stu-id="e695d-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="e695d-107">串流處理時，每個片段會傳送至用戶端或伺服器，它會變成可用，而不是等待所有的資料變成可用。</span><span class="sxs-lookup"><span data-stu-id="e695d-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e695d-108">ASP.NET Core SignalR 支援資料流的伺服器方法的傳回值。</span><span class="sxs-lookup"><span data-stu-id="e695d-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="e695d-109">這是適用於案例經過一段時間抵達的資料片段的位置。</span><span class="sxs-lookup"><span data-stu-id="e695d-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="e695d-110">時傳回的值串流處理到用戶端，每個片段會傳送至用戶端，它會變成可用，而不是等待變成可用的所有資料。</span><span class="sxs-lookup"><span data-stu-id="e695d-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="e695d-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e695d-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="e695d-112">設定串流處理的中樞</span><span class="sxs-lookup"><span data-stu-id="e695d-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e695d-113">中樞的方法會自動變成資料流處理的中樞方法傳回時<xref:System.Threading.Channels.ChannelReader%601>， `IAsyncEnumerable<T>`， `Task<ChannelReader<T>>`，或`Task<IAsyncEnumerable<T>>`。</span><span class="sxs-lookup"><span data-stu-id="e695d-113">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601>, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e695d-114">中樞的方法會自動變成資料流處理的中樞方法傳回時<xref:System.Threading.Channels.ChannelReader%601>或`Task<ChannelReader<T>>`。</span><span class="sxs-lookup"><span data-stu-id="e695d-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="e695d-115">伺服器到用戶端串流</span><span class="sxs-lookup"><span data-stu-id="e695d-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e695d-116">串流處理中樞方法可以傳回`IAsyncEnumerable<T>`除了`ChannelReader<T>`。</span><span class="sxs-lookup"><span data-stu-id="e695d-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="e695d-117">最簡單的方式傳回`IAsyncEnumerable<T>`是透過讓中樞方法的非同步迭代器方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="e695d-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="e695d-118">中樞非同步迭代器方法可以接受`CancellationToken`時從資料流的用戶端取消訂閱所觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="e695d-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="e695d-119">非同步迭代器方法避免常見的問題，通道，例如未傳回`ChannelReader`及早或結束方法，而不會完成<xref:System.Threading.Channels.ChannelWriter`1>。</span><span class="sxs-lookup"><span data-stu-id="e695d-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="e695d-120">下列範例顯示資料串流到用戶端會使用通道的基本的概念。</span><span class="sxs-lookup"><span data-stu-id="e695d-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="e695d-121">每當物件寫入<xref:System.Threading.Channels.ChannelWriter%601>，該物件會立即傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="e695d-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="e695d-122">在結束時，`ChannelWriter`完成告知用戶端的資料流已關閉。</span><span class="sxs-lookup"><span data-stu-id="e695d-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="e695d-123">寫入`ChannelWriter<T>`在背景執行緒，然後傳回`ChannelReader`儘速。</span><span class="sxs-lookup"><span data-stu-id="e695d-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="e695d-124">其他中樞叫用會封鎖直到`ChannelReader`會傳回。</span><span class="sxs-lookup"><span data-stu-id="e695d-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="e695d-125">中的邏輯包裝`try ... catch`。</span><span class="sxs-lookup"><span data-stu-id="e695d-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="e695d-126">完成`Channel`中`catch`和外部`catch`確定中樞方法叫用會正確地完成。</span><span class="sxs-lookup"><span data-stu-id="e695d-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="e695d-127">伺服器到用戶端資料流的中樞方法可以接受`CancellationToken`時從資料流的用戶端取消訂閱所觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="e695d-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="e695d-128">若要停止伺服器作業，並釋放任何資源，如果用戶端中斷連線的資料流結束前使用此權杖。</span><span class="sxs-lookup"><span data-stu-id="e695d-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="e695d-129">用戶端-伺服器串流處理</span><span class="sxs-lookup"><span data-stu-id="e695d-129">Client-to-server streaming</span></span>

<span data-ttu-id="e695d-130">中樞的方法會自動變成用戶端-伺服器串流處理中樞方法，當它接受一或多個<xref:System.Threading.Channels.ChannelReader`1>s。</span><span class="sxs-lookup"><span data-stu-id="e695d-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more <xref:System.Threading.Channels.ChannelReader`1>s.</span></span> <span data-ttu-id="e695d-131">下列範例示範讀取從用戶端傳送的資料流處理資料的基本概念。</span><span class="sxs-lookup"><span data-stu-id="e695d-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="e695d-132">每當用戶端會寫入<xref:System.Threading.Channels.ChannelWriter`1>，將資料寫入到`ChannelReader`中樞方法讀取伺服器上。</span><span class="sxs-lookup"><span data-stu-id="e695d-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter`1>, the data is written into the `ChannelReader` on the server that the hub method is reading from.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="e695d-133">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e695d-133">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="e695d-134">伺服器到用戶端串流</span><span class="sxs-lookup"><span data-stu-id="e695d-134">Server-to-client streaming</span></span>

<span data-ttu-id="e695d-135">`StreamAsChannelAsync`方法`HubConnection`用來叫用伺服器到用戶端的串流處理方式。</span><span class="sxs-lookup"><span data-stu-id="e695d-135">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="e695d-136">將中樞方法的名稱和定義中的中樞方法的引數傳遞`StreamAsChannelAsync`。</span><span class="sxs-lookup"><span data-stu-id="e695d-136">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="e695d-137">泛型參數上的`StreamAsChannelAsync<T>`指定的資料流的方法所傳回的物件類型。</span><span class="sxs-lookup"><span data-stu-id="e695d-137">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="e695d-138">A`ChannelReader<T>`傳回的資料流的引動過程，和代表用戶端上的資料流。</span><span class="sxs-lookup"><span data-stu-id="e695d-138">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="e695d-139">用戶端-伺服器串流處理</span><span class="sxs-lookup"><span data-stu-id="e695d-139">Client-to-server streaming</span></span>

<span data-ttu-id="e695d-140">若要叫用用戶端-伺服器串流處理中樞方法從.NET 用戶端，建立`Channel`，並傳遞`ChannelReader`做為引數`SendAsync`， `InvokeAsync`，或`StreamAsChannelAsync`，取決於叫用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="e695d-140">To invoke a client-to-server streaming hub method from the .NET client, create a `Channel` and pass the `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="e695d-141">每當資料寫入`ChannelWriter`，在伺服器上的中樞方法從用戶端接收資料的新項目。</span><span class="sxs-lookup"><span data-stu-id="e695d-141">Whenever data is written to the `ChannelWriter`, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="e695d-142">若要結束資料流，完成與通道`channel.Writer.Complete()`。</span><span class="sxs-lookup"><span data-stu-id="e695d-142">To end the stream, complete the channel with `channel.Writer.Complete()`.</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="e695d-143">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e695d-143">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="e695d-144">伺服器到用戶端串流</span><span class="sxs-lookup"><span data-stu-id="e695d-144">Server-to-client streaming</span></span>

<span data-ttu-id="e695d-145">JavaScript 用戶端呼叫伺服器到用戶端串流上的方法與中樞`connection.stream`。</span><span class="sxs-lookup"><span data-stu-id="e695d-145">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="e695d-146">`stream` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="e695d-146">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="e695d-147">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="e695d-147">The name of the hub method.</span></span> <span data-ttu-id="e695d-148">下列範例中，在中樞方法名稱是`Counter`。</span><span class="sxs-lookup"><span data-stu-id="e695d-148">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="e695d-149">中樞的方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="e695d-149">Arguments defined in the hub method.</span></span> <span data-ttu-id="e695d-150">下列範例中，引數都要接收的資料流項目和資料流項目之間的延遲數目的計數。</span><span class="sxs-lookup"><span data-stu-id="e695d-150">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="e695d-151">`connection.stream` 會傳回`IStreamResult`，其中包含`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="e695d-151">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="e695d-152">傳遞`IStreamSubscriber`要`subscribe`並設定`next`， `error`，並`complete`回呼以接收來自通知`stream`引動過程。</span><span class="sxs-lookup"><span data-stu-id="e695d-152">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="e695d-153">若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="e695d-153">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="e695d-154">呼叫這個方法會導致取消`CancellationToken`中樞方法參數，如果您提供的其中一個。</span><span class="sxs-lookup"><span data-stu-id="e695d-154">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="e695d-155">若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="e695d-155">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="e695d-156">用戶端-伺服器串流處理</span><span class="sxs-lookup"><span data-stu-id="e695d-156">Client-to-server streaming</span></span>

<span data-ttu-id="e695d-157">JavaScript 用戶端中樞上呼叫用戶端-伺服器串流處理方法，藉由傳入`Subject`做為引數`send`， `invoke`，或`stream`，取決於叫用中樞方法。</span><span class="sxs-lookup"><span data-stu-id="e695d-157">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="e695d-158">`Subject`是一個類別，看起來像`Subject`。</span><span class="sxs-lookup"><span data-stu-id="e695d-158">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="e695d-159">比方說在 RxJS，您可以使用[主旨](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)從該程式庫的類別。</span><span class="sxs-lookup"><span data-stu-id="e695d-159">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="e695d-160">呼叫`subject.next(item)`與項目會寫入資料流中的項目和中樞方法接收伺服器上的項目。</span><span class="sxs-lookup"><span data-stu-id="e695d-160">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="e695d-161">若要結束資料流，呼叫`subject.complete()`。</span><span class="sxs-lookup"><span data-stu-id="e695d-161">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="e695d-162">Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="e695d-162">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="e695d-163">伺服器到用戶端串流</span><span class="sxs-lookup"><span data-stu-id="e695d-163">Server-to-client streaming</span></span>

<span data-ttu-id="e695d-164">SignalR Java 用戶端會使用`stream`方法來叫用資料流的方法。</span><span class="sxs-lookup"><span data-stu-id="e695d-164">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="e695d-165">`stream` 接受三個或多個引數：</span><span class="sxs-lookup"><span data-stu-id="e695d-165">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="e695d-166">資料流項目的預期的類型。</span><span class="sxs-lookup"><span data-stu-id="e695d-166">The expected type of the stream items.</span></span>
* <span data-ttu-id="e695d-167">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="e695d-167">The name of the hub method.</span></span>
* <span data-ttu-id="e695d-168">中樞的方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="e695d-168">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="e695d-169">`stream`方法`HubConnection`傳回資料流項目類型的可預見值。</span><span class="sxs-lookup"><span data-stu-id="e695d-169">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="e695d-170">可觀察的型別`subscribe`方法正是`onNext`，`onError`和`onCompleted`定義處理常式。</span><span class="sxs-lookup"><span data-stu-id="e695d-170">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e695d-171">其他資源</span><span class="sxs-lookup"><span data-stu-id="e695d-171">Additional resources</span></span>

* [<span data-ttu-id="e695d-172">中樞</span><span class="sxs-lookup"><span data-stu-id="e695d-172">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e695d-173">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="e695d-173">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e695d-174">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="e695d-174">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e695d-175">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="e695d-175">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
