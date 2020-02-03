---
title: ASP.NET Core 中的分散式快取
author: guardrex
description: 瞭解如何使用 ASP.NET Core 分散式快取來改善應用程式效能和擴充性，尤其是在雲端或伺服器陣列環境中。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727237"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="56e3d-103">ASP.NET Core 中的分散式快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="56e3d-104">作者： [Luke Latham](https://github.com/guardrex)、 [Mohsin Nasir](https://github.com/mohsinnasir)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="56e3d-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="56e3d-105">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="56e3d-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="56e3d-106">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="56e3d-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="56e3d-107">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="56e3d-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="56e3d-108">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="56e3d-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="56e3d-109">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="56e3d-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="56e3d-110">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="56e3d-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="56e3d-111">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="56e3d-111">Doesn't use local memory.</span></span>

<span data-ttu-id="56e3d-112">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="56e3d-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="56e3d-113">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="56e3d-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="56e3d-114">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="56e3d-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="56e3d-115">無論選取哪一個執行，應用程式都會使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面來與快取互動。</span><span class="sxs-lookup"><span data-stu-id="56e3d-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="56e3d-116">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56e3d-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56e3d-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="56e3d-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56e3d-118">若要使用 SQL Server 分散式快取，請將套件參考新增至[Microsoft. extension. cache. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="56e3d-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="56e3d-119">若要使用 Redis 分散式快取，請將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="56e3d-120">若要使用 NCache 分散式快取，請將套件參考新增至[NCache. 開放原始碼](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="56e3d-121">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="56e3d-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="56e3d-122">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。</span><span class="sxs-lookup"><span data-stu-id="56e3d-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="56e3d-123">Redis 套件不包含在 `Microsoft.AspNetCore.App` 套件中，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="56e3d-124">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="56e3d-125">NCache 套件不包含在 `Microsoft.AspNetCore.App` 套件中，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="56e3d-126">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="56e3d-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="56e3d-127">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="56e3d-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="56e3d-128">Redis 套件不包含在 `Microsoft.AspNetCore.App` 套件中，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="56e3d-129">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="56e3d-130">NCache 套件不包含在 `Microsoft.AspNetCore.App` 套件中，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="56e3d-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="56e3d-131">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="56e3d-131">IDistributedCache interface</span></span>

<span data-ttu-id="56e3d-132"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="56e3d-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="56e3d-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; 會接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="56e3d-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="56e3d-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; 會使用字串索引鍵，將專案（做為 `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="56e3d-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="56e3d-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; 會根據其索引鍵重新整理快取中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="56e3d-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="56e3d-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; 會根據它的字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="56e3d-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="56e3d-137">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="56e3d-137">Establish distributed caching services</span></span>

<span data-ttu-id="56e3d-138">在 `Startup.ConfigureServices`中註冊 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 的執行。</span><span class="sxs-lookup"><span data-stu-id="56e3d-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="56e3d-139">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="56e3d-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="56e3d-140">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="56e3d-141">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="56e3d-142">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="56e3d-143">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="56e3d-144">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-144">Distributed Memory Cache</span></span>

<span data-ttu-id="56e3d-145">分散式記憶體快取（<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>）是架構提供的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 的執行，可將專案儲存在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="56e3d-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="56e3d-146">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="56e3d-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="56e3d-147">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="56e3d-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="56e3d-148">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="56e3d-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="56e3d-149">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="56e3d-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="56e3d-150">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="56e3d-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="56e3d-151">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="56e3d-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="56e3d-152">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="56e3d-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="56e3d-153">當應用程式在 `Startup.ConfigureServices`的開發環境中執行時，範例應用程式會使用分散式記憶體快取：</span><span class="sxs-lookup"><span data-stu-id="56e3d-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="56e3d-154">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="56e3d-155">分散式 SQL Server 快取執行（<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="56e3d-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="56e3d-156">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="56e3d-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="56e3d-157">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="56e3d-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="56e3d-158">藉由執行 `sql-cache create` 命令，在 SQL Server 中建立資料表。</span><span class="sxs-lookup"><span data-stu-id="56e3d-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="56e3d-159">提供 SQL Server 實例（`Data Source`）、資料庫（`Initial Catalog`）、架構（例如 `dbo`）和資料表名稱（例如 `TestCache`）：</span><span class="sxs-lookup"><span data-stu-id="56e3d-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="56e3d-160">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="56e3d-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="56e3d-161">`sql-cache` 工具所建立的資料表具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="56e3d-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="56e3d-163">應用程式應該使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>的實例來操作快取值，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>。</span><span class="sxs-lookup"><span data-stu-id="56e3d-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="56e3d-164">範例應用程式會在 `Startup.ConfigureServices`的非開發環境中執行 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>：</span><span class="sxs-lookup"><span data-stu-id="56e3d-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="56e3d-165"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> （以及選擇性的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings*/*appsettings 儲存。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="56e3d-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="56e3d-166">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="56e3d-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="56e3d-167">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-167">Distributed Redis Cache</span></span>

<span data-ttu-id="56e3d-168">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="56e3d-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="56e3d-169">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="56e3d-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56e3d-170">應用程式會在 `Startup.ConfigureServices`的非開發環境中，使用 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 實例（<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>）來設定快取執行：</span><span class="sxs-lookup"><span data-stu-id="56e3d-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="56e3d-171">應用程式會在 `Startup.ConfigureServices`的非開發環境中，使用 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 實例（<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>）來設定快取執行：</span><span class="sxs-lookup"><span data-stu-id="56e3d-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="56e3d-172">應用程式會使用 <xref:Microsoft.Extensions.Caching.Redis.RedisCache> 實例（<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>）來設定快取執行：</span><span class="sxs-lookup"><span data-stu-id="56e3d-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="56e3d-173">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="56e3d-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="56e3d-174">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="56e3d-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="56e3d-175">從命令提示字元執行 `redis-server`。</span><span class="sxs-lookup"><span data-stu-id="56e3d-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="56e3d-176">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-176">Distributed NCache Cache</span></span>

<span data-ttu-id="56e3d-177">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="56e3d-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="56e3d-178">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="56e3d-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="56e3d-179">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="56e3d-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="56e3d-180">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="56e3d-180">To configure NCache:</span></span>

1. <span data-ttu-id="56e3d-181">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="56e3d-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="56e3d-182">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="56e3d-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="56e3d-183">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="56e3d-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="56e3d-184">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-184">Use the distributed cache</span></span>

<span data-ttu-id="56e3d-185">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請從應用程式中的任何一個函式要求 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 的實例。</span><span class="sxs-lookup"><span data-stu-id="56e3d-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="56e3d-186">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="56e3d-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="56e3d-187">當範例應用程式啟動時，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="56e3d-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="56e3d-188">目前的時間是使用 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> 快取的（如需詳細資訊，請參閱[泛型主機： IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)）：</span><span class="sxs-lookup"><span data-stu-id="56e3d-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="56e3d-189">當範例應用程式啟動時，<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="56e3d-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="56e3d-190">目前的時間是使用 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> 快取的（如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="56e3d-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="56e3d-191">範例應用程式會將 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 插入 `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="56e3d-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="56e3d-192">每次載入 [索引] 頁面時，都會檢查快取中是否有快取的時間 `OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="56e3d-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="56e3d-193">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="56e3d-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="56e3d-194">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="56e3d-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="56e3d-195">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="56e3d-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="56e3d-196">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="56e3d-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="56e3d-197"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例不需要使用單一或範圍的存留期（至少適用于內建的執行）。</span><span class="sxs-lookup"><span data-stu-id="56e3d-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="56e3d-198">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="56e3d-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="56e3d-199">建議</span><span class="sxs-lookup"><span data-stu-id="56e3d-199">Recommendations</span></span>

<span data-ttu-id="56e3d-200">決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 的執行最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="56e3d-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="56e3d-201">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="56e3d-201">Existing infrastructure</span></span>
* <span data-ttu-id="56e3d-202">效能需求</span><span class="sxs-lookup"><span data-stu-id="56e3d-202">Performance requirements</span></span>
* <span data-ttu-id="56e3d-203">Cost</span><span class="sxs-lookup"><span data-stu-id="56e3d-203">Cost</span></span>
* <span data-ttu-id="56e3d-204">小組經驗</span><span class="sxs-lookup"><span data-stu-id="56e3d-204">Team experience</span></span>

<span data-ttu-id="56e3d-205">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="56e3d-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="56e3d-206">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="56e3d-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="56e3d-207">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="56e3d-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="56e3d-208">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="56e3d-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="56e3d-209">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="56e3d-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="56e3d-210">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="56e3d-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56e3d-211">其他資源</span><span class="sxs-lookup"><span data-stu-id="56e3d-211">Additional resources</span></span>

* [<span data-ttu-id="56e3d-212">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="56e3d-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="56e3d-213">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="56e3d-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="56e3d-214">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="56e3d-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
