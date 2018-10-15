---
title: 快取在記憶體中的 ASP.NET Core
author: rick-anderson
description: 了解如何在 ASP.NET Core 中的記憶體中的資料快取。
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2018
uid: performance/caching/memory
ms.openlocfilehash: 960aa18f9d14f633118ccd716201e61464085c05
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325922"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="68058-103">快取在記憶體中的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68058-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="68058-104">藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [John Luo](https://github.com/JunTaoLuo)，和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="68058-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="68058-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="68058-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="68058-106">快取的基本概念</span><span class="sxs-lookup"><span data-stu-id="68058-106">Caching basics</span></span>

<span data-ttu-id="68058-107">快取可大幅改善的效能和延展性的應用程式藉由減少產生的內容所需的工作。</span><span class="sxs-lookup"><span data-stu-id="68058-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="68058-108">快取的運作最佳不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="68058-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="68058-109">快取會建立一份可以傳回大部分的資料比原始來源的更快。</span><span class="sxs-lookup"><span data-stu-id="68058-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="68058-110">您應該撰寫並測試您的應用程式永遠不會相依於快取的資料。</span><span class="sxs-lookup"><span data-stu-id="68058-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="68058-111">ASP.NET Core 支援數個不同的快取。</span><span class="sxs-lookup"><span data-stu-id="68058-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="68058-112">最簡單的快取為基礎[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)，代表儲存在 web 伺服器的記憶體中快取。</span><span class="sxs-lookup"><span data-stu-id="68058-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="68058-113">在多部伺服器的伺服器陣列執行的應用程式應該確定使用記憶體中快取時，會自黏工作階段。</span><span class="sxs-lookup"><span data-stu-id="68058-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="68058-114">黏性工作階段中，請確定所有用戶端的後續要求，請移至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="68058-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="68058-115">例如，Azure Web 應用程式會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR)，所有後續的要求路由至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="68058-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="68058-116">在 web 伺服陣列中的非黏性工作階段需要[分散式快取](distributed.md)若要避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="68058-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="68058-117">對於某些應用程式中，分散式快取可以支援更高版本向外延展比記憶體中快取。</span><span class="sxs-lookup"><span data-stu-id="68058-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="68058-118">使用分散式快取卸載到外部處理序快取記憶體。</span><span class="sxs-lookup"><span data-stu-id="68058-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="68058-119">`IMemoryCache`快取將會收回快取項目，記憶體不足的壓力，除非[快取的優先順序](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority)設定為`CacheItemPriority.NeverRemove`。</span><span class="sxs-lookup"><span data-stu-id="68058-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="68058-120">您可以設定`CacheItemPriority`調整與快取收回記憶體不足壓力下的項目優先順序。</span><span class="sxs-lookup"><span data-stu-id="68058-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="68058-121">記憶體中快取可以儲存任何物件;分散式快取介面僅限於`byte[]`。</span><span class="sxs-lookup"><span data-stu-id="68058-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="68058-122">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="68058-122">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="68058-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)) 適用於：</span><span class="sxs-lookup"><span data-stu-id="68058-123"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="68058-124">.NET standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="68058-124">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="68058-125">任何[.NET 實作](/dotnet/standard/net-standard#net-implementation-support)為目標的.NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="68058-125">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="68058-126">例如，ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="68058-126">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="68058-127">.NET framework 4.5 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="68058-127">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="68058-128">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` （如本主題所述） 建議`System.Runtime.Caching` / `MemoryCache`因為深入整合到 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="68058-128">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this topic) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="68058-129">例如，`IMemoryCache`適用於原生 ASP.NET Core[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="68058-129">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="68058-130">使用`System.Runtime.Caching` / `MemoryCache`橋樑相容性移植程式碼，從 ASP.NET 4.x 到 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="68058-130">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="68058-131">快取指引</span><span class="sxs-lookup"><span data-stu-id="68058-131">Cache guidelines</span></span>

* <span data-ttu-id="68058-132">程式碼，應一律具有擷取資料的後援選項及**不**取決於快取的值，可供使用。</span><span class="sxs-lookup"><span data-stu-id="68058-132">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="68058-133">快取佔用很少的資源時，記憶體。</span><span class="sxs-lookup"><span data-stu-id="68058-133">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="68058-134">限制快取增長：</span><span class="sxs-lookup"><span data-stu-id="68058-134">Limit cache growth:</span></span>
  * <span data-ttu-id="68058-135">請勿**不**外部輸入做為快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="68058-135">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="68058-136">您可以使用到期時間，限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="68058-136">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="68058-137">使用 SetSize、 大小和 SizeLimit 限制快取大小</span><span class="sxs-lookup"><span data-stu-id="68058-137">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="68058-138">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="68058-138">Using IMemoryCache</span></span>

<span data-ttu-id="68058-139">記憶體中快取*服務*從您的應用程式使用參考[相依性插入](../../fundamentals/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="68058-139">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="68058-140">呼叫`AddMemoryCache`在`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="68058-140">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="68058-141">要求`IMemoryCache`建構函式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="68058-141">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="68058-142">`IMemoryCache` 需要 NuGet 套件[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。</span><span class="sxs-lookup"><span data-stu-id="68058-142">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="68058-143">`IMemoryCache` 需要 NuGet 套件[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)，這是用於[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="68058-143">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="68058-144">`IMemoryCache` 需要 NuGet 套件[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)，這是用於[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="68058-144">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="68058-145">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查一次是否快取中。</span><span class="sxs-lookup"><span data-stu-id="68058-145">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="68058-146">如果不快取的時間，建立並使用快取中加入新項目[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)。</span><span class="sxs-lookup"><span data-stu-id="68058-146">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="68058-147">會顯示目前的時間和快取的時間：</span><span class="sxs-lookup"><span data-stu-id="68058-147">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="68058-148">快取`DateTime`內逾時期限 （和任何因記憶體壓力而收回） 要求時的值會保留在快取。</span><span class="sxs-lookup"><span data-stu-id="68058-148">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="68058-149">下圖顯示目前的時間和較舊的時間，從快取擷取：</span><span class="sxs-lookup"><span data-stu-id="68058-149">The following image shows the current time and an older time retrieved from the cache:</span></span>

![索引檢視，其中顯示兩個不同的時間](memory/_static/time.png)

<span data-ttu-id="68058-151">下列程式碼會使用[GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)並[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)快取資料。</span><span class="sxs-lookup"><span data-stu-id="68058-151">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="68058-152">下列程式碼會呼叫[取得](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)擷取快取的時間：</span><span class="sxs-lookup"><span data-stu-id="68058-152">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="68058-153">請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)並[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)如需快取方法的描述。</span><span class="sxs-lookup"><span data-stu-id="68058-153">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="68058-154">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="68058-154">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="68058-155">下列範例：</span><span class="sxs-lookup"><span data-stu-id="68058-155">The following sample:</span></span>

- <span data-ttu-id="68058-156">設定絕對到期時間。</span><span class="sxs-lookup"><span data-stu-id="68058-156">Sets the absolute expiration time.</span></span> <span data-ttu-id="68058-157">這是最長的時間可以快取項目，並防止項目時，會持續更新滑動期限變得太過時。</span><span class="sxs-lookup"><span data-stu-id="68058-157">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="68058-158">設定滑動期限。</span><span class="sxs-lookup"><span data-stu-id="68058-158">Sets a sliding expiration time.</span></span> <span data-ttu-id="68058-159">要求存取此快取的項目會重設滑動的到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="68058-159">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="68058-160">若要設定快取優先權`CacheItemPriority.NeverRemove`。</span><span class="sxs-lookup"><span data-stu-id="68058-160">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="68058-161">設定組[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) ，之後就會呼叫從快取收回項目的。</span><span class="sxs-lookup"><span data-stu-id="68058-161">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="68058-162">從快取中移除的項目程式碼在不同執行緒上執行的回呼。</span><span class="sxs-lookup"><span data-stu-id="68058-162">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="68058-163">使用 SetSize、 大小和 SizeLimit 限制快取大小</span><span class="sxs-lookup"><span data-stu-id="68058-163">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="68058-164">A`MemoryCache`執行個體可能會選擇性地指定並強制執行大小限制。</span><span class="sxs-lookup"><span data-stu-id="68058-164">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="68058-165">因為快取有沒有機制可測量的項目大小，則記憶體大小限制並沒有定義的測量單位。</span><span class="sxs-lookup"><span data-stu-id="68058-165">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="68058-166">如果設定的快取記憶體大小限制，所有項目必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="68058-166">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="68058-167">指定的大小是以開發人員選擇的單位。</span><span class="sxs-lookup"><span data-stu-id="68058-167">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="68058-168">例如: </span><span class="sxs-lookup"><span data-stu-id="68058-168">For example:</span></span>

* <span data-ttu-id="68058-169">如果 web 應用程式的主要快取字串，每個快取項目大小可能是字串的長度。</span><span class="sxs-lookup"><span data-stu-id="68058-169">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="68058-170">應用程式可以指定成 1 的所有項目的大小和大小上限是項目計數。</span><span class="sxs-lookup"><span data-stu-id="68058-170">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="68058-171">下列程式碼建立 unitless 固定大小[MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1)供[相依性插入](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="68058-171">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="68058-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit)沒有單位。</span><span class="sxs-lookup"><span data-stu-id="68058-172">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="68058-173">快取項目必須以指定其認定執行快取記憶體大小已設定的最適合任何單位。</span><span class="sxs-lookup"><span data-stu-id="68058-173">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="68058-174">快取執行個體的所有使用者都應該都使用相同單位的系統。</span><span class="sxs-lookup"><span data-stu-id="68058-174">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="68058-175">將快取項目，如果快取項目大小的總和超過所指定的值`SizeLimit`。</span><span class="sxs-lookup"><span data-stu-id="68058-175">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="68058-176">如果沒有快取大小上限設定，將會忽略項目上設定的快取大小。</span><span class="sxs-lookup"><span data-stu-id="68058-176">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="68058-177">下列程式碼的暫存器`MyMemoryCache`具有[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="68058-177">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="68058-178">`MyMemoryCache` 會建立為獨立的記憶體內部快取，了解此大小限制快取，並了解如何適當地設定快取項目大小的元件。</span><span class="sxs-lookup"><span data-stu-id="68058-178">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="68058-179">下列程式碼會使用`MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="68058-179">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="68058-180">可以設定快取項目的大小[大小](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size)或[SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)擴充方法：</span><span class="sxs-lookup"><span data-stu-id="68058-180">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="68058-181">快取相依性</span><span class="sxs-lookup"><span data-stu-id="68058-181">Cache dependencies</span></span>

<span data-ttu-id="68058-182">下列範例示範如何過期的快取項目，如果相依項目到期。</span><span class="sxs-lookup"><span data-stu-id="68058-182">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="68058-183">A`CancellationChangeToken`新增至快取的項目。</span><span class="sxs-lookup"><span data-stu-id="68058-183">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="68058-184">當`Cancel`上呼叫`CancellationTokenSource`，收回這兩個快取項目。</span><span class="sxs-lookup"><span data-stu-id="68058-184">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="68058-185">使用`CancellationTokenSource`可讓多個快取項目被收回做為群組。</span><span class="sxs-lookup"><span data-stu-id="68058-185">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="68058-186">具有`using`上述程式碼中的模式，在建立快取項目`using`區塊會在觸發程序和到期日設定繼承。</span><span class="sxs-lookup"><span data-stu-id="68058-186">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="68058-187">其他備註</span><span class="sxs-lookup"><span data-stu-id="68058-187">Additional notes</span></span>

- <span data-ttu-id="68058-188">當使用回呼，以便重新填入快取項目：</span><span class="sxs-lookup"><span data-stu-id="68058-188">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="68058-189">多個要求可以找到快取索引鍵的值空白因為尚未完成的回呼。</span><span class="sxs-lookup"><span data-stu-id="68058-189">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="68058-190">這會導致數個執行緒重新填入快取的項目。</span><span class="sxs-lookup"><span data-stu-id="68058-190">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="68058-191">以建立另一個使用一個快取項目時，父項目的到期的權杖和以時間為基礎的到期日設定，也會複製子系。</span><span class="sxs-lookup"><span data-stu-id="68058-191">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="68058-192">手動移除已到期或更新的父項目，則不是子系。</span><span class="sxs-lookup"><span data-stu-id="68058-192">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="68058-193">使用[PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks)設定從快取收回快取項目之後會引發的回呼。</span><span class="sxs-lookup"><span data-stu-id="68058-193">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="68058-194">其他資源</span><span class="sxs-lookup"><span data-stu-id="68058-194">Additional resources</span></span>

* [<span data-ttu-id="68058-195">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="68058-195">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="68058-196">使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="68058-196">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="68058-197">回應快取</span><span class="sxs-lookup"><span data-stu-id="68058-197">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="68058-198">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="68058-198">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="68058-199">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="68058-199">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="68058-200">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="68058-200">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
