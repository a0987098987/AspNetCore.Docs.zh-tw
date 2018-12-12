---
title: Redis ASP.NET Core SignalR 向外延展後的擋板
author: tdykstra
description: 了解如何設定 Redis 後擋板，若要啟用向外延展的 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: 343cb5b2c7ed7162bae7865553a783fea45f0cfb
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284463"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="83298-103">設定 ASP.NET Core SignalR 向外延展 Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="83298-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="83298-104">藉由[Andrew Stanton-nurse](https://twitter.com/anurse)， [Brady Gaster](https://twitter.com/bradygaster)，並[Tom Dykstra](https://github.com/tdykstra)，</span><span class="sxs-lookup"><span data-stu-id="83298-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="83298-105">這篇文章說明設定 SignalR 特定層面[Redis](https://redis.io/)来用於相應放大的 ASP.NET Core SignalR 應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="83298-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="83298-106">設定 Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="83298-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="83298-107">部署 Redis 伺服器。</span><span class="sxs-lookup"><span data-stu-id="83298-107">Deploy a Redis server.</span></span>

  <span data-ttu-id="83298-108">針對生產用途，Redis 後擋板建議只針對內部部署基礎結構。</span><span class="sxs-lookup"><span data-stu-id="83298-108">For production use, a Redis backplane is recommended only for on-premises infrastructure.</span></span> <span data-ttu-id="83298-109">若要延遲降至最低，Redis 伺服器應該在相同資料中心與 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="83298-109">To minimize latency, the Redis server should be in the same data center as the SignalR app.</span></span> <span data-ttu-id="83298-110">如果您的 SignalR 應用程式正在執行 Azure 雲端中，我們會建議 Azure SignalR 服務，而不是 Redis 後擋板。</span><span class="sxs-lookup"><span data-stu-id="83298-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="83298-111">您可以使用 Azure Redis 快取服務進行開發和測試環境。</span><span class="sxs-lookup"><span data-stu-id="83298-111">You can use the Azure Redis Cache Service for development and test environments.</span></span> <span data-ttu-id="83298-112">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="83298-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="83298-113">Redis 文件</span><span class="sxs-lookup"><span data-stu-id="83298-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="83298-114">Azure Redis 快取文件</span><span class="sxs-lookup"><span data-stu-id="83298-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="83298-115">中的 SignalR 應用程式中，安裝`Microsoft.AspNetCore.SignalR.Redis`NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="83298-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span> <span data-ttu-id="83298-116">(另外還有`Microsoft.AspNetCore.SignalR.StackExchangeRedis`封裝，但是，另一個 ASP.NET Core 2.2 和更新版本。)</span><span class="sxs-lookup"><span data-stu-id="83298-116">(There is also a `Microsoft.AspNetCore.SignalR.StackExchangeRedis` package, but that one is for ASP.NET Core 2.2 and later.)</span></span>

* <span data-ttu-id="83298-117">在 `Startup.ConfigureServices`方法中，呼叫`AddRedis`之後`AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="83298-117">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="83298-118">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="83298-118">Configure options as needed:</span></span>
 
  <span data-ttu-id="83298-119">您可以設定大部分的選項，在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件。</span><span class="sxs-lookup"><span data-stu-id="83298-119">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="83298-120">中指定的選項`ConfigurationOptions`覆寫連接字串中設定的原則。</span><span class="sxs-lookup"><span data-stu-id="83298-120">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="83298-121">下列範例示範如何在中設定選項`ConfigurationOptions`物件。</span><span class="sxs-lookup"><span data-stu-id="83298-121">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="83298-122">此範例會將通道的前置詞，讓多個應用程式可以共用相同的 Redis 執行個體下, 一個步驟中所述。</span><span class="sxs-lookup"><span data-stu-id="83298-122">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="83298-123">在上述程式碼，`options.Configuration`任何指定的連接字串中進行初始化。</span><span class="sxs-lookup"><span data-stu-id="83298-123">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* <span data-ttu-id="83298-124">中的 SignalR 應用程式中，安裝下列 NuGet 套件的其中一個：</span><span class="sxs-lookup"><span data-stu-id="83298-124">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="83298-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` -取決於 StackExchange.Redis 2.X.X。</span><span class="sxs-lookup"><span data-stu-id="83298-125">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="83298-126">這是 ASP.NET Core 2.2 和更新版本的建議的封裝。</span><span class="sxs-lookup"><span data-stu-id="83298-126">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="83298-127">`Microsoft.AspNetCore.SignalR.Redis` -取決於 StackExchange.Redis 1.X.X。</span><span class="sxs-lookup"><span data-stu-id="83298-127">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="83298-128">此套件不會在 ASP.NET Core 3.0 出貨。</span><span class="sxs-lookup"><span data-stu-id="83298-128">This package will not be shipping in ASP.NET Core 3.0.</span></span>

* <span data-ttu-id="83298-129">在 `Startup.ConfigureServices`方法中，呼叫`AddStackExchangeRedis`之後`AddSignalR`:</span><span class="sxs-lookup"><span data-stu-id="83298-129">In the `Startup.ConfigureServices` method, call `AddStackExchangeRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="83298-130">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="83298-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="83298-131">您可以設定大部分的選項，在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件。</span><span class="sxs-lookup"><span data-stu-id="83298-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="83298-132">中指定的選項`ConfigurationOptions`覆寫連接字串中設定的原則。</span><span class="sxs-lookup"><span data-stu-id="83298-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="83298-133">下列範例示範如何在中設定選項`ConfigurationOptions`物件。</span><span class="sxs-lookup"><span data-stu-id="83298-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="83298-134">此範例會將通道的前置詞，讓多個應用程式可以共用相同的 Redis 執行個體下, 一個步驟中所述。</span><span class="sxs-lookup"><span data-stu-id="83298-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="83298-135">在上述程式碼，`options.Configuration`任何指定的連接字串中進行初始化。</span><span class="sxs-lookup"><span data-stu-id="83298-135">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="83298-136">如需 Redis 選項的資訊，請參閱[StackExchange Redis 文件](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。</span><span class="sxs-lookup"><span data-stu-id="83298-136">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="83298-137">如果您使用一個 Redis 伺服器的多個的 SignalR 應用程式，請針對每個 SignalR 應用程式使用不同的通道前置詞。</span><span class="sxs-lookup"><span data-stu-id="83298-137">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="83298-138">設定通道的前置詞會隔離其他使用不同的通道前置詞的項目從一個 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="83298-138">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="83298-139">如果您不指派不同的前置詞，從一個應用程式傳送至所有它自己的用戶端的訊息會移至後擋板以使用 Redis 伺服器的所有應用程式的所有用戶端中。</span><span class="sxs-lookup"><span data-stu-id="83298-139">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="83298-140">設定您的伺服器的伺服陣列負載平衡軟體黏性工作階段。</span><span class="sxs-lookup"><span data-stu-id="83298-140">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="83298-141">以下是一些有關如何這麼做的文件範例：</span><span class="sxs-lookup"><span data-stu-id="83298-141">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="83298-142">IIS</span><span class="sxs-lookup"><span data-stu-id="83298-142">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="83298-143">HAProxy</span><span class="sxs-lookup"><span data-stu-id="83298-143">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="83298-144">Nginx</span><span class="sxs-lookup"><span data-stu-id="83298-144">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="83298-145">pfSense</span><span class="sxs-lookup"><span data-stu-id="83298-145">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="83298-146">Redis 伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="83298-146">Redis server errors</span></span>

<span data-ttu-id="83298-147">當 Redis 伺服器當機時，SignalR 擲回的例外狀況，表示不會將訊息傳遞。</span><span class="sxs-lookup"><span data-stu-id="83298-147">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="83298-148">某些一般的例外狀況訊息：</span><span class="sxs-lookup"><span data-stu-id="83298-148">Some typical exception messages:</span></span>

* <span data-ttu-id="83298-149">*失敗的寫入訊息*</span><span class="sxs-lookup"><span data-stu-id="83298-149">*Failed writing message*</span></span>
* <span data-ttu-id="83298-150">*無法叫用中樞方法 'MethodName'*</span><span class="sxs-lookup"><span data-stu-id="83298-150">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="83298-151">*無法連線至 Redis*</span><span class="sxs-lookup"><span data-stu-id="83298-151">*Connection to Redis failed*</span></span>

<span data-ttu-id="83298-152">SignalR 未緩衝訊息傳送給他們時伺服器恢復運作。</span><span class="sxs-lookup"><span data-stu-id="83298-152">SignalR doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="83298-153">Redis 伺服器已關閉時傳送任何訊息都會遺失。</span><span class="sxs-lookup"><span data-stu-id="83298-153">Any messages sent while the Redis server is down are lost.</span></span>

<span data-ttu-id="83298-154">Redis 伺服器再次可用時，會自動重新連線 SignalR。</span><span class="sxs-lookup"><span data-stu-id="83298-154">SignalR automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="83298-155">連線失敗的自訂行為</span><span class="sxs-lookup"><span data-stu-id="83298-155">Custom behavior for connection failures</span></span>

<span data-ttu-id="83298-156">以下是範例，示範如何處理 Redis 連線失敗事件。</span><span class="sxs-lookup"><span data-stu-id="83298-156">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="clustering"></a><span data-ttu-id="83298-157">群集</span><span class="sxs-lookup"><span data-stu-id="83298-157">Clustering</span></span>

<span data-ttu-id="83298-158">叢集是使用多個 Redis 伺服器達到高可用性的方法。</span><span class="sxs-lookup"><span data-stu-id="83298-158">Clustering is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="83298-159">叢集未正式支援，但可能會運作。</span><span class="sxs-lookup"><span data-stu-id="83298-159">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83298-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="83298-160">Next steps</span></span>

<span data-ttu-id="83298-161">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="83298-161">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="83298-162">Redis 文件</span><span class="sxs-lookup"><span data-stu-id="83298-162">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="83298-163">StackExchange Redis 文件</span><span class="sxs-lookup"><span data-stu-id="83298-163">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="83298-164">Azure Redis 快取文件</span><span class="sxs-lookup"><span data-stu-id="83298-164">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/en-us/azure/redis-cache/)
