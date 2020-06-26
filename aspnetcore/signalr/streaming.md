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
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="df8a7-103">在 ASP.NET Core 中使用串流SignalR</span><span class="sxs-lookup"><span data-stu-id="df8a7-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="df8a7-104">依[Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="df8a7-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df8a7-105">ASP.NET Core SignalR 支援從用戶端到伺服器，以及從伺服器到用戶端的串流處理。</span><span class="sxs-lookup"><span data-stu-id="df8a7-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="df8a7-106">這適用于資料片段在一段時間內抵達的案例。</span><span class="sxs-lookup"><span data-stu-id="df8a7-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="df8a7-107">進行串流處理時，每個片段會在用戶端或伺服器可供使用時立即傳送到該伺服器，而不是等待所有資料都可供使用。</span><span class="sxs-lookup"><span data-stu-id="df8a7-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="df8a7-108">ASP.NET Core SignalR 支援伺服器方法的資料流程傳回值。</span><span class="sxs-lookup"><span data-stu-id="df8a7-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="df8a7-109">這適用于資料片段在一段時間內抵達的案例。</span><span class="sxs-lookup"><span data-stu-id="df8a7-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="df8a7-110">當傳回值串流處理至用戶端時，每個片段會在其可用時立即傳送至用戶端，而不是等待所有資料都可供使用。</span><span class="sxs-lookup"><span data-stu-id="df8a7-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="df8a7-111">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="df8a7-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="df8a7-112">設定用於串流的中樞</span><span class="sxs-lookup"><span data-stu-id="df8a7-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df8a7-113">當中樞方法傳回 <xref:System.Collections.Generic.IAsyncEnumerable`1> 、、或時，它會自動變成串流中樞方法 <xref:System.Threading.Channels.ChannelReader%601> `Task<IAsyncEnumerable<T>>` `Task<ChannelReader<T>>` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="df8a7-114">中樞方法會在傳回或時自動變成串流中樞方法 <xref:System.Threading.Channels.ChannelReader%601> `Task<ChannelReader<T>>` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="df8a7-115">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df8a7-116">除了之外，串流中樞方法還可以傳回 `IAsyncEnumerable<T>` `ChannelReader<T>` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="df8a7-117">最簡單的方法 `IAsyncEnumerable<T>` 是將中樞方法設為非同步反覆運算器方法，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="df8a7-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="df8a7-118">中樞非同步反覆運算器方法可以接受 `CancellationToken` 用戶端從串流取消訂閱時所觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="df8a7-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="df8a7-119">非同步反覆運算器方法會避免通道常見的問題，例如不 `ChannelReader` 會提早傳回，也不會結束方法，而不會完成 <xref:System.Threading.Channels.ChannelWriter`1> 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="df8a7-120">下列範例顯示使用通道將資料串流處理至用戶端的基本概念。</span><span class="sxs-lookup"><span data-stu-id="df8a7-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="df8a7-121">每當物件寫入時 <xref:System.Threading.Channels.ChannelWriter%601> ，物件就會立即傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="df8a7-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="df8a7-122">結束 `ChannelWriter` 時，會完成以告知用戶端資料流程已關閉。</span><span class="sxs-lookup"><span data-stu-id="df8a7-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="df8a7-123">將寫入 `ChannelWriter<T>` 背景執行緒，並 `ChannelReader` 儘快傳回。</span><span class="sxs-lookup"><span data-stu-id="df8a7-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="df8a7-124">在傳回之前，會封鎖其他中樞調用 `ChannelReader` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="df8a7-125">將邏輯包裝在中 `try ... catch` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="df8a7-126">完成 `Channel` 中 `catch` 和外部的， `catch` 以確定中樞方法叫用已正確完成。</span><span class="sxs-lookup"><span data-stu-id="df8a7-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="df8a7-127">伺服器對用戶端串流中樞方法可以接受 `CancellationToken` 用戶端從串流取消訂閱時所觸發的參數。</span><span class="sxs-lookup"><span data-stu-id="df8a7-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="df8a7-128">若用戶端在資料流程結尾之前中斷連線，請使用此權杖來停止伺服器作業並釋放任何資源。</span><span class="sxs-lookup"><span data-stu-id="df8a7-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="df8a7-129">用戶端對伺服器串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-129">Client-to-server streaming</span></span>

