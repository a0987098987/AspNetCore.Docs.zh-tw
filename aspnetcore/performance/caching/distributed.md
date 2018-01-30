---
title: "使用分散式快取中 ASP.NET Core"
author: ardalis
description: "了解如何使用分散式快取以改善效能和延展性的 ASP.NET Core 應用程式，尤其是裝載於雲端或伺服器的伺服陣列環境中。"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 86fd40863f6eeef3c129335141d704769d36b4c1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="4cd38-103">使用分散式快取中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cd38-103">Working with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="4cd38-104">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4cd38-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4cd38-105">分散式快取可以改善效能和延展性的 ASP.NET Core 應用程式，尤其是裝載於雲端或伺服器的伺服陣列環境中。</span><span class="sxs-lookup"><span data-stu-id="4cd38-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="4cd38-106">本文說明如何使用 ASP.NET Core 內建的分散式快取的抽象概念和實作。</span><span class="sxs-lookup"><span data-stu-id="4cd38-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="4cd38-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4cd38-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="4cd38-108">什麼是分散式快取</span><span class="sxs-lookup"><span data-stu-id="4cd38-108">What is a distributed cache</span></span>

<span data-ttu-id="4cd38-109">分散式快取由多個應用程式伺服器共用 (請參閱[快取的基本概念](memory.md#caching-basics))。</span><span class="sxs-lookup"><span data-stu-id="4cd38-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="4cd38-110">快取中的資訊不會儲存在個別 web 伺服器的記憶體，而且快取的資料可用於所有應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="4cd38-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="4cd38-111">這會提供數個優點：</span><span class="sxs-lookup"><span data-stu-id="4cd38-111">This provides several advantages:</span></span>

1. <span data-ttu-id="4cd38-112">快取的資料是一致的所有 web 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="4cd38-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="4cd38-113">使用者沒有看到不同的結果，根據哪個 web 伺服器會處理其要求</span><span class="sxs-lookup"><span data-stu-id="4cd38-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="4cd38-114">快取的資料未 web 伺服器重新啟動並部署。</span><span class="sxs-lookup"><span data-stu-id="4cd38-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="4cd38-115">個別的網頁伺服器可以移除或加入，而不會影響快取。</span><span class="sxs-lookup"><span data-stu-id="4cd38-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="4cd38-116">來源資料存放區有較少對它 （非多個記憶體中快取或完全不需要與快取完全） 提出的要求。</span><span class="sxs-lookup"><span data-stu-id="4cd38-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="4cd38-117">如果使用 SQL Server 分散式快取，這些優點有些只比快取中的應用程式的來源資料使用不同的資料庫執行個體，如果為 true。</span><span class="sxs-lookup"><span data-stu-id="4cd38-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="4cd38-118">任何快取中，例如分散式快取可以大幅改善應用程式的回應性，通常比更快，從關聯式資料庫 （或 web 服務） 快取中擷取資料之後。</span><span class="sxs-lookup"><span data-stu-id="4cd38-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="4cd38-119">快取設定是特定的實作。</span><span class="sxs-lookup"><span data-stu-id="4cd38-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="4cd38-120">本文說明如何設定兩者 Redis 和 SQL Server 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="4cd38-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="4cd38-121">無論選取哪一個實作時，應用程式互動使用共同的快取`IDistributedCache`介面。</span><span class="sxs-lookup"><span data-stu-id="4cd38-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="4cd38-122">IDistributedCache 介面</span><span class="sxs-lookup"><span data-stu-id="4cd38-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="4cd38-123">`IDistributedCache`介面包括同步和非同步方法。</span><span class="sxs-lookup"><span data-stu-id="4cd38-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="4cd38-124">介面允許加入、 擷取，並從分散式快取實作中移除的項目。</span><span class="sxs-lookup"><span data-stu-id="4cd38-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="4cd38-125">`IDistributedCache`介面包含下列方法：</span><span class="sxs-lookup"><span data-stu-id="4cd38-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="4cd38-126">**Get、 GetAsync**</span><span class="sxs-lookup"><span data-stu-id="4cd38-126">**Get, GetAsync**</span></span>

<span data-ttu-id="4cd38-127">會採用字串索引鍵，並擷取快取的項目為`byte[]`如果快取中找到。</span><span class="sxs-lookup"><span data-stu-id="4cd38-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="4cd38-128">**集合 SetAsync**</span><span class="sxs-lookup"><span data-stu-id="4cd38-128">**Set, SetAsync**</span></span>

<span data-ttu-id="4cd38-129">將項目 (做為`byte[]`) 至快取使用字串索引鍵。</span><span class="sxs-lookup"><span data-stu-id="4cd38-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="4cd38-130">**重新整理 RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="4cd38-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="4cd38-131">重新整理它的金鑰，重設其滑動到期逾時 （如果有的話） 為基礎的快取中的項目。</span><span class="sxs-lookup"><span data-stu-id="4cd38-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="4cd38-132">**Remove、 RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="4cd38-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="4cd38-133">根據索引鍵的快取項目中移除。</span><span class="sxs-lookup"><span data-stu-id="4cd38-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="4cd38-134">若要使用`IDistributedCache`介面：</span><span class="sxs-lookup"><span data-stu-id="4cd38-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="4cd38-135">將必要的 NuGet 封裝加入至您的專案檔。</span><span class="sxs-lookup"><span data-stu-id="4cd38-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="4cd38-136">設定的特定實作`IDistributedCache`中您`Startup`類別的`ConfigureServices`方法，並將它新增至那里容器。</span><span class="sxs-lookup"><span data-stu-id="4cd38-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="4cd38-137">從應用程式的[中介軟體](../../fundamentals/middleware.md)MVC 控制器類別中，要求的執行個體或`IDistributedCache`從建構函式。</span><span class="sxs-lookup"><span data-stu-id="4cd38-137">From the app's [Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="4cd38-138">執行個體將會由提供[相依性插入](../../fundamentals/dependency-injection.md)(DI)。</span><span class="sxs-lookup"><span data-stu-id="4cd38-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="4cd38-139">若要使用的單一或 Scoped 存留期不需要`IDistributedCache`執行個體 (至少為內建實作)。</span><span class="sxs-lookup"><span data-stu-id="4cd38-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="4cd38-140">您也可以建立執行個體，只要您可能需要一個 (而不是使用[相依性插入](../../fundamentals/dependency-injection.md))，但是這可以讓您的程式碼不容易進行測試，且違反[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="4cd38-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="4cd38-141">下列範例示範如何使用的執行個體`IDistributedCache`簡單的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="4cd38-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]

<span data-ttu-id="4cd38-142">在上述程式碼快取的值是讀取，但永遠不會寫入。</span><span class="sxs-lookup"><span data-stu-id="4cd38-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="4cd38-143">在此範例中，當伺服器啟動，而且不會變更時，才會設定值。</span><span class="sxs-lookup"><span data-stu-id="4cd38-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="4cd38-144">在多伺服器案例中，最新的伺服器，準備開始將會覆寫任何先前由其他伺服器設定的值。</span><span class="sxs-lookup"><span data-stu-id="4cd38-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="4cd38-145">`Get`和`Set`方法使用`byte[]`型別。</span><span class="sxs-lookup"><span data-stu-id="4cd38-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="4cd38-146">因此，字串值必須轉換使用`Encoding.UTF8.GetString`(如`Get`) 和`Encoding.UTF8.GetBytes`(如`Set`)。</span><span class="sxs-lookup"><span data-stu-id="4cd38-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="4cd38-147">下列程式碼會從*Startup.cs*示範所設定的值：</span><span class="sxs-lookup"><span data-stu-id="4cd38-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]

> [!NOTE]
> <span data-ttu-id="4cd38-148">因為`IDistributedCache`中設定`ConfigureServices`方法，您就能夠`Configure`做為參數的方法。</span><span class="sxs-lookup"><span data-stu-id="4cd38-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="4cd38-149">將它加入做為參數，可讓透過 DI 提供設定的執行個體。</span><span class="sxs-lookup"><span data-stu-id="4cd38-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="4cd38-150">使用分散式的 Redis 快取</span><span class="sxs-lookup"><span data-stu-id="4cd38-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="4cd38-151">[Redis](https://redis.io/)是開放原始碼記憶體中的資料存放區，通常是做為分散式快取。</span><span class="sxs-lookup"><span data-stu-id="4cd38-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="4cd38-152">您可以使用在本機，並且可以設定[Azure Redis 快取](https://azure.microsoft.com/services/cache/)Azure 裝載的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4cd38-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="4cd38-153">您的 ASP.NET Core 應用程式會設定快取實作使用`RedisDistributedCache`執行個體。</span><span class="sxs-lookup"><span data-stu-id="4cd38-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="4cd38-154">設定中的 Redis 實作`ConfigureServices`，而且應用程式程式碼中存取所要求的執行個體`IDistributedCache`（請參閱上面的程式碼）。</span><span class="sxs-lookup"><span data-stu-id="4cd38-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="4cd38-155">範例程式碼，`RedisCache`設定伺服器時，會使用實作`Staging`環境。</span><span class="sxs-lookup"><span data-stu-id="4cd38-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="4cd38-156">因此`ConfigureStagingServices`方法會設定`RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="4cd38-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]

> [!NOTE]
> <span data-ttu-id="4cd38-157">若要安裝 Redis 在本機電腦上，安裝 chocolatey 封裝[https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/)並執行`redis-server`從命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="4cd38-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="4cd38-158">使用 SQL Server 分散式快取</span><span class="sxs-lookup"><span data-stu-id="4cd38-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="4cd38-159">SqlServerCache 實作可讓分散式快取，以使用 SQL Server 資料庫做為其備份存放區。</span><span class="sxs-lookup"><span data-stu-id="4cd38-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="4cd38-160">若要建立 SQL Server 資料表，您可以使用 sql 快取工具，此工具會建立資料表，與您指定的名稱和結構描述。</span><span class="sxs-lookup"><span data-stu-id="4cd38-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="4cd38-161">若要使用 sql 快取工具，加入`SqlConfig.Tools`至`<ItemGroup>`元素*.csproj*檔，然後執行 dotnet 還原。</span><span class="sxs-lookup"><span data-stu-id="4cd38-161">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

[!code-xml[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]

<span data-ttu-id="4cd38-162">執行下列命令來測試 SqlConfig.Tools</span><span class="sxs-lookup"><span data-stu-id="4cd38-162">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="4cd38-163">sql 快取工具會顯示使用量、 選項和命令的說明，現在您可以建立資料表至 sql server，執行 「 建立 sql 快取 」 的命令：</span><span class="sxs-lookup"><span data-stu-id="4cd38-163">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="4cd38-164">建立的資料表具有下列結構描述：</span><span class="sxs-lookup"><span data-stu-id="4cd38-164">The created table has the following schema:</span></span>

![SqlServer 快取表格](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="4cd38-166">所有快取實作，例如您的應用程式應該取得並設定使用的執行個體的快取值`IDistributedCache`，而非`SqlServerCache`。</span><span class="sxs-lookup"><span data-stu-id="4cd38-166">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="4cd38-167">此範例會實作`SqlServerCache`中`Production`環境 (因此中設定它`ConfigureProductionServices`)。</span><span class="sxs-lookup"><span data-stu-id="4cd38-167">The sample implements `SqlServerCache` in the `Production` environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]

> [!NOTE]
> <span data-ttu-id="4cd38-168">`ConnectionString` (並選擇性地`SchemaName`和`TableName`) 應該通常會儲存在外部原始檔控制 （例如 UserSecrets)，因為它們可能包含認證。</span><span class="sxs-lookup"><span data-stu-id="4cd38-168">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="4cd38-169">建議</span><span class="sxs-lookup"><span data-stu-id="4cd38-169">Recommendations</span></span>

<span data-ttu-id="4cd38-170">當您決定哪一個實作時`IDistributedCache`是適合您的應用程式中，選擇 Redis 和 SQL Server 根據現有的基礎結構和環境、 您效能需求，以及小組的體驗。</span><span class="sxs-lookup"><span data-stu-id="4cd38-170">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="4cd38-171">如果您的小組更方便使用 Redis，它會是很好的選擇。</span><span class="sxs-lookup"><span data-stu-id="4cd38-171">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="4cd38-172">如果小組慣用的 SQL Server，您可以實作以及信心。</span><span class="sxs-lookup"><span data-stu-id="4cd38-172">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="4cd38-173">請注意傳統快取方案儲存在記憶體資料可快速擷取資料。</span><span class="sxs-lookup"><span data-stu-id="4cd38-173">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="4cd38-174">您應該在快取中儲存常用的資料，並將整個資料儲存在 SQL Server 或 Azure 儲存體之類的後端持續性存放區。</span><span class="sxs-lookup"><span data-stu-id="4cd38-174">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="4cd38-175">Redis 快取是快取的解決方案，可提供您高輸送量和低度延遲相較於 SQL 快取。</span><span class="sxs-lookup"><span data-stu-id="4cd38-175">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cd38-176">其他資源</span><span class="sxs-lookup"><span data-stu-id="4cd38-176">Additional resources</span></span>

* [<span data-ttu-id="4cd38-177">Redis 快取，在 Azure 上</span><span class="sxs-lookup"><span data-stu-id="4cd38-177">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="4cd38-178">在 Azure 上的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="4cd38-178">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="4cd38-179">記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="4cd38-179">In-memory caching</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="4cd38-180">使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="4cd38-180">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="4cd38-181">回應快取</span><span class="sxs-lookup"><span data-stu-id="4cd38-181">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="4cd38-182">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="4cd38-182">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="4cd38-183">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="4cd38-183">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="4cd38-184">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="4cd38-184">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
