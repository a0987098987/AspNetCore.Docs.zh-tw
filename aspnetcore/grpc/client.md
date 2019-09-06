---
title: 使用 .NET 用戶端呼叫 gRPC 服務
author: jamesnk
description: 瞭解如何使用 .NET gRPC 用戶端呼叫 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 5518a221c4641ba0d1da051a14750e3165944455
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310671"
---
# <a name="call-grpc-services-with-the-net-client"></a>使用 .NET 用戶端呼叫 gRPC 服務

.NET gRPC 用戶端程式庫可在[gRPC .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)NuGet 套件中取得。 本檔說明如何：

* 將 gRPC 用戶端設定為呼叫 gRPC 服務。
* 對一元、伺服器資料流程、用戶端資料流程和雙向串流方法進行 gRPC 呼叫。

## <a name="configure-grpc-client"></a>設定 gRPC 用戶端

gRPC 用戶端是[從 *\*proto*檔案產生](xref:grpc/basics#generated-c-assets)的具體用戶端類型。 具體的 gRPC 用戶端具有轉譯為 *\*proto*檔案中 gRPC 服務的方法。

GRPC 用戶端是從通道建立的。 首先，使用`GrpcChannel.ForAddress`來建立通道，然後使用通道來建立 gRPC 用戶端：

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

通道代表 gRPC 服務的長時間連接。 建立通道時，會使用與呼叫服務相關的選項進行設定。 例如， `HttpClient`用來進行呼叫的、傳送和接收訊息大小上限，以及記錄可以在上`GrpcChannelOptions`指定並搭配使用`GrpcChannel.ForAddress`。 如需完整的選項清單，請參閱[用戶端設定選項](xref:grpc/configuration#configure-client-options)。

建立通道可能是昂貴的作業，且重複使用通道進行 gRPC 呼叫可提供效能優勢。 可以從通道建立多個具體的 gRPC 用戶端，包括不同類型的用戶端。 具體的 gRPC 用戶端類型是輕量物件，而且可以在需要時建立。

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

`GrpcChannel.ForAddress`不是建立 gRPC 用戶端的唯一選項。 如果您是從 ASP.NET Core 應用程式呼叫 gRPC services，請考慮[gRPC 用戶端工廠整合](xref:grpc/clientfactory)。 gRPC 與的`HttpClientFactory`整合提供了建立 gRPC 用戶端的集中式替代方案。

## <a name="make-grpc-calls"></a>進行 gRPC 呼叫

GRPC 呼叫是藉由呼叫用戶端上的方法來起始。 GRPC 用戶端會處理訊息序列化，並將 gRPC 呼叫定址至正確的服務。

gRPC 有不同類型的方法。 您使用用戶端進行 gRPC 呼叫的方式，取決於您所呼叫的方法類型。 GRPC 方法類型為：

* 一元
* 伺服器串流
* 用戶端串流
* 雙向串流

### <a name="unary-call"></a>一元呼叫

一元呼叫會從傳送要求訊息的用戶端開始。 當服務完成時，就會傳迴響應消息。

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

*\*Proto*檔案中的每個一元服務方法都會在具象 gRPC 用戶端類型上產生兩個 .net 方法，以呼叫方法：非同步方法和封鎖方法。 例如， `GreeterClient`有兩種方法可以呼叫`SayHello`：

* `GreeterClient.SayHelloAsync`-以`Greeter.SayHello`非同步方式呼叫服務。 可以等候。
* `GreeterClient.SayHello`-呼叫`Greeter.SayHello`服務並封鎖直到完成為止。 不要在非同步程式碼中使用。

### <a name="server-streaming-call"></a>伺服器串流呼叫

伺服器串流呼叫會從傳送要求訊息的用戶端開始。 `ResponseStream.MoveNext()`讀取從服務資料流程處理的訊息。 當傳回時`ResponseStream.MoveNext()` `false`，會完成伺服器資料流程呼叫。

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // Greeting: Hello World" is streamed multiple times
    }
}
```

如果您使用C# 8 或更新版本`await foreach` ，則可以使用語法來讀取訊息。 `IAsyncStreamReader<T>.ReadAllAsync()`擴充方法會讀取來自回應資料流程的所有訊息：

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is streamed multiple times
    }
}
```

### <a name="client-streaming-call"></a>用戶端串流呼叫

用戶端串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。 用戶端可以選擇使用`RequestStream.WriteAsync`傳送訊息。 當用戶端完成傳送訊息`RequestStream.CompleteAsync`時，應該呼叫以通知服務。 當服務傳迴響應消息時，就會完成呼叫。

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

### <a name="bi-directional-streaming-call"></a>雙向串流呼叫

雙向串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。 用戶端可以選擇用`RequestStream.WriteAsync`來傳送訊息。 從服務串流處理`ResponseStream.MoveNext()`的訊息可以透過或`ResponseStream.ReadAllAsync()`來存取。 當沒有任何訊息時`ResponseStream` ，雙向串流呼叫會完成。

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

在雙向串流呼叫期間，用戶端和服務可以隨時傳送訊息給彼此。 與雙向呼叫互動的最佳用戶端邏輯會根據服務邏輯而有所不同。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
