---
title: ASP.NET Core 中的記憶體快取
author: rick-anderson
description: 了解如何快取 ASP.NET Core 中的資料和記憶體。
ms.author: riande
ms.custom: mvc
ms.date: 8/22/2019
uid: performance/caching/memory
ms.openlocfilehash: 0479df14dd5d757710e361fce43782a8137588cb
ms.sourcegitcommit: 0365af91518004c4a44a30dc3a8ac324558a399b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198902"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="9f488-103">ASP.NET Core 中的記憶體快取</span><span class="sxs-lookup"><span data-stu-id="9f488-103">Cache in-memory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9f488-104">作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9f488-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9f488-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f488-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="9f488-106">快取基本概念</span><span class="sxs-lookup"><span data-stu-id="9f488-106">Caching basics</span></span>

<span data-ttu-id="9f488-107">快取可以藉由減少產生內容所需的工作，大幅改善應用程式的效能和擴充性。</span><span class="sxs-lookup"><span data-stu-id="9f488-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="9f488-108">快取最適用于不常變更**且**產生成本昂貴的資料。</span><span class="sxs-lookup"><span data-stu-id="9f488-108">Caching works best with data that changes infrequently **and** is expensive to generate.</span></span> <span data-ttu-id="9f488-109">快取會製作一份資料複本，其傳回的速度會比從來源快得多。</span><span class="sxs-lookup"><span data-stu-id="9f488-109">Caching makes a copy of data that can be returned much faster than from the source.</span></span> <span data-ttu-id="9f488-110">應用程式應撰寫並測試為**永遠不會**依賴快取的資料。</span><span class="sxs-lookup"><span data-stu-id="9f488-110">Apps should be written and tested to **never** depend on cached data.</span></span>

<span data-ttu-id="9f488-111">ASP.NET Core 支援數種不同的快取。</span><span class="sxs-lookup"><span data-stu-id="9f488-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="9f488-112">最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎。</span><span class="sxs-lookup"><span data-stu-id="9f488-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache).</span></span> <span data-ttu-id="9f488-113">`IMemoryCache`代表儲存在 web 伺服器記憶體中的快取。</span><span class="sxs-lookup"><span data-stu-id="9f488-113">`IMemoryCache` represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="9f488-114">使用記憶體內部快取時，在伺服器陣列（多部伺服器）上執行的應用程式應該確保會話是固定的。</span><span class="sxs-lookup"><span data-stu-id="9f488-114">Apps running on a server farm (multiple servers) should ensure sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="9f488-115">[粘滯會話] 可確保來自用戶端的後續要求都會移至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="9f488-115">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="9f488-116">例如，Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)（ARR），將所有後續要求路由傳送至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="9f488-116">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="9f488-117">Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取，以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="9f488-117">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="9f488-118">針對某些應用程式，分散式快取可支援比記憶體內部快取更高的相應放大。</span><span class="sxs-lookup"><span data-stu-id="9f488-118">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="9f488-119">使用分散式快取會將快取記憶體卸載至外部進程。</span><span class="sxs-lookup"><span data-stu-id="9f488-119">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="9f488-120">記憶體內部快取可以儲存任何物件。</span><span class="sxs-lookup"><span data-stu-id="9f488-120">The in-memory cache can store any object.</span></span> <span data-ttu-id="9f488-121">分散式快取介面僅限於`byte[]`。</span><span class="sxs-lookup"><span data-stu-id="9f488-121">The distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="9f488-122">記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="9f488-123">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="9f488-123">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="9f488-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>（[NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)）可與搭配使用：</span><span class="sxs-lookup"><span data-stu-id="9f488-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="9f488-125">.NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f488-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="9f488-126">以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。</span><span class="sxs-lookup"><span data-stu-id="9f488-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="9f488-127">例如，ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f488-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="9f488-128">.NET Framework 4.5 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f488-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="9f488-129">/建議使用`IMemoryCache` [MicrosoftExtensions`MemoryCache` . Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) (如本文所述), 因為它已更緊密整合到 ASP.NET Core 中。`System.Runtime.Caching` /</span><span class="sxs-lookup"><span data-stu-id="9f488-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this article) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="9f488-130">例如， `IMemoryCache`會以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="9f488-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="9f488-131">將`System.Runtime.Caching`程式碼從 ASP.NET 4.x 移植到 ASP.NET Core 時，請使用/ `MemoryCache`做為相容性橋接器。</span><span class="sxs-lookup"><span data-stu-id="9f488-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="9f488-132">快取指導方針</span><span class="sxs-lookup"><span data-stu-id="9f488-132">Cache guidelines</span></span>

