---
title: ASP.NET Core 中的記憶體快取
author: rick-anderson
description: 了解如何快取 ASP.NET Core 中的資料和記憶體。
ms.author: riande
ms.custom: mvc
ms.date: 8/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 474f71225371ea89b023ee077d4ecc03e9751681
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030322"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="bb04e-103">ASP.NET Core 中的記憶體快取</span><span class="sxs-lookup"><span data-stu-id="bb04e-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="bb04e-104">作者: [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="bb04e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="bb04e-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bb04e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="bb04e-106">快取基本概念</span><span class="sxs-lookup"><span data-stu-id="bb04e-106">Caching basics</span></span>

<span data-ttu-id="bb04e-107">快取可以藉由減少產生內容所需的工作, 大幅改善應用程式的效能和擴充性。</span><span class="sxs-lookup"><span data-stu-id="bb04e-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="bb04e-108">快取最適合不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="bb04e-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="bb04e-109">快取會製作一份資料複本, 其傳回的速度會比原始來源更快。</span><span class="sxs-lookup"><span data-stu-id="bb04e-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="bb04e-110">應用程式應撰寫並測試為**永遠不會**依賴快取的資料。</span><span class="sxs-lookup"><span data-stu-id="bb04e-110">Apps should be written and tested to **never** depend on cached data.</span></span>

<span data-ttu-id="bb04e-111">ASP.NET Core 支援數種不同的快取。</span><span class="sxs-lookup"><span data-stu-id="bb04e-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="bb04e-112">最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎, 代表儲存在 web 伺服器記憶體中的快取。</span><span class="sxs-lookup"><span data-stu-id="bb04e-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="bb04e-113">在多部伺服器的伺服器陣列上執行的應用程式, 應該在使用記憶體內部快取時, 確保會話是固定的。</span><span class="sxs-lookup"><span data-stu-id="bb04e-113">Apps that run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="bb04e-114">[粘滯會話] 可確保來自用戶端的後續要求都會移至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="bb04e-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="bb04e-115">例如, Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR), 將所有後續要求路由傳送至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="bb04e-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="bb04e-116">Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取, 以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="bb04e-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="bb04e-117">針對某些應用程式, 分散式快取可支援比記憶體內部快取更高的相應放大。</span><span class="sxs-lookup"><span data-stu-id="bb04e-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="bb04e-118">使用分散式快取會將快取記憶體卸載至外部進程。</span><span class="sxs-lookup"><span data-stu-id="bb04e-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bb04e-119">除非快取[優先順序](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority)設定為`CacheItemPriority.NeverRemove`, 否則快取會收回記憶體壓力下的快取專案。 `IMemoryCache`</span><span class="sxs-lookup"><span data-stu-id="bb04e-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="bb04e-120">您可以設定`CacheItemPriority` , 以調整快取在記憶體壓力下收回專案的優先順序。</span><span class="sxs-lookup"><span data-stu-id="bb04e-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

::: moniker-end

<span data-ttu-id="bb04e-121">記憶體內部快取可以儲存任何物件;分散式快取介面僅限於`byte[]`。</span><span class="sxs-lookup"><span data-stu-id="bb04e-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span> <span data-ttu-id="bb04e-122">記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。</span><span class="sxs-lookup"><span data-stu-id="bb04e-122">The in-memory and distributed cache store cache items as key-value pairs.</span></span>

## <a name="systemruntimecachingmemorycache"></a><span data-ttu-id="bb04e-123">System.Runtime.Caching/MemoryCache</span><span class="sxs-lookup"><span data-stu-id="bb04e-123">System.Runtime.Caching/MemoryCache</span></span>

<span data-ttu-id="bb04e-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)) 可與搭配使用:</span><span class="sxs-lookup"><span data-stu-id="bb04e-124"><xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet package](https://www.nuget.org/packages/System.Runtime.Caching/)) can be used with:</span></span>

* <span data-ttu-id="bb04e-125">.NET Standard 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="bb04e-125">.NET Standard 2.0 or later.</span></span>
* <span data-ttu-id="bb04e-126">以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。</span><span class="sxs-lookup"><span data-stu-id="bb04e-126">Any [.NET implementation](/dotnet/standard/net-standard#net-implementation-support) that targets .NET Standard 2.0 or later.</span></span> <span data-ttu-id="bb04e-127">例如, ASP.NET Core 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="bb04e-127">For example, ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="bb04e-128">.NET Framework 4.5 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="bb04e-128">.NET Framework 4.5 or later.</span></span>

<span data-ttu-id="bb04e-129">/建議使用`IMemoryCache` [MicrosoftExtensions`MemoryCache` . Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) (如本文所述), 因為它已更緊密整合到 ASP.NET Core 中。`System.Runtime.Caching` /</span><span class="sxs-lookup"><span data-stu-id="bb04e-129">[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)/`IMemoryCache` (described in this article) is recommended over `System.Runtime.Caching`/`MemoryCache` because it's better integrated into ASP.NET Core.</span></span> <span data-ttu-id="bb04e-130">例如, `IMemoryCache`會以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="bb04e-130">For example, `IMemoryCache` works natively with ASP.NET Core [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="bb04e-131">將`System.Runtime.Caching`程式碼從 ASP.NET 4.x 移植到 ASP.NET Core 時, 請使用/ `MemoryCache`做為相容性橋接器。</span><span class="sxs-lookup"><span data-stu-id="bb04e-131">Use `System.Runtime.Caching`/`MemoryCache` as a compatibility bridge when porting code from ASP.NET 4.x to ASP.NET Core.</span></span>

## <a name="cache-guidelines"></a><span data-ttu-id="bb04e-132">快取指導方針</span><span class="sxs-lookup"><span data-stu-id="bb04e-132">Cache guidelines</span></span>

* <span data-ttu-id="bb04e-133">程式碼應該一律要有可用於擷取資料的後援選項，而**不**應該依賴快取的值。</span><span class="sxs-lookup"><span data-stu-id="bb04e-133">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="bb04e-134">快取使用不足的資源, 記憶體。</span><span class="sxs-lookup"><span data-stu-id="bb04e-134">The cache uses a scarce resource, memory.</span></span> <span data-ttu-id="bb04e-135">限制快取成長:</span><span class="sxs-lookup"><span data-stu-id="bb04e-135">Limit cache growth:</span></span>
  * <span data-ttu-id="bb04e-136">請**勿**使用外部輸入作為快取索引鍵。</span><span class="sxs-lookup"><span data-stu-id="bb04e-136">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="bb04e-137">使用到期時間來限制快取成長。</span><span class="sxs-lookup"><span data-stu-id="bb04e-137">Use expirations to limit cache growth.</span></span>
  * <span data-ttu-id="bb04e-138">[使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-138">[Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span> <span data-ttu-id="bb04e-139">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-139">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="bb04e-140">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-140">It's up to the developer to limit cache size.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="bb04e-141">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="bb04e-141">Using IMemoryCache</span></span>

> [!WARNING]
> <span data-ttu-id="bb04e-142">從相依性[插入](xref:fundamentals/dependency-injection)使用共用記憶體快取, `SetSize`並`Size`呼叫、 `SizeLimit`或來限制快取大小, 可能會導致應用程式失敗。</span><span class="sxs-lookup"><span data-stu-id="bb04e-142">Using a *shared* memory cache from [Dependency Injection](xref:fundamentals/dependency-injection) and calling `SetSize`, `Size`, or `SizeLimit` to limit cache size can cause the app to fail.</span></span> <span data-ttu-id="bb04e-143">在快取上設定大小限制時, 所有專案在新增時都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-143">When a size limit is set on a cache, all entries must specify a size when being added.</span></span> <span data-ttu-id="bb04e-144">這可能會導致問題, 因為開發人員可能無法完全控制使用共用快取的內容。</span><span class="sxs-lookup"><span data-stu-id="bb04e-144">This can lead to issues since developers may not have full control on what uses the shared cache.</span></span> <span data-ttu-id="bb04e-145">例如, Entity Framework Core 使用共用快取, 且未指定大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-145">For example, Entity Framework Core uses the shared cache and does not specify a size.</span></span> <span data-ttu-id="bb04e-146">如果應用程式設定快取大小限制, 並使用 EF Core, 應用程式`InvalidOperationException`會擲回。</span><span class="sxs-lookup"><span data-stu-id="bb04e-146">If an app sets a cache size limit and uses EF Core, the app throws an `InvalidOperationException`.</span></span>
> <span data-ttu-id="bb04e-147">使用`SetSize`、 `Size`或來限制快取時,請建立單一快取來進行快取。`SizeLimit`</span><span class="sxs-lookup"><span data-stu-id="bb04e-147">When using `SetSize`, `Size`, or `SizeLimit` to limit cache, create a cache singleton for caching.</span></span> <span data-ttu-id="bb04e-148">如需詳細資訊和範例, 請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-148">For more information and an example, see [Use SetSize, Size, and SizeLimit to limit cache size](#use-setsize-size-and-sizelimit-to-limit-cache-size).</span></span>

<span data-ttu-id="bb04e-149">記憶體內部快取是使用相依性插入從您的應用程式參考的一[項](xref:fundamentals/dependency-injection)*服務*。</span><span class="sxs-lookup"><span data-stu-id="bb04e-149">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bb04e-150">在`AddMemoryCache` 中`ConfigureServices`呼叫:</span><span class="sxs-lookup"><span data-stu-id="bb04e-150">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="bb04e-151">在此函數中要求實例:`IMemoryCache`</span><span class="sxs-lookup"><span data-stu-id="bb04e-151">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bb04e-152">`IMemoryCache`需要 NuGet 套件[Microsoft Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。</span><span class="sxs-lookup"><span data-stu-id="bb04e-152">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb04e-153">`IMemoryCache`需要 AspNetCore 的[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)NuGet 套件。[[Microsoft.AspNetCore 所有中繼套件](xref:fundamentals/metapackage)] 中都有提供。</span><span class="sxs-lookup"><span data-stu-id="bb04e-153">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="bb04e-154">`IMemoryCache`需要 NuGet 套件 AspNetCore。您可以在[中繼套件](xref:fundamentals/metapackage-app)中找到該[儲存體](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。</span><span class="sxs-lookup"><span data-stu-id="bb04e-154">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="bb04e-155">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。</span><span class="sxs-lookup"><span data-stu-id="bb04e-155">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="bb04e-156">如果沒有快取時間, 則會建立新的專案, 並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。</span><span class="sxs-lookup"><span data-stu-id="bb04e-156">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="bb04e-157">此時會顯示目前的時間和快取時間:</span><span class="sxs-lookup"><span data-stu-id="bb04e-157">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="bb04e-158">當超時時間內有要求時, 快取的值會保留在快取中。`DateTime`</span><span class="sxs-lookup"><span data-stu-id="bb04e-158">The cached `DateTime` value remains in the cache while there are requests within the timeout period.</span></span> <span data-ttu-id="bb04e-159">下圖顯示從快取中抓取的目前時間和較舊的時間:</span><span class="sxs-lookup"><span data-stu-id="bb04e-159">The following image shows the current time and an older time retrieved from the cache:</span></span>

![顯示兩個不同時間的索引視圖](memory/_static/time.png)

<span data-ttu-id="bb04e-161">下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。</span><span class="sxs-lookup"><span data-stu-id="bb04e-161">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="bb04e-162">下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間:</span><span class="sxs-lookup"><span data-stu-id="bb04e-162">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="bb04e-163"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>和[Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)是[CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>類別的擴充方法部分, 可延伸的功能。</span><span class="sxs-lookup"><span data-stu-id="bb04e-163"><xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*> , <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, and [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) are extension methods part of the [CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) class that extends the capability of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>.</span></span> <span data-ttu-id="bb04e-164">如需其他快取方法的說明, 請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)。</span><span class="sxs-lookup"><span data-stu-id="bb04e-164">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of other cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="bb04e-165">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="bb04e-165">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="bb04e-166">下列範例:</span><span class="sxs-lookup"><span data-stu-id="bb04e-166">The following sample:</span></span>

* <span data-ttu-id="bb04e-167">設定絕對到期時間。</span><span class="sxs-lookup"><span data-stu-id="bb04e-167">Sets the absolute expiration time.</span></span> <span data-ttu-id="bb04e-168">這是可快取專案的最長時間, 並防止當滑動到期時間持續更新時, 專案變得太過時。</span><span class="sxs-lookup"><span data-stu-id="bb04e-168">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
* <span data-ttu-id="bb04e-169">設定滑動到期時間。</span><span class="sxs-lookup"><span data-stu-id="bb04e-169">Sets a sliding expiration time.</span></span> <span data-ttu-id="bb04e-170">存取此快取專案的要求將會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="bb04e-170">Requests that access this cached item will reset the sliding expiration clock.</span></span>
* <span data-ttu-id="bb04e-171">將快取優先順序設定`CacheItemPriority.NeverRemove`為。</span><span class="sxs-lookup"><span data-stu-id="bb04e-171">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
* <span data-ttu-id="bb04e-172">設定在從快取中收回專案之後, 將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。</span><span class="sxs-lookup"><span data-stu-id="bb04e-172">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="bb04e-173">回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="bb04e-173">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="bb04e-174">使用 SetSize、Size 和 SizeLimit 來限制快取大小</span><span class="sxs-lookup"><span data-stu-id="bb04e-174">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="bb04e-175">`MemoryCache`實例可以選擇性地指定並強制執行大小限制。</span><span class="sxs-lookup"><span data-stu-id="bb04e-175">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="bb04e-176">記憶體大小限制沒有定義的測量單位, 因為快取沒有測量專案大小的機制。</span><span class="sxs-lookup"><span data-stu-id="bb04e-176">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="bb04e-177">如果已設定快取記憶體大小限制, 所有專案都必須指定大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-177">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="bb04e-178">ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-178">The ASP.NET Core runtime does not limit cache size based on memory pressure.</span></span> <span data-ttu-id="bb04e-179">開發人員需要限制快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-179">It's up to the developer to limit cache size.</span></span> <span data-ttu-id="bb04e-180">指定的大小是開發人員選擇的單位。</span><span class="sxs-lookup"><span data-stu-id="bb04e-180">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="bb04e-181">例如:</span><span class="sxs-lookup"><span data-stu-id="bb04e-181">For example:</span></span>

* <span data-ttu-id="bb04e-182">如果 web 應用程式主要是快取字串, 則每個快取專案大小都可以是字串長度。</span><span class="sxs-lookup"><span data-stu-id="bb04e-182">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="bb04e-183">應用程式可以將所有專案的大小指定為 1, 而大小限制是專案的計數。</span><span class="sxs-lookup"><span data-stu-id="bb04e-183">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="bb04e-184">下列程式碼會建立可透過相依性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小[MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) :</span><span class="sxs-lookup"><span data-stu-id="bb04e-184">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="bb04e-185">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit)沒有單位。</span><span class="sxs-lookup"><span data-stu-id="bb04e-185">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="bb04e-186">如果已設定快取記憶體大小, 則快取的專案必須以其認為最適合的任何單位來指定大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-186">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="bb04e-187">快取實例的所有使用者都應使用相同的單位系統。</span><span class="sxs-lookup"><span data-stu-id="bb04e-187">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="bb04e-188">如果快取的專案大小總和超過所指定`SizeLimit`的值, 則不會快取專案。</span><span class="sxs-lookup"><span data-stu-id="bb04e-188">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="bb04e-189">如果未設定快取大小限制, 則會忽略在該專案上設定的快取大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-189">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="bb04e-190">下列程式碼會`MyMemoryCache`向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。</span><span class="sxs-lookup"><span data-stu-id="bb04e-190">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="bb04e-191">`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取, 並知道如何適當地設定快取專案大小。</span><span class="sxs-lookup"><span data-stu-id="bb04e-191">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="bb04e-192">下列程式碼會`MyMemoryCache`使用:</span><span class="sxs-lookup"><span data-stu-id="bb04e-192">The following code uses `MyMemoryCache`:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

<span data-ttu-id="bb04e-193">快取專案的大小可以透過[大小](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size)或[SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)擴充方法來設定:</span><span class="sxs-lookup"><span data-stu-id="bb04e-193">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="bb04e-194">快取相依性</span><span class="sxs-lookup"><span data-stu-id="bb04e-194">Cache dependencies</span></span>

<span data-ttu-id="bb04e-195">下列範例示範如何在相依項目過期時將快取項目設定為過期。</span><span class="sxs-lookup"><span data-stu-id="bb04e-195">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="bb04e-196">`CancellationChangeToken` 會新增至快取的項目。</span><span class="sxs-lookup"><span data-stu-id="bb04e-196">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="bb04e-197">當在 `CancellationTokenSource` 上呼叫 `Cancel` 時，會撤出兩個快取項目。</span><span class="sxs-lookup"><span data-stu-id="bb04e-197">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="bb04e-198">`CancellationTokenSource`使用可讓多個快取專案收回為群組。</span><span class="sxs-lookup"><span data-stu-id="bb04e-198">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="bb04e-199">使用上述`using`程式碼中的模式時, 在`using`區塊內建立的快取專案將會繼承觸發程式和到期設定。</span><span class="sxs-lookup"><span data-stu-id="bb04e-199">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="bb04e-200">其他備註</span><span class="sxs-lookup"><span data-stu-id="bb04e-200">Additional notes</span></span>

* <span data-ttu-id="bb04e-201">使用回呼來重新擴展快取專案時:</span><span class="sxs-lookup"><span data-stu-id="bb04e-201">When using a callback to repopulate a cache item:</span></span>

  * <span data-ttu-id="bb04e-202">因為回呼尚未完成, 所以多個要求可以將快取的金鑰值空白。</span><span class="sxs-lookup"><span data-stu-id="bb04e-202">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  * <span data-ttu-id="bb04e-203">這可能會導致數個執行緒重新填入快取的專案。</span><span class="sxs-lookup"><span data-stu-id="bb04e-203">This can result in several threads repopulating the cached item.</span></span>

* <span data-ttu-id="bb04e-204">當有一個快取專案用來建立另一個時, 子系會複製父系專案的到期權杖和以時間為基礎的到期設定。</span><span class="sxs-lookup"><span data-stu-id="bb04e-204">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="bb04e-205">子系不會因為手動移除或更新父專案而過期。</span><span class="sxs-lookup"><span data-stu-id="bb04e-205">The child isn't expired by manual removal or updating of the parent entry.</span></span>

* <span data-ttu-id="bb04e-206">使用[PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks)設定在從快取中收回快取專案之後, 將會引發的回呼。</span><span class="sxs-lookup"><span data-stu-id="bb04e-206">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb04e-207">其他資源</span><span class="sxs-lookup"><span data-stu-id="bb04e-207">Additional resources</span></span>

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
