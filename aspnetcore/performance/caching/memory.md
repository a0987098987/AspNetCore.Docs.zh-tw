---
title: "記憶體中快取中 ASP.NET Core"
author: rick-anderson
description: "示範如何快取在記憶體中 ASP.NET Core 的資料。"
keywords: "ASP.NET Core，快取中，記憶體中效能"
ms.author: riande
manager: wpickett
ms.date: 12/14/2016
ms.topic: article
ms.assetid: 819511cf-d33e-410a-b5a9-bef7fa64d2f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/memory
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5ce865427b6ca44c76888908fdeea9cd45c881c4
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-in-memory-caching-in-aspnet-core"></a><span data-ttu-id="dcb57-104">介紹記憶體中快取中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dcb57-104">Introduction to in-memory caching in ASP.NET Core</span></span>

<span data-ttu-id="dcb57-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [John Luo](https://github.com/JunTaoLuo)，和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="dcb57-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="dcb57-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dcb57-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="dcb57-107">快取的基本概念</span><span class="sxs-lookup"><span data-stu-id="dcb57-107">Caching basics</span></span>

<span data-ttu-id="dcb57-108">快取可大幅改善的效能和延展性的應用程式藉由減少產生內容所需的工作。</span><span class="sxs-lookup"><span data-stu-id="dcb57-108">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="dcb57-109">快取就可以最適合不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="dcb57-109">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="dcb57-110">快取會建立一份可以傳回大部分的資料較快，從原始來源。</span><span class="sxs-lookup"><span data-stu-id="dcb57-110">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="dcb57-111">您應該撰寫並測試您的應用程式永遠不會取決於快取的資料。</span><span class="sxs-lookup"><span data-stu-id="dcb57-111">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="dcb57-112">ASP.NET Core 支援數個不同的快取。</span><span class="sxs-lookup"><span data-stu-id="dcb57-112">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="dcb57-113">最簡單的快取根據[IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache)，代表儲存在 web 伺服器的記憶體中快取。</span><span class="sxs-lookup"><span data-stu-id="dcb57-113">The simplest cache is based on the [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="dcb57-114">在多部伺服器的伺服器陣列執行的應用程式應該確保使用記憶體中快取時，會自黏工作階段。</span><span class="sxs-lookup"><span data-stu-id="dcb57-114">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="dcb57-115">自黏工作階段會確保後續所有用戶端的要求移到相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="dcb57-115">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="dcb57-116">例如，Azure Web 應用程式使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR) 會將所有的後續要求路由至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="dcb57-116">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="dcb57-117">Web 伺服陣列中的非黏性工作階段需要[分散式快取](distributed.md)以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="dcb57-117">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="dcb57-118">對於某些應用程式中，分散式快取可以支援更高範圍外比記憶體中快取。</span><span class="sxs-lookup"><span data-stu-id="dcb57-118">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="dcb57-119">使用分散式快取卸載到外部處理序的快取記憶體。</span><span class="sxs-lookup"><span data-stu-id="dcb57-119">Using a distributed cache offloads the cache memory to an external process.</span></span> 

<span data-ttu-id="dcb57-120">`IMemoryCache`快取將會收回快取記憶體不足壓力下的項目，除非[快取優先順序](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority)設`CacheItemPriority.NeverRemove`。</span><span class="sxs-lookup"><span data-stu-id="dcb57-120">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="dcb57-121">您可以設定`CacheItemPriority`調整優先順序快取收回記憶體不足壓力下的項目。</span><span class="sxs-lookup"><span data-stu-id="dcb57-121">You can set the `CacheItemPriority` to adjust the priority the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="dcb57-122">記憶體中快取可以儲存任何物件。分散式快取介面僅限於`byte[]`。</span><span class="sxs-lookup"><span data-stu-id="dcb57-122">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="dcb57-123">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="dcb57-123">Using IMemoryCache</span></span>

