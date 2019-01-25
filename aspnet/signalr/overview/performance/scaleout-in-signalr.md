---
uid: signalr/overview/performance/scaleout-in-signalr
title: SignalR 的向外延展簡介 |Microsoft Docs
author: bradygaster
description: 軟體使用本主題的 Visual Studio 2013.NET 4.5 SignalR 本主題的第 2 版上一個版本的版本較早版本的相關資訊...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 60ae0353745284796eb7e0ddb6397ecb48eceaf0
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836195"
---
<a name="introduction-to-scaleout-in-signalr"></a>SignalR 的向外延展簡介
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR 第 2 版
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>本主題的上一個版本
>
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
>
> ## <a name="questions-and-comments"></a>提出問題或意見
>
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


一般情況下，有兩種方式可調整的 web 應用程式：*相應放大*並*相應放大*。

- 相應增加方法更大的伺服器 （或較大的 VM） 使用更多的 RAM、 Cpu、 等等。
- 相應放大表示新增更多伺服器來處理負載。

向上擴充問題是，您會快速達到機器的大小限制。 除此之外，您需要相應放大。不過，當您相應放大，則用戶端就可以會路由至不同的伺服器。 連線到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。

![](scaleout-in-signalr/_static/image1.png)

一個解決方案是使用呼叫元件的伺服器之間的訊息轉送給*後擋板*。 後擋板，已啟用，與每個應用程式執行個體傳送訊息至後擋板，並後擋板將它們轉送到其他應用程式執行個體。 （若電子業、 後擋板就是平行的連接器群組。 比方說，為 SignalR 後擋板連接多部伺服器。）

![](scaleout-in-signalr/_static/image2.png)

SignalR 目前提供三個的背板：

- **Azure 服務匯流排**。 服務匯流排是讓元件能夠以鬆散耦合的方式傳送訊息的訊息基礎結構。
- **Redis**。 Redis 是記憶體中索引鍵-值存放區。 Redis 支援發行/訂閱 ("pub/sub") 的模式來傳送訊息。
- **SQL Server**。 SQL Server 後擋板會將訊息寫入 SQL 資料表。 後擋板會使用 Service Broker 有效率之傳訊。 不過，它也適用於如果未啟用 Service Broker。

如果您部署在 Azure 上的應用程式，請考慮使用 Redis 後擋板[Azure Redis 快取](https://azure.microsoft.com/services/cache/)。 如果您要部署至伺服器陣列，請考慮 SQL Server 或 Redis 背板。

下列主題包含每個後擋板逐步教學的課程：

- [SignalR 向外延展與 Azure 服務匯流排](scaleout-with-windows-azure-service-bus.md)
- [SignalR 向外延展與 Redis](scaleout-with-redis.md)
- [SignalR 向外延展與 SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>實作

Signalr，每則訊息是透過訊息匯流排傳送。 訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象概念。 藉由取代預設的背板運作**IMessageBus**與專為該後擋板匯流排。 比方說，是適用於 Redis 訊息匯流排[RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)，並使用 Redis [pub/sub](http://redis.io/topics/pubsub)機制來傳送和接收訊息。

每個伺服器執行個體連線到透過匯流排後擋板。 當訊息傳送時，會先移至後擋板，並後擋板將它傳送至每一部伺服器。 當伺服器收到訊息時從後擋板時，它會將訊息放在其本機快取中。 伺服器再將訊息傳遞至用戶端從其本機快取。

每個用戶端連線，在讀取訊息資料流的用戶端的程序是使用資料指標來追蹤。 （資料指標表示訊息資料流中的位置）。如果用戶端中斷連線，並再重新連接時，它會要求用戶端的資料指標的值之後進入的任何訊息匯流排。 連線使用時，會發生相同的作業[長輪詢](../getting-started/introduction-to-signalr.md#transports)。 長時間輪詢要求完成之後，用戶端就會開啟新的連線，並要求提供已到達游標後的訊息。

資料指標機制的運作即使用戶端時，會路由至不同的伺服器上，在重新連線。 後擋板所知的所有伺服器，並不重要的用戶端連接至的伺服器。

## <a name="limitations"></a>限制

使用後擋板，最大訊息輸送量低於時用戶端直接與單一伺服器節點。 這是因為後擋板轉送至每個節點中，每個訊息，讓後擋板成為瓶頸。 這項限制是否發生問題，則應用程式而定。 例如，以下是一些典型的 SignalR 案例：

- [伺服器廣播](../getting-started/tutorial-server-broadcast-with-signalr.md)（例如，股票行情指示器）：背板適用於此案例中，因為伺服器控制傳送訊息的速率。
- [用戶端到用戶端](../getting-started/tutorial-getting-started-with-signalr.md)（例如聊天）：在此案例中後, 擋板如果可能會發生瓶頸的訊息數目隨著; 的用戶端數目也就是說，如果訊息的速率成長按比例越多的用戶端加入。
- [高頻率即時](../getting-started/tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）：後擋板不建議此案例中。

## <a name="enabling-tracing-for-signalr-scaleout"></a>啟用 SignalR 向外延展的追蹤

若要啟用追蹤的背板，請將下列各節新增至 web.config 檔案中的根目錄下**組態**項目：

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
