---
title: "路由至控制器的動作"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: 5a0b5399f7441035cb1231a009681ca22b07ab4e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="604f8-103">路由至控制器的動作</span><span class="sxs-lookup"><span data-stu-id="604f8-103">Routing to Controller Actions</span></span>

<span data-ttu-id="604f8-104">由[Ryan Nowak](https://github.com/rynowak)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="604f8-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="604f8-105">ASP.NET Core MVC 會使用路由[中介軟體](../../fundamentals/middleware.md)符合傳入要求的 Url，並將它們對應的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-105">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="604f8-106">啟始程式碼或屬性中定義的路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="604f8-107">路由會說明如何 URL 路徑應比對的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="604f8-108">路由也可用來產生 Url (連結） 送出回應中。</span><span class="sxs-lookup"><span data-stu-id="604f8-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="604f8-109">動作可以依照慣例路由傳送或路由屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="604f8-110">放置控制器或動作的路由可讓路由的屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="604f8-111">請參閱[混合路由](#routing-mixed-ref-label)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="604f8-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="604f8-112">本文件將說明 MVC 和路由，及如何在一般 MVC 應用程式請之間的互動使用路由的功能。</span><span class="sxs-lookup"><span data-stu-id="604f8-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="604f8-113">請參閱[路由](xref:fundamentals/routing)如進階路由的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="604f8-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="604f8-114">設定路由的中介軟體</span><span class="sxs-lookup"><span data-stu-id="604f8-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="604f8-115">在您*設定*方法，您可能會看到類似的程式碼：</span><span class="sxs-lookup"><span data-stu-id="604f8-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="604f8-116">若要呼叫的內部`UseMvc`，`MapRoute`用來建立單一的路由，我們將做為參照`default`路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="604f8-117">大部分的 MVC 應用程式會使用類似於範本中使用的路由`default`路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="604f8-118">路由範本`"{controller=Home}/{action=Index}/{id?}"`可以比對 URL 路徑，如`/Products/Details/5`並會解壓縮路由值`{ controller = Products, action = Details, id = 5 }`的 token 化的路徑。</span><span class="sxs-lookup"><span data-stu-id="604f8-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="604f8-119">MVC 會嘗試找出的控制站`ProductsController`並執行動作`Details`:</span><span class="sxs-lookup"><span data-stu-id="604f8-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="604f8-120">請注意，在此範例中，模型繫結會使用的值`id = 5`設定`id`參數`5`時叫用這個動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="604f8-121">請參閱[模型繫結](../models/model-binding.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="604f8-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="604f8-122">使用`default`路由：</span><span class="sxs-lookup"><span data-stu-id="604f8-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="604f8-123">路由的路由範本：</span><span class="sxs-lookup"><span data-stu-id="604f8-123">The route template:</span></span>

* <span data-ttu-id="604f8-124">`{controller=Home}`定義`Home`作為預設值`controller`</span><span class="sxs-lookup"><span data-stu-id="604f8-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="604f8-125">`{action=Index}`定義`Index`作為預設值`action`</span><span class="sxs-lookup"><span data-stu-id="604f8-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="604f8-126">`{id?}`定義`id`為選擇性</span><span class="sxs-lookup"><span data-stu-id="604f8-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="604f8-127">預設和選擇性的路由參數不需要出現在相符的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="604f8-127">Default and optional route parameters do not need to be present in the URL path for a match.</span></span> <span data-ttu-id="604f8-128">請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)的路由範本語法的詳細描述。</span><span class="sxs-lookup"><span data-stu-id="604f8-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="604f8-129">`"{controller=Home}/{action=Index}/{id?}"`可以比對的 URL 路徑`/`而且將會產生路由值`{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="604f8-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="604f8-130">值`controller`和`action`進行的預設值，使用`id`不會產生一個值，因為 URL 路徑中沒有任何對應的區段。</span><span class="sxs-lookup"><span data-stu-id="604f8-130">The values for `controller` and `action` make use of the default values, `id` does not produce a value since there is no corresponding segment in the URL path.</span></span> <span data-ttu-id="604f8-131">MVC 會使用這些路由值進行選取`HomeController`和`Index`動作：</span><span class="sxs-lookup"><span data-stu-id="604f8-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="604f8-132">使用此控制站定義和路由範本`HomeController.Index`動作就會執行任何下列的 URL 路徑：</span><span class="sxs-lookup"><span data-stu-id="604f8-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="604f8-133">便利的方法， `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="604f8-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="604f8-134">可用來取代：</span><span class="sxs-lookup"><span data-stu-id="604f8-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="604f8-135">`UseMvc`和`UseMvcWithDefaultRoute`加入執行個體`RouterMiddleware`到中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="604f8-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="604f8-136">MVC 不直接互動中介軟體，並使用路由處理要求。</span><span class="sxs-lookup"><span data-stu-id="604f8-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="604f8-137">透過執行個體的路由連線 MVC `MvcRouteHandler`。</span><span class="sxs-lookup"><span data-stu-id="604f8-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="604f8-138">在程式碼`UseMvc`與下列類似：</span><span class="sxs-lookup"><span data-stu-id="604f8-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="604f8-139">`UseMvc`沒有直接定義的任何路由，它將預留位置加入至路由集合`attribute`路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-139">`UseMvc` does not directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="604f8-140">多載`UseMvc(Action<IRouteBuilder>)`可讓您新增您自己的路由，也支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="604f8-141">`UseMvc`和所有其變化新增預留位置，代表屬性路由-屬性路由是一律可以使用，不論您如何設定`UseMvc`。</span><span class="sxs-lookup"><span data-stu-id="604f8-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="604f8-142">`UseMvcWithDefaultRoute`定義預設路由，並支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="604f8-143">[屬性路由](#attribute-routing-ref-label)章節包含有關屬性路由的更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="604f8-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name=routing-conventional-ref-label></a>

## <a name="conventional-routing"></a><span data-ttu-id="604f8-144">傳統的路由</span><span class="sxs-lookup"><span data-stu-id="604f8-144">Conventional routing</span></span>

<span data-ttu-id="604f8-145">`default`路由：</span><span class="sxs-lookup"><span data-stu-id="604f8-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="604f8-146">舉例*傳統路由*。</span><span class="sxs-lookup"><span data-stu-id="604f8-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="604f8-147">我們會呼叫這個樣式*傳統路由*因為它會建立*慣例*的 URL 路徑：</span><span class="sxs-lookup"><span data-stu-id="604f8-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="604f8-148">第一個路徑區段對應至控制器名稱</span><span class="sxs-lookup"><span data-stu-id="604f8-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="604f8-149">第二個對應至動作名稱。</span><span class="sxs-lookup"><span data-stu-id="604f8-149">the second maps to the action name.</span></span>

* <span data-ttu-id="604f8-150">第三個區段適用於選擇性`id`可用來對應至模型實體</span><span class="sxs-lookup"><span data-stu-id="604f8-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="604f8-151">使用此`default`路由、 URL path`/Products/List`對應至`ProductsController.List`動作，以及`/Blog/Article/17`對應至`BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="604f8-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="604f8-152">此對應根據控制器和動作名稱**只**並不基礎命名空間、 原始程式檔位置或方法參數。</span><span class="sxs-lookup"><span data-stu-id="604f8-152">This mapping is based on the controller and action names **only** and is not based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="604f8-153">使用預設路由與傳統路由可讓您快速建置應用程式，而不需對您定義每個動作的新 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="604f8-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="604f8-154">執行 CRUD 動作，樣式的應用程式，具有一致性 url，透過您控制站有助於簡化您的程式碼，並讓 UI 更容易預測。</span><span class="sxs-lookup"><span data-stu-id="604f8-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="604f8-155">`id`定義為選擇性的路由範本，這表示您的動作可以執行而不做為 URL 的一部分提供的識別碼。</span><span class="sxs-lookup"><span data-stu-id="604f8-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="604f8-156">通常會發生什麼事如果`id`省略 URL 中是它會設為`0`模型繫結程序，因此沒有實體位於資料庫的比對`id == 0`。</span><span class="sxs-lookup"><span data-stu-id="604f8-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="604f8-157">路由屬性可讓您更細微的控制，以便對於某些動作，並不供其他人所需的識別碼。</span><span class="sxs-lookup"><span data-stu-id="604f8-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="604f8-158">依照慣例將包含文件等選擇性參數`id`時可能會出現在正確的使用方式。</span><span class="sxs-lookup"><span data-stu-id="604f8-158">By convention the documentation will include optional parameters like `id` when they are likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="604f8-159">多個路由</span><span class="sxs-lookup"><span data-stu-id="604f8-159">Multiple routes</span></span>

<span data-ttu-id="604f8-160">您可以加入多個路由內`UseMvc`藉由新增多個呼叫`MapRoute`。</span><span class="sxs-lookup"><span data-stu-id="604f8-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="604f8-161">這樣做可讓您定義多個慣例，或將傳統的路由，則會專用於特定的動作，例如：</span><span class="sxs-lookup"><span data-stu-id="604f8-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="604f8-162">`blog`這裡路由是*專用的傳統路由*，這表示它會使用傳統的路由系統，但是專用於特定的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="604f8-163">因為`controller`和`action`並未出現在路由範本中，做為參數，它們只可以有預設值，因此此路由會一律對應到動作`BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="604f8-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="604f8-164">路由的路由集合中已排序，並將它們加入的順序處理。</span><span class="sxs-lookup"><span data-stu-id="604f8-164">Routes in the route collection are ordered, and will be processed in the order they are added.</span></span> <span data-ttu-id="604f8-165">在此範例中，因此`blog`路由將會嘗試先`default`路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-166">*專用傳統路由*通常會使用所有路由的參數，如`{*article}`擷取 URL 路徑的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="604f8-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="604f8-167">這可讓路由 '太窮盡' 表示其符合您預定要比對其他路由的 Url。</span><span class="sxs-lookup"><span data-stu-id="604f8-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="604f8-168">將 '窮盡' 路由放稍後在路由表，來解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="604f8-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="604f8-169">後援</span><span class="sxs-lookup"><span data-stu-id="604f8-169">Fallback</span></span>

<span data-ttu-id="604f8-170">要求處理的一部分，MVC 會確認路由值，可用來尋找應用程式中的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="604f8-171">如果路由值不符合動作然後路由不會視為相符，並會嘗試下一個路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-171">If the route values don't match an action then the route is not considered a match, and the next route will be tried.</span></span> <span data-ttu-id="604f8-172">這稱為*後援*，而且它具有能簡化傳統路由個重疊的情況。</span><span class="sxs-lookup"><span data-stu-id="604f8-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="604f8-173">釐清動作</span><span class="sxs-lookup"><span data-stu-id="604f8-173">Disambiguating actions</span></span>

<span data-ttu-id="604f8-174">當兩個動作符合透過路由時，MVC 必須區分來選擇最佳' 的候選項目，否則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="604f8-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="604f8-175">例如: </span><span class="sxs-lookup"><span data-stu-id="604f8-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="604f8-176">此控制器會定義將會比對的 URL 路徑的兩個動作`/Products/Edit/17`和路由資料`{ controller = Products, action = Edit, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="604f8-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="604f8-177">這是 MVC 控制器的典型模式其中`Edit(int)`顯示表單，以編輯產品，以及`Edit(int, Product)`處理已張貼的表單。</span><span class="sxs-lookup"><span data-stu-id="604f8-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="604f8-178">若要進行這項作業 MVC 就必須選擇`Edit(int, Product)`當要求是 HTTP`POST`和`Edit(int)`時的 HTTP 動詞命令是任何其他項目。</span><span class="sxs-lookup"><span data-stu-id="604f8-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="604f8-179">`HttpPostAttribute` ( `[HttpPost]` ) 是實作`IActionConstraint`，只會允許的 HTTP 動詞命令時，必須選取動作`POST`。</span><span class="sxs-lookup"><span data-stu-id="604f8-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="604f8-180">與否`IActionConstraint`讓`Edit(int, Product)`'好' 符合比`Edit(int)`，因此`Edit(int, Product)`會先嘗試。</span><span class="sxs-lookup"><span data-stu-id="604f8-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="604f8-181">您只需要撰寫自訂`IActionConstraint`實作中特定的案例，但它的一定要了解角色的屬性，例如`HttpPostAttribute`-相似的屬性定義的其他 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="604f8-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="604f8-182">在傳統路由是普遍的動作時要使用相同的動作名稱的一部份`show form -> submit form`工作流程。</span><span class="sxs-lookup"><span data-stu-id="604f8-182">In conventional routing it's common for actions to use the same action name when they are part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="604f8-183">此模式的方便性，不論會變得更加明顯檢閱之後[了解 IActionConstraint](#understanding-iactionconstraint) > 一節。</span><span class="sxs-lookup"><span data-stu-id="604f8-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="604f8-184">如果多個路由相符，而且 MVC 找不到 '最佳' 的路由，則會擲回`AmbiguousActionException`。</span><span class="sxs-lookup"><span data-stu-id="604f8-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name=routing-route-name-ref-label></a>

### <a name="route-names"></a><span data-ttu-id="604f8-185">路由名稱</span><span class="sxs-lookup"><span data-stu-id="604f8-185">Route names</span></span>

<span data-ttu-id="604f8-186">字串`"blog"`和`"default"`在下列範例中是路由名稱：</span><span class="sxs-lookup"><span data-stu-id="604f8-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="604f8-187">路由名稱給與路由邏輯的名稱，讓具名的路由可用於 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="604f8-188">這可以大幅簡化 URL 建立時的路由順序可能會使複雜 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="604f8-189">路由名稱必須是唯一的應用程式層級。</span><span class="sxs-lookup"><span data-stu-id="604f8-189">Routes names must be unique application-wide.</span></span>

<span data-ttu-id="604f8-190">路由名稱不會有影響 url 比對或處理的要求。它們只用於 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-190">Route names have no impact on URL matching or handling of requests; they are used only for URL generation.</span></span> <span data-ttu-id="604f8-191">[路由](xref:fundamentals/routing)有更詳細資訊包括在 MVC 特定協助程式 URL 的產生 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name=attribute-routing-ref-label></a>

## <a name="attribute-routing"></a><span data-ttu-id="604f8-192">路由屬性</span><span class="sxs-lookup"><span data-stu-id="604f8-192">Attribute routing</span></span>

<span data-ttu-id="604f8-193">路由屬性，可用於屬性的一組動作會直接對應到路由範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="604f8-194">在下列範例中，`app.UseMvc();`用於`Configure`傳遞方法，並沒有路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="604f8-195">`HomeController`會比對一組類似於預設路由的 Url 的`{controller=Home}/{action=Index}/{id?}`會比對：</span><span class="sxs-lookup"><span data-stu-id="604f8-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="604f8-196">`HomeController.Index()`動作將會執行任何的 URL 路徑`/`， `/Home`，或`/Home/Index`。</span><span class="sxs-lookup"><span data-stu-id="604f8-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-197">此範例中會反白顯示屬性路由和傳統路由的主要程式設計差異。</span><span class="sxs-lookup"><span data-stu-id="604f8-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="604f8-198">屬性路由需要更多的輸入，以指定的路由。傳統的預設路由更簡潔的方式處理路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="604f8-199">不過，屬性路由可讓，並需要精確地控制其中的路由範本套用至每個動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="604f8-200">路由的控制器名稱和動作名稱的屬性與播放**沒有**在其中選取動作的角色。</span><span class="sxs-lookup"><span data-stu-id="604f8-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="604f8-201">這個範例會比對上一個範例相同的 Url。</span><span class="sxs-lookup"><span data-stu-id="604f8-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="604f8-202">上述的路由範本未定義的路由參數`action`， `area`，和`controller`。</span><span class="sxs-lookup"><span data-stu-id="604f8-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="604f8-203">事實上，這些路由參數不允許在屬性路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="604f8-204">既然已與動作相關聯的路由範本，它對於意義剖析從 URL 的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="604f8-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="604f8-205">屬性 Http [動詞] 屬性與路由</span><span class="sxs-lookup"><span data-stu-id="604f8-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="604f8-206">路由屬性也可使用`Http[Verb]`等屬性`HttpPostAttribute`。</span><span class="sxs-lookup"><span data-stu-id="604f8-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="604f8-207">所有這些屬性可以接受的路由範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="604f8-208">此範例會顯示符合相同的路由範本的兩個動作：</span><span class="sxs-lookup"><span data-stu-id="604f8-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="604f8-209">類似的 URL 路徑`/products` `ProductsApi.ListProducts` HTTP 動詞命令時，將會執行動作`GET`和`ProductsApi.CreateProduct`HTTP 動詞命令時，將會執行`POST`。</span><span class="sxs-lookup"><span data-stu-id="604f8-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="604f8-210">屬性路由會先根據路由屬性所定義的路由範本組 URL 相符。</span><span class="sxs-lookup"><span data-stu-id="604f8-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="604f8-211">一旦與相符的路由範本，`IActionConstraint`條件約束會套用至可執行的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="604f8-212">當建置 REST API，很少會想要使用`[Route(...)]`動作方法上。</span><span class="sxs-lookup"><span data-stu-id="604f8-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="604f8-213">最好是使用多個特定`Http*Verb*Attributes`要精確地指定您的 API 的支援。</span><span class="sxs-lookup"><span data-stu-id="604f8-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="604f8-214">REST Api 的用戶端應該知道什麼路徑和 HTTP 動詞命令將對應至特定的邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="604f8-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="604f8-215">因為屬性路由適用於特定的動作，所以可以輕鬆地進行所需的路由範本定義一部分的參數。</span><span class="sxs-lookup"><span data-stu-id="604f8-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="604f8-216">在此範例中，`id`是 URL 路徑中的必要部分。</span><span class="sxs-lookup"><span data-stu-id="604f8-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="604f8-217">`ProductsApi.GetProduct(int)`動作將會執行類似的 URL 路徑`/products/3`但不適用於 URL 路徑，如`/products`。</span><span class="sxs-lookup"><span data-stu-id="604f8-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="604f8-218">請參閱[路由](../../fundamentals/routing.md)路由樣板與相關的選項的完整說明。</span><span class="sxs-lookup"><span data-stu-id="604f8-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="604f8-219">路由名稱</span><span class="sxs-lookup"><span data-stu-id="604f8-219">Route Name</span></span>

<span data-ttu-id="604f8-220">下列程式碼定義*路由名稱*的`Products_List`:</span><span class="sxs-lookup"><span data-stu-id="604f8-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="604f8-221">路由名稱可以用來產生特定的路由為基礎的 URL。</span><span class="sxs-lookup"><span data-stu-id="604f8-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="604f8-222">路由名稱的 URL 相符的路由行為造成任何影響，而且僅適用於 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="604f8-223">路由名稱必須是唯一的應用程式層級。</span><span class="sxs-lookup"><span data-stu-id="604f8-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-224">這和傳統*預設路由*，而後者可定義`id`參數為選擇性 (`{id?}`)。</span><span class="sxs-lookup"><span data-stu-id="604f8-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="604f8-225">這項功能精確地指定應用程式開發介面有優點，例如允許`/products`和`/products/5`分派至不同的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name=routing-combining-ref-label></a>

### <a name="combining-routes"></a><span data-ttu-id="604f8-226">結合路由</span><span class="sxs-lookup"><span data-stu-id="604f8-226">Combining routes</span></span>

<span data-ttu-id="604f8-227">若要讓屬性路由較不重複，路由在控制器上的屬性會結合路由屬性上的個別動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="604f8-228">在控制器上定義的任何路由範本前面加上了動作的路由範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="604f8-229">在控制器上放置路由屬性可讓**所有**控制器中的動作會使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="604f8-230">在此範例中的 URL 路徑`/products`可以比對`ProductsApi.ListProducts`，和 URL 路徑`/products/5`可以比對`ProductsApi.GetProduct(int)`。</span><span class="sxs-lookup"><span data-stu-id="604f8-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="604f8-231">這兩種動作只會比對 HTTP`GET`因為使用裝飾`HttpGetAttribute`。</span><span class="sxs-lookup"><span data-stu-id="604f8-231">Both of these actions only match HTTP `GET` because they are decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="604f8-232">路由範本套用至動作開頭`/`未執行取得結合套用至控制器的路由範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-232">Route templates applied to an action that begin with a `/` do not get combined with route templates applied to the controller.</span></span> <span data-ttu-id="604f8-233">這個範例會比對一組類似的 URL 路徑*預設路由*。</span><span class="sxs-lookup"><span data-stu-id="604f8-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
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

<a name=routing-ordering-ref-label></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="604f8-234">排序屬性路由</span><span class="sxs-lookup"><span data-stu-id="604f8-234">Ordering attribute routes</span></span>

<span data-ttu-id="604f8-235">相較於傳統路由定義的順序執行，屬性路由建置樹狀結構，並同時符合所有路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="604f8-236">這個行為會如同-如果路由項目放入的理想排序;最適合的路由有機會執行之前的較通用的路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="604f8-237">比方說，路由喜歡`blog/search/{topic}`更為具體的路由，像是`blog/{*article}`。</span><span class="sxs-lookup"><span data-stu-id="604f8-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="604f8-238">邏輯上來說`blog/search/{topic}`路由 '執行' 首先，根據預設，因為這是只有合理的排序。</span><span class="sxs-lookup"><span data-stu-id="604f8-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="604f8-239">使用傳統的路由，開發人員會負責依預期順序放置路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="604f8-240">路由屬性可以設定的順序，使用`Order`提供架構路由屬性的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="604f8-241">路由的處理方式遞增排序`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="604f8-242">預設順序是`0`。</span><span class="sxs-lookup"><span data-stu-id="604f8-242">The default order is `0`.</span></span> <span data-ttu-id="604f8-243">設定路由使用`Order = -1`之前不會設定訂單的路由會執行。</span><span class="sxs-lookup"><span data-stu-id="604f8-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="604f8-244">設定路由使用`Order = 1`之後預設路由順序會執行。</span><span class="sxs-lookup"><span data-stu-id="604f8-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="604f8-245">避免取決於`Order`。</span><span class="sxs-lookup"><span data-stu-id="604f8-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="604f8-246">如果您的 URL 空間需要明確的順序值正確路由，則用戶端也可能會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="604f8-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="604f8-247">在 一般屬性路由選取正確的路由與 URL 相符。</span><span class="sxs-lookup"><span data-stu-id="604f8-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="604f8-248">如果不使用用來產生 URL 的預設順序，使用的路由名稱通常比套用覆寫現狀`Order`屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name=routing-token-replacement-templates-ref-label></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="604f8-249">語彙基元取代路由範本中的 ([控制器]，[動作]，[區域])</span><span class="sxs-lookup"><span data-stu-id="604f8-249">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="604f8-250">為了方便起見，屬性路由支援*語彙基元取代*所封入語彙基元，在方括號中 (`[`， `]`)。</span><span class="sxs-lookup"><span data-stu-id="604f8-250">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="604f8-251">語彙基元`[action]`， `[area]`，和`[controller]`會被取代的動作名稱、 區域名稱和控制器名稱，從路由定義所在的動作值。</span><span class="sxs-lookup"><span data-stu-id="604f8-251">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="604f8-252">在此範例中的動作可以比對 URL 路徑註解中所述：</span><span class="sxs-lookup"><span data-stu-id="604f8-252">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="604f8-253">取代 token，就會發生的建置屬性路由的最後一個步驟。</span><span class="sxs-lookup"><span data-stu-id="604f8-253">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="604f8-254">上述範例中的行為相同，如以下程式碼：</span><span class="sxs-lookup"><span data-stu-id="604f8-254">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="604f8-255">也可以使用繼承結合屬性路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-255">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="604f8-256">這是特別強大結合 token 取代。</span><span class="sxs-lookup"><span data-stu-id="604f8-256">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="604f8-257">語彙基元替換也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="604f8-257">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="604f8-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]`會產生唯一的路由名稱，每個動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="604f8-259">要比對常值語彙基元取代分隔符號`[`或`]`，它藉由重複的字元逸出 (`[[`或`]]`)。</span><span class="sxs-lookup"><span data-stu-id="604f8-259">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name=routing-multiple-routes-ref-label></a>

### <a name="multiple-routes"></a><span data-ttu-id="604f8-260">多個路由</span><span class="sxs-lookup"><span data-stu-id="604f8-260">Multiple Routes</span></span>

<span data-ttu-id="604f8-261">此屬性定義的多個連線到相同的動作的路由的路由支援。</span><span class="sxs-lookup"><span data-stu-id="604f8-261">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="604f8-262">若要模擬的行為是最常見的使用*傳統的預設路由*如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="604f8-262">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="604f8-263">將多個路由屬性放置在控制器上，表示每個會合併與每個路由屬性，在動作方法。</span><span class="sxs-lookup"><span data-stu-id="604f8-263">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="604f8-264">當多個路由屬性 (可實作`IActionConstraint`) 放置動作，則每個動作的條件約束結合從定義該屬性的路由範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-264">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="604f8-265">雖然使用多個路由動作可以看起來是功能強大，最好是保留您的應用程式 URL 空間，簡單且妥善定義。</span><span class="sxs-lookup"><span data-stu-id="604f8-265">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="604f8-266">只有在需要時，例如若要支援現有的用戶端時，才使用多個路由動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-266">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name=routing-attr-options></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="604f8-267">指定屬性路由選擇性參數、 預設值和條件約束</span><span class="sxs-lookup"><span data-stu-id="604f8-267">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="604f8-268">屬性路由支援做為傳統的路由，指定選擇性參數、 預設值和條件約束相同的內嵌語法。</span><span class="sxs-lookup"><span data-stu-id="604f8-268">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="604f8-269">請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)的路由範本語法的詳細描述。</span><span class="sxs-lookup"><span data-stu-id="604f8-269">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name=routing-cust-rt-attr-irt-ref-label></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="604f8-270">使用的自訂路由屬性`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="604f8-270">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="604f8-271">所有 framework 所提供的路由屬性 ( `[Route(...)]`，`[HttpGet(...)]`等等) 實作`IRouteTemplateProvider`介面。</span><span class="sxs-lookup"><span data-stu-id="604f8-271">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="604f8-272">MVC 控制器類別和動作方法上的屬性時應用程式啟動，且會使用可實作看起來`IRouteTemplateProvider`建置一組初始的路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-272">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="604f8-273">您可以實作`IRouteTemplateProvider`來定義您自己的路由屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-273">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="604f8-274">每個`IRouteTemplateProvider`可讓您定義的單一路由，以自訂路由範本中，排序和名稱：</span><span class="sxs-lookup"><span data-stu-id="604f8-274">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="604f8-275">上述範例中的屬性會自動設定`Template`至`"api/[controller]"`時`[MyApiController]`套用。</span><span class="sxs-lookup"><span data-stu-id="604f8-275">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name=routing-app-model-ref-label></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="604f8-276">使用自訂屬性路由的應用程式模型</span><span class="sxs-lookup"><span data-stu-id="604f8-276">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="604f8-277">*應用程式模型*是建立在啟動使用 MVC 路由，並執行動作的中繼資料的所有物件模型。</span><span class="sxs-lookup"><span data-stu-id="604f8-277">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="604f8-278">*應用程式模型*包含所有路由屬性從收集的資料 (透過`IRouteTemplateProvider`)。</span><span class="sxs-lookup"><span data-stu-id="604f8-278">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="604f8-279">您可以撰寫*慣例*修改應用程式模型，在啟動時，以自訂路由的運作方式。</span><span class="sxs-lookup"><span data-stu-id="604f8-279">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="604f8-280">本節說明自訂路由使用應用程式模型的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="604f8-280">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name=routing-mixed-ref-label></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="604f8-281">混合的路由： 路由與傳統路由的屬性</span><span class="sxs-lookup"><span data-stu-id="604f8-281">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="604f8-282">MVC 應用程式可以混合使用傳統的路由和屬性路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-282">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="604f8-283">這是通常使用傳統的路由提供 HTML 網頁瀏覽器，服務控制站，以及屬性控制站服務 REST Api 的路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-283">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="604f8-284">動作可以依照慣例路由傳送或路由屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-284">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="604f8-285">放置控制器或動作的路由可讓路由的屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-285">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="604f8-286">定義屬性路由的動作無法透過傳統的路由，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="604f8-286">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="604f8-287">**任何**路由在控制器上的屬性可讓路由在控制器屬性中的所有動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-287">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-288">路由系統兩種類型的差異是 URL 符合的路由範本之後，套用的程序。</span><span class="sxs-lookup"><span data-stu-id="604f8-288">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="604f8-289">在傳統的路由，從符合的路由值可用來從查閱資料表的所有傳統的路由動作選擇動作與控制器。</span><span class="sxs-lookup"><span data-stu-id="604f8-289">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="604f8-290">路由屬性，每個範本已經與動作相關聯，不必採取任何進一步的查閱。</span><span class="sxs-lookup"><span data-stu-id="604f8-290">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name=routing-url-gen-ref-label></a>

## <a name="url-generation"></a><span data-ttu-id="604f8-291">URL 的產生</span><span class="sxs-lookup"><span data-stu-id="604f8-291">URL Generation</span></span>

<span data-ttu-id="604f8-292">MVC 應用程式可以使用路由的 URL 產生功能，產生動作的 URL 連結。</span><span class="sxs-lookup"><span data-stu-id="604f8-292">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="604f8-293">產生 Url 排除硬式編碼的 Url，讓程式碼更穩定、 更容易維護。</span><span class="sxs-lookup"><span data-stu-id="604f8-293">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="604f8-294">本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 的產生方式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="604f8-294">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="604f8-295">請參閱[路由](../../fundamentals/routing.md)為 URL 的產生的詳細描述。</span><span class="sxs-lookup"><span data-stu-id="604f8-295">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="604f8-296">`IUrlHelper`介面是之間 MVC 和 URL 的產生的路由基礎結構的基礎部分。</span><span class="sxs-lookup"><span data-stu-id="604f8-296">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="604f8-297">您可以找到的執行個體`IUrlHelper`可透過`Url`中控制站、 檢視和檢視元件屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-297">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="604f8-298">在此範例中，`IUrlHelper`透過使用介面`Controller.Url`產生另一個動作的 URL 屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-298">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="604f8-299">如果應用程式使用傳統的預設路由，值`url`變數將會是 URL 路徑字串`/UrlGeneration/Destination`。</span><span class="sxs-lookup"><span data-stu-id="604f8-299">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="604f8-300">此 URL 路徑，會使用傳遞給的值建立的路由，方法是結合從目前的要求 （環境值）、 路由值`Url.Action`並替代為這些值到路由範本：</span><span class="sxs-lookup"><span data-stu-id="604f8-300">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="604f8-301">每個路由中的路由範本有參數值和環境的值的比對名稱來取代其值。</span><span class="sxs-lookup"><span data-stu-id="604f8-301">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="604f8-302">路由參數沒有值，可以使用預設值，如果有一個，或如果它是選擇性會略過 (如果是做為`id`在此範例中)。</span><span class="sxs-lookup"><span data-stu-id="604f8-302">A route parameter that does not have a value can use a default value if it has one, or be skipped if it is optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="604f8-303">URL 的產生任何必要的路由參數沒有對應的值將會失敗。</span><span class="sxs-lookup"><span data-stu-id="604f8-303">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="604f8-304">如果路由 URL 的產生失敗，已嘗試所有的路由，或找到相符項目之前，會嘗試下一個路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-304">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="604f8-305">此範例的`Url.Action`上方假設傳統的路由，但是 URL 產生適用於類似屬性路由，雖然這些概念都不同。</span><span class="sxs-lookup"><span data-stu-id="604f8-305">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="604f8-306">與傳統的路由，路由值可用來依序展開 範本時和路由值`controller`和`action`通常會出現在該範本-因為相符路由的 Url 會遵循*慣例*.</span><span class="sxs-lookup"><span data-stu-id="604f8-306">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="604f8-307">在屬性路由，路由值`controller`和`action`不允許出現在 範本-它們會改為用來查閱要使用哪一個範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-307">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they are instead used to look up which template to use.</span></span>

<span data-ttu-id="604f8-308">這個範例會使用屬性的路由：</span><span class="sxs-lookup"><span data-stu-id="604f8-308">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="604f8-309">MVC 建置查閱資料表的所有屬性路由動作，並將符合`controller`和`action`值，以選取要用於產生 URL 的路由範本。</span><span class="sxs-lookup"><span data-stu-id="604f8-309">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="604f8-310">在上述範例`custom/url/to/destination`產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-310">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="604f8-311">產生 Url 的動作名稱</span><span class="sxs-lookup"><span data-stu-id="604f8-311">Generating URLs by action name</span></span>

<span data-ttu-id="604f8-312">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="604f8-312">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="604f8-313">`Action`) 和所有相關的所有以您想要指定哪些您連結到所指定的控制器名稱和動作名稱的概念為基礎的多載。</span><span class="sxs-lookup"><span data-stu-id="604f8-313">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-314">使用時`Url.Action`，目前的路由值`controller`和`action`會為您指定的值`controller`和`action`同時屬於*環境值***和***值*。</span><span class="sxs-lookup"><span data-stu-id="604f8-314">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="604f8-315">此方法`Url.Action`，一律會使用目前的值`action`和`controller`，且會產生將路由傳送至目前的動作是 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="604f8-315">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="604f8-316">路由嘗試使用中環境值的值來填入您在產生 URL 時未提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="604f8-316">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="604f8-317">使用類似的路由`{a}/{b}/{c}/{d}`和環境值`{ a = Alice, b = Bob, c = Carol, d = David }`、 路由有足夠的資訊產生不含任何其他值的 URL-因為所有路由參數值。</span><span class="sxs-lookup"><span data-stu-id="604f8-317">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="604f8-318">如果您加入值`{ d = Donovan }`，值`{ d = David }`會遭到忽略，而且產生的 URL 路徑會是`Alice/Bob/Carol/Donovan`。</span><span class="sxs-lookup"><span data-stu-id="604f8-318">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="604f8-319">URL 路徑是階層式。</span><span class="sxs-lookup"><span data-stu-id="604f8-319">URL paths are hierarchical.</span></span> <span data-ttu-id="604f8-320">在此範例中，如果您已新增值`{ c = Cheryl }`，兩個值`{ c = Carol, d = David }`會遭到忽略。</span><span class="sxs-lookup"><span data-stu-id="604f8-320">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="604f8-321">在此情況下我們不再需要的值`d`和 URL 的產生將會失敗。</span><span class="sxs-lookup"><span data-stu-id="604f8-321">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="604f8-322">您必須指定所需的值為`c`和`d`。</span><span class="sxs-lookup"><span data-stu-id="604f8-322">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="604f8-323">您可能預期叫用此問題的預設路由 (`{controller}/{action}/{id?}`)-但很少會發生這種作法，因為行為`Url.Action`會永遠明確指定`controller`和`action`值。</span><span class="sxs-lookup"><span data-stu-id="604f8-323">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="604f8-324">較長的多載`Url.Action`也則額外接受一個*路由值*以提供路由參數值以外的物件`controller`和`action`。</span><span class="sxs-lookup"><span data-stu-id="604f8-324">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="604f8-325">通常您會看到這搭配`id`像`Url.Action("Buy", "Products", new { id = 17 })`。</span><span class="sxs-lookup"><span data-stu-id="604f8-325">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="604f8-326">依照慣例*路由值*物件通常是匿名類型的物件，但它也可以是`IDictionary<>`或*純舊的.NET 物件*。</span><span class="sxs-lookup"><span data-stu-id="604f8-326">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="604f8-327">不相符的路由參數的任何其他的路由值會放在查詢字串。</span><span class="sxs-lookup"><span data-stu-id="604f8-327">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="604f8-328">若要建立的絕對 URL，使用多載，接受`protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="604f8-328">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name=routing-gen-urls-route-ref-label></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="604f8-329">產生 Url 的路由</span><span class="sxs-lookup"><span data-stu-id="604f8-329">Generating URLs by route</span></span>

<span data-ttu-id="604f8-330">上述程式碼會示範產生傳入控制器和動作名稱的 URL。</span><span class="sxs-lookup"><span data-stu-id="604f8-330">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="604f8-331">`IUrlHelper`也提供`Url.RouteUrl`系列的方法。</span><span class="sxs-lookup"><span data-stu-id="604f8-331">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="604f8-332">這些方法都類似於`Url.Action`，但它們不會複製目前的值`action`和`controller`為路由的值。</span><span class="sxs-lookup"><span data-stu-id="604f8-332">These methods are similar to `Url.Action`, but they do not copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="604f8-333">最常見的用法是，指定要產生的 URL，通常使用特定的路由的路由名稱*沒有*指定控制器或動作名稱。</span><span class="sxs-lookup"><span data-stu-id="604f8-333">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name=routing-gen-urls-html-ref-label></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="604f8-334">以 HTML 產生 Url</span><span class="sxs-lookup"><span data-stu-id="604f8-334">Generating URLs in HTML</span></span>

<span data-ttu-id="604f8-335">`IHtmlHelper`提供`HtmlHelper`方法`Html.BeginForm`和`Html.ActionLink`產生`<form>`和`<a>`元素分別。</span><span class="sxs-lookup"><span data-stu-id="604f8-335">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="604f8-336">這些方法會使用`Url.Action`方法來產生 URL 以及接受類似的引數。</span><span class="sxs-lookup"><span data-stu-id="604f8-336">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="604f8-337">`Url.RouteUrl`馬其頓騎兵如`HtmlHelper`是`Html.BeginRouteForm`和`Html.RouteLink`其具有類似的功能。</span><span class="sxs-lookup"><span data-stu-id="604f8-337">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="604f8-338">TagHelpers 產生透過 Url `form` TagHelper 和`<a>`TagHelper。</span><span class="sxs-lookup"><span data-stu-id="604f8-338">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="604f8-339">這兩種使用`IUrlHelper`其實作。</span><span class="sxs-lookup"><span data-stu-id="604f8-339">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="604f8-340">請參閱[使用表單](../views/working-with-forms.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="604f8-340">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="604f8-341">在檢視內`IUrlHelper`可透過`Url`未涵蓋的上述任何特定 URL 的產生的屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-341">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name=routing-gen-urls-action-ref-label></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="604f8-342">在動作結果中產生 URL</span><span class="sxs-lookup"><span data-stu-id="604f8-342">Generating URLS in Action Results</span></span>

<span data-ttu-id="604f8-343">上述範例顯示使用`IUrlHelper`中控制站，而在控制器中最常見的用法是做為一部分的動作結果產生的 URL。</span><span class="sxs-lookup"><span data-stu-id="604f8-343">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="604f8-344">`ControllerBase`和`Controller`基底類別會提供便利的方法參考另一個動作的動作結果。</span><span class="sxs-lookup"><span data-stu-id="604f8-344">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="604f8-345">一個典型的使用方式是接受使用者輸入之後重新導向。</span><span class="sxs-lookup"><span data-stu-id="604f8-345">One typical usage is to redirect after accepting user input.</span></span>

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

<span data-ttu-id="604f8-346">動作結果的 factory 方法的方法遵循類似的模式上`IUrlHelper`。</span><span class="sxs-lookup"><span data-stu-id="604f8-346">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name=routing-dedicated-ref-label></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="604f8-347">特殊的專用的傳統路由案例</span><span class="sxs-lookup"><span data-stu-id="604f8-347">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="604f8-348">傳統的路由，可以使用一種特殊的路由定義稱為*專用的傳統路由*。</span><span class="sxs-lookup"><span data-stu-id="604f8-348">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="604f8-349">在下列範例中，路由命名為`blog`是專用的傳統路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-349">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="604f8-350">使用這些路由定義，`Url.Action("Index", "Home")`將產生的 URL 路徑`/`與`default`路由，但為什麼？</span><span class="sxs-lookup"><span data-stu-id="604f8-350">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="604f8-351">您猜的路由值`{ controller = Home, action = Index }`足以產生的 URL 使用`blog`，而結果會是`/blog?action=Index&controller=Home`。</span><span class="sxs-lookup"><span data-stu-id="604f8-351">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="604f8-352">專用的傳統路由依賴為特殊的行為，沒有對應的路由參數可防止路由的預設值是 「 太窮盡 」 與 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-352">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="604f8-353">預設值是在此情況下`{ controller = Blog, action = Article }`，並沒有`controller`也`action`會顯示為路由參數。</span><span class="sxs-lookup"><span data-stu-id="604f8-353">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="604f8-354">當執行路由 URL 的產生時，所提供的值必須符合的預設值。</span><span class="sxs-lookup"><span data-stu-id="604f8-354">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="604f8-355">URL 產生使用`blog`會失敗，因為值`{ controller = Home, action = Index }`不符`{ controller = Blog, action = Article }`。</span><span class="sxs-lookup"><span data-stu-id="604f8-355">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="604f8-356">路由然後再試一次回落`default`，這會成功。</span><span class="sxs-lookup"><span data-stu-id="604f8-356">Routing then falls back to try `default`, which succeeds.</span></span>

<a name=routing-areas-ref-label></a>

## <a name="areas"></a><span data-ttu-id="604f8-357">區域</span><span class="sxs-lookup"><span data-stu-id="604f8-357">Areas</span></span>

<span data-ttu-id="604f8-358">[區域](areas.md)是 MVC 功能，用來組織到群組的相關的功能，做為個別路由-命名空間 （適用於控制器動作） 以及 （適用於檢視） 的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="604f8-358">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="604f8-359">使用區域，可讓應用程式有多個具有相同名稱的控制器，只要它們具有不同*區域*。</span><span class="sxs-lookup"><span data-stu-id="604f8-359">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="604f8-360">使用區域建立階層，以新增另一個路由參數，路由`area`至`controller`和`action`。</span><span class="sxs-lookup"><span data-stu-id="604f8-360">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="604f8-361">本節將討論如何路由互動區域-請參閱[區域](areas.md)詳細資料區域與檢視的使用方式。</span><span class="sxs-lookup"><span data-stu-id="604f8-361">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="604f8-362">下列範例會設定要使用預設的傳統路由 MVC 和*區域路由*名為某區域`Blog`:</span><span class="sxs-lookup"><span data-stu-id="604f8-362">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="604f8-363">比對類似的 URL 路徑時`/Manage/Users/AddUser`，第一個路由會產生路由值`{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="604f8-363">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="604f8-364">`area`路由值所產生的預設值`area`，事實上所建立的路由`MapAreaRoute`相當於下列：</span><span class="sxs-lookup"><span data-stu-id="604f8-364">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="604f8-365">`MapAreaRoute`建立路由，使用預設值和條件約束`area`使用提供的區域名稱，在此情況下`Blog`。</span><span class="sxs-lookup"><span data-stu-id="604f8-365">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="604f8-366">預設值可確保路由，一律會產生`{ area = Blog, ... }`，條件約束需要值`{ area = Blog, ... }`為 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-366">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="604f8-367">傳統的路由是順序而定。</span><span class="sxs-lookup"><span data-stu-id="604f8-367">Conventional routing is order-dependent.</span></span> <span data-ttu-id="604f8-368">一般情況下，路由與區域應放稍早在路由表中因為它們是更具體比沒有區域的路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-368">In general, routes with areas should be placed earlier in the route table as they are more specific than routes without an area.</span></span>

<span data-ttu-id="604f8-369">使用上述範例中，路由值會比對下列動作：</span><span class="sxs-lookup"><span data-stu-id="604f8-369">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="604f8-370">`AreaAttribute`是什麼代表控制器區域的一部分，我們稱此控制器處於`Blog`區域。</span><span class="sxs-lookup"><span data-stu-id="604f8-370">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="604f8-371">控制站不含`[Area]`屬性不是成員的任何區域中，並將**不**相符時`area`路由值係由路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-371">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="604f8-372">在下列範例中，只有列出的第一個控制器會比對路由值`{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="604f8-372">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="604f8-373">每個控制站的命名空間如下所示為求完整起見，否則控制器會有命名衝突，並產生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="604f8-373">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="604f8-374">類別的命名空間有不會影響 MVC 的路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-374">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="604f8-375">前兩個控制站的區域成員，只有項目時符合其各自的區域名稱係由`area`路由值。</span><span class="sxs-lookup"><span data-stu-id="604f8-375">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="604f8-376">第三個控制站不是成員的任何區域中，並可以唯一相符項目沒有值時`area`係由路由。</span><span class="sxs-lookup"><span data-stu-id="604f8-376">The third controller is not a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-377">根據比對*沒有值*，缺少`area`值都相同一樣的值`area`null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="604f8-377">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="604f8-378">當執行區域內的動作時，路由值`area`將可提供作為*環境值*路由用於 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="604f8-378">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="604f8-379">這表示，依預設區域做*自黏*URL 產生如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="604f8-379">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name=iactionconstraint-ref-label></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="604f8-380">了解 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="604f8-380">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="604f8-381">此區段可深入了解架構的內部狀態和 MVC 如何選擇要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-381">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="604f8-382">一般應用程式不需要自訂`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="604f8-382">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="604f8-383">您可能已用過`IActionConstraint`即使您不熟悉的介面。</span><span class="sxs-lookup"><span data-stu-id="604f8-383">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="604f8-384">`[HttpGet]`屬性和類似`[Http-VERB]`屬性實作`IActionConstraint`以限制動作方法執行。</span><span class="sxs-lookup"><span data-stu-id="604f8-384">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="604f8-385">假設傳統，預設路由的 URL 路徑`/Products/Edit`值將會產生`{ controller = Products, action = Edit }`，其會符合**兩者**如下所示的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-385">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="604f8-386">在`IActionConstraint`術語，我們會假設這兩種動作被視為候選項目-因為它們都符合的路由資料。</span><span class="sxs-lookup"><span data-stu-id="604f8-386">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="604f8-387">當`HttpGetAttribute`執行時，它會顯示， *Edit()*的相符項目*取得*而且不是任何其他 HTTP 動詞命令的相符項目。</span><span class="sxs-lookup"><span data-stu-id="604f8-387">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and is not a match for any other HTTP verb.</span></span> <span data-ttu-id="604f8-388">`Edit(...)`動作沒有任何條件約束定義，並因此將會比對任何 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="604f8-388">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="604f8-389">因此假設`POST`-只`Edit(...)`比對。</span><span class="sxs-lookup"><span data-stu-id="604f8-389">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="604f8-390">但對於`GET`這兩個動作仍可以符合-但是，動作與`IActionConstraint`一律視為*更佳*動作，而不比。</span><span class="sxs-lookup"><span data-stu-id="604f8-390">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="604f8-391">因此因為`Edit()`具有`[HttpGet]`它會被視為更明確，而且如果這兩個動作可以比對會選取。</span><span class="sxs-lookup"><span data-stu-id="604f8-391">So because `Edit()` has `[HttpGet]` it is considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="604f8-392">就概念而言，`IActionConstraint`是一種*多載*，但而不多載具有相同名稱的方法，它多載之間的相同的 URL 相符的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-392">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it is overloading between actions that match the same URL.</span></span> <span data-ttu-id="604f8-393">路由屬性也會使用`IActionConstraint`並可能導致從不同的控制器動作這兩個正在考慮的候選項目。</span><span class="sxs-lookup"><span data-stu-id="604f8-393">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name=iactionconstraint-impl-ref-label></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="604f8-394">實作 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="604f8-394">Implementing IActionConstraint</span></span>

<span data-ttu-id="604f8-395">最簡單的方式實作`IActionConstraint`是建立類別，衍生自`System.Attribute`並將它放在您的動作和控制站。</span><span class="sxs-lookup"><span data-stu-id="604f8-395">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="604f8-396">MVC 會自動探索任何`IActionConstraint`，套用做為屬性。</span><span class="sxs-lookup"><span data-stu-id="604f8-396">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="604f8-397">您可以使用應用程式模型來套用條件約束，，這可能是因為它可讓您套用的方式 metaprogram 最彈性的方法。</span><span class="sxs-lookup"><span data-stu-id="604f8-397">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they are applied.</span></span>

<span data-ttu-id="604f8-398">在下列範例中的條件約束會選擇根據動作*國碼*從路由資料。</span><span class="sxs-lookup"><span data-stu-id="604f8-398">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="604f8-399">[完整範例 GitHub 上](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。</span><span class="sxs-lookup"><span data-stu-id="604f8-399">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="604f8-400">您必須負責實作`Accept`方法，並選擇 'Order' 條件約束來執行。</span><span class="sxs-lookup"><span data-stu-id="604f8-400">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="604f8-401">在此情況下，`Accept`方法會傳回`true`代表的動作是相符項目時`country`路由值相符項目。</span><span class="sxs-lookup"><span data-stu-id="604f8-401">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="604f8-402">這點不同於`RouteValueAttribute`在於，前者允許後援至未使用屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="604f8-402">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="604f8-403">此範例會示範，是否您定義`en-US`動作然後類似的國家/地區碼`fr-FR`會切換回更泛型的控制站沒有`[CountrySpecific(...)]`套用。</span><span class="sxs-lookup"><span data-stu-id="604f8-403">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that does not have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="604f8-404">`Order`屬性決定其*階段*條件約束是的一部分。</span><span class="sxs-lookup"><span data-stu-id="604f8-404">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="604f8-405">根據的群組中執行的動作條件約束`Order`。</span><span class="sxs-lookup"><span data-stu-id="604f8-405">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="604f8-406">例如，所有的 framework 提供 HTTP 方法的屬性使用相同`Order`值，使其在同一個階段中執行。</span><span class="sxs-lookup"><span data-stu-id="604f8-406">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="604f8-407">您可以有許多階段視需要實作所需的原則。</span><span class="sxs-lookup"><span data-stu-id="604f8-407">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="604f8-408">若要決定的值`Order`考慮是否應該套用您的條件約束之前 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="604f8-408">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="604f8-409">先執行較低的數字。</span><span class="sxs-lookup"><span data-stu-id="604f8-409">Lower numbers run first.</span></span>
