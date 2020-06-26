---
title: ASP.NET Core 相應放大的 Redis 背板 SignalR
author: bradygaster
description: 瞭解如何設定 Redis 背板，以啟用 ASP.NET Core 應用程式的相應放大 SignalR 。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/redis-backplane
ms.openlocfilehash: 58c1ff2c9334e75535f6e5f0f418976176822724
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408470"
---
# <a name="set-up-a-redis-backplane-for-aspnet-core-signalr-scale-out"></a>為 ASP.NET Core 相應放大設定 Redis 背板 SignalR

[Andrew Stanton-護士](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)和[Tom 作者: dykstra](https://github.com/tdykstra)，

本文說明 SignalR 設定[Redis](https://redis.io/)伺服器以用來相應放大 ASP.NET Core 應用程式的特定層面 SignalR 。

## <a name="set-up-a-redis-backplane"></a>設定 Redis 背板

* 部署 Redis 伺服器。

  > [!IMPORTANT] 
  > 針對生產用途，只有在與應用程式相同的資料中心內執行時，才建議使用 Redis 背板 SignalR 。 否則，網路延遲會降低效能。 如果您 SignalR 的應用程式是在 azure 雲端中執行，建議 SignalR 使用 azure 服務，而不是 Redis 背板。 您可以使用 Azure Redis 快取服務進行開發和測試環境。

  如需詳細資訊，請參閱下列資源：

  * <xref:signalr/scale>
  * [Redis 文件](https://redis.io/)
  * [Azure Redis 快取文件](https://docs.microsoft.com/azure/redis-cache/)

::: moniker range="= aspnetcore-2.1"

* 在 SignalR 應用程式中，安裝 `Microsoft.AspNetCore.SignalR.Redis` NuGet 套件。
* 在 `Startup.ConfigureServices` 方法中，呼叫 `AddRedis` after `AddSignalR` ：

  ```csharp
  services.AddSignalR().AddRedis("<your_Redis_connection_string>");
  ```

* 視需要設定選項：
 
  大部分選項都可以在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件中設定。 中指定的選項會覆 `ConfigurationOptions` 寫連接字串中的設定。

  下列範例顯示如何在物件中設定選項 `ConfigurationOptions` 。 這個範例會新增通道前置詞，讓多個應用程式可以共用相同的 Redis 實例，如下列步驟所述。

  ```csharp
  services.AddSignalR()
    .AddRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  在上述程式碼中， `options.Configuration` 會使用連接字串中指定的內容來初始化。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* 在 SignalR 應用程式中，安裝下列其中一個 NuGet 套件：

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`-相依于 Stackexchange.redis. Redis 2. X.X。 這是建議用於 ASP.NET Core 2.2 和更新版本的套件。
  * `Microsoft.AspNetCore.SignalR.Redis`-相依于 Stackexchange.redis. Redis 1. X.X。 此套件不包含在 ASP.NET Core 3.0 和更新版本中。

* 在 `Startup.ConfigureServices` 方法中，呼叫 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*> ：

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```

 使用時 `Microsoft.AspNetCore.SignalR.Redis` ，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*> 。

* 視需要設定選項：
 
  大部分選項都可以在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件中設定。 中指定的選項會覆 `ConfigurationOptions` 寫連接字串中的設定。

  下列範例顯示如何在物件中設定選項 `ConfigurationOptions` 。 這個範例會新增通道前置詞，讓多個應用程式可以共用相同的 Redis 實例，如下列步驟所述。

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

 使用時 `Microsoft.AspNetCore.SignalR.Redis` ，請呼叫 <xref:Microsoft.Extensions.DependencyInjection.RedisDependencyInjectionExtensions.AddRedis*> 。

  在上述程式碼中， `options.Configuration` 會使用連接字串中指定的內容來初始化。

  如需 Redis 選項的詳細資訊，請參閱[Stackexchange.redis Redis 檔](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

* 在 SignalR 應用程式中，安裝下列 NuGet 套件：

  * `Microsoft.AspNetCore.SignalR.StackExchangeRedis`
  
* 在 `Startup.ConfigureServices` 方法中，呼叫 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisDependencyInjectionExtensions.AddStackExchangeRedis*> ：

  ```csharp
  services.AddSignalR().AddStackExchangeRedis("<your_Redis_connection_string>");
  ```
  
* 視需要設定選項：
 
  大部分選項都可以在連接字串或[ConfigurationOptions](https://stackexchange.github.io/StackExchange.Redis/Configuration#configuration-options)物件中設定。 中指定的選項會覆 `ConfigurationOptions` 寫連接字串中的設定。

  下列範例顯示如何在物件中設定選項 `ConfigurationOptions` 。 這個範例會新增通道前置詞，讓多個應用程式可以共用相同的 Redis 實例，如下列步驟所述。

  ```csharp
  services.AddSignalR()
    .AddStackExchangeRedis(connectionString, options => {
        options.Configuration.ChannelPrefix = "MyApp";
    });
  ```

  在上述程式碼中， `options.Configuration` 會使用連接字串中指定的內容來初始化。

  如需 Redis 選項的詳細資訊，請參閱[Stackexchange.redis Redis 檔](https://stackexchange.github.io/StackExchange.Redis/Configuration.html)。

::: moniker-end

* 如果您針對多個應用程式使用一部 Redis 伺服器 SignalR ，請為每個應用程式使用不同的通道首碼 SignalR 。

  設定通道前置詞會隔離另一個 SignalR 應用程式與其他使用不同通道首碼的 app。 如果您未指派不同的前置詞，則從某個應用程式傳送到其所有用戶端的訊息，將會移至使用 Redis 伺服器做為背板的所有應用程式的所有用戶端。

* 針對粘滯會話設定伺服器陣列的負載平衡軟體。 以下是有關如何執行此動作的一些檔範例：

  * [IIS](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing)
  * [HAProxy](https://www.haproxy.com/blog/load-balancing-affinity-persistence-sticky-sessions-what-you-need-to-know/)
  * [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/#sticky)
  * [pfSense](https://www.netgate.com/docs/pfsense/loadbalancing/inbound-load-balancing.html#sticky-connections)

## <a name="redis-server-errors"></a>Redis 伺服器錯誤

當 Redis 伺服器關閉時，會擲回 SignalR 指出不會傳遞訊息的例外狀況。 一些一般的例外狀況訊息：

* *無法寫入訊息*
* *無法叫用中樞方法 ' 方法名稱 '*
* *連接至 Redis 失敗*

SignalR當伺服器恢復運作時，不會緩衝處理訊息以傳送它們。 Redis 伺服器關閉時傳送的任何訊息都會遺失。

SignalR當 Redis 伺服器再次可供使用時，自動重新連接。

### <a name="custom-behavior-for-connection-failures"></a>連接失敗的自訂行為

以下範例示範如何處理 Redis 連接失敗事件。

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

## <a name="redis-clustering"></a>Redis 叢集

[Redis](https://redis.io/topics/cluster-spec)叢集是使用多部 Redis 伺服器達到高可用性的方法。 叢集並未正式支援，但可能會有作用。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱下列資源：

* <xref:signalr/scale>
* [Redis 文件](https://redis.io/documentation)
* [Stackexchange.redis Redis 檔](https://stackexchange.github.io/StackExchange.Redis/)
* [Azure Redis 快取文件](https://docs.microsoft.com/azure/redis-cache/)
