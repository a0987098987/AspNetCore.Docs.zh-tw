---
title: ASP.NET Core 的 URL 重寫中介軟體
author: guardrex
description: 了解如何使用 ASP.NET Core 的 URL 重寫中介軟體，進行 URL 重寫與重新導向作業。
ms.author: riande
ms.custom: mvc
ms.date: 11/19/2018
uid: fundamentals/url-rewriting
ms.openlocfilehash: 98787891a97e49081d72107484f030d216d82f45
ms.sourcegitcommit: ad28d1bc6657a743d5c2fa8902f82740689733bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "52256563"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a>ASP.NET Core 的 URL 重寫中介軟體

由 [Luke Latham](https://github.com/guardrex) 和 [Mikael Mengistu](https://github.com/mikaelm12) 提供

::: moniker range="<= aspnetcore-1.1"

如需本主題的 1.1 版，請下載 [ASP.NET Core 中的 URL 重寫中介軟體 (1.1 版，PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/URL_Rewriting_1.1.pdf)。

::: moniker-end

本文件介紹 URL 重寫，並提供如何在 ASP.NET Core 應用程式中使用 URL 重寫中介軟體的指示。

URL 重寫是指根據一或多個預先定義的規則來修改要求 URL 的動作。 URL 重寫會在資源位置和位址之間建立一個抽象層，讓位置和位址不那麼緊密連結。 下列幾種情況非常適合使用 URL 重寫：

* 暫時或永久性移動/取代伺服器資源，並為這些資源保持穩定的定位器時。
* 跨不同應用程式或跨單一應用程式各區域來分割要求處理作業。
* 移除、新增或重組傳入要求的 URL 區段。
* 針對搜尋引擎最佳化 (SEO) 來將公開 URL 最佳化。
* 允許使用易懂的公開 URL，來協助訪客預測要求資源所傳回的內容。
* 將不安全的要求重新導向至安全的端點。
* 防止直接連結，即外部網站透過將另一個網站的資產連結至本身的內容，來盜用該網站擁有的靜態資產。

> [!NOTE]
> URL 重寫可能會降低應用程式的效能。 如果可行的話，請限制規則的數目與複雜程度。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="url-redirect-and-url-rewrite"></a>URL 重新導向和 URL 重寫

乍看之下，「URL 重新導向」和「URL 重寫」在用語上的差異不太明顯，但卻對提供資源給用戶端方面有重要的影響。 ASP.NET Core 的 URL 重寫中介軟體可以同時滿足這兩種需求。

「URL 重新導向」牽涉有關用戶端的作業，其會指示用戶端到其他位址存取資源，而非用戶端原本要求的位址。 這項作業需要伺服器的來回行程。 當用戶端提出新的資源要求時，系統會將重新導向 URL 傳回給用戶端，並顯示在瀏覽器的網址列中。

若 `/resource` 重新導向 至 `/different-resource`，則伺服器會回應用戶端應前往 `/different-resource` 取得資源，並提供狀態碼，指出重新導向為暫時性或永久性。

![伺服器上的 WebAPI 服務端點已暫時從第 1 版 (v1) 變更為第 2 版 (v2)。 用戶端在第 1 版的 /v1/api 路徑提出服務要求。 伺服器會在第 2 版的 /v2/api 新服務暫存路徑傳回 302 (已找到) 回應。 用戶端即會在重新導向 URL 處提出第二個服務要求。 伺服器會回應 200 (確定) 狀態碼。](url-rewriting/_static/url_redirect.png)

將要求重新導向至不同的 URL 時，可以透過指定具狀態的回應，來指出該重新導向為永久性或暫時性：

* 如果資源具有新的永久 URL，且您希望告知用戶端所有未來的資源要求皆應使用這個新 URL，請使用 301 (已永久移動) 狀態碼。 用戶端收到 301 狀態碼時，可能會快取並重複使用回應。

* 若重新導向為暫時性或很有可能變更時，請使用 302 (已找到) 狀態碼。 302 狀態碼會指示用戶端不要儲存 URL 並在之後使用。

如需狀態碼的詳細資訊，請參閱 [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616：狀態碼定義)。

「URL 重寫」伺服器端作業，會從用戶端要求以外的其他資源位址提供資源。 重寫 URL 並不需要伺服器的來回行程。 系統不會將重寫的 URL 傳回用戶端，也不會顯示在瀏覽器的網址列中。

若 `/resource`「重寫」至 `/different-resource`，則伺服器會於「內部」擷取資源，並在 `/different-resource` 傳回資源。

雖然用戶端或許可以在重寫的 URL 處擷取資源，但用戶端並不會在提出要求與接收回應時得知資源位於重寫的 URL 處。

![伺服器上的 WebAPI 服務端點已從第 1 版 (v1) 變更為第 2 版 (v2)。 用戶端在第 1 版的 /v1/api 路徑提出服務要求。 系統會將要求 URL 重寫以存取第 2 版路徑 /v2/api 的服務。 服務會將 200 (確定) 狀態碼回應給用戶端。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a>URL 重寫範例應用程式

您可以利用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)，來探索 URL 重寫中介軟體的功能。 範例應用程式會套用重新導向與重寫規則，還會示範多種情況下的重新導向或重寫 URL。

## <a name="when-to-use-url-rewriting-middleware"></a>URL 重寫中介軟體的使用時機

當您無法使用以下方法時，請使用重寫中介軟體：

* [Windows Server 上的 IIS URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite)
* [Apache Server 上的 Apache mod_rewrite 模組](https://httpd.apache.org/docs/2.4/rewrite/)
* [Nginx 上的 URL 重寫](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)

此外也請在應用程式裝載於 [HTTP.sys server](xref:fundamentals/servers/httpsys) (前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 時使用中介軟體。

使用 IIS、Apache 和 Nginx 所提供伺服器型 URL 重寫技術的主要原因包括：

* 中介軟體不支援這些模組的完整功能。

  有部分伺服器模組的功能不適用於 ASP.NET Core 專案，例如 IIS Rewrite Module 規則的 `IsFile` 和 `IsDirectory` 條件約束。 在這些情況下，請改為使用中介軟體。
* 中介軟體的效能可能比不上模組的效能時。

  如果要確實得知哪種方法會降低最多效能，或是降低的效能可以忽略的話，進行效能評定是唯一方法。

## <a name="package"></a>Package

若要在您的專案中包含中介軟體，請在包含 [Microsoft.AspNetCore.Rewrite](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite) 套件的專案檔中，將套件參考新增至 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

不使用 `Microsoft.AspNetCore.App` 中繼套件時，請將專案參考新增至 `Microsoft.AspNetCore.Rewrite` 套件。

## <a name="extension-and-options"></a>延伸模組和選項

若要建立 URL 重寫和重新導向規則，您可以針對每個重寫規則的擴充方法建立 [RewriteOptions](xref:Microsoft.AspNetCore.Rewrite.RewriteOptions) 類別的執行個體。 依據所需的處理順序來鏈結多個規則。 系統會使用 <xref:Microsoft.AspNetCore.Builder.RewriteBuilderExtensions.UseRewriter*> 將 `RewriteOptions` 新增至要求管線，並將其傳入 URL 重寫中介軟體：

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

### <a name="redirect-non-www-to-www"></a>將非 www 重新導向 www

有三個選項可讓應用程式將非 `www` 要求重新導向 `www`：

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWwwPermanent*> &ndash; 將要求永久重新導向 `www` 子網域 (如果要求為非 `www`)。 使用 [Status308PermanentRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status308PermanentRedirect) 狀態代碼重新導向。

* <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToWww*> &ndash; 將要求重新導向 `www` 子網域 (如果要求為非 `www`)。 使用 [Status307TemporaryRedirect](xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect) 狀態代碼重新導向。 多載可讓您提供回應的狀態代碼。 請使用 <xref:Microsoft.AspNetCore.Http.StatusCodes> 類別的欄位來進行狀態碼指派。

### <a name="url-redirect"></a>URL 重新導向

使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirect*> 將要求重新導向。 第一個參數會包含您的 Regex，以比對傳入的 URL 路徑。 第二個參數是取代字串。 第三個參數 (如果有的話) 會指定狀態碼。 如果您未指定狀態碼，則狀態碼會預設為 302 (已找到)，表示已暫時移動或取代資源。

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=9)]

在啟用開發人員工具的瀏覽器中，使用 `/redirect-rule/1234/5678` 路徑提出範例應用程式的要求。 Regex 會比對 `redirect-rule/(.*)` 上的要求路徑，並會將路徑取代為 `/redirected/1234/5678`。 系統會將重新導向 URL 與 302 (已找到) 狀態碼傳回給用戶端。 瀏覽器在重新導向 URL 處提出新要求，該 URL 也會顯示在瀏覽器的網址列中。 因為範例應用程式與重新導向 URL 沒有任何相符規則：

* 第二個要求會從應用程式收到 200 (確定) 回應。
* 回應的本文會顯示重新導向 URL。

「重新導向」URL 時，會對伺服器進行來回行程。

> [!WARNING]
> 建立重新導向規則時，請務必謹慎。 每向應用程式提出要求時 (包括重新導向後)，請評估您的重新導向規則。 因為您有可能會不小心建立無限重新導向迴圈。

原始要求：`/redirect-rule/1234/5678`

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect.png)

括弧內所含的運算式部分稱為「擷取群組」。 運算式的點 (`.`) 表示「比對任何字元」。 星號 (`*`) 表示「比對前置字元零或多次」。 因此，擷取群組 `(.*)` 會擷取 URL 的 `1234/5678` 最後這兩個路徑區段。 這個單一擷取群組會擷取您在要求 URL 中的 `redirect-rule/` 之後所提供的任何值。

系統會將擷取的群組以貨幣符號 (`$`) 後接擷取序號的形式，插入取代字串中。 使用 `$1` 可取得第一個擷取群組值、使用 `$2` 則會取得第二個，依您的 Regex 擷取群組順序以此類推。 在範例應用程式中，重新導向規則 Regex 只有一個擷取的群組，因此在取代字串中只有 `$1` 這一個插入的群組。 套用規則時，URL 會變成 `/redirected/1234/5678`。

### <a name="url-redirect-to-a-secure-endpoint"></a>將 URL 重新導向至安全端點

使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttps*> 將 HTTP 要求重新導向至使用 HTTPS 通訊協定的相同主機與路徑。 如果未提供狀態碼，中介軟體會預設為 302 (已找到)。 若未提供連接埠：

* 中介軟體會預設為 `null`。
* 配置會變更為 `https` (HTTPS 通訊協定)，且用戶端會在連接埠 443 上存取資源。

下列範例示範了如何將狀態碼設為 301 (已永久移動)，並將連接埠變更為 5001。

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRedirectToHttpsPermanent*> 將不安全的要求重新導向至使用安全 HTTPS 通訊協定 (在連接埠 443 上) 的相同主機與路徑。 中介軟體會將狀態碼設定為 301 (已永久移動)。

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> 在不要求額外重新導向規則的情況下重新導向至安全的端點時，建議使用 HTTPS 重新導向中介軟體。 如需詳細資訊，請參閱[強制執行 HTTPS](xref:security/enforcing-ssl#require-https) 主題。

範例應用程式可以示範如何使用 `AddRedirectToHttps` 或 `AddRedirectToHttpsPermanent`。 將擴充方法新增至 `RewriteOptions`。 在任何 URL 位置，向應用程式提出不安全的要求。 關閉瀏覽器自我簽署憑證不受信任的安全性警告，或建立信任憑證的例外狀況。

使用 `AddRedirectToHttps(301, 5001)` 的原始要求：`http://localhost:5000/secure`

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https.png)

使用 `AddRedirectToHttpsPermanent` 的原始要求：`http://localhost:5000/secure`

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a>URL 重寫

使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.AddRewrite*> 來建立 URL 重寫的規則。 第一個參數會包含 Regex，以比對傳入的 URL 路徑。 第二個參數是取代字串。 第三個參數 `skipRemainingRules: {true|false}` 指出中介軟體是否要在套用目前規則時略過其他重寫規則。

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=10-11)]

