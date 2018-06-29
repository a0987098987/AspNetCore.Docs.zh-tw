---
title: 回應快取中 ASP.NET Core
author: rick-anderson
description: 了解如何使用快取以較低的頻寬需求的回應，並增加 ASP.NET Core 應用程式的效能。
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077760"
---
# <a name="response-caching-in-aspnet-core"></a>回應快取中 ASP.NET Core

由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)

> [!NOTE]
> 回應快取中 Razor 頁面是使用 ASP.NET Core 2.1 或更新版本。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

回應快取可減少用戶端或 proxy 可讓 web 伺服器的要求數目。 回應快取也可以減少網頁伺服器執行產生回應的工作。 指定您要用戶端、 proxy、 和中的介軟體來快取回應的標頭會控制快取回應。

Web 伺服器可以快取的回應，當您將加入[回應快取中介軟體](xref:performance/caching/middleware)。

## <a name="http-based-response-caching"></a>HTTP 回應的快取

[HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234)描述網際網路快取的行為方式。 主要用於快取的 HTTP 標頭是[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)，用來指定快取*指示詞*。 要求對其的方式從用戶端伺服器和個回應傳回給用戶端從伺服器進行其方法，指示詞會控制快取行為。 要求和回應移動透過 proxy 伺服器和 proxy 伺服器也必須符合 HTTP 1.1 快取的規格。

一般`Cache-Control`指示詞會顯示下表中。

