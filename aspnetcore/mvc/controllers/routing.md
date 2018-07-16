---
title: ASP.NET Core 中的路由至控制器動作
author: rick-anderson
description: 了解 ASP.NET Core MVC 如何使用路由中介軟體來比對內送要求的 URL，並將這些 URL 對應至動作。
ms.author: riande
ms.date: 03/14/2017
uid: mvc/controllers/routing
ms.openlocfilehash: 081332fd1007db5292a8812fc6ae934cb07dffb5
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952977"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a>ASP.NET Core 中的路由至控制器動作

作者：[Ryan Nowak](https://github.com/rynowak) 與 [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC 使用路由[中介軟體](xref:fundamentals/middleware/index)來比對內送要求的 URL，並將這些 URL 對應至動作。 路由是在啟動程式碼或屬性中定義。 路由描述 URL 路徑應該如何與動作進行比對。 路由也可用來產生回應中所送出的連結 URL。 

動作可以使用慣例路由或屬性路由。 將路由放在控制器或動作上，即可讓它使用屬性路由。 如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。

本文件將說明 MVC 與路由之間的互動，並說明一般 MVC 應用程式如何使用路由功能。 如需進階路由的詳細資料，請參閱[路由](xref:fundamentals/routing)。

## <a name="setting-up-routing-middleware"></a>設定路由中介軟體

在 *Configure* 方法中，您可能會看到類似如下的程式碼：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

在 `UseMvc` 呼叫內，`MapRoute` 是用來建立單一路由，稱為 `default` 路由。 大多數 MVC 應用程式會使用具有類似於 `default` 路由之範本的路由。

路由範本 `"{controller=Home}/{action=Index}/{id?}"` 可能符合某個 URL 路徑 (例如 `/Products/Details/5`)，並將透過 Token 化路徑來擷取路由值 `{ controller = Products, action = Details, id = 5 }`。 MVC 會嘗試尋找名為 `ProductsController` 的控制器，並執行動作 `Details`：

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

請注意，在此範例中，模型繫結會在叫用此動作時，使用 `id = 5` 值將 `id` 參數設定為 `5`。 如需詳細資料，請參閱[模型繫結](../models/model-binding.md)。

使用 `default` 路由：

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

路由範本：

* `{controller=Home}` 將 `Home` 定義為預設的 `controller`

* `{action=Index}` 將 `Index` 定義為預設的 `action`

* `{id?}` 將 `id` 定義為選擇性

預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。 如需路由範本語法的詳細描述，請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)。

`"{controller=Home}/{action=Index}/{id?}"` 可能符合 URL 路徑 `/`，並將產生路由值 `{ controller = Home, action = Index }`。 `controller` 和 `action` 的值使用預設值；`id` 不會產生值，因為 URL 路徑中沒有對應的區段。 MVC 會使用這些路由值來選取 `HomeController` 和 `Index` 動作：

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

使用此控制器定義和路由範本，就會對下列任何 URL 路徑執行 `HomeController.Index` 動作：

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

`UseMvcWithDefaultRoute` 方法很方便：

```csharp
app.UseMvcWithDefaultRoute();
```

