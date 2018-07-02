---
title: ASP.NET Core 中的工作階段與應用程式狀態
author: rick-anderson
description: 探索方法以在要求之間保留工作階段和應用程式狀態。
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2018
uid: fundamentals/app-state
ms.openlocfilehash: 9c63d9313acb055e6c692a7fef3d28e94cb37093
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272879"
---
# <a name="session-and-app-state-in-aspnet-core"></a>ASP.NET Core 中的工作階段與應用程式狀態

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)、[Diana LaRose](https://github.com/DianaLaRose) 和 [Luke Latham](https://github.com/guardrex)

HTTP 是無狀態的通訊協定。 若不採取其他步驟，HTTP 要求是獨立的訊息，不會保留使用者的值或應用程式狀態。 本文描述數種方法來在要求之間保留使用者資料和應用程式狀態。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

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
| [快取](#cache) | 伺服器端應用程式程式碼 |
| [相依性插入](#dependency-injection) | 伺服器端應用程式程式碼 |

## <a name="cookies"></a>Cookie

Cookie 會在要求之間儲存資料。 因為 Cookie 會隨著每個要求傳送，所以其大小應該保持最小。 在理想情況下，應該只有識別碼儲存在 Cookie 中，而資料由應用程式儲存。 大部分的瀏覽器將 Cookie 大小限制為 4096 個位元組。 每個網域只有數量有限的 Cookie 可供使用。

由於 Cookie 可能會遭到竄改，因此必須由應用程式加以驗證。 使用者可以刪除 Cookie，而且 Cookie 會在用戶端上過期。 不過，Cookie 通常是在用戶端上資料持續性最持久的形式。

Cookie 通常可用於個人化，其中內容會針對已知的使用者自訂。 在大部分情況下，只會識別使用者，而未加以驗證。 Cookie 可以儲存使用者的名稱、帳戶名稱或唯一使用者識別碼 (例如 GUID)。 您接著可以使用 Cookie 來存取使用者的個人化設定，例如其慣用網站的背景色彩。

在發出 Cookie 及處理隱私權顧慮時，請留意[歐盟一般資料保護規定 (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection)。 如需詳細資訊，請參閱 [ASP.NET Core 中的一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。

## <a name="session-state"></a>工作階段狀態

工作階段狀態是用來在使用者瀏覽 Web 應用程式時存放使用者資料的 ASP.NET Core 情節。 工作階段狀態使用應用程式所維護的存放區，在用戶端的要求之間保存資料。 工作階段資料受到快取的支援，並被視為暫時資料&mdash;網站沒有工作階段資料應該也會繼續運作。

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

> [!WARNING]
> 請勿將敏感性資料存放在工作階段狀態。 使用者可能不會關閉瀏覽器，並清除工作階段 Cookie。 某些瀏覽器會在瀏覽器視窗之間維護有效的工作階段 Cookie。 工作階段可能無法限制為單一使用者&mdash;下一位使用者可能會繼續使用相同的工作階段 Cookie 瀏覽應用程式。

記憶體中快取提供者會將工作階段資料存放在應用程式所在伺服器的記憶體中。 在伺服器陣列案例中：

* 使用「黏性工作階段」將每個工作階段繫結至個別伺服器上的特定應用程式執行個體。 [Azure App Service](https://azure.microsoft.com/services/app-service/) 預設會使用[應用程式要求路由 (ARR)](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) 來強制執行自黏工作階段。 不過，黏性工作階段可能會影響延展性，並使 Web 應用程式更新複雜化。 較好的方法是使用 Redis 或 SQL Server 分散式快取，這不需要黏性工作階段。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。
* 工作階段 Cookie 是透過 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密。 必須正確設定資料保護，以閱讀每一部機器上的工作階段 Cookie。 如需詳細資訊，請參閱 [ASP.NET Core 的資料保護](xref:security/data-protection/index)和[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers)。

### <a name="configure-session-state"></a>設定工作階段狀態

::: moniker range=">= aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件隨附於 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，提供用來管理工作階段狀態的中介軟體。 若要啟用工作階段中介軟體，`Startup` 必須包含：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session/) 套件提供用來管理工作階段狀態的中介軟體。 若要啟用工作階段中介軟體，`Startup` 必須包含：

::: moniker-end

* 任一 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 記憶體快取。 `IDistributedCache` 實作會作為工作階段的支援存放區。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。
* 在 `ConfigureServices` 中呼叫 [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions.addsession)。
* 在 `Configure` 中呼叫 [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)。

下列程式碼示範如何設定記憶體內部工作階段提供者，以及 `IDistributedCache` 的預設記憶體中實作。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13-18,39)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5,7-12,19)]

::: moniker-end

中介軟體的順序很重要。 在上述範例中，如果在 `UseMvc` 之後叫用 `UseSession`，則會發生 `InvalidOperationException`　例外狀況。 如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#ordering)。

設定工作階段狀態之後便可以使用 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。

呼叫 `UseSession` 之前無法存取 `HttpContext.Session`。

應用程式開始寫入回應資料流之後，無法建立具有新工作階段 Cookie 的新工作階段。 例外狀況會記錄在 Web 伺服器記錄檔中，而不會顯示在瀏覽器。

### <a name="load-session-state-asynchronously"></a>非同步載入工作階段狀態

只有在 [TryGetValue](/dotnet/api/microsoft.aspnetcore.http.isession.trygetvalue)、[Set](/dotnet/api/microsoft.aspnetcore.http.isession.set) 或 [Remove](/dotnet/api/microsoft.aspnetcore.http.isession.remove) 方法之前明確呼叫 [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession.loadasync) 方法時，ASP.NET Core 中的預設工作階段提供者才會從基礎 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) 備份存放區以非同步方式載入工作階段記錄。 如果並未先呼叫 `LoadAsync`，則基礎工作階段記錄會同步載入，這可能大規模地為效能帶來負面影響。

若要讓應用程式強制執行此模式，請使用未在 `TryGetValue`、`Set` 或 `Remove` 之前呼叫 `LoadAsync` 方法時擲回例外狀況的版本來包裝 [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) 和 [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 實作。 請在服務容器中註冊已包裝的版本。

### <a name="session-options"></a>工作階段選項

若要覆寫工作階段的預設值，請使用 [SessionOptions](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions)。

::: moniker range=">= aspnetcore-2.0"

| 選項 | 描述 |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookie) | 決定用來建立 Cookie 的設定。 [Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name) 預設為 [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`)。 [Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path) 預設為 [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`)。 [SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite) 預設為 [SameSiteMode.Lax](/dotnet/api/microsoft.aspnetcore.http.samesitemode) (`1`)。 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`。 [IsEssential](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.isessential) 預設為 `false`。 |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` 指出工作階段可以閒置多久，之後才會放棄它的內容。 每個工作階段存取都會重設逾時。 請注意，這只適用於工作階段的內容，而不適用於 Cookie。 預設值是 20 分鐘。 |
| [IOTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.iotimeout) | 從存放區載入工作階段，或將它認可回到存放區時，所允許的最大時間量。 請注意，這可能只適用於非同步作業。 此逾時可以使用 [InfiniteTimeSpan](/dotnet/api/system.threading.timeout.infinitetimespan) 來停用。 預設為 1 分鐘。 |

工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。 此 Cookie 預設名為 `.AspNetCore.Session`，並使用路徑 `/`。 由於 Cookie 預設值未指定網域，因此它不會提供給頁面上的用戶端指令碼 (因為 [HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly) 預設為 `true`)。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| 選項 | 描述 |
| ------ | ----------- |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiedomain) | 決定用來建立 Cookie 的網域。 預設不會設定 `CookieDomain`。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiehttponly) | 決定瀏覽器是否應該允許 Cookie 由用戶端 JavaScript 存取。 預設值是 `true`，這表示 Cookie 僅會傳遞給 HTTP 要求，並未開放給頁面上的指令碼使用。 |
| [CookieName](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiename) | 決定用來保存工作階段識別碼的 Cookie 名稱。 預設值是 [SessionDefaults.CookieName](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiename) (`.AspNetCore.Session`)。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiepath) | 決定用來建立 Cookie 的路徑。 預設為 [SessionDefaults.CookiePath](/dotnet/api/microsoft.aspnetcore.session.sessiondefaults.cookiepath) (`/`)。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.cookiesecure) | 決定是否應該只在 HTTPS 要求傳送 Cookie。 預設值是 [CookieSecurePolicy.None](/dotnet/api/microsoft.aspnetcore.http.cookiesecurepolicy) (`2`)。 |
| [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) | `IdleTimeout` 指出工作階段可以閒置多久，之後才會放棄它的內容。 每個工作階段存取都會重設逾時。 請注意，這只適用於工作階段的內容，而不適用於 Cookie。 預設值是 20 分鐘。 |

