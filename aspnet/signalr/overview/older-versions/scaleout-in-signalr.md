---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: SignalR 的向外延展簡介 1.x |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: a492eabfb5a74472b44095f24704028f6cd8a12a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375217"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a><span data-ttu-id="99a3d-102">SignalR 的向外延展簡介 1.x</span><span class="sxs-lookup"><span data-stu-id="99a3d-102">Introduction to Scaleout in SignalR 1.x</span></span>
====================
<span data-ttu-id="99a3d-103">藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="99a3d-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="99a3d-104">一般情況下，有兩種方式可調整的 web 應用程式：*相應放大*並*相應放大*。</span><span class="sxs-lookup"><span data-stu-id="99a3d-104">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="99a3d-105">相應增加方法更大的伺服器 （或較大的 VM） 使用更多的 RAM、 Cpu、 等等。</span><span class="sxs-lookup"><span data-stu-id="99a3d-105">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="99a3d-106">相應放大表示新增更多伺服器來處理負載。</span><span class="sxs-lookup"><span data-stu-id="99a3d-106">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="99a3d-107">向上擴充問題是，您會快速達到機器的大小限制。</span><span class="sxs-lookup"><span data-stu-id="99a3d-107">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="99a3d-108">除此之外，您需要相應放大。不過，當您相應放大，則用戶端就可以會路由至不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="99a3d-108">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="99a3d-109">連線到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="99a3d-109">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="99a3d-110">一個解決方案是使用呼叫元件的伺服器之間的訊息轉送給*後擋板*。</span><span class="sxs-lookup"><span data-stu-id="99a3d-110">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="99a3d-111">後擋板，已啟用，與每個應用程式執行個體傳送訊息至後擋板，並後擋板將它們轉送到其他應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="99a3d-111">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="99a3d-112">（若電子業、 後擋板就是平行的連接器群組。</span><span class="sxs-lookup"><span data-stu-id="99a3d-112">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="99a3d-113">比方說，為 SignalR 後擋板連接多部伺服器。）</span><span class="sxs-lookup"><span data-stu-id="99a3d-113">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="99a3d-114">SignalR 目前提供三個的背板：</span><span class="sxs-lookup"><span data-stu-id="99a3d-114">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="99a3d-115">**Azure 服務匯流排**。</span><span class="sxs-lookup"><span data-stu-id="99a3d-115">**Azure Service Bus**.</span></span> <span data-ttu-id="99a3d-116">服務匯流排是讓元件能夠以鬆散耦合的方式傳送訊息的訊息基礎結構。</span><span class="sxs-lookup"><span data-stu-id="99a3d-116">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="99a3d-117">**Redis**。</span><span class="sxs-lookup"><span data-stu-id="99a3d-117">**Redis**.</span></span> <span data-ttu-id="99a3d-118">Redis 是記憶體中索引鍵-值存放區。</span><span class="sxs-lookup"><span data-stu-id="99a3d-118">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="99a3d-119">Redis 支援發行/訂閱 ("pub/sub") 的模式來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="99a3d-119">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="99a3d-120">**SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="99a3d-120">**SQL Server**.</span></span> <span data-ttu-id="99a3d-121">SQL Server 後擋板會將訊息寫入 SQL 資料表。</span><span class="sxs-lookup"><span data-stu-id="99a3d-121">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="99a3d-122">後擋板會使用 Service Broker 有效率之傳訊。</span><span class="sxs-lookup"><span data-stu-id="99a3d-122">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="99a3d-123">不過，它也適用於如果未啟用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="99a3d-123">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="99a3d-124">如果您部署在 Azure 上的應用程式，請考慮使用 Azure 服務匯流排後擋板。</span><span class="sxs-lookup"><span data-stu-id="99a3d-124">If you deploy your application on Azure, consider using the Azure Service Bus backplane.</span></span> <span data-ttu-id="99a3d-125">如果您要部署至伺服器陣列，請考慮 SQL Server 或 Redis 背板。</span><span class="sxs-lookup"><span data-stu-id="99a3d-125">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="99a3d-126">下列主題包含每個後擋板逐步教學的課程：</span><span class="sxs-lookup"><span data-stu-id="99a3d-126">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="99a3d-127">SignalR 向外延展與 Azure 服務匯流排</span><span class="sxs-lookup"><span data-stu-id="99a3d-127">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="99a3d-128">SignalR 向外延展與 Redis</span><span class="sxs-lookup"><span data-stu-id="99a3d-128">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="99a3d-129">SignalR 向外延展與 SQL Server</span><span class="sxs-lookup"><span data-stu-id="99a3d-129">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="99a3d-130">實作</span><span class="sxs-lookup"><span data-stu-id="99a3d-130">Implementation</span></span>

