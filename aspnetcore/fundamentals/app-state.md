---
title: ASP.NET Core 中的會話
author: rick-anderson
description: 探索在要求之間保留會話的方法。
ms.author: riande
ms.custom: mvc
ms.date: 03/06/2020
no-loc:
- SignalR
uid: fundamentals/app-state
ms.openlocfilehash: 0cf75c14e09744907af926f0ec314801efeb3023
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083274"
---
# <a name="session-and-state-management-in-aspnet-core"></a>ASP.NET Core 中的會話和狀態管理

::: moniker range=">= aspnetcore-3.0"

由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Kirk Larkin](https://twitter.com/serpent5)和[Diana LaRose](https://github.com/DianaLaRose)

HTTP 是無狀態的通訊協定。 根據預設，HTTP 要求是不會保留使用者值的獨立訊息。 本文說明在要求之間保留使用者資料的數種方法。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/app-state/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="state-management"></a>狀態管理

可以使用數種方法來儲存狀態。 本主題稍後將提供每種方法的描述。

| 儲存方法 | 儲存機制 |
| ---------------- | ----------------- |
| [Cookie](#cookies) | HTTP cookie。 可能包含使用伺服器端應用程式程式碼所儲存的資料。 |
| [工作階段狀態](#session-state) | HTTP Cookie 和伺服器端應用程式程式碼 |
| [TempData](#tempdata) | HTTP Cookie 或工作階段狀態 |
| [查詢字串](#query-strings) | HTTP 查詢字串 |
| [隱藏欄位](#hidden-fields) | HTTP 表單欄位 |
| [HttpContext.Items](#httpcontextitems) | 伺服器端應用程式程式碼 |
| [Cache](#cache) | 伺服器端應用程式程式碼 |

## <a name="cookies"></a>Cookies

Cookie 會在要求之間儲存資料。 因為 Cookie 會隨著每個要求傳送，所以其大小應該保持最小。 在理想情況下，應該只有識別碼儲存在 Cookie 中，而資料由應用程式儲存。 大部分的瀏覽器將 Cookie 大小限制為 4096 個位元組。 每個網域只有數量有限的 Cookie 可供使用。

由於 Cookie 可能會遭到竄改，因此必須由應用程式加以驗證。 使用者可以刪除 Cookie，而且 Cookie 會在用戶端上過期。 不過，Cookie 通常是在用戶端上資料持續性最持久的形式。

Cookie 通常可用於個人化，其中內容會針對已知的使用者自訂。 在大部分情況下，只會識別使用者，而未加以驗證。 Cookie 可以儲存使用者的名稱、帳戶名稱或唯一的使用者識別碼，例如 GUID。 Cookie 可以用來存取使用者的個人化設定，例如其慣用的網站背景色彩。

發行 cookie 並處理隱私權考慮時，請參閱[歐盟一般資料保護規定（GDPR）](https://ec.europa.eu/info/law/law-topic/data-protection) 。 如需詳細資訊，請參閱 [ASP.NET Core 中的一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。

## <a name="session-state"></a>工作階段狀態

工作階段狀態是用來在使用者瀏覽 Web 應用程式時存放使用者資料的 ASP.NET Core 情節。 工作階段狀態使用應用程式所維護的存放區，在用戶端的要求之間保存資料。 會話資料是由快取所支援，並被視為暫時資料。 網站應該在沒有會話資料的情況下繼續運作。 重要應用程式資料應該儲存在使用者資料庫，並只在工作階段中快取以獲得效能最佳化。

[SignalR](xref:signalr/index) 應用程式中不支援工作階段，因為 [SignalR 中樞](xref:signalr/hubs)可獨立於 HTTP 內容之外而執行。 例如，當長時間輪詢要求由中樞維持開啟，超過要求的 HTTP 內容存留期時，便可能發生此情況。

ASP.NET Core 會藉由提供 cookie 給包含會話識別碼的用戶端來維護會話狀態。 Cookie 會話識別碼：

* 會使用每個要求傳送至應用程式。
* 應用程式會使用來提取會話資料。

工作階段狀態表現下列行為：

* 會話 cookie 是瀏覽器特有的。 會話不會在瀏覽器之間共用。
* 在瀏覽器工作階段結束時，會刪除工作階段 Cookie。
* 如果收到過期工作階段的 Cookie，則會建立使用相同工作階段 Cookie 的新工作階段。
* 空白會話不會保留。 會話必須至少設定一個值，才能在要求之間保存會話。 不保留工作階段時，會為每個新的要求產生新的工作階段識別碼。
* 應用程式會在最後一個要求之後，保留工作階段一段有限的時間。 應用程式會設定工作階段逾時或使用預設值 20 分鐘。 會話狀態適合用來儲存使用者資料：
  * 專屬於特定的會話。
  * 其中的資料不需要跨會話的永久儲存體。
* 工作階段資料會在呼叫 [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) 實作或工作階段過期時刪除。
* 沒有任何預設機制可通知應用程式程式碼，用戶端瀏覽器已關閉，或工作階段 Cookie 遭到刪除或在用戶端上已過期。
* 根據預設，會話狀態 cookie 不會標示為必要。 除非網站訪客允許追蹤，否則會話狀態無法運作。 如需詳細資訊，請參閱 <xref:security/gdpr#tempdata-provider-and-session-state-cookies-arent-essential>。

> [!WARNING]
> 請勿將敏感性資料存放在工作階段狀態。 使用者可能不會關閉瀏覽器，並清除工作階段 Cookie。 某些瀏覽器會在瀏覽器視窗之間維護有效的工作階段 Cookie。 會話可能無法限制為單一使用者。 下一個使用者可能會繼續流覽具有相同會話 cookie 的應用程式。

記憶體中快取提供者會將工作階段資料存放在應用程式所在伺服器的記憶體中。 在伺服器陣列案例中：

* 使用「黏性工作階段」將每個工作階段繫結至個別伺服器上的特定應用程式執行個體。 [Azure App Service](https://azure.microsoft.com/services/app-service/) 預設會使用[應用程式要求路由 (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) 來強制執行自黏工作階段。 不過，黏性工作階段可能會影響延展性，並使 Web 應用程式更新複雜化。 較好的方法是使用 Redis 或 SQL Server 分散式快取，這不需要黏性工作階段。 如需詳細資訊，請參閱 <xref:performance/caching/distributed>。
* 工作階段 Cookie 是透過 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密。 必須正確設定資料保護，以閱讀每一部機器上的工作階段 Cookie。 如需詳細資訊，請參閱 <xref:security/data-protection/introduction>與[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。

### <a name="configure-session-state"></a>設定工作階段狀態

[AspNetCore 會話](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/)封裝：

* 由架構隱含包含。
* 提供用來管理會話狀態的中介軟體。

若要啟用工作階段中介軟體，`Startup` 必須包含：

* 任一 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 記憶體快取。 `IDistributedCache` 實作會作為工作階段的支援存放區。 如需詳細資訊，請參閱 <xref:performance/caching/distributed>。
* 在 [ 中呼叫 ](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession)AddSession`ConfigureServices`。
* 在 [ 中呼叫 ](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions.usesession#Microsoft_AspNetCore_Builder_SessionMiddlewareExtensions_UseSession_Microsoft_AspNetCore_Builder_IApplicationBuilder_)UseSession`Configure`。

下列程式碼示範如何設定記憶體內部工作階段提供者，以及 `IDistributedCache` 的預設記憶體中實作。

[!code-csharp[](app-state/samples/3.x/SessionSample/Startup4.cs?name=snippet1&highlight=12-19,39)]

上述程式碼會設定簡短的時間，以簡化測試。

中介軟體的順序很重要。  在 `UseRouting` 和 `UseEndpoints`之前呼叫 `UseSession`。 請參閱[中介軟體順序](xref:fundamentals/middleware/index#order)。

設定工作階段狀態之後便可以使用 [HttpContext.Session](xref:Microsoft.AspNetCore.Http.HttpContext.Session)。

呼叫 `HttpContext.Session` 之前無法存取 `UseSession`。

應用程式開始寫入回應資料流之後，無法建立具有新工作階段 Cookie 的新工作階段。 例外狀況會記錄在 Web 伺服器記錄檔中，而不會顯示在瀏覽器。

### <a name="load-session-state-asynchronously"></a>非同步載入工作階段狀態

只有在 [TryGetValue](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache)、[Set](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) 或 [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue) 方法之前明確呼叫 [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.set) 方法時，ASP.NET Core 中的預設工作階段提供者才會從基礎 [IDistributedCache](/dotnet/api/microsoft.aspnetcore.http.isession.remove) 備份存放區以非同步方式載入工作階段記錄。 如果並未先呼叫 `LoadAsync`，則基礎工作階段記錄會同步載入，這可能大規模地為效能帶來負面影響。

若要讓應用程式強制執行此模式，請使用未在 [、](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) 或 [ 之前呼叫 ](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 方法時擲回例外狀況的版本來包裝 `LoadAsync`DistributedSessionStore`TryGetValue` 和 `Set`DistributedSession`Remove` 實作。 請在服務容器中註冊已包裝的版本。

### <a name="session-options"></a>工作階段選項

若要覆寫工作階段的預設值，請使用 [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions)。

| 選項 | 描述 |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | 決定用來建立 Cookie 的設定。 [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) 預設為 [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`)。 [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) 預設為 [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`)。 [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) 預設為 [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`)。 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`。 [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) 預設為 `false`。 |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` 指出工作階段可以閒置多久，之後才會放棄它的內容。 每個工作階段存取都會重設逾時。 此設定只適用於工作階段的內容，而非 Cookie。 預設值是 20 分鐘。 |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | 從存放區載入工作階段，或將它認可回到存放區時，所允許的時間長度上限。 此設定只可能適用於非同步作業。 此逾時可以使用 [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan) 來停用。 預設值是 1 分鐘。 |

工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。 此 Cookie 預設名為 `.AspNetCore.Session`，並使用路徑 `/`。 由於 Cookie 預設值未指定網域，因此它不會提供給頁面上的用戶端指令碼 (因為 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`)。

若要覆寫 Cookie 工作階段的預設值，請使用 <xref:Microsoft.AspNetCore.Builder.SessionOptions>：

[!code-csharp[](app-state/samples/3.x/SessionSample/Startup2.cs?name=snippet1&highlight=5-10)]

應用程式會使用 [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) 屬性，判斷工作階段可以在放棄伺服器快取中的內容之前閒置多長時間。 這個屬性與 Cookie 到期日無關。 透過[工作階段中介軟體](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware)傳遞的每個要求都會重設逾時。

工作階段狀態為「非鎖定」。 如果兩個要求同時嘗試修改工作階段的內容，則最後一個要求會覆寫第一個要求。 `Session` 會實作為「一致性工作階段」，這表示所有內容會都儲存在一起。 當兩個要求試圖修改不同的工作階段值時，最後一個要求可能會覆寫第一個要求所做的工作階段變更。

### <a name="set-and-get-session-values"></a>設定和取得工作階段值

工作階段狀態的存取，是從 Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 類別，或 MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) 類別搭配 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。 這個屬性是 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 實作。

`ISession` 實作提供數個延伸模組來設定和擷取整數和字串值。 擴充方法位於[AspNetCore](/dotnet/api/microsoft.aspnetcore.http)命名空間中。

`ISession` 擴充方法：

* [Get(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32(ISession, String, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString(ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

下列範例會在 Razor Pages 頁面中，擷取 `IndexModel.SessionKeyName` 索引鍵的工作階段值 (範例應用程式中的 `_Name`)：

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

下列範例示範如何設定及取得整數和字串：

[!code-csharp[](app-state/samples/3.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

若要啟用分散式快取案例，即使是使用記憶體中快取時，都必須序列化所有工作階段資料。 字串和整數序列化程式是由[ISession](/dotnet/api/microsoft.aspnetcore.http.isession)的擴充方法所提供）。 複雜類型必須由使用者使用另一個機制加以序列化，例如 JSON。

使用下列範例程式碼來序列化物件：

[!code-csharp[](app-state/samples/3.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

下列範例顯示如何使用 `SessionExtensions` 類別來設定和取得可序列化物件：

[!code-csharp[](app-state/samples/3.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="tempdata"></a>TempData

ASP.NET Core 會公開 Razor Pages [TempData](xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.TempData)或控制器 <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>。 這個屬性會儲存資料，直到讀取另一個要求為止。 在要求結束時，可以使用[保留（字串）](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Keep*)和[查看（字串）](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Peek*)方法來檢查資料，而不需要刪除。 [保留](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Keep*)將字典中的所有專案標示為保留。 `TempData` 是：

* 適用于需要超過單一要求的資料時重新導向。
* 由使用 cookie 或會話狀態 `TempData` 提供者所執行。

## <a name="tempdata-samples"></a>TempData 範例

請考慮下列會建立客戶的頁面：

[!code-csharp[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet&highlight=15-16,30)]

下列頁面會顯示 `TempData["Message"]`：

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/IndexPeek.cshtml?range=1-14)]

在上述標記中，在要求結束時，**不**會刪除 `TempData["Message"]`，因為會使用 `Peek`。 重新整理頁面會顯示 `TempData["Message"]`的內容。

下列標記與上述程式碼類似，但使用 `Keep` 在要求結束時保留資料：

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/IndexKeep.cshtml?range=1-14)]

在 [ *IndexPeek* ] 和 [ *IndexKeep* ] 頁面之間流覽並不會 `TempData["Message"]`刪除。

下列程式碼會顯示 `TempData["Message"]`，但在要求結束時，`TempData["Message"]` 會被刪除：

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/Index.cshtml?range=1-14)]

### <a name="tempdata-providers"></a>TempData 提供者

預設會使用以 Cookie 為基礎的 TempData 提供者，以將 TempData 儲存在 Cookie 中。

Cookie 資料是使用 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密，並使用 [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) 編碼，然後分成區塊。 由於加密和區塊化的緣故，cookie 大小的最大值小於[4096 個位元組](http://www.faqs.org/rfcs/rfc2965.html)。 Cookie 資料不會壓縮，因為壓縮加密資料可能會導致安全性問題，例如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻擊。 如需 Cookie 架構 TempData 提供者的詳細資訊，請參閱 [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider)。

### <a name="choose-a-tempdata-provider"></a>選擇 TempData 提供者

選擇 TempData 提供者涉及數項考量，例如：

* 應用程式已經使用工作階段狀態了嗎？ 若是如此，使用會話狀態 TempData 提供者對應用程式而言，不會超過資料大小的額外成本。
* 應用程式是否只針對相對較少量的資料（最多500個位元組）使用 TempData？ 如果是的話，Cookie TempData 提供者將對包含 TempData 的每個要求新增少量成本。 如果不是的話，則工作階段狀態 TempData 提供者可能有助於避免在每個要求中來回傳送大量資料，直到取用 TempData 為止。
* 應用程式在伺服器陣列中的多部伺服器上執行？ 如果是的話，不需要額外組態，即可在資料保護之外使用 Cookie TempData 提供者 (請參閱 <xref:security/data-protection/introduction>與[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers))。

大部分的 web 用戶端（例如網頁瀏覽器）都會強制限制每個 cookie 的大小上限，以及 cookie 的總數目。 使用 cookie TempData 提供者時，請確認應用程式不會超過[這些限制](http://www.faqs.org/rfcs/rfc2965.html)。 請考慮資料的大小總計。 請考慮因為加密和區塊處理而增加的 Cookie 大小。

### <a name="configure-the-tempdata-provider"></a>設定 TempData 提供者

預設會啟用 Cookie 架構 TempData 提供者。

若要啟用以會話為基礎的 TempData 提供者，請使用[AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider)擴充方法。 只需要 `AddSessionStateTempDataProvider` 的一次呼叫：

[!code-csharp[](app-state/samples/3.x/SessionSample/Startup3.cs?name=snippet1&highlight=4,6,30)]

## <a name="query-strings"></a>查詢字串

可以將數量有限的資料從某個要求傳遞到另一個要求，方法是將其新增至新要求的查詢字串。 這對於以持續方式擷取狀態很有用，可讓內嵌狀態的連結透過電子郵件或社交網路共用。 因為 URL 查詢字串為公用，所以請絕對不要使用查詢字串來處理敏感性資料。

除了非預期的共用之外，包含查詢字串中的資料可能會將應用程式公開給[跨網站偽造要求（CSRF）](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))攻擊。 任何保留的會話狀態都必須防範 CSRF 攻擊。 如需詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊](xref:security/anti-request-forgery)。

## <a name="hidden-fields"></a>隱藏欄位

資料可以儲存在隱藏的表單欄位，並貼回下一個要求。 這在多頁表單中很常見。 因為用戶端可能會竄改資料，所以應用程式必須一律重新驗證存放在隱藏欄位中的資料。

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) 集合用來在處理單一要求時存放資料。 集合的內容會在每個要求處理之後捨棄。 當元件或中介軟體在要求期間的不同時間點運作，而且沒有可傳遞參數的直接方式時，`Items` 集合經常用來允許元件或中介軟體進行通訊。

在下列範例中，[中介軟體](xref:fundamentals/middleware/index)會將 `isVerified` 新增至 `Items` 集合：

[!code-csharp[](app-state/samples/3.x/SessionSample/Startup.cs?name=snippet1)]

針對只在單一應用程式中使用的中介軟體，已修正可接受 `string` 金鑰。 應用程式之間共用的中介軟體應使用唯一的物件索引鍵，以避免發生按鍵衝突。 下列範例示範如何使用中介軟體類別中定義的唯一物件索引鍵：

[!code-csharp[](app-state/samples/3.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

其他程式碼可以使用中介軟體類別所公開的索引鍵，來存取 `HttpContext.Items` 中儲存的值：

[!code-csharp[](app-state/samples/3.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

此方法也有在程式碼中消除使用索引鍵字串的優點。

## <a name="cache"></a>快取

快取是儲存和擷取資料的有效方式。 應用程式可以控制快取項目的存留期。 如需詳細資訊，請參閱 <xref:performance/caching/response>。

快取的資料未與特定要求、使用者或工作階段建立關聯。 **不要快取其他使用者要求可能會抓取的使用者特定資料。**

若要快取整個應用程式的資料，請參閱 <xref:performance/caching/memory>。

## <a name="common-errors"></a>常見錯誤

* 「嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore' 時，無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 。」

  這通常是因為無法設定至少一個 `IDistributedCache` 的執行所造成。 如需詳細資訊，請參閱<xref:performance/caching/distributed>和<xref:performance/caching/memory>。

如果會話中介軟體無法保存會話：

* 中介軟體會記錄例外狀況，要求會繼續正常執行。
* 這會導致無法預期的行為。

如果備份存放區無法使用，則會話中介軟體可能無法保存會話。 例如，使用者在工作階段中存放購物車。 使用者在購物車新增一個項目，但認可失敗。 應用程式未察覺到失敗，因此它向使用者報告項目已新增至購物車，但這並不正確。

檢查錯誤的建議方法是在應用程式完成寫入會話時呼叫 `await feature.Session.CommitAsync`。 備份存放區無法使用時，<xref:Microsoft.AspNetCore.Http.ISession.CommitAsync*> 會擲回例外狀況。 如果 `CommitAsync` 失敗，應用程式可以處理例外狀況。 當資料存放區無法使用時，<xref:Microsoft.AspNetCore.Http.ISession.LoadAsync*> 會在相同的情況下擲回。
  
## <a name="signalr-and-session-state"></a>SignalR 和會話狀態

SignalR apps 不應使用會話狀態來儲存資訊。 SignalR apps 可以在中樞的 `Context.Items` 中，儲存每個線上狀態。 <!-- https://github.com/aspnet/SignalR/issues/2139 -->

## <a name="additional-resources"></a>其他資源

<xref:host-and-deploy/web-farm>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)、[Diana LaRose](https://github.com/DianaLaRose) 和 [Luke Latham](https://github.com/guardrex)

HTTP 是無狀態的通訊協定。 若不採取其他步驟，HTTP 要求是獨立的訊息，不會保留使用者的值或應用程式狀態。 本文描述數種方法來在要求之間保留使用者資料和應用程式狀態。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/app-state/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="state-management"></a>狀態管理

可以使用數種方法來儲存狀態。 本主題稍後將提供每種方法的描述。

| 儲存方法 | 儲存機制 |
| ---------------- | ----------------- |
| [Cookie](#cookies) | HTTP cookie (可能包含使用伺服器端應用程式程式碼所儲存的資料) |
| [工作階段狀態](#session-state) | HTTP Cookie 和伺服器端應用程式程式碼 |
| [TempData](#tempdata) | HTTP Cookie 或工作階段狀態 |
| [查詢字串](#query-strings) | HTTP 查詢字串 |
| [隱藏欄位](#hidden-fields) | HTTP 表單欄位 |
| [HttpContext.Items](#httpcontextitems) | 伺服器端應用程式程式碼 |
| [Cache](#cache) | 伺服器端應用程式程式碼 |
| [相依性插入](#dependency-injection) | 伺服器端應用程式程式碼 |

## <a name="cookies"></a>Cookies

Cookie 會在要求之間儲存資料。 因為 Cookie 會隨著每個要求傳送，所以其大小應該保持最小。 在理想情況下，應該只有識別碼儲存在 Cookie 中，而資料由應用程式儲存。 大部分的瀏覽器將 Cookie 大小限制為 4096 個位元組。 每個網域只有數量有限的 Cookie 可供使用。

由於 Cookie 可能會遭到竄改，因此必須由應用程式加以驗證。 使用者可以刪除 Cookie，而且 Cookie 會在用戶端上過期。 不過，Cookie 通常是在用戶端上資料持續性最持久的形式。

Cookie 通常可用於個人化，其中內容會針對已知的使用者自訂。 在大部分情況下，只會識別使用者，而未加以驗證。 Cookie 可以儲存使用者的名稱、帳戶名稱或唯一使用者識別碼 (例如 GUID)。 您接著可以使用 Cookie 來存取使用者的個人化設定，例如其慣用網站的背景色彩。

在發出 Cookie 及處理隱私權顧慮時，請留意[歐盟一般資料保護規定 (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection)。 如需詳細資訊，請參閱 [ASP.NET Core 中的一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。

## <a name="session-state"></a>工作階段狀態

工作階段狀態是用來在使用者瀏覽 Web 應用程式時存放使用者資料的 ASP.NET Core 情節。 工作階段狀態使用應用程式所維護的存放區，在用戶端的要求之間保存資料。 工作階段資料受到快取的支援，並被視為暫時資料&mdash;網站沒有工作階段資料應該也會繼續運作。 重要應用程式資料應該儲存在使用者資料庫，並只在工作階段中快取以獲得效能最佳化。

> [!NOTE]
> [SignalR](xref:signalr/index) 應用程式中不支援工作階段，因為 [SignalR 中樞](xref:signalr/hubs)可獨立於 HTTP 內容之外而執行。 例如，當長時間輪詢要求由中樞維持開啟，超過要求的 HTTP 內容存留期時，便可能發生此情況。

ASP.NET Core 可維護工作階段狀態，方法是提供包含工作階段識別碼的 Cookie 給用戶端，以便將其隨著每個要求傳送至應用程式。 應用程式則使用工作階段識別碼來擷取工作階段資料。

工作階段狀態表現下列行為：

* 因為工作階段 Cookie 是瀏覽器所特有，所以不會跨瀏覽器共用工作階段。
* 在瀏覽器工作階段結束時，會刪除工作階段 Cookie。
* 如果收到過期工作階段的 Cookie，則會建立使用相同工作階段 Cookie 的新工作階段。
* 並不會保留空白工作階段&mdash;必須在工作階段中設定至少一個值，才能在要求之間保存工作階段。 不保留工作階段時，會為每個新的要求產生新的工作階段識別碼。
* 應用程式會在最後一個要求之後，保留工作階段一段有限的時間。 應用程式會設定工作階段逾時或使用預設值 20 分鐘。 工作階段狀態適合用來儲存特定工作階段特定的使用者資料，但資料不需要在工作階段之間永久儲存的情況。
* 工作階段資料會在呼叫 [ISession.Clear](/dotnet/api/microsoft.aspnetcore.http.isession.clear) 實作或工作階段過期時刪除。
* 沒有任何預設機制可通知應用程式程式碼，用戶端瀏覽器已關閉，或工作階段 Cookie 遭到刪除或在用戶端上已過期。
* ASP.NET Core MVC 和 Razor Pages 範本包含對一般資料保護規定 (GDPR) 的支援。 工作階段狀態 Cookie 未預設標示為必要項目，因此工作階段狀態無法運作，除網站訪客允許追蹤。 如需詳細資訊，請參閱 <xref:security/gdpr#tempdata-provider-and-session-state-cookies-arent-essential>。

> [!WARNING]
> 請勿將敏感性資料存放在工作階段狀態。 使用者可能不會關閉瀏覽器，並清除工作階段 Cookie。 某些瀏覽器會在瀏覽器視窗之間維護有效的工作階段 Cookie。 工作階段可能無法限制為單一使用者&mdash;下一位使用者可能會繼續使用相同的工作階段 Cookie 瀏覽應用程式。

記憶體中快取提供者會將工作階段資料存放在應用程式所在伺服器的記憶體中。 在伺服器陣列案例中：

* 使用「黏性工作階段」將每個工作階段繫結至個別伺服器上的特定應用程式執行個體。 [Azure App Service](https://azure.microsoft.com/services/app-service/) 預設會使用[應用程式要求路由 (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) 來強制執行自黏工作階段。 不過，黏性工作階段可能會影響延展性，並使 Web 應用程式更新複雜化。 較好的方法是使用 Redis 或 SQL Server 分散式快取，這不需要黏性工作階段。 如需詳細資訊，請參閱 <xref:performance/caching/distributed>。
* 工作階段 Cookie 是透過 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密。 必須正確設定資料保護，以閱讀每一部機器上的工作階段 Cookie。 如需詳細資訊，請參閱 <xref:security/data-protection/introduction>與[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。

### <a name="configure-session-state"></a>設定工作階段狀態

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件隨附於 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，提供用來管理工作階段狀態的中介軟體。 若要啟用工作階段中介軟體，`Startup` 必須包含：

* 任一 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 記憶體快取。 `IDistributedCache` 實作會作為工作階段的支援存放區。 如需詳細資訊，請參閱 <xref:performance/caching/distributed>。
* 在 [ 中呼叫 ](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession)AddSession`ConfigureServices`。
* 在 [ 中呼叫 ](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions.usesession#Microsoft_AspNetCore_Builder_SessionMiddlewareExtensions_UseSession_Microsoft_AspNetCore_Builder_IApplicationBuilder_)UseSession`Configure`。

下列程式碼示範如何設定記憶體內部工作階段提供者，以及 `IDistributedCache` 的預設記憶體中實作。

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=5-14,34)]

中介軟體的順序很重要。 在上述範例中，如果在 `InvalidOperationException` 之後叫用 `UseSession`，則會發生 `UseMvc`　例外狀況。 如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#order)。

設定工作階段狀態之後便可以使用 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。

呼叫 `HttpContext.Session` 之前無法存取 `UseSession`。

應用程式開始寫入回應資料流之後，無法建立具有新工作階段 Cookie 的新工作階段。 例外狀況會記錄在 Web 伺服器記錄檔中，而不會顯示在瀏覽器。

### <a name="load-session-state-asynchronously"></a>非同步載入工作階段狀態

只有在 [TryGetValue](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache)、[Set](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) 或 [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue) 方法之前明確呼叫 [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.set) 方法時，ASP.NET Core 中的預設工作階段提供者才會從基礎 [IDistributedCache](/dotnet/api/microsoft.aspnetcore.http.isession.remove) 備份存放區以非同步方式載入工作階段記錄。 如果並未先呼叫 `LoadAsync`，則基礎工作階段記錄會同步載入，這可能大規模地為效能帶來負面影響。

若要讓應用程式強制執行此模式，請使用未在 [、](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) 或 [ 之前呼叫 ](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 方法時擲回例外狀況的版本來包裝 `LoadAsync`DistributedSessionStore`TryGetValue` 和 `Set`DistributedSession`Remove` 實作。 請在服務容器中註冊已包裝的版本。

### <a name="session-options"></a>工作階段選項

若要覆寫工作階段的預設值，請使用 [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions)。

| 選項 | 描述 |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | 決定用來建立 Cookie 的設定。 [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) 預設為 [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`)。 [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) 預設為 [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`)。 [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) 預設為 [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`)。 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`。 [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) 預設為 `false`。 |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` 指出工作階段可以閒置多久，之後才會放棄它的內容。 每個工作階段存取都會重設逾時。 此設定只適用於工作階段的內容，而非 Cookie。 預設值是 20 分鐘。 |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | 從存放區載入工作階段，或將它認可回到存放區時，所允許的時間長度上限。 此設定只可能適用於非同步作業。 此逾時可以使用 [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan) 來停用。 預設值是 1 分鐘。 |

工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。 此 Cookie 預設名為 `.AspNetCore.Session`，並使用路徑 `/`。 由於 Cookie 預設值未指定網域，因此它不會提供給頁面上的用戶端指令碼 (因為 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`)。

若要覆寫 Cookie 工作階段的預設值，請使用 `SessionOptions`：

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=14-19)]

應用程式會使用 [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) 屬性，判斷工作階段可以在放棄伺服器快取中的內容之前閒置多長時間。 這個屬性與 Cookie 到期日無關。 透過[工作階段中介軟體](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware)傳遞的每個要求都會重設逾時。

工作階段狀態為「非鎖定」。 如果兩個要求同時嘗試修改工作階段的內容，則最後一個要求會覆寫第一個要求。 `Session` 會實作為「一致性工作階段」，這表示所有內容會都儲存在一起。 當兩個要求試圖修改不同的工作階段值時，最後一個要求可能會覆寫第一個要求所做的工作階段變更。

### <a name="set-and-get-session-values"></a>設定和取得工作階段值

工作階段狀態的存取，是從 Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 類別，或 MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) 類別搭配 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。 這個屬性是 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 實作。

`ISession` 實作提供數個延伸模組來設定和擷取整數和字串值。 當專案參考 [Microsoft.AspNetCore.Http.Extensions](/dotnet/api/microsoft.aspnetcore.http) 套件時，擴充方法位於 `using Microsoft.AspNetCore.Http;`Microsoft.AspNetCore.Http[ 命名空間 (新增 ](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) 陳述式即可取得擴充方法的存取權)。 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)包含兩個套件。

`ISession` 擴充方法：

* [Get(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.get)
* [GetInt32(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getint32)
* [GetString(ISession, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.getstring)
* [SetInt32(ISession, String, Int32)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setint32)
* [SetString(ISession, String, String)](/dotnet/api/microsoft.aspnetcore.http.sessionextensions.setstring)

下列範例會在 Razor Pages 頁面中，擷取 `IndexModel.SessionKeyName` 索引鍵的工作階段值 (範例應用程式中的 `_Name`)：

```csharp
@page
@using Microsoft.AspNetCore.Http
@model IndexModel

...

Name: @HttpContext.Session.GetString(IndexModel.SessionKeyName)
```

下列範例示範如何設定及取得整數和字串：

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

若要啟用分散式快取案例，即使是使用記憶體中快取時，都必須序列化所有工作階段資料。 字串和整數序列化程式是由[ISession](/dotnet/api/microsoft.aspnetcore.http.isession)的擴充方法所提供）。 複雜類型必須由使用者使用另一個機制加以序列化，例如 JSON。

新增下列擴充方法，即可設定和取得可序列化物件：

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

下列範例示範如何使用擴充方法來設定和取得可序列化物件：

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="tempdata"></a>TempData

ASP.NET Core 會公開 Razor Pages [TempData](xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.TempData)或控制器 <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>。 這個屬性會儲存資料，直到讀取另一個要求為止。 在要求結束時，可以使用[保留（字串）](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Keep*)和[查看（字串）](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Peek*)方法來檢查資料，而不需要刪除。 [Keep （）](xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ITempDataDictionary.Keep*)會標示字典中的所有專案以進行保留。 當有多個要求需要資料時，`TempData` 特別適合用來重新導向。 `TempData` 是由使用 cookie 或會話狀態的 `TempData` 提供者所執行。

## <a name="tempdata-samples"></a>TempData 範例

請考慮下列會建立客戶的頁面：

[!code-csharp[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet&highlight=15-16,30)]

下列頁面會顯示 `TempData["Message"]`：

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/IndexPeek.cshtml?range=1-14)]

在上述標記中，在要求結束時，**不**會刪除 `TempData["Message"]`，因為會使用 `Peek`。 重新整理頁面會顯示 `TempData["Message"]`。

下列標記與上述程式碼類似，但使用 `Keep` 在要求結束時保留資料：

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/IndexKeep.cshtml?range=1-14)]

在 [ *IndexPeek* ] 和 [ *IndexKeep* ] 頁面之間流覽並不會 `TempData["Message"]`刪除。

下列程式碼會顯示 `TempData["Message"]`，但在要求結束時，`TempData["Message"]` 會被刪除：

[!code-cshtml[](app-state/3.0samples/RazorPagesContacts/Pages/Customers/Index.cshtml?range=1-14)]

### <a name="tempdata-providers"></a>TempData 提供者

預設會使用以 Cookie 為基礎的 TempData 提供者，以將 TempData 儲存在 Cookie 中。

Cookie 資料是使用 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密，並使用 [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) 編碼，然後分成區塊。 因為 Cookie 分成區塊，所以不適用 ASP.NET Core 1.x 中找到的 Cookie 大小限制。 Cookie 資料不會壓縮，因為壓縮加密資料可能會導致安全性問題，例如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻擊。 如需 Cookie 架構 TempData 提供者的詳細資訊，請參閱 [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider)。

### <a name="choose-a-tempdata-provider"></a>選擇 TempData 提供者

選擇 TempData 提供者涉及數項考量，例如：

1. 應用程式已經使用工作階段狀態了嗎？ 如果是的話，使用工作階段狀態 TempData 提供者沒有額外的應用程式成本 (除了資料的大小之外)。
2. 應用程式是否盡量只將 TempData 用於相對少量的資料 (最多 500 個位元組)？ 如果是的話，Cookie TempData 提供者將對包含 TempData 的每個要求新增少量成本。 如果不是的話，則工作階段狀態 TempData 提供者可能有助於避免在每個要求中來回傳送大量資料，直到取用 TempData 為止。
3. 應用程式在伺服器陣列中的多部伺服器上執行？ 如果是的話，不需要額外組態，即可在資料保護之外使用 Cookie TempData 提供者 (請參閱 <xref:security/data-protection/introduction>與[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers))。

> [!NOTE]
> 大部分的 Web 用戶端 (例如網頁瀏覽器) 會強制執行每個 Cookie 的大小上限、Cookie 總數或這兩者的限制。 使用 Cookie TempData 提供者時，請確認應用程式不會超過這些限制。 請考慮資料的大小總計。 請考慮因為加密和區塊處理而增加的 Cookie 大小。

### <a name="configure-the-tempdata-provider"></a>設定 TempData 提供者

預設會啟用 Cookie 架構 TempData 提供者。

若要啟用以工作階段為基礎的 TempData 提供者，請使用 [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) 擴充方法：

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

中介軟體的順序很重要。 在上述範例中，如果在 `InvalidOperationException` 之後叫用 `UseSession`，則會發生 `UseMvc`　例外狀況。 如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#order)。

> [!IMPORTANT]
> 如果目標為 .NET Framework 且使用以工作階段為基礎的 TempData 提供者，請將 [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件新增至專案。

## <a name="query-strings"></a>查詢字串

可以將數量有限的資料從某個要求傳遞到另一個要求，方法是將其新增至新要求的查詢字串。 這對於以持續方式擷取狀態很有用，可讓內嵌狀態的連結透過電子郵件或社交網路共用。 因為 URL 查詢字串為公用，所以請絕對不要使用查詢字串來處理敏感性資料。

除了非預期的共用之外，在查詢字串中包括資料還可能會為[跨網站偽造要求 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻擊創造機會，這些攻擊可能會誘騙使用者在驗證時瀏覽惡意網站。 然後，攻擊者可能會竊取應用程式中的使用者資料，或代表使用者採取惡意動作。 任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。 如需詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊](xref:security/anti-request-forgery)。

## <a name="hidden-fields"></a>隱藏欄位

資料可以儲存在隱藏的表單欄位，並貼回下一個要求。 這在多頁表單中很常見。 因為用戶端可能會竄改資料，所以應用程式必須一律重新驗證存放在隱藏欄位中的資料。

## <a name="httpcontextitems"></a>HttpContext.Items

[HttpContext.Items](/dotnet/api/microsoft.aspnetcore.http.httpcontext.items) 集合用來在處理單一要求時存放資料。 集合的內容會在每個要求處理之後捨棄。 當元件或中介軟體在要求期間的不同時間點運作，而且沒有可傳遞參數的直接方式時，`Items` 集合經常用來允許元件或中介軟體進行通訊。

在下列範例中，[中介軟體](xref:fundamentals/middleware/index)會將 `isVerified` 新增至 `Items` 集合。

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

稍後在管線中，另一個中介軟體可以存取 `isVerified` 的值：

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync($"Verified: {context.Items["isVerified"]}");
});
```

如果是只由單一應用程式使用的中介軟體，可接受 `string` 索引鍵。 應用程式執行個體之間共用的中介軟體，應該使用唯一的物件索引鍵，來避免索引鍵衝突。 下列範例示範如何使用中介軟體類別中定義的唯一物件索引鍵：

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

其他程式碼可以使用中介軟體類別所公開的索引鍵，來存取 `HttpContext.Items` 中儲存的值：

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

此方法也有在程式碼中消除使用索引鍵字串的優點。

## <a name="cache"></a>快取

快取是儲存和擷取資料的有效方式。 應用程式可以控制快取項目的存留期。

快取的資料未與特定要求、使用者或工作階段建立關聯。 **請小心不要快取可能由其他使用者要求所擷取的特定使用者資料。**

如需詳細資訊，請參閱 <xref:performance/caching/response>。

## <a name="dependency-injection"></a>相依性插入

請使用[相依性插入](xref:fundamentals/dependency-injection)將資料提供給所有使用者：

1. 定義包含資料的服務。 例如，已定義名為 `MyAppData` 的類別：

    ```csharp
    public class MyAppData
    {
        // Declare properties and methods
    }
    ```

2. 將服務類別新增至 `Startup.ConfigureServices`：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<MyAppData>();
    }
    ```

3. 取用資料服務類別：

    ```csharp
    public class IndexModel : PageModel
    {
        public IndexModel(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

## <a name="common-errors"></a>常見錯誤

* 「嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore' 時，無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 。」

  這種情形通常是因為無法設定至少一個 `IDistributedCache` 實作。 如需詳細資訊，請參閱<xref:performance/caching/distributed>和<xref:performance/caching/memory>。

* 如果工作階段中介軟體無法持續存在於工作階段 (例如，備份存放區無法使用)，中介軟體會記錄例外狀況，而要求會照常繼續進行。 這會導致無法預期的行為。

  例如，使用者在工作階段中存放購物車。 使用者在購物車新增一個項目，但認可失敗。 應用程式未察覺到失敗，因此它向使用者報告項目已新增至購物車，但這並不正確。

  檢查是否有錯誤的建議方法是，當應用程式完成寫入至工作階段後，從應用程式程式碼呼叫 `await feature.Session.CommitAsync();`。 備份存放區無法使用時，`CommitAsync` 會擲回例外狀況。 如果 `CommitAsync` 失敗，應用程式可以處理例外狀況。 `LoadAsync` 在無法使用資料存放區的相同情況下會擲回。
  
## <a name="opno-locsignalr-and-session-state"></a>SignalR 和會話狀態

SignalR 應用程式不應使用會話狀態來儲存資訊。 SignalR 的應用程式可以在中樞的 `Context.Items` 中儲存每個線上狀態。 <!-- https://github.com/aspnet/SignalR/issues/2139 -->

## <a name="additional-resources"></a>其他資源

<xref:host-and-deploy/web-farm>
::: moniker-end
