---
title: 快取在記憶體中的 ASP.NET Core
author: rick-anderson
description: 了解如何在 ASP.NET Core 中的記憶體中的資料快取。
ms.author: riande
ms.custom: mvc
ms.date: 09/15/2018
uid: performance/caching/memory
ms.openlocfilehash: be2e81d1aa6a89d65414d53a70ca2d2fb5d2d3a3
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477185"
---
# <a name="cache-in-memory-in-aspnet-core"></a>快取在記憶體中的 ASP.NET Core

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)， [John Luo](https://github.com/JunTaoLuo)，和[Steve Smith](https://ardalis.com/)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>快取的基本概念

快取可大幅改善的效能和延展性的應用程式藉由減少產生的內容所需的工作。 快取的運作最佳不常變更的資料。 快取會建立一份可以傳回大部分的資料比原始來源的更快。 您應該撰寫並測試您的應用程式永遠不會相依於快取的資料。

ASP.NET Core 支援數個不同的快取。 最簡單的快取為基礎[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)，代表儲存在 web 伺服器的記憶體中快取。 在多部伺服器的伺服器陣列執行的應用程式應該確定使用記憶體中快取時，會自黏工作階段。 黏性工作階段中，請確定所有用戶端的後續要求，請移至相同的伺服器。 例如，Azure Web 應用程式會使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR)，所有後續的要求路由至相同的伺服器。

在 web 伺服陣列中的非黏性工作階段需要[分散式快取](distributed.md)若要避免快取一致性問題。 對於某些應用程式中，分散式快取可以支援更高版本向外延展比記憶體中快取。 使用分散式快取卸載到外部處理序快取記憶體。

::: moniker range="< aspnetcore-2.0"

`IMemoryCache`快取將會收回快取項目，記憶體不足的壓力，除非[快取的優先順序](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority)設定為`CacheItemPriority.NeverRemove`。 您可以設定`CacheItemPriority`調整與快取收回記憶體不足壓力下的項目優先順序。

::: moniker-end

記憶體中快取可以儲存任何物件;分散式快取介面僅限於`byte[]`。

## <a name="systemruntimecachingmemorycache"></a>System.Runtime.Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache> ([NuGet 套件](https://www.nuget.org/packages/System.Runtime.Caching/)) 適用於：

* .NET standard 2.0 或更新版本。
* 任何[.NET 實作](/dotnet/standard/net-standard#net-implementation-support)為目標的.NET Standard 2.0 或更新版本。 例如，ASP.NET Core 2.0 或更新版本。
* .NET framework 4.5 或更新版本。

[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` （如本主題所述） 建議`System.Runtime.Caching` / `MemoryCache`因為深入整合到 ASP.NET Core。 例如，`IMemoryCache`適用於原生 ASP.NET Core[相依性插入](xref:fundamentals/dependency-injection)。

使用`System.Runtime.Caching` / `MemoryCache`橋樑相容性移植程式碼，從 ASP.NET 4.x 到 ASP.NET Core。

## <a name="cache-guidelines"></a>快取指引

* 程式碼，應一律具有擷取資料的後援選項及**不**取決於快取的值，可供使用。
* 快取佔用很少的資源時，記憶體。 限制快取增長：
  * 請勿**不**外部輸入做為快取索引鍵。
  * 您可以使用到期時間，限制快取成長。
  * [使用 SetSize、 大小和 SizeLimit 限制快取大小](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a>使用 IMemoryCache

記憶體中快取*服務*從您的應用程式使用參考[相依性插入](../../fundamentals/dependency-injection.md)。 呼叫`AddMemoryCache`在`ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

要求`IMemoryCache`建構函式的執行個體：

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` 需要 NuGet 套件[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` 需要 NuGet 套件[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)，這是用於[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)。

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` 需要 NuGet 套件[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)，這是用於[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

::: moniker-end

下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查一次是否快取中。 如果不快取的時間，建立並使用快取中加入新項目[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)。

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

會顯示目前的時間和快取的時間：

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

快取`DateTime`內逾時期限 （和任何因記憶體壓力而收回） 要求時的值會保留在快取。 下圖顯示目前的時間和較舊的時間，從快取擷取：

![索引檢視，其中顯示兩個不同的時間](memory/_static/time.png)

下列程式碼會使用[GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)並[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)快取資料。 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

下列程式碼會呼叫[取得](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)擷取快取的時間：

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)並[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)如需快取方法的描述。

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

下列範例：

- 設定絕對到期時間。 這是最長的時間可以快取項目，並防止項目時，會持續更新滑動期限變得太過時。
- 設定滑動期限。 要求存取此快取的項目會重設滑動的到期時鐘。
- 若要設定快取優先權`CacheItemPriority.NeverRemove`。
- 設定組[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) ，之後就會呼叫從快取收回項目的。 從快取中移除的項目程式碼在不同執行緒上執行的回呼。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>使用 SetSize、 大小和 SizeLimit 限制快取大小

A`MemoryCache`執行個體可能會選擇性地指定並強制執行大小限制。 因為快取有沒有機制可測量的項目大小，則記憶體大小限制並沒有定義的測量單位。 如果設定的快取記憶體大小限制，所有項目必須指定大小。 指定的大小是以開發人員選擇的單位。

例如: 

* 如果 web 應用程式的主要快取字串，每個快取項目大小可能是字串的長度。
* 應用程式可以指定成 1 的所有項目的大小和大小上限是項目計數。

下列程式碼建立 unitless 固定大小[MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1)供[相依性插入](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit)沒有單位。 快取項目必須以指定其認定執行快取記憶體大小已設定的最適合任何單位。 快取執行個體的所有使用者都應該都使用相同單位的系統。 將快取項目，如果快取項目大小的總和超過所指定的值`SizeLimit`。 如果沒有快取大小上限設定，將會忽略項目上設定的快取大小。

下列程式碼的暫存器`MyMemoryCache`具有[相依性插入](xref:fundamentals/dependency-injection)容器。

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` 會建立為獨立的記憶體內部快取，了解此大小限制快取，並了解如何適當地設定快取項目大小的元件。

下列程式碼會使用`MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

可以設定快取項目的大小[大小](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size)或[SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_)擴充方法：

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>快取相依性

下列範例示範如何過期的快取項目，如果相依項目到期。 A`CancellationChangeToken`新增至快取的項目。 當`Cancel`上呼叫`CancellationTokenSource`，收回這兩個快取項目。

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

使用`CancellationTokenSource`可讓多個快取項目被收回做為群組。 具有`using`上述程式碼中的模式，在建立快取項目`using`區塊會在觸發程序和到期日設定繼承。

## <a name="additional-notes"></a>其他備註

- 當使用回呼，以便重新填入快取項目：

  - 多個要求可以找到快取索引鍵的值空白因為尚未完成的回呼。
  - 這會導致數個執行緒重新填入快取的項目。

- 以建立另一個使用一個快取項目時，父項目的到期的權杖和以時間為基礎的到期日設定，也會複製子系。 手動移除已到期或更新的父項目，則不是子系。

- 使用[PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks)設定從快取收回快取項目之後會引發的回呼。

## <a name="additional-resources"></a>其他資源

* [使用分散式快取](xref:performance/caching/distributed)
* [使用變更權杖來偵測變更](xref:fundamentals/change-tokens)
* [回應快取](xref:performance/caching/response)
* [回應快取中介軟體](xref:performance/caching/middleware)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