<span data-ttu-id="df8a7-130">中樞方法會在接受一或多個類型或的物件時，自動成為用戶端對伺服器的串流中樞方法 <xref:System.Threading.Channels.ChannelReader%601> <xref:System.Collections.Generic.IAsyncEnumerable%601> 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="df8a7-131">下列範例顯示讀取從用戶端傳送之串流資料的基本概念。</span><span class="sxs-lookup"><span data-stu-id="df8a7-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="df8a7-132">每當用戶端寫入時 <xref:System.Threading.Channels.ChannelWriter%601> ，就會將資料寫入 `ChannelReader` 中樞方法所讀取之伺服器上的。</span><span class="sxs-lookup"><span data-stu-id="df8a7-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="df8a7-133"><xref:System.Collections.Generic.IAsyncEnumerable%601>以下是方法的版本。</span><span class="sxs-lookup"><span data-stu-id="df8a7-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

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

## <a name="net-client"></a><span data-ttu-id="df8a7-134">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="df8a7-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="df8a7-135">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="df8a7-136">`StreamAsync`上的和 `StreamAsChannelAsync` 方法 `HubConnection` 是用來叫用伺服器對用戶端串流方法。</span><span class="sxs-lookup"><span data-stu-id="df8a7-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="df8a7-137">將中樞方法中定義的中樞方法名稱和引數傳遞至 `StreamAsync` 或 `StreamAsChannelAsync` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="df8a7-138">和上的泛型參數會 `StreamAsync<T>` `StreamAsChannelAsync<T>` 指定串流方法所傳回的物件類型。</span><span class="sxs-lookup"><span data-stu-id="df8a7-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="df8a7-139">型別 `IAsyncEnumerable<T>` 為或的物件 `ChannelReader<T>` 會從資料流程調用傳回，並代表用戶端上的資料流程。</span><span class="sxs-lookup"><span data-stu-id="df8a7-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="df8a7-140">傳回的 `StreamAsync` 範例 `IAsyncEnumerable<int>` ：</span><span class="sxs-lookup"><span data-stu-id="df8a7-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

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

<span data-ttu-id="df8a7-141">傳回的對應 `StreamAsChannelAsync` 範例 `ChannelReader<int>` ：</span><span class="sxs-lookup"><span data-stu-id="df8a7-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

<span data-ttu-id="df8a7-142">`StreamAsChannelAsync`上的方法 `HubConnection` 是用來叫用伺服器對用戶端串流方法。</span><span class="sxs-lookup"><span data-stu-id="df8a7-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="df8a7-143">將中樞方法中定義的中樞方法名稱和引數傳遞至 `StreamAsChannelAsync` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="df8a7-144">上的泛型參數會 `StreamAsChannelAsync<T>` 指定串流方法所傳回的物件類型。</span><span class="sxs-lookup"><span data-stu-id="df8a7-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="df8a7-145">`ChannelReader<T>`會從資料流程調用傳回，並代表用戶端上的資料流程。</span><span class="sxs-lookup"><span data-stu-id="df8a7-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="df8a7-146">`StreamAsChannelAsync`上的方法 `HubConnection` 是用來叫用伺服器對用戶端串流方法。</span><span class="sxs-lookup"><span data-stu-id="df8a7-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="df8a7-147">將中樞方法中定義的中樞方法名稱和引數傳遞至 `StreamAsChannelAsync` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="df8a7-148">上的泛型參數會 `StreamAsChannelAsync<T>` 指定串流方法所傳回的物件類型。</span><span class="sxs-lookup"><span data-stu-id="df8a7-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="df8a7-149">`ChannelReader<T>`會從資料流程調用傳回，並代表用戶端上的資料流程。</span><span class="sxs-lookup"><span data-stu-id="df8a7-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="df8a7-150">用戶端對伺服器串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-150">Client-to-server streaming</span></span>

<span data-ttu-id="df8a7-151">有兩種方式可從 .NET 用戶端叫用用戶端到伺服器的串流中樞方法。</span><span class="sxs-lookup"><span data-stu-id="df8a7-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="df8a7-152">`IAsyncEnumerable<T>` `ChannelReader` `SendAsync` 視所叫用 `InvokeAsync` `StreamAsChannelAsync` 的中樞方法而定，您可以將或當做引數傳入、或。</span><span class="sxs-lookup"><span data-stu-id="df8a7-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="df8a7-153">每次將資料寫入 `IAsyncEnumerable` 或 `ChannelWriter` 物件時，伺服器上的中樞方法都會收到新的專案，其中包含來自用戶端的資料。</span><span class="sxs-lookup"><span data-stu-id="df8a7-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="df8a7-154">如果使用 `IAsyncEnumerable` 物件，則資料流程會在傳回資料流程專案的方法結束後結束。</span><span class="sxs-lookup"><span data-stu-id="df8a7-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

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

