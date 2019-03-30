---
title: 分散式快取的 ASP.NET Core
author: guardrex
description: 了解如何使用 ASP.NET Core 分散式快取來改善應用程式效能和延展性，特別是在雲端或伺服器陣列環境中。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: performance/caching/distributed
ms.openlocfilehash: c3774c26116a4cb70386d0060f2244d224fec8e1
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750993"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="f4bb1-103">分散式快取的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4bb1-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="f4bb1-104">藉由[Luke Latham](https://github.com/guardrex)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f4bb1-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f4bb1-105">分散式快取是由多個應用程式伺服器，通常做為存取它的應用程式伺服器的外部服務維護共用快取。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="f4bb1-106">分散式快取可以改善的效能和延展性的 ASP.NET Core 應用程式，尤其是當應用程式由雲端服務或伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="f4bb1-107">分散式快取有其他快取的資料儲存在個別的應用程式伺服器的快取案例的幾項優點。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="f4bb1-108">分散式快取的資料的時，資料：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="f4bb1-109">已*一致*（一致） 跨多部伺服器的要求。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="f4bb1-110">不受影響伺服器重新啟動和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="f4bb1-111">不會使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-111">Doesn't use local memory.</span></span>

<span data-ttu-id="f4bb1-112">特定的實作分散式快取設定。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="f4bb1-113">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="f4bb1-114">協力廠商實作也會提供的這類[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([在 GitHub 上的 NCache](https://github.com/Alachisoft/NCache))。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="f4bb1-115">無論選取哪一個實作時，應用程式與快取使用的互動<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="f4bb1-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4bb1-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4bb1-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="f4bb1-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f4bb1-118">若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-118">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="f4bb1-119">若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，並新增套件參考[Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-119">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="f4bb1-120">Redis 封裝不包含在`Microsoft.AspNetCore.App`套件，因此您必須分別參考 Redis 封裝，專案檔中。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-120">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f4bb1-121">若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="f4bb1-122">若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，並新增套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="f4bb1-123">Redis 封裝不包含在`Microsoft.AspNetCore.App`套件，因此您必須分別參考 Redis 封裝，專案檔中。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="f4bb1-124">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="f4bb1-124">IDistributedCache interface</span></span>

<span data-ttu-id="f4bb1-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作的分散式快取實作中的項目：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-125">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="f4bb1-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;接受字串索引鍵，並擷取快取的項目為`byte[]`陣列，如果快取中找到。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="f4bb1-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;將項目 (做為`byte[]`陣列) 來使用字串索引鍵的快取。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="f4bb1-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;根據索引鍵，（如果有的話），重設其滑動的到期逾時的快取中的項目會重新整理。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="f4bb1-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;移除其字串索引鍵為基礎的快取項目。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="f4bb1-130">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="f4bb1-130">Establish distributed caching services</span></span>

<span data-ttu-id="f4bb1-131">註冊的實作<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>在`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-131">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f4bb1-132">本主題中所述的架構提供實作包括：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-132">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="f4bb1-133">分散式記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-133">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="f4bb1-134">分散式的 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-134">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="f4bb1-135">分散式的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-135">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="f4bb1-136">分散式記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-136">Distributed Memory Cache</span></span>

<span data-ttu-id="f4bb1-137">分散式記憶體內部快取 (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) 是一種架構提供的實作<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>，儲存在記憶體中的項目。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-137">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="f4bb1-138">分散式記憶體內部快取並不實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-138">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="f4bb1-139">快取項目會儲存在應用程式執行所在的伺服器上的應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-139">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="f4bb1-140">分散式記憶體內部快取是很有用的實作：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-140">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="f4bb1-141">在開發和測試案例。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-141">In development and testing scenarios.</span></span>
* <span data-ttu-id="f4bb1-142">當一部伺服器用於生產環境和記憶體耗用量並不是問題。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-142">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="f4bb1-143">實作分散式記憶體內部快取摘要快取資料儲存體。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-143">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="f4bb1-144">它可讓實作真正分散式快取的解決方案，在未來如果，多個節點或容錯移轉會變得必要。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-144">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="f4bb1-145">範例應用程式會利用分散式記憶體內部快取中的開發環境中執行應用程式時`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f4bb1-145">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="f4bb1-146">分散式的 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-146">Distributed SQL Server Cache</span></span>

<span data-ttu-id="f4bb1-147">分散式的 SQL Server 快取實作 (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) 可讓分散式快取，以使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-147">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="f4bb1-148">若要建立 SQL Server 快取項目資料表中的 SQL Server 執行個體，您可以使用`sql-cache`工具。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-148">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="f4bb1-149">此工具會建立資料表的名稱和您指定的結構描述。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-149">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="f4bb1-150">SQL Server 中建立資料表，藉由執行`sql-cache create`命令。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-150">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="f4bb1-151">提供 SQL Server 執行個體 (`Data Source`)，資料庫 (`Initial Catalog`)，結構描述 (例如`dbo`)，和資料表名稱 (例如`TestCache`):</span><span class="sxs-lookup"><span data-stu-id="f4bb1-151">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="f4bb1-152">表示此工具已成功，會記錄的訊息：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-152">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="f4bb1-153">所建立的資料表`sql-cache`工具有下列結構描述：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-153">The table created by the `sql-cache` tool has the following schema:</span></span>

![Sql Server 快取表格](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="f4bb1-155">應用程式應該操作使用的執行個體的快取值<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>，而非<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-155">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="f4bb1-156">此範例應用程式會實作<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>非開發環境中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f4bb1-156">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="f4bb1-157">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (並選擇性地<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>和<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) 通常儲存在原始檔控制外部 (比方說，藉由儲存[Secret Manager](xref:security/app-secrets)或是在*appsettings.json* /*appsettings。{ENVIRONMENT}.json*檔案)。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-157">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="f4bb1-158">連接字串可能包含應保留從原始檔控制系統的認證。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-158">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="f4bb1-159">分散式的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-159">Distributed Redis Cache</span></span>

<span data-ttu-id="f4bb1-160">[Redis](https://redis.io/)是開放原始碼記憶體中的資料存放區，通常是做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-160">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="f4bb1-161">您可以在本機，使用 Redis，您可以設定[Azure Redis 快取](https://azure.microsoft.com/services/cache/)Azure 託管的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-161">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="f4bb1-162">應用程式會設定快取實作使用<xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache>執行個體 (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) 中的非開發環境中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f4bb1-162">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="f4bb1-163">應用程式會設定快取實作使用<xref:Microsoft.Extensions.Caching.Redis.RedisCache>執行個體 (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="f4bb1-163">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="f4bb1-164">若要在本機電腦上安裝 Redis:</span><span class="sxs-lookup"><span data-stu-id="f4bb1-164">To install Redis on your local machine:</span></span>

* <span data-ttu-id="f4bb1-165">安裝[Chocolatey Redis 封裝](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-165">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="f4bb1-166">執行`redis-server`從命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-166">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="f4bb1-167">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="f4bb1-167">Use the distributed cache</span></span>

<span data-ttu-id="f4bb1-168">若要使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面中，要求的執行個體<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>從應用程式中的任何建構函式。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-168">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="f4bb1-169">提供執行個體[相依性插入 (DI)](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-169">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="f4bb1-170">當應用程式啟動時，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>插入至`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-170">When the app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="f4bb1-171">使用快取的目前時間<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>(如需詳細資訊，請參閱[Web 主機：IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="f4bb1-171">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="f4bb1-172">範例應用程式會插入<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>成`IndexModel`供 [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-172">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="f4bb1-173">每次載入 [索引] 頁面時，快取的時間，以檢查快取`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-173">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="f4bb1-174">如果尚未過期的快取的時間，則會顯示的時間。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-174">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="f4bb1-175">如果經過 20 秒自前次存取快取的時間 （上一次此頁面已載入），頁面會顯示*快取的時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-175">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="f4bb1-176">藉由選取立即更新為目前時間的快取的時間**重設快取時間** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-176">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="f4bb1-177">按鈕觸發程序`OnPostResetCachedTime`處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-177">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="f4bb1-178">若要使用的單一或範圍存留期不需要<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行個體 (至少為內建實作)。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-178">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="f4bb1-179">您也可以建立<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行個體，只要您可能需要其中一個，而不是使用 DI，但在程式碼中建立的執行個體可以讓您的程式碼難以測試，且違反[明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-179">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="f4bb1-180">建議</span><span class="sxs-lookup"><span data-stu-id="f4bb1-180">Recommendations</span></span>

<span data-ttu-id="f4bb1-181">當您決定哪一個實作時<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>最適合您的應用程式，請考慮下列：</span><span class="sxs-lookup"><span data-stu-id="f4bb1-181">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="f4bb1-182">現有的基礎結構</span><span class="sxs-lookup"><span data-stu-id="f4bb1-182">Existing infrastructure</span></span>
* <span data-ttu-id="f4bb1-183">效能需求</span><span class="sxs-lookup"><span data-stu-id="f4bb1-183">Performance requirements</span></span>
* <span data-ttu-id="f4bb1-184">成本</span><span class="sxs-lookup"><span data-stu-id="f4bb1-184">Cost</span></span>
* <span data-ttu-id="f4bb1-185">Team 經驗</span><span class="sxs-lookup"><span data-stu-id="f4bb1-185">Team experience</span></span>

<span data-ttu-id="f4bb1-186">快取的解決方案通常依賴於記憶體中儲存體，以提供快速擷取快取的資料，但記憶體是有限的資源和成本以展開。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-186">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="f4bb1-187">唯一存放區通常會使用快取中的資料。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-187">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="f4bb1-188">一般而言，Redis 快取提供更高的輸送量和較低的延遲，比 SQL Server 快取。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-188">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="f4bb1-189">不過，基準測試，通常需要動作來判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-189">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="f4bb1-190">作為分散式快取備份存放區使用 SQL Server 時，快取與應用程式的一般資料存放區使用相同的資料庫，並擷取可以對這兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-190">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="f4bb1-191">我們建議使用專用的 SQL Server 執行個體的分散式快取備份存放區。</span><span class="sxs-lookup"><span data-stu-id="f4bb1-191">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4bb1-192">其他資源</span><span class="sxs-lookup"><span data-stu-id="f4bb1-192">Additional resources</span></span>

* [<span data-ttu-id="f4bb1-193">Redis 快取，在 Azure 上</span><span class="sxs-lookup"><span data-stu-id="f4bb1-193">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="f4bb1-194">在 Azure 上的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="f4bb1-194">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="f4bb1-195">[ASP.NET Core IDistributedCache NCache Web 伺服陣列中的提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)([GitHub 上的 NCache](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="f4bb1-195">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
