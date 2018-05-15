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
ms.openlocfilehash: 2e1257639ec41f657093439c5245b50adbad34dc
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="6c849-103">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="6c849-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="6c849-104">作者：[Ryan Nowak](https://github.com/rynowak)、[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6c849-104">By [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6c849-105">路由功能負責將傳入要求對應至路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="6c849-105">Routing functionality is responsible for mapping an incoming request to a route handler.</span></span> <span data-ttu-id="6c849-106">路由定義於 ASP.NET 應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="6c849-106">Routes are defined in the ASP.NET app and configured when the app starts up.</span></span> <span data-ttu-id="6c849-107">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-107">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="6c849-108">利用 ASP.NET 應用程式中的路由資訊，路由功能也能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-108">Using route information from the ASP.NET app, the routing functionality is also able to generate URLs that map to route handlers.</span></span> <span data-ttu-id="6c849-109">因此，路由可以依據 URL 找到路由處理常式，或依據路由處理常式資訊找到對應至指定路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-109">Therefore, routing can find a route handler based on a URL, or the URL corresponding to a given route handler based on route handler information.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="6c849-110">本文件涵蓋最低層級的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="6c849-110">This document covers the low level ASP.NET Core routing.</span></span> <span data-ttu-id="6c849-111">如需 ASP.NET Core MVC 路由，請參閱[路由至控制器動作](../mvc/controllers/routing.md)</span><span class="sxs-lookup"><span data-stu-id="6c849-111">For ASP.NET Core MVC routing, see [Route to controller actions](../mvc/controllers/routing.md)</span></span>

<span data-ttu-id="6c849-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6c849-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="6c849-113">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="6c849-113">Routing basics</span></span>

<span data-ttu-id="6c849-114">路由 (routing) 功能會使用「路由」(*route*) ([IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter) 的實作) 來：</span><span class="sxs-lookup"><span data-stu-id="6c849-114">Routing uses *routes* (implementations of [IRouter](/dotnet/api/microsoft.aspnetcore.routing.irouter)) to:</span></span>

* <span data-ttu-id="6c849-115">將傳入要求對應至「路由處理常式」</span><span class="sxs-lookup"><span data-stu-id="6c849-115">map incoming requests to *route handlers*</span></span>

* <span data-ttu-id="6c849-116">產生用於回應的 URL</span><span class="sxs-lookup"><span data-stu-id="6c849-116">generate URLs used in responses</span></span>

<span data-ttu-id="6c849-117">一般而言，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="6c849-117">Generally, an app has a single collection of routes.</span></span> <span data-ttu-id="6c849-118">當要求到達時，會依序處理路由集合。</span><span class="sxs-lookup"><span data-stu-id="6c849-118">When a request arrives, the route collection is processed in order.</span></span> <span data-ttu-id="6c849-119">傳入要求會在路由集合的每個可用路由上呼叫 `RouteAsync` 方法，藉以尋找符合所要求 URL 的路由。</span><span class="sxs-lookup"><span data-stu-id="6c849-119">The incoming request looks for a route that matches the request URL by calling the `RouteAsync` method on each available route in the route collection.</span></span> <span data-ttu-id="6c849-120">相反地，回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此不需要硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="6c849-120">By contrast, a response can use routing to generate URLs (for example, for redirection or links) based on route information, and thus avoid having to hard-code URLs, which helps maintainability.</span></span>

<span data-ttu-id="6c849-121">路由會透過 `RouterMiddleware` 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="6c849-121">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the `RouterMiddleware` class.</span></span> <span data-ttu-id="6c849-122">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分。</span><span class="sxs-lookup"><span data-stu-id="6c849-122">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration.</span></span> <span data-ttu-id="6c849-123">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#using-routing-middleware)。</span><span class="sxs-lookup"><span data-stu-id="6c849-123">To learn about using routing as a standalone component, see [Using routing middleware](#using-routing-middleware).</span></span>

<a name="url-matching-ref"></a>

### <a name="url-matching"></a><span data-ttu-id="6c849-124">URL 比對</span><span class="sxs-lookup"><span data-stu-id="6c849-124">URL matching</span></span>

<span data-ttu-id="6c849-125">URL 比對是路由用來將傳入要求分派給「處理常式」的處理序。</span><span class="sxs-lookup"><span data-stu-id="6c849-125">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="6c849-126">這個處理序通常是根據 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="6c849-126">This process is generally based on data in the URL path, but can be extended to consider any data in the request.</span></span> <span data-ttu-id="6c849-127">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="6c849-127">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an application.</span></span>

<span data-ttu-id="6c849-128">傳入要求將進入 `RouterMiddleware`，而後者會依序在每個路由上呼叫 `RouteAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="6c849-128">Incoming requests enter the `RouterMiddleware`, which calls the `RouteAsync` method on each route in sequence.</span></span> <span data-ttu-id="6c849-129">`IRouter` 執行個體可將 `RouteContext.Handler` 設定為非 Null 的 `RequestDelegate`，來選擇是否要「處理」要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-129">The `IRouter` instance chooses whether to *handle* the request by setting the `RouteContext.Handler` to a non-null `RequestDelegate`.</span></span> <span data-ttu-id="6c849-130">如果路由為要求設定了處理常式，則路由處理會停止，且將叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-130">If a route sets a handler for the request, route processing stops and the handler will be invoked to process the request.</span></span> <span data-ttu-id="6c849-131">如果已嘗試所有路由，且未找到要求的處理常式，中介軟體會呼叫下一個，而要求管線中的下一個中介軟體將被叫用。</span><span class="sxs-lookup"><span data-stu-id="6c849-131">If all routes are tried and no handler is found for the request, the middleware calls *next* and the next middleware in the request pipeline is invoked.</span></span>

<span data-ttu-id="6c849-132">`RouteAsync` 的主要輸入是與目前要求建立關聯的 `RouteContext.HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="6c849-132">The primary input to `RouteAsync` is the `RouteContext.HttpContext` associated with the current request.</span></span> <span data-ttu-id="6c849-133">`RouteContext.Handler` 和 `RouteContext.RouteData` 輸出將在路由比對之後設定。</span><span class="sxs-lookup"><span data-stu-id="6c849-133">The `RouteContext.Handler` and `RouteContext.RouteData` are outputs that will be set after a route matches.</span></span>

<span data-ttu-id="6c849-134">`RouteAsync` 期間的比對也會依據到目前為止完成的要求處理，將 `RouteContext.RouteData` 的屬性設定為適當值。</span><span class="sxs-lookup"><span data-stu-id="6c849-134">A match during `RouteAsync` will also set the properties of the `RouteContext.RouteData` to appropriate values based on the request processing done so far.</span></span> <span data-ttu-id="6c849-135">如果路由符合要求，`RouteContext.RouteData` 將包含與「結果」有關的重要狀態資訊。</span><span class="sxs-lookup"><span data-stu-id="6c849-135">If a route matches a request, the `RouteContext.RouteData` will contain important state information about the *result*.</span></span>

<span data-ttu-id="6c849-136">`RouteData.Values` 是路由所產生之「路由值」的字典。</span><span class="sxs-lookup"><span data-stu-id="6c849-136">`RouteData.Values` is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="6c849-137">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="6c849-137">These values are usually determined by tokenizing the URL, and can be used to accept user input, or to make further dispatching decisions inside the application.</span></span>

<span data-ttu-id="6c849-138">`RouteData.DataTokens` 是與相符路由相關之其他資料的屬性包。</span><span class="sxs-lookup"><span data-stu-id="6c849-138">`RouteData.DataTokens`  is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="6c849-139">提供了 `DataTokens` 來支援與每個路由相關聯的狀態資料，因此應用程式可以依據符合哪一個路由來做出決策。</span><span class="sxs-lookup"><span data-stu-id="6c849-139">`DataTokens` are provided to support associating state data with each route so the application can make decisions later based on which route matched.</span></span> <span data-ttu-id="6c849-140">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="6c849-140">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="6c849-141">此外，隱藏在資料語彙基元中的值可以是任何類型，相較於路由值，它必須能夠輕鬆地來回轉換字串。</span><span class="sxs-lookup"><span data-stu-id="6c849-141">Additionally, values stashed in data tokens can be of any type, in contrast to route values, which must be easily convertible to and from strings.</span></span>

<span data-ttu-id="6c849-142">`RouteData.Routers` 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="6c849-142">`RouteData.Routers` is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="6c849-143">路由可以用巢狀方式置於彼此內部，而 `Routers` 屬性則會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="6c849-143">Routes can be nested inside one another, and the `Routers` property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="6c849-144">一般而言，`Routers` 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-144">Generally the first item in `Routers` is the route collection, and should be used for URL generation.</span></span> <span data-ttu-id="6c849-145">`Routers` 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="6c849-145">The last item in `Routers` is the route handler that matched.</span></span>

### <a name="url-generation"></a><span data-ttu-id="6c849-146">URL 產生</span><span class="sxs-lookup"><span data-stu-id="6c849-146">URL generation</span></span>

<span data-ttu-id="6c849-147">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="6c849-147">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="6c849-148">這可讓您在處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="6c849-148">This allows for a logical separation between your handlers and the URLs that access them.</span></span>

<span data-ttu-id="6c849-149">URL 產生遵循類似的反覆執行處理序，但一開始是呼叫路由集合之 `GetVirtualPath` 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="6c849-149">URL generation follows a similar iterative process, but starts with user or framework code calling into the `GetVirtualPath` method of the route collection.</span></span> <span data-ttu-id="6c849-150">然後，每個「路由」將會依序呼叫其 `GetVirtualPath` 方法，直到傳回非 Null 的 `VirtualPathData` 為止。</span><span class="sxs-lookup"><span data-stu-id="6c849-150">Each *route* will then have its `GetVirtualPath` method called in sequence until a non-null `VirtualPathData` is returned.</span></span>

<span data-ttu-id="6c849-151">`GetVirtualPath` 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="6c849-151">The primary inputs to `GetVirtualPath` are:</span></span>

* `VirtualPathContext.HttpContext`

* `VirtualPathContext.Values`

* `VirtualPathContext.AmbientValues`

<span data-ttu-id="6c849-152">路由主要使用 `Values` 和 `AmbientValues` 所提供的路由值，以決定可能產生 URL　的位置以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="6c849-152">Routes primarily use the route values provided by the `Values` and `AmbientValues` to decide where it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="6c849-153">`AmbientValues` 是比對目前要求與路由系統所產生之路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="6c849-153">The `AmbientValues` are the set of route values that were produced from matching the current request with the routing system.</span></span> <span data-ttu-id="6c849-154">相反地，`Values` 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="6c849-154">In contrast, `Values` are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="6c849-155">提供了 `HttpContext` 以防路由需要取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="6c849-155">The `HttpContext` is provided in case a route needs to get services or additional data associated with the current context.</span></span>

<span data-ttu-id="6c849-156">提示：將 `Values` 視為 `AmbientValues` 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="6c849-156">Tip: Think of `Values` as being a set of overrides for the `AmbientValues`.</span></span> <span data-ttu-id="6c849-157">URL 產生會嘗試重複使用來自目前要求的路由值，讓您輕鬆地使用相同的路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-157">URL generation tries to reuse route values from the current request to make it easy to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="6c849-158">`GetVirtualPath` 的輸出是 `VirtualPathData`。</span><span class="sxs-lookup"><span data-stu-id="6c849-158">The output of `GetVirtualPath` is a `VirtualPathData`.</span></span> <span data-ttu-id="6c849-159">`VirtualPathData` 是 `RouteData` 的平行處理；它包含用於輸出 URL 的 `VirtualPath`，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="6c849-159">`VirtualPathData` is a parallel of `RouteData`; it contains the `VirtualPath` for the output URL as well as the some additional properties that should be set by the route.</span></span>

<span data-ttu-id="6c849-160">`VirtualPathData.VirtualPath` 屬性包含路由所產生的「虛擬路徑」。</span><span class="sxs-lookup"><span data-stu-id="6c849-160">The `VirtualPathData.VirtualPath` property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="6c849-161">根據您的需求，您可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="6c849-161">Depending on your needs you may need to process the path further.</span></span> <span data-ttu-id="6c849-162">比方說，如果您想要以 HTML 呈現產生的 URL ，則需要在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="6c849-162">For instance, if you want to render the generated URL in HTML you need to prepend the base path of the application.</span></span>

<span data-ttu-id="6c849-163">`VirtualPathData.Router` 是成功產生 URL 的路由的參考。</span><span class="sxs-lookup"><span data-stu-id="6c849-163">The `VirtualPathData.Router` is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="6c849-164">`VirtualPathData.DataTokens` 屬性是與產生 URL 的路由相關的其他資料的字典。</span><span class="sxs-lookup"><span data-stu-id="6c849-164">The `VirtualPathData.DataTokens` properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="6c849-165">這是 `RouteData.DataTokens` 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="6c849-165">This is the parallel of `RouteData.DataTokens`.</span></span>

### <a name="creating-routes"></a><span data-ttu-id="6c849-166">建立路由</span><span class="sxs-lookup"><span data-stu-id="6c849-166">Creating routes</span></span>

<span data-ttu-id="6c849-167">路由提供 `Route` 類別作為 `IRouter` 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="6c849-167">Routing provides the `Route` class as the standard implementation of `IRouter`.</span></span> <span data-ttu-id="6c849-168">在呼叫 `RouteAsync` 時，`Route` 會使用「路由範本」語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="6c849-168">`Route` uses the *route template* syntax to define patterns that will match against the URL path when `RouteAsync` is called.</span></span> <span data-ttu-id="6c849-169">在呼叫 `GetVirtualPath` 時，`Route` 將使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-169">`Route` will use the same route template to generate a URL when `GetVirtualPath` is called.</span></span>

<span data-ttu-id="6c849-170">大部分的應用程式將藉由呼叫 `MapRoute` 或其中一個 `IRouteBuilder` 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="6c849-170">Most applications will create routes by calling `MapRoute` or one of the similar extension methods defined on `IRouteBuilder`.</span></span> <span data-ttu-id="6c849-171">所有這些方法將建立 `Route` 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="6c849-171">All of these methods will create an instance of `Route` and add it to the route collection.</span></span>

<span data-ttu-id="6c849-172">注意：`MapRoute` 不會採用路由處理常式參數 - 它只會新增 `DefaultHandler` 將處理的路由。</span><span class="sxs-lookup"><span data-stu-id="6c849-172">Note: `MapRoute` doesn't take a route handler parameter - it only adds routes that will be handled by the `DefaultHandler`.</span></span> <span data-ttu-id="6c849-173">由於預設處理常式是 `IRouter`，它可能決定不處理要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-173">Since the default handler is an `IRouter`, it may decide not to handle the request.</span></span> <span data-ttu-id="6c849-174">例如，ASP.NET MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-174">For example, ASP.NET MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="6c849-175">若要深入了解如何路由至 MVC，請參閱[路由至控制器動作](../mvc/controllers/routing.md)。</span><span class="sxs-lookup"><span data-stu-id="6c849-175">To learn more about routing to MVC, see [Route to controller actions](../mvc/controllers/routing.md).</span></span>

<span data-ttu-id="6c849-176">這是典型 ASP.NET MVC 路由定義所使用之 `MapRoute` 呼叫的範例：</span><span class="sxs-lookup"><span data-stu-id="6c849-176">This is an example of a `MapRoute` call used by a typical ASP.NET MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6c849-177">此範本將比對 `/Products/Details/17` 等 URL 路徑，並擷取路由值 `{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="6c849-177">This template will match a URL path like `/Products/Details/17` and extract the route values `{ controller = Products, action = Details, id = 17 }`.</span></span> <span data-ttu-id="6c849-178">路由值是通過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="6c849-178">The route values are determined by splitting the URL path into segments, and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="6c849-179">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="6c849-179">Route parameters are named.</span></span> <span data-ttu-id="6c849-180">它們透過以括弧 `{ }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="6c849-180">They're defined by enclosing the parameter name in braces `{ }`.</span></span>

<span data-ttu-id="6c849-181">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="6c849-181">The template above could also match the URL path `/` and would produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="6c849-182">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="6c849-182">This happens because the `{controller}` and `{action}` route parameters have default values, and the `id` route parameter is optional.</span></span> <span data-ttu-id="6c849-183">路由參數名稱之後緊接著值的等於 `=` 符號定義參數的預設值。</span><span class="sxs-lookup"><span data-stu-id="6c849-183">An equals `=` sign followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="6c849-184">路由參數名稱之後的問號 `?` 則會將參數定義為選擇性。</span><span class="sxs-lookup"><span data-stu-id="6c849-184">A question mark `?` after the route parameter name defines the parameter as optional.</span></span> <span data-ttu-id="6c849-185">在路由相符時，具有預設值的路由參數一定會產生路由值 - 如果沒有任何對應的 URL 路徑區段，選擇性參數將不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="6c849-185">Route parameters with a default value *always* produce a route value when the route matches - optional parameters won't produce a route value if there was no corresponding URL path segment.</span></span>

<span data-ttu-id="6c849-186">如需路由範本功能和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)。</span><span class="sxs-lookup"><span data-stu-id="6c849-186">See [route-template-reference](#route-template-reference) for a thorough description of route template features and syntax.</span></span>

<span data-ttu-id="6c849-187">這個範例包含「路由條件約束」：</span><span class="sxs-lookup"><span data-stu-id="6c849-187">This example includes a *route constraint*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="6c849-188">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="6c849-188">This template will match a URL path like `/Products/Details/17`, but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="6c849-189">路由參數定義 `{id:int}` 定義 `id` 路由參數的「路由條件約束」。</span><span class="sxs-lookup"><span data-stu-id="6c849-189">The route parameter definition `{id:int}` defines a *route constraint* for the `id` route parameter.</span></span> <span data-ttu-id="6c849-190">路由條件約束會實作 `IRouteConstraint`，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="6c849-190">Route constraints implement `IRouteConstraint` and inspect route values to verify them.</span></span> <span data-ttu-id="6c849-191">在此範例中，路由值 `id` 必須可轉換成整數。</span><span class="sxs-lookup"><span data-stu-id="6c849-191">In this example the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="6c849-192">如需架構所提供之路由條件約束的詳細說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="6c849-192">See [route-constraint-reference](#route-constraint-reference) for a more detailed explanation of route constraints that are provided by the framework.</span></span>

<span data-ttu-id="6c849-193">`MapRoute` 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="6c849-193">Additional overloads of `MapRoute` accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="6c849-194">這些額外的 `MapRoute` 參數定義為 `object` 類型。</span><span class="sxs-lookup"><span data-stu-id="6c849-194">These additional parameters of `MapRoute` are defined as type `object`.</span></span> <span data-ttu-id="6c849-195">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="6c849-195">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="6c849-196">下列兩個範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="6c849-196">The following two examples create equivalent routes:</span></span>

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6c849-197">提示：對於簡單的路由而言，定義條件約束和預設值的內嵌語法可能更方便。</span><span class="sxs-lookup"><span data-stu-id="6c849-197">Tip: The inline syntax for defining constraints and defaults can be more convenient for simple routes.</span></span> <span data-ttu-id="6c849-198">不過，內嵌語法不支援資料語彙基元等功能。</span><span class="sxs-lookup"><span data-stu-id="6c849-198">However, there are features such as data tokens which are not supported by inline syntax.</span></span>

<span data-ttu-id="6c849-199">下列範例示範一些更多的功能：</span><span class="sxs-lookup"><span data-stu-id="6c849-199">This example demonstrates a few more features:</span></span>

```csharp
routes.MapRoute(
  name: "blog",
  template: "Blog/{*article}",
  defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="6c849-200">此範本將符合 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="6c849-200">This template will match a URL path like `/Blog/All-About-Routing/Introduction` and will extract the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="6c849-201">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="6c849-201">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="6c849-202">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="6c849-202">Default values can be specified in the route template.</span></span> <span data-ttu-id="6c849-203">`article` 路由參數透過在路由參數名稱之前加上 `*`，定義為「全部擷取」參數。</span><span class="sxs-lookup"><span data-stu-id="6c849-203">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk `*` before the route parameter name.</span></span> <span data-ttu-id="6c849-204">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6c849-204">Catch-all route parameters capture the remainder of the URL path, and can also match the empty string.</span></span>

<span data-ttu-id="6c849-205">這個範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="6c849-205">This example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="6c849-206">此範本將符合 `/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="6c849-206">This template will match a URL path like `/Products/5` and will extract the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

<a name="id1"></a>

### <a name="url-generation"></a><span data-ttu-id="6c849-208">URL 產生</span><span class="sxs-lookup"><span data-stu-id="6c849-208">URL generation</span></span>

<span data-ttu-id="6c849-209">`Route` 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-209">The `Route` class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="6c849-210">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="6c849-210">This is logically the reverse process of matching the URL path.</span></span>

<span data-ttu-id="6c849-211">提示：若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="6c849-211">Tip: To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="6c849-212">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="6c849-212">What values would be produced?</span></span> <span data-ttu-id="6c849-213">這與 URL 產生在 `Route` 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="6c849-213">This is the rough equivalent of how URL generation works in the `Route` class.</span></span>

<span data-ttu-id="6c849-214">這個範例使用基本的 ASP.NET MVC 樣式路由：</span><span class="sxs-lookup"><span data-stu-id="6c849-214">This example uses a basic ASP.NET MVC style route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="6c849-215">路由值為 `{ controller = Products, action = List }` 時，此路由將產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="6c849-215">With the route values `{ controller = Products, action = List }`, this route will generate the URL `/Products/List`.</span></span> <span data-ttu-id="6c849-216">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="6c849-216">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="6c849-217">由於 `id` 是選擇性路由參數，因此它沒有值也不會有問題。</span><span class="sxs-lookup"><span data-stu-id="6c849-217">Since `id` is an optional route parameter, it's no problem that it doesn't have a value.</span></span>

<span data-ttu-id="6c849-218">路由值為 `{ controller = Home, action = Index }` 時，此路由將產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="6c849-218">With the route values `{ controller = Home, action = Index }`, this route will generate the URL `/`.</span></span> <span data-ttu-id="6c849-219">所提供的路由值符合預設值，因此可以放心地省略這些值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="6c849-219">The route values that were provided match the default values so the segments corresponding to those values can be safely omitted.</span></span> <span data-ttu-id="6c849-220">請注意，這兩個產生的 URL 會使用此路由定義反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="6c849-220">Note that both URLs generated would round-trip with this route definition and produce the same route values that were used to generate the URL.</span></span>

<span data-ttu-id="6c849-221">提示：使用 ASP.NET MVC 的應用程式應該使用 `UrlHelper` 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="6c849-221">Tip: An app using ASP.NET MVC should use `UrlHelper` to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="6c849-222">如需 URL 產生處理序的詳細資料，請參閱 [URL 產生參考](#url-generation-reference)。</span><span class="sxs-lookup"><span data-stu-id="6c849-222">For more details about the URL generation process, see [url-generation-reference](#url-generation-reference).</span></span>

## <a name="using-routing-middleware"></a><span data-ttu-id="6c849-223">設定路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="6c849-223">Using Routing Middleware</span></span>

<span data-ttu-id="6c849-224">新增 NuGet 套件 "Microsoft.AspNetCore.Routing"。</span><span class="sxs-lookup"><span data-stu-id="6c849-224">Add the NuGet package "Microsoft.AspNetCore.Routing".</span></span>

<span data-ttu-id="6c849-225">在 *Startup.cs* 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="6c849-225">Add routing to the service container in *Startup.cs*:</span></span>

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?highlight=3&start=11&end=14)]

<span data-ttu-id="6c849-226">路由必須設定在 `Startup` 類別的 `Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="6c849-226">Routes must be configured in the `Configure` method in the `Startup` class.</span></span> <span data-ttu-id="6c849-227">下列範例使用這些 API：</span><span class="sxs-lookup"><span data-stu-id="6c849-227">The sample below uses these APIs:</span></span>

* `RouteBuilder`
* `Build`
* <span data-ttu-id="6c849-228">`MapGet`  僅符合 HTTP GET 要求</span><span class="sxs-lookup"><span data-stu-id="6c849-228">`MapGet`  Matches only HTTP GET requests</span></span>
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

<span data-ttu-id="6c849-229">下表顯示使用給定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="6c849-229">The table below shows the responses with the given URIs.</span></span>

| <span data-ttu-id="6c849-230">URI</span><span class="sxs-lookup"><span data-stu-id="6c849-230">URI</span></span> | <span data-ttu-id="6c849-231">回應</span><span class="sxs-lookup"><span data-stu-id="6c849-231">Response</span></span>  |
| ------- | -------- |
| <span data-ttu-id="6c849-232">/package/create/3</span><span class="sxs-lookup"><span data-stu-id="6c849-232">/package/create/3</span></span>  | <span data-ttu-id="6c849-233">Hello!</span><span class="sxs-lookup"><span data-stu-id="6c849-233">Hello!</span></span> <span data-ttu-id="6c849-234">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="6c849-234">Route values: [operation, create], [id, 3]</span></span> |
| <span data-ttu-id="6c849-235">/package/track/-3</span><span class="sxs-lookup"><span data-stu-id="6c849-235">/package/track/-3</span></span>  | <span data-ttu-id="6c849-236">Hello!</span><span class="sxs-lookup"><span data-stu-id="6c849-236">Hello!</span></span> <span data-ttu-id="6c849-237">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="6c849-237">Route values: [operation, track], [id, -3]</span></span> |
| <span data-ttu-id="6c849-238">/package/track/-3/</span><span class="sxs-lookup"><span data-stu-id="6c849-238">/package/track/-3/</span></span> | <span data-ttu-id="6c849-239">Hello!</span><span class="sxs-lookup"><span data-stu-id="6c849-239">Hello!</span></span> <span data-ttu-id="6c849-240">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="6c849-240">Route values: [operation, track], [id, -3]</span></span>  |
| <span data-ttu-id="6c849-241">/package/track/</span><span class="sxs-lookup"><span data-stu-id="6c849-241">/package/track/</span></span> | <span data-ttu-id="6c849-242">\<正常執行，沒有相符項目></span><span class="sxs-lookup"><span data-stu-id="6c849-242">\<Fall through, no match></span></span> |
| <span data-ttu-id="6c849-243">GET /hello/Joe</span><span class="sxs-lookup"><span data-stu-id="6c849-243">GET /hello/Joe</span></span> | <span data-ttu-id="6c849-244">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="6c849-244">Hi, Joe!</span></span> |
| <span data-ttu-id="6c849-245">POST /hello/Joe</span><span class="sxs-lookup"><span data-stu-id="6c849-245">POST /hello/Joe</span></span> | <span data-ttu-id="6c849-246">\<正常執行，僅符合 HTTP GET></span><span class="sxs-lookup"><span data-stu-id="6c849-246">\<Fall through, matches HTTP GET only></span></span> |
| <span data-ttu-id="6c849-247">GET /hello/Joe/Smith</span><span class="sxs-lookup"><span data-stu-id="6c849-247">GET /hello/Joe/Smith</span></span> | <span data-ttu-id="6c849-248">\<正常執行，沒有相符項目></span><span class="sxs-lookup"><span data-stu-id="6c849-248">\<Fall through, no match></span></span> |

<span data-ttu-id="6c849-249">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 `app.UseRouter`。</span><span class="sxs-lookup"><span data-stu-id="6c849-249">If you are configuring a single route, call `app.UseRouter` passing in an `IRouter` instance.</span></span> <span data-ttu-id="6c849-250">您不需要呼叫 `RouteBuilder`。</span><span class="sxs-lookup"><span data-stu-id="6c849-250">You won't need to call `RouteBuilder`.</span></span>

<span data-ttu-id="6c849-251">架構會提供一組擴充方法來建立路由，例如：</span><span class="sxs-lookup"><span data-stu-id="6c849-251">The framework provides a set of extension methods for creating routes such as:</span></span>

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

<span data-ttu-id="6c849-252">其中一些方法 (例如 `MapGet`) 需要提供 `RequestDelegate`。</span><span class="sxs-lookup"><span data-stu-id="6c849-252">Some of these methods such as `MapGet` require a `RequestDelegate` to be provided.</span></span> <span data-ttu-id="6c849-253">路由相符時，`RequestDelegate` 將會作為「路由處理常式」。</span><span class="sxs-lookup"><span data-stu-id="6c849-253">The `RequestDelegate` will be used as the *route handler* when the route matches.</span></span> <span data-ttu-id="6c849-254">此系列中的其他方法允許設定將作為路由處理常式的中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="6c849-254">Other methods in this family allow configuring a middleware pipeline which will be used as the route handler.</span></span> <span data-ttu-id="6c849-255">如果 *Map* 方法不接受處理常式 (例如 `MapRoute`)，則它將使用 `DefaultHandler`。</span><span class="sxs-lookup"><span data-stu-id="6c849-255">If the *Map* method doesn't accept a handler, such as `MapRoute`, then it will use the `DefaultHandler`.</span></span>

<span data-ttu-id="6c849-256">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="6c849-256">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="6c849-257">例如，請參閱 [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) 和 [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180)。</span><span class="sxs-lookup"><span data-stu-id="6c849-257">For example, see [MapGet](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L85-L88) and [MapVerb](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/RequestDelegateRouteBuilderExtensions.cs#L156-L180).</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="6c849-258">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="6c849-258">Route Template Reference</span></span>

<span data-ttu-id="6c849-259">大括弧 (`{ }`) 內的語彙基元定義路由相符時將繫結的「路由參數」。</span><span class="sxs-lookup"><span data-stu-id="6c849-259">Tokens within curly braces (`{ }`) define *route parameters* which will be bound if the route is matched.</span></span> <span data-ttu-id="6c849-260">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="6c849-260">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="6c849-261">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 和 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="6c849-261">For example `{controller=Home}{action=Index}` wouldn't be a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="6c849-262">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="6c849-262">These route parameters must have a name, and may have additional attributes specified.</span></span>

<span data-ttu-id="6c849-263">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="6c849-263">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="6c849-264">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="6c849-264">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="6c849-265">若要與常值路由參數分隔符號 `{` 或 `}` 相符，請重複字元 (`{{` 或 `}}`) 來將它逸出。</span><span class="sxs-lookup"><span data-stu-id="6c849-265">To match the literal route parameter delimiter `{` or  `}`, escape it by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="6c849-266">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="6c849-266">URL patterns that attempt to capture a filename with an optional file extension have additional considerations.</span></span> <span data-ttu-id="6c849-267">例如，使用範本 `files/{filename}.{ext?}` - 當 `filename` 和 `ext` 都存在時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="6c849-267">For example, using the template `files/{filename}.{ext?}` - When both `filename` and `ext` exist, both values will be populated.</span></span> <span data-ttu-id="6c849-268">如果 URL 中只有 `filename` 存在，由於結尾的句點 `.` 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="6c849-268">If only `filename` exists in the URL, the route matches because the trailing period `.` is  optional.</span></span> <span data-ttu-id="6c849-269">下列 URL 會符合此路由：</span><span class="sxs-lookup"><span data-stu-id="6c849-269">The following URLs would match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile.`
* `/files/myFile`

<span data-ttu-id="6c849-270">您可以使用 `*` 字元作為路由參數的前置詞以繫結至 URI 的其餘部分 - 這稱為「全部擷取」參數。</span><span class="sxs-lookup"><span data-stu-id="6c849-270">You can use the `*` character as a prefix to a route parameter to bind to the rest of the URI - this is called a *catch-all* parameter.</span></span> <span data-ttu-id="6c849-271">例如，`blog/{*slug}` 會符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="6c849-271">For example, `blog/{*slug}` would match any URI that started with `/blog` and had any value following it (which would be assigned to the `slug` route value).</span></span> <span data-ttu-id="6c849-272">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="6c849-272">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="6c849-273">路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以 `=` 分隔。</span><span class="sxs-lookup"><span data-stu-id="6c849-273">Route parameters may have *default values*, designated by specifying the default after the parameter name, separated by an `=`.</span></span> <span data-ttu-id="6c849-274">例如，`{controller=Home}` 會定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="6c849-274">For example, `{controller=Home}` would define `Home` as the default value for `controller`.</span></span> <span data-ttu-id="6c849-275">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="6c849-275">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="6c849-276">除了預設值之外，路由參數也可以是選擇性參數 (指定方法是在參數名稱結尾附加 `?`，如 `id?` 所示)。</span><span class="sxs-lookup"><span data-stu-id="6c849-276">In addition to default values, route parameters may be optional (specified by appending a `?` to the end of the parameter name, as in `id?`).</span></span> <span data-ttu-id="6c849-277">選擇性與「具有預設值」之間的差異在於，具有預設值的路由參數一定會產生值；選擇性參數只有在提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="6c849-277">The difference between optional and "has default" is that a route parameter with a default value always produces a value; an optional parameter has a value only when one is provided.</span></span>

<span data-ttu-id="6c849-278">路由參數也可能會有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="6c849-278">Route parameters may also have constraints, which must match the route value bound from the URL.</span></span> <span data-ttu-id="6c849-279">在路由參數名稱之後新增分號 `:` 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。</span><span class="sxs-lookup"><span data-stu-id="6c849-279">Adding a colon `:` and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="6c849-280">如果條件約束需要引數，您可以在條件約束名稱後面以括弧 `( )` 括住方式來提供這些引數。</span><span class="sxs-lookup"><span data-stu-id="6c849-280">If the constraint requires arguments those are provided enclosed in parentheses `( )` after the constraint name.</span></span> <span data-ttu-id="6c849-281">指定多個內嵌條件約束的方法是附加另一個冒號 `:` 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="6c849-281">Multiple inline constraints can be specified by appending another colon `:` and constraint name.</span></span> <span data-ttu-id="6c849-282">條件約束名稱會傳遞至 `IInlineConstraintResolver` 服務來建立 `IRouteConstraint` 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="6c849-282">The constraint name is passed to the `IInlineConstraintResolver` service to create an instance of `IRouteConstraint` to use in URL processing.</span></span> <span data-ttu-id="6c849-283">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="6c849-283">For example, the route template `blog/{article:minlength(10)}` specifies the `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="6c849-284">如需路由條件約束詳細描述和架構所提供之條件約束的清單，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="6c849-284">For more description route constraints, and a listing of the constraints provided by the framework, see [route-constraint-reference](#route-constraint-reference).</span></span>

<span data-ttu-id="6c849-285">下表示範一些路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="6c849-285">The following table demonstrates some route templates and their behavior.</span></span>

| <span data-ttu-id="6c849-286">路由範本</span><span class="sxs-lookup"><span data-stu-id="6c849-286">Route Template</span></span> | <span data-ttu-id="6c849-287">範例比對 URL</span><span class="sxs-lookup"><span data-stu-id="6c849-287">Example Matching URL</span></span> | <span data-ttu-id="6c849-288">注意</span><span class="sxs-lookup"><span data-stu-id="6c849-288">Notes</span></span> |
| -------- | -------- | ------- |
| <span data-ttu-id="6c849-289">hello</span><span class="sxs-lookup"><span data-stu-id="6c849-289">hello</span></span>  | <span data-ttu-id="6c849-290">/hello</span><span class="sxs-lookup"><span data-stu-id="6c849-290">/hello</span></span>  | <span data-ttu-id="6c849-291">只比對單一路徑 `/hello`</span><span class="sxs-lookup"><span data-stu-id="6c849-291">Only matches the single path `/hello`</span></span> |
| <span data-ttu-id="6c849-292">{Page=Home}</span><span class="sxs-lookup"><span data-stu-id="6c849-292">{Page=Home}</span></span> | / | <span data-ttu-id="6c849-293">比對並將 `Page` 設定為 `Home`</span><span class="sxs-lookup"><span data-stu-id="6c849-293">Matches and sets `Page` to `Home`</span></span> |
| <span data-ttu-id="6c849-294">{Page=Home}</span><span class="sxs-lookup"><span data-stu-id="6c849-294">{Page=Home}</span></span>  | <span data-ttu-id="6c849-295">/Contact</span><span class="sxs-lookup"><span data-stu-id="6c849-295">/Contact</span></span>  | <span data-ttu-id="6c849-296">比對並將 `Page` 設定為 `Contact`</span><span class="sxs-lookup"><span data-stu-id="6c849-296">Matches and sets `Page` to `Contact`</span></span> |
| <span data-ttu-id="6c849-297">{controller}/{action}/{id?}</span><span class="sxs-lookup"><span data-stu-id="6c849-297">{controller}/{action}/{id?}</span></span> | <span data-ttu-id="6c849-298">/Products/List</span><span class="sxs-lookup"><span data-stu-id="6c849-298">/Products/List</span></span> | <span data-ttu-id="6c849-299">對應至 `Products` 控制器和 `List` 動作</span><span class="sxs-lookup"><span data-stu-id="6c849-299">Maps to `Products` controller and `List`  action</span></span> |
| <span data-ttu-id="6c849-300">{controller}/{action}/{id?}</span><span class="sxs-lookup"><span data-stu-id="6c849-300">{controller}/{action}/{id?}</span></span> | <span data-ttu-id="6c849-301">/Products/Details/123</span><span class="sxs-lookup"><span data-stu-id="6c849-301">/Products/Details/123</span></span>  |  <span data-ttu-id="6c849-302">對應至 `Products` 控制器和 `Details` 動作。</span><span class="sxs-lookup"><span data-stu-id="6c849-302">Maps to `Products` controller and  `Details` action.</span></span>  <span data-ttu-id="6c849-303">將 `id` 設定為 123</span><span class="sxs-lookup"><span data-stu-id="6c849-303">`id` set to 123</span></span> |
| <span data-ttu-id="6c849-304">{controller=Home}/{action=Index}/{id?}</span><span class="sxs-lookup"><span data-stu-id="6c849-304">{controller=Home}/{action=Index}/{id?}</span></span> | /  |  <span data-ttu-id="6c849-305">對應至 `Home` 控制器和 `Index` 方法；會忽略 `id`。</span><span class="sxs-lookup"><span data-stu-id="6c849-305">Maps to `Home` controller and `Index`  method; `id` is ignored.</span></span> |

<span data-ttu-id="6c849-306">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="6c849-306">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="6c849-307">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="6c849-307">Constraints and defaults can also be specified outside the route template.</span></span>

<span data-ttu-id="6c849-308">提示：啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 `Route`) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-308">Tip: Enable [Logging](xref:fundamentals/logging/index) to see how the built in routing implementations, such as `Route`, match requests.</span></span>

## <a name="route-constraint-reference"></a><span data-ttu-id="6c849-309">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="6c849-309">Route Constraint Reference</span></span>

<span data-ttu-id="6c849-310">路由條件約束的執行時機是 `Route` 已符合傳入 URL 的語法，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="6c849-310">Route constraints execute when a `Route` has matched the syntax of the incoming URL and tokenized the URL path into route values.</span></span> <span data-ttu-id="6c849-311">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出簡單的是/否決策。</span><span class="sxs-lookup"><span data-stu-id="6c849-311">Route constraints generally inspect the route value associated via the route template and make a simple yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="6c849-312">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-312">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="6c849-313">例如，`HttpMethodRouteConstraint` 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="6c849-313">For example, the `HttpMethodRouteConstraint` can accept or reject a request based on its HTTP verb.</span></span>

>[!WARNING]
> <span data-ttu-id="6c849-314">請避免針對**輸入驗證**使用條件約束，因為這麼做表示無效的輸入會導致產生 404 (找不到)，而不是 400 與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="6c849-314">Avoid using constraints for **input validation**, because doing so means that invalid input will result in a 404 (Not Found) instead of a 400 with an appropriate error message.</span></span> <span data-ttu-id="6c849-315">路由條件約束應該用來**釐清**類似的路由，不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="6c849-315">Route constraints should be used to **disambiguate** between similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="6c849-316">下表示範一些路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="6c849-316">The following table demonstrates some route constraints and their expected behavior.</span></span>

| <span data-ttu-id="6c849-317">條件約束</span><span class="sxs-lookup"><span data-stu-id="6c849-317">constraint</span></span> | <span data-ttu-id="6c849-318">範例</span><span class="sxs-lookup"><span data-stu-id="6c849-318">Example</span></span> | <span data-ttu-id="6c849-319">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="6c849-319">Example Matches</span></span> | <span data-ttu-id="6c849-320">注意</span><span class="sxs-lookup"><span data-stu-id="6c849-320">Notes</span></span> |
| --------   | ------- | ------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="6c849-321">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="6c849-321">`123456789`, `-123456789`</span></span>  | <span data-ttu-id="6c849-322">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="6c849-322">Matches any integer</span></span> |
| `bool`  | `{active:bool}` | <span data-ttu-id="6c849-323">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="6c849-323">`true`, `FALSE`</span></span> | <span data-ttu-id="6c849-324">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="6c849-324">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="6c849-325">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="6c849-325">`2016-12-31`, `2016-12-31 7:32pm`</span></span>  | <span data-ttu-id="6c849-326">符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="6c849-326">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="6c849-327">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="6c849-327">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="6c849-328">符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="6c849-328">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double`  | `{weight:double}` | <span data-ttu-id="6c849-329">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="6c849-329">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="6c849-330">符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="6c849-330">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float`  | `{weight:float}` | <span data-ttu-id="6c849-331">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="6c849-331">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="6c849-332">符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="6c849-332">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid`  | `{id:guid}` | <span data-ttu-id="6c849-333">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="6c849-333">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="6c849-334">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="6c849-334">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="6c849-335">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="6c849-335">`123456789`, `-123456789`</span></span> | <span data-ttu-id="6c849-336">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="6c849-336">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="6c849-337">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="6c849-337">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="6c849-338">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="6c849-338">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="6c849-339">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="6c849-339">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="6c849-340">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="6c849-340">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="6c849-341">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="6c849-341">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` |  `91` | <span data-ttu-id="6c849-342">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="6c849-342">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="6c849-343">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="6c849-343">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="6c849-344">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="6c849-344">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="6c849-345">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="6c849-345">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required`  | `{name:required}` | `Rick` |  <span data-ttu-id="6c849-346">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="6c849-346">Used to enforce that a non-parameter value is present during URL generation</span></span> |

>[!WARNING]
> <span data-ttu-id="6c849-347">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性 - 它們假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="6c849-347">Route constraints that verify the URL can be converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture - they assume the URL is non-localizable.</span></span> <span data-ttu-id="6c849-348">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="6c849-348">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="6c849-349">所有從 URL 剖析而來的路由值將儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="6c849-349">All route values parsed from the URL will be stored as strings.</span></span> <span data-ttu-id="6c849-350">例如，[Float 路由條件約束](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60)會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="6c849-350">For example, the [Float route constraint](https://github.com/aspnet/Routing/blob/1.0.0/src/Microsoft.AspNetCore.Routing/Constraints/FloatRouteConstraint.cs#L44-L60) will attempt to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="6c849-351">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6c849-351">Regular expressions</span></span> 

<span data-ttu-id="6c849-352">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="6c849-352">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="6c849-353">如需這些成員的描述，請參閱 [RegexOptions 列舉](/dotnet/api/system.text.regularexpressions.regexoptions)。</span><span class="sxs-lookup"><span data-stu-id="6c849-353">See [RegexOptions Enumeration](/dotnet/api/system.text.regularexpressions.regexoptions) for a description of these members.</span></span>

<span data-ttu-id="6c849-354">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="6c849-354">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="6c849-355">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="6c849-355">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="6c849-356">例如，若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，它必須在 C# 原始程式檔中將 `\` 字元輸入為 `\\`，以逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string)。</span><span class="sxs-lookup"><span data-stu-id="6c849-356">For example, to use the regular expression `^\d{3}-\d{2}-\d{4}$` in Routing, it needs to have the `\` characters typed in as `\\` in the C# source file to escape the `\` string escape character (unless using [verbatim string literals](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/string).</span></span> <span data-ttu-id="6c849-357">`{`、`}`、'[' 和 ']' 字元必須加以逸出，方法是成對使用以逸出路由參數的分隔符號字元。</span><span class="sxs-lookup"><span data-stu-id="6c849-357">The `{` , `}` , '[' and ']' characters need to be escaped by doubling them to escape the Routing parameter delimiter characters.</span></span>  <span data-ttu-id="6c849-358">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="6c849-358">The table below shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="6c849-359">運算式</span><span class="sxs-lookup"><span data-stu-id="6c849-359">Expression</span></span>               | <span data-ttu-id="6c849-360">注意事項</span><span class="sxs-lookup"><span data-stu-id="6c849-360">Note</span></span> |
| ----------------- | ------------ | 
| `^\d{3}-\d{2}-\d{4}$` | <span data-ttu-id="6c849-361">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6c849-361">Regular expression</span></span> |
| `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` | <span data-ttu-id="6c849-362">已逸出</span><span class="sxs-lookup"><span data-stu-id="6c849-362">Escaped</span></span>  |
| `^[a-z]{2}$` | <span data-ttu-id="6c849-363">規則運算式</span><span class="sxs-lookup"><span data-stu-id="6c849-363">Regular expression</span></span> |
| `^[[a-z]]{{2}}$` | <span data-ttu-id="6c849-364">已逸出</span><span class="sxs-lookup"><span data-stu-id="6c849-364">Escaped</span></span>  |

<span data-ttu-id="6c849-365">路由中所使用的規則運算式通常以 `^` 字元 (符合字串的起始位置) 開頭，並以 `$` 字元 (符合字串的結束位置) 結尾。</span><span class="sxs-lookup"><span data-stu-id="6c849-365">Regular expressions used in routing will often start with the `^` character (match starting position of the string) and end with the `$` character (match ending position of the string).</span></span> <span data-ttu-id="6c849-366">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="6c849-366">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="6c849-367">若不使用 `^` 和 `$` 字元，規則運算式將符合字串內的任何子字串，這通常是不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="6c849-367">Without the `^` and `$` characters the regular expression will match any sub-string within the string, which is often not what you want.</span></span> <span data-ttu-id="6c849-368">下表顯示一些範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="6c849-368">The table below shows some examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="6c849-369">運算式</span><span class="sxs-lookup"><span data-stu-id="6c849-369">Expression</span></span>               | <span data-ttu-id="6c849-370">String</span><span class="sxs-lookup"><span data-stu-id="6c849-370">String</span></span> | <span data-ttu-id="6c849-371">比對</span><span class="sxs-lookup"><span data-stu-id="6c849-371">Match</span></span> | <span data-ttu-id="6c849-372">註解</span><span class="sxs-lookup"><span data-stu-id="6c849-372">Comment</span></span> |
| ----------------- | ------------ |  ------------ |  ------------ | 
| `[a-z]{2}` | <span data-ttu-id="6c849-373">hello</span><span class="sxs-lookup"><span data-stu-id="6c849-373">hello</span></span> | <span data-ttu-id="6c849-374">是</span><span class="sxs-lookup"><span data-stu-id="6c849-374">yes</span></span> | <span data-ttu-id="6c849-375">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="6c849-375">substring matches</span></span> |
| `[a-z]{2}` | <span data-ttu-id="6c849-376">123abc456</span><span class="sxs-lookup"><span data-stu-id="6c849-376">123abc456</span></span> | <span data-ttu-id="6c849-377">是</span><span class="sxs-lookup"><span data-stu-id="6c849-377">yes</span></span> | <span data-ttu-id="6c849-378">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="6c849-378">substring matches</span></span> |
| `[a-z]{2}` | <span data-ttu-id="6c849-379">mz</span><span class="sxs-lookup"><span data-stu-id="6c849-379">mz</span></span> | <span data-ttu-id="6c849-380">是</span><span class="sxs-lookup"><span data-stu-id="6c849-380">yes</span></span> | <span data-ttu-id="6c849-381">符合運算式</span><span class="sxs-lookup"><span data-stu-id="6c849-381">matches expression</span></span> |
| `[a-z]{2}` | <span data-ttu-id="6c849-382">MZ</span><span class="sxs-lookup"><span data-stu-id="6c849-382">MZ</span></span> | <span data-ttu-id="6c849-383">是</span><span class="sxs-lookup"><span data-stu-id="6c849-383">yes</span></span> | <span data-ttu-id="6c849-384">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="6c849-384">not case sensitive</span></span> |
| `^[a-z]{2}$` |  <span data-ttu-id="6c849-385">hello</span><span class="sxs-lookup"><span data-stu-id="6c849-385">hello</span></span> | <span data-ttu-id="6c849-386">否</span><span class="sxs-lookup"><span data-stu-id="6c849-386">no</span></span> | <span data-ttu-id="6c849-387">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="6c849-387">see `^` and `$` above</span></span> |
| `^[a-z]{2}$` |  <span data-ttu-id="6c849-388">123abc456</span><span class="sxs-lookup"><span data-stu-id="6c849-388">123abc456</span></span> | <span data-ttu-id="6c849-389">否</span><span class="sxs-lookup"><span data-stu-id="6c849-389">no</span></span> | <span data-ttu-id="6c849-390">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="6c849-390">see `^` and `$` above</span></span> |

<span data-ttu-id="6c849-391">如需規則運算式的詳細資訊，請參閱 [.NET Framework 規則運算式](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="6c849-391">Refer to [.NET Framework Regular Expressions](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) for more information on regular expression syntax.</span></span>

<span data-ttu-id="6c849-392">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6c849-392">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="6c849-393">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="6c849-393">For example `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="6c849-394">如果已傳入條件約束字典，字串 "^(list|get|create)$" 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="6c849-394">If passed into the constraints dictionary, the string "^(list|get|create)$" would be equivalent.</span></span> <span data-ttu-id="6c849-395">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="6c849-395">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="6c849-396">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="6c849-396">URL Generation Reference</span></span>

<span data-ttu-id="6c849-397">下列範例示範如何在給定路由值字典和 `RouteCollection` 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="6c849-397">The example below shows how to generate a link to a route given a dictionary of route values and a `RouteCollection`.</span></span>

[!code-csharp[](../fundamentals/routing/sample/RoutingSample/Startup.cs?range=45-59)]

<span data-ttu-id="6c849-398">在上述範例的結尾產生的 `VirtualPath` 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="6c849-398">The `VirtualPath` generated at the end of the sample above is `/package/create/123`.</span></span>

<span data-ttu-id="6c849-399">`VirtualPathContext` 建構函式的第二個參數是「環境值」的集合。</span><span class="sxs-lookup"><span data-stu-id="6c849-399">The second parameter to the `VirtualPathContext` constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="6c849-400">環境值為方便起見，會限制開發人員必須在特定要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="6c849-400">Ambient values provide convenience by limiting the number of values a developer must specify within a certain request context.</span></span> <span data-ttu-id="6c849-401">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="6c849-401">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="6c849-402">例如，在 ASP.NET MVC 應用程式中，如果您處於 `HomeController` 的 `About` 動作，則不需要指定控制器路由值以連結到 `Index` 動作 (將使用 `Home` 的環境值)。</span><span class="sxs-lookup"><span data-stu-id="6c849-402">For example, in an ASP.NET MVC app if you are in the `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action (the ambient value of `Home` will be used).</span></span>

<span data-ttu-id="6c849-403">不符合參數的環境值會被忽略，當明確提供的值在 URL 中從左到右進行覆寫時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="6c849-403">Ambient values that don't match a parameter are ignored, and ambient values are also ignored when an explicitly-provided value overrides it, going from left to right in the URL.</span></span>

<span data-ttu-id="6c849-404">明確提供但不符合任何項目的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="6c849-404">Values that are explicitly provided but which don't match anything are added to the query string.</span></span> <span data-ttu-id="6c849-405">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="6c849-405">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="6c849-406">環境值</span><span class="sxs-lookup"><span data-stu-id="6c849-406">Ambient Values</span></span> | <span data-ttu-id="6c849-407">明確值</span><span class="sxs-lookup"><span data-stu-id="6c849-407">Explicit Values</span></span> | <span data-ttu-id="6c849-408">結果</span><span class="sxs-lookup"><span data-stu-id="6c849-408">Result</span></span> |
| -------------   | -------------- | ------ |
| <span data-ttu-id="6c849-409">controller="Home"</span><span class="sxs-lookup"><span data-stu-id="6c849-409">controller="Home"</span></span> | <span data-ttu-id="6c849-410">action="About"</span><span class="sxs-lookup"><span data-stu-id="6c849-410">action="About"</span></span> | `/Home/About` |
| <span data-ttu-id="6c849-411">controller="Home"</span><span class="sxs-lookup"><span data-stu-id="6c849-411">controller="Home"</span></span> | <span data-ttu-id="6c849-412">controller="Order",action="About"</span><span class="sxs-lookup"><span data-stu-id="6c849-412">controller="Order",action="About"</span></span> | `/Order/About` |
| <span data-ttu-id="6c849-413">controller="Home",color="Red"</span><span class="sxs-lookup"><span data-stu-id="6c849-413">controller="Home",color="Red"</span></span> | <span data-ttu-id="6c849-414">action="About"</span><span class="sxs-lookup"><span data-stu-id="6c849-414">action="About"</span></span> | `/Home/About` |
| <span data-ttu-id="6c849-415">controller="Home"</span><span class="sxs-lookup"><span data-stu-id="6c849-415">controller="Home"</span></span> | <span data-ttu-id="6c849-416">action="About",color="Red"</span><span class="sxs-lookup"><span data-stu-id="6c849-416">action="About",color="Red"</span></span> | `/Home/About?color=Red`

<span data-ttu-id="6c849-417">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值。</span><span class="sxs-lookup"><span data-stu-id="6c849-417">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value.</span></span> <span data-ttu-id="6c849-418">例如: </span><span class="sxs-lookup"><span data-stu-id="6c849-418">For example:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
  defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="6c849-419">提供了控制器與動作的相符值時，連結產生只會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="6c849-419">Link generation would only generate a link for this route when the matching values for controller and action are provided.</span></span>