工作階段使用 Cookie 來追蹤和識別來自單一瀏覽器的要求。 此 Cookie 預設名為 `.AspNet.Session`，並使用路徑 `/`。

::: moniker-end

若要覆寫 Cookie 工作階段的預設值，請使用 `SessionOptions`：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/2.x/SessionSample/Startup.cs?name=snippet1&highlight=13-18)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples_snapshot/1.x/SessionSample/Startup.cs?name=snippet1&highlight=5-9)]

::: moniker-end

應用程式會使用 [IdleTimeout](/dotnet/api/microsoft.aspnetcore.builder.sessionoptions.idletimeout) 屬性，判斷工作階段可以在放棄伺服器快取中的內容之前閒置多長時間。 這個屬性與 Cookie 到期日無關。 透過[工作階段中介軟體](/dotnet/api/microsoft.aspnetcore.session.sessionmiddleware)傳遞的每個要求都會重設逾時。

工作階段狀態為「非鎖定」。 如果兩個要求同時嘗試修改工作階段的內容，則最後一個要求會覆寫第一個要求。 `Session` 會實作為「一致性工作階段」，這表示所有內容會都儲存在一起。 當兩個要求試圖修改不同的工作階段值時，最後一個要求可能會覆寫第一個要求所做的工作階段變更。

