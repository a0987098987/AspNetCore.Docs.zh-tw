---
title: "記憶體中快取中 ASP.NET Core"
author: rick-anderson
description: "了解如何快取在記憶體中 ASP.NET Core 的資料。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: 8256240b46873d53bf1a6f6616ea5b520cfadf2e
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/03/2018
---
# <a name="in-memory-caching-in-aspnet-core"></a>記憶體中快取中 ASP.NET Core

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [John Luo](https://github.com/JunTaoLuo)，和[Steve Smith](https://ardalis.com/)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>快取的基本概念

快取可大幅改善的效能和延展性的應用程式藉由減少產生內容所需的工作。 快取就可以最適合不常變更的資料。 快取會建立一份可以傳回大部分的資料較快，從原始來源。 您應該撰寫並測試您的應用程式永遠不會取決於快取的資料。

ASP.NET Core 支援數個不同的快取。 最簡單的快取根據[IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache)，代表儲存在 web 伺服器的記憶體中快取。 在多部伺服器的伺服器陣列執行的應用程式應該確保使用記憶體中快取時，會自黏工作階段。 自黏工作階段會確保後續所有用戶端的要求移到相同的伺服器。 例如，Azure Web 應用程式使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR) 會將所有的後續要求路由至相同的伺服器。

Web 伺服陣列中的非黏性工作階段需要[分散式快取](distributed.md)以避免快取一致性問題。 對於某些應用程式中，分散式快取可以支援更高範圍外比記憶體中快取。 使用分散式快取卸載到外部處理序的快取記憶體。 

`IMemoryCache`快取將會收回快取記憶體不足壓力下的項目，除非[快取優先順序](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority)設`CacheItemPriority.NeverRemove`。 您可以設定`CacheItemPriority`調整優先順序快取收回記憶體不足壓力下的項目。

記憶體中快取可以儲存任何物件。分散式快取介面僅限於`byte[]`。

## <a name="using-imemorycache"></a>使用 IMemoryCache

記憶體中快取是*服務*參考從您的應用程式使用[相依性插入](../../fundamentals/dependency-injection.md)。 呼叫`AddMemoryCache`中`ConfigureServices`:

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

要求`IMemoryCache`建構函式中的執行個體：

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-999)] 

`IMemoryCache`需要 NuGet 套件 」 Microsoft.Extensions.Caching.Memory"。

下列程式碼會使用[TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)來檢查目前是否為快取中。 如果不快取項目，建立並與快取中加入新項目[設定](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_)。

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

會顯示目前的時間和快取的時間：

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

快取`DateTime`值將會留在快取，而沒有要求的逾時期限 （及任何因記憶體不足的壓力而收回） 內。 下圖顯示目前的時間和時間早從快取擷取：

![具有兩個不同的時間顯示的索引檢視](memory/_static/time.png)

下列程式碼會使用[GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)快取資料。 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

下列程式碼會呼叫[取得](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)來擷取快取的時間：

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

請參閱[IMemoryCache 方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions)的快取方法的描述。

## <a name="using-memorycacheentryoptions"></a>使用 MemoryCacheEntryOptions

下列範例：

- 設定絕對到期時間。 這是可以快取之項目的時間上限，並防止持續更新變動到期後變成過時的項目。
- 設定滑動期限。 要求存取此快取的項目會重設滑動到期時鐘。
- 若要設定快取優先權`CacheItemPriority.NeverRemove`。 
- 設定[PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) ，將會在從快取收回項目之後呼叫。 回呼的程式碼快取中移除的項目與另一個執行緒上執行。

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>快取相依性

下列範例會示範如何過期的快取項目，如果相依項目到期為止。 A`CancellationChangeToken`新增至快取的項目。 當`Cancel`上呼叫`CancellationTokenSource`，收回兩個快取項目。 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

使用`CancellationTokenSource`可讓多個快取項目被收回為群組。 與`using`上述程式碼模式，快取項目內建立`using`區塊將會繼承觸發程序和到期日設定。

## <a name="additional-notes"></a>其他備註

- 使用時回呼重新擴展快取項目：

  - 多個要求可以找到的快取索引鍵值空因為尚未完成的回呼。 
  - 這會導致多個執行緒重新填入快取的項目。

- 若要建立另一個使用一個快取項目時，子將複製的父項目到期語彙基元和以時間為基礎的到期日設定。 子系不過期手動移除或更新的父項目。

## <a name="additional-resources"></a>其他資源

* [使用分散式快取](xref:performance/caching/distributed)
* [使用變更權杖來偵測變更](xref:fundamentals/primitives/change-tokens)
* [回應快取](xref:performance/caching/response)
* [回應快取中介軟體](xref:performance/caching/middleware)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