原始要求：`/rewrite-rule/1234/5678`

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_rewrite.png)

運算式開頭的插入號 (`^`) 表示，比對會從 URL 路徑的開頭開始。

在先前的重新導向規則範例 `redirect-rule/(.*)` 中，Regex 的開頭沒有插入號 (`^`)。 因此，就算 `redirect-rule/` 前有任何字元也能成功比對。

| 路徑                               | 比對 |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | [是]   |
| `/my-cool-redirect-rule/1234/5678` | [是]   |
| `/anotherredirect-rule/1234/5678`  | [是]   |

`^rewrite-rule/(\d+)/(\d+)` 重寫規則只會比對開頭為 `rewrite-rule/` 的路徑。 請注意下表中的比對差異。

| 路徑                              | 比對 |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | [是]   |
| `/my-cool-rewrite-rule/1234/5678` | 否    |
| `/anotherrewrite-rule/1234/5678`  | 否    |

運算式的 `^rewrite-rule/` 部分之後，有 `(\d+)/(\d+)` 這兩個擷取群組。 `\d` 表示「比對數字」。 加號 (`+`) 表示「比對一或多個前置字元」。 因此，URL 必須包含某個數字，後接斜線與另一個數字。 這些擷取群組會以 `$1` 和 `$2` 形式插入重寫的 URL。 重寫規則的取代字串會將擷取的群組放入查詢字串中。 系統會重寫 `/rewrite-rule/1234/5678` 的要求路徑，以取得位於 `/rewritten?var1=1234&var2=5678` 的資源。 如果原始要求上有查詢字串，則會在重寫 URL 時予以保留。