可用來取代：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`UseMvc` 和 `UseMvcWithDefaultRoute` 會將 `RouterMiddleware` 的執行個體新增至中介軟體管線。 MVC 不會直接與中介軟體互動，而是使用路由來處理要求。 MVC 會透過 `MvcRouteHandler` 的執行個體連線到路由。 `UseMvc` 內的程式碼類似如下：

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc` 不會直接定義任何路由，而是將預留位置新增至 `attribute` 路由的路由集合。 多載 `UseMvc(Action<IRouteBuilder>)` 可讓您新增自己的路由，同時支援屬性路由。  `UseMvc` 及其所有變化會新增屬性路由的預留位置；不論您如何設定`UseMvc`，一律可以使用屬性路由。 `UseMvcWithDefaultRoute` 會定義預設路由並支援屬性路由。 [屬性路由](#attribute-routing-ref-label)一節包含屬性路由的詳細資料。

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>慣例路由

`default` 路由：

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

即為「慣例路由」的範例。 我們將此樣式稱為「慣例路由」，因為它會建立 URL 路徑的「慣例」：

* 第一個路徑區段對應至控制器名稱

* 第二個對應至動作名稱

* 第三個區段代表用來對應至模型實體的選擇性 `id`

使用此 `default` 路由，URL 路徑 `/Products/List` 會對應至 `ProductsController.List` 動作；而 `/Blog/Article/17` 會對應至 `BlogController.Article`。 此對應**只會**根據控制器和動作名稱，而不會根據命名空間、來源檔案位置或方法參數。

> [!TIP]
> 慣例路由與預設路由的搭配使用可讓您快速建置應用程式，而不需要針對每個定義的動作產生新的 URL 模式。 針對具有 CRUD 樣式動作的應用程式，在控制器之間保持一致的 URL 有助於簡化程式碼，並讓 UI 更容易預測。

> [!WARNING]
> 路由範本將 `id` 定義為選擇性，也就是您的動作可以直接執行，而不需要將識別碼當作 URL 的一部分提供。 如果省略 URL 中的 `id`，通常表示它會由模型繫結設定為 `0`，因此在符合 `id == 0` 的資料庫中找不到任何實體。 屬性路由可提供您更細微的控制，讓某些動作需要此識別碼，而其他動作則不需要。 依照慣例，本文件將會包含可能出現在正確使用中的選擇性參數 (例如 `id`)。

## <a name="multiple-routes"></a>多個路由

您可以將更多呼叫新增至 `MapRoute`，以在 `UseMvc` 內新增多個路由。 這樣做可讓您定義多個慣例，或新增特定動作專用的慣例路由，例如：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

這裡的 `blog` 路由是「專用慣例路由」，也就是它會使用慣例路由系統，但專用於特定動作。 由於 `controller` 和 `action` 並未作為參數出現在路由範本中，它們只能有預設值，因此此路由一律會對應至動作 `BlogController.Article`。

路由集合中的路由已經過排序，並將依其新增順序進行處理。 因此在此範例中，會先嘗試 `blog` 路由，再嘗試 `default` 路由。

> [!NOTE]
> 「專用慣例路由」通常會使用 `{*article}` 等 catch-all 路由參數，來擷取 URL 路徑的其餘部分。 這可能會讓路由變得「太窮盡」，也就是它會比對您想要讓其他路由比對的 URL。 將這些「窮盡」路由放在路由表後面可解決此問題。

### <a name="fallback"></a>後援

在要求處理過程中，MVC 會確認路由值是否可用來尋找應用程式中的控制器和動作。 如果路由值未符合任何動作，則不會將此路由視為一個相符項目，並將嘗試下一個路由。 這稱為「遞補」，其目的在於簡化慣例路由重疊的情況。

### <a name="disambiguating-actions"></a>釐清動作

若透過路由符合兩個動作，MVC 必須釐清並選擇「最佳」候選項目，否則會擲回例外狀況。 例如: 

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

此控制器定義兩個符合 URL 路徑 `/Products/Edit/17` 和路由資料 `{ controller = Products, action = Edit, id = 17 }` 的動作。 這是 MVC 控制器的典型模式，其中 `Edit(int)` 會顯示用以編輯產品的表單，而 `Edit(int, Product)` 會處理已張貼的表單。 若要執行這項操作，MVC 必須在要求為 HTTP `POST` 時選擇 `Edit(int, Product)`，並在 HTTP 動詞命令為任何其他項目時選擇 `Edit(int)`。

`HttpPostAttribute` ( `[HttpPost]` ) 是 `IActionConstraint` 的實作，只有在 HTTP 動詞命令為 `POST` 時，才能選取此動作。 `IActionConstraint` 的存在使 `Edit(int, Product)` 比 `Edit(int)`「更符合」，因此會先嘗試 `Edit(int, Product)`。

您只需要在特殊情況下撰寫自訂 `IActionConstraint` 實作，但請務必了解 `HttpPostAttribute` 等屬性的角色 (其他 HTTP 動詞命令會定義類似的屬性)。 在慣例路由中，當動作是 `show form -> submit form` 工作流程的一部分時，通常會使用相同的動作名稱。 檢閱[了解 IActionConstraint](#understanding-iactionconstraint) 一節之後，會更清楚此模式的便利性。

如果多個路由相符，而且 MVC 找不到「最佳」路由，則會擲回 `AmbiguousActionException`。

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>路由名稱

下列範例中的字串 `"blog"` 和 `"default"` 是路由名稱：


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

路由名稱為路由提供邏輯名稱，因此可以使用具名路由來產生 URL。 當路由排序可能使 URL 產生作業變得複雜時，這樣做可大幅簡化 URL 產生作業。 在整個應用程式內路由名稱必須是唯一的。

路由名稱不會影響要求的 URL 比對或處理，只會用於 URL 產生。 [路由](xref:fundamentals/routing)提供 URL 產生的詳細資訊，包括 MVC 特定協助程式中的 URL 產生。

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>屬性路由

屬性路由使用一組屬性，將動作直接對應至路由範本。 在下列範例中，在 `Configure` 方法中使用 `app.UseMvc();`，且未傳遞任何路由。 `HomeController` 會比對一組 類似於預設路由 `{controller=Home}/{action=Index}/{id?}` 所比對的 URL：

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

`HomeController.Index()` 動作會針對 URL 路徑 `/`、`/Home` 或 `/Home/Index` 的任何一個來執行。

> [!NOTE]
> 此範例醒目提示屬性路由與慣例路由之間的主要程式設計差異。 屬性路由需要更多輸入才能指定路由，而慣例預設路由處理路由的方式則更簡潔。 不過，屬性路由允許 (並需要) 精確地控制套用至每個動作的路由範本。

使用屬性路由，選取動作時「不會」根據控制器名稱和動作名稱。 此範例會符合與上一個範例相同的 URL。

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> 上述路由範本未定義 `action`、`area` 和 `controller` 的路由參數。 事實上，屬性路由中不允許有這些路由參數。 由於路由範本已與一個動作建立關聯，因此剖析 URL 中的動作名稱並沒有任何意義。

## <a name="attribute-routing-with-httpverb-attributes"></a>使用 Http[Verb] 屬性的屬性路由

屬性路由也可以使用 `Http[Verb]` 屬性，例如 `HttpPostAttribute`。 這些屬性都會接受路由範本。 此範例顯示兩個符合相同路由範本的動作：

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

針對 `/products` 等 URL 路徑，當 HTTP 動詞命令為 `GET` 時，會執行 `ProductsApi.ListProducts`；當 HTTP 動詞命令為 `POST` 時，會執行 `ProductsApi.CreateProduct`。 屬性路由會先根據路由屬性所定義的一組路由範本來比對 URL。 一旦有路由範本相符，就會套用 `IActionConstraint` 條件約束以決定可執行的動作。

> [!TIP]
> 建置 REST API 時，很少會想要在動作方法上使用 `[Route(...)]`。 最好是使用更明確的 `Http*Verb*Attributes`，以精確地指定 API 的支援項目。 REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。

由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。 在此範例中，`id` 是 URL 路徑的必要部分。

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)` 動作會針對 `/products/3` 等 URL 路徑來執行，但不會針對 `/products` 等 URL 路徑來執行。 如需路由範本和相關選項的完整描述，請參閱[路由](../../fundamentals/routing.md)。

