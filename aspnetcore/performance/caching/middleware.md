---
title: "快取中 ASP.NET Core 中的介軟體的回應"
author: guardrex
description: "了解如何設定和使用 ASP.NET Core 在回應快取中介軟體。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 01/26/2017
ms.topic: article
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: a355dda0fdffe1e936c119ffb477c2817f28bd9c
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2018
---
# <a name="response-caching-middleware-in-aspnet-core"></a>快取中 ASP.NET Core 中的介軟體的回應

由[Luke Latham](https://github.com/guardrex)和[John Luo](https://github.com/JunTaoLuo)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

本文說明如何設定 ASP.NET Core 應用程式中的回應快取中介軟體。 中介軟體會決定當回應的快取、 儲存區回應，以及做回應從快取。 如需簡介 HTTP 快取和`ResponseCache`屬性，請參閱[快取回應](xref:performance/caching/response)。

## <a name="package"></a>Package

若要在專案中包含中介軟體，將參考加入[ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)封裝，或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)封裝 (ASP.NET Core 2.0 或更新版本為目標的.NET Core 時)。

## <a name="configuration"></a>組態

在`ConfigureServices`，將中介軟體新增至服務集合。

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet1&highlight=3)]

設定應用程式使用中的介軟體`UseResponseCaching`擴充方法，將中介軟體新增至處理管線的要求。 範例應用程式將[ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2)標頭至回應的快取的回應會快取多達 10 秒。 此範例傳送[ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4)標頭設定的中介軟體提供快取回的應才[ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)後續要求的標頭與相符的原始要求。

[!code-csharp[Main](middleware/sample/Startup.cs?name=snippet2&highlight=3,7-12)]

回應快取中介軟體只會快取伺服器的回應，導致 200 （確定） 」 狀態碼。 任何其他回應，包括[錯誤網頁](xref:fundamentals/error-handling)中, 介軟體會被忽略。