<span data-ttu-id="df8a7-155">或者，如果您正在使用 `ChannelWriter` ，您可以使用來完成通道 `channel.Writer.Complete()` ：</span><span class="sxs-lookup"><span data-stu-id="df8a7-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="df8a7-156">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="df8a7-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="df8a7-157">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-157">Server-to-client streaming</span></span>

<span data-ttu-id="df8a7-158">JavaScript 用戶端會使用在中樞上呼叫伺服器對用戶端串流方法 `connection.stream` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="df8a7-159">`stream`方法接受兩個引數：</span><span class="sxs-lookup"><span data-stu-id="df8a7-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="df8a7-160">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="df8a7-160">The name of the hub method.</span></span> <span data-ttu-id="df8a7-161">在下列範例中，中樞方法名稱是 `Counter` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="df8a7-162">中樞方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="df8a7-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="df8a7-163">在下列範例中，引數是要接收的資料流程專案數，以及資料流程專案之間的延遲計數。</span><span class="sxs-lookup"><span data-stu-id="df8a7-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="df8a7-164">`connection.stream`傳回 `IStreamResult` ，其中包含 `subscribe` 方法。</span><span class="sxs-lookup"><span data-stu-id="df8a7-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="df8a7-165">將傳遞 `IStreamSubscriber` 至， `subscribe` 並設定 `next` 、 `error` 和 `complete` 回呼，以接收來自調用的通知 `stream` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="df8a7-166">若要從用戶端結束資料流程，請 `dispose` 在 `ISubscription` 從方法傳回的上呼叫方法 `subscribe` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="df8a7-167">`CancellationToken`如果您提供中樞方法的參數，呼叫這個方法會導致取消。</span><span class="sxs-lookup"><span data-stu-id="df8a7-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="df8a7-168">若要從用戶端結束資料流程，請 `dispose` 在 `ISubscription` 從方法傳回的上呼叫方法 `subscribe` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="df8a7-169">用戶端對伺服器串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-169">Client-to-server streaming</span></span>

<span data-ttu-id="df8a7-170">JavaScript 用戶端會呼叫中樞上的用戶端對伺服器串流方法，方法是將當做 `Subject` 引數傳遞至 `send` 、 `invoke` 或 `stream` ，視所叫用的中樞方法而定。</span><span class="sxs-lookup"><span data-stu-id="df8a7-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="df8a7-171">`Subject`是看起來像的類別 `Subject` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="df8a7-172">例如，在 RxJS 中，您可以使用該程式庫中的[Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject)類別。</span><span class="sxs-lookup"><span data-stu-id="df8a7-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="df8a7-173">使用專案呼叫會將 `subject.next(item)` 專案寫入資料流程，而中樞方法會在伺服器上接收專案。</span><span class="sxs-lookup"><span data-stu-id="df8a7-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="df8a7-174">若要結束資料流程，請呼叫 `subject.complete()` 。</span><span class="sxs-lookup"><span data-stu-id="df8a7-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="df8a7-175">Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="df8a7-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="df8a7-176">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="df8a7-176">Server-to-client streaming</span></span>

<span data-ttu-id="df8a7-177">SignalRJAVA 用戶端會使用 `stream` 方法來叫用串流方法。</span><span class="sxs-lookup"><span data-stu-id="df8a7-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="df8a7-178">`stream`接受三個或多個引數：</span><span class="sxs-lookup"><span data-stu-id="df8a7-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="df8a7-179">資料流程專案的預期型別。</span><span class="sxs-lookup"><span data-stu-id="df8a7-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="df8a7-180">中樞方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="df8a7-180">The name of the hub method.</span></span>
* <span data-ttu-id="df8a7-181">中樞方法中定義的引數。</span><span class="sxs-lookup"><span data-stu-id="df8a7-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="df8a7-182">上的方法會傳回 `stream` `HubConnection` 資料流程專案類型的可觀察。</span><span class="sxs-lookup"><span data-stu-id="df8a7-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="df8a7-183">可觀察類型的 `subscribe` 方法為 `onNext` ， `onError` 並 `onCompleted` 定義處理常式。</span><span class="sxs-lookup"><span data-stu-id="df8a7-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="df8a7-184">其他資源</span><span class="sxs-lookup"><span data-stu-id="df8a7-184">Additional resources</span></span>

* [<span data-ttu-id="df8a7-185">中樞</span><span class="sxs-lookup"><span data-stu-id="df8a7-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="df8a7-186">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="df8a7-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="df8a7-187">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="df8a7-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="df8a7-188">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="df8a7-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
