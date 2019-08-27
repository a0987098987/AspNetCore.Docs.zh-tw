---
title: ASP.NET Core 中的記憶體快取
author: rick-anderson
description: 了解如何快取 ASP.NET Core 中的資料和記憶體。
ms.author: riande
ms.custom: mvc
ms.date: 8/22/2019
uid: performance/caching/memory
ms.openlocfilehash: 1519abbca6430063f037372a4927f5818f160457
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024792"
---
# <a name="cache-in-memory-in-aspnet-core"></a>ASP.NET Core 中的記憶體快取

::: moniker range=">= aspnetcore-3.0"

作者: [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/3.0sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>快取基本概念

快取可以藉由減少產生內容所需的工作, 大幅改善應用程式的效能和擴充性。 快取最適用于不常變更**且**產生成本昂貴的資料。 快取會製作一份資料複本, 其傳回的速度會比從來源快得多。 應用程式應撰寫並測試為**永遠不會**依賴快取的資料。

ASP.NET Core 支援數種不同的快取。 最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎。 `IMemoryCache`代表儲存在 web 伺服器記憶體中的快取。 使用記憶體內部快取時, 在伺服器陣列 (多部伺服器) 上執行的應用程式應該確保會話是固定的。 [粘滯會話] 可確保來自用戶端的後續要求都會移至相同的伺服器。 例如, Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR), 將所有後續要求路由傳送至相同的伺服器。

Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取, 以避免快取一致性問題。 針對某些應用程式, 分散式快取可支援比記憶體內部快取更高的相應放大。 使用分散式快取會將快取記憶體卸載至外部進程。

記憶體內部快取可以儲存任何物件。 分散式快取介面僅限於`byte[]`。 記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)) 可與搭配使用:

* .NET Standard 2.0 或更新版本。
* 以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。 例如, ASP.NET Core 2.0 或更新版本。
* .NET Framework 4.5 或更新版本。

/建議使用`IMemoryCache` [MicrosoftExtensions`MemoryCache` . Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) (如本文所述), 因為它已更緊密整合到 ASP.NET Core 中。`System.Runtime.Caching` / 例如, `IMemoryCache`會以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。

將`System.Runtime.Caching`程式碼從 ASP.NET 4.x 移植到 ASP.NET Core 時, 請使用/ `MemoryCache`做為相容性橋接器。

## <a name="cache-guidelines"></a>快取指導方針

* 程式碼應該一律要有可用於擷取資料的後援選項，而**不**應該依賴快取的值。
* 快取使用不足的資源, 記憶體。 限制快取成長:
  * 請**勿**使用外部輸入作為快取索引鍵。
  * 使用到期時間來限制快取成長。
  * [使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。 ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。 開發人員需要限制快取大小。

## <a name="use-imemorycache"></a>使用 IMemoryCache

> [!WARNING]
> 從相依性[插入](xref:fundamentals/dependency-injection)使用共用記憶體快取, `SetSize`並`Size`呼叫、 `SizeLimit`或來限制快取大小, 可能會導致應用程式失敗。 在快取上設定大小限制時, 所有專案在新增時都必須指定大小。 這可能會導致問題, 因為開發人員可能無法完全控制使用共用快取的內容。 例如, Entity Framework Core 使用共用快取, 且未指定大小。 如果應用程式設定快取大小限制, 並使用 EF Core, 應用程式`InvalidOperationException`會擲回。
> 使用`SetSize`、 `Size`或來限制快取時,請建立單一快取來進行快取。`SizeLimit` 如需詳細資訊和範例, 請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。

記憶體內部快取是使用相依性[插入](xref:fundamentals/dependency-injection)從應用程式參考的*服務*。 在此函數中要求實例:`IMemoryCache`

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ctor)]

下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。 如果沒有快取時間, 則會建立新的專案, 並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。 `CacheKeys`類別是下載範例的一部分。