### <a name="set-and-get-session-values"></a>設定和取得工作階段值

工作階段狀態的存取，是從 Razor Pages [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 類別，或 MVC [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller) 類別搭配 [HttpContext.Session](/dotnet/api/microsoft.aspnetcore.http.httpcontext.session)。 這個屬性是 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 實作。

::: moniker range=">= aspnetcore-2.0"

`ISession` 實作提供數個擴充方法來設定和擷取整數和字串值。 當專案參考 [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) 套件時，擴充方法位於 [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) 命名空間 (新增 `using Microsoft.AspNetCore.Http;` 陳述式即可取得擴充方法的存取權)。 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)包含兩個套件。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`ISession` 實作提供數個擴充方法來設定和擷取整數和字串值。 當專案參考 [Microsoft.AspNetCore.Http.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.Http.Extensions/) 套件時，擴充方法位於 [Microsoft.AspNetCore.Http](/dotnet/api/microsoft.aspnetcore.http) 命名空間 (新增 `using Microsoft.AspNetCore.Http;` 陳述式即可取得擴充方法的存取權)。

::: moniker-end

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=18-19,22-23)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet1&highlight=10-11,18-19)]

::: moniker-end

若要啟用分散式快取案例，即使是使用記憶體中快取時，都必須序列化所有工作階段資料。 已提供最小字串及數字的序列化程式 (請參閱 [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 的方法和擴充方法)。 複雜類型必須由使用者使用另一個機制加以序列化，例如 JSON。

新增下列擴充方法，即可設定和取得可序列化物件：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Extensions/SessionExtensions.cs?name=snippet1)]

::: moniker-end

下列範例示範如何使用擴充方法來設定和取得可序列化物件：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet2&highlight=4,12)]

::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core 會公開 [Razor Pages 頁面模型的 TempData 屬性](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.tempdata)或 [MVC 控制器的 TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata)。 這個屬性會儲存資料，直到讀取為止。 [Keep](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.keep) 和 [Peek](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.itempdatadictionary.peek) 方法可以用來檢查資料，不用刪除。 當有多個要求需要資料時，TempData 對重新導向很有幫助。 TempData 是由 TempData 提供者使用 Cookie 或工作階段狀態來實作。

### <a name="tempdata-providers"></a>TempData 提供者

::: moniker range=">= aspnetcore-2.0"

在 ASP.NET Core 2.0 或更新版本中，預設會使用 Cookie 架構 TempData 提供者，將 TempData 儲存在 Cookie 中。