* <span data-ttu-id="9f488-133">程式碼應該一律要有可用於擷取資料的後援選項，而**不**應該依賴快取的值。</span><span class="sxs-lookup"><span data-stu-id="9f488-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="9f488-134">快取使用不足的資源，記憶體。</span><span class="sxs-lookup"><span data-stu-id="9f488-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="9f488-135">限制快取成長：</span><span class="sxs-lookup"><span data-stu-id="9f488-135">Limit cache growth:</span></span>
  * <span data-ttu-id="9f488-136">請**勿**使用外部輸入作為快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9f488-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="9f488-137">使用到期時間來限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="9f488-137">Use expirations to limit cache growth.</span></span>
  * <span data-ttu-id="9f488-138">[使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-138">[Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span> <span data-ttu-id="9f488-139">ASP.NET Core 執行時間不會根據記憶體壓力**來限制快**取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-139">The ASP.NET Core runtime does **not** limit cache size based on memory pressure.</span></span> <span data-ttu-id="9f488-140">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-140">It's up to the developer to limit cache size.</span></span>

## <a name="use-imemorycache"></a><span data-ttu-id="9f488-141">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9f488-141">Use IMemoryCache</span></span>

> [!WARNING]
> <span data-ttu-id="9f488-142">從相依性[插入](xref:fundamentals/dependency-injection)使用共用記憶體快取， `SetSize`並`Size`呼叫、 `SizeLimit`或來限制快取大小，可能會導致應用程式失敗。</span><span class="sxs-lookup"><span data-stu-id="9f488-142">Using a *shared* memory cache from [Dependency Injection](xref:fundamentals/dependency-injection) and calling `SetSize`, `Size`, or `SizeLimit` to limit cache size can cause the app to fail.</span></span> <span data-ttu-id="9f488-143">在快取上設定大小限制時，所有專案在新增時都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-143">When a size limit is set on a cache, all entries must specify a size when being added.</span></span> <span data-ttu-id="9f488-144">這可能會導致問題，因為開發人員可能無法完全控制使用共用快取的內容。</span><span class="sxs-lookup"><span data-stu-id="9f488-144">This can lead to issues since developers may not have full control on what uses the shared cache.</span></span> <span data-ttu-id="9f488-145">例如，Entity Framework Core 使用共用快取，且未指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-145">For example, Entity Framework Core uses the shared cache and does not specify a size.</span></span> <span data-ttu-id="9f488-146">如果應用程式設定快取大小限制，並使用 EF Core，應用程式`InvalidOperationException`會擲回。</span><span class="sxs-lookup"><span data-stu-id="9f488-146">If an app sets a cache size limit and uses EF Core, the app throws an `InvalidOperationException`.</span></span>
> <span data-ttu-id="9f488-147">使用`SetSize`、 `Size`或來限制快取時，請建立單一快取來進行快取。`SizeLimit`</span><span class="sxs-lookup"><span data-stu-id="9f488-147">When using `SetSize`, `Size`, or `SizeLimit` to limit cache, create a cache singleton for caching.</span></span> <span data-ttu-id="9f488-148">如需詳細資訊和範例，請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-148">For more information and an example, see [Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span>

<span data-ttu-id="9f488-149">記憶體內部快取是使用相依性[插入](xref:fundamentals/dependency-injection)從應用程式參考的*服務*。</span><span class="sxs-lookup"><span data-stu-id="9f488-149">In-memory caching is a *service* that's referenced from an app using [Dependency Injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9f488-150">在此函數中要求實例：`IMemoryCache`</span><span class="sxs-lookup"><span data-stu-id="9f488-150">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

<span data-ttu-id="9f488-151">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。</span><span class="sxs-lookup"><span data-stu-id="9f488-151">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="9f488-152">如果沒有快取時間，則會建立新的專案，並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。</span><span class="sxs-lookup"><span data-stu-id="9f488-152">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span> <span data-ttu-id="9f488-153">`CacheKeys`類別是下載範例的一部分。</span><span class="sxs-lookup"><span data-stu-id="9f488-153">The `CacheKeys` class is part of the download sample.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="9f488-154">此時會顯示目前的時間和快取時間：</span><span class="sxs-lookup"><span data-stu-id="9f488-154">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

<span data-ttu-id="9f488-155">當超時時間內有要求時，快取的值會保留在快取中。`DateTime`</span><span class="sxs-lookup"><span data-stu-id="9f488-155">The cached `DateTime` value remains in the cache while there are requests within the timeout period.</span></span>

<span data-ttu-id="9f488-156">下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。</span><span class="sxs-lookup"><span data-stu-id="9f488-156">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="9f488-157">下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間：</span><span class="sxs-lookup"><span data-stu-id="9f488-157">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="9f488-158">下列程式碼會取得或建立具有絕對過期的快取專案：</span><span class="sxs-lookup"><span data-stu-id="9f488-158">The following code gets or creates a cached item with absolute expiration:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

<span data-ttu-id="9f488-159">只有具有滑動期限的快取專案集，才會有過時的風險。</span><span class="sxs-lookup"><span data-stu-id="9f488-159">A cached item set with a sliding expiration only is at risk of becoming stale.</span></span> <span data-ttu-id="9f488-160">如果存取的頻率高於滑動到期時間間隔，專案將永遠不會過期。</span><span class="sxs-lookup"><span data-stu-id="9f488-160">If it's accessed more frequently than the sliding expiration interval, the item will never expire.</span></span> <span data-ttu-id="9f488-161">結合滑動期限與絕對到期日，以保證專案在其絕對到期時間通過後到期。</span><span class="sxs-lookup"><span data-stu-id="9f488-161">Combine a sliding expiration with an absolute expiration to guarantee that the item expires once its absolute expiration time passes.</span></span> <span data-ttu-id="9f488-162">絕對到期時間會設定專案可快取的時間上限，但如果在滑動到期時間間隔內未要求，仍然允許專案提早過期。</span><span class="sxs-lookup"><span data-stu-id="9f488-162">The absolute expiration sets an upper bound to how long the item can be cached while still allowing the item to expire earlier if it isn't requested within the sliding expiration interval.</span></span> <span data-ttu-id="9f488-163">當同時指定絕對和滑動期限時，會以邏輯方式 ORed 到期日。</span><span class="sxs-lookup"><span data-stu-id="9f488-163">When both absolute and sliding expiration are specified, the expirations are logically ORed.</span></span> <span data-ttu-id="9f488-164">如果滑動到期間隔*或*絕對到期時間經過，則會從快取中收回專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-164">If either the sliding expiration interval *or* the absolute expiration time pass, the item is evicted from the cache.</span></span>

<span data-ttu-id="9f488-165">下列程式碼會取得或建立具有滑動*和*絕對到期的快取專案：</span><span class="sxs-lookup"><span data-stu-id="9f488-165">The following code gets or creates a cached item with both sliding *and* absolute expiration:</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

<span data-ttu-id="9f488-166">上述程式碼可確保資料不會快取超過絕對時間。</span><span class="sxs-lookup"><span data-stu-id="9f488-166">The preceding code guarantees the data will not be cached longer than the absolute time.</span></span>

<span data-ttu-id="9f488-167"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions>和<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*>是類別中的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="9f488-167"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>, <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*> are extension methods in the <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions> class.</span></span> <span data-ttu-id="9f488-168">這些方法會擴充的功能<xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>。</span><span class="sxs-lookup"><span data-stu-id="9f488-168">These methods extend the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="9f488-169">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="9f488-169">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="9f488-170">下列範例：</span><span class="sxs-lookup"><span data-stu-id="9f488-170">The following sample:</span></span>

* <span data-ttu-id="9f488-171">設定滑動到期時間。</span><span class="sxs-lookup"><span data-stu-id="9f488-171">Sets a sliding expiration time.</span></span> <span data-ttu-id="9f488-172">存取此快取專案的要求將會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="9f488-172">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="9f488-173">將快取優先順序設定為[CacheItemPriority. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove)。</span><span class="sxs-lookup"><span data-stu-id="9f488-173">Sets the cache priority to [CacheItemPriority.NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove).</span></span>
* <span data-ttu-id="9f488-174">設定在從快取中收回專案之後，將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。</span><span class="sxs-lookup"><span data-stu-id="9f488-174">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="9f488-175">回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="9f488-175">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="9f488-176">使用 SetSize、Size 和 SizeLimit 來限制快取大小</span><span class="sxs-lookup"><span data-stu-id="9f488-176">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="9f488-177">`MemoryCache`實例可以選擇性地指定並強制執行大小限制。</span><span class="sxs-lookup"><span data-stu-id="9f488-177">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="9f488-178">記憶體大小限制沒有定義的測量單位，因為快取沒有測量專案大小的機制。</span><span class="sxs-lookup"><span data-stu-id="9f488-178">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="9f488-179">如果已設定快取記憶體大小限制，所有專案都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-179">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="9f488-180">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-180">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="9f488-181">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-181">It's up to the developer to limit cache size.</span></span> <span data-ttu-id="9f488-182">指定的大小是開發人員選擇的單位。</span><span class="sxs-lookup"><span data-stu-id="9f488-182">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="9f488-183">例如:</span><span class="sxs-lookup"><span data-stu-id="9f488-183">For example:</span></span>

* <span data-ttu-id="9f488-184">如果 web 應用程式主要是快取字串，則每個快取專案大小都可以是字串長度。</span><span class="sxs-lookup"><span data-stu-id="9f488-184">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="9f488-185">應用程式可以將所有專案的大小指定為1，而大小限制是專案的計數。</span><span class="sxs-lookup"><span data-stu-id="9f488-185">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="9f488-186">如果<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit>未設定，則快取會成長而不受限制。</span><span class="sxs-lookup"><span data-stu-id="9f488-186">If <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> is not set, the cache grows without bound.</span></span> <span data-ttu-id="9f488-187">當系統記憶體不足時，ASP.NET Core 執行時間不會修剪快取。</span><span class="sxs-lookup"><span data-stu-id="9f488-187">The ASP.NET Core runtime does not trim the cache when system memory is low.</span></span> <span data-ttu-id="9f488-188">應用程式的架構大致如下：</span><span class="sxs-lookup"><span data-stu-id="9f488-188">Apps much be architected to:</span></span>

* <span data-ttu-id="9f488-189">限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="9f488-189">Limit cache growth.</span></span>
* <span data-ttu-id="9f488-190">當<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*>可用<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*>的記憶體受到限制時，呼叫或：</span><span class="sxs-lookup"><span data-stu-id="9f488-190">Call <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> or <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> when available memory is limited:</span></span>

<span data-ttu-id="9f488-191">下列程式碼會建立可透過相依<xref:Microsoft.Extensions.Caching.Memory.MemoryCache>性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小：</span><span class="sxs-lookup"><span data-stu-id="9f488-191">The following code creates a unitless fixed size <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="9f488-192">`SizeLimit`沒有單位。</span><span class="sxs-lookup"><span data-stu-id="9f488-192">`SizeLimit` does not have units.</span></span> <span data-ttu-id="9f488-193">如果已設定快取記憶體大小，則快取的專案必須以其認為最適合的任何單位來指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-193">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="9f488-194">快取實例的所有使用者都應使用相同的單位系統。</span><span class="sxs-lookup"><span data-stu-id="9f488-194">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="9f488-195">如果快取的專案大小總和超過所指定`SizeLimit`的值，則不會快取專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-195">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="9f488-196">如果未設定快取大小限制，則會忽略在該專案上設定的快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-196">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="9f488-197">下列程式碼會`MyMemoryCache`向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。</span><span class="sxs-lookup"><span data-stu-id="9f488-197">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

<span data-ttu-id="9f488-198">`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取，並知道如何適當地設定快取專案大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-198">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="9f488-199">下列程式碼會`MyMemoryCache`使用：</span><span class="sxs-lookup"><span data-stu-id="9f488-199">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

<span data-ttu-id="9f488-200">快取專案的大小可以透過<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*>或擴充方法來設定：</span><span class="sxs-lookup"><span data-stu-id="9f488-200">The size of the cache entry can be set by <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> or the <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*> extension methods:</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a><span data-ttu-id="9f488-201">MemoryCache Compact</span><span class="sxs-lookup"><span data-stu-id="9f488-201">MemoryCache.Compact</span></span>

<span data-ttu-id="9f488-202">`MemoryCache.Compact`嘗試依下列順序移除指定百分比的快取：</span><span class="sxs-lookup"><span data-stu-id="9f488-202">`MemoryCache.Compact` attempts to remove the specified percentage of the cache in the following order:</span></span>

* <span data-ttu-id="9f488-203">所有過期的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-203">All expired items.</span></span>
* <span data-ttu-id="9f488-204">依優先順序排序的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-204">Items by priority.</span></span> <span data-ttu-id="9f488-205">系統會先移除最低優先順序專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-205">Lowest priority items are removed first.</span></span>
* <span data-ttu-id="9f488-206">最近最少使用的物件。</span><span class="sxs-lookup"><span data-stu-id="9f488-206">Least recently used objects.</span></span>
* <span data-ttu-id="9f488-207">具有最早絕對到期的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-207">Items with the earliest absolute expiration.</span></span>
* <span data-ttu-id="9f488-208">具有最早滑動到期日的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-208">Items with the earliest sliding expiration.</span></span>

<span data-ttu-id="9f488-209">具有優先順序<xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove>的固定專案永遠不會被移除。</span><span class="sxs-lookup"><span data-stu-id="9f488-209">Pinned items with priority <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> are never removed.</span></span> <span data-ttu-id="9f488-210">下列程式碼會移除快取專案並`Compact`呼叫：</span><span class="sxs-lookup"><span data-stu-id="9f488-210">The following code removes a cache item and calls `Compact`:</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

<span data-ttu-id="9f488-211">如需詳細資訊，請參閱[GitHub 上的 Compact 來源](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393)。</span><span class="sxs-lookup"><span data-stu-id="9f488-211">See [Compact source on GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) for more information.</span></span>

## <a name="cache-dependencies"></a><span data-ttu-id="9f488-212">快取相依性</span><span class="sxs-lookup"><span data-stu-id="9f488-212">Cache dependencies</span></span>

<span data-ttu-id="9f488-213">下列範例示範如何在相依項目過期時將快取項目設定為過期。</span><span class="sxs-lookup"><span data-stu-id="9f488-213">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="9f488-214">`CancellationChangeToken` 會新增至快取的項目。</span><span class="sxs-lookup"><span data-stu-id="9f488-214">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="9f488-215">當在 `CancellationTokenSource` 上呼叫 `Cancel` 時，會撤出兩個快取項目。</span><span class="sxs-lookup"><span data-stu-id="9f488-215">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="9f488-216">`CancellationTokenSource`使用可讓多個快取專案收回為群組。</span><span class="sxs-lookup"><span data-stu-id="9f488-216">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="9f488-217">使用上述`using`程式碼中的模式時，在`using`區塊內建立的快取專案將會繼承觸發程式和到期設定。</span><span class="sxs-lookup"><span data-stu-id="9f488-217">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="9f488-218">其他備註</span><span class="sxs-lookup"><span data-stu-id="9f488-218">Additional notes</span></span>

* <span data-ttu-id="9f488-219">到期不會在背景中發生。</span><span class="sxs-lookup"><span data-stu-id="9f488-219">Expiration doesn't happen in the background.</span></span> <span data-ttu-id="9f488-220">沒有任何計時器會主動掃描快取中是否有過期的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-220">There is no timer that actively scans the cache for expired items.</span></span> <span data-ttu-id="9f488-221">快取上的任何活動`Get`（ `Set`、 `Remove`、）都可以觸發過期專案的背景掃描。</span><span class="sxs-lookup"><span data-stu-id="9f488-221">Any activity on the cache (`Get`, `Set`, `Remove`) can trigger a background scan for expired items.</span></span> <span data-ttu-id="9f488-222">`CancellationTokenSource` （`CancelAfter`）上的計時器也會移除專案，並觸發過期專案的掃描。</span><span class="sxs-lookup"><span data-stu-id="9f488-222">A timer on the `CancellationTokenSource` (`CancelAfter`) would also remove the entry and trigger a scan for expired items.</span></span> <span data-ttu-id="9f488-223">例如，不是使用`SetAbsoluteExpiration(TimeSpan.FromHours(1))`，而是針對註冊的權杖使用。 `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))`</span><span class="sxs-lookup"><span data-stu-id="9f488-223">For example, rather than using `SetAbsoluteExpiration(TimeSpan.FromHours(1))`, use `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))` for the registered token.</span></span> <span data-ttu-id="9f488-224">當此標記引發時，它會立即移除該專案，並引發收回回呼。</span><span class="sxs-lookup"><span data-stu-id="9f488-224">When this token fires it removes the entry immediately and fires the eviction callbacks.</span></span> <span data-ttu-id="9f488-225">如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/Caching/issues/248)。</span><span class="sxs-lookup"><span data-stu-id="9f488-225">For more information, see [this GitHub issue](https://github.com/aspnet/Caching/issues/248).</span></span>
* <span data-ttu-id="9f488-226">使用回呼來重新擴展快取專案時：</span><span class="sxs-lookup"><span data-stu-id="9f488-226">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="9f488-227">因為回呼尚未完成，所以多個要求可以將快取的金鑰值空白。</span><span class="sxs-lookup"><span data-stu-id="9f488-227">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="9f488-228">這可能會導致數個執行緒重新填入快取的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-228">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="9f488-229">當有一個快取專案用來建立另一個時，子系會複製父系專案的到期權杖和以時間為基礎的到期設定。</span><span class="sxs-lookup"><span data-stu-id="9f488-229">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="9f488-230">子系不會因為手動移除或更新父專案而過期。</span><span class="sxs-lookup"><span data-stu-id="9f488-230">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="9f488-231">用<xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks>來設定將在從快取中收回快取專案之後引發的回呼。</span><span class="sxs-lookup"><span data-stu-id="9f488-231">Use <xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks> to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>
* <span data-ttu-id="9f488-232">大部分的應用程式`IMemoryCache`都已啟用。</span><span class="sxs-lookup"><span data-stu-id="9f488-232">For most apps, `IMemoryCache` is enabled.</span></span> <span data-ttu-id="9f488-233">例如，在中`AddMvc` `AddMvcCore().AddRazorViewEngine` `AddRazorPages` `AddControllersWithViews` `Add{Service}`呼叫、、、和許多其他方法，會啟用`IMemoryCache`。 `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="9f488-233">For example, calling `AddMvc`, `AddControllersWithViews`, `AddRazorPages`, `AddMvcCore().AddRazorViewEngine`, and many other `Add{Service}` methods in `ConfigureServices`, enables `IMemoryCache`.</span></span> <span data-ttu-id="9f488-234">對於未呼叫上述`Add{Service}`其中一種方法的應用程式，可能需要在中`ConfigureServices`呼叫<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> 。</span><span class="sxs-lookup"><span data-stu-id="9f488-234">For apps that are not calling one of the preceding `Add{Service}` methods, it may be necessary to call <xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> in `ConfigureServices`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f488-235">其他資源</span><span class="sxs-lookup"><span data-stu-id="9f488-235">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<!-- This is the 2.1 version -->
<span data-ttu-id="9f488-236">作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9f488-236">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9f488-237">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f488-237">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="9f488-238">快取基本概念</span><span class="sxs-lookup"><span data-stu-id="9f488-238">Caching basics</span></span>

