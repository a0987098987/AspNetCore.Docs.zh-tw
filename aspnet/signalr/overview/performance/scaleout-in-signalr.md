---
uid: signalr/overview/performance/scaleout-in-signalr
title: "向外延展 SignalR 中簡介 |Microsoft 文件"
author: MikeWasson
description: "軟體版本本主題中使用 Visual Studio 2013.NET 4.5 SignalR 第 2 版舊版的此主題的較早版本的相關資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-scaleout-in-signalr"></a>向外延展 SignalR 中簡介
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


一般情況下，有兩種方式來調整 web 應用程式：*向上延展*和*向外延展*。

- 向上擴充更大的伺服器 （或更大的 VM） 使用更多 RAM、 Cpu 和其他內容的方式。
- 擴充方法新增更多伺服器來處理負載。

向上擴充問題是您快速地叫用的機器大小的限制。 此外，您需要向外延展。不過，當您向外延展，用戶端可以取得路由傳送至不同的伺服器。 連接到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。

![](scaleout-in-signalr/_static/image1.png)

其中一個解決方案是使用名為元件的伺服器之間的訊息轉送*後擋板*。 後擋板，已啟用，與每個應用程式執行個體將訊息傳送至後擋板，並後擋板，已轉送至其他應用程式執行個體。 （若電子產品中，在後擋板就是一組平行的連接器。 比方說，由 SignalR 後擋板連接多部伺服器。）

![](scaleout-in-signalr/_static/image2.png)

SignalR 目前提供三個的背板：

- **Azure 服務匯流排**。 Service Bus 是讓元件能夠以鬆散偶合的方式傳送訊息的訊息基礎架構。
- **Redis**。 Redis 是記憶體中索引鍵-值存放區。 Redis 支援發行/訂閱 (「 pub/sub") 的模式來傳送訊息。
- **SQL Server**。 SQL Server 後擋板訊息寫入 SQL 資料表。 後擋板有效率的通訊使用 Service Broker。 不過，它也適用於未啟用 Service Broker。

如果您部署在 Azure 上的應用程式時，請考慮使用 Redis 後擋板，已使用[Azure Redis 快取](https://azure.microsoft.com/services/cache/)。 如果您要部署至伺服器陣列，請考慮 SQL Server 或 Redis 背板。

下列主題包含每個後擋板逐步教學的課程：

- [SignalR 向外延展與 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)
- [SignalR 向外延展與 Redis](scaleout-with-redis.md)
- [SignalR 向外延展與 SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>實作

SignalR，每個訊息傳送至訊息匯流排。 訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象的。 運作方式是取代預設的背板**IMessageBus**與針對該後擋板匯流排。 比方說，是 redis 訊息匯流排[RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)，並使用 Redis [pub/sub](http://redis.io/topics/pubsub)機制來傳送和接收訊息。

每個伺服器執行個體連接到後擋板，已透過匯流排。 當訊息傳送時，它會移至後擋板，並後擋板，已將它傳送至每一部伺服器。 當伺服器收到訊息時從後擋板時，它會將訊息放在其本機快取中。 伺服器再將訊息傳遞至用戶端從其本機快取。

每個用戶端連線，讀取訊息資料流的用戶端的程序是使用資料指標來追蹤。 （資料指標表示訊息資料流中的位置）。如果用戶端中斷連接，然後再重新連接，它會要求匯流排已到達用戶端資料指標的值之後的任何訊息。 連線使用時，會發生相同的作業[長輪詢](../getting-started/introduction-to-signalr.md#transports)。 長時間輪詢要求完成之後，用戶端就會開啟新的連線，並要求到達游標後的訊息。

資料指標機制的運作即使在路由上的用戶端傳送到不同的伺服器重新連接。 後擋板可感知的所有伺服器，而且用戶端連接至哪些伺服器沒有任何影響。

## <a name="limitations"></a>限制

使用後擋板，最大訊息輸送量會比單一伺服器節點直接向用戶端時。 這是因為後擋板轉送至每個節點，每個訊息，因此後擋板成為瓶頸。 這項限制是否發生問題，取決於應用程式。 例如，以下是一些典型的 SignalR 案例：

- [伺服器廣播](../getting-started/tutorial-server-broadcast-with-signalr.md)（例如，股票行情指示器）： 背板很適合此案例中，因為伺服器控制傳送訊息的速率。
- [用戶端](../getting-started/tutorial-getting-started-with-signalr.md)（例如聊天）： 在此案例中後, 擋板可能是效能瓶頸如果用戶端數目的訊息數目縮放比例; 也就是說，如果訊息的速率增加按比例越多的用戶端加入。
- [高頻率即時](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）： 後擋板建議您不要在此案例。

## <a name="enabling-tracing-for-signalr-scaleout"></a>啟用 SignalR 範圍外的追蹤

若要啟用追蹤的背板，加入下列各節 web.config 檔案中，根目錄下**組態**項目：

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