Cookie 資料是使用 [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) 加密，並使用 [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder) 編碼，然後分成區塊。 因為 Cookie 分成區塊，所以不適用 ASP.NET Core 1.x 中找到的 Cookie 大小限制。 Cookie 資料不會壓縮，因為壓縮加密資料可能會導致安全性問題，例如 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 和 [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻擊。 如需 Cookie 架構 TempData 提供者的詳細資訊，請參閱 [CookieTempDataProvider](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.cookietempdataprovider)。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

在 ASP.NET Core 1.0 和 1.1 中，工作階段狀態 TempData 提供者是預設提供者。

::: moniker-end

### <a name="choose-a-tempdata-provider"></a>選擇 TempData 提供者

選擇 TempData 提供者涉及數項考量，例如：

1. 應用程式已經使用工作階段狀態了嗎？ 如果是的話，使用工作階段狀態 TempData 提供者沒有額外的應用程式成本 (除了資料的大小之外)。
2. 應用程式是否盡量只將 TempData 用於相對少量的資料 (最多 500 個位元組)？ 如果是的話，Cookie TempData 提供者將對包含 TempData 的每個要求新增少量成本。 如果不是的話，則工作階段狀態 TempData 提供者可能有助於避免在每個要求中來回傳送大量資料，直到取用 TempData 為止。
3. 應用程式在伺服器陣列中的多部伺服器上執行？ 如果是的話，不需要額外組態，即可在資料保護之外使用 Cookie TempData 提供者 (請參閱[資料保護](xref:security/data-protection/index)和[金鑰儲存提供者](xref:security/data-protection/implementation/key-storage-providers))。

> [!NOTE]
> 大部分的 Web 用戶端 (例如網頁瀏覽器) 會強制執行每個 Cookie 的大小上限、Cookie 總數或這兩者的限制。 使用 Cookie TempData 提供者時，請確認應用程式不會超過這些限制。 請考慮資料的大小總計。 請考慮因為加密和區塊處理而增加的 Cookie 大小。

### <a name="configure-the-tempdata-provider"></a>設定 TempData 提供者

::: moniker range=">= aspnetcore-2.0"

預設會啟用 Cookie 架構 TempData 提供者。

若要啟用以工作階段為基礎的 TempData 提供者，請使用 [AddSessionStateTempDataProvider](/dotnet/api/microsoft.extensions.dependencyinjection.mvcviewfeaturesmvcbuilderextensions.addsessionstatetempdataprovider) 擴充方法：

[!code-csharp[](app-state/samples_snapshot_2/2.x/SessionSample/Startup.cs?name=snippet1&highlight=11,13,32)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

下列 `Startup` 類別程式碼會設定工作階段架構 TempData 提供者：

[!code-csharp[](app-state/samples_snapshot_2/1.x/SessionSample/Startup.cs?name=snippet1&highlight=4,9)]

::: moniker-end

中介軟體的順序很重要。 在上述範例中，如果在 `UseMvc` 之後叫用 `UseSession`，則會發生 `InvalidOperationException`　例外狀況。 如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#ordering)。

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=4,13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Middleware/HttpContextItemsMiddleware.cs?name=snippet1&highlight=5,14)]

::: moniker-end

其他程式碼可以使用中介軟體類別所公開的索引鍵，來存取 `HttpContext.Items` 中儲存的值：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-state/samples/2.x/SessionSample/Pages/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](app-state/samples/1.x/SessionSample/Controllers/HomeController.cs?name=snippet3)]

::: moniker-end

此方法也有在程式碼中消除使用索引鍵字串的優點。

## <a name="cache"></a>快取

快取是儲存和擷取資料的有效方式。 應用程式可以控制快取項目的存留期。

快取的資料未與特定要求、使用者或工作階段建立關聯。 **請小心不要快取可能由其他使用者要求所擷取的特定使用者資料。**

如需詳細資訊，請參閱[快取回應](xref:performance/caching/index)主題。

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

    ::: moniker range=">= aspnetcore-2.0"

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

    ::: moniker-end

    ::: moniker range="< aspnetcore-2.0"

    ```csharp
    public class HomeController : Controller
    {
        public HomeController(MyAppData myService)
        {
            // Do something with the service
            //    Examples: Read data, store in a field or property
        }
    }
    ```

    ::: moniker-end

## <a name="common-errors"></a>常見的錯誤

* 「嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore' 時，無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 。」

  這種情形通常是因為無法設定至少一個 `IDistributedCache` 實作。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[在記憶體中快取](xref:performance/caching/memory)。

* 如果工作階段中介軟體無法持續存在於工作階段 (例如，備份存放區無法使用)，中介軟體會記錄例外狀況，而要求會照常繼續進行。 這會導致無法預期的行為。

  例如，使用者在工作階段中存放購物車。 使用者在購物車新增一個項目，但認可失敗。 應用程式未察覺到失敗，因此它向使用者報告項目已新增至購物車，但這並不正確。

  檢查是否有錯誤的建議方法是，當應用程式完成寫入至工作階段後，從應用程式程式碼呼叫 `await feature.Session.CommitAsync();`。 備份存放區無法使用時，`CommitAsync` 會擲回例外狀況。 如果 `CommitAsync` 失敗，應用程式可以處理例外狀況。 `LoadAsync` 在無法使用資料存放區的相同情況下會擲回。
