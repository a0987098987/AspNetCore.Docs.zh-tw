---
title: ASP.NET Core 中的記憶體快取
author: rick-anderson
description: 了解如何快取 ASP.NET Core 中的資料和記憶體。
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/caching/memory
ms.openlocfilehash: 8eec361efbc3c7dca6c0bef65b6f6b40b3b46798
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404609"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="e6002-103">ASP.NET Core 中的記憶體快取</span><span class="sxs-lookup"><span data-stu-id="e6002-103">Cache in-memory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e6002-104">作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e6002-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e6002-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="e6002-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="e6002-106">快取基本概念</span><span class="sxs-lookup"><span data-stu-id="e6002-106">Caching basics</span></span>

<span data-ttu-id="e6002-107">快取可以藉由減少產生內容所需的工作，大幅改善應用程式的效能和擴充性。</span><span class="sxs-lookup"><span data-stu-id="e6002-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="e6002-108">快取最適用于不常變更**且**產生成本昂貴的資料。</span><span class="sxs-lookup"><span data-stu-id="e6002-108">Caching works best with data that changes infrequently **and** is expensive to generate.</span></span> <span data-ttu-id="e6002-109">快取會製作一份資料複本，其傳回的速度會比從來源快得多。</span><span class="sxs-lookup"><span data-stu-id="e6002-109">Caching makes a copy of data that can be returned much faster than from the source.</span></span> <span data-ttu-id="e6002-110">應用程式應撰寫並測試為**永遠不會**依賴快取的資料。</span><span class="sxs-lookup"><span data-stu-id="e6002-110">Apps should be written and tested to **never** depend on cached data.</span></span>

<span data-ttu-id="e6002-111">ASP.NET Core 支援數種不同的快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="e6002-112">最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎。</span><span class="sxs-lookup"><span data-stu-id="e6002-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="e6002-113">`IMemoryCache`代表儲存在 web 伺服器記憶體中的快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-113">`IMemoryCache` represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="e6002-114">使用記憶體內部快取時，在伺服器陣列（多部伺服器）上執行的應用程式應該確保會話是固定的。</span><span class="sxs-lookup"><span data-stu-id="e6002-114">Apps running on a server farm (multiple servers) should ensure sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="e6002-115">[粘滯會話] 可確保來自用戶端的後續要求都會移至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6002-115">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="e6002-116">例如，Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)（ARR），將所有後續要求路由傳送至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6002-116">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="e6002-117">Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取，以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="e6002-117">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="e6002-118">針對某些應用程式，分散式快取可支援比記憶體內部快取更高的相應放大。</span><span class="sxs-lookup"><span data-stu-id="e6002-118">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="e6002-119">使用分散式快取會將快取記憶體卸載至外部進程。</span><span class="sxs-lookup"><span data-stu-id="e6002-119">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="e6002-120">記憶體內部快取可以儲存任何物件。</span><span class="sxs-lookup"><span data-stu-id="e6002-120">The in-memory cache can store any object.</span></span> <span data-ttu-id="e6002-121">分散式快取介面僅限於 `byte[]` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-121">The distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="e6002-122">記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="e6002-123">System.web. Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="e6002-123">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="e6002-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>（[NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)）可與搭配使用：</span><span class="sxs-lookup"><span data-stu-id="e6002-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="e6002-125">.NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6002-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="e6002-126">以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。</span><span class="sxs-lookup"><span data-stu-id="e6002-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="e6002-127">例如，ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6002-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e6002-128">.NET Framework 4.5 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6002-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="e6002-129">建議使用[Microsoft Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` （如本文所述）， `System.Runtime.Caching` / `MemoryCache` 因為它已更緊密整合到 ASP.NET Core 中。</span><span class="sxs-lookup"><span data-stu-id="e6002-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this article) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="e6002-130">例如，會 `IMemoryCache` 以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="e6002-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="e6002-131">將程式 `System.Runtime.Caching` / `MemoryCache` 代碼從 ASP.NET 4.x 移植到 ASP.NET Core 時，請使用做為相容性橋接器。</span><span class="sxs-lookup"><span data-stu-id="e6002-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="e6002-132">快取指導方針</span><span class="sxs-lookup"><span data-stu-id="e6002-132">Cache guidelines</span></span>

