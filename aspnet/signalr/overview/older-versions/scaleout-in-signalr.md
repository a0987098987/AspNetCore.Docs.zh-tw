---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: "向外延展 SignalR 中簡介 1.x |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: e6230d4d65adb8c9a064545ad761898ca53562bf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-scaleout-in-signalr-1x"></a><span data-ttu-id="5949e-102">向外延展 SignalR 中簡介 1.x</span><span class="sxs-lookup"><span data-stu-id="5949e-102">Introduction to Scaleout in SignalR 1.x</span></span>
====================
<span data-ttu-id="5949e-103">由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5949e-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="5949e-104">一般情況下，有兩種方式來調整 web 應用程式：*向上延展*和*向外延展*。</span><span class="sxs-lookup"><span data-stu-id="5949e-104">In general, there are two ways to scale a web application: *scale up* and *scale out*.</span></span>

- <span data-ttu-id="5949e-105">向上擴充更大的伺服器 （或更大的 VM） 使用更多 RAM、 Cpu 和其他內容的方式。</span><span class="sxs-lookup"><span data-stu-id="5949e-105">Scale up means using a larger server (or a larger VM) with more RAM, CPUs, etc.</span></span>
- <span data-ttu-id="5949e-106">擴充方法新增更多伺服器來處理負載。</span><span class="sxs-lookup"><span data-stu-id="5949e-106">Scale out means adding more servers to handle the load.</span></span>

<span data-ttu-id="5949e-107">向上擴充問題是您快速地叫用的機器大小的限制。</span><span class="sxs-lookup"><span data-stu-id="5949e-107">The problem with scaling up is that you quickly hit a limit on the size of the machine.</span></span> <span data-ttu-id="5949e-108">此外，您需要向外延展。不過，當您向外延展，用戶端可以取得路由傳送至不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5949e-108">Beyond that, you need to scale out. However, when you scale out, clients can get routed to different servers.</span></span> <span data-ttu-id="5949e-109">連接到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="5949e-109">A client that is connected to one server will not receive messages sent from another server.</span></span>

![](scaleout-in-signalr/_static/image1.png)

<span data-ttu-id="5949e-110">其中一個解決方案是使用名為元件的伺服器之間的訊息轉送*後擋板*。</span><span class="sxs-lookup"><span data-stu-id="5949e-110">One solution is to forward messages between servers, using a component called a *backplane*.</span></span> <span data-ttu-id="5949e-111">後擋板，已啟用，與每個應用程式執行個體將訊息傳送至後擋板，並後擋板，已轉送至其他應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="5949e-111">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span> <span data-ttu-id="5949e-112">（若電子產品中，在後擋板就是一組平行的連接器。</span><span class="sxs-lookup"><span data-stu-id="5949e-112">(In electronics, a backplane is a group of parallel connectors.</span></span> <span data-ttu-id="5949e-113">比方說，由 SignalR 後擋板連接多部伺服器。）</span><span class="sxs-lookup"><span data-stu-id="5949e-113">By analogy, a SignalR backplane connects multiple servers.)</span></span>

![](scaleout-in-signalr/_static/image2.png)

<span data-ttu-id="5949e-114">SignalR 目前提供三個的背板：</span><span class="sxs-lookup"><span data-stu-id="5949e-114">SignalR currently provides three backplanes:</span></span>

