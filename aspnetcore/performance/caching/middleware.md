---
title: ASP.NET Core 中的回應快取中介軟體
author: rick-anderson
description: 了解如何設定和使用 ASP.NET Core 中的回應快取中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/caching/middleware
ms.openlocfilehash: 4deac15538d4607bd611c4e072daae39447681c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655733"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>ASP.NET Core 中的回應快取中介軟體

作者：[John Luo](https://github.com/JunTaoLuo)

::: moniker range=">= aspnetcore-3.0"

本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。 中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。 如需 HTTP 快取和[`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性的簡介，請參閱[回應](xref:performance/caching/response)快取。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="configuration"></a>組態

回應快取中介軟體可透過共用架構隱含地用於 ASP.NET Core 應用程式。

在 `Startup.ConfigureServices`中，將回應快取中介軟體新增至服務集合：

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，將中介軟體新增至 `Startup.Configure`中的要求處理管線：

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

範例應用程式會新增標頭，以在後續要求中控制快取：

* 快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)&ndash; 快取最多10秒的快取回應。
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; 設定中介軟體只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會提供快取的回應。

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。 中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。

> [!WARNING]
> 包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。 如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。

## <a name="options"></a>選項。

回應快取選項如下表所示。

| 選項 | 描述 |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | 回應主體的最大可快取大小（以位元組為單位）。 預設值為 `64 * 1024 * 1024` （64 MB）。 |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | 回應快取中介軟體的大小限制（以位元組為單位）。 預設值為 `100 * 1024 * 1024` （100 MB）。 |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | 決定是否在區分大小寫的路徑上快取回應。 預設值是 `false`。 |

下列範例會將中介軟體設定為：

* 具有小於或等於1024位元組之主體大小的快取回應。
* 將回應儲存為區分大小寫的路徑。 例如，`/page1` 和 `/Page1` 會分別儲存。

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

使用 MVC/Web API 控制器或 Razor Pages 頁面模型時， [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性會指定為回應快取設定適當標頭所需的參數。 嚴格要求中介軟體的 `[ResponseCache]` 屬性的唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>，這不會對應到實際的 HTTP 標頭。 如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute>。

當不使用 `[ResponseCache]` 屬性時，回應快取可能會隨著 `VaryByQueryKeys`而不同。 直接從[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Features)使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature>：

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

使用等於 `*` 的單一值 `VaryByQueryKeys` 會依所有要求查詢參數來改變快取。

## <a name="http-headers-used-by-response-caching-middleware"></a>回應快取中介軟體所使用的 HTTP 標頭

下表提供會影響回應快取的 HTTP 標頭資訊。

| 頁首 | 詳細資料 |
| ------ | ------- |
| `Authorization` | 如果標頭存在，則不會快取回應。 |
| `Cache-Control` | 中介軟體只會考慮以 `public` cache 指示詞標示的快取回應。 使用下列參數來控制快取：<ul><li>最大壽命</li><li>最大-過時&#8224;</li><li>最小-全新</li><li>must-revalidate</li><li>no-cache</li><li>否-存放區</li><li>僅限-快取</li><li>私用</li><li>公開</li><li>s-maxage</li><li>proxy-重新驗證&#8225;</li></ul>&#8224;如果未指定 `max-stale`的限制，中介軟體不會採取任何動作。<br>&#8225;`proxy-revalidate` 與 `must-revalidate`的效果相同。<br><br>如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。 |
| `Pragma` | 要求中的 `Pragma: no-cache` 標頭會產生與 `Cache-Control: no-cache`相同的效果。 `Cache-Control` 標頭中的相關指示詞會覆寫此標頭（如果有的話）。 視為與 HTTP/1.0 的回溯相容性。 |
| `Set-Cookie` | 如果標頭存在，則不會快取回應。 要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。  |
| `Vary` | `Vary` 標頭是用來依另一個標頭來改變快取的回應。 例如，藉由包含 `Vary: Accept-Encoding` 標頭，以編碼方式快取回應，其會快取標頭 `Accept-Encoding: gzip` 和 `Accept-Encoding: text/plain` 的要求回應。 永遠不會儲存標頭值為 `*` 的回應。 |
| `Expires` | 除非由其他 `Cache-Control` 標頭覆寫，否則此標頭所視為過時的回應不會儲存或抓取。 |
| `If-None-Match` | 如果值不 `*`，而且回應的 `ETag` 不符合任何提供的值，則會從快取中提供完整回應。 否則，會提供304（未修改）的回應。 |
| `If-Modified-Since` | 如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。 否則，會提供*304-未修改*的回應。 |
| `Date` | 從快取提供服務時，如果原始回應未提供 `Date` 標頭，則會由中介軟體設定。 |
| `Content-Length` | 從快取提供服務時，如果原始回應未提供 `Content-Length` 標頭，則會由中介軟體設定。 |
| `Age` | 會忽略原始回應中所傳送的 `Age` 標頭。 中介軟體會在服務快取回應時計算新的值。 |

## <a name="caching-respects-request-cache-control-directives"></a>快取遵循要求快取控制指示詞

中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。 這些規則需要快取以接受用戶端所傳送的有效 `Cache-Control` 標頭。 在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。 目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。

若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。 請參閱下列主題：

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>疑難排解

如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。 檢查要求的傳入標頭和回應的傳出標頭。 啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。

在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。 例如，瀏覽器可能會在重新整理頁面時，將 `Cache-Control` 標頭設定為 `no-cache` 或 `max-age=0`。 下列工具可以明確設定要求標頭，並慣用來測試快取：

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>快取的條件

* 要求必須產生具有200（確定）狀態碼的伺服器回應。
* 要求方法必須是 GET 或 HEAD。
* 在 `Startup.Configure`中，回應快取中介軟體必須放在需要快取的中介軟體前面。 如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。
* `Authorization` 標頭必須不存在。
* `Cache-Control` 標頭參數必須是有效的，而且必須將回應標記為 `public` 而不是標示為 `private`。
* 如果 `Cache-Control` 標頭不存在，則不能出現 `Pragma: no-cache` 標頭，因為 `Cache-Control` 標頭會在出現時覆寫 `Pragma` 標頭。
* `Set-Cookie` 標頭必須不存在。
* `Vary` 標頭參數必須有效，而且不等於 `*`。
* `Content-Length` 標頭值（如果設定）必須符合回應主體的大小。
* 不使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>。
* 回應不得過時，如 `Expires` 標頭和 `max-age` 和 `s-maxage` cache 指示詞所指定。
* 回應緩衝處理必須成功。 回應的大小必須小於設定的 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>或預設值。 回應的主體大小必須小於設定的或預設 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>。
* 回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。 例如，[`no-store`] 指示詞不能存在於 [要求] 或 [回應標頭] 欄位中。 如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。

> [!NOTE]
> 用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設定為 `no-cache`，如此就不會快取回應。 如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。 中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。 如需 HTTP 快取和[`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性的簡介，請參閱[回應](xref:performance/caching/response)快取。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="configuration"></a>組態

使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)套件。

在 `Startup.ConfigureServices`中，將回應快取中介軟體新增至服務集合：

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，將中介軟體新增至 `Startup.Configure`中的要求處理管線：

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

範例應用程式會新增標頭，以在後續要求中控制快取：

* 快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)&ndash; 快取最多10秒的快取回應。
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; 設定中介軟體只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會提供快取的回應。

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。 中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。

