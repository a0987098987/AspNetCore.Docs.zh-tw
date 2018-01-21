---
title: "在 ASP.NET Core 的工作階段和應用程式狀態"
author: rick-anderson
description: "在要求之間的方法來保留應用程式和使用者 （工作階段） 狀態。"
ms.author: riande
manager: wpickett
ms.date: 11/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 13b4d759ae574cdf9899ca148f0ffd3d9df6f9ae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>工作階段和應用程式的狀態，在 ASP.NET Core 簡介

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Diana LaRose](https://github.com/DianaLaRose)

HTTP 是無狀態的通訊協定。 Web 伺服器視為獨立的要求中的每個 HTTP 要求，並不會保留來自前一個要求的使用者值。 本文將討論不同的方式來保留應用程式和要求之間的工作階段狀態。 

## <a name="session-state"></a>工作階段狀態

在 ASP.NET Core 中的工作階段狀態功能，可用於在使用者瀏覽您的 Web 應用程式時，儲存及存放使用者資料。 伺服器上的字典或雜湊資料表所組成，工作階段狀態資料保存跨要求從瀏覽器。 快取所支援的工作階段資料。

ASP.NET Core 會讓用戶端的 cookie，包含工作階段識別碼，以便傳送至每個要求的伺服器維護工作階段狀態。 伺服器會使用工作階段識別碼來擷取工作階段資料。 因為工作階段 cookie 是瀏覽器專用的所以您無法跨瀏覽器共用工作階段。 只有在瀏覽器工作階段結束時，會刪除工作階段 cookie。 如果 cookie 已過期的工作階段收到時，會建立新的工作階段使用相同的工作階段 cookie。 

伺服器會保留有限的時間，最後一個要求之後的工作階段。 您可以設定工作階段逾時或使用預設值為 20 分鐘。 工作階段狀態是適合用來儲存使用者資料屬於特定的工作階段，但不需要永久保存。 當您呼叫，會有資料從備份存放區是刪除`Session.Clear`或資料存放區中的工作階段到期時。 關閉瀏覽器時，或刪除工作階段 cookie 時，伺服器不知道。

> [!WARNING]
> 不要儲存工作階段中的機密資料。 用戶端不可能在關閉瀏覽器，並清除工作階段 cookie （和某些瀏覽器工作階段 cookie 存留在整個保留 windows）。 此外，工作階段可能無法限制為單一使用者。下一位使用者可能會繼續使用相同的工作階段。

記憶體中工作階段提供者會在本機伺服器儲存工作階段資料。 如果您打算在伺服器陣列上執行您 web 應用程式，您必須將繫結到特定伺服器的每個工作階段使用自黏工作階段。 Windows Azure Web Sites 平台預設值是自黏工作階段 （應用程式要求路由或 ARR）。 不過，自黏工作階段可能會影響延展性，而且使得 web 應用程式更新。 更好的選項是使用 Redis 或 SQL Server 分散式快取，而這不需要黏性工作階段。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。 如需設定服務提供者的詳細資訊，請參閱[設定工作階段](#configuring-session)本文稍後。

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC 公開[TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData)屬性[控制器](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)。 這個屬性會儲存資料，直到讀取為止。 `Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。 `TempData`就特別有用的重新導向，當超過單一要求所需的資料。 `TempData`是 TempData 提供者實作，例如，使用 cookie、 工作階段狀態。

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>TempData 提供者

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 版及更新版本中，以 cookie 為基礎 TempData 使用提供者是預設 TempData 儲存在 cookie 中。

Cookie 資料以編碼[Base64UrlTextEncoder](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0)。 Cookie 會進行加密及區塊處理，因為單一 cookie 的大小限制 1.x 不適用的 ASP.NET Core 中找到。 Cookie 資料不壓縮，因為壓縮加密的資料可能會導致安全性問題例如[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))和[破壞](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。 如需有關 cookie 架構 TempData 提供者的詳細資訊，請參閱[CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

在 ASP.NET Core 1.0 和 1.1 中，工作階段狀態 TempData 提供者是預設值。

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>選擇 TempData 提供者

選擇 TempData 提供者包括數個考量，例如：

1. 應用程式已無法用於其他用途使用工作階段狀態？ 如果是的話，使用工作階段狀態 TempData 提供者有任何額外的成本 （除了資料的大小） 應用程式。
2. 沒有應用程式 TempData 只盡量少用，適用於相對較小量的資料 （最多 500 個位元組）？ 因此，cookie TempData 提供者要新增至每個要求傳送 TempData 的小型的成本。 如果沒有，則工作階段狀態 TempData 提供者可能會以避免往返大量的每個要求中的資料，直到取用 TempData 很有用。
3. 應用程式執行 web 伺服陣列 （多部伺服器） 嗎？ 如果是，沒有需要使用 cookie TempData 提供者的其他設定。

> [!NOTE]
> 大部分的 web 用戶端 （例如網頁瀏覽器） 強制執行每個 cookie、 總數的 cookie，或兩者的最大大小的限制。 因此，當使用 cookie TempData 提供者，請確認應用程式將不會超過這些限制。 請考慮帳戶加密的負荷處理和區塊處理的資料大小總計。

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>TempData 提供者設定

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

預設會啟用 cookie 架構 TempData 提供者。 下列`Startup`類別程式碼會設定工作階段為基礎的 TempData 提供者：

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

下列`Startup`類別程式碼會設定工作階段為基礎的 TempData 提供者：

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

順序很重要的中介軟體元件。 在上述範例中，類型的例外狀況`InvalidOperationException`發生時`UseSession`之後叫用`UseMvcWithDefaultRoute`。 請參閱[中介軟體訂購](xref:fundamentals/middleware#ordering)如需詳細資訊。

> [!IMPORTANT]
> 如果目標為.NET Framework 和使用工作階段為基礎的提供者，將[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet 封裝加入您的專案。

## <a name="query-strings"></a>查詢字串

您可以傳遞有限的數量的資料從一個要求到另一個方法是加入新的要求查詢字串。 這可用於擷取狀態，以持續方式，可讓連結與內嵌的狀態，要透過電子郵件或社交網路共用。 不過，基於這個理由，您應該永遠不會使用查詢字串之機密資料。 除了輕鬆地共用，在查詢字串中包括的資料可以建立的機會[跨網站要求偽造 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))可以誘騙瀏覽惡意網站時驗證使用者的攻擊。 攻擊者可能會再竊取您的應用程式中的使用者資料，或者要代表使用者惡意動作。 任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。 如需有關 CSRF 攻擊的詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core](../security/anti-request-forgery.md)。

## <a name="post-data-and-hidden-fields"></a>張貼資料和隱藏的欄位

資料可以儲存在隱藏的表單欄位和公佈回於下一個要求。 這是一般多頁表單中。 不過，用戶端可能可以修改資料，因為伺服器必須一律重新驗證它。 

## <a name="cookies"></a>Cookie

Cookie 會提供在 web 應用程式儲存使用者專屬資料的方法。 每個要求傳送 cookie，因為其大小應該降到最低。 在理想情況下，只有識別碼應該與實際伺服器上儲存的資料儲存在 cookie 中。 大部分的瀏覽器限制為 4096 個位元組的 cookie。 此外，只有少數的 cookie 可供每個網域。  

Cookie 是有可能遭到竄改，因為它們必須在伺服器上驗證。 雖然用戶端上的 cookie 持久性受限於使用者介入的情況下與到期日，通常都在用戶端上的資料持續性最持久表單。

Cookie 通常可用來個人化，其中內容自訂為已知的使用者。 因為使用者只能識別，並且尚未驗證在大部分情況下，您通常可以藉由在 cookie 中儲存的使用者名稱、 帳戶名稱或唯一的使用者識別碼 （例如 GUID) 來保護 cookie。 Cookie 然後可用來存取站台的使用者個人化基礎結構。

## <a name="httpcontextitems"></a>HttpContext.Items

`Items`集合是個不錯的位置來儲存資料所需時，才處理一個特定的要求。 每個要求之後，會捨棄集合的內容。 `Items`集合最適合用做為元件或中介軟體的方式來通訊時可操作不同時間點期間要求的時間，並且具有沒有直接的方式，將參數傳遞。 如需詳細資訊，請參閱[使用 HttpContext.Items](#working-with-httpcontextitems)在本文稍後。

## <a name="cache"></a>快取

快取是有效的方式儲存和擷取資料。 您可以控制時間和其他考量為基礎的快取項目存留的期。 深入了解[快取](../performance/caching/index.md)。

<a name="session"></a>
## <a name="working-with-session-state"></a>使用工作階段狀態

### <a name="configuring-session"></a>設定工作階段

`Microsoft.AspNetCore.Session`套件提供中介軟體管理的工作階段狀態。 若要啟用工作階段中介軟體`Startup`必須包含：

- 任一[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)記憶體快取。 `IDistributedCache`做為備份存放區使用實作工作階段。
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_)呼叫，這需要 NuGet 套件 」 Microsoft.AspNetCore.Session"。
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)呼叫。

下列程式碼會示範如何設定記憶體中工作階段提供者。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

您可以參考從工作階段`HttpContext`之後就會安裝並設定。

如果您嘗試存取`Session`之前`UseSession`已經呼叫，例外狀況`InvalidOperationException: Session has not been configured for this application or request`就會擲回。

如果您嘗試建立新`Session`（也就是任何工作階段 cookie 具有已建立） 已經開始寫入之後`Response`串流處理的例外狀況`InvalidOperationException: The session cannot be established after the response has started`就會擲回。 例外狀況可以在 web 伺服器記錄檔。它不會顯示在瀏覽器中。

### <a name="loading-session-asynchronously"></a>非同步載入工作階段 

在 ASP.NET Core 的預設工作階段提供者會從基礎載入工作階段記錄[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)存放區以非同步方式只有當[ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync)之前明確地呼叫方法 `TryGetValue`， `Set`，或`Remove`方法。 如果`LoadAsync`則不會呼叫第一次，基礎工作階段記錄同步載入，這可能會影響應用程式能夠調整。

若要強制執行此模式的應用程式，請包裝[DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore)和[DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession)實作時擲回例外狀況的版本與`LoadAsync`方法不是之前呼叫`TryGetValue`， `Set`，或`Remove`。 註冊服務容器中的已包裝的版本。

### <a name="implementation-details"></a>實作詳細資料

工作階段會追蹤並找出要求從單一瀏覽器使用 cookie。 根據預設，此 cookie 名為"。AspNet.Session 」，並使用的路徑"/"。 Cookie 預設未指定網域，因為它不開放給用戶端指令碼頁面上 (因為`CookieHttpOnly`預設為`true`)。

若要覆寫工作階段的預設值，請使用`SessionOptions`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

伺服器會使用`IdleTimeout`屬性來判斷如何在工作階段前可保留長度閒置其內容會被放棄。 這個屬性是獨立的 cookie 到期日。 通過的工作階段中介軟體 （讀取或寫入） 的每個要求會重設時的逾時。

因為`Session`是*非鎖定*，如果兩個要求同時嘗試修改的工作階段中，最後一個內容，會覆寫第一個。 `Session`會實作為*一致的工作階段*，這表示所有內容會都儲存在一起。 要修改的工作階段 （不同的索引鍵） 的不同部分的兩個要求仍可能會影響彼此。

### <a name="setting-and-getting-session-values"></a>設定和取得工作階段值

工作階段透過存取`Session`屬性`HttpContext`。 這個屬性是[ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession)實作。

下列範例示範設定和取得整數和字串：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

如果您加入下列的擴充方法，您可以設定並取得工作階段可序列化的物件：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

下列範例會示範如何設定和取得可序列化的物件：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>使用 HttpContext.Items

`HttpContext`字典集合類型的抽象提供支援`IDictionary<object, object>`，稱為`Items`。 此集合可從開頭*HttpRequest*並捨棄每個要求的結尾。 將值指派至一個索引鍵的項目，或要求特定的索引鍵的值，您可以存取它。

在下列範例，[中介軟體](middleware.md)新增`isVerified`至`Items`集合。

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

稍後在管線中，另一個中介軟體，也無法存取它：

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

只會由一個單一的應用程式的中介軟體`string`索引鍵是可接受。 不過，將應用程式之間共用的中介軟體應該使用唯一物件索引鍵，以避免任何可能發生的索引鍵衝突。 如果您正在開發必須跨多個應用程式的中介軟體，請使用定義在您的中介軟體類別如下所示的唯一物件索引鍵：

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

其他程式碼可以存取儲存的值`HttpContext.Items`使用由中介軟體類別公開的索引鍵：

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

此方法也有消除重複的程式碼中的多個位置中的"magic 字串 」 的優點。

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>應用程式狀態資料

使用[相依性插入](xref:fundamentals/dependency-injection)可讓所有使用者使用資料：

1. 定義包含資料服務 (例如，名為類別`MyAppData`)。

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. 將服務類別，加入`ConfigureServices`(例如`services.AddSingleton<MyAppData>();`)。
3. 使用每個控制站與資料服務類別：

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

* 「 無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 服務嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore'。 」

  這種情形通常因未能設定至少一個`IDistributedCache`實作。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[中記憶體內部快取](xref:performance/caching/memory)。

* 事件的工作階段中介軟體無法保存工作階段 (例如： 如果資料庫無法使用)，它記錄例外狀況，抑制它。 要求將再繼續正常運作，這會導致非常無法預期的行為。

典型的範例：

有人會以購物籃儲存工作階段中。 使用者將項目，但認可會失敗。 應用程式並不知道有關失敗，因此它會報告訊息 」 項目已加入"，這並不正確。

檢查是否有這類錯誤的建議的方式是呼叫`await feature.Session.CommitAsync();`從應用程式程式碼完成後寫入工作階段。 然後您可以執行您要與錯誤。 它的運作方式相同呼叫時`LoadAsync`。


### <a name="additional-resources"></a>其他資源


* [ASP.NET Core 1.x： 本文件中使用的程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x： 本文件中使用的程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
