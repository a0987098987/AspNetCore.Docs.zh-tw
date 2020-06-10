---
title: ASP.NET Core 中的分散式快取
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 分散式快取來改善應用程式效能和擴充性，尤其是在雲端或伺服器陣列環境中。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/caching/distributed
ms.openlocfilehash: d655e48f9504d337b0ffdbd6819f32101730310b
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84106698"
---
# <a name="distributed-caching-in-aspnet-core"></a>ASP.NET Core 中的分散式快取

作者： [Mohsin Nasir](https://github.com/mohsinnasir)和[Steve Smith](https://ardalis.com/)

::: moniker range=">= aspnetcore-3.0"

「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。 分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。

分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。

散發快取的資料時，資料：

* 跨多部*伺服器的要求之間具有一致*（一致）。
* 不受伺服器重新開機和應用程式部署。
* 不使用本機記憶體。

分散式快取設定是特定的執行。 本文說明如何設定 SQL Server 和 Redis 分散式快取。 協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。 無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="prerequisites"></a>必要條件

若要使用 SQL Server 分散式快取，請將套件參考新增至[Microsoft. extension. cache. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 分散式快取，請將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)套件。

若要使用 NCache 分散式快取，請將套件參考新增至[NCache. 開放原始碼](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。

## <a name="idistributedcache-interface"></a>IDistributedCache 介面

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。

## <a name="establish-distributed-caching-services"></a>建立分散式快取服務

在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。 本主題所描述的架構提供的實作為包括：

* [分散式記憶體快取](#distributed-memory-cache)
* [分散式 SQL Server 快取](#distributed-sql-server-cache)
* [分散式 Redis 快取](#distributed-redis-cache)
* [分散式 NCache 快取](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a>分散式記憶體快取

分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。 分散式記憶體快取不是實際的分散式快取。 在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。

分散式記憶體快取是一種實用的執行方式：

* 在開發和測試案例中。
* 在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。 執行分散式記憶體快取會抽象化快取的資料存放區。 如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。

當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a>分散式 SQL Server 快取

分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。 若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。 此工具會建立一個資料表，其中包含您指定的名稱和架構。

執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。 提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

會記錄一則訊息，指出工具已成功：

```console
Table and index were created successfully.
```

此工具所建立的資料表 `sql-cache` 具有下列架構：

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> 應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。

範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*appsettings 儲存） / *。環境} json*檔案）。 連接字串可能包含應保留在原始檔控制系統中的認證。

### <a name="distributed-redis-cache"></a>分散式 Redis 快取

[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。 您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。

應用程式會在的 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 非開發環境中，使用實例（）來設定快取執行 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*> `Startup.ConfigureServices` ：

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

若要在本機電腦上安裝 Redis：

1. 安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。
1. `redis-server`從命令提示字元執行。

### <a name="distributed-ncache-cache"></a>分散式 NCache 快取

[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。 NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。

若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。

若要設定 NCache：

1. 安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。
1. 在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。
1. 將下列程式碼新增至 `Startup.ConfigureServices`：

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a>使用分散式快取

若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。 實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。

當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。 目前的時間是使用快取的 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> （如需詳細資訊，請參閱[泛型主機： IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)）：

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。

每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。 如果快取的時間尚未過期，則會顯示時間。 如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。

選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。 按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> 實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。
>
> 您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。

## <a name="recommendations"></a>建議

當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：

* 現有基礎結構
* 效能需求
* 成本
* 小組經驗

快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。 只會將常用的資料儲存在快取中。

一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。 不過，通常需要進行基準測試，以判斷快取策略的效能特性。

當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。 我們建議使用分散式快取備份存放區的專用 SQL Server 實例。

## <a name="additional-resources"></a>其他資源

* [Azure 上的 Redis 快取](/azure/azure-cache-for-redis/)
* [Azure 上的 SQL Database](/azure/sql-database/)
* [Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。 分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。

分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。

散發快取的資料時，資料：

* 跨多部*伺服器的要求之間具有一致*（一致）。
* 不受伺服器重新開機和應用程式部署。
* 不使用本機記憶體。

分散式快取設定是特定的執行。 本文說明如何設定 SQL Server 和 Redis 分散式快取。 協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。 無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="prerequisites"></a>必要條件

若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。 Redis 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 Redis 套件。

若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。 NCache 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 NCache 套件。

## <a name="idistributedcache-interface"></a>IDistributedCache 介面

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。

## <a name="establish-distributed-caching-services"></a>建立分散式快取服務

在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。 本主題所描述的架構提供的實作為包括：

* [分散式記憶體快取](#distributed-memory-cache)
* [分散式 SQL Server 快取](#distributed-sql-server-cache)
* [分散式 Redis 快取](#distributed-redis-cache)
* [分散式 NCache 快取](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a>分散式記憶體快取

分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。 分散式記憶體快取不是實際的分散式快取。 在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。

分散式記憶體快取是一種實用的執行方式：

* 在開發和測試案例中。
* 在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。 執行分散式記憶體快取會抽象化快取的資料存放區。 如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。

當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a>分散式 SQL Server 快取

分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。 若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。 此工具會建立一個資料表，其中包含您指定的名稱和架構。

執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。 提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

會記錄一則訊息，指出工具已成功：

```console
Table and index were created successfully.
```

此工具所建立的資料表 `sql-cache` 具有下列架構：

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> 應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。

範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*appsettings 儲存） / *。環境} json*檔案）。 連接字串可能包含應保留在原始檔控制系統中的認證。

### <a name="distributed-redis-cache"></a>分散式 Redis 快取

[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。 您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。

應用程式會在的 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 非開發環境中，使用實例（）來設定快取執行 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*> `Startup.ConfigureServices` ：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

若要在本機電腦上安裝 Redis：

1. 安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。
1. `redis-server`從命令提示字元執行。

### <a name="distributed-ncache-cache"></a>分散式 NCache 快取

[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。 NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。

若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。

若要設定 NCache：

1. 安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。
1. 在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。
1. 將下列程式碼新增至 `Startup.ConfigureServices`：

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a>使用分散式快取

若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。 實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。

當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。 目前的時間是使用快取的 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> （如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。

每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。 如果快取的時間尚未過期，則會顯示時間。 如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。

選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。 按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> 實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。
>
> 您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。

## <a name="recommendations"></a>建議

當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：

* 現有基礎結構
* 效能需求
* 成本
* 小組經驗

快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。 只會將常用的資料儲存在快取中。

一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。 不過，通常需要進行基準測試，以判斷快取策略的效能特性。

當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。 我們建議使用分散式快取備份存放區的專用 SQL Server 實例。

## <a name="additional-resources"></a>其他資源

* [Azure 上的 Redis 快取](/azure/azure-cache-for-redis/)
* [Azure 上的 SQL Database](/azure/sql-database/)
* [Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。 分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。

分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。

散發快取的資料時，資料：

* 跨多部*伺服器的要求之間具有一致*（一致）。
* 不受伺服器重新開機和應用程式部署。
* 不使用本機記憶體。

分散式快取設定是特定的執行。 本文說明如何設定 SQL Server 和 Redis 分散式快取。 協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。 無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="prerequisites"></a>必要條件

若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。

若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。 Redis 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 Redis 套件。

若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。 NCache 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 NCache 套件。

## <a name="idistributedcache-interface"></a>IDistributedCache 介面

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。

## <a name="establish-distributed-caching-services"></a>建立分散式快取服務

在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。 本主題所描述的架構提供的實作為包括：

* [分散式記憶體快取](#distributed-memory-cache)
* [分散式 SQL Server 快取](#distributed-sql-server-cache)
* [分散式 Redis 快取](#distributed-redis-cache)
* [分散式 NCache 快取](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a>分散式記憶體快取

分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。 分散式記憶體快取不是實際的分散式快取。 在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。

分散式記憶體快取是一種實用的執行方式：

* 在開發和測試案例中。
* 在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。 執行分散式記憶體快取會抽象化快取的資料存放區。 如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。

當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a>分散式 SQL Server 快取

分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。 若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。 此工具會建立一個資料表，其中包含您指定的名稱和架構。

執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。 提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

會記錄一則訊息，指出工具已成功：

```console
Table and index were created successfully.
```

此工具所建立的資料表 `sql-cache` 具有下列架構：

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> 應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。

範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*appsettings 儲存） / *。環境} json*檔案）。 連接字串可能包含應保留在原始檔控制系統中的認證。

### <a name="distributed-redis-cache"></a>分散式 Redis 快取

[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。 您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。

應用程式會使用實例（）來設定快取執行 <xref:Microsoft.Extensions.Caching.Redis.RedisCache> <xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*> ：

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

若要在本機電腦上安裝 Redis：

1. 安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。
1. `redis-server`從命令提示字元執行。

### <a name="distributed-ncache-cache"></a>分散式 NCache 快取

[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。 NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。

若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。

若要設定 NCache：

1. 安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。
1. 在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。
1. 將下列程式碼新增至 `Startup.ConfigureServices`：

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a>使用分散式快取

若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。 實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。

當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。 目前的時間是使用快取的 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> （如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。

每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。 如果快取的時間尚未過期，則會顯示時間。 如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。

選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。 按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> 實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。
>
> 您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。

## <a name="recommendations"></a>建議

當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：

* 現有基礎結構
* 效能需求
* 成本
* 小組經驗

快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。 只會將常用的資料儲存在快取中。

一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。 不過，通常需要進行基準測試，以判斷快取策略的效能特性。

當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。 我們建議使用分散式快取備份存放區的專用 SQL Server 實例。

## <a name="additional-resources"></a>其他資源

* [Azure 上的 Redis 快取](/azure/azure-cache-for-redis/)
* [Azure 上的 SQL Database](/azure/sql-database/)
* [Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end
 