---
title: "快取中 ASP.NET Core 中的介軟體的回應"
author: guardrex
description: "設定及使用回應快取中介軟體中 ASP.NET Core 應用程式。"
keywords: "ASP.NET Core 回應快取，快取，ResponseCache，ResponseCaching，快取控制、 VaryByQueryKeys、 中介軟體"
ms.author: riande
manager: wpickett
ms.date: 08/22/2017
ms.topic: article
ms.assetid: f9267eab-2762-42ac-1638-4a25d2c9d67c
ms.prod: asp.net-core
uid: performance/caching/middleware
ms.openlocfilehash: 07626ae7f40dc6f704d69d71cb7f95d318e6f503
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2017
---
# <a name="response-caching-middleware-in-aspnet-core"></a>快取中 ASP.NET Core 中的介軟體的回應

由[Luke Latham](https://github.com/guardrex)和[John Luo](https://github.com/JunTaoLuo)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples)

本文件提供有關如何設定 ASP.NET Core 應用程式中的回應快取中介軟體的詳細資料。 中介軟體會決定當回應的快取、 儲存區回應，以及做回應從快取。 如需簡介 HTTP 快取和`ResponseCache`屬性，請參閱[快取回應](response.md)。

## <a name="package"></a>Package
若要在專案中包含中介軟體，將參考加入[ `Microsoft.AspNetCore.ResponseCaching` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)封裝，或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)封裝。

## <a name="configuration"></a>組態
在`ConfigureServices`，將中介軟體新增至服務集合。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

設定應用程式使用中的介軟體`UseResponseCaching`擴充方法，將中介軟體新增至處理管線的要求。 範例應用程式將[ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2)標頭至回應的快取的回應會快取多達 10 秒。 此範例傳送[ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4)標頭設定的中介軟體提供快取回的應才[ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4)後續要求的標頭與相符的原始要求。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](middleware/samples/2.x/Program.cs?name=snippet1&highlight=8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](middleware/samples/1.x/Startup.cs?name=snippet2&highlight=3)]

---

回應快取中介軟體只會快取 200 （確定） 伺服器的回應。 任何其他回應，包括[錯誤網頁](xref:fundamentals/error-handling)中, 介軟體會被忽略。