<span data-ttu-id="dcb57-124">記憶體中快取是*服務*參考從您的應用程式使用[相依性插入](../../fundamentals/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="dcb57-124">In-memory caching is a *service* that is referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="dcb57-125">呼叫`AddMemoryCache`中`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="dcb57-125">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

<span data-ttu-id="dcb57-126">要求`IMemoryCache`建構函式中的執行個體：</span><span class="sxs-lookup"><span data-stu-id="dcb57-126">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

<span data-ttu-id="dcb57-127">`IMemoryCache`需要 NuGet 套件 」 Microsoft.Extensions.Caching.Memory"。</span><span class="sxs-lookup"><span data-stu-id="dcb57-127">`IMemoryCache` requires NuGet package "Microsoft.Extensions.Caching.Memory".</span></span>

<span data-ttu-id="dcb57-128">下列程式碼會使用[TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查目前是否為快取中。</span><span class="sxs-lookup"><span data-stu-id="dcb57-128">The following code uses [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if the current time is in the cache.</span></span> <span data-ttu-id="dcb57-129">如果沒有快取項目，建立並與快取中加入新項目[設定](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_)。</span><span class="sxs-lookup"><span data-stu-id="dcb57-129">If the item is not cached, a new entry is created and added to the cache with [Set](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="dcb57-130">會顯示目前的時間和快取的時間：</span><span class="sxs-lookup"><span data-stu-id="dcb57-130">The current time and the cached time is displayed:</span></span>

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="dcb57-131">快取`DateTime`值將會留在快取，而沒有要求的逾時期限 （及任何因記憶體不足的壓力而收回） 內。</span><span class="sxs-lookup"><span data-stu-id="dcb57-131">The cached `DateTime` value will remain in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="dcb57-132">下圖顯示目前的時間和時間早從快取擷取：</span><span class="sxs-lookup"><span data-stu-id="dcb57-132">The image below shows the current time and an older time retrieved from cache:</span></span>

![具有兩個不同的時間顯示的索引檢視](memory/_static/time.png)

<span data-ttu-id="dcb57-134">下列程式碼會使用[GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)快取資料。</span><span class="sxs-lookup"><span data-stu-id="dcb57-134">The following code uses [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="dcb57-135">下列程式碼會呼叫[取得](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)來擷取快取的時間：</span><span class="sxs-lookup"><span data-stu-id="dcb57-135">The following code calls [Get](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="dcb57-136">請參閱[IMemoryCache 方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions)的快取方法的描述。</span><span class="sxs-lookup"><span data-stu-id="dcb57-136">See [IMemoryCache methods](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="dcb57-137">使用 MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="dcb57-137">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="dcb57-138">下列範例：</span><span class="sxs-lookup"><span data-stu-id="dcb57-138">The following sample:</span></span>

- <span data-ttu-id="dcb57-139">設定絕對到期時間。</span><span class="sxs-lookup"><span data-stu-id="dcb57-139">Sets the absolute expiration time.</span></span> <span data-ttu-id="dcb57-140">這是可以快取之項目的時間上限，並防止持續更新變動到期後變成過時的項目。</span><span class="sxs-lookup"><span data-stu-id="dcb57-140">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="dcb57-141">設定滑動期限。</span><span class="sxs-lookup"><span data-stu-id="dcb57-141">Sets a sliding expiration time.</span></span> <span data-ttu-id="dcb57-142">要求存取此快取的項目會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="dcb57-142">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="dcb57-143">若要設定快取優先權`CacheItemPriority.NeverRemove`。</span><span class="sxs-lookup"><span data-stu-id="dcb57-143">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span> 
- <span data-ttu-id="dcb57-144">設定[PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) ，將會在從快取收回項目之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="dcb57-144">Sets a [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="dcb57-145">回呼的程式碼快取中移除的項目與另一個執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="dcb57-145">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a><span data-ttu-id="dcb57-146">快取相依性</span><span class="sxs-lookup"><span data-stu-id="dcb57-146">Cache dependencies</span></span>

<span data-ttu-id="dcb57-147">下列範例會示範如何過期的快取項目，如果相依項目到期為止。</span><span class="sxs-lookup"><span data-stu-id="dcb57-147">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="dcb57-148">A`CancellationChangeToken`新增至快取的項目。</span><span class="sxs-lookup"><span data-stu-id="dcb57-148">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="dcb57-149">當`Cancel`上呼叫`CancellationTokenSource`，收回兩個快取項目。</span><span class="sxs-lookup"><span data-stu-id="dcb57-149">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span> 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="dcb57-150">使用`CancellationTokenSource`可讓多個快取項目被收回為群組。</span><span class="sxs-lookup"><span data-stu-id="dcb57-150">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="dcb57-151">與`using`上述程式碼模式，快取項目內建立`using`區塊將會繼承觸發程序和到期日設定。</span><span class="sxs-lookup"><span data-stu-id="dcb57-151">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="dcb57-152">其他備註</span><span class="sxs-lookup"><span data-stu-id="dcb57-152">Additional notes</span></span>

- <span data-ttu-id="dcb57-153">使用時回呼重新擴展快取項目：</span><span class="sxs-lookup"><span data-stu-id="dcb57-153">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="dcb57-154">多個要求可以找到的快取索引鍵值空因為尚未完成的回呼。</span><span class="sxs-lookup"><span data-stu-id="dcb57-154">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span> 
  - <span data-ttu-id="dcb57-155">這會導致多個執行緒重新填入快取的項目。</span><span class="sxs-lookup"><span data-stu-id="dcb57-155">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="dcb57-156">若要建立另一個使用一個快取項目時，子將複製的父項目到期語彙基元和以時間為基礎的到期日設定。</span><span class="sxs-lookup"><span data-stu-id="dcb57-156">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="dcb57-157">子不是手動移除已過期或更新的父項目。</span><span class="sxs-lookup"><span data-stu-id="dcb57-157">The child is not expired by manual removal or updating of the parent entry.</span></span>

### <a name="other-resources"></a><span data-ttu-id="dcb57-158">其他資源</span><span class="sxs-lookup"><span data-stu-id="dcb57-158">Other Resources</span></span>

* [<span data-ttu-id="dcb57-159">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="dcb57-159">Working with a Distributed Cache</span></span>](distributed.md)
* [<span data-ttu-id="dcb57-160">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="dcb57-160">Response caching middleware</span></span>](middleware.md)