- <span data-ttu-id="5949e-115">**Azure 服務匯流排**。</span><span class="sxs-lookup"><span data-stu-id="5949e-115">**Azure Service Bus**.</span></span> <span data-ttu-id="5949e-116">Service Bus 是讓元件能夠以鬆散偶合的方式傳送訊息的訊息基礎架構。</span><span class="sxs-lookup"><span data-stu-id="5949e-116">Service Bus is a messaging infrastructure that allows components to send messages in a loosely coupled way.</span></span>
- <span data-ttu-id="5949e-117">**Redis**。</span><span class="sxs-lookup"><span data-stu-id="5949e-117">**Redis**.</span></span> <span data-ttu-id="5949e-118">Redis 是記憶體中索引鍵-值存放區。</span><span class="sxs-lookup"><span data-stu-id="5949e-118">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="5949e-119">Redis 支援發行/訂閱 (「 pub/sub") 的模式來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="5949e-119">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>
- <span data-ttu-id="5949e-120">**SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="5949e-120">**SQL Server**.</span></span> <span data-ttu-id="5949e-121">SQL Server 後擋板訊息寫入 SQL 資料表。</span><span class="sxs-lookup"><span data-stu-id="5949e-121">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="5949e-122">後擋板有效率的通訊使用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="5949e-122">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="5949e-123">不過，它也適用於未啟用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="5949e-123">However, it also works if Service Broker is not enabled.</span></span>

<span data-ttu-id="5949e-124">如果您部署在 Azure 上的應用程式，請考慮使用 Azure 服務匯流排後擋板。</span><span class="sxs-lookup"><span data-stu-id="5949e-124">If you deploy your application on Azure, consider using the Azure Service Bus backplane.</span></span> <span data-ttu-id="5949e-125">如果您要部署至伺服器陣列，請考慮 SQL Server 或 Redis 背板。</span><span class="sxs-lookup"><span data-stu-id="5949e-125">If you are deploying to your own server farm, consider the SQL Server or Redis backplanes.</span></span>

<span data-ttu-id="5949e-126">下列主題包含每個後擋板逐步教學的課程：</span><span class="sxs-lookup"><span data-stu-id="5949e-126">The following topics contain step-by-step tutorials for each backplane:</span></span>

- [<span data-ttu-id="5949e-127">Azure 服務匯流排與 SignalR 範圍外</span><span class="sxs-lookup"><span data-stu-id="5949e-127">SignalR Scaleout with Azure Service Bus</span></span>](scaleout-with-windows-azure-service-bus.md)
- [<span data-ttu-id="5949e-128">使用 Redis SignalR 範圍外</span><span class="sxs-lookup"><span data-stu-id="5949e-128">SignalR Scaleout with Redis</span></span>](scaleout-with-redis.md)
- [<span data-ttu-id="5949e-129">與 SQL Server 的 SignalR 範圍外</span><span class="sxs-lookup"><span data-stu-id="5949e-129">SignalR Scaleout with SQL Server</span></span>](scaleout-with-sql-server.md)

## <a name="implementation"></a><span data-ttu-id="5949e-130">實作</span><span class="sxs-lookup"><span data-stu-id="5949e-130">Implementation</span></span>

<span data-ttu-id="5949e-131">SignalR，每個訊息傳送至訊息匯流排。</span><span class="sxs-lookup"><span data-stu-id="5949e-131">In SignalR, every message is sent through a message bus.</span></span> <span data-ttu-id="5949e-132">訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象的。</span><span class="sxs-lookup"><span data-stu-id="5949e-132">A message bus implements the [IMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="5949e-133">運作方式是取代預設的背板**IMessageBus**與針對該後擋板匯流排。</span><span class="sxs-lookup"><span data-stu-id="5949e-133">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span> <span data-ttu-id="5949e-134">比方說，是 redis 訊息匯流排[RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)，並使用 Redis [pub/sub](http://redis.io/topics/pubsub)機制來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="5949e-134">For example, the message bus for Redis is [RedisMessageBus](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), and it uses the Redis [pub/sub](http://redis.io/topics/pubsub) mechanism to send and receive messages.</span></span>

<span data-ttu-id="5949e-135">每個伺服器執行個體連接到後擋板，已透過匯流排。</span><span class="sxs-lookup"><span data-stu-id="5949e-135">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="5949e-136">當訊息傳送時，它會移至後擋板，並後擋板，已將它傳送至每一部伺服器。</span><span class="sxs-lookup"><span data-stu-id="5949e-136">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="5949e-137">當伺服器收到訊息時從後擋板時，它會將訊息放在其本機快取中。</span><span class="sxs-lookup"><span data-stu-id="5949e-137">When a server gets a message from the backplane, it puts the message in its local cache.</span></span> <span data-ttu-id="5949e-138">伺服器再將訊息傳遞至用戶端從其本機快取。</span><span class="sxs-lookup"><span data-stu-id="5949e-138">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="5949e-139">每個用戶端連線，讀取訊息資料流的用戶端的程序是使用資料指標來追蹤。</span><span class="sxs-lookup"><span data-stu-id="5949e-139">For each client connection, the client's progress in reading the message stream is tracked using a cursor.</span></span> <span data-ttu-id="5949e-140">（資料指標表示訊息資料流中的位置）。如果用戶端中斷連接，然後再重新連接，它會要求匯流排已到達用戶端資料指標的值之後的任何訊息。</span><span class="sxs-lookup"><span data-stu-id="5949e-140">(A cursor represents a position in the message stream.) If a client disconnects and then reconnects, it asks the bus for any messages that arrived after the client's cursor value.</span></span> <span data-ttu-id="5949e-141">連線使用時，會發生相同的作業[長輪詢](../getting-started/introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="5949e-141">The same thing happens when a connection uses [long polling](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="5949e-142">長時間輪詢要求完成之後，用戶端就會開啟新的連線，並要求到達游標後的訊息。</span><span class="sxs-lookup"><span data-stu-id="5949e-142">After a long poll request completes, the client opens a new connection and asks for messages that arrived after the cursor.</span></span>

<span data-ttu-id="5949e-143">資料指標機制的運作即使在路由上的用戶端傳送到不同的伺服器重新連接。</span><span class="sxs-lookup"><span data-stu-id="5949e-143">The cursor mechanism works even if a client is routed to a different server on reconnect.</span></span> <span data-ttu-id="5949e-144">後擋板可感知的所有伺服器，而且用戶端連接至哪些伺服器沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="5949e-144">The backplane is aware of all the servers, and it doesn't matter which server a client connects to.</span></span>

## <a name="limitations"></a><span data-ttu-id="5949e-145">限制</span><span class="sxs-lookup"><span data-stu-id="5949e-145">Limitations</span></span>

<span data-ttu-id="5949e-146">使用後擋板，最大訊息輸送量會比單一伺服器節點直接向用戶端時。</span><span class="sxs-lookup"><span data-stu-id="5949e-146">Using a backplane, the maximum message throughput is lower than it is when clients talk directly to a single server node.</span></span> <span data-ttu-id="5949e-147">這是因為後擋板轉送至每個節點，每個訊息，因此後擋板成為瓶頸。</span><span class="sxs-lookup"><span data-stu-id="5949e-147">That's because the backplane forwards every message to every node, so the backplane can become a bottleneck.</span></span> <span data-ttu-id="5949e-148">這項限制是否發生問題，取決於應用程式。</span><span class="sxs-lookup"><span data-stu-id="5949e-148">Whether this limitation is a problem depends on the application.</span></span> <span data-ttu-id="5949e-149">例如，以下是一些典型的 SignalR 案例：</span><span class="sxs-lookup"><span data-stu-id="5949e-149">For example, here are some typical SignalR scenarios:</span></span>

- <span data-ttu-id="5949e-150">[伺服器廣播](tutorial-server-broadcast-with-aspnet-signalr.md)（例如，股票行情指示器）： 背板很適合此案例中，因為伺服器控制傳送訊息的速率。</span><span class="sxs-lookup"><span data-stu-id="5949e-150">[Server broadcast](tutorial-server-broadcast-with-aspnet-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
- <span data-ttu-id="5949e-151">[用戶端](tutorial-getting-started-with-signalr.md)（例如聊天）： 在此案例中後, 擋板可能是效能瓶頸如果用戶端數目的訊息數目縮放比例; 也就是說，如果訊息的速率增加按比例越多的用戶端加入。</span><span class="sxs-lookup"><span data-stu-id="5949e-151">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
- <span data-ttu-id="5949e-152">[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）： 後擋板建議您不要在此案例。</span><span class="sxs-lookup"><span data-stu-id="5949e-152">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>

## <a name="enabling-tracing-for-signalr-scaleout"></a><span data-ttu-id="5949e-153">啟用 SignalR 範圍外的追蹤</span><span class="sxs-lookup"><span data-stu-id="5949e-153">Enabling Tracing For SignalR Scaleout</span></span>

<span data-ttu-id="5949e-154">若要啟用追蹤的背板，加入下列各節 web.config 檔案中，根目錄下**組態**項目：</span><span class="sxs-lookup"><span data-stu-id="5949e-154">To enable tracing for the backplanes, add the following sections to the web.config file, under the root **configuration** element:</span></span>

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
