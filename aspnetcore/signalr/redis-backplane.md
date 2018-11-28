---
title: Redis ASP.NET Core SignalR 向外延展後的擋板
author: tdykstra
description: 了解如何設定 Redis 後擋板，若要啟用向外延展的 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: c8b09c0d482da344b54d167c0c9757167eaa6186
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52452924"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="10eb8-103">設定 ASP.NET Core SignalR 向外延展 Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="10eb8-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="10eb8-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)， [Brady Gaster](https://twitter.com/bradygaster)，並[Tom Dykstra](https://github.com/tdykstra)，</span><span class="sxs-lookup"><span data-stu-id="10eb8-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="10eb8-105">這篇文章說明設定 SignalR 特定層面[Redis](https://redis.io/)来用於相應放大的 ASP.NET Core SignalR 應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="10eb8-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="10eb8-106">設定 Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="10eb8-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="10eb8-107">部署 Redis 伺服器。</span><span class="sxs-lookup"><span data-stu-id="10eb8-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="10eb8-108">針對生產用途，Redis 後擋板建議只針對內部部署基礎結構。</span><span class="sxs-lookup"><span data-stu-id="10eb8-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="10eb8-109">若要延遲降至最低，Redis 伺服器應該在相同資料中心與 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="10eb8-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="10eb8-110">如果您的 SignalR 應用程式正在執行 Azure 雲端中，我們會建議 Azure SignalR 服務，而不是 Redis 後擋板。</span><span class="sxs-lookup"><span data-stu-id="10eb8-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="10eb8-111">您可以使用 Azure Redis 快取服務進行開發和測試環境。</span><span class="sxs-lookup"><span data-stu-id="10eb8-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="10eb8-112">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="10eb8-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="10eb8-113">Redis 文件</span><span class="sxs-lookup"><span data-stu-id="10eb8-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="10eb8-114">Azure Redis 快取文件</span><span class="sxs-lookup"><span data-stu-id="10eb8-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="10eb8-115">中的 SignalR 應用程式中，安裝`Microsoft.AspNetCore.SignalR.Redis`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>

* <span data-ttu-id="10eb8-116">在 `Startup.ConfigureServices`方法中，呼叫`AddRedis`之後`AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="10eb8-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="10eb8-117">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="10eb8-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="10eb8-118">您可以設定大部分的選項，在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="10eb8-119">中指定的選項`ConfigurationOptions`覆寫連接字串中設定的原則。</span><span class="sxs-lookup"><span data-stu-id="10eb8-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="10eb8-120">下列範例示範如何在中設定選項`ConfigurationOptions`物件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="10eb8-121">此範例會將通道的前置詞，讓多個應用程式可以共用相同的 Redis 執行個體下, 一個步驟中所述。</span><span class="sxs-lookup"><span data-stu-id="10eb8-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="10eb8-122">在上述程式碼，`options.Configuration`任何指定的連接字串中進行初始化。</span><span class="sxs-lookup"><span data-stu-id="10eb8-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="10eb8-123">中的 SignalR 應用程式中，安裝`Microsoft.AspNetCore.SignalR.StackExchangeRedis`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-123">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.StackExchangeRedis` NuGet package.</span></span>

* <span data-ttu-id="10eb8-124">在 `Startup.ConfigureServices`方法中，呼叫`AddStackExchangeRedis`之後`AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="10eb8-124">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="10eb8-125">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="10eb8-125">Configure options as needed:</span></span>
 
  <span data-ttu-id="10eb8-126">您可以設定大部分的選項，在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-126">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="10eb8-127">中指定的選項`ConfigurationOptions`覆寫連接字串中設定的原則。</span><span class="sxs-lookup"><span data-stu-id="10eb8-127">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="10eb8-128">下列範例示範如何在中設定選項`ConfigurationOptions`物件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-128">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="10eb8-129">此範例會將通道的前置詞，讓多個應用程式可以共用相同的 Redis 執行個體下, 一個步驟中所述。</span><span class="sxs-lookup"><span data-stu-id="10eb8-129">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="10eb8-130">在上述程式碼，`options.Configuration`任何指定的連接字串中進行初始化。</span><span class="sxs-lookup"><span data-stu-id="10eb8-130">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="10eb8-131">如需 Redis 選項的資訊，請參閱[StackExchange Redis 文件](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。</span><span class="sxs-lookup"><span data-stu-id="10eb8-131">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="10eb8-132">如果您使用一個 Redis 伺服器的多個的 SignalR 應用程式，請針對每個 SignalR 應用程式使用不同的通道前置詞。</span><span class="sxs-lookup"><span data-stu-id="10eb8-132">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="10eb8-133">設定通道的前置詞會隔離其他使用不同的通道前置詞的項目從一個 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="10eb8-133">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="10eb8-134">如果您不指派不同的前置詞，從一個應用程式傳送至所有它自己的用戶端的訊息會移至後擋板以使用 Redis 伺服器的所有應用程式的所有用戶端中。</span><span class="sxs-lookup"><span data-stu-id="10eb8-134">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="10eb8-135">設定您的伺服器的伺服陣列負載平衡軟體黏性工作階段。</span><span class="sxs-lookup"><span data-stu-id="10eb8-135">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="10eb8-136">以下是一些有關如何這麼做的文件範例：</span><span class="sxs-lookup"><span data-stu-id="10eb8-136">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="10eb8-137">IIS</span><span class="sxs-lookup"><span data-stu-id="10eb8-137">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="10eb8-138">HAProxy</span><span class="sxs-lookup"><span data-stu-id="10eb8-138">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="10eb8-139">Nginx</span><span class="sxs-lookup"><span data-stu-id="10eb8-139">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="10eb8-140">pfSense</span><span class="sxs-lookup"><span data-stu-id="10eb8-140">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="10eb8-141">Redis 伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="10eb8-141">Redis server errors</span></span>

<span data-ttu-id="10eb8-142">當 Redis 伺服器當機時，SignalR 擲回的例外狀況，表示不會將訊息傳遞。</span><span class="sxs-lookup"><span data-stu-id="10eb8-142">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="10eb8-143">某些一般的例外狀況訊息：</span><span class="sxs-lookup"><span data-stu-id="10eb8-143">Some typical exception messages:</span></span>

* <span data-ttu-id="10eb8-144">*失敗的寫入訊息*</span><span class="sxs-lookup"><span data-stu-id="10eb8-144">*Failed writing message*</span></span>
* <span data-ttu-id="10eb8-145">*無法叫用中樞方法 'MethodName'*</span><span class="sxs-lookup"><span data-stu-id="10eb8-145">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="10eb8-146">*無法連線至 Redis*</span><span class="sxs-lookup"><span data-stu-id="10eb8-146">*Connection to Redis failed*</span></span>

<span data-ttu-id="10eb8-147">SignalR 未緩衝訊息傳送給他們時伺服器恢復運作。</span><span class="sxs-lookup"><span data-stu-id="10eb8-147">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="10eb8-148">Redis 伺服器已關閉時傳送任何訊息都會遺失。</span><span class="sxs-lookup"><span data-stu-id="10eb8-148">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="10eb8-149">Redis 伺服器再次可用時，會自動重新連線 SignalR。</span><span class="sxs-lookup"><span data-stu-id="10eb8-149">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="10eb8-150">連線失敗的自訂行為</span><span class="sxs-lookup"><span data-stu-id="10eb8-150">Custom behavior for connection failures</span></span>

<span data-ttu-id="10eb8-151">以下是範例，示範如何處理 Redis 連線失敗事件。</span><span class="sxs-lookup"><span data-stu-id="10eb8-151">Here's an example that shows how to handle Redis connection failure events.</span></span>

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

::: moniker range="> aspnetcore-2.1"

```csharp
services.AddSignalR()
        .AddMessagePackProtocol()
        .AddStackExchangeRedis(o =>
        {
            o.ConnectionFactory = async writer =>
            {
                var config = new ConfigurationOptions
                {
                    AbortOnConnectFail = false
                };
                config.EndPoints.Add(IPAddress.Loopback, 0);
                config.SetDefaultPorts();
                var connection = await ConnectionMultiplexer.ConnectAsync(config, writer);
                connection.ConnectionFailed += (_, e) =>
                {
                    Console.WriteLine("Connection to Redis failed.");
                };

                if (!connection.IsConnected)
                {
                    Console.WriteLine("Did not connect to Redis.");
                }

                return connection;
            };
        });
```

::: moniker-end

## <a name="clustering"></a><span data-ttu-id="10eb8-152">群集</span><span class="sxs-lookup"><span data-stu-id="10eb8-152">Clustering</span></span>

<span data-ttu-id="10eb8-153">叢集是使用多個 Redis 伺服器達到高可用性的方法。</span><span class="sxs-lookup"><span data-stu-id="10eb8-153">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="10eb8-154">叢集未正式支援，但可能會運作。</span><span class="sxs-lookup"><span data-stu-id="10eb8-154">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10eb8-155">後續步驟</span><span class="sxs-lookup"><span data-stu-id="10eb8-155">Next steps</span></span>

<span data-ttu-id="10eb8-156">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="10eb8-156">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="10eb8-157">Redis 文件</span><span class="sxs-lookup"><span data-stu-id="10eb8-157">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="10eb8-158">StackExchange Redis 文件</span><span class="sxs-lookup"><span data-stu-id="10eb8-158">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="10eb8-159">Azure Redis 快取文件</span><span class="sxs-lookup"><span data-stu-id="10eb8-159">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
