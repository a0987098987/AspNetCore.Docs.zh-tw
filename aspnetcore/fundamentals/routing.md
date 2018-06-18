---
title: ASP.NET Core 中的路由
author: ardalis
description: 探索 ASP.NET Core 路由功能如何負責將傳入要求對應至路由處理常式。
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/routing
ms.openlocfilehash: a23e2e1a1dd25a57e5d6189bbd5938c48078515b
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341778"
---
# <a name="routing-in-aspnet-core"></a>ASP.NET Core 中的路由

作者：[Ryan Nowak](https://github.com/rynowak)、[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

路由功能負責將傳入要求對應至路由處理常式。 路由定義於 ASP.NET 應用程式，並在該應用程式啟動時進行設定。 路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。 利用 ASP.NET 應用程式中的路由資訊，路由功能也能夠產生對應至路由處理常式的 URL。 因此，路由可以依據 URL 找到路由處理常式，或依據路由處理常式資訊找到對應至指定路由處理常式的 URL。

>[!IMPORTANT]
> 本文件涵蓋最低層級的 ASP.NET Core 路由。 如需 ASP.NET Core MVC 路由，請參閱[路由至控制器動作](../mvc/controllers/routing.md)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>路由的基本概念

路由 (routing) 功能會使用「路由」(*route*) ([IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter) 的實作) 來：

* 將傳入要求對應至「路由處理常式」

* 產生用於回應的 URL

一般而言，應用程式有一個路由集合。 當要求到達時，會依序處理路由集合。 傳入要求會在路由集合的每個可用路由上呼叫 `RouteAsync` 方法，藉以尋找符合所要求 URL 的路由。 相反地，回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此不需要硬式編碼的 URL，這有助於可維護性。

路由會透過 `RouterMiddleware` 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。 [ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分。 若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#using-routing-middleware)。

<a name="url-matching-ref"></a>

### <a name="url-matching"></a>URL 比對

URL 比對是路由用來將傳入要求分派給「處理常式」的處理序。 這個處理序通常是根據 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。 分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。

傳入要求將進入 `RouterMiddleware`，而後者會依序在每個路由上呼叫 `RouteAsync` 方法。 `IRouter` 執行個體可將 `RouteContext.Handler` 設定為非 Null 的 `RequestDelegate`，來選擇是否要「處理」要求。 如果路由為要求設定了處理常式，則路由處理會停止，且將叫用該處理常式來處理要求。 如果已嘗試所有路由，且未找到要求的處理常式，中介軟體會呼叫下一個，而要求管線中的下一個中介軟體將被叫用。

`RouteAsync` 的主要輸入是與目前要求建立關聯的 `RouteContext.HttpContext`。 `RouteContext.Handler` 和 `RouteContext.RouteData` 輸出將在路由比對之後設定。

`RouteAsync` 期間的比對也會依據到目前為止完成的要求處理，將 `RouteContext.RouteData` 的屬性設定為適當值。 如果路由符合要求，`RouteContext.RouteData` 將包含與「結果」有關的重要狀態資訊。

`RouteData.Values` 是路由所產生之「路由值」的字典。 這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。

`RouteData.DataTokens` 是與相符路由相關之其他資料的屬性包。 提供了 `DataTokens` 來支援與每個路由相關聯的狀態資料，因此應用程式可以依據符合哪一個路由來做出決策。 這些是開發人員定義的值，**不會**以任何方式影響路由的行為。 此外，隱藏在資料語彙基元中的值可以是任何類型，相較於路由值，它必須能夠輕鬆地來回轉換字串。

`RouteData.Routers` 是成功符合要求的參與路由清單。 路由可以用巢狀方式置於彼此內部，而 `Routers` 屬性則會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。 一般而言，`Routers` 中的第一個項目是路由集合，應該用於產生 URL。 `Routers` 中的最後一個項目是相符的路由處理常式。

### <a name="url-generation"></a>URL 產生

URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。 這可讓您在處理常式和存取它們的 URL 之間建立邏輯分隔。

URL 產生遵循類似的反覆執行處理序，但一開始是呼叫路由集合之 `GetVirtualPath` 方法的使用者或架構程式碼。 然後，每個「路由」將會依序呼叫其 `GetVirtualPath` 方法，直到傳回非 Null 的 `VirtualPathData` 為止。

`GetVirtualPath` 的主要輸入是：

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

路由主要使用 `Values` 和 `AmbientValues` 所提供的路由值，以決定可能產生 URL　的位置以及要包含哪些值。 `AmbientValues` 是比對目前要求與路由系統所產生之路由值的集合。 相反地，`Values` 是指定如何產生目前作業所需之 URL 的路由值。 提供了 `HttpContext` 以防路由需要取得服務或與目前內容建立關聯的其他資料。

提示：將 `Values` 視為 `AmbientValues` 的覆寫項目集合。 URL 產生會嘗試重複使用來自目前要求的路由值，讓您輕鬆地使用相同的路由或路由值產生連結的 URL。

`GetVirtualPath` 的輸出是 `VirtualPathData`。 `VirtualPathData` 是 `RouteData` 的平行處理；它包含用於輸出 URL 的 `VirtualPath`，以及一些路由應該設定的其他屬性。

`VirtualPathData.VirtualPath` 屬性包含路由所產生的「虛擬路徑」。 根據您的需求，您可能需要進一步處理路徑。 比方說，如果您想要以 HTML 呈現產生的 URL ，則需要在前面加上應用程式的基底路徑。

`VirtualPathData.Router` 是成功產生 URL 的路由的參考。

`VirtualPathData.DataTokens` 屬性是與產生 URL 的路由相關的其他資料的字典。 這是 `RouteData.DataTokens` 的平行處理。

### <a name="creating-routes"></a>建立路由

路由提供 `Route` 類別作為 `IRouter` 的標準實作。 在呼叫 `RouteAsync` 時，`Route` 會使用「路由範本」語法來定義將比對 URL 路徑的模式。 在呼叫 `GetVirtualPath` 時，`Route` 將使用相同的路由範本來產生 URL。

大部分的應用程式將藉由呼叫 `MapRoute` 或其中一個 `IRouteBuilder` 上定義的類似擴充方法來定建立路由。 所有這些方法將建立 `Route` 的執行個體，並將它新增至路由集合。

注意：`MapRoute` 不會採用路由處理常式參數 - 它只會新增 `DefaultHandler` 將處理的路由。 由於預設處理常式是 `IRouter`，它可能決定不處理要求。 例如，ASP.NET MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。 若要深入了解如何路由至 MVC，請參閱[路由至控制器動作](../mvc/controllers/routing.md)。

這是典型 ASP.NET MVC 路由定義所使用之 `MapRoute` 呼叫的範例：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

此範本將比對 `/Products/Details/17` 等 URL 路徑，並擷取路由值 `{ controller = Products, action = Details, id = 17 }`。 路由值是通過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。 路由參數為具名。 它們透過以括弧 `{ }` 括住參數名稱來定義。

上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。 發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。 路由參數名稱之後緊接著值的等於 `=` 符號定義參數的預設值。 路由參數名稱之後的問號 `?` 則會將參數定義為選擇性。 在路由相符時，具有預設值的路由參數一定會產生路由值 - 如果沒有任何對應的 URL 路徑區段，選擇性參數將不會產生路由值。

如需路由範本功能和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)。

這個範例包含「路由條件約束」：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。 路由參數定義 `{id:int}` 定義 `id` 路由參數的「路由條件約束」。 路由條件約束會實作 `IRouteConstraint`，並檢查路由值以進行驗證。 在此範例中，路由值 `id` 必須可轉換成整數。 如需架構所提供之路由條件約束的詳細說明，請參閱[路由條件約束參考](#route-constraint-reference)。

`MapRoute` 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。 這些額外的 `MapRoute` 參數定義為 `object` 類型。 這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。

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

提示：對於簡單的路由而言，定義條件約束和預設值的內嵌語法可能更方便。 不過，內嵌語法不支援資料語彙基元等功能。

下列範例示範一些更多的功能：

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

此範本將符合 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。 即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。 預設值可以在路由範本中指定。 `article` 路由參數透過在路由參數名稱之前加上 `*`，定義為「全部擷取」參數。 全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。

這個範例會新增路由條件約束和資料語彙基元：

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

此範本將符合 `/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a>URL 產生

`Route` 類別也可以結合一組路由值與其路由範本來產生 URL。 這在邏輯上是比對 URL 路徑的反向處理序。

提示：若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。 產生的值為何？ 這與 URL 產生在 `Route` 類別中的運作方式大致相同。

這個範例使用基本的 ASP.NET MVC 樣式路由：

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

路由值為 `{ controller = Products, action = List }` 時，此路由將產生 URL `/Products/List`。 路由值會取代對應的路由參數，以形成 URL 路徑。 由於 `id` 是選擇性路由參數，因此它沒有值也不會有問題。

路由值為 `{ controller = Home, action = Index }` 時，此路由將產生 URL `/`。 所提供的路由值符合預設值，因此可以放心地省略這些值對應的區段。 請注意，這兩個產生的 URL 會使用此路由定義反覆存取，並產生用來產生 URL 的相同路由值。

提示：使用 ASP.NET MVC 的應用程式應該使用 `UrlHelper` 來產生 URL，而不是直接呼叫路由。

如需 URL 產生處理序的詳細資料，請參閱 [URL 產生參考](#url-generation-reference)。

## <a name="using-routing-middleware"></a>設定路由中介軟體

新增 NuGet 套件 "Microsoft.AspNetCore.Routing"。

在 *Startup.cs* 中，將路由新增至服務容器：

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

路由必須設定在 `Startup` 類別的 `Configure` 方法中。 下列範例使用這些 API：

* `RouteBuilder`
* `Build`
* `MapGet`  僅符合 HTTP GET 要求
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

下表顯示使用給定 URL 的回應。

| URI | 回應  |
| ------- | -------- |
| /package/create/3  | Hello! Route values: [operation, create], [id, 3] |
| /package/track/-3  | Hello! Route values: [operation, track], [id, -3] |
| /package/track/-3/ | Hello! Route values: [operation, track], [id, -3]  |
| /package/track/ | \<正常執行，沒有相符項目> |
| GET /hello/Joe | Hi, Joe! |
| POST /hello/Joe | \<正常執行，僅符合 HTTP GET> |
| GET /hello/Joe/Smith | \<正常執行，沒有相符項目> |

如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 `app.UseRouter`。 您不需要呼叫 `RouteBuilder`。

架構會提供一組擴充方法來建立路由，例如：

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

其中一些方法 (例如 `MapGet`) 需要提供 `RequestDelegate`。 路由相符時，`RequestDelegate` 將會作為「路由處理常式」。 此系列中的其他方法允許設定將作為路由處理常式的中介軟體管線。 如果 *Map* 方法不接受處理常式 (例如 `MapRoute`)，則它將使用 `DefaultHandler`。

`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。 例如，請參閱 [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) 和 [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)。

## <a name="route-template-reference"></a>路由範本參考

大括弧 (`{ }`) 內的語彙基元定義路由相符時將繫結的「路由參數」。 您可以在路由區段中定義多個路由參數，但其必須以常值分隔。 例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 和 `{action}` 之間沒有任何常值。 這些路由參數必須有一個名稱，並且可以指定其他屬性。

路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。 文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。 若要與常值路由參數分隔符號 `{` 或 `}` 相符，請重複字元 (`{{` 或 `}}`) 來將它逸出。

URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。 例如，使用範本 `files/{filename}.{ext?}` - 當 `filename` 和 `ext` 都存在時，就會填入這兩個值。 如果 URL 中只有 `filename` 存在，由於結尾的句點 `.` 是選擇性項目，因此路由相符。 下列 URL 會符合此路由：

* `/files/myFile.txt`
* `/files/myFile`

您可以使用 `*` 字元作為路由參數的前置詞以繫結至 URI 的其餘部分 - 這稱為「全部擷取」參數。 例如，`blog/{*slug}` 會符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。 全部擷取參數也可以符合空字串。

路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以 `=` 分隔。 例如，`{controller=Home}` 會定義 `Home` 作為 `controller` 的預設值。 如果 URL 中沒有用於參數的任何值，則會使用預設值。 除了預設值之外，路由參數也可以是選擇性參數 (指定方法是在參數名稱結尾附加 `?`，如 `id?` 所示)。 選擇性與「具有預設值」之間的差異在於，具有預設值的路由參數一定會產生值；選擇性參數只有在提供值時才會有值。

路由參數也可能會有條件約束，這些條件約束必須符合與 URL 繫結的路由值。 在路由參數名稱之後新增分號 `:` 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。 如果條件約束需要引數，您可以在條件約束名稱後面以括弧 `( )` 括住方式來提供這些引數。 指定多個內嵌條件約束的方法是附加另一個冒號 `:` 和條件約束名稱。 條件約束名稱會傳遞至 `IInlineConstraintResolver` 服務來建立 `IRouteConstraint` 的執行個體，以用於 URL 處理。 例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。 如需路由條件約束詳細描述和架構所提供之條件約束的清單，請參閱[路由條件約束參考](#route-constraint-reference)。

下表示範一些路由範本及其行為。

| 路由範本 | 範例比對 URL | 注意 |
| -------- | -------- | ------- |
| hello  | /hello  | 只比對單一路徑 `/hello` |
| {Page=Home} | / | 比對並將 `Page` 設定為 `Home` |
| {Page=Home}  | /Contact  | 比對並將 `Page` 設定為 `Contact` |
| {controller}/{action}/{id?} | /Products/List | 對應至 `Products` 控制器和 `List` 動作 |
| {controller}/{action}/{id?} | /Products/Details/123  |  對應至 `Products` 控制器和 `Details` 動作。  將 `id` 設定為 123 |
| {controller=Home}/{action=Index}/{id?} | /  |  對應至 `Home` 控制器和 `Index` 方法；會忽略 `id`。 |

使用範本通常是最簡單的路由方式。 條件約束和預設值也可以在路由範本外部指定。

提示：啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 `Route`) 如何符合要求。

## <a name="route-constraint-reference"></a>路由條件約束參考

路由條件約束的執行時機是 `Route` 已符合傳入 URL 的語法，並將 URL 路徑語彙基元化成路由值時。 路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出簡單的是/否決策。 某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。 例如，`HttpMethodRouteConstraint` 可以依據其 HTTP 指令動詞接受或拒絕要求。

>[!WARNING]
> 請避免針對**輸入驗證**使用條件約束，因為這麼做表示無效的輸入會導致產生 404 (找不到)，而不是 400 與適當的錯誤訊息。 路由條件約束應該用來**釐清**類似的路由，不是用來驗證特定路由的輸入。

下表示範一些路由條件約束及其預期行為。

| 條件約束 | 範例 | 範例相符項目 | 注意 |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | 符合任何整數 |
| `bool`  | `{active:bool}` | `true`, `FALSE` | 符合 `true` 或 `false` (不區分大小寫) |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | 符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | 符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `double`  | `{weight:double}` | `1.234`, `-1,001.01e8` | 符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `float`  | `{weight:float}` | `1.234`, `-1,001.01e8` | 符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告) |
| `guid`  | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | 符合有效的 `Guid` 值 |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | 符合有效的 `long` 值 |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | 字串必須至少有 4 個字元 |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | 字串不能超過 8 個字元 |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | 字串長度必須剛好是 12 個字元 |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | 字串長度必須至少有 8 個字元，但不能超過 16 個字元 |
| `min(value)` | `{age:min(18)}` | `19` | 整數值必須至少為 18 |
| `max(value)` | `{age:max(120)}` |  `91` | 整數值不能超過 120 |
| `range(min,max)` | `{age:range(18,120)}` | `91` | 整數值必須至少為 18，但不能超過 120 |
| `alpha` | `{name:alpha}` | `Rick` | 字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫) |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | 字串必須符合規則運算式 (請參閱有關定義規則運算式的提示) |
| `required`  | `{name:required}` | `Rick` |  用來強制執行在 URL 產生期間呈現非參數值 |

