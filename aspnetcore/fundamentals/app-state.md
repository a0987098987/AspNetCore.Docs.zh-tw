---
title: ASP.NET Core 中的工作階段與應用程式狀態
author: rick-anderson
description: 在要求之間保留應用程式和使用者 (工作階段) 狀態的方法。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 887aefdeaa45957f7b95bfe8df342eb34d267e3a
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2018
---
# <a name="session-and-application-state-in-aspnet-core"></a>ASP.NET Core 中的工作階段與應用程式狀態

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/) 和 [Diana LaRose](https://github.com/DianaLaRose)

HTTP 是無狀態的通訊協定。 Web 伺服器將每個 HTTP 要求視為獨立要求，而不會保留上一個要求中的使用者值。 本文將討論在要求之間保留應用程式和工作階段狀態的不同方式。

## <a name="session-state"></a>工作階段狀態

在 ASP.NET Core 中的工作階段狀態功能，可用於在使用者瀏覽您的 Web 應用程式時，儲存及存放使用者資料。 工作階段狀態由伺服器上的字典或雜湊資料表所組成，可從瀏覽器跨要求保存資料。 快取支援工作階段資料。

ASP.NET Core 可維護工作階段狀態，方法是提供包含工作階段識別碼的 Cookie 給用戶端，以便將其隨著每個要求傳送至伺服器。 伺服器則使用工作階段識別碼來擷取工作階段資料。 因為工作階段 Cookie 是瀏覽器所特有，所以您無法跨瀏覽器共用工作階段。 只有在瀏覽器工作階段結束時，才會刪除工作階段 Cookie。 如果收到過期工作階段的 Cookie，則會建立使用相同工作階段 Cookie 的新工作階段。

伺服器會在最後一個要求之後，保留工作階段一段有限的時間。 請設定工作階段逾時或使用預設值 20 分鐘。 工作階段狀態適合用來儲存特定工作階段特有的使用者資料，但不需要永久保存。 呼叫 `Session.Clear` 時或資料存放區中的工作階段到期時，就會從支援存放區中刪除資料。 伺服器並不知道何時會關閉瀏覽器或刪除工作階段 Cookie。

> [!WARNING]
> 請勿將機密資料存放在工作階段。 用戶端可能不會關閉瀏覽器並清除工作階段 Cookie (而某些瀏覽器會將工作階段 Cookie 跨 Windows 保持運作)。 此外，工作階段可能無法限制為單一使用者；下一位使用者可能會繼續使用相同的工作階段。

記憶體內部工作階段提供者會在本機伺服器上儲存工作階段資料。 如果您打算在伺服器陣列上執行 Web 應用程式，則必須使用黏性工作階段將每個工作階段繫結到特定伺服器。 Windows Azure 網站平台預設為黏性工作階段 (應用程式要求路由或 ARR)。 不過，黏性工作階段可能會影響延展性，並使 Web 應用程式更新複雜化。 較好的選項是使用 Redis 或 SQL Server 分散式快取，這不需要黏性工作階段。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。 如需設定服務提供者的詳細資料，請參閱本文稍後的[設定工作階段](#configuring-session)。

## <a name="tempdata"></a>TempData

ASP.NET Core MVC 公開[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)上的 [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 屬性。 這個屬性會儲存資料，直到讀取為止。 `Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。 當有多個要求需要資料時，`TempData` 對重新導向很有幫助。 `TempData` 是 TempData 提供者的實作；例如，使用 Cookie 或工作階段狀態。

### <a name="tempdata-providers"></a>TempData 提供者

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

在 ASP.NET Core 2.0 和更新版本中，預設會使用 Cookie 架構 TempData 提供者，將 TempData 儲存在 Cookie 中。

Cookie 資料是使用 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密，並使用 [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) 編碼，然後分成區塊。 因為 Cookie 分成區塊，所以不適用 ASP.NET Core 1.x 中找到的 Cookie 大小限制。 Cookie 資料不會壓縮，因為壓縮加密資料可能會導致安全性問題，例如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻擊。 如需 Cookie 架構 TempData 提供者的詳細資訊，請參閱 [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在 ASP.NET Core 1.0 和 1.1 中，工作階段狀態 TempData 提供者是預設值。

---

### <a name="choosing-a-tempdata-provider"></a>選擇 TempData 提供者

選擇 TempData 提供者涉及數項考量，例如：

1. 應用程式是否已將工作階段狀態用於其他用途？ 如果是的話，使用工作階段狀態 TempData 提供者沒有額外的應用程式成本 (除了資料的大小之外)。
2. 應用程式是否盡量只將 TempData 用於相對少量的資料 (最多 500 個位元組)？ 如果是的話，Cookie TempData 提供者將對包含 TempData 的每個要求新增少量成本。 如果不是的話，則工作階段狀態 TempData 提供者可能有助於避免在每個要求中來回傳送大量資料，直到取用 TempData 為止。
3. 應用程式是否在 Web 伺服陣列 (多部伺服器) 中執行？ 如果是的話，使用 Cookie TempData 提供者不需要其他組態。

> [!NOTE]
> 大部分的 Web 用戶端 (例如網頁瀏覽器) 會強制執行每個 Cookie 的大小上限、Cookie 總數或這兩者的限制。 因此，使用 Cookie TempData 提供者時，請確認應用程式不會超過這些限制。 請考慮資料大小總計，並考量加密和區塊處理的額外負荷。

### <a name="configure-the-tempdata-provider"></a>設定 TempData 提供者

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

預設會啟用 Cookie 架構 TempData 提供者。 下列 `Startup` 類別程式碼會設定工作階段架構 TempData 提供者：

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

下列 `Startup` 類別程式碼會設定工作階段架構 TempData 提供者：

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

順序對中介軟體元件來說很重要。 在上述範例中，如果在 `UseMvcWithDefaultRoute` 之後叫用 `UseSession`，則會發生 `InvalidOperationException`　類型的例外狀況。 如需詳細資料，請參閱[中介軟體順序](xref:fundamentals/middleware/index#ordering)。

> [!IMPORTANT]
> 如果目標為 .NET Framework 且使用工作階段架構提供者，請將 [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet 套件新增至您的專案。

## <a name="query-strings"></a>查詢字串

您可以將數量有限的資料從某個要求傳遞到另一個要求，方法是將其新增至新要求的查詢字串。 這對於以持續方式擷取狀態很有用，可讓內嵌狀態的連結透過電子郵件或社交網路共用。 不過，基於這個原因，您永遠不應該針對敏感性資料使用查詢字串。 除了輕鬆共用之外，在查詢字串中包括資料還可能會為[跨網站偽造要求 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻擊創造機會，這些攻擊可能會誘騙使用者在驗證時瀏覽惡意網站。 然後，攻擊者可能會竊取應用程式中的使用者資料，或代表使用者採取惡意動作。 任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。 如需詳細資訊，請參閱[防止跨網站偽造要求 (XSRF/CSRF) 攻擊](xref:security/anti-request-forgery)。

## <a name="post-data-and-hidden-fields"></a>張貼資料和隱藏欄位

資料可以儲存在隱藏的表單欄位，並貼回下一個要求。 這在多頁表單中很常見。 不過，因為用戶端可能會竄改資料，所以伺服器必須一律重新驗證它。

## <a name="cookies"></a>Cookie

Cookie 提供在 Web 應用程式中儲存使用者特定資料的方法。 因為 Cookie 會隨著每個要求傳送，所以其大小應該保持最小。 在理想情況下，應該只有識別碼儲存在 Cookie 中，而實際資料儲存在伺服器上。 大部分的瀏覽器將 Cookie 限制為 4096 個位元組。 此外，每個網域只有數量有限的 Cookie 可供使用。

由於 Cookie 可能會遭到竄改，因此必須在伺服器上加以驗證。 雖然用戶端上的 Cookie 持久性受限於使用者介入與到期日，但它們通常是用戶端上最持久的資料持續性形式。

Cookie 通常可用於個人化，其中內容會針對已知的使用者自訂。 因為在大部分情況下，只會識別使用者而不會驗證，所以您通常可以藉由在 Cookie 中儲存使用者名稱、帳戶名稱或唯一使用者識別碼 (例如 GUID) 來保護 Cookie。 然後，該Cookie 可用來存取網站的使用者個人化基礎結構。

## <a name="httpcontextitems"></a>HttpContext.Items

要儲存只有處理某個特定要求時所需的資料，`Items` 集合是個不錯的位置。 集合的內容會在每個要求之後捨棄。 當元件或中介軟體在要求期間的不同時間點運作，而且沒有可傳遞參數的直接方式時，`Items` 集合是元件或中介軟體用來通訊的最佳方式。 如需詳細資訊，請參閱本文稍後的[使用 HttpContext.Items](#working-with-httpcontextitems)。

## <a name="cache"></a>快取

快取是儲存和擷取資料的有效方式。 您可以依據時間和其他考量控制快取項目的存留期。 深入了解[如何快取](../performance/caching/index.md)。

## <a name="working-with-session-state"></a>使用工作階段狀態

### <a name="configuring-session"></a>設定工作階段

`Microsoft.AspNetCore.Session` 套件提供用來管理工作階段狀態的中介軟體。 若要啟用工作階段中介軟體，`Startup` 必須包含：

- 任一 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 記憶體快取。 `IDistributedCache` 實作會作為工作階段的支援存放區。
- [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 呼叫，這需要 NuGet 套件 "Microsoft.AspNetCore.Session"。
- [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) 呼叫。

下列程式碼示範如何設定記憶體內部工作階段提供者。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

在其安裝並設定之後，您就可以從 `HttpContext` 參考工作階段。

如果在已呼叫 `UseSession` 之前嘗試存取 `Session`，就會擲回 `InvalidOperationException: Session has not been configured for this application or request` 例外狀況。

如果在已經開始寫入 `Response` 資料流之後，嘗試建立新的 `Session` (也就是未建立任何工作階段 Cookie)，則會擲回 `InvalidOperationException: The session cannot be established after the response has started` 例外狀況。 此例外狀況可以在網頁伺服器記錄檔中找到；它不會顯示在瀏覽器中。

### <a name="loading-session-asynchronously"></a>非同步載入工作階段

只有在 `TryGetValue`、`Set` 或 `Remove` 方法之前明確呼叫 [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) 方法時，ASP.NET Core 中的預設工作階段提供者才會從基礎 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 存放區以非同步方式載入工作階段記錄。 如果並未先呼叫 `LoadAsync`，則基礎工作階段記錄會同步載入，這可能會影響應用程式的擴展能力。

若要讓應用程式強制執行此模式，請使用未在 `TryGetValue`、`Set` 或 `Remove` 之前呼叫 `LoadAsync` 方法時擲回例外狀況的版本來包裝 [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) 和 [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 實作。 請在服務容器中註冊已包裝的版本。

### <a name="implementation-details"></a>實作詳細資料

工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。 此 Cookie 預設名為 ".AspNet.Session"，並使用路徑 "/"。 由於 Cookie 預設值未指定網域，因此它不會提供給頁面上的用戶端指令碼 (因為 `CookieHttpOnly` 預設為 `true`)。

若要覆寫工作階段的預設值，請使用 `SessionOptions`：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

伺服器會使用 `IdleTimeout` 屬性，判斷工作階段可以在放棄其內容之前閒置多長時間。 這個屬性與 Cookie 到期日無關。 透過工作階段中介軟體傳遞 (讀取或寫入) 的每個要求會重設逾時。

因為 `Session` 為「非鎖定」，如果兩個要求同時嘗試修改工作階段的內容，則最後一個內容會覆寫第一個內容。 `Session` 會實作為「一致性工作階段」，這表示所有內容會都儲存在一起。 要修改工作階段不同部分 (不同的索引鍵) 的兩個要求仍可能會彼此影響。

### <a name="set-and-get-session-values"></a>設定和取得工作階段值

工作階段會使用 `Context.Session` 從 Razor 頁面或檢視進行存取：

[!code-cshtml[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Views/Home/About.cshtml)]

工作階段是使用 `HttpContext.Session` 從 `PageModel` 類別或控制器進行存取。 這個屬性是 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 實作。

下列範例示範如何設定和取得整數及字串：

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

如果新增下列擴充方法，您可以設定和取得工作階段的可序列化物件：

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

下列範例示範如何設定和取得可序列化物件：

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]

## <a name="working-with-httpcontextitems"></a>使用 HttpContext.Items

`HttpContext` 抽象支援 `IDictionary<object, object>` 類型的字典集合，稱為 `Items`。 此集合可在 *HttpRequest* 的開頭取得，並於每個要求的結尾處捨棄。 透過將值指派給索引鍵項目，或要求特定索引鍵的值，即可存取它。

在下列範例中，[中介軟體](xref:fundamentals/middleware/index)會將 `isVerified` 新增至 `Items` 集合。

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

稍後在管線中，另一個中介軟體可以存取它：

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " +
        context.Items["isVerified"]);
});
```

如果是只由單一應用程式使用的中介軟體，可接受 `string` 索引鍵。 不過，將在應用程式之間共用的中介軟體應該使用唯一物件索引鍵，以避免任何可能發生的索引鍵衝突。 如果您要開發必須跨多個應用程式運作的中介軟體，請使用中介軟體類別中定義的唯一物件索引鍵，如下所示：

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

其他程式碼可以使用中介軟體類別所公開的索引鍵，來存取 `HttpContext.Items` 中儲存的值：

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

此方法也有在程式碼的多個位置中消除重複的魔術字串 (magic string) 的優點。

## <a name="application-state-data"></a>應用程式狀態資料

請使用[相依性插入](xref:fundamentals/dependency-injection)將資料提供給所有使用者：

1. 定義包含資料的服務 (例如，名為 `MyAppData` 的類別)。

    ```csharp
    public class MyAppData
    {
        // Declare properties/methods/etc.
    } 
    ```

2. 將服務類別新增至 `ConfigureServices` (例如 `services.AddSingleton<MyAppData>();`)。

3. 在每個控制器中取用資料服務類別：

    ```csharp
    public class MyController : Controller
    {
        public MyController(MyAppData myService)
        {
            // Do something with the service (read some data from it, 
            // store it in a private field/property, etc.)
        }
    } 
    ```

## <a name="common-errors-when-working-with-session"></a>使用工作階段時的常見錯誤

* 「嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore' 時，無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 。」

  這種情形通常是因為無法設定至少一個 `IDistributedCache` 實作。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[記憶體內部快取](xref:performance/caching/memory)。

* 如果工作階段中介軟體無法保存工作階段 (例如：如果資料庫無法使用)，它會記錄例外狀況並抑制它。 然後要求將繼續正常運作，這會導致完全無法預期的行為。

典型的範例：

有人會在工作階段中儲存購物籃。 使用者新增一個項目，但認可失敗。 應用程式未察覺到失敗，因此它會報告「已新增項目」訊息，但這並不正確。

檢查是否有這類錯誤的建議方式是，當您完成寫入工作階段後，從應用程式程式碼呼叫 `await feature.Session.CommitAsync();`。 然後，您就可以對錯誤執行想要的操作。 這與呼叫 `LoadAsync` 時的運作方式相同。

### <a name="additional-resources"></a>其他資源

* [ASP.NET Core 1.x：本文件中使用的程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x：本文件中使用的程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
