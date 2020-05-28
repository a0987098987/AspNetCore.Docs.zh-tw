---
<span data-ttu-id="d203e-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="d203e-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="d203e-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="d203e-102">'Blazor'</span></span>
- <span data-ttu-id="d203e-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="d203e-103">'Identity'</span></span>
- <span data-ttu-id="d203e-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="d203e-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="d203e-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="d203e-105">'Razor'</span></span>
- <span data-ttu-id="d203e-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="d203e-106">'SignalR' uid:</span></span> 

---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="d203e-107">ASP.NET Core 中的分散式快取</span><span class="sxs-lookup"><span data-stu-id="d203e-107">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="d203e-108">作者： [Mohsin Nasir](https://github.com/mohsinnasir)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d203e-108">By [Mohsin Nasir](https://github.com/mohsinnasir) and [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d203e-109">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="d203e-109">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="d203e-110">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="d203e-110">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="d203e-111">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="d203e-111">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="d203e-112">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="d203e-112">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="d203e-113">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="d203e-113">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="d203e-114">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="d203e-114">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="d203e-115">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="d203e-115">Doesn't use local memory.</span></span>

<span data-ttu-id="d203e-116">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="d203e-116">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="d203e-117">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-117">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="d203e-118">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="d203e-118">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="d203e-119">無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。</span><span class="sxs-lookup"><span data-stu-id="d203e-119">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="d203e-120">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d203e-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d203e-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="d203e-121">Prerequisites</span></span>

<span data-ttu-id="d203e-122">若要使用 SQL Server 分散式快取，請將套件參考新增至[Microsoft. extension. cache. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="d203e-122">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="d203e-123">若要使用 Redis 分散式快取，請將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-123">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="d203e-124">若要使用 NCache 分散式快取，請將套件參考新增至[NCache. 開放原始碼](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-124">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="d203e-125">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="d203e-125">IDistributedCache interface</span></span>

<span data-ttu-id="d203e-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="d203e-126">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="d203e-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="d203e-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*>: Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="d203e-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*>: Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="d203e-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="d203e-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>: Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="d203e-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="d203e-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*>: Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="d203e-131">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="d203e-131">Establish distributed caching services</span></span>

<span data-ttu-id="d203e-132">在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-132">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d203e-133">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="d203e-133">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="d203e-134">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="d203e-134">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="d203e-135">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-135">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="d203e-136">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-136">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="d203e-137">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-137">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="d203e-138">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="d203e-138">Distributed Memory Cache</span></span>

<span data-ttu-id="d203e-139">分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d203e-139">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="d203e-140">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-140">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="d203e-141">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="d203e-141">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="d203e-142">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="d203e-142">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="d203e-143">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="d203e-143">In development and testing scenarios.</span></span>
* <span data-ttu-id="d203e-144">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="d203e-144">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="d203e-145">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="d203e-145">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="d203e-146">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="d203e-146">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="d203e-147">當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d203e-147">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="d203e-148">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-148">Distributed SQL Server Cache</span></span>

<span data-ttu-id="d203e-149">分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="d203e-149">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="d203e-150">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="d203e-150">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="d203e-151">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="d203e-151">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="d203e-152">執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-152">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="d203e-153">提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：</span><span class="sxs-lookup"><span data-stu-id="d203e-153">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="d203e-154">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="d203e-154">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="d203e-155">此工具所建立的資料表 `sql-cache` 具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="d203e-155">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="d203e-157">應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。</span><span class="sxs-lookup"><span data-stu-id="d203e-157">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="d203e-158">範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：</span><span class="sxs-lookup"><span data-stu-id="d203e-158">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="d203e-159"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*appsettings 儲存） / *。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="d203e-159">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="d203e-160">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="d203e-160">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="d203e-161">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-161">Distributed Redis Cache</span></span>

<span data-ttu-id="d203e-162">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-162">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="d203e-163">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-163">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="d203e-164">應用程式會在的 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 非開發環境中，使用實例（）來設定快取執行 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d203e-164">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

<span data-ttu-id="d203e-165">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="d203e-165">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="d203e-166">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-166">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="d203e-167">`redis-server`從命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="d203e-167">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="d203e-168">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-168">Distributed NCache Cache</span></span>

<span data-ttu-id="d203e-169">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-169">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="d203e-170">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="d203e-170">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="d203e-171">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-171">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="d203e-172">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="d203e-172">To configure NCache:</span></span>

1. <span data-ttu-id="d203e-173">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-173">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="d203e-174">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="d203e-174">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="d203e-175">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="d203e-175">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="d203e-176">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="d203e-176">Use the distributed cache</span></span>

<span data-ttu-id="d203e-177">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="d203e-177">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="d203e-178">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="d203e-178">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="d203e-179">當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-179">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="d203e-180">目前的時間是使用快取的 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> （如需詳細資訊，請參閱[泛型主機： IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)）：</span><span class="sxs-lookup"><span data-stu-id="d203e-180">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="d203e-181">範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="d203e-181">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="d203e-182">每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-182">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="d203e-183">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="d203e-183">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="d203e-184">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="d203e-184">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="d203e-185">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="d203e-185">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="d203e-186">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="d203e-186">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="d203e-187">實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="d203e-187">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="d203e-188">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="d203e-188">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="d203e-189">建議</span><span class="sxs-lookup"><span data-stu-id="d203e-189">Recommendations</span></span>

<span data-ttu-id="d203e-190">當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="d203e-190">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="d203e-191">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="d203e-191">Existing infrastructure</span></span>
* <span data-ttu-id="d203e-192">效能需求</span><span class="sxs-lookup"><span data-stu-id="d203e-192">Performance requirements</span></span>
* <span data-ttu-id="d203e-193">成本</span><span class="sxs-lookup"><span data-stu-id="d203e-193">Cost</span></span>
* <span data-ttu-id="d203e-194">小組經驗</span><span class="sxs-lookup"><span data-stu-id="d203e-194">Team experience</span></span>

<span data-ttu-id="d203e-195">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="d203e-195">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="d203e-196">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="d203e-196">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="d203e-197">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="d203e-197">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="d203e-198">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="d203e-198">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="d203e-199">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="d203e-199">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="d203e-200">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="d203e-200">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d203e-201">其他資源</span><span class="sxs-lookup"><span data-stu-id="d203e-201">Additional resources</span></span>

* [<span data-ttu-id="d203e-202">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-202">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="d203e-203">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="d203e-203">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="d203e-204">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="d203e-204">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="d203e-205">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="d203e-205">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="d203e-206">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="d203e-206">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="d203e-207">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="d203e-207">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="d203e-208">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="d203e-208">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="d203e-209">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="d203e-209">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="d203e-210">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="d203e-210">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="d203e-211">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="d203e-211">Doesn't use local memory.</span></span>

<span data-ttu-id="d203e-212">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="d203e-212">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="d203e-213">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-213">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="d203e-214">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="d203e-214">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="d203e-215">無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。</span><span class="sxs-lookup"><span data-stu-id="d203e-215">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="d203e-216">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d203e-216">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d203e-217">必要條件</span><span class="sxs-lookup"><span data-stu-id="d203e-217">Prerequisites</span></span>

<span data-ttu-id="d203e-218">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="d203e-218">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="d203e-219">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)封裝。</span><span class="sxs-lookup"><span data-stu-id="d203e-219">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="d203e-220">Redis 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-220">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="d203e-221">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-221">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="d203e-222">NCache 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-222">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="d203e-223">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="d203e-223">IDistributedCache interface</span></span>

<span data-ttu-id="d203e-224"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="d203e-224">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="d203e-225"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="d203e-225"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*>: Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="d203e-226"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-226"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*>: Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="d203e-227"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="d203e-227"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>: Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="d203e-228"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="d203e-228"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*>: Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="d203e-229">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="d203e-229">Establish distributed caching services</span></span>

<span data-ttu-id="d203e-230">在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-230">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d203e-231">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="d203e-231">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="d203e-232">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="d203e-232">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="d203e-233">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-233">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="d203e-234">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-234">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="d203e-235">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-235">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="d203e-236">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="d203e-236">Distributed Memory Cache</span></span>

<span data-ttu-id="d203e-237">分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d203e-237">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="d203e-238">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-238">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="d203e-239">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="d203e-239">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="d203e-240">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="d203e-240">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="d203e-241">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="d203e-241">In development and testing scenarios.</span></span>
* <span data-ttu-id="d203e-242">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="d203e-242">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="d203e-243">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="d203e-243">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="d203e-244">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="d203e-244">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="d203e-245">當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d203e-245">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="d203e-246">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-246">Distributed SQL Server Cache</span></span>

<span data-ttu-id="d203e-247">分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="d203e-247">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="d203e-248">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="d203e-248">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="d203e-249">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="d203e-249">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="d203e-250">執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-250">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="d203e-251">提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：</span><span class="sxs-lookup"><span data-stu-id="d203e-251">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="d203e-252">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="d203e-252">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="d203e-253">此工具所建立的資料表 `sql-cache` 具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="d203e-253">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="d203e-255">應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。</span><span class="sxs-lookup"><span data-stu-id="d203e-255">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="d203e-256">範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：</span><span class="sxs-lookup"><span data-stu-id="d203e-256">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="d203e-257"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*appsettings 儲存） / *。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="d203e-257">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="d203e-258">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="d203e-258">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="d203e-259">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-259">Distributed Redis Cache</span></span>

<span data-ttu-id="d203e-260">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-260">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="d203e-261">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-261">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="d203e-262">應用程式會在的 <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> 非開發環境中，使用實例（）來設定快取執行 <xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d203e-262">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

<span data-ttu-id="d203e-263">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="d203e-263">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="d203e-264">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-264">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="d203e-265">`redis-server`從命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="d203e-265">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="d203e-266">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-266">Distributed NCache Cache</span></span>

<span data-ttu-id="d203e-267">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-267">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="d203e-268">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="d203e-268">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="d203e-269">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-269">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="d203e-270">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="d203e-270">To configure NCache:</span></span>

1. <span data-ttu-id="d203e-271">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-271">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="d203e-272">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="d203e-272">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="d203e-273">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="d203e-273">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="d203e-274">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="d203e-274">Use the distributed cache</span></span>

<span data-ttu-id="d203e-275">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="d203e-275">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="d203e-276">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="d203e-276">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="d203e-277">當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-277">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="d203e-278">目前的時間是使用快取的 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> （如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="d203e-278">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="d203e-279">範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="d203e-279">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="d203e-280">每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-280">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="d203e-281">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="d203e-281">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="d203e-282">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="d203e-282">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="d203e-283">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="d203e-283">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="d203e-284">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="d203e-284">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="d203e-285">實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="d203e-285">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="d203e-286">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="d203e-286">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="d203e-287">建議</span><span class="sxs-lookup"><span data-stu-id="d203e-287">Recommendations</span></span>

<span data-ttu-id="d203e-288">當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="d203e-288">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="d203e-289">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="d203e-289">Existing infrastructure</span></span>
* <span data-ttu-id="d203e-290">效能需求</span><span class="sxs-lookup"><span data-stu-id="d203e-290">Performance requirements</span></span>
* <span data-ttu-id="d203e-291">成本</span><span class="sxs-lookup"><span data-stu-id="d203e-291">Cost</span></span>
* <span data-ttu-id="d203e-292">小組經驗</span><span class="sxs-lookup"><span data-stu-id="d203e-292">Team experience</span></span>

<span data-ttu-id="d203e-293">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="d203e-293">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="d203e-294">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="d203e-294">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="d203e-295">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="d203e-295">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="d203e-296">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="d203e-296">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="d203e-297">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="d203e-297">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="d203e-298">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="d203e-298">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d203e-299">其他資源</span><span class="sxs-lookup"><span data-stu-id="d203e-299">Additional resources</span></span>

* [<span data-ttu-id="d203e-300">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-300">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="d203e-301">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="d203e-301">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="d203e-302">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="d203e-302">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d203e-303">「分散式快取」是由多個應用程式伺服器共用的快取，通常會以外部服務的形式保留給存取該快取的應用程式伺服器。</span><span class="sxs-lookup"><span data-stu-id="d203e-303">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="d203e-304">分散式快取可以改善 ASP.NET Core 應用程式的效能和擴充性，特別是當應用程式是由雲端服務或伺服器陣列所裝載時。</span><span class="sxs-lookup"><span data-stu-id="d203e-304">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="d203e-305">分散式快取與其他快取案例相比，有數個優點，其中快取的資料會儲存在個別應用程式伺服器上。</span><span class="sxs-lookup"><span data-stu-id="d203e-305">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="d203e-306">散發快取的資料時，資料：</span><span class="sxs-lookup"><span data-stu-id="d203e-306">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="d203e-307">跨多部*伺服器的要求之間具有一致*（一致）。</span><span class="sxs-lookup"><span data-stu-id="d203e-307">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="d203e-308">不受伺服器重新開機和應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="d203e-308">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="d203e-309">不使用本機記憶體。</span><span class="sxs-lookup"><span data-stu-id="d203e-309">Doesn't use local memory.</span></span>

<span data-ttu-id="d203e-310">分散式快取設定是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="d203e-310">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="d203e-311">本文說明如何設定 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-311">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="d203e-312">協力廠商實現也可以使用，例如[NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) （[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）。</span><span class="sxs-lookup"><span data-stu-id="d203e-312">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="d203e-313">無論選取哪一個實作為，應用程式都會使用介面與快取互動 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 。</span><span class="sxs-lookup"><span data-stu-id="d203e-313">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="d203e-314">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="d203e-314">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d203e-315">必要條件</span><span class="sxs-lookup"><span data-stu-id="d203e-315">Prerequisites</span></span>

<span data-ttu-id="d203e-316">若要使用 SQL Server 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[Microsoft Extensions. Caching](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer)封裝。</span><span class="sxs-lookup"><span data-stu-id="d203e-316">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="d203e-317">若要使用 Redis 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis)封裝。</span><span class="sxs-lookup"><span data-stu-id="d203e-317">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="d203e-318">Redis 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 Redis 套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-318">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="d203e-319">若要使用 NCache 分散式快取，請參考[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，並將套件參考新增至[NCache.](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource)套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-319">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="d203e-320">NCache 套件不包含在封裝中 `Microsoft.AspNetCore.App` ，因此您必須在專案檔中分別參考 NCache 套件。</span><span class="sxs-lookup"><span data-stu-id="d203e-320">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

## <a name="idistributedcache-interface"></a><span data-ttu-id="d203e-321">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="d203e-321">IDistributedCache interface</span></span>

<span data-ttu-id="d203e-322"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>介面提供下列方法來操作分散式快取實作為中的專案：</span><span class="sxs-lookup"><span data-stu-id="d203e-322">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="d203e-323"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> ：接受字串索引鍵，並在快取中找到快取的專案做為 `byte[]` 陣列。</span><span class="sxs-lookup"><span data-stu-id="d203e-323"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*>: Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="d203e-324"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> ：使用字串索引鍵將專案（ `byte[]` 陣列）新增至快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-324"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*>: Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="d203e-325"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>，：根據索引鍵重新整理快取 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> 中的專案，並重設其滑動到期時間（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="d203e-325"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*>: Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="d203e-326"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ：根據其字串索引鍵移除快取專案。</span><span class="sxs-lookup"><span data-stu-id="d203e-326"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*>: Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="d203e-327">建立分散式快取服務</span><span class="sxs-lookup"><span data-stu-id="d203e-327">Establish distributed caching services</span></span>

<span data-ttu-id="d203e-328">在中註冊的 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 執行 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-328">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d203e-329">本主題所描述的架構提供的實作為包括：</span><span class="sxs-lookup"><span data-stu-id="d203e-329">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="d203e-330">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="d203e-330">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="d203e-331">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-331">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="d203e-332">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-332">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="d203e-333">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-333">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="d203e-334">分散式記憶體快取</span><span class="sxs-lookup"><span data-stu-id="d203e-334">Distributed Memory Cache</span></span>

<span data-ttu-id="d203e-335">分散式記憶體快取（ <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*> ）是一種架構提供的執行 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，可將專案儲存于記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d203e-335">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="d203e-336">分散式記憶體快取不是實際的分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-336">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="d203e-337">在應用程式執行所在的伺服器上，應用程式實例會儲存快取的專案。</span><span class="sxs-lookup"><span data-stu-id="d203e-337">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="d203e-338">分散式記憶體快取是一種實用的執行方式：</span><span class="sxs-lookup"><span data-stu-id="d203e-338">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="d203e-339">在開發和測試案例中。</span><span class="sxs-lookup"><span data-stu-id="d203e-339">In development and testing scenarios.</span></span>
* <span data-ttu-id="d203e-340">在生產環境中使用單一伺服器時，記憶體耗用量並不會產生問題。</span><span class="sxs-lookup"><span data-stu-id="d203e-340">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="d203e-341">執行分散式記憶體快取會抽象化快取的資料存放區。</span><span class="sxs-lookup"><span data-stu-id="d203e-341">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="d203e-342">如果需要多個節點或容錯功能，它可讓您在未來執行真正的分散式快取解決方案。</span><span class="sxs-lookup"><span data-stu-id="d203e-342">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="d203e-343">當應用程式在開發環境中執行時，範例應用程式會使用分散式記憶體快取 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d203e-343">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="d203e-344">分散式 SQL Server 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-344">Distributed SQL Server Cache</span></span>

<span data-ttu-id="d203e-345">分散式 SQL Server 快取執行（ <xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*> ）可讓分散式快取使用 SQL Server 資料庫作為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="d203e-345">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="d203e-346">若要在 SQL Server 實例中建立 SQL Server 快取的專案資料表，您可以使用 `sql-cache` 工具。</span><span class="sxs-lookup"><span data-stu-id="d203e-346">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="d203e-347">此工具會建立一個資料表，其中包含您指定的名稱和架構。</span><span class="sxs-lookup"><span data-stu-id="d203e-347">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="d203e-348">執行命令，在 SQL Server 中建立資料表 `sql-cache create` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-348">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="d203e-349">提供 SQL Server 實例（ `Data Source` ）、資料庫（ `Initial Catalog` ）、架構（例如 `dbo` ）和資料表名稱（例如 `TestCache` ）：</span><span class="sxs-lookup"><span data-stu-id="d203e-349">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="d203e-350">會記錄一則訊息，指出工具已成功：</span><span class="sxs-lookup"><span data-stu-id="d203e-350">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="d203e-351">此工具所建立的資料表 `sql-cache` 具有下列架構：</span><span class="sxs-lookup"><span data-stu-id="d203e-351">The table created by the `sql-cache` tool has the following schema:</span></span>

![SqlServer 快取資料表](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="d203e-353">應用程式應該使用的實例來操作快取值 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> ，而不是 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 。</span><span class="sxs-lookup"><span data-stu-id="d203e-353">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="d203e-354">範例應用程式會在的 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> 非開發環境中 `Startup.ConfigureServices` 執行：</span><span class="sxs-lookup"><span data-stu-id="d203e-354">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="d203e-355"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*>（而且選擇性地， <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> 和 <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*> ）通常會儲存在原始檔控制外部（例如，由[秘密管理員](xref:security/app-secrets)或*appsettings. json*appsettings 儲存） / *。環境} json*檔案）。</span><span class="sxs-lookup"><span data-stu-id="d203e-355">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="d203e-356">連接字串可能包含應保留在原始檔控制系統中的認證。</span><span class="sxs-lookup"><span data-stu-id="d203e-356">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="d203e-357">分散式 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-357">Distributed Redis Cache</span></span>

<span data-ttu-id="d203e-358">[Redis](https://redis.io/)是一個開放原始碼記憶體內部資料存放區，通常用來做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-358">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="d203e-359">您可以在本機使用 Redis，而且您可以為 Azure 託管的 ASP.NET Core 應用程式設定[Azure Redis](https://azure.microsoft.com/services/cache/)快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-359">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

<span data-ttu-id="d203e-360">應用程式會使用實例（）來設定快取執行 <xref:Microsoft.Extensions.Caching.Redis.RedisCache> <xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*> ：</span><span class="sxs-lookup"><span data-stu-id="d203e-360">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

<span data-ttu-id="d203e-361">若要在本機電腦上安裝 Redis：</span><span class="sxs-lookup"><span data-stu-id="d203e-361">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="d203e-362">安裝[Chocolatey Redis 套件](https://chocolatey.org/packages/redis-64/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-362">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="d203e-363">`redis-server`從命令提示字元執行。</span><span class="sxs-lookup"><span data-stu-id="d203e-363">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="d203e-364">分散式 NCache 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-364">Distributed NCache Cache</span></span>

<span data-ttu-id="d203e-365">[NCache](https://github.com/Alachisoft/NCache)是在 .NET 和 .net Core 中以原生方式開發的開放原始碼記憶體內部分散式快取。</span><span class="sxs-lookup"><span data-stu-id="d203e-365">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="d203e-366">NCache 可在本機運作，並已設定為在 Azure 或其他裝載平臺上執行之 ASP.NET Core 應用程式的分散式快取叢集。</span><span class="sxs-lookup"><span data-stu-id="d203e-366">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="d203e-367">若要在本機電腦上安裝及設定 NCache，請參閱[適用于 Windows 的 NCache 消費者入門指南](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-367">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="d203e-368">若要設定 NCache：</span><span class="sxs-lookup"><span data-stu-id="d203e-368">To configure NCache:</span></span>

1. <span data-ttu-id="d203e-369">安裝[NCache 開放原始碼 NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)。</span><span class="sxs-lookup"><span data-stu-id="d203e-369">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="d203e-370">在[ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html)中設定快取叢集。</span><span class="sxs-lookup"><span data-stu-id="d203e-370">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="d203e-371">將下列程式碼新增至 `Startup.ConfigureServices`：</span><span class="sxs-lookup"><span data-stu-id="d203e-371">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="d203e-372">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="d203e-372">Use the distributed cache</span></span>

<span data-ttu-id="d203e-373">若要使用 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面，請 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 從應用程式中的任何一個函式要求的實例。</span><span class="sxs-lookup"><span data-stu-id="d203e-373">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="d203e-374">實例是由相依性[插入（DI）](xref:fundamentals/dependency-injection)提供。</span><span class="sxs-lookup"><span data-stu-id="d203e-374">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="d203e-375">當範例應用程式啟動時， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 會插入 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-375">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="d203e-376">目前的時間是使用快取的 <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> （如需詳細資訊，請參閱[Web 主機： IApplicationLifetime 介面](xref:fundamentals/host/web-host#iapplicationlifetime-interface)）：</span><span class="sxs-lookup"><span data-stu-id="d203e-376">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="d203e-377">範例應用程式會將插入， <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> `IndexModel` 以供 [索引] 頁面使用。</span><span class="sxs-lookup"><span data-stu-id="d203e-377">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="d203e-378">每次載入 [索引] 頁面時，都會檢查快取的快取時間 `OnGetAsync` 。</span><span class="sxs-lookup"><span data-stu-id="d203e-378">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="d203e-379">如果快取的時間尚未過期，則會顯示時間。</span><span class="sxs-lookup"><span data-stu-id="d203e-379">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="d203e-380">如果自上次存取快取時間之後已經過20秒（上次載入此頁面的時間），頁面就會顯示快取的*時間已過期*。</span><span class="sxs-lookup"><span data-stu-id="d203e-380">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="d203e-381">選取 [**重設**快取時間] 按鈕，立即將快取的時間更新為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="d203e-381">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="d203e-382">按鈕會觸發 `OnPostResetCachedTime` 處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="d203e-382">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="d203e-383">實例不需要使用單一或範圍存留期 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> （至少適用于內建的實作為）。</span><span class="sxs-lookup"><span data-stu-id="d203e-383">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="d203e-384">您也可以建立 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實例，而不需要使用 DI，但在程式碼中建立實例可能會使您的程式碼更難測試，並違反[明確](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)的相依性準則。</span><span class="sxs-lookup"><span data-stu-id="d203e-384">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="d203e-385">建議</span><span class="sxs-lookup"><span data-stu-id="d203e-385">Recommendations</span></span>

<span data-ttu-id="d203e-386">當決定哪一個 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 最適合您的應用程式時，請考慮下列事項：</span><span class="sxs-lookup"><span data-stu-id="d203e-386">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="d203e-387">現有基礎結構</span><span class="sxs-lookup"><span data-stu-id="d203e-387">Existing infrastructure</span></span>
* <span data-ttu-id="d203e-388">效能需求</span><span class="sxs-lookup"><span data-stu-id="d203e-388">Performance requirements</span></span>
* <span data-ttu-id="d203e-389">成本</span><span class="sxs-lookup"><span data-stu-id="d203e-389">Cost</span></span>
* <span data-ttu-id="d203e-390">小組經驗</span><span class="sxs-lookup"><span data-stu-id="d203e-390">Team experience</span></span>

<span data-ttu-id="d203e-391">快取解決方案通常會依賴記憶體內部儲存體，以快速抓取快取的資料，但是記憶體是有限的資源，而且需要擴充的成本。</span><span class="sxs-lookup"><span data-stu-id="d203e-391">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="d203e-392">只會將常用的資料儲存在快取中。</span><span class="sxs-lookup"><span data-stu-id="d203e-392">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="d203e-393">一般來說，Redis 快取提供的輸送量較高，且延遲比 SQL Server 快取更低。</span><span class="sxs-lookup"><span data-stu-id="d203e-393">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="d203e-394">不過，通常需要進行基準測試，以判斷快取策略的效能特性。</span><span class="sxs-lookup"><span data-stu-id="d203e-394">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="d203e-395">當 SQL Server 做為分散式快取備份存放區時，使用相同的資料庫進行快取，應用程式的一般資料儲存和抓取可能會對兩者的效能產生負面影響。</span><span class="sxs-lookup"><span data-stu-id="d203e-395">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="d203e-396">我們建議使用分散式快取備份存放區的專用 SQL Server 實例。</span><span class="sxs-lookup"><span data-stu-id="d203e-396">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d203e-397">其他資源</span><span class="sxs-lookup"><span data-stu-id="d203e-397">Additional resources</span></span>

* [<span data-ttu-id="d203e-398">Azure 上的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="d203e-398">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="d203e-399">Azure 上的 SQL Database</span><span class="sxs-lookup"><span data-stu-id="d203e-399">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="d203e-400">[Web 伺服陣列中 NCache 的 ASP.NET Core IDistributedCache 提供者](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)（[GitHub 上的 NCache](https://github.com/Alachisoft/NCache)）</span><span class="sxs-lookup"><span data-stu-id="d203e-400">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>

::: moniker-end
 