<span data-ttu-id="9f488-239">快取可以藉由減少產生內容所需的工作，大幅改善應用程式的效能和擴充性。</span><span class="sxs-lookup"><span data-stu-id="9f488-239">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="9f488-240">快取最適合不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="9f488-240">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="9f488-241">快取會製作一份資料複本，其傳回的速度會比原始來源更快。</span><span class="sxs-lookup"><span data-stu-id="9f488-241">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="9f488-242">撰寫和測試程式碼時，**絕對不**會依賴快取的資料。</span><span class="sxs-lookup"><span data-stu-id="9f488-242">Code should be written and tested to **never** depend on cached data.</span></span>

<span data-ttu-id="9f488-243">ASP.NET Core 支援數種不同的快取。</span><span class="sxs-lookup"><span data-stu-id="9f488-243">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="9f488-244">最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎，代表儲存在 web 伺服器記憶體中的快取。</span><span class="sxs-lookup"><span data-stu-id="9f488-244">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="9f488-245">在伺服器陣列（多部伺服器）上執行的應用程式應該在使用記憶體內部快取時，確保會話是粘滯的。</span><span class="sxs-lookup"><span data-stu-id="9f488-245">Apps that run on a server farm (multiple servers) should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="9f488-246">[粘滯會話] 可確保之後從用戶端提出的要求全都會移至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="9f488-246">Sticky sessions ensure that later requests from a client all go to the same server.</span></span> <span data-ttu-id="9f488-247">例如，Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)（ARR），將來自使用者代理程式的所有要求路由傳送至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="9f488-247">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all requests from a user agent to the same server.</span></span>

