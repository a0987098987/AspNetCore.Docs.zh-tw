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
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/caching/distributed
ms.openlocfilehash: 56c67178bd5c63f08a812357a4f8e672dd483994
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405389"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="605e4-103">ASP.NET Core 中的分散式快取</span><span class="sxs-lookup"><span data-stu-id="605e4-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="605e4-104">作者： [Mohsin Nasir](https://github.com/mohsinnasir)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="605e4-104">By [Mohsin Nasir](https://github.com/mohsinnasir) and [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="605e4-105">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="605e4-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="605e4-106">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="605e4-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="605e4-107">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="605e4-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="605e4-108">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="605e4-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="605e4-109">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="605e4-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="605e4-110">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="605e4-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="605e4-111">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="605e4-111">Doesn't use local memory.</span></span>

<span data-ttu-id="605e4-112">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="605e4-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="605e4-113">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="605e4-114">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="605e4-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="605e4-115">無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。</span><span class="sxs-lookup"><span data-stu-id="605e4-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="605e4-116">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="605e4-116">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="605e4-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="605e4-117">Prerequisites</span></span>

<span data-ttu-id="605e4-118">若要使用 SQL Server 分散式快取，請將套件參考新增至[Microsoft. extension. cache. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="605e4-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="605e4-119">若要使用 Redis 分散式快取，請將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="605e4-120">若要使用 NCache 分散式快取，請將套件參考新增至[NCache. 開放原始碼](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="605e4-121">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="605e4-121">IDistributedCache interface</span></span>

<span data-ttu-id="605e4-122"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="605e4-122">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="605e4-123"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="605e4-123"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*>: Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="605e4-124"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-124"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*>: Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="605e4-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="605e4-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>: Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="605e4-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="605e4-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*>: Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="605e4-127">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="605e4-127">Establish distributed caching services</span></span>

<span data-ttu-id="605e4-128">在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-128">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="605e4-129">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="605e4-129">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="605e4-130">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="605e4-130">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="605e4-131">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-131">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="605e4-132">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-132">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="605e4-133">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-133">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="605e4-134">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="605e4-134">Distributed Memory Cache</span></span>

<span data-ttu-id="605e4-135">分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="605e4-135">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="605e4-136">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-136">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="605e4-137">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="605e4-137">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="605e4-138">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="605e4-138">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="605e4-139">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="605e4-139">In development and testing scenarios.</span></span>
* <span data-ttu-id="605e4-140">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="605e4-140">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="605e4-141">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="605e4-141">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="605e4-142">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="605e4-142">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="605e4-143">當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="605e4-143">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="605e4-144">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-144">Distributed SQL Server Cache</span></span>

<span data-ttu-id="605e4-145">分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="605e4-145">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="605e4-146">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="605e4-146">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="605e4-147">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="605e4-147">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="605e4-148">執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-148">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="605e4-149">提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：</span><span class="sxs-lookup"><span data-stu-id="605e4-149">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="605e4-150">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="605e4-150">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="605e4-151">此工具所建立的資料表 `sql-cache` 具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="605e4-151">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="605e4-153">應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。</span><span class="sxs-lookup"><span data-stu-id="605e4-153">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="605e4-154">範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：</span><span class="sxs-lookup"><span data-stu-id="605e4-154">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="605e4-155"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)儲存，或 appsettings 中的*appsettings.js* / *。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="605e4-155">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="605e4-156">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="605e4-156">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="605e4-157">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-157">Distributed Redis Cache</span></span>

<span data-ttu-id="605e4-158">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-158">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="605e4-159">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-159">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="605e4-160">應用程式會在的 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 非開發環境中，使用實例（）來設定快取執行 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="605e4-160">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

<span data-ttu-id="605e4-161">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="605e4-161">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="605e4-162">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-162">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="605e4-163">`redis-server`從命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="605e4-163">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="605e4-164">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-164">Distributed NCache Cache</span></span>

<span data-ttu-id="605e4-165">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-165">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="605e4-166">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="605e4-166">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="605e4-167">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-167">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="605e4-168">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="605e4-168">To configure NCache:</span></span>

1. <span data-ttu-id="605e4-169">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-169">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="605e4-170">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="605e4-170">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="605e4-171">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="605e4-171">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="605e4-172">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="605e4-172">Use the distributed cache</span></span>

<span data-ttu-id="605e4-173">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="605e4-173">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="605e4-174">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="605e4-174">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="605e4-175">當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-175">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="605e4-176">目前的時間是使用快取的 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> （如需詳細資訊，請參閱[泛型主機： IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)）：</span><span class="sxs-lookup"><span data-stu-id="605e4-176">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="605e4-177">範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="605e4-177">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="605e4-178">每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-178">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="605e4-179">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="605e4-179">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="605e4-180">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="605e4-180">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="605e4-181">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="605e4-181">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="605e4-182">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="605e4-182">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="605e4-183">實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="605e4-183">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="605e4-184">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="605e4-184">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="605e4-185">建議</span><span class="sxs-lookup"><span data-stu-id="605e4-185">Recommendations</span></span>

<span data-ttu-id="605e4-186">當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="605e4-186">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="605e4-187">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="605e4-187">Existing infrastructure</span></span>
* <span data-ttu-id="605e4-188">效能需求</span><span class="sxs-lookup"><span data-stu-id="605e4-188">Performance requirements</span></span>
* <span data-ttu-id="605e4-189">成本</span><span class="sxs-lookup"><span data-stu-id="605e4-189">Cost</span></span>
* <span data-ttu-id="605e4-190">小組經驗</span><span class="sxs-lookup"><span data-stu-id="605e4-190">Team experience</span></span>

<span data-ttu-id="605e4-191">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="605e4-191">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="605e4-192">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="605e4-192">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="605e4-193">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="605e4-193">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="605e4-194">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="605e4-194">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="605e4-195">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="605e4-195">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="605e4-196">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="605e4-196">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="605e4-197">其他資源</span><span class="sxs-lookup"><span data-stu-id="605e4-197">Additional resources</span></span>

* [<span data-ttu-id="605e4-198">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-198">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="605e4-199">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="605e4-199">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="605e4-200">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="605e4-200">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="605e4-201">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="605e4-201">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="605e4-202">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="605e4-202">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="605e4-203">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="605e4-203">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="605e4-204">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="605e4-204">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="605e4-205">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="605e4-205">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="605e4-206">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="605e4-206">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="605e4-207">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="605e4-207">Doesn't use local memory.</span></span>

<span data-ttu-id="605e4-208">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="605e4-208">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="605e4-209">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-209">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="605e4-210">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="605e4-210">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="605e4-211">無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。</span><span class="sxs-lookup"><span data-stu-id="605e4-211">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="605e4-212">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="605e4-212">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="605e4-213">必要條件</span><span class="sxs-lookup"><span data-stu-id="605e4-213">Prerequisites</span></span>

<span data-ttu-id="605e4-214">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="605e4-214">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="605e4-215">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。</span><span class="sxs-lookup"><span data-stu-id="605e4-215">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="605e4-216">Redis 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-216">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="605e4-217">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-217">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="605e4-218">NCache 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-218">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="605e4-219">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="605e4-219">IDistributedCache interface</span></span>

<span data-ttu-id="605e4-220"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="605e4-220">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="605e4-221"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="605e4-221"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*>: Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="605e4-222"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-222"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*>: Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="605e4-223"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="605e4-223"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>: Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="605e4-224"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="605e4-224"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*>: Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="605e4-225">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="605e4-225">Establish distributed caching services</span></span>

<span data-ttu-id="605e4-226">在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-226">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="605e4-227">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="605e4-227">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="605e4-228">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="605e4-228">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="605e4-229">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-229">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="605e4-230">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-230">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="605e4-231">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-231">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="605e4-232">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="605e4-232">Distributed Memory Cache</span></span>

<span data-ttu-id="605e4-233">分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="605e4-233">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="605e4-234">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-234">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="605e4-235">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="605e4-235">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="605e4-236">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="605e4-236">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="605e4-237">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="605e4-237">In development and testing scenarios.</span></span>
* <span data-ttu-id="605e4-238">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="605e4-238">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="605e4-239">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="605e4-239">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="605e4-240">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="605e4-240">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="605e4-241">當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="605e4-241">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="605e4-242">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-242">Distributed SQL Server Cache</span></span>

<span data-ttu-id="605e4-243">分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="605e4-243">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="605e4-244">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="605e4-244">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="605e4-245">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="605e4-245">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="605e4-246">執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-246">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="605e4-247">提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：</span><span class="sxs-lookup"><span data-stu-id="605e4-247">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="605e4-248">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="605e4-248">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="605e4-249">此工具所建立的資料表 `sql-cache` 具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="605e4-249">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="605e4-251">應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。</span><span class="sxs-lookup"><span data-stu-id="605e4-251">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="605e4-252">範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：</span><span class="sxs-lookup"><span data-stu-id="605e4-252">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="605e4-253"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)儲存，或 appsettings 中的*appsettings.js* / *。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="605e4-253">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="605e4-254">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="605e4-254">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="605e4-255">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-255">Distributed Redis Cache</span></span>

<span data-ttu-id="605e4-256">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-256">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="605e4-257">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-257">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="605e4-258">應用程式會在的 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 非開發環境中，使用實例（）來設定快取執行 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="605e4-258">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

<span data-ttu-id="605e4-259">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="605e4-259">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="605e4-260">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-260">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="605e4-261">`redis-server`從命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="605e4-261">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="605e4-262">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-262">Distributed NCache Cache</span></span>

<span data-ttu-id="605e4-263">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-263">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="605e4-264">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="605e4-264">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="605e4-265">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-265">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="605e4-266">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="605e4-266">To configure NCache:</span></span>

1. <span data-ttu-id="605e4-267">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-267">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="605e4-268">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="605e4-268">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="605e4-269">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="605e4-269">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="605e4-270">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="605e4-270">Use the distributed cache</span></span>

<span data-ttu-id="605e4-271">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="605e4-271">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="605e4-272">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="605e4-272">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="605e4-273">當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-273">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="605e4-274">目前的時間是使用快取的 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> （如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="605e4-274">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="605e4-275">範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="605e4-275">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="605e4-276">每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-276">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="605e4-277">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="605e4-277">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="605e4-278">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="605e4-278">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="605e4-279">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="605e4-279">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="605e4-280">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="605e4-280">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="605e4-281">實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="605e4-281">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="605e4-282">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="605e4-282">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="605e4-283">建議</span><span class="sxs-lookup"><span data-stu-id="605e4-283">Recommendations</span></span>

<span data-ttu-id="605e4-284">當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="605e4-284">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="605e4-285">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="605e4-285">Existing infrastructure</span></span>
* <span data-ttu-id="605e4-286">效能需求</span><span class="sxs-lookup"><span data-stu-id="605e4-286">Performance requirements</span></span>
* <span data-ttu-id="605e4-287">成本</span><span class="sxs-lookup"><span data-stu-id="605e4-287">Cost</span></span>
* <span data-ttu-id="605e4-288">小組經驗</span><span class="sxs-lookup"><span data-stu-id="605e4-288">Team experience</span></span>

<span data-ttu-id="605e4-289">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="605e4-289">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="605e4-290">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="605e4-290">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="605e4-291">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="605e4-291">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="605e4-292">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="605e4-292">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="605e4-293">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="605e4-293">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="605e4-294">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="605e4-294">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="605e4-295">其他資源</span><span class="sxs-lookup"><span data-stu-id="605e4-295">Additional resources</span></span>

* [<span data-ttu-id="605e4-296">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-296">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="605e4-297">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="605e4-297">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="605e4-298">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="605e4-298">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="605e4-299">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="605e4-299">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="605e4-300">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="605e4-300">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="605e4-301">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="605e4-301">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="605e4-302">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="605e4-302">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="605e4-303">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="605e4-303">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="605e4-304">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="605e4-304">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="605e4-305">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="605e4-305">Doesn't use local memory.</span></span>

<span data-ttu-id="605e4-306">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="605e4-306">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="605e4-307">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-307">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="605e4-308">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="605e4-308">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="605e4-309">無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。</span><span class="sxs-lookup"><span data-stu-id="605e4-309">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="605e4-310">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="605e4-310">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="605e4-311">必要條件</span><span class="sxs-lookup"><span data-stu-id="605e4-311">Prerequisites</span></span>

<span data-ttu-id="605e4-312">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="605e4-312">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="605e4-313">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="605e4-313">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="605e4-314">Redis 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-314">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="605e4-315">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-315">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="605e4-316">NCache 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="605e4-316">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="605e4-317">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="605e4-317">IDistributedCache interface</span></span>

<span data-ttu-id="605e4-318"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="605e4-318">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="605e4-319"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="605e4-319"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*>: Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="605e4-320"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-320"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*>: Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="605e4-321"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="605e4-321"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>: Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="605e4-322"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="605e4-322"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*>: Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="605e4-323">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="605e4-323">Establish distributed caching services</span></span>

<span data-ttu-id="605e4-324">在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-324">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="605e4-325">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="605e4-325">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="605e4-326">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="605e4-326">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="605e4-327">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-327">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="605e4-328">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-328">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="605e4-329">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-329">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="605e4-330">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="605e4-330">Distributed Memory Cache</span></span>

<span data-ttu-id="605e4-331">分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="605e4-331">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="605e4-332">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-332">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="605e4-333">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="605e4-333">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="605e4-334">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="605e4-334">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="605e4-335">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="605e4-335">In development and testing scenarios.</span></span>
* <span data-ttu-id="605e4-336">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="605e4-336">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="605e4-337">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="605e4-337">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="605e4-338">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="605e4-338">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="605e4-339">當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="605e4-339">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="605e4-340">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-340">Distributed SQL Server Cache</span></span>

<span data-ttu-id="605e4-341">分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="605e4-341">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="605e4-342">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="605e4-342">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="605e4-343">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="605e4-343">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="605e4-344">執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-344">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="605e4-345">提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：</span><span class="sxs-lookup"><span data-stu-id="605e4-345">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="605e4-346">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="605e4-346">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="605e4-347">此工具所建立的資料表 `sql-cache` 具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="605e4-347">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="605e4-349">應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。</span><span class="sxs-lookup"><span data-stu-id="605e4-349">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="605e4-350">範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：</span><span class="sxs-lookup"><span data-stu-id="605e4-350">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="605e4-351"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)儲存，或 appsettings 中的*appsettings.js* / *。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="605e4-351">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="605e4-352">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="605e4-352">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="605e4-353">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-353">Distributed Redis Cache</span></span>

<span data-ttu-id="605e4-354">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-354">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="605e4-355">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-355">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="605e4-356">應用程式會使用實例（）來設定快取執行 <xref:Microsoft.Extensions.Caching.Redis.RedisCache> <xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*> ：</span><span class="sxs-lookup"><span data-stu-id="605e4-356">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

<span data-ttu-id="605e4-357">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="605e4-357">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="605e4-358">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-358">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="605e4-359">`redis-server`從命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="605e4-359">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="605e4-360">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-360">Distributed NCache Cache</span></span>

<span data-ttu-id="605e4-361">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="605e4-361">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="605e4-362">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="605e4-362">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="605e4-363">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-363">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="605e4-364">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="605e4-364">To configure NCache:</span></span>

1. <span data-ttu-id="605e4-365">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="605e4-365">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="605e4-366">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="605e4-366">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="605e4-367">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="605e4-367">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="605e4-368">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="605e4-368">Use the distributed cache</span></span>

<span data-ttu-id="605e4-369">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="605e4-369">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="605e4-370">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="605e4-370">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="605e4-371">當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-371">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="605e4-372">目前的時間是使用快取的 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> （如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="605e4-372">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="605e4-373">範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="605e4-373">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="605e4-374">每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。</span><span class="sxs-lookup"><span data-stu-id="605e4-374">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="605e4-375">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="605e4-375">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="605e4-376">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="605e4-376">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="605e4-377">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="605e4-377">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="605e4-378">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="605e4-378">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="605e4-379">實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="605e4-379">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="605e4-380">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="605e4-380">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="605e4-381">建議</span><span class="sxs-lookup"><span data-stu-id="605e4-381">Recommendations</span></span>

<span data-ttu-id="605e4-382">當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="605e4-382">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="605e4-383">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="605e4-383">Existing infrastructure</span></span>
* <span data-ttu-id="605e4-384">效能需求</span><span class="sxs-lookup"><span data-stu-id="605e4-384">Performance requirements</span></span>
* <span data-ttu-id="605e4-385">成本</span><span class="sxs-lookup"><span data-stu-id="605e4-385">Cost</span></span>
* <span data-ttu-id="605e4-386">小組經驗</span><span class="sxs-lookup"><span data-stu-id="605e4-386">Team experience</span></span>

<span data-ttu-id="605e4-387">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="605e4-387">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="605e4-388">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="605e4-388">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="605e4-389">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="605e4-389">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="605e4-390">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="605e4-390">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="605e4-391">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="605e4-391">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="605e4-392">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="605e4-392">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="605e4-393">其他資源</span><span class="sxs-lookup"><span data-stu-id="605e4-393">Additional resources</span></span>

* [<span data-ttu-id="605e4-394">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="605e4-394">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="605e4-395">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="605e4-395">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="605e4-396">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="605e4-396">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end
 