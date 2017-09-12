---
title: "在 ASP.NET Core 的工作階段和應用程式狀態"
author: rick-anderson
description: "在要求之間的方法來保留應用程式和使用者 （工作階段） 狀態。"
keywords: "ASP.NET Core 應用程式狀態、 工作階段狀態、 查詢字串，張貼"
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b451bde1e3180d12781d55113638cc1a99182c8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>工作階段和應用程式的狀態，在 ASP.NET Core 簡介

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Diana LaRose](https://github.com/DianaLaRose)

HTTP 是無狀態的通訊協定。 Web 伺服器視為獨立的要求中的每個 HTTP 要求，並不會保留來自前一個要求的使用者值。 本文將討論不同的方式來保留應用程式和要求之間的工作階段狀態。 

## <a name="session-state"></a>工作階段狀態

工作階段狀態會是可用來儲存和還原使用者資料，而使用者瀏覽您的 web 應用程式的 ASP.NET 核心功能。 伺服器上的字典或雜湊資料表所組成，工作階段狀態資料保存跨要求從瀏覽器。 快取所支援的工作階段資料。

ASP.NET Core 會讓用戶端的 cookie，包含工作階段識別碼，以便傳送至每個要求的伺服器維護工作階段狀態。 伺服器會使用工作階段識別碼來擷取工作階段資料。 因為工作階段 cookie 是瀏覽器專用的所以您無法跨瀏覽器共用工作階段。 只有在瀏覽器工作階段結束時，會刪除工作階段 cookie。 如果 cookie 已過期的工作階段收到時，會建立新的工作階段使用相同的工作階段 cookie。 

伺服器會保留有限的時間，最後一個要求之後的工作階段。 您可以設定工作階段逾時或使用預設值為 20 分鐘。 工作階段狀態是適合用來儲存使用者資料屬於特定的工作階段，但不需要永久保存。 當您呼叫，會有資料從備份存放區是刪除`Session.Clear`或資料存放區中的工作階段到期時。 關閉瀏覽器時，或刪除工作階段 cookie 時，伺服器不知道。

> [!WARNING]
> 不要儲存工作階段中的機密資料。 用戶端不可能在關閉瀏覽器，並清除工作階段 cookie （和某些瀏覽器工作階段 cookie 存留在整個保留 windows）。 此外，工作階段可能無法限制為單一使用者。下一位使用者可能會繼續使用相同的工作階段。

記憶體中工作階段提供者會在本機伺服器儲存工作階段資料。 如果您打算在伺服器陣列上執行您 web 應用程式，您必須將繫結到特定伺服器的每個工作階段使用自黏工作階段。 Windows Azure Web Sites 平台預設值是自黏工作階段 （應用程式要求路由或 ARR）。 不過，自黏工作階段可能會影響延展性，而且使得 web 應用程式更新。 更好的選項是使用 Redis 或 SQL Server 分散式快取，而這不需要黏性工作階段。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。 如需設定服務提供者的詳細資訊，請參閱[設定工作階段](#configuring-session)本文稍後。

本章節的其餘部分描述來儲存使用者資料的選項。

<a name="temp"></a>
### <a name="tempdata"></a>TempData

ASP.NET Core MVC 公開[TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData)屬性[控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)。 這個屬性會儲存資料，直到讀取為止。 `Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。 `TempData`就特別有用的重新導向，當超過單一要求所需的資料。 `TempData`建置工作階段狀態的頂端。 

## <a name="cookie-based-tempdata-provider"></a>Cookie 架構 TempData 提供者 

在 ASP.NET Core 1.1 和更新版本，您可以使用 cookie 架構 TempData 提供者來儲存使用者的 TempData 在 cookie 中。 若要啟用的 cookie 架構 TempData 提供者，註冊`CookieTempDataProvider`服務`ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

Cookie 資料以編碼[Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder)。 Cookie 會進行加密及區塊處理，因為單一 cookie 大小限制不適用。 Cookie 資料不會壓縮，因為壓縮 encryped 資料可能會導致安全性問題例如[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))和[破壞](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。 如需有關 cookie 架構 TempData 提供者的詳細資訊，請參閱[CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)。

### <a name="query-strings"></a>查詢字串

您可以傳遞有限的數量的資料從一個要求到另一個方法是加入新的要求查詢字串。 這可用於擷取狀態，以持續方式，可讓連結與內嵌的狀態，要透過電子郵件或社交網路共用。 不過，基於這個理由，您應該永遠不會使用查詢字串之機密資料。 除了輕鬆地共用，在查詢字串中包括的資料可以建立的機會[跨網站要求偽造 (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))可以誘騙瀏覽惡意網站時驗證使用者的攻擊。 攻擊者可能會再竊取您的應用程式中的使用者資料，或者要代表使用者惡意動作。 任何保留的應用程式或工作階段狀態必須防範 CSRF 攻擊。 如需有關 CSRF 攻擊的詳細資訊，請參閱[防止跨站台要求偽造 (XSRF/CSRF) 攻擊，在 ASP.NET Core](../security/anti-request-forgery.md)。

### <a name="post-data-and-hidden-fields"></a>張貼資料和隱藏的欄位

資料可以儲存在隱藏的表單欄位和公佈回於下一個要求。 這是一般多頁的表單中。 不過，用戶端可能可以修改資料，因為伺服器必須一律重新驗證它。 

### <a name="cookies"></a>Cookie

Cookie 會提供在 web 應用程式儲存使用者專屬資料的方法。 每個要求傳送 cookie，因為其大小應該降到最低。 在理想情況下，只有識別碼應該與實際伺服器上儲存的資料儲存在 cookie 中。 大部分的瀏覽器限制為 4096 個位元組的 cookie。 此外，只有少數的 cookie 可供每個網域。  

Cookie 是有可能遭到竄改，因為它們必須在伺服器上驗證。 雖然用戶端上的 cookie 持久性受限於使用者介入的情況下與到期日，通常都在用戶端上的資料持續性最持久表單。

Cookie 通常可用來個人化，其中內容自訂為已知的使用者。 因為使用者只能識別，並且尚未驗證在大部分情況下，您通常可以藉由在 cookie 中儲存的使用者名稱、 帳戶名稱或唯一的使用者識別碼 （例如 GUID) 來保護 cookie。 Cookie 然後可用來存取站台的使用者個人化基礎結構。

### <a name="httpcontextitems"></a>HttpContext.Items

`Items`集合是個不錯的位置來儲存資料所需時，才處理一個特定的要求。 每個要求之後，會捨棄集合的內容。 `Items`集合最適合用做為元件或中介軟體的方式來通訊時可操作不同時間點期間要求的時間，並且具有沒有直接的方式，將參數傳遞。 如需詳細資訊，請參閱[使用 HttpContext.Items](#working-with-httpcontextitems)在本文稍後。

### <a name="cache"></a>快取

快取是有效的方式儲存和擷取資料。 您可以控制時間和其他考量為基礎的快取項目存留的期。 深入了解[快取](../performance/caching/index.md)。

<a name=session></a>

## <a name="configuring-session"></a>設定工作階段

`Microsoft.AspNetCore.Session`套件提供中介軟體管理的工作階段狀態。 若要啟用工作階段中介軟體`Startup`必須包含：

- 任一[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)記憶體快取。 `IDistributedCache`做為備份存放區使用實作工作階段。
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_)呼叫，這需要 NuGet 套件 」 Microsoft.AspNetCore.Session"。
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)呼叫。

