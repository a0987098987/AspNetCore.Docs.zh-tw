---
title: ASP.NET Core 中的路由至控制器動作
author: rick-anderson
description: 了解 ASP.NET Core MVC 如何使用路由中介軟體來比對內送要求的 URL，並將這些 URL 對應至動作。
ms.author: riande
ms.date: 01/24/2019
uid: mvc/controllers/routing
ms.openlocfilehash: b4d5cd3add3fda6b70873eb5cce1dcee651f9185
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087510"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="43920-103">ASP.NET Core 中的路由至控制器動作</span><span class="sxs-lookup"><span data-stu-id="43920-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="43920-104">作者：[Ryan Nowak](https://github.com/rynowak) 與 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="43920-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="43920-105">ASP.NET Core MVC 使用路由[中介軟體](xref:fundamentals/middleware/index)來比對內送要求的 URL，並將這些 URL 對應至動作。</span><span class="sxs-lookup"><span data-stu-id="43920-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="43920-106">路由是在啟動程式碼或屬性中定義。</span><span class="sxs-lookup"><span data-stu-id="43920-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="43920-107">路由描述 URL 路徑應該如何與動作進行比對。</span><span class="sxs-lookup"><span data-stu-id="43920-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="43920-108">路由也可用來產生回應中所送出的連結 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="43920-109">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="43920-110">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="43920-111">如需詳細資訊，請參閱[混合路由](#routing-mixed-ref-label)。</span><span class="sxs-lookup"><span data-stu-id="43920-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="43920-112">本文件將說明 MVC 與路由之間的互動，並說明一般 MVC 應用程式如何使用路由功能。</span><span class="sxs-lookup"><span data-stu-id="43920-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="43920-113">如需進階路由的詳細資料，請參閱[路由](xref:fundamentals/routing)。</span><span class="sxs-lookup"><span data-stu-id="43920-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="43920-114">設定路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="43920-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="43920-115">在 *Configure* 方法中，您可能會看到類似如下的程式碼：</span><span class="sxs-lookup"><span data-stu-id="43920-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="43920-116">在 `UseMvc` 呼叫內，`MapRoute` 是用來建立單一路由，稱為 `default` 路由。</span><span class="sxs-lookup"><span data-stu-id="43920-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="43920-117">大多數 MVC 應用程式會使用具有類似於 `default` 路由之範本的路由。</span><span class="sxs-lookup"><span data-stu-id="43920-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="43920-118">路由範本 `"{controller=Home}/{action=Index}/{id?}"` 可能符合某個 URL 路徑 (例如 `/Products/Details/5`)，並將透過 Token 化路徑來擷取路由值 `{ controller = Products, action = Details, id = 5 }`。</span><span class="sxs-lookup"><span data-stu-id="43920-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="43920-119">MVC 會嘗試尋找名為 `ProductsController` 的控制器，並執行動作 `Details`：</span><span class="sxs-lookup"><span data-stu-id="43920-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="43920-120">請注意，在此範例中，模型繫結會在叫用此動作時，使用 `id = 5` 值將 `id` 參數設定為 `5`。</span><span class="sxs-lookup"><span data-stu-id="43920-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="43920-121">如需詳細資料，請參閱[模型繫結](../models/model-binding.md)。</span><span class="sxs-lookup"><span data-stu-id="43920-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="43920-122">使用 `default` 路由：</span><span class="sxs-lookup"><span data-stu-id="43920-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="43920-123">路由範本：</span><span class="sxs-lookup"><span data-stu-id="43920-123">The route template:</span></span>

* <span data-ttu-id="43920-124">`{controller=Home}` 將 `Home` 定義為預設的 `controller`</span><span class="sxs-lookup"><span data-stu-id="43920-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="43920-125">`{action=Index}` 將 `Index` 定義為預設的 `action`</span><span class="sxs-lookup"><span data-stu-id="43920-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="43920-126">`{id?}` 將 `id` 定義為選擇性</span><span class="sxs-lookup"><span data-stu-id="43920-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="43920-127">預設和選擇性路由參數不一定要全部出現在 URL 路徑中才算相符。</span><span class="sxs-lookup"><span data-stu-id="43920-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="43920-128">如需路由範本語法的詳細描述，請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="43920-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="43920-129">`"{controller=Home}/{action=Index}/{id?}"` 可能符合 URL 路徑 `/`，並將產生路由值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="43920-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="43920-130">`controller` 和 `action` 的值使用預設值；`id` 不會產生值，因為 URL 路徑中沒有對應的區段。</span><span class="sxs-lookup"><span data-stu-id="43920-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="43920-131">MVC 會使用這些路由值來選取 `HomeController` 和 `Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="43920-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="43920-132">使用此控制器定義和路由範本，就會對下列任何 URL 路徑執行 `HomeController.Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="43920-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="43920-133">`UseMvcWithDefaultRoute` 方法很方便：</span><span class="sxs-lookup"><span data-stu-id="43920-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="43920-134">可用來取代：</span><span class="sxs-lookup"><span data-stu-id="43920-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="43920-135">`UseMvc` 和 `UseMvcWithDefaultRoute` 會將 `RouterMiddleware` 的執行個體新增至中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="43920-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="43920-136">MVC 不會直接與中介軟體互動，而是使用路由來處理要求。</span><span class="sxs-lookup"><span data-stu-id="43920-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="43920-137">MVC 會透過 `MvcRouteHandler` 的執行個體連線到路由。</span><span class="sxs-lookup"><span data-stu-id="43920-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="43920-138">`UseMvc` 內的程式碼類似如下：</span><span class="sxs-lookup"><span data-stu-id="43920-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="43920-139">`UseMvc` 不會直接定義任何路由，而是將預留位置新增至 `attribute` 路由的路由集合。</span><span class="sxs-lookup"><span data-stu-id="43920-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="43920-140">多載 `UseMvc(Action<IRouteBuilder>)` 可讓您新增自己的路由，同時支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="43920-141">`UseMvc` 及其所有變化會新增屬性路由的預留位置；不論您如何設定`UseMvc`，一律可以使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="43920-142">`UseMvcWithDefaultRoute` 會定義預設路由並支援屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="43920-143">[屬性路由](#attribute-routing-ref-label)一節包含屬性路由的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="43920-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="43920-144">慣例路由</span><span class="sxs-lookup"><span data-stu-id="43920-144">Conventional routing</span></span>

<span data-ttu-id="43920-145">`default` 路由：</span><span class="sxs-lookup"><span data-stu-id="43920-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="43920-146">即為「慣例路由」的範例。</span><span class="sxs-lookup"><span data-stu-id="43920-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="43920-147">我們將此樣式稱為「慣例路由」，因為它會建立 URL 路徑的「慣例」：</span><span class="sxs-lookup"><span data-stu-id="43920-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="43920-148">第一個路徑區段對應至控制器名稱</span><span class="sxs-lookup"><span data-stu-id="43920-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="43920-149">第二個對應至動作名稱</span><span class="sxs-lookup"><span data-stu-id="43920-149">the second maps to the action name.</span></span>

* <span data-ttu-id="43920-150">第三個區段代表用來對應至模型實體的選擇性 `id`</span><span class="sxs-lookup"><span data-stu-id="43920-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="43920-151">使用此 `default` 路由，URL 路徑 `/Products/List` 會對應至 `ProductsController.List` 動作；而 `/Blog/Article/17` 會對應至 `BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="43920-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="43920-152">此對應**只會**根據控制器和動作名稱，而不會根據命名空間、來源檔案位置或方法參數。</span><span class="sxs-lookup"><span data-stu-id="43920-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="43920-153">慣例路由與預設路由的搭配使用可讓您快速建置應用程式，而不需要針對每個定義的動作產生新的 URL 模式。</span><span class="sxs-lookup"><span data-stu-id="43920-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="43920-154">針對具有 CRUD 樣式動作的應用程式，在控制器之間保持一致的 URL 有助於簡化程式碼，並讓 UI 更容易預測。</span><span class="sxs-lookup"><span data-stu-id="43920-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="43920-155">路由範本將 `id` 定義為選擇性，也就是您的動作可以直接執行，而不需要將識別碼當作 URL 的一部分提供。</span><span class="sxs-lookup"><span data-stu-id="43920-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="43920-156">如果省略 URL 中的 `id`，通常表示它會由模型繫結設定為 `0`，因此在符合 `id == 0` 的資料庫中找不到任何實體。</span><span class="sxs-lookup"><span data-stu-id="43920-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="43920-157">屬性路由可提供您更細微的控制，讓某些動作需要此識別碼，而其他動作則不需要。</span><span class="sxs-lookup"><span data-stu-id="43920-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="43920-158">依照慣例，本文件將會包含可能出現在正確使用中的選擇性參數 (例如 `id`)。</span><span class="sxs-lookup"><span data-stu-id="43920-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="43920-159">多個路由</span><span class="sxs-lookup"><span data-stu-id="43920-159">Multiple routes</span></span>

<span data-ttu-id="43920-160">您可以將更多呼叫新增至 `MapRoute`，以在 `UseMvc` 內新增多個路由。</span><span class="sxs-lookup"><span data-stu-id="43920-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="43920-161">這樣做可讓您定義多個慣例，或新增特定動作專用的慣例路由，例如：</span><span class="sxs-lookup"><span data-stu-id="43920-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="43920-162">這裡的 `blog` 路由是「專用慣例路由」，也就是它會使用慣例路由系統，但專用於特定動作。</span><span class="sxs-lookup"><span data-stu-id="43920-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="43920-163">由於 `controller` 和 `action` 並未作為參數出現在路由範本中，它們只能有預設值，因此此路由一律會對應至動作 `BlogController.Article`。</span><span class="sxs-lookup"><span data-stu-id="43920-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="43920-164">路由集合中的路由已經過排序，並將依其新增順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="43920-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="43920-165">因此在此範例中，會先嘗試 `blog` 路由，再嘗試 `default` 路由。</span><span class="sxs-lookup"><span data-stu-id="43920-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="43920-166">「專用慣例路由」通常會使用 `{*article}` 等 catch-all 路由參數，來擷取 URL 路徑的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="43920-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="43920-167">這可能會讓路由變得「太窮盡」，也就是它會比對您想要讓其他路由比對的 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="43920-168">將這些「窮盡」路由放在路由表後面可解決此問題。</span><span class="sxs-lookup"><span data-stu-id="43920-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="43920-169">後援</span><span class="sxs-lookup"><span data-stu-id="43920-169">Fallback</span></span>

<span data-ttu-id="43920-170">在要求處理過程中，MVC 會確認路由值是否可用來尋找應用程式中的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="43920-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="43920-171">如果路由值未符合任何動作，則不會將此路由視為一個相符項目，並將嘗試下一個路由。</span><span class="sxs-lookup"><span data-stu-id="43920-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="43920-172">這稱為「遞補」，其目的在於簡化慣例路由重疊的情況。</span><span class="sxs-lookup"><span data-stu-id="43920-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="43920-173">釐清動作</span><span class="sxs-lookup"><span data-stu-id="43920-173">Disambiguating actions</span></span>

<span data-ttu-id="43920-174">若透過路由符合兩個動作，MVC 必須釐清並選擇「最佳」候選項目，否則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="43920-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="43920-175">例如：</span><span class="sxs-lookup"><span data-stu-id="43920-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="43920-176">此控制器定義兩個符合 URL 路徑 `/Products/Edit/17` 和路由資料 `{ controller = Products, action = Edit, id = 17 }` 的動作。</span><span class="sxs-lookup"><span data-stu-id="43920-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="43920-177">這是 MVC 控制器的典型模式，其中 `Edit(int)` 會顯示用以編輯產品的表單，而 `Edit(int, Product)` 會處理已張貼的表單。</span><span class="sxs-lookup"><span data-stu-id="43920-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="43920-178">若要執行這項操作，MVC 必須在要求為 HTTP `POST` 時選擇 `Edit(int, Product)`，並在 HTTP 動詞命令為任何其他項目時選擇 `Edit(int)`。</span><span class="sxs-lookup"><span data-stu-id="43920-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="43920-179">`HttpPostAttribute` ( `[HttpPost]` ) 是 `IActionConstraint` 的實作，只有在 HTTP 動詞命令為 `POST` 時，才能選取此動作。</span><span class="sxs-lookup"><span data-stu-id="43920-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="43920-180">`IActionConstraint` 的存在使 `Edit(int, Product)` 比 `Edit(int)`「更符合」，因此會先嘗試 `Edit(int, Product)`。</span><span class="sxs-lookup"><span data-stu-id="43920-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="43920-181">您只需要在特殊情況下撰寫自訂 `IActionConstraint` 實作，但請務必了解 `HttpPostAttribute` 等屬性的角色 (其他 HTTP 動詞命令會定義類似的屬性)。</span><span class="sxs-lookup"><span data-stu-id="43920-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="43920-182">在慣例路由中，當動作是 `show form -> submit form` 工作流程的一部分時，通常會使用相同的動作名稱。</span><span class="sxs-lookup"><span data-stu-id="43920-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="43920-183">檢閱[了解 IActionConstraint](#understanding-iactionconstraint) 一節之後，會更清楚此模式的便利性。</span><span class="sxs-lookup"><span data-stu-id="43920-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="43920-184">如果多個路由相符，而且 MVC 找不到「最佳」路由，則會擲回 `AmbiguousActionException`。</span><span class="sxs-lookup"><span data-stu-id="43920-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="43920-185">路由名稱</span><span class="sxs-lookup"><span data-stu-id="43920-185">Route names</span></span>

<span data-ttu-id="43920-186">下列範例中的字串 `"blog"` 和 `"default"` 是路由名稱：</span><span class="sxs-lookup"><span data-stu-id="43920-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="43920-187">路由名稱為路由提供邏輯名稱，因此可以使用具名路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="43920-188">當路由排序可能使 URL 產生作業變得複雜時，這樣做可大幅簡化 URL 產生作業。</span><span class="sxs-lookup"><span data-stu-id="43920-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="43920-189">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="43920-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="43920-190">路由名稱不會影響要求的 URL 比對或處理，只會用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="43920-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="43920-191">[路由](xref:fundamentals/routing)提供 URL 產生的詳細資訊，包括 MVC 特定協助程式中的 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="43920-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="43920-192">屬性路由</span><span class="sxs-lookup"><span data-stu-id="43920-192">Attribute routing</span></span>

<span data-ttu-id="43920-193">屬性路由使用一組屬性，將動作直接對應至路由範本。</span><span class="sxs-lookup"><span data-stu-id="43920-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="43920-194">在下列範例中，在 `Configure` 方法中使用 `app.UseMvc();`，且未傳遞任何路由。</span><span class="sxs-lookup"><span data-stu-id="43920-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="43920-195">`HomeController` 會比對一組 類似於預設路由 `{controller=Home}/{action=Index}/{id?}` 所比對的 URL：</span><span class="sxs-lookup"><span data-stu-id="43920-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="43920-196">`HomeController.Index()` 動作會針對 URL 路徑 `/`、`/Home` 或 `/Home/Index` 的任何一個來執行。</span><span class="sxs-lookup"><span data-stu-id="43920-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="43920-197">此範例醒目提示屬性路由與慣例路由之間的主要程式設計差異。</span><span class="sxs-lookup"><span data-stu-id="43920-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="43920-198">屬性路由需要更多輸入才能指定路由，而慣例預設路由處理路由的方式則更簡潔。</span><span class="sxs-lookup"><span data-stu-id="43920-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="43920-199">不過，屬性路由允許 (並需要) 精確地控制套用至每個動作的路由範本。</span><span class="sxs-lookup"><span data-stu-id="43920-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="43920-200">使用屬性路由，選取動作時「不會」根據控制器名稱和動作名稱。</span><span class="sxs-lookup"><span data-stu-id="43920-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="43920-201">此範例會符合與上一個範例相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="43920-202">上述路由範本未定義 `action`、`area` 和 `controller` 的路由參數。</span><span class="sxs-lookup"><span data-stu-id="43920-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="43920-203">事實上，屬性路由中不允許有這些路由參數。</span><span class="sxs-lookup"><span data-stu-id="43920-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="43920-204">由於路由範本已與一個動作建立關聯，因此剖析 URL 中的動作名稱並沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="43920-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="43920-205">使用 Http[Verb] 屬性的屬性路由</span><span class="sxs-lookup"><span data-stu-id="43920-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="43920-206">屬性路由也可以使用 `Http[Verb]` 屬性，例如 `HttpPostAttribute`。</span><span class="sxs-lookup"><span data-stu-id="43920-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="43920-207">這些屬性都會接受路由範本。</span><span class="sxs-lookup"><span data-stu-id="43920-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="43920-208">此範例顯示兩個符合相同路由範本的動作：</span><span class="sxs-lookup"><span data-stu-id="43920-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="43920-209">針對 `/products` 等 URL 路徑，當 HTTP 動詞命令為 `GET` 時，會執行 `ProductsApi.ListProducts`；當 HTTP 動詞命令為 `POST` 時，會執行 `ProductsApi.CreateProduct`。</span><span class="sxs-lookup"><span data-stu-id="43920-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="43920-210">屬性路由會先根據路由屬性所定義的一組路由範本來比對 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="43920-211">一旦有路由範本相符，就會套用 `IActionConstraint` 條件約束以決定可執行的動作。</span><span class="sxs-lookup"><span data-stu-id="43920-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="43920-212">建置 REST API 時，很少會想要在動作方法上使用 `[Route(...)]`。</span><span class="sxs-lookup"><span data-stu-id="43920-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="43920-213">最好是使用更明確的 `Http*Verb*Attributes`，以精確地指定 API 的支援項目。</span><span class="sxs-lookup"><span data-stu-id="43920-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="43920-214">REST API 的用戶端必須知道哪些路徑和 HTTP 動詞命令對應至特定邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="43920-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="43920-215">由於屬性路由會套用至特定動作，因此輕鬆就能將參數設為路由範本定義的必要部分。</span><span class="sxs-lookup"><span data-stu-id="43920-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="43920-216">在此範例中，`id` 是 URL 路徑的必要部分。</span><span class="sxs-lookup"><span data-stu-id="43920-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="43920-217">`ProductsApi.GetProduct(int)` 動作會針對 `/products/3` 等 URL 路徑來執行，但不會針對 `/products` 等 URL 路徑來執行。</span><span class="sxs-lookup"><span data-stu-id="43920-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="43920-218">如需路由範本和相關選項的完整描述，請參閱[路由](../../fundamentals/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="43920-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="43920-219">路由名稱</span><span class="sxs-lookup"><span data-stu-id="43920-219">Route Name</span></span>

<span data-ttu-id="43920-220">下列程式碼會定義 `Products_List` 的「路由名稱」：</span><span class="sxs-lookup"><span data-stu-id="43920-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="43920-221">您可以使用路由名稱根據特定路由來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="43920-222">路由名稱不會影響路由的 URL 比對行為，只會用於 URL 產生。</span><span class="sxs-lookup"><span data-stu-id="43920-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="43920-223">在整個應用程式內路由名稱必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="43920-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="43920-224">慣例「預設路由」與此相反，只會將 `id` 參數定義為選擇性 (`{id?}`)。</span><span class="sxs-lookup"><span data-stu-id="43920-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="43920-225">能夠精確指定 API 有些優點，像是允許將 `/products` 和 `/products/5` 分派至不同的動作。</span><span class="sxs-lookup"><span data-stu-id="43920-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="43920-226">合併路由</span><span class="sxs-lookup"><span data-stu-id="43920-226">Combining routes</span></span>

<span data-ttu-id="43920-227">為了避免屬性路由過於重複，控制器上的路由屬性可與個別動作上的路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="43920-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="43920-228">控制器上定義的任何路由範本都會附加到動作上的路由範本之前。</span><span class="sxs-lookup"><span data-stu-id="43920-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="43920-229">將路由屬性放在控制器上，即可讓控制器中的**所有**動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="43920-230">在此範例中，URL 路徑 `/products` 可能符合 `ProductsApi.ListProducts`，而 URL 路徑 `/products/5` 可能符合 `ProductsApi.GetProduct(int)`。</span><span class="sxs-lookup"><span data-stu-id="43920-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="43920-231">由於這兩種動作是以 `HttpGetAttribute` 裝飾，因此只會符合 HTTP `GET`。</span><span class="sxs-lookup"><span data-stu-id="43920-231">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="43920-232">套用至開頭為 `/` 或 `~/` 之動作的路由範本，無法與套用至控制器的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="43920-232">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="43920-233">此範例會比對一組類似於「預設路由」的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="43920-233">This example matches a set of URL paths similar to the *default route*.</span></span>

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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="43920-234">排序屬性路由</span><span class="sxs-lookup"><span data-stu-id="43920-234">Ordering attribute routes</span></span>

<span data-ttu-id="43920-235">相較於依已定義順序執行的慣例路由，屬性路由會建立樹狀並同時比對所有路由。</span><span class="sxs-lookup"><span data-stu-id="43920-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="43920-236">此行為如同依理想的排序來排列路由項目，最明確的路由會有機會在較泛型的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="43920-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="43920-237">例如，`blog/search/{topic}` 等路由比 `blog/{*article}` 等路由更明確。</span><span class="sxs-lookup"><span data-stu-id="43920-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="43920-238">邏輯上來說，預設會先「執行」`blog/search/{topic}`，因為這是唯一合理的排序。</span><span class="sxs-lookup"><span data-stu-id="43920-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="43920-239">使用慣例路由，開發人員會負責依想要的順序來排列路由。</span><span class="sxs-lookup"><span data-stu-id="43920-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="43920-240">您可以使用架構提供之所有路由屬性的 `Order` 屬性，來設定屬性路由的順序。</span><span class="sxs-lookup"><span data-stu-id="43920-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="43920-241">路由會依 `Order` 屬性的遞增排序來處理。</span><span class="sxs-lookup"><span data-stu-id="43920-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="43920-242">預設順序為 `0`。</span><span class="sxs-lookup"><span data-stu-id="43920-242">The default order is `0`.</span></span> <span data-ttu-id="43920-243">使用 `Order = -1` 設定的路由會在未設定順序的路由之前執行。</span><span class="sxs-lookup"><span data-stu-id="43920-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="43920-244">使用 `Order = 1` 設定的路由會在預設路由排序之後執行。</span><span class="sxs-lookup"><span data-stu-id="43920-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="43920-245">請避免依賴 `Order`。</span><span class="sxs-lookup"><span data-stu-id="43920-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="43920-246">如果您的 URL 空間需要明確的順序值才能正確地路由，則同樣也可能會使用戶端混淆。</span><span class="sxs-lookup"><span data-stu-id="43920-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="43920-247">一般而言，屬性路由會透過 URL 比對來選取正確的路由。</span><span class="sxs-lookup"><span data-stu-id="43920-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="43920-248">如果用於 URL 產生的預設順序無效，使用路由名稱作為覆寫通常會比套用 `Order` 屬性更簡單。</span><span class="sxs-lookup"><span data-stu-id="43920-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="43920-249">Razor Pages 路由和 MVC 控制器路由會共用實作。</span><span class="sxs-lookup"><span data-stu-id="43920-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="43920-250">如需 Razor Pages 主題中路由順序的資訊，請參閱 [Razor Pages 路由和應用程式慣例：路由順序](xref:razor-pages/razor-pages-conventions#route-order)。</span><span class="sxs-lookup"><span data-stu-id="43920-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="43920-251">路由範本中的語彙基元取代 ([controller]、[action]、[area])</span><span class="sxs-lookup"><span data-stu-id="43920-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="43920-252">為了方便起見，屬性路由支援以方括號 (`[`、`]`) 括住語彙基元的「語彙基元取代」。</span><span class="sxs-lookup"><span data-stu-id="43920-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="43920-253">語彙基元 `[action]`、`[area]` 與 `[controller]` 會分別以定義路由之動作的動作名稱值、區域名稱值和控制器名稱值來取代。</span><span class="sxs-lookup"><span data-stu-id="43920-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="43920-254">在下列範例中，動作會符合註解中所述的 URL 路徑：</span><span class="sxs-lookup"><span data-stu-id="43920-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="43920-255">語彙基元取代會在建立屬性路由的最後一個步驟發生。</span><span class="sxs-lookup"><span data-stu-id="43920-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="43920-256">上述範例的運作方式與下列程式碼相同：</span><span class="sxs-lookup"><span data-stu-id="43920-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="43920-257">屬性路由也可以透過繼承來合併。</span><span class="sxs-lookup"><span data-stu-id="43920-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="43920-258">這與語彙基元取代結合會特別強大。</span><span class="sxs-lookup"><span data-stu-id="43920-258">This is particularly powerful combined with token replacement.</span></span>

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

<span data-ttu-id="43920-259">語彙基元取代也適用於屬性路由所定義的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="43920-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="43920-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` 會針對每個動作產生唯一的路由名稱。</span><span class="sxs-lookup"><span data-stu-id="43920-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="43920-261">若要比對常值語彙基元取代分隔符號 `[` 或 `]`，請重複字元 (`[[` 或 `]]`) 來將它逸出。</span><span class="sxs-lookup"><span data-stu-id="43920-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="43920-262">使用參數轉換程式自訂語彙基元取代</span><span class="sxs-lookup"><span data-stu-id="43920-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="43920-263">可以使用參數轉換程式自訂語彙基元取代。</span><span class="sxs-lookup"><span data-stu-id="43920-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="43920-264">參數轉換程式會實作 `IOutboundParameterTransformer` 並轉換參數值。</span><span class="sxs-lookup"><span data-stu-id="43920-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="43920-265">例如，自訂 `SlugifyParameterTransformer` 參數轉換器會將 `SubscriptionManagement` 路由值變更為 `subscription-management`。</span><span class="sxs-lookup"><span data-stu-id="43920-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="43920-266">`RouteTokenTransformerConvention` 是一個應用程式模型慣例，它會：</span><span class="sxs-lookup"><span data-stu-id="43920-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="43920-267">將參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="43920-268">在取代屬性路由語彙基元值時自訂它們。</span><span class="sxs-lookup"><span data-stu-id="43920-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="43920-269">`RouteTokenTransformerConvention` 會在 `ConfigureServices` 中註冊為選項。</span><span class="sxs-lookup"><span data-stu-id="43920-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="43920-270">多個路由</span><span class="sxs-lookup"><span data-stu-id="43920-270">Multiple Routes</span></span>

<span data-ttu-id="43920-271">屬性路由支援定義多個路由來達到相同的動作。</span><span class="sxs-lookup"><span data-stu-id="43920-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="43920-272">最常見的用法是模擬「預設慣例路由」的行為，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="43920-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="43920-273">將多個路由屬性放在控制器上，表示這些屬性會各自與動作方法上的每個路由屬性合併。</span><span class="sxs-lookup"><span data-stu-id="43920-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="43920-274">當一個動作上放有多個路由屬性 (實作`IActionConstraint`) 時，則每個動作條件約束都會與屬性中用來定義的路由範本合併。</span><span class="sxs-lookup"><span data-stu-id="43920-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="43920-275">雖然在動作上使用多個路由看似強大，但最好保持簡單且妥善定義的應用程式 URL 空間。</span><span class="sxs-lookup"><span data-stu-id="43920-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="43920-276">只有必要時，才在動作上使用多個路由；例如，為了支援現有的用戶端。</span><span class="sxs-lookup"><span data-stu-id="43920-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="43920-277">指定屬性路由的選擇性參數、預設值和條件約束</span><span class="sxs-lookup"><span data-stu-id="43920-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="43920-278">屬性路由支援使用與慣例路由相同的內嵌語法，來指定選擇性參數、預設值和條件約束。</span><span class="sxs-lookup"><span data-stu-id="43920-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="43920-279">如需路由範本語法的詳細描述，請參閱[路由範本參考](../../fundamentals/routing.md#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="43920-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="43920-280">使用 `IRouteTemplateProvider` 的自訂路由屬性</span><span class="sxs-lookup"><span data-stu-id="43920-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="43920-281">架構中提供的所有路由屬性 (`[Route(...)]`、`[HttpGet(...)]` 等) 都會實作 `IRouteTemplateProvider` 介面。</span><span class="sxs-lookup"><span data-stu-id="43920-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="43920-282">當應用程式啟動並使用實作 `IRouteTemplateProvider` 的屬性來建立初始路由集時，MVC 會尋找控制器類別和動作方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="43920-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="43920-283">您可以實作 `IRouteTemplateProvider` 來定義自己的路由屬性。</span><span class="sxs-lookup"><span data-stu-id="43920-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="43920-284">每個 `IRouteTemplateProvider` 都可讓您定義具有自訂路由範本、順序和名稱的單一路由：</span><span class="sxs-lookup"><span data-stu-id="43920-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="43920-285">上述範例中的屬性會在套用 `[MyApiController]` 時，自動將 `Template` 設定為 `"api/[controller]"`。</span><span class="sxs-lookup"><span data-stu-id="43920-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="43920-286">使用應用程式模型自訂屬性路由</span><span class="sxs-lookup"><span data-stu-id="43920-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="43920-287">「應用程式模型」是在啟動時建立的物件模型，其中包含 MVC 用來路由及執行動作的所有中繼資料。</span><span class="sxs-lookup"><span data-stu-id="43920-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="43920-288">「應用程式模型」包含從路由屬性收集的所有資料 (透過 `IRouteTemplateProvider`)。</span><span class="sxs-lookup"><span data-stu-id="43920-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="43920-289">您可以撰寫「慣例」來修改啟動時的應用程式模型，以自訂路由的運作方式。</span><span class="sxs-lookup"><span data-stu-id="43920-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="43920-290">本節簡單示範如何使用應用程式模型來自訂路由。</span><span class="sxs-lookup"><span data-stu-id="43920-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="43920-291">混合路由：屬性路由與慣例路由</span><span class="sxs-lookup"><span data-stu-id="43920-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="43920-292">MVC 應用程式可以混用慣例路由與屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="43920-293">控制器通常會使用慣例路由來提供 HTML 頁面給瀏覽器，並使用屬性路由來提供 REST API。</span><span class="sxs-lookup"><span data-stu-id="43920-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="43920-294">動作可以使用慣例路由或屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="43920-295">將路由放在控制器或動作上，即可讓它使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="43920-296">定義屬性路由的動作無法透過慣例路由到達，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="43920-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="43920-297">控制器上的**任何**路由屬性可讓控制器中的所有動作使用屬性路由。</span><span class="sxs-lookup"><span data-stu-id="43920-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="43920-298">這兩種路由系統的區別在於 URL 符合某個路由範本之後所套用的程序。</span><span class="sxs-lookup"><span data-stu-id="43920-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="43920-299">在慣例路由中，會使用相符項目中的路由值，從所有慣例路由動作的查閱資料表中選擇動作和控制器。</span><span class="sxs-lookup"><span data-stu-id="43920-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="43920-300">在屬性路由中，每個範本已與一個動作建立關聯，因此不需要進一步查閱。</span><span class="sxs-lookup"><span data-stu-id="43920-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="43920-301">複雜區段</span><span class="sxs-lookup"><span data-stu-id="43920-301">Complex segments</span></span>

<span data-ttu-id="43920-302">複雜區段 (例如，`[Route("/dog{token}cat")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="43920-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="43920-303">如需說明，請參閱[原始程式碼](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296)。</span><span class="sxs-lookup"><span data-stu-id="43920-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="43920-304">如需詳細資訊，請參閱[此問題](https://github.com/aspnet/AspNetCore.Docs/issues/8197)。</span><span class="sxs-lookup"><span data-stu-id="43920-304">For more information, see [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="43920-305">URL 產生</span><span class="sxs-lookup"><span data-stu-id="43920-305">URL Generation</span></span>

<span data-ttu-id="43920-306">MVC 應用程式可以使用路由的 URL 產生功能，來產生動作的 URL 連結。</span><span class="sxs-lookup"><span data-stu-id="43920-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="43920-307">產生 URL 可排除硬式編碼的 URL，讓程式碼更穩定且更容易維護。</span><span class="sxs-lookup"><span data-stu-id="43920-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="43920-308">本節著重於 MVC 所提供的 URL 產生功能，並只會涵蓋 URL 產生運作方式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="43920-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="43920-309">如需 URL 產生的詳細描述，請參閱[路由](../../fundamentals/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="43920-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="43920-310">`IUrlHelper` 介面是 MVC 與用於產生 URL 的路由之間的基礎結構部分。</span><span class="sxs-lookup"><span data-stu-id="43920-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="43920-311">您將會透過控制器、檢視和檢視元件中 `Url` 屬性，來尋找可用的 `IUrlHelper` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="43920-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="43920-312">在此範例中，會透過 `Controller.Url` 屬性使用 `IUrlHelper` 介面來產生另一個動作的 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="43920-313">如果應用程式使用預設慣例路由，`url` 變數的值會是 URL 路徑字串 `/UrlGeneration/Destination`。</span><span class="sxs-lookup"><span data-stu-id="43920-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="43920-314">此 URL 路徑是透過路由所建立，結合了目前要求中的路由值 (環境值) 與傳遞至 `Url.Action` 的值，並在路由範本中取代這些值：</span><span class="sxs-lookup"><span data-stu-id="43920-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="43920-315">路由範本中每個路由參數的值都會以相符名稱的值和環境值所取代。</span><span class="sxs-lookup"><span data-stu-id="43920-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="43920-316">不具有任何值的路由參數可以使用預設值 (若有)；如果是選擇性，則可以略過 (如此範例中的 `id` 所示)。</span><span class="sxs-lookup"><span data-stu-id="43920-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="43920-317">如果任何必要的路由參數沒有對應的值，URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="43920-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="43920-318">如果某個路由的 URL 產生失敗，則會嘗試下一個路由，直到嘗試所有路由或找到相符項目為止。</span><span class="sxs-lookup"><span data-stu-id="43920-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="43920-319">上述 `Url.Action` 範例假設使用慣例路由，但 URL 產生的運作方式會類似屬性路由，雖然概念完全不同。</span><span class="sxs-lookup"><span data-stu-id="43920-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="43920-320">在慣例路由中，使用路由值來展開範本，而且 `controller` 和 `action` 的路由值通常會出現在該範本中 (這是因為路由比對的 URL 符合「慣例」)。</span><span class="sxs-lookup"><span data-stu-id="43920-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="43920-321">在屬性路由中，`controller` 和 `action` 的路由值不可以出現在範本中，而是用來查閱要使用的範本。</span><span class="sxs-lookup"><span data-stu-id="43920-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="43920-322">此範例使用屬性路由：</span><span class="sxs-lookup"><span data-stu-id="43920-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="43920-323">MVC 建立所有屬性路由動作的查閱資料表，並將比對 `controller` 和 `action` 值，以選取要用於 URL 產生的路由範本。</span><span class="sxs-lookup"><span data-stu-id="43920-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="43920-324">在上述範例中，會產生 `custom/url/to/destination`。</span><span class="sxs-lookup"><span data-stu-id="43920-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="43920-325">由動作名稱產生 URL</span><span class="sxs-lookup"><span data-stu-id="43920-325">Generating URLs by action name</span></span>

<span data-ttu-id="43920-326">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="43920-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="43920-327">`Action`) 及所有相關多載的基本概念，都是您想要透過指定控制器名稱和動作名稱，來指定連結的項目。</span><span class="sxs-lookup"><span data-stu-id="43920-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="43920-328">使用 `Url.Action` 時，會為您指定`controller` 和 `action` 的目前路由值 (`controller` 和 `action` 的值都屬於「環境值」**和「值」**)。</span><span class="sxs-lookup"><span data-stu-id="43920-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="43920-329">方法 `Url.Action` 一律會使用 `action` 和 `controller` 的目前值，而且會產生路由至目前動作的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="43920-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="43920-330">路由會嘗試使用環境值中的值，來填入您在產生 URL 時未提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="43920-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="43920-331">使用 `{a}/{b}/{c}/{d}` 等路由和環境值 `{ a = Alice, b = Bob, c = Carol, d = David }`，路由就會有足夠的資訊來產生不含任何其他值的 URL (因為所有路由參數都具有值)。</span><span class="sxs-lookup"><span data-stu-id="43920-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="43920-332">如果您新增值 `{ d = Donovan }`，則會忽略值 `{ d = David }`，而且產生的 URL 路徑會是 `Alice/Bob/Carol/Donovan`。</span><span class="sxs-lookup"><span data-stu-id="43920-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="43920-333">URL 路徑是階層式。</span><span class="sxs-lookup"><span data-stu-id="43920-333">URL paths are hierarchical.</span></span> <span data-ttu-id="43920-334">在上述範例中，如果您新增了值 `{ c = Cheryl }`，則會忽略 `{ c = Carol, d = David }` 這兩個值。</span><span class="sxs-lookup"><span data-stu-id="43920-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="43920-335">在此情況下，不再有 `d` 的值，因此 URL 產生會失敗。</span><span class="sxs-lookup"><span data-stu-id="43920-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="43920-336">您必須指定 `c` 和 `d` 所需的值。</span><span class="sxs-lookup"><span data-stu-id="43920-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="43920-337">使用預設路由 (`{controller}/{action}/{id?}`) 可能會遇到此問題，但實際上很少會遇到此行為，因為 `Url.Action` 一律會明確指定 `controller` 和 `action` 值。</span><span class="sxs-lookup"><span data-stu-id="43920-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="43920-338">較長的 `Url.Action` 多載還會接受一個額外的「路由值」物件，來為 `controller` 和 `action` 以外的路由參數提供值。</span><span class="sxs-lookup"><span data-stu-id="43920-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="43920-339">您最常會在搭配 `id` 時看到此用法，例如 `Url.Action("Buy", "Products", new { id = 17 })`。</span><span class="sxs-lookup"><span data-stu-id="43920-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="43920-340">依照慣例，「路由值」物件通常是匿名型別的物件，但也可以是 `IDictionary<>` 或「純舊 .NET 物件」。</span><span class="sxs-lookup"><span data-stu-id="43920-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="43920-341">不符合路由參數的任何額外路由值都會放在查詢字串中。</span><span class="sxs-lookup"><span data-stu-id="43920-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="43920-342">若要建立絕對 URL，請使用接受 `protocol` 的多載：`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="43920-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="43920-343">由路由產生 URL</span><span class="sxs-lookup"><span data-stu-id="43920-343">Generating URLs by route</span></span>

<span data-ttu-id="43920-344">上述程式碼示範如何藉由傳入控制器和動作名稱來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="43920-345">`IUrlHelper` 也提供 `Url.RouteUrl` 系列方法。</span><span class="sxs-lookup"><span data-stu-id="43920-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="43920-346">這些方法類似於 `Url.Action`，但不會將 `action` 和 `controller` 的目前值複製到路由值。</span><span class="sxs-lookup"><span data-stu-id="43920-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="43920-347">最常見的用法是指定路由名稱使用特定路由來產生 URL，通常「不需要」指定控制器或動作名稱。</span><span class="sxs-lookup"><span data-stu-id="43920-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="43920-348">在 HTML 中產生 URL</span><span class="sxs-lookup"><span data-stu-id="43920-348">Generating URLs in HTML</span></span>

<span data-ttu-id="43920-349">`IHtmlHelper` 提供 `HtmlHelper` 方法 `Html.BeginForm` 和 `Html.ActionLink`，以分別產生 `<form>` 和 `<a>` 項目。</span><span class="sxs-lookup"><span data-stu-id="43920-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="43920-350">這些方法使用 `Url.Action` 方法來產生 URL，並接受類似的引數。</span><span class="sxs-lookup"><span data-stu-id="43920-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="43920-351">`HtmlHelper` 的成對 `Url.RouteUrl` 為 `Html.BeginRouteForm` 和 `Html.RouteLink`，這兩者的功能很類似。</span><span class="sxs-lookup"><span data-stu-id="43920-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="43920-352">TagHelper 透過 `form` TagHelper 和 `<a>` TagHelper 產生 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="43920-353">這兩者使用 `IUrlHelper` 進行實作。</span><span class="sxs-lookup"><span data-stu-id="43920-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="43920-354">如需詳細資訊，請參閱[使用表單](../views/working-with-forms.md)。</span><span class="sxs-lookup"><span data-stu-id="43920-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="43920-355">在檢視中，可透過 `Url` 屬性使用 `IUrlHelper` 來產生上述未涵蓋的任何特定 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="43920-356">在動作結果中產生 URL</span><span class="sxs-lookup"><span data-stu-id="43920-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="43920-357">上述範例示範如何在控制器中使用 `IUrlHelper`，但控制器中最常見的用法是產生 URL 以作為動作結果的一部分。</span><span class="sxs-lookup"><span data-stu-id="43920-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="43920-358">`ControllerBase` 和 `Controller` 基底類別提供便利的方法讓動作結果可參考其他動作。</span><span class="sxs-lookup"><span data-stu-id="43920-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="43920-359">一個典型的用法是在接受使用者輸入之後重新導向。</span><span class="sxs-lookup"><span data-stu-id="43920-359">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

<span data-ttu-id="43920-360">動作結果的 Factory 方法遵循類似於 `IUrlHelper` 上方法的模式。</span><span class="sxs-lookup"><span data-stu-id="43920-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="43920-361">專用慣例路由的特殊案例</span><span class="sxs-lookup"><span data-stu-id="43920-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="43920-362">慣例路由可使用一種特殊路由定義，稱為「專用慣例路由」。</span><span class="sxs-lookup"><span data-stu-id="43920-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="43920-363">在下列範例中，名為 `blog` 的路由是專用慣例路由。</span><span class="sxs-lookup"><span data-stu-id="43920-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="43920-364">透過這些路由定義，`Url.Action("Index", "Home")` 會使用 `default` 路由產生 URL 路徑 `/`，但為什麼？</span><span class="sxs-lookup"><span data-stu-id="43920-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="43920-365">您可能會猜想路由值 `{ controller = Home, action = Index }` 便足以使用 `blog` 來產生 URL，且結果會是 `/blog?action=Index&controller=Home`。</span><span class="sxs-lookup"><span data-stu-id="43920-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="43920-366">專用慣例路由依賴沒有對應路由參數之預設值的特殊行為，以防止用於 URL 產生的路由變得「太窮盡」。</span><span class="sxs-lookup"><span data-stu-id="43920-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="43920-367">在本例中，預設值為 `{ controller = Blog, action = Article }`，`controller` 和 `action` 都不會顯示為路由參數。</span><span class="sxs-lookup"><span data-stu-id="43920-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="43920-368">當路由執行 URL 產生時，所提供的值必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="43920-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="43920-369">使用 `blog` 產生 URL 會失敗，因為值 `{ controller = Home, action = Index }` 不符合 `{ controller = Blog, action = Article }`。</span><span class="sxs-lookup"><span data-stu-id="43920-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="43920-370">路由會接著切換並嘗試 `default`，此時會成功。</span><span class="sxs-lookup"><span data-stu-id="43920-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="43920-371">區域</span><span class="sxs-lookup"><span data-stu-id="43920-371">Areas</span></span>

<span data-ttu-id="43920-372">[區域](areas.md)是 MVC 功能，可將相關功能組織成群組，作為個別路由命名空間 (適用於控制器動作) 和資料夾結構 (適用於檢視)。</span><span class="sxs-lookup"><span data-stu-id="43920-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="43920-373">使用區域可讓應用程式具有多個同名的控制器 (只要這些控制器具有不同的「區域」即可)。</span><span class="sxs-lookup"><span data-stu-id="43920-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="43920-374">使用區域可建立用於路由的階層，方法是將另一個路由參數 `area` 新增至 `controller` 和 `action`。</span><span class="sxs-lookup"><span data-stu-id="43920-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="43920-375">本節將討論路由如何與區域互動；如需區域如何與檢視搭配使用的詳細資料，請參閱[區域](areas.md)。</span><span class="sxs-lookup"><span data-stu-id="43920-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="43920-376">下列範例會設定 MVC 使用預設慣例路由，並為名為 `Blog` 的區域設定「區域路由」：</span><span class="sxs-lookup"><span data-stu-id="43920-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="43920-377">當符合 `/Manage/Users/AddUser` 等 URL 路徑時，第一個路由會產生路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="43920-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="43920-378">`area` 路由值是由 `area` 的預設值所產生；事實上，`MapAreaRoute` 所建立的路由相當於：</span><span class="sxs-lookup"><span data-stu-id="43920-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="43920-379">`MapAreaRoute` 會針對使用所提供之區域名稱 (在本例中為 `Blog`) 的 `area`，使用預設值和條件約束來建立路由。</span><span class="sxs-lookup"><span data-stu-id="43920-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="43920-380">預設值可確保路由一律會產生 `{ area = Blog, ... }`，而條件約束需要 `{ area = Blog, ... }` 值以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="43920-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="43920-381">慣例路由與順序息息相關。</span><span class="sxs-lookup"><span data-stu-id="43920-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="43920-382">一般而言，具有區域的路由應該放在路由表前面，因為這些路由比沒有區域的路由更明確。</span><span class="sxs-lookup"><span data-stu-id="43920-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="43920-383">使用上述範例，路由值會符合下列動作：</span><span class="sxs-lookup"><span data-stu-id="43920-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="43920-384">`AreaAttribute` 可用來指定控制器為區域的一部分，假設此控制器在 `Blog` 區域中。</span><span class="sxs-lookup"><span data-stu-id="43920-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="43920-385">不含 `[Area]` 屬性的控制器不是任何區域的成員，因此當路由提供 `area` 路由值時**不會**符合。</span><span class="sxs-lookup"><span data-stu-id="43920-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="43920-386">在下列範例中，只有列出的第一個控制器可能符合路由值 `{ area = Blog, controller = Users, action = AddUser }`。</span><span class="sxs-lookup"><span data-stu-id="43920-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="43920-387">為求完整起見，此處顯示每個控制器的命名空間，否則控制器會有命名衝突並產生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="43920-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="43920-388">類別命名空間不會影響 MVC 的路由。</span><span class="sxs-lookup"><span data-stu-id="43920-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="43920-389">前兩個控制器是區域的成員，只有在 `area` 路由值提供其各自的區域名稱時才會符合。</span><span class="sxs-lookup"><span data-stu-id="43920-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="43920-390">第三個控制器不是任何區域的成員，只有路由未提供任何值給 `area` 時才會符合。</span><span class="sxs-lookup"><span data-stu-id="43920-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="43920-391">在「未符合任何值」的情況下，缺少 `area` 值相當於 `area` 的值為 Null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="43920-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="43920-392">執行區域中的動作時，`area` 的路由值會作為路由用於 URL 產生的「環境值」。</span><span class="sxs-lookup"><span data-stu-id="43920-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="43920-393">這表示區域預設會以「黏性」方式來處理 URL 產生，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="43920-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="43920-394">了解 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="43920-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="43920-395">本節深入探討架構內部及 MVC 如何選擇要執行的動作。</span><span class="sxs-lookup"><span data-stu-id="43920-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="43920-396">一般應用程式不需要自訂 `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="43920-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="43920-397">即使您不熟悉 `IActionConstraint`，也可能已使用過此介面。</span><span class="sxs-lookup"><span data-stu-id="43920-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="43920-398">`[HttpGet]` 屬性和類似的 `[Http-VERB]` 屬性會實作 `IActionConstraint` 來限制動作方法的執行。</span><span class="sxs-lookup"><span data-stu-id="43920-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="43920-399">假設在預設慣例路由中，URL 路徑 `/Products/Edit` 會產生值 `{ controller = Products, action = Edit }`，該值符合此處所示的**兩個**動作。</span><span class="sxs-lookup"><span data-stu-id="43920-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="43920-400">在 `IActionConstraint` 術語中，我們會假設這兩個動作可視為候選項目 (因為它們都符合路由資料)。</span><span class="sxs-lookup"><span data-stu-id="43920-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="43920-401">當 `HttpGetAttribute` 執行時，*Edit()* 會符合 *GET*，但不符合任何其他 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="43920-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="43920-402">`Edit(...)` 動作未定義任何條件約束，因此會符合任何 HTTP 動詞命令。</span><span class="sxs-lookup"><span data-stu-id="43920-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="43920-403">假設 `POST` - 只有 `Edit(...)` 相符。</span><span class="sxs-lookup"><span data-stu-id="43920-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="43920-404">但對於 `GET`，這兩個動作仍然相符；不過，具有 `IActionConstraint` 的動作一律比沒有的動作「更符合」。</span><span class="sxs-lookup"><span data-stu-id="43920-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="43920-405">由於 `Edit()` 具有 `[HttpGet]`，因此更明確；如果兩個動作都相符，就會選取此動作。</span><span class="sxs-lookup"><span data-stu-id="43920-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="43920-406">就概念而言，`IActionConstraint` 是一種「多載」形式，但它會在符合相同 URL　的動作之間進行多載，而不是多載具有相同名稱的方法。</span><span class="sxs-lookup"><span data-stu-id="43920-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="43920-407">屬性路由也會使用 `IActionConstraint`，因此可能導致來自不同控制器的動作被視為候選項目。</span><span class="sxs-lookup"><span data-stu-id="43920-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="43920-408">實作 IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="43920-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="43920-409">實作 `IActionConstraint` 的最簡單方式是建立衍生自 `System.Attribute` 的類別，並將它放在您的動作和控制器上。</span><span class="sxs-lookup"><span data-stu-id="43920-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="43920-410">MVC 會自動探索作為屬性套用的任何 `IActionConstraint`。</span><span class="sxs-lookup"><span data-stu-id="43920-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="43920-411">您可以使用應用程式模型來套用條件約束，由於您可以之後編寫其套用方式的程式，因此可能是最彈性的方法。</span><span class="sxs-lookup"><span data-stu-id="43920-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="43920-412">在下列範例中，條件約束會根據路由資料中的「國碼 (地區碼)」來選擇動作。</span><span class="sxs-lookup"><span data-stu-id="43920-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="43920-413">[GitHub 上有完整範例](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs)。</span><span class="sxs-lookup"><span data-stu-id="43920-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="43920-414">您必須負責實作 `Accept` 方法，並選擇 'Order' 作為要執行的條件約束。</span><span class="sxs-lookup"><span data-stu-id="43920-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="43920-415">在本例中，`Accept` 方法會傳回 `true`，表示當 `country` 路由值相符時動作會符合。</span><span class="sxs-lookup"><span data-stu-id="43920-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="43920-416">這與 `RouteValueAttribute` 不同，後者允許切換回未使用屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="43920-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="43920-417">此範例顯示如果您定義 `en-US` 動作，則 `fr-FR` 等國碼 (地區碼) 會切換回未套用 `[CountrySpecific(...)]` 的更泛型控制器。</span><span class="sxs-lookup"><span data-stu-id="43920-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="43920-418">`Order` 屬性決定條件約束所屬的「階段」。</span><span class="sxs-lookup"><span data-stu-id="43920-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="43920-419">群組中的動作條件約束會根據 `Order` 來執行。</span><span class="sxs-lookup"><span data-stu-id="43920-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="43920-420">例如，架構提供的所有 HTTP 方法屬性都會使用相同的 `Order` 值，以便在同一個階段中執行。</span><span class="sxs-lookup"><span data-stu-id="43920-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="43920-421">您可以視需要擁有許多階段來實作所需的原則。</span><span class="sxs-lookup"><span data-stu-id="43920-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="43920-422">若要決定 `Order` 的值，請考慮是否應該在 HTTP 方法之前套用條件約束。</span><span class="sxs-lookup"><span data-stu-id="43920-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="43920-423">數值愈低會愈先執行。</span><span class="sxs-lookup"><span data-stu-id="43920-423">Lower numbers run first.</span></span>
