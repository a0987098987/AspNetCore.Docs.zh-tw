---
title: ASP.NET Core SignalR 實際執行裝載和擴充
author: bradygaster
description: 了解如何避免效能和調整使用 ASP.NET Core SignalR 應用程式中的問題。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836918"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a><span data-ttu-id="b6953-103">ASP.NET Core SignalR 裝載和擴充</span><span class="sxs-lookup"><span data-stu-id="b6953-103">ASP.NET Core SignalR hosting and scaling</span></span>

<span data-ttu-id="b6953-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)， [Brady Gaster](https://twitter.com/bradygaster)，並[Tom Dykstra](https://github.com/tdykstra)，</span><span class="sxs-lookup"><span data-stu-id="b6953-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="b6953-105">這篇文章說明裝載及調整高流量的應用程式使用 ASP.NET Core SignalR 的考量。</span><span class="sxs-lookup"><span data-stu-id="b6953-105">This article explains hosting and scaling considerations for high-traffic apps that use ASP.NET Core SignalR.</span></span>

## <a name="tcp-connection-resources"></a><span data-ttu-id="b6953-106">TCP 連線資源</span><span class="sxs-lookup"><span data-stu-id="b6953-106">TCP connection resources</span></span>

<span data-ttu-id="b6953-107">僅限於 web 伺服器可支援的並行 TCP 連線數目。</span><span class="sxs-lookup"><span data-stu-id="b6953-107">The number of concurrent TCP connections that a web server can support is limited.</span></span> <span data-ttu-id="b6953-108">使用標準的 HTTP 用戶端*暫時*連線。</span><span class="sxs-lookup"><span data-stu-id="b6953-108">Standard HTTP clients use *ephemeral* connections.</span></span> <span data-ttu-id="b6953-109">用戶端進入閒置，並稍後再重新開啟時，就可以關閉這些連線。</span><span class="sxs-lookup"><span data-stu-id="b6953-109">These connections can be closed when the client goes idle and reopened later.</span></span> <span data-ttu-id="b6953-110">相反地，SignalR 連線已*永續性*。</span><span class="sxs-lookup"><span data-stu-id="b6953-110">On the other hand, a SignalR connection is *persistent*.</span></span> <span data-ttu-id="b6953-111">SignalR 連線保持開啟甚至當用戶端變成閒置時。</span><span class="sxs-lookup"><span data-stu-id="b6953-111">SignalR connections stay open even when the client goes idle.</span></span> <span data-ttu-id="b6953-112">在許多用戶端提供服務的高流量應用程式，這些持續性連線可能會導致叫用其最大連接數目的伺服器。</span><span class="sxs-lookup"><span data-stu-id="b6953-112">In a high-traffic app that serves many clients, these persistent connections can cause servers to hit their maximum number of connections.</span></span>

<span data-ttu-id="b6953-113">持續連線也會使用一些額外的記憶體，來追蹤每個連線。</span><span class="sxs-lookup"><span data-stu-id="b6953-113">Persistent connections also consume some additional memory, to track each connection.</span></span>

<span data-ttu-id="b6953-114">連線相關的資源，由 SignalR 大量使用可能會影響其他裝載在相同的伺服器的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b6953-114">The heavy use of connection-related resources by SignalR can affect other web apps that are hosted on the same server.</span></span> <span data-ttu-id="b6953-115">當 SignalR 便會開啟，並保留最後一個可用的 TCP 連線時，在相同伺服器上的其他 web 應用程式也會有沒有更多的連線提供給他們。</span><span class="sxs-lookup"><span data-stu-id="b6953-115">When SignalR opens and holds the last available TCP connections, other web apps on the same server also have no more connections available to them.</span></span>

<span data-ttu-id="b6953-116">當伺服器連線，您會看到隨機通訊端錯誤，以及連接重設錯誤。</span><span class="sxs-lookup"><span data-stu-id="b6953-116">If a server runs out of connections, you'll see random socket errors and connection reset errors.</span></span> <span data-ttu-id="b6953-117">例如: </span><span class="sxs-lookup"><span data-stu-id="b6953-117">For example:</span></span>

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

<span data-ttu-id="b6953-118">若要防止 SignalR 資源使用狀況導致錯誤，其他的 web 應用程式中，執行 SignalR 比其他 web 應用程式的不同伺服器上。</span><span class="sxs-lookup"><span data-stu-id="b6953-118">To keep SignalR resource usage from causing errors in other web apps, run SignalR on different servers than your other web apps.</span></span>

<span data-ttu-id="b6953-119">為了避免 SignalR 資源使用狀況導致錯誤的 SignalR 應用程式中，向外延展至限制的伺服器必須處理的連線數目。</span><span class="sxs-lookup"><span data-stu-id="b6953-119">To keep SignalR resource usage from causing errors in a SignalR app, scale out to limit the number of connections a server has to handle.</span></span>