>[!WARNING]
> 確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性 - 它們假設 URL 不可當地語系化。 架構提供的路由條件約束不會修改路由值中儲存的值。 所有從 URL 剖析而來的路由值將儲存為字串。 例如，[Float 路由條件約束](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。

## <a name="regular-expressions"></a>規則運算式 

ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。 如需這些成員的描述，請參閱 [RegexOptions 列舉](/dotnet/api/system.text.regularexpressions.regexoptions)。

規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。 規則運算式的語彙基元必須逸出。 例如，若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，它必須在 C# 原始程式檔中將 `\` 字元輸入為 `\\`，以逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)。 `{`、`}`、'[' 和 ']' 字元必須加以逸出，方法是成對使用以逸出路由參數的分隔符號字元。  下表顯示規則運算式和逸出的版本。

| 運算式               | 注意事項 |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | 規則運算式 |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | 已逸出  |
| `^[a-z]{2}$` | 規則運算式 |
| `^[[a-z]]{{2}}$` | 已逸出  |

路由中所使用的規則運算式通常以 `^` 字元 (符合字串的起始位置) 開頭，並以 `$` 字元 (符合字串的結束位置) 結尾。 `^` 和 `$` 字元可確保規則運算式符合整個路由參數值。 若不使用 `^` 和 `$` 字元，規則運算式將符合字串內的任何子字串，這通常是不是您想要的結果。 下表顯示一些範例，並說明它們符合或無法符合的原因。

| 運算式               | String | 比對 | 註解 |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | hello | 是 | 子字串相符項目 |
| `[a-z]{2}` | 123abc456 | 是 | 子字串相符項目 |
| `[a-z]{2}` | mz | 是 | 符合運算式 |
| `[a-z]{2}` | MZ | 是 | 不區分大小寫 |
| `^[a-z]{2}$` |  hello | 否 | 請參閱上述的 `^` 和 `$` |
| `^[a-z]{2}$` |  123abc456 | 否 | 請參閱上述的 `^` 和 `$` |

如需規則運算式的詳細資訊，請參閱 [.NET Framework 規則運算式](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)。

若要將參數限制為一組已知的可能值，請使用規則運算式。 例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。 如果已傳入條件約束字典，字串 "^(list|get|create)$" 則是對等項目。 已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。

## <a name="url-generation-reference"></a>URL 產生參考

下列範例示範如何在給定路由值字典和 `RouteCollection` 的情況下產生路由的連結。

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

在上述範例的結尾產生的 `VirtualPath` 是 `/package/create/123`。

`VirtualPathContext` 建構函式的第二個參數是「環境值」的集合。 環境值為方便起見，會限制開發人員必須在特定要求內容中指定的值數目。 目前要求的目前路由值被視為用於連結產生的環境值。 例如，在 ASP.NET MVC 應用程式中，如果您處於 `HomeController` 的 `About` 動作，則不需要指定控制器路由值以連結到 `Index` 動作 (將使用 `Home` 的環境值)。

不符合參數的環境值會被忽略，當明確提供的值在 URL 中從左到右進行覆寫時，也會忽略環境值。

明確提供但不符合任何項目的值會新增至查詢字串。 下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。

| 環境值 | 明確值 | 結果 |
| -------------   | -------------- | ------ |
| controller="Home" | action="About" | `/Home/About` |
| controller="Home" | controller="Order",action="About" | `/Order/About` |
| controller="Home",color="Red" | action="About" | `/Home/About` |
| controller="Home" | action="About",color="Red" | `/Home/About?color=Red`

如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值。 例如: 

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

提供了控制器與動作的相符值時，連結產生只會產生此路由的連結。