<span data-ttu-id="9f488-248">Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取，以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="9f488-248">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="9f488-249">針對某些應用程式，分散式快取可支援比記憶體內部快取更高的相應放大。</span><span class="sxs-lookup"><span data-stu-id="9f488-249">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="9f488-250">使用分散式快取會將快取記憶體卸載至外部進程。</span><span class="sxs-lookup"><span data-stu-id="9f488-250">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="9f488-251">記憶體內部快取可以儲存任何物件。</span><span class="sxs-lookup"><span data-stu-id="9f488-251">The in-memory cache can store any object.</span></span> <span data-ttu-id="9f488-252">分散式快取介面僅限於`byte[]`。</span><span class="sxs-lookup"><span data-stu-id="9f488-252">The distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="9f488-253">記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-253">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="9f488-254">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="9f488-254">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="9f488-255"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>（[NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)）可與搭配使用：</span><span class="sxs-lookup"><span data-stu-id="9f488-255"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="9f488-256">.NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f488-256">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="9f488-257">以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。</span><span class="sxs-lookup"><span data-stu-id="9f488-257">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="9f488-258">例如，ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f488-258">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="9f488-259">.NET Framework 4.5 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9f488-259">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="9f488-260">/建議使用`IMemoryCache` [MicrosoftExtensions`MemoryCache` . Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) (如本文所述), 因為它已更緊密整合到 ASP.NET Core 中。`System.Runtime.Caching` /</span><span class="sxs-lookup"><span data-stu-id="9f488-260">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this article) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="9f488-261">例如， `IMemoryCache`會以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="9f488-261">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="9f488-262">將`System.Runtime.Caching`程式碼從 ASP.NET 4.x 移植到 ASP.NET Core 時，請使用/ `MemoryCache`做為相容性橋接器。</span><span class="sxs-lookup"><span data-stu-id="9f488-262">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="9f488-263">快取指導方針</span><span class="sxs-lookup"><span data-stu-id="9f488-263">Cache guidelines</span></span>