> [!WARNING]
> 回應包含內容的已驗證的用戶端必須標示為不可快取，以防止從儲存和處理這些回應的中介軟體。 請參閱[快取的條件](#conditions-for-caching)如需詳細資訊中, 介軟體如何判斷是否可快取的回應。

## <a name="options"></a>選項

中介軟體提供三個選項來控制回應快取。

| 選項                | 預設值 |
| --------------------- | ------------- |
| UseCaseSensitivePaths | 決定是否區分大小寫的路徑上快取的回應。</p><p>預設值是 `false`。 |
| MaximumBodySize       | 回應主體，以位元組為單位的最大的可快取大小。</p>預設值是`64 * 1024 * 1024`(64 MB)。 |
| SizeLimit             | 回應快取中介軟體，以位元組為單位的大小限制。 預設值是`100 * 1024 * 1024`(100 MB)。 |

下列範例會設定中的介軟體：

* 快取回應小於或等於 1024 個位元組。
* 回應儲存透過區分大小寫的路徑 (例如，`/page1`和`/Page1`分開儲存)。

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

當使用 MVC/Web API 控制器或 Razor 頁面的頁面模型`ResponseCache`屬性會指定所需的設定適當的標頭，為快取回應的參數。 唯一的參數`ResponseCache`絕對需要中, 介軟體的屬性是`VaryByQueryKeys`，這並不對應到實際的 HTTP 標頭。 如需詳細資訊，請參閱[ResponseCache 屬性](xref:performance/caching/response#responsecache-attribute)。

不使用時`ResponseCache`屬性快取回應差異性與`VaryByQueryKeys`功能。 使用`ResponseCachingFeature`直接從`IFeatureCollection`的`HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>回應快取中介軟體所使用的 HTTP 標頭

回應的快取中介軟體是使用 HTTP 標頭設定。

| 頁首 | 詳細資料 |
| ------ | ------- |
| 授權 | 如果標頭存在，回應不是快取。 |
| Cache-Control | 中介軟體只會考慮快取回應標記為`public`快取的指示詞。 控制快取包含下列參數：<ul><li>保留時間上限</li><li>max-stale&#8224;</li><li>min 全新</li><li>must-revalidate</li><li>無快取</li><li>no-store</li><li>僅限 if-快取</li><li>private</li><li>public</li><li>s maxage</li><li>proxy-revalidate&#8225;</li></ul>&#8224; 若要指定無限制到`max-stale`中, 介軟體會採取任何動作。<br>&#8225;`proxy-revalidate`具有相同的效果`must-revalidate`。<br><br>如需詳細資訊，請參閱[RFC 7231： 要求的快取控制指示詞](https://tools.ietf.org/html/rfc7234#section-5.2.1)。 |
| Pragma | A`Pragma: no-cache`要求標頭會產生相同的效果`Cache-Control: no-cache`。 此標頭中的相關指示詞會覆寫`Cache-Control`標頭，如果有的話。 與 HTTP/1.0 回溯相容性考量。 |
| Set-Cookie | 如果標頭存在，回應不是快取。 |
| 而有所不同 | `Vary`標頭由另一個標頭用來變更快取的回應。 比方說，快取的回應編碼方式包括`Vary: Accept-Encoding`快取回應之要求標頭的標頭`Accept-Encoding: gzip`和`Accept-Encoding: text/plain`分開。 回應標頭值是`*`絕對不會儲存。 |
| 到期 | 此標頭視為過時的回應不是存放或擷取除非覆寫其他`Cache-Control`標頭。 |
| If-None-Match | 如果此值不是從快取提供完整的回應`*`和`ETag`的回應不符合任何提供的值。 否則，提供 304 （未修改） 的回應。 |
| If-Modified-Since | 如果`If-None-Match`標頭不存在，如果快取的回應日期比所提供的值從快取提供完整的回應。 否則，提供 304 （未修改） 的回應。 |
| 日期 | 當服務從快取，`Date`若未提供原始回應上設定標頭中介軟體。 |
| Content-Length | 當服務從快取，`Content-Length`若未提供原始回應上設定標頭中介軟體。 |
| 存留期 | `Age`原始回應中傳送的標頭會被忽略。 當服務快取的回應中, 介軟體會計算新值。 |

## <a name="caching-respects-request-cache-control-directives"></a>快取尊重要求快取控制指示詞

中介軟體會遵守的規則[HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234#section-5.2)。 在規則需要遵守有效的快取`Cache-Control`用戶端傳送的標頭。 此規格，在用戶端可要求`no-cache`標頭值，強制產生新的回應，每個要求的伺服器。 目前，此快取行為沒有開發人員控制時使用中介軟體，由於中介軟體遵守正式的快取規格。

如需更多控制快取行為的詳細資訊，瀏覽 ASP.NET Core 其他快取的功能。 請參閱下列主題：

* [記憶體內部快取](xref:performance/caching/memory)
* [使用分散式快取](xref:performance/caching/distributed)
* [快取中 ASP.NET Core MVC 標記協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)

## <a name="troubleshooting"></a>疑難排解

如果快取行為是未如預期般，，確認回應的快取，並且能夠提供從快取。 檢查要求的連入標頭和回應的傳出標頭。 啟用[記錄](xref:fundamentals/logging/index)來協助偵錯。

當測試及疑難排解快取行為，在瀏覽器可能設定要求標頭之影響快取不想要的方式。 例如，可能會設定瀏覽器`Cache-Control`標頭`no-cache`或`max-age=0`時重新整理頁面。 下列工具可以明確地將要求標頭，以及慣用發佈點進行測試快取：

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>快取的條件

* 要求必須產生伺服器回應 200 （確定） 狀態碼。
* 要求方法必須是 GET 或 HEAD。
* 終端機的中介軟體，例如[靜態檔案中介軟體](xref:fundamentals/static-files)，必須處理前回應快取中介軟體的回應。
* `Authorization`標頭不得存在。
* `Cache-Control`標頭參數必須是有效，而且必須標示為回應`public`且未標記為`private`。
* `Pragma: no-cache`標頭不能有如果`Cache-Control`標頭不是呈現為`Cache-Control`標頭會覆寫`Pragma`標頭時出現。
* `Set-Cookie`標頭不得存在。
* `Vary`標頭參數必須是有效且不等於`*`。
* `Content-Length`標頭值 (如果設定) 必須符合回應主體的大小。
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature)不會使用。
* 回應不是所指定過時`Expires`標頭和`max-age`和`s-maxage`快取指示詞。
* 回應緩衝處理必須先成功完成，且回應的大小必須小於設定或預設`SizeLimit`。
* 回應必須是根據可快取[RFC 7234](https://tools.ietf.org/html/rfc7234)規格。 例如，`no-store`指示詞不能存在於要求或回應標頭欄位。 請參閱*區段 3： 儲存在快取的回應*的[RFC 7234](https://tools.ietf.org/html/rfc7234)如需詳細資訊。

> [!NOTE]
> Antiforgery 系統是否有產生安全性權杖，以防止跨站台要求偽造 (CSRF) 攻擊集`Cache-Control`和`Pragma`標頭`no-cache`以便回應未快取。

## <a name="additional-resources"></a>其他資源

* [應用程式啟動](xref:fundamentals/startup)
* [中介軟體](xref:fundamentals/middleware)
* [記憶體內部快取](xref:performance/caching/memory)
* [使用分散式快取](xref:performance/caching/distributed)
* [使用變更權杖來偵測變更](xref:fundamentals/primitives/change-tokens)
* [回應快取](xref:performance/caching/response)
* [快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
