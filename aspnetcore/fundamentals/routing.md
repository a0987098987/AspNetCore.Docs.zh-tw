---
title: "在 ASP.NET Core 路由"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bbbcf9e4-3c4c-4f50-b91e-175fe9cae4e2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/routing
ms.openlocfilehash: 98756e2c5b336aabcf5155d929160b616baaf2ee
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="49c03-103">在 ASP.NET Core 路由</span><span class="sxs-lookup"><span data-stu-id="49c03-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="49c03-104">由[Ryan Nowak](https://github.com/rynowak)， [Steve Smith](http://ardalis.com)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="49c03-104">By [Ryan Nowak](https://github.com/rynowak), [Steve Smith](http://ardalis.com), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="49c03-105">路由功能是負責將傳入的要求對應至路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="49c03-105">Routing functionality is responsible for mapping an incoming request to a route handler.</span></span> <span data-ttu-id="49c03-106">在 ASP.NET 應用程式中定義和設定應用程式啟動時的路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-106">Routes are defined in the ASP.NET app and configured when the app starts up.</span></span> <span data-ttu-id="49c03-107">路由可以選擇性地從要求中所含的 URL 擷取值，這些值就用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="49c03-107">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="49c03-108">使用 ASP.NET 應用程式中的路由資訊，路由的功能也會產生對應至路由處理常式的 Url。</span><span class="sxs-lookup"><span data-stu-id="49c03-108">Using route information from the ASP.NET app, the routing functionality is also able to generate URLs that map to route handlers.</span></span> <span data-ttu-id="49c03-109">因此，路由，可以找到 URL 或對應至路由處理常式資訊為基礎的指定的路由處理常式的 URL 為基礎的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="49c03-109">Therefore, routing can find a route handler based on a URL, or the URL corresponding to a given route handler based on route handler information.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="49c03-110">本文件涵蓋的最低層級的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-110">This document covers the low level ASP.NET Core routing.</span></span> <span data-ttu-id="49c03-111">ASP.NET Core MVC 路由，請參閱[路由至控制器的動作](../mvc/controllers/routing.md)</span><span class="sxs-lookup"><span data-stu-id="49c03-111">For ASP.NET Core MVC routing, see [Routing to Controller Actions](../mvc/controllers/routing.md)</span></span>

[<span data-ttu-id="49c03-112">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="49c03-112">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample)

## <a name="routing-basics"></a><span data-ttu-id="49c03-113">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="49c03-113">Routing basics</span></span>

