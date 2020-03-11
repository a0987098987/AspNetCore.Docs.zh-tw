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
# <a name="call-grpc-services-with-the-net-client"></a>使用 .NET 用戶端呼叫 gRPC 服務

.NET gRPC 用戶端程式庫可在[gRPC .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)NuGet 套件中取得。 本檔說明如何：

* 將 gRPC 用戶端設定為呼叫 gRPC 服務。
* 對一元、伺服器資料流程、用戶端資料流程和雙向串流方法進行 gRPC 呼叫。

## <a name="configure-grpc-client"></a>設定 gRPC 用戶端

gRPC 用戶端是[從 *\*的 proto*檔案產生](xref:grpc/basics#generated-c-assets)的具體用戶端類型。 具體 gRPC 用戶端的方法會轉譯為 *\*的 proto*檔案中的 gRPC 服務。

GRPC 用戶端是從通道建立的。 首先，使用 `GrpcChannel.ForAddress` 建立通道，然後使用通道來建立 gRPC 用戶端：

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

通道代表 gRPC 服務的長時間連接。 建立通道時，會使用與呼叫服務相關的選項進行設定。 例如，用來進行呼叫的 `HttpClient`、傳送和接收訊息大小上限，以及記錄可以在 `GrpcChannelOptions` 上指定，並與 `GrpcChannel.ForAddress`搭配使用。 如需完整的選項清單，請參閱[用戶端設定選項](xref:grpc/configuration#configure-client-options)。

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

通道和用戶端效能和使用方式：

* 建立通道可能是昂貴的作業。 重複使用 gRPC 呼叫的通道可提供效能優勢。
* 系統會使用通道來建立 gRPC 用戶端。 gRPC 用戶端是輕量物件，不需要快取或重複使用。
* 您可以從通道建立多個 gRPC 用戶端，包括不同類型的用戶端。
* 通道和從通道建立的用戶端可以安全地由多個執行緒使用。
* 從通道建立的用戶端可以進行多個同時呼叫。

`GrpcChannel.ForAddress` 不是建立 gRPC 用戶端的唯一選項。 如果您是從 ASP.NET Core 應用程式呼叫 gRPC services，請考慮[gRPC 用戶端工廠整合](xref:grpc/clientfactory)。 gRPC 與 `HttpClientFactory` 整合提供了建立 gRPC 用戶端的集中式替代方案。

> [!NOTE]
> 需要其他設定，才能[使用 .net 用戶端呼叫不安全的 gRPC 服務](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)。

> [!NOTE]
> Xamarin 目前不支援透過 HTTP/2 使用 `Grpc.Net.Client` 呼叫 gRPC。 我們正致力於改善未來 Xamarin 版本中的 HTTP/2 支援。 [Grpc](https://www.nuget.org/packages/Grpc.Core)和[Grpc-Web](xref:grpc/browser)是可行的替代方案。

## <a name="make-grpc-calls"></a>進行 gRPC 呼叫

GRPC 呼叫是藉由呼叫用戶端上的方法來起始。 GRPC 用戶端會處理訊息序列化，並將 gRPC 呼叫定址至正確的服務。

gRPC 有不同類型的方法。 您使用用戶端進行 gRPC 呼叫的方式，取決於您所呼叫的方法類型。 GRPC 方法類型為：

* 一元 (Unary)
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

*\** 中的每個一元服務方法都會在具體 gRPC 用戶端類型上產生兩個 .net 方法，以呼叫方法：非同步方法和封鎖方法。 例如，在 `GreeterClient` 上，有兩種方式可呼叫 `SayHello`：

* `GreeterClient.SayHelloAsync`-以非同步方式呼叫 `Greeter.SayHello` 服務。 可以等候。
* `GreeterClient.SayHello`-呼叫 `Greeter.SayHello` 服務並封鎖直到完成為止。 不要在非同步程式碼中使用。

### <a name="server-streaming-call"></a>伺服器串流呼叫

伺服器串流呼叫會從傳送要求訊息的用戶端開始。 `ResponseStream.MoveNext()` 讀取從服務資料流程處理的訊息。 當 `ResponseStream.MoveNext()` 傳回 `false`時，就會完成伺服器資料流程呼叫。

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

如果您使用C# 8 或更新版本，則可以使用 `await foreach` 語法來讀取訊息。 `IAsyncStreamReader<T>.ReadAllAsync()` 擴充方法會讀取來自回應資料流程的所有訊息：

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

### <a name="client-streaming-call"></a>用戶端串流呼叫

用戶端串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。 用戶端可以選擇傳送具有 `RequestStream.WriteAsync`的訊息。 當用戶端完成傳送訊息時 `RequestStream.CompleteAsync` 應該呼叫來通知服務。 當服務傳迴響應消息時，就會完成呼叫。

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

雙向串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。 用戶端可以選擇傳送具有 `RequestStream.WriteAsync`的訊息。 從服務串流處理的訊息可以透過 `ResponseStream.MoveNext()` 或 `ResponseStream.ReadAllAsync()`來存取。 當 `ResponseStream` 沒有其他訊息時，雙向串流呼叫會完成。

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
