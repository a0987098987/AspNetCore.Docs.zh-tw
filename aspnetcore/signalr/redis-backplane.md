---
title: Redis ASP.NET Core SignalR 向外延展後的擋板
author: bradygaster
description: 了解如何設定 Redis 後擋板，若要啟用向外延展的 ASP.NET Core SignalR 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/redis-backplane
ms.openlocfilehash: 9d2a942dba6abe669126efee7f2b3cdd6560658e
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012639"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>設定 ASP.NET Core SignalR 向外延展 Redis 後擋板

藉由[Andrew Stanton-nurse](https://twitter.com/anurse)， [Brady Gaster](https://twitter.com/bradygaster)，並[Tom Dykstra](https://github.com/tdykstra)，

這篇文章說明設定 SignalR 特定層面[Redis](https://redis.io/)来用於相應放大的 ASP.NET Core SignalR 應用程式伺服器。

## <a name="set-up-a-redis-backplane"></a>設定 Redis 後擋板

* 部署 Redis 伺服器。

  > [!IMPORTANT] 
  > 針對生產用途，它會在相同的資料中心與 SignalR 應用程式執行時，才建議 Redis 後擋板。 否則，網路延遲會降低效能。 如果您的 SignalR 應用程式正在執行 Azure 雲端中，我們會建議 Azure SignalR 服務，而不是 Redis 後擋板。 您可以使用 Azure Redis 快取服務進行開發和測試環境。

  如需詳細資訊，請參閱下列資源：

  * <xref:signalr/scale>
  * [Redis 文件](https://redis.io/)
  * [Azure Redis 快取文件](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* 中的 SignalR 應用程式中，安裝`Microsoft.AspNetCore.SignalR.Redis`NuGet 套件。 (另外還有`Microsoft.AspNetCore.SignalR.StackExchangeRedis`封裝，但是，另一個 ASP.NET Core 2.2 和更新版本。)

* 在 `Startup.ConfigureServices`方法中，呼叫`AddRedis`之後`AddSignalR`:

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* 視需要設定選項：
 
  您可以設定大部分的選項，在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件。 中指定的選項`ConfigurationOptions`覆寫連接字串中設定的原則。

  下列範例示範如何在中設定選項`ConfigurationOptions`物件。 此範例會將通道的前置詞，讓多個應用程式可以共用相同的 Redis 執行個體下, 一個步驟中所述。

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  在上述程式碼，`options.Configuration`任何指定的連接字串中進行初始化。

::: moniker-end

::: moniker range="> aspnetcore-2.1"

* 中的 SignalR 應用程式中，安裝下列 NuGet 套件的其中一個：

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis` -取決於 StackExchange.Redis 2.X.X。 這是 ASP.NET Core 2.2 和更新版本的建議的封裝。
  * `Microsoft.AspNetCore.SignalR.Redis` -取決於 StackExchange.Redis 1.X.X。 此套件不會在 ASP.NET Core 3.0 出貨。

* 在 `Startup.ConfigureServices`方法中，呼叫`AddStackExchangeRedis`之後`AddSignalR`:

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

* 視需要設定選項：
 
  您可以設定大部分的選項，在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件。 中指定的選項`ConfigurationOptions`覆寫連接字串中設定的原則。

  下列範例示範如何在中設定選項`ConfigurationOptions`物件。 此範例會將通道的前置詞，讓多個應用程式可以共用相同的 Redis 執行個體下, 一個步驟中所述。

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  在上述程式碼，`options.Configuration`任何指定的連接字串中進行初始化。

  如需 Redis 選項的資訊，請參閱[StackExchange Redis 文件](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。

::: moniker-end

* 如果您使用一個 Redis 伺服器的多個的 SignalR 應用程式，請針對每個 SignalR 應用程式使用不同的通道前置詞。

  設定通道的前置詞會隔離其他使用不同的通道前置詞的項目從一個 SignalR 應用程式。 如果您不指派不同的前置詞，從一個應用程式傳送至所有它自己的用戶端的訊息會移至後擋板以使用 Redis 伺服器的所有應用程式的所有用戶端中。

* 設定您的伺服器的伺服陣列負載平衡軟體黏性工作階段。 以下是一些有關如何這麼做的文件範例：

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Redis 伺服器錯誤

當 Redis 伺服器當機時，SignalR 擲回的例外狀況，表示不會將訊息傳遞。 某些一般的例外狀況訊息：

* *失敗的寫入訊息*
* *無法叫用中樞方法 'MethodName'*
* *無法連線至 Redis*

SignalR 未緩衝訊息傳送給他們時伺服器恢復運作。 Redis 伺服器已關閉時傳送任何訊息都會遺失。

Redis 伺服器再次可用時，會自動重新連線 SignalR。

### <a name="custom-behavior-for-connection-failures"></a>連線失敗的自訂行為

以下是範例，示範如何處理 Redis 連線失敗事件。

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

## <a name="clustering"></a>群集

叢集是使用多個 Redis 伺服器達到高可用性的方法。 叢集未正式支援，但可能會運作。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* <xref:signalr/scale>
* [Redis 文件](https://redis.io/documentation)
* [StackExchange Redis 文件](https://stackexchange.github.io/StackExchange.Redis/)
* [Azure Redis 快取文件](https://docs.microsoft.com/azure/redis-cache/)
