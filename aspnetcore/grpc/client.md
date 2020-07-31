---
title: 利用 .NET 用戶端呼叫 gRPC 服務
author: jamesnk
description: 瞭解如何使用 .NET gRPC 用戶端呼叫 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/27/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/client
ms.openlocfilehash: 0d8856bba5afaaed4d9552480e4ae5dcbb7704d5
ms.sourcegitcommit: 5a36758cca2861aeb10840093e46d273a6e6e91d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303543"
---
# <a name="call-grpc-services-with-the-net-client"></a>利用 .NET 用戶端呼叫 gRPC 服務

.NET gRPC 用戶端程式庫可在[gRPC .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)NuGet 套件中取得。 本檔說明如何：

* 將 gRPC 用戶端設定為呼叫 gRPC 服務。
* 對一元、伺服器資料流程、用戶端資料流程和雙向串流方法進行 gRPC 呼叫。

## <a name="configure-grpc-client"></a>設定 gRPC 用戶端

gRPC 用戶端是[從* \* proto*檔案產生](xref:grpc/basics#generated-c-assets)的具體用戶端類型。 具體的 gRPC 用戶端具有轉譯為* \* proto*檔案中 gRPC 服務的方法。

GRPC 用戶端是從通道建立的。 首先，使用 `GrpcChannel.ForAddress` 來建立通道，然後使用通道來建立 gRPC 用戶端：

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

通道代表 gRPC 服務的長時間連接。 建立通道時，會使用與呼叫服務相關的選項進行設定。 例如， `HttpClient` 用來進行呼叫的、傳送和接收訊息大小上限，以及記錄可以在上指定並搭配 `GrpcChannelOptions` 使用 `GrpcChannel.ForAddress` 。 如需完整的選項清單，請參閱[用戶端設定選項](xref:grpc/configuration#configure-client-options)。

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

### <a name="configure-tls"></a>設定 TLS

GRPC 用戶端必須使用與所呼叫服務相同的連接層級安全性。 建立 gRPC 通道時，會設定 gRPC 用戶端傳輸層安全性（TLS）。 當 gRPC 用戶端呼叫服務，且通道和服務的連線層級安全性不相符時，就會擲回錯誤。

若要將 gRPC 通道設定為使用 TLS，請確定伺服器位址的開頭為 `https` 。 例如，會 `GrpcChannel.ForAddress("https://localhost:5001")` 使用 HTTPS 通訊協定。 GRPC 通道會自動 negotates TLS 所保護的連接，並使用安全連線來進行 gRPC 呼叫。

> [!TIP]
> gRPC 支援透過 TLS 的用戶端憑證驗證。 如需使用 gRPC 通道來設定用戶端憑證的詳細資訊，請參閱 <xref:grpc/authn-and-authz#client-certificate-authentication> 。

若要呼叫不安全的 gRPC 服務，請確定伺服器位址的開頭為 `http` 。 例如，會 `GrpcChannel.ForAddress("http://localhost:5000")` 使用 HTTP 通訊協定。 在 .NET Core 3.1 或更新版本中，需要額外的設定，才能[使用 .net 用戶端呼叫不安全的 gRPC 服務](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)。

### <a name="client-performance"></a>用戶端效能

通道和用戶端效能和使用方式：

* 建立通道可能是昂貴的作業。 重複使用 gRPC 呼叫的通道可提供效能優勢。
* 系統會使用通道來建立 gRPC 用戶端。 gRPC 用戶端是輕量物件，不需要快取或重複使用。
* 您可以從通道建立多個 gRPC 用戶端，包括不同類型的用戶端。
* 通道和從通道建立的用戶端可以安全地由多個執行緒使用。
* 從通道建立的用戶端可以進行多個同時呼叫。

`GrpcChannel.ForAddress`不是建立 gRPC 用戶端的唯一選項。 如果從 ASP.NET Core 應用程式呼叫 gRPC services，請考慮[gRPC 用戶端 factory 整合](xref:grpc/clientfactory)。 gRPC 與的整合 `HttpClientFactory` 提供了建立 gRPC 用戶端的集中式替代方案。

> [!NOTE]
> Xamarin 目前不支援透過 HTTP/2 呼叫 gRPC `Grpc.Net.Client` 。 我們正致力於改善未來 Xamarin 版本中的 HTTP/2 支援。 [Grpc](https://www.nuget.org/packages/Grpc.Core)和[Grpc-Web](xref:grpc/browser)是可行的替代方案。

## <a name="make-grpc-calls"></a>進行 gRPC 呼叫

GRPC 呼叫是藉由呼叫用戶端上的方法來起始。 GRPC 用戶端會處理訊息序列化，並將 gRPC 呼叫定址至正確的服務。

gRPC 有不同類型的方法。 如何使用用戶端進行 gRPC 呼叫，取決於呼叫的方法類型。 GRPC 方法類型為：

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

* \* Proto*檔案中的每個一元服務方法都會在具象 gRPC 用戶端類型上產生兩個 .net 方法，以呼叫方法：非同步方法和封鎖方法。 例如， `GreeterClient` 有兩種方法可以呼叫 `SayHello` ：

* `GreeterClient.SayHelloAsync`- `Greeter.SayHello` 以非同步方式呼叫服務。 可以等候。
* `GreeterClient.SayHello`-呼叫 `Greeter.SayHello` 服務並封鎖直到完成為止。 不要在非同步程式碼中使用。

### <a name="server-streaming-call"></a>伺服器串流呼叫

伺服器串流呼叫會從傳送要求訊息的用戶端開始。 `ResponseStream.MoveNext()`讀取從服務資料流程處理的訊息。 當傳回時，會完成伺服器資料流程呼叫 `ResponseStream.MoveNext()` `false` 。

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

while (await call.ResponseStream.MoveNext())
{
    Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
    // "Greeting: Hello World" is written multiple times
}
```

使用 c # 8 或更新版本時， `await foreach` 語法可以用來讀取訊息。 `IAsyncStreamReader<T>.ReadAllAsync()`擴充方法會讀取來自回應資料流程的所有訊息：

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}
```

### <a name="client-streaming-call"></a>用戶端串流呼叫

用戶端串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。 用戶端可以選擇用來傳送訊息 `RequestStream.WriteAsync` 。 當用戶端完成傳送訊息時， `RequestStream.CompleteAsync` 應該呼叫來通知服務。 當服務傳迴響應消息時，就會完成呼叫。

```csharp
var client = new Counter.CounterClient(channel);
using var call = client.AccumulateCount();

for (var i = 0; i < 3; i++)
{
    await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
}
await call.RequestStream.CompleteAsync();

var response = await call;
Console.WriteLine($"Count: {response.Count}");
// Count: 3
```

### <a name="bi-directional-streaming-call"></a>雙向串流呼叫

雙向串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。 用戶端可以選擇用來傳送訊息 `RequestStream.WriteAsync` 。 從服務串流處理的訊息可以透過 `ResponseStream.MoveNext()` 或來存取 `ResponseStream.ReadAllAsync()` 。 當沒有任何訊息時，雙向串流呼叫會完成 `ResponseStream` 。

```csharp
var client = new Echo.EchoClient(channel);
using var call = client.Echo();

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
```

在雙向串流呼叫期間，用戶端和服務可以隨時傳送訊息給彼此。 與雙向呼叫互動的最佳用戶端邏輯會根據服務邏輯而有所不同。

## <a name="access-grpc-trailers"></a>存取權 gRPC 尾端

gRPC 呼叫可能會傳回 gRPC 尾端。 gRPC 尾端用來提供關於呼叫的名稱/值中繼資料。 尾端提供類似于 HTTP 標頭的功能，但會在通話結束時收到。

gRPC 尾端可使用來存取 `GetTrailers()` ，這會傳回中繼資料的集合。 在回應完成後會傳回結尾，因此，您必須在存取結尾之前等待所有回應訊息。

在呼叫之前，一元和用戶端資料流程呼叫必須等候 `ResponseAsync` `GetTrailers()` ：

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
var response = await call.ResponseAsync;

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World

var trailers = call.GetTrailers();
var myValue = trailers.GetValue("my-trailer-name");
```

在呼叫之前，伺服器和雙向串流呼叫必須完成等待回應資料流程的作業 `GetTrailers()` ：

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}

var trailers = call.GetTrailers();
var myValue = trailers.GetValue("my-trailer-name");
```

gRPC 尾端也可以從存取 `RpcException` 。 服務可能會傳回尾端和非 OK gRPC 狀態。 在此情況下，會從 gRPC 用戶端擲回的例外狀況中抓取尾端：

```csharp
var client = new Greet.GreeterClient(channel);
string myValue = null;

try
{
    using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
    var response = await call.ResponseAsync;

    Console.WriteLine("Greeting: " + response.Message);
    // Greeting: Hello World

    var trailers = call.GetTrailers();
    myValue = trailers.GetValue("my-trailer-name");
}
catch (RpcException ex)
{
    var trailers = ex.Trailers;
    myValue = trailers.GetValue("my-trailer-name");
}
```

## <a name="additional-resources"></a>其他資源

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