<span data-ttu-id="99a3d-131">Signalr，每則訊息是透過訊息匯流排傳送。</span><span class="sxs-lookup"><span data-stu-id="99a3d-131">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="99a3d-132">訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象概念。</span><span class="sxs-lookup"><span data-stu-id="99a3d-132">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="99a3d-133">藉由取代預設的背板運作**IMessageBus**與專為該後擋板匯流排。</span><span class="sxs-lookup"><span data-stu-id="99a3d-133">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="99a3d-134">比方說，是適用於 Redis 訊息匯流排[RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)，並使用 Redis [pub/sub](http://redis.io/topics/pubsub)機制來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="99a3d-134">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="99a3d-135">每個伺服器執行個體連線到透過匯流排後擋板。</span><span class="sxs-lookup"><span data-stu-id="99a3d-135">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="99a3d-136">當訊息傳送時，會先移至後擋板，並後擋板將它傳送至每一部伺服器。</span><span class="sxs-lookup"><span data-stu-id="99a3d-136">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="99a3d-137">當伺服器收到訊息時從後擋板時，它會將訊息放在其本機快取中。</span><span class="sxs-lookup"><span data-stu-id="99a3d-137">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="99a3d-138">伺服器再將訊息傳遞至用戶端從其本機快取。</span><span class="sxs-lookup"><span data-stu-id="99a3d-138">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="99a3d-139">每個用戶端連線，在讀取訊息資料流的用戶端的程序是使用資料指標來追蹤。</span><span class="sxs-lookup"><span data-stu-id="99a3d-139">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="99a3d-140">（資料指標表示訊息資料流中的位置）。如果用戶端中斷連線，並再重新連接時，它會要求用戶端的資料指標的值之後進入的任何訊息匯流排。</span><span class="sxs-lookup"><span data-stu-id="99a3d-140">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="99a3d-141">連線使用時，會發生相同的作業[長輪詢](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="99a3d-141">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="99a3d-142">長時間輪詢要求完成之後，用戶端就會開啟新的連線，並要求提供已到達游標後的訊息。</span><span class="sxs-lookup"><span data-stu-id="99a3d-142">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="99a3d-143">資料指標機制的運作即使用戶端時，會路由至不同的伺服器上，在重新連線。</span><span class="sxs-lookup"><span data-stu-id="99a3d-143">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="99a3d-144">後擋板所知的所有伺服器，並不重要的用戶端連接至的伺服器。</span><span class="sxs-lookup"><span data-stu-id="99a3d-144">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="99a3d-145">限制</span><span class="sxs-lookup"><span data-stu-id="99a3d-145">Limitations</span></span>

<span data-ttu-id="99a3d-146">使用後擋板，最大訊息輸送量低於時用戶端直接與單一伺服器節點。</span><span class="sxs-lookup"><span data-stu-id="99a3d-146">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="99a3d-147">這是因為後擋板轉送至每個節點中，每個訊息，讓後擋板成為瓶頸。</span><span class="sxs-lookup"><span data-stu-id="99a3d-147">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="99a3d-148">這項限制是否發生問題，則應用程式而定。</span><span class="sxs-lookup"><span data-stu-id="99a3d-148">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="99a3d-149">例如，以下是一些典型的 SignalR 案例：</span><span class="sxs-lookup"><span data-stu-id="99a3d-149">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="99a3d-150">[伺服器廣播](tutorial-server-broadcast-with-aspnet-signalr.md)（例如，股票行情指示器）： 背板適用於此案例中，因為伺服器控制傳送訊息的速率。</span><span class="sxs-lookup"><span data-stu-id="99a3d-150">[Server broadcast](tutorial-server-broadcast-with-aspnet-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="99a3d-151">[用戶端到用戶端](tutorial-getting-started-with-signalr.md)（例如聊天）： 在此案例中後, 擋板瓶頸的訊息數目隨著用戶端數目; 也就是說，如果訊息的速率成長按比例越多的用戶端加入。</span><span class="sxs-lookup"><span data-stu-id="99a3d-151">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="99a3d-152">[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）： 這種情況下不建議後擋板。</span><span class="sxs-lookup"><span data-stu-id="99a3d-152">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="99a3d-153">啟用 SignalR 向外延展的追蹤</span><span class="sxs-lookup"><span data-stu-id="99a3d-153">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="99a3d-154">若要啟用追蹤的背板，請將下列各節新增至 web.config 檔案中的根目錄下**組態**項目：</span><span class="sxs-lookup"><span data-stu-id="99a3d-154">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
