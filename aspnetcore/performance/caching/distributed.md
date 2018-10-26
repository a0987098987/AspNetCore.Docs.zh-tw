---
title: 分散式快取的 ASP.NET Core
author: guardrex
description: 了解如何使用 ASP.NET Core 分散式快取來改善應用程式效能和延展性，特別是在雲端或伺服器陣列環境中。
ms.author: riande
ms.custom: mvc
ms.date: 10/19/2018
uid: performance/caching/distributed
ms.openlocfilehash: 46a93125e8b25a66b5a1ead3b72c55db146b5a10
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090559"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="67769-103">分散式快取的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67769-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="67769-104">作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="67769-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="67769-105">分散式快取是由多個應用程式伺服器，通常做為存取它的應用程式伺服器的外部服務維護共用快取。</span><span class="sxs-lookup"><span data-stu-id="67769-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="67769-106">分散式快取可以改善的效能和延展性的 ASP.NET Core 應用程式，尤其是當應用程式由雲端服務或伺服器陣列。</span><span class="sxs-lookup"><span data-stu-id="67769-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="67769-107">分散式快取有其他快取的資料儲存在個別的應用程式伺服器的快取案例的幾項優點。</span><span class="sxs-lookup"><span data-stu-id="67769-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="67769-108">分散式快取的資料的時，資料：</span><span class="sxs-lookup"><span data-stu-id="67769-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="67769-109">已*一致*（一致） 跨多部伺服器的要求。</span><span class="sxs-lookup"><span data-stu-id="67769-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="67769-110">不受影響伺服器重新啟動和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="67769-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="67769-111">不會使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="67769-111">Doesn't use local memory.</span></span>