這麼做就不需要伺服器的來回行程，即可取得資源。 如果資源存在，就會擷取該資源，並傳回 200 (確定) 狀態碼給用戶端。 因為系統並未重新導向用戶端，因此瀏覽器網址列中的 URL 不會變更。 用戶端也無法偵測到伺服器上發生 URL 重寫作業。

> [!NOTE]
> 因為比對規則是計算繁複的程序，且會增加應用程式的回應時間，所以請盡可能使用 `skipRemainingRules: true`。 如需最快速的應用程式回應：
>
> * 排序重寫規則；從最常比對的規則排到最不常比對的規則。
> * 當出現符合項目且不需要任何額外的規則處理時，即略過剩下的規則處理。

### <a name="apache-modrewrite"></a>Apache mod_rewrite

使用 <xref:Microsoft.AspNetCore.Rewrite.ApacheModRewriteOptionsExtensions.AddApacheModRewrite*> 來套用 Apache mod_rewrite 規則。 請確認應用程式已部署規則檔。 如需 mod_rewrite 規則的詳細資訊和範例，請參閱 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)。

系統會使用 <xref:System.IO.StreamReader> 來讀取 *ApacheModRewrite.txt* 規則檔案的規則：

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=3-4,12)]

範例應用程式會將要求從 `/apache-mod-rules-redirect/(.\*)` 重新導向至 `/redirected?id=$1`。 回應狀態碼為 302 (已找到)。