## <a name="route-name"></a>路由名稱

下列程式碼會定義 `Products_List` 的「路由名稱」：

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

您可以使用路由名稱根據特定路由來產生 URL。 路由名稱不會影響路由的 URL 比對行為，只會用於 URL 產生。 在整個應用程式內路由名稱必須是唯一的。

> [!NOTE]
> 慣例「預設路由」與此相反，只會將 `id` 參數定義為選擇性 (`{id?}`)。 能夠精確指定 API 有些優點，像是允許將 `/products` 和 `/products/5` 分派至不同的動作。

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>合併路由

為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。 控制器上定義的任何路由範本都會附加到動作上的路由範本之前。 將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

在此範例中，URL 路徑 `/products` 可能符合 `ProductsApi.ListProducts`，而 URL 路徑 `/products/5` 可能符合 `ProductsApi.GetProduct(int)`。 由於這兩種動作是以 `HttpGetAttribute` 裝飾，因此只會符合 HTTP `GET`。

套用至開頭為 `/` 之動作的路由範本，無法與套用至控制器的路由範本合併。 此範例會比對一組類似於「預設路由」的 URL 路徑。

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a>排序屬性路由

相較於依已定義順序執行的慣例路由，屬性路由會建立樹狀並同時比對所有路由。 此行為如同依理想的排序來排列路由項目，最明確的路由會有機會在較泛型的路由之前執行。