<span data-ttu-id="67769-112">特定的實作分散式快取設定。</span><span class="sxs-lookup"><span data-stu-id="67769-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="67769-113">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="67769-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="67769-114">協力廠商實作也會提供的這類[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([在 GitHub 上的 NCache](https://github.com/Alachisoft/NCache))。</span><span class="sxs-lookup"><span data-stu-id="67769-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="67769-115">無論選取哪一個實作時，應用程式與快取使用的互動<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面。</span><span class="sxs-lookup"><span data-stu-id="67769-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="67769-116">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="67769-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67769-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="67769-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="67769-118">若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="67769-118">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="67769-119">若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，並新增套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="67769-119">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="67769-120">Redis 封裝不包含在`Microsoft.AspNetCore.App`套件，因此您必須分別參考 Redis 封裝，專案檔中。</span><span class="sxs-lookup"><span data-stu-id="67769-120">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="67769-121">若要使用 SQL Server 分散式快取中，參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="67769-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="67769-122">若要使用 Redis 的分散式快取中，參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或新增的套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="67769-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="67769-123">Redis 套件包含在`Microsoft.AspNetCore.All`套件，因此您不需要參考個別專案檔中的 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="67769-123">The Redis package is included in `Microsoft.AspNetCore.All` package, so you don't need to reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="67769-124">若要使用 SQL Server 分散式快取中，新增的套件參考[Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="67769-124">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="67769-125">若要使用 Redis 的分散式快取中，新增的套件參考[Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="67769-125">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="67769-126">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="67769-126">IDistributedCache interface</span></span>

<span data-ttu-id="67769-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作的分散式快取實作中的項目：</span><span class="sxs-lookup"><span data-stu-id="67769-127">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="67769-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;接受字串索引鍵，並擷取快取的項目為`byte[]`陣列，如果快取中找到。</span><span class="sxs-lookup"><span data-stu-id="67769-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="67769-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;將項目 (做為`byte[]`陣列) 來使用字串索引鍵的快取。</span><span class="sxs-lookup"><span data-stu-id="67769-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="67769-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash;根據索引鍵，（如果有的話），重設其滑動的到期逾時的快取中的項目會重新整理。</span><span class="sxs-lookup"><span data-stu-id="67769-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="67769-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;移除其字串索引鍵為基礎的快取項目。</span><span class="sxs-lookup"><span data-stu-id="67769-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="67769-132">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="67769-132">Establish distributed caching services</span></span>

<span data-ttu-id="67769-133">註冊的實作<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>在`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="67769-133">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="67769-134">本主題中所述的架構提供實作包括：</span><span class="sxs-lookup"><span data-stu-id="67769-134">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="67769-135">分散式記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="67769-135">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="67769-136">分散式的 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="67769-136">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="67769-137">分散式的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="67769-137">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="67769-138">分散式記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="67769-138">Distributed Memory Cache</span></span>

<span data-ttu-id="67769-139">分散式記憶體內部快取 (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) 是一種架構提供的實作`IDistributedCache`，儲存在記憶體中的項目。</span><span class="sxs-lookup"><span data-stu-id="67769-139">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of `IDistributedCache` that stores items in memory.</span></span> <span data-ttu-id="67769-140">分散式記憶體內部快取並不實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="67769-140">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="67769-141">快取項目會儲存在應用程式執行所在的伺服器上的應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="67769-141">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="67769-142">分散式記憶體內部快取是很有用的實作：</span><span class="sxs-lookup"><span data-stu-id="67769-142">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="67769-143">在開發和測試案例。</span><span class="sxs-lookup"><span data-stu-id="67769-143">In development and testing scenarios.</span></span>
* <span data-ttu-id="67769-144">當一部伺服器用於生產環境和記憶體耗用量並不是問題。</span><span class="sxs-lookup"><span data-stu-id="67769-144">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="67769-145">實作分散式記憶體內部快取摘要快取資料儲存體。</span><span class="sxs-lookup"><span data-stu-id="67769-145">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="67769-146">它可讓實作真正分散式快取的解決方案，在未來如果，多個節點或容錯移轉會變得必要。</span><span class="sxs-lookup"><span data-stu-id="67769-146">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="67769-147">範例應用程式會利用分散式記憶體內部快取，在開發環境中執行應用程式時：</span><span class="sxs-lookup"><span data-stu-id="67769-147">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="67769-148">分散式的 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="67769-148">Distributed SQL Server Cache</span></span>

<span data-ttu-id="67769-149">分散式的 SQL Server 快取實作 (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) 可讓分散式快取，以使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="67769-149">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="67769-150">若要建立 SQL Server 快取項目資料表中的 SQL Server 執行個體，您可以使用`sql-cache`工具。</span><span class="sxs-lookup"><span data-stu-id="67769-150">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="67769-151">此工具會建立資料表的名稱和您指定的結構描述。</span><span class="sxs-lookup"><span data-stu-id="67769-151">The tool creates a table with the name and schema that you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="67769-152">新增`SqlConfig.Tools`要`<ItemGroup>`項目的專案檔與執行`dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="67769-152">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="67769-153">SQL Server 中建立資料表，藉由執行`sql-cache create`命令。</span><span class="sxs-lookup"><span data-stu-id="67769-153">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="67769-154">提供 SQL Server 執行個體 (`Data Source`)，資料庫 (`Initial Catalog`)，結構描述 (例如`dbo`)，和資料表名稱 (例如`TestCache`):</span><span class="sxs-lookup"><span data-stu-id="67769-154">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="67769-155">表示此工具已成功，會記錄的訊息：</span><span class="sxs-lookup"><span data-stu-id="67769-155">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="67769-156">所建立的資料表`sql-cache`工具有下列結構描述：</span><span class="sxs-lookup"><span data-stu-id="67769-156">The table created by the `sql-cache` tool has the following schema:</span></span>

![Sql Server 快取表格](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="67769-158">應用程式應該操作使用的執行個體的快取值<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>，而非<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>。</span><span class="sxs-lookup"><span data-stu-id="67769-158">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="67769-159">此範例應用程式會實作<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>非開發環境中：</span><span class="sxs-lookup"><span data-stu-id="67769-159">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> <span data-ttu-id="67769-160">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (並選擇性地<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>和<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) 通常儲存在原始檔控制外部 (比方說，藉由儲存[Secret Manager](xref:security/app-secrets)或是在*appsettings.json* /*appsettings。{Environment}.json*檔案)。</span><span class="sxs-lookup"><span data-stu-id="67769-160">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{Environment}.json* files).</span></span> <span data-ttu-id="67769-161">連接字串可能包含應保留從原始檔控制系統的認證。</span><span class="sxs-lookup"><span data-stu-id="67769-161">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="67769-162">分散式的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="67769-162">Distributed Redis Cache</span></span>

<span data-ttu-id="67769-163">[Redis](https://redis.io/)是開放原始碼記憶體中的資料存放區，通常是做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="67769-163">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="67769-164">您可以在本機，使用 Redis，您可以設定[Azure Redis 快取](https://azure.microsoft.com/services/cache/)Azure 託管的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="67769-164">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span> <span data-ttu-id="67769-165">應用程式會設定快取實作使用<xref:Microsoft.Extensions.Caching.Redis.RedisCache>執行個體 (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="67769-165">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

<span data-ttu-id="67769-166">若要在本機電腦上安裝 Redis:</span><span class="sxs-lookup"><span data-stu-id="67769-166">To install Redis on your local machine:</span></span>

* <span data-ttu-id="67769-167">安裝[Chocolatey Redis 封裝](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="67769-167">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="67769-168">執行`redis-server`從命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="67769-168">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="67769-169">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="67769-169">Use the distributed cache</span></span>

<span data-ttu-id="67769-170">若要使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面中，要求的執行個體`IDistributedCache`從應用程式中的任何建構函式。</span><span class="sxs-lookup"><span data-stu-id="67769-170">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of `IDistributedCache` from any constructor in the app.</span></span> <span data-ttu-id="67769-171">提供執行個體[相依性插入 (DI)](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="67769-171">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="67769-172">當應用程式啟動時，`IDistributedCache`插入至`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="67769-172">When the app starts, `IDistributedCache` is injected into `Startup.Configure`.</span></span> <span data-ttu-id="67769-173">使用快取的目前時間<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>(如需詳細資訊，請參閱 < [Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="67769-173">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="67769-174">範例應用程式會插入`IDistributedCache`成`IndexModel`供 [索引] 頁面。</span><span class="sxs-lookup"><span data-stu-id="67769-174">The sample app injects `IDistributedCache` into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="67769-175">每次載入 [索引] 頁面時，快取的時間，以檢查快取`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="67769-175">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="67769-176">如果尚未過期的快取的時間，則會顯示的時間。</span><span class="sxs-lookup"><span data-stu-id="67769-176">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="67769-177">如果經過 20 秒自前次存取快取的時間 （上一次此頁面已載入），頁面會顯示*快取的時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="67769-177">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="67769-178">藉由選取立即更新為目前時間的快取的時間**重設快取時間** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="67769-178">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="67769-179">按鈕觸發程序`OnPostResetCachedTime`處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="67769-179">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="67769-180">若要使用的單一或範圍存留期不需要`IDistributedCache`執行個體 (至少為內建實作)。</span><span class="sxs-lookup"><span data-stu-id="67769-180">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="67769-181">您也可以建立`IDistributedCache`執行個體，只要您可能需要其中一個，而不是使用 DI，但在程式碼中建立的執行個體可以讓您的程式碼難以測試，且違反[明確相依性準則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="67769-181">You can also create an `IDistributedCache` instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="67769-182">建議</span><span class="sxs-lookup"><span data-stu-id="67769-182">Recommendations</span></span>

<span data-ttu-id="67769-183">當您決定哪一個實作時`IDistributedCache`最適合您的應用程式，請考慮下列：</span><span class="sxs-lookup"><span data-stu-id="67769-183">When deciding which implementation of `IDistributedCache` is best for your app, consider the following:</span></span>

* <span data-ttu-id="67769-184">現有的基礎結構</span><span class="sxs-lookup"><span data-stu-id="67769-184">Existing infrastructure</span></span>
* <span data-ttu-id="67769-185">效能需求</span><span class="sxs-lookup"><span data-stu-id="67769-185">Performance requirements</span></span>
* <span data-ttu-id="67769-186">成本</span><span class="sxs-lookup"><span data-stu-id="67769-186">Cost</span></span>
* <span data-ttu-id="67769-187">Team 經驗</span><span class="sxs-lookup"><span data-stu-id="67769-187">Team experience</span></span>

<span data-ttu-id="67769-188">快取的解決方案通常依賴於記憶體中儲存體，以提供快速擷取快取的資料，但記憶體是有限的資源和成本以展開。</span><span class="sxs-lookup"><span data-stu-id="67769-188">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="67769-189">唯一存放區通常會使用快取中的資料。</span><span class="sxs-lookup"><span data-stu-id="67769-189">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="67769-190">一般而言，Redis 快取提供更高的輸送量和較低的延遲，比 SQL Server 快取。</span><span class="sxs-lookup"><span data-stu-id="67769-190">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="67769-191">不過，基準測試，通常需要動作來判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="67769-191">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="67769-192">作為分散式快取備份存放區使用 SQL Server 時，快取與應用程式的一般資料存放區使用相同的資料庫，並擷取可以對這兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="67769-192">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="67769-193">我們建議使用專用的 SQL Server 執行個體的分散式快取備份存放區。</span><span class="sxs-lookup"><span data-stu-id="67769-193">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="67769-194">其他資源</span><span class="sxs-lookup"><span data-stu-id="67769-194">Additional resources</span></span>

* [<span data-ttu-id="67769-195">Redis 快取，在 Azure 上</span><span class="sxs-lookup"><span data-stu-id="67769-195">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="67769-196">在 Azure 上的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="67769-196">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <span data-ttu-id="67769-197">[ASP.NET Core IDistributedCache NCache Web 伺服陣列中的提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)([GitHub 上的 NCache](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="67769-197">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
