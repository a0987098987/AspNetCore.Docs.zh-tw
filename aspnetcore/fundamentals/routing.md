---
title: "在 ASP.NET Core 路由"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bbbcf9e4-3c4c-4f50-b91e-175fe9cae4e2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: 469c30cf66d28e82519d5eff7f2fc82d490827b7
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="routing-in-aspnet-core"></a>在 ASP.NET Core 路由

由[Ryan Nowak](https://github.com/rynowak)， [Steve Smith](https://ardalis.com/)，和[Rick Anderson](https://twitter.com/RickAndMSFT)

路由功能是負責將傳入的要求對應至路由處理常式。 在 ASP.NET 應用程式中定義和設定應用程式啟動時的路由。 路由可以選擇性地從要求中所含的 URL 擷取值，這些值就用於處理要求。 使用 ASP.NET 應用程式中的路由資訊，路由的功能也會產生對應至路由處理常式的 Url。 因此，路由，可以找到 URL 或對應至路由處理常式資訊為基礎的指定的路由處理常式的 URL 為基礎的路由處理常式。

>[!IMPORTANT]
> 本文件涵蓋的最低層級的 ASP.NET Core 路由。 ASP.NET Core MVC 路由，請參閱[路由至控制器的動作](../mvc/controllers/routing.md)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)

## <a name="routing-basics"></a>路由的基本概念

