---
title: ASP.NET Core 中的路由
author: rick-anderson
description: 探索 ASP.NET Core 路由功能如何負責將要求 URI 對應至端點選取器，並將傳入要求分派給端點。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/24/2019
uid: fundamentals/routing
ms.openlocfilehash: 8b4da4e1e262ec82225413d0338b3492d0b5e152
ms.sourcegitcommit: 032113208bb55ecfb2faeb6d3e9ea44eea827950
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2019
ms.locfileid: "73190512"
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="2b03a-103">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="2b03a-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="2b03a-104">作者：[Ryan Nowak](https://github.com/rynowak)、[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2b03a-104">By [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2b03a-105">路由會負責將要求 Uri 對應至端點，並將傳入要求分派至這些端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-105">Routing is responsible for mapping request URIs to endpoints and dispatching incoming requests to those endpoints.</span></span> <span data-ttu-id="2b03a-106">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-106">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="2b03a-107">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-107">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="2b03a-108">使用來自應用程式的路由資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="2b03a-108">Using route information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b03a-109">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-109">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="2b03a-110">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-110">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="2b03a-111">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-111">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="2b03a-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b03a-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="2b03a-113">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="2b03a-113">Routing basics</span></span>

<span data-ttu-id="2b03a-114">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="2b03a-114">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="2b03a-115">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="2b03a-115">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="2b03a-116">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-116">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="2b03a-117">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-117">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="2b03a-118">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-118">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="2b03a-119">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-119">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="2b03a-120">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-120">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="2b03a-121">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="2b03a-121">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="2b03a-122">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="2b03a-122">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="2b03a-123">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-123">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="2b03a-124">如需詳細資訊，請參閱 <xref:razor-pages/index> 與 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-124">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="2b03a-125">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-125">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="2b03a-126">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-126">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="2b03a-127">路由功能使用「端點」(`Endpoint`) 來表示應用程式中的邏輯端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-127">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="2b03a-128">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-128">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="2b03a-129">中繼資料是用來根據附加至每個端點的原則和設定來執行跨領域考慮。</span><span class="sxs-lookup"><span data-stu-id="2b03a-129">The metadata is used to implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="2b03a-130">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2b03a-130">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="2b03a-131">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-131">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="2b03a-132">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="2b03a-132">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="2b03a-133"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-133"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="2b03a-134">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="2b03a-134">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="2b03a-135">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-135">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="2b03a-136">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-136">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="2b03a-137">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-137">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="2b03a-138">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-138">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="2b03a-139">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="2b03a-139">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="2b03a-140">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-140">The Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="2b03a-141">如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-141">Where the Link Generator API isn't available via DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="2b03a-142">端點連結僅限於 MVC/Razor Pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="2b03a-142">Endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="2b03a-143">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-143">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

<span data-ttu-id="2b03a-144">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="2b03a-144">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="2b03a-145">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-145">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="2b03a-146">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-146">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="2b03a-147">URL 比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-147">URL matching</span></span>

<span data-ttu-id="2b03a-148">URL 比對是路由用來將傳入要求分派給「端點」的處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-148">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="2b03a-149">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="2b03a-149">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="2b03a-150">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="2b03a-150">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="2b03a-151">當路由中介軟體執行時，它會將端點 (`Endpoint`) 和路由值設定為 <xref:Microsoft.AspNetCore.Http.HttpContext> 上的功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-151">When a Routing Middleware executes, it sets an endpoint (`Endpoint`) and route values to a feature on the <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span> <span data-ttu-id="2b03a-152">針對目前的要求：</span><span class="sxs-lookup"><span data-stu-id="2b03a-152">For the current request:</span></span>

* <span data-ttu-id="2b03a-153">呼叫 `HttpContext.GetEndpoint` 可取得端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-153">Calling `HttpContext.GetEndpoint` gets the endpoint.</span></span>
* <span data-ttu-id="2b03a-154">`HttpRequest.RouteValues` 會取得路由值的集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-154">`HttpRequest.RouteValues` gets the collection of route values.</span></span>

<span data-ttu-id="2b03a-155">在路由中介軟體之後執行的中介軟體可以看到端點，並採取動作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-155">Middleware running after the Routing Middleware can see the endpoint and take action.</span></span> <span data-ttu-id="2b03a-156">例如，授權中介軟體可以針對授權原則，查閱端點的中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-156">For example, an Authorization Middleware can interrogate the endpoint's metadata collection for an authorization policy.</span></span> <span data-ttu-id="2b03a-157">執行要求處理管線中的所有中介軟體之後，會叫用所選端點的委派。</span><span class="sxs-lookup"><span data-stu-id="2b03a-157">After all of the middleware in the request processing pipeline is executed, the selected endpoint's delegate is invoked.</span></span>

<span data-ttu-id="2b03a-158">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-158">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="2b03a-159">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-159">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="2b03a-160">執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-160">When the endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="2b03a-161">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-161">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="2b03a-162">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-162">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="2b03a-163">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="2b03a-163">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="2b03a-164">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-164"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="2b03a-165">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-165">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="2b03a-166">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="2b03a-166">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="2b03a-167">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="2b03a-167">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="2b03a-168">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="2b03a-168">Routes can be nested inside of one another.</span></span> <span data-ttu-id="2b03a-169"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-169">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="2b03a-170">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-170">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="2b03a-171"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-171">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a><span data-ttu-id="2b03a-172">使用 LinkGenerator 產生 URL</span><span class="sxs-lookup"><span data-stu-id="2b03a-172">URL generation with LinkGenerator</span></span>

<span data-ttu-id="2b03a-173">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-173">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="2b03a-174">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-174">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="2b03a-175">端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-175">Endpoint routing includes the Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>).</span></span> <span data-ttu-id="2b03a-176"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 是可從 DI 擷取的單一服務。</span><span class="sxs-lookup"><span data-stu-id="2b03a-176"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is a singleton service that can be retrieved from DI.</span></span> <span data-ttu-id="2b03a-177">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="2b03a-177">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="2b03a-178">MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-178">MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="2b03a-179">連結產生器背後支援的概念為「位址」和「位址配置」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-179">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="2b03a-180">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-180">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="2b03a-181">例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-181">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="2b03a-182">連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="2b03a-182">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="2b03a-183">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-183">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="2b03a-184">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="2b03a-184">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="2b03a-185">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-185">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="2b03a-186">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-186">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="2b03a-187">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-187">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="2b03a-188">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="2b03a-188">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="2b03a-189">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="2b03a-189"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="2b03a-190">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="2b03a-190">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="2b03a-191">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="2b03a-191">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="2b03a-192">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-192">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="2b03a-193">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="2b03a-193">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="2b03a-194"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-194">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="2b03a-195">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="2b03a-195">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="2b03a-196">擴充方法</span><span class="sxs-lookup"><span data-stu-id="2b03a-196">Extension Method</span></span> | <span data-ttu-id="2b03a-197">描述</span><span class="sxs-lookup"><span data-stu-id="2b03a-197">Description</span></span> |
| ---------------- | ----------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="2b03a-198">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-198">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="2b03a-199">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-199">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="2b03a-200">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="2b03a-200">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="2b03a-201">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="2b03a-201">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="2b03a-202">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="2b03a-202">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="2b03a-203">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-203">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="2b03a-204">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2b03a-204">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="2b03a-205">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-205">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="2b03a-206">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-206">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="2b03a-207">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="2b03a-207">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="2b03a-208">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="2b03a-208">Differences from earlier versions of routing</span></span>

<span data-ttu-id="2b03a-209">端點路由和之前的路由版本之間存在一些差異，早于 ASP.NET Core 2.2：</span><span class="sxs-lookup"><span data-stu-id="2b03a-209">A few differences exist between endpoint routing and versions of routing earlier than in ASP.NET Core 2.2:</span></span>

* <span data-ttu-id="2b03a-210">端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。</span><span class="sxs-lookup"><span data-stu-id="2b03a-210">The endpoint routing system doesn't support <xref:Microsoft.AspNetCore.Routing.IRouter>-based extensibility, including inheriting from <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>

* <span data-ttu-id="2b03a-211">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-211">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="2b03a-212">請使用 2.1 [相容性版本](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) 以繼續使用相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-212">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="2b03a-213">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-213">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="2b03a-214">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="2b03a-214">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="2b03a-215">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="2b03a-215">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="2b03a-216">使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="2b03a-216">With <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="2b03a-217">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-217">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="2b03a-218">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-218">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="2b03a-219">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-219">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="2b03a-220">當嘗試連結至不存在的控制器/動作或頁面時，MVC/Razor Pages 所使用的連結產生與傳統路由的行為不同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-220">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="2b03a-221">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="2b03a-221">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="2b03a-222">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="2b03a-222">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="2b03a-223">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="2b03a-223">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="2b03a-224">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-224">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="2b03a-225">不過，如果動作不存在，則端點路由會產生空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-225">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="2b03a-226">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="2b03a-226">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="2b03a-227">連結產生「環境值失效演算法」在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-227">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="2b03a-228">「環境值失效」是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-228">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="2b03a-229">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="2b03a-229">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="2b03a-230">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-230">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="2b03a-231">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2b03a-231">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="2b03a-232">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="2b03a-232">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="2b03a-233">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-233">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="2b03a-234">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="2b03a-234">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="2b03a-235">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="2b03a-235">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="2b03a-236">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="2b03a-236">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="2b03a-237">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-237">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="2b03a-238">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="2b03a-238">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="2b03a-239">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-239">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="2b03a-240">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-240">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="2b03a-241">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-241">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="2b03a-242">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-242">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="2b03a-243">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-243">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="2b03a-244">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="2b03a-244">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="2b03a-245">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-245">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="2b03a-246">路由</span><span class="sxs-lookup"><span data-stu-id="2b03a-246">Route</span></span>              | <span data-ttu-id="2b03a-247">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="2b03a-247">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | <span data-ttu-id="2b03a-248">`/search/admin%2Fproducts` (斜線會經過編碼)</span><span class="sxs-lookup"><span data-stu-id="2b03a-248">`/search/admin%2Fproducts` (the forward slash is encoded)</span></span>             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a><span data-ttu-id="2b03a-249">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="2b03a-249">Middleware example</span></span>

<span data-ttu-id="2b03a-250">在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-250">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create link to an action method that lists store products.</span></span> <span data-ttu-id="2b03a-251">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-251">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a><span data-ttu-id="2b03a-252">建立路由</span><span class="sxs-lookup"><span data-stu-id="2b03a-252">Create routes</span></span>

<span data-ttu-id="2b03a-253">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-253">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="2b03a-254">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-254">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="2b03a-255"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-255"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="2b03a-256"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-256"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="2b03a-257">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-257">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="2b03a-258">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="2b03a-258">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="2b03a-259">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-259">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="2b03a-260">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-260">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="2b03a-261">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-261">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="2b03a-262">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="2b03a-262">Route parameters are named.</span></span> <span data-ttu-id="2b03a-263">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="2b03a-263">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="2b03a-264">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-264">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="2b03a-265">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-265">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="2b03a-266">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-266">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="2b03a-267">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-267">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="2b03a-268">在路由相符時，具有預設值的路由參數一定會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-268">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="2b03a-269">如果沒有任何對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-269">Optional parameters don't produce a route value if there was no corresponding URL path segment.</span></span> <span data-ttu-id="2b03a-270">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-270">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="2b03a-271">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="2b03a-271">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="2b03a-272">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-272">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="2b03a-273">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="2b03a-273">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="2b03a-274">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-274">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="2b03a-275">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-275">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="2b03a-276"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-276">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="2b03a-277">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="2b03a-277">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="2b03a-278">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-278">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> <span data-ttu-id="2b03a-279">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="2b03a-279">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="2b03a-280">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-280">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="2b03a-281">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="2b03a-281">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="2b03a-282">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-282">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="2b03a-283">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-283">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="2b03a-284">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-284">Default values can be specified in the route template.</span></span> <span data-ttu-id="2b03a-285">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="2b03a-285">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="2b03a-286">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-286">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="2b03a-287">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="2b03a-287">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="2b03a-288">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-288">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="2b03a-290">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="2b03a-290">Route class URL generation</span></span>

<span data-ttu-id="2b03a-291"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-291">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="2b03a-292">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-292">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-293">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-293">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="2b03a-294">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="2b03a-294">What values would be produced?</span></span> <span data-ttu-id="2b03a-295">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-295">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="2b03a-296">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-296">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="2b03a-297">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-297">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="2b03a-298">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-298">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="2b03a-299">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-299">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="2b03a-300">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-300">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="2b03a-301">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="2b03a-301">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="2b03a-302">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-302">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="2b03a-303">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-303">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="2b03a-304">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-304">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="2b03a-305">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="2b03a-305">Use Routing Middleware</span></span>

<span data-ttu-id="2b03a-306">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-306">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="2b03a-307">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="2b03a-307">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="2b03a-308">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="2b03a-308">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="2b03a-309">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="2b03a-309">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="2b03a-310"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; 僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-310"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="2b03a-311">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="2b03a-311">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="2b03a-312">URI</span><span class="sxs-lookup"><span data-stu-id="2b03a-312">URI</span></span>                    | <span data-ttu-id="2b03a-313">回應</span><span class="sxs-lookup"><span data-stu-id="2b03a-313">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="2b03a-314">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-314">Hello!</span></span> <span data-ttu-id="2b03a-315">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-315">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="2b03a-316">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-316">Hello!</span></span> <span data-ttu-id="2b03a-317">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-317">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="2b03a-318">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-318">Hello!</span></span> <span data-ttu-id="2b03a-319">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-319">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="2b03a-320">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-320">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="2b03a-321">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="2b03a-321">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="2b03a-322">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="2b03a-322">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="2b03a-323">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-323">The request falls through, no match.</span></span>              |

<span data-ttu-id="2b03a-324">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="2b03a-324">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

<span data-ttu-id="2b03a-325">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="2b03a-325">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="2b03a-326">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-326">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="2b03a-327">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-327">Route template reference</span></span>

<span data-ttu-id="2b03a-328">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-328">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="2b03a-329">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-329">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="2b03a-330">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-330">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="2b03a-331">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-331">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="2b03a-332">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="2b03a-332">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="2b03a-333">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="2b03a-333">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="2b03a-334">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-334">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="2b03a-335">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="2b03a-335">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="2b03a-336">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-336">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="2b03a-337">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-337">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="2b03a-338">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="2b03a-338">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="2b03a-339">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-339">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="2b03a-340">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="2b03a-340">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="2b03a-341">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-341">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="2b03a-342">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-342">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="2b03a-343">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-343">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="2b03a-344">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-344">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="2b03a-345">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-345">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="2b03a-346">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="2b03a-346">Note the escaped forward slash.</span></span> <span data-ttu-id="2b03a-347">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="2b03a-347">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="2b03a-348">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-348">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="2b03a-349">路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-349">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="2b03a-350">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-350">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="2b03a-351">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-351">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="2b03a-352">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="2b03a-352">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="2b03a-353">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-353">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="2b03a-354">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-354">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="2b03a-355">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-355">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="2b03a-356">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="2b03a-356">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="2b03a-357">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="2b03a-357">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="2b03a-358">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-358">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="2b03a-359">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="2b03a-359">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="2b03a-360">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-360">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="2b03a-361">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-361">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="2b03a-362">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-362">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="2b03a-363">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="2b03a-363">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="2b03a-364">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-364">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="2b03a-365">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-365">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="2b03a-366">路由範本</span><span class="sxs-lookup"><span data-stu-id="2b03a-366">Route Template</span></span>                           | <span data-ttu-id="2b03a-367">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="2b03a-367">Example Matching URI</span></span>    | <span data-ttu-id="2b03a-368">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b03a-368">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="2b03a-369">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-369">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="2b03a-370">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-370">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="2b03a-371">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-371">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="2b03a-372">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-372">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="2b03a-373">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-373">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="2b03a-374">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-374">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="2b03a-375">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-375">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="2b03a-376">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-376">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-377">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-377">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="2b03a-378">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="2b03a-378">Reserved routing names</span></span>

<span data-ttu-id="2b03a-379">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="2b03a-379">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="2b03a-380">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-380">Route constraint reference</span></span>

<span data-ttu-id="2b03a-381">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="2b03a-381">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="2b03a-382">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-382">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="2b03a-383">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-383">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="2b03a-384">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-384">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="2b03a-385">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="2b03a-385">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="2b03a-386">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="2b03a-386">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="2b03a-387">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」回應，而不是「400 - 錯誤要求」與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="2b03a-387">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="2b03a-388">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="2b03a-388">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="2b03a-389">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-389">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="2b03a-390">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="2b03a-390">constraint</span></span> | <span data-ttu-id="2b03a-391">範例</span><span class="sxs-lookup"><span data-stu-id="2b03a-391">Example</span></span> | <span data-ttu-id="2b03a-392">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-392">Example Matches</span></span> | <span data-ttu-id="2b03a-393">備註</span><span class="sxs-lookup"><span data-stu-id="2b03a-393">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="2b03a-394">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="2b03a-394">`123456789`, `-123456789`</span></span> | <span data-ttu-id="2b03a-395">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="2b03a-395">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="2b03a-396">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="2b03a-396">`true`, `FALSE`</span></span> | <span data-ttu-id="2b03a-397">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="2b03a-397">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="2b03a-398">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="2b03a-398">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="2b03a-399">符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-399">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="2b03a-400">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="2b03a-400">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="2b03a-401">符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-401">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double` | `{weight:double}` | <span data-ttu-id="2b03a-402">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="2b03a-402">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="2b03a-403">符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-403">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float` | `{weight:float}` | <span data-ttu-id="2b03a-404">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="2b03a-404">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="2b03a-405">符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-405">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid` | `{id:guid}` | <span data-ttu-id="2b03a-406">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="2b03a-406">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="2b03a-407">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="2b03a-407">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="2b03a-408">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="2b03a-408">`123456789`, `-123456789`</span></span> | <span data-ttu-id="2b03a-409">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="2b03a-409">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="2b03a-410">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-410">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="2b03a-411">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-411">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="2b03a-412">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-412">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="2b03a-413">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-413">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="2b03a-414">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="2b03a-414">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="2b03a-415">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="2b03a-415">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="2b03a-416">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="2b03a-416">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="2b03a-417">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="2b03a-417">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="2b03a-418">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="2b03a-418">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="2b03a-419">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="2b03a-419">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="2b03a-420">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-420">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="2b03a-421">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="2b03a-421">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="2b03a-422">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-422">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="2b03a-423">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2b03a-423">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="2b03a-424">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-424">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="2b03a-425">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-425">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="2b03a-426">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-426">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="2b03a-427">規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-427">Regular expressions</span></span>

<span data-ttu-id="2b03a-428">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-428">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="2b03a-429">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-429">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="2b03a-430">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-430">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="2b03a-431">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-431">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="2b03a-432">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="2b03a-432">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="2b03a-433">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="2b03a-433">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="2b03a-434">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="2b03a-434">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="2b03a-435">規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-435">Regular Expression</span></span>    | <span data-ttu-id="2b03a-436">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-436">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="2b03a-437">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-437">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="2b03a-438">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="2b03a-438">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="2b03a-439">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-439">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="2b03a-440">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="2b03a-440">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="2b03a-441">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="2b03a-441">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="2b03a-442">運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-442">Expression</span></span>   | <span data-ttu-id="2b03a-443">String</span><span class="sxs-lookup"><span data-stu-id="2b03a-443">String</span></span>    | <span data-ttu-id="2b03a-444">比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-444">Match</span></span> | <span data-ttu-id="2b03a-445">註解</span><span class="sxs-lookup"><span data-stu-id="2b03a-445">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-446">hello</span><span class="sxs-lookup"><span data-stu-id="2b03a-446">hello</span></span>     | <span data-ttu-id="2b03a-447">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-447">Yes</span></span>   | <span data-ttu-id="2b03a-448">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-448">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-449">123abc456</span><span class="sxs-lookup"><span data-stu-id="2b03a-449">123abc456</span></span> | <span data-ttu-id="2b03a-450">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-450">Yes</span></span>   | <span data-ttu-id="2b03a-451">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-451">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-452">mz</span><span class="sxs-lookup"><span data-stu-id="2b03a-452">mz</span></span>        | <span data-ttu-id="2b03a-453">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-453">Yes</span></span>   | <span data-ttu-id="2b03a-454">符合運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-454">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-455">MZ</span><span class="sxs-lookup"><span data-stu-id="2b03a-455">MZ</span></span>        | <span data-ttu-id="2b03a-456">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-456">Yes</span></span>   | <span data-ttu-id="2b03a-457">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="2b03a-457">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="2b03a-458">hello</span><span class="sxs-lookup"><span data-stu-id="2b03a-458">hello</span></span>     | <span data-ttu-id="2b03a-459">否</span><span class="sxs-lookup"><span data-stu-id="2b03a-459">No</span></span>    | <span data-ttu-id="2b03a-460">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="2b03a-460">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="2b03a-461">123abc456</span><span class="sxs-lookup"><span data-stu-id="2b03a-461">123abc456</span></span> | <span data-ttu-id="2b03a-462">否</span><span class="sxs-lookup"><span data-stu-id="2b03a-462">No</span></span>    | <span data-ttu-id="2b03a-463">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="2b03a-463">See `^` and `$` above</span></span> |

<span data-ttu-id="2b03a-464">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-464">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="2b03a-465">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-465">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="2b03a-466">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="2b03a-466">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="2b03a-467">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-467">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="2b03a-468">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-468">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="2b03a-469">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="2b03a-469">Custom Route Constraints</span></span>

<span data-ttu-id="2b03a-470">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="2b03a-470">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="2b03a-471"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-471">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="2b03a-472">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="2b03a-472">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="2b03a-473"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-473">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="2b03a-474">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="2b03a-474">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="2b03a-475">例如:</span><span class="sxs-lookup"><span data-stu-id="2b03a-475">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="2b03a-476">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-476">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="2b03a-477">例如:</span><span class="sxs-lookup"><span data-stu-id="2b03a-477">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a><span data-ttu-id="2b03a-478">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-478">Parameter transformer reference</span></span>

<span data-ttu-id="2b03a-479">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="2b03a-479">Parameter transformers:</span></span>

* <span data-ttu-id="2b03a-480">會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="2b03a-480">Execute when generating a link for a <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>
* <span data-ttu-id="2b03a-481">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-481">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="2b03a-482">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="2b03a-482">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="2b03a-483">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-483">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="2b03a-484">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-484">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="2b03a-485">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-485">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="2b03a-486">若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：</span><span class="sxs-lookup"><span data-stu-id="2b03a-486">To use a parameter transformer in a route pattern, configure it first using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

<span data-ttu-id="2b03a-487">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-487">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="2b03a-488">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-488">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="2b03a-489">使用上述的路由，動作 `SubscriptionManagementController.GetAll()` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="2b03a-489">With the preceding route, the action `SubscriptionManagementController.GetAll()` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="2b03a-490">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-490">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="2b03a-491">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-491">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="2b03a-492">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="2b03a-492">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="2b03a-493">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-493">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="2b03a-494">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-494">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="2b03a-495">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-495">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="2b03a-496">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-496">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="2b03a-497">Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-497">Razor Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="2b03a-498">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="2b03a-498">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="2b03a-499">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="2b03a-499">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="2b03a-500">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-500">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="2b03a-501">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-501">URL generation reference</span></span>

<span data-ttu-id="2b03a-502">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-502">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/3.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="2b03a-503">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-503">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="2b03a-504">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-504">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="2b03a-505">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-505">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="2b03a-506"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」的集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-506">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="2b03a-507">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-507">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="2b03a-508">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-508">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="2b03a-509">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-509">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="2b03a-510">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="2b03a-510">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="2b03a-511">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-511">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="2b03a-512">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="2b03a-512">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="2b03a-513">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-513">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="2b03a-514">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="2b03a-514">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="2b03a-515">環境值</span><span class="sxs-lookup"><span data-stu-id="2b03a-515">Ambient Values</span></span>                     | <span data-ttu-id="2b03a-516">明確值</span><span class="sxs-lookup"><span data-stu-id="2b03a-516">Explicit Values</span></span>                        | <span data-ttu-id="2b03a-517">結果</span><span class="sxs-lookup"><span data-stu-id="2b03a-517">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="2b03a-518">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-518">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-519">action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-519">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="2b03a-520">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-520">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-521">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-521">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="2b03a-522">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="2b03a-522">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="2b03a-523">action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-523">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="2b03a-524">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-524">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-525">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="2b03a-525">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="2b03a-526">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="2b03a-526">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="2b03a-527">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-527">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="2b03a-528">複雜區段</span><span class="sxs-lookup"><span data-stu-id="2b03a-528">Complex segments</span></span>

<span data-ttu-id="2b03a-529">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-529">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="2b03a-530">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="2b03a-530">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="2b03a-531">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="2b03a-531">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

## <a name="configuring-endpoint-metadata"></a><span data-ttu-id="2b03a-532">設定端點中繼資料</span><span class="sxs-lookup"><span data-stu-id="2b03a-532">Configuring endpoint metadata</span></span>

<span data-ttu-id="2b03a-533">下列連結提供設定端點中繼資料的相關資訊：</span><span class="sxs-lookup"><span data-stu-id="2b03a-533">The following links provide information on configuring endpoint metadata:</span></span>

* [<span data-ttu-id="2b03a-534">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="2b03a-534">Enable Cors with endpoint routing</span></span>](xref:security/cors#enable-cors-with-endpoint-routing)
* <span data-ttu-id="2b03a-535">使用自訂 `[MinimumAgeAuthorize]` 屬性的[IAuthorizationPolicyProvider 範例](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)</span><span class="sxs-lookup"><span data-stu-id="2b03a-535">[IAuthorizationPolicyProvider sample](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider) using a custom `[MinimumAgeAuthorize]` attribute</span></span>
* <span data-ttu-id="2b03a-536">[使用 [授權] 屬性測試驗證](xref:security/authentication/identity#test-identity)</span><span class="sxs-lookup"><span data-stu-id="2b03a-536">[Test authentication with the [Authorize] attribute](xref:security/authentication/identity#test-identity)</span></span>
* <xref:Microsoft.AspNetCore.Builder.AuthorizationEndpointConventionBuilderExtensions.RequireAuthorization*>
* <span data-ttu-id="2b03a-537">[選取具有 [授權] 屬性的配置](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span><span class="sxs-lookup"><span data-stu-id="2b03a-537">[Selecting the scheme with the [Authorize] attribute](xref:security/authorization/limitingidentitybyscheme#selecting-the-scheme-with-the-authorize-attribute)</span></span>
* <span data-ttu-id="2b03a-538">[使用 [授權] 屬性套用原則](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span><span class="sxs-lookup"><span data-stu-id="2b03a-538">[Applying policies using the [Authorize] attribute](xref:security/authorization/policies#applying-policies-to-mvc-controllers)</span></span>
* <xref:security/authorization/roles>

<a name="hostmatch"></a>

## <a name="host-matching-in-routes-with-requirehost"></a><span data-ttu-id="2b03a-539">搭配 RequireHost 的路由中的主機比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-539">Host matching in routes with RequireHost</span></span>

<span data-ttu-id="2b03a-540">`RequireHost` 將條件約束套用至需要指定之主機的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-540">`RequireHost` applies a constraint to the route which requires the specified host.</span></span> <span data-ttu-id="2b03a-541">`RequireHost` 或 `[Host]` 參數可以是：</span><span class="sxs-lookup"><span data-stu-id="2b03a-541">The `RequireHost` or `[Host]` parameter can be:</span></span>

* <span data-ttu-id="2b03a-542">主機： `www.domain.com` （符合任何埠的 `www.domain.com`）</span><span class="sxs-lookup"><span data-stu-id="2b03a-542">Host: `www.domain.com` (matches `www.domain.com` with any port)</span></span>
* <span data-ttu-id="2b03a-543">具有萬用字元的主機： `*.domain.com` （符合任何埠上的 `www.domain.com`、`subdomain.domain.com`或 `www.subdomain.domain.com`）</span><span class="sxs-lookup"><span data-stu-id="2b03a-543">Host with wildcard: `*.domain.com` (matches `www.domain.com`, `subdomain.domain.com`, or `www.subdomain.domain.com` on any port)</span></span>
* <span data-ttu-id="2b03a-544">埠： `*:5000` （符合任何主機的通訊埠5000）</span><span class="sxs-lookup"><span data-stu-id="2b03a-544">Port: `*:5000` (matches port 5000 with any host)</span></span>
* <span data-ttu-id="2b03a-545">主機和埠： `www.domain.com:5000`、`*.domain.com:5000` （符合主機和埠）</span><span class="sxs-lookup"><span data-stu-id="2b03a-545">Host and port: `www.domain.com:5000`, `*.domain.com:5000` (matches host and port)</span></span>

<span data-ttu-id="2b03a-546">您可以使用 `RequireHost` 或 `[Host]`來指定多個參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-546">Multiple parameters can be specified using `RequireHost` or `[Host]`.</span></span> <span data-ttu-id="2b03a-547">條件約束會符合任何參數的有效主機。</span><span class="sxs-lookup"><span data-stu-id="2b03a-547">The constraint will match hosts valid for any of the parameters.</span></span> <span data-ttu-id="2b03a-548">例如，`[Host("domain.com", "*.domain.com")]` 會符合 `domain.com`、`www.domain.com`或 `subdomain.domain.com`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-548">For example, `[Host("domain.com", "*.domain.com")]` will match `domain.com`, `www.domain.com`, or `subdomain.domain.com`.</span></span>

<span data-ttu-id="2b03a-549">下列程式碼會使用 `RequireHost` 來要求路由上指定的主機：</span><span class="sxs-lookup"><span data-stu-id="2b03a-549">The following code uses `RequireHost` to require the specified host on the route:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi Contoso!"))
            .RequireHost("contoso.com");
        endpoints.MapGet("/", context => context.Response.WriteAsync("Hi AdventureWorks!"))
            .RequireHost("adventure-works.com");
        endpoints.MapHealthChecks("/healthz").RequireHost("*:8080");
    });
}
```

<span data-ttu-id="2b03a-550">下列程式碼會使用 `[Host]` 屬性來要求控制器上的指定主機：</span><span class="sxs-lookup"><span data-stu-id="2b03a-550">The following code uses the `[Host]` attribute to require the specified host on the controller:</span></span>

```csharp
[Host("contoso.com", "adventure-works.com")]
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    [Host("example.com:8080")]
    public IActionResult Privacy()
    {
        return View();
    }

}
```

<span data-ttu-id="2b03a-551">當 `[Host]` 屬性同時套用至控制器和動作方法時：</span><span class="sxs-lookup"><span data-stu-id="2b03a-551">When the `[Host]` attribute is applied to both the controller and action method:</span></span>

* <span data-ttu-id="2b03a-552">會使用動作上的屬性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-552">The attribute on the action is used.</span></span>
* <span data-ttu-id="2b03a-553">已忽略控制器屬性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-553">The controller attribute is ignored.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="2b03a-554">路由會負責將要求 Uri 對應至端點，並將傳入要求分派至這些端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-554">Routing is responsible for mapping request URIs to endpoints and dispatching incoming requests to those endpoints.</span></span> <span data-ttu-id="2b03a-555">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-555">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="2b03a-556">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-556">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="2b03a-557">使用來自應用程式的路由資訊，路由也能夠產生對應至端點的 Url。</span><span class="sxs-lookup"><span data-stu-id="2b03a-557">Using route information from the app, routing is also able to generate URLs that map to endpoints.</span></span>

<span data-ttu-id="2b03a-558">若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="2b03a-558">To use the latest routing scenarios in ASP.NET Core 2.2, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="2b03a-559"><xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-559">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions.EnableEndpointRouting> option determines if routing should internally use endpoint-based logic or the <xref:Microsoft.AspNetCore.Routing.IRouter>-based logic of ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="2b03a-560">當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-560">When the compatibility version is set to 2.2 or later, the default value is `true`.</span></span> <span data-ttu-id="2b03a-561">將值設定為 `false` 可使用舊版路由邏輯：</span><span class="sxs-lookup"><span data-stu-id="2b03a-561">Set the value to `false` to use the prior routing logic:</span></span>

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="2b03a-562">如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-562">For more information on <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, see the [ASP.NET Core 2.1 version of this topic](/aspnet/core/fundamentals/routing?view=aspnetcore-2.1).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2b03a-563">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-563">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="2b03a-564">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-564">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="2b03a-565">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-565">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="2b03a-566">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b03a-566">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="2b03a-567">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="2b03a-567">Routing basics</span></span>

<span data-ttu-id="2b03a-568">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="2b03a-568">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="2b03a-569">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="2b03a-569">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="2b03a-570">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-570">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="2b03a-571">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-571">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="2b03a-572">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-572">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="2b03a-573">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-573">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="2b03a-574">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-574">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="2b03a-575">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="2b03a-575">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="2b03a-576">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="2b03a-576">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="2b03a-577">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-577">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="2b03a-578">如需詳細資訊，請參閱 <xref:razor-pages/index> 與 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-578">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="2b03a-579">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-579">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="2b03a-580">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-580">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="2b03a-581">路由功能使用「端點」(`Endpoint`) 來表示應用程式中的邏輯端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-581">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="2b03a-582">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-582">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="2b03a-583">中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="2b03a-583">The metadata is used implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="2b03a-584">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2b03a-584">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="2b03a-585">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-585">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="2b03a-586">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="2b03a-586">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="2b03a-587"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-587"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="2b03a-588">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="2b03a-588">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="2b03a-589">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-589">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="2b03a-590">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-590">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="2b03a-591">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-591">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="2b03a-592">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-592">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="2b03a-593">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="2b03a-593">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="2b03a-594">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-594">The Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="2b03a-595">如果無法透過 DI 使用連結產生器 API，<xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-595">Where the Link Generator API isn't available via DI, <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="2b03a-596">在 ASP.NET Core 2.2 中發行端點路由時，端點連結限制在 MVC/Razor Pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="2b03a-596">With the release of endpoint routing in ASP.NET Core 2.2, endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="2b03a-597">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-597">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

<span data-ttu-id="2b03a-598">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="2b03a-598">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="2b03a-599">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-599">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="2b03a-600">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-600">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="2b03a-601">URL 比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-601">URL matching</span></span>

<span data-ttu-id="2b03a-602">URL 比對是路由用來將傳入要求分派給「端點」的處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-602">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="2b03a-603">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="2b03a-603">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="2b03a-604">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="2b03a-604">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="2b03a-605">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-605">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="2b03a-606">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-606">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="2b03a-607">執行端點委派時，會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-607">When the endpoint delegate is executed, the properties of [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) are set to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="2b03a-608">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-608">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="2b03a-609">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-609">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="2b03a-610">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="2b03a-610">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="2b03a-611">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-611"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="2b03a-612">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-612">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="2b03a-613">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="2b03a-613">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="2b03a-614">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="2b03a-614">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="2b03a-615">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="2b03a-615">Routes can be nested inside of one another.</span></span> <span data-ttu-id="2b03a-616"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-616">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="2b03a-617">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-617">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="2b03a-618"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-618">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation-with-linkgenerator"></a><span data-ttu-id="2b03a-619">使用 LinkGenerator 產生 URL</span><span class="sxs-lookup"><span data-stu-id="2b03a-619">URL generation with LinkGenerator</span></span>

<span data-ttu-id="2b03a-620">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-620">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="2b03a-621">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-621">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="2b03a-622">端點路由包含連結產生器 API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-622">Endpoint routing includes the Link Generator API (<xref:Microsoft.AspNetCore.Routing.LinkGenerator>).</span></span> <span data-ttu-id="2b03a-623"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 是可從 DI 擷取的單一服務。</span><span class="sxs-lookup"><span data-stu-id="2b03a-623"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is a singleton service that can be retrieved from DI.</span></span> <span data-ttu-id="2b03a-624">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="2b03a-624">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="2b03a-625">MVC 的 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 及依賴 <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-625">MVC's <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> and scenarios that rely on <xref:Microsoft.AspNetCore.Mvc.IUrlHelper>, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="2b03a-626">連結產生器背後支援的概念為「位址」和「位址配置」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-626">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="2b03a-627">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-627">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="2b03a-628">例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-628">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="2b03a-629">連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="2b03a-629">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*>
* <xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetPathByPage*>
* <xref:Microsoft.AspNetCore.Routing.PageLinkGeneratorExtensions.GetUriByPage*>

<span data-ttu-id="2b03a-630">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-630">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="2b03a-631">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="2b03a-631">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="2b03a-632">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-632">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="2b03a-633">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-633">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="2b03a-634">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-634">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="2b03a-635">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="2b03a-635">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="2b03a-636">呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="2b03a-636"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> is called with an address.</span></span> <span data-ttu-id="2b03a-637">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="2b03a-637">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="2b03a-638">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="2b03a-638">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="2b03a-639">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-639">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="2b03a-640">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="2b03a-640">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="2b03a-641"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="2b03a-641">The methods provided by <xref:Microsoft.AspNetCore.Routing.LinkGenerator> support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="2b03a-642">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="2b03a-642">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="2b03a-643">擴充方法</span><span class="sxs-lookup"><span data-stu-id="2b03a-643">Extension Method</span></span>   | <span data-ttu-id="2b03a-644">描述</span><span class="sxs-lookup"><span data-stu-id="2b03a-644">Description</span></span>                                                         |
| ------------------ | ------------------------------------------------------------------- |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetPathByAddress*> | <span data-ttu-id="2b03a-645">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-645">Generates a URI with an absolute path based on the provided values.</span></span> |
| <xref:Microsoft.AspNetCore.Routing.LinkGenerator.GetUriByAddress*> | <span data-ttu-id="2b03a-646">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-646">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="2b03a-647">注意呼叫 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="2b03a-647">Pay attention to the following implications of calling <xref:Microsoft.AspNetCore.Routing.LinkGenerator> methods:</span></span>
>
> * <span data-ttu-id="2b03a-648">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="2b03a-648">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="2b03a-649">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="2b03a-649">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="2b03a-650">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-650">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="2b03a-651">使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator>，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2b03a-651">Use <xref:Microsoft.AspNetCore.Routing.LinkGenerator> with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="2b03a-652">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-652">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="2b03a-653">所有的 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-653">All of the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> APIs allow specifying a base path.</span></span> <span data-ttu-id="2b03a-654">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="2b03a-654">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="2b03a-655">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="2b03a-655">Differences from earlier versions of routing</span></span>

<span data-ttu-id="2b03a-656">ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：</span><span class="sxs-lookup"><span data-stu-id="2b03a-656">A few differences exist between endpoint routing in ASP.NET Core 2.2 or later and earlier versions of routing in ASP.NET Core:</span></span>

* <span data-ttu-id="2b03a-657">端點路由系統不支援以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的擴充性，包括從 <xref:Microsoft.AspNetCore.Routing.Route> 繼承。</span><span class="sxs-lookup"><span data-stu-id="2b03a-657">The endpoint routing system doesn't support <xref:Microsoft.AspNetCore.Routing.IRouter>-based extensibility, including inheriting from <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>

* <span data-ttu-id="2b03a-658">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-658">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="2b03a-659">請使用 2.1 [相容性版本](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) 以繼續使用相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-659">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="2b03a-660">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-660">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="2b03a-661">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="2b03a-661">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="2b03a-662">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="2b03a-662">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="2b03a-663">使用以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="2b03a-663">With <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="2b03a-664">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-664">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="2b03a-665">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-665">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="2b03a-666">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-666">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="2b03a-667">當嘗試連結至不存在的控制器/動作或頁面時，MVC/Razor Pages 所使用的連結產生與傳統路由的行為不同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-667">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="2b03a-668">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="2b03a-668">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="2b03a-669">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="2b03a-669">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="2b03a-670">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="2b03a-670">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="2b03a-671">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-671">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="2b03a-672">不過，如果動作不存在，則端點路由會產生空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-672">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="2b03a-673">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="2b03a-673">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="2b03a-674">連結產生「環境值失效演算法」在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-674">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="2b03a-675">「環境值失效」是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-675">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="2b03a-676">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="2b03a-676">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="2b03a-677">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-677">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="2b03a-678">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2b03a-678">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="2b03a-679">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="2b03a-679">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="2b03a-680">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-680">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="2b03a-681">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="2b03a-681">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="2b03a-682">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="2b03a-682">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="2b03a-683">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="2b03a-683">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="2b03a-684">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-684">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="2b03a-685">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="2b03a-685">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="2b03a-686">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-686">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="2b03a-687">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-687">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="2b03a-688">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-688">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="2b03a-689">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-689">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="2b03a-690">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-690">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="2b03a-691">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="2b03a-691">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="2b03a-692">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-692">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="2b03a-693">路由</span><span class="sxs-lookup"><span data-stu-id="2b03a-693">Route</span></span>              | <span data-ttu-id="2b03a-694">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="2b03a-694">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | <span data-ttu-id="2b03a-695">`/search/admin%2Fproducts` (斜線會經過編碼)</span><span class="sxs-lookup"><span data-stu-id="2b03a-695">`/search/admin%2Fproducts` (the forward slash is encoded)</span></span>             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a><span data-ttu-id="2b03a-696">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="2b03a-696">Middleware example</span></span>

<span data-ttu-id="2b03a-697">在下列範例中，中介軟體使用 <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-697">In the following example, a middleware uses the <xref:Microsoft.AspNetCore.Routing.LinkGenerator> API to create link to an action method that lists store products.</span></span> <span data-ttu-id="2b03a-698">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-698">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

```csharp
using Microsoft.AspNetCore.Routing;

public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GetPathByAction("ListProducts", "Store");

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

### <a name="create-routes"></a><span data-ttu-id="2b03a-699">建立路由</span><span class="sxs-lookup"><span data-stu-id="2b03a-699">Create routes</span></span>

<span data-ttu-id="2b03a-700">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-700">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="2b03a-701">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-701">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="2b03a-702"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-702"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="2b03a-703"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-703"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="2b03a-704">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-704">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="2b03a-705">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="2b03a-705">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="2b03a-706">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-706">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="2b03a-707">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-707">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="2b03a-708">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-708">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="2b03a-709">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="2b03a-709">Route parameters are named.</span></span> <span data-ttu-id="2b03a-710">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="2b03a-710">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="2b03a-711">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-711">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="2b03a-712">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-712">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="2b03a-713">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-713">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="2b03a-714">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-714">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="2b03a-715">在路由相符時，具有預設值的路由參數一定會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-715">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="2b03a-716">如果沒有任何對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-716">Optional parameters don't produce a route value if there was no corresponding URL path segment.</span></span> <span data-ttu-id="2b03a-717">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-717">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="2b03a-718">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="2b03a-718">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="2b03a-719">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-719">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="2b03a-720">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="2b03a-720">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="2b03a-721">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-721">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="2b03a-722">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-722">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="2b03a-723"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-723">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="2b03a-724">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="2b03a-724">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="2b03a-725">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-725">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> <span data-ttu-id="2b03a-726">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="2b03a-726">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="2b03a-727">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-727">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="2b03a-728">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="2b03a-728">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="2b03a-729">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-729">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="2b03a-730">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-730">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="2b03a-731">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-731">Default values can be specified in the route template.</span></span> <span data-ttu-id="2b03a-732">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="2b03a-732">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="2b03a-733">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-733">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="2b03a-734">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="2b03a-734">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="2b03a-735">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-735">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="2b03a-737">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="2b03a-737">Route class URL generation</span></span>

<span data-ttu-id="2b03a-738"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-738">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="2b03a-739">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-739">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-740">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-740">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="2b03a-741">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="2b03a-741">What values would be produced?</span></span> <span data-ttu-id="2b03a-742">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-742">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="2b03a-743">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-743">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="2b03a-744">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-744">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="2b03a-745">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-745">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="2b03a-746">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-746">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="2b03a-747">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-747">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="2b03a-748">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="2b03a-748">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="2b03a-749">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-749">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="2b03a-750">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-750">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="2b03a-751">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-751">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="2b03a-752">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="2b03a-752">Use Routing Middleware</span></span>

<span data-ttu-id="2b03a-753">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-753">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="2b03a-754">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="2b03a-754">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="2b03a-755">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="2b03a-755">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="2b03a-756">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="2b03a-756">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="2b03a-757"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; 僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-757"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="2b03a-758">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="2b03a-758">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="2b03a-759">URI</span><span class="sxs-lookup"><span data-stu-id="2b03a-759">URI</span></span>                    | <span data-ttu-id="2b03a-760">回應</span><span class="sxs-lookup"><span data-stu-id="2b03a-760">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="2b03a-761">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-761">Hello!</span></span> <span data-ttu-id="2b03a-762">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-762">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="2b03a-763">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-763">Hello!</span></span> <span data-ttu-id="2b03a-764">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-764">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="2b03a-765">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-765">Hello!</span></span> <span data-ttu-id="2b03a-766">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-766">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="2b03a-767">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-767">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="2b03a-768">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="2b03a-768">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="2b03a-769">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="2b03a-769">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="2b03a-770">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-770">The request falls through, no match.</span></span>              |

<span data-ttu-id="2b03a-771">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="2b03a-771">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

<span data-ttu-id="2b03a-772">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="2b03a-772">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="2b03a-773">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-773">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="2b03a-774">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-774">Route template reference</span></span>

<span data-ttu-id="2b03a-775">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-775">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="2b03a-776">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-776">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="2b03a-777">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-777">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="2b03a-778">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-778">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="2b03a-779">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="2b03a-779">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="2b03a-780">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="2b03a-780">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="2b03a-781">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-781">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="2b03a-782">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="2b03a-782">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="2b03a-783">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-783">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="2b03a-784">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-784">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="2b03a-785">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="2b03a-785">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="2b03a-786">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-786">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="2b03a-787">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="2b03a-787">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="2b03a-788">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-788">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="2b03a-789">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-789">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="2b03a-790">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-790">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="2b03a-791">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-791">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="2b03a-792">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-792">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="2b03a-793">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="2b03a-793">Note the escaped forward slash.</span></span> <span data-ttu-id="2b03a-794">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="2b03a-794">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="2b03a-795">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-795">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

<span data-ttu-id="2b03a-796">路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-796">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="2b03a-797">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-797">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="2b03a-798">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-798">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="2b03a-799">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="2b03a-799">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="2b03a-800">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-800">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="2b03a-801">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-801">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="2b03a-802">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-802">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="2b03a-803">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="2b03a-803">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="2b03a-804">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="2b03a-804">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="2b03a-805">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-805">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="2b03a-806">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="2b03a-806">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="2b03a-807">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-807">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="2b03a-808">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-808">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="2b03a-809">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-809">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="2b03a-810">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="2b03a-810">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="2b03a-811">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-811">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

<span data-ttu-id="2b03a-812">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-812">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="2b03a-813">路由範本</span><span class="sxs-lookup"><span data-stu-id="2b03a-813">Route Template</span></span>                           | <span data-ttu-id="2b03a-814">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="2b03a-814">Example Matching URI</span></span>    | <span data-ttu-id="2b03a-815">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b03a-815">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="2b03a-816">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-816">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="2b03a-817">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-817">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="2b03a-818">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-818">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="2b03a-819">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-819">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="2b03a-820">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-820">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="2b03a-821">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-821">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="2b03a-822">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-822">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="2b03a-823">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-823">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-824">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-824">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="2b03a-825">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="2b03a-825">Reserved routing names</span></span>

<span data-ttu-id="2b03a-826">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="2b03a-826">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="2b03a-827">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-827">Route constraint reference</span></span>

<span data-ttu-id="2b03a-828">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="2b03a-828">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="2b03a-829">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-829">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="2b03a-830">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-830">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="2b03a-831">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-831">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="2b03a-832">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="2b03a-832">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="2b03a-833">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="2b03a-833">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="2b03a-834">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」回應，而不是「400 - 錯誤要求」與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="2b03a-834">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="2b03a-835">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="2b03a-835">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="2b03a-836">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-836">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="2b03a-837">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="2b03a-837">constraint</span></span> | <span data-ttu-id="2b03a-838">範例</span><span class="sxs-lookup"><span data-stu-id="2b03a-838">Example</span></span> | <span data-ttu-id="2b03a-839">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-839">Example Matches</span></span> | <span data-ttu-id="2b03a-840">備註</span><span class="sxs-lookup"><span data-stu-id="2b03a-840">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="2b03a-841">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="2b03a-841">`123456789`, `-123456789`</span></span> | <span data-ttu-id="2b03a-842">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="2b03a-842">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="2b03a-843">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="2b03a-843">`true`, `FALSE`</span></span> | <span data-ttu-id="2b03a-844">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="2b03a-844">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="2b03a-845">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="2b03a-845">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="2b03a-846">符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-846">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="2b03a-847">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="2b03a-847">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="2b03a-848">符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-848">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double` | `{weight:double}` | <span data-ttu-id="2b03a-849">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="2b03a-849">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="2b03a-850">符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-850">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float` | `{weight:float}` | <span data-ttu-id="2b03a-851">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="2b03a-851">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="2b03a-852">符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-852">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid` | `{id:guid}` | <span data-ttu-id="2b03a-853">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="2b03a-853">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="2b03a-854">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="2b03a-854">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="2b03a-855">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="2b03a-855">`123456789`, `-123456789`</span></span> | <span data-ttu-id="2b03a-856">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="2b03a-856">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="2b03a-857">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-857">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="2b03a-858">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-858">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="2b03a-859">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-859">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="2b03a-860">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-860">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="2b03a-861">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="2b03a-861">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="2b03a-862">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="2b03a-862">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="2b03a-863">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="2b03a-863">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="2b03a-864">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="2b03a-864">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="2b03a-865">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="2b03a-865">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="2b03a-866">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="2b03a-866">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="2b03a-867">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-867">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="2b03a-868">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="2b03a-868">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="2b03a-869">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-869">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="2b03a-870">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2b03a-870">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="2b03a-871">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-871">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="2b03a-872">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-872">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="2b03a-873">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-873">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="2b03a-874">規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-874">Regular expressions</span></span>

<span data-ttu-id="2b03a-875">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-875">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="2b03a-876">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-876">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="2b03a-877">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-877">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="2b03a-878">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-878">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="2b03a-879">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="2b03a-879">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="2b03a-880">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="2b03a-880">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="2b03a-881">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="2b03a-881">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="2b03a-882">規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-882">Regular Expression</span></span>    | <span data-ttu-id="2b03a-883">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-883">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="2b03a-884">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-884">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="2b03a-885">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="2b03a-885">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="2b03a-886">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-886">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="2b03a-887">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="2b03a-887">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="2b03a-888">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="2b03a-888">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="2b03a-889">運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-889">Expression</span></span>   | <span data-ttu-id="2b03a-890">String</span><span class="sxs-lookup"><span data-stu-id="2b03a-890">String</span></span>    | <span data-ttu-id="2b03a-891">比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-891">Match</span></span> | <span data-ttu-id="2b03a-892">註解</span><span class="sxs-lookup"><span data-stu-id="2b03a-892">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-893">hello</span><span class="sxs-lookup"><span data-stu-id="2b03a-893">hello</span></span>     | <span data-ttu-id="2b03a-894">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-894">Yes</span></span>   | <span data-ttu-id="2b03a-895">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-895">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-896">123abc456</span><span class="sxs-lookup"><span data-stu-id="2b03a-896">123abc456</span></span> | <span data-ttu-id="2b03a-897">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-897">Yes</span></span>   | <span data-ttu-id="2b03a-898">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-898">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-899">mz</span><span class="sxs-lookup"><span data-stu-id="2b03a-899">mz</span></span>        | <span data-ttu-id="2b03a-900">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-900">Yes</span></span>   | <span data-ttu-id="2b03a-901">符合運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-901">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-902">MZ</span><span class="sxs-lookup"><span data-stu-id="2b03a-902">MZ</span></span>        | <span data-ttu-id="2b03a-903">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-903">Yes</span></span>   | <span data-ttu-id="2b03a-904">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="2b03a-904">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="2b03a-905">hello</span><span class="sxs-lookup"><span data-stu-id="2b03a-905">hello</span></span>     | <span data-ttu-id="2b03a-906">否</span><span class="sxs-lookup"><span data-stu-id="2b03a-906">No</span></span>    | <span data-ttu-id="2b03a-907">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="2b03a-907">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="2b03a-908">123abc456</span><span class="sxs-lookup"><span data-stu-id="2b03a-908">123abc456</span></span> | <span data-ttu-id="2b03a-909">否</span><span class="sxs-lookup"><span data-stu-id="2b03a-909">No</span></span>    | <span data-ttu-id="2b03a-910">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="2b03a-910">See `^` and `$` above</span></span> |

<span data-ttu-id="2b03a-911">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-911">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="2b03a-912">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-912">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="2b03a-913">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="2b03a-913">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="2b03a-914">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-914">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="2b03a-915">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-915">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="2b03a-916">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="2b03a-916">Custom Route Constraints</span></span>

<span data-ttu-id="2b03a-917">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="2b03a-917">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="2b03a-918"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-918">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="2b03a-919">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="2b03a-919">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="2b03a-920"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-920">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="2b03a-921">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="2b03a-921">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="2b03a-922">例如:</span><span class="sxs-lookup"><span data-stu-id="2b03a-922">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="2b03a-923">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-923">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="2b03a-924">例如:</span><span class="sxs-lookup"><span data-stu-id="2b03a-924">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="parameter-transformer-reference"></a><span data-ttu-id="2b03a-925">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-925">Parameter transformer reference</span></span>

<span data-ttu-id="2b03a-926">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="2b03a-926">Parameter transformers:</span></span>

* <span data-ttu-id="2b03a-927">會在為 <xref:Microsoft.AspNetCore.Routing.Route> 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="2b03a-927">Execute when generating a link for a <xref:Microsoft.AspNetCore.Routing.Route>.</span></span>
* <span data-ttu-id="2b03a-928">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-928">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="2b03a-929">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="2b03a-929">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="2b03a-930">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-930">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="2b03a-931">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-931">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="2b03a-932">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-932">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="2b03a-933">若要在路由模式中使用參數轉換器，請先在 `Startup.ConfigureServices` 中使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定：</span><span class="sxs-lookup"><span data-stu-id="2b03a-933">To use a parameter transformer in a route pattern, configure it first using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRouting(options =>
{
    // Replace the type and the name used to refer to it with your own
    // IOutboundParameterTransformer implementation
    options.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer);
});
```

<span data-ttu-id="2b03a-934">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-934">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="2b03a-935">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-935">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller:slugify=Home}/{action:slugify=Index}/{id?}");
```

<span data-ttu-id="2b03a-936">使用上述的路由，動作 `SubscriptionManagementController.GetAll()` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="2b03a-936">With the preceding route, the action `SubscriptionManagementController.GetAll()` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="2b03a-937">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-937">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="2b03a-938">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-938">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="2b03a-939">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="2b03a-939">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="2b03a-940">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-940">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="2b03a-941">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-941">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="2b03a-942">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-942">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="2b03a-943">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-943">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="2b03a-944">Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-944">Razor Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="2b03a-945">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="2b03a-945">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="2b03a-946">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="2b03a-946">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="2b03a-947">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-947">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

## <a name="url-generation-reference"></a><span data-ttu-id="2b03a-948">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-948">URL generation reference</span></span>

<span data-ttu-id="2b03a-949">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-949">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="2b03a-950">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-950">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="2b03a-951">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-951">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="2b03a-952">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-952">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="2b03a-953"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」的集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-953">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="2b03a-954">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-954">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="2b03a-955">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-955">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="2b03a-956">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-956">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="2b03a-957">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="2b03a-957">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="2b03a-958">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-958">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="2b03a-959">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="2b03a-959">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="2b03a-960">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-960">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="2b03a-961">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="2b03a-961">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="2b03a-962">環境值</span><span class="sxs-lookup"><span data-stu-id="2b03a-962">Ambient Values</span></span>                     | <span data-ttu-id="2b03a-963">明確值</span><span class="sxs-lookup"><span data-stu-id="2b03a-963">Explicit Values</span></span>                        | <span data-ttu-id="2b03a-964">結果</span><span class="sxs-lookup"><span data-stu-id="2b03a-964">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="2b03a-965">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-965">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-966">action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-966">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="2b03a-967">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-967">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-968">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-968">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="2b03a-969">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="2b03a-969">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="2b03a-970">action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-970">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="2b03a-971">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-971">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-972">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="2b03a-972">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="2b03a-973">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="2b03a-973">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="2b03a-974">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-974">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="2b03a-975">複雜區段</span><span class="sxs-lookup"><span data-stu-id="2b03a-975">Complex segments</span></span>

<span data-ttu-id="2b03a-976">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-976">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="2b03a-977">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="2b03a-977">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="2b03a-978">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="2b03a-978">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="2b03a-979">路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-979">Routing is responsible for mapping request URIs to route handlers and dispatching an incoming requests.</span></span> <span data-ttu-id="2b03a-980">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-980">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="2b03a-981">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-981">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="2b03a-982">使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-982">Using configured routes from the app, routing is able to generate URLs that map to route handlers.</span></span>

<span data-ttu-id="2b03a-983">若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="2b03a-983">To use the latest routing scenarios in ASP.NET Core 2.1, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

> [!IMPORTANT]
> <span data-ttu-id="2b03a-984">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-984">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="2b03a-985">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-985">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="2b03a-986">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-986">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="2b03a-987">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2b03a-987">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="2b03a-988">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="2b03a-988">Routing basics</span></span>

<span data-ttu-id="2b03a-989">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="2b03a-989">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="2b03a-990">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="2b03a-990">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="2b03a-991">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-991">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="2b03a-992">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="2b03a-992">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="2b03a-993">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-993">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="2b03a-994">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-994">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="2b03a-995">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-995">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="2b03a-996">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="2b03a-996">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="2b03a-997">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="2b03a-997">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="2b03a-998">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-998">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="2b03a-999">如需詳細資訊，請參閱 <xref:razor-pages/index> 與 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-999">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="2b03a-1000">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1000">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="2b03a-1001">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1001">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

<span data-ttu-id="2b03a-1002">路由 (routing) 功能會使用「路由」(*route*) (<xref:Microsoft.AspNetCore.Routing.IRouter> 的實作) 來：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1002">Routing uses *routes* (implementations of <xref:Microsoft.AspNetCore.Routing.IRouter>) to:</span></span>

* <span data-ttu-id="2b03a-1003">將傳入要求對應至「路由處理常式」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1003">Map incoming requests to *route handlers*.</span></span>
* <span data-ttu-id="2b03a-1004">產生用於回應的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1004">Generate the URLs used in responses.</span></span>

<span data-ttu-id="2b03a-1005">根據預設，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1005">By default, an app has a single collection of routes.</span></span> <span data-ttu-id="2b03a-1006">當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1006">When a request arrives, the routes in the collection are processed in the order that they exist in the collection.</span></span> <span data-ttu-id="2b03a-1007">此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1007">The framework attempts to match an incoming request URL to a route in the collection by calling the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in the collection.</span></span> <span data-ttu-id="2b03a-1008">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1008">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>

<span data-ttu-id="2b03a-1009">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1009">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="2b03a-1010">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1010">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="2b03a-1011">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1011">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="2b03a-1012"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1012"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="2b03a-1013">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有路由，這些路由的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1013">App models, such as MVC/Razor Pages, register all of their routes, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="2b03a-1014">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1014">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="2b03a-1015">URL 是根據支援任意擴充性的路由所產生。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1015">URL generation is based on routes, which support arbitrary extensibility.</span></span> <span data-ttu-id="2b03a-1016"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> 提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1016"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper> offers methods to build URLs.</span></span>

<span data-ttu-id="2b03a-1017">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1017">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="2b03a-1018">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1018">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="2b03a-1019">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1019">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="2b03a-1020">URL 比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-1020">URL matching</span></span>

<span data-ttu-id="2b03a-1021">URL 比對是路由用來將傳入要求分派給「處理常式」的處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1021">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="2b03a-1022">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1022">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="2b03a-1023">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1023">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="2b03a-1024">傳入要求將進入 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1024">Incoming requests enter the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware>, which calls the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in sequence.</span></span> <span data-ttu-id="2b03a-1025"><xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1025">The <xref:Microsoft.AspNetCore.Routing.IRouter> instance chooses whether to *handle* the request by setting the [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) to a non-null <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="2b03a-1026">如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1026">If a route sets a handler for the request, route processing stops, and the handler is invoked to process the request.</span></span> <span data-ttu-id="2b03a-1027">如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1027">If no route handler is found to process the request, the middleware hands the request off to the next middleware in the request pipeline.</span></span>

<span data-ttu-id="2b03a-1028"><xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1028">The primary input to <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is the [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associated with the current request.</span></span> <span data-ttu-id="2b03a-1029">[RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是在比對路由之後設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1029">The [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler) and [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) are outputs set after a route is matched.</span></span>

<span data-ttu-id="2b03a-1030">呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 的比對也會根據到目前為止所執行的要求處理，將 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) 的屬性設定為適當的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1030">A match that calls <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> also sets the properties of the [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData) to appropriate values based on the request processing performed thus far.</span></span>

<span data-ttu-id="2b03a-1031">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1031">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="2b03a-1032">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1032">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="2b03a-1033">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1033">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="2b03a-1034">提供了 <xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1034"><xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*> are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="2b03a-1035">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1035">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="2b03a-1036">此外，儲藏在 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 中的值可以是任何類型，對比之下，[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values) 則必須可轉換成字串或可從字串轉換。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1036">Additionally, values stashed in [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) can be of any type, in contrast to [RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values), which must be convertible to and from strings.</span></span>

<span data-ttu-id="2b03a-1037">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1037">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="2b03a-1038">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1038">Routes can be nested inside of one another.</span></span> <span data-ttu-id="2b03a-1039"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1039">The <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="2b03a-1040">一般而言，<xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1040">Generally, the first item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="2b03a-1041"><xref:Microsoft.AspNetCore.Routing.RouteData.Routers> 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1041">The last item in <xref:Microsoft.AspNetCore.Routing.RouteData.Routers> is the route handler that matched.</span></span>

<a name="lg"></a>

### <a name="url-generation"></a><span data-ttu-id="2b03a-1042">URL 產生</span><span class="sxs-lookup"><span data-stu-id="2b03a-1042">URL generation</span></span>

<span data-ttu-id="2b03a-1043">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1043">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="2b03a-1044">這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1044">This allows for a logical separation between route handlers and the URLs that access them.</span></span>

<span data-ttu-id="2b03a-1045">URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1045">URL generation follows a similar iterative process, but it starts with user or framework code calling into the <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method of the route collection.</span></span> <span data-ttu-id="2b03a-1046">每個「路由」會依序呼叫其 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1046">Each *route* has its <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method called in sequence until a non-null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> is returned.</span></span>

<span data-ttu-id="2b03a-1047"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1047">The primary inputs to <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> are:</span></span>

* [<span data-ttu-id="2b03a-1048">VirtualPathContext.HttpContext</span><span class="sxs-lookup"><span data-stu-id="2b03a-1048">VirtualPathContext.HttpContext</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext)
* [<span data-ttu-id="2b03a-1049">VirtualPathContext.Values</span><span class="sxs-lookup"><span data-stu-id="2b03a-1049">VirtualPathContext.Values</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values)
* [<span data-ttu-id="2b03a-1050">VirtualPathContext.AmbientValues</span><span class="sxs-lookup"><span data-stu-id="2b03a-1050">VirtualPathContext.AmbientValues</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues)

<span data-ttu-id="2b03a-1051">路由主要使用 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 和 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1051">Routes primarily use the route values provided by <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> and <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> to decide whether it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="2b03a-1052"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> 是比對目前要求所產生的路由值集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1052">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues> are the set of route values that were produced from matching the current request.</span></span> <span data-ttu-id="2b03a-1053">相反地，<xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1053">In contrast, <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values> are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="2b03a-1054">提供了 <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext>，以防路由應該取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1054">The <xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext> is provided in case a route should obtain services or additional data associated with the current context.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-1055">將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1055">Think of [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) as a set of overrides for the [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*).</span></span> <span data-ttu-id="2b03a-1056">URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1056">URL generation attempts to reuse route values from the current request to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="2b03a-1057"><xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 的輸出是 <xref:Microsoft.AspNetCore.Routing.VirtualPathData>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1057">The output of <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is a <xref:Microsoft.AspNetCore.Routing.VirtualPathData>.</span></span> <span data-ttu-id="2b03a-1058"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 是 <xref:Microsoft.AspNetCore.Routing.RouteData> 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1058"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> is a parallel of <xref:Microsoft.AspNetCore.Routing.RouteData>.</span></span> <span data-ttu-id="2b03a-1059"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> 包含用於輸出 URL 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath>，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1059"><xref:Microsoft.AspNetCore.Routing.VirtualPathData> contains the <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> for the output URL and some additional properties that should be set by the route.</span></span>

<span data-ttu-id="2b03a-1060">[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1060">The [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="2b03a-1061">視您需求的不同，可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1061">Depending on your needs, you may need to process the path further.</span></span> <span data-ttu-id="2b03a-1062">如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1062">If you want to render the generated URL in HTML, prepend the base path of the app.</span></span>

<span data-ttu-id="2b03a-1063">[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1063">The [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="2b03a-1064">[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1064">The [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="2b03a-1065">這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1065">This is the parallel of [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).</span></span>

### <a name="create-routes"></a><span data-ttu-id="2b03a-1066">建立路由</span><span class="sxs-lookup"><span data-stu-id="2b03a-1066">Create routes</span></span>

<span data-ttu-id="2b03a-1067">路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1067">Routing provides the <xref:Microsoft.AspNetCore.Routing.Route> class as the standard implementation of <xref:Microsoft.AspNetCore.Routing.IRouter>.</span></span> <span data-ttu-id="2b03a-1068">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用「路由範本」語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1068"><xref:Microsoft.AspNetCore.Routing.Route> uses the *route template* syntax to define patterns to match against the URL path when <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is called.</span></span> <span data-ttu-id="2b03a-1069">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 時，<xref:Microsoft.AspNetCore.Routing.Route> 會使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1069"><xref:Microsoft.AspNetCore.Routing.Route> uses the same route template to generate a URL when <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> is called.</span></span>

<span data-ttu-id="2b03a-1070">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1070">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="2b03a-1071">任何 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1071">Any of the <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

<span data-ttu-id="2b03a-1072"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1072"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> doesn't accept a route handler parameter.</span></span> <span data-ttu-id="2b03a-1073"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1073"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="2b03a-1074">預設處理常式為 `IRouter`，該處理常式可能無法處理要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1074">The default handler is an `IRouter`, and the handler might not handle the request.</span></span> <span data-ttu-id="2b03a-1075">例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1075">For example, ASP.NET Core MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="2b03a-1076">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1076">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

<span data-ttu-id="2b03a-1077">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1077">The following code example is an example of a <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="2b03a-1078">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1078">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="2b03a-1079">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1079">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="2b03a-1080">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1080">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="2b03a-1081">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1081">Route parameters are named.</span></span> <span data-ttu-id="2b03a-1082">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1082">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="2b03a-1083">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1083">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="2b03a-1084">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1084">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="2b03a-1085">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1085">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="2b03a-1086">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1086">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="2b03a-1087">在路由相符時，具有預設值的路由參數一定會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1087">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="2b03a-1088">如果沒有任何對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1088">Optional parameters don't produce a route value if there was no corresponding URL path segment.</span></span> <span data-ttu-id="2b03a-1089">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1089">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="2b03a-1090">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1090">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="2b03a-1091">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1091">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="2b03a-1092">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1092">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="2b03a-1093">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1093">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="2b03a-1094">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1094">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="2b03a-1095"><xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1095">Additional overloads of <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="2b03a-1096">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1096">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="2b03a-1097">下列 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1097">The following <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> examples create equivalent routes:</span></span>

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> <span data-ttu-id="2b03a-1098">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1098">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="2b03a-1099">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1099">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="2b03a-1100">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1100">The following example demonstrates a few additional scenarios:</span></span>

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="2b03a-1101">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1101">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="2b03a-1102">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1102">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="2b03a-1103">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1103">Default values can be specified in the route template.</span></span> <span data-ttu-id="2b03a-1104">`article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1104">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk (`*`) before the route parameter name.</span></span> <span data-ttu-id="2b03a-1105">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1105">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

<span data-ttu-id="2b03a-1106">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1106">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="2b03a-1107">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1107">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="2b03a-1109">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="2b03a-1109">Route class URL generation</span></span>

<span data-ttu-id="2b03a-1110"><xref:Microsoft.AspNetCore.Routing.Route> 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1110">The <xref:Microsoft.AspNetCore.Routing.Route> class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="2b03a-1111">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1111">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-1112">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1112">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="2b03a-1113">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="2b03a-1113">What values would be produced?</span></span> <span data-ttu-id="2b03a-1114">這與 URL 產生在 <xref:Microsoft.AspNetCore.Routing.Route> 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1114">This is the rough equivalent of how URL generation works in the <xref:Microsoft.AspNetCore.Routing.Route> class.</span></span>

<span data-ttu-id="2b03a-1115">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1115">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="2b03a-1116">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1116">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="2b03a-1117">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1117">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="2b03a-1118">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1118">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="2b03a-1119">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1119">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="2b03a-1120">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1120">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="2b03a-1121">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1121">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="2b03a-1122">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1122">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="2b03a-1123">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1123">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="2b03a-1124">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="2b03a-1124">Use Routing Middleware</span></span>

<span data-ttu-id="2b03a-1125">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1125">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="2b03a-1126">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1126">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="2b03a-1127">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1127">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="2b03a-1128">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1128">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="2b03a-1129"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; 僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1129"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="2b03a-1130">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1130">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="2b03a-1131">URI</span><span class="sxs-lookup"><span data-stu-id="2b03a-1131">URI</span></span>                    | <span data-ttu-id="2b03a-1132">回應</span><span class="sxs-lookup"><span data-stu-id="2b03a-1132">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="2b03a-1133">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-1133">Hello!</span></span> <span data-ttu-id="2b03a-1134">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1134">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="2b03a-1135">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-1135">Hello!</span></span> <span data-ttu-id="2b03a-1136">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1136">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="2b03a-1137">Hello!</span><span class="sxs-lookup"><span data-stu-id="2b03a-1137">Hello!</span></span> <span data-ttu-id="2b03a-1138">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1138">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="2b03a-1139">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1139">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="2b03a-1140">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="2b03a-1140">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="2b03a-1141">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1141">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="2b03a-1142">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1142">The request falls through, no match.</span></span>              |

<span data-ttu-id="2b03a-1143">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1143">If you're configuring a single route, call <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passing in an `IRouter` instance.</span></span> <span data-ttu-id="2b03a-1144">您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1144">You won't need to use <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.</span></span>

<span data-ttu-id="2b03a-1145">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1145">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareDelete*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareGet*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewarePut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapMiddlewareVerb*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPost*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapPut*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>
* <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>

<span data-ttu-id="2b03a-1146">其中一些列出的方法 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>) 需要 <xref:Microsoft.AspNetCore.Http.RequestDelegate>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1146">Some of listed methods, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*>, require a <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="2b03a-1147">路由相符時，<xref:Microsoft.AspNetCore.Http.RequestDelegate> 會作為「路由處理常式」使用。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1147">The <xref:Microsoft.AspNetCore.Http.RequestDelegate> is used as the *route handler* when the route matches.</span></span> <span data-ttu-id="2b03a-1148">此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1148">Other methods in this family allow configuring a middleware pipeline for use as the route handler.</span></span> <span data-ttu-id="2b03a-1149">如果 `Map*` 方法不接受處理常式 (例如 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1149">If the `Map*` method doesn't accept a handler, such as <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapRoute*>, it uses the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span>

<span data-ttu-id="2b03a-1150">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1150">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="2b03a-1151">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1151">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="2b03a-1152">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-1152">Route template reference</span></span>

<span data-ttu-id="2b03a-1153">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1153">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="2b03a-1154">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1154">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="2b03a-1155">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1155">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="2b03a-1156">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1156">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="2b03a-1157">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1157">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="2b03a-1158">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1158">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="2b03a-1159">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1159">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="2b03a-1160">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1160">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="2b03a-1161">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1161">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="2b03a-1162">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1162">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="2b03a-1163">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1163">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="2b03a-1164">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1164">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

<span data-ttu-id="2b03a-1165">您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1165">You can use the asterisk (`*`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="2b03a-1166">這稱為「全部擷取」參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1166">This is called a *catch-all* parameter.</span></span> <span data-ttu-id="2b03a-1167">例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1167">For example, `blog/{*slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="2b03a-1168">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1168">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="2b03a-1169">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1169">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="2b03a-1170">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1170">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="2b03a-1171">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1171">Note the escaped forward slash.</span></span>

<span data-ttu-id="2b03a-1172">路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1172">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="2b03a-1173">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1173">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="2b03a-1174">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1174">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="2b03a-1175">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1175">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="2b03a-1176">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1176">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="2b03a-1177">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1177">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="2b03a-1178">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1178">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="2b03a-1179">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1179">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="2b03a-1180">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1180">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="2b03a-1181">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1181">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="2b03a-1182">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1182">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="2b03a-1183">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1183">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

<span data-ttu-id="2b03a-1184">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1184">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="2b03a-1185">路由範本</span><span class="sxs-lookup"><span data-stu-id="2b03a-1185">Route Template</span></span>                           | <span data-ttu-id="2b03a-1186">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="2b03a-1186">Example Matching URI</span></span>    | <span data-ttu-id="2b03a-1187">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="2b03a-1187">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="2b03a-1188">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1188">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="2b03a-1189">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1189">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="2b03a-1190">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1190">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="2b03a-1191">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1191">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="2b03a-1192">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1192">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| `{controller=Home}/{action=Index}/{id?}` | `/`                     | <span data-ttu-id="2b03a-1193">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1193">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="2b03a-1194">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1194">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="2b03a-1195">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1195">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="2b03a-1196">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 <xref:Microsoft.AspNetCore.Routing.Route>) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1196">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="2b03a-1197">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="2b03a-1197">Reserved routing names</span></span>

<span data-ttu-id="2b03a-1198">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1198">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="2b03a-1199">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-1199">Route constraint reference</span></span>

<span data-ttu-id="2b03a-1200">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1200">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="2b03a-1201">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1201">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="2b03a-1202">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1202">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="2b03a-1203">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1203">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="2b03a-1204">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1204">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="2b03a-1205">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1205">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="2b03a-1206">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」回應，而不是「400 - 錯誤要求」與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1206">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="2b03a-1207">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1207">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="2b03a-1208">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1208">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="2b03a-1209">constraint (條件約束)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1209">constraint</span></span> | <span data-ttu-id="2b03a-1210">範例</span><span class="sxs-lookup"><span data-stu-id="2b03a-1210">Example</span></span> | <span data-ttu-id="2b03a-1211">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-1211">Example Matches</span></span> | <span data-ttu-id="2b03a-1212">備註</span><span class="sxs-lookup"><span data-stu-id="2b03a-1212">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="2b03a-1213">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1213">`123456789`, `-123456789`</span></span> | <span data-ttu-id="2b03a-1214">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="2b03a-1214">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="2b03a-1215">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1215">`true`, `FALSE`</span></span> | <span data-ttu-id="2b03a-1216">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1216">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="2b03a-1217">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1217">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="2b03a-1218">符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1218">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="2b03a-1219">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1219">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="2b03a-1220">符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1220">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double` | `{weight:double}` | <span data-ttu-id="2b03a-1221">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1221">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="2b03a-1222">符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1222">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float` | `{weight:float}` | <span data-ttu-id="2b03a-1223">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1223">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="2b03a-1224">符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1224">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid` | `{id:guid}` | <span data-ttu-id="2b03a-1225">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1225">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="2b03a-1226">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="2b03a-1226">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="2b03a-1227">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1227">`123456789`, `-123456789`</span></span> | <span data-ttu-id="2b03a-1228">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="2b03a-1228">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="2b03a-1229">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-1229">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="2b03a-1230">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-1230">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="2b03a-1231">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-1231">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="2b03a-1232">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="2b03a-1232">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="2b03a-1233">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="2b03a-1233">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="2b03a-1234">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="2b03a-1234">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="2b03a-1235">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="2b03a-1235">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="2b03a-1236">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1236">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="2b03a-1237">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="2b03a-1237">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="2b03a-1238">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="2b03a-1238">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="2b03a-1239">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1239">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="2b03a-1240">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1240">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="2b03a-1241">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1241">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="2b03a-1242">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1242">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="2b03a-1243">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1243">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="2b03a-1244">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1244">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="2b03a-1245">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1245">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="2b03a-1246">規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-1246">Regular expressions</span></span>

<span data-ttu-id="2b03a-1247">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1247">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="2b03a-1248">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1248">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="2b03a-1249">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1249">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="2b03a-1250">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1250">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="2b03a-1251">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1251">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="2b03a-1252">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="2b03a-1252">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="2b03a-1253">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1253">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="2b03a-1254">規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-1254">Regular Expression</span></span>    | <span data-ttu-id="2b03a-1255">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-1255">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="2b03a-1256">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1256">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="2b03a-1257">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1257">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="2b03a-1258">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1258">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="2b03a-1259">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1259">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="2b03a-1260">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1260">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="2b03a-1261">運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-1261">Expression</span></span>   | <span data-ttu-id="2b03a-1262">String</span><span class="sxs-lookup"><span data-stu-id="2b03a-1262">String</span></span>    | <span data-ttu-id="2b03a-1263">比對</span><span class="sxs-lookup"><span data-stu-id="2b03a-1263">Match</span></span> | <span data-ttu-id="2b03a-1264">註解</span><span class="sxs-lookup"><span data-stu-id="2b03a-1264">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-1265">hello</span><span class="sxs-lookup"><span data-stu-id="2b03a-1265">hello</span></span>     | <span data-ttu-id="2b03a-1266">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1266">Yes</span></span>   | <span data-ttu-id="2b03a-1267">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-1267">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-1268">123abc456</span><span class="sxs-lookup"><span data-stu-id="2b03a-1268">123abc456</span></span> | <span data-ttu-id="2b03a-1269">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1269">Yes</span></span>   | <span data-ttu-id="2b03a-1270">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="2b03a-1270">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-1271">mz</span><span class="sxs-lookup"><span data-stu-id="2b03a-1271">mz</span></span>        | <span data-ttu-id="2b03a-1272">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1272">Yes</span></span>   | <span data-ttu-id="2b03a-1273">符合運算式</span><span class="sxs-lookup"><span data-stu-id="2b03a-1273">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="2b03a-1274">MZ</span><span class="sxs-lookup"><span data-stu-id="2b03a-1274">MZ</span></span>        | <span data-ttu-id="2b03a-1275">[是]</span><span class="sxs-lookup"><span data-stu-id="2b03a-1275">Yes</span></span>   | <span data-ttu-id="2b03a-1276">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="2b03a-1276">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="2b03a-1277">hello</span><span class="sxs-lookup"><span data-stu-id="2b03a-1277">hello</span></span>     | <span data-ttu-id="2b03a-1278">否</span><span class="sxs-lookup"><span data-stu-id="2b03a-1278">No</span></span>    | <span data-ttu-id="2b03a-1279">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1279">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="2b03a-1280">123abc456</span><span class="sxs-lookup"><span data-stu-id="2b03a-1280">123abc456</span></span> | <span data-ttu-id="2b03a-1281">否</span><span class="sxs-lookup"><span data-stu-id="2b03a-1281">No</span></span>    | <span data-ttu-id="2b03a-1282">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="2b03a-1282">See `^` and `$` above</span></span> |

<span data-ttu-id="2b03a-1283">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1283">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="2b03a-1284">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1284">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="2b03a-1285">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1285">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="2b03a-1286">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1286">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="2b03a-1287">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1287">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

## <a name="custom-route-constraints"></a><span data-ttu-id="2b03a-1288">自訂路由限制式</span><span class="sxs-lookup"><span data-stu-id="2b03a-1288">Custom Route Constraints</span></span>

<span data-ttu-id="2b03a-1289">除了內建的路由限制式之外，自訂路由限制式也可以透過實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面來建立。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1289">In addition to the built-in route constraints, custom route constraints can be created by implementing the <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface.</span></span> <span data-ttu-id="2b03a-1290"><xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 介面包含單一方法 `Match`，此方法會在滿足限制式時傳回 `true`，否則會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1290">The <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> interface contains a single method, `Match`, which returns `true` if the constraint is satisfied and `false` otherwise.</span></span>

<span data-ttu-id="2b03a-1291">若要使用自訂 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，路由限制式型別必須必須向應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> (在應用程式的服務容器中) 註冊。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1291">To use a custom <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>, the route constraint type must be registered with the app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> in the app's service container.</span></span> <span data-ttu-id="2b03a-1292"><xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 是一個目錄，它將路由限制式機碼對應到可驗證那些限制式的 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 實作。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1292">A <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> is a dictionary that maps route constraint keys to <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementations that validate those constraints.</span></span> <span data-ttu-id="2b03a-1293">更新應用程式的 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 時，可在 `Startup.ConfigureServices` 中於進行 [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) 呼叫時更新，或透過使用 `services.Configure<RouteOptions>` 直接設定 <xref:Microsoft.AspNetCore.Routing.RouteOptions> 來更新。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1293">An app's <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> can be updated in `Startup.ConfigureServices` either as part of a [services.AddRouting](xref:Microsoft.Extensions.DependencyInjection.RoutingServiceCollectionExtensions.AddRouting*) call or by configuring <xref:Microsoft.AspNetCore.Routing.RouteOptions> directly with `services.Configure<RouteOptions>`.</span></span> <span data-ttu-id="2b03a-1294">例如:</span><span class="sxs-lookup"><span data-stu-id="2b03a-1294">For example:</span></span>

```csharp
services.AddRouting(options =>
{
    options.ConstraintMap.Add("customName", typeof(MyCustomConstraint));
});
```

<span data-ttu-id="2b03a-1295">限制式接著能以一般方式套用到路由 (使用註冊限制式型別時使用名稱)。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1295">The constraint can then be applied to routes in the usual manner, using the name specified when registering the constraint type.</span></span> <span data-ttu-id="2b03a-1296">例如:</span><span class="sxs-lookup"><span data-stu-id="2b03a-1296">For example:</span></span>

```csharp
[HttpGet("{id:customName}")]
public ActionResult<string> Get(string id)
```

## <a name="url-generation-reference"></a><span data-ttu-id="2b03a-1297">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="2b03a-1297">URL generation reference</span></span>

<span data-ttu-id="2b03a-1298">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1298">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="2b03a-1299">在上述範例的結尾產生的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1299">The <xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath> generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="2b03a-1300">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1300">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="2b03a-1301">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1301">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="2b03a-1302"><xref:Microsoft.AspNetCore.Routing.VirtualPathContext> 建構函式的第二個參數是「環境值」的集合。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1302">The second parameter to the <xref:Microsoft.AspNetCore.Routing.VirtualPathContext> constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="2b03a-1303">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1303">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="2b03a-1304">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1304">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="2b03a-1305">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1305">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="2b03a-1306">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1306">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="2b03a-1307">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1307">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="2b03a-1308">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1308">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="2b03a-1309">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1309">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="2b03a-1310">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1310">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="2b03a-1311">環境值</span><span class="sxs-lookup"><span data-stu-id="2b03a-1311">Ambient Values</span></span>                     | <span data-ttu-id="2b03a-1312">明確值</span><span class="sxs-lookup"><span data-stu-id="2b03a-1312">Explicit Values</span></span>                        | <span data-ttu-id="2b03a-1313">結果</span><span class="sxs-lookup"><span data-stu-id="2b03a-1313">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="2b03a-1314">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1314">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-1315">action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1315">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="2b03a-1316">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1316">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-1317">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1317">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="2b03a-1318">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1318">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="2b03a-1319">action = "About"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1319">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="2b03a-1320">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1320">controller = "Home"</span></span>                | <span data-ttu-id="2b03a-1321">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="2b03a-1321">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="2b03a-1322">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="2b03a-1322">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="2b03a-1323">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1323">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="2b03a-1324">複雜區段</span><span class="sxs-lookup"><span data-stu-id="2b03a-1324">Complex segments</span></span>

<span data-ttu-id="2b03a-1325">複雜區段 (例如，`[Route("/x{token}y")]`) 會透過以非窮盡的方式，由右至左比對常值來處理。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1325">Complex segments (for example `[Route("/x{token}y")]`) are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="2b03a-1326">請參閱[此程式碼](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)以了解如何比對複雜區段的詳細解釋。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1326">See [this code](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) for a detailed explanation of how complex segments are matched.</span></span> <span data-ttu-id="2b03a-1327">[程式法範例](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293)不是由 ASP.NET Core 使用，但它提供一個好的複雜區段解釋。</span><span class="sxs-lookup"><span data-stu-id="2b03a-1327">The [code sample](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Http/Routing/src/Patterns/RoutePatternMatcher.cs#L293) is not used by ASP.NET Core, but it provides a good explanation of complex segments.</span></span>
<!-- While that code is no longer used by ASP.NET Core for complex segment matching, it provides a good match to the current algorithm. The [current code](https://github.com/aspnet/AspNetCore/blob/91514c9af7e0f4c44029b51f05a01c6fe4c96e4c/src/Http/Routing/src/Matching/DfaMatcherBuilder.cs#L227-L244) is too abstracted from matching to be useful for understanding complex segment matching.
-->

::: moniker-end