[!code[](url-rewriting/samples/2.x/SampleApp/ApacheModRewrite.txt)]

原始要求：`/apache-mod-rules-redirect/1234`

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_apache_mod_redirect.png)

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
* TIME
* TIME_DAY
* TIME_HOUR
* TIME_MIN
* TIME_MON
* TIME_SEC
* TIME_WDAY
* TIME_YEAR

### <a name="iis-url-rewrite-module-rules"></a>IIS URL Rewrite Module 規則

若要使用與套用至 IIS URL Rewrite Module 一樣的規則集，請使用 <xref:Microsoft.AspNetCore.Rewrite.IISUrlRewriteOptionsExtensions.AddIISUrlRewrite*>。 請確認應用程式已部署規則檔。 在 Windows Server IIS 上執行時，請不要引導中介軟體使用應用程式的 *web.config* 檔案。 使用 IIS 時，這些規則應該儲存在應用程式的 *web.config* 檔案外部，以免與 IIS Rewrite Module 發生衝突。 如需 IIS URL Rewrite Module 規則的詳細資訊和範例，請參閱 [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0) 和 [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)。

系統會使用 <xref:System.IO.StreamReader> 來讀取 *IISUrlRewrite.xml* 規則檔案的規則：

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5-6,13)]

範例應用程式會將要求從 `/iis-rules-rewrite/(.*)` 重寫至 `/rewritten?id=$1`。 系統會將回應與 200 (確定) 狀態碼傳送給用戶端。

[!code-xml[](url-rewriting/samples/2.x/SampleApp/IISUrlRewrite.xml)]

原始要求：`/iis-rules-rewrite/1234`

![瀏覽器視窗，其中的開發人員工具會追蹤要求和回應](url-rewriting/_static/add_iis_url_rewrite.png)

如果您具備使用中且已設定伺服器層級規則的 IIS Rewrite Module，但其會以非預期的方式影響應用程式，則您可以停用應用程式的 IIS Rewrite Module 。 如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。

#### <a name="unsupported-features"></a>不支援的功能

與 ASP.NET Core 2.x 一起發行的中介軟體不支援下列 IIS URL Rewrite Module 功能：

* 輸出規則
* 自訂伺服器變數
* 萬用字元
* LogRewrittenUrl

#### <a name="supported-server-variables"></a>支援的伺服器變數

中介軟體可支援下列 IIS URL Rewrite Module 伺服器變數：

* CONTENT_LENGTH
* CONTENT_TYPE
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
> 您也可以透過 <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 取得 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。 這種方法可讓重寫規則檔案的位置更有彈性。 請確認重寫規則檔案已部署到您所提供之路徑的伺服器。
>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a>以方法為基礎的規則

使用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 以在方法中實作您自己的規則邏輯。 `Add` 會公開 <xref:Microsoft.AspNetCore.Rewrite.RewriteContext>，使 <xref:Microsoft.AspNetCore.Http.HttpContext> 可用於您的方法中。 [RewriteContext.Result](xref:Microsoft.AspNetCore.Rewrite.RewriteContext.Result*)可判斷其他管線處理的執行方式。 請將值設定為下表中描述的其中一個 <xref:Microsoft.AspNetCore.Rewrite.RuleResult> 欄位。