| 指示詞                                                       | 動作 |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | 快取可能會將回應儲存。 |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | 回應不能儲存的共用快取。 私用快取可以儲存和重複使用的回應。 |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | 用戶端不接受其年齡大於指定的秒數的回應。 範例： `max-age=60` （60 秒）， `max-age=2592000` （1 個月） |
| [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **在要求上**： 快取不能使用的預存的回應滿足要求。 注意： 為用戶端，來源伺服器重新產生的回應和中介軟體會更新其快取中的預存的回應。<br><br>**回應**： 回應未必須用於後續的要求，但不在原始伺服器上的驗證。 |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **在要求上**： 快取不能儲存的要求。<br><br>**回應**： 快取不能儲存任何回應的一部分。 |

其他扮演的角色中快取的快取標頭會顯示下表中。

| 頁首                                                     | 功能 |
| ---------------------------------------------------------- | -------- |
| [存留期](https://tools.ietf.org/html/rfc7234#section-5.1)     | 估計的時間，以秒為單位，因為產生的回應，或是在原始伺服器成功驗證。 |
| [到期](https://tools.ietf.org/html/rfc7234#section-5.3) | 日期/時間之後的回應會被視為過時了。 |
| [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | 回溯相容性 HTTP/1.0 會在快取設定存在`no-cache`行為。 如果`Cache-Control`標頭已存在，`Pragma`標頭會被忽略。 |
| [而有所不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | 指定快取的回應必須不傳送除非所有的`Vary`標頭欄位相符的原始要求的快取的回應和新的要求。 |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>以 HTTP 為基礎的快取方面要求快取控制指示詞

[HTTP 1.1 快取的快取控制標頭的規格](https://tools.ietf.org/html/rfc7234#section-5.2)需要遵守有效的快取`Cache-Control`用戶端傳送的標頭。 用戶端可要求`no-cache`標頭值，強制產生新的回應，每個要求的伺服器。

永遠接受用戶端`Cache-Control`要求標頭就有意義，如果您考慮 HTTP 快取的目標。 官方規格，在快取是用來減少滿足要求的用戶端、 proxy 和伺服器在網路上的延遲和網路負擔。 它不一定能夠控制在來源伺服器上的負載。

沒有目前開發人員控制此快取行為時沒有使用[回應快取中介軟體](xref:performance/caching/middleware)由於中介軟體遵守正式快取規格。 [未來的增強功能的中介軟體](https://github.com/aspnet/ResponseCaching/issues/96)設定要略過要求的中介軟體將會允許`Cache-Control`標頭決定要提供快取的回應時。 這會提供您更好的控制負載伺服器上有機會時使用的中介軟體。

## <a name="other-caching-technology-in-aspnet-core"></a>在 ASP.NET Core 其他快取技術

### <a name="in-memory-caching"></a>記憶體中快取

記憶體中快取使用伺服器記憶體來儲存快取的資料。 這種類型的快取是適用於單一伺服器或多部伺服器使用*黏性工作階段*。 自黏工作階段表示，從用戶端要求一律將路由傳送至相同的伺服器進行處理。

如需詳細資訊，請參閱[快取記憶體中](xref:performance/caching/memory)。

### <a name="distributed-cache"></a>分散式快取

若要將資料儲存在記憶體中，當應用程式裝載於雲端或伺服器陣列中使用分散式快取。 處理要求的伺服器之間共用快取。 用戶端可以提交要求，如果用戶端快取的資料可由群組中的任何伺服器。 ASP.NET Core 提供 SQL Server 和分散式的 Redis 快取。

如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。

### <a name="cache-tag-helper"></a>快取標記協助程式

您可以與快取標記協助程式快取中的 MVC 檢視或 Razor 頁面的內容。 快取標記協助程式使用記憶體中快取來儲存資料。

如需詳細資訊，請參閱[ASP.NET Core MVC 中的快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。

### <a name="distributed-cache-tag-helper"></a>分散式快取標籤協助程式

您可以使用分散式快取標記協助程式快取的 MVC 檢視或分散式的雲端或 web 伺服陣列案例中的 Razor 頁面的內容。 分散式快取標記協助程式用來儲存資料的 SQL Server 或 Redis。

如需詳細資訊，請參閱[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。

## <a name="responsecache-attribute"></a>ResponseCache 屬性

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)指定在回應快取中設定適當的標頭的必要參數。

> [!WARNING]
> 停用快取內容，其中包含已驗證的用戶端的資訊。 快取，才應該啟用並不會變更使用者的身分識別或使用者是否登入為基礎的內容。

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)預存的回應因指定的查詢索引鍵清單的值。 單一值時`*`是所有的回應要求查詢字串參數提供中介軟體而有所不同。 `VaryByQueryKeys` 需要 ASP.NET Core 1.1 或更新版本。

必須啟用回應快取中介軟體，才能設定`VaryByQueryKeys`屬性; 否則擲回執行階段例外狀況。 沒有對應的 HTTP 標頭的`VaryByQueryKeys`屬性。 屬性是 HTTP 功能由回應快取中介軟體。 中介軟體提供快取回的應，查詢字串和查詢字串值必須符合先前的要求。 例如，請考慮要求和結果下表所示的順序。

| 要求                          | 結果                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | 從伺服器傳回     |
| `http://example.com?key1=value1` | 所傳回的中介軟體 |
| `http://example.com?key1=value2` | 從伺服器傳回     |

第一個要求是由伺服器傳回，而且快取中介軟體。 第二個要求會傳回由中介軟體，因為查詢字串比對前一個要求。 第三項要求不在的中介軟體快取中，因為查詢字串值不符合先前的要求。 

`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)。 `ResponseCacheFilter`執行的工作更新適當的 HTTP 標頭和回應的功能。 篩選器：

* 移除任何現有的標頭的`Vary`， `Cache-Control`，和`Pragma`。 
* 寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。 
* 更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。

### <a name="vary"></a>而有所不同

此標頭僅寫入時`VaryByHeader`屬性設定。 設定為`Vary`屬性的值。 下列範例會使用`VaryByHeader`屬性：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

您可以檢視您的瀏覽器網路工具的回應標頭。 下圖顯示輸出的邊緣 F12**網路**索引標籤時`About2`動作方法會重新整理：

![About2 動作方法呼叫時，在 [網路] 索引標籤上邊緣 F12 輸出](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore 和 Location.None

`NoStore` 大部分的其他屬性會覆寫。 當這個屬性設定為`true`、`Cache-Control`標頭設定為`no-store`。 如果`Location`設`None`:

* `Cache-Control` 設定為 `no-store,no-cache`。
* `Pragma` 設定為 `no-cache`。

如果`NoStore`是`false`和`Location`是`None`，`Cache-Control`和`Pragma`設為`no-cache`。

您通常將`NoStore`至`true`錯誤頁面上。 例如: 

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

若要啟用快取，`Duration`必須設為正值和`Location`都必須在`Any`（預設值） 或`Client`。 在此情況下，`Cache-Control`後面的位置值來設定標頭`max-age`的回應。

> [!NOTE]
> `Location`選項`Any`和`Client`轉譯`Cache-Control`標頭值的`public`和`private`分別。 如先前所述，設定`Location`至`None`會同時設定`Cache-Control`和`Pragma`標頭`no-cache`。

以下範例，示範標頭所產生的設定`Duration`並讓預設`Location`值：

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

而不必重複`ResponseCache`上許多的控制器動作屬性，快取設定檔的設定可以設定為選項，設定在 MVC 時`ConfigureServices`方法中的`Startup`。 參考的快取設定檔中的值會使用由預設`ResponseCache`屬性，並會覆寫屬性指定任何屬性。

設定快取設定檔：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

參考快取設定檔：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

`ResponseCache`屬性可以套用到動作 （方法） 和控制站 （類別）。 方法層級屬性會覆寫類別層級屬性中指定的設定。

在上述範例中，類別層級屬性指定持續時間為 30 秒，而方法層級屬性設定為 60 秒持續時間的參考快取設定檔。

產生的標頭：

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>其他資源

* [將回應儲存在快取](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [記憶體中快取](xref:performance/caching/memory)
* [使用分散式快取](xref:performance/caching/distributed)
* [使用變更權杖來偵測變更](xref:fundamentals/primitives/change-tokens)
* [回應快取中介軟體](xref:performance/caching/middleware)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