例如，`blog/search/{topic}` 等路由比 `blog/{*article}` 等路由更明確。 邏輯上來說，預設會先「執行」`blog/search/{topic}`，因為這是唯一合理的排序。 使用慣例路由，開發人員會負責依想要的順序來排列路由。

您可以使用架構提供之所有路由屬性的 `Order` 屬性，來設定屬性路由的順序。 路由會依 `Order` 屬性的遞增排序來處理。 預設順序為 `0`。 使用 `Order = -1` 設定的路由會在未設定順序的路由之前執行。 使用 `Order = 1` 設定的路由會在預設路由排序之後執行。

> [!TIP]
> 請避免依賴 `Order`。 如果您的 URL 空間需要明確的順序值才能正確地路由，則同樣也可能會使用戶端混淆。 一般而言，屬性路由會透過 URL 比對來選取正確的路由。 如果用於 URL 產生的預設順序無效，使用路由名稱作為覆寫通常會比套用 `Order` 屬性更簡單。

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>路由範本中的語彙基元取代 ([controller]、[action]、[area])

為了方便起見，屬性路由支援以方括號 (`[`、`]`) 括住語彙基元的「語彙基元取代」。 語彙基元 `[action]`、`[area]` 和 `[controller]` 會分別以定義路由之動作的動作名稱值、區域名稱值和控制器名稱值來取代。 在此範例中，動作可能符合註解中所述的 URL 路徑：

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

語彙基元取代會在建立屬性路由的最後一個步驟發生。 上述範例的運作方式與下列程式碼相同：

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

屬性路由也可以透過繼承來合併。 這與語彙基元取代結合會特別強大。

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

語彙基元取代也適用於屬性路由所定義的路由名稱。 `[Route("[controller]/[action]", Name="[controller]_[action]")]` 會針對每個動作產生唯一的路由名稱。

若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>多個路由

屬性路由支援定義多個路由來達到相同的動作。 最常見的用法是模擬「預設慣例路由」的行為，如下列範例所示：

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

將多個路由屬性放在控制器上，表示這些屬性會各自與動作方法上的每個路由屬性合併。

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

當一個動作上放有多個路由屬性 (實作`IActionConstraint`) 時，則每個動作條件約束都會與屬性中用來定義的路由範本合併。

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> 雖然在動作上使用多個路由看似強大，但最好保持簡單且妥善定義的應用程式 URL 空間。 只有必要時，才在動作上使用多個路由；例如，為了支援現有的用戶端。

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>指定屬性路由的選擇性參數、預設值和條件約束

屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

如需路由範本語法的詳細描述，請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)。

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>使用 `IRouteTemplateProvider` 的自訂路由屬性

架構中提供的所有路由屬性 (`[Route(...)]`、`[HttpGet(...)]` 等) 都會實作 `IRouteTemplateProvider` 介面。 當應用程式啟動並使用實作 `IRouteTemplateProvider` 的屬性來建立初始路由集時，MVC 會尋找控制器類別和動作方法上的屬性。

您可以實作 `IRouteTemplateProvider` 來定義自己的路由屬性。 每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

上述範例中的屬性會在套用 `[MyApiController]` 時，自動將 `Template` 設定為 `"api/[controller]"`。

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>使用應用程式模型自訂屬性路由

