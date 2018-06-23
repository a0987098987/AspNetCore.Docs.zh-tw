---
title: 使用資料流中 ASP.NET Core SignalR
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 08ddea4fb83150bab27a9e2685c75ff34565606b
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327489"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>使用資料流中 ASP.NET Core SignalR

由[Brennan 瑜吉](https://github.com/BrennanConroy)

ASP.NET Core SignalR 支援串流伺服器方法傳回值。 這是適用於其中的資料片段會有一段時間的案例。 時傳回的值串流至用戶端，每個片段會傳送至用戶端時它會變成立即可用，而非等待變成可用的所有資料。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>設定中樞

中樞方法會自動變成資料流處理的中樞方法，當它傳回`ChannelReader<T>`或`Task<ChannelReader<T>>`。 以下是示範串流到用戶端資料的基本概念的範例。 寫入物件是每當`ChannelReader`該物件會立即傳送至用戶端。 在結束時，`ChannelReader`完成告訴用戶端的資料流已關閉。

> [!NOTE]
> 寫入`ChannelReader`在背景執行緒，然後傳回`ChannelReader`儘速。 其他中樞引動過程將會封鎖直到`ChannelReader`傳回。

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>.NET 用戶端

`StreamAsChannelAsync`方法`HubConnection`用來叫用的資料流的方法。 將中樞方法的名稱和中樞方法中定義的引數傳遞`StreamAsChannelAsync`。 上的泛型參數`StreamAsChannelAsync<T>`指定的資料流的方法所傳回的物件類型。 A`ChannelReader<T>`會傳回從資料流引動過程中，並代表用戶端上的資料流。 若要讀取資料，常見的模式是迴圈時`WaitToReadAsync`並呼叫`TryRead`資料時使用。 在伺服器上，資料流已關閉或取消語彙基元傳遞至時，迴圈將結束`StreamAsChannelAsync`已取消。

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a>JavaScript 用戶端

JavaScript 用戶端呼叫中樞上的資料流的方法，使用`connection.stream`。 `stream`方法接受兩個引數：

* 中樞方法的名稱。 下列範例中，在中樞方法的名稱是`Counter`。
* 定義在中樞方法的引數。 在下列範例中，引數為： 接收的資料流項目和資料流的項目之間的延遲數的計數。

`connection.stream` 傳回`IStreamResult`包含`subscribe`方法。 傳遞`IStreamSubscriber`至`subscribe`並設定`next`， `error`，和`complete`回呼，以取得通知從`stream`引動過程。

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

若要結束用戶端呼叫從資料流`dispose`方法`ISubscription`從傳回`subscribe`方法。

## <a name="related-resources"></a>相關資源

* [中樞](xref:signalr/hubs)
* [.NET 用戶端](xref:signalr/dotnet-client)
* [JavaScript 用戶端](xref:signalr/javascript-client)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
