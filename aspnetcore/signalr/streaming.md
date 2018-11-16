---
title: 使用資料流在 ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 6d5f707bd2a37e1999c6e87e3cfc369aa0301207
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708435"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="12e4c-102">使用資料流在 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="12e4c-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="12e4c-103">由[brennan Conroy](https://github.com/BrennanConroy)提供</span><span class="sxs-lookup"><span data-stu-id="12e4c-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="12e4c-104">ASP.NET Core SignalR 支援資料流的伺服器方法的傳回值。</span><span class="sxs-lookup"><span data-stu-id="12e4c-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="12e4c-105">這是適用於其中的資料片段會在一段時間的案例。</span><span class="sxs-lookup"><span data-stu-id="12e4c-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="12e4c-106">時傳回的值串流處理到用戶端，每個片段會傳送至用戶端，它會變成可用，而不是等待變成可用的所有資料。</span><span class="sxs-lookup"><span data-stu-id="12e4c-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="12e4c-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12e4c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="12e4c-108">設定中樞</span><span class="sxs-lookup"><span data-stu-id="12e4c-108">Set up the hub</span></span>

<span data-ttu-id="12e4c-109">中樞的方法會自動變成資料流處理的中樞方法傳回時`ChannelReader<T>`或`Task<ChannelReader<T>>`。</span><span class="sxs-lookup"><span data-stu-id="12e4c-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="12e4c-110">以下是此範例示範的資料串流到用戶端的基本概念。</span><span class="sxs-lookup"><span data-stu-id="12e4c-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="12e4c-111">每當物件寫入`ChannelReader`該物件會立即傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="12e4c-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="12e4c-112">在結束時，`ChannelReader`完成告知用戶端的資料流已關閉。</span><span class="sxs-lookup"><span data-stu-id="12e4c-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="12e4c-113">寫入`ChannelReader`在背景執行緒，然後傳回`ChannelReader`儘速。</span><span class="sxs-lookup"><span data-stu-id="12e4c-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="12e4c-114">其他中樞引動過程將會遭到封鎖，直到`ChannelReader`會傳回。</span><span class="sxs-lookup"><span data-stu-id="12e4c-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?range=12-36)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=11-35)]

> [!NOTE]
> <span data-ttu-id="12e4c-115">在 ASP.NET Core 2.2 或更新版本，串流處理中樞方法可以接受`CancellationToken`從資料流的用戶端取消訂閱時，會觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="12e4c-115">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="12e4c-116">若要停止伺服器作業，並釋放任何資源，如果用戶端中斷連線的資料流結束前使用此權杖。</span><span class="sxs-lookup"><span data-stu-id="12e4c-116">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="12e4c-117">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="12e4c-117">.NET client</span></span>

<span data-ttu-id="12e4c-118">`StreamAsChannelAsync`方法`HubConnection`用來叫用的資料流的方法。</span><span class="sxs-lookup"><span data-stu-id="12e4c-118">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="12e4c-119">將中樞方法的名稱和定義中的中樞方法的引數傳遞`StreamAsChannelAsync`。</span><span class="sxs-lookup"><span data-stu-id="12e4c-119">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="12e4c-120">泛型參數上的`StreamAsChannelAsync<T>`指定的資料流的方法所傳回的物件類型。</span><span class="sxs-lookup"><span data-stu-id="12e4c-120">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="12e4c-121">A`ChannelReader<T>`傳回的資料流引動過程，和代表用戶端上的資料流。</span><span class="sxs-lookup"><span data-stu-id="12e4c-121">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="12e4c-122">若要讀取資料，常見的模式是迴圈`WaitToReadAsync`並呼叫`TryRead`可用資料時。</span><span class="sxs-lookup"><span data-stu-id="12e4c-122">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="12e4c-123">資料流已關閉伺服器，或取消語彙基元傳遞至時，將會結束迴圈`StreamAsChannelAsync`已取消。</span><span class="sxs-lookup"><span data-stu-id="12e4c-123">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
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

## <a name="javascript-client"></a><span data-ttu-id="12e4c-124">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="12e4c-124">JavaScript client</span></span>

<span data-ttu-id="12e4c-125">JavaScript 用戶端呼叫中樞上資料流的方法，使用`connection.stream`。</span><span class="sxs-lookup"><span data-stu-id="12e4c-125">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="12e4c-126">`stream` 方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="12e4c-126">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="12e4c-127">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="12e4c-127">The name of the hub method.</span></span> <span data-ttu-id="12e4c-128">下列範例中，在中樞方法名稱是`Counter`。</span><span class="sxs-lookup"><span data-stu-id="12e4c-128">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="12e4c-129">中樞的方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="12e4c-129">Arguments defined in the hub method.</span></span> <span data-ttu-id="12e4c-130">在下列範例中，引數是： 資料流項目，若要接收，以及資料流項目之間的延遲數目的計數。</span><span class="sxs-lookup"><span data-stu-id="12e4c-130">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="12e4c-131">`connection.stream` 會傳回`IStreamResult`其中包含`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="12e4c-131">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="12e4c-132">傳遞`IStreamSubscriber`要`subscribe`並設定`next`， `error`，和`complete`若要取得通知的回呼`stream`引動過程。</span><span class="sxs-lookup"><span data-stu-id="12e4c-132">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="12e4c-133">若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="12e4c-133">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="12e4c-134">若要結束用戶端從資料流，呼叫`dispose`方法`ISubscription`傳回`subscribe`方法。</span><span class="sxs-lookup"><span data-stu-id="12e4c-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="12e4c-135">呼叫這個方法會造成`CancellationToken`參數 （如果您提供一個） 之中樞方法的取消。</span><span class="sxs-lookup"><span data-stu-id="12e4c-135">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="12e4c-136">相關資源</span><span class="sxs-lookup"><span data-stu-id="12e4c-136">Related resources</span></span>

* [<span data-ttu-id="12e4c-137">中樞</span><span class="sxs-lookup"><span data-stu-id="12e4c-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="12e4c-138">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="12e4c-138">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="12e4c-139">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="12e4c-139">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="12e4c-140">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="12e4c-140">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