<span data-ttu-id="49c03-114">路由會使用*路由*(實作[IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) 至：</span><span class="sxs-lookup"><span data-stu-id="49c03-114">Routing uses *routes* (implementations of [IRouter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.routing.irouter)) to:</span></span>

* <span data-ttu-id="49c03-115">對應的內送要求*路由處理常式*</span><span class="sxs-lookup"><span data-stu-id="49c03-115">map incoming requests to *route handlers*</span></span>

* <span data-ttu-id="49c03-116">產生用於回應的 Url</span><span class="sxs-lookup"><span data-stu-id="49c03-116">generate URLs used in responses</span></span>

<span data-ttu-id="49c03-117">一般而言，應用程式沒有單一的路由集合。</span><span class="sxs-lookup"><span data-stu-id="49c03-117">Generally, an app has a single collection of routes.</span></span> <span data-ttu-id="49c03-118">當要求到達時，會依序處理的路由集合。</span><span class="sxs-lookup"><span data-stu-id="49c03-118">When a request arrives, the route collection is processed in order.</span></span> <span data-ttu-id="49c03-119">藉由呼叫符合要求 URL 的路由連入要求會尋找`RouteAsync`路由集合中每個可用的路由上的方法。</span><span class="sxs-lookup"><span data-stu-id="49c03-119">The incoming request looks for a route that matches the request URL by calling the `RouteAsync` method on each available route in the route collection.</span></span> <span data-ttu-id="49c03-120">相反地，回應可以使用路由來產生根據路由資訊的 Url （例如，針對重新導向或連結），因此不需要硬式編碼的 Url，可協助維護性。</span><span class="sxs-lookup"><span data-stu-id="49c03-120">By contrast, a response can use routing to generate URLs (for example, for redirection or links) based on route information, and thus avoid having to hard-code URLs, which helps maintainability.</span></span>

<span data-ttu-id="49c03-121">路由會連接到[中介軟體](middleware.md)由管線`RouterMiddleware`類別。</span><span class="sxs-lookup"><span data-stu-id="49c03-121">Routing is connected to the [middleware](middleware.md) pipeline by the `RouterMiddleware` class.</span></span> <span data-ttu-id="49c03-122">[ASP.NET MVC](../mvc/overview.md)新增路由傳送至中介軟體管線，其組態的一部分。</span><span class="sxs-lookup"><span data-stu-id="49c03-122">[ASP.NET MVC](../mvc/overview.md) adds routing to the middleware pipeline as part of its configuration.</span></span> <span data-ttu-id="49c03-123">若要深入了解使用路由作為獨立元件，請參閱[使用路由的中介軟體](#using-routing-middleware)。</span><span class="sxs-lookup"><span data-stu-id="49c03-123">To learn about using routing as a standalone component, see [using-routing-middleware](#using-routing-middleware).</span></span>

<a name=url-matching-ref></a>

### <a name="url-matching"></a><span data-ttu-id="49c03-124">URL 比對</span><span class="sxs-lookup"><span data-stu-id="49c03-124">URL matching</span></span>

<span data-ttu-id="49c03-125">URL 比對是由哪些路由分派傳入要求的程序*處理常式*。</span><span class="sxs-lookup"><span data-stu-id="49c03-125">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="49c03-126">此程序通常根據 URL 路徑中的資料，但是可以擴充考量在要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="49c03-126">This process is generally based on data in the URL path, but can be extended to consider any data in the request.</span></span> <span data-ttu-id="49c03-127">分派分開處理常式要求的能力是調整的大小和複雜度的應用程式的關鍵。</span><span class="sxs-lookup"><span data-stu-id="49c03-127">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an application.</span></span>

<span data-ttu-id="49c03-128">內送要求輸入`RouterMiddleware`，而它會呼叫`RouteAsync`序列中的每個路由上的方法。</span><span class="sxs-lookup"><span data-stu-id="49c03-128">Incoming requests enter the `RouterMiddleware`, which calls the `RouteAsync` method on each route in sequence.</span></span> <span data-ttu-id="49c03-129">`IRouter`執行個體選擇是否要*處理*藉由設定要求`RouteContext``Handler`為非 null `RequestDelegate`。</span><span class="sxs-lookup"><span data-stu-id="49c03-129">The `IRouter` instance chooses whether to *handle* the request by setting the `RouteContext` `Handler` to a non-null `RequestDelegate`.</span></span> <span data-ttu-id="49c03-130">如果路由設定要求的處理常式，則會叫用路由處理會停止，而此處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="49c03-130">If a route sets a handler for the request, route processing stops and the handler will be invoked to process the request.</span></span> <span data-ttu-id="49c03-131">如果嘗試所有的路由時，而且沒有處理常式的要求中, 介軟體呼叫*下一步*並叫用要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="49c03-131">If all routes are tried and no handler is found for the request, the middleware calls *next* and the next middleware in the request pipeline is invoked.</span></span>

<span data-ttu-id="49c03-132">主要輸入`RouteAsync`是`RouteContext``HttpContext`與目前的要求相關聯。</span><span class="sxs-lookup"><span data-stu-id="49c03-132">The primary input to `RouteAsync` is the `RouteContext` `HttpContext` associated with the current request.</span></span> <span data-ttu-id="49c03-133">`RouteContext.Handler`和`RouteContext``RouteData`是會在之後路由符合設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="49c03-133">The `RouteContext.Handler` and `RouteContext` `RouteData` are outputs that will be set after a route matches.</span></span>

<span data-ttu-id="49c03-134">相符項目期間`RouteAsync`也會設定的屬性`RouteContext.RouteData`根據到目前為止完成的要求處理的適當值。</span><span class="sxs-lookup"><span data-stu-id="49c03-134">A match during `RouteAsync` will also set the properties of the `RouteContext.RouteData` to appropriate values based on the request processing done so far.</span></span> <span data-ttu-id="49c03-135">如果路由符合要求，`RouteContext.RouteData`會包含重要的狀態資訊的相關*結果*。</span><span class="sxs-lookup"><span data-stu-id="49c03-135">If a route matches a request, the `RouteContext.RouteData` will contain important state information about the *result*.</span></span>

<span data-ttu-id="49c03-136">`RouteData``Values`是*路由值*所產生的路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-136">`RouteData` `Values` is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="49c03-137">這些值通常由 token 化 URL，可以用來接受使用者輸入，或進一步分派做出應用程式內。</span><span class="sxs-lookup"><span data-stu-id="49c03-137">These values are usually determined by tokenizing the URL, and can be used to accept user input, or to make further dispatching decisions inside the application.</span></span>

<span data-ttu-id="49c03-138">`RouteData``DataTokens`是相符路由相關的其他資料的屬性包。</span><span class="sxs-lookup"><span data-stu-id="49c03-138">`RouteData` `DataTokens`  is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="49c03-139">`DataTokens`被提供來支援與每個路由，因此應用程式，可以根據哪一個路由決策的資料比對關聯性的狀態。</span><span class="sxs-lookup"><span data-stu-id="49c03-139">`DataTokens` are provided to support associating state data with each route so the application can make decisions later based on which route matched.</span></span> <span data-ttu-id="49c03-140">這些值是開發人員定義，並且不要**不**影響行為的任何方式的路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-140">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="49c03-141">此外，隱藏資料語彙基元中的值可以是任何類型，相較於路由值，必須與字串輕鬆轉換。</span><span class="sxs-lookup"><span data-stu-id="49c03-141">Additionally, values stashed in data tokens can be of any type, in contrast to route values, which must be easily convertible to and from strings.</span></span>

<span data-ttu-id="49c03-142">`RouteData``Routers`是花費在成功比對要求中的組件的路由清單。</span><span class="sxs-lookup"><span data-stu-id="49c03-142">`RouteData` `Routers` is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="49c03-143">路由可以巢狀內，而`Routers`屬性會反映透過邏輯樹狀結構的相符項目所導致的路由路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-143">Routes can be nested inside one another, and the `Routers` property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="49c03-144">通常第一個項目`Routers`路由集合中，而且應該用於 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="49c03-144">Generally the first item in `Routers` is the route collection, and should be used for URL generation.</span></span> <span data-ttu-id="49c03-145">中的最後一個項目`Routers`是相符路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="49c03-145">The last item in `Routers` is the route handler that matched.</span></span>

### <a name="url-generation"></a><span data-ttu-id="49c03-146">URL 的產生</span><span class="sxs-lookup"><span data-stu-id="49c03-146">URL generation</span></span>

<span data-ttu-id="49c03-147">URL 的產生是程序的路由可以建立一組路由值為基礎的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-147">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="49c03-148">這可讓您的處理常式和 Url 存取它們的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="49c03-148">This allows for a logical separation between your handlers and the URLs that access them.</span></span>

<span data-ttu-id="49c03-149">URL 的產生遵循類似的反覆程序，但是一開始呼叫的使用者或 framework 程式碼`GetVirtualPath`路由集合的方法。</span><span class="sxs-lookup"><span data-stu-id="49c03-149">URL generation follows a similar iterative process, but starts with user or framework code calling into the `GetVirtualPath` method of the route collection.</span></span> <span data-ttu-id="49c03-150">每個*路由*後來會有其`GetVirtualPath`方法呼叫序列中非 null 直到`VirtualPathData`傳回。</span><span class="sxs-lookup"><span data-stu-id="49c03-150">Each *route* will then have its `GetVirtualPath` method called in sequence until a non-null `VirtualPathData` is returned.</span></span>

<span data-ttu-id="49c03-151">輸入主要`GetVirtualPath`是：</span><span class="sxs-lookup"><span data-stu-id="49c03-151">The primary inputs to `GetVirtualPath` are:</span></span>

* <span data-ttu-id="49c03-152">`VirtualPathContext` `HttpContext`</span><span class="sxs-lookup"><span data-stu-id="49c03-152">`VirtualPathContext` `HttpContext`</span></span>

* <span data-ttu-id="49c03-153">`VirtualPathContext` `Values`</span><span class="sxs-lookup"><span data-stu-id="49c03-153">`VirtualPathContext` `Values`</span></span>

* <span data-ttu-id="49c03-154">`VirtualPathContext` `AmbientValues`</span><span class="sxs-lookup"><span data-stu-id="49c03-154">`VirtualPathContext` `AmbientValues`</span></span>

<span data-ttu-id="49c03-155">路由主要會使用所提供的路由值`Values`和`AmbientValues`判斷很可能產生的 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="49c03-155">Routes primarily use the route values provided by the `Values` and `AmbientValues` to decide where it is possible to generate a URL and what values to include.</span></span> <span data-ttu-id="49c03-156">`AmbientValues`是從符合目前要求的路由系統所產生的路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="49c03-156">The `AmbientValues` are the set of route values that were produced from matching the current request with the routing system.</span></span> <span data-ttu-id="49c03-157">相反地，`Values`會指定如何產生目前作業所需的 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="49c03-157">In contrast, `Values` are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="49c03-158">`HttpContext`提供當路由需要取得的服務或與目前內容關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="49c03-158">The `HttpContext` is provided in case a route needs to get services or additional data associated with the current context.</span></span>

<span data-ttu-id="49c03-159">提示： 將`Values`為覆寫一組`AmbientValues`。</span><span class="sxs-lookup"><span data-stu-id="49c03-159">Tip: Think of `Values` as being a set of overrides for the `AmbientValues`.</span></span> <span data-ttu-id="49c03-160">URL 的產生會嘗試重複使用目前的要求，讓您輕鬆使用相同的路由或路由值的連結產生 Url 的路由值。</span><span class="sxs-lookup"><span data-stu-id="49c03-160">URL generation tries to reuse route values from the current request to make it easy to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="49c03-161">輸出`GetVirtualPath`是`VirtualPathData`。</span><span class="sxs-lookup"><span data-stu-id="49c03-161">The output of `GetVirtualPath` is a `VirtualPathData`.</span></span> <span data-ttu-id="49c03-162">`VirtualPathData`是的平行處理`RouteData`; 它包含`VirtualPath`輸出 URL，以及一些額外的屬性應該設定路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-162">`VirtualPathData` is a parallel of `RouteData`; it contains the `VirtualPath` for the output URL as well as the some additional properties that should be set by the route.</span></span>

<span data-ttu-id="49c03-163">`VirtualPathData` `VirtualPath`屬性包含*虛擬路徑*所產生的路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-163">The `VirtualPathData` `VirtualPath` property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="49c03-164">根據您的需求，您可能需要處理進一步的路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-164">Depending on your needs you may need to process the path further.</span></span> <span data-ttu-id="49c03-165">比方說，如果您想要呈現以 HTML 產生的 URL 需要在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-165">For instance, if you want to render the generated URL in HTML you need to prepend the base path of the application.</span></span>

<span data-ttu-id="49c03-166">`VirtualPathData` `Router`是已成功產生 URL 的路由的參考。</span><span class="sxs-lookup"><span data-stu-id="49c03-166">The `VirtualPathData` `Router` is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="49c03-167">`VirtualPathData` `DataTokens`屬性是產生 URL 的路由相關的其他資料的字典。</span><span class="sxs-lookup"><span data-stu-id="49c03-167">The `VirtualPathData` `DataTokens` properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="49c03-168">這是的平行`RouteData.DataTokens`。</span><span class="sxs-lookup"><span data-stu-id="49c03-168">This is the parallel of `RouteData.DataTokens`.</span></span>

### <a name="creating-routes"></a><span data-ttu-id="49c03-169">建立路由</span><span class="sxs-lookup"><span data-stu-id="49c03-169">Creating routes</span></span>

<span data-ttu-id="49c03-170">路由提供`Route`類別做為標準的實作`IRouter`。</span><span class="sxs-lookup"><span data-stu-id="49c03-170">Routing provides the `Route` class as the standard implementation of `IRouter`.</span></span> <span data-ttu-id="49c03-171">`Route`使用*路由範本*語法來定義將會比對的 URL 路徑的模式時`RouteAsync`呼叫。</span><span class="sxs-lookup"><span data-stu-id="49c03-171">`Route` uses the *route template* syntax to define patterns that will match against the URL path when `RouteAsync` is called.</span></span> <span data-ttu-id="49c03-172">`Route`將使用相同的路由範本來產生 URL 時`GetVirtualPath`呼叫。</span><span class="sxs-lookup"><span data-stu-id="49c03-172">`Route` will use the same route template to generate a URL when `GetVirtualPath` is called.</span></span>

<span data-ttu-id="49c03-173">大部分的應用程式會藉由呼叫建立路由`MapRoute`或類似上定義的擴充方法的其中一個`IRouteBuilder`。</span><span class="sxs-lookup"><span data-stu-id="49c03-173">Most applications will create routes by calling `MapRoute` or one of the similar extension methods defined on `IRouteBuilder`.</span></span> <span data-ttu-id="49c03-174">所有這些方法會建立的執行個體`Route`並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="49c03-174">All of these methods will create an instance of `Route` and add it to the route collection.</span></span>

<span data-ttu-id="49c03-175">注意：`MapRoute`不接受路由處理常式參數-它只會將所要處理的路由`DefaultHandler`。</span><span class="sxs-lookup"><span data-stu-id="49c03-175">Note: `MapRoute` doesn't take a route handler parameter - it only adds routes that will be handled by the `DefaultHandler`.</span></span> <span data-ttu-id="49c03-176">由於預設處理常式是`IRouter`，它可能會決定不處理要求。</span><span class="sxs-lookup"><span data-stu-id="49c03-176">Since the default handler is an `IRouter`, it may decide not to handle the request.</span></span> <span data-ttu-id="49c03-177">例如，ASP.NET MVC 通常設定為預設處理常式，只處理要求該相符項目，可用的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="49c03-177">For example, ASP.NET MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="49c03-178">若要了解有關 mvc 路由的詳細資訊，請參閱[路由至控制器動作](../mvc/controllers/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="49c03-178">To learn more about routing to MVC, see [Routing to Controller Actions](../mvc/controllers/routing.md).</span></span>

<span data-ttu-id="49c03-179">這是範例`MapRoute`典型的 ASP.NET MVC 路由定義所使用的呼叫：</span><span class="sxs-lookup"><span data-stu-id="49c03-179">This is an example of a `MapRoute` call used by a typical ASP.NET MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="49c03-180">此範本會比對 URL 路徑，如`/Products/Details/17`擷取路由值和`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="49c03-180">This template will match a URL path like `/Products/Details/17` and extract the route values `{ controller = Products, action = Details, id = 17 }`.</span></span> <span data-ttu-id="49c03-181">路由會判斷的值分割的 URL 路徑區段，並比對每個含有區段*路由參數*路由範本的名稱。</span><span class="sxs-lookup"><span data-stu-id="49c03-181">The route values are determined by splitting the URL path into segments, and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="49c03-182">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="49c03-182">Route parameters are named.</span></span> <span data-ttu-id="49c03-183">它們由以大括號括住參數名稱定義`{ }`。</span><span class="sxs-lookup"><span data-stu-id="49c03-183">They are defined by enclosing the parameter name in braces `{ }`.</span></span>

<span data-ttu-id="49c03-184">上述範本也比對的 URL 路徑`/`並產生值`{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="49c03-184">The template above could also match the URL path `/` and would produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="49c03-185">這是因為`{controller}`和`{action}`路由參數有預設值，而`id`路由參數為選用。</span><span class="sxs-lookup"><span data-stu-id="49c03-185">This happens because the `{controller}` and `{action}` route parameters have default values, and the `id` route parameter is optional.</span></span> <span data-ttu-id="49c03-186">Equals`=`號後面接著值之後路由參數名稱定義參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="49c03-186">An equals `=` sign followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="49c03-187">問號`?`路由參數名稱定義為選擇性參數之後。</span><span class="sxs-lookup"><span data-stu-id="49c03-187">A question mark `?` after the route parameter name defines the parameter as optional.</span></span> <span data-ttu-id="49c03-188">路由預設值的參數*一律*路由符合-選擇性參數不會產生路由值，如果沒有任何對應的 URL 路徑區段時，產生的路由值。</span><span class="sxs-lookup"><span data-stu-id="49c03-188">Route parameters with a default value *always* produce a route value when the route matches - optional parameters will not produce a route value if there was no corresponding URL path segment.</span></span>

<span data-ttu-id="49c03-189">請參閱[路由範本參考](#route-template-reference)如的路由範本功能和語法的完整描述。</span><span class="sxs-lookup"><span data-stu-id="49c03-189">See [route-template-reference](#route-template-reference) for a thorough description of route template features and syntax.</span></span>

<span data-ttu-id="49c03-190">這個範例包含*路由條件約束*:</span><span class="sxs-lookup"><span data-stu-id="49c03-190">This example includes a *route constraint*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="49c03-191">此範本會比對 URL 路徑，如`/Products/Details/17`，但不是`/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="49c03-191">This template will match a URL path like `/Products/Details/17`, but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="49c03-192">路由參數定義`{id:int}`定義*路由條件約束*如`id`路由參數。</span><span class="sxs-lookup"><span data-stu-id="49c03-192">The route parameter definition `{id:int}` defines a *route constraint* for the `id` route parameter.</span></span> <span data-ttu-id="49c03-193">路由條件約束實作`IRouteConstraint`和檢查以進行驗證的路由值。</span><span class="sxs-lookup"><span data-stu-id="49c03-193">Route constraints implement `IRouteConstraint` and inspect route values to verify them.</span></span> <span data-ttu-id="49c03-194">在此範例中的路由值`id`必須可轉換成整數。</span><span class="sxs-lookup"><span data-stu-id="49c03-194">In this example the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="49c03-195">請參閱[路由條件約束參考](#route-constraint-reference)如 framework 所提供的路由條件約束的詳細說明。</span><span class="sxs-lookup"><span data-stu-id="49c03-195">See [route-constraint-reference](#route-constraint-reference) for a more detailed explanation of route constraints that are provided by the framework.</span></span>

<span data-ttu-id="49c03-196">其他多載`MapRoute`接受值`constraints`， `dataTokens`，和`defaults`。</span><span class="sxs-lookup"><span data-stu-id="49c03-196">Additional overloads of `MapRoute` accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="49c03-197">這些額外參數的`MapRoute`定義為型別`object`。</span><span class="sxs-lookup"><span data-stu-id="49c03-197">These additional parameters of `MapRoute` are defined as type `object`.</span></span> <span data-ttu-id="49c03-198">這些參數通常是用來傳遞匿名型別的物件，其中的屬性名稱的匿名型別比對路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="49c03-198">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="49c03-199">下列兩個範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="49c03-199">The following two examples create equivalent routes:</span></span>

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="49c03-200">提示： 內嵌語法，來定義條件約束和預設值可以是簡單的路由更方便。</span><span class="sxs-lookup"><span data-stu-id="49c03-200">Tip: The inline syntax for defining constraints and defaults can be more convenient for simple routes.</span></span> <span data-ttu-id="49c03-201">不過，有功能，例如資料語彙基元不支援的內嵌語法。</span><span class="sxs-lookup"><span data-stu-id="49c03-201">However, there are features such as data tokens which are not supported by inline syntax.</span></span>

<span data-ttu-id="49c03-202">這個範例示範一些更多的功能：</span><span class="sxs-lookup"><span data-stu-id="49c03-202">This example demonstrates a few more features:</span></span>

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="49c03-203">此範本會比對 URL 路徑，如`/Blog/All-About-Routing/Introduction`將擷取的值和`{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="49c03-203">This template will match a URL path like `/Blog/All-About-Routing/Introduction` and will extract the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="49c03-204">預設路由值`controller`和`action`即使範本中沒有任何對應的路由參數所產生的路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-204">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="49c03-205">在路由範本中，可以指定預設值。</span><span class="sxs-lookup"><span data-stu-id="49c03-205">Default values can be specified in the route template.</span></span> <span data-ttu-id="49c03-206">`article`路由參數定義為*包羅萬象*星號外觀`*`路由參數名稱之前。</span><span class="sxs-lookup"><span data-stu-id="49c03-206">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk `*` before the route parameter name.</span></span> <span data-ttu-id="49c03-207">所有的路由參數擷取 URL 路徑的其餘部分，而且也可以比對空字串。</span><span class="sxs-lookup"><span data-stu-id="49c03-207">Catch-all route parameters capture the remainder of the URL path, and can also match the empty string.</span></span>

<span data-ttu-id="49c03-208">這個範例會將路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="49c03-208">This example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="49c03-209">此範本會比對 URL 路徑，如`/Products/5`將擷取的值和`{ controller = Products, action = Details, id = 5 }`和資料語彙基元`{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="49c03-209">This template will match a URL path like `/Products/5` and will extract the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

<a name=id1></a>

### <a name="url-generation"></a><span data-ttu-id="49c03-211">URL 的產生</span><span class="sxs-lookup"><span data-stu-id="49c03-211">URL generation</span></span>

<span data-ttu-id="49c03-212">`Route`類別也可以執行一組路由值結合其路由範本的 URL 的產生。</span><span class="sxs-lookup"><span data-stu-id="49c03-212">The `Route` class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="49c03-213">這是邏輯上對應的 URL 路徑的反向程序。</span><span class="sxs-lookup"><span data-stu-id="49c03-213">This is logically the reverse process of matching the URL path.</span></span>

<span data-ttu-id="49c03-214">提示： 若要進一步了解 URL 的產生，假設您想要產生並考慮如何路由範本將會比對該 URL 哪些的 URL。</span><span class="sxs-lookup"><span data-stu-id="49c03-214">Tip: To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="49c03-215">會產生值為何？</span><span class="sxs-lookup"><span data-stu-id="49c03-215">What values would be produced?</span></span> <span data-ttu-id="49c03-216">這相當於粗略 URL 層代中的運作方式`Route`類別。</span><span class="sxs-lookup"><span data-stu-id="49c03-216">This is the rough equivalent of how URL generation works in the `Route` class.</span></span>

<span data-ttu-id="49c03-217">這個範例會使用基本的 ASP.NET MVC 樣式路由：</span><span class="sxs-lookup"><span data-stu-id="49c03-217">This example uses a basic ASP.NET MVC style route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="49c03-218">路由值`{ controller = Products, action = List }`，此路由會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="49c03-218">With the route values `{ controller = Products, action = List }`, this route will generate the URL `/Products/List`.</span></span> <span data-ttu-id="49c03-219">路由值取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-219">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="49c03-220">因為`id`是選用路由參數，它沒有值的任何問題。</span><span class="sxs-lookup"><span data-stu-id="49c03-220">Since `id` is an optional route parameter, it's no problem that it doesn't have a value.</span></span>

<span data-ttu-id="49c03-221">路由值`{ controller = Home, action = Index }`，此路由會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="49c03-221">With the route values `{ controller = Home, action = Index }`, this route will generate the URL `/`.</span></span> <span data-ttu-id="49c03-222">所提供的路由值符合預設值，因此可以放心地省略這些值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="49c03-222">The route values that were provided match the default values so the segments corresponding to those values can be safely omitted.</span></span> <span data-ttu-id="49c03-223">請注意，產生這兩個 Url 會與此路由定義的反覆存取產生相同的路由值，用來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="49c03-223">Note that both URLs generated would round-trip with this route definition and produce the same route values that were used to generate the URL.</span></span>

<span data-ttu-id="49c03-224">提示： 使用 ASP.NET MVC 應用程式應該使用`UrlHelper`產生而不是直接路由至呼叫的 Url。</span><span class="sxs-lookup"><span data-stu-id="49c03-224">Tip: An app using ASP.NET MVC should use `UrlHelper` to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="49c03-225">如需 URL 的產生程序的詳細資訊，請參閱[url 產生參考](#url-generation-reference)。</span><span class="sxs-lookup"><span data-stu-id="49c03-225">For more details about the URL generation process, see [url-generation-reference](#url-generation-reference).</span></span>

## <a name="using-routing-middleware"></a><span data-ttu-id="49c03-226">使用路由的中介軟體</span><span class="sxs-lookup"><span data-stu-id="49c03-226">Using Routing Middleware</span></span>

<span data-ttu-id="49c03-227">新增 NuGet 封裝 「 Microsoft.AspNetCore.Routing"。</span><span class="sxs-lookup"><span data-stu-id="49c03-227">Add the NuGet package "Microsoft.AspNetCore.Routing".</span></span>

<span data-ttu-id="49c03-228">新增至服務容器中路由*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="49c03-228">Add routing to the service container in *Startup.cs*:</span></span>

<span data-ttu-id="49c03-229">[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]</span><span class="sxs-lookup"><span data-stu-id="49c03-229">[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]</span></span>

<span data-ttu-id="49c03-230">必須在設定路由`Configure`方法中的`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="49c03-230">Routes must be configured in the `Configure` method in the `Startup` class.</span></span> <span data-ttu-id="49c03-231">下列範例會使用這些 Api:</span><span class="sxs-lookup"><span data-stu-id="49c03-231">The sample below uses these APIs:</span></span>

* `RouteBuilder`
* `Build`
* <span data-ttu-id="49c03-232">`MapGet`比對僅 HTTP GET 要求</span><span class="sxs-lookup"><span data-stu-id="49c03-232">`MapGet`  Matches only HTTP GET requests</span></span>
* `UseRouter`

<!-- literal_block {"xml:space": "preserve", "source": "fundamentals/routing/sample/RoutingSample/Startup.cs", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->

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

<span data-ttu-id="49c03-233">下表顯示與指定之 Uri 的回應。</span><span class="sxs-lookup"><span data-stu-id="49c03-233">The table below shows the responses with the given URIs.</span></span>

| <span data-ttu-id="49c03-234">URI</span><span class="sxs-lookup"><span data-stu-id="49c03-234">URI</span></span> | <span data-ttu-id="49c03-235">回應</span><span class="sxs-lookup"><span data-stu-id="49c03-235">Response</span></span>  |
| ------- | -------- |
| <span data-ttu-id="49c03-236">/package/create/3</span><span class="sxs-lookup"><span data-stu-id="49c03-236">/package/create/3</span></span>  | <span data-ttu-id="49c03-237">Hello!</span><span class="sxs-lookup"><span data-stu-id="49c03-237">Hello!</span></span> <span data-ttu-id="49c03-238">路由值: [作業中，建立]，[id，3]</span><span class="sxs-lookup"><span data-stu-id="49c03-238">Route values: [operation, create], [id, 3]</span></span> |
| <span data-ttu-id="49c03-239">封裝/追蹤 /-3</span><span class="sxs-lookup"><span data-stu-id="49c03-239">/package/track/-3</span></span>  | <span data-ttu-id="49c03-240">Hello!</span><span class="sxs-lookup"><span data-stu-id="49c03-240">Hello!</span></span> <span data-ttu-id="49c03-241">路由值: [作業，追蹤]，[id，-3]</span><span class="sxs-lookup"><span data-stu-id="49c03-241">Route values: [operation, track], [id, -3]</span></span> |
| <span data-ttu-id="49c03-242">/ 封裝追蹤 /-3 /</span><span class="sxs-lookup"><span data-stu-id="49c03-242">/package/track/-3/</span></span> | <span data-ttu-id="49c03-243">Hello!</span><span class="sxs-lookup"><span data-stu-id="49c03-243">Hello!</span></span> <span data-ttu-id="49c03-244">路由值: [作業，追蹤]，[id，-3]</span><span class="sxs-lookup"><span data-stu-id="49c03-244">Route values: [operation, track], [id, -3]</span></span>  |
| <span data-ttu-id="49c03-245">/package/追蹤 /</span><span class="sxs-lookup"><span data-stu-id="49c03-245">/package/track/</span></span> | <span data-ttu-id="49c03-246">\<落入沒有相符項目 ></span><span class="sxs-lookup"><span data-stu-id="49c03-246">\<Fall through, no match></span></span> |
| <span data-ttu-id="49c03-247">取得 /hello/Joe</span><span class="sxs-lookup"><span data-stu-id="49c03-247">GET /hello/Joe</span></span> | <span data-ttu-id="49c03-248">您好，Joe ！</span><span class="sxs-lookup"><span data-stu-id="49c03-248">Hi, Joe!</span></span> |
| <span data-ttu-id="49c03-249">POST /hello/Joe</span><span class="sxs-lookup"><span data-stu-id="49c03-249">POST /hello/Joe</span></span> | <span data-ttu-id="49c03-250">\<落入、 符合 HTTP GET 只 ></span><span class="sxs-lookup"><span data-stu-id="49c03-250">\<Fall through, matches HTTP GET only></span></span> |
| <span data-ttu-id="49c03-251">取得 /hello/Joe/Smith</span><span class="sxs-lookup"><span data-stu-id="49c03-251">GET /hello/Joe/Smith</span></span> | <span data-ttu-id="49c03-252">\<落入沒有相符項目 ></span><span class="sxs-lookup"><span data-stu-id="49c03-252">\<Fall through, no match></span></span> |

<span data-ttu-id="49c03-253">如果您要設定單一路由，呼叫`app.UseRouter`傳入`IRouter`執行個體。</span><span class="sxs-lookup"><span data-stu-id="49c03-253">If you are configuring a single route, call `app.UseRouter` passing in an `IRouter` instance.</span></span> <span data-ttu-id="49c03-254">您不需要呼叫`RouteBuilder`。</span><span class="sxs-lookup"><span data-stu-id="49c03-254">You won't need to call `RouteBuilder`.</span></span>

<span data-ttu-id="49c03-255">架構會提供一組擴充方法，例如建立路由：</span><span class="sxs-lookup"><span data-stu-id="49c03-255">The framework provides a set of extension methods for creating routes such as:</span></span>

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

<span data-ttu-id="49c03-256">例如，這些方法的某些`MapGet`需要`RequestDelegate`提供。</span><span class="sxs-lookup"><span data-stu-id="49c03-256">Some of these methods such as `MapGet` require a `RequestDelegate` to be provided.</span></span> <span data-ttu-id="49c03-257">`RequestDelegate`會用作*路由處理常式*路由時符合。</span><span class="sxs-lookup"><span data-stu-id="49c03-257">The `RequestDelegate` will be used as the *route handler* when the route matches.</span></span> <span data-ttu-id="49c03-258">此系列中的其他方法可讓設定中介軟體管線，將作為路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="49c03-258">Other methods in this family allow configuring a middleware pipeline which will be used as the route handler.</span></span> <span data-ttu-id="49c03-259">如果*對應*方法不會接受處理常式，例如`MapRoute`，則它會使用`DefaultHandler`。</span><span class="sxs-lookup"><span data-stu-id="49c03-259">If the *Map* method doesn't accept a handler, such as `MapRoute`, then it will use the `DefaultHandler`.</span></span>

<span data-ttu-id="49c03-260">`Map[Verb]`方法會使用條件約束限制 HTTP 指令動詞的方法名稱中的路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-260">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="49c03-261">例如，請參閱[MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88)和[MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)。</span><span class="sxs-lookup"><span data-stu-id="49c03-261">For example, see [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) and [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="49c03-262">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="49c03-262">Route Template Reference</span></span>

<span data-ttu-id="49c03-263">大括號內的語彙基元 (`{ }`) 定義*路由參數*它將繫結如果路由會比對。</span><span class="sxs-lookup"><span data-stu-id="49c03-263">Tokens within curly braces (`{ }`) define *route parameters* which will be bound if the route is matched.</span></span> <span data-ttu-id="49c03-264">您可以定義一個以上的路由參數的路由區段中，但必須以常值。</span><span class="sxs-lookup"><span data-stu-id="49c03-264">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="49c03-265">例如`{controller=Home}{action=Index}`不是有效的路由，因為沒有任何常值之間`{controller}`和`{action}`。</span><span class="sxs-lookup"><span data-stu-id="49c03-265">For example `{controller=Home}{action=Index}` would not be a valid route, since there is no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="49c03-266">這些路由參數必須有名稱，並可能有其他屬性指定。</span><span class="sxs-lookup"><span data-stu-id="49c03-266">These route parameters must have a name, and may have additional attributes specified.</span></span>

<span data-ttu-id="49c03-267">不是路由參數的常值文字 (例如， `{id}`) 及路徑分隔符號`/`必須符合在 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="49c03-267">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="49c03-268">文字比對是區分大小寫，根據已解碼的表示法的 Url 路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-268">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="49c03-269">要比對常值的路由參數分隔符號`{`或`}`，它藉由重複的字元逸出 (`{{`或`}}`)。</span><span class="sxs-lookup"><span data-stu-id="49c03-269">To match the literal route parameter delimiter `{` or  `}`, escape it by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="49c03-270">嘗試擷取具有選用的檔案副檔名的檔案名稱的 URL 模式有其他考量。</span><span class="sxs-lookup"><span data-stu-id="49c03-270">URL patterns that attempt to capture a filename with an optional file extension have additional considerations.</span></span> <span data-ttu-id="49c03-271">例如，使用範本`files/{filename}.{ext?}`-當兩者`filename`和`ext`存在，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="49c03-271">For example, using the template `files/{filename}.{ext?}` - When both `filename` and `ext` exist, both values will be populated.</span></span> <span data-ttu-id="49c03-272">如果只有`filename`在 URL 中，路由相符的項目存在，因為結尾的句點`.`是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="49c03-272">If only `filename` exists in the URL, the route matches because the trailing period `.` is  optional.</span></span> <span data-ttu-id="49c03-273">下列 Url 會符合此路由：</span><span class="sxs-lookup"><span data-stu-id="49c03-273">The following URLs would match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

<span data-ttu-id="49c03-274">您可以使用`*`字元做為前置詞以路由參數繫結至其餘的 URI-這會呼叫*包羅萬象*參數。</span><span class="sxs-lookup"><span data-stu-id="49c03-274">You can use the `*` character as a prefix to a route parameter to bind to the rest of the URI - this is called a *catch-all* parameter.</span></span> <span data-ttu-id="49c03-275">例如，`blog/{*slug}`會比對任何 URI，以啟動`/blog`和其後的任何值 (這會指派給`slug`路由值)。</span><span class="sxs-lookup"><span data-stu-id="49c03-275">For example, `blog/{*slug}` would match any URI that started with `/blog` and had any value following it (which would be assigned to the `slug` route value).</span></span> <span data-ttu-id="49c03-276">Catch all 參數也可以比對空字串。</span><span class="sxs-lookup"><span data-stu-id="49c03-276">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="49c03-277">路由參數可能*預設值*、 指定藉由指定預設參數名稱之後，以分隔`=`。</span><span class="sxs-lookup"><span data-stu-id="49c03-277">Route parameters may have *default values*, designated by specifying the default after the parameter name, separated by an `=`.</span></span> <span data-ttu-id="49c03-278">例如，`{controller=Home}`會定義`Home`的預設值為`controller`。</span><span class="sxs-lookup"><span data-stu-id="49c03-278">For example, `{controller=Home}` would define `Home` as the default value for `controller`.</span></span> <span data-ttu-id="49c03-279">出現在 URL 中的參數沒有值時，會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="49c03-279">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="49c03-280">預設值，除了路由參數可能會是選擇性 (藉由附加指定`?`參數名稱，如下所示的結尾`id?`)。</span><span class="sxs-lookup"><span data-stu-id="49c03-280">In addition to default values, route parameters may be optional (specified by appending a `?` to the end of the parameter name, as in `id?`).</span></span> <span data-ttu-id="49c03-281">選擇性之間的差異以及 「 具有預設值 」 是預設值的路由參數一定會產生值。選擇性的參數在其中一個提供時，才有值。</span><span class="sxs-lookup"><span data-stu-id="49c03-281">The difference between optional and "has default" is that a route parameter with a default value always produces a value; an optional parameter has a value only when one is provided.</span></span>

<span data-ttu-id="49c03-282">路由參數可能也會有條件約束，它必須符合繫結從 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="49c03-282">Route parameters may also have constraints, which must match the route value bound from the URL.</span></span> <span data-ttu-id="49c03-283">新增分號`:`和條件約束名稱之後指定的路由參數名稱*內部條件約束*路由參數上。</span><span class="sxs-lookup"><span data-stu-id="49c03-283">Adding a colon `:` and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="49c03-284">如果條件約束需要引數所提供的括號內`( )`條件約束名稱後面。</span><span class="sxs-lookup"><span data-stu-id="49c03-284">If the constraint requires arguments those are provided enclosed in parentheses `( )` after the constraint name.</span></span> <span data-ttu-id="49c03-285">可以指定多個內嵌條件約束附加另一個冒號`:`和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="49c03-285">Multiple inline constraints can be specified by appending another colon `:` and constraint name.</span></span> <span data-ttu-id="49c03-286">條件約束名稱傳遞至`IInlineConstraintResolver`服務來建立的執行個體`IRouteConstraint`URL 處理使用。</span><span class="sxs-lookup"><span data-stu-id="49c03-286">The constraint name is passed to the `IInlineConstraintResolver` service to create an instance of `IRouteConstraint` to use in URL processing.</span></span> <span data-ttu-id="49c03-287">例如，路由範本`blog/{article:minlength(10)}`指定`minlength`與引數的條件約束`10`。</span><span class="sxs-lookup"><span data-stu-id="49c03-287">For example, the route template `blog/{article:minlength(10)}` specifies the `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="49c03-288">詳細描述路由條件約束和 framework 所提供的條件約束的清單，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="49c03-288">For more description route constraints, and a listing of the constraints provided by the framework, see [route-constraint-reference](#route-constraint-reference).</span></span>

<span data-ttu-id="49c03-289">下表示範一些路由範本，以及它們的行為。</span><span class="sxs-lookup"><span data-stu-id="49c03-289">The following table demonstrates some route templates and their behavior.</span></span>

| <span data-ttu-id="49c03-290">路由範本</span><span class="sxs-lookup"><span data-stu-id="49c03-290">Route Template</span></span> | <span data-ttu-id="49c03-291">範例對應 URL</span><span class="sxs-lookup"><span data-stu-id="49c03-291">Example Matching URL</span></span> | <span data-ttu-id="49c03-292">注意</span><span class="sxs-lookup"><span data-stu-id="49c03-292">Notes</span></span> |
| -------- | -------- | ------- |
| <span data-ttu-id="49c03-293">hello</span><span class="sxs-lookup"><span data-stu-id="49c03-293">hello</span></span>  | <span data-ttu-id="49c03-294">/hello</span><span class="sxs-lookup"><span data-stu-id="49c03-294">/hello</span></span>  | <span data-ttu-id="49c03-295">只比對單一路徑`/hello`</span><span class="sxs-lookup"><span data-stu-id="49c03-295">Only matches the single path `/hello`</span></span> |
| <span data-ttu-id="49c03-296">{頁面 = Home}</span><span class="sxs-lookup"><span data-stu-id="49c03-296">{Page=Home}</span></span> | / | <span data-ttu-id="49c03-297">比對，並設定`Page`至`Home`</span><span class="sxs-lookup"><span data-stu-id="49c03-297">Matches and sets `Page` to `Home`</span></span> |
| <span data-ttu-id="49c03-298">{頁面 = Home}</span><span class="sxs-lookup"><span data-stu-id="49c03-298">{Page=Home}</span></span>  | <span data-ttu-id="49c03-299">/ 連絡人</span><span class="sxs-lookup"><span data-stu-id="49c03-299">/Contact</span></span>  | <span data-ttu-id="49c03-300">比對，並設定`Page`至`Contact`</span><span class="sxs-lookup"><span data-stu-id="49c03-300">Matches and sets `Page` to `Contact`</span></span> |
| <span data-ttu-id="49c03-301">{controller} / {action} / {id} 嗎？</span><span class="sxs-lookup"><span data-stu-id="49c03-301">{controller}/{action}/{id?}</span></span> | <span data-ttu-id="49c03-302">/ 產品/清單</span><span class="sxs-lookup"><span data-stu-id="49c03-302">/Products/List</span></span> | <span data-ttu-id="49c03-303">對應至`Products`控制站和`List`動作</span><span class="sxs-lookup"><span data-stu-id="49c03-303">Maps to `Products` controller and `List`  action</span></span> |
| <span data-ttu-id="49c03-304">{controller} / {action} / {id} 嗎？</span><span class="sxs-lookup"><span data-stu-id="49c03-304">{controller}/{action}/{id?}</span></span> | <span data-ttu-id="49c03-305">/ 產品/詳細資料/123</span><span class="sxs-lookup"><span data-stu-id="49c03-305">/Products/Details/123</span></span>  |  <span data-ttu-id="49c03-306">對應至`Products`控制站和`Details`動作。</span><span class="sxs-lookup"><span data-stu-id="49c03-306">Maps to `Products` controller and  `Details` action.</span></span>  <span data-ttu-id="49c03-307">`id`設定為 123</span><span class="sxs-lookup"><span data-stu-id="49c03-307">`id` set to 123</span></span> |
| <span data-ttu-id="49c03-308">{控制器 = Home} / {動作 = 索引} / {id} 嗎？</span><span class="sxs-lookup"><span data-stu-id="49c03-308">{controller=Home}/{action=Index}/{id?}</span></span> | /  |  <span data-ttu-id="49c03-309">對應至`Home`控制站和`Index`方法。`id`會被忽略。</span><span class="sxs-lookup"><span data-stu-id="49c03-309">Maps to `Home` controller and `Index`  method; `id` is ignored.</span></span> |

<span data-ttu-id="49c03-310">使用範本通常是最簡單的方式路由。</span><span class="sxs-lookup"><span data-stu-id="49c03-310">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="49c03-311">條件約束和預設值也可以指定外部的路由範本。</span><span class="sxs-lookup"><span data-stu-id="49c03-311">Constraints and defaults can also be specified outside the route template.</span></span>

<span data-ttu-id="49c03-312">提示： 啟用[記錄](logging.md)若要查看如何在路由實作中，例如內建`Route`，符合要求。</span><span class="sxs-lookup"><span data-stu-id="49c03-312">Tip: Enable [Logging](logging.md) to see how the built in routing implementations, such as `Route`, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="49c03-313">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="49c03-313">Route Constraint Reference</span></span>

<span data-ttu-id="49c03-314">路由條件約束時執行`Route`具有比對傳入 URL 的語法和語彙基元化成路由值的 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="49c03-314">Route constraints execute when a `Route` has matched the syntax of the incoming URL and tokenized the URL path into route values.</span></span> <span data-ttu-id="49c03-315">路由條件約束通常會檢查透過路由範本相關聯的路由值，並進行簡單的 yes/no 決策的相關值可接受。</span><span class="sxs-lookup"><span data-stu-id="49c03-315">Route constraints generally inspect the route value associated via the route template and make a simple yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="49c03-316">某些路由條件約束會使用以外的路由值的資料，請考慮是否可以路由傳送要求。</span><span class="sxs-lookup"><span data-stu-id="49c03-316">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="49c03-317">例如，`HttpMethodRouteConstraint`可以接受或拒絕要求，根據其 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="49c03-317">For example, the `HttpMethodRouteConstraint` can accept or reject a request based on its HTTP verb.</span></span>

>[!WARNING]
> <span data-ttu-id="49c03-318">請避免使用條件約束**輸入驗證**，因為這麼做，意味著該無效的輸入會導致 「 404 （找不到） 而不是 400 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="49c03-318">Avoid using constraints for **input validation**, because doing so means that invalid input will result in a 404 (Not Found) instead of a 400 with an appropriate error message.</span></span> <span data-ttu-id="49c03-319">應該用來路由條件約束**釐清**類似的路由，不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="49c03-319">Route constraints should be used to **disambiguate** between similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="49c03-320">下表示範一些路由條件約束，以及其預期的行為。</span><span class="sxs-lookup"><span data-stu-id="49c03-320">The following table demonstrates some route constraints and their expected behavior.</span></span>

| <span data-ttu-id="49c03-321">條件約束</span><span class="sxs-lookup"><span data-stu-id="49c03-321">constraint</span></span> | <span data-ttu-id="49c03-322">範例</span><span class="sxs-lookup"><span data-stu-id="49c03-322">Example</span></span> | <span data-ttu-id="49c03-323">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="49c03-323">Example Matches</span></span> | <span data-ttu-id="49c03-324">注意</span><span class="sxs-lookup"><span data-stu-id="49c03-324">Notes</span></span> |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="49c03-325">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="49c03-325">`123456789`, `-123456789`</span></span>  | <span data-ttu-id="49c03-326">比對任何整數</span><span class="sxs-lookup"><span data-stu-id="49c03-326">Matches any integer</span></span> |
| `bool`  | `{active:bool}` | <span data-ttu-id="49c03-327">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="49c03-327">`true`, `FALSE`</span></span> | <span data-ttu-id="49c03-328">相符項目`true`或`false`（不區分大小寫）</span><span class="sxs-lookup"><span data-stu-id="49c03-328">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="49c03-329">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="49c03-329">`2016-12-31`, `2016-12-31 7:32pm`</span></span>  | <span data-ttu-id="49c03-330">符合有效`DateTime`值 （在文化特性而異-看見警告）</span><span class="sxs-lookup"><span data-stu-id="49c03-330">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="49c03-331">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="49c03-331">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="49c03-332">符合有效`decimal`值 （在文化特性而異-看見警告）</span><span class="sxs-lookup"><span data-stu-id="49c03-332">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double`  | `{weight:double}` | <span data-ttu-id="49c03-333">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="49c03-333">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="49c03-334">符合有效`double`值 （在文化特性而異-看見警告）</span><span class="sxs-lookup"><span data-stu-id="49c03-334">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float`  | `{weight:float}` | <span data-ttu-id="49c03-335">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="49c03-335">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="49c03-336">符合有效`float`值 （在文化特性而異-看見警告）</span><span class="sxs-lookup"><span data-stu-id="49c03-336">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid`  | `{id:guid}` | <span data-ttu-id="49c03-337">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="49c03-337">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="49c03-338">符合有效`Guid`值</span><span class="sxs-lookup"><span data-stu-id="49c03-338">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="49c03-339">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="49c03-339">`123456789`, `-123456789`</span></span> | <span data-ttu-id="49c03-340">符合有效`long`值</span><span class="sxs-lookup"><span data-stu-id="49c03-340">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="49c03-341">字串必須至少 4 個字元</span><span class="sxs-lookup"><span data-stu-id="49c03-341">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="49c03-342">必須不能超過 8 個字元字串。</span><span class="sxs-lookup"><span data-stu-id="49c03-342">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="49c03-343">字串必須完全 12 個字元長</span><span class="sxs-lookup"><span data-stu-id="49c03-343">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="49c03-344">字串必須至少 8 到 16 個以上字元</span><span class="sxs-lookup"><span data-stu-id="49c03-344">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="49c03-345">整數值必須至少 18</span><span class="sxs-lookup"><span data-stu-id="49c03-345">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` |  `91` | <span data-ttu-id="49c03-346">整數值必須不能超過 120</span><span class="sxs-lookup"><span data-stu-id="49c03-346">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="49c03-347">整數值必須至少為 18，但不是能超過 120</span><span class="sxs-lookup"><span data-stu-id="49c03-347">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="49c03-348">字串必須包含一個或多個字母的字元 (`a`-`z`、 不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="49c03-348">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="49c03-349">字串必須符合規則運算式 （請參閱秘訣定義規則運算式）</span><span class="sxs-lookup"><span data-stu-id="49c03-349">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required`  | `{name:required}` | `Rick` |  <span data-ttu-id="49c03-350">用來強制執行非參數值，也會顯示 URL 產生期間</span><span class="sxs-lookup"><span data-stu-id="49c03-350">Used to enforce that a non-parameter value is present during URL generation</span></span> |

>[!WARNING]
> <span data-ttu-id="49c03-351">請確認 URL 的路由條件約束可以轉換成 CLR 型別 (例如`int`或`DateTime`) 一律使用文化特性而異-它們假設的 URL 是不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="49c03-351">Route constraints that verify the URL can be converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture - they assume the URL is non-localizable.</span></span> <span data-ttu-id="49c03-352">Framework 提供的路由條件約束不會修改中的路由值儲存的值。</span><span class="sxs-lookup"><span data-stu-id="49c03-352">The framework-provided route constraints do not modify the values stored in route values.</span></span> <span data-ttu-id="49c03-353">所有的路由值剖析從 URL 會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="49c03-353">All route values parsed from the URL will be stored as strings.</span></span> <span data-ttu-id="49c03-354">例如， [Float 路由條件約束](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="49c03-354">For example, the [Float route constraint](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) will attempt to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="49c03-355">規則運算式</span><span class="sxs-lookup"><span data-stu-id="49c03-355">Regular expressions</span></span> 

<span data-ttu-id="49c03-356">加入 ASP.NET Core framework`RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant`規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="49c03-356">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="49c03-357">請參閱[RegexOptions 列舉型別](https://msdn.microsoft.com/library/system.text.regularexpressions.regexoptions(v=vs.110).aspx)如需這些成員的說明。</span><span class="sxs-lookup"><span data-stu-id="49c03-357">See [RegexOptions Enumeration](https://msdn.microsoft.com/library/system.text.regularexpressions.regexoptions(v=vs.110).aspx) for a description of these members.</span></span>

<span data-ttu-id="49c03-358">規則運算式會使用分隔符號，類似於所使用的路由和 C# 語言的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="49c03-358">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="49c03-359">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="49c03-359">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="49c03-360">例如，若要使用規則運算式`^\d{3}-\d{2}-\d{4}$`路由，在它需要擁有`\`字元輸入為`\\`C# 原始程式檔來逸出`\`字串逸出字元 (除非使用[逐字字串常值](https://msdn.microsoft.com/library/aa691090(v=vs.71).aspx))。</span><span class="sxs-lookup"><span data-stu-id="49c03-360">For example, to use the regular expression `^\d{3}-\d{2}-\d{4}$` in Routing, it needs to have the `\` characters typed in as `\\` in the C# source file to escape the `\` string escape character (unless using [verbatim string literals](https://msdn.microsoft.com/library/aa691090(v=vs.71).aspx)).</span></span> <span data-ttu-id="49c03-361">`{` ， `}` ，' [' 和 ']' 必須成對使用加以逸出路由參數的分隔符號字元逸出字元。</span><span class="sxs-lookup"><span data-stu-id="49c03-361">The `{` , `}` , '[' and ']' characters need to be escaped by doubling them to escape the Routing parameter delimiter characters.</span></span>  <span data-ttu-id="49c03-362">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="49c03-362">The table below shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="49c03-363">運算式</span><span class="sxs-lookup"><span data-stu-id="49c03-363">Expression</span></span>               | <span data-ttu-id="49c03-364">注意事項</span><span class="sxs-lookup"><span data-stu-id="49c03-364">Note</span></span> |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | <span data-ttu-id="49c03-365">規則運算式</span><span class="sxs-lookup"><span data-stu-id="49c03-365">Regular expression</span></span> |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | <span data-ttu-id="49c03-366">逸出</span><span class="sxs-lookup"><span data-stu-id="49c03-366">Escaped</span></span>  |
| `^[a-z]{2}$` | <span data-ttu-id="49c03-367">規則運算式</span><span class="sxs-lookup"><span data-stu-id="49c03-367">Regular expression</span></span> |
| `^[[a-z]]{{2}}$` | <span data-ttu-id="49c03-368">逸出</span><span class="sxs-lookup"><span data-stu-id="49c03-368">Escaped</span></span>  |

<span data-ttu-id="49c03-369">路由中所用的規則運算式通常會啟動`^`字元 （比對字串的起始位置），且結尾`$`字元 （比對結束之字串的位置）。</span><span class="sxs-lookup"><span data-stu-id="49c03-369">Regular expressions used in routing will often start with the `^` character (match starting position of the string) and end with the `$` character (match ending position of the string).</span></span> <span data-ttu-id="49c03-370">`^`和`$`確保規則運算式比對整個路由參數值的字元。</span><span class="sxs-lookup"><span data-stu-id="49c03-370">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="49c03-371">不含`^`和`$`字元的規則運算式會比對的字串，通常是不是您想要的任何子字串。</span><span class="sxs-lookup"><span data-stu-id="49c03-371">Without the `^` and `$` characters the regular expression will match any sub-string within the string, which is often not what you want.</span></span> <span data-ttu-id="49c03-372">下表顯示一些範例，並說明為何比對或比對會失敗。</span><span class="sxs-lookup"><span data-stu-id="49c03-372">The table below shows some examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="49c03-373">運算式</span><span class="sxs-lookup"><span data-stu-id="49c03-373">Expression</span></span>               | <span data-ttu-id="49c03-374">字串</span><span class="sxs-lookup"><span data-stu-id="49c03-374">String</span></span> | <span data-ttu-id="49c03-375">比對</span><span class="sxs-lookup"><span data-stu-id="49c03-375">Match</span></span> | <span data-ttu-id="49c03-376">註解</span><span class="sxs-lookup"><span data-stu-id="49c03-376">Comment</span></span> |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | <span data-ttu-id="49c03-377">hello</span><span class="sxs-lookup"><span data-stu-id="49c03-377">hello</span></span> | <span data-ttu-id="49c03-378">是</span><span class="sxs-lookup"><span data-stu-id="49c03-378">yes</span></span> | <span data-ttu-id="49c03-379">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="49c03-379">substring matches</span></span> |
| `[a-z]{2}` | <span data-ttu-id="49c03-380">123abc456</span><span class="sxs-lookup"><span data-stu-id="49c03-380">123abc456</span></span> | <span data-ttu-id="49c03-381">是</span><span class="sxs-lookup"><span data-stu-id="49c03-381">yes</span></span> | <span data-ttu-id="49c03-382">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="49c03-382">substring matches</span></span> |
| `[a-z]{2}` | <span data-ttu-id="49c03-383">mz</span><span class="sxs-lookup"><span data-stu-id="49c03-383">mz</span></span> | <span data-ttu-id="49c03-384">是</span><span class="sxs-lookup"><span data-stu-id="49c03-384">yes</span></span> | <span data-ttu-id="49c03-385">符合運算式</span><span class="sxs-lookup"><span data-stu-id="49c03-385">matches expression</span></span> |
| `[a-z]{2}` | <span data-ttu-id="49c03-386">MZ</span><span class="sxs-lookup"><span data-stu-id="49c03-386">MZ</span></span> | <span data-ttu-id="49c03-387">是</span><span class="sxs-lookup"><span data-stu-id="49c03-387">yes</span></span> | <span data-ttu-id="49c03-388">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="49c03-388">not case sensitive</span></span> |
| `^[a-z]{2}$` |  <span data-ttu-id="49c03-389">hello</span><span class="sxs-lookup"><span data-stu-id="49c03-389">hello</span></span> | <span data-ttu-id="49c03-390">no</span><span class="sxs-lookup"><span data-stu-id="49c03-390">no</span></span> | <span data-ttu-id="49c03-391">請參閱`^`和`$`上方</span><span class="sxs-lookup"><span data-stu-id="49c03-391">see `^` and `$` above</span></span> |
| `^[a-z]{2}$` |  <span data-ttu-id="49c03-392">123abc456</span><span class="sxs-lookup"><span data-stu-id="49c03-392">123abc456</span></span> | <span data-ttu-id="49c03-393">no</span><span class="sxs-lookup"><span data-stu-id="49c03-393">no</span></span> | <span data-ttu-id="49c03-394">請參閱`^`和`$`上方</span><span class="sxs-lookup"><span data-stu-id="49c03-394">see `^` and `$` above</span></span> |

<span data-ttu-id="49c03-395">請參閱[.NET Framework 規則運算式](https://msdn.microsoft.com/library/hs600312(v=vs.110).aspx)如需有關規則運算式語法。</span><span class="sxs-lookup"><span data-stu-id="49c03-395">Refer to [.NET Framework Regular Expressions](https://msdn.microsoft.com/library/hs600312(v=vs.110).aspx) for more information on regular expression syntax.</span></span>

<span data-ttu-id="49c03-396">若要限制一組已知的可能值的參數，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="49c03-396">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="49c03-397">例如`{action:regex(^(list|get|create)$)}`只符合`action`路由值`list`， `get`，或`create`。</span><span class="sxs-lookup"><span data-stu-id="49c03-397">For example `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="49c03-398">如果傳遞到此條件約束的字典，字串"^ (清單 | get | 建立) $"相當。</span><span class="sxs-lookup"><span data-stu-id="49c03-398">If passed into the constraints dictionary, the string "^(list|get|create)$" would be equivalent.</span></span> <span data-ttu-id="49c03-399">條件約束的條件約束字典 （沒有內嵌在樣板內） 傳入不符合其中一個已知的條件約束也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="49c03-399">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="49c03-400">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="49c03-400">URL Generation Reference</span></span>

<span data-ttu-id="49c03-401">下列範例示範如何產生連結至指定的路由值字典的路由和`RouteCollection`。</span><span class="sxs-lookup"><span data-stu-id="49c03-401">The example below shows how to generate a link to a route given a dictionary of route values and a `RouteCollection`.</span></span>

<span data-ttu-id="49c03-402">[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]</span><span class="sxs-lookup"><span data-stu-id="49c03-402">[!code-csharp[Main](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]</span></span>

<span data-ttu-id="49c03-403">`VirtualPath`上述範例的結尾產生`/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="49c03-403">The `VirtualPath` generated at the end of the sample above is `/package/create/123`.</span></span>

<span data-ttu-id="49c03-404">第二個參數`VirtualPathContext`建構函式是一組*環境值*。</span><span class="sxs-lookup"><span data-stu-id="49c03-404">The second parameter to the `VirtualPathContext` constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="49c03-405">環境值會提供方便起見，來限制開發人員必須在特定的要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="49c03-405">Ambient values provide convenience by limiting the number of values a developer must specify within a certain request context.</span></span> <span data-ttu-id="49c03-406">目前的路由值，在目前要求的連結產生視為環境的值。</span><span class="sxs-lookup"><span data-stu-id="49c03-406">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="49c03-407">例如，如果您是在 ASP.NET MVC 應用程式在`About`動作`HomeController`，您不需要指定要連結到的控制器的路由值`Index`動作 (環境的值`Home`將使用)。</span><span class="sxs-lookup"><span data-stu-id="49c03-407">For example, in an ASP.NET MVC app if you are in the `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action (the ambient value of `Home` will be used).</span></span>

<span data-ttu-id="49c03-408">環境不符合參數的值會被忽略，並明確地提供的值覆寫時，會從左到右，也會忽略環境的值在 URL 中。</span><span class="sxs-lookup"><span data-stu-id="49c03-408">Ambient values that don't match a parameter are ignored, and ambient values are also ignored when an explicitly-provided value overrides it, going from left to right in the URL.</span></span>

<span data-ttu-id="49c03-409">值提供的但其不符合任何項目會加入至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="49c03-409">Values that are explicitly provided but which don't match anything are added to the query string.</span></span> <span data-ttu-id="49c03-410">下表顯示結果時使用的路由範本`{controller}/{action}/{id?}`。</span><span class="sxs-lookup"><span data-stu-id="49c03-410">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="49c03-411">環境的值</span><span class="sxs-lookup"><span data-stu-id="49c03-411">Ambient Values</span></span> | <span data-ttu-id="49c03-412">明確的值</span><span class="sxs-lookup"><span data-stu-id="49c03-412">Explicit Values</span></span> | <span data-ttu-id="49c03-413">結果</span><span class="sxs-lookup"><span data-stu-id="49c03-413">Result</span></span> |
| -------------   | -------------- | ------ |
| <span data-ttu-id="49c03-414">控制器 = 常用</span><span class="sxs-lookup"><span data-stu-id="49c03-414">controller="Home"</span></span> | <span data-ttu-id="49c03-415">動作 = 「 關於 」</span><span class="sxs-lookup"><span data-stu-id="49c03-415">action="About"</span></span> | `/Home/About` |
| <span data-ttu-id="49c03-416">控制器 = 常用</span><span class="sxs-lookup"><span data-stu-id="49c03-416">controller="Home"</span></span> | <span data-ttu-id="49c03-417">控制器 ="Order"，動作 = 「 關於 」</span><span class="sxs-lookup"><span data-stu-id="49c03-417">controller="Order",action="About"</span></span> | `/Order/About` |
| <span data-ttu-id="49c03-418">控制站 「 家用 」，= color ="Red"</span><span class="sxs-lookup"><span data-stu-id="49c03-418">controller="Home",color="Red"</span></span> | <span data-ttu-id="49c03-419">動作 = 「 關於 」</span><span class="sxs-lookup"><span data-stu-id="49c03-419">action="About"</span></span> | `/Home/About` |
| <span data-ttu-id="49c03-420">控制器 = 常用</span><span class="sxs-lookup"><span data-stu-id="49c03-420">controller="Home"</span></span> | <span data-ttu-id="49c03-421">動作 = [關於]，色彩 ="Red"</span><span class="sxs-lookup"><span data-stu-id="49c03-421">action="About",color="Red"</span></span> | `/Home/About?color=Red`

<span data-ttu-id="49c03-422">如果路由了未對應至參數的預設值，該值會明確提供，它必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="49c03-422">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value.</span></span> <span data-ttu-id="49c03-423">例如: </span><span class="sxs-lookup"><span data-stu-id="49c03-423">For example:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="49c03-424">當未提供對應的控制器與動作值時，連結產生只會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="49c03-424">Link generation would only generate a link for this route when the matching values for controller and action are provided.</span></span>
