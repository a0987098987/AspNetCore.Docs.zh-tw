---
title: ASP.NET Core SignalR相應放大的 Redis 背板
author: bradygaster
description: 瞭解如何設定 Redis 背板，以啟用 ASP.NET Core SignalR應用程式的相應放大。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 6068890f4089a13add05bf8cf8009367e343adce
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775345"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a><span data-ttu-id="34e5f-103">為 ASP.NET Core SignalR相應放大設定 Redis 背板</span><span class="sxs-lookup"><span data-stu-id="34e5f-103">Set up a Redis backplane for ASP.NET Core SignalR scale-out</span></span>

<span data-ttu-id="34e5f-104">[Andrew Stanton-護士](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)和[Tom 作者: dykstra](https://github.com/tdykstra)，</span><span class="sxs-lookup"><span data-stu-id="34e5f-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="34e5f-105">本文說明SignalR設定[Redis](https://redis.io/)伺服器以用來相應放大 ASP.NET Core SignalR應用程式的特定層面。</span><span class="sxs-lookup"><span data-stu-id="34e5f-105">This article explains SignalR-specific aspects of setting up a [Redis](https://redis.io/) server to use for scaling out an ASP.NET Core SignalR app.</span></span>

## <a name="set-up-a-redis-backplane"></a><span data-ttu-id="34e5f-106">設定 Redis 背板</span><span class="sxs-lookup"><span data-stu-id="34e5f-106">Set up a Redis backplane</span></span>

* <span data-ttu-id="34e5f-107">部署 Redis 伺服器。</span><span class="sxs-lookup"><span data-stu-id="34e5f-107">Deploy a Redis server.</span></span>

  > [!IMPORTANT] 
  > <span data-ttu-id="34e5f-108">針對生產用途，只有在與SignalR應用程式相同的資料中心內執行時，才建議使用 Redis 背板。</span><span class="sxs-lookup"><span data-stu-id="34e5f-108">For production use, a Redis backplane is recommended only when it runs in the same data center as the SignalR app.</span></span> <span data-ttu-id="34e5f-109">否則，網路延遲會降低效能。</span><span class="sxs-lookup"><span data-stu-id="34e5f-109">Otherwise, network latency degrades performance.</span></span> <span data-ttu-id="34e5f-110">如果您SignalR的應用程式是在 azure 雲端中執行，建議SignalR使用 azure 服務，而不是 Redis 背板。</span><span class="sxs-lookup"><span data-stu-id="34e5f-110">If your SignalR app is running in the Azure cloud, we recommend Azure SignalR Service instead of a Redis backplane.</span></span> <span data-ttu-id="34e5f-111">您可以使用 Azure Redis 快取服務進行開發和測試環境。</span><span class="sxs-lookup"><span data-stu-id="34e5f-111">You can use the Azure Redis Cache Service for development and test environments.</span></span>

  <span data-ttu-id="34e5f-112">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="34e5f-112">For more information, see the following resources:</span></span>

  * <xref:signalr/scale>
  * [<span data-ttu-id="34e5f-113">Redis 文件</span><span class="sxs-lookup"><span data-stu-id="34e5f-113">Redis documentation</span></span>](https://redis.io/)
  * [<span data-ttu-id="34e5f-114">Azure Redis 快取文件</span><span class="sxs-lookup"><span data-stu-id="34e5f-114">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* <span data-ttu-id="34e5f-115">在SignalR應用程式中，安裝`Microsoft.AspNetCore.SignalR.Redis` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="34e5f-115">In the SignalR app, install the `Microsoft.AspNetCore.SignalR.Redis` NuGet package.</span></span>
* <span data-ttu-id="34e5f-116">在`Startup.ConfigureServices`方法中，呼叫`AddRedis` after `AddSignalR`：</span><span class="sxs-lookup"><span data-stu-id="34e5f-116">In the `Startup.ConfigureServices` method, call `AddRedis` after `AddSignalR`:</span></span>

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* <span data-ttu-id="34e5f-117">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="34e5f-117">Configure options as needed:</span></span>
 
  <span data-ttu-id="34e5f-118">大部分選項都可以在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件中設定。</span><span class="sxs-lookup"><span data-stu-id="34e5f-118">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="34e5f-119">中`ConfigurationOptions`指定的選項會覆寫連接字串中的設定。</span><span class="sxs-lookup"><span data-stu-id="34e5f-119">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="34e5f-120">下列範例顯示如何在`ConfigurationOptions`物件中設定選項。</span><span class="sxs-lookup"><span data-stu-id="34e5f-120">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="34e5f-121">這個範例會新增通道前置詞，讓多個應用程式可以共用相同的 Redis 實例，如下列步驟所述。</span><span class="sxs-lookup"><span data-stu-id="34e5f-121">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="34e5f-122">在上述程式碼中`options.Configuration` ，會使用連接字串中指定的內容來初始化。</span><span class="sxs-lookup"><span data-stu-id="34e5f-122">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="34e5f-123">在SignalR應用程式中，安裝下列其中一個 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="34e5f-123">In the SignalR app, install one of the following NuGet packages:</span></span>

  * <span data-ttu-id="34e5f-124">`Microsoft.AspNetCore.SignalR.StackExchangeRedis`-相依于 Stackexchange.redis. Redis 2. X.X。</span><span class="sxs-lookup"><span data-stu-id="34e5f-124">`Microsoft.AspNetCore.SignalR.StackExchangeRedis` - Depends on StackExchange.Redis 2.X.X.</span></span> <span data-ttu-id="34e5f-125">這是建議用於 ASP.NET Core 2.2 和更新版本的套件。</span><span class="sxs-lookup"><span data-stu-id="34e5f-125">This is the recommended package for ASP.NET Core 2.2 and later.</span></span>
  * <span data-ttu-id="34e5f-126">`Microsoft.AspNetCore.SignalR.Redis`-相依于 Stackexchange.redis. Redis 1. X.X。</span><span class="sxs-lookup"><span data-stu-id="34e5f-126">`Microsoft.AspNetCore.SignalR.Redis` - Depends on StackExchange.Redis 1.X.X.</span></span> <span data-ttu-id="34e5f-127">此套件不包含在 ASP.NET Core 3.0 和更新版本中。</span><span class="sxs-lookup"><span data-stu-id="34e5f-127">This package isn't included in ASP.NET Core 3.0 and later.</span></span>

* <span data-ttu-id="34e5f-128">在`Startup.ConfigureServices`方法中，呼叫<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>：</span><span class="sxs-lookup"><span data-stu-id="34e5f-128">In the `Startup.ConfigureServices` method, call <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 <span data-ttu-id="34e5f-129">使用`Microsoft.AspNetCore.SignalR.Redis`時，請<xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>呼叫。</span><span class="sxs-lookup"><span data-stu-id="34e5f-129">When using `Microsoft.AspNetCore.SignalR.Redis`, call <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span></span>

* <span data-ttu-id="34e5f-130">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="34e5f-130">Configure options as needed:</span></span>
 
  <span data-ttu-id="34e5f-131">大部分選項都可以在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件中設定。</span><span class="sxs-lookup"><span data-stu-id="34e5f-131">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="34e5f-132">中`ConfigurationOptions`指定的選項會覆寫連接字串中的設定。</span><span class="sxs-lookup"><span data-stu-id="34e5f-132">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="34e5f-133">下列範例顯示如何在`ConfigurationOptions`物件中設定選項。</span><span class="sxs-lookup"><span data-stu-id="34e5f-133">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="34e5f-134">這個範例會新增通道前置詞，讓多個應用程式可以共用相同的 Redis 實例，如下列步驟所述。</span><span class="sxs-lookup"><span data-stu-id="34e5f-134">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 <span data-ttu-id="34e5f-135">使用`Microsoft.AspNetCore.SignalR.Redis`時，請<xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>呼叫。</span><span class="sxs-lookup"><span data-stu-id="34e5f-135">When using `Microsoft.AspNetCore.SignalR.Redis`, call <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*>.</span></span>

  <span data-ttu-id="34e5f-136">在上述程式碼中`options.Configuration` ，會使用連接字串中指定的內容來初始化。</span><span class="sxs-lookup"><span data-stu-id="34e5f-136">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="34e5f-137">如需 Redis 選項的詳細資訊，請參閱[Stackexchange.redis Redis 檔](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。</span><span class="sxs-lookup"><span data-stu-id="34e5f-137">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="34e5f-138">在SignalR應用程式中，安裝下列 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="34e5f-138">In the SignalR app, install the following NuGet package:</span></span>

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* <span data-ttu-id="34e5f-139">在`Startup.ConfigureServices`方法中，呼叫<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>：</span><span class="sxs-lookup"><span data-stu-id="34e5f-139">In the `Startup.ConfigureServices` method, call <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*>:</span></span>

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* <span data-ttu-id="34e5f-140">視需要設定選項：</span><span class="sxs-lookup"><span data-stu-id="34e5f-140">Configure options as needed:</span></span>
 
  <span data-ttu-id="34e5f-141">大部分選項都可以在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件中設定。</span><span class="sxs-lookup"><span data-stu-id="34e5f-141">Most options can be set in the connection string or in the [ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options) object.</span></span> <span data-ttu-id="34e5f-142">中`ConfigurationOptions`指定的選項會覆寫連接字串中的設定。</span><span class="sxs-lookup"><span data-stu-id="34e5f-142">Options specified in `ConfigurationOptions` override the ones set in the connection string.</span></span>

  <span data-ttu-id="34e5f-143">下列範例顯示如何在`ConfigurationOptions`物件中設定選項。</span><span class="sxs-lookup"><span data-stu-id="34e5f-143">The following example shows how to set options in the `ConfigurationOptions` object.</span></span> <span data-ttu-id="34e5f-144">這個範例會新增通道前置詞，讓多個應用程式可以共用相同的 Redis 實例，如下列步驟所述。</span><span class="sxs-lookup"><span data-stu-id="34e5f-144">This example adds a channel prefix so that multiple apps can share the same Redis instance, as explained in the following step.</span></span>

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  <span data-ttu-id="34e5f-145">在上述程式碼中`options.Configuration` ，會使用連接字串中指定的內容來初始化。</span><span class="sxs-lookup"><span data-stu-id="34e5f-145">In the preceding code, `options.Configuration` is initialized with whatever was specified in the connection string.</span></span>

  <span data-ttu-id="34e5f-146">如需 Redis 選項的詳細資訊，請參閱[Stackexchange.redis Redis 檔](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。</span><span class="sxs-lookup"><span data-stu-id="34e5f-146">For information about Redis options, see the [StackExchange Redis documentation](https://stackexchange.github.io/StackExchange.Redis/Configuration.html).</span></span>

::: moniker-end

* <span data-ttu-id="34e5f-147">如果您針對多個SignalR應用程式使用一部 Redis 伺服器，請為每個SignalR應用程式使用不同的通道首碼。</span><span class="sxs-lookup"><span data-stu-id="34e5f-147">If you're using one Redis server for multiple SignalR apps, use a different channel prefix for each SignalR app.</span></span>

  <span data-ttu-id="34e5f-148">設定通道前置詞會隔離SignalR另一個應用程式與其他使用不同通道首碼的 app。</span><span class="sxs-lookup"><span data-stu-id="34e5f-148">Setting a channel prefix isolates one SignalR app from others that use different channel prefixes.</span></span> <span data-ttu-id="34e5f-149">如果您未指派不同的前置詞，則從某個應用程式傳送到其所有用戶端的訊息，將會移至使用 Redis 伺服器做為背板的所有應用程式的所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="34e5f-149">If you don't assign different prefixes, a message sent from one app to all of its own clients will go to all clients of all apps that use the Redis server as a backplane.</span></span>

* <span data-ttu-id="34e5f-150">針對粘滯會話設定伺服器陣列的負載平衡軟體。</span><span class="sxs-lookup"><span data-stu-id="34e5f-150">Configure your server farm load balancing software for sticky sessions.</span></span> <span data-ttu-id="34e5f-151">以下是有關如何執行此動作的一些檔範例：</span><span class="sxs-lookup"><span data-stu-id="34e5f-151">Here are some examples of documentation on how to do that:</span></span>

  * [<span data-ttu-id="34e5f-152">IIS</span><span class="sxs-lookup"><span data-stu-id="34e5f-152">IIS</span></span>](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [<span data-ttu-id="34e5f-153">HAProxy</span><span class="sxs-lookup"><span data-stu-id="34e5f-153">HAProxy</span></span>](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [<span data-ttu-id="34e5f-154">Nginx</span><span class="sxs-lookup"><span data-stu-id="34e5f-154">Nginx</span></span>](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [<span data-ttu-id="34e5f-155">pfSense</span><span class="sxs-lookup"><span data-stu-id="34e5f-155">pfSense</span></span>](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a><span data-ttu-id="34e5f-156">Redis 伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="34e5f-156">Redis server errors</span></span>

<span data-ttu-id="34e5f-157">當 Redis 伺服器關閉時， SignalR會擲回指出不會傳遞訊息的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="34e5f-157">When a Redis server goes down, SignalR throws exceptions that indicate messages won't be delivered.</span></span> <span data-ttu-id="34e5f-158">一些一般的例外狀況訊息：</span><span class="sxs-lookup"><span data-stu-id="34e5f-158">Some typical exception messages:</span></span>

* <span data-ttu-id="34e5f-159">*無法寫入訊息*</span><span class="sxs-lookup"><span data-stu-id="34e5f-159">*Failed writing message*</span></span>
* <span data-ttu-id="34e5f-160">*無法叫用中樞方法 ' 方法名稱 '*</span><span class="sxs-lookup"><span data-stu-id="34e5f-160">*Failed to invoke hub method 'MethodName'*</span></span>
* <span data-ttu-id="34e5f-161">*連接至 Redis 失敗*</span><span class="sxs-lookup"><span data-stu-id="34e5f-161">*Connection to Redis failed*</span></span>

SignalR<span data-ttu-id="34e5f-162">當伺服器恢復運作時，不會緩衝處理訊息以傳送它們。</span><span class="sxs-lookup"><span data-stu-id="34e5f-162"> doesn't buffer messages to send them when the server comes back up.</span></span> <span data-ttu-id="34e5f-163">Redis 伺服器關閉時傳送的任何訊息都會遺失。</span><span class="sxs-lookup"><span data-stu-id="34e5f-163">Any messages sent while the Redis server is down are lost.</span></span>

SignalR<span data-ttu-id="34e5f-164">當 Redis 伺服器再次可供使用時，自動重新連接。</span><span class="sxs-lookup"><span data-stu-id="34e5f-164"> automatically reconnects when the Redis server is available again.</span></span>

### <a name="custom-behavior-for-connection-failures"></a><span data-ttu-id="34e5f-165">連接失敗的自訂行為</span><span class="sxs-lookup"><span data-stu-id="34e5f-165">Custom behavior for connection failures</span></span>

<span data-ttu-id="34e5f-166">以下範例示範如何處理 Redis 連接失敗事件。</span><span class="sxs-lookup"><span data-stu-id="34e5f-166">Here's an example that shows how to handle Redis connection failure events.</span></span>

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

## <a name="redis-clustering"></a><span data-ttu-id="34e5f-167">Redis 叢集</span><span class="sxs-lookup"><span data-stu-id="34e5f-167">Redis Clustering</span></span>

<span data-ttu-id="34e5f-168">[Redis](https://redis.io/topics/cluster-spec)叢集是使用多部 Redis 伺服器達到高可用性的方法。</span><span class="sxs-lookup"><span data-stu-id="34e5f-168">[Redis Clustering](https://redis.io/topics/cluster-spec) is a method for achieving high availability by using multiple Redis servers.</span></span> <span data-ttu-id="34e5f-169">叢集並未正式支援，但可能會有作用。</span><span class="sxs-lookup"><span data-stu-id="34e5f-169">Clustering isn't officially supported, but it might work.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34e5f-170">後續步驟</span><span class="sxs-lookup"><span data-stu-id="34e5f-170">Next steps</span></span>

<span data-ttu-id="34e5f-171">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="34e5f-171">For more information, see the following resources:</span></span>

* <xref:signalr/scale>
* [<span data-ttu-id="34e5f-172">Redis 文件</span><span class="sxs-lookup"><span data-stu-id="34e5f-172">Redis documentation</span></span>](https://redis.io/documentation)
* [<span data-ttu-id="34e5f-173">Stackexchange.redis Redis 檔</span><span class="sxs-lookup"><span data-stu-id="34e5f-173">StackExchange Redis documentation</span></span>](https://stackexchange.github.io/StackExchange.Redis/)
* [<span data-ttu-id="34e5f-174">Azure Redis 快取文件</span><span class="sxs-lookup"><span data-stu-id="34e5f-174">Azure Redis Cache documentation</span></span>](https://docs.microsoft.com/azure/redis-cache/)