* <span data-ttu-id="e6002-133">程式碼應該一律具有 [回溯] 選項來提取資料，而**不**是取決於可用的快取值。</span><span class="sxs-lookup"><span data-stu-id="e6002-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="e6002-134">快取使用不足的資源，記憶體。</span><span class="sxs-lookup"><span data-stu-id="e6002-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="e6002-135">限制快取成長：</span><span class="sxs-lookup"><span data-stu-id="e6002-135">Limit cache growth:</span></span>
  * <span data-ttu-id="e6002-136">請勿**使用外部輸入做為**快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e6002-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="e6002-137">使用到期時間來限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="e6002-137">Use expirations to limit cache growth.</span></span>
  * <span data-ttu-id="e6002-138">[使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-138">[Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span> <span data-ttu-id="e6002-139">ASP.NET Core 執行時間不會根據記憶體壓力**來限制快**取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-139">The ASP.NET Core runtime does **not** limit cache size based on memory pressure.</span></span> <span data-ttu-id="e6002-140">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-140">It's up to the developer to limit cache size.</span></span>

## <a name="use-imemorycache"></a><span data-ttu-id="e6002-141">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="e6002-141">Use IMemoryCache</span></span>

> [!WARNING]
> <span data-ttu-id="e6002-142">從相依性[插入](xref:fundamentals/dependency-injection)使用*共用*記憶體快取，並呼叫 `SetSize` 、 `Size` 或 `SizeLimit` 來限制快取大小，可能會導致應用程式失敗。</span><span class="sxs-lookup"><span data-stu-id="e6002-142">Using a *shared* memory cache from [Dependency Injection](xref:fundamentals/dependency-injection) and calling `SetSize`, `Size`, or `SizeLimit` to limit cache size can cause the app to fail.</span></span> <span data-ttu-id="e6002-143">在快取上設定大小限制時，所有專案在新增時都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-143">When a size limit is set on a cache, all entries must specify a size when being added.</span></span> <span data-ttu-id="e6002-144">這可能會導致問題，因為開發人員可能無法完全控制使用共用快取的內容。</span><span class="sxs-lookup"><span data-stu-id="e6002-144">This can lead to issues since developers may not have full control on what uses the shared cache.</span></span> <span data-ttu-id="e6002-145">例如，Entity Framework Core 使用共用快取，且未指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-145">For example, Entity Framework Core uses the shared cache and does not specify a size.</span></span> <span data-ttu-id="e6002-146">如果應用程式設定快取大小限制，並使用 EF Core，應用程式會擲回 `InvalidOperationException` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-146">If an app sets a cache size limit and uses EF Core, the app throws an `InvalidOperationException`.</span></span>
> <span data-ttu-id="e6002-147">使用 `SetSize` 、 `Size` 或來限制快取時，請建立單一快取 `SizeLimit` 來進行快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-147">When using `SetSize`, `Size`, or `SizeLimit` to limit cache, create a cache singleton for caching.</span></span> <span data-ttu-id="e6002-148">如需詳細資訊和範例，請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-148">For more information and an example, see [Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span>
> <span data-ttu-id="e6002-149">共用快取是由其他架構或程式庫共用。</span><span class="sxs-lookup"><span data-stu-id="e6002-149">A shared cache is one shared by other frameworks or libraries.</span></span> <span data-ttu-id="e6002-150">例如，EF Core 使用共用快取，且未指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-150">For example, EF Core uses the shared cache and does not specify a size.</span></span> 

<span data-ttu-id="e6002-151">記憶體內部快取是使用相依性[插入](xref:fundamentals/dependency-injection)從應用程式參考的*服務*。</span><span class="sxs-lookup"><span data-stu-id="e6002-151">In-memory caching is a *service* that's referenced from an app using [Dependency Injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e6002-152">`IMemoryCache`在此函數中要求實例：</span><span class="sxs-lookup"><span data-stu-id="e6002-152">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

<span data-ttu-id="e6002-153">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。</span><span class="sxs-lookup"><span data-stu-id="e6002-153">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="e6002-154">如果沒有快取時間，則會建立新的專案，並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。</span><span class="sxs-lookup"><span data-stu-id="e6002-154">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span> <span data-ttu-id="e6002-155">`CacheKeys`類別是下載範例的一部分。</span><span class="sxs-lookup"><span data-stu-id="e6002-155">The `CacheKeys` class is part of the download sample.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="e6002-156">此時會顯示目前的時間和快取時間：</span><span class="sxs-lookup"><span data-stu-id="e6002-156">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

<span data-ttu-id="e6002-157">`DateTime`當超時時間內有要求時，快取的值會保留在快取中。</span><span class="sxs-lookup"><span data-stu-id="e6002-157">The cached `DateTime` value remains in the cache while there are requests within the timeout period.</span></span>

<span data-ttu-id="e6002-158">下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。</span><span class="sxs-lookup"><span data-stu-id="e6002-158">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="e6002-159">下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間：</span><span class="sxs-lookup"><span data-stu-id="e6002-159">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="e6002-160">下列程式碼會取得或建立具有絕對過期的快取專案：</span><span class="sxs-lookup"><span data-stu-id="e6002-160">The following code gets or creates a cached item with absolute expiration:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

<span data-ttu-id="e6002-161">只有具有滑動期限的快取專案集，才會有過時的風險。</span><span class="sxs-lookup"><span data-stu-id="e6002-161">A cached item set with a sliding expiration only is at risk of becoming stale.</span></span> <span data-ttu-id="e6002-162">如果存取的頻率高於滑動到期時間間隔，專案將永遠不會過期。</span><span class="sxs-lookup"><span data-stu-id="e6002-162">If it's accessed more frequently than the sliding expiration interval, the item will never expire.</span></span> <span data-ttu-id="e6002-163">結合滑動期限與絕對到期日，以保證專案在其絕對到期時間通過後到期。</span><span class="sxs-lookup"><span data-stu-id="e6002-163">Combine a sliding expiration with an absolute expiration to guarantee that the item expires once its absolute expiration time passes.</span></span> <span data-ttu-id="e6002-164">絕對到期時間會設定專案可快取的時間上限，但如果在滑動到期時間間隔內未要求，仍然允許專案提早過期。</span><span class="sxs-lookup"><span data-stu-id="e6002-164">The absolute expiration sets an upper bound to how long the item can be cached while still allowing the item to expire earlier if it isn't requested within the sliding expiration interval.</span></span> <span data-ttu-id="e6002-165">當同時指定絕對和滑動期限時，會以邏輯方式 ORed 到期日。</span><span class="sxs-lookup"><span data-stu-id="e6002-165">When both absolute and sliding expiration are specified, the expirations are logically ORed.</span></span> <span data-ttu-id="e6002-166">如果滑動到期間隔*或*絕對到期時間經過，則會從快取中收回專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-166">If either the sliding expiration interval *or* the absolute expiration time pass, the item is evicted from the cache.</span></span>

<span data-ttu-id="e6002-167">下列程式碼會取得或建立具有滑動*和*絕對到期的快取專案：</span><span class="sxs-lookup"><span data-stu-id="e6002-167">The following code gets or creates a cached item with both sliding *and* absolute expiration:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

<span data-ttu-id="e6002-168">上述程式碼可確保資料不會快取超過絕對時間。</span><span class="sxs-lookup"><span data-stu-id="e6002-168">The preceding code guarantees the data will not be cached longer than the absolute time.</span></span>

<span data-ttu-id="e6002-169"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> 和 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> 是類別中的擴充方法 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions> 。</span><span class="sxs-lookup"><span data-stu-id="e6002-169"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> are extension methods in the <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions> class.</span></span> <span data-ttu-id="e6002-170">這些方法會擴充的功能 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 。</span><span class="sxs-lookup"><span data-stu-id="e6002-170">These methods extend the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="e6002-171">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="e6002-171">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="e6002-172">下列範例：</span><span class="sxs-lookup"><span data-stu-id="e6002-172">The following sample:</span></span>

* <span data-ttu-id="e6002-173">設定滑動到期時間。</span><span class="sxs-lookup"><span data-stu-id="e6002-173">Sets a sliding expiration time.</span></span> <span data-ttu-id="e6002-174">存取此快取專案的要求將會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="e6002-174">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="e6002-175">將快取優先順序設定為[CacheItemPriority. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove)。</span><span class="sxs-lookup"><span data-stu-id="e6002-175">Sets the cache priority to [CacheItemPriority.NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove).</span></span>
* <span data-ttu-id="e6002-176">設定在從快取中收回專案之後，將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。</span><span class="sxs-lookup"><span data-stu-id="e6002-176">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="e6002-177">回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="e6002-177">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="e6002-178">使用 SetSize、Size 和 SizeLimit 來限制快取大小</span><span class="sxs-lookup"><span data-stu-id="e6002-178">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="e6002-179">`MemoryCache`實例可以選擇性地指定並強制執行大小限制。</span><span class="sxs-lookup"><span data-stu-id="e6002-179">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="e6002-180">快取大小限制沒有定義的測量單位，因為快取沒有測量專案大小的機制。</span><span class="sxs-lookup"><span data-stu-id="e6002-180">The cache size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="e6002-181">如果已設定快取大小限制，所有專案都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-181">If the cache size limit is set, all entries must specify size.</span></span> <span data-ttu-id="e6002-182">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-182">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="e6002-183">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-183">It's up to the developer to limit cache size.</span></span> <span data-ttu-id="e6002-184">指定的大小是開發人員選擇的單位。</span><span class="sxs-lookup"><span data-stu-id="e6002-184">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="e6002-185">例如：</span><span class="sxs-lookup"><span data-stu-id="e6002-185">For example:</span></span>

* <span data-ttu-id="e6002-186">如果 web 應用程式主要是快取字串，則每個快取專案大小都可以是字串長度。</span><span class="sxs-lookup"><span data-stu-id="e6002-186">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="e6002-187">應用程式可以將所有專案的大小指定為1，而大小限制是專案的計數。</span><span class="sxs-lookup"><span data-stu-id="e6002-187">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="e6002-188">如果 <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> 未設定，快取會成長而不受限制。</span><span class="sxs-lookup"><span data-stu-id="e6002-188">If <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> isn't set, the cache grows without bound.</span></span> <span data-ttu-id="e6002-189">當系統記憶體不足時，ASP.NET Core 執行時間不會修剪快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-189">The ASP.NET Core runtime doesn't trim the cache when system memory is low.</span></span> <span data-ttu-id="e6002-190">應用程式必須架構為：</span><span class="sxs-lookup"><span data-stu-id="e6002-190">Apps must be architected to:</span></span>

* <span data-ttu-id="e6002-191">限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="e6002-191">Limit cache growth.</span></span>
* <span data-ttu-id="e6002-192"><xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> 當可用的記憶體受到限制時，呼叫或：</span><span class="sxs-lookup"><span data-stu-id="e6002-192">Call <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> or <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> when available memory is limited:</span></span>

<span data-ttu-id="e6002-193">下列程式碼會建立可透過相依 <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> 性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小：</span><span class="sxs-lookup"><span data-stu-id="e6002-193">The following code creates a unitless fixed size <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="e6002-194">`SizeLimit`沒有單位。</span><span class="sxs-lookup"><span data-stu-id="e6002-194">`SizeLimit` does not have units.</span></span> <span data-ttu-id="e6002-195">如果已設定快取大小限制，則快取的專案必須以其認為最適合的任何單位來指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-195">Cached entries must specify size in whatever units they deem most appropriate if the cache size limit has been set.</span></span> <span data-ttu-id="e6002-196">快取實例的所有使用者都應使用相同的單位系統。</span><span class="sxs-lookup"><span data-stu-id="e6002-196">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="e6002-197">如果快取的專案大小總和超過所指定的值，則不會快取專案 `SizeLimit` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-197">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="e6002-198">如果未設定快取大小限制，則會忽略在該專案上設定的快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-198">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="e6002-199">下列程式碼會 `MyMemoryCache` 向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e6002-199">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

<span data-ttu-id="e6002-200">`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取，並知道如何適當地設定快取專案大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-200">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="e6002-201">下列程式碼會使用 `MyMemoryCache` ：</span><span class="sxs-lookup"><span data-stu-id="e6002-201">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

<span data-ttu-id="e6002-202">快取專案的大小可以透過 <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> 或擴充方法來設定 <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*> ：</span><span class="sxs-lookup"><span data-stu-id="e6002-202">The size of the cache entry can be set by <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> or the <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*> extension methods:</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a><span data-ttu-id="e6002-203">MemoryCache Compact</span><span class="sxs-lookup"><span data-stu-id="e6002-203">MemoryCache.Compact</span></span>

<span data-ttu-id="e6002-204">`MemoryCache.Compact`嘗試依下列順序移除指定百分比的快取：</span><span class="sxs-lookup"><span data-stu-id="e6002-204">`MemoryCache.Compact` attempts to remove the specified percentage of the cache in the following order:</span></span>

* <span data-ttu-id="e6002-205">所有過期的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-205">All expired items.</span></span>
* <span data-ttu-id="e6002-206">依優先順序排序的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-206">Items by priority.</span></span> <span data-ttu-id="e6002-207">系統會先移除最低優先順序專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-207">Lowest priority items are removed first.</span></span>
* <span data-ttu-id="e6002-208">最近最少使用的物件。</span><span class="sxs-lookup"><span data-stu-id="e6002-208">Least recently used objects.</span></span>
* <span data-ttu-id="e6002-209">具有最早絕對到期的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-209">Items with the earliest absolute expiration.</span></span>
* <span data-ttu-id="e6002-210">具有最早滑動到期日的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-210">Items with the earliest sliding expiration.</span></span>

<span data-ttu-id="e6002-211">具有優先順序的固定專案永遠不會 <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> 被移除。</span><span class="sxs-lookup"><span data-stu-id="e6002-211">Pinned items with priority <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> are never removed.</span></span> <span data-ttu-id="e6002-212">下列程式碼會移除快取專案並呼叫 `Compact` ：</span><span class="sxs-lookup"><span data-stu-id="e6002-212">The following code removes a cache item and calls `Compact`:</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

<span data-ttu-id="e6002-213">如需詳細資訊，請參閱[GitHub 上的 Compact 來源](https://github.com/dotnet/extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393)。</span><span class="sxs-lookup"><span data-stu-id="e6002-213">See [Compact source on GitHub](https://github.com/dotnet/extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) for more information.</span></span>

## <a name="cache-dependencies"></a><span data-ttu-id="e6002-214">快取相依性</span><span class="sxs-lookup"><span data-stu-id="e6002-214">Cache dependencies</span></span>

<span data-ttu-id="e6002-215">下列範例示範如果相依專案過期，如何讓快取專案過期。</span><span class="sxs-lookup"><span data-stu-id="e6002-215">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="e6002-216"><xref:Microsoft.Extensions.Primitives.CancellationChangeToken>會新增至快取的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-216">A <xref:Microsoft.Extensions.Primitives.CancellationChangeToken> is added to the cached item.</span></span> <span data-ttu-id="e6002-217">`Cancel`在上呼叫時 `CancellationTokenSource` ，會收回這兩個快取專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-217">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="e6002-218">使用 <xref:System.Threading.CancellationTokenSource> 可讓多個快取專案收回為群組。</span><span class="sxs-lookup"><span data-stu-id="e6002-218">Using a <xref:System.Threading.CancellationTokenSource> allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="e6002-219">使用 `using` 上述程式碼中的模式時，在區塊內建立的快取專案 `using` 將會繼承觸發程式和到期設定。</span><span class="sxs-lookup"><span data-stu-id="e6002-219">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="e6002-220">其他注意事項</span><span class="sxs-lookup"><span data-stu-id="e6002-220">Additional notes</span></span>

* <span data-ttu-id="e6002-221">到期不會在背景中發生。</span><span class="sxs-lookup"><span data-stu-id="e6002-221">Expiration doesn't happen in the background.</span></span> <span data-ttu-id="e6002-222">沒有任何計時器會主動掃描快取中是否有過期的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-222">There is no timer that actively scans the cache for expired items.</span></span> <span data-ttu-id="e6002-223">快取上的任何活動（ `Get` 、 `Set` 、 `Remove` ）都可以觸發過期專案的背景掃描。</span><span class="sxs-lookup"><span data-stu-id="e6002-223">Any activity on the cache (`Get`, `Set`, `Remove`) can trigger a background scan for expired items.</span></span> <span data-ttu-id="e6002-224">（）上的 `CancellationTokenSource` 計時器 <xref:System.Threading.CancellationTokenSource.CancelAfter*> 也會移除專案，並觸發已過期專案的掃描。</span><span class="sxs-lookup"><span data-stu-id="e6002-224">A timer on the `CancellationTokenSource` (<xref:System.Threading.CancellationTokenSource.CancelAfter*>) also removes the entry and trigger a scan for expired items.</span></span> <span data-ttu-id="e6002-225">下列範例會使用[CancellationTokenSource （TimeSpan）](/dotnet/api/system.threading.cancellationtokensource.-ctor)作為已註冊的權杖。</span><span class="sxs-lookup"><span data-stu-id="e6002-225">The following example uses [CancellationTokenSource(TimeSpan)](/dotnet/api/system.threading.cancellationtokensource.-ctor) for the registered token.</span></span> <span data-ttu-id="e6002-226">當此標記引發時，它會立即移除該專案，並引發收回回呼：</span><span class="sxs-lookup"><span data-stu-id="e6002-226">When this token fires it removes the entry immediately and fires the eviction callbacks:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ae)]

* <span data-ttu-id="e6002-227">使用回呼來重新擴展快取專案時：</span><span class="sxs-lookup"><span data-stu-id="e6002-227">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="e6002-228">因為回呼尚未完成，所以多個要求可以將快取的金鑰值空白。</span><span class="sxs-lookup"><span data-stu-id="e6002-228">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="e6002-229">這可能會導致數個執行緒重新填入快取的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-229">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="e6002-230">當有一個快取專案用來建立另一個時，子系會複製父系專案的到期權杖和以時間為基礎的到期設定。</span><span class="sxs-lookup"><span data-stu-id="e6002-230">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="e6002-231">子系不會因為手動移除或更新父專案而過期。</span><span class="sxs-lookup"><span data-stu-id="e6002-231">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="e6002-232">用 <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks> 來設定將在從快取中收回快取專案之後引發的回呼。</span><span class="sxs-lookup"><span data-stu-id="e6002-232">Use <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks> to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>
* <span data-ttu-id="e6002-233">大部分的應用程式 `IMemoryCache` 都已啟用。</span><span class="sxs-lookup"><span data-stu-id="e6002-233">For most apps, `IMemoryCache` is enabled.</span></span> <span data-ttu-id="e6002-234">例如， `AddMvc` 在中呼叫、、 `AddControllersWithViews` `AddRazorPages` 、 `AddMvcCore().AddRazorViewEngine` 和許多其他 `Add{Service}` 方法，會 `ConfigureServices` 啟用 `IMemoryCache` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-234">For example, calling `AddMvc`, `AddControllersWithViews`, `AddRazorPages`, `AddMvcCore().AddRazorViewEngine`, and many other `Add{Service}` methods in `ConfigureServices`, enables `IMemoryCache`.</span></span> <span data-ttu-id="e6002-235">對於未呼叫上述其中一種方法的應用程式 `Add{Service}` ，可能需要 <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> 在中呼叫 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-235">For apps that are not calling one of the preceding `Add{Service}` methods, it may be necessary to call <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> in `ConfigureServices`.</span></span>

## <a name="background-cache-update"></a><span data-ttu-id="e6002-236">背景快取更新</span><span class="sxs-lookup"><span data-stu-id="e6002-236">Background cache update</span></span>

<span data-ttu-id="e6002-237">使用[背景服務](xref:fundamentals/host/hosted-services)（例如） <xref:Microsoft.Extensions.Hosting.IHostedService> 來更新快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-237">Use a [background service](xref:fundamentals/host/hosted-services) such as <xref:Microsoft.Extensions.Hosting.IHostedService> to update the cache.</span></span> <span data-ttu-id="e6002-238">背景服務可以重新計算專案，然後只有在準備就緒時，才將它們指派給快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-238">The background service can recompute the entries and then assign them to the cache only when they’re ready.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6002-239">其他資源</span><span class="sxs-lookup"><span data-stu-id="e6002-239">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<!-- This is the 2.1 version -->
<span data-ttu-id="e6002-240">作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e6002-240">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e6002-241">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="e6002-241">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="e6002-242">快取基本概念</span><span class="sxs-lookup"><span data-stu-id="e6002-242">Caching basics</span></span>

<span data-ttu-id="e6002-243">快取可以藉由減少產生內容所需的工作，大幅改善應用程式的效能和擴充性。</span><span class="sxs-lookup"><span data-stu-id="e6002-243">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="e6002-244">快取最適合不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="e6002-244">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="e6002-245">快取會製作一份資料複本，其傳回的速度會比原始來源更快。</span><span class="sxs-lookup"><span data-stu-id="e6002-245">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="e6002-246">撰寫和測試程式碼時，**絕對不**會依賴快取的資料。</span><span class="sxs-lookup"><span data-stu-id="e6002-246">Code should be written and tested to **never** depend on cached data.</span></span>

<span data-ttu-id="e6002-247">ASP.NET Core 支援數種不同的快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-247">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="e6002-248">最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎，代表儲存在 web 伺服器記憶體中的快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-248">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="e6002-249">在伺服器陣列（多部伺服器）上執行的應用程式應該在使用記憶體內部快取時，確保會話是粘滯的。</span><span class="sxs-lookup"><span data-stu-id="e6002-249">Apps that run on a server farm (multiple servers) should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="e6002-250">[粘滯會話] 可確保之後從用戶端提出的要求全都會移至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6002-250">Sticky sessions ensure that later requests from a client all go to the same server.</span></span> <span data-ttu-id="e6002-251">例如，Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)（ARR），將來自使用者代理程式的所有要求路由傳送至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="e6002-251">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all requests from a user agent to the same server.</span></span>

<span data-ttu-id="e6002-252">Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取，以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="e6002-252">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="e6002-253">針對某些應用程式，分散式快取可支援比記憶體內部快取更高的相應放大。</span><span class="sxs-lookup"><span data-stu-id="e6002-253">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="e6002-254">使用分散式快取會將快取記憶體卸載至外部進程。</span><span class="sxs-lookup"><span data-stu-id="e6002-254">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="e6002-255">記憶體內部快取可以儲存任何物件。</span><span class="sxs-lookup"><span data-stu-id="e6002-255">The in-memory cache can store any object.</span></span> <span data-ttu-id="e6002-256">分散式快取介面僅限於 `byte[]` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-256">The distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="e6002-257">記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-257">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="e6002-258">System.web. Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="e6002-258">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="e6002-259"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>（[NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)）可與搭配使用：</span><span class="sxs-lookup"><span data-stu-id="e6002-259"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="e6002-260">.NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6002-260">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="e6002-261">以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。</span><span class="sxs-lookup"><span data-stu-id="e6002-261">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="e6002-262">例如，ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6002-262">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="e6002-263">.NET Framework 4.5 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="e6002-263">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="e6002-264">建議使用[Microsoft Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` （如本文所述）， `System.Runtime.Caching` / `MemoryCache` 因為它已更緊密整合到 ASP.NET Core 中。</span><span class="sxs-lookup"><span data-stu-id="e6002-264">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this article) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="e6002-265">例如，會 `IMemoryCache` 以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="e6002-265">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="e6002-266">將程式 `System.Runtime.Caching` / `MemoryCache` 代碼從 ASP.NET 4.x 移植到 ASP.NET Core 時，請使用做為相容性橋接器。</span><span class="sxs-lookup"><span data-stu-id="e6002-266">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="e6002-267">快取指導方針</span><span class="sxs-lookup"><span data-stu-id="e6002-267">Cache guidelines</span></span>

* <span data-ttu-id="e6002-268">程式碼應該一律具有 [回溯] 選項來提取資料，而**不**是取決於可用的快取值。</span><span class="sxs-lookup"><span data-stu-id="e6002-268">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="e6002-269">快取使用不足的資源，記憶體。</span><span class="sxs-lookup"><span data-stu-id="e6002-269">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="e6002-270">限制快取成長：</span><span class="sxs-lookup"><span data-stu-id="e6002-270">Limit cache growth:</span></span>
  * <span data-ttu-id="e6002-271">請勿**使用外部輸入做為**快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e6002-271">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="e6002-272">使用到期時間來限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="e6002-272">Use expirations to limit cache growth.</span></span>
  * <span data-ttu-id="e6002-273">[使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-273">[Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span> <span data-ttu-id="e6002-274">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-274">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="e6002-275">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-275">It's up to the developer to limit cache size.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="e6002-276">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="e6002-276">Using IMemoryCache</span></span>

> [!WARNING]
> <span data-ttu-id="e6002-277">從相依性[插入](xref:fundamentals/dependency-injection)使用*共用*記憶體快取，並呼叫 `SetSize` 、 `Size` 或 `SizeLimit` 來限制快取大小，可能會導致應用程式失敗。</span><span class="sxs-lookup"><span data-stu-id="e6002-277">Using a *shared* memory cache from [Dependency Injection](xref:fundamentals/dependency-injection) and calling `SetSize`, `Size`, or `SizeLimit` to limit cache size can cause the app to fail.</span></span> <span data-ttu-id="e6002-278">在快取上設定大小限制時，所有專案在新增時都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-278">When a size limit is set on a cache, all entries must specify a size when being added.</span></span> <span data-ttu-id="e6002-279">這可能會導致問題，因為開發人員可能無法完全控制使用共用快取的內容。</span><span class="sxs-lookup"><span data-stu-id="e6002-279">This can lead to issues since developers may not have full control on what uses the shared cache.</span></span> <span data-ttu-id="e6002-280">例如，Entity Framework Core 使用共用快取，且未指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-280">For example, Entity Framework Core uses the shared cache and does not specify a size.</span></span> <span data-ttu-id="e6002-281">如果應用程式設定快取大小限制，並使用 EF Core，應用程式會擲回 `InvalidOperationException` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-281">If an app sets a cache size limit and uses EF Core, the app throws an `InvalidOperationException`.</span></span>
> <span data-ttu-id="e6002-282">使用 `SetSize` 、 `Size` 或來限制快取時，請建立單一快取 `SizeLimit` 來進行快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-282">When using `SetSize`, `Size`, or `SizeLimit` to limit cache, create a cache singleton for caching.</span></span> <span data-ttu-id="e6002-283">如需詳細資訊和範例，請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-283">For more information and an example, see [Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span>

<span data-ttu-id="e6002-284">記憶體內部快取是使用相依性插入從您的應用程式參考的一[項](../../fundamentals/dependency-injection.md)*服務*。</span><span class="sxs-lookup"><span data-stu-id="e6002-284">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="e6002-285">`AddMemoryCache`在中呼叫 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="e6002-285">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="e6002-286">`IMemoryCache`在此函數中要求實例：</span><span class="sxs-lookup"><span data-stu-id="e6002-286">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

<span data-ttu-id="e6002-287">`IMemoryCache`需要 NuGet 套件 AspNetCore。您可以在[中繼套件](xref:fundamentals/metapackage-app)中找到該[儲存體](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。</span><span class="sxs-lookup"><span data-stu-id="e6002-287">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="e6002-288">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。</span><span class="sxs-lookup"><span data-stu-id="e6002-288">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="e6002-289">如果沒有快取時間，則會建立新的專案，並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。</span><span class="sxs-lookup"><span data-stu-id="e6002-289">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="e6002-290">此時會顯示目前的時間和快取時間：</span><span class="sxs-lookup"><span data-stu-id="e6002-290">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="e6002-291">`DateTime`當超時時間內有要求時，快取的值會保留在快取中。</span><span class="sxs-lookup"><span data-stu-id="e6002-291">The cached `DateTime` value remains in the cache while there are requests within the timeout period.</span></span> <span data-ttu-id="e6002-292">下圖顯示從快取中抓取的目前時間和較舊的時間：</span><span class="sxs-lookup"><span data-stu-id="e6002-292">The following image shows the current time and an older time retrieved from the cache:</span></span>

![顯示兩個不同時間的索引視圖](memory/_static/time.png)

<span data-ttu-id="e6002-294">下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。</span><span class="sxs-lookup"><span data-stu-id="e6002-294">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="e6002-295">下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間：</span><span class="sxs-lookup"><span data-stu-id="e6002-295">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="e6002-296"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> 和[Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)是[CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)類別的擴充方法部分，可延伸的功能 <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> 。</span><span class="sxs-lookup"><span data-stu-id="e6002-296"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="e6002-297">如需其他快取方法的說明，請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)。</span><span class="sxs-lookup"><span data-stu-id="e6002-297">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="e6002-298">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="e6002-298">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="e6002-299">下列範例：</span><span class="sxs-lookup"><span data-stu-id="e6002-299">The following sample:</span></span>

* <span data-ttu-id="e6002-300">設定滑動到期時間。</span><span class="sxs-lookup"><span data-stu-id="e6002-300">Sets a sliding expiration time.</span></span> <span data-ttu-id="e6002-301">存取此快取專案的要求將會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="e6002-301">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="e6002-302">將快取優先順序設定為 `CacheItemPriority.NeverRemove` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-302">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
* <span data-ttu-id="e6002-303">設定在從快取中收回專案之後，將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。</span><span class="sxs-lookup"><span data-stu-id="e6002-303">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="e6002-304">回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="e6002-304">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="e6002-305">使用 SetSize、Size 和 SizeLimit 來限制快取大小</span><span class="sxs-lookup"><span data-stu-id="e6002-305">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="e6002-306">`MemoryCache`實例可以選擇性地指定並強制執行大小限制。</span><span class="sxs-lookup"><span data-stu-id="e6002-306">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="e6002-307">快取大小限制沒有定義的測量單位，因為快取沒有測量專案大小的機制。</span><span class="sxs-lookup"><span data-stu-id="e6002-307">The cache size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="e6002-308">如果已設定快取大小限制，所有專案都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-308">If the cache size limit is set, all entries must specify size.</span></span> <span data-ttu-id="e6002-309">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-309">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="e6002-310">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-310">It's up to the developer to limit cache size.</span></span> <span data-ttu-id="e6002-311">指定的大小是開發人員選擇的單位。</span><span class="sxs-lookup"><span data-stu-id="e6002-311">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="e6002-312">例如：</span><span class="sxs-lookup"><span data-stu-id="e6002-312">For example:</span></span>

* <span data-ttu-id="e6002-313">如果 web 應用程式主要是快取字串，則每個快取專案大小都可以是字串長度。</span><span class="sxs-lookup"><span data-stu-id="e6002-313">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="e6002-314">應用程式可以將所有專案的大小指定為1，而大小限制是專案的計數。</span><span class="sxs-lookup"><span data-stu-id="e6002-314">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="e6002-315">如果 <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> 未設定，則快取會成長而不受限制。</span><span class="sxs-lookup"><span data-stu-id="e6002-315">If <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> is not set, the cache grows without bound.</span></span> <span data-ttu-id="e6002-316">當系統記憶體不足時，ASP.NET Core 執行時間不會修剪快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-316">The ASP.NET Core runtime does not trim the cache when system memory is low.</span></span> <span data-ttu-id="e6002-317">應用程式的架構大致如下：</span><span class="sxs-lookup"><span data-stu-id="e6002-317">Apps much be architected to:</span></span>

* <span data-ttu-id="e6002-318">限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="e6002-318">Limit cache growth.</span></span>
* <span data-ttu-id="e6002-319"><xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> 當可用的記憶體受到限制時，呼叫或：</span><span class="sxs-lookup"><span data-stu-id="e6002-319">Call <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> or <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> when available memory is limited:</span></span>

<span data-ttu-id="e6002-320">下列程式碼會建立可透過相依 <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> 性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小：</span><span class="sxs-lookup"><span data-stu-id="e6002-320">The following code creates a unitless fixed size <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="e6002-321">`SizeLimit`沒有單位。</span><span class="sxs-lookup"><span data-stu-id="e6002-321">`SizeLimit` does not have units.</span></span> <span data-ttu-id="e6002-322">如果已設定快取大小限制，則快取的專案必須以其認為最適合的任何單位來指定大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-322">Cached entries must specify size in whatever units they deem most appropriate if the cache size limit has been set.</span></span> <span data-ttu-id="e6002-323">快取實例的所有使用者都應使用相同的單位系統。</span><span class="sxs-lookup"><span data-stu-id="e6002-323">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="e6002-324">如果快取的專案大小總和超過所指定的值，則不會快取專案 `SizeLimit` 。</span><span class="sxs-lookup"><span data-stu-id="e6002-324">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="e6002-325">如果未設定快取大小限制，則會忽略在該專案上設定的快取大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-325">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="e6002-326">下列程式碼會 `MyMemoryCache` 向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e6002-326">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="e6002-327">`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取，並知道如何適當地設定快取專案大小。</span><span class="sxs-lookup"><span data-stu-id="e6002-327">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="e6002-328">下列程式碼會使用 `MyMemoryCache` ：</span><span class="sxs-lookup"><span data-stu-id="e6002-328">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="e6002-329">快取專案的大小可以透過[大小](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size)或[SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)擴充方法來設定：</span><span class="sxs-lookup"><span data-stu-id="e6002-329">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a><span data-ttu-id="e6002-330">MemoryCache Compact</span><span class="sxs-lookup"><span data-stu-id="e6002-330">MemoryCache.Compact</span></span>

<span data-ttu-id="e6002-331">`MemoryCache.Compact`嘗試依下列順序移除指定百分比的快取：</span><span class="sxs-lookup"><span data-stu-id="e6002-331">`MemoryCache.Compact` attempts to remove the specified percentage of the cache in the following order:</span></span>

* <span data-ttu-id="e6002-332">所有過期的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-332">All expired items.</span></span>
* <span data-ttu-id="e6002-333">依優先順序排序的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-333">Items by priority.</span></span> <span data-ttu-id="e6002-334">系統會先移除最低優先順序專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-334">Lowest priority items are removed first.</span></span>
* <span data-ttu-id="e6002-335">最近最少使用的物件。</span><span class="sxs-lookup"><span data-stu-id="e6002-335">Least recently used objects.</span></span>
* <span data-ttu-id="e6002-336">具有最早絕對到期的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-336">Items with the earliest absolute expiration.</span></span>
* <span data-ttu-id="e6002-337">具有最早滑動到期日的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-337">Items with the earliest sliding expiration.</span></span>

<span data-ttu-id="e6002-338">具有優先順序的固定專案永遠不會 <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> 被移除。</span><span class="sxs-lookup"><span data-stu-id="e6002-338">Pinned items with priority <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> are never removed.</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

<span data-ttu-id="e6002-339">如需詳細資訊，請參閱[GitHub 上的 Compact 來源](https://github.com/dotnet/extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393)。</span><span class="sxs-lookup"><span data-stu-id="e6002-339">See [Compact source on GitHub](https://github.com/dotnet/extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) for more information.</span></span>

## <a name="cache-dependencies"></a><span data-ttu-id="e6002-340">快取相依性</span><span class="sxs-lookup"><span data-stu-id="e6002-340">Cache dependencies</span></span>

<span data-ttu-id="e6002-341">下列範例示範如果相依專案過期，如何讓快取專案過期。</span><span class="sxs-lookup"><span data-stu-id="e6002-341">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="e6002-342"><xref:Microsoft.Extensions.Primitives.CancellationChangeToken>會新增至快取的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-342">A <xref:Microsoft.Extensions.Primitives.CancellationChangeToken> is added to the cached item.</span></span> <span data-ttu-id="e6002-343">`Cancel`在上呼叫時 `CancellationTokenSource` ，會收回這兩個快取專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-343">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="e6002-344">使用 `CancellationTokenSource` 可讓多個快取專案收回為群組。</span><span class="sxs-lookup"><span data-stu-id="e6002-344">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="e6002-345">使用 `using` 上述程式碼中的模式時，在區塊內建立的快取專案 `using` 將會繼承觸發程式和到期設定。</span><span class="sxs-lookup"><span data-stu-id="e6002-345">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="e6002-346">其他注意事項</span><span class="sxs-lookup"><span data-stu-id="e6002-346">Additional notes</span></span>

* <span data-ttu-id="e6002-347">使用回呼來重新擴展快取專案時：</span><span class="sxs-lookup"><span data-stu-id="e6002-347">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="e6002-348">因為回呼尚未完成，所以多個要求可以將快取的金鑰值空白。</span><span class="sxs-lookup"><span data-stu-id="e6002-348">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="e6002-349">這可能會導致數個執行緒重新填入快取的專案。</span><span class="sxs-lookup"><span data-stu-id="e6002-349">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="e6002-350">當有一個快取專案用來建立另一個時，子系會複製父系專案的到期權杖和以時間為基礎的到期設定。</span><span class="sxs-lookup"><span data-stu-id="e6002-350">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="e6002-351">子系不會因為手動移除或更新父專案而過期。</span><span class="sxs-lookup"><span data-stu-id="e6002-351">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="e6002-352">使用[PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks)設定在從快取中收回快取專案之後，將會引發的回呼。</span><span class="sxs-lookup"><span data-stu-id="e6002-352">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="background-cache-update"></a><span data-ttu-id="e6002-353">背景快取更新</span><span class="sxs-lookup"><span data-stu-id="e6002-353">Background cache update</span></span>

<span data-ttu-id="e6002-354">使用[背景服務](xref:fundamentals/host/hosted-services)（例如） <xref:Microsoft.Extensions.Hosting.IHostedService> 來更新快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-354">Use a [background service](xref:fundamentals/host/hosted-services) such as <xref:Microsoft.Extensions.Hosting.IHostedService> to update the cache.</span></span> <span data-ttu-id="e6002-355">背景服務可以重新計算專案，然後只有在準備就緒時，才將它們指派給快取。</span><span class="sxs-lookup"><span data-stu-id="e6002-355">The background service can recompute the entries and then assign them to the cache only when they’re ready.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6002-356">其他資源</span><span class="sxs-lookup"><span data-stu-id="e6002-356">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