下列程式碼會示範如何設定記憶體中工作階段提供者。

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

您可以參考從工作階段`HttpContext`之後就會安裝並設定。

如果您嘗試存取`Session`之前`UseSession`已經呼叫，例外狀況`InvalidOperationException: Session has not been configured for this application or request`就會擲回。

如果您嘗試建立新`Session`（也就是任何工作階段 cookie 具有已建立） 已經開始寫入之後`Response`串流處理的例外狀況`InvalidOperationException: The session cannot be established after the response has started`就會擲回。 例外狀況可以在 web 伺服器記錄檔。它不會顯示在瀏覽器中。

### <a name="loading-session-asynchronously"></a>非同步載入工作階段 

在 ASP.NET Core 的預設工作階段提供者會從基礎載入工作階段記錄[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)存放區以非同步方式只有當[ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync)之前明確地呼叫方法 `TryGetValue`， `Set`，或`Remove`方法。 如果`LoadAsync`則不會呼叫第一次，基礎工作階段記錄同步載入，這可能會影響應用程式能夠調整。

若要強制執行此模式的應用程式，請包裝[DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore)和[DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession)實作時擲回例外狀況的版本與`LoadAsync`方法不是之前呼叫`TryGetValue`， `Set`，或`Remove`。 註冊服務容器中的已包裝的版本。

### <a name="implementation-details"></a>實作詳細資料

工作階段會追蹤並找出要求從單一瀏覽器使用 cookie。 根據預設，此 cookie 名為"。AspNet.Session 」，並使用的路徑"/"。 Cookie 預設未指定網域，因為它不開放給用戶端指令碼頁面上 (因為`CookieHttpOnly`預設為`true`)。

若要覆寫工作階段的預設值，請使用`SessionOptions`:

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

伺服器會使用`IdleTimeout`屬性來判斷如何在工作階段前可保留長度閒置其內容會被放棄。 這個屬性是獨立的 cookie 到期日。 通過的工作階段中介軟體 （讀取或寫入） 的每個要求會重設時的逾時。

因為`Session`是*非鎖定*，如果兩個要求同時嘗試修改的工作階段中，最後一個內容，會覆寫第一個。 `Session`會實作為*一致的工作階段*，這表示所有內容會都儲存在一起。 要修改的工作階段 （不同的索引鍵） 的不同部分的兩個要求仍可能會影響彼此。

## <a name="setting-and-getting-session-values"></a>設定和取得工作階段值

工作階段透過存取`Session`屬性`HttpContext`。 這個屬性是[ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession)實作。

下列範例示範設定和取得整數和字串：

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

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

<a name=appstate-errors></a>

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

### <a name="common-errors-when-working-with-session"></a>使用工作階段時的常見錯誤

* 「 無法解析類型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' 服務嘗試啟動 'Microsoft.AspNetCore.Session.DistributedSessionStore'。 」

  這種情形通常因未能設定至少一個`IDistributedCache`實作。 如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)和[中記憶體內部快取](xref:performance/caching/memory)。

### <a name="additional-resources"></a>其他資源


* [本文件中使用的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
