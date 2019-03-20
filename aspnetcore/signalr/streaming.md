---
title: 使用資料流在 ASP.NET Core SignalR
author: bradygaster
description: 了解如何從伺服器中樞方法會傳回值的資料流，並取用使用.NET 和 JavaScript 的用戶端的資料流。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 7c176e3f21ffca7b97d9d3c2e8861032f22587b8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264310"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="c5294-103">使用資料流在 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c5294-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c5294-104">由[brennan Conroy](https://github.com/BrennanConroy)提供</span><span class="sxs-lookup"><span data-stu-id="c5294-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="c5294-105">ASP.NET Core SignalR 支援資料流的伺服器方法的傳回值。</span><span class="sxs-lookup"><span data-stu-id="c5294-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="c5294-106">這是適用於其中的資料片段會在一段時間的案例。</span><span class="sxs-lookup"><span data-stu-id="c5294-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="c5294-107">時傳回的值串流處理到用戶端，每個片段會傳送至用戶端，它會變成可用，而不是等待變成可用的所有資料。</span><span class="sxs-lookup"><span data-stu-id="c5294-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="c5294-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c5294-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="c5294-109">設定中樞</span><span class="sxs-lookup"><span data-stu-id="c5294-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c5294-110">中樞的方法會自動變成資料流處理的中樞方法傳回時`ChannelReader<T>`， `IAsyncEnumerable<T>`， `Task<ChannelReader<T>>`，或`Task<IAsyncEnumerable<T>>`。</span><span class="sxs-lookup"><span data-stu-id="c5294-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c5294-111">中樞的方法會自動變成資料流處理的中樞方法傳回時`ChannelReader<T>`或`Task<ChannelReader<T>>`。</span><span class="sxs-lookup"><span data-stu-id="c5294-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c5294-112">在 ASP.NET Core 3.0 或更新版本、 串流處理中樞方法可以傳回`IAsyncEnumerable<T>`除了`ChannelReader<T>`。</span><span class="sxs-lookup"><span data-stu-id="c5294-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="c5294-113">最簡單的方式傳回`IAsyncEnumerable<T>`是透過讓中樞方法的非同步迭代器方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="c5294-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="c5294-114">中樞非同步迭代器方法可以接受`CancellationToken`從資料流的用戶端取消訂閱時，會觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="c5294-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="c5294-115">非同步迭代器方法，輕鬆避免常見的問題，通道等不會傳回`ChannelReader`及早或結束方法，而不會完成`ChannelWriter`。</span><span class="sxs-lookup"><span data-stu-id="c5294-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="c5294-116">下列範例顯示資料串流到用戶端會使用通道的基本的概念。</span><span class="sxs-lookup"><span data-stu-id="c5294-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="c5294-117">每當物件寫入`ChannelWriter`該物件會立即傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="c5294-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="c5294-118">在結束時，`ChannelWriter`完成告知用戶端的資料流已關閉。</span><span class="sxs-lookup"><span data-stu-id="c5294-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="c5294-119">寫入`ChannelWriter`在背景執行緒，然後傳回`ChannelReader`儘速。</span><span class="sxs-lookup"><span data-stu-id="c5294-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="c5294-120">其他中樞引動過程將會遭到封鎖，直到`ChannelReader`會傳回。</span><span class="sxs-lookup"><span data-stu-id="c5294-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="c5294-121">包裝您的邏輯中`try ... catch`並完成`Channel`在 catch 和外部 catch 以確保中樞方法叫用正確地完成。</span><span class="sxs-lookup"><span data-stu-id="c5294-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="c5294-122">在 ASP.NET Core 2.2 或更新版本，串流處理中樞方法可以接受`CancellationToken`從資料流的用戶端取消訂閱時，會觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="c5294-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="c5294-123">若要停止伺服器作業，並釋放任何資源，如果用戶端中斷連線的資料流結束前使用此權杖。</span><span class="sxs-lookup"><span data-stu-id="c5294-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="c5294-124">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="c5294-124">.NET client</span></span>

<span data-ttu-id="c5294-125">`StreamAsChannelAsync`方法`HubConnection`用來叫用的資料流的方法。</span><span class="sxs-lookup"><span data-stu-id="c5294-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="c5294-126">將中樞方法的名稱和定義中的中樞方法的引數傳遞`StreamAsChannelAsync`。</span><span class="sxs-lookup"><span data-stu-id="c5294-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="c5294-127">泛型參數上的`StreamAsChannelAsync<T>`指定的資料流的方法所傳回的物件類型。</span><span class="sxs-lookup"><span data-stu-id="c5294-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="c5294-128">A`ChannelReader<T>`傳回的資料流引動過程，和代表用戶端上的資料流。</span><span class="sxs-lookup"><span data-stu-id="c5294-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="c5294-129">若要讀取資料，常見的模式是迴圈`WaitToReadAsync`並呼叫`TryRead`可用資料時。</span><span class="sxs-lookup"><span data-stu-id="c5294-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="c5294-130">資料流已關閉伺服器，或取消語彙基元傳遞至時，將會結束迴圈`StreamAsChannelAsync`已取消。</span><span class="sxs-lookup"><span data-stu-id="c5294-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="c5294-131">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="c5294-131">JavaScript client</span></span>

<span data-ttu-id="c5294-132">JavaScript 用戶端呼叫中樞上資料流的方法，使用`connection.stream`。</span><span class="sxs-lookup"><span data-stu-id="c5294-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="c5294-133">`stream` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="c5294-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="c5294-134">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="c5294-134">The name of the hub method.</span></span> <span data-ttu-id="c5294-135">下列範例中，在中樞方法名稱是`Counter`。</span><span class="sxs-lookup"><span data-stu-id="c5294-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="c5294-136">中樞的方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="c5294-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="c5294-137">在下列範例中，引數是： 資料流項目，若要接收，以及資料流項目之間的延遲數目的計數。</span><span class="sxs-lookup"><span data-stu-id="c5294-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="c5294-138">`connection.stream` 會傳回`IStreamResult`其中包含`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="c5294-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="c5294-139">傳遞`IStreamSubscriber`要`subscribe`並設定`next`， `error`，和`complete`若要取得通知的回呼`stream`引動過程。</span><span class="sxs-lookup"><span data-stu-id="c5294-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="c5294-140">若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="c5294-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="c5294-141">若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="c5294-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="c5294-142">呼叫這個方法會造成`CancellationToken`參數 （如果您提供一個） 之中樞方法的取消。</span><span class="sxs-lookup"><span data-stu-id="c5294-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="java-client"></a><span data-ttu-id="c5294-143">Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="c5294-143">Java client</span></span>

<span data-ttu-id="c5294-144">SignalR Java 用戶端會使用`stream`方法來叫用資料流的方法。</span><span class="sxs-lookup"><span data-stu-id="c5294-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="c5294-145">它接受三個或多個引數：</span><span class="sxs-lookup"><span data-stu-id="c5294-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="c5294-146">資料流項目的預期的類型</span><span class="sxs-lookup"><span data-stu-id="c5294-146">The expected type of the stream items</span></span>
* <span data-ttu-id="c5294-147">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="c5294-147">The name of the hub method.</span></span>
* <span data-ttu-id="c5294-148">中樞的方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="c5294-148">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="c5294-149">`stream`方法`HubConnection`傳回資料流項目類型的可預見值。</span><span class="sxs-lookup"><span data-stu-id="c5294-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="c5294-150">可觀察的型別`subscribe`方法可讓您定義您`onNext`，`onError`和`onCompleted`處理常式。</span><span class="sxs-lookup"><span data-stu-id="c5294-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="c5294-151">相關資源</span><span class="sxs-lookup"><span data-stu-id="c5294-151">Related resources</span></span>

* [<span data-ttu-id="c5294-152">中樞</span><span class="sxs-lookup"><span data-stu-id="c5294-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c5294-153">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="c5294-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c5294-154">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="c5294-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c5294-155">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="c5294-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