「應用程式模型」是在啟動時建立的物件模型，其中包含 MVC 用來路由及執行動作的所有中繼資料。 「應用程式模型」包含從路由屬性收集的所有資料 (透過 `IRouteTemplateProvider`)。 您可以撰寫「慣例」來修改啟動時的應用程式模型，以自訂路由的運作方式。 本節簡單示範如何使用應用程式模型來自訂路由。

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>混合路由：屬性路由與慣例路由

MVC 應用程式可以混用慣例路由與屬性路由。 控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。

動作可以使用慣例路由或屬性路由。 將路由放在控制器或動作上，即可讓它使用屬性路由。 定義屬性路由的動作無法透過慣例路由到達，反之亦然。 控制器上的**任何**路由屬性可讓控制器中的所有動作使用屬性路由。

> [!NOTE]
> 這兩種路由系統的區別在於 URL 符合某個路由範本之後所套用的程序。 在慣例路由中，會使用相符項目中的路由值，從所有慣例路由動作的查閱資料表中選擇動作和控制器。 在屬性路由中，每個範本已與一個動作建立關聯，因此不需要進一步查閱。

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL 產生

MVC 應用程式可以使用路由的 URL 產生功能，來產生動作的 URL 連結。 產生 URL 可排除硬式編碼的 URL，讓程式碼更穩定且更容易維護。 本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 產生運作方式的基本概念。 如需 URL 產生的詳細描述，請參閱[路由](../../fundamentals/routing.md)。

`IUrlHelper` 介面是 MVC 與用於產生 URL 的路由之間的基礎結構部分。 您將會透過控制器、檢視和檢視元件中 `Url` 屬性，來尋找可用的 `IUrlHelper` 執行個體。

在此範例中，會透過 `Controller.Url` 屬性使用 `IUrlHelper` 介面來產生另一個動作的 URL。

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

如果應用程式使用預設慣例路由，`url` 變數的值會是 URL 路徑字串 `/UrlGeneration/Destination`。 此 URL 路徑是透過路由所建立，結合了目前要求中的路由值 (環境值) 與傳遞至 `Url.Action` 的值，並在路由範本中取代這些值：

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。 不具有任何值的路由參數可以使用預設值 (若有)；如果是選擇性，則可以略過 (如此範例中的 `id` 所示)。 如果任何必要的路由參數沒有對應的值，URL 產生會失敗。 如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。

上述 `Url.Action` 範例假設使用慣例路由，但 URL 產生的運作方式會類似屬性路由，雖然概念完全不同。 在慣例路由中，使用路由值來展開範本，而且 `controller` 和 `action` 的路由值通常會出現在該範本中 (這是因為路由比對的 URL 符合「慣例」)。 在屬性路由中，`controller` 和 `action` 的路由值不可以出現在範本中，而是用來查閱要使用的範本。

此範例使用屬性路由：

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC 建立所有屬性路由動作的查閱資料表，並將比對 `controller` 和 `action` 值，以選取要用於 URL 產生的路由範本。 在上述範例中，會產生 `custom/url/to/destination`。

### <a name="generating-urls-by-action-name"></a>由動作名稱產生 URL

`Url.Action` (`IUrlHelper` . `Action`) 及所有相關多載的基本概念，都是您想要透過指定控制器名稱和動作名稱，來指定連結的項目。

> [!NOTE]
> 使用 `Url.Action` 時，會為您指定`controller` 和 `action` 的目前路由值 (`controller` 和 `action` 的值都屬於「環境值」**和「值」**)。 方法 `Url.Action` 一律會使用 `action` 和 `controller` 的目前值，而且會產生路由至目前動作的 URL 路徑。

路由會嘗試使用環境值中的值，來填入您在產生 URL 時未提供的資訊。 使用 `{a}/{b}/{c}/{d}` 等路由和環境值 `{ a = Alice, b = Bob, c = Carol, d = David }`，路由就會有足夠的資訊來產生不含任何其他值的 URL (因為所有路由參數都具有值)。 如果您新增值 `{ d = Donovan }`，則會忽略值 `{ d = David }`，而且產生的 URL 路徑會是 `Alice/Bob/Carol/Donovan`。