> [!WARNING]
> 包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。 如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。

## <a name="options"></a>選項。

回應快取選項如下表所示。

| 選項 | 描述 |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | 回應主體的最大可快取大小（以位元組為單位）。 預設值為 `64 * 1024 * 1024` （64 MB）。 |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | 回應快取中介軟體的大小限制（以位元組為單位）。 預設值為 `100 * 1024 * 1024` （100 MB）。 |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | 決定是否在區分大小寫的路徑上快取回應。 預設值是 `false`。 |

下列範例會將中介軟體設定為：

* 具有小於或等於1024位元組之主體大小的快取回應。
* 將回應儲存為區分大小寫的路徑。 例如，`/page1` 和 `/Page1` 會分別儲存。

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

使用 MVC/Web API 控制器或 Razor Pages 頁面模型時， [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性會指定為回應快取設定適當標頭所需的參數。 嚴格要求中介軟體的 `[ResponseCache]` 屬性的唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>，這不會對應到實際的 HTTP 標頭。 如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute>。

當不使用 `[ResponseCache]` 屬性時，回應快取可能會隨著 `VaryByQueryKeys`而不同。 直接從[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Features)使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature>：

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

使用等於 `*` 的單一值 `VaryByQueryKeys` 會依所有要求查詢參數來改變快取。

## <a name="http-headers-used-by-response-caching-middleware"></a>回應快取中介軟體所使用的 HTTP 標頭

下表提供會影響回應快取的 HTTP 標頭資訊。

| 頁首 | 詳細資料 |
| ------ | ------- |
| `Authorization` | 如果標頭存在，則不會快取回應。 |
| `Cache-Control` | 中介軟體只會考慮以 `public` cache 指示詞標示的快取回應。 使用下列參數來控制快取：<ul><li>最大壽命</li><li>最大-過時&#8224;</li><li>最小-全新</li><li>must-revalidate</li><li>no-cache</li><li>否-存放區</li><li>僅限-快取</li><li>私用</li><li>公開</li><li>s-maxage</li><li>proxy-重新驗證&#8225;</li></ul>&#8224;如果未指定 `max-stale`的限制，中介軟體不會採取任何動作。<br>&#8225;`proxy-revalidate` 與 `must-revalidate`的效果相同。<br><br>如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。 |
| `Pragma` | 要求中的 `Pragma: no-cache` 標頭會產生與 `Cache-Control: no-cache`相同的效果。 `Cache-Control` 標頭中的相關指示詞會覆寫此標頭（如果有的話）。 視為與 HTTP/1.0 的回溯相容性。 |
| `Set-Cookie` | 如果標頭存在，則不會快取回應。 要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。  |
| `Vary` | `Vary` 標頭是用來依另一個標頭來改變快取的回應。 例如，藉由包含 `Vary: Accept-Encoding` 標頭，以編碼方式快取回應，其會快取標頭 `Accept-Encoding: gzip` 和 `Accept-Encoding: text/plain` 的要求回應。 永遠不會儲存標頭值為 `*` 的回應。 |
| `Expires` | 除非由其他 `Cache-Control` 標頭覆寫，否則此標頭所視為過時的回應不會儲存或抓取。 |
| `If-None-Match` | 如果值不 `*`，而且回應的 `ETag` 不符合任何提供的值，則會從快取中提供完整回應。 否則，會提供304（未修改）的回應。 |
| `If-Modified-Since` | 如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。 否則，會提供*304-未修改*的回應。 |
| `Date` | 從快取提供服務時，如果原始回應未提供 `Date` 標頭，則會由中介軟體設定。 |
| `Content-Length` | 從快取提供服務時，如果原始回應未提供 `Content-Length` 標頭，則會由中介軟體設定。 |
| `Age` | 會忽略原始回應中所傳送的 `Age` 標頭。 中介軟體會在服務快取回應時計算新的值。 |

## <a name="caching-respects-request-cache-control-directives"></a>快取遵循要求快取控制指示詞

中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。 這些規則需要快取以接受用戶端所傳送的有效 `Cache-Control` 標頭。 在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。 目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。

若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。 請參閱下列主題：

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>疑難排解

如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。 檢查要求的傳入標頭和回應的傳出標頭。 啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。

在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。 例如，瀏覽器可能會在重新整理頁面時，將 `Cache-Control` 標頭設定為 `no-cache` 或 `max-age=0`。 下列工具可以明確設定要求標頭，並慣用來測試快取：

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>快取的條件

* 要求必須產生具有200（確定）狀態碼的伺服器回應。
* 要求方法必須是 GET 或 HEAD。
* 在 `Startup.Configure`中，回應快取中介軟體必須放在需要快取的中介軟體前面。 如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。
* `Authorization` 標頭必須不存在。
* `Cache-Control` 標頭參數必須是有效的，而且必須將回應標記為 `public` 而不是標示為 `private`。
* 如果 `Cache-Control` 標頭不存在，則不能出現 `Pragma: no-cache` 標頭，因為 `Cache-Control` 標頭會在出現時覆寫 `Pragma` 標頭。
* `Set-Cookie` 標頭必須不存在。
* `Vary` 標頭參數必須有效，而且不等於 `*`。
* `Content-Length` 標頭值（如果設定）必須符合回應主體的大小。
* 不使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>。
* 回應不得過時，如 `Expires` 標頭和 `max-age` 和 `s-maxage` cache 指示詞所指定。
* 回應緩衝處理必須成功。 回應的大小必須小於設定的 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>或預設值。 回應的主體大小必須小於設定的或預設 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>。
* 回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。 例如，[`no-store`] 指示詞不能存在於 [要求] 或 [回應標頭] 欄位中。 如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。

> [!NOTE]
> 用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設定為 `no-cache`，如此就不會快取回應。 如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