路由會使用*路由*(實作[IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) 至：

* 對應的內送要求*路由處理常式*

* 產生用於回應的 Url

一般而言，應用程式沒有單一的路由集合。 當要求到達時，會依序處理的路由集合。 藉由呼叫符合要求 URL 的路由連入要求會尋找`RouteAsync`路由集合中每個可用的路由上的方法。 相反地，回應可以使用路由來產生根據路由資訊的 Url （例如，針對重新導向或連結），因此不需要硬式編碼的 Url，可協助維護性。

路由會連接到[中介軟體](middleware.md)由管線`RouterMiddleware`類別。 [ASP.NET MVC](../mvc/overview.md)新增路由傳送至中介軟體管線，其組態的一部分。 若要深入了解使用路由作為獨立元件，請參閱[使用路由的中介軟體](#using-routing-middleware)。

<a name=url-matching-ref></a>

### <a name="url-matching"></a>URL 比對

URL 比對是由哪些路由分派傳入要求的程序*處理常式*。 此程序通常根據 URL 路徑中的資料，但是可以擴充考量在要求中的任何資料。 分派分開處理常式要求的能力是調整的大小和複雜度的應用程式的關鍵。

內送要求輸入`RouterMiddleware`，而它會呼叫`RouteAsync`序列中的每個路由上的方法。 `IRouter`執行個體選擇是否要*處理*藉由設定要求`RouteContext``Handler`為非 null `RequestDelegate`。 如果路由設定要求的處理常式，則會叫用路由處理會停止，而此處理常式來處理要求。 如果嘗試所有的路由時，而且沒有處理常式的要求中, 介軟體呼叫*下一步*並叫用要求管線中的下一個中介軟體。

主要輸入`RouteAsync`是`RouteContext``HttpContext`與目前的要求相關聯。 `RouteContext.Handler`和`RouteContext``RouteData`是會在之後路由符合設定的輸出。

相符項目期間`RouteAsync`也會設定的屬性`RouteContext.RouteData`根據到目前為止完成的要求處理的適當值。 如果路由符合要求，`RouteContext.RouteData`會包含重要的狀態資訊的相關*結果*。

`RouteData``Values`是*路由值*所產生的路由。 這些值通常由 token 化 URL，可以用來接受使用者輸入，或進一步分派做出應用程式內。

`RouteData``DataTokens`是相符路由相關的其他資料的屬性包。 `DataTokens`被提供來支援與每個路由，因此應用程式，可以根據哪一個路由決策的資料比對關聯性的狀態。 這些值是開發人員定義，並且不要**不**影響行為的任何方式的路由。 此外，隱藏資料語彙基元中的值可以是任何類型，相較於路由值，必須與字串輕鬆轉換。

`RouteData``Routers`是花費在成功比對要求中的組件的路由清單。 路由可以巢狀內，而`Routers`屬性會反映透過邏輯樹狀結構的相符項目所導致的路由路徑。 通常第一個項目`Routers`路由集合中，而且應該用於 URL 的產生。 中的最後一個項目`Routers`是相符路由處理常式。

### <a name="url-generation"></a>URL 的產生

URL 的產生是程序的路由可以建立一組路由值為基礎的 URL 路徑。 這可讓您的處理常式和 Url 存取它們的邏輯分隔。

URL 的產生遵循類似的反覆程序，但是一開始呼叫的使用者或 framework 程式碼`GetVirtualPath`路由集合的方法。 每個*路由*後來會有其`GetVirtualPath`方法呼叫序列中非 null 直到`VirtualPathData`傳回。

輸入主要`GetVirtualPath`是：

* `VirtualPathContext` `HttpContext`

* `VirtualPathContext` `Values`

* `VirtualPathContext` `AmbientValues`

路由主要會使用所提供的路由值`Values`和`AmbientValues`判斷很可能產生的 URL，以及要包含哪些值。 `AmbientValues`是從符合目前要求的路由系統所產生的路由值的集合。 相反地，`Values`會指定如何產生目前作業所需的 URL 的路由值。 `HttpContext`提供當路由需要取得的服務或與目前內容關聯的其他資料。

提示： 將`Values`為覆寫一組`AmbientValues`。 URL 的產生會嘗試重複使用目前的要求，讓您輕鬆使用相同的路由或路由值的連結產生 Url 的路由值。

輸出`GetVirtualPath`是`VirtualPathData`。 `VirtualPathData`是的平行處理`RouteData`; 它包含`VirtualPath`輸出 URL，以及一些額外的屬性應該設定路由。

`VirtualPathData` `VirtualPath`屬性包含*虛擬路徑*所產生的路由。 根據您的需求，您可能需要處理進一步的路徑。 比方說，如果您想要呈現以 HTML 產生的 URL 需要在前面加上應用程式的基底路徑。

`VirtualPathData` `Router`是已成功產生 URL 的路由的參考。

`VirtualPathData` `DataTokens`屬性是產生 URL 的路由相關的其他資料的字典。 這是的平行`RouteData.DataTokens`。

### <a name="creating-routes"></a>建立路由

路由提供`Route`類別做為標準的實作`IRouter`。 `Route`使用*路由範本*語法來定義將會比對的 URL 路徑的模式時`RouteAsync`呼叫。 `Route`將使用相同的路由範本來產生 URL 時`GetVirtualPath`呼叫。

大部分的應用程式會藉由呼叫建立路由`MapRoute`或類似上定義的擴充方法的其中一個`IRouteBuilder`。 所有這些方法會建立的執行個體`Route`並將它新增至路由集合。

注意：`MapRoute`不接受路由處理常式參數-它只會將所要處理的路由`DefaultHandler`。 由於預設處理常式是`IRouter`，它可能會決定不處理要求。 例如，ASP.NET MVC 通常設定為預設處理常式，只處理要求該相符項目，可用的控制器和動作。 若要了解有關 mvc 路由的詳細資訊，請參閱[路由至控制器動作](../mvc/controllers/routing.md)。

這是範例`MapRoute`典型的 ASP.NET MVC 路由定義所使用的呼叫：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

此範本會比對 URL 路徑，如`/Products/Details/17`擷取路由值和`{ controller = Products, action = Details, id = 17 }`。 路由會判斷的值分割的 URL 路徑區段，並比對每個含有區段*路由參數*路由範本的名稱。 路由參數為具名。 它們由以大括號括住參數名稱定義`{ }`。

上述範本也比對的 URL 路徑`/`並產生值`{ controller = Home, action = Index }`。 這是因為`{controller}`和`{action}`路由參數有預設值，而`id`路由參數為選用。 Equals`=`號後面接著值之後路由參數名稱定義參數的預設值。 問號`?`路由參數名稱定義為選擇性參數之後。 路由預設值的參數*一律*路由符合-選擇性參數不會產生路由值，如果沒有任何對應的 URL 路徑區段時，產生的路由值。

請參閱[路由範本參考](#route-template-reference)如的路由範本功能和語法的完整描述。

這個範例包含*路由條件約束*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

此範本會比對 URL 路徑，如`/Products/Details/17`，但不是`/Products/Details/Apples`。 路由參數定義`{id:int}`定義*路由條件約束*如`id`路由參數。 路由條件約束實作`IRouteConstraint`和檢查以進行驗證的路由值。 在此範例中的路由值`id`必須可轉換成整數。 請參閱[路由條件約束參考](#route-constraint-reference)如 framework 所提供的路由條件約束的詳細說明。

其他多載`MapRoute`接受值`constraints`， `dataTokens`，和`defaults`。 這些額外參數的`MapRoute`定義為型別`object`。 這些參數通常是用來傳遞匿名型別的物件，其中的屬性名稱的匿名型別比對路由參數名稱。

下列兩個範例會建立對等的路由：

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

提示： 內嵌語法，來定義條件約束和預設值可以是簡單的路由更方便。 不過，有功能，例如資料語彙基元不支援的內嵌語法。

這個範例示範一些更多的功能：

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

此範本會比對 URL 路徑，如`/Blog/All-About-Routing/Introduction`將擷取的值和`{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。 預設路由值`controller`和`action`即使範本中沒有任何對應的路由參數所產生的路由。 在路由範本中，可以指定預設值。 `article`路由參數定義為*包羅萬象*星號外觀`*`路由參數名稱之前。 所有的路由參數擷取 URL 路徑的其餘部分，而且也可以比對空字串。

這個範例會將路由條件約束和資料語彙基元：

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

此範本會比對 URL 路徑，如`/Products/5`將擷取的值和`{ controller = Products, action = Details, id = 5 }`和資料語彙基元`{ locale = en-US }`。

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

<a name=id1></a>

### <a name="url-generation"></a>URL 的產生

`Route`類別也可以執行一組路由值結合其路由範本的 URL 的產生。 這是邏輯上對應的 URL 路徑的反向程序。

提示： 若要進一步了解 URL 的產生，假設您想要產生並考慮如何路由範本將會比對該 URL 哪些的 URL。 會產生值為何？ 這相當於粗略 URL 層代中的運作方式`Route`類別。

這個範例會使用基本的 ASP.NET MVC 樣式路由：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

路由值`{ controller = Products, action = List }`，此路由會產生 URL `/Products/List`。 路由值取代對應的路由參數，以形成 URL 路徑。 因為`id`是選用路由參數，它沒有值的任何問題。

路由值`{ controller = Home, action = Index }`，此路由會產生 URL `/`。 所提供的路由值符合預設值，因此可以放心地省略這些值對應的區段。 請注意，產生這兩個 Url 會與此路由定義的反覆存取產生相同的路由值，用來產生 URL。

提示： 使用 ASP.NET MVC 應用程式應該使用`UrlHelper`產生而不是直接路由至呼叫的 Url。

如需 URL 的產生程序的詳細資訊，請參閱[url 產生參考](#url-generation-reference)。

## <a name="using-routing-middleware"></a>使用路由的中介軟體

新增 NuGet 封裝 「 Microsoft.AspNetCore.Routing"。

新增至服務容器中路由*Startup.cs*:

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

必須在設定路由`Configure`方法中的`Startup`類別。 下列範例會使用這些 Api:

* `RouteBuilder`
* `Build`
* `MapGet`比對僅 HTTP GET 要求
* `UseRouter`

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)
{
    var trackPackageRouteHandler = new RouteHandler(context =>
    {
        var routeValues = context.GetRouteData().Values;
        return context.Response.WriteAsync(
            $"Hello! Route values: {string.Join(", ", routeValues)}");
    });

    var routeBuilder = new RouteBuilder(app, trackPackageRouteHandler);

    routeBuilder.MapRoute(
        "Track Package Route",
        "package/{operation:regex(^(track|create|detonate)$)}/{id:int}");

    routeBuilder.MapGet("hello/{name}", context =>
    {
        var name = context.GetRouteValue("name");
        // This is the route handler when HTTP GET "hello/<anything>"  matches
        // To match HTTP GET "hello/<anything>/<anything>,
        // use routeBuilder.MapGet("hello/{*name}"
        return context.Response.WriteAsync($"Hi, {name}!");
    });

    var routes = routeBuilder.Build();
    app.UseRouter(routes);
}
```

下表顯示與指定之 Uri 的回應。

| URI | 回應  |
| ------- | -------- |
| /package/create/3  | Hello! 路由值: [作業中，建立]，[id，3] |
| 封裝/追蹤 /-3  | Hello! 路由值: [作業，追蹤]，[id，-3] |
| / 封裝追蹤 /-3 / | Hello! 路由值: [作業，追蹤]，[id，-3]  |
| /package/追蹤 / | \<落入沒有相符項目 > |
| 取得 /hello/Joe | 您好，Joe ！ |
| POST /hello/Joe | \<落入、 符合 HTTP GET 只 > |
| 取得 /hello/Joe/Smith | \<落入沒有相符項目 > |

如果您要設定單一路由，呼叫`app.UseRouter`傳入`IRouter`執行個體。 您不需要呼叫`RouteBuilder`。

架構會提供一組擴充方法，例如建立路由：

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

例如，這些方法的某些`MapGet`需要`RequestDelegate`提供。 `RequestDelegate`會用作*路由處理常式*路由時符合。 此系列中的其他方法可讓設定中介軟體管線，將作為路由處理常式。 如果*對應*方法不會接受處理常式，例如`MapRoute`，則它會使用`DefaultHandler`。

`Map[Verb]`方法會使用條件約束限制 HTTP 指令動詞的方法名稱中的路由。 例如，請參閱[MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88)和[MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)。

## <a name="route-template-reference"></a>路由範本參考

大括號內的語彙基元 (`{ }`) 定義*路由參數*它將繫結如果路由會比對。 您可以定義一個以上的路由參數的路由區段中，但必須以常值。 例如`{controller=Home}{action=Index}`不是有效的路由，因為沒有任何常值之間`{controller}`和`{action}`。 這些路由參數必須有名稱，並可能有其他屬性指定。

不是路由參數的常值文字 (例如， `{id}`) 及路徑分隔符號`/`必須符合在 URL 中的文字。 文字比對是區分大小寫，根據已解碼的表示法的 Url 路徑。 要比對常值的路由參數分隔符號`{`或`}`，它藉由重複的字元逸出 (`{{`或`}}`)。

嘗試擷取具有選用的檔案副檔名的檔案名稱的 URL 模式有其他考量。 例如，使用範本`files/{filename}.{ext?}`-當兩者`filename`和`ext`存在，就會填入這兩個值。 如果只有`filename`在 URL 中，路由相符的項目存在，因為結尾的句點`.`是選擇性的。 下列 Url 會符合此路由：

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

您可以使用`*`字元做為前置詞以路由參數繫結至其餘的 URI-這會呼叫*包羅萬象*參數。 例如，`blog/{*slug}`會比對任何 URI，以啟動`/blog`和其後的任何值 (這會指派給`slug`路由值)。 Catch all 參數也可以比對空字串。

路由參數可能*預設值*、 指定藉由指定預設參數名稱之後，以分隔`=`。 例如，`{controller=Home}`會定義`Home`的預設值為`controller`。 出現在 URL 中的參數沒有值時，會使用預設值。 預設值，除了路由參數可能會是選擇性 (藉由附加指定`?`參數名稱，如下所示的結尾`id?`)。 選擇性之間的差異以及 「 具有預設值 」 是預設值的路由參數一定會產生值。選擇性的參數在其中一個提供時，才有值。

路由參數可能也會有條件約束，它必須符合繫結從 URL 的路由值。 新增分號`:`和條件約束名稱之後指定的路由參數名稱*內部條件約束*路由參數上。 如果條件約束需要引數所提供的括號內`( )`條件約束名稱後面。 可以指定多個內嵌條件約束附加另一個冒號`:`和條件約束名稱。 條件約束名稱傳遞至`IInlineConstraintResolver`服務來建立的執行個體`IRouteConstraint`URL 處理使用。 例如，路由範本`blog/{article:minlength(10)}`指定`minlength`與引數的條件約束`10`。 詳細描述路由條件約束和 framework 所提供的條件約束的清單，請參閱[路由條件約束參考](#route-constraint-reference)。

下表示範一些路由範本，以及它們的行為。

| 路由範本 | 範例對應 URL | 注意 |
| -------- | -------- | ------- |
| hello  | /hello  | 只比對單一路徑`/hello` |
| {頁面 = Home} | / | 比對，並設定`Page`至`Home` |
| {頁面 = Home}  | / 連絡人  | 比對，並設定`Page`至`Contact` |
| {controller} / {action} / {id} 嗎？ | / 產品/清單 | 對應至`Products`控制站和`List`動作 |
| {controller} / {action} / {id} 嗎？ | / 產品/詳細資料/123  |  對應至`Products`控制站和`Details`動作。  `id`設定為 123 |
| {控制器 = Home} / {動作 = 索引} / {id} 嗎？ | /  |  對應至`Home`控制站和`Index`方法。`id`會被忽略。 |

使用範本通常是最簡單的方式路由。 條件約束和預設值也可以指定外部的路由範本。

提示： 啟用[記錄](logging.md)若要查看如何在路由實作中，例如內建`Route`，符合要求。

## <a name="route-constraint-reference"></a>路由條件約束參考

路由條件約束時執行`Route`具有比對傳入 URL 的語法和語彙基元化成路由值的 URL 路徑。 路由條件約束通常會檢查透過路由範本相關聯的路由值，並進行簡單的 yes/no 決策的相關值可接受。 某些路由條件約束會使用以外的路由值的資料，請考慮是否可以路由傳送要求。 例如，`HttpMethodRouteConstraint`可以接受或拒絕要求，根據其 HTTP 指令動詞。

>[!WARNING]
> 請避免使用條件約束**輸入驗證**，因為這麼做，意味著該無效的輸入會導致 「 404 （找不到） 而不是 400 與適當的錯誤訊息。 應該用來路由條件約束**釐清**類似的路由，不是用來驗證特定路由的輸入。

下表示範一些路由條件約束，以及其預期的行為。

| 條件約束 | 範例 | 範例相符項目 | 注意 |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | 比對任何整數 |
| `bool`  | `{active:bool}` | `true`, `FALSE` | 相符項目`true`或`false`（不區分大小寫） |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | 符合有效`DateTime`值 （在文化特性而異-看見警告） |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 符合有效`decimal`值 （在文化特性而異-看見警告） |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | 符合有效`double`值 （在文化特性而異-看見警告） |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | 符合有效`float`值 （在文化特性而異-看見警告） |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 符合有效`Guid`值 |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 符合有效`long`值 |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 字串必須至少 4 個字元 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | 必須不能超過 8 個字元字串。 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 字串必須完全 12 個字元長 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 字串必須至少 8 到 16 個以上字元 |
| `min(value)` | `{age:min(18)}` | `19` | 整數值必須至少 18 |
| `max(value)` | `{age:max(120)}` |  `91` | 整數值必須不能超過 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 整數值必須至少為 18，但不是能超過 120 |
| `alpha` | `{name:alpha}` | `Rick` | 字串必須包含一個或多個字母的字元 (`a`-`z`、 不區分大小寫) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 字串必須符合規則運算式 （請參閱秘訣定義規則運算式） |
| `required`  | `{name:required}` | `Rick` |  用來強制執行非參數值，也會顯示 URL 產生期間 |

>[!WARNING]
> 請確認 URL 的路由條件約束可以轉換成 CLR 型別 (例如`int`或`DateTime`) 一律使用文化特性而異-它們假設的 URL 是不可當地語系化。 Framework 提供的路由條件約束不會修改中的路由值儲存的值。 所有的路由值剖析從 URL 會儲存為字串。 例如， [Float 路由條件約束](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。

## <a name="regular-expressions"></a>規則運算式 

加入 ASP.NET Core framework`RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant`規則運算式建構函式。 請參閱[RegexOptions 列舉型別](https://docs.microsoft.com/dotnet/api/system.text.regularexpressions.regexoptions)如需這些成員的說明。

規則運算式會使用分隔符號，類似於所使用的路由和 C# 語言的語彙基元。 規則運算式的語彙基元必須逸出。 例如，若要使用規則運算式`^\d{3}-\d{2}-\d{4}$`路由，在它需要擁有`\`字元輸入為`\\`C# 原始程式檔來逸出`\`字串逸出字元 (除非使用[逐字字串常值](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)。 `{` ， `}` ，' [' 和 ']' 必須成對使用加以逸出路由參數的分隔符號字元逸出字元。  下表顯示規則運算式和逸出的版本。

| 運算式               | 注意事項 |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | 規則運算式 |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | 逸出  |
| `^[a-z]{2}$` | 規則運算式 |
| `^[[a-z]]{{2}}$` | 逸出  |

路由中所用的規則運算式通常會啟動`^`字元 （比對字串的起始位置），且結尾`$`字元 （比對結束之字串的位置）。 `^`和`$`確保規則運算式比對整個路由參數值的字元。 不含`^`和`$`字元的規則運算式會比對的字串，通常是不是您想要的任何子字串。 下表顯示一些範例，並說明為何比對或比對會失敗。

| 運算式               | 字串 | 比對 | 註解 |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | 是 | 子字串相符項目 |
| `[a-z]{2}` | 123abc456 | 是 | 子字串相符項目 |
| `[a-z]{2}` | mz | 是 | 符合運算式 |
| `[a-z]{2}` | MZ | 是 | 不區分大小寫 |
| `^[a-z]{2}$` |  hello | no | 請參閱`^`和`$`上方 |
| `^[a-z]{2}$` |  123abc456 | no | 請參閱`^`和`$`上方 |

請參閱[.NET Framework 規則運算式](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)如需有關規則運算式語法。

若要限制一組已知的可能值的參數，請使用規則運算式。 例如`{action:regex(^(list|get|create)$)}`只符合`action`路由值`list`， `get`，或`create`。 如果傳遞到此條件約束的字典，字串"^ (清單 | get | 建立) $"相當。 條件約束的條件約束字典 （沒有內嵌在樣板內） 傳入不符合其中一個已知的條件約束也會被視為規則運算式。

## <a name="url-generation-reference"></a>URL 產生參考

下列範例示範如何產生連結至指定的路由值字典的路由和`RouteCollection`。

[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

`VirtualPath`上述範例的結尾產生`/package/create/123`。

第二個參數`VirtualPathContext`建構函式是一組*環境值*。 環境值會提供方便起見，來限制開發人員必須在特定的要求內容中指定的值數目。 目前的路由值，在目前要求的連結產生視為環境的值。 例如，如果您是在 ASP.NET MVC 應用程式在`About`動作`HomeController`，您不需要指定要連結到的控制器的路由值`Index`動作 (環境的值`Home`將使用)。

環境不符合參數的值會被忽略，並明確地提供的值覆寫時，會從左到右，也會忽略環境的值在 URL 中。

值提供的但其不符合任何項目會加入至查詢字串。 下表顯示結果時使用的路由範本`{controller}/{action}/{id?}`。

| 環境的值 | 明確的值 | 結果 |
| -------------   | -------------- | ------ |
| 控制器 = 常用 | 動作 = 「 關於 」 | `/Home/About` |
| 控制器 = 常用 | 控制器 ="Order"，動作 = 「 關於 」 | `/Order/About` |
| 控制站 「 家用 」，= color ="Red" | 動作 = 「 關於 」 | `/Home/About` |
| 控制器 = 常用 | 動作 = [關於]，色彩 ="Red" | `/Home/About?color=Red`

如果路由了未對應至參數的預設值，該值會明確提供，它必須符合預設值。 例如: 

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

當未提供對應的控制器與動作值時，連結產生只會產生此路由的連結。
