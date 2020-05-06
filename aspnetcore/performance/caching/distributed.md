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
ms.openlocfilehash: 206ff55aa530cd06c162e49f400b436e9fb9f07a
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775293"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="a9168-103">ASP.NET Core 中的分散式快取</span><span class="sxs-lookup"><span data-stu-id="a9168-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="a9168-104">作者： [Mohsin Nasir](https://github.com/mohsinnasir)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a9168-104">By [Mohsin Nasir](https://github.com/mohsinnasir) and [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a9168-105">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9168-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="a9168-106">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="a9168-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="a9168-107">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="a9168-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="a9168-108">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="a9168-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="a9168-109">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="a9168-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="a9168-110">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="a9168-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="a9168-111">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="a9168-111">Doesn't use local memory.</span></span>

<span data-ttu-id="a9168-112">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="a9168-113">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="a9168-114">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="a9168-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="a9168-115">無論選取哪一個實作為，應用程式都會使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面與快取互動。</span><span class="sxs-lookup"><span data-stu-id="a9168-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="a9168-116">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a9168-116">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9168-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="a9168-117">Prerequisites</span></span>

<span data-ttu-id="a9168-118">若要使用 SQL Server 分散式快取，請將套件參考新增至[Microsoft. extension. cache. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="a9168-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="a9168-119">若要使用 Redis 分散式快取，請將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="a9168-120">若要使用 NCache 分散式快取，請將套件參考新增至[NCache. 開放原始碼](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="a9168-121">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="a9168-121">IDistributedCache interface</span></span>

<span data-ttu-id="a9168-122"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="a9168-122">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="a9168-123"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;會接受字串索引鍵，並在快取中找到`byte[]`快取的專案做為陣列。</span><span class="sxs-lookup"><span data-stu-id="a9168-123"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="a9168-124"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;會使用字串索引鍵`byte[]` ，將專案（陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-124"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="a9168-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>會根據索引鍵重新整理快取中的專案，重設其滑動到期時間（如果有的話&ndash; ）。</span><span class="sxs-lookup"><span data-stu-id="a9168-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="a9168-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;會根據它的字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="a9168-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="a9168-127">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="a9168-127">Establish distributed caching services</span></span>

<span data-ttu-id="a9168-128">在中`Startup.ConfigureServices`註冊的<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-128">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9168-129">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="a9168-129">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="a9168-130">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="a9168-130">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="a9168-131">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-131">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="a9168-132">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-132">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="a9168-133">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-133">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="a9168-134">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="a9168-134">Distributed Memory Cache</span></span>

<span data-ttu-id="a9168-135">分散式記憶體快取（<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>）是一種架構提供的<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="a9168-135">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="a9168-136">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-136">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="a9168-137">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="a9168-137">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="a9168-138">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="a9168-138">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="a9168-139">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="a9168-139">In development and testing scenarios.</span></span>
* <span data-ttu-id="a9168-140">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="a9168-140">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="a9168-141">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="a9168-141">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="a9168-142">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="a9168-142">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="a9168-143">當應用程式在開發環境中`Startup.ConfigureServices`執行時，範例應用程式會使用分散式記憶體快取：</span><span class="sxs-lookup"><span data-stu-id="a9168-143">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="a9168-144">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-144">Distributed SQL Server Cache</span></span>

<span data-ttu-id="a9168-145">分散式 SQL Server 快取執行（<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="a9168-145">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="a9168-146">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以`sql-cache`使用工具。</span><span class="sxs-lookup"><span data-stu-id="a9168-146">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="a9168-147">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="a9168-147">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="a9168-148">執行`sql-cache create`命令，在 SQL Server 中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="a9168-148">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="a9168-149">提供 SQL Server 實例`Data Source`（）、資料庫（`Initial Catalog`）、架構（例如`dbo`）和資料表名稱（例如`TestCache`）：</span><span class="sxs-lookup"><span data-stu-id="a9168-149">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="a9168-150">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="a9168-150">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="a9168-151">此`sql-cache`工具所建立的資料表具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="a9168-151">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="a9168-153">應用程式應該使用的實例來<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>操作快取值，而<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>不是。</span><span class="sxs-lookup"><span data-stu-id="a9168-153">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="a9168-154">範例應用程式<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>會在的非開發環境中`Startup.ConfigureServices`執行：</span><span class="sxs-lookup"><span data-stu-id="a9168-154">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="a9168-155"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> （而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>和<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*/*appsettings 儲存）。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="a9168-155">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="a9168-156">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="a9168-156">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="a9168-157">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-157">Distributed Redis Cache</span></span>

<span data-ttu-id="a9168-158">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-158">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="a9168-159">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-159">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="a9168-160">應用程式會在的非開發環境<xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache>中，<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>使用實例（）來設定快取`Startup.ConfigureServices`執行：</span><span class="sxs-lookup"><span data-stu-id="a9168-160">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

<span data-ttu-id="a9168-161">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="a9168-161">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="a9168-162">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-162">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="a9168-163">從`redis-server`命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-163">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="a9168-164">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-164">Distributed NCache Cache</span></span>

<span data-ttu-id="a9168-165">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-165">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="a9168-166">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="a9168-166">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="a9168-167">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-167">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="a9168-168">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="a9168-168">To configure NCache:</span></span>

1. <span data-ttu-id="a9168-169">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-169">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="a9168-170">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="a9168-170">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="a9168-171">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="a9168-171">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="a9168-172">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="a9168-172">Use the distributed cache</span></span>

<span data-ttu-id="a9168-173">若要使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面，請從應用程式<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="a9168-173">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="a9168-174">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="a9168-174">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a9168-175">當範例應用程式啟動時<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，會插入`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="a9168-175">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="a9168-176">目前的時間是使用<xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime>快取的（如需詳細資訊，請參閱[泛型主機： IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)）：</span><span class="sxs-lookup"><span data-stu-id="a9168-176">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="a9168-177">範例應用程式<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>會將插入`IndexModel` ，以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="a9168-177">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="a9168-178">每次載入 [索引] 頁面時，都會檢查快取的快取時間`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="a9168-178">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="a9168-179">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="a9168-179">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="a9168-180">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="a9168-180">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="a9168-181">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="a9168-181">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="a9168-182">按鈕會觸發`OnPostResetCachedTime`處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="a9168-182">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="a9168-183"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>實例不需要使用單一或範圍存留期（至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="a9168-183">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="a9168-184">您也可以建立<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="a9168-184">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="a9168-185">建議</span><span class="sxs-lookup"><span data-stu-id="a9168-185">Recommendations</span></span>

<span data-ttu-id="a9168-186">當決定哪一個<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="a9168-186">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="a9168-187">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="a9168-187">Existing infrastructure</span></span>
* <span data-ttu-id="a9168-188">效能需求</span><span class="sxs-lookup"><span data-stu-id="a9168-188">Performance requirements</span></span>
* <span data-ttu-id="a9168-189">成本</span><span class="sxs-lookup"><span data-stu-id="a9168-189">Cost</span></span>
* <span data-ttu-id="a9168-190">小組經驗</span><span class="sxs-lookup"><span data-stu-id="a9168-190">Team experience</span></span>

<span data-ttu-id="a9168-191">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="a9168-191">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="a9168-192">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="a9168-192">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="a9168-193">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="a9168-193">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="a9168-194">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="a9168-194">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="a9168-195">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="a9168-195">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="a9168-196">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="a9168-196">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9168-197">其他資源</span><span class="sxs-lookup"><span data-stu-id="a9168-197">Additional resources</span></span>

* [<span data-ttu-id="a9168-198">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-198">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="a9168-199">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="a9168-199">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="a9168-200">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="a9168-200">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a9168-201">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9168-201">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="a9168-202">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="a9168-202">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="a9168-203">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="a9168-203">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="a9168-204">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="a9168-204">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="a9168-205">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="a9168-205">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="a9168-206">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="a9168-206">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="a9168-207">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="a9168-207">Doesn't use local memory.</span></span>

<span data-ttu-id="a9168-208">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-208">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="a9168-209">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-209">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="a9168-210">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="a9168-210">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="a9168-211">無論選取哪一個實作為，應用程式都會使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面與快取互動。</span><span class="sxs-lookup"><span data-stu-id="a9168-211">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="a9168-212">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a9168-212">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9168-213">必要條件</span><span class="sxs-lookup"><span data-stu-id="a9168-213">Prerequisites</span></span>

<span data-ttu-id="a9168-214">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="a9168-214">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="a9168-215">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。</span><span class="sxs-lookup"><span data-stu-id="a9168-215">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="a9168-216">Redis 套件不包含在`Microsoft.AspNetCore.App`封裝中，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-216">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="a9168-217">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-217">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="a9168-218">NCache 套件不包含在`Microsoft.AspNetCore.App`封裝中，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-218">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="a9168-219">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="a9168-219">IDistributedCache interface</span></span>

<span data-ttu-id="a9168-220"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="a9168-220">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="a9168-221"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;會接受字串索引鍵，並在快取中找到`byte[]`快取的專案做為陣列。</span><span class="sxs-lookup"><span data-stu-id="a9168-221"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="a9168-222"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;會使用字串索引鍵`byte[]` ，將專案（陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-222"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="a9168-223"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>會根據索引鍵重新整理快取中的專案，重設其滑動到期時間（如果有的話&ndash; ）。</span><span class="sxs-lookup"><span data-stu-id="a9168-223"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="a9168-224"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;會根據它的字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="a9168-224"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="a9168-225">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="a9168-225">Establish distributed caching services</span></span>

<span data-ttu-id="a9168-226">在中`Startup.ConfigureServices`註冊的<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-226">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9168-227">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="a9168-227">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="a9168-228">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="a9168-228">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="a9168-229">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-229">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="a9168-230">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-230">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="a9168-231">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-231">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="a9168-232">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="a9168-232">Distributed Memory Cache</span></span>

<span data-ttu-id="a9168-233">分散式記憶體快取（<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>）是一種架構提供的<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="a9168-233">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="a9168-234">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-234">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="a9168-235">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="a9168-235">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="a9168-236">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="a9168-236">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="a9168-237">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="a9168-237">In development and testing scenarios.</span></span>
* <span data-ttu-id="a9168-238">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="a9168-238">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="a9168-239">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="a9168-239">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="a9168-240">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="a9168-240">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="a9168-241">當應用程式在開發環境中`Startup.ConfigureServices`執行時，範例應用程式會使用分散式記憶體快取：</span><span class="sxs-lookup"><span data-stu-id="a9168-241">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="a9168-242">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-242">Distributed SQL Server Cache</span></span>

<span data-ttu-id="a9168-243">分散式 SQL Server 快取執行（<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="a9168-243">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="a9168-244">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以`sql-cache`使用工具。</span><span class="sxs-lookup"><span data-stu-id="a9168-244">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="a9168-245">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="a9168-245">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="a9168-246">執行`sql-cache create`命令，在 SQL Server 中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="a9168-246">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="a9168-247">提供 SQL Server 實例`Data Source`（）、資料庫（`Initial Catalog`）、架構（例如`dbo`）和資料表名稱（例如`TestCache`）：</span><span class="sxs-lookup"><span data-stu-id="a9168-247">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="a9168-248">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="a9168-248">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="a9168-249">此`sql-cache`工具所建立的資料表具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="a9168-249">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="a9168-251">應用程式應該使用的實例來<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>操作快取值，而<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>不是。</span><span class="sxs-lookup"><span data-stu-id="a9168-251">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="a9168-252">範例應用程式<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>會在的非開發環境中`Startup.ConfigureServices`執行：</span><span class="sxs-lookup"><span data-stu-id="a9168-252">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="a9168-253"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> （而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>和<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*/*appsettings 儲存）。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="a9168-253">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="a9168-254">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="a9168-254">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="a9168-255">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-255">Distributed Redis Cache</span></span>

<span data-ttu-id="a9168-256">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-256">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="a9168-257">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-257">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="a9168-258">應用程式會在的非開發環境<xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache>中，<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>使用實例（）來設定快取`Startup.ConfigureServices`執行：</span><span class="sxs-lookup"><span data-stu-id="a9168-258">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

<span data-ttu-id="a9168-259">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="a9168-259">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="a9168-260">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-260">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="a9168-261">從`redis-server`命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-261">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="a9168-262">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-262">Distributed NCache Cache</span></span>

<span data-ttu-id="a9168-263">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-263">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="a9168-264">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="a9168-264">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="a9168-265">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-265">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="a9168-266">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="a9168-266">To configure NCache:</span></span>

1. <span data-ttu-id="a9168-267">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-267">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="a9168-268">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="a9168-268">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="a9168-269">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="a9168-269">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="a9168-270">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="a9168-270">Use the distributed cache</span></span>

<span data-ttu-id="a9168-271">若要使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面，請從應用程式<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="a9168-271">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="a9168-272">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="a9168-272">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a9168-273">當範例應用程式啟動時<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，會插入`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="a9168-273">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="a9168-274">目前的時間是使用<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>快取的（如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="a9168-274">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="a9168-275">範例應用程式<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>會將插入`IndexModel` ，以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="a9168-275">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="a9168-276">每次載入 [索引] 頁面時，都會檢查快取的快取時間`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="a9168-276">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="a9168-277">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="a9168-277">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="a9168-278">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="a9168-278">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="a9168-279">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="a9168-279">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="a9168-280">按鈕會觸發`OnPostResetCachedTime`處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="a9168-280">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="a9168-281"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>實例不需要使用單一或範圍存留期（至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="a9168-281">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="a9168-282">您也可以建立<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="a9168-282">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="a9168-283">建議</span><span class="sxs-lookup"><span data-stu-id="a9168-283">Recommendations</span></span>

<span data-ttu-id="a9168-284">當決定哪一個<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="a9168-284">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="a9168-285">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="a9168-285">Existing infrastructure</span></span>
* <span data-ttu-id="a9168-286">效能需求</span><span class="sxs-lookup"><span data-stu-id="a9168-286">Performance requirements</span></span>
* <span data-ttu-id="a9168-287">成本</span><span class="sxs-lookup"><span data-stu-id="a9168-287">Cost</span></span>
* <span data-ttu-id="a9168-288">小組經驗</span><span class="sxs-lookup"><span data-stu-id="a9168-288">Team experience</span></span>

<span data-ttu-id="a9168-289">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="a9168-289">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="a9168-290">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="a9168-290">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="a9168-291">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="a9168-291">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="a9168-292">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="a9168-292">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="a9168-293">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="a9168-293">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="a9168-294">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="a9168-294">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9168-295">其他資源</span><span class="sxs-lookup"><span data-stu-id="a9168-295">Additional resources</span></span>

* [<span data-ttu-id="a9168-296">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-296">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="a9168-297">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="a9168-297">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="a9168-298">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="a9168-298">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a9168-299">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9168-299">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="a9168-300">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="a9168-300">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="a9168-301">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="a9168-301">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="a9168-302">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="a9168-302">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="a9168-303">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="a9168-303">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="a9168-304">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="a9168-304">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="a9168-305">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="a9168-305">Doesn't use local memory.</span></span>

<span data-ttu-id="a9168-306">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-306">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="a9168-307">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-307">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="a9168-308">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="a9168-308">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="a9168-309">無論選取哪一個實作為，應用程式都會使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面與快取互動。</span><span class="sxs-lookup"><span data-stu-id="a9168-309">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="a9168-310">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a9168-310">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9168-311">必要條件</span><span class="sxs-lookup"><span data-stu-id="a9168-311">Prerequisites</span></span>

<span data-ttu-id="a9168-312">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="a9168-312">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="a9168-313">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="a9168-313">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="a9168-314">Redis 套件不包含在`Microsoft.AspNetCore.App`封裝中，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-314">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="a9168-315">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-315">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="a9168-316">NCache 套件不包含在`Microsoft.AspNetCore.App`封裝中，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="a9168-316">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="a9168-317">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="a9168-317">IDistributedCache interface</span></span>

<span data-ttu-id="a9168-318"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="a9168-318">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="a9168-319"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash;會接受字串索引鍵，並在快取中找到`byte[]`快取的專案做為陣列。</span><span class="sxs-lookup"><span data-stu-id="a9168-319"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="a9168-320"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash;會使用字串索引鍵`byte[]` ，將專案（陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-320"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="a9168-321"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>會根據索引鍵重新整理快取中的專案，重設其滑動到期時間（如果有的話&ndash; ）。</span><span class="sxs-lookup"><span data-stu-id="a9168-321"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="a9168-322"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash;會根據它的字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="a9168-322"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="a9168-323">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="a9168-323">Establish distributed caching services</span></span>

<span data-ttu-id="a9168-324">在中`Startup.ConfigureServices`註冊的<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-324">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a9168-325">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="a9168-325">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="a9168-326">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="a9168-326">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="a9168-327">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-327">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="a9168-328">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-328">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="a9168-329">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-329">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="a9168-330">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="a9168-330">Distributed Memory Cache</span></span>

<span data-ttu-id="a9168-331">分散式記憶體快取（<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>）是一種架構提供的<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>執行，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="a9168-331">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="a9168-332">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-332">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="a9168-333">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="a9168-333">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="a9168-334">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="a9168-334">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="a9168-335">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="a9168-335">In development and testing scenarios.</span></span>
* <span data-ttu-id="a9168-336">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="a9168-336">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="a9168-337">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="a9168-337">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="a9168-338">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="a9168-338">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="a9168-339">當應用程式在開發環境中`Startup.ConfigureServices`執行時，範例應用程式會使用分散式記憶體快取：</span><span class="sxs-lookup"><span data-stu-id="a9168-339">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="a9168-340">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-340">Distributed SQL Server Cache</span></span>

<span data-ttu-id="a9168-341">分散式 SQL Server 快取執行（<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="a9168-341">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="a9168-342">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以`sql-cache`使用工具。</span><span class="sxs-lookup"><span data-stu-id="a9168-342">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="a9168-343">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="a9168-343">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="a9168-344">執行`sql-cache create`命令，在 SQL Server 中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="a9168-344">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="a9168-345">提供 SQL Server 實例`Data Source`（）、資料庫（`Initial Catalog`）、架構（例如`dbo`）和資料表名稱（例如`TestCache`）：</span><span class="sxs-lookup"><span data-stu-id="a9168-345">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="a9168-346">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="a9168-346">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="a9168-347">此`sql-cache`工具所建立的資料表具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="a9168-347">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="a9168-349">應用程式應該使用的實例來<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>操作快取值，而<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>不是。</span><span class="sxs-lookup"><span data-stu-id="a9168-349">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="a9168-350">範例應用程式<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>會在的非開發環境中`Startup.ConfigureServices`執行：</span><span class="sxs-lookup"><span data-stu-id="a9168-350">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="a9168-351"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> （而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*>和<xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*/*appsettings 儲存）。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="a9168-351">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="a9168-352">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="a9168-352">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="a9168-353">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-353">Distributed Redis Cache</span></span>

<span data-ttu-id="a9168-354">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-354">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="a9168-355">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-355">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="a9168-356">應用程式會使用<xref:Microsoft.Extensions.Caching.Redis.RedisCache>實例（<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>）來設定快取執行：</span><span class="sxs-lookup"><span data-stu-id="a9168-356">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

<span data-ttu-id="a9168-357">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="a9168-357">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="a9168-358">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-358">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="a9168-359">從`redis-server`命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="a9168-359">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="a9168-360">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-360">Distributed NCache Cache</span></span>

<span data-ttu-id="a9168-361">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="a9168-361">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="a9168-362">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="a9168-362">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="a9168-363">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-363">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="a9168-364">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="a9168-364">To configure NCache:</span></span>

1. <span data-ttu-id="a9168-365">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="a9168-365">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="a9168-366">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="a9168-366">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="a9168-367">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="a9168-367">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="a9168-368">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="a9168-368">Use the distributed cache</span></span>

<span data-ttu-id="a9168-369">若要使用<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面，請從應用程式<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="a9168-369">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="a9168-370">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="a9168-370">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a9168-371">當範例應用程式啟動時<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，會插入`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="a9168-371">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="a9168-372">目前的時間是使用<xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime>快取的（如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="a9168-372">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="a9168-373">範例應用程式<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>會將插入`IndexModel` ，以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="a9168-373">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="a9168-374">每次載入 [索引] 頁面時，都會檢查快取的快取時間`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="a9168-374">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="a9168-375">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="a9168-375">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="a9168-376">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="a9168-376">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="a9168-377">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="a9168-377">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="a9168-378">按鈕會觸發`OnPostResetCachedTime`處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="a9168-378">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="a9168-379"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>實例不需要使用單一或範圍存留期（至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="a9168-379">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="a9168-380">您也可以建立<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="a9168-380">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="a9168-381">建議</span><span class="sxs-lookup"><span data-stu-id="a9168-381">Recommendations</span></span>

<span data-ttu-id="a9168-382">當決定哪一個<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="a9168-382">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="a9168-383">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="a9168-383">Existing infrastructure</span></span>
* <span data-ttu-id="a9168-384">效能需求</span><span class="sxs-lookup"><span data-stu-id="a9168-384">Performance requirements</span></span>
* <span data-ttu-id="a9168-385">成本</span><span class="sxs-lookup"><span data-stu-id="a9168-385">Cost</span></span>
* <span data-ttu-id="a9168-386">小組經驗</span><span class="sxs-lookup"><span data-stu-id="a9168-386">Team experience</span></span>

<span data-ttu-id="a9168-387">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="a9168-387">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="a9168-388">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="a9168-388">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="a9168-389">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="a9168-389">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="a9168-390">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="a9168-390">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="a9168-391">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="a9168-391">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="a9168-392">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="a9168-392">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9168-393">其他資源</span><span class="sxs-lookup"><span data-stu-id="a9168-393">Additional resources</span></span>

* [<span data-ttu-id="a9168-394">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="a9168-394">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="a9168-395">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="a9168-395">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="a9168-396">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="a9168-396">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end
 