## <a name="scale-out"></a><span data-ttu-id="b6953-120">向外擴充</span><span class="sxs-lookup"><span data-stu-id="b6953-120">Scale out</span></span>

<span data-ttu-id="b6953-121">使用 SignalR 的應用程式需要追蹤的所有連線，這會建立伺服器陣列的問題。</span><span class="sxs-lookup"><span data-stu-id="b6953-121">An app that uses SignalR needs to keep track of all its connections, which creates problems for a server farm.</span></span> <span data-ttu-id="b6953-122">新增伺服器，，然後它會取得其他伺服器不了解的新連線。</span><span class="sxs-lookup"><span data-stu-id="b6953-122">Add a server, and it gets new connections that the other servers don't know about.</span></span> <span data-ttu-id="b6953-123">比方說下, 圖中的每部伺服器上的 SignalR 會察覺有其他伺服器上的連線。</span><span class="sxs-lookup"><span data-stu-id="b6953-123">For example, SignalR on each server in the following diagram is unaware of the connections on the other servers.</span></span> <span data-ttu-id="b6953-124">當一部伺服器上的 SignalR 想要將訊息傳送至所有用戶端時，訊息只會連線到該伺服器的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b6953-124">When SignalR on one of the servers wants to send a message to all clients, the message only goes to the clients connected to that server.</span></span>

![調整 SignalR 後擋板沒有](scale/_static/scale-no-backplane.png)