> [!WARNING]
> 回應包含內容的已驗證的用戶端必須標示為不可快取，以防止從儲存和處理這些回應的中介軟體。 請參閱[快取的條件](#conditions-for-caching)如需詳細資訊中, 介軟體如何判斷是否可快取的回應。

## <a name="options"></a>選項
中介軟體提供三個選項來控制回應快取。

| 選項                | 預設值 |
| --------------------- | ------------- |
| UseCaseSensitivePaths | 決定是否區分大小寫的路徑上快取的回應。</p><p>預設值是 `false`。 |
| MaximumBodySize       | 回應主體，以位元組為單位的最大的可快取大小。</p>預設值是`64 * 1024 * 1024`(64 MB)。 |
| SizeLimit             | 回應快取中介軟體，以位元組為單位的大小限制。 預設值是`100 * 1024 * 1024`(100 MB)。 |

下列範例會設定來快取回應小於或等於 1024 個位元組，使用區分大小寫的路徑，將儲存至回應的中介軟體`/page1`和`/Page1`分開。

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys
當使用 MVC、`ResponseCache`屬性會指定所需的設定適當的標頭，為快取回應的參數。 唯一的參數`ResponseCache`絕對需要中, 介軟體的屬性是`VaryByQueryKeys`，這並不對應到實際的 HTTP 標頭。 如需詳細資訊，請參閱[ResponseCache 屬性](response.md#responsecache-attribute)。

當不使用 MVC，您可以變更回應快取`VaryByQueryKeys`功能。 使用`ResponseCachingFeature`直接從`IFeatureCollection`的`HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

## <a name="http-headers-used-by-response-caching-middleware"></a>回應快取中介軟體所使用的 HTTP 標頭
中介軟體的快取的回應設定透過 HTTP 標頭。 以下列出相關的標頭與它們如何影響快取的注意事項。

| 頁首 | 詳細資料 |
| ------ | ------- |
| 授權 | 如果標頭存在，回應不是快取。 |
| 快取控制 | 中介軟體只會考慮快取回應標記為`public`快取的指示詞。 您可以控制快取包含下列參數：<ul><li>保留時間上限</li><li>最大過時 &#8224;</li><li>min 全新</li><li>必須指令</li><li>無快取</li><li>無存放區</li><li>僅限 if-快取</li><li>private</li><li>public</li><li>s maxage</li><li>proxy 指令 &#8225;</li></ul>&#8224; 若要指定無限制到`max-stale`中, 介軟體會採取任何動作。<br>&#8225;`proxy-revalidate`具有相同的效果`must-revalidate`。<br><br>如需詳細資訊，請參閱[RFC 7231： 要求的快取控制指示詞](https://tools.ietf.org/html/rfc7234#section-5.2.1)。 |
| Pragma | A`Pragma: no-cache`要求標頭會產生相同的效果`Cache-Control: no-cache`。 此標頭中的相關指示詞會覆寫`Cache-Control`標頭，如果有的話。 與 HTTP/1.0 回溯相容性考量。 |
| 設定 Cookie | 如果標頭存在，回應不是快取。 |
| 而有所不同 | `Vary`標頭由另一個標頭用來變更快取的回應。 比方說，您可以快取回應編碼包含`Vary: Accept-Encoding`快取回應之要求標頭的標頭`Accept-Encoding: gzip`和`Accept-Encoding: text/plain`分開。 回應標頭值是`*`絕對不會儲存。 |
| 到期 | 此標頭視為過時的回應不是存放或擷取除非覆寫其他`Cache-Control`標頭。 |
| 如果 If-none-Match | 如果此值不是從快取提供完整的回應`*`和`ETag`的回應不符合任何提供的值。 否則，提供 304 （未修改） 的回應。 |
| 如果修改自 | 如果`If-None-Match`標頭不存在，如果快取的回應日期比所提供的值從快取提供完整的回應。 否則，提供 304 （未修改） 的回應。 |
| 日期 | 當服務從快取，`Date`若未提供原始回應上設定標頭中介軟體。 |
| 內容長度 | 當服務從快取，`Content-Length`若未提供原始回應上設定標頭中介軟體。 |
| 存留期 | `Age`原始回應中傳送的標頭會被忽略。 當服務快取的回應中, 介軟體會計算新值。 |

## <a name="troubleshooting"></a>疑難排解
如果快取行為是未如預期般，，確認回應的快取，並且可以從快取由檢查要求的連入標頭和回應的傳出標頭。 啟用[記錄](xref:fundamentals/logging)有助於偵錯時。 快取行為，並回應從快取的擷取時此中介軟體記錄。

當測試及疑難排解快取行為，在瀏覽器可能設定要求標頭之影響快取不想要的方式。 例如，可能會設定瀏覽器`Cache-Control`標頭`no-cache`時重新整理頁面。 下列工具可以明確地將要求標頭，以及慣用發佈點進行測試快取：

* [Fiddler](http://www.telerik.com/fiddler)
* [Firebug 這類](http://getfirebug.com/)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>快取的條件
* 要求必須導致伺服器 200 （確定） 回應。
* 要求方法必須是 GET 或 HEAD。
* 終端機中的介軟體，例如靜態檔案中介軟體，必須處理前回應快取中介軟體的回應。
* `Authorization`標頭不得存在。
* `Cache-Control`標頭參數必須是有效，而且必須標示為回應`public`且未標記為`private`。
* `Pragma: no-cache`標頭/值不能存在如果`Cache-Control`標頭不是呈現為`Cache-Control`標頭會覆寫`Pragma`標頭時出現。
* `Set-Cookie`標頭不得存在。
* `Vary`標頭參數必須是有效且不等於`*`。
* `Content-Length`標頭值 (如果設定) 必須符合回應主體的大小。
* [IHttpSendFileFeature](/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature)不會使用。
* 回應不是所指定過時`Expires`標頭和`max-age`和`s-maxage`快取指示詞。
* 回應緩衝處理是否成功，並回應的大小會小於所設定，或預設`SizeLimit`。
* 回應必須是根據可快取[RFC 7234](https://tools.ietf.org/html/rfc7234)規格。 例如，`no-store`指示詞不能存在於要求或回應標頭欄位。 請參閱*區段 3： 儲存在快取的回應*的[RFC 7234](https://tools.ietf.org/html/rfc7234)如需詳細資訊。

> [!NOTE]
> Antiforgery 系統是否有產生安全性權杖，以防止跨站台要求偽造 (CSRF) 攻擊集`Cache-Control`和`Pragma`標頭`no-cache`以便回應未快取。

## <a name="additional-resources"></a>其他資源

* [應用程式啟動](xref:fundamentals/startup)
* [中介軟體](xref:fundamentals/middleware)
