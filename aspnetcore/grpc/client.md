---
title: 利用 .NET 用戶端呼叫 gRPC 服務
author: jamesnk
description: 瞭解如何使用 .NET gRPC 用戶端呼叫 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 04/21/2020
uid: grpc/client
ms.openlocfilehash: aefa52a5c4c66178c5978aebd4cd9b00559c7f54
ms.sourcegitcommit: c9d1208e86160615b2d914cce74a839ae41297a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2020
ms.locfileid: "81791555"
---
# <a name="call-grpc-services-with-the-net-client"></a>利用 .NET 用戶端呼叫 gRPC 服務

.NET gRPC 用戶端庫在[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet 包中可用。 此文件說明如何:

* 配置 gRPC 用戶端以呼叫 gRPC 服務。
* 對一元、伺服器流、用戶端流和雙向流式處理方法進行 gRPC 調用。

## <a name="configure-grpc-client"></a>設定 gRPC 用戶端

gRPC 用戶端是從[*\*.proto*檔生成的](xref:grpc/basics#generated-c-assets)具體用戶端類型。 具體的 gRPC 用戶端具有在*\*.proto*檔中轉換為 gRPC 服務的方法。

gRPC 用戶端是從通道創建的。 首先使用`GrpcChannel.ForAddress`建立通道,然後使用通道建立 gRPC 用戶端:

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

通道表示與 gRPC 服務的長時間連接。 創建通道時,將配置與調用服務相關的選項。 例如,`HttpClient`用於進行調用、最大發送和接收消息大小以及日誌記錄可以指定`GrpcChannelOptions`並`GrpcChannel.ForAddress`隨 使用。 有關選項的完整清單,請參閱[客戶端設定選項](xref:grpc/configuration#configure-client-options)。

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

通路和用戶端效能與使用方式:

* 創建通道可能是一項昂貴的操作。 對 gRPC 調用重用通道可提供性能優勢。
* gRPC 用戶端是使用通道創建的。 gRPC 客戶端是輕量級物件,不需要緩存或重用。
* 可以從通道創建多個 gRPC 用戶端,包括不同類型的用戶端。
* 從通道創建的通道和用戶端可以安全地由多個線程使用。
* 從通道創建的用戶端可以同時進行多個調用。

`GrpcChannel.ForAddress`不是創建 gRPC 用戶端的唯一選項。 如果從ASP.NET酷睿應用呼叫 gRPC 服務,請考慮[gRPC 客戶端工廠整合](xref:grpc/clientfactory)。 gRPC`HttpClientFactory`整合提供建立 gRPC 用戶端的集中替代方案。

> [!NOTE]
> 使用[.NET 用戶端呼叫不安全的 gRPC 服務](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)需要額外的配置。

> [!NOTE]
> Xamarin 上目前不支援`Grpc.Net.Client`通過 HTTP/2 調用 gRPC。 我們正在努力在未來 Xamarin 版本中改進 HTTP/2 支援。 [Grpc.Core](https://www.nuget.org/packages/Grpc.Core)和[gRPC-Web](xref:grpc/browser)是當今可行的替代方案。

## <a name="make-grpc-calls"></a>撥打 gRPC 呼叫

gRPC 調用通過在用戶端上調用方法來啟動。 gRPC 客戶端將處理消息序列化和處理 gRPC 對正確服務的調用。

gRPC 具有不同類型的方法。 用戶端如何用於進行 gRPC 調用取決於調用的方法的類型。 gRPC 方法類型為:

* 一元 (Unary)
* 伺服器流程處理
* 用戶端流程式處理
* 雙向流式處理

### <a name="unary-call"></a>一元撥打電話

一元呼叫從發送請求消息的客戶端開始。 服務完成後返回回應消息。

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

.proto 檔中的每個一元服務方法將導致在具體 gRPC 客戶端類型上產生兩個 .NET 方法來調用該方法:非同步*\** 方法和阻塞方法。 例如,在`GreeterClient`有兩種調`SayHello`用 方法:

* `GreeterClient.SayHelloAsync`-`Greeter.SayHello`非同步調用服務。 可以等待。
* `GreeterClient.SayHello`-`Greeter.SayHello`調用服務和阻止,直到完成。 不要在異步代碼中使用。

### <a name="server-streaming-call"></a>伺服器流式呼叫

伺服器流式處理調用從發送請求消息的客戶端開始。 `ResponseStream.MoveNext()`讀取從服務流式傳輸的消息。 返回`ResponseStream.MoveNext()`時 ,伺服器流`false`式處理 調用已完成。

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

while (await call.ResponseStream.MoveNext())
{
    Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
    // "Greeting: Hello World" is written multiple times
}
```

使用 C# 8`await foreach`或更高 版本時,語法可用於讀取消息。 擴`IAsyncStreamReader<T>.ReadAllAsync()`充方法從回應串流讀取所有訊息:

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}
```

### <a name="client-streaming-call"></a>用戶端串流式呼叫

用戶端流式處理調用啟動*而不*發送消息的用戶端。 用戶端可以選擇使用`RequestStream.WriteAsync`發送消息。 當用戶端完成發送消息後,`RequestStream.CompleteAsync`應呼叫以通知服務。 當服務返回回應消息時,呼叫已完成。

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

### <a name="bi-directional-streaming-call"></a>雙向流式處理呼叫

雙向流式處理調用啟動*而不*由客戶端發送消息。 用戶端可以選擇使用`RequestStream.WriteAsync`發送消息。 從服務流式傳輸的消息可通過`ResponseStream.MoveNext()``ResponseStream.ReadAllAsync()`或訪問。 當 沒有更多消息時`ResponseStream`, 雙向流式處理調用即完成。

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

在雙向流式處理呼叫期間,客戶端和服務可以隨時相互發送消息。 與雙向調用交互的最佳用戶端邏輯因服務邏輯而異。

## <a name="access-grpc-trailers"></a>存取 gRPC 拖車

gRPC 呼叫可能會返回 gRPC 拖車。 gRPC 預告片用於提供有關呼叫的名稱/值元數據。 預告片提供與 HTTP 標頭類似的功能,但在呼叫結束時收到。

gRPC 預告片可以使用`GetTrailers()`返回 元數據集合。 在回應完成後返回拖車,因此,您必須等待所有回應消息才能訪問預告片。

一個客戶端流式呼叫必須先`ResponseAsync`等待,`GetTrailers()`然後才能呼叫 :

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
var response = await call.ResponseAsync;

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World

var trailers = call.GetTrailers();
var myValue = trailers.First(e => e.Key == "my-trailer-name");
```

伺服器和雙向串流式處理呼叫在呼`GetTrailers()`叫 之前必須完成等待回應流:

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}

var trailers = call.GetTrailers();
var myValue = trailers.First(e => e.Key == "my-trailer-name");
```

gRPC 拖車也可`RpcException`從 訪問。 服務可以返回拖車以及非 OK gRPC 狀態。 在此情況下,將從 gRPC 客戶端引發的異常中檢索拖車:

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
    myValue = trailers.First(e => e.Key == "my-trailer-name");
}
catch (RpcException ex)
{
    var trailers = ex.Trailers;
    myValue = trailers.First(e => e.Key == "my-trailer-name");
}
```

## <a name="additional-resources"></a>其他資源

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