* <span data-ttu-id="9f488-264">程式碼應該一律要有可用於擷取資料的後援選項，而**不**應該依賴快取的值。</span><span class="sxs-lookup"><span data-stu-id="9f488-264">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="9f488-265">快取使用不足的資源，記憶體。</span><span class="sxs-lookup"><span data-stu-id="9f488-265">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="9f488-266">限制快取成長：</span><span class="sxs-lookup"><span data-stu-id="9f488-266">Limit cache growth:</span></span>
  * <span data-ttu-id="9f488-267">請**勿**使用外部輸入作為快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9f488-267">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="9f488-268">使用到期時間來限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="9f488-268">Use expirations to limit cache growth.</span></span>
  * <span data-ttu-id="9f488-269">[使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-269">[Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span> <span data-ttu-id="9f488-270">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-270">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="9f488-271">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-271">It's up to the developer to limit cache size.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="9f488-272">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9f488-272">Using IMemoryCache</span></span>

> [!WARNING]
> <span data-ttu-id="9f488-273">從相依性[插入](xref:fundamentals/dependency-injection)使用共用記憶體快取， `SetSize`並`Size`呼叫、 `SizeLimit`或來限制快取大小，可能會導致應用程式失敗。</span><span class="sxs-lookup"><span data-stu-id="9f488-273">Using a *shared* memory cache from [Dependency Injection](xref:fundamentals/dependency-injection) and calling `SetSize`, `Size`, or `SizeLimit` to limit cache size can cause the app to fail.</span></span> <span data-ttu-id="9f488-274">在快取上設定大小限制時，所有專案在新增時都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-274">When a size limit is set on a cache, all entries must specify a size when being added.</span></span> <span data-ttu-id="9f488-275">這可能會導致問題，因為開發人員可能無法完全控制使用共用快取的內容。</span><span class="sxs-lookup"><span data-stu-id="9f488-275">This can lead to issues since developers may not have full control on what uses the shared cache.</span></span> <span data-ttu-id="9f488-276">例如，Entity Framework Core 使用共用快取，且未指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-276">For example, Entity Framework Core uses the shared cache and does not specify a size.</span></span> <span data-ttu-id="9f488-277">如果應用程式設定快取大小限制，並使用 EF Core，應用程式`InvalidOperationException`會擲回。</span><span class="sxs-lookup"><span data-stu-id="9f488-277">If an app sets a cache size limit and uses EF Core, the app throws an `InvalidOperationException`.</span></span>
> <span data-ttu-id="9f488-278">使用`SetSize`、 `Size`或來限制快取時，請建立單一快取來進行快取。`SizeLimit`</span><span class="sxs-lookup"><span data-stu-id="9f488-278">When using `SetSize`, `Size`, or `SizeLimit` to limit cache, create a cache singleton for caching.</span></span> <span data-ttu-id="9f488-279">如需詳細資訊和範例，請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-279">For more information and an example, see [Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span>

<span data-ttu-id="9f488-280">記憶體內部快取是使用相依性插入從您的應用程式參考的一[項](../../fundamentals/dependency-injection.md)*服務*。</span><span class="sxs-lookup"><span data-stu-id="9f488-280">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="9f488-281">在`AddMemoryCache` 中`ConfigureServices`呼叫：</span><span class="sxs-lookup"><span data-stu-id="9f488-281">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="9f488-282">在此函數中要求實例：`IMemoryCache`</span><span class="sxs-lookup"><span data-stu-id="9f488-282">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

<span data-ttu-id="9f488-283">`IMemoryCache`需要 NuGet 套件 AspNetCore。您可以在[中繼套件](xref:fundamentals/metapackage-app)中找到該[儲存體](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。</span><span class="sxs-lookup"><span data-stu-id="9f488-283">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="9f488-284">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。</span><span class="sxs-lookup"><span data-stu-id="9f488-284">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="9f488-285">如果沒有快取時間，則會建立新的專案，並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。</span><span class="sxs-lookup"><span data-stu-id="9f488-285">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

<span data-ttu-id="9f488-286">[！ code-csharp [] （memory/sample/WebCache/CacheKeys .cs） [](memory/sample/WebCache/CacheKeys.cs)]</span><span class="sxs-lookup"><span data-stu-id="9f488-286">[!code-csharp [](memory/sample/WebCache/CacheKeys.cs) [](memory/sample/WebCache/CacheKeys.cs)]</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="9f488-287">此時會顯示目前的時間和快取時間：</span><span class="sxs-lookup"><span data-stu-id="9f488-287">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="9f488-288">當超時時間內有要求時，快取的值會保留在快取中。`DateTime`</span><span class="sxs-lookup"><span data-stu-id="9f488-288">The cached `DateTime` value remains in the cache while there are requests within the timeout period.</span></span> <span data-ttu-id="9f488-289">下圖顯示從快取中抓取的目前時間和較舊的時間：</span><span class="sxs-lookup"><span data-stu-id="9f488-289">The following image shows the current time and an older time retrieved from the cache:</span></span>

![顯示兩個不同時間的索引視圖](memory/_static/time.png)

<span data-ttu-id="9f488-291">下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。</span><span class="sxs-lookup"><span data-stu-id="9f488-291">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="9f488-292">下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間：</span><span class="sxs-lookup"><span data-stu-id="9f488-292">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="9f488-293"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>和[Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)是[CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>類別的擴充方法部分，可延伸的功能。</span><span class="sxs-lookup"><span data-stu-id="9f488-293"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="9f488-294">如需其他快取方法的說明，請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)。</span><span class="sxs-lookup"><span data-stu-id="9f488-294">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="9f488-295">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="9f488-295">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="9f488-296">下列範例：</span><span class="sxs-lookup"><span data-stu-id="9f488-296">The following sample:</span></span>

* <span data-ttu-id="9f488-297">設定滑動到期時間。</span><span class="sxs-lookup"><span data-stu-id="9f488-297">Sets a sliding expiration time.</span></span> <span data-ttu-id="9f488-298">存取此快取專案的要求將會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="9f488-298">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="9f488-299">將快取優先順序設定`CacheItemPriority.NeverRemove`為。</span><span class="sxs-lookup"><span data-stu-id="9f488-299">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
* <span data-ttu-id="9f488-300">設定在從快取中收回專案之後，將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。</span><span class="sxs-lookup"><span data-stu-id="9f488-300">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="9f488-301">回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="9f488-301">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="9f488-302">使用 SetSize、Size 和 SizeLimit 來限制快取大小</span><span class="sxs-lookup"><span data-stu-id="9f488-302">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="9f488-303">`MemoryCache`實例可以選擇性地指定並強制執行大小限制。</span><span class="sxs-lookup"><span data-stu-id="9f488-303">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="9f488-304">記憶體大小限制沒有定義的測量單位，因為快取沒有測量專案大小的機制。</span><span class="sxs-lookup"><span data-stu-id="9f488-304">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="9f488-305">如果已設定快取記憶體大小限制，所有專案都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-305">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="9f488-306">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-306">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="9f488-307">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-307">It's up to the developer to limit cache size.</span></span> <span data-ttu-id="9f488-308">指定的大小是開發人員選擇的單位。</span><span class="sxs-lookup"><span data-stu-id="9f488-308">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="9f488-309">例如:</span><span class="sxs-lookup"><span data-stu-id="9f488-309">For example:</span></span>

* <span data-ttu-id="9f488-310">如果 web 應用程式主要是快取字串，則每個快取專案大小都可以是字串長度。</span><span class="sxs-lookup"><span data-stu-id="9f488-310">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="9f488-311">應用程式可以將所有專案的大小指定為1，而大小限制是專案的計數。</span><span class="sxs-lookup"><span data-stu-id="9f488-311">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="9f488-312">如果<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit>未設定，則快取會成長而不受限制。</span><span class="sxs-lookup"><span data-stu-id="9f488-312">If <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit> is not set, the cache grows without bound.</span></span> <span data-ttu-id="9f488-313">當系統記憶體不足時，ASP.NET Core 執行時間不會修剪快取。</span><span class="sxs-lookup"><span data-stu-id="9f488-313">The ASP.NET Core runtime does not trim the cache when system memory is low.</span></span> <span data-ttu-id="9f488-314">應用程式的架構大致如下：</span><span class="sxs-lookup"><span data-stu-id="9f488-314">Apps much be architected to:</span></span>

* <span data-ttu-id="9f488-315">限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="9f488-315">Limit cache growth.</span></span>
* <span data-ttu-id="9f488-316">當<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*>可用<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*>的記憶體受到限制時，呼叫或：</span><span class="sxs-lookup"><span data-stu-id="9f488-316">Call <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*> or <xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*> when available memory is limited:</span></span>

<span data-ttu-id="9f488-317">下列程式碼會建立可透過相依<xref:Microsoft.Extensions.Caching.Memory.MemoryCache>性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小：</span><span class="sxs-lookup"><span data-stu-id="9f488-317">The following code creates a unitless fixed size <xref:Microsoft.Extensions.Caching.Memory.MemoryCache> accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="9f488-318">`SizeLimit`沒有單位。</span><span class="sxs-lookup"><span data-stu-id="9f488-318">`SizeLimit` does not have units.</span></span> <span data-ttu-id="9f488-319">如果已設定快取記憶體大小，則快取的專案必須以其認為最適合的任何單位來指定大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-319">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="9f488-320">快取實例的所有使用者都應使用相同的單位系統。</span><span class="sxs-lookup"><span data-stu-id="9f488-320">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="9f488-321">如果快取的專案大小總和超過所指定`SizeLimit`的值，則不會快取專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-321">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="9f488-322">如果未設定快取大小限制，則會忽略在該專案上設定的快取大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-322">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="9f488-323">下列程式碼會`MyMemoryCache`向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。</span><span class="sxs-lookup"><span data-stu-id="9f488-323">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="9f488-324">`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取，並知道如何適當地設定快取專案大小。</span><span class="sxs-lookup"><span data-stu-id="9f488-324">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="9f488-325">下列程式碼會`MyMemoryCache`使用：</span><span class="sxs-lookup"><span data-stu-id="9f488-325">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="9f488-326">快取專案的大小可以透過[大小](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size)或[SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)擴充方法來設定：</span><span class="sxs-lookup"><span data-stu-id="9f488-326">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a><span data-ttu-id="9f488-327">MemoryCache Compact</span><span class="sxs-lookup"><span data-stu-id="9f488-327">MemoryCache.Compact</span></span>

<span data-ttu-id="9f488-328">`MemoryCache.Compact`嘗試依下列順序移除指定百分比的快取：</span><span class="sxs-lookup"><span data-stu-id="9f488-328">`MemoryCache.Compact` attempts to remove the specified percentage of the cache in the following order:</span></span>

* <span data-ttu-id="9f488-329">所有過期的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-329">All expired items.</span></span>
* <span data-ttu-id="9f488-330">依優先順序排序的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-330">Items by priority.</span></span> <span data-ttu-id="9f488-331">系統會先移除最低優先順序專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-331">Lowest priority items are removed first.</span></span>
* <span data-ttu-id="9f488-332">最近最少使用的物件。</span><span class="sxs-lookup"><span data-stu-id="9f488-332">Least recently used objects.</span></span>
* <span data-ttu-id="9f488-333">具有最早絕對到期的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-333">Items with the earliest absolute expiration.</span></span>
* <span data-ttu-id="9f488-334">具有最早滑動到期日的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-334">Items with the earliest sliding expiration.</span></span>

<span data-ttu-id="9f488-335">具有優先順序<xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove>的固定專案永遠不會被移除。</span><span class="sxs-lookup"><span data-stu-id="9f488-335">Pinned items with priority <xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove> are never removed.</span></span>

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

<span data-ttu-id="9f488-336">如需詳細資訊，請參閱[GitHub 上的 Compact 來源](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393)。</span><span class="sxs-lookup"><span data-stu-id="9f488-336">See [Compact source on GitHub](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393) for more information.</span></span>

## <a name="cache-dependencies"></a><span data-ttu-id="9f488-337">快取相依性</span><span class="sxs-lookup"><span data-stu-id="9f488-337">Cache dependencies</span></span>

<span data-ttu-id="9f488-338">下列範例示範如何在相依項目過期時將快取項目設定為過期。</span><span class="sxs-lookup"><span data-stu-id="9f488-338">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="9f488-339">`CancellationChangeToken` 會新增至快取的項目。</span><span class="sxs-lookup"><span data-stu-id="9f488-339">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="9f488-340">當在 `CancellationTokenSource` 上呼叫 `Cancel` 時，會撤出兩個快取項目。</span><span class="sxs-lookup"><span data-stu-id="9f488-340">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="9f488-341">`CancellationTokenSource`使用可讓多個快取專案收回為群組。</span><span class="sxs-lookup"><span data-stu-id="9f488-341">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="9f488-342">使用上述`using`程式碼中的模式時，在`using`區塊內建立的快取專案將會繼承觸發程式和到期設定。</span><span class="sxs-lookup"><span data-stu-id="9f488-342">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="9f488-343">其他備註</span><span class="sxs-lookup"><span data-stu-id="9f488-343">Additional notes</span></span>

* <span data-ttu-id="9f488-344">使用回呼來重新擴展快取專案時：</span><span class="sxs-lookup"><span data-stu-id="9f488-344">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="9f488-345">因為回呼尚未完成，所以多個要求可以將快取的金鑰值空白。</span><span class="sxs-lookup"><span data-stu-id="9f488-345">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="9f488-346">這可能會導致數個執行緒重新填入快取的專案。</span><span class="sxs-lookup"><span data-stu-id="9f488-346">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="9f488-347">當有一個快取專案用來建立另一個時，子系會複製父系專案的到期權杖和以時間為基礎的到期設定。</span><span class="sxs-lookup"><span data-stu-id="9f488-347">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="9f488-348">子系不會因為手動移除或更新父專案而過期。</span><span class="sxs-lookup"><span data-stu-id="9f488-348">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="9f488-349">使用[PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks)設定在從快取中收回快取專案之後，將會引發的回呼。</span><span class="sxs-lookup"><span data-stu-id="9f488-349">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f488-350">其他資源</span><span class="sxs-lookup"><span data-stu-id="9f488-350">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
