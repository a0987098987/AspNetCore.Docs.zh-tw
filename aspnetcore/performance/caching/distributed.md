---
title: 分散式快取的 ASP.NET Core
author: guardrex
description: 了解如何使用 ASP.NET Core 分散式快取來改善應用程式效能和延展性，特別是在雲端或伺服器陣列環境中。
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: performance/caching/distributed
ms.openlocfilehash: 7337ee3b823064c942832d8a44e4d4289bc4fd0e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899420"
---
# <a name="distributed-caching-in-aspnet-core"></a>分散式快取的 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)

分散式快取是由多個應用程式伺服器，通常做為存取它的應用程式伺服器的外部服務維護共用快取。 分散式快取可以改善的效能和延展性的 ASP.NET Core 應用程式，尤其是當應用程式由雲端服務或伺服器陣列。

分散式快取有其他快取的資料儲存在個別的應用程式伺服器的快取案例的幾項優點。

分散式快取的資料的時，資料：

* 已*一致*（一致） 跨多部伺服器的要求。
* 不受影響伺服器重新啟動和應用程式部署。
* 不會使用本機記憶體。

特定的實作分散式快取設定。 本文說明如何設定 SQL Server 和 Redis 分散式快取。 協力廠商實作也會提供的這類[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([在 GitHub 上的 NCache](https://github.com/Alachisoft/NCache))。 無論選取哪一個實作時，應用程式與快取使用的互動<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

::: moniker range=">= aspnetcore-2.2"

若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，並新增套件參考[Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。 Redis 封裝不包含在`Microsoft.AspNetCore.App`套件，因此您必須分別參考 Redis 封裝，專案檔中。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，並新增套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。 Redis 封裝不包含在`Microsoft.AspNetCore.App`套件，因此您必須分別參考 Redis 封裝，專案檔中。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或新增的套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。 Redis 套件包含在`Microsoft.AspNetCore.All`套件，因此您不需要參考個別專案檔中的 Redis 套件。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要使用 SQL Server 分散式快取中，新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 的分散式快取中，新增的套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。

::: moniker-end

## <a name="idistributedcache-interface"></a>IDistributedCache 介面

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作的分散式快取實作中的項目：

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;接受字串索引鍵，並擷取快取的項目為`byte[]`陣列，如果快取中找到。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;將項目 (做為`byte[]`陣列) 來使用字串索引鍵的快取。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;根據索引鍵，（如果有的話），重設其滑動的到期逾時的快取中的項目會重新整理。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;移除其字串索引鍵為基礎的快取項目。

## <a name="establish-distributed-caching-services"></a>建立分散式快取服務

註冊的實作<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>在`Startup.ConfigureServices`。 本主題中所述的架構提供實作包括：

* [分散式記憶體內部快取](#distributed-memory-cache)
* [分散式的 SQL Server 快取](#distributed-sql-server-cache)
* [分散式的 Redis 快取](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>分散式記憶體內部快取

分散式記憶體內部快取 (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) 是一種架構提供的實作<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>，儲存在記憶體中的項目。 分散式記憶體內部快取並不實際的分散式快取。 快取項目會儲存在應用程式執行所在的伺服器上的應用程式執行個體。

分散式記憶體內部快取是很有用的實作：

* 在開發和測試案例。
* 當一部伺服器用於生產環境和記憶體耗用量並不是問題。 實作分散式記憶體內部快取摘要快取資料儲存體。 它可讓實作真正分散式快取的解決方案，在未來如果，多個節點或容錯移轉會變得必要。

範例應用程式會利用分散式記憶體內部快取，在開發環境中執行應用程式時：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a>分散式的 SQL Server 快取

分散式的 SQL Server 快取實作 (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) 可讓分散式快取，以使用 SQL Server 資料庫作為其備份存放區。 若要建立 SQL Server 快取項目資料表中的 SQL Server 執行個體，您可以使用`sql-cache`工具。 此工具會建立資料表的名稱和您指定的結構描述。

::: moniker range="< aspnetcore-2.1"

新增`SqlConfig.Tools`要`<ItemGroup>`項目的專案檔與執行`dotnet restore`。

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

SQL Server 中建立資料表，藉由執行`sql-cache create`命令。 提供 SQL Server 執行個體 (`Data Source`)，資料庫 (`Initial Catalog`)，結構描述 (例如`dbo`)，和資料表名稱 (例如`TestCache`):

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

表示此工具已成功，會記錄的訊息：

```console
Table and index were created successfully.
```

所建立的資料表`sql-cache`工具有下列結構描述：

![Sql Server 快取表格](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> 應用程式應該操作使用的執行個體的快取值<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>，而非<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>。

此範例應用程式會實作<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>非開發環境中：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (並選擇性地<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>和<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) 通常儲存在原始檔控制外部 (比方說，藉由儲存[Secret Manager](xref:security/app-secrets)或是在*appsettings.json* /*appsettings。{Environment}.json*檔案)。 連接字串可能包含應保留從原始檔控制系統的認證。

### <a name="distributed-redis-cache"></a>分散式的 Redis 快取

[Redis](https://redis.io/)是開放原始碼記憶體中的資料存放區，通常是做為分散式快取。 您可以在本機，使用 Redis，您可以設定[Azure Redis 快取](https://azure.microsoft.com/services/cache/)Azure 託管的 ASP.NET Core 應用程式。 應用程式會設定快取實作使用<xref:Microsoft.Extensions.Caching.Redis.RedisCache>執行個體 (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

若要在本機電腦上安裝 Redis:

* 安裝[Chocolatey Redis 封裝](https://chocolatey.org/packages/redis-64/)。
* 執行`redis-server`從命令提示字元。

## <a name="use-the-distributed-cache"></a>使用分散式快取

若要使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面中，要求的執行個體<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>從應用程式中的任何建構函式。 提供執行個體[相依性插入 (DI)](xref:fundamentals/dependency-injection)。

當應用程式啟動時，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>插入至`Startup.Configure`。 使用快取的目前時間<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>(如需詳細資訊，請參閱[Web 主機：IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

範例應用程式會插入<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>成`IndexModel`供 [索引] 頁面。

每次載入 [索引] 頁面時，快取的時間，以檢查快取`OnGetAsync`。 如果尚未過期的快取的時間，則會顯示的時間。 如果經過 20 秒自前次存取快取的時間 （上一次此頁面已載入），頁面會顯示*快取的時間已過期*。

藉由選取立即更新為目前時間的快取的時間**重設快取時間** 按鈕。 按鈕觸發程序`OnPostResetCachedTime`處理常式方法。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> 若要使用的單一或範圍存留期不需要<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行個體 (至少為內建實作)。
>
> 您也可以建立<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行個體，只要您可能需要其中一個，而不是使用 DI，但在程式碼中建立的執行個體可以讓您的程式碼難以測試，且違反[明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。

## <a name="recommendations"></a>建議

當您決定哪一個實作時<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>最適合您的應用程式，請考慮下列：

* 現有的基礎結構
* 效能需求
* 成本
* Team 經驗

快取的解決方案通常依賴於記憶體中儲存體，以提供快速擷取快取的資料，但記憶體是有限的資源和成本以展開。 唯一存放區通常會使用快取中的資料。

一般而言，Redis 快取提供更高的輸送量和較低的延遲，比 SQL Server 快取。 不過，基準測試，通常需要動作來判斷快取策略的效能特性。

作為分散式快取備份存放區使用 SQL Server 時，快取與應用程式的一般資料存放區使用相同的資料庫，並擷取可以對這兩者的效能產生負面影響。 我們建議使用專用的 SQL Server 執行個體的分散式快取備份存放區。

## <a name="additional-resources"></a>其他資源

* [Redis 快取，在 Azure 上](https://azure.microsoft.com/documentation/services/redis-cache/)
* [在 Azure 上的 SQL 資料庫](https://azure.microsoft.com/documentation/services/sql-database/)
* [ASP.NET Core IDistributedCache NCache Web 伺服陣列中的提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)([GitHub 上的 NCache](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
