---
title: "URL 重寫中 ASP.NET Core 中的介軟體"
author: guardrex
description: "簡介 URL 重寫，並將重新導向，其中包含有關如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。"
keywords: "ASP.NET Core URL 重寫，URL 重寫 URL 重新導向 URL 重新導向中, 介軟體，apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 44c78a6304eacc70cdee9bb0d9407376017abcac
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>URL 重寫中 ASP.NET Core 中的介軟體

由[Luke Latham](https://github.com/guardrex)和[黃冠馬力](https://github.com/mikaelm12)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)

URL 重寫是指修改的要求 Url 會根據一或多個預先定義的規則。 URL 重寫會建立資源位置和地址之間的抽象概念，讓不緊密連結的位置和地址。 有幾種的情況很重要 URL 重寫：
* 移動或取代伺服器資源暫時或永久地維持穩定的定位器，針對這些資源
* 分割的要求處理跨不同的應用程式或跨區域的一個應用程式
* 移除、 加入或重新組織的連入要求的 URL 區段
* 最佳化公用 Url 搜尋引擎最佳化 (SEO)
* 允許使用易記的公用 Url，以協助使用者在預測他們會依照下列連結找到的內容
* 將保護端點不安全的要求重新導向
* 防止 hotlinking 映像

您可以定義規則來變更數種方式的 URL，包括 regex、 Apache mod_rewrite 模組規則、 IIS 重寫模組規則和使用自訂規則的邏輯。 本文件介紹 URL 重寫有關如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。

> [!NOTE]
> URL 重寫可能會降低應用程式的效能。 在可行的時候，您應該限制的數目與複雜度的規則。

## <a name="url-redirect-and-url-rewrite"></a>重新導向 URL 和 URL 重寫
用語之間的差異*URL 重新導向*和*URL 重寫*可能看起來很難以察覺在第一次，但是擁有資源提供給用戶端重要的影響。 ASP.NET Core URL 重寫中介軟體都可以達成兩個需要。

A *URL 重新導向*是用戶端的作業，其中會指示用戶端在另一個位址存取資源。 這需要往返伺服器，並傳回至用戶端重新導向 URL 出現在瀏覽器的網址列時用戶端提出新要求的資源。 如果`/resource`是*重新導向*至`/different-resource`，用戶端要求`/resource`，伺服器會回應，用戶端應該取得的資源和`/different-resource`狀態程式碼，表示重新導向為與暫時或永久的。 用戶端執行新的資源重新導向 URL 的要求。

![WebAPI 服務端點已暫時變更從版本 1 (v1) 為伺服器上的版本 2 (v2)。 用戶端在第 1 版路徑 /v1/api 服務提出要求。 伺服器會在第 2 版 /v2/api 傳回 302 （找到） 回應服務的新的暫存路徑。 用戶端會對服務在重新導向 URL 中的第二個要求。 伺服器會回應 200 （確定） 」 狀態碼。](url-rewriting/_static/url_redirect.png)

將要求重新導向至不同的 URL，當您指出是否永久或暫時重新導向。 301 （已永久移動） 狀態碼使用資源具有新的永久的 URL，而且您想以指示用戶端的所有未來的要求資源應該使用新的 URL。 *用戶端收到 301 狀態程式碼時，可能會快取的回應。* 302 （找到） 狀態碼會重新導向其中是暫存或通常主旨變更，使得用戶端不應儲存，並在未來重複使用重新導向 URL。 如需詳細資訊，請參閱[RFC 2616： 狀態碼定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)。

A *URL 重寫*是伺服器端作業，以提供不同的資源位址中的資源。 重寫 URL，並不需要往返伺服器。 重寫的 URL 不傳回至用戶端，並不會出現在瀏覽器的網址列。 當`/resource`是*重寫*至`/different-resource`，用戶端要求`/resource`，與伺服器*內部*擷取資源時`/different-resource`。 雖然用戶端可能會無法擷取在重寫 URL 資源，用戶端將不會收到通知時，它會建立其要求並接收回應的資源存在於重寫 URL。

![WebAPI 服務端點已從版本 1 (v1) 變更為伺服器上的版本 2 (v2)。 用戶端在第 1 版路徑 /v1/api 服務提出要求。 要求 URL 重寫存取第 2 版路徑 /v2/api 的服務。 服務會回應 200 （確定） 」 狀態碼的用戶端。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL 重寫的範例應用程式
您可以瀏覽的 URL 重寫中介軟體功能[URL 重寫的範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)。 應用程式適用重新寫入和重新導向規則，並顯示重寫或重新導向 URL。

## <a name="when-to-use-url-rewriting-middleware"></a>使用 URL 重寫中介軟體的時機
當您無法使用，請使用 URL 重寫中介軟體[URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite)與 Windows Server 上的 IIS [Apache mod_rewrite 模組](https://httpd.apache.org/docs/2.4/rewrite/)Apache 在伺服器上， [URL 重寫 Nginx上](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)，或者您的應用程式裝載於[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)(先前稱為[WebListener](xref:fundamentals/servers/weblistener))。 若要使用的伺服器為基礎的 URL 重寫技術 IIS、 Apache 或 Nginx 中的主要原因是中, 介軟體不支援這些模組的完整功能，和中介軟體的效能可能不符合模組。 不過，有一些功能，不使用 ASP.NET Core 專案，例如伺服器模組`IsFile`和`IsDirectory`IIS Rewrite module 的條件約束。 在這些情況下，請改為使用中介軟體。

## <a name="package"></a>Package
若要在專案中包含中介軟體，將參考加入[ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)封裝。 這項功能是適用於 ASP.NET Core 1.1 版為目標的應用程式或更新版本。

## <a name="extension-and-options"></a>擴充功能和選項
建立您的 URL 重寫，並重新導向規則建立的執行個體`RewriteOptions`與每個規則的擴充方法的類別。 鏈結多個規則，您想要處理它們的順序。 `RewriteOptions`將加入至要求管線傳入 URL 重寫中介軟體`app.UseRewriter(options);`。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a>重新導向 URL
使用`AddRedirect`將要求重新導向。 第一個參數會包含您的 regex 進行比對傳入 URL 的路徑上。 第二個參數是取代字串。 第三個參數，如果有的話，指定的狀態碼。 如果您未指定的狀態碼，則預設為 302 （找到），表示已暫時移動或取代資源。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

使用開發人員工具啟用瀏覽器，提出要求的路徑範例應用程式`/redirect-rule/1234/5678`。 Regex 上相符的要求路徑`redirect-rule/(.*)`，而且路徑取代`/redirected/1234/5678`。 重新導向 URL 會傳送回用戶端以 302 （找到） 狀態碼。 瀏覽器重新導向 URL，它會出現在瀏覽器的網址列在提出新要求。 範例應用程式中的任何規則不比對重新導向 URL，因為第二個要求收到回應 200 （確定），從應用程式，並將回應主體會顯示重新導向 URL。 URL 時對伺服器提出往返*重新導向*。

> [!WARNING]
> 建立重新導向規則時，請務必謹慎。 重新導向規則會評估每個應用程式，其中包括後重新導向的要求。 很容易意外建立無限重新導向迴圈。

原始要求：`/redirect-rule/1234/5678`

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_redirect.png)

包含在括號內的運算式部份稱為*擷取群組*。 點 (`.`) 的運算式表示*比對任何字元*。 星號 (`*`) 表示*零或多次比對前一個字元*。 因此，URL 的最後兩個路徑區段`1234/5678`，擷取群組所擷取`(.*)`。 您在之後的要求 URL 中提供的任何值`redirect-rule/`此單一擷取群組所擷取。

取代字串中的擷取的群組會插入貨幣符號的字串 (`$`) 後面接著擷取的序號。 取得第一個擷取群組值`$1`、 與第二個`$2`，而且會持續在您的 regex 的擷取群組的順序。 只有一個擷取的群組中沒有重新導向規則 regex 範例應用程式，因此在取代字串中，這是只有一個插入的群組`$1`。 套用規則時，URL 會成為`/redirected/1234/5678`。

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a>安全端點的 URL 重新導向
使用`AddRedirectToHttps`將 HTTP 要求重新導向至相同的主機，並使用 HTTPS 的路徑 (`https://`)。 如果未提供的狀態碼中, 介軟體會預設為 302 （找到）。 如果未提供連接埠中, 介軟體會預設為`null`，這表示通訊協定變更為`https://`和用戶端會存取連接埠 443 上的資源。 此範例示範如何設定狀態碼 301 （已永久移動） 到，並將連接埠變更為 5001。
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
使用`AddRedirectToHttpsPermanent`將不安全的要求重新導向至相同的主機，並使用安全的 HTTPS 通訊協定的路徑 (`https://`連接埠 443)。 中介軟體設定的狀態碼 301 （已永久移動）。

範例應用程式是否能夠示範如何使用`AddRedirectToHttps`或`AddRedirectToHttpsPermanent`。 Add 擴充方法，以`RewriteOptions`。 執行任何 URL 的應用程式會對不安全的要求。 關閉瀏覽器安全性警告的自我簽署的憑證不受信任。

原始要求使用`AddRedirectToHttps(301, 5001)`:`/secure`

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_redirect_to_https.png)

原始要求使用`AddRedirectToHttpsPermanent`:`/secure`

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL 重寫
使用`AddRewrite`建立 Url 重寫規則。 第一個參數會包含您的 regex 的比對連入的 URL 路徑。 第二個參數是取代字串。 第三個參數， `skipRemainingRules: {true|false}`，指示中介軟體，就不會略過其他重寫規則，如果套用目前的規則。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

原始要求：`/rewrite-rule/1234/5678`

![瀏覽器視窗中使用追蹤要求和回應的開發人員工具](url-rewriting/_static/add_rewrite.png)

您會注意到 regex 中第一件事是克拉記號 (`^`) 運算式的開頭。 這表示，比對會從 URL 路徑的開頭。

重新導向規則，在前面範例中`redirect-rule/(.*)`，在 regex 開始處沒有任何克拉記號; 因此，任何字元可能會在`redirect-rule/`之路徑的成功比對。

| 路徑                               | 比對 |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | 是   |
| `/my-cool-redirect-rule/1234/5678` | 是   |
| `/anotherredirect-rule/1234/5678`  | 是   |

請重寫規則， `^rewrite-rule/(\d+)/(\d+)`，只比對路徑，開頭為`rewrite-rule/`。 請注意的差異比對下列重寫規則與上述的重新導向規則。

| 路徑                              | 比對 |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | 是   |
| `/my-cool-rewrite-rule/1234/5678` | 否    |
| `/anotherrewrite-rule/1234/5678`  | 否    |

遵循`^rewrite-rule/`部份的運算式，包含兩個擷取群組， `(\d+)/(\d+)`。 `\d`表示*比對的數字 （數字）*。 加號 (`+`) 表示*符合一或多個前置字元*。 因此，此 URL 必須包含後面接著斜線後面接著另一個數字的數字。 這些群組會插入做為 URL 重寫的擷取`$1`和`$2`。 重寫規則的取代字串會將擷取的群組放入查詢字串。 要求的路徑的`/rewrite-rule/1234/5678`取得的資源重寫`/rewritten?var1=1234&var2=5678`。 如果查詢字串是原始要求上呈現，則會保留在重寫 URL 時。

沒有任何往返到伺服器，以取得資源。 如果資源存在，它具有讀取，並傳回 200 (OK) 狀態碼的用戶端。 因為用戶端未重新導向，則不會變更瀏覽器網址列中的 URL。 當用戶端是而言，URL 重寫永遠不會發生作業。

> [!NOTE]
> 使用`skipRemainingRules: true`可能的話，因為比對規則是高度耗費資源的程序，並減少應用程式回應時間。 最快速的應用程式回應：
> * 訂購至少經常比對規則通常比對規則重寫規則。
> * 當出現相符，而且需要任何額外的規則處理時，略過剩餘的規則的處理。

### <a name="apache-modrewrite"></a>Apache mod_rewrite
適用於使用 Apache mod_rewrite 規則`AddApacheModRewrite`。 請確定規則檔與應用程式部署。 如需詳細資訊和 mod_rewrite 規則的範例，請參閱[Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A`StreamReader`用於讀取從規則*ApacheModRewrite.txt*規則檔案。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

第一個參數會採用`IFileProvider`，這會透過提供[相依性插入](dependency-injection.md)。 `IHostingEnvironment`插入提供`ContentRootFileProvider`。 第二個參數是您規則的檔案，也就是路徑*ApacheModRewrite.txt*範例應用程式。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

範例應用程式將要求重新導向`/apache-mod-rules-redirect/(.\*)`至`/redirected?id=$1`。 回應狀態碼為 302 （找到）。

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

原始要求：`/apache-mod-rules-redirect/1234`

![瀏覽器視窗中使用追蹤的要求和回應的開發人員工具](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a>支援的伺服器變數
中介軟體可支援下列 Apache mod_rewrite 伺服器變數：
* CONN_REMOTE_ADDR
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_FORWARDED
* HTTP_HOST
* HTTP_REFERER
* HTTP_USER_AGENT
* HTTPS
* IPV6
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_METHOD
* REQUEST_SCHEME
* REQUEST_URI
* SCRIPT_FILENAME
* SERVER_ADDR
* SERVER_PORT
* SERVER_PROTOCOL
* 時間
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>IIS URL Rewrite 模組規則
若要使用適用於 IIS URL Rewrite 模組的規則， `AddIISUrlRewrite`。 請確定規則檔與應用程式部署。 不直接的中介軟體来使用您*web.config*檔案時在 Windows Server IIS 上執行。 使用 IIS 時，應該儲存這些規則的外部程式*web.config*以避免與 IIS Rewrite module 發生衝突。 如需詳細資訊和 IIS URL Rewrite 模組規則的範例，請參閱[使用 Url 重寫模組 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)和[URL Rewrite 模組組態參考](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

A`StreamReader`用於讀取從規則*IISUrlRewrite.xml*規則檔案。

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

第一個參數會採用`IFileProvider`，而第二個參數則是 XML 規則檔案，也就是路徑*IISUrlRewrite.xml*範例應用程式。

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

範例應用程式重寫從要求`/iis-rules-rewrite/(.*)`至`/rewritten?id=$1`。 回應會傳送至用戶端以 200 （確定） 狀態碼。

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

原始要求：`/iis-rules-rewrite/1234`

![瀏覽器視窗中使用追蹤要求和回應的開發人員工具](url-rewriting/_static/add_iis_url_rewrite.png)

如果您有使用中的 IIS 重寫模組的伺服器層級規則設定，會影響您的應用程式非預期的方式，您可以停用應用程式的 IIS Rewrite Module。 如需詳細資訊，請參閱[停用 IIS 模組](xref:hosting/iis-modules#disabling-iis-modules)。

#### <a name="unsupported-features"></a>不支援的功能

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

發行的中介軟體與 ASP.NET Core 2.x 不支援下列 IIS URL Rewrite Module 功能：
* 輸出規則
* 自訂伺服器變數
* 萬用字元
* LogRewrittenUrl

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

發行的中介軟體與 ASP.NET Core 1.x 不支援下列 IIS URL Rewrite Module 功能：
* 全域規則
* 輸出規則
* 請重寫對應
* CustomResponse 動作
* 自訂伺服器變數
* 萬用字元
* CustomResponse 動作：
* LogRewrittenUrl

---

#### <a name="supported-server-variables"></a>支援的伺服器變數
中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：
* CONTENT_LENGTH
* 有效
* HTTP_ACCEPT
* HTTP_CONNECTION
* HTTP_COOKIE
* HTTP_HOST
* HTTP_REFERER
* HTTP_URL
* HTTP_USER_AGENT
* HTTPS
* LOCAL_ADDR
* QUERY_STRING
* REMOTE_ADDR
* REMOTE_PORT
* REQUEST_FILENAME
* REQUEST_URI

> [!NOTE]
> 您也可以取得`IFileProvider`透過`PhysicalFileProvider`。 規則檔案時，這種方法可能會提供更靈活彈性，您的重寫的位置。 請確定您重寫規則的檔案會部署到您提供的路徑的伺服器。
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>以方法為基礎的規則
使用`Add(Action<RewriteContext> applyRule)`來實作您自己的規則邏輯的方法中。 `RewriteContext`公開`HttpContext`以便用於您的方法。 `context.Result`判斷其他管線處理。

| 內容。結果                       | 動作                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| `RuleResult.ContinueRules` (預設值) | 繼續套用規則                                         |
| `RuleResult.EndResponse`             | 停止套用規則，並傳送回應                       |
| `RuleResult.SkipRemainingRules`      | 停止套用規則，並將內容傳送至下一個中介軟體 |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

範例應用程式會示範一個方法重新導向要求的路徑結尾*.xml*。 如果您為要求`/file.xml`，它會重新導向至`/xmlfiles/file.xml`。 狀態碼會設定為 301 （已永久移動）。 重新導向，您必須明確設定的狀態碼的回應。否則，會傳回 200 （確定） 」 狀態碼，並重新導向不會發生在用戶端。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

原始要求：`/file.xml`

![瀏覽器視窗中使用追蹤的要求和回應的 file.xml 的開發人員工具](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a>IRule 型規則
使用`Add(IRule)`衍生自的類別中實作您自己的規則邏輯`IRule`。 使用`IRule`使用以方法為基礎的規則方法相比，提供更大的彈性。 您的衍生的類別可能包括建構函式，您可以傳遞參數中`ApplyRule`方法。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

範例應用程式中的參數值`extension`和`newPath`會檢查，以符合數個條件。 `extension`必須包含值，而且此值必須是*.png*， *.jpg*，或*.gif*。 如果`newPath`不是有效的`ArgumentException`就會擲回。 如果您為要求*image.png*，它會重新導向至`/png-images/image.png`。 如果您為要求*image.jpg*，它會重新導向至`/jpg-images/image.jpg`。 狀態碼 301 （已永久移動），來設定和`context.Result`設停止處理規則，並傳送回應。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

原始要求：`/image.png`

![瀏覽器視窗中使用追蹤的要求和回應的 image.png 的開發人員工具](url-rewriting/_static/add_redirect_png_requests.png)

原始要求：`/image.jpg`

![瀏覽器視窗中使用追蹤的要求和回應的 image.jpg 的開發人員工具](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Regex 範例

| Goal | Regex 字串 （& s)<br>比對範例 | 取代字串 （& s)<br>輸出範例 |
| ---- | :-----------------------------: | :------------------------------------: |
| 重新撰寫成查詢字串的路徑 | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| 寬帶尾端斜線 | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| 強制執行結尾的斜線 | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| 避免重新撰寫特定的要求 | `(.*[^(\.axd)])$`<br>[是]:`/resource.htm`<br>否：`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| 重新排列 URL 區段 | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| 取代 URL 區段 | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>其他資源
* [應用程式啟動](startup.md)
* [中介軟體](middleware.md)
* [.NET 中的規則運算式](/dotnet/articles/standard/base-types/regular-expressions)
* [規則運算式語言 - 快速參考](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [使用 Url Rewrite 模組 2.0 （適用於 IIS)](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [URL 重寫模組的組態參考](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [IIS URL 重寫模組論壇](https://forums.iis.net/1152.aspx)
* [保持簡單的 URL 結構](https://support.google.com/webmasters/answer/76329?hl=en)
* [10 的 URL 重寫的秘訣和訣竅](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [斜線或不進行斜線](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