> [!WARNING]
> URL 路徑是階層式。 在上述範例中，如果您新增了值 `{ c = Cheryl }`，則會忽略 `{ c = Carol, d = David }` 這兩個值。 在此情況下，不再有 `d` 的值，因此 URL 產生會失敗。 您必須指定 `c` 和 `d` 所需的值。  使用預設路由 (`{controller}/{action}/{id?}`) 可能會遇到此問題，但實際上很少會遇到此行為，因為 `Url.Action` 一律會明確指定 `controller` 和 `action` 值。

較長的 `Url.Action` 多載還會接受一個額外的「路由值」物件，來為 `controller` 和 `action` 以外的路由參數提供值。 您最常會在搭配 `id` 時看到此用法，例如 `Url.Action("Buy", "Products", new { id = 17 })`。 依照慣例，「路由值」物件通常是匿名型別的物件，但也可以是 `IDictionary<>` 或「純舊 .NET 物件」。 不符合路由參數的任何額外路由值都會放在查詢字串中。

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> 若要建立絕對 URL，請使用接受 `protocol` 的多載：`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>由路由產生 URL

上述程式碼示範如何藉由傳入控制器和動作名稱來產生 URL。 `IUrlHelper` 也提供 `Url.RouteUrl` 系列方法。 這些方法類似於 `Url.Action`，但不會將 `action` 和 `controller` 的目前值複製到路由值。 最常見的用法是指定路由名稱使用特定路由來產生 URL，通常「不需要」指定控制器或動作名稱。

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>在 HTML 中產生 URL

`IHtmlHelper` 提供 `HtmlHelper` 方法 `Html.BeginForm` 和 `Html.ActionLink`，以分別產生 `<form>` 和 `<a>` 項目。 這些方法使用 `Url.Action` 方法來產生 URL，並接受類似的引數。 `HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。

TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。 這兩者使用 `IUrlHelper` 進行實作。 如需詳細資訊，請參閱[使用表單](../views/working-with-forms.md)。

在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>在動作結果中產生 URL

上述範例示範如何在控制器中使用 `IUrlHelper`，但控制器中最常見的用法是產生 URL 以作為動作結果的一部分。

`ControllerBase` 和 `Controller` 基底類別提供便利的方法讓動作結果可參考其他動作。 一個典型的用法是在接受使用者輸入之後重新導向。

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

動作結果的 Factory 方法遵循類似於 `IUrlHelper` 上方法的模式。

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>專用慣例路由的特殊案例

慣例路由可使用一種特殊路由定義，稱為「專用慣例路由」。 在下列範例中，名為 `blog` 的路由是專用慣例路由。

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

透過這些路由定義，`Url.Action("Index", "Home")` 會使用 `default` 路由產生 URL 路徑 `/`，但為什麼？ 您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。

專用慣例路由依賴沒有對應路由參數之預設值的特殊行為，以防止用於 URL 產生的路由變得「太窮盡」。 在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。 當路由執行 URL 產生時，所提供的值必須符合預設值。 使用 `blog` 產生 URL 會失敗，因為值 `{ controller = Home, action = Index }` 不符合 `{ controller = Blog, action = Article }`。 路由會接著切換並嘗試 `default`，此時會成功。

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>區域

[區域](areas.md)是 MVC 功能，可將相關功能組織成群組，作為個別路由命名空間 (適用於控制器動作) 和資料夾結構 (適用於檢視)。 使用區域可讓應用程式具有多個同名的控制器 (只要這些控制器具有不同的「區域」即可)。 使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。 本節將討論路由如何與區域互動；如需區域如何與檢視搭配使用的詳細資料，請參閱[區域](areas.md)。

下列範例會設定 MVC 使用預設慣例路由，並為名為 `Blog` 的區域設定「區域路由」：

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

