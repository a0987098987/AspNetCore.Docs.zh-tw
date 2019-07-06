---
title: ASP.NET Core 中的回應快取中介軟體
author: guardrex
description: 了解如何設定和使用 ASP.NET Core 中的回應快取中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: performance/caching/middleware
ms.openlocfilehash: d6756ce16396133da643cc08ca0f48369479ad3a
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596144"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>ASP.NET Core 中的回應快取中介軟體

藉由[Luke Latham](https://github.com/guardrex)和[John Luo](https://github.com/JunTaoLuo)

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

這篇文章說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。 中介軟體會判斷何時可快取回應、存放回應，以及從快取提供回應。 如需 HTTP 快取的簡介和[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性，請參閱[回應快取](xref:performance/caching/response)。

## <a name="configuration"></a>組態

使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或新增的套件參考[Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)封裝。

在  `Startup.ConfigureServices`，將回應快取中介軟體新增至服務集合：

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

設定應用程式使用中的介軟體<xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>擴充方法，將中介軟體新增至要求處理管線中`Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

範例應用程式會新增要控制快取在後續的要求標頭：

* [快取控制](https://tools.ietf.org/html/rfc7234#section-5.2)&ndash;快取可快取的回應，最多 10 秒的時間。
* [改變](https://tools.ietf.org/html/rfc7231#section-7.1.4)&ndash;設定中介軟體提供快取回的應只有當[ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)後續的要求標頭是否完全符合原始要求。

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

回應快取中介軟體只會快取產生 200 (沒問題) 狀態碼的伺服器回應。 中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。

> [!WARNING]
> 包含內容的已驗證的用戶端的回應必須標示為不可快取，以防止從儲存和處理這些回應的中介軟體。 請參閱[快取的條件](#conditions-for-caching)如需有關如何在中介軟體，判斷回應是否可快取。

## <a name="options"></a>選項

回應快取選項會顯示下表中。

| 選項 | 描述 |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | 回應內文，以位元組為單位的最大的快取大小。 預設值是`64 * 1024 * 1024`(64 MB)。 |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | 回應快取中介軟體，以位元組為單位的大小限制。 預設值是`100 * 1024 * 1024`(100 MB)。 |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | 決定回應會快取在區分大小寫的路徑上。 預設值為 `false`。 |

下列範例會設定中的介軟體：

* 內文大小小於或等於 1024 個位元組的快取回應。
* 將回應儲存透過區分大小寫的路徑。 例如，`/page1`和`/Page1`分開儲存。

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

使用 MVC 時 / web API 控制器或 Razor 頁面的頁面模型[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性指定設定適當的標頭的回應快取所需的參數。 唯一的參數`[ResponseCache]`嚴格需要的中介軟體的屬性是<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>，不會對應到實際的 HTTP 標頭。 如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute>。

不使用時`[ResponseCache]`屬性，回應快取可因使用`VaryByQueryKeys`。 使用<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature>直接從[HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

使用單一的值等於`*`在`VaryByQueryKeys`會快取所要求的所有查詢參數而有所不同。

## <a name="http-headers-used-by-response-caching-middleware"></a>回應快取中介軟體所使用的 HTTP 標頭

下表提供會影響回應快取的 HTTP 標頭資訊。

| 標頭 | 詳細資料 |
| ------ | ------- |
| `Authorization` | 如果此標頭存在，回應不會被加入快取中。 |
| `Cache-Control` | 中介軟體只會考慮快取標有 `public` 快取指示詞的回應。 使用下列參數控制快取：<ul><li>max-age</li><li>max-stale&#8224;</li><li>min-fresh</li><li>must-revalidate</li><li>no-cache</li><li>no-store</li><li>only-if-cached</li><li>private</li><li>public</li><li>s maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224;如果沒有指定限制到 `max-stale`，中介軟體不會採取任何動作。<br>&#8225;`proxy-revalidate`具有與 `must-revalidate` 相同的效果。<br><br>如需詳細資訊，請參閱[RFC 7231：要求快取控制指示詞](https://tools.ietf.org/html/rfc7234#section-5.2.1)。 |
| `Pragma` | 要求中的 `Pragma: no-cache` 標頭會產生與 `Cache-Control: no-cache` 相同的效果。 此標頭會由 `Cache-Control` 中的相關指示詞覆寫 (如果有的話)。 已考慮與 HTTP/1.0 的回溯相容性。 |
| `Set-Cookie` | 如果此標頭存在，回應不會被加入快取中。 設定一或多個 cookie 的要求處理管線中的任何中介軟體會防止從快取的回應的回應快取中介軟體 (例如[cookie 架構 TempData 提供者](xref:fundamentals/app-state#tempdata))。  |
| `Vary` | `Vary`標頭會由另一個標頭用來改變快取回應。 例如，透過包含 `Vary: Accept-Encoding` 標頭，以編碼方式快取回應，這會分別為具有 `Accept-Encoding: gzip` 與 `Accept-Encoding: text/plain` 的標頭的要求快取回應。 一律不會儲存具有開頭值 `*` 的回應。 |
| `Expires` | 除非由其他 `Cache-Control` 標頭覆寫，否則不會存儲或檢索被此標頭視為過時的回應。 |
| `If-None-Match` | 如果值不是 `*` 且回應的 `ETag` 與提供的任何值都不相符，則從快取提供完整回應。 否則，會提供 304 (未修改) 的回應。 |
| `If-Modified-Since` | 如果`If-None-Match`標頭不存在，如果快取的回應日期比所提供的值從快取提供完整的回應。 否則，請*304-未修改*提供回應。 |
| `Date` | 從快取提供時，如果原始回應中未提供 `Date` 標頭，則由中介軟體設定。 |
| `Content-Length` | 從快取提供時，如果原始回應中未提供 `Content-Length` 標頭，則由中介軟體設定。 |
| `Age` | 在 `Age`原始回應中傳送的標頭會被忽略。 中介軟體提供快取的回應時會計算新的值。 |

## <a name="caching-respects-request-cache-control-directives"></a>快取會遵守要求快取控制指示詞

中介軟體會遵守的規則[快取的 HTTP 1.1 規格](https://tools.ietf.org/html/rfc7234#section-5.2)。 規則需要接受有效的快取`Cache-Control`用戶端傳送的標頭。 在規格中，在用戶端可要求`no-cache`標頭值並強制產生新的回應，每個要求的伺服器。 目前沒有任何開發人員會控制此快取的行為時使用的中介軟體，因為中介軟體會遵守官方的快取規格。

進一步控制快取行為的詳細資訊，探索 ASP.NET Core 的其他快取功能。 請參閱下列主題：

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>疑難排解

如果快取行為，將無法如預期般運作，請確認回應的快取，而且能夠從快取所提供。 檢查要求的連入標頭和回應的傳出標頭。 啟用[記錄](xref:fundamentals/logging/index)來協助偵錯。

當進行測試和疑難排解快取行為，在瀏覽器可能設定影響不想要的方式快取的要求標頭。 例如，可能會設定瀏覽器`Cache-Control`標頭`no-cache`或`max-age=0`時重新整理頁面。 下列工具可以明確設定要求標頭，並正適合測試快取：

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>快取的條件

* 伺服器回應 200 （確定） 狀態碼必須產生要求。
* 要求方法必須是 GET 或 HEAD。
* 在  `Startup.Configure`，需要快取的中介軟體之前，必須放回應快取中介軟體。 如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。
* `Authorization`不得存在於標頭。
* `Cache-Control` 標頭參數必須有效，且必須標示為回應`public`而未標示為`private`。
* `Pragma: no-cache`標頭不能有如果`Cache-Control`標頭不存在，作為`Cache-Control`標頭會覆寫`Pragma`標頭時出現。
* `Set-Cookie`不得存在於標頭。
* `Vary` 標頭參數必須是有效且不等於`*`。
* `Content-Length`標頭值 (如果設定) 必須符合回應主體的大小。
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>不會使用。
* 回應不是所指定的過時`Expires`標頭和`max-age`和`s-maxage`快取指示詞。
* 回應緩衝處理必須先成功完成。 回應的大小必須小於所設定，或預設<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>。 回應本文大小必須小於所設定，或預設<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>。
* 回應必須是根據快取[RFC 7234](https://tools.ietf.org/html/rfc7234)規格。 比方說，`no-store`指示詞不能存在於要求或回應標頭欄位。 請參閱*第 3 節：將回應儲存在快取*的[RFC 7234](https://tools.ietf.org/html/rfc7234)如需詳細資訊。

> [!NOTE]
> 防偽系統是否有產生安全的權杖，以防止跨網站要求偽造 (CSRF) 攻擊集`Cache-Control`並`Pragma`標頭`no-cache`，因此不會快取的回應。 如需如何停用 HTML 表單元素的 antiforgery 權杖的資訊，請參閱<xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
