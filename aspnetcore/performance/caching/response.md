---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/04/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/caching/response
ms.openlocfilehash: 87ff2633ded612eba2c996583b4a6cf997fe8e18
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84105762"
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core 中的回應快取

作者： [John 羅文](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Steve Smith](https://ardalis.com/)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples)（[如何下載](xref:index#how-to-download-a-sample)）

回應快取可減少用戶端或 proxy 對 web 伺服器提出的要求數目。 回應快取也會減少 web 伺服器執行以產生回應的工作量。 回應快取是由標頭控制，可指定您希望用戶端、proxy 和中介軟體快取回應的方式。

[ResponseCache 屬性](#responsecache-attribute)會參與設定回應快取標頭。 用戶端和中繼 proxy 應接受[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格底下快取回應的標頭。

若為遵循 HTTP 1.1 快取規格的伺服器端快取，請使用回應快取[中介軟體](xref:performance/caching/middleware)。 中介軟體可以使用 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 屬性來影響伺服器端快取行為。

## <a name="http-based-response-caching"></a>以 HTTP 為基礎的回應快取

[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格描述網際網路快取的行為。 用於快取的主要 HTTP 標頭是快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)，用來指定快取指示*詞。* 指示詞會控制快取行為，因為要求會從用戶端傳送到伺服器，而回應會從伺服器傳回用戶端。 要求和回應會在 proxy 伺服器上移動，而 proxy 伺服器也必須符合 HTTP 1.1 快取規格。

通用指示詞如下 `Cache-Control` 表所示。

| 指示詞                                                       | 動作 |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | 快取可能會儲存回應。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 回應不得由共用快取儲存。 私用快取可能會儲存並重複使用回應。 |
| [最大壽命](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | 用戶端不接受其年齡大於指定秒數的回應。 範例： `max-age=60` （60秒）、 `max-age=2592000` （1個月） |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **在要求上**：快取不得使用預存回應來滿足要求。 源伺服器會重新產生用戶端的回應，中介軟體會在其快取中更新儲存的回應。<br><br>**回應時**：回應不得用於後續要求，而不需在源伺服器上進行驗證。 |
| [否-存放區](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **在要求上**：快取不得儲存要求。<br><br>**回應時**：快取不得儲存回應的任何部分。 |

下表顯示在快取中扮演角色的其他快取標頭。

| Header                                                     | 函式 |
| ---------------------------------------------------------- | -------- |
| [存在](https://tools.ietf.org/html/rfc7234#section-5.1)     | 在源伺服器上產生或成功驗證回應後的時間量估計（以秒為單位）。 |
| [失效](https://tools.ietf.org/html/rfc7234#section-5.3) | 回應被視為過時的時間。 |
| [雜](https://tools.ietf.org/html/rfc7234#section-5.4)  | 存在於與 HTTP/1.0 快取的回溯相容性，以進行設定 `no-cache` 行為。 如果 `Cache-Control` 標頭存在，則 `Pragma` 會忽略標頭。 |
| [相同](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定不應傳送快取的回應，除非所有 `Vary` 標頭欄位都符合快取回應的原始要求和新的要求。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>以 HTTP 為基礎的快取會遵循要求快取控制指示詞

快取[控制標頭的 HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。 用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。

如果您考慮 HTTP 快取的目標，一律接受用戶端 `Cache-Control` 要求標頭是合理的。 在官方規格中，快取的目的是要降低跨用戶端、proxy 和伺服器網路上滿足要求的延遲和網路負擔。 這不一定是控制源伺服器負載的方式。

使用回應快取[中介軟體](xref:performance/caching/middleware)時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。 [中介軟體的規劃增強功能](https://github.com/dotnet/AspNetCore/issues/2612)，是在決定提供快取回應時，設定中介軟體忽略要求 `Cache-Control` 標頭的機會。 規劃的增強功能可讓您有機會更妥善控制伺服器負載。

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core 中的其他快取技術

### <a name="in-memory-caching"></a>記憶體內部快取

記憶體內部快取會使用伺服器記憶體來儲存快取的資料。 這種類型的快取適用于單一伺服器，或使用*粘滯會話*的多部伺服器。 「粘滯話」表示用戶端的要求一律會路由傳送至相同的伺服器進行處理。

如需詳細資訊，請參閱 <xref:performance/caching/memory> 。

### <a name="distributed-cache"></a>分散式快取

當應用程式裝載于雲端或伺服器陣列時，使用分散式快取將資料儲存在記憶體中。 快取會在處理要求的伺服器之間共用。 如果用戶端的快取資料可供使用，則用戶端可以提交由群組中的任何伺服器所處理的要求。 ASP.NET Core 適用于 SQL Server、 [Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)和[NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)分散式快取。

如需詳細資訊，請參閱 <xref:performance/caching/distributed> 。

### <a name="cache-tag-helper"></a>快取標籤協助程式

使用快取標籤協助程式從 MVC 視圖或頁面快取內容 Razor 。 快取標記協助程式會使用記憶體內部快取來儲存資料。

如需詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper> 。

### <a name="distributed-cache-tag-helper"></a>分散式快取標籤協助程式

使用分散式快取標記協助程式 Razor ，從分散式雲端或 web 伺服陣列案例中的 MVC 視圖或頁面快取內容。 分散式快取標記協助程式會使用 SQL Server、 [Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)或[NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)來儲存資料。

如需詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper> 。

## <a name="responsecache-attribute"></a>ResponseCache 屬性

會 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 指定在回應快取中設定適當標頭所需的參數。

> [!WARNING]
> 針對包含已驗證用戶端資訊的內容停用快取。 應該只針對不會根據使用者身分識別或使用者是否已登入而變更的內容來啟用快取。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>依據指定的查詢索引鍵清單值，改變儲存的回應。 當提供單一值時 `*` ，中介軟體會依所有要求查詢字串參數來改變回應。

必須啟用回應快取[中介軟體](xref:performance/caching/middleware)才能設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 屬性。 否則，就會擲回執行時間例外狀況。 屬性沒有對應的 HTTP 標頭 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 。 屬性是由回應快取中介軟體所處理的 HTTP 功能。 若要讓中介軟體提供快取的回應，查詢字串和查詢字串值必須符合先前的要求。 例如，請考慮下表所示的要求和結果順序。

| 要求                          | 結果                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | 從伺服器傳回。 |
| `http://example.com?key1=value1` | 從中介軟體傳回。 |
| `http://example.com?key1=value2` | 從伺服器傳回。 |

第一個要求是由伺服器傳回，並在中介軟體中快取。 中介軟體會傳回第二個要求，因為查詢字串符合先前的要求。 第三個要求不在中介軟體快取中，因為查詢字串值不符合先前的要求。

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>是用來設定和建立（via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> ） `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter` 。 會 `ResponseCacheFilter` 執行更新適當 HTTP 標頭和回應功能的工作。 篩選準則：

* 移除、和的任何現有標頭 `Vary` `Cache-Control` `Pragma` 。
* 根據中設定的屬性寫出適當的標頭 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 。
* 如果已設定，則更新回應快取 HTTP 功能 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 。

### <a name="vary"></a>相同

只有在設定屬性時，才會寫入這個標頭 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 。 屬性會設定為 `Vary` 屬性的值。 下列範例會使用 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 屬性：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

使用範例應用程式，使用瀏覽器的網路工具來查看回應標頭。 下列回應標頭會與 Cache1 頁面回應一起傳送：

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a>NoStore 和 Location。無

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>覆寫大部分的其他屬性。 當這個屬性設定為時 `true` ， `Cache-Control` 標頭會設定為 `no-store` 。 如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 設定為 `None` ：

* `Cache-Control` 設定為 `no-store,no-cache`。
* `Pragma` 設定為 `no-cache`。

如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 是 `false` <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ，而且是 `None` 、和，則 `Cache-Control` `Pragma` 會設定為 `no-cache` 。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>對於錯誤網頁，通常會設定為 `true` 。 範例應用程式中的 [Cache2] 頁面會產生回應標頭，以指示用戶端不要儲存回應。

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

範例應用程式會傳回具有下列標頭的 Cache2 頁面：

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>位置和持續時間

若要啟用快取， <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 必須將設為正值，而且 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 必須是 `Any` （預設值）或 `Client` 。 架構會將 `Cache-Control` 標頭設定為位置值，後面接著 `max-age` 回應的。

<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>的選項 `Any` 和會 `Client` 分別轉譯為 `Cache-Control` 和的標頭值 `public` `private` 。 如[NoStore 和 Location. None](#nostore-and-locationnone)區段中所述，將設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 為 `None` `Cache-Control` ，並將 `Pragma` 標頭設定為 `no-cache` 。

`Location.Any`（ `Cache-Control` 設為 `public` ）表示*用戶端或任何中繼 proxy*可能會快取此值，包括回應快取[中介軟體](xref:performance/caching/middleware)。

`Location.Client`（ `Cache-Control` 設為 `private` ）表示*只有用戶端*可以快取此值。 任何中繼快取都不應該快取值，包括回應快取[中介軟體](xref:performance/caching/middleware)。

快取控制標頭只會在和如何快取回應時，提供用戶端和中繼 proxy 的指引。 不保證用戶端和 proxy 會接受[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格。 [回應快取中介軟體](xref:performance/caching/middleware)一律會遵循規格所配置的快取規則。

下列範例顯示來自範例應用程式的 Cache3 頁面模型，以及設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 和保留預設值所產生的標頭 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

範例應用程式會傳回具有下列標頭的 Cache3 頁面：

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a>快取設定檔

快取設定檔在中設定 MVC/Pages 時，可以設定為選項，而不是複製許多控制器動作屬性的回應快取設定 Razor `Startup.ConfigureServices` 。 在參考的快取設定檔中找到的值是用來做為的預設值 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> ，而且會由屬性上指定的任何屬性加以覆寫。

設定快取設定檔。 下列範例顯示範例應用程式中的30秒快取設定檔 `Startup.ConfigureServices` ：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

範例應用程式的 Cache4 頁面模型會參考快取 `Default30` 設定檔：

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>可套用至：

* Razor頁面處理常式（類別）：屬性無法套用至處理常式方法。
* MVC 控制器（類別）。
* MVC 動作（方法）：方法層級屬性會覆寫在類別層級屬性中指定的設定。

由快取設定檔套用至 Cache4 頁面回應的產生標頭 `Default30` ：

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a>其他資源

* [將回應儲存在快取中](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-控制項](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