<span data-ttu-id="b6953-126">解決此問題的選項都會[Azure SignalR 服務](#azure-signalr-service)並[Redis 後擋板](#redis-backplane)。</span><span class="sxs-lookup"><span data-stu-id="b6953-126">The options for solving this problem are the [Azure SignalR Service](#azure-signalr-service) and [Redis backplane](#redis-backplane).</span></span>

## <a name="azure-signalr-service"></a><span data-ttu-id="b6953-127">Azure SignalR 服務</span><span class="sxs-lookup"><span data-stu-id="b6953-127">Azure SignalR Service</span></span>

<span data-ttu-id="b6953-128">Azure SignalR 服務是 proxy，而不是後擋板。</span><span class="sxs-lookup"><span data-stu-id="b6953-128">The Azure SignalR Service is a proxy rather than a backplane.</span></span> <span data-ttu-id="b6953-129">用戶端啟動連線到伺服器，每次用戶端重新導向連線到服務。</span><span class="sxs-lookup"><span data-stu-id="b6953-129">Each time a client initiates a connection to the server, the client is redirected to connect to the service.</span></span> <span data-ttu-id="b6953-130">下圖說明這個程序：</span><span class="sxs-lookup"><span data-stu-id="b6953-130">That process is illustrated in the following diagram:</span></span>

![建立 Azure SignalR 服務的連接](scale/_static/azure-signalr-service-one-connection.png)

<span data-ttu-id="b6953-132">結果會是服務管理的所有用戶端連線，而每部伺服器都必須只有常數少數，連線至服務，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="b6953-132">The result is that the service manages all of the client connections, while each server needs only a small constant number of connections to the service, as shown in the following diagram:</span></span>

![用戶端連接至服務，連接至服務的伺服器](scale/_static/azure-signalr-service-multiple-connections.png)

<span data-ttu-id="b6953-134">向外延展至這個方法有幾項優點 Redis 後擋板替代方案：</span><span class="sxs-lookup"><span data-stu-id="b6953-134">This approach to scale-out has several advantages over the Redis backplane alternative:</span></span>

* <span data-ttu-id="b6953-135">黏性工作階段，也稱為[用戶端親和性](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)，不需要，因為用戶端會立即重新導向至 Azure SignalR 服務連線時。</span><span class="sxs-lookup"><span data-stu-id="b6953-135">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is not required, because clients are immediately redirected to the Azure SignalR Service when they connect.</span></span>
* <span data-ttu-id="b6953-136">SignalR，應用程式可以相應放大為基礎的 Azure SignalR 服務自動進行調整以處理任何數量的連線時傳送的訊息數目。</span><span class="sxs-lookup"><span data-stu-id="b6953-136">A SignalR app can scale out based on the number of messages sent, while the Azure SignalR Service automatically scales to handle any number of connections.</span></span> <span data-ttu-id="b6953-137">比方說，可能有數千個用戶端，但如果只傳送幾秒的訊息，SignalR 應用程式不需要向外延展至多部伺服器，只是為了處理本身的連線。</span><span class="sxs-lookup"><span data-stu-id="b6953-137">For example, there could be thousands of clients, but if only a few messages per second are sent, the SignalR app won't need to scale out to multiple servers just to handle the connections themselves.</span></span>
* <span data-ttu-id="b6953-138">SignalR 應用程式不會使用比沒有 SignalR 的 web 應用程式的更多連線資源。</span><span class="sxs-lookup"><span data-stu-id="b6953-138">A SignalR app won't use significantly more connection resources than a web app without SignalR.</span></span>

<span data-ttu-id="b6953-139">基於這些理由，我們建議 Azure SignalR 服務的所有裝載於 Azure，包括應用程式服務、 Vm 和容器上的 ASP.NET Core SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b6953-139">For these reasons, we recommend the Azure SignalR Service for all ASP.NET Core SignalR apps hosted on Azure, including App Service, VMs, and containers.</span></span>

<span data-ttu-id="b6953-140">如需詳細資訊，請參閱[Azure SignalR 服務文件](/azure/azure-signalr/signalr-overview)。</span><span class="sxs-lookup"><span data-stu-id="b6953-140">For more information see the [Azure SignalR Service documentation](/azure/azure-signalr/signalr-overview).</span></span>

## <a name="redis-backplane"></a><span data-ttu-id="b6953-141">Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="b6953-141">Redis backplane</span></span>

<span data-ttu-id="b6953-142">[Redis](https://redis.io/)是記憶體中索引鍵-值存放區支援發行/訂閱模型的傳訊系統。</span><span class="sxs-lookup"><span data-stu-id="b6953-142">[Redis](https://redis.io/) is an in-memory key-value store that supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="b6953-143">Redis 的 SignalR 後擋板會使用發佈/訂閱功能，將訊息轉送到其他伺服器。</span><span class="sxs-lookup"><span data-stu-id="b6953-143">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span> <span data-ttu-id="b6953-144">當用戶端連接，連接資訊會傳遞至後擋板。</span><span class="sxs-lookup"><span data-stu-id="b6953-144">When a client makes a connection, the connection information is passed to the backplane.</span></span> <span data-ttu-id="b6953-145">當伺服器想要將訊息傳送至所有用戶端時，它會傳送至後擋板。</span><span class="sxs-lookup"><span data-stu-id="b6953-145">When a server wants to send a message to all clients, it sends to the backplane.</span></span> <span data-ttu-id="b6953-146">後擋板知道所有已連線的用戶端和它們在上伺服器。</span><span class="sxs-lookup"><span data-stu-id="b6953-146">The backplane knows all connected clients and which servers they're on.</span></span> <span data-ttu-id="b6953-147">會透過其個別的伺服器的所有用戶端傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="b6953-147">It sends the message to all clients via their respective servers.</span></span> <span data-ttu-id="b6953-148">下圖說明此程序：</span><span class="sxs-lookup"><span data-stu-id="b6953-148">This process is illustrated in the following diagram:</span></span>

![Redis 後擋板，從一部伺服器傳送至所有用戶端的訊息](scale/_static/redis-backplane.png)

<span data-ttu-id="b6953-150">Redis 後擋板是建議的向外延展方法，在您自己的基礎結構上託管的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b6953-150">The Redis backplane is the recommended scale-out approach for apps hosted on your own infrastructure.</span></span> <span data-ttu-id="b6953-151">Azure SignalR 服務並不是實際的選項，用於在內部部署應用程式，因為您的資料中心與 Azure 資料中心之間的連線延遲時間與實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="b6953-151">Azure SignalR Service isn't a practical option for production use with on-premises apps due to connection latency between your data center and an Azure data center.</span></span>

<span data-ttu-id="b6953-152">稍早所述 Azure SignalR 服務的優點是 Redis 後擋板的缺點：</span><span class="sxs-lookup"><span data-stu-id="b6953-152">The Azure SignalR Service advantages noted earlier are disadvantages for the Redis backplane:</span></span>

* <span data-ttu-id="b6953-153">黏性工作階段，也稱為[用戶端親和性](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)，需要。</span><span class="sxs-lookup"><span data-stu-id="b6953-153">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is required.</span></span> <span data-ttu-id="b6953-154">一旦在伺服器啟動連線，連線內必須停留在該伺服器上。</span><span class="sxs-lookup"><span data-stu-id="b6953-154">Once a connection is initiated on a server, the connection has to stay on that server.</span></span>
* <span data-ttu-id="b6953-155">SignalR 應用程式必須相應放大的用戶端數量中，即使傳送一些訊息。</span><span class="sxs-lookup"><span data-stu-id="b6953-155">A SignalR app must scale out based on number of clients even if few messages are being sent.</span></span>
* <span data-ttu-id="b6953-156">SignalR 應用程式會使用比沒有 SignalR 的 web 應用程式的更多連線資源。</span><span class="sxs-lookup"><span data-stu-id="b6953-156">A SignalR app uses significantly more connection resources than a web app without SignalR.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6953-157">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b6953-157">Next steps</span></span>

<span data-ttu-id="b6953-158">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="b6953-158">For more information, see the following resources:</span></span>

* [<span data-ttu-id="b6953-159">Azure SignalR 服務文件</span><span class="sxs-lookup"><span data-stu-id="b6953-159">Azure SignalR Service documentation</span></span>](/azure/azure-signalr/signalr-overview)
* [<span data-ttu-id="b6953-160">設定 Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="b6953-160">Set up a Redis backplane</span></span>](xref:signalr/redis-backplane)
