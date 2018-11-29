---
title: ASP.NET Core 中的路由
author: rick-anderson
description: 探索 ASP.NET Core 路由功能如何負責將要求 URI 對應至端點選取器，並將傳入要求分派給端點。
ms.author: riande
ms.custom: mvc
ms.date: 11/15/2018
uid: fundamentals/routing
ms.openlocfilehash: f18ec1da2affbf67b7ada570b68f98a42c7256a5
ms.sourcegitcommit: ad28d1bc6657a743d5c2fa8902f82740689733bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "52256589"
---
# <a name="routing-in-aspnet-core"></a><span data-ttu-id="ac02c-103">ASP.NET Core 中的路由</span><span class="sxs-lookup"><span data-stu-id="ac02c-103">Routing in ASP.NET Core</span></span>

<span data-ttu-id="ac02c-104">作者：[Ryan Nowak](https://github.com/rynowak)、[Steve Smith](https://ardalis.com/) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ac02c-104">By [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="ac02c-105">如需本主題的 1.1 版，請下載 [ASP.NET Core 中的路由 (1.1 版，PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Routing_1.x.pdf)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-105">For the 1.1 version of this topic, download [Routing in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Routing_1.x.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-106">路由功能負責將要求 URI 對應至端點選取器，並將傳入要求分派給端點。</span><span class="sxs-lookup"><span data-stu-id="ac02c-106">Routing is responsible for mapping request URIs to endpoint selectors and dispatching incoming requests to endpoints.</span></span> <span data-ttu-id="ac02c-107">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-107">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ac02c-108">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-108">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="ac02c-109">使用應用程式中的路由資訊，路由功能也能夠產生對應至端點選取器的 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-109">Using route information from the app, routing is also able to generate URLs that map to endpoint selectors.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="ac02c-110">若要使用 ASP.NET Core 2.2 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="ac02c-110">To use the latest routing scenarios in ASP.NET Core 2.2, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="ac02c-111">`EnableEndpointRouting` 選項會決定路由功能應該在內部使用以端點為基礎的邏輯，或以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎的邏輯 (適用於 ASP.NET Core 2.1 或更舊版本)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-111">The `EnableEndpointRouting` option determines if routing should internally use endpoint-based logic or the <xref:Microsoft.AspNetCore.Routing.IRouter>-based logic of ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="ac02c-112">當相容性版本設定為 2.2 或更新版本時，預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-112">When the compatibility version is set to 2.2 or later, the default value is `true`.</span></span> <span data-ttu-id="ac02c-113">將值設定為 `false` 可使用舊版路由邏輯：</span><span class="sxs-lookup"><span data-stu-id="ac02c-113">Set the value to `false` to use the prior routing logic:</span></span>

```csharp
// Use the routing logic of ASP.NET Core 2.1 or earlier:
services.AddMvc(options => options.EnableEndpointRouting = false)
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="ac02c-114">如需以 <xref:Microsoft.AspNetCore.Routing.IRouter> 為基礎之路由的詳細資訊，請參閱[本主題的 ASP.NET Core 2.1 版本](xref:fundamentals/routing?view=aspnetcore-2.1)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-114">For more information on <xref:Microsoft.AspNetCore.Routing.IRouter>-based routing, see the [ASP.NET Core 2.1 version of this topic](xref:fundamentals/routing?view=aspnetcore-2.1).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-115">路由功能負責將要求 URI 對應至路由處理常式，並分派傳入要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-115">Routing is responsible for mapping request URIs to route handlers and dispatching an incoming requests.</span></span> <span data-ttu-id="ac02c-116">路由定義於應用程式，並在該應用程式啟動時進行設定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-116">Routes are defined in the app and configured when the app starts.</span></span> <span data-ttu-id="ac02c-117">路由可以選擇性地從要求中所包含的 URL 擷取值，然後這些值就可用於處理要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-117">A route can optionally extract values from the URL contained in the request, and these values can then be used for request processing.</span></span> <span data-ttu-id="ac02c-118">使用應用程式中已設定的路由，路由功能將能夠產生對應至路由處理常式的 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-118">Using configured routes from the app, routing is able to generate URLs that map to route handlers.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="ac02c-119">若要使用 ASP.NET Core 2.1 中的最新路由情節，請將[相容性版本](xref:mvc/compatibility-version)指定為 `Startup.ConfigureServices` 中的 MVC 服務註冊：</span><span class="sxs-lookup"><span data-stu-id="ac02c-119">To use the latest routing scenarios in ASP.NET Core 2.1, specify the [compatibility version](xref:mvc/compatibility-version) to the MVC services registration in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

::: moniker-end

> [!IMPORTANT]
> <span data-ttu-id="ac02c-120">本文件涵蓋低階的 ASP.NET Core 路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-120">This document covers low-level ASP.NET Core routing.</span></span> <span data-ttu-id="ac02c-121">如需 ASP.NET Core MVC 路由的資訊，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-121">For information on ASP.NET Core MVC routing, see <xref:mvc/controllers/routing>.</span></span> <span data-ttu-id="ac02c-122">如需 Razor Pages 中路由慣例的資訊，請參閱 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-122">For information on routing conventions in Razor Pages, see <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ac02c-123">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ac02c-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="routing-basics"></a><span data-ttu-id="ac02c-124">路由的基本概念</span><span class="sxs-lookup"><span data-stu-id="ac02c-124">Routing basics</span></span>

<span data-ttu-id="ac02c-125">大部分應用程式都應該選擇基本的描述性路由傳送配置，讓 URL 可讀且有意義。</span><span class="sxs-lookup"><span data-stu-id="ac02c-125">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="ac02c-126">預設慣例路由 `{controller=Home}/{action=Index}/{id?}`：</span><span class="sxs-lookup"><span data-stu-id="ac02c-126">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="ac02c-127">支援基本的描述性路由配置。</span><span class="sxs-lookup"><span data-stu-id="ac02c-127">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="ac02c-128">適合作為 UI 型應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="ac02c-128">Is a useful starting point for UI-based apps.</span></span>

<span data-ttu-id="ac02c-129">在特殊情況下，開發人員通常會使用[屬性路由](xref:mvc/controllers/routing#attribute-routing)或專屬的慣例路由，新增額外的簡潔路由到應用程式的高流量區域 (例如，部落格和電子商務端點)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-129">Developers commonly add additional terse routes to high-traffic areas of an app in specialized situations (for example, blog and ecommerce endpoints) using [attribute routing](xref:mvc/controllers/routing#attribute-routing) or dedicated conventional routes.</span></span>

<span data-ttu-id="ac02c-130">Web API 應該使用屬性路由傳送來將應用程式功能模型建構為作業由 HTTP 指令動詞代表的資源集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-130">Web APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="ac02c-131">這表示相同邏輯資源上的許多作業 (例如，GET、POST) 都會使用相同的 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-131">This means that many operations (for example, GET, POST) on the same logical resource will use the same URL.</span></span> <span data-ttu-id="ac02c-132">屬性路由提供仔細設計 API 公用端點配置所需的控制層級。</span><span class="sxs-lookup"><span data-stu-id="ac02c-132">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="ac02c-133">Razor Pages 應用程式使用預設慣例路由，來提供應用程式 *Pages* 資料夾中的具名資源。</span><span class="sxs-lookup"><span data-stu-id="ac02c-133">Razor Pages apps use default conventional routing to serve named resources in the *Pages* folder of an app.</span></span> <span data-ttu-id="ac02c-134">還有其他慣例可讓您自訂 Razor Pages 路由行為。</span><span class="sxs-lookup"><span data-stu-id="ac02c-134">Additional conventions are available that allow you to customize Razor Pages routing behavior.</span></span> <span data-ttu-id="ac02c-135">如需詳細資訊，請參閱 <xref:razor-pages/index> 與 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-135">For more information, see <xref:razor-pages/index> and <xref:razor-pages/razor-pages-conventions>.</span></span>

<span data-ttu-id="ac02c-136">URL 產生支援允許在不需要硬式編碼的 URL 來連結應用程式的情況下開發應用程式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-136">URL generation support allows the app to be developed without hard-coding URLs to link the app together.</span></span> <span data-ttu-id="ac02c-137">這項支援可讓您從基本路由設定開始，並在決定應用程式資源配置之後修改路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-137">This support allows for starting with a basic routing configuration and modifying the routes after the app's resource layout is determined.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-138">路由功能使用「端點」(`Endpoint`) 來表示應用程式中的邏輯端點。</span><span class="sxs-lookup"><span data-stu-id="ac02c-138">Routing uses *endpoints* (`Endpoint`) to represent logical endpoints in an app.</span></span>

<span data-ttu-id="ac02c-139">端點會定義用來處理要求的一項委派及一個任意中繼資料集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-139">An endpoint defines a delegate to process requests and a collection of arbitrary metadata.</span></span> <span data-ttu-id="ac02c-140">中繼資料可根據附加至每個端點的原則和組態，來實作跨領域關注。</span><span class="sxs-lookup"><span data-stu-id="ac02c-140">The metadata is used implement cross-cutting concerns based on policies and configuration attached to each endpoint.</span></span>

<span data-ttu-id="ac02c-141">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ac02c-141">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="ac02c-142">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-142">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="ac02c-143">允許傳統式和屬性式端點設定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-143">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="ac02c-144">`IRouteConstraint` 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-144">`IRouteConstraint` is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="ac02c-145">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有端點，這些端點的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="ac02c-145">App models, such as MVC/Razor Pages, register all of their endpoints, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="ac02c-146">路由實作會在中介軟體管線需要時制定路由決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-146">The routing implementation makes routing decisions wherever desired in the middleware pipeline.</span></span>
* <span data-ttu-id="ac02c-147">在路由中介軟體之後出現的中介軟體可以檢查指定要求 URI 的路由中介軟體端點決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-147">Middleware that appears after a Routing Middleware can inspect the result of the Routing Middleware's endpoint decision for a given request URI.</span></span>
* <span data-ttu-id="ac02c-148">您可以針對中介軟體管線中任何位置的應用程式，列舉其中的所有端點。</span><span class="sxs-lookup"><span data-stu-id="ac02c-148">It's possible to enumerate all of the endpoints in the app anywhere in the middleware pipeline.</span></span>
* <span data-ttu-id="ac02c-149">應用程式可以根據端點資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ac02c-149">An app can use routing to generate URLs (for example, for redirection or links) based on endpoint information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="ac02c-150">URL 是根據支援任意擴充性的位址所產生：</span><span class="sxs-lookup"><span data-stu-id="ac02c-150">URL generation is based on addresses, which support arbitrary extensibility:</span></span>

  * <span data-ttu-id="ac02c-151">您可以在任何位置使用[相依性插入 (DI)](xref:fundamentals/dependency-injection) 來解析連結產生器 API (`LinkGenerator`)，以產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-151">The Link Generator API (`LinkGenerator`) can be resolved anywhere using [dependency injection (DI)](xref:fundamentals/dependency-injection) to generate URLs.</span></span>
  * <span data-ttu-id="ac02c-152">如果無法透過 DI 使用連結產生器 API，`IUrlHelper` 會提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-152">Where the Link Generator API isn't available via DI, `IUrlHelper` offers methods to build URLs.</span></span>

> [!NOTE]
> <span data-ttu-id="ac02c-153">在 ASP.NET Core 2.2 中發行端點路由時，端點連結限制在 MVC/Razor Pages 動作和頁面。</span><span class="sxs-lookup"><span data-stu-id="ac02c-153">With the release of endpoint routing in ASP.NET Core 2.2, endpoint linking is limited to MVC/Razor Pages actions and pages.</span></span> <span data-ttu-id="ac02c-154">未來版本將規劃擴充端點連結功能。</span><span class="sxs-lookup"><span data-stu-id="ac02c-154">The expansions of endpoint-linking capabilities is planned for future releases.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-155">路由 (routing) 功能會使用「路由」(*route*) (<xref:Microsoft.AspNetCore.Routing.IRouter> 的實作) 來：</span><span class="sxs-lookup"><span data-stu-id="ac02c-155">Routing uses *routes* (implementations of <xref:Microsoft.AspNetCore.Routing.IRouter>) to:</span></span>

* <span data-ttu-id="ac02c-156">將傳入要求對應至「路由處理常式」。</span><span class="sxs-lookup"><span data-stu-id="ac02c-156">Map incoming requests to *route handlers*.</span></span>
* <span data-ttu-id="ac02c-157">產生用於回應的 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-157">Generate the URLs used in responses.</span></span>

<span data-ttu-id="ac02c-158">根據預設，應用程式有一個路由集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-158">By default, an app has a single collection of routes.</span></span> <span data-ttu-id="ac02c-159">當要求抵達時，集合中的路由會依其存在於集合的順序進行處理。</span><span class="sxs-lookup"><span data-stu-id="ac02c-159">When a request arrives, the routes in the collection are processed in the order that they exist in the collection.</span></span> <span data-ttu-id="ac02c-160">此架構會嘗試對集合中的每個路由呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法，藉以比對傳入要求 URL 與集合中的路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-160">The framework attempts to match an incoming request URL to a route in the collection by calling the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in the collection.</span></span> <span data-ttu-id="ac02c-161">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ac02c-161">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>

<span data-ttu-id="ac02c-162">路由系統具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="ac02c-162">The routing system has the following characteristics:</span></span>

* <span data-ttu-id="ac02c-163">使用路由範本語法以 Token 化路由參數來定義路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-163">Route template syntax is used to define routes with tokenized route parameters.</span></span>
* <span data-ttu-id="ac02c-164">允許傳統式和屬性式端點組態。</span><span class="sxs-lookup"><span data-stu-id="ac02c-164">Conventional-style and attribute-style endpoint configuration is permitted.</span></span>
* <span data-ttu-id="ac02c-165">`IRouteConstraint` 可用來判斷 URL 參數是否包含對指定端點條件約束有效的值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-165">`IRouteConstraint` is used to determine whether a URL parameter contains a valid value for a given endpoint constraint.</span></span>
* <span data-ttu-id="ac02c-166">應用程式模型 (例如 MVC/Razor Pages) 會註冊其所有路由，這些路由的路由情節實作符合預期。</span><span class="sxs-lookup"><span data-stu-id="ac02c-166">App models, such as MVC/Razor Pages, register all of their routes, which have a predictable implementation of routing scenarios.</span></span>
* <span data-ttu-id="ac02c-167">回應可以根據路由資訊使用路由來產生 URL (例如，針對重新導向或連結)，因此避免硬式編碼的 URL，這有助於可維護性。</span><span class="sxs-lookup"><span data-stu-id="ac02c-167">A response can use routing to generate URLs (for example, for redirection or links) based on route information and thus avoid hard-coded URLs, which helps maintainability.</span></span>
* <span data-ttu-id="ac02c-168">URL 是根據支援任意擴充性的路由所產生。</span><span class="sxs-lookup"><span data-stu-id="ac02c-168">URL generation is based on routes, which support arbitrary extensibility.</span></span> <span data-ttu-id="ac02c-169">`IUrlHelper` 提供方法來建立 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-169">`IUrlHelper` offers methods to build URLs.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-170">路由會透過 <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> 類別連線到[中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="ac02c-170">Routing is connected to the [middleware](xref:fundamentals/middleware/index) pipeline by the <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> class.</span></span> <span data-ttu-id="ac02c-171">[ASP.NET Core MVC](xref:mvc/overview) 會將路由新增至中介軟體管線，作為其組態的一部分，並處理 MVC 和 Razor Pages 應用程式中的路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-171">[ASP.NET Core MVC](xref:mvc/overview) adds routing to the middleware pipeline as part of its configuration and handles routing in MVC and Razor Pages apps.</span></span> <span data-ttu-id="ac02c-172">若要了解如何使用路由作為獨立元件，請參閱[使用路由中介軟體](#use-routing-middleware)一節。</span><span class="sxs-lookup"><span data-stu-id="ac02c-172">To learn how to use routing as a standalone component, see the [Use Routing Middleware](#use-routing-middleware) section.</span></span>

### <a name="url-matching"></a><span data-ttu-id="ac02c-173">URL 比對</span><span class="sxs-lookup"><span data-stu-id="ac02c-173">URL matching</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-174">URL 比對是路由用來將傳入要求分派給「端點」的處理序。</span><span class="sxs-lookup"><span data-stu-id="ac02c-174">URL matching is the process by which routing dispatches an incoming request to an *endpoint*.</span></span> <span data-ttu-id="ac02c-175">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ac02c-175">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="ac02c-176">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="ac02c-176">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="ac02c-177">端點路由中的路由系統負責制定所有分派決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-177">The routing system in endpoint routing is responsible for all dispatching decisions.</span></span> <span data-ttu-id="ac02c-178">由於中介軟體會根據所選取的端點來套用原則，因此請務必在路由系統內制定可能影響分派或應用安全性原則的任何決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-178">Since the middleware applies policies based on the selected endpoint, it's important that any decision that can affect dispatching or the application of security policies is made inside the routing system.</span></span>

<span data-ttu-id="ac02c-179">執行端點委派時，請依據到目前為止執行的要求處理，將 `RouteContext.RouteData` 的屬性設定為適當值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-179">When the endpoint delegate is executed, the properties of `RouteContext.RouteData` are set to appropriate values based on the request processing performed thus far.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-180">URL 比對是路由用來將傳入要求分派給「處理常式」的處理序。</span><span class="sxs-lookup"><span data-stu-id="ac02c-180">URL matching is the process by which routing dispatches an incoming request to a *handler*.</span></span> <span data-ttu-id="ac02c-181">這個處理序是基於 URL 路徑中的資料，但是可以擴展為考慮要求中的任何資料。</span><span class="sxs-lookup"><span data-stu-id="ac02c-181">This process is based on data in the URL path but can be extended to consider any data in the request.</span></span> <span data-ttu-id="ac02c-182">分派要求給不同處理常式的能力，是調整應用程式大小和複雜度的關鍵。</span><span class="sxs-lookup"><span data-stu-id="ac02c-182">The ability to dispatch requests to separate handlers is key to scaling the size and complexity of an app.</span></span>

<span data-ttu-id="ac02c-183">傳入要求將進入 `RouterMiddleware`，而後者會依序在每個路由上呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 方法。</span><span class="sxs-lookup"><span data-stu-id="ac02c-183">Incoming requests enter the `RouterMiddleware`, which calls the <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> method on each route in sequence.</span></span> <span data-ttu-id="ac02c-184"><xref:Microsoft.AspNetCore.Routing.IRouter> 執行個體可將 [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) 設定為非 Null 的 <xref:Microsoft.AspNetCore.Http.RequestDelegate>，來選擇是否要「處理」要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-184">The <xref:Microsoft.AspNetCore.Routing.IRouter> instance chooses whether to *handle* the request by setting the [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) to a non-null <xref:Microsoft.AspNetCore.Http.RequestDelegate>.</span></span> <span data-ttu-id="ac02c-185">如果路由為要求設定了處理常式，則路由處理會停止，且會叫用該處理常式來處理要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-185">If a route sets a handler for the request, route processing stops, and the handler is invoked to process the request.</span></span> <span data-ttu-id="ac02c-186">如果找不到處理要求的路由處理常式，中介軟體會將要求傳遞給要求管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ac02c-186">If no route handler is found to process the request, the middleware hands the request off to the next middleware in the request pipeline.</span></span>

<span data-ttu-id="ac02c-187">`RouteAsync` 的主要輸入是與目前要求建立關聯的 [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-187">The primary input to `RouteAsync` is the [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*) associated with the current request.</span></span> <span data-ttu-id="ac02c-188">`RouteContext.Handler` 和 [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) 是路由相符之後設定的輸出。</span><span class="sxs-lookup"><span data-stu-id="ac02c-188">The `RouteContext.Handler` and [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) are outputs set after a route is matched.</span></span>

<span data-ttu-id="ac02c-189">呼叫 `RouteAsync` 的相符項目也會依據到目前為止執行的要求處理，將 `RouteContext.RouteData` 的屬性設定為適當值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-189">A match that calls `RouteAsync` also sets the properties of the `RouteContext.RouteData` to appropriate values based on the request processing performed thus far.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-190">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) 是「路由值」的字典，而路由值產生自路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-190">[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) is a dictionary of *route values* produced from the route.</span></span> <span data-ttu-id="ac02c-191">這些值通常是透過將 URL 語彙基元化來決定，可以用來接受使用者輸入，或在應用程式內做出進一步的分派決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-191">These values are usually determined by tokenizing the URL and can be used to accept user input or to make further dispatching decisions inside the app.</span></span>

<span data-ttu-id="ac02c-192">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 是其他資料的屬性包，而這些資料與相符路由相關。</span><span class="sxs-lookup"><span data-stu-id="ac02c-192">[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) is a property bag of additional data related to the matched route.</span></span> <span data-ttu-id="ac02c-193">提供了 `DataTokens` 來支援與每個路由建立關聯的狀態資料，因此應用程式可以依據符合哪一個路由來制定決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-193">`DataTokens` are provided to support associating state data with each route so that the app can make decisions based on which route matched.</span></span> <span data-ttu-id="ac02c-194">這些是開發人員定義的值，**不會**以任何方式影響路由的行為。</span><span class="sxs-lookup"><span data-stu-id="ac02c-194">These values are developer-defined and do **not** affect the behavior of routing in any way.</span></span> <span data-ttu-id="ac02c-195">此外，隱藏於 `RouteData.DataTokens` 的值可以為任何類型，相較於 `RouteData.Values`，其必須能夠來回轉換字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-195">Additionally, values stashed in `RouteData.DataTokens` can be of any type, in contrast to `RouteData.Values`, which must be convertible to and from strings.</span></span>

<span data-ttu-id="ac02c-196">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) 是成功符合要求的參與路由清單。</span><span class="sxs-lookup"><span data-stu-id="ac02c-196">[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) is a list of the routes that took part in successfully matching the request.</span></span> <span data-ttu-id="ac02c-197">路由可以用巢狀方式置於彼此內部。</span><span class="sxs-lookup"><span data-stu-id="ac02c-197">Routes can be nested inside of one another.</span></span> <span data-ttu-id="ac02c-198">`Routers` 屬性會透過導致產生相符項目的路由邏輯樹狀結構反映路徑。</span><span class="sxs-lookup"><span data-stu-id="ac02c-198">The `Routers` property reflects the path through the logical tree of routes that resulted in a match.</span></span> <span data-ttu-id="ac02c-199">一般而言，`Routers` 中的第一個項目是路由集合，應該用於產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-199">Generally, the first item in `Routers` is the route collection and should be used for URL generation.</span></span> <span data-ttu-id="ac02c-200">`Routers` 中的最後一個項目是相符的路由處理常式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-200">The last item in `Routers` is the route handler that matched.</span></span>

### <a name="url-generation"></a><span data-ttu-id="ac02c-201">URL 產生</span><span class="sxs-lookup"><span data-stu-id="ac02c-201">URL generation</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-202">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="ac02c-202">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="ac02c-203">這可讓您在端點和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ac02c-203">This allows for a logical separation between your endpoints and the URLs that access them.</span></span>

<span data-ttu-id="ac02c-204">端點路由包含連結產生器 API (`LinkGenerator`)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-204">Endpoint routing includes the Link Generator API (`LinkGenerator`).</span></span> <span data-ttu-id="ac02c-205">`LinkGenerator` 是可從 DI 擷取的單一服務。</span><span class="sxs-lookup"><span data-stu-id="ac02c-205">`LinkGenerator` is a singleton service that can be retrieved from DI.</span></span> <span data-ttu-id="ac02c-206">您可以在執行要求內容外部使用此 API。</span><span class="sxs-lookup"><span data-stu-id="ac02c-206">The API can be used outside of the context of an executing request.</span></span> <span data-ttu-id="ac02c-207">MVC 的 `IUrlHelper` 及依賴 `IUrlHelper` 的情節 (例如[標籤協助程式](xref:mvc/views/tag-helpers/intro)、HTML 協助程式和[動作結果](xref:mvc/controllers/actions)) 均使用連結產生器來提供連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ac02c-207">MVC's `IUrlHelper` and scenarios that rely on `IUrlHelper`, such as [Tag Helpers](xref:mvc/views/tag-helpers/intro), HTML Helpers, and [Action Results](xref:mvc/controllers/actions), use the link generator to provide link generating capabilities.</span></span>

<span data-ttu-id="ac02c-208">連結產生器背後支援的概念為「位址」和「位址配置」。</span><span class="sxs-lookup"><span data-stu-id="ac02c-208">The link generator is backed by the concept of an *address* and *address schemes*.</span></span> <span data-ttu-id="ac02c-209">位址配置可讓您判斷應考慮用於連結產生的端點。</span><span class="sxs-lookup"><span data-stu-id="ac02c-209">An address scheme is a way of determining the endpoints that should be considered for link generation.</span></span> <span data-ttu-id="ac02c-210">例如，MVC/Razor Pages 中許多使用者所熟悉的路由名稱和路由值情節，都會實作為位址配置。</span><span class="sxs-lookup"><span data-stu-id="ac02c-210">For example, the route name and route values scenarios many users are familiar with from MVC/Razor Pages are implemented as an address scheme.</span></span>

<span data-ttu-id="ac02c-211">連結產生器可以透過下列擴充方法，連結至 MVC/Razor Pages 動作和頁面：</span><span class="sxs-lookup"><span data-stu-id="ac02c-211">The link generator can link to MVC/Razor Pages actions and pages via the following extension methods:</span></span>

* `GetPathByAction`
* `GetUriByAction`
* `GetPathByPage`
* `GetUriByPage`

<span data-ttu-id="ac02c-212">這些方法的多載接受包含 `HttpContext` 的引數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-212">An overload of these methods accepts arguments that include the `HttpContext`.</span></span> <span data-ttu-id="ac02c-213">這些方法的功能等同於 `Url.Action` 和 `Url.Page`，但提供更多彈性和選項。</span><span class="sxs-lookup"><span data-stu-id="ac02c-213">These methods are functionally equivalent to `Url.Action` and `Url.Page` but offer additional flexibility and options.</span></span>

<span data-ttu-id="ac02c-214">`GetPath*` 方法與 `Url.Action` 和 `Url.Page` 最類似，因為它們會產生包含絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-214">The `GetPath*` methods are most similar to `Url.Action` and `Url.Page` in that they generate a URI containing an absolute path.</span></span> <span data-ttu-id="ac02c-215">`GetUri*` 方法一律會產生包含配置和主機的絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-215">The `GetUri*` methods always generate an absolute URI containing a scheme and host.</span></span> <span data-ttu-id="ac02c-216">接受 `HttpContext` 的方法會在執行要求的內容中產生 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-216">The methods that accept an `HttpContext` generate a URI in the context of the executing request.</span></span> <span data-ttu-id="ac02c-217">除非遭到覆寫，否則會使用來自執行要求的環境路由值、URL 基底路徑、配置和主機。</span><span class="sxs-lookup"><span data-stu-id="ac02c-217">The ambient route values, URL base path, scheme, and host from the executing request are used unless overridden.</span></span>

<span data-ttu-id="ac02c-218">呼叫 `LinkGenerator` 並指定一個位址。</span><span class="sxs-lookup"><span data-stu-id="ac02c-218">`LinkGenerator` is called with an address.</span></span> <span data-ttu-id="ac02c-219">執行下列兩個步驟來產生 URI：</span><span class="sxs-lookup"><span data-stu-id="ac02c-219">Generating a URI occurs in two steps:</span></span>

1. <span data-ttu-id="ac02c-220">將位址繫結至符合該位址的端點清單。</span><span class="sxs-lookup"><span data-stu-id="ac02c-220">An address is bound to a list of endpoints that match the address.</span></span>
1. <span data-ttu-id="ac02c-221">評估每個端點的 `RoutePattern`，直到找到符合所提供值的路由模式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-221">Each endpoint's `RoutePattern` is evaluated until a route pattern that matches the supplied values is found.</span></span> <span data-ttu-id="ac02c-222">產生的輸出會與提供給連結產生器的其他 URI 組件合併並傳回。</span><span class="sxs-lookup"><span data-stu-id="ac02c-222">The resulting output is combined with the other URI parts supplied to the link generator and returned.</span></span>

<span data-ttu-id="ac02c-223">`LinkGenerator` 提供的方法支援適用於任何位址類型的標準連結產生功能。</span><span class="sxs-lookup"><span data-stu-id="ac02c-223">The methods provided by `LinkGenerator` support standard link generation capabilities for any type of address.</span></span> <span data-ttu-id="ac02c-224">使用連結產生器的最便利方式是透過執行特定位址類型作業的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ac02c-224">The most convenient way to use the link generator is through extension methods that perform operations for a specific address type.</span></span>

| <span data-ttu-id="ac02c-225">擴充方法</span><span class="sxs-lookup"><span data-stu-id="ac02c-225">Extension Method</span></span>   | <span data-ttu-id="ac02c-226">描述</span><span class="sxs-lookup"><span data-stu-id="ac02c-226">Description</span></span>                                                         |
| ------------------ | ------------------------------------------------------------------- |
| `GetPathByAddress` | <span data-ttu-id="ac02c-227">根據提供的值產生具有絕對路徑的 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-227">Generates a URI with an absolute path based on the provided values.</span></span> |
| `GetUriByAddress`  | <span data-ttu-id="ac02c-228">根據提供的值產生絕對 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-228">Generates an absolute URI based on the provided values.</span></span>             |

> [!WARNING]
> <span data-ttu-id="ac02c-229">注意呼叫 `LinkGenerator` 方法的下列影響：</span><span class="sxs-lookup"><span data-stu-id="ac02c-229">Pay attention to the following implications of calling `LinkGenerator` methods:</span></span>
>
> * <span data-ttu-id="ac02c-230">使用 `GetUri*` 擴充方法，並注意應用程式組態不會驗證傳入要求的 `Host` 標頭。</span><span class="sxs-lookup"><span data-stu-id="ac02c-230">Use `GetUri*` extension methods with caution in an app configuration that doesn't validate the `Host` header of incoming requests.</span></span> <span data-ttu-id="ac02c-231">如果傳入要求的 `Host` 標頭未經驗證，則可能將未受信任的要求輸入傳回檢視/頁面 URI 中的用戶端。</span><span class="sxs-lookup"><span data-stu-id="ac02c-231">If the `Host` header of incoming requests isn't validated, untrusted request input can be sent back to the client in URIs in a view/page.</span></span> <span data-ttu-id="ac02c-232">建議所有生產應用程式將其伺服器設定為驗證 `Host` 標頭是否為已知有效值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-232">We recommend that all production apps configure their server to validate the `Host` header against known valid values.</span></span>
>
> * <span data-ttu-id="ac02c-233">使用 `LinkGenerator`，並注意與 `Map` 或 `MapWhen` 搭配使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ac02c-233">Use `LinkGenerator` with caution in middleware in combination with `Map` or `MapWhen`.</span></span> <span data-ttu-id="ac02c-234">`Map*` 會變更執行要求的基底路徑，這會影響連結產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="ac02c-234">`Map*` changes the base path of the executing request, which affects the output of link generation.</span></span> <span data-ttu-id="ac02c-235">所有的 `LinkGenerator` API 都允許指定基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ac02c-235">All of the `LinkGenerator` APIs allow specifying a base path.</span></span> <span data-ttu-id="ac02c-236">請一律指定空白基底路徑來恢復 `Map*` 對連結產生的影響。</span><span class="sxs-lookup"><span data-stu-id="ac02c-236">Always specify an empty base path to undo `Map*`'s affect on link generation.</span></span>

## <a name="differences-from-earlier-versions-of-routing"></a><span data-ttu-id="ac02c-237">與舊版路由的差異</span><span class="sxs-lookup"><span data-stu-id="ac02c-237">Differences from earlier versions of routing</span></span>

<span data-ttu-id="ac02c-238">ASP.NET Core 2.2 或更新版本中的端點路由與 ASP.NET Core 中的舊版路由之間有一些差異：</span><span class="sxs-lookup"><span data-stu-id="ac02c-238">A few differences exist between endpoint routing in ASP.NET Core 2.2 or later and earlier versions of routing in ASP.NET Core:</span></span>

* <span data-ttu-id="ac02c-239">端點路由系統不支援以 `IRouter` 為基礎的擴充性，包括從 `Route` 繼承。</span><span class="sxs-lookup"><span data-stu-id="ac02c-239">The endpoint routing system doesn't support `IRouter`-based extensibility, including inheriting from `Route`.</span></span>

* <span data-ttu-id="ac02c-240">端點路由不支援 [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-240">Endpoint routing doesn't support [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim).</span></span> <span data-ttu-id="ac02c-241">請使用 2.1 [相容性版本](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) 以繼續使用相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="ac02c-241">Use the 2.1 [compatibility version](xref:mvc/compatibility-version) (`.SetCompatibilityVersion(CompatibilityVersion.Version_2_1)`) to continue using the compatibility shim.</span></span>

* <span data-ttu-id="ac02c-242">端點路由與使用傳統路由時所產生 URI 大小寫的行為不同。</span><span class="sxs-lookup"><span data-stu-id="ac02c-242">Endpoint Routing has different behavior for the casing of generated URIs when using conventional routes.</span></span>

  <span data-ttu-id="ac02c-243">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="ac02c-243">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="ac02c-244">假設您使用下列路由來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="ac02c-244">Suppose you generate a link to an action using the following route:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "blog", new { id = 17, });
  ```

  <span data-ttu-id="ac02c-245">使用以 `IRouter` 為基礎的路由，此程式碼會產生 `/blog/ReadPost/17` 的 URI，其遵守所提供路由值的大小寫。</span><span class="sxs-lookup"><span data-stu-id="ac02c-245">With `IRouter`-based routing, this code generates a URI of `/blog/ReadPost/17`, which respects the casing of the provided route value.</span></span> <span data-ttu-id="ac02c-246">ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17` ("Blog" 為大寫)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-246">Endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` ("Blog" is capitalized).</span></span> <span data-ttu-id="ac02c-247">端點路由提供 `IOutboundParameterTransformer` 介面，可用來全域自訂此行為，或為對應 URL 套用不同的慣例。</span><span class="sxs-lookup"><span data-stu-id="ac02c-247">Endpoint routing provides the `IOutboundParameterTransformer` interface that can be used to customize this behavior globally or to apply different conventions for mapping URLs.</span></span>

  <span data-ttu-id="ac02c-248">如需詳細資訊，請參閱[參數轉換器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ac02c-248">For more information, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

* <span data-ttu-id="ac02c-249">當嘗試連結至不存在的控制器/動作或頁面時，MVC/Razor Pages 所使用的連結產生與傳統路由的行為不同。</span><span class="sxs-lookup"><span data-stu-id="ac02c-249">Link Generation used by MVC/Razor Pages with conventional routes behaves differently when attempting to link to an controller/action or page that doesn't exist.</span></span>

  <span data-ttu-id="ac02c-250">請考慮下列預設路由範本：</span><span class="sxs-lookup"><span data-stu-id="ac02c-250">Consider the following default route template:</span></span>

  ```csharp
  app.UseMvc(routes =>
  {
      routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
  });
  ```

  <span data-ttu-id="ac02c-251">假設您使用預設範本和下列程式碼來產生動作連結：</span><span class="sxs-lookup"><span data-stu-id="ac02c-251">Suppose you generate a link to an action using the default template with the following:</span></span>

  ```csharp
  var link = Url.Action("ReadPost", "Blog", new { id = 17, });
  ```

  <span data-ttu-id="ac02c-252">使用以 `IRouter` 為基礎的路由，結果一律為 `/Blog/ReadPost/17`，即使 `BlogController` 不存在或沒有 `ReadPost` 動作方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="ac02c-252">With `IRouter`-based routing, the result is always `/Blog/ReadPost/17`, even if the `BlogController` doesn't exist or doesn't have a `ReadPost` action method.</span></span> <span data-ttu-id="ac02c-253">如預期，如果動作方法存在，則 ASP.NET Core 2.2 或更新版本中的端點路由會產生 `/Blog/ReadPost/17`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-253">As expected, endpoint routing in ASP.NET Core 2.2 or later produces `/Blog/ReadPost/17` if the action method exists.</span></span> <span data-ttu-id="ac02c-254">不過，如果動作不存在，則端點路由會產生空字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-254">*However, endpoint routing produces an empty string if the action doesn't exist.*</span></span> <span data-ttu-id="ac02c-255">就概念而言，如果動作不存在，則端點路由不會假設端點存在。</span><span class="sxs-lookup"><span data-stu-id="ac02c-255">Conceptually, endpoint routing doesn't assume that the endpoint exists if the action doesn't exist.</span></span>

* <span data-ttu-id="ac02c-256">連結產生「環境值失效演算法」在搭配端點路由使用時會有不同的行為。</span><span class="sxs-lookup"><span data-stu-id="ac02c-256">The link generation *ambient value invalidation algorithm* behaves differently when used with endpoint routing.</span></span>

  <span data-ttu-id="ac02c-257">「環境值失效」是一種演算法，會從目前執行的要求 (環境值) 決定可用於連結產生作業的路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-257">*Ambient value invalidation* is the algorithm that decides which route values from the currently executing request (the ambient values) can be used in link generation operations.</span></span> <span data-ttu-id="ac02c-258">傳統路由一律會在連結至其他動作時，使額外的路由值失效。</span><span class="sxs-lookup"><span data-stu-id="ac02c-258">Conventional routing always invalidated extra route values when linking to a different action.</span></span> <span data-ttu-id="ac02c-259">在 ASP.NET Core 2.2 版以前，屬性路由沒有此行為。</span><span class="sxs-lookup"><span data-stu-id="ac02c-259">Attribute routing didn't have this behavior prior to the release of ASP.NET Core 2.2.</span></span> <span data-ttu-id="ac02c-260">在舊版的 ASP.NET Core 中，連結至使用相同路由參數名稱的其他動作會導致連結產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ac02c-260">In earlier versions of ASP.NET Core, links to another action that use the same route parameter names resulted in link generation errors.</span></span> <span data-ttu-id="ac02c-261">在 ASP.NET Core 2.2 或更新版本中，這兩種路由形式都會在連結至其他動作時使值失效。</span><span class="sxs-lookup"><span data-stu-id="ac02c-261">In ASP.NET Core 2.2 or later, both forms of routing invalidate values when linking to another action.</span></span>

  <span data-ttu-id="ac02c-262">請考慮 ASP.NET Core 2.1 或更舊版本中的下列範例。</span><span class="sxs-lookup"><span data-stu-id="ac02c-262">Consider the following example in ASP.NET Core 2.1 or earlier.</span></span> <span data-ttu-id="ac02c-263">連結至其他動作 (或其他頁面) 時，路由值可能會不適當地重複使用。</span><span class="sxs-lookup"><span data-stu-id="ac02c-263">When linking to another action (or another page), route values can be reused in undesirable ways.</span></span>

  <span data-ttu-id="ac02c-264">在 */Pages/Store/Product.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="ac02c-264">In */Pages/Store/Product.cshtml*:</span></span>

  ```cshtml
  @page "{id}"
  @Url.Page("/Login")
  ```

  <span data-ttu-id="ac02c-265">在 */Pages/Login.cshtml* 中：</span><span class="sxs-lookup"><span data-stu-id="ac02c-265">In */Pages/Login.cshtml*:</span></span>

  ```cshtml
  @page "{id?}"
  ```

  <span data-ttu-id="ac02c-266">如果 URI 在 ASP.NET Core 2.1 或更舊版本中為 `/Store/Product/18`，則 `@Url.Page("/Login")` 在 Store/Info 頁面中產生的連結為 `/Login/18`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-266">If the URI is `/Store/Product/18` in ASP.NET Core 2.1 or earlier, the link generated on the Store/Info page by `@Url.Page("/Login")` is `/Login/18`.</span></span> <span data-ttu-id="ac02c-267">這會重複使用 `id` 值 18，即使連結目的地是完全不同的應用程式組件也一樣。</span><span class="sxs-lookup"><span data-stu-id="ac02c-267">The `id` value of 18 is reused, even though the link destination is different part of the app entirely.</span></span> <span data-ttu-id="ac02c-268">`/Login` 頁面內容中的 `id` 路由值可能是使用者識別碼值，而不是市集產品識別碼值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-268">The `id` route value in the context of the `/Login` page is probably a user ID value, not a store product ID value.</span></span>

  <span data-ttu-id="ac02c-269">在 ASP.NET Core 2.2 或更新版本中的端點路由中，結果為 `/Login`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-269">In endpoint routing with ASP.NET Core 2.2 or later, the result is `/Login`.</span></span> <span data-ttu-id="ac02c-270">當連結的目的地是不同的動作或頁面時，不會重複使用環境值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-270">Ambient values aren't reused when the linked destination is a different action or page.</span></span>

* <span data-ttu-id="ac02c-271">往返路由參數語法：使用雙星號 (`**`) catch-all 參數語法時，斜線不會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="ac02c-271">Round-tripping route parameter syntax: Forward slashes aren't encoded when using a double-asterisk (`**`) catch-all parameter syntax.</span></span>

  <span data-ttu-id="ac02c-272">在連結產生期間，除了斜線，路由系統還會對雙星號 (`**`) catch-all 參數 (例如 `{**myparametername}`) 中擷取的值進行編碼。</span><span class="sxs-lookup"><span data-stu-id="ac02c-272">During link generation, the routing system encodes the value captured in a double-asterisk (`**`) catch-all parameter (for example, `{**myparametername}`) except the forward slashes.</span></span> <span data-ttu-id="ac02c-273">ASP.NET Core 2.2 或更新版本中以 `IRouter` 為基礎的路由支援雙星號 catch-all。</span><span class="sxs-lookup"><span data-stu-id="ac02c-273">The double-asterisk catch-all is supported with `IRouter`-based routing in ASP.NET Core 2.2 or later.</span></span>

  <span data-ttu-id="ac02c-274">舊版 ASP.NET Core 中的單一星號 catch-all (`{*myparametername}`) 參數語法仍會受到支援，而且斜線會經過編碼。</span><span class="sxs-lookup"><span data-stu-id="ac02c-274">The single asterisk catch-all parameter syntax in prior versions of ASP.NET Core (`{*myparametername}`) remains supported, and forward slashes are encoded.</span></span>

  | <span data-ttu-id="ac02c-275">路由</span><span class="sxs-lookup"><span data-stu-id="ac02c-275">Route</span></span>              | <span data-ttu-id="ac02c-276">以下列項目產生的連結：</span><span class="sxs-lookup"><span data-stu-id="ac02c-276">Link generated with</span></span><br>`Url.Action(new { category = "admin/products" })`&hellip; |
  | ------------------ | --------------------------------------------------------------------- |
  | `/search/{*page}`  | <span data-ttu-id="ac02c-277">`/search/admin%2Fproducts` (斜線會經過編碼)</span><span class="sxs-lookup"><span data-stu-id="ac02c-277">`/search/admin%2Fproducts` (the forward slash is encoded)</span></span>             |
  | `/search/{**page}` | `/search/admin/products`                                              |

### <a name="middleware-example"></a><span data-ttu-id="ac02c-278">中介軟體範例</span><span class="sxs-lookup"><span data-stu-id="ac02c-278">Middleware example</span></span>

<span data-ttu-id="ac02c-279">在下列範例中，中介軟體使用 `LinkGenerator` API 建立列出市集產品的動作方法連結。</span><span class="sxs-lookup"><span data-stu-id="ac02c-279">In the following example, a middleware uses the `LinkGenerator` API to create link to an action method that lists store products.</span></span> <span data-ttu-id="ac02c-280">應用程式中的任何類別都可以使用連結產生器，方法是將它插入類別並呼叫 `GenerateLink`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-280">Using the link generator by injecting it into a class and calling `GenerateLink` is available to any class in an app.</span></span>

```csharp
public class ProductsLinkMiddleware
{
    private readonly LinkGenerator _linkGenerator;

    public ProductsLinkMiddleware(RequestDelegate next, LinkGenerator linkGenerator)
    {
        _linkGenerator = linkGenerator;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        var url = _linkGenerator.GenerateLink(new { controller = "Store",
                                                    action = "ListProducts" });

        httpContext.Response.ContentType = "text/plain";

        await httpContext.Response.WriteAsync($"Go to {url} to see our products.");
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-281">URL 產生是路由可用來依據一組路由值建立 URL 路徑的處理序。</span><span class="sxs-lookup"><span data-stu-id="ac02c-281">URL generation is the process by which routing can create a URL path based on a set of route values.</span></span> <span data-ttu-id="ac02c-282">這可讓您在路由處理常式和存取它們的 URL 之間建立邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ac02c-282">This allows for a logical separation between route handlers and the URLs that access them.</span></span>

<span data-ttu-id="ac02c-283">URL 產生遵循類似的反覆執行處理序，但開頭是呼叫路由集合 <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> 方法的使用者或架構程式碼。</span><span class="sxs-lookup"><span data-stu-id="ac02c-283">URL generation follows a similar iterative process, but it starts with user or framework code calling into the <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*> method of the route collection.</span></span> <span data-ttu-id="ac02c-284">每個「路由」會依序呼叫其 `GetVirtualPath` 方法，直到傳回非 Null 的 <xref:Microsoft.AspNetCore.Routing.VirtualPathData> 為止。</span><span class="sxs-lookup"><span data-stu-id="ac02c-284">Each *route* has its `GetVirtualPath` method called in sequence until a non-null <xref:Microsoft.AspNetCore.Routing.VirtualPathData> is returned.</span></span>

<span data-ttu-id="ac02c-285">`GetVirtualPath` 的主要輸入是：</span><span class="sxs-lookup"><span data-stu-id="ac02c-285">The primary inputs to `GetVirtualPath` are:</span></span>

* [<span data-ttu-id="ac02c-286">VirtualPathContext.HttpContext</span><span class="sxs-lookup"><span data-stu-id="ac02c-286">VirtualPathContext.HttpContext</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [<span data-ttu-id="ac02c-287">VirtualPathContext.Values</span><span class="sxs-lookup"><span data-stu-id="ac02c-287">VirtualPathContext.Values</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [<span data-ttu-id="ac02c-288">VirtualPathContext.AmbientValues</span><span class="sxs-lookup"><span data-stu-id="ac02c-288">VirtualPathContext.AmbientValues</span></span>](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

<span data-ttu-id="ac02c-289">路由主要使用 `Values` 和 `AmbientValues` 所提供的路由值，以決定是否可能產生 URL，以及要包含哪些值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-289">Routes primarily use the route values provided by `Values` and `AmbientValues` to decide whether it's possible to generate a URL and what values to include.</span></span> <span data-ttu-id="ac02c-290">`AmbientValues` 是比對目前要求所產生的路由值集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-290">The `AmbientValues` are the set of route values that were produced from matching the current request.</span></span> <span data-ttu-id="ac02c-291">相反地，`Values` 是指定如何產生目前作業所需之 URL 的路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-291">In contrast, `Values` are the route values that specify how to generate the desired URL for the current operation.</span></span> <span data-ttu-id="ac02c-292">提供了 `HttpContext`，以防路由應該取得服務或與目前內容建立關聯的其他資料。</span><span class="sxs-lookup"><span data-stu-id="ac02c-292">The `HttpContext` is provided in case a route should obtain services or additional data associated with the current context.</span></span>

> [!TIP]
> <span data-ttu-id="ac02c-293">將 [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) 視為 [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) 的覆寫項目集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-293">Think of [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) as a set of overrides for the [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*).</span></span> <span data-ttu-id="ac02c-294">URL 產生會嘗試重複使用來自目前要求的路由值，讓您使用相同路由或路由值產生連結的 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-294">URL generation attempts to reuse route values from the current request to generate URLs for links using the same route or route values.</span></span>

<span data-ttu-id="ac02c-295">`GetVirtualPath` 的輸出是 `VirtualPathData`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-295">The output of `GetVirtualPath` is a `VirtualPathData`.</span></span> <span data-ttu-id="ac02c-296">`VirtualPathData` 是 `RouteData` 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="ac02c-296">`VirtualPathData` is a parallel of `RouteData`.</span></span> <span data-ttu-id="ac02c-297">`VirtualPathData` 包含用於輸出 URL 的 `VirtualPath`，以及一些路由應該設定的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ac02c-297">`VirtualPathData` contains the `VirtualPath` for the output URL and some additional properties that should be set by the route.</span></span>

<span data-ttu-id="ac02c-298">[VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) 屬性包含路由所產生的「虛擬路徑」。</span><span class="sxs-lookup"><span data-stu-id="ac02c-298">The [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*) property contains the *virtual path* produced by the route.</span></span> <span data-ttu-id="ac02c-299">視您需求的不同，可能需要進一步處理路徑。</span><span class="sxs-lookup"><span data-stu-id="ac02c-299">Depending on your needs, you may need to process the path further.</span></span> <span data-ttu-id="ac02c-300">如果您想要以 HTML 呈現產生的 URL，請在前面加上應用程式的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="ac02c-300">If you want to render the generated URL in HTML, prepend the base path of the app.</span></span>

<span data-ttu-id="ac02c-301">[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) 是成功產生 URL 的路由參考。</span><span class="sxs-lookup"><span data-stu-id="ac02c-301">The [VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) is a reference to the route that successfully generated the URL.</span></span>

<span data-ttu-id="ac02c-302">[VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) 屬性是其他資料的字典，而這些資料與產生 URL 的路由相關。</span><span class="sxs-lookup"><span data-stu-id="ac02c-302">The [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*) properties is a dictionary of additional data related to the route that generated the URL.</span></span> <span data-ttu-id="ac02c-303">這是 [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) 的平行處理。</span><span class="sxs-lookup"><span data-stu-id="ac02c-303">This is the parallel of [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).</span></span>

::: moniker-end

### <a name="create-routes"></a><span data-ttu-id="ac02c-304">建立路由</span><span class="sxs-lookup"><span data-stu-id="ac02c-304">Create routes</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-305">路由提供 <xref:Microsoft.AspNetCore.Routing.Route> 類別作為 <xref:Microsoft.AspNetCore.Routing.IRouter> 的標準實作。</span><span class="sxs-lookup"><span data-stu-id="ac02c-305">Routing provides the <xref:Microsoft.AspNetCore.Routing.Route> class as the standard implementation of <xref:Microsoft.AspNetCore.Routing.IRouter>.</span></span> <span data-ttu-id="ac02c-306">在呼叫 <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> 時，`Route` 會使用「路由範本」語法來定義將比對 URL 路徑的模式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-306">`Route` uses the *route template* syntax to define patterns to match against the URL path when <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> is called.</span></span> <span data-ttu-id="ac02c-307">在呼叫 `GetVirtualPath` 時，`Route` 會使用相同的路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-307">`Route` uses the same route template to generate a URL when `GetVirtualPath` is called.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-308">大部分的應用程式會藉由呼叫 <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> 或其中一個 <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> 上定義的類似擴充方法來定建立路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-308">Most apps create routes by calling <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> or one of the similar extension methods defined on <xref:Microsoft.AspNetCore.Routing.IRouteBuilder>.</span></span> <span data-ttu-id="ac02c-309">任何 `IRouteBuilder` 擴充方法都會建立 <xref:Microsoft.AspNetCore.Routing.Route> 的執行個體，並將它新增至路由集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-309">Any of the `IRouteBuilder` extension methods create an instance of <xref:Microsoft.AspNetCore.Routing.Route> and add it to the route collection.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-310">`MapRoute` 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-310">`MapRoute` doesn't accept a route handler parameter.</span></span> <span data-ttu-id="ac02c-311">`MapRoute` 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-311">`MapRoute` only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="ac02c-312">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-312">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-313">`MapRoute` 不接受路由處理常式參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-313">`MapRoute` doesn't accept a route handler parameter.</span></span> <span data-ttu-id="ac02c-314">`MapRoute` 只會新增 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> 所處理的路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-314">`MapRoute` only adds routes that are handled by the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span> <span data-ttu-id="ac02c-315">預設處理常式為 `IRouter`，該處理常式可能無法處理要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-315">The default handler is an `IRouter`, and the handler might not handle the request.</span></span> <span data-ttu-id="ac02c-316">例如，ASP.NET Core MVC 通常會設定為預設處理常式，只處理符合可用控制器和動作的要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-316">For example, ASP.NET Core MVC is typically configured as a default handler that only handles requests that match an available controller and action.</span></span> <span data-ttu-id="ac02c-317">若要深入了解 MVC 中的路由功能，請參閱 <xref:mvc/controllers/routing>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-317">To learn more about routing in MVC, see <xref:mvc/controllers/routing>.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-318">下列程式碼範例是典型 ASP.NET Core MVC 路由定義所使用的 `MapRoute` 呼叫範例：</span><span class="sxs-lookup"><span data-stu-id="ac02c-318">The following code example is an example of a `MapRoute` call used by a typical ASP.NET Core MVC route definition:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ac02c-319">此範本會比對 URL 路徑，並擷取路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-319">This template matches a URL path and extracts the route values.</span></span> <span data-ttu-id="ac02c-320">例如，路徑 `/Products/Details/17` 會產生下列路由值：`{ controller = Products, action = Details, id = 17 }`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-320">For example, the path `/Products/Details/17` generates the following route values: `{ controller = Products, action = Details, id = 17 }`.</span></span>

<span data-ttu-id="ac02c-321">路由值是透過將 URL 路徑分割成區段，並比對每個區段與路由範本中的「路由參數」名稱來判定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-321">Route values are determined by splitting the URL path into segments and matching each segment with the *route parameter* name in the route template.</span></span> <span data-ttu-id="ac02c-322">路由參數為具名。</span><span class="sxs-lookup"><span data-stu-id="ac02c-322">Route parameters are named.</span></span> <span data-ttu-id="ac02c-323">參數是透過以括弧 `{ ... }` 括住參數名稱來定義。</span><span class="sxs-lookup"><span data-stu-id="ac02c-323">The parameters defined by enclosing the parameter name in braces `{ ... }`.</span></span>

<span data-ttu-id="ac02c-324">上述範本也可以比對 URL 路徑 `/` 並產生值 `{ controller = Home, action = Index }`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-324">The preceding template could also match the URL path `/` and produce the values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="ac02c-325">發生這種情況是因為 `{controller}` 和 `{action}` 路由參數有預設值，而 `id` 路由參數為選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-325">This occurs because the `{controller}` and `{action}` route parameters have default values and the `id` route parameter is optional.</span></span> <span data-ttu-id="ac02c-326">路由參數名稱之後緊接著值的等號 (`=`) 會定義參數預設值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-326">An equals sign (`=`) followed by a value after the route parameter name defines a default value for the parameter.</span></span> <span data-ttu-id="ac02c-327">路由參數名稱之後的問號 (`?`) 會定義選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-327">A question mark (`?`) after the route parameter name defines an optional parameter.</span></span>

<span data-ttu-id="ac02c-328">在路由相符時，具有預設值的路由參數一定會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-328">Route parameters with a default value *always* produce a route value when the route matches.</span></span> <span data-ttu-id="ac02c-329">如果沒有任何對應的 URL 路徑區段，選擇性參數不會產生路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-329">Optional parameters don't produce a route value if there was no corresponding URL path segment.</span></span> <span data-ttu-id="ac02c-330">如需路由範本情節和語法的詳細描述，請參閱[路由範本參考](#route-template-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ac02c-330">See the [Route template reference](#route-template-reference) section for a thorough description of route template scenarios and syntax.</span></span>

<span data-ttu-id="ac02c-331">在下列範例中，路由參數定義 `{id:int}` 會定義 `id` 路由參數的[路由條件約束](#route-constraint-reference)：</span><span class="sxs-lookup"><span data-stu-id="ac02c-331">In the following example, the route parameter definition `{id:int}` defines a [route constraint](#route-constraint-reference) for the `id` route parameter:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

<span data-ttu-id="ac02c-332">此範本會符合 `/Products/Details/17` 等 URL 路徑，但不符合 `/Products/Details/Apples`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-332">This template matches a URL path like `/Products/Details/17` but not `/Products/Details/Apples`.</span></span> <span data-ttu-id="ac02c-333">路由條件約束會實作 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint>，並檢查路由值以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ac02c-333">Route constraints implement <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> and inspect route values to verify them.</span></span> <span data-ttu-id="ac02c-334">在此範例中，路由值 `id` 必須可以轉換為整數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-334">In this example, the route value `id` must be convertible to an integer.</span></span> <span data-ttu-id="ac02c-335">如需架構所提供之路由條件約束的說明，請參閱[路由條件約束參考](#route-constraint-reference)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-335">See [route-constraint-reference](#route-constraint-reference) for an explanation of route constraints provided by the framework.</span></span>

<span data-ttu-id="ac02c-336">`MapRoute` 的其他多載接受 `constraints`、`dataTokens` 和 `defaults` 的值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-336">Additional overloads of `MapRoute` accept values for `constraints`, `dataTokens`, and `defaults`.</span></span> <span data-ttu-id="ac02c-337">這些參數通常是用來傳遞匿名類型的物件，其中匿名類型的屬性名稱符合路由參數名稱。</span><span class="sxs-lookup"><span data-stu-id="ac02c-337">The typical usage of these parameters is to pass an anonymously typed object, where the property names of the anonymous type match route parameter names.</span></span>

<span data-ttu-id="ac02c-338">下列 `MapRoute` 範例會建立對等的路由：</span><span class="sxs-lookup"><span data-stu-id="ac02c-338">The following `MapRoute` examples create equivalent routes:</span></span>

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
> <span data-ttu-id="ac02c-339">對於簡單的路由而言，定義條件約束和預設的內嵌語法可能很方便。</span><span class="sxs-lookup"><span data-stu-id="ac02c-339">The inline syntax for defining constraints and defaults can be convenient for simple routes.</span></span> <span data-ttu-id="ac02c-340">不過，內嵌語法不支援某些情節 (例如資料語彙基元)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-340">However, there are scenarios, such as data tokens, that aren't supported by inline syntax.</span></span>

<span data-ttu-id="ac02c-341">下列範例將示範一些其他情節：</span><span class="sxs-lookup"><span data-stu-id="ac02c-341">The following example demonstrates a few additional scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{**article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="ac02c-342">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-342">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="ac02c-343">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-343">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="ac02c-344">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-344">Default values can be specified in the route template.</span></span> <span data-ttu-id="ac02c-345">`article` 路由參數透過在路由參數名稱之前加上雙星號 (`**`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="ac02c-345">The `article` route parameter is defined as a *catch-all* by the appearance of an double asterisk (`**`) before the route parameter name.</span></span> <span data-ttu-id="ac02c-346">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-346">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

<span data-ttu-id="ac02c-347">上述範本會比對 `/Blog/All-About-Routing/Introduction` 等 URL 路徑，並擷取值 `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-347">The preceding template matches a URL path like `/Blog/All-About-Routing/Introduction` and extracts the values `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }`.</span></span> <span data-ttu-id="ac02c-348">即使範本中沒有任何對應的路由參數，路由也會產生 `controller` 和 `action` 的預設路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-348">The default route values for `controller` and `action` are produced by the route even though there are no corresponding route parameters in the template.</span></span> <span data-ttu-id="ac02c-349">預設值可以在路由範本中指定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-349">Default values can be specified in the route template.</span></span> <span data-ttu-id="ac02c-350">`article` 路由參數透過在路由參數名稱之前加上一個星號 (`*`) 來定義為 *catch-all*。</span><span class="sxs-lookup"><span data-stu-id="ac02c-350">The `article` route parameter is defined as a *catch-all* by the appearance of an asterisk (`*`) before the route parameter name.</span></span> <span data-ttu-id="ac02c-351">全部擷取路由參數會擷取 URL 路徑的其餘部分，而且也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-351">Catch-all route parameters capture the remainder of the URL path and can also match the empty string.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-352">下列範例會新增路由條件約束和資料語彙基元：</span><span class="sxs-lookup"><span data-stu-id="ac02c-352">The following example adds route constraints and data tokens:</span></span>

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

<span data-ttu-id="ac02c-353">上述範本會比對 `/en-US/Products/5` 等 URL 路徑，並擷取值 `{ controller = Products, action = Details, id = 5 }` 和資料語彙基元 `{ locale = en-US }`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-353">The preceding template matches a URL path like `/en-US/Products/5` and extracts the values `{ controller = Products, action = Details, id = 5 }` and the data tokens `{ locale = en-US }`.</span></span>

![[區域變數] 視窗語彙基元](routing/_static/tokens.png)

### <a name="route-class-url-generation"></a><span data-ttu-id="ac02c-355">路由類別 URL 產生</span><span class="sxs-lookup"><span data-stu-id="ac02c-355">Route class URL generation</span></span>

<span data-ttu-id="ac02c-356">`Route` 類別也可以結合一組路由值與其路由範本來產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-356">The `Route` class can also perform URL generation by combining a set of route values with its route template.</span></span> <span data-ttu-id="ac02c-357">這在邏輯上是比對 URL 路徑的反向處理序。</span><span class="sxs-lookup"><span data-stu-id="ac02c-357">This is logically the reverse process of matching the URL path.</span></span>

> [!TIP]
> <span data-ttu-id="ac02c-358">若要進一步了解 URL 產生，請假設您想要產生的 URL，並考慮路由範本將會如何比對該 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-358">To better understand URL generation, imagine what URL you want to generate and then think about how a route template would match that URL.</span></span> <span data-ttu-id="ac02c-359">產生的值為何？</span><span class="sxs-lookup"><span data-stu-id="ac02c-359">What values would be produced?</span></span> <span data-ttu-id="ac02c-360">這與 URL 產生在 `Route` 類別中的運作方式大致相同。</span><span class="sxs-lookup"><span data-stu-id="ac02c-360">This is the rough equivalent of how URL generation works in the `Route` class.</span></span>

<span data-ttu-id="ac02c-361">下列範例使用一般 ASP.NET Core MVC 預設路由：</span><span class="sxs-lookup"><span data-stu-id="ac02c-361">The following example uses a general ASP.NET Core MVC default route:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="ac02c-362">路由值為 `{ controller = Products, action = List }` 時，會產生 URL `/Products/List`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-362">With the route values `{ controller = Products, action = List }`, the URL `/Products/List` is generated.</span></span> <span data-ttu-id="ac02c-363">路由值會取代對應的路由參數，以形成 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="ac02c-363">The route values are substituted for the corresponding route parameters to form the URL path.</span></span> <span data-ttu-id="ac02c-364">由於 `id` 是選擇性路由參數，因此沒有 `id` 值也可以成功產生 URL。</span><span class="sxs-lookup"><span data-stu-id="ac02c-364">Since `id` is an optional route parameter, the URL is successfully generated without a value for `id`.</span></span>

<span data-ttu-id="ac02c-365">路由值為 `{ controller = Home, action = Index }` 時，會產生 URL `/`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-365">With the route values `{ controller = Home, action = Index }`, the URL `/` is generated.</span></span> <span data-ttu-id="ac02c-366">所提供的路由值符合預設值，因此可以放心地省略這些預設值對應的區段。</span><span class="sxs-lookup"><span data-stu-id="ac02c-366">The provided route values match the default values, and the segments corresponding to the default values are safely omitted.</span></span>

<span data-ttu-id="ac02c-367">這兩個產生的 URL 會使用下列路由定義 (`/Home/Index` 和 `/`) 反覆存取，並產生用來產生 URL 的相同路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-367">Both URLs generated round-trip with the following route definition (`/Home/Index` and `/`) produce the same route values that were used to generate the URL.</span></span>

> [!NOTE]
> <span data-ttu-id="ac02c-368">使用 ASP.NET Core MVC 的應用程式應該使用 <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> 來產生 URL，而不是直接呼叫路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-368">An app using ASP.NET Core MVC should use <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> to generate URLs instead of calling into routing directly.</span></span>

<span data-ttu-id="ac02c-369">如需 URL 產生的詳細資訊，請參閱 [URI 產生參考](#url-generation-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ac02c-369">For more information on URL generation, see the [Url generation reference](#url-generation-reference) section.</span></span>

## <a name="use-routing-middleware"></a><span data-ttu-id="ac02c-370">使用路由中介軟體</span><span class="sxs-lookup"><span data-stu-id="ac02c-370">Use Routing Middleware</span></span>

<span data-ttu-id="ac02c-371">參考應用程式專案檔中的 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-371">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in the app's project file.</span></span>

<span data-ttu-id="ac02c-372">在 `Startup.ConfigureServices` 中，將路由新增至服務容器：</span><span class="sxs-lookup"><span data-stu-id="ac02c-372">Add routing to the service container in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="ac02c-373">路由必須設定在 `Startup.Configure` 方法中。</span><span class="sxs-lookup"><span data-stu-id="ac02c-373">Routes must be configured in the `Startup.Configure` method.</span></span> <span data-ttu-id="ac02c-374">範例應用程式使用下列 API：</span><span class="sxs-lookup"><span data-stu-id="ac02c-374">The sample app uses the following APIs:</span></span>

* <xref:Microsoft.AspNetCore.Routing.RouteBuilder>
* <span data-ttu-id="ac02c-375"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; 僅符合 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-375"><xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> &ndash; Matches only HTTP GET requests.</span></span>
* <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

<span data-ttu-id="ac02c-376">下表顯示使用指定 URL 的回應。</span><span class="sxs-lookup"><span data-stu-id="ac02c-376">The following table shows the responses with the given URIs.</span></span>

| <span data-ttu-id="ac02c-377">URI</span><span class="sxs-lookup"><span data-stu-id="ac02c-377">URI</span></span>                    | <span data-ttu-id="ac02c-378">回應</span><span class="sxs-lookup"><span data-stu-id="ac02c-378">Response</span></span>                                          |
| ---------------------- | ------------------------------------------------- |
| `/package/create/3`    | <span data-ttu-id="ac02c-379">Hello!</span><span class="sxs-lookup"><span data-stu-id="ac02c-379">Hello!</span></span> <span data-ttu-id="ac02c-380">Route values: [operation, create], [id, 3]</span><span class="sxs-lookup"><span data-stu-id="ac02c-380">Route values: [operation, create], [id, 3]</span></span> |
| `/package/track/-3`    | <span data-ttu-id="ac02c-381">Hello!</span><span class="sxs-lookup"><span data-stu-id="ac02c-381">Hello!</span></span> <span data-ttu-id="ac02c-382">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ac02c-382">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/-3/`   | <span data-ttu-id="ac02c-383">Hello!</span><span class="sxs-lookup"><span data-stu-id="ac02c-383">Hello!</span></span> <span data-ttu-id="ac02c-384">Route values: [operation, track], [id, -3]</span><span class="sxs-lookup"><span data-stu-id="ac02c-384">Route values: [operation, track], [id, -3]</span></span> |
| `/package/track/`      | <span data-ttu-id="ac02c-385">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ac02c-385">The request falls through, no match.</span></span>              |
| `GET /hello/Joe`       | <span data-ttu-id="ac02c-386">Hi, Joe!</span><span class="sxs-lookup"><span data-stu-id="ac02c-386">Hi, Joe!</span></span>                                          |
| `POST /hello/Joe`      | <span data-ttu-id="ac02c-387">要求失敗，僅符合 HTTP GET。</span><span class="sxs-lookup"><span data-stu-id="ac02c-387">The request falls through, matches HTTP GET only.</span></span> |
| `GET /hello/Joe/Smith` | <span data-ttu-id="ac02c-388">要求失敗，沒有相符項目。</span><span class="sxs-lookup"><span data-stu-id="ac02c-388">The request falls through, no match.</span></span>              |

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-389">如果您要設定單一路由，請呼叫傳入 `IRouter` 執行個體的 <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-389">If you're configuring a single route, call <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> passing in an `IRouter` instance.</span></span> <span data-ttu-id="ac02c-390">您不需要使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-390">You won't need to use <xref:Microsoft.AspNetCore.Routing.RouteBuilder>.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-391">架構會提供一組擴充方法來建立路由 (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>)：</span><span class="sxs-lookup"><span data-stu-id="ac02c-391">The framework provides a set of extension methods for creating routes (<xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions>):</span></span>

* `MapDelete`
* `MapGet`
* `MapMiddlewareDelete`
* `MapMiddlewareGet`
* `MapMiddlewarePost`
* `MapMiddlewarePut`
* `MapMiddlewareRoute`
* `MapMiddlewareVerb`
* `MapPost`
* `MapPut`
* `MapRoute`
* `MapVerb`

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-392">其中一些列出的方法 (例如 `MapGet`) 需要 `RequestDelegate`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-392">Some of listed methods, such as `MapGet`, require a `RequestDelegate`.</span></span> <span data-ttu-id="ac02c-393">路由相符時，`RequestDelegate` 會作為「路由處理常式」使用。</span><span class="sxs-lookup"><span data-stu-id="ac02c-393">The `RequestDelegate` is used as the *route handler* when the route matches.</span></span> <span data-ttu-id="ac02c-394">此系列中的其他方法允許設定中介軟體管線，以作為路由處理常式使用。</span><span class="sxs-lookup"><span data-stu-id="ac02c-394">Other methods in this family allow configuring a middleware pipeline for use as the route handler.</span></span> <span data-ttu-id="ac02c-395">如果 `Map*` 方法不接受處理常式 (例如 `MapRoute`)，則會使用 <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-395">If the `Map*` method doesn't accept a handler, such as `MapRoute`, it uses the <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-396">`Map[Verb]` 方法會使用條件約束，將路由限制為方法名稱中的 HTTP 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="ac02c-396">The `Map[Verb]` methods use constraints to limit the route to the HTTP Verb in the method name.</span></span> <span data-ttu-id="ac02c-397">如需範例，請參閱 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> 與 <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-397">For example, see <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> and <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.</span></span>

## <a name="route-template-reference"></a><span data-ttu-id="ac02c-398">路由範本參考</span><span class="sxs-lookup"><span data-stu-id="ac02c-398">Route template reference</span></span>

<span data-ttu-id="ac02c-399">大括弧 (`{ ... }`) 內的語彙基元定義路由相符時會繫結的「路由參數」。</span><span class="sxs-lookup"><span data-stu-id="ac02c-399">Tokens within curly braces (`{ ... }`) define *route parameters* that are bound if the route is matched.</span></span> <span data-ttu-id="ac02c-400">您可以在路由區段中定義多個路由參數，但其必須以常值分隔。</span><span class="sxs-lookup"><span data-stu-id="ac02c-400">You can define more than one route parameter in a route segment, but they must be separated by a literal value.</span></span> <span data-ttu-id="ac02c-401">例如，`{controller=Home}{action=Index}` 不是有效的路由，因為 `{controller}` 與 `{action}` 之間沒有任何常值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-401">For example, `{controller=Home}{action=Index}` isn't a valid route, since there's no literal value between `{controller}` and `{action}`.</span></span> <span data-ttu-id="ac02c-402">這些路由參數必須有一個名稱，並且可以指定其他屬性。</span><span class="sxs-lookup"><span data-stu-id="ac02c-402">These route parameters must have a name and may have additional attributes specified.</span></span>

<span data-ttu-id="ac02c-403">路由參數之外的常值文字 (例如，`{id}`) 和路徑分隔符號 `/` 必須符合 URL 中的文字。</span><span class="sxs-lookup"><span data-stu-id="ac02c-403">Literal text other than route parameters (for example, `{id}`) and the path separator `/` must match the text in the URL.</span></span> <span data-ttu-id="ac02c-404">文字比對會區分大小寫，並以 URL 路徑的已解碼表示法為基礎。</span><span class="sxs-lookup"><span data-stu-id="ac02c-404">Text matching is case-insensitive and based on the decoded representation of the URLs path.</span></span> <span data-ttu-id="ac02c-405">若要比對常值路由參數分隔符號 (`{` 或 `}`)，請重複字元 (`{{` 或 `}}`) 來將分隔符號逸出。</span><span class="sxs-lookup"><span data-stu-id="ac02c-405">To match a literal route parameter delimiter (`{` or `}`), escape the delimiter by repeating the character (`{{` or `}}`).</span></span>

<span data-ttu-id="ac02c-406">URL 模式嘗試擷取具有選擇性副檔名的檔案名稱時，具有其他考量。</span><span class="sxs-lookup"><span data-stu-id="ac02c-406">URL patterns that attempt to capture a file name with an optional file extension have additional considerations.</span></span> <span data-ttu-id="ac02c-407">以範本 `files/{filename}.{ext?}` 為例。</span><span class="sxs-lookup"><span data-stu-id="ac02c-407">For example, consider the template `files/{filename}.{ext?}`.</span></span> <span data-ttu-id="ac02c-408">當 `filename` 和 `ext` 都存在值時，就會填入這兩個值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-408">When values for both `filename` and `ext` exist, both values are populated.</span></span> <span data-ttu-id="ac02c-409">如果 URL 中只有 `filename` 存在值，由於結尾的句點 (`.`) 是選擇性項目，因此路由相符。</span><span class="sxs-lookup"><span data-stu-id="ac02c-409">If only a value for `filename` exists in the URL, the route matches because the trailing period (`.`) is  optional.</span></span> <span data-ttu-id="ac02c-410">下列 URL 符合此路由：</span><span class="sxs-lookup"><span data-stu-id="ac02c-410">The following URLs match this route:</span></span>

* `/files/myFile.txt`
* `/files/myFile`

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-411">您可以使用一個星號 (`*`) 或雙星號 (`**`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ac02c-411">You can use an asterisk (`*`) or double asterisk (`**`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="ac02c-412">這稱為 *catch-all* 參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-412">These are called a *catch-all* parameters.</span></span> <span data-ttu-id="ac02c-413">例如，`blog/{**slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-413">For example, `blog/{**slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="ac02c-414">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-414">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ac02c-415">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="ac02c-415">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="ac02c-416">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-416">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ac02c-417">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ac02c-417">Note the escaped forward slash.</span></span> <span data-ttu-id="ac02c-418">若要反覆存取路徑分隔符號字元，請使用 `**` 路由參數前置詞。</span><span class="sxs-lookup"><span data-stu-id="ac02c-418">To round-trip path separator characters, use the `**` route parameter prefix.</span></span> <span data-ttu-id="ac02c-419">具有 `{ path = "my/path" }` 的路由 `foo/{**path}` 會產生 `foo/my/path`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-419">The route `foo/{**path}` with `{ path = "my/path" }` generates `foo/my/path`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ac02c-420">您可以使用星號 (`*`) 作為路由參數的前置詞，以繫結至 URI 的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="ac02c-420">You can use the asterisk (`*`) as a prefix to a route parameter to bind to the rest of the URI.</span></span> <span data-ttu-id="ac02c-421">這稱為「全部擷取」參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-421">This is called a *catch-all* parameter.</span></span> <span data-ttu-id="ac02c-422">例如，`blog/{*slug}` 符合以 `/blog` 開頭且其後有任何值 (這會指派給 `slug` 路由值) 的所有 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-422">For example, `blog/{*slug}` matches any URI that starts with `/blog` and has any value following it, which is assigned to the `slug` route value.</span></span> <span data-ttu-id="ac02c-423">全部擷取參數也可以符合空字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-423">Catch-all parameters can also match the empty string.</span></span>

<span data-ttu-id="ac02c-424">當使用路由產生 URL (包括路徑分隔符號 (`/`) 字元) 時，catch-all 參數會逸出適當的字元。</span><span class="sxs-lookup"><span data-stu-id="ac02c-424">The catch-all parameter escapes the appropriate characters when the route is used to generate a URL, including path separator (`/`) characters.</span></span> <span data-ttu-id="ac02c-425">例如，路由值為 `{ path = "my/path" }` 的路由 `foo/{*path}` 會產生 `foo/my%2Fpath`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-425">For example, the route `foo/{*path}` with route values `{ path = "my/path" }` generates `foo/my%2Fpath`.</span></span> <span data-ttu-id="ac02c-426">請注意逸出的斜線。</span><span class="sxs-lookup"><span data-stu-id="ac02c-426">Note the escaped forward slash.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-427">路由參數可能有「預設值」，指定方法是在參數名稱之後指定預設值，並以等號 (`=`) 分隔。</span><span class="sxs-lookup"><span data-stu-id="ac02c-427">Route parameters may have *default values* designated by specifying the default value after the parameter name separated by an equals sign (`=`).</span></span> <span data-ttu-id="ac02c-428">例如，`{controller=Home}` 定義 `Home` 作為 `controller` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-428">For example, `{controller=Home}` defines `Home` as the default value for `controller`.</span></span> <span data-ttu-id="ac02c-429">如果 URL 中沒有用於參數的任何值，則會使用預設值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-429">The default value is used if no value is present in the URL for the parameter.</span></span> <span data-ttu-id="ac02c-430">路由參數也可以設為選擇性，方法是在參數名稱結尾附加問號 (`?`)，如 `id?` 中所示。</span><span class="sxs-lookup"><span data-stu-id="ac02c-430">Route parameters are made optional by appending a question mark (`?`) to the end of the parameter name, as in `id?`.</span></span> <span data-ttu-id="ac02c-431">選擇性值與預設路由參數之間的差異在於，具有預設值的路由參數一定會產生值&mdash;選擇性參數只有在要求 URL 提供值時才會有值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-431">The difference between optional values and default route parameters is that a route parameter with a default value always produces a value&mdash;an optional parameter has a value only when a value is provided by the request URL.</span></span>

<span data-ttu-id="ac02c-432">路由參數可能具有條件約束，這些條件約束必須符合與 URL 繫結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-432">Route parameters may have constraints that must match the route value bound from the URL.</span></span> <span data-ttu-id="ac02c-433">在路由參數名稱之後新增分號 (`:`) 和條件約束名稱，即可指定路由參數的「內嵌條件約束」。</span><span class="sxs-lookup"><span data-stu-id="ac02c-433">Adding a colon (`:`) and constraint name after the route parameter name specifies an *inline constraint* on a route parameter.</span></span> <span data-ttu-id="ac02c-434">如果條件約束需要引數，這些引數會在條件約束名稱後面以括弧 (`(...)`) 括住。</span><span class="sxs-lookup"><span data-stu-id="ac02c-434">If the constraint requires arguments, they're enclosed in parentheses (`(...)`) after the constraint name.</span></span> <span data-ttu-id="ac02c-435">指定多個內嵌條件約束的方法是附加另一個冒號 (`:`) 和條件約束名稱。</span><span class="sxs-lookup"><span data-stu-id="ac02c-435">Multiple inline constraints can be specified by appending another colon (`:`) and constraint name.</span></span>

<span data-ttu-id="ac02c-436">條件約束名稱和引述會傳遞至 <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> 服務來建立 <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> 的執行個體，以用於 URL 處理。</span><span class="sxs-lookup"><span data-stu-id="ac02c-436">The constraint name and arguments are passed to the <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver> service to create an instance of <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> to use in URL processing.</span></span> <span data-ttu-id="ac02c-437">例如，路由範本 `blog/{article:minlength(10)}` 指定具有引數 `10` 的 `minlength` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ac02c-437">For example, the route template `blog/{article:minlength(10)}` specifies a `minlength` constraint with the argument `10`.</span></span> <span data-ttu-id="ac02c-438">如需路由條件約束詳細資訊和架構所提供的條件約束清單，請參閱[路由條件約束參考](#route-constraint-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ac02c-438">For more information on route constraints and a list of the constraints provided by the framework, see the [Route constraint reference](#route-constraint-reference) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ac02c-439">路由參數也可以具有參數轉換器，以在產生連結及根據 URL 比對動作和頁面時，轉換參數的值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-439">Route parameters may also have parameter transformers, which transform a parameter's value when generating links and matching actions and pages to URLs.</span></span> <span data-ttu-id="ac02c-440">與條件約束類似，可以透過在路徑參數名稱後面新增冒號 (`:`) 和轉換器名稱，將參數轉換器內嵌新增至路徑參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-440">Like constraints, parameter transformers can be added inline to a route parameter by adding a colon (`:`) and transformer name after the route parameter name.</span></span> <span data-ttu-id="ac02c-441">例如，路由範本 `blog/{article:slugify}` 會指定 `slugify` 轉換器。</span><span class="sxs-lookup"><span data-stu-id="ac02c-441">For example, the route template `blog/{article:slugify}` specifies a `slugify` transformer.</span></span> <span data-ttu-id="ac02c-442">如需參數轉換器的詳細資訊，請參閱[參數轉器參考](#parameter-transformer-reference)一節。</span><span class="sxs-lookup"><span data-stu-id="ac02c-442">For more information on parameter transformers, see the [Parameter transformer reference](#parameter-transformer-reference) section.</span></span>

::: moniker-end

<span data-ttu-id="ac02c-443">下表示範範例路由範本及其行為。</span><span class="sxs-lookup"><span data-stu-id="ac02c-443">The following table demonstrates example route templates and their behavior.</span></span>

| <span data-ttu-id="ac02c-444">路由範本</span><span class="sxs-lookup"><span data-stu-id="ac02c-444">Route Template</span></span>                           | <span data-ttu-id="ac02c-445">範例比對 URI</span><span class="sxs-lookup"><span data-stu-id="ac02c-445">Example Matching URI</span></span>    | <span data-ttu-id="ac02c-446">要求 URI&hellip;</span><span class="sxs-lookup"><span data-stu-id="ac02c-446">The request URI&hellip;</span></span>                                                    |
| ---------------------------------------- | ----------------------- | -------------------------------------------------------------------------- |
| `hello`                                  | `/hello`                | <span data-ttu-id="ac02c-447">只比對單一路徑 `/hello`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-447">Only matches the single path `/hello`.</span></span>                                     |
| `{Page=Home}`                            | `/`                     | <span data-ttu-id="ac02c-448">比對並將 `Page` 設定為 `Home`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-448">Matches and sets `Page` to `Home`.</span></span>                                         |
| `{Page=Home}`                            | `/Contact`              | <span data-ttu-id="ac02c-449">比對並將 `Page` 設定為 `Contact`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-449">Matches and sets `Page` to `Contact`.</span></span>                                      |
| `{controller}/{action}/{id?}`            | `/Products/List`        | <span data-ttu-id="ac02c-450">對應至 `Products` 控制器和 `List` 動作。</span><span class="sxs-lookup"><span data-stu-id="ac02c-450">Maps to the `Products` controller and `List` action.</span></span>                       |
| `{controller}/{action}/{id?}`            | `/Products/Details/123` | <span data-ttu-id="ac02c-451">對應至 `Products` 控制器和 `Details` 動作 (`id` 設定為 123)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-451">Maps to the `Products` controller and  `Details` action (`id` set to 123).</span></span> |
| <span data-ttu-id="ac02c-452">`{controller=Home}/{action=Index}/{id?`}</span><span class="sxs-lookup"><span data-stu-id="ac02c-452">`{controller=Home}/{action=Index}/{id?`}</span></span> | `/`                     | <span data-ttu-id="ac02c-453">對應至 `Home` 控制器和 `Index` 方法 (會忽略 `id`)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-453">Maps to the `Home` controller and `Index` method (`id` is ignored).</span></span>        |

<span data-ttu-id="ac02c-454">使用範本通常是最簡單的路由方式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-454">Using a template is generally the simplest approach to routing.</span></span> <span data-ttu-id="ac02c-455">條件約束和預設值也可以在路由範本外部指定。</span><span class="sxs-lookup"><span data-stu-id="ac02c-455">Constraints and defaults can also be specified outside the route template.</span></span>

> [!TIP]
> <span data-ttu-id="ac02c-456">啟用[記錄](xref:fundamentals/logging/index)以查看內建路由實作 (例如 `Route`) 如何符合要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-456">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as `Route`, match requests.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="ac02c-457">保留的路由名稱</span><span class="sxs-lookup"><span data-stu-id="ac02c-457">Reserved routing names</span></span>

<span data-ttu-id="ac02c-458">下列關鍵字是保留的名稱，不能用作路由名稱或參數：</span><span class="sxs-lookup"><span data-stu-id="ac02c-458">The following keywords are reserved names and can't be used as route names or parameters:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a><span data-ttu-id="ac02c-459">路由條件約束參考</span><span class="sxs-lookup"><span data-stu-id="ac02c-459">Route constraint reference</span></span>

<span data-ttu-id="ac02c-460">路由條件約束執行時機是出現符合傳入 URL 的項目，並將 URL 路徑語彙基元化成路由值時。</span><span class="sxs-lookup"><span data-stu-id="ac02c-460">Route constraints execute when a match has occurred to the incoming URL and the URL path is tokenized into route values.</span></span> <span data-ttu-id="ac02c-461">路由條件約束通常會透過路由範本檢查相關聯的路由值，並對是否可接受值做出是/否決策。</span><span class="sxs-lookup"><span data-stu-id="ac02c-461">Route constraints generally inspect the route value associated via the route template and make a yes/no decision about whether or not the value is acceptable.</span></span> <span data-ttu-id="ac02c-462">某些路由條件約束會使用路由值以外的資料，以考慮是否可以路由要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-462">Some route constraints use data outside the route value to consider whether the request can be routed.</span></span> <span data-ttu-id="ac02c-463">例如，<xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> 可以依據其 HTTP 指令動詞接受或拒絕要求。</span><span class="sxs-lookup"><span data-stu-id="ac02c-463">For example, the <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> can accept or reject a request based on its HTTP verb.</span></span> <span data-ttu-id="ac02c-464">條件約束可用於路由要求和連結產生。</span><span class="sxs-lookup"><span data-stu-id="ac02c-464">Constraints are used in routing requests and link generation.</span></span>

> [!WARNING]
> <span data-ttu-id="ac02c-465">請勿針對**輸入驗證**使用條件約束。</span><span class="sxs-lookup"><span data-stu-id="ac02c-465">Don't use constraints for **input validation**.</span></span> <span data-ttu-id="ac02c-466">如果針對**輸入驗證**使用條件約束，則無效的輸入會導致產生「404 - 找不到」回應，而不是「400 - 錯誤要求」與適當的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ac02c-466">If constraints are used for **input validation**, invalid input results in a *404 - Not Found* response instead of a *400 - Bad Request* with an appropriate error message.</span></span> <span data-ttu-id="ac02c-467">路由條件約束會用來**釐清**類似的路由，而不是用來驗證特定路由的輸入。</span><span class="sxs-lookup"><span data-stu-id="ac02c-467">Route constraints are used to **disambiguate** similar routes, not to validate the inputs for a particular route.</span></span>

<span data-ttu-id="ac02c-468">下表示範範例路由條件約束及其預期行為。</span><span class="sxs-lookup"><span data-stu-id="ac02c-468">The following table demonstrates example route constraints and their expected behavior.</span></span>

| <span data-ttu-id="ac02c-469">條件約束</span><span class="sxs-lookup"><span data-stu-id="ac02c-469">constraint</span></span> | <span data-ttu-id="ac02c-470">範例</span><span class="sxs-lookup"><span data-stu-id="ac02c-470">Example</span></span> | <span data-ttu-id="ac02c-471">範例相符項目</span><span class="sxs-lookup"><span data-stu-id="ac02c-471">Example Matches</span></span> | <span data-ttu-id="ac02c-472">注意</span><span class="sxs-lookup"><span data-stu-id="ac02c-472">Notes</span></span> |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | <span data-ttu-id="ac02c-473">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ac02c-473">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ac02c-474">符合任何整數</span><span class="sxs-lookup"><span data-stu-id="ac02c-474">Matches any integer</span></span> |
| `bool` | `{active:bool}` | <span data-ttu-id="ac02c-475">`true`、 `FALSE`</span><span class="sxs-lookup"><span data-stu-id="ac02c-475">`true`, `FALSE`</span></span> | <span data-ttu-id="ac02c-476">符合 `true` 或 `false` (不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="ac02c-476">Matches `true` or `false` (case-insensitive)</span></span> |
| `datetime` | `{dob:datetime}` | <span data-ttu-id="ac02c-477">`2016-12-31`、 `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="ac02c-477">`2016-12-31`, `2016-12-31 7:32pm`</span></span> | <span data-ttu-id="ac02c-478">符合有效的 `DateTime` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="ac02c-478">Matches a valid `DateTime` value (in the invariant culture - see warning)</span></span> |
| `decimal` | `{price:decimal}` | <span data-ttu-id="ac02c-479">`49.99`、 `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="ac02c-479">`49.99`, `-1,000.01`</span></span> | <span data-ttu-id="ac02c-480">符合有效的 `decimal` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="ac02c-480">Matches a valid `decimal` value (in the invariant culture - see warning)</span></span> |
| `double` | `{weight:double}` | <span data-ttu-id="ac02c-481">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ac02c-481">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ac02c-482">符合有效的 `double` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="ac02c-482">Matches a valid `double` value (in the invariant culture - see warning)</span></span> |
| `float` | `{weight:float}` | <span data-ttu-id="ac02c-483">`1.234`、 `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="ac02c-483">`1.234`, `-1,001.01e8`</span></span> | <span data-ttu-id="ac02c-484">符合有效的 `float` 值 (在不因國別而異的文化特性中 - 請參閱警告)</span><span class="sxs-lookup"><span data-stu-id="ac02c-484">Matches a valid `float` value (in the invariant culture - see warning)</span></span> |
| `guid` | `{id:guid}` | <span data-ttu-id="ac02c-485">`CD2C1638-1638-72D5-1638-DEADBEEF1638`、 `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="ac02c-485">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="ac02c-486">符合有效的 `Guid` 值</span><span class="sxs-lookup"><span data-stu-id="ac02c-486">Matches a valid `Guid` value</span></span> |
| `long` | `{ticks:long}` | <span data-ttu-id="ac02c-487">`123456789`、 `-123456789`</span><span class="sxs-lookup"><span data-stu-id="ac02c-487">`123456789`, `-123456789`</span></span> | <span data-ttu-id="ac02c-488">符合有效的 `long` 值</span><span class="sxs-lookup"><span data-stu-id="ac02c-488">Matches a valid `long` value</span></span> |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | <span data-ttu-id="ac02c-489">字串必須至少有 4 個字元</span><span class="sxs-lookup"><span data-stu-id="ac02c-489">String must be at least 4 characters</span></span> |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | <span data-ttu-id="ac02c-490">字串不能超過 8 個字元</span><span class="sxs-lookup"><span data-stu-id="ac02c-490">String must be no more than 8 characters</span></span> |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | <span data-ttu-id="ac02c-491">字串長度必須剛好是 12 個字元</span><span class="sxs-lookup"><span data-stu-id="ac02c-491">String must be exactly 12 characters long</span></span> |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | <span data-ttu-id="ac02c-492">字串長度必須至少有 8 個字元，但不能超過 16 個字元</span><span class="sxs-lookup"><span data-stu-id="ac02c-492">String must be at least 8 and no more than 16 characters long</span></span> |
| `min(value)` | `{age:min(18)}` | `19` | <span data-ttu-id="ac02c-493">整數值必須至少為 18</span><span class="sxs-lookup"><span data-stu-id="ac02c-493">Integer value must be at least 18</span></span> |
| `max(value)` | `{age:max(120)}` | `91` | <span data-ttu-id="ac02c-494">整數值不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ac02c-494">Integer value must be no more than 120</span></span> |
| `range(min,max)` | `{age:range(18,120)}` | `91` | <span data-ttu-id="ac02c-495">整數值必須至少為 18，但不能超過 120</span><span class="sxs-lookup"><span data-stu-id="ac02c-495">Integer value must be at least 18 but no more than 120</span></span> |
| `alpha` | `{name:alpha}` | `Rick` | <span data-ttu-id="ac02c-496">字串必須包含一或多個字母字元 (`a`-`z`，不區分大小寫)</span><span class="sxs-lookup"><span data-stu-id="ac02c-496">String must consist of one or more alphabetical characters (`a`-`z`, case-insensitive)</span></span> |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | <span data-ttu-id="ac02c-497">字串必須符合規則運算式 (請參閱有關定義規則運算式的提示)</span><span class="sxs-lookup"><span data-stu-id="ac02c-497">String must match the regular expression (see tips about defining a regular expression)</span></span> |
| `required` | `{name:required}` | `Rick` | <span data-ttu-id="ac02c-498">用來強制執行在 URL 產生期間呈現非參數值</span><span class="sxs-lookup"><span data-stu-id="ac02c-498">Used to enforce that a non-parameter value is present during URL generation</span></span> |

<span data-ttu-id="ac02c-499">以冒號分隔的多個條件約束，可以套用至單一參數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-499">Multiple, colon-delimited constraints can be applied to a single parameter.</span></span> <span data-ttu-id="ac02c-500">例如，下列條件約束會將參數限制在 1 或更大的整數值：</span><span class="sxs-lookup"><span data-stu-id="ac02c-500">For example, the following constraint restricts a parameter to an integer value of 1 or greater:</span></span>

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> <span data-ttu-id="ac02c-501">確認 URL 可以轉換成 CLR 類型的路由條件約束 (例如 `int` 或 `DateTime`) 一律使用不因國別而異的文化特性。</span><span class="sxs-lookup"><span data-stu-id="ac02c-501">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="ac02c-502">這些條件約束假設 URL 不可當地語系化。</span><span class="sxs-lookup"><span data-stu-id="ac02c-502">These constraints assume that the URL is non-localizable.</span></span> <span data-ttu-id="ac02c-503">架構提供的路由條件約束不會修改路由值中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-503">The framework-provided route constraints don't modify the values stored in route values.</span></span> <span data-ttu-id="ac02c-504">所有從 URL 剖析而來的路由值會儲存為字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-504">All route values parsed from the URL are stored as strings.</span></span> <span data-ttu-id="ac02c-505">例如，`float` 條件約束會嘗試將路由值轉換成浮點數，但轉換的值只能用來確認它可以轉換成浮點數。</span><span class="sxs-lookup"><span data-stu-id="ac02c-505">For example, the `float` constraint attempts to convert the route value to a float, but the converted value is used only to verify it can be converted to a float.</span></span>

## <a name="regular-expressions"></a><span data-ttu-id="ac02c-506">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ac02c-506">Regular expressions</span></span>

<span data-ttu-id="ac02c-507">ASP.NET Core 架構將 `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` 新增至規則運算式建構函式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-507">The ASP.NET Core framework adds `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` to the regular expression constructor.</span></span> <span data-ttu-id="ac02c-508">如需這些成員的說明，請參閱 <xref:System.Text.RegularExpressions.RegexOptions>。</span><span class="sxs-lookup"><span data-stu-id="ac02c-508">See <xref:System.Text.RegularExpressions.RegexOptions> for a description of these members.</span></span>

<span data-ttu-id="ac02c-509">規則運算式使用的分隔符號和語彙基元，類似於路由和 C# 語言所使用的分隔符號和語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ac02c-509">Regular expressions use delimiters and tokens similar to those used by Routing and the C# language.</span></span> <span data-ttu-id="ac02c-510">規則運算式的語彙基元必須逸出。</span><span class="sxs-lookup"><span data-stu-id="ac02c-510">Regular expression tokens must be escaped.</span></span> <span data-ttu-id="ac02c-511">若要在路由中使用規則運算式 `^\d{3}-\d{2}-\d{4}$`，運算式必須以字串中所提供的 `\` (單一反斜線) 字元作為 C# 原始程式檔中的 `\\` (雙反斜線) 字元，才能逸出 `\` 字串逸出字元 (除非使用[逐字字串常值](/dotnet/csharp/language-reference/keywords/string))。</span><span class="sxs-lookup"><span data-stu-id="ac02c-511">To use the regular expression `^\d{3}-\d{2}-\d{4}$` in routing, the expression must have the `\` (single backslash) characters provided in the string as `\\` (double backslash) characters in the C# source file in order to escape the `\` string escape character (unless using [verbatim string literals](/dotnet/csharp/language-reference/keywords/string)).</span></span> <span data-ttu-id="ac02c-512">若要逸出路由參數分隔符號字元 (`{`、`}`、`[`、`]`)，請在運算式中使用雙字元 (`{{`、`}`、`[[`、`]]`).</span><span class="sxs-lookup"><span data-stu-id="ac02c-512">To escape routing parameter delimiter characters (`{`, `}`, `[`, `]`), double the characters in the expression (`{{`, `}`, `[[`, `]]`).</span></span> <span data-ttu-id="ac02c-513">下表顯示規則運算式和逸出的版本。</span><span class="sxs-lookup"><span data-stu-id="ac02c-513">The following table shows a regular expression and the escaped version.</span></span>

| <span data-ttu-id="ac02c-514">規則運算式</span><span class="sxs-lookup"><span data-stu-id="ac02c-514">Regular Expression</span></span>    | <span data-ttu-id="ac02c-515">逸出的規則運算式</span><span class="sxs-lookup"><span data-stu-id="ac02c-515">Escaped Regular Expression</span></span>     |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

<span data-ttu-id="ac02c-516">路由中所使用的規則運算式通常以插入號 (`^`) 字元開頭，並符合字串的開始位置。</span><span class="sxs-lookup"><span data-stu-id="ac02c-516">Regular expressions used in routing often start with the caret (`^`) character and match starting position of the string.</span></span> <span data-ttu-id="ac02c-517">運算式通常以貨幣符號 (`$`) 字元結尾，並符合字串的結尾。</span><span class="sxs-lookup"><span data-stu-id="ac02c-517">The expressions often end with the dollar sign (`$`) character and match end of the string.</span></span> <span data-ttu-id="ac02c-518">`^` 和 `$` 字元可確保規則運算式符合整個路由參數值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-518">The `^` and `$` characters ensure that the regular expression match the entire route parameter value.</span></span> <span data-ttu-id="ac02c-519">若不使用 `^` 與 `$` 字元，規則運算式會比對字串內的所有部分字串，這通常不是您想要的結果。</span><span class="sxs-lookup"><span data-stu-id="ac02c-519">Without the `^` and `$` characters, the regular expression match any substring within the string, which is often undesirable.</span></span> <span data-ttu-id="ac02c-520">下表提供範例，並說明它們符合或無法符合的原因。</span><span class="sxs-lookup"><span data-stu-id="ac02c-520">The following table provides examples and explains why they match or fail to match.</span></span>

| <span data-ttu-id="ac02c-521">運算式</span><span class="sxs-lookup"><span data-stu-id="ac02c-521">Expression</span></span>   | <span data-ttu-id="ac02c-522">String</span><span class="sxs-lookup"><span data-stu-id="ac02c-522">String</span></span>    | <span data-ttu-id="ac02c-523">比對</span><span class="sxs-lookup"><span data-stu-id="ac02c-523">Match</span></span> | <span data-ttu-id="ac02c-524">註解</span><span class="sxs-lookup"><span data-stu-id="ac02c-524">Comment</span></span>               |
| ------------ | --------- | :---: |  -------------------- |
| `[a-z]{2}`   | <span data-ttu-id="ac02c-525">hello</span><span class="sxs-lookup"><span data-stu-id="ac02c-525">hello</span></span>     | <span data-ttu-id="ac02c-526">[是]</span><span class="sxs-lookup"><span data-stu-id="ac02c-526">Yes</span></span>   | <span data-ttu-id="ac02c-527">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ac02c-527">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ac02c-528">123abc456</span><span class="sxs-lookup"><span data-stu-id="ac02c-528">123abc456</span></span> | <span data-ttu-id="ac02c-529">[是]</span><span class="sxs-lookup"><span data-stu-id="ac02c-529">Yes</span></span>   | <span data-ttu-id="ac02c-530">子字串相符項目</span><span class="sxs-lookup"><span data-stu-id="ac02c-530">Substring matches</span></span>     |
| `[a-z]{2}`   | <span data-ttu-id="ac02c-531">mz</span><span class="sxs-lookup"><span data-stu-id="ac02c-531">mz</span></span>        | <span data-ttu-id="ac02c-532">[是]</span><span class="sxs-lookup"><span data-stu-id="ac02c-532">Yes</span></span>   | <span data-ttu-id="ac02c-533">符合運算式</span><span class="sxs-lookup"><span data-stu-id="ac02c-533">Matches expression</span></span>    |
| `[a-z]{2}`   | <span data-ttu-id="ac02c-534">MZ</span><span class="sxs-lookup"><span data-stu-id="ac02c-534">MZ</span></span>        | <span data-ttu-id="ac02c-535">[是]</span><span class="sxs-lookup"><span data-stu-id="ac02c-535">Yes</span></span>   | <span data-ttu-id="ac02c-536">不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="ac02c-536">Not case sensitive</span></span>    |
| `^[a-z]{2}$` | <span data-ttu-id="ac02c-537">hello</span><span class="sxs-lookup"><span data-stu-id="ac02c-537">hello</span></span>     | <span data-ttu-id="ac02c-538">否</span><span class="sxs-lookup"><span data-stu-id="ac02c-538">No</span></span>    | <span data-ttu-id="ac02c-539">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ac02c-539">See `^` and `$` above</span></span> |
| `^[a-z]{2}$` | <span data-ttu-id="ac02c-540">123abc456</span><span class="sxs-lookup"><span data-stu-id="ac02c-540">123abc456</span></span> | <span data-ttu-id="ac02c-541">否</span><span class="sxs-lookup"><span data-stu-id="ac02c-541">No</span></span>    | <span data-ttu-id="ac02c-542">請參閱上述的 `^` 和 `$`</span><span class="sxs-lookup"><span data-stu-id="ac02c-542">See `^` and `$` above</span></span> |

<span data-ttu-id="ac02c-543">如需規則運算式語法的詳細資訊，請參閱 [.NET Framework 規則運算式](/dotnet/standard/base-types/regular-expression-language-quick-reference)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-543">For more information on regular expression syntax, see [.NET Framework Regular Expressions](/dotnet/standard/base-types/regular-expression-language-quick-reference).</span></span>

<span data-ttu-id="ac02c-544">若要將參數限制為一組已知的可能值，請使用規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-544">To constrain a parameter to a known set of possible values, use a regular expression.</span></span> <span data-ttu-id="ac02c-545">例如，`{action:regex(^(list|get|create)$)}` 只會將 `action` 路由值與 `list`、`get` 或 `create` 相符。</span><span class="sxs-lookup"><span data-stu-id="ac02c-545">For example, `{action:regex(^(list|get|create)$)}` only matches the `action` route value to `list`, `get`, or `create`.</span></span> <span data-ttu-id="ac02c-546">如果已傳入條件約束字典，字串 `^(list|get|create)$` 則是對等項目。</span><span class="sxs-lookup"><span data-stu-id="ac02c-546">If passed into the constraints dictionary, the string `^(list|get|create)$` is equivalent.</span></span> <span data-ttu-id="ac02c-547">已傳入條件約束字典 (未內嵌在範本內) 的條件約束，即使不符合其中一個已知的條件約束，也會被視為規則運算式。</span><span class="sxs-lookup"><span data-stu-id="ac02c-547">Constraints that are passed in the constraints dictionary (not inline within a template) that don't match one of the known constraints are also treated as regular expressions.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="parameter-transformer-reference"></a><span data-ttu-id="ac02c-548">參數轉換器參考</span><span class="sxs-lookup"><span data-stu-id="ac02c-548">Parameter transformer reference</span></span>

<span data-ttu-id="ac02c-549">參數轉換程式：</span><span class="sxs-lookup"><span data-stu-id="ac02c-549">Parameter transformers:</span></span>

* <span data-ttu-id="ac02c-550">會在為 `Route` 產生連結時執行。</span><span class="sxs-lookup"><span data-stu-id="ac02c-550">Execute when generating a link for a `Route`.</span></span>
* <span data-ttu-id="ac02c-551">實作 `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-551">Implement `Microsoft.AspNetCore.Routing.IOutboundParameterTransformer`.</span></span>
* <span data-ttu-id="ac02c-552">是使用 <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap> 進行設定的。</span><span class="sxs-lookup"><span data-stu-id="ac02c-552">Are configured using <xref:Microsoft.AspNetCore.Routing.RouteOptions.ConstraintMap>.</span></span>
* <span data-ttu-id="ac02c-553">採用參數的路由值，並將它轉換為新的字串值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-553">Take the parameter's route value and transform it to a new string value.</span></span>
* <span data-ttu-id="ac02c-554">導致在所產生連結中使用已轉換的值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-554">Result in using the transformed value in the generated link.</span></span>

<span data-ttu-id="ac02c-555">例如，具有 `Url.Action(new { article = "MyTestArticle" })` 之路由模式 `blog\{article:slugify}` 中的自訂 `slugify` 參數轉換器，會產生 `blog\my-test-article`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-555">For example, a custom `slugify` parameter transformer in route pattern `blog\{article:slugify}` with `Url.Action(new { article = "MyTestArticle" })` generates `blog\my-test-article`.</span></span>

<span data-ttu-id="ac02c-556">架構會使用參數轉換器來轉換端點解析的 URI。</span><span class="sxs-lookup"><span data-stu-id="ac02c-556">Parameter transformers are used by the framework to transform the URI where an endpoint resolves.</span></span> <span data-ttu-id="ac02c-557">例如，ASP.NET Core MVC 會使用參數轉換器，轉換用於比對 `area`、`controller`、`action` 和 `page` 的路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-557">For example, ASP.NET Core MVC uses parameter transformers to transform the route value used to match an `area`, `controller`, `action`, and `page`.</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home:slugify}/{action=Index:slugify}/{id?}");
```

<span data-ttu-id="ac02c-558">使用上述的路由，動作 `SubscriptionManagementController.GetAll()` 會與URI `/subscription-management/get-all` 比對。</span><span class="sxs-lookup"><span data-stu-id="ac02c-558">With the preceding route, the action `SubscriptionManagementController.GetAll()` is matched with the URI `/subscription-management/get-all`.</span></span> <span data-ttu-id="ac02c-559">參數轉換器不會變更用來產生連結的路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-559">A parameter transformer doesn't change the route values used to generate a link.</span></span> <span data-ttu-id="ac02c-560">例如，`Url.Action("GetAll", "SubscriptionManagement")` 會輸出 `/subscription-management/get-all`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-560">For example, `Url.Action("GetAll", "SubscriptionManagement")` outputs `/subscription-management/get-all`.</span></span>

<span data-ttu-id="ac02c-561">ASP.NET Core 針對搭配產生的路由使用參數轉換程式提供了 API 慣例：</span><span class="sxs-lookup"><span data-stu-id="ac02c-561">ASP.NET Core provides API conventions for using a parameter transformers with generated routes:</span></span>

* <span data-ttu-id="ac02c-562">ASP.NET Core MVC 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ac02c-562">ASP.NET Core MVC has the `Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention` API convention.</span></span> <span data-ttu-id="ac02c-563">此慣例會將所指定參數轉換程式套用到應用程式中的所有屬性路由。</span><span class="sxs-lookup"><span data-stu-id="ac02c-563">This convention applies a specified parameter transformer to all attribute routes in the app.</span></span> <span data-ttu-id="ac02c-564">參數轉換程式會在被取代時轉換屬性路由語彙基元。</span><span class="sxs-lookup"><span data-stu-id="ac02c-564">The parameter transformer transforms attribute route tokens as they are replaced.</span></span> <span data-ttu-id="ac02c-565">如需詳細資訊，請參閱[使用參數轉換程式自訂語彙基元取代](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-565">For more information, see [Use a parameter transformer to customize token replacement](/aspnet/core/mvc/controllers/routing#use-a-parameter-transformer-to-customize-token-replacement).</span></span>
* <span data-ttu-id="ac02c-566">Razor Pages 有 `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API 慣例。</span><span class="sxs-lookup"><span data-stu-id="ac02c-566">Razor Pages has the `Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteTransformerConvention` API convention.</span></span> <span data-ttu-id="ac02c-567">此慣例會將所指定參數轉換器套用至所有自動探索到的 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="ac02c-567">This convention applies a specified parameter transformer to all automatically discovered Razor Pages.</span></span> <span data-ttu-id="ac02c-568">參數轉換器會轉換 Razor Pages 路由的資料夾與檔案名稱區段。</span><span class="sxs-lookup"><span data-stu-id="ac02c-568">The parameter transformer transforms the folder and file name segments of Razor Pages routes.</span></span> <span data-ttu-id="ac02c-569">如需詳細資訊，請參閱[使用參數轉換程式自訂頁面路由](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes)。</span><span class="sxs-lookup"><span data-stu-id="ac02c-569">For more information, see [Use a parameter transformer to customize page routes](/aspnet/core/razor-pages/razor-pages-conventions#use-a-parameter-transformer-to-customize-page-routes).</span></span>

::: moniker-end

## <a name="url-generation-reference"></a><span data-ttu-id="ac02c-570">URL 產生參考</span><span class="sxs-lookup"><span data-stu-id="ac02c-570">URL generation reference</span></span>

<span data-ttu-id="ac02c-571">下列範例示範如何在指定路由值字典和 <xref:Microsoft.AspNetCore.Routing.RouteCollection> 的情況下產生路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ac02c-571">The following example shows how to generate a link to a route given a dictionary of route values and a <xref:Microsoft.AspNetCore.Routing.RouteCollection>.</span></span>

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

<span data-ttu-id="ac02c-572">在上述範例的結尾產生的 `VirtualPath` 是 `/package/create/123`。</span><span class="sxs-lookup"><span data-stu-id="ac02c-572">The `VirtualPath` generated at the end of the preceding sample is `/package/create/123`.</span></span> <span data-ttu-id="ac02c-573">字典提供「追蹤套件路由」範本 `package/{operation}/{id}` 的 `operation` 和 `id` 路由值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-573">The dictionary supplies the `operation` and `id` route values of the "Track Package Route" template, `package/{operation}/{id}`.</span></span> <span data-ttu-id="ac02c-574">如需詳細資訊，請參閱[使用路由中介軟體](#use-routing-middleware)一節或[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples)中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="ac02c-574">For details, see the sample code in the [Use Routing Middleware](#use-routing-middleware) section or the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).</span></span>

<span data-ttu-id="ac02c-575">`VirtualPathContext` 建構函式的第二個參數是「環境值」的集合。</span><span class="sxs-lookup"><span data-stu-id="ac02c-575">The second parameter to the `VirtualPathContext` constructor is a collection of *ambient values*.</span></span> <span data-ttu-id="ac02c-576">環境值便於使用，因為它們會限制開發人員必須在要求內容中指定的值數目。</span><span class="sxs-lookup"><span data-stu-id="ac02c-576">Ambient values are convenient to use because they limit the number of values a developer must specify within a request context.</span></span> <span data-ttu-id="ac02c-577">目前要求的目前路由值被視為用於連結產生的環境值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-577">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ac02c-578">在 ASP.NET Core MVC 應用程式 `HomeController` 的 `About` 動作中，您不需要指定控制器路由值以連結到 `Index` 動作&mdash;會使用 `Home` 的環境值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-578">In an ASP.NET Core MVC app's `About` action of the `HomeController`, you don't need to specify the controller route value to link to the `Index` action&mdash;the ambient value of `Home` is used.</span></span>

<span data-ttu-id="ac02c-579">不符合參數的環境值會予以忽略。</span><span class="sxs-lookup"><span data-stu-id="ac02c-579">Ambient values that don't match a parameter are ignored.</span></span> <span data-ttu-id="ac02c-580">當明確提供的值覆寫環境值時，也會忽略環境值。</span><span class="sxs-lookup"><span data-stu-id="ac02c-580">Ambient values are also ignored when an explicitly provided value overrides the ambient value.</span></span> <span data-ttu-id="ac02c-581">URL 中的比對是從左到右。</span><span class="sxs-lookup"><span data-stu-id="ac02c-581">Matching occurs from left to right in the URL.</span></span>

<span data-ttu-id="ac02c-582">明確提供但不符合路由區段的值會新增至查詢字串。</span><span class="sxs-lookup"><span data-stu-id="ac02c-582">Values explicitly provided but that don't match a segment of the route are added to the query string.</span></span> <span data-ttu-id="ac02c-583">下表顯示使用路由範本 `{controller}/{action}/{id?}` 時的結果。</span><span class="sxs-lookup"><span data-stu-id="ac02c-583">The following table shows the result when using the route template `{controller}/{action}/{id?}`.</span></span>

| <span data-ttu-id="ac02c-584">環境值</span><span class="sxs-lookup"><span data-stu-id="ac02c-584">Ambient Values</span></span>                     | <span data-ttu-id="ac02c-585">明確值</span><span class="sxs-lookup"><span data-stu-id="ac02c-585">Explicit Values</span></span>                        | <span data-ttu-id="ac02c-586">結果</span><span class="sxs-lookup"><span data-stu-id="ac02c-586">Result</span></span>                  |
| ---------------------------------- | -------------------------------------- | ----------------------- |
| <span data-ttu-id="ac02c-587">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ac02c-587">controller = "Home"</span></span>                | <span data-ttu-id="ac02c-588">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ac02c-588">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ac02c-589">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ac02c-589">controller = "Home"</span></span>                | <span data-ttu-id="ac02c-590">controller = "Order", action = "About"</span><span class="sxs-lookup"><span data-stu-id="ac02c-590">controller = "Order", action = "About"</span></span> | `/Order/About`          |
| <span data-ttu-id="ac02c-591">controller = "Home", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ac02c-591">controller = "Home", color = "Red"</span></span> | <span data-ttu-id="ac02c-592">action = "About"</span><span class="sxs-lookup"><span data-stu-id="ac02c-592">action = "About"</span></span>                       | `/Home/About`           |
| <span data-ttu-id="ac02c-593">controller = "Home"</span><span class="sxs-lookup"><span data-stu-id="ac02c-593">controller = "Home"</span></span>                | <span data-ttu-id="ac02c-594">action = "About", color = "Red"</span><span class="sxs-lookup"><span data-stu-id="ac02c-594">action = "About", color = "Red"</span></span>        | `/Home/About?color=Red` |

<span data-ttu-id="ac02c-595">如果路由具有未對應至參數的預設值，且該值會明確提供，它必須符合預設值：</span><span class="sxs-lookup"><span data-stu-id="ac02c-595">If a route has a default value that doesn't correspond to a parameter and that value is explicitly provided, it must match the default value:</span></span>

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

<span data-ttu-id="ac02c-596">連結產生只有在提供了 `controller` 與 `action` 的相符值時，才會產生此路由的連結。</span><span class="sxs-lookup"><span data-stu-id="ac02c-596">Link generation only generates a link for this route when the matching values for `controller` and `action` are provided.</span></span>
