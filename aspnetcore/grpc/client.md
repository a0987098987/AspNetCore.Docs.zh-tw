---
title: 使用 .NET 用戶端呼叫 gRPC 服務
author: jamesnk
description: 瞭解如何使用 .NET gRPC 用戶端呼叫 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667171"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="d4902-103">使用 .NET 用戶端呼叫 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="d4902-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="d4902-104">.NET gRPC 用戶端程式庫可在[gRPC .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)NuGet 套件中取得。</span><span class="sxs-lookup"><span data-stu-id="d4902-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="d4902-105">本檔說明如何：</span><span class="sxs-lookup"><span data-stu-id="d4902-105">This document explains how to:</span></span>

* <span data-ttu-id="d4902-106">將 gRPC 用戶端設定為呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="d4902-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="d4902-107">對一元、伺服器資料流程、用戶端資料流程和雙向串流方法進行 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="d4902-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="d4902-108">設定 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="d4902-108">Configure gRPC client</span></span>

<span data-ttu-id="d4902-109">gRPC 用戶端是[從 *\*的 proto*檔案產生](xref:grpc/basics#generated-c-assets)的具體用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="d4902-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="d4902-110">具體 gRPC 用戶端的方法會轉譯為 *\*的 proto*檔案中的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="d4902-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="d4902-111">GRPC 用戶端是從通道建立的。</span><span class="sxs-lookup"><span data-stu-id="d4902-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="d4902-112">首先，使用 `GrpcChannel.ForAddress` 建立通道，然後使用通道來建立 gRPC 用戶端：</span><span class="sxs-lookup"><span data-stu-id="d4902-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="d4902-113">通道代表 gRPC 服務的長時間連接。</span><span class="sxs-lookup"><span data-stu-id="d4902-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="d4902-114">建立通道時，會使用與呼叫服務相關的選項進行設定。</span><span class="sxs-lookup"><span data-stu-id="d4902-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="d4902-115">例如，用來進行呼叫的 `HttpClient`、傳送和接收訊息大小上限，以及記錄可以在 `GrpcChannelOptions` 上指定，並與 `GrpcChannel.ForAddress`搭配使用。</span><span class="sxs-lookup"><span data-stu-id="d4902-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="d4902-116">如需完整的選項清單，請參閱[用戶端設定選項](xref:grpc/configuration#configure-client-options)。</span><span class="sxs-lookup"><span data-stu-id="d4902-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="d4902-117">通道和用戶端效能和使用方式：</span><span class="sxs-lookup"><span data-stu-id="d4902-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="d4902-118">建立通道可能是昂貴的作業。</span><span class="sxs-lookup"><span data-stu-id="d4902-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="d4902-119">重複使用 gRPC 呼叫的通道可提供效能優勢。</span><span class="sxs-lookup"><span data-stu-id="d4902-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="d4902-120">系統會使用通道來建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d4902-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="d4902-121">gRPC 用戶端是輕量物件，不需要快取或重複使用。</span><span class="sxs-lookup"><span data-stu-id="d4902-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="d4902-122">您可以從通道建立多個 gRPC 用戶端，包括不同類型的用戶端。</span><span class="sxs-lookup"><span data-stu-id="d4902-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="d4902-123">通道和從通道建立的用戶端可以安全地由多個執行緒使用。</span><span class="sxs-lookup"><span data-stu-id="d4902-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="d4902-124">從通道建立的用戶端可以進行多個同時呼叫。</span><span class="sxs-lookup"><span data-stu-id="d4902-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="d4902-125">`GrpcChannel.ForAddress` 不是建立 gRPC 用戶端的唯一選項。</span><span class="sxs-lookup"><span data-stu-id="d4902-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="d4902-126">如果您是從 ASP.NET Core 應用程式呼叫 gRPC services，請考慮[gRPC 用戶端工廠整合](xref:grpc/clientfactory)。</span><span class="sxs-lookup"><span data-stu-id="d4902-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="d4902-127">gRPC 與 `HttpClientFactory` 整合提供了建立 gRPC 用戶端的集中式替代方案。</span><span class="sxs-lookup"><span data-stu-id="d4902-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="d4902-128">需要其他設定，才能[使用 .net 用戶端呼叫不安全的 gRPC 服務](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)。</span><span class="sxs-lookup"><span data-stu-id="d4902-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="d4902-129">Xamarin 目前不支援透過 HTTP/2 使用 `Grpc.Net.Client` 呼叫 gRPC。</span><span class="sxs-lookup"><span data-stu-id="d4902-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="d4902-130">我們正致力於改善未來 Xamarin 版本中的 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="d4902-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="d4902-131">[Grpc](https://www.nuget.org/packages/Grpc.Core)和[Grpc-Web](xref:grpc/browser)是可行的替代方案。</span><span class="sxs-lookup"><span data-stu-id="d4902-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="d4902-132">進行 gRPC 呼叫</span><span class="sxs-lookup"><span data-stu-id="d4902-132">Make gRPC calls</span></span>

<span data-ttu-id="d4902-133">GRPC 呼叫是藉由呼叫用戶端上的方法來起始。</span><span class="sxs-lookup"><span data-stu-id="d4902-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="d4902-134">GRPC 用戶端會處理訊息序列化，並將 gRPC 呼叫定址至正確的服務。</span><span class="sxs-lookup"><span data-stu-id="d4902-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="d4902-135">gRPC 有不同類型的方法。</span><span class="sxs-lookup"><span data-stu-id="d4902-135">gRPC has different types of methods.</span></span> <span data-ttu-id="d4902-136">您使用用戶端進行 gRPC 呼叫的方式，取決於您所呼叫的方法類型。</span><span class="sxs-lookup"><span data-stu-id="d4902-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="d4902-137">GRPC 方法類型為：</span><span class="sxs-lookup"><span data-stu-id="d4902-137">The gRPC method types are:</span></span>

* <span data-ttu-id="d4902-138">一元 (Unary)</span><span class="sxs-lookup"><span data-stu-id="d4902-138">Unary</span></span>
* <span data-ttu-id="d4902-139">伺服器串流</span><span class="sxs-lookup"><span data-stu-id="d4902-139">Server streaming</span></span>
* <span data-ttu-id="d4902-140">用戶端串流</span><span class="sxs-lookup"><span data-stu-id="d4902-140">Client streaming</span></span>
* <span data-ttu-id="d4902-141">雙向串流</span><span class="sxs-lookup"><span data-stu-id="d4902-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="d4902-142">一元呼叫</span><span class="sxs-lookup"><span data-stu-id="d4902-142">Unary call</span></span>

<span data-ttu-id="d4902-143">一元呼叫會從傳送要求訊息的用戶端開始。</span><span class="sxs-lookup"><span data-stu-id="d4902-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="d4902-144">當服務完成時，就會傳迴響應消息。</span><span class="sxs-lookup"><span data-stu-id="d4902-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="d4902-145">*\** 中的每個一元服務方法都會在具體 gRPC 用戶端類型上產生兩個 .net 方法，以呼叫方法：非同步方法和封鎖方法。</span><span class="sxs-lookup"><span data-stu-id="d4902-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="d4902-146">例如，在 `GreeterClient` 上，有兩種方式可呼叫 `SayHello`：</span><span class="sxs-lookup"><span data-stu-id="d4902-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="d4902-147">`GreeterClient.SayHelloAsync`-以非同步方式呼叫 `Greeter.SayHello` 服務。</span><span class="sxs-lookup"><span data-stu-id="d4902-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="d4902-148">可以等候。</span><span class="sxs-lookup"><span data-stu-id="d4902-148">Can be awaited.</span></span>
* <span data-ttu-id="d4902-149">`GreeterClient.SayHello`-呼叫 `Greeter.SayHello` 服務並封鎖直到完成為止。</span><span class="sxs-lookup"><span data-stu-id="d4902-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="d4902-150">不要在非同步程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="d4902-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="d4902-151">伺服器串流呼叫</span><span class="sxs-lookup"><span data-stu-id="d4902-151">Server streaming call</span></span>

<span data-ttu-id="d4902-152">伺服器串流呼叫會從傳送要求訊息的用戶端開始。</span><span class="sxs-lookup"><span data-stu-id="d4902-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="d4902-153">`ResponseStream.MoveNext()` 讀取從服務資料流程處理的訊息。</span><span class="sxs-lookup"><span data-stu-id="d4902-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="d4902-154">當 `ResponseStream.MoveNext()` 傳回 `false`時，就會完成伺服器資料流程呼叫。</span><span class="sxs-lookup"><span data-stu-id="d4902-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

<span data-ttu-id="d4902-155">如果您使用C# 8 或更新版本，則可以使用 `await foreach` 語法來讀取訊息。</span><span class="sxs-lookup"><span data-stu-id="d4902-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="d4902-156">`IAsyncStreamReader<T>.ReadAllAsync()` 擴充方法會讀取來自回應資料流程的所有訊息：</span><span class="sxs-lookup"><span data-stu-id="d4902-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="d4902-157">用戶端串流呼叫</span><span class="sxs-lookup"><span data-stu-id="d4902-157">Client streaming call</span></span>

<span data-ttu-id="d4902-158">用戶端串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。</span><span class="sxs-lookup"><span data-stu-id="d4902-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="d4902-159">用戶端可以選擇傳送具有 `RequestStream.WriteAsync`的訊息。</span><span class="sxs-lookup"><span data-stu-id="d4902-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="d4902-160">當用戶端完成傳送訊息時 `RequestStream.CompleteAsync` 應該呼叫來通知服務。</span><span class="sxs-lookup"><span data-stu-id="d4902-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="d4902-161">當服務傳迴響應消息時，就會完成呼叫。</span><span class="sxs-lookup"><span data-stu-id="d4902-161">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="d4902-162">雙向串流呼叫</span><span class="sxs-lookup"><span data-stu-id="d4902-162">Bi-directional streaming call</span></span>

<span data-ttu-id="d4902-163">雙向串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。</span><span class="sxs-lookup"><span data-stu-id="d4902-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="d4902-164">用戶端可以選擇傳送具有 `RequestStream.WriteAsync`的訊息。</span><span class="sxs-lookup"><span data-stu-id="d4902-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="d4902-165">從服務串流處理的訊息可以透過 `ResponseStream.MoveNext()` 或 `ResponseStream.ReadAllAsync()`來存取。</span><span class="sxs-lookup"><span data-stu-id="d4902-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="d4902-166">當 `ResponseStream` 沒有其他訊息時，雙向串流呼叫會完成。</span><span class="sxs-lookup"><span data-stu-id="d4902-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

<span data-ttu-id="d4902-167">在雙向串流呼叫期間，用戶端和服務可以隨時傳送訊息給彼此。</span><span class="sxs-lookup"><span data-stu-id="d4902-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="d4902-168">與雙向呼叫互動的最佳用戶端邏輯會根據服務邏輯而有所不同。</span><span class="sxs-lookup"><span data-stu-id="d4902-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4902-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="d4902-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