[! code-csharp [] (memory/3.0 sample/WebCacheSample/CacheKeys .cs) [](memory/3.0sample/WebCacheSample/CacheKeys.cs)]

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet1)]

此時會顯示目前的時間和快取時間:

[!code-cshtml[](memory/3.0sample/WebCacheSample/Views/Home/Cache.cshtml)]

當超時時間內有要求時, 快取的值會保留在快取中。`DateTime`

下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_gct)]

下列程式碼會取得或建立具有絕對過期的快取專案:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet99)]

只有具有滑動期限的快取專案集, 才會有過時的風險。 如果存取的頻率高於滑動到期時間間隔, 專案將永遠不會過期。 結合滑動期限與絕對到期日, 以保證專案在其絕對到期時間通過後到期。 絕對到期時間會設定專案可快取的時間上限, 但如果在滑動到期時間間隔內未要求, 仍然允許專案提早過期。 當同時指定絕對和滑動期限時, 會以邏輯方式 ORed 到期日。 如果滑動到期間隔*或*絕對到期時間經過, 則會從快取中收回專案。

下列程式碼會取得或建立具有滑動*和*絕對到期的快取專案:

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet9)]

上述程式碼可確保資料不會快取超過絕對時間。

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*> <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions>和<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.Get*>是類別中的擴充方法。 這些方法會擴充的功能<xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>。

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

下列範例:

* 設定滑動到期時間。 存取此快取專案的要求將會重設滑動到期時鐘。
* 將快取優先順序設定為[CacheItemPriority. NeverRemove](xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove)。
* 設定在從快取中收回專案之後, 將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。 回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>使用 SetSize、Size 和 SizeLimit 來限制快取大小

`MemoryCache`實例可以選擇性地指定並強制執行大小限制。 記憶體大小限制沒有定義的測量單位, 因為快取沒有測量專案大小的機制。 如果已設定快取記憶體大小限制, 所有專案都必須指定大小。 ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。 開發人員需要限制快取大小。 指定的大小是開發人員選擇的單位。

例如:

* 如果 web 應用程式主要是快取字串, 則每個快取專案大小都可以是字串長度。
* 應用程式可以將所有專案的大小指定為 1, 而大小限制是專案的計數。

如果<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit>未設定, 則快取會成長而不受限制。 當系統記憶體不足時, ASP.NET Core 執行時間不會修剪快取。 應用程式的架構大致如下:

* 限制快取成長。
* 當<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*>可用<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*>的記憶體受到限制時, 呼叫或:

下列程式碼會建立可透過相依<xref:Microsoft.Extensions.Caching.Memory.MemoryCache>性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小:

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit`沒有單位。 如果已設定快取記憶體大小, 則快取的專案必須以其認為最適合的任何單位來指定大小。 快取實例的所有使用者都應使用相同的單位系統。 如果快取的專案大小總和超過所指定`SizeLimit`的值, 則不會快取專案。 如果未設定快取大小限制, 則會忽略在該專案上設定的快取大小。

下列程式碼會`MyMemoryCache`向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。

[!code-csharp[](memory/3.0sample/RPcache/Startup.cs?name=snippet)]

`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取, 並知道如何適當地設定快取專案大小。

下列程式碼會`MyMemoryCache`使用:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet)]

快取專案的大小可以透過<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryOptions.Size> <xref:Microsoft.Extensions.Caching.Memory.MemoryCacheEntryExtensions.SetSize*>或擴充方法來設定:

[!code-csharp[](memory/3.0sample/RPcache/Pages/SetSize.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>MemoryCache Compact

`MemoryCache.Compact`嘗試依下列順序移除指定百分比的快取:

* 所有過期的專案。
* 依優先順序排序的專案。 系統會先移除最低優先順序專案。
* 最近最少使用的物件。
* 具有最早絕對到期的專案。
* 具有最早滑動到期日的專案。

具有優先順序<xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove>的固定專案永遠不會被移除。 下列程式碼會移除快取專案並`Compact`呼叫:

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

如需詳細資訊, 請參閱[GitHub 上的 Compact 來源](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393)。

## <a name="cache-dependencies"></a>快取相依性

下列範例示範如何在相依項目過期時將快取項目設定為過期。 `CancellationChangeToken` 會新增至快取的項目。 當在 `CancellationTokenSource` 上呼叫 `Cancel` 時，會撤出兩個快取項目。

[!code-csharp[](memory/3.0sample/WebCacheSample/Controllers/HomeController.cs?name=snippet_ed)]

`CancellationTokenSource`使用可讓多個快取專案收回為群組。 使用上述`using`程式碼中的模式時, 在`using`區塊內建立的快取專案將會繼承觸發程式和到期設定。

## <a name="additional-notes"></a>其他備註

* 到期不會在背景中發生。 沒有任何計時器會主動掃描快取中是否有過期的專案。 快取上的任何活動`Get`( `Set`、 `Remove`、) 都可以觸發過期專案的背景掃描。 `CancellationTokenSource` (`CancelAfter`) 上的計時器也會移除專案, 並觸發過期專案的掃描。 例如, 不是使用`SetAbsoluteExpiration(TimeSpan.FromHours(1))`, 而是針對註冊的權杖使用。 `CancellationTokenSource.CancelAfter(TimeSpan.FromHours(1))` 當此標記引發時, 它會立即移除該專案, 並引發收回回呼。 如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/Caching/issues/248)。
* 使用回呼來重新擴展快取專案時:

  * 因為回呼尚未完成, 所以多個要求可以將快取的金鑰值空白。
  * 這可能會導致數個執行緒重新填入快取的專案。

* 當有一個快取專案用來建立另一個時, 子系會複製父系專案的到期權杖和以時間為基礎的到期設定。 子系不會因為手動移除或更新父專案而過期。

* 用<xref:Microsoft.Extensions.Caching.Memory.ICacheEntry.PostEvictionCallbacks>來設定將在從快取中收回快取專案之後引發的回呼。
* 大部分的應用程式`IMemoryCache`都已啟用。 例如, 在中`AddMvc` `AddMvcCore().AddRazorViewEngine` `AddRazorPages` `AddControllersWithViews` `Add{Service}`呼叫、、、和許多其他方法, 會啟用`IMemoryCache`。 `ConfigureServices` 對於未呼叫上述`Add{Service}`其中一種方法的應用程式, 可能需要在中`ConfigureServices`呼叫<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddMemoryCache*> 。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<!-- This is the 2.1 version -->
作者: [Rick Anderson](https://twitter.com/RickAndMSFT)、 [John 羅文](https://github.com/JunTaoLuo)和[Steve Smith](https://ardalis.com/)

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>快取基本概念

快取可以藉由減少產生內容所需的工作, 大幅改善應用程式的效能和擴充性。 快取最適合不常變更的資料。 快取會製作一份資料複本, 其傳回的速度會比原始來源更快。 撰寫和測試程式碼時,**絕對不**會依賴快取的資料。

ASP.NET Core 支援數種不同的快取。 最簡單的快取是以[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)為基礎, 代表儲存在 web 伺服器記憶體中的快取。 在伺服器陣列 (多部伺服器) 上執行的應用程式應該在使用記憶體內部快取時, 確保會話是粘滯的。 [粘滯會話] 可確保之後從用戶端提出的要求全都會移至相同的伺服器。 例如, Azure Web apps 會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR), 將來自使用者代理程式的所有要求路由傳送至相同的伺服器。

Web 伺服陣列中的非粘滯話需要[分散式](distributed.md)快取, 以避免快取一致性問題。 針對某些應用程式, 分散式快取可支援比記憶體內部快取更高的相應放大。 使用分散式快取會將快取記憶體卸載至外部進程。

記憶體內部快取可以儲存任何物件。 分散式快取介面僅限於`byte[]`。 記憶體內部和分散式快取存放區會以索引鍵/值組的形式快取專案。

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)) 可與搭配使用:

* .NET Standard 2.0 或更新版本。
* 以 .NET Standard 2.0 或更新版本為目標的任何[.net 執行](/dotnet/standard/net-standard#net-implementation-support)。 例如, ASP.NET Core 2.0 或更新版本。
* .NET Framework 4.5 或更新版本。

/建議使用`IMemoryCache` [MicrosoftExtensions`MemoryCache` . Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) (如本文所述), 因為它已更緊密整合到 ASP.NET Core 中。`System.Runtime.Caching` / 例如, `IMemoryCache`會以原生方式使用 ASP.NET Core 相依性[插入](xref:fundamentals/dependency-injection)。

將`System.Runtime.Caching`程式碼從 ASP.NET 4.x 移植到 ASP.NET Core 時, 請使用/ `MemoryCache`做為相容性橋接器。

## <a name="cache-guidelines"></a>快取指導方針

* 程式碼應該一律要有可用於擷取資料的後援選項，而**不**應該依賴快取的值。
* 快取使用不足的資源, 記憶體。 限制快取成長:
  * 請**勿**使用外部輸入作為快取索引鍵。
  * 使用到期時間來限制快取成長。
  * [使用 SetSize、Size 和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。 ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。 開發人員需要限制快取大小。

## <a name="using-imemorycache"></a>使用 IMemoryCache

> [!WARNING]
> 從相依性[插入](xref:fundamentals/dependency-injection)使用共用記憶體快取, `SetSize`並`Size`呼叫、 `SizeLimit`或來限制快取大小, 可能會導致應用程式失敗。 在快取上設定大小限制時, 所有專案在新增時都必須指定大小。 這可能會導致問題, 因為開發人員可能無法完全控制使用共用快取的內容。 例如, Entity Framework Core 使用共用快取, 且未指定大小。 如果應用程式設定快取大小限制, 並使用 EF Core, 應用程式`InvalidOperationException`會擲回。
> 使用`SetSize`、 `Size`或來限制快取時,請建立單一快取來進行快取。`SizeLimit` 如需詳細資訊和範例, 請參閱[使用 SetSize、大小和 SizeLimit 來限制](#use-setsize-size-and-sizelimit-to-limit-cache-size)快取大小。

記憶體內部快取是使用相依性插入從您的應用程式參考的一[項](../../fundamentals/dependency-injection.md)*服務*。 在`AddMemoryCache` 中`ConfigureServices`呼叫:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

在此函數中要求實例:`IMemoryCache`

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

`IMemoryCache`需要 NuGet 套件 AspNetCore。您可以在[中繼套件](xref:fundamentals/metapackage-app)中找到該[儲存體](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。

下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查快取中是否有時間。 如果沒有快取時間, 則會建立新的專案, 並將其新增至已[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)的快取中。

[! code-csharp [] (memory/sample/WebCache/CacheKeys .cs) [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

此時會顯示目前的時間和快取時間:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

當超時時間內有要求時, 快取的值會保留在快取中。`DateTime` 下圖顯示從快取中抓取的目前時間和較舊的時間:

![顯示兩個不同時間的索引視圖](memory/_static/time.png)

下列程式碼會使用[getorcreate 設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)來快取資料。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

下列程式碼會呼叫 Get 來提取[快](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)取的時間:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>、 <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>和[Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)是[CacheExtensions](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>類別的擴充方法部分, 可延伸的功能。 如需其他快取方法的說明, 請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)。

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

下列範例:

* 設定滑動到期時間。 存取此快取專案的要求將會重設滑動到期時鐘。
* 將快取優先順序設定`CacheItemPriority.NeverRemove`為。
* 設定在從快取中收回專案之後, 將會呼叫的[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) 。 回呼會在與從快取中移除專案的程式碼不同的執行緒上執行。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>使用 SetSize、Size 和 SizeLimit 來限制快取大小

`MemoryCache`實例可以選擇性地指定並強制執行大小限制。 記憶體大小限制沒有定義的測量單位, 因為快取沒有測量專案大小的機制。 如果已設定快取記憶體大小限制, 所有專案都必須指定大小。 ASP.NET Core 執行時間不會根據記憶體壓力來限制快取大小。 開發人員需要限制快取大小。 指定的大小是開發人員選擇的單位。

例如:

* 如果 web 應用程式主要是快取字串, 則每個快取專案大小都可以是字串長度。
* 應用程式可以將所有專案的大小指定為 1, 而大小限制是專案的計數。

如果<xref:Microsoft.Extensions.Caching.Memory.MemoryCacheOptions.SizeLimit>未設定, 則快取會成長而不受限制。 當系統記憶體不足時, ASP.NET Core 執行時間不會修剪快取。 應用程式的架構大致如下:

* 限制快取成長。
* 當<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Compact*>可用<xref:Microsoft.Extensions.Caching.Memory.MemoryCache.Remove*>的記憶體受到限制時, 呼叫或:

下列程式碼會建立可透過相依<xref:Microsoft.Extensions.Caching.Memory.MemoryCache>性[插入](xref:fundamentals/dependency-injection)來存取的無單位固定大小:

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

`SizeLimit`沒有單位。 如果已設定快取記憶體大小, 則快取的專案必須以其認為最適合的任何單位來指定大小。 快取實例的所有使用者都應使用相同的單位系統。 如果快取的專案大小總和超過所指定`SizeLimit`的值, 則不會快取專案。 如果未設定快取大小限制, 則會忽略在該專案上設定的快取大小。

下列程式碼會`MyMemoryCache`向相依性[插入](xref:fundamentals/dependency-injection)容器註冊。

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`會建立為可感知此大小限制快取之元件的獨立記憶體快取, 並知道如何適當地設定快取專案大小。

下列程式碼會`MyMemoryCache`使用:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

快取專案的大小可以透過[大小](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size)或[SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)擴充方法來設定:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

### <a name="memorycachecompact"></a>MemoryCache Compact

`MemoryCache.Compact`嘗試依下列順序移除指定百分比的快取:

* 所有過期的專案。
* 依優先順序排序的專案。 系統會先移除最低優先順序專案。
* 最近最少使用的物件。
* 具有最早絕對到期的專案。
* 具有最早滑動到期日的專案。

具有優先順序<xref:Microsoft.Extensions.Caching.Memory.CacheItemPriority.NeverRemove>的固定專案永遠不會被移除。

[!code-csharp[](memory/3.0sample/RPcache/Pages/TestCache.cshtml.cs?name=snippet3)]

如需詳細資訊, 請參閱[GitHub 上的 Compact 來源](https://github.com/aspnet/Extensions/blob/v3.0.0-preview8.19405.4/src/Caching/Memory/src/MemoryCache.cs#L382-L393)。

## <a name="cache-dependencies"></a>快取相依性

下列範例示範如何在相依項目過期時將快取項目設定為過期。 `CancellationChangeToken` 會新增至快取的項目。 當在 `CancellationTokenSource` 上呼叫 `Cancel` 時，會撤出兩個快取項目。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

`CancellationTokenSource`使用可讓多個快取專案收回為群組。 使用上述`using`程式碼中的模式時, 在`using`區塊內建立的快取專案將會繼承觸發程式和到期設定。

## <a name="additional-notes"></a>其他備註

* 使用回呼來重新擴展快取專案時:

  * 因為回呼尚未完成, 所以多個要求可以將快取的金鑰值空白。
  * 這可能會導致數個執行緒重新填入快取的專案。

* 當有一個快取專案用來建立另一個時, 子系會複製父系專案的到期權杖和以時間為基礎的到期設定。 子系不會因為手動移除或更新父專案而過期。

* 使用[PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks)設定在從快取中收回快取專案之後, 將會引發的回呼。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
