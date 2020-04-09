---
title: 使用 .NET 用戶端呼叫 gRPC 服務
author: jamesnk
description: 瞭解如何使用 .NET gRPC 用戶端呼叫 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667171"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="c057a-103">使用 .NET 用戶端呼叫 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="c057a-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="c057a-104">.NET gRPC 用戶端庫在[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet 包中可用。</span><span class="sxs-lookup"><span data-stu-id="c057a-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="c057a-105">此文件說明如何:</span><span class="sxs-lookup"><span data-stu-id="c057a-105">This document explains how to:</span></span>

* <span data-ttu-id="c057a-106">配置 gRPC 用戶端以呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="c057a-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="c057a-107">對一元、伺服器流、用戶端流和雙向流式處理方法進行 gRPC 調用。</span><span class="sxs-lookup"><span data-stu-id="c057a-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="c057a-108">設定 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="c057a-108">Configure gRPC client</span></span>

<span data-ttu-id="c057a-109">gRPC 用戶端是從[*\*.proto*檔生成的](xref:grpc/basics#generated-c-assets)具體用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="c057a-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="c057a-110">具體的 gRPC 用戶端具有在*\*.proto*檔中轉換為 gRPC 服務的方法。</span><span class="sxs-lookup"><span data-stu-id="c057a-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="c057a-111">gRPC 用戶端是從通道創建的。</span><span class="sxs-lookup"><span data-stu-id="c057a-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="c057a-112">首先使用`GrpcChannel.ForAddress`建立通道,然後使用通道建立 gRPC 用戶端:</span><span class="sxs-lookup"><span data-stu-id="c057a-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="c057a-113">通道表示與 gRPC 服務的長時間連接。</span><span class="sxs-lookup"><span data-stu-id="c057a-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="c057a-114">創建通道時,將配置與調用服務相關的選項。</span><span class="sxs-lookup"><span data-stu-id="c057a-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="c057a-115">例如,`HttpClient`用於進行調用、最大發送和接收消息大小以及日誌記錄可以指定`GrpcChannelOptions`並`GrpcChannel.ForAddress`隨 使用。</span><span class="sxs-lookup"><span data-stu-id="c057a-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="c057a-116">有關選項的完整清單,請參閱[客戶端設定選項](xref:grpc/configuration#configure-client-options)。</span><span class="sxs-lookup"><span data-stu-id="c057a-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="c057a-117">通路和用戶端效能與使用方式:</span><span class="sxs-lookup"><span data-stu-id="c057a-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="c057a-118">創建通道可能是一項昂貴的操作。</span><span class="sxs-lookup"><span data-stu-id="c057a-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="c057a-119">對 gRPC 調用重用通道可提供性能優勢。</span><span class="sxs-lookup"><span data-stu-id="c057a-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="c057a-120">gRPC 用戶端是使用通道創建的。</span><span class="sxs-lookup"><span data-stu-id="c057a-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="c057a-121">gRPC 客戶端是輕量級物件,不需要緩存或重用。</span><span class="sxs-lookup"><span data-stu-id="c057a-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="c057a-122">可以從通道創建多個 gRPC 用戶端,包括不同類型的用戶端。</span><span class="sxs-lookup"><span data-stu-id="c057a-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="c057a-123">從通道創建的通道和用戶端可以安全地由多個線程使用。</span><span class="sxs-lookup"><span data-stu-id="c057a-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="c057a-124">從通道創建的用戶端可以同時進行多個調用。</span><span class="sxs-lookup"><span data-stu-id="c057a-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="c057a-125">`GrpcChannel.ForAddress`不是創建 gRPC 用戶端的唯一選項。</span><span class="sxs-lookup"><span data-stu-id="c057a-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="c057a-126">如果您從ASP.NET酷睿應用呼叫 gRPC 服務,請考慮[gRPC 客戶端工廠整合](xref:grpc/clientfactory)。</span><span class="sxs-lookup"><span data-stu-id="c057a-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="c057a-127">gRPC`HttpClientFactory`整合提供建立 gRPC 用戶端的集中替代方案。</span><span class="sxs-lookup"><span data-stu-id="c057a-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="c057a-128">使用[.NET 用戶端呼叫不安全的 gRPC 服務](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)需要額外的配置。</span><span class="sxs-lookup"><span data-stu-id="c057a-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="c057a-129">Xamarin 上目前不支援`Grpc.Net.Client`通過 HTTP/2 調用 gRPC。</span><span class="sxs-lookup"><span data-stu-id="c057a-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="c057a-130">我們正在努力在未來 Xamarin 版本中改進 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="c057a-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="c057a-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core)和[gRPC-Web](xref:grpc/browser)是當今可行的替代方案。</span><span class="sxs-lookup"><span data-stu-id="c057a-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="c057a-132">撥打 gRPC 呼叫</span><span class="sxs-lookup"><span data-stu-id="c057a-132">Make gRPC calls</span></span>

<span data-ttu-id="c057a-133">gRPC 調用通過在用戶端上調用方法來啟動。</span><span class="sxs-lookup"><span data-stu-id="c057a-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="c057a-134">gRPC 客戶端將處理消息序列化和處理 gRPC 對正確服務的調用。</span><span class="sxs-lookup"><span data-stu-id="c057a-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="c057a-135">gRPC 具有不同類型的方法。</span><span class="sxs-lookup"><span data-stu-id="c057a-135">gRPC has different types of methods.</span></span> <span data-ttu-id="c057a-136">如何使用用戶端進行 gRPC 調用取決於要調用的方法的類型。</span><span class="sxs-lookup"><span data-stu-id="c057a-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="c057a-137">gRPC 方法類型為:</span><span class="sxs-lookup"><span data-stu-id="c057a-137">The gRPC method types are:</span></span>

* <span data-ttu-id="c057a-138">一元 (Unary)</span><span class="sxs-lookup"><span data-stu-id="c057a-138">Unary</span></span>
* <span data-ttu-id="c057a-139">伺服器流程處理</span><span class="sxs-lookup"><span data-stu-id="c057a-139">Server streaming</span></span>
* <span data-ttu-id="c057a-140">用戶端流程式處理</span><span class="sxs-lookup"><span data-stu-id="c057a-140">Client streaming</span></span>
* <span data-ttu-id="c057a-141">雙向流式處理</span><span class="sxs-lookup"><span data-stu-id="c057a-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="c057a-142">一元撥打電話</span><span class="sxs-lookup"><span data-stu-id="c057a-142">Unary call</span></span>

<span data-ttu-id="c057a-143">一元呼叫從發送請求消息的客戶端開始。</span><span class="sxs-lookup"><span data-stu-id="c057a-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="c057a-144">服務完成後返回回應消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="c057a-145">.proto 檔中的每個一元服務方法將導致在具體 gRPC 客戶端類型上產生兩個 .NET 方法來調用該方法:非同步*\** 方法和阻塞方法。</span><span class="sxs-lookup"><span data-stu-id="c057a-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="c057a-146">例如,在`GreeterClient`有兩種調`SayHello`用 方法:</span><span class="sxs-lookup"><span data-stu-id="c057a-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="c057a-147">`GreeterClient.SayHelloAsync`-`Greeter.SayHello`非同步調用服務。</span><span class="sxs-lookup"><span data-stu-id="c057a-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="c057a-148">可以等待。</span><span class="sxs-lookup"><span data-stu-id="c057a-148">Can be awaited.</span></span>
* <span data-ttu-id="c057a-149">`GreeterClient.SayHello`-`Greeter.SayHello`調用服務和阻止,直到完成。</span><span class="sxs-lookup"><span data-stu-id="c057a-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="c057a-150">不要在異步代碼中使用。</span><span class="sxs-lookup"><span data-stu-id="c057a-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="c057a-151">伺服器流式呼叫</span><span class="sxs-lookup"><span data-stu-id="c057a-151">Server streaming call</span></span>

<span data-ttu-id="c057a-152">伺服器流式處理調用從發送請求消息的客戶端開始。</span><span class="sxs-lookup"><span data-stu-id="c057a-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="c057a-153">`ResponseStream.MoveNext()`讀取從服務流式傳輸的消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="c057a-154">返回`ResponseStream.MoveNext()`時 ,伺服器流`false`式處理 調用已完成。</span><span class="sxs-lookup"><span data-stu-id="c057a-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="c057a-155">如果使用 C# 8 或`await foreach`更高版本, 則語法可用於讀取消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="c057a-156">擴`IAsyncStreamReader<T>.ReadAllAsync()`充方法從回應串流讀取所有訊息:</span><span class="sxs-lookup"><span data-stu-id="c057a-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="c057a-157">用戶端串流式呼叫</span><span class="sxs-lookup"><span data-stu-id="c057a-157">Client streaming call</span></span>

<span data-ttu-id="c057a-158">用戶端流式處理調用啟動*而不*發送消息的用戶端。</span><span class="sxs-lookup"><span data-stu-id="c057a-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="c057a-159">用戶端可以選擇使用`RequestStream.WriteAsync`發送消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="c057a-160">當用戶端完成發送訊息時,`RequestStream.CompleteAsync`應呼叫以通知服務。</span><span class="sxs-lookup"><span data-stu-id="c057a-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="c057a-161">當服務返回回應消息時,呼叫已完成。</span><span class="sxs-lookup"><span data-stu-id="c057a-161">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="c057a-162">雙向流式處理呼叫</span><span class="sxs-lookup"><span data-stu-id="c057a-162">Bi-directional streaming call</span></span>

<span data-ttu-id="c057a-163">雙向流式處理調用啟動*而不*由客戶端發送消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="c057a-164">用戶端可以選擇使用`RequestStream.WriteAsync`發送消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="c057a-165">從服務流式傳輸的消息可通過`ResponseStream.MoveNext()``ResponseStream.ReadAllAsync()`或訪問。</span><span class="sxs-lookup"><span data-stu-id="c057a-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="c057a-166">當 沒有更多消息時`ResponseStream`, 雙向流式處理調用即完成。</span><span class="sxs-lookup"><span data-stu-id="c057a-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="c057a-167">在雙向流式處理呼叫期間,客戶端和服務可以隨時相互發送消息。</span><span class="sxs-lookup"><span data-stu-id="c057a-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="c057a-168">與雙向調用交互的最佳用戶端邏輯因服務邏輯而異。</span><span class="sxs-lookup"><span data-stu-id="c057a-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c057a-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="c057a-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
