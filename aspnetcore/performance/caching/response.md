---
title: "回應快取"
author: rick-anderson
description: "說明如何使用快取以降低頻寬，並提升效能的回應。"
keywords: "ASP.NET Core，快取 HTTP 標頭的回應"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: af114401d2f6f183291caba3c015359afb737d93
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="response-caching"></a>回應快取

由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)，和[Steve Smith](https://ardalis.com/)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>什麼是快取回應

*快取回應*將快取相關的標頭新增至回應。 這些標頭，指定您想用戶端、 proxy 和中的介軟體來快取回應。 回應快取可以減少用戶端或 proxy 可讓 web 伺服器的要求數目。 回應快取也可以減少量網頁伺服器執行以產生回應的工作。 

主要用於快取的 HTTP 標頭是`Cache-Control`。 請參閱[HTTP 1.1 快取](https://tools.ietf.org/html/rfc7234#section-5.2)如需詳細資訊。 一般快取指示詞：

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [無快取](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)
* [而有所不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)

Web 伺服器可以快取回應，藉由新增快取中介軟體的回應。 請參閱[快取中介軟體的回應](middleware.md)如需詳細資訊。

## <a name="distributed-cache-tag-helper"></a>分散式快取標記協助程式

[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper)啟用分散式快取。


## <a name="responsecache-attribute"></a>ResponseCache 屬性

`ResponseCacheAttribute`指定在回應快取中設定適當的標頭的必要參數。 請參閱[ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)參數的描述。

>[!WARNING]
> 停用快取內容，其中包含已驗證的用戶端的資訊。 快取，才應該啟用不會變更的內容會根據使用者的身分識別，或者是否使用者登入。

`VaryByQueryKeys string[]`(需要 ASP.NET Core 1.1.0 和更新版本): 設定時，快取中介軟體的回應會將預存的回應因指定的查詢索引鍵清單的值。 必須啟用快取中介軟體的回應，才能設定`VaryByQueryKeys`屬性，否則執行階段例外狀況將會擲回。 沒有對應的 HTTP 標頭`VaryByQueryKeys`屬性。 這個屬性是由快取中介軟體的回應的 HTTP 功能。 中介軟體提供快取回的應，查詢字串和查詢字串值必須符合先前的要求。 例如，請考慮下列順序：

| 要求          | 結果 |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | 從伺服器傳回 |
| `http://example.com?key1=value1` | 所傳回的中介軟體 |
| `http://example.com?key1=value2` | 從伺服器傳回 |

第一個要求是由伺服器傳回，而且快取中介軟體。 第二個要求會傳回由中介軟體，因為查詢字串比對前一個要求。 第三項要求不在中介軟體快取因為查詢字串值不符合先前的要求。 

`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) `ResponseCacheFilter`。 `ResponseCacheFilter`執行的工作更新適當的 HTTP 標頭和回應的功能。 篩選器：

* 移除任何現有的標頭的`Vary`， `Cache-Control`，和`Pragma`。 
* 寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。 
* 更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。

### <a name="vary"></a>而有所不同

此標頭僅寫入時`VaryByHeader`屬性設定。 設定為`Vary`屬性的值。 下列範例會使用`VaryByHeader`屬性。

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

您的瀏覽器網路工具，您可以檢視回應標頭。 下圖顯示輸出的邊緣 F12**網路**索引標籤時`About2`動作方法會重新整理。 

![在上邊緣 F12 輸出 * * 網路 * * 索引標籤時呼叫 'About2' 動作方法](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore 和 Location.None

`NoStore`大部分的其他屬性會覆寫。 當這個屬性設定為`true`、`Cache-Control`標頭會設定為 「 無存放區 」。 如果`Location`設`None`:

* `Cache-Control` 設定為 `"no-store, no-cache"`。 
* `Pragma` 設定為 `no-cache`。 

如果`NoStore`是`false`和`Location`是`None`，`Cache-Control`和`Pragma`會設定為`no-cache`。

您通常將`NoStore`至`true`錯誤頁面上。 例如: 

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

這會導致下列標頭：

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>位置和持續時間

若要啟用快取，`Duration`必須設為正值和`Location`都必須在`Any`（預設值） 或`Client`。 在此情況下，`Cache-Control`標頭會設定為後面接著"的保留時間上限 」 的回應的位置值。

> [!NOTE]
> `Location`選項`Any`和`Client`轉譯`Cache-Control`標頭值的`public`和`private`分別。 如先前所述，設定`Location`至`None`同時設定`Cache-Control`和`Pragma`標頭`no-cache`。

以下範例，示範標頭所產生的設定`Duration`並讓預設`Location`值。

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

會產生下列標頭：

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>快取設定檔

而不必重複`ResponseCache`上許多的控制器動作屬性，快取設定檔的設定可以設定為選項，設定在 MVC 時`ConfigureServices`方法中的`Startup`。 參考的快取設定檔中的值將使用的預設值為`ResponseCache`屬性，然後將會覆寫屬性指定任何屬性。

設定快取設定檔：

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

參考快取設定檔：

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache`屬性可以套用到動作 （方法），以及控制站 （類別）。 方法層級屬性會覆寫類別層級屬性中指定的設定。

在上述範例中，類別層級屬性會指定持續時間為 30 秒，而方法層級屬性設定為 60 秒持續時間的參考快取設定檔。

產生的標頭：

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>其他資源

* [快取 http 規格](https://tools.ietf.org/html/rfc7234#section-3)
* [快取控制](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
