---
title: "路由至控制器的動作"
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: 497ce47fa567f163cb7b1eb891408f0100d15b8a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="routing-to-controller-actions"></a>路由至控制器的動作

由[Ryan Nowak](https://github.com/rynowak)和[Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC 會使用路由[中介軟體](../../fundamentals/middleware.md)符合傳入要求的 Url，並將它們對應的動作。 啟始程式碼或屬性中定義的路由。 路由會說明如何 URL 路徑應比對的動作。 路由也可用來產生 Url (連結） 送出回應中。 

動作可以依照慣例路由傳送或路由屬性。 放置控制器或動作的路由可讓路由的屬性。 請參閱[混合路由](#routing-mixed-ref-label)如需詳細資訊。

本文件將說明 MVC 和路由，及如何在一般 MVC 應用程式請之間的互動使用路由的功能。 請參閱[路由](xref:fundamentals/routing)如進階路由的詳細資訊。

## <a name="setting-up-routing-middleware"></a>設定路由的中介軟體

在您*設定*方法，您可能會看到類似的程式碼：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

若要呼叫的內部`UseMvc`，`MapRoute`用來建立單一的路由，我們將做為參照`default`路由。 大部分的 MVC 應用程式會使用類似於範本中使用的路由`default`路由。

路由範本`"{controller=Home}/{action=Index}/{id?}"`可以比對 URL 路徑，如`/Products/Details/5`並會解壓縮路由值`{ controller = Products, action = Details, id = 5 }`的 token 化的路徑。 MVC 會嘗試找出的控制站`ProductsController`並執行動作`Details`:

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

請注意，在此範例中，模型繫結會使用的值`id = 5`設定`id`參數`5`時叫用這個動作。 請參閱[模型繫結](../models/model-binding.md)如需詳細資訊。

使用`default`路由：

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

路由的路由範本：

* `{controller=Home}`定義`Home`作為預設值`controller`

* `{action=Index}`定義`Index`作為預設值`action`

* `{id?}`定義`id`為選擇性

預設和選擇性的路由參數不需要出現在相符的 URL 路徑。 請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)的路由範本語法的詳細描述。

`"{controller=Home}/{action=Index}/{id?}"`可以比對的 URL 路徑`/`而且將會產生路由值`{ controller = Home, action = Index }`。 值`controller`和`action`進行的預設值，使用`id`沒有產生值，因為 URL 路徑中沒有任何對應的區段。 MVC 會使用這些路由值進行選取`HomeController`和`Index`動作：

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

使用此控制站定義和路由範本`HomeController.Index`動作就會執行任何下列的 URL 路徑：

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

便利的方法， `UseMvcWithDefaultRoute`:

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

`UseMvc`和`UseMvcWithDefaultRoute`加入執行個體`RouterMiddleware`到中介軟體管線。 MVC 不直接互動中介軟體，並使用路由處理要求。 透過執行個體的路由連線 MVC `MvcRouteHandler`。 在程式碼`UseMvc`與下列類似：

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

`UseMvc`不會直接定義的任何路由，它將預留位置加入至路由集合`attribute`路由。 多載`UseMvc(Action<IRouteBuilder>)`可讓您新增您自己的路由，也支援屬性路由。  `UseMvc`和所有其變化新增預留位置，代表屬性路由-屬性路由是一律可以使用，不論您如何設定`UseMvc`。 `UseMvcWithDefaultRoute`定義預設路由，並支援屬性路由。 [屬性路由](#attribute-routing-ref-label)章節包含有關屬性路由的更多詳細資料。

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a>傳統的路由

`default`路由：

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

舉例*傳統路由*。 我們會呼叫這個樣式*傳統路由*因為它會建立*慣例*的 URL 路徑：

* 第一個路徑區段對應至控制器名稱

* 第二個對應至動作名稱。

* 第三個區段適用於選擇性`id`可用來對應至模型實體

使用此`default`路由、 URL path`/Products/List`對應至`ProductsController.List`動作，以及`/Blog/Article/17`對應至`BlogController.Article`。 此對應根據控制器和動作名稱**只**並不根據命名空間、 原始程式檔位置或方法參數。

> [!TIP]
> 使用預設路由與傳統路由可讓您快速建置應用程式，而不需對您定義每個動作的新 URL 模式。 執行 CRUD 動作，樣式的應用程式，具有一致性 url，透過您控制站有助於簡化您的程式碼，並讓 UI 更容易預測。

> [!WARNING]
> `id`定義為選擇性的路由範本，這表示您的動作可以執行而不做為 URL 的一部分提供的識別碼。 通常會發生什麼事如果`id`省略 URL 中是它會設為`0`模型繫結程序，因此沒有實體位於資料庫的比對`id == 0`。 路由屬性可讓您更細微的控制，以便對於某些動作，並不供其他人所需的識別碼。 依照慣例將包含文件等選擇性參數`id`當它們可能會出現在正確的使用方式。

## <a name="multiple-routes"></a>多個路由

您可以加入多個路由內`UseMvc`藉由新增多個呼叫`MapRoute`。 這樣做可讓您定義多個慣例，或將傳統的路由，則會專用於特定的動作，例如：

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

`blog`這裡路由是*專用的傳統路由*，這表示它會使用傳統的路由系統，但是專用於特定的動作。 因為`controller`和`action`並未出現在路由範本中，做為參數，它們只可以有預設值，因此此路由會一律對應到動作`BlogController.Article`。

路由的路由集合中已排序，並將它們加入的順序處理。 在此範例中，因此`blog`路由將會嘗試先`default`路由。

> [!NOTE]
> *專用傳統路由*通常會使用所有路由的參數，如`{*article}`擷取 URL 路徑的其餘部分。 這可讓路由 '太窮盡' 表示其符合您預定要比對其他路由的 Url。 將 '窮盡' 路由放稍後在路由表，來解決這個問題。

### <a name="fallback"></a>後援

要求處理的一部分，MVC 會確認路由值，可用來尋找應用程式中的控制器和動作。 如果路由值的動作不相符路由則不被視為相符項目，並會嘗試下一個路由。 這稱為*後援*，而且它具有能簡化傳統路由個重疊的情況。

### <a name="disambiguating-actions"></a>釐清動作

當兩個動作符合透過路由時，MVC 必須區分來選擇最佳' 的候選項目，否則會擲回例外狀況。 例如: 

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

此控制器會定義將會比對的 URL 路徑的兩個動作`/Products/Edit/17`和路由資料`{ controller = Products, action = Edit, id = 17 }`。 這是 MVC 控制器的典型模式其中`Edit(int)`顯示表單，以編輯產品，以及`Edit(int, Product)`處理已張貼的表單。 若要進行這項作業 MVC 就必須選擇`Edit(int, Product)`當要求是 HTTP`POST`和`Edit(int)`時的 HTTP 動詞命令是任何其他項目。

`HttpPostAttribute` ( `[HttpPost]` ) 是實作`IActionConstraint`，只會允許的 HTTP 動詞命令時，必須選取動作`POST`。 與否`IActionConstraint`讓`Edit(int, Product)`'好' 符合比`Edit(int)`，因此`Edit(int, Product)`會先嘗試。

您只需要撰寫自訂`IActionConstraint`實作中特定的案例，但它的一定要了解角色的屬性，例如`HttpPostAttribute`-相似的屬性定義的其他 HTTP 動詞命令。 在傳統路由是普遍的動作，即可使用相同的動作名稱，在他們的一部分`show form -> submit form`工作流程。 此模式的方便性，不論會變得更加明顯檢閱之後[了解 IActionConstraint](#understanding-iactionconstraint) > 一節。

如果多個路由相符，而且 MVC 找不到 '最佳' 的路由，則會擲回`AmbiguousActionException`。

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a>路由名稱

字串`"blog"`和`"default"`在下列範例中是路由名稱：


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

路由名稱給與路由邏輯的名稱，讓具名的路由可用於 URL 的產生。 這可以大幅簡化 URL 建立時的路由順序可能會使複雜 URL 的產生。 路由名稱必須是唯一的應用程式層級。

路由名稱不會有影響 url 比對或處理的要求。它們是僅適用於 URL 的產生。 [路由](xref:fundamentals/routing)有更詳細資訊包括在 MVC 特定協助程式 URL 的產生 URL 的產生。

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a>路由屬性

路由屬性，可用於屬性的一組動作會直接對應到路由範本。 在下列範例中，`app.UseMvc();`用於`Configure`傳遞方法，並沒有路由。 `HomeController`會比對一組類似於預設路由的 Url 的`{controller=Home}/{action=Index}/{id?}`會比對：

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

`HomeController.Index()`動作將會執行任何的 URL 路徑`/`， `/Home`，或`/Home/Index`。

> [!NOTE]
> 此範例中會反白顯示屬性路由和傳統路由的主要程式設計差異。 屬性路由需要更多的輸入，以指定的路由。傳統的預設路由更簡潔的方式處理路由。 不過，屬性路由可讓，並需要精確地控制其中的路由範本套用至每個動作。

路由的控制器名稱和動作名稱的屬性與播放**沒有**在其中選取動作的角色。 這個範例會比對上一個範例相同的 Url。

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
> 上述的路由範本未定義的路由參數`action`， `area`，和`controller`。 事實上，這些路由參數不允許在屬性路由。 既然已與動作相關聯的路由範本，它對於意義剖析從 URL 的動作名稱。

## <a name="attribute-routing-with-httpverb-attributes"></a>屬性 Http[動詞] 屬性與路由

路由屬性也可使用`Http[Verb]`等屬性`HttpPostAttribute`。 所有這些屬性可以接受的路由範本。 此範例會顯示符合相同的路由範本的兩個動作：

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

類似的 URL 路徑`/products` `ProductsApi.ListProducts` HTTP 動詞命令時，將會執行動作`GET`和`ProductsApi.CreateProduct`HTTP 動詞命令時，將會執行`POST`。 屬性路由會先根據路由屬性所定義的路由範本組 URL 相符。 一旦與相符的路由範本，`IActionConstraint`條件約束會套用至可執行的動作。

> [!TIP]
> 當建置 REST API，很少會想要使用`[Route(...)]`動作方法上。 最好是使用多個特定`Http*Verb*Attributes`要精確地指定您的 API 的支援。 REST Api 的用戶端應該知道什麼路徑和 HTTP 動詞命令將對應至特定的邏輯作業。

因為屬性路由適用於特定的動作，所以可以輕鬆地進行所需的路由範本定義一部分的參數。 在此範例中，`id`是 URL 路徑中的必要部分。

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

`ProductsApi.GetProduct(int)`動作將會執行類似的 URL 路徑`/products/3`但不適用於 URL 路徑，如`/products`。 請參閱[路由](../../fundamentals/routing.md)路由樣板與相關的選項的完整說明。

## <a name="route-name"></a>路由名稱

下列程式碼定義*路由名稱*的`Products_List`:

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

路由名稱可以用來產生特定的路由為基礎的 URL。 路由名稱的 URL 相符的路由行為造成任何影響，而且僅適用於 URL 的產生。 路由名稱必須是唯一的應用程式層級。

> [!NOTE]
> 這和傳統*預設路由*，而後者可定義`id`參數為選擇性 (`{id?}`)。 這項功能精確地指定應用程式開發介面有優點，例如允許`/products`和`/products/5`分派至不同的動作。

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a>結合路由

若要讓屬性路由較不重複，路由在控制器上的屬性會結合路由屬性上的個別動作。 在控制器上定義的任何路由範本前面加上了動作的路由範本。 在控制器上放置路由屬性可讓**所有**控制器中的動作會使用屬性路由。

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

在此範例中的 URL 路徑`/products`可以比對`ProductsApi.ListProducts`，和 URL 路徑`/products/5`可以比對`ProductsApi.GetProduct(int)`。 這兩種動作只會比對 HTTP`GET`因為它們以裝飾`HttpGetAttribute`。

路由範本套用至動作開頭`/`不取得結合套用至控制器的路由範本。 這個範例會比對一組類似的 URL 路徑*預設路由*。

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

相較於傳統路由定義的順序執行，屬性路由建置樹狀結構，並同時符合所有路由。 這個行為會如同-如果路由項目放入的理想排序;最適合的路由有機會執行之前的較通用的路由。

比方說，路由喜歡`blog/search/{topic}`更為具體的路由，像是`blog/{*article}`。 邏輯上來說`blog/search/{topic}`路由 '執行' 首先，根據預設，因為這是只有合理的排序。 使用傳統的路由，開發人員會負責依預期順序放置路由。

路由屬性可以設定的順序，使用`Order`提供架構路由屬性的所有屬性。 路由的處理方式遞增排序`Order`屬性。 預設順序是`0`。 設定路由使用`Order = -1`之前不會設定訂單的路由會執行。 設定路由使用`Order = 1`之後預設路由順序會執行。

> [!TIP]
> 避免取決於`Order`。 如果您的 URL 空間需要明確的順序值正確路由，則用戶端也可能會造成混淆。 在 一般屬性路由選取正確的路由與 URL 相符。 如果不使用用來產生 URL 的預設順序，使用的路由名稱通常比套用覆寫現狀`Order`屬性。

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a>語彙基元取代路由範本中的 ([控制器]，[動作]，[區域])

為了方便起見，屬性路由支援*語彙基元取代*所封入語彙基元，在方括號中 (`[`， `]`)。 語彙基元`[action]`， `[area]`，和`[controller]`會被取代的動作名稱、 區域名稱和控制器名稱，從路由定義所在的動作值。 在此範例中的動作可以比對 URL 路徑註解中所述：

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

取代 token，就會發生的建置屬性路由的最後一個步驟。 上述範例中的行為相同，如以下程式碼：

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

也可以使用繼承結合屬性路由。 這是特別強大結合 token 取代。

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

語彙基元替換也適用於屬性路由所定義的路由名稱。 `[Route("[controller]/[action]", Name="[controller]_[action]")]`會產生唯一的路由名稱，每個動作。

要比對常值語彙基元取代分隔符號`[`或`]`，它藉由重複的字元逸出 (`[[`或`]]`)。

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a>多個路由

此屬性定義的多個連線到相同的動作的路由的路由支援。 若要模擬的行為是最常見的使用*傳統的預設路由*如下列範例所示：

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

將多個路由屬性放置在控制器上，表示每個會合併與每個路由屬性，在動作方法。

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

當多個路由屬性 (可實作`IActionConstraint`) 放置動作，則每個動作的條件約束結合從定義該屬性的路由範本。

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
> 雖然使用多個路由動作可以看起來是功能強大，最好是保留您的應用程式 URL 空間，簡單且妥善定義。 只有在需要時，例如若要支援現有的用戶端時，才使用多個路由動作。

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a>指定屬性路由選擇性參數、 預設值和條件約束

屬性路由支援做為傳統的路由，指定選擇性參數、 預設值和條件約束相同的內嵌語法。

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)的路由範本語法的詳細描述。

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a>使用的自訂路由屬性`IRouteTemplateProvider`

所有 framework 所提供的路由屬性 ( `[Route(...)]`，`[HttpGet(...)]`等等) 實作`IRouteTemplateProvider`介面。 MVC 控制器類別和動作方法上的屬性時應用程式啟動，且會使用可實作看起來`IRouteTemplateProvider`建置一組初始的路由。

您可以實作`IRouteTemplateProvider`來定義您自己的路由屬性。 每個`IRouteTemplateProvider`可讓您定義的單一路由，以自訂路由範本中，排序和名稱：

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

上述範例中的屬性會自動設定`Template`至`"api/[controller]"`時`[MyApiController]`套用。

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a>使用自訂屬性路由的應用程式模型

*應用程式模型*是建立在啟動使用 MVC 路由，並執行動作的中繼資料的所有物件模型。 *應用程式模型*包含所有路由屬性從收集的資料 (透過`IRouteTemplateProvider`)。 您可以撰寫*慣例*修改應用程式模型，在啟動時，以自訂路由的運作方式。 本節說明自訂路由使用應用程式模型的簡單範例。

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a>混合的路由： 路由與傳統路由的屬性

MVC 應用程式可以混合使用傳統的路由和屬性路由。 這是通常使用傳統的路由提供 HTML 網頁瀏覽器，服務控制站，以及屬性控制站服務 REST Api 的路由。

動作可以依照慣例路由傳送或路由屬性。 放置控制器或動作的路由可讓路由的屬性。 定義屬性路由的動作無法透過傳統的路由，反之亦然。 **任何**路由在控制器上的屬性可讓路由在控制器屬性中的所有動作。

> [!NOTE]
> 路由系統兩種類型的差異是 URL 符合的路由範本之後，套用的程序。 在傳統的路由，從符合的路由值可用來從查閱資料表的所有傳統的路由動作選擇動作與控制器。 路由屬性，每個範本已經與動作相關聯，不必採取任何進一步的查閱。

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a>URL 的產生

MVC 應用程式可以使用路由的 URL 產生功能，產生動作的 URL 連結。 產生 Url 排除硬式編碼的 Url，讓程式碼更穩定、 更容易維護。 本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 的產生方式的基本概念。 請參閱[路由](../../fundamentals/routing.md)為 URL 的產生的詳細描述。

`IUrlHelper`介面是之間 MVC 和 URL 的產生的路由基礎結構的基礎部分。 您可以找到的執行個體`IUrlHelper`可透過`Url`中控制站、 檢視和檢視元件屬性。

在此範例中，`IUrlHelper`透過使用介面`Controller.Url`產生另一個動作的 URL 屬性。

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

如果應用程式使用傳統的預設路由，值`url`變數將會是 URL 路徑字串`/UrlGeneration/Destination`。 此 URL 路徑，會使用傳遞給的值建立的路由，方法是結合從目前的要求 （環境值）、 路由值`Url.Action`並替代為這些值到路由範本：

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

每個路由中的路由範本有參數值和環境的值的比對名稱來取代其值。 路由參數沒有值，可以使用預設值，如果有一個，或如果它是選擇性會略過 (如果是做為`id`在此範例中)。 URL 的產生任何必要的路由參數沒有對應的值將會失敗。 如果路由 URL 的產生失敗，已嘗試所有的路由，或找到相符項目之前，會嘗試下一個路由。

此範例的`Url.Action`上方假設傳統的路由，但是 URL 產生適用於類似屬性路由，雖然這些概念都不同。 與傳統的路由，路由值可用來依序展開 範本時和路由值`controller`和`action`通常會出現在該範本-因為相符路由的 Url 會遵循*慣例*. 在屬性路由，路由值`controller`和`action`不允許出現在 範本-改為用來查閱要使用哪一個範本。

這個範例會使用屬性的路由：

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

MVC 建置查閱資料表的所有屬性路由動作，並將符合`controller`和`action`值，以選取要用於產生 URL 的路由範本。 在上述範例`custom/url/to/destination`產生。

### <a name="generating-urls-by-action-name"></a>產生 Url 的動作名稱

`Url.Action` (`IUrlHelper` . `Action`) 和所有相關的所有以您想要指定哪些您連結到所指定的控制器名稱和動作名稱的概念為基礎的多載。

> [!NOTE]
> 使用時`Url.Action`，目前的路由值`controller`和`action`會為您指定的值`controller`和`action`同時屬於*環境值***和***值*。 此方法`Url.Action`，一律會使用目前的值`action`和`controller`，且會產生將路由傳送至目前的動作是 URL 路徑。

路由嘗試使用中環境值的值來填入您在產生 URL 時未提供的資訊。 使用類似的路由`{a}/{b}/{c}/{d}`和環境值`{ a = Alice, b = Bob, c = Carol, d = David }`、 路由有足夠的資訊產生不含任何其他值的 URL-因為所有路由參數值。 如果您加入值`{ d = Donovan }`，值`{ d = David }`會遭到忽略，而且產生的 URL 路徑會是`Alice/Bob/Carol/Donovan`。

> [!WARNING]
> URL 路徑是階層式。 在此範例中，如果您已新增值`{ c = Cheryl }`，兩個值`{ c = Carol, d = David }`會遭到忽略。 在此情況下我們不再需要的值`d`和 URL 的產生將會失敗。 您必須指定所需的值為`c`和`d`。  您可能預期叫用此問題的預設路由 (`{controller}/{action}/{id?}`)-但很少會發生這種作法，因為行為`Url.Action`會永遠明確指定`controller`和`action`值。

較長的多載`Url.Action`也則額外接受一個*路由值*以提供路由參數值以外的物件`controller`和`action`。 通常您會看到這搭配`id`像`Url.Action("Buy", "Products", new { id = 17 })`。 依照慣例*路由值*物件通常是匿名類型的物件，但它也可以是`IDictionary<>`或*純舊的.NET 物件*。 不相符的路由參數的任何其他的路由值會放在查詢字串。

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> 若要建立的絕對 URL，使用多載，接受`protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a>產生 Url 的路由

上述程式碼會示範產生傳入控制器和動作名稱的 URL。 `IUrlHelper`也提供`Url.RouteUrl`系列的方法。 這些方法都類似於`Url.Action`，但它們不會複製目前的值`action`和`controller`為路由的值。 最常見的用法是，指定要產生的 URL，通常使用特定的路由的路由名稱*沒有*指定控制器或動作名稱。

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a>以 HTML 產生 Url

`IHtmlHelper`提供`HtmlHelper`方法`Html.BeginForm`和`Html.ActionLink`產生`<form>`和`<a>`元素分別。 這些方法會使用`Url.Action`方法來產生 URL 以及接受類似的引數。 `Url.RouteUrl`馬其頓騎兵如`HtmlHelper`是`Html.BeginRouteForm`和`Html.RouteLink`其具有類似的功能。

TagHelpers 產生透過 Url `form` TagHelper 和`<a>`TagHelper。 這兩種使用`IUrlHelper`其實作。 請參閱[使用表單](../views/working-with-forms.md)如需詳細資訊。

在檢視內`IUrlHelper`可透過`Url`未涵蓋的上述任何特定 URL 的產生的屬性。

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a>在動作結果中產生 URL

上述範例顯示使用`IUrlHelper`中控制站，而在控制器中最常見的用法是做為一部分的動作結果產生的 URL。

`ControllerBase`和`Controller`基底類別會提供便利的方法參考另一個動作的動作結果。 一個典型的使用方式是接受使用者輸入之後重新導向。

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

動作結果的 factory 方法的方法遵循類似的模式上`IUrlHelper`。

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a>特殊的專用的傳統路由案例

傳統的路由，可以使用一種特殊的路由定義稱為*專用的傳統路由*。 在下列範例中，路由命名為`blog`是專用的傳統路由。

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

使用這些路由定義，`Url.Action("Index", "Home")`將產生的 URL 路徑`/`與`default`路由，但為什麼？ 您猜的路由值`{ controller = Home, action = Index }`足以產生的 URL 使用`blog`，而結果會是`/blog?action=Index&controller=Home`。

專用的傳統路由依賴為特殊的行為，沒有對應的路由參數可防止路由的預設值是 「 太窮盡 」 與 URL 的產生。 預設值是在此情況下`{ controller = Blog, action = Article }`，並沒有`controller`也`action`會顯示為路由參數。 當執行路由 URL 的產生時，所提供的值必須符合的預設值。 URL 產生使用`blog`會失敗，因為值`{ controller = Home, action = Index }`不符`{ controller = Blog, action = Article }`。 路由然後再試一次回落`default`，這會成功。

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a>區域

[區域](areas.md)是 MVC 功能，用來組織到群組的相關的功能，做為個別路由-命名空間 （適用於控制器動作） 以及 （適用於檢視） 的資料夾結構。 使用區域，可讓應用程式有多個具有相同名稱的控制器，只要它們具有不同*區域*。 使用區域建立階層，以新增另一個路由參數，路由`area`至`controller`和`action`。 本節將討論如何路由互動區域-請參閱[區域](areas.md)詳細資料區域與檢視的使用方式。

下列範例會設定要使用預設的傳統路由 MVC 和*區域路由*名為某區域`Blog`:

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

比對類似的 URL 路徑時`/Manage/Users/AddUser`，第一個路由會產生路由值`{ area = Blog, controller = Users, action = AddUser }`。 `area`路由值所產生的預設值`area`，事實上所建立的路由`MapAreaRoute`相當於下列：

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

`MapAreaRoute`建立路由，使用預設值和條件約束`area`使用提供的區域名稱，在此情況下`Blog`。 預設值可確保路由，一律會產生`{ area = Blog, ... }`，條件約束需要值`{ area = Blog, ... }`為 URL 的產生。

> [!TIP]
> 傳統的路由是順序而定。 一般情況下，路由與區域應放稍早在路由表中為它們比沒有區域的路由更明確。

使用上述範例中，路由值會比對下列動作：

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

`AreaAttribute`是什麼代表控制器區域的一部分，我們稱此控制器處於`Blog`區域。 控制站不含`[Area]`屬性不是成員的任何區域中，並將**不**相符時`area`路由值係由路由。 在下列範例中，只有列出的第一個控制器會比對路由值`{ area = Blog, controller = Users, action = AddUser }`。

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> 每個控制站的命名空間如下所示為求完整起見，否則控制器會有命名衝突，並產生編譯器錯誤。 類別的命名空間有不會影響 MVC 的路由。

前兩個控制站的區域成員，只有項目時符合其各自的區域名稱係由`area`路由值。 第三個控制站不是任何區域中，並可以唯一相符項目時沒有值的成員`area`係由路由。

> [!NOTE]
> 根據比對*沒有值*，缺少`area`值都相同一樣的值`area`null 或空字串。

當執行區域內的動作時，路由值`area`將可提供作為*環境值*路由用於 URL 的產生。 這表示，依預設區域做*自黏*URL 產生如下列範例所示。

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a>了解 IActionConstraint

> [!NOTE]
> 此區段可深入了解架構的內部狀態和 MVC 如何選擇要執行的動作。 一般應用程式不需要自訂`IActionConstraint`

您可能已用過`IActionConstraint`即使您不熟悉的介面。 `[HttpGet]`屬性和類似`[Http-VERB]`屬性實作`IActionConstraint`以限制動作方法執行。

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

假設傳統，預設路由的 URL 路徑`/Products/Edit`值將會產生`{ controller = Products, action = Edit }`，其會符合**兩者**如下所示的動作。 在`IActionConstraint`術語，我們會假設這兩種動作被視為候選項目-因為它們都符合的路由資料。

當`HttpGetAttribute`執行時，它會顯示， *Edit()*的相符項目*取得*，而且不用於任何其他 HTTP 動詞命令相符項目。 `Edit(...)`動作沒有任何條件約束定義，並因此將會比對任何 HTTP 指令動詞。 因此假設`POST`-只`Edit(...)`比對。 但對於`GET`這兩個動作仍可以符合-但是，動作與`IActionConstraint`一律視為*更佳*動作，而不比。 因此因為`Edit()`具有`[HttpGet]`它會被視為更明確，而且如果這兩個動作可以比對會選取。

就概念而言，`IActionConstraint`是一種*多載*，但而不多載具有相同名稱的方法，它多載之間的相同的 URL 相符的動作。 路由屬性也會使用`IActionConstraint`並可能導致從不同的控制器動作這兩個正在考慮的候選項目。

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a>實作 IActionConstraint

最簡單的方式實作`IActionConstraint`是建立類別，衍生自`System.Attribute`並將它放在您的動作和控制站。 MVC 會自動探索任何`IActionConstraint`，套用做為屬性。 您可以使用應用程式模型來套用條件約束，，這可能是因為它可讓您套用方式 metaprogram 最彈性的方法。

在下列範例中的條件約束會選擇根據動作*國碼*從路由資料。 [完整範例 GitHub 上](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。

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

您必須負責實作`Accept`方法，並選擇 'Order' 條件約束來執行。 在此情況下，`Accept`方法會傳回`true`代表的動作是相符項目時`country`路由值相符項目。 這點不同於`RouteValueAttribute`在於，前者允許後援至未使用屬性的動作。 此範例會示範，是否您定義`en-US`動作然後類似的國家/地區碼`fr-FR`會切換回更泛型的控制站沒有`[CountrySpecific(...)]`套用。

`Order`屬性決定其*階段*條件約束是的一部分。 根據的群組中執行的動作條件約束`Order`。 例如，所有的 framework 提供 HTTP 方法的屬性使用相同`Order`值，使其在同一個階段中執行。 您可以有許多階段視需要實作所需的原則。

> [!TIP]
> 若要決定的值`Order`考慮是否應該套用您的條件約束之前 HTTP 方法。 先執行較低的數字。