| `RewriteContext.Result`              | 動作                                                           |
| ------------------------------------ | ---------------------------------------------------------------- |
| `RuleResult.ContinueRules` (預設值) | 繼續套用規則。                                         |
| `RuleResult.EndResponse`             | 停止套用規則，並傳送回應。                       |
| `RuleResult.SkipRemainingRules`      | 停止套用規則，並將內容傳送至下一個中介軟體。 |

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=14)]

範例應用程式會示範一個方法，以重新導向對 *.xml* 結尾之路徑的要求。 若要求是針對 `/file.xml` 發出，則要求會重新導向至 `/xmlfiles/file.xml`。 狀態碼會設定為 301 (已永久移動)。 當瀏覽器針對 */xmlfiles/file.xml*發出新的要求時，靜態檔案中介軟體會從 *wwwroot/xmlfiles* 資料夾將檔案提供給用戶端。 若要重新導向，請明確設定回應的狀態碼。 否則會傳回 200 (確定) 狀態碼，用戶端上也不會發生重新導向。

*RewriteRules.cs*：

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectXmlFileRequests&highlight=14-18)]

這種方法也能重寫要求。 範例應用程式示範了如何重寫任何文字檔要求的路徑，以從 *wwwroot* 資料夾提供 *file.txt* 文字檔。 靜態檔案中介軟體會根據更新的要求路徑來提供檔案：

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=15,22)]

*RewriteRules.cs*：

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RewriteTextFileRequests&highlight=7-8)]

### <a name="irule-based-rule"></a>以 IRule 為基礎的規則

利用 <xref:Microsoft.AspNetCore.Rewrite.RewriteOptionsExtensions.Add*> 來使用類別中實作 <xref:Microsoft.AspNetCore.Rewrite.IRule> 介面的專屬規則邏輯。 相較於使用以方法為基礎的規則方法，`IRule` 提供了更高彈性。 您的實作類別可以包含建構函式，以便您在其中傳入 <xref:Microsoft.AspNetCore.Rewrite.IRule.ApplyRule*> 方法的參數。

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=16-17)]

為了滿足若干條件，系統會檢查範例應用程式中的 `extension` 和 `newPath` 參數值。 `extension` 必須包含值，而且值必須是 *.png*、*.jpg* 或 *.gif*。 如果 `newPath` 無效，就會擲回 <xref:System.ArgumentException>。 若要求是針對 *image.png* 發出，則要求會重新導向至 `/png-images/image.png`。 若要求是針對 *image.jpg* 發出，則要求會重新導向至 `/jpg-images/image.jpg`。 狀態碼會設定為 301 (已永久移動)，而 `context.Result` 會設定為停止處理規則，並傳送回應。

[!code-csharp[](url-rewriting/samples/2.x/SampleApp/RewriteRules.cs?name=snippet_RedirectImageRequests)]

原始要求：`/image.png`

![瀏覽器視窗，其中的開發人員工具會追蹤 image.png 的要求和回應](url-rewriting/_static/add_redirect_png_requests.png)

原始要求：`/image.jpg`

![瀏覽器視窗，其中的開發人員工具會追蹤 image.jpg 的要求和回應](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a>Regex 範例

| Goal | Regex 字串及<br>比對範例 | 取代字串及<br>輸出範例 |
| ---- | ------------------------------- | -------------------------------------- |
| 將路徑重寫成查詢字串 | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| 移除斜線 | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| 強制使用斜線 | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| 避免重寫特定的要求 | `^(.*)(?<!\.axd)$` 或 `^(?!.*\.axd$)(.*)$`<br>是：`/resource.htm`<br>否：`/resource.axd` | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| 重新排列 URL 區段 | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| 取代 URL 區段 | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a>其他資源

* [應用程式啟動](startup.md)
* [中介軟體](xref:fundamentals/middleware/index)
* [.NET 中的規則運算式](/dotnet/articles/standard/base-types/regular-expressions)
* [規則運算式語言 - 快速參考](/dotnet/articles/standard/base-types/quick-ref)
* [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)
* [Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (使用 URL Rewrite Module 2.0 (適用於 IIS))
* [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (URL Rewrite Module 組態參考)
* [IIS URL Rewrite Module 論壇](https://forums.iis.net/1152.aspx)
* [Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (保持精簡的 URL 結構)
* [10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 個重寫 URL 的祕訣與技巧)
* [To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (是否要使用斜線)