當符合 `/Manage/Users/AddUser` 等 URL 路徑時，第一個路由會產生路由值 `{ area = Blog, controller = Users, action = AddUser }`。 `area` 路由值是由 `area` 的預設值所產生；事實上，`MapAreaRoute` 所建立的路由相當於：

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。 預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。

> [!TIP]
> 慣例路由與順序息息相關。 一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。

使用上述範例，路由值會符合下列動作：

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute` 可用來指定控制器為區域的一部分，假設此控制器在 `Blog` 區域中。 不含 `[Area]` 屬性的控制器不是任何區域的成員，因此當路由提供 `area` 路由值時**不會**符合。 在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> 為求完整起見，此處顯示每個控制器的命名空間，否則控制器會有命名衝突並產生編譯器錯誤。 類別命名空間不會影響 MVC 的路由。

前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。 第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。

> [!NOTE]
> 在「未符合任何值」的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。

執行區域中的動作時，`area` 的路由值會作為路由用於 URL 產生的「環境值」。 這表示區域預設會以「黏性」方式來處理 URL 產生，如下列範例所示。

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>了解 IActionConstraint

> [!NOTE]
> 本節深入探討架構內部及 MVC 如何選擇要執行的動作。 一般應用程式不需要自訂 `IActionConstraint`

即使您不熟悉 `IActionConstraint`，也可能已使用過此介面。 `[HttpGet]` 屬性和類似的 `[Http-VERB]` 屬性會實作 `IActionConstraint` 來限制動作方法的執行。

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

假設在預設慣例路由中，URL 路徑 `/Products/Edit` 會產生值 `{ controller = Products, action = Edit }`，該值符合此處所示的**兩個**動作。 在 `IActionConstraint` 術語中，我們會假設這兩個動作可視為候選項目 (因為它們都符合路由資料)。

當 `HttpGetAttribute` 執行時，*Edit()* 會符合 *GET*，但不符合任何其他 HTTP 動詞命令。 `Edit(...)` 動作未定義任何條件約束，因此會符合任何 HTTP 動詞命令。 假設 `POST` - 只有 `Edit(...)` 相符。 但對於 `GET`，這兩個動作仍然相符；不過，具有 `IActionConstraint` 的動作一律比沒有的動作「更符合」。 由於 `Edit()` 具有 `[HttpGet]`，因此更明確；如果兩個動作都相符，就會選取此動作。

就概念而言，`IActionConstraint` 是一種「多載」形式，但它會在符合相同 URL　的動作之間進行多載，而不是多載具有相同名稱的方法。 屬性路由也會使用 `IActionConstraint`，因此可能導致來自不同控制器的動作被視為候選項目。

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>實作 IActionConstraint

實作 `IActionConstraint` 的最簡單方式是建立衍生自 `System.Attribute` 的類別，並將它放在您的動作和控制器上。 MVC 會自動探索作為屬性套用的任何 `IActionConstraint`。 您可以使用應用程式模型來套用條件約束，由於您可以之後編寫其套用方式的程式，因此可能是最彈性的方法。

在下列範例中，條件約束會根據路由資料中的「國碼 (地區碼)」來選擇動作。 [GitHub 上有完整範例](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

您必須負責實作 `Accept` 方法，並選擇 'Order' 作為要執行的條件約束。 在本例中，`Accept` 方法會傳回 `true`，表示當 `country` 路由值相符時動作會符合。 這與 `RouteValueAttribute` 不同，後者允許切換回未使用屬性的動作。 此範例顯示如果您定義 `en-US` 動作，則 `fr-FR` 等國碼 (地區碼) 會切換回未套用 `[CountrySpecific(...)]` 的更泛型控制器。

`Order` 屬性決定條件約束所屬的「階段」。 群組中的動作條件約束會根據 `Order` 來執行。 例如，架構提供的所有 HTTP 方法屬性都會使用相同的 `Order` 值，以便在同一個階段中執行。 您可以視需要擁有許多階段來實作所需的原則。

> [!TIP]
> 若要決定 `Order` 的值，請考慮是否應該在 HTTP 方法之前套用條件約束。 數值愈低會愈先執行。
