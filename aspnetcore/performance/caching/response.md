---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098944"
---
# <a name="response-caching-in-aspnet-core"></a>ASP.NET Core 中的回應快取

藉由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)

> [!NOTE]
> 回應快取中的 Razor 頁面是適用於 ASP.NET Core 2.1 或更新版本。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

回應快取可減少用戶端或 proxy 會向 web 伺服器提出的要求數目。 回應快取也可以減少 web 伺服器執行產生回應的工作。 回應快取是由指定您想用戶端、 proxy 和中的介軟體來快取回應標頭來控制。

[ResponseCache 屬性](#responsecache-attribute)參與設定快取標頭，快取的回應時，可能會接受用戶端的回應。 [回應快取中介軟體](xref:performance/caching/middleware)可以用來在伺服器上的快取回應。 中介軟體可以使用`ResponseCache`屬性來影響伺服器端快取行為的內容。

## <a name="http-based-response-caching"></a>以 HTTP 為基礎的回應快取

[快取的 HTTP 1.1 規格](https://tools.ietf.org/html/rfc7234)說明網際網路快取的行為方式。 主要用於快取的 HTTP 標頭[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)，這用來指定快取*指示詞*。 要求對它們的方式從用戶端伺服器內容和回應給用戶端從伺服器進行的途中，指示詞會控制快取行為。 要求和回應移動透過 proxy 伺服器和 proxy 伺服器也必須符合 HTTP 1.1 快取規格。

常見`Cache-Control`指示詞會顯示下表中。

| 指示詞                                                       | 動作 |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | 快取可以儲存回應。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 回應不能儲存的共用快取。 私用快取可以儲存和重複使用的回應。 |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | 用戶端不會接受的回應其年齡大於指定的秒數。 例如：`max-age=60` （60 秒）， `max-age=2592000` （1 個月） |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **在要求上**:快取來滿足要求，絕不能使用的預存的回應。 注意:原始伺服器重新產生回應的用戶端和中介軟體會更新其快取中的預存的回應。<br><br>**在回應上**:回應不必須用於後續的要求，而不需在來源伺服器上的驗證。 |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **在要求上**:快取不得儲存要求。<br><br>**在回應上**:快取不得儲存回應的任何部分。 |

其他扮演的角色中快取的快取標頭會顯示下表中。

| 頁首                                                     | 功能 |
| ---------------------------------------------------------- | -------- |
| [存留期](https://tools.ietf.org/html/rfc7234#section-5.1)     | 估計的秒數之後產生回應，或在原始伺服器已成功驗證的時間量。 |
| [到期](https://tools.ietf.org/html/rfc7234#section-5.3) | 日期/時間之後，回應會被視為過時。 |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | 回溯相容性，使用 HTTP/1.0 會在快取設定存在`no-cache`行為。 如果`Cache-Control`標頭已存在，`Pragma`標頭被忽略。 |
| [而有所不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定快取的回應必須不傳送除非所有的`Vary`快取回的應的原始要求和新的要求中符合的標頭欄位。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>以 HTTP 為基礎的快取方面要求快取控制指示詞

[Cache-control 標頭的 HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234#section-5.2)需要接受有效的快取`Cache-Control`用戶端傳送的標頭。 用戶端可要求`no-cache`標頭值並強制產生新的回應，每個要求的伺服器。

一律接受用戶端`Cache-Control`才有您考慮的目標 HTTP 快取要求標頭強大的意義。 正式規格，在快取的目的在於降低跨網路的用戶端、 proxy 和伺服器滿足要求的延遲和網路額外負荷。 它不一定是控制原始伺服器上的負載的方式。

沒有此快取的行為沒有開發人員控制使用時[回應快取中介軟體](xref:performance/caching/middleware)因為中介軟體會遵守官方快取規格。 [計劃中介軟體的增強功能](https://github.com/aspnet/AspNetCore/issues/2612)是設定來略過要求的中介軟體的好機會`Cache-Control`時決定要做為快取回的應標頭。 計劃的增強功能提供更好的控制伺服器負載的機會。

## <a name="other-caching-technology-in-aspnet-core"></a>ASP.NET Core 中的其他快取技術

### <a name="in-memory-caching"></a>記憶體中快取

記憶體中快取會使用伺服器記憶體儲存快取的資料。 這種類型的快取是適用於單一伺服器或多部伺服器，使用*黏性工作階段*。 黏性工作階段，表示用戶端的要求會一律路由傳送至相同的伺服器進行處理。

如需詳細資訊，請參閱 <<c0> [ 快取於記憶體中](xref:performance/caching/memory)。

### <a name="distributed-cache"></a>分散式快取

若要將資料儲存在記憶體中，應用程式裝載在雲端或伺服器的伺服器陣列時使用分散式快取。 處理要求的伺服器之間共用快取。 用戶端可以提交的要求，如果用戶端快取的資料可用，由群組中的任何伺服器。 ASP.NET Core 提供 SQL Server 和分散式的 Redis 快取。

如需詳細資訊，請參閱<xref:performance/caching/distributed>。

### <a name="cache-tag-helper"></a>快取標籤協助程式

您可以使用快取標籤協助程式，快取的 MVC 檢視或 Razor 頁面的內容。 快取標籤協助程式會使用記憶體中快取來儲存資料。

如需詳細資訊，請參閱 < [ASP.NET Core MVC 中的快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。

### <a name="distributed-cache-tag-helper"></a>分散式快取標籤協助程式

您可以使用分散式快取標籤協助程式，快取中的 MVC 檢視或分散式的雲端或 web 伺服陣列案例中的 Razor 頁面的內容。 分散式快取標籤協助程式會使用 SQL Server 或 Redis 來儲存資料。

如需詳細資訊，請參閱 <<c0> [ 分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。

## <a name="responsecache-attribute"></a>ResponseCache 屬性

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)指定回應的快取中設定適當的標頭所需的參數。

> [!WARNING]
> 停用快取內容，其中包含已驗證的用戶端的資訊。 啟用快取應該只針對不會變更使用者的身分識別或使用者已登入為基礎的內容。

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)查詢金鑰之指定清單的值而異的預存的回應。 單一值時`*`是所有的回應要求查詢字串參數提供中介軟體而異。 `VaryByQueryKeys` 需要 ASP.NET Core 1.1 或更新版本。

[回應快取中介軟體](xref:performance/caching/middleware)必須設定啟用`VaryByQueryKeys`屬性; 否則會擲回執行階段例外狀況。 沒有對應的 HTTP 標頭，如`VaryByQueryKeys`屬性。 回應快取中介軟體處理一項 HTTP 功能屬性。 做為快取回的應中介軟體，查詢字串和查詢字串值必須符合先前的要求。 例如，請考慮要求和下表所示的結果的順序。

| 要求                          | 結果                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | 從伺服器傳回     |
| `http://example.com?key1=value1` | 傳回的中介軟體 |
| `http://example.com?key1=value2` | 從伺服器傳回     |

第一個要求是由伺服器傳回，而且快取中介軟體中。 第二個要求會傳回由中介軟體中，因為查詢字串比對前一個要求。 第三個要求不在快取中介軟體，因為查詢字串值不符合先前的要求。 

`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)。 `ResponseCacheFilter`之更新適當的 HTTP 標頭和回應的功能。 篩選器：

* 移除任何現有的標頭，如`Vary`， `Cache-Control`，和`Pragma`。 
* 寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。 
* 更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。

### <a name="vary"></a>而有所不同

此標頭，才會寫入時`VaryByHeader`屬性設定。 它會設定為`Vary`屬性的值。 下列範例會使用`VaryByHeader`屬性：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

您可以檢視您的瀏覽器的網路工具的回應標頭。 下圖顯示的輸出上的 Edge F12**網路**索引標籤時`About2`動作方法會重新整理：

![呼叫 About2 動作方法時，在 [網路] 索引標籤上邊緣 F12 輸出](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore 和 Location.None

`NoStore` 大部分的其他屬性會覆寫。 當這個屬性設定為`true`，則`Cache-Control`標頭設定為`no-store`。 如果`Location`設為`None`:

* `Cache-Control` 設定為 `no-store,no-cache`。
* `Pragma` 設定為 `no-cache`。

如果`NoStore`是`false`並`Location`是`None`，`Cache-Control`並`Pragma`設為`no-cache`。

您通常會設定`NoStore`至`true`錯誤頁面上。 例如: 

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

這會導致下列標頭：

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>位置和持續時間

若要啟用快取`Duration`必須設為正值和`Location`必須是`Any`（預設值） 或`Client`。 在此情況下，`Cache-Control`標頭設定為後面的位置值`max-age`的回應。

> [!NOTE]
> `Location`選項`Any`並`Client`轉譯成`Cache-Control`標頭值`public`和`private`分別。 如先前所述，設定`Location`要`None`同時設定`Cache-Control`並`Pragma`標頭`no-cache`。

以下範例顯示標頭產生 splittunneling`Duration`並保留預設值`Location`值：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

這會產生下列標頭：

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>快取設定檔

而不必重複`ResponseCache`許多的控制器動作屬性，快取設定檔上的設定可以設定為選項，設定中的 MVC 時`ConfigureServices`方法中的`Startup`。 在參考的快取設定檔中找到的值會做的預設值`ResponseCache`屬性，並會覆寫為任何屬性上指定的屬性。

設定快取設定檔：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

參考的快取設定檔：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

`ResponseCache`可以套用屬性，包括動作 （方法），以及控制站 （類別）。 方法層級屬性會覆寫類別層級屬性中指定的設定。

在上述範例中，類別層級屬性會指定持續時間為 30 秒，雖然方法層級屬性會參考快取設定檔設定為 60 秒的持續時間。

產生的標頭：

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>其他資源

* [將回應儲存在快取](